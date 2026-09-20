# Blackjack Eval

Kids, dont gamble. (REQUIRES NITRO)

Keep in mind this requires a ballsdex clone! Please follow the tutorial for that in the Tutorials category if you want to make one!


import discord
import random

deck = [
    (rank, suit)
    for suit in ["♠️", "♥️", "♦️", "♣️"]
    for rank in ["A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"]
]
random.shuffle(deck)

player = [deck.pop(), deck.pop()]
dealer = [deck.pop(), deck.pop()]

def value(hand):
    total = 0
    aces = 0

    for rank, suit in hand:
        if rank in ["J", "Q", "K"]:
            total += 10
        elif rank == "A":
            total += 11
            aces += 1
        else:
            total += int(rank)

    while total > 21 and aces:
        total -= 10
        aces -= 1

    return total

def cards(hand):
    return " ".join(f"`{r}{s}`" for r, s in hand)

def game_text(hidden=True):
    dealer_cards = f"`{dealer[0][0]}{dealer[0][1]}` 🂠" if hidden else cards(dealer)

    return (
        f"🃏 **Blackjack**\n\n"
        f"**Dealer:** {dealer_cards}\n"
        f"**You:** {cards(player)} — **{value(player)}**"
    )

view = discord.ui.View(timeout=120)

async def finish(interaction, result):
    view.stop()
    await interaction.response.edit_message(
        content=(
            f"🃏 **Blackjack — {result}**\n\n"
            f"**Dealer:** {cards(dealer)} — **{value(dealer)}**\n"
            f"**You:** {cards(player)} — **{value(player)}**"
        ),
        view=None
    )

async def hit(interaction):
    if interaction.user.id != ctx.author.id:
        await interaction.response.send_message(
            "This isn't your game!", ephemeral=True
        )
        return

    player.append(deck.pop())
    total = value(player)

    if total > 21:
        await finish(interaction, "💥 Bust!")
    elif total == 21:
        await stand(interaction)
    else:
        await interaction.response.edit_message(
            content=game_text(),
            view=view
        )

async def stand(interaction):
    if interaction.user.id != ctx.author.id:
        await interaction.response.send_message(
            "This isn't your game!", ephemeral=True
        )
        return

    while value(dealer) < 17:
        dealer.append(deck.pop())

    p = value(player)
    d = value(dealer)

    if d > 21 or p > d:
        result = "🎉 You win!"
    elif p == d:
        result = "🤝 Push!"
    else:
        result = "🤖 Dealer wins!"

    await finish(interaction, result)

async def double_down(interaction):
    if interaction.user.id != ctx.author.id:
        await interaction.response.send_message(
            "This isn't your game!", ephemeral=True
        )
        return

    player.append(deck.pop())

    if value(player) > 21:
        await finish(interaction, "💥 Bust!")
    else:
        await stand(interaction)

hit_button = discord.ui.Button(
    label="Hit",
    emoji="👊",
    style=discord.ButtonStyle.primary
)

stand_button = discord.ui.Button(
    label="Stand",
    emoji="✋",
    style=discord.ButtonStyle.success
)

double_button = discord.ui.Button(
    label="Double Down",
    emoji="💰",
    style=discord.ButtonStyle.secondary
)

hit_button.callback = hit
stand_button.callback = stand
double_button.callback = double_down

view.add_item(hit_button)
view.add_item(stand_button)
view.add_item(double_button)

# Initial blackjack
if value(player) == 21:
    await ctx.send(
        f"🃏 **Blackjack!**\n\n"
        f"**Dealer:** {cards(dealer)} — **{value(dealer)}**\n"
        f"**You:** {cards(player)} — **21**"
    )
else:
    await ctx.send(game_text(), view=view)



Enjoy my page!!
