# Slots!

Kids, dont gamble. (NITRO IS REQUIRED.)


import random,asyncio,discord
from discord.ui import View,button

money={}

class Slots(View):
 def __init__(self,u):
  super().__init__(timeout=180);self.u=u;self.debt=0;self.jack=False
 def E(self,t,d,c=0x8B5CF6):
  e=discord.Embed(title=t,description=d,color=c);e.set_footer(text="🎰 Lucky Slots • Good luck!")
  return e
 async def edit(self,i,e):
  await i.response.edit_message(embed=e,view=self)

 @button(label="🎰 SPIN • $10",style=discord.ButtonStyle.green)
 async def spin(self,i,b):
  if i.user.id!=self.u:return
  m=money.setdefault(self.u,100)
  if m<=0:
   return await self.edit(i,self.E("💀 BANKRUPT","You've run out of money!\n\n💥 Your **2% JACKPOT** is available.",0xEF4444))
  money[self.u]-=10
  s=[random.choice(["🍒","🍋","🔔","💎","7️⃣"]) for _ in range(3)]
  if random.random()<.25:
   p=random.choice([25,50,100,250]);money[self.u]+=p
   r=f"🎉 **WIN!** You won **${p:,}**!";c=0x22C55E
  else:r="💨 **No luck this spin!**";c=0x64748B
  await self.edit(i,self.E("🎰 LUCKY SLOTS",f"### {' │ '.join(s)}\n\n{r}\n\n💰 **Balance:** `${money[self.u]:,}`",c))

 @button(label="⚡ DOUBLE DOWN • $25",style=discord.ButtonStyle.blurple)
 async def double(self,i,b):
  if i.user.id!=self.u:return
  if money.get(self.u,0)<25:return await i.response.send_message("💸 You need $25!",ephemeral=True)
  if random.random()<.05:
   money[self.u]+=500;r="💎 **HIT! +$500**";c=0xF59E0B
  else:
   money[self.u]-=50;r="💀 **FAILED! -$50**";c=0xEF4444
  await self.edit(i,self.E("⚡ DOUBLE DOWN",f"{r}\n\n💰 **Balance:** `${money[self.u]:,}`",c))

 async def loan(self,i,a,f):
  if self.debt:return await i.response.send_message("🚨 You already have a loan!",ephemeral=True)
  self.debt=a;money[self.u]=money.get(self.u,0)+a
  await self.edit(i,self.E("💰 LOAN APPROVED",f"**+${a:,}** added!\n\n⏰ Repay **${a:,}** within **30 seconds**.\n🚨 Default: **-${f:,}**",0x3B82F6))
  await asyncio.sleep(30)
  if self.debt==a:
   money[self.u]-=f;self.debt=0
   await i.message.edit(embed=self.E("🚨 LOAN DEFAULTED",f"💸 Fine: **-${f:,}**\n\n💰 **Balance:** `${money[self.u]:,}`",0xDC2626),view=self)

 @button(label="💵 BORROW $100",style=discord.ButtonStyle.gray)
 async def small(self,i,b):
  if i.user.id==self.u:await self.loan(i,100,200)

 @button(label="💰 BORROW $1,000",style=discord.ButtonStyle.red)
 async def big(self,i,b):
  if i.user.id==self.u:await self.loan(i,1000,10000)

 @button(label="💳 REPAY LOAN",style=discord.ButtonStyle.green)
 async def repay(self,i,b):
  if i.user.id!=self.u or not self.debt:return
  if money.get(self.u,0)<self.debt:return await i.response.send_message(f"💸 You need ${self.debt:,}!",ephemeral=True)
  money[self.u]-=self.debt;self.debt=0
  await self.edit(i,self.E("✅ LOAN REPAID",f"Debt cleared!\n\n💰 **Balance:** `${money[self.u]:,}`",0x22C55E))

 @button(label="💥 JACKPOT • 2%",style=discord.ButtonStyle.red)
 async def jackpot(self,i,b):
  if i.user.id!=self.u:return
  if money.get(self.u,0)>0 or self.jack:return await i.response.send_message("🎰 You can't use this yet!",ephemeral=True)
  self.jack=True;b.disabled=True
  if random.random()<.02:money[self.u]=10000;r="🎉 **JACKPOT! +$10,000!**";c=0xF59E0B
  else:r="💀 **JACKPOT MISSED!**\n\nGame over.";c=0xEF4444
  await self.edit(i,self.E("💥 JACKPOT ATTEMPT",r,c))

u=ctx.author.id
money.setdefault(u,100)
e=discord.Embed(title="🎰  LUCKY SLOTS  🎰",description=f"👤 **Player:** {ctx.author.mention}\n💰 **Balance:** `${money[u]:,}`\n\n━━━━━━━━━━━━━━━━━━━━\n🎰 Spin **$10** — **25%** win\n⚡ Double Down **$25** — **5%** win / ×2 loss\n💵 Borrow **$100** — **$200** fine\n💰 Borrow **$1,000** — **$10,000** fine\n💥 Jackpot — **2%** when bankrupt",color=0x8B5CF6)
e.set_footer(text="🎰 Lucky Slots • High risk, high reward!")
await ctx.send(embed=e,view=Slots(u))




Enjoy my page!!