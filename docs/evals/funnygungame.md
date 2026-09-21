# Funny Gun Game (Roulette)

Hey parents check your kids devices! Your child is on a eval made for 13+ users only!!

(NITRO NEEDED)


```import random,asyncio,discord
from discord.ui import View,button

class R(View):
 def __init__(s,u):
  super().__init__(timeout=180);s.u=u;s.me=100;s.op=100;s.r=1;s.t=0;s.c=1;s.n=1
 def E(s,t,d,c=0x7C3AED):return discord.Embed(title=t,description=d,color=c)
 def I(s):return f"👤 ${s.me:,}  🤖 ${s.op:,}\n🎲 Risk: **{s.r}/7**  👤 Opponent #{s.n}"
 async def go(s,i,w,sp=0):
  dead=s.r==7 or random.randrange(7)<max(0,s.r-sp)
  await i.edit_original_response(embed=s.E("🎲 THE MOMENT",f"**{w} takes the chance...**\n\n`...`\n\n`...`",0x991B1B),view=None)
  await asyncio.sleep(.7)
  if dead:
   if w=="You":s.stop();return await i.edit_original_response(embed=s.E("☠️ YOU LOST","**GAME OVER.**",0x050505),view=None)
   s.n+=1;s.op=100;s.t=0;s.c=1
   await i.edit_original_response(embed=s.E("🏆 OPPONENT LOST",f"You survived!\n\n💰 Won **${s.me+s.op:,}**\n\n👤 **Opponent #{s.n}** enters...",0x22C55E),view=None)
   await asyncio.sleep(.8)
   return await i.edit_original_response(embed=s.E("👁️ NEW OPPONENT",s.I(),0x7C3AED),view=s)
  if w=="You":s.me+=25
  else:s.op+=25
  s.r=min(7,s.r+1);s.c-=1
  await i.edit_original_response(embed=s.E("🟢 THEY LIVED",f"**{w} survived!**\n\n{s.I()}",0x22C55E),view=None)
  await asyncio.sleep(.5)
  if s.c:return await i.edit_original_response(embed=s.E("🔥 AGAIN",f"**{w} gets another chance!**\n\n{s.I()}",0xF59E0B),view=s)
  s.t^=1;s.c=1
  if s.t:await s.ai(i)
  else:await i.edit_original_response(embed=s.E("👁️ YOUR TURN",s.I(),0x7C3AED),view=s)
 async def ai(s,i):
  x=random.random()
  if x<.22:
   s.t=0;s.c=2
   return await i.edit_original_response(embed=s.E("⏭️ OPPONENT SKIPS","They skip.\n\n**You get 2 chances!**",0x64748B),view=s)
  sp=x<.52
  await i.edit_original_response(embed=s.E("🤖 OPPONENT'S MOVE",f"They choose **{'SPIN' if sp else 'TAKE'}**...",0x991B1B),view=None)
  await asyncio.sleep(.6);await s.go(i,"Opponent",sp)
 @button(label="🎲 TAKE",style=discord.ButtonStyle.red)
 async def take(s,i,b):
  if i.user.id==s.u and not s.t:await i.response.defer();await s.go(i,"You")
 @button(label="⏭️ SKIP",style=discord.ButtonStyle.gray)
 async def skip(s,i,b):
  if i.user.id!=s.u or s.t:return
  await i.response.defer();s.t=1;s.c=1
  await i.edit_original_response(embed=s.E("🤖 OPPONENT'S TURN","You skipped.\n\nThey're deciding...",0x991B1B),view=None)
  await asyncio.sleep(.5);await s.ai(i)
 @button(label="🔄 SPIN",style=discord.ButtonStyle.blurple)
 async def spin(s,i,b):
  if i.user.id==s.u and not s.t:await i.response.defer();await s.go(i,"You",1)
 @button(label="💵 CASH OUT",style=discord.ButtonStyle.green)
 async def cash(s,i,b):
  if i.user.id==s.u:
   s.stop();await i.response.edit_message(embed=s.E("💵 CASHED OUT",f"You leave with **${s.me:,}**.",0x22C55E),view=None)

g=R(ctx.author.id)
await ctx.send(embed=g.E("🎲 LIFE OR DEATH",f"👤 **{ctx.author.mention}**\n\n💰 You and each opponent start with **$100**.\n🎲 Risk rises every turn.\n🔄 Spin lowers the current chance.\n⏭️ Skip gives the other player **2 chances if they survive**.\n☠️ **7/7 = instant loss.**\n♾️ Defeat an opponent to face another."),view=g)```

Enjoy My Page!!