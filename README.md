# Chat Clip Queue

Queues video links posted in a Twitch chat and plays them one at a time: Twitch clips, YouTube (including Shorts), TikTok, Instagram reels, X/Twitter posts and Streamable.

**Use it:** open https://portalq.github.io/clip-queue/, type a Twitch channel name and press Connect. It reads chat anonymously, so no login is needed.

## YouTube chat

The queue can read YouTube live chat at the same time as Twitch. YouTube needs a free API key, which you set up once:

1. Go to https://console.cloud.google.com and sign in. Create a project (any name, e.g. `clip-queue`).
2. Open **APIs & Services → Library**, search for **YouTube Data API v3** and click **Enable**.
3. Open **APIs & Services → Credentials → Create credentials → API key**.
4. Click the new key to restrict it:
   - **Application restrictions:** Websites, then add `https://portalq.github.io/*`
   - **API restrictions:** Restrict key, then tick **YouTube Data API v3**
   - Click **Save**.
5. Copy the key and paste it into **Settings → YouTube chat** on the queue page.

To connect, paste your stream link (or your `@handle`) into the YouTube box at the top and press **Connect YouTube**. A stream link uses less of the daily allowance than an @handle.

The free allowance resets at 8am UK time. Checking every 10 seconds covers about 5 hours of streaming a day; change this under Settings.

YouTube moderators and the channel owner can use the `!cq` commands. Channel members count as "subs" for the "who can add links" setting.

## Mod commands

| Command | What it does |
|---|---|
| `!cq open` / `!cq close` | Start or stop taking links |
| `!cq next` / `!cq prev` | Skip forward or back |
| `!cq remove name` | Remove someone's links |
| `!cq clear` | Empty the queue |

Keyboard: **N** next, **P** previous.

## Notes

- Only YouTube moves on automatically when a video ends. For the other sites, press Next.
- Short TikTok links (vm.tiktok.com) can't be played in the page. Full `tiktok.com/@name/video/…` links work.
- Settings, the queue and history are saved in your browser.
