# Making ballsdex clones
Hello! This tutorial requires a PC, so if you dont have one, im sorry but you gotta get the fuck off this page.

## The Tutorial Summarized:

1: Get GIT and Docker. (BOTH ARE NEEDED)

Leave docker open in the background ALWAYS.

2: Type in **GIT Bash** in your destops search bar and open it.

3: Type this: git clone https://github.com/Ballsdex-Team/BallsDex-DiscordBot.git

(Add a name at the end if hosting multiple bots)

4: Open the bots folder and open it in WSL.

(Intergrate WSL Distro in Dockers settings.)

5: Type docker compose build in WSL.

6: Type docker compose run --rm admin-panel python3 -m django createsuperuser and create a user for it.

(Remember the user and password for later.)

7: Type docker compose up -d proxy to use the admin panel.

(You must do this to customize the bots settings)

8: Paste your bot token, cilent id, and cilent secret in the admin panel settings.

9: Customize the settings however you want


10: now run docker compose up -d to run the bot

Thx for reading!!

Enjoy my page!!