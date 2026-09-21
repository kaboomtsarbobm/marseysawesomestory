# Maze Game Eval\

Find my 10 exits (NITRO REQUIRED)

``` py 
import random,asyncio,time,discord
from discord.ui import View,button

class Maze(View):
 def __init__(s,u):
  super().__init__(timeout=70);s.u=u;s.dead=0;s.l=1;s.task=None;s.new()
 def new(s):
  if s.task:s.task.cancel()
  s.z=min(4+s.l//2,9);s.x=s.y=0;s.g=(random.randrange(s.z),random.randrange(s.z))
  while s.g==(0,0):s.g=(random.randrange(s.z),random.randrange(s.z))
  s.time=30 if s.l==10 else 60;s.end=time.monotonic()+s.time
  s.traps=set()
  if s.l>=4:
   n=s.z*s.z//3 if s.l<10 else s.z*s.z*3//4
   while len(s.traps)<n:
    p=(random.randrange(s.z),random.randrange(s.z))
    if p not in[(0,0),s.g]:s.traps.add(p)
  s.flip=s.l==10 and random.random()<.45
  s.msg=random.choice(["Something is watching.","You hear breathing.","The walls moved.","A door slammed.","Something whispered your name."])
  if s.l==10:s.msg="👁️ **THE EXIT IS REVEALED. RUN.**"
  s.task=asyncio.create_task(s.clock())
 def em(s):
  p=f"📍 `{s.x},{s.y}`" if s.l<=6 else "📍 **POSITION UNKNOWN**"
  ex=f"\n🚪 **EXIT:** `{s.g[0]},{s.g[1]}`" if s.l==10 else "\n🚪 **Exit hidden.**"
  return discord.Embed(title=f"🌀 MAZE • {s.l}/10",description=f"{s.msg}\n\n{p}{ex}\n\n⏳ **{max(0,int(s.end-time.monotonic()))}s REMAINING**",color=0x990000 if s.l==10 else 0x7C3AED)
 async def clock(s):
  try:
   while not s.dead:
    left=s.end-time.monotonic()
    if left<=0:
     s.dead=1
     return await s.message.edit(embed=discord.Embed(title="☠️ THE MAZE SWALLOWED YOU",description="The walls went silent.\n\n**SCRATCH.**\n**SCRATCH.**\n\nSomething is behind you.\n\n**I CAN SEE YOU.**",color=0x000000),view=None)
    await asyncio.sleep(min(5,left))
    if not s.dead:await s.message.edit(embed=s.em(),view=s)
  except asyncio.CancelledError:pass
  except:pass
 async def move(s,i,dx,dy):
  if i.user.id!=s.u.id or s.dead:return
  if time.monotonic()>=s.end:
   s.dead=1
   return await i.response.edit_message(embed=discord.Embed(title="☠️ GAME OVER",description="**I CAN SEE YOU.**",color=0x000000),view=None)
  if s.flip:dx,dy=-dx,-dy
  await i.response.defer()
  s.x=max(0,min(s.z-1,s.x+dx));s.y=max(0,min(s.z-1,s.y+dy))
  if s.l==10 and random.random()<.08:
   s.flip=not s.flip
   await i.followup.send("⚠️ **YOUR CONTROLS HAVE BEEN INVERTED.**",ephemeral=True)
  if (s.x,s.y) in s.traps:
   s.dead=1
   if s.task:s.task.cancel()
   return await i.edit_original_response(embed=discord.Embed(title="☠️ GAME OVER",description="**I CAN SEE YOU.**",color=0x000000),view=None)
  if s.l>=6 and random.random()<.12:
   s.x=random.randrange(s.z);s.y=random.randrange(s.z);s.msg="🌀 **The maze moved you.**"
  if (s.x,s.y)==s.g:
   if s.l==10:
    s.dead=1
    if s.task:s.task.cancel()
    return await i.edit_original_response(embed=discord.Embed(title="🏆 YOU ESCAPED",description="You survived all ten mazes.",color=0x22C55E),view=None)
   s.l+=1;s.new()
  await i.edit_original_response(embed=s.em(),view=s)
 @button(label="⬆️",style=discord.ButtonStyle.blurple)
 async def up(s,i,b):await s.move(i,0,1)
 @button(label="⬅️",style=discord.ButtonStyle.blurple)
 async def left(s,i,b):await s.move(i,-1,0)
 @button(label="⬇️",style=discord.ButtonStyle.blurple)
 async def down(s,i,b):await s.move(i,0,-1)
 @button(label="➡️",style=discord.ButtonStyle.blurple)
 async def right(s,i,b):await s.move(i,1,0)

g=Maze(ctx.author)
g.message=await ctx.send(embed=g.em(),view=g)
```

Enjoy my page!!