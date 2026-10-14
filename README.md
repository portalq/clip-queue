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

## Reviewing links

By default, links from Twitch and YouTube chat go to the **Requests** tab first. Approve them there to add them to the queue, or reject them. Switch this off under **Settings → Who can add links → Review chat links before they join the queue** to send chat links straight to the queue.

## Admin link

Someone the streamer trusts can help manage the queue from their own device, with no sign-in.

1. On the queue page, open **Settings → Admin link** and tick **Let someone help manage the queue from their own device**.
2. Copy the link and send it privately to the helper. Never post it in chat: anyone with it can manage the queue.
3. The helper opens it to see Now playing, Requests and the Queue, and can approve, reject, remove, skip, and open or close the queue.

Changes apply while the streamer's queue page is open. If the link leaks, press **Make a new admin link** and the old one stops working.

This needs the Firebase database, and the **Rules** in Firebase must be the latest [`firestore.rules`](firestore.rules).

## Website submissions

Viewers can send links from a web page instead of posting them in chat, with or without a name.

1. On the queue page, open **Settings → Website submissions** and tick **Let viewers send links from a web page**.
2. Copy the link it shows and share it (a chat command, panel or pinned message).
3. Links sent from the page wait in the **Requests** tab until you approve them. You can turn that review step off in the same settings.

The submit page shows viewers whether the queue is open. Each viewer can send one link every 20 seconds, and the per-person limit in Settings still applies. If the link gets spammed, press **Make a new link** and the old one stops working.

Viewers can also send links while the queue page is closed. The submit page shows the streamer as offline, and the next time the queue page is opened, anything sent in the last 12 hours appears in **Requests** for approval. Links older than 12 hours are dropped.

Without Firebase set up, submissions travel through [ntfy.sh](https://ntfy.sh), a free public relay that keeps messages for 12 hours.

### Keeping website links for 190 days (Firebase)

Once `config.js` has a Firebase project in it, website links are stored for 190 days instead. One project serves everyone who uses this site.

1. Go to https://console.firebase.google.com and click **Create a project**. You can pick your existing Google Cloud project. Google Analytics isn't needed.
2. Open **Build → Firestore Database → Create database**. Choose a location (e.g. `europe-west2 (London)`) and **Start in production mode**.
3. Open the **Rules** tab, replace everything with the contents of [`firestore.rules`](firestore.rules) and click **Publish**.
4. Click the gear icon → **Project settings**. Under **Your apps**, click the web icon `</>`, give it any name and click **Register app**.
5. Copy `projectId` and `apiKey` from the code it shows into `config.js`. These two values are meant to be public; the rules control what anyone can do.
6. Optional cleanup: in Firestore, open **TTL**, add a policy for collection group `subs` with timestamp field `exp`. Old links are then deleted automatically after 190 days.

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
