# boats.py
The official discord.boats API wrapper for Python

# Examples
## Post stats
With Tasks (Must be using discord.py version 1.1.0+):
```python
import discord, discordboats, asyncio
from discord.ext import commands, tasks

class Discord_Boats(commands.Cog):
    """Interacts with the discord.boats API"""

    def __init__(self, bot):
        self.bot = bot
        self.update_stats.start()
        self.dbpy = discordboats.Client("Token", loop=self.bot.loop) # Token obtained from discord.boats

    def cog_unload(self):
        self.update_stats.cancel()

    @tasks.loop(minutes=30.0)
    async def update_stats(self):
        """This automatically updates your server count to discord.boats every 30 minutes."""
        await dbpy.post_stats(self.bot.user.id, len(self.bot.guilds))

def setup(bot):
    bot.add_cog(Discord_Boats(bot))
```
Without Tasks:
```python
import discord, discordboats, asyncio
from discord.ext import commands

class Discord_Boats(commands.Cog):
    """Interacts with the discord.boats API"""

    def __init__(self, bot):
        self.bot = bot
        self.dbpy = discordboats.Client("Token", loop=self.bot.loop) # Token obtained from discord.boats
        
    async def update_stats(self):
        """This automatically updates your server count to discord.boats every 30 minutes."""
        while True:
            await dbpy.post_stats(self.bot.user.id, len(self.bot.guilds))
            await asyncio.sleep(1800)

def setup(bot):
    bot.loop.create_task(update_stats())
    bot.add_cog(Discord_Boats(bot))
```
