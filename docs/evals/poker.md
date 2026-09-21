# Poker Eval

Hey lovelies!! This eval is quite buggy, so expect it to kinda make your bot a little slow.

(NITRO REQUIRED)

``` py 
import random,discord
from discord.ui import View,button,Modal,TextInput
M={};R="23456789TJQKA";S="♠♥♦♣"
def rank(c):
 v=sorted(["23456789TJQKA".index(x[0]) for x in c],reverse=True);s=[x[1] for x in c]
 n=sorted([v.count(x) for x in set(v)],reverse=True)
 if len(set(s))==1:return(5,)
 if n[0]==4:return(4,)
 if n[0]==3 and n[1]==2:return(3,)
 if n[0]==3:return(2,)
 if n[0]==2 and n[1]==2:return(1,)
 return(0,*v)
class Bet(Modal,title="💰 BET"):
 x=TextInput(label="$1-$250",max_length=3)
 def __init__(s,g):super().__init__();s.g=g
 async def on_submit(s,i):
  try:n=int(s.x.value)
  except:return await i.response.send_message("❌ Invalid.",ephemeral=True)
  if n>250:return await i.response.send_message("Isnt that a bit too much..?",ephemeral=True)
  if n<1 or n>M[s.g.u]:return await i.response.send_message("💸 Invalid bet.",ephemeral=True)
  await s.g.go(i,n)
class P(View):
 def __init__(s,u):super().__init__(timeout=180);s.u=u;s.new()
 def new(s):
  s.d=[r+x for r in R for x in S];random.shuffle(s.d);s.a=[s.d.pop() for _ in range(2)];s.o=[s.d.pop() for _ in range(2)];s.c=[];s.p=0;s.n=0
 def e(s,t,d,c=0x7C3AED):return discord.Embed(title=t,description=d,color=c)
 def info(s):return f"💳 **${M[s.u]}** • 💰 **${s.p}**\n🃏 `{' '.join(s.a)}`\n🎴 `{' '.join(s.c) or '?? ?? ?? ?? ??'}`"
 async def go(s,i,n):
  await i.response.defer();M[s.u]-=n;s.p+=n
  if s.n==0:s.c=[s.d.pop() for _ in range(3)]
  elif s.n<3:s.c+=s.d.pop(),
  else:return await s.show(i)
  s.n+=1;await i.edit_original_response(embed=s.e("♠️ POKER",s.info()),view=s)
 async def show(s,i):
  a,b=rank(s.a+s.c),rank(s.o+s.c)
  if random.random()<.3:b=(9,)
  if a>b:M[s.u]+=s.p*2;r=f"🏆 WIN +${s.p*2}";c=0x22C55E
  elif a==b:M[s.u]+=s.p;r="🤝 PUSH";c=0xF59E0B
  else:r=f"💀 LOSE -${s.p}";c=0xEF4444
  await i.edit_original_response(embed=s.e("♠️ SHOWDOWN",f"{r}\n\nYou: **{a[0]}**\nDealer: **{b[0]}**\n\n💳 **${M[s.u]}**",c),view=E(s))
 @button(label="💵 BET",style=discord.ButtonStyle.green)
 async def bet(s,i,b):await i.response.send_modal(Bet(s))
 @button(label="🔥 ALL IN",style=discord.ButtonStyle.red)
 async def all(s,i,b):await s.go(i,M[s.u])
 @button(label="✓ CHECK",style=discord.ButtonStyle.blurple)
 async def check(s,i,b):await s.go(i,0)
 @button(label="🏳️ FOLD",style=discord.ButtonStyle.gray)
 async def fold(s,i,b):
  await i.response.defer();M[s.u]-=s.p;await i.edit_original_response(embed=s.e("🏳️ FOLDED",f"💳 **${M[s.u]}**",0xEF4444),view=None)
class E(View):
 def __init__(s,g):super().__init__(timeout=120);s.g=g
 @button(label="💵 CASH OUT",style=discord.ButtonStyle.green)
 async def cash(s,i,b):await i.response.edit_message(embed=s.g.e("💵 CASHED OUT",f"**${M[s.g.u]}** secured!"),view=None)
 @button(label="🎰 KEEP PLAYING",style=discord.ButtonStyle.blurple)
 async def play(s,i,b):
  g=P(s.g.u);await i.response.edit_message(embed=g.e("♠️ NEW HAND",g.info()),view=g)
u=ctx.author.id;M.setdefault(u,500);g=P(u)
await ctx.send(embed=g.e("♠️ ROYAL POKER",f"{ctx.author.mention}\n\n{g.info()}\n\n💵 **BET $1-$250**\n🔥 **ALL IN**\n\nWin → **CASH OUT** or **KEEP PLAYING**"),view=g)
```


Enjoy my page!!