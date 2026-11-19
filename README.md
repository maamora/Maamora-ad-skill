# Maamora Ad Skill

> Turn a background photo, your app screens and a logo into a vertical **"AI made this ad"** short, in the Maamora ad format. Built as a [Claude skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview).

You give Claude your brand assets. It builds a 1080×1920 video (30 fps) of a desk scene: a monitor and a laptop both play your brand ad, with a hook caption above and a call-to-action caption below. You can add a voiceover from your own audio, and the video stretches to fit it.

---

## What you get

| Output | Format | Use for |
|---|---|---|
| `out.mp4` | 9:16, 1080×1920, 30 fps | Reels, Shorts, TikTok |
| `out_4x5.mp4` | 4:5, 1080×1350 | Instagram and Facebook feed posts |
| Post texts | Text | One for Instagram, one for Facebook, ready to paste |

The silent version runs 22.5 s. With a voiceover, the video stretches to match the audio length.

## The ad timeline

The structure stays the same for every brand. Only your content changes.

| Time (s) | Beat |
|---|---|
| 0 – 1.9 | Logo sting: the mark draws with a spark, the wordmark appears |
| 1.75 – 7.3 | Phone: home screen, search typed, results, product page, button press, order confirmed |
| 7.15 – 9.95 | Delivery map: route draws, "Arriving today" card |
| 9.8 – 13.75 | Box slides in, logo appears on it, "Delivered" notification |
| 13.7 – 15.3 | Text line 1 with underline |
| 15.25 – 17.2 | Text line 2, mark draws over it |
| 17.0 – 22.5 | Logo and slogan, slow push-in |

---

## What you need to provide

1. **Background image.** Any photo; landscape works best.
2. **Logo.** Best is a symbol and a wordmark that can be separated. A single logo also works.
3. **App screens.** Screenshots or a design of the home screen, search results and product page.
4. **One product.** Name, photo, price, and a discount or group price if you have one.
5. **Brand details.** Primary and dark colours, slogan, and the words typed in the search bar.
6. **Hook and CTA text**, in your audience's language.
   - Default hook: `Made 100% by AI 🤯`
   - Default CTA: `Comment “AD” to get the prompt ⬇️`
7. **Voiceover (optional).** Your own recording (MP3, M4A or WAV) and the exact words you said. If you haven't recorded yet, Claude can write the script with you first.

Anything missing becomes a clearly flagged placeholder. Claude never presents invented prices or discounts as real.

---

## Install

### Claude.ai (web / desktop)

1. Download this repo as a ZIP (**Code → Download ZIP**), or zip the `maamora/` folder.
2. In Claude, go to **Settings → Capabilities → Skills** and upload the ZIP.
3. Make sure the skill is switched on.

### Claude Code

```bash
git clone https://github.com/maamora/Maamora-ad-skill.git
cp -r Maamora-ad-skill/maamora ~/.claude/skills/maamora
```

Restart Claude Code, and the skill will be picked up automatically.

---

## Use

Start a chat and ask for it in plain words:

> Make a Maamora-style ad for **[brand]**. Here are the background image, logo and app screens.

Claude walks you through the assets it still needs, builds the template, checks stills of every beat, and then renders the final videos.

### With a voiceover

> Make a Maamora-style ad for **[brand]** with my voiceover. Here is the recording, and this is what I said: "…"

Claude finds the pauses in your audio, matches each line to its on-screen beat, and writes a `timing.json`. Listen to the result and tell Claude if any line is off. It only needs to move that line.

ElevenLabs is an optional extra. Claude uses it only if you ask and have it connected.

---

## Requirements for rendering

The skill renders locally in Claude's workspace. If you run the template yourself, you need:

- **Node.js** and **Playwright** (`playwright` or `playwright-core`)
- **Chrome or Chromium** (set `CHROME=/path/to/chrome` if it isn't found automatically)
- **ffmpeg**
- **Python 3** (for the voiceover alignment script)
- Fonts: Cairo, Barlow Condensed and Noto Color Emoji, installed via `@fontsource` in the skill's setup step

---

## What's inside

```
Maamora-ad-skill/
├── README.md
└── maamora/
    └── SKILL.md      # instructions + the four template files
```

`SKILL.md` carries the workflow and the full working template:

| File (inside `SKILL.md`) | Role |
|---|---|
| `ad.html` | The ad itself: logo sting, phone screens, delivery map, box, text beats |
| `scene.html` | The desk scene: background, monitor, laptop, hook and CTA captions |
| `render.js` | Renders stills (`snap`) and the final videos (`full`) |
| `align.py` | Matches voiceover lines to on-screen beats and writes `timing.json` |

---

## Good to know

- **Use only your own assets.** Logo, screens and photos must be yours. Don't imitate another company's brand.
- **One-line captions.** Keep each caption to about 35 characters. A longer caption also covers more of the background.
- **Posting on Instagram and Facebook.** Feed posts only accept 4:5 to 16:9, so a 9:16 video must go up as a Reel. Use `out_4x5.mp4` for a normal feed post.
- **Everything is drawn.** The scene contains no filmed footage, so don't claim "shot with no camera" if you used real footage.
- **Check claims.** Any discount, price or number in the video must be true. Claude flags anything the video can't back up.

---

## About

Made by the [Maamora](https://github.com/maamora) team. Maamora is a social commerce app built around group buying and cash on delivery in Morocco.
