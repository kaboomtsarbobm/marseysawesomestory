# 50 Doors Eval

(Buggy. Sorry)



``` py import random,asyncio

async def game():
 n=1
 while n<50:
  await ctx.send(f"🚪 **DOOR {n}/50**\n\nChoose: `1` `2` `3`")
  try:
   m=await ctx.bot.wait_for("message",timeout=30,check=lambda x:x.author==ctx.author and x.channel==ctx.channel)
  except asyncio.TimeoutError:
   return await ctx.send("☠️ **GAME OVER**\n\nYou waited too long.")
  if m.content not in("1","2","3"):continue
  if n>10 and random.random()<.5:
   return await ctx.send(f"☠️ **DOOR {n}**\n\nThe door opens.\n\n**I CAN SEE YOU.**")
  n+=1
 if n==50:
  win=random.randint(1,25)
  await ctx.send("🚪 **DOOR 50**\n\nThere are **25 doors**.\n\nOnly ONE works.\n\nChoose a number from **1-25**.")
  try:
   m=await ctx.bot.wait_for("message",timeout=30,check=lambda x:x.author==ctx.author and x.channel==ctx.channel)
   x=int(m.content)
  except:
   return await ctx.send("☠️ **GAME OVER**")
  if x==win:
   await ctx.send("🏆 **YOU ESCAPED**\n\nYou chose the only door that worked.\n\n**YOU SURVIVED ALL 50 DOORS.**")
  else:
   await ctx.send("☠️ **GAME OVER**\n\nThe door opens.\n\n**I CAN SEE YOU.**")

await game()```

Enjoy My Page!!
