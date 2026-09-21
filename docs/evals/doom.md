# DOOM Eval

If its a dex, it can run bootleg DOOM. (REQUIRES NITRO)

Keep in mind this requires a ballsdex clone! Please follow the tutorial for that in the Tutorials category if you want to make one!



```import discord
import random

class DoomView(discord.ui.View):
    def __init__(self):
        super().__init__(timeout=180)
        self.hp = 100
        self.ammo = 6
        self.enemy_hp = 50
        self.score = 0

    def status(self):
        return (
            f"💀 **DOOMDEX**\n\n"
            f"❤️ Health: `{self.hp}/100`\n"
            f"🔫 Ammo: `{self.ammo}/6`\n"
            f"👹 Demon HP: `{max(0, self.enemy_hp)}`\n"
            f"🏆 Score: `{self.score}`"
        )

    async def enemy_attack(self, interaction):
        if self.enemy_hp > 0:
            damage = random.randint(5, 15)
            self.hp -= damage

    @discord.ui.button(label="🔫 SHOOT", style=discord.ButtonStyle.danger)
    async def shoot(self, interaction, button):
        if self.ammo <= 0:
            await interaction.response.send_message(
                "🔫 CLICK... CLICK... **OUT OF AMMO!**", ephemeral=True
            )
            return

        self.ammo -= 1
        damage = random.randint(10, 25)
        self.enemy_hp -= damage

        if self.enemy_hp <= 0:
            self.score += 100
            self.enemy_hp = 50
            self.ammo = 6
            text = "💥 **DEMON DESTROYED!** A new demon appears!"
        else:
            await self.enemy_attack(interaction)
            text = f"💥 You dealt `{damage}` damage!"

        if self.hp <= 0:
            self.stop()
            text += "\n\n☠️ **YOU DIED.**"

        await interaction.response.edit_message(
            content=text + "\n\n" + self.status(), view=self
        )

    @discord.ui.button(label="💊 HEAL", style=discord.ButtonStyle.success)
    async def heal(self, interaction, button):
        if self.hp >= 100:
            await interaction.response.send_message(
                "❤️ You're already at full health!", ephemeral=True
            )
            return

        amount = random.randint(10, 25)
        self.hp = min(100, self.hp + amount)

        await self.enemy_attack(interaction)

        if self.hp <= 0:
            self.stop()
            await interaction.response.edit_message(
                content="☠️ **THE DEMON GOT YOU. GAME OVER.**",
                view=self
            )
            return

        await interaction.response.edit_message(
            content=f"💊 Healed `{amount}` HP!\n\n{self.status()}",
            view=self
        )

    @discord.ui.button(label="⚔️ MELEE", style=discord.ButtonStyle.secondary)
    async def melee(self, interaction, button):
        damage = random.randint(5, 18)
        self.enemy_hp -= damage

        await self.enemy_attack(interaction)

        if self.enemy_hp <= 0:
            self.score += 75
            self.enemy_hp = 50
            text = "💀 **MELEE KILL!**"
        else:
            text = f"👊 You punched the demon for `{damage}` damage!"

        if self.hp <= 0:
            self.stop()
            text += "\n\n☠️ **YOU DIED.**"

        await interaction.response.edit_message(
            content=text + "\n\n" + self.status(), view=self
        )

    @discord.ui.button(label="🔄 RELOAD", style=discord.ButtonStyle.primary)
    async def reload(self, interaction, button):
        self.ammo = 6
        await interaction.response.edit_message(
            content="🔄 **RELOADED!**\n\n" + self.status(),
            view=self
        )


view = DoomView()

await ctx.send(
    "💀 **WELCOME TO DOOM**\n\n"
    "A demon is approaching. **Kill it.**\n\n" + view.status(),
    view=view
)```


Enjoy my page!!