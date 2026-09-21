# Tic Tac Toe Eval

Keep in mind this requires a ballsdex clone! Please follow the tutorial for that in the Tutorials category if you want to make one!


```import discord,random

b=["⬜"]*9;v=discord.ui.View(timeout=120)

def win(x):
    return any(b[a]==b[c]==b[d]==x for a,c,d in[(0,1,2),(3,4,5),(6,7,8),(0,3,6),(1,4,7),(2,5,8),(0,4,8),(2,4,6)])

async def mv(i):
    async def f(it):
        if it.user.id!=ctx.author.id:return await it.response.send_message("Not your game!",ephemeral=True)
        if b[i]!="⬜":return await it.response.send_message("Taken!",ephemeral=True)
        b[i]="❌"
        if win("❌"): return await it.response.edit_message(content="🎉 You win!",view=None)
        q=[j for j in range(9) if b[j]=="⬜"]
        if not q:return await it.response.edit_message(content="🤝 Draw!",view=None)
        b[random.choice(q)]="⭕"
        for j,x in enumerate(v.children): x.label=b[j]
        await it.response.edit_message(content="🎮 Tic-Tac-Toe",view=v)
    return f

for i in range(9):
    x=discord.ui.Button(label="⬜",row=i//3)
    x.callback=await mv(i);v.add_item(x)

await ctx.send("🎮 **Tic-Tac-Toe**\nYou are ❌",view=v)```


Enjoy my page!