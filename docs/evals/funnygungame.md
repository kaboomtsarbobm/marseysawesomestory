# Funny Gun Game (Roulette)

Hey parents check your kids devices! Your child is on a eval made for 13+ users only!!

(NITRO NEEDED)


``` py 
import random,asyncio,discord
class R(discord.ui.View):
 def __init__(s,u):
  super().__init__(timeout=180);s.u=u;s.me=s.op=100;s.r=7;s.b=1;s.t=0;s.n=1;s.turns=0;s.skip=None;s.bonus=0;s.cd=0
 def E(s,t,d,c=0x7C3AED):return discord.Embed(title=t,description=d,color=c)
 def I(s):return f"👤 ${s.me:,}  🤖 ${s.op:,}\n🔫 Risk: **{s.b}/{s.r}**  👤 Opponent #{s.n}"
 async def go(s,i,w,sp=0):
  x=max(1,s.b-sp);dead=random.randrange(s.r)<x
  if not i.response.is_done():await i.response.edit_message(embed=s.E("🎲 THE MOMENT",f"**{w} takes the chance...**\n\n`🔫 {x}/{s.r}`",0x991B1B),view=None)
  else:await i.edit_original_response(embed=s.E("🎲 THE MOMENT",f"**{w} takes the chance...**\n\n`🔫 {x}/{s.r}`",0x991B1B),view=None)
  await asyncio.sleep(.7)
  if dead:
   if w=="You":s.stop();return await i.edit_original_response(embed=s.E("☠️ YOU LOST","**GAME OVER.**",0x050505),view=None)
   s.n+=1;s.op=100;s.t=0;s.turns=0;s.r+=1;s.b=1;s.skip=None;s.bonus=0;s.cd=0
   return await i.edit_original_response(embed=s.E("🏆 OPPONENT LOST",f"You survived!\n\n💰 Won **${s.me+s.op:,}**\n\n🔫 Chambers: **{s.r}**\n\n👤 **Opponent #{s.n}** enters...",0x22C55E),view=s)
  if w=="You":s.me+=25
  else:s.op+=25
  s.turns+=1;s.b=min(s.r,s.b+1)
  if s.n>=5 and s.turns%3==0:s.b=min(s.r,s.b+1)

  if s.cd:s.cd-=1

  if s.bonus:
   s.bonus-=1
   if s.bonus:return await i.edit_original_response(embed=s.E("🔥 BONUS TURN",f"**{w} gets another chance!**\n\n{s.I()}",0xF59E0B),view=s) if w=="You" else await s.ai(i)
  elif s.skip==w:
   s.skip=None;s.bonus=2
   return await i.edit_original_response(embed=s.E("🔥 SKIP BONUS",f"**{w} survived!**\n\nThe player who skipped gets **2 turns!**",0xF59E0B),view=s) if w=="You" else await s.ai(i)

  s.t^=1
  if s.t:return await s.ai(i)
  await i.edit_original_response(embed=s.E("👁️ YOUR TURN",s.I()),view=s)

 async def ai(s,i):
  x=random.random()
  if x<.22 and s.skip!="You":
   s.skip="Opponent";s.t=0;s.cd=1
   return await i.edit_original_response(embed=s.E("⏭️ OPPONENT SKIPS","They skipped.\n\n**You get 1 chance.**\nIf you survive, **they get 2 turns.**",0x64748B),view=s)
  await i.edit_original_response(embed=s.E("🤖 OPPONENT'S MOVE",f"They choose **{'SPIN' if x<.52 else 'TAKE'}**...",0x991B1B),view=None)
  await asyncio.sleep(.4);await s.go(i,"Opponent",x<.52)

 @discord.ui.button(label="🎲 TAKE",style=discord.ButtonStyle.red)
 async def take(s,i,b):
  if i.user.id==s.u and not s.t:await i.response.defer();await s.go(i,"You")

 @discord.ui.button(label="⏭️ SKIP",style=discord.ButtonStyle.gray)
 async def skip(s,i,b):
  if i.user.id!=s.u or s.t or s.skip=="Opponent" or s.cd:return
  await i.response.defer();s.skip="You";s.t=1;s.cd=1
  await i.edit_original_response(embed=s.E("⏭️ YOU SKIPPED","You skipped.\n\n**Opponent gets 1 chance.**\nIf they survive, **you get 2 turns.**",0x64748B),view=None)
  await asyncio.sleep(.4);await s.ai(i)

 @discord.ui.button(label="🔄 SPIN",style=discord.ButtonStyle.blurple)
 async def spin(s,i,b):
  if i.user.id==s.u and not s.t:await i.response.defer();await s.go(i,"You",1)

 @discord.ui.button(label="💵 CASH OUT",style=discord.ButtonStyle.green)
 async def cash(s,i,b):
  if i.user.id==s.u:s.stop();await i.response.edit_message(embed=s.E("💵 CASHED OUT",f"You leave with **${s.me:,}**.",0x22C55E),view=None)

g=R(ctx.author.id)
await ctx.send(embed=g.E("🎲 LIFE OR DEATH",f"👤 **{ctx.author.mention}**\n\n💰 Start: **$100**\n🔫 Starting risk: **1/7**\n💀 Every survived chance adds **1 bullet**.\n💀 Every defeated opponent adds **1 chamber**.\n🔥 From Opponent #5, +1 bullet every 3 turns.\n🔄 Spin lowers bullets by 1.\n⏭️ Skip → other player gets 1 chance; if they survive, the skipper gets **2 turns**.\n⏳ Skip has a **1-turn cooldown**.\n☠️ Bullets ≥ chambers = guaranteed.",0x7C3AED),view=g)
```

Enjoy My Page!!