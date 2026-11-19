---
name: "maamora"
description: "Make a vertical \"AI made this ad\" short for any brand from a background image, app screens and a logo, in the Maamora ad format, with an optional voiceover from the user's own audio. Use when asked for a Maamora-style ad or short."
---

# Maamora ad short

Builds a 1080x1920, 30 fps video (22.5 s silent, or stretched to fit a voiceover): a desk scene (background photo, monitor, laptop with a video editor) where the monitor plays a brand ad and the laptop preview plays the same ad. A hook caption sits above the monitor and a CTA caption over the laptop.

The four files below are the working template. The default content is the Maamora ad. For a new brand, change only what the user supplied and keep the timing and structure.

## Ask the user for

1. Background image (any photo; landscape works best).
2. Logo. Best: a symbol and a wordmark that can be separated. A single logo also works.
3. App screens (screenshots or a design): home, search results, product page.
4. One product: name, photo, price, and a discount or group price if they have one.
5. Brand colours (primary + dark), slogan, the search words typed in the app.
6. Hook text (top) and CTA text (bottom), in the language of the audience. Defaults: `Made 100% by AI 🤯` and `Comment “AD” to get the prompt ⬇️`. Maamora's Darija versions: `Video 100% Bel Ai 🤯` and `Comment “AD” bach nsifet lik prompt ⬇️`.
7. Optional voiceover: an audio file they recorded or made themselves (any app or voice tool; MP3, M4A or WAV), plus the exact words they said. If they haven't recorded yet, offer to write the script with them first. Use ElevenLabs only if they ask for it and have it connected.

If something is missing, use a sensible placeholder and say which values are placeholders. Never present invented prices or discounts as real.

## Ad timeline (keep these beats)

| Time (s) | Beat |
|---|---|
| 0-1.9 | Logo sting: mark draws with a spark, wordmark appears |
| 1.75-7.3 | Phone: home screen, search typed, results with floating cards, product page, button press, order confirmed |
| 7.15-9.95 | Delivery map: route draws, "Arriving today" card |
| 9.8-13.75 | Box slides in, logo appears on it, "Delivered" notification |
| 13.7-15.3 | Text line 1 with underline |
| 15.25-17.2 | Text line 2, mark draws over it |
| 17.0-22.5 | Logo and slogan, slow push-in |

## Steps

1. Make a work folder and save the four template files into it exactly as given below.
2. Fonts (run in the work folder):
   ```bash
   npm pack @fontsource/cairo @fontsource/barlow-condensed @fontsource/noto-color-emoji --silent
   mkdir -p fonts emoji
   tar xzf fontsource-cairo-*.tgz --strip-components=2 -C fonts package/files
   tar xzf fontsource-barlow-condensed-*.tgz --strip-components=2 -C fonts package/files
   tar xzf fontsource-noto-color-emoji-*.tgz --strip-components=1 -C emoji
   ```
   Cairo is for Arabic text. For a Latin-only brand it is still loaded but unused.
3. Assets, saved in the work folder with these names:
   - `bg.png`: the background image.
   - `product.jpg`: the product photo, cropped square.
   - `word.png`: the wordmark in white on transparent, trimmed to its edges.
   - `mark.png`: the symbol in its brand colours on transparent, trimmed. If the logo has no separate symbol, put the whole logo (light version) in `word.png`, skip `mark.png` and set `hasMark:false`.
   - To cut a logo that sits on a white background: alpha = (255 - min(R,G,B)) / 233 clipped to 0..1, un-multiply the colour, then crop. Set the wordmark pixels to white.
4. Edit `ad.html`:
   - `CFG` block: `hasMark`, the mark's position and size relative to the wordmark (`markX`, `markW`, `markAspect`, `markGap`), and `searchText`.
   - Colours: replace the orange family (`#F28A1E`, `#f08a2c`, `#f5a33a`, `#ffd43b`, and the `rgba(242,138,30,…)` glows) with the brand's primary, and `#1a2433` with its dark colour. Recolour the box (`#f9931f`, `#e2740f`, `#ffc061`, `#ffa433`, `#c25f0a`) to match.
   - Phone screens `#pH` (home), `#pA` (results), `#pd` (product page): rebuild them to match the user's app screens: layout, language, direction (`direction:rtl` or `ltr`), labels. Keep the element ids used by `seek()`: `ph1`, `suggs`, `grid`, `b1`, `placed`.
   - Products: the `P` array (name, price, old price, hue shift, badge text, badge colour), the suggestions, and the product page name and prices.
   - Texts: `#track` and `#notif` cards, text line 1 (`#sT1`), text line 2 (`#a2a`, where `#A1` and `#A2` mark the two letters the mark spans), `#slogan`.
5. Edit `scene.html`: the two caption texts (emoji go inside `<span class="e">`), and `SCENE.bgPosition` so the subject of the photo shows above the monitor.
6. Check stills before the full render: `node render.js snap 1.2 3.5 5.8 6.9 8.8 12 14.6 16.2 21`, then look at every `s_*.jpg`. Fix anything cut off, overlapping or unreadable.
7. Optional voiceover: follow "Voiceover" below before rendering. It adds `voice.mp3`, `script.txt` and `timing.json` to the work folder.
8. Render once: `node render.js full` writes `out.mp4` (9:16, for Reels, Shorts and TikTok) and `out_4x5.mp4` (1080x1350). If `voice.mp3` is in the folder it is merged into both. It needs Node, Playwright (`playwright` or `playwright-core`), Chrome or Chromium (set `CHROME=/path` if not found), and ffmpeg.
9. Send both files. Instagram and Facebook feed posts only accept 4:5 to 16:9, so a 9:16 video posted as a normal post is refused; it must go up as a Reel, or use `out_4x5.mp4`. The 4:5 crop keeps both captions, the monitor and the laptop screen, and loses the top of the sky and the keyboard.
10. Offer post texts: one for Instagram (short hook, 4 benefit lines with emoji, the comment CTA, hashtags) and one for Facebook (a question hook, a few sentences, the app link, the comment CTA). Write them in the same language mix as the voiceover. Flag any claim the video can't back up, such as a discount figure or "no camera" when real footage was used.

## Voiceover

This needs nothing but Claude and the user's audio file. ElevenLabs is an optional extra at the end.

1. Script: one line per beat, in the user's language mix. About 55 words come out at about 26 s; that is fine, the video stretches to the voice. Maamora's script:

   | Beat | Line |
   |---|---|
   | Logo | معمورة… l'application اللي كتخلّيك تشري مع الجماعة. |
   | Search | كتقلّب على داكشي اللي بغيتي، en quelques secondes. |
   | Results, product page | كتلقى le produit، وكتشوف le prix ديال الجماعة. |
   | Group bar | كتدخل مع صحابك ف groupe، وكتخلّص قل. |
   | Order confirmed | Commande validée ! |
   | Map | La livraison كتجي حتى لباب الدار. |
   | Box | وما كتخلّص حتى توصلك: paiement à la livraison. |
   | Text line 1 | كاع داكشي اللي كتبغي… |
   | Text line 2 | de A تنجيبو الدورة حتال A. |
   | Logo, slogan | معمورة. شري مع الجماعة. Téléchargez l'application maintenant. |

   If the user hasn't recorded yet, write the script with them, let them correct the wording (dialect phrasing is theirs to judge), and have them record it with any app or voice tool, leaving a short breath between lines. If they already have a recording, ask for the exact words they said: you cannot transcribe audio yourself.
2. Save the audio in the work folder as `voice.mp3` (convert other formats with `ffmpeg -i input.m4a voice.mp3`) and the words as `script.txt`: 10 lines, one per beat, in the order of the table. If their text isn't split that way, split it at sentence ends so each line matches what is on screen at that beat.
3. Run `python3 align.py voice.mp3 script.txt`. It finds the pauses in the audio with ffmpeg, chooses the pause where each line starts (close to where the line's share of the text puts it, preferring long pauses), and writes `timing.json`: `{"duration": D, "anchors": [[videoSecond, adSecond], ...]}`. Check its printout: the times must rise line by line, and any line marked as an estimate needs a closer look. On Maamora's recording every line landed within 0.2 s of where it is spoken.
4. Render stills at each printed line time plus 0.5 s (`node render.js snap ...`) and check that each shows its beat. To move a line, change the first number of its anchor in `timing.json`. Ad-time beat starts: logo 0, search 1.9, results 3.9, group bar 5.95, order confirmed 6.35, map 7.15, box 9.85, text line 1 13.7, text line 2 15.25, final logo 17.0, slogan 20.0, end 22.5.
5. Render. You cannot listen to the audio, so give the user the line times and ask them to check the sync by ear; if one line is off, they tell you which and you move its anchor.

### Optional: ElevenLabs

Only when the user asks for it and has the ElevenLabs connector. Each generation spends their credits, so never re-run one to retry.

1. `creative_create_flow`, then `creative_list_voices` (filter by language and accent; use the user's own clone if they have one), then `creative_generate_speech` with model `eleven_v4` and the script as the prompt.
2. Downloads from ElevenLabs links are often blocked in the sandbox: ask the user to download the take they like as MP3 and attach it, then continue from step 2 above.
3. For exact word times, `creative_transcribe_audio` (model `eleven_scribe_v1`, `connect_from` the speech node) followed by one `creative_get_flow_run_status` call returns `transcripts[0].words` with start and end seconds. Put each line's first-word start into `timing.json` in place of the `align.py` result.

## Rules

- Use only the user's own logo, screens and photos. Do not imitate another company's brand.
- Keep each caption on one line: about 35 characters at the default size.
- The mark drawing over text line 2 suits arc or swoosh marks. With `hasMark:false` an underline is drawn instead.
- The whole scene is drawn, so there is no filmed footage in it.
- A caption that is too long for one line also covers more of the background; shorten it rather than shrink the font.

## Template file: `ad.html`

```html
<!DOCTYPE html>
<html><head><meta charset="utf-8">
<style>
@font-face{font-family:Cairo;font-weight:900;src:url(fonts/cairo-arabic-900-normal.woff2)}
@font-face{font-family:Cairo;font-weight:700;src:url(fonts/cairo-arabic-700-normal.woff2)}
@font-face{font-family:Cairo;font-weight:400;src:url(fonts/cairo-arabic-400-normal.woff2)}
*{margin:0;padding:0;box-sizing:border-box}
html,body{width:1280px;height:720px;overflow:hidden;background:#000}
#stage{position:relative;width:1280px;height:720px;overflow:hidden;background:#050505;font-family:"Inter Display","Inter",sans-serif;color:#fff}
#glow{position:absolute;inset:0;background:radial-gradient(ellipse 60% 70% at 50% 62%,rgba(242,138,30,.34) 0%,rgba(120,60,10,.16) 40%,rgba(0,0,0,0) 75%)}
.scene{position:absolute;inset:0;opacity:0}
.abs{position:absolute}
/* logo */
#logo{position:absolute;left:400px;top:262px;width:480px;height:160px}
#logoWord{position:absolute;left:0;top:82px;width:480px;filter:drop-shadow(0 0 10px rgba(255,214,90,.55)) drop-shadow(0 0 30px rgba(255,190,60,.3))}
#arc{position:absolute;filter:drop-shadow(0 0 10px rgba(255,170,40,.7))}
#spark{position:absolute;width:26px;height:26px;border-radius:50%;background:radial-gradient(circle,#fff 0%,#ffd766 30%,rgba(255,160,30,0) 70%);box-shadow:0 0 40px 12px rgba(255,170,40,.55)}
.ray{position:absolute;height:3px;border-radius:2px;background:linear-gradient(90deg,rgba(255,200,80,0),#ffcf5a);transform-origin:0 50%}
#slogan{position:absolute;left:0;right:0;top:455px;text-align:center;font-family:Cairo;font-weight:900;font-size:46px;direction:rtl}
#slogan span{color:#F28A1E}
/* phone */
#phone{position:absolute;left:515px;top:100px;width:250px;height:520px;border-radius:40px;background:#fff;border:5px solid #0c111b;
 box-shadow:0 0 0 2px #f5a33a,0 0 60px 10px rgba(242,138,30,.6);overflow:hidden;transform-origin:50% 0;font-family:Cairo,"Inter",sans-serif;color:#1a2433}
.pscreen{position:absolute;inset:0;direction:rtl;background:#fff;isolation:isolate}
.status{position:absolute;left:0;right:0;top:0;display:flex;justify-content:space-between;font:700 10px Inter;padding:11px 18px 0;direction:rtl;z-index:3}
.status u{display:inline-block;width:12px;height:6px;border-radius:2px;background:currentColor;margin-left:3px;text-decoration:none}
.sq{width:26px;height:26px;border-radius:8px;background:#fff;border:1px solid #e6e8ec;display:flex;align-items:center;justify-content:center;flex:none}
.sq svg,.nav svg,.srch svg{width:13px;height:13px;stroke:#1a2433;fill:none;stroke-width:2;stroke-linecap:round;stroke-linejoin:round}
.hd{position:absolute;left:14px;right:14px;top:30px;display:flex;align-items:center;gap:6px}
.hd b{font-weight:900;font-size:15px;flex:1}
.logoC{width:27px;height:27px;border-radius:50%;background:#f08a2c;display:flex;align-items:center;justify-content:center}
.logoC svg{stroke:#fff;fill:none;stroke-width:2;width:13px;height:13px}
.srch{position:absolute;left:14px;right:14px;height:34px;border-radius:11px;background:#f4f4f2;border:1px solid #e8e8e6;display:flex;align-items:center;padding:0 10px;font-size:10px;color:#9aa0a8;gap:7px}
.srch span.t{flex:1;white-space:nowrap;overflow:hidden}
.typed{direction:ltr;text-align:left;color:#1a2433;font:600 10.5px Inter}
.caret{display:inline-block;width:1.5px;height:12px;background:#f08a2c;margin-left:1px;vertical-align:-2px}
#suggs{position:absolute;left:14px;right:14px;top:110px;background:#fff;border-radius:11px;border:1px solid #e8e8e6;box-shadow:0 8px 20px rgba(0,0,0,.12);direction:ltr;z-index:5;overflow:hidden}
.sugg{font:500 9.5px Inter;color:#1a2433;padding:8px 10px;border-bottom:1px solid #f0f0f0;display:flex;align-items:center;gap:7px}
.sugg svg{width:10px;height:10px;stroke:#9aa0a8;fill:none;stroke-width:2}
.body{position:absolute;left:0;right:0;bottom:0;background:#f4f4f2}
.cats{display:flex;gap:8px;padding:12px 14px 0}
.cat{flex:1;text-align:center;font-size:7.5px;font-weight:700}
.cat i{display:block;height:38px;border-radius:11px;margin-bottom:4px}
.hero2{margin:10px 14px 0;border-radius:16px;background:#1a2433;color:#fff;padding:14px 14px 16px;position:relative;overflow:hidden}
.hero2:after{content:"";position:absolute;left:-20px;bottom:-40px;width:90px;height:90px;border-radius:50%;background:rgba(255,255,255,.07)}
.hero2 small{display:inline-block;border:1px solid #f08a2c;border-radius:10px;font-size:6.5px;padding:1px 7px;font-weight:700}
.hero2 h2{font-weight:900;font-size:19px;line-height:1.25;margin-top:6px}.hero2 h2 span{color:#f08a2c}
.hero2 p{font-size:7.5px;color:#c9ced6;margin-top:5px;line-height:1.6}
.sec{display:flex;justify-content:space-between;align-items:center;padding:10px 14px 0;font-weight:900;font-size:11px}
.sec a{font-size:7.5px;color:#f08a2c;font-weight:700}
.chips{position:absolute;left:14px;right:14px;top:110px;display:flex;gap:5px}
.chip{font-size:7.5px;font-weight:700;border:1px solid #e3e5e8;border-radius:11px;padding:3px 10px;background:#fff}
.chip.on{background:#1a2433;color:#fff;border-color:#1a2433}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:8px;padding:10px 14px}
.card{background:#fff;border-radius:12px;border:1px solid #ececea;overflow:hidden;position:relative;direction:rtl;font-family:Cairo,"Inter",sans-serif;color:#1a2433}
.card .pic{height:78px;background:#eef0f2;overflow:hidden}
.card .pic img{width:100%;height:100%;object-fit:cover;display:block}
.card .hrt{position:absolute;right:6px;top:6px;width:17px;height:17px;border-radius:50%;background:#fff;display:flex;align-items:center;justify-content:center}
.card .hrt svg{width:9px;height:9px;stroke:#b7bcc4;fill:none;stroke-width:2}
.card .bdg{position:absolute;left:6px;top:6px;font-size:6.5px;font-weight:700;color:#fff;border-radius:8px;padding:0 6px;line-height:13px}
.card b{display:block;font-size:8.5px;font-weight:700;padding:6px 8px 0;line-height:1.35}
.card em{display:block;font-style:normal;font-size:10px;font-weight:900;color:#f08a2c;padding:1px 8px 7px}
.card em s{color:#a9afb7;font-weight:400;font-size:7.5px;margin-right:4px}
.fcard{position:absolute;width:122px;box-shadow:0 10px 30px rgba(0,0,0,.5)}
.nav{position:absolute;left:0;right:0;bottom:0;height:46px;background:#fff;border-top:1px solid #ececea;display:flex;justify-content:space-around;align-items:center;font-size:6.5px;font-weight:700;color:#8a9099;z-index:4}
.nav div{text-align:center}.nav svg{display:block;margin:0 auto 2px;stroke:#8a9099}
.nav .on{color:#f08a2c}.nav .on svg{stroke:#f08a2c}
.nav .mid{width:34px;height:34px;border-radius:50%;background:#f08a2c;border:3px solid #fff;margin-top:-26px;display:flex;align-items:center;justify-content:center;box-shadow:0 3px 8px rgba(240,138,44,.5)}
.nav .mid svg{stroke:#fff;margin:0;width:15px;height:15px}
/* product detail */
#pd .hero{position:absolute;left:0;right:0;top:0;height:205px;background:#eef0f2;overflow:hidden}
#pd .hero img{width:100%;height:100%;object-fit:cover}
#pd .in{position:absolute;left:14px;right:14px;top:212px}
#pd h3{font-size:12.5px;font-weight:900;line-height:1.35}
#pd .rate{font:600 8px Inter;color:#8a9099;margin-top:2px}#pd .rate span{color:#f0a020}
#pd .pbox{margin-top:8px;border:1px solid #f6d9b8;background:#fff8f0;border-radius:11px;height:40px;display:flex;align-items:center;justify-content:space-between;padding:0 10px}
#pd .pbox b{font-weight:900;font-size:15px;color:#f08a2c}#pd .pbox b small{font-size:8px}
#pd .pbox i{font-style:normal;font-size:7.5px;font-weight:700;color:#2f8f5b;background:#e6f4ec;border-radius:9px;padding:2px 8px}
#pd .gbar{margin-top:8px;background:#1a2433;color:#fff;border-radius:11px;height:42px;display:flex;align-items:center;padding:0 10px;gap:8px}
#pd .gbar .ic{width:24px;height:24px;border-radius:50%;background:rgba(240,138,44,.25);display:flex;align-items:center;justify-content:center}
#pd .gbar svg{width:12px;height:12px;stroke:#f08a2c;fill:none;stroke-width:2}
#pd .gbar b{font-size:9px;font-weight:900;display:block}#pd .gbar small{font-size:7px;color:#c9ced6;display:block}
#pd .qty{margin-top:9px;display:flex;justify-content:space-between;align-items:center;font-size:9px;font-weight:700}
#pd .qty span{display:flex;align-items:center;gap:8px;font:700 10px Inter}
#pd .qty u{width:22px;height:22px;border-radius:7px;border:1px solid #e3e5e8;display:flex;align-items:center;justify-content:center;text-decoration:none;color:#8a9099}
#pd .qty u.p{border-color:#f08a2c;color:#f08a2c}
#pd .bot{position:absolute;left:0;right:0;bottom:0;height:58px;background:#fff;border-top:1px solid #ececea;display:flex;align-items:center;padding:0 14px;gap:10px}
#b1{flex:1;height:36px;border-radius:11px;background:#f08a2c;color:#fff;font-weight:900;font-size:11px;display:flex;align-items:center;justify-content:center;box-shadow:0 4px 10px rgba(240,138,44,.4)}
#pd .tot{font-size:7px;color:#8a9099;line-height:1.3}#pd .tot b{display:block;font-size:11px;color:#1a2433;font-weight:900}
#placed{position:absolute;left:14px;right:14px;top:408px;height:36px;border-radius:18px;background:#1a2433;color:#fff;display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:700;z-index:6;box-shadow:0 6px 16px rgba(0,0,0,.3)}
#placed i{width:15px;height:15px;border-radius:50%;background:#22c55e;margin-left:7px;position:relative}
#placed i:after{content:"";position:absolute;left:5px;top:2px;width:3.5px;height:7.5px;border:solid #fff;border-width:0 2px 2px 0;transform:rotate(45deg)}
/* map */
#mapWrap{position:absolute;inset:0;perspective:900px;overflow:hidden;background:#0b0f14}
#map{position:absolute;left:-600px;top:-520px;width:2600px;height:1700px;transform-origin:50% 50%;
 background:
 repeating-linear-gradient(90deg,rgba(120,190,220,.55) 0 5px,transparent 5px 260px),
 repeating-linear-gradient(0deg,rgba(120,190,220,.55) 0 5px,transparent 5px 200px),
 repeating-linear-gradient(90deg,#0d1218 0 14px,transparent 14px 65px),
 repeating-linear-gradient(0deg,#0d1218 0 14px,transparent 14px 50px),#3a424c}
#map svg{position:absolute;inset:0}
#mapShade{position:absolute;inset:0;background:radial-gradient(ellipse at 50% 55%,rgba(0,0,0,0) 30%,rgba(0,0,0,.75) 100%)}
#track{position:absolute;left:60px;top:44px;width:400px;border-radius:14px;background:rgba(24,26,30,.92);border:1px solid #333;padding:16px 20px 16px}
#track h4{font-size:22px;font-weight:700}#track p{font-size:14px;color:#bbb;margin-top:2px}
#bar{position:relative;height:4px;background:#444;border-radius:2px;margin:18px 6px 8px}
#barFill{position:absolute;left:0;top:0;height:4px;background:#f5a33a;border-radius:2px}
.dot{position:absolute;top:-5px;width:14px;height:14px;border-radius:50%;margin-left:-7px;background:#555}
.lbls{display:flex;justify-content:space-between;font-size:11.5px;font-weight:600;color:#ddd}
/* box */
#table{position:absolute;left:0;right:0;bottom:0;height:300px;background:linear-gradient(180deg,#2a1c0c 0%,#120c05 70%,#080604 100%)}
#wall{position:absolute;left:0;right:0;top:0;height:420px;background:radial-gradient(ellipse 50% 80% at 50% 80%,rgba(190,120,40,.38),rgba(20,14,8,0) 70%),#0d0b08}
#door{position:absolute;left:820px;top:60px;width:240px;height:360px;border:6px solid rgba(90,70,45,.5);border-bottom:none;background:rgba(40,30,18,.4)}
#boxG{position:absolute;left:440px;top:230px;width:400px;height:330px}
#shadow{position:absolute;left:-10px;top:292px;width:440px;height:60px;border-radius:50%;background:radial-gradient(ellipse,rgba(0,0,0,.75),rgba(0,0,0,0) 70%)}
#barc{position:absolute;left:119px;top:146px;width:156px;filter:drop-shadow(0 1px 2px rgba(90,40,0,.55))}
#bword{position:absolute;left:64px;top:184px;width:220px;filter:drop-shadow(0 1px 2px rgba(90,40,0,.45))}
#notif{position:absolute;left:330px;top:40px;width:620px;border-radius:18px;background:rgba(30,32,36,.94);border:1px solid #3a3a3a;padding:14px 18px;display:flex;align-items:center}
#notif .ic{width:44px;height:44px;border-radius:11px;background:#1a2433;margin-right:14px;position:relative;flex:none}
#notif .ic img{position:absolute;left:6px;top:16px;width:32px}
#notif small{font-size:12px;color:#aaa;display:block}#notif b{font-size:17px;display:block}#notif span{font-size:14px;color:#ccc}
/* type scenes */
.big{position:absolute;left:0;right:0;text-align:center;font-weight:700;color:#f6ead2;letter-spacing:-1px}
#ul{position:absolute;height:6px;border-radius:3px;background:linear-gradient(90deg,#ffd43b,#f28a1e);box-shadow:0 0 14px rgba(255,180,50,.8)}
</style></head>
<body><div id="stage">
<div id="glow"></div>

<div class="scene" id="sMap">
 <div id="mapWrap"><div id="map"><svg viewBox="0 0 2600 1700">
  <polyline id="routeG" points="300,1002 822,1002 822,802 1342,802 1342,1002 1862,1002 1862,602 2100,602" fill="none" stroke="rgba(255,170,40,.45)" stroke-width="26" stroke-linecap="round" stroke-linejoin="round"/>
  <polyline id="route" points="300,1002 822,1002 822,802 1342,802 1342,1002 1862,1002 1862,602 2100,602" fill="none" stroke="#ffc233" stroke-width="9" stroke-linecap="round" stroke-linejoin="round"/>
  <circle cx="2100" cy="602" r="26" fill="#ff9a1f"/><circle cx="2100" cy="602" r="60" fill="rgba(255,150,30,.3)"/>
  <circle id="van" r="16" fill="#fff"/>
 </svg></div><div id="mapShade"></div></div>
 <div id="track"><h4>Arriving today</h4><p>Out for delivery &middot; pay on delivery</p>
  <div id="bar"><div id="barFill"></div><div class="dot" style="left:0"></div><div class="dot" style="left:33%"></div><div class="dot" style="left:66%"></div><div class="dot" style="left:100%"></div></div>
  <div class="lbls"><span>Ordered</span><span>Shipped</span><span>Out for delivery</span><span>Delivered</span></div></div>
</div>

<div class="scene" id="sBox">
 <div id="wall"></div><div id="door"></div><div id="table"></div>
 <div id="boxG"><div id="shadow"></div>
  <svg width="400" height="330" viewBox="0 0 400 330" style="position:absolute;left:0;top:0">
   <defs><linearGradient id="f" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#f9931f"/><stop offset="1" stop-color="#e2740f"/></linearGradient>
   <linearGradient id="tp" x1="0" y1="0" x2="1" y2="1"><stop offset="0" stop-color="#ffc061"/><stop offset="1" stop-color="#ffa433"/></linearGradient></defs>
   <polygon points="20,70 110,14 386,14 330,70" fill="url(#tp)"/>
   <polygon points="330,70 386,14 386,250 330,318" fill="#c25f0a"/>
   <rect x="20" y="70" width="310" height="248" fill="url(#f)"/>
   <polygon points="150,70 172,14 208,14 196,70" fill="rgba(255,235,190,.55)"/>
   <rect x="150" y="70" width="46" height="40" fill="rgba(255,225,170,.35)"/>
  </svg>
  <img id="barc" src="mark.png"><img id="bword" src="word.png"></div>
 <div id="notif"><div class="ic"><img src="mark.png"></div><div><small>maamora &middot; now</small><b>Delivered</b><span>Your order has arrived. Pay on delivery.</span></div></div>
</div>

<div class="scene" id="sPhone">
 <div id="floats"></div>
 <div id="phone">
  <div class="pscreen" id="pH">
   <div class="status"><span>9:41</span><span><u></u><u style="opacity:.4"></u></span></div>
   <div class="hd"><div class="logoC"><svg viewBox="0 0 24 24"><path d="M5 8h14l-1 12H6z"/><path d="M9 8a3 3 0 016 0"/></svg></div><b>معمورة</b><div class="sq"><svg viewBox="0 0 24 24"><path d="M5 8h14l-1 12H6z"/><path d="M9 8a3 3 0 016 0"/></svg></div><div class="sq"><svg viewBox="0 0 24 24"><path d="M6 9a6 6 0 0112 0c0 6 2 7 2 7H4s2-1 2-7"/><path d="M10 20a2 2 0 004 0"/></svg></div></div>
   <div class="srch" style="top:68px"><svg viewBox="0 0 24 24"><circle cx="11" cy="11" r="7"/><path d="M21 21l-4.5-4.5"/></svg><span class="t" id="ph1">ابحث عمّا تحتاجه...</span><div class="sq" style="width:20px;height:20px;border-radius:6px"><svg viewBox="0 0 24 24"><path d="M5 7h14M8 12h8M10 17h4"/></svg></div></div>
   <div id="suggs"><div class="sugg"><svg viewBox="0 0 24 24"><circle cx="11" cy="11" r="7"/><path d="M21 21l-4.5-4.5"/></svg>AI Companion Robots</div><div class="sugg"><svg viewBox="0 0 24 24"><circle cx="11" cy="11" r="7"/><path d="M21 21l-4.5-4.5"/></svg>AI companion robot for desk</div><div class="sugg"><svg viewBox="0 0 24 24"><circle cx="11" cy="11" r="7"/><path d="M21 21l-4.5-4.5"/></svg>AI robot pet</div></div>
   <div class="body" style="top:112px">
    <div class="cats"><div class="cat"><i style="background:#fde8ea"></i>مجوهرات</div><div class="cat"><i style="background:#e6f3ea"></i>ديكور</div><div class="cat"><i style="background:#fdf0dc"></i>الجمال</div><div class="cat"><i style="background:#e4ecfb"></i>ألعاب</div></div>
    <div class="hero2"><small>التجارة الاجتماعية · المغرب</small><h2>تسوّق معاً.<br><span>وفّر حتى 30%</span></h2><p>كوّن مجموعة مع أصدقائك واحصل على السعر المخفّض عند اكتمالها.</p></div>
    <div class="sec"><span>الأكثر رواجاً</span><a>عرض الكل</a></div>
   </div><div class="nav"><div class="on"><svg viewBox="0 0 24 24"><path d="M4 11l8-7 8 7v9H4z"/></svg>الرئيسية</div><div class=""><svg viewBox="0 0 24 24"><path d="M4 9h16v11H4zM4 9l2-5h12l2 5"/></svg>المتجر</div><div><div class="mid"><svg viewBox="0 0 24 24"><circle cx="9" cy="9" r="3.5"/><path d="M2.5 20a6.5 6.5 0 0113 0"/><circle cx="17" cy="8" r="2.5"/><path d="M17.5 14a5 5 0 014 6"/></svg></div><span style="color:#1a2433">معاً</span></div><div class=""><svg viewBox="0 0 24 24"><path d="M5 8h14l-1 12H6z"/><path d="M9 8a3 3 0 016 0"/></svg>السلة</div><div class=""><svg viewBox="0 0 24 24"><circle cx="12" cy="8" r="4"/><path d="M4 21a8 8 0 0116 0"/></svg>حسابي</div></div>
  </div>
  <div class="pscreen" id="pA">
   <div class="status"><span>9:41</span><span><u></u><u style="opacity:.4"></u></span></div>
   <div class="hd"><div class="sq"><svg viewBox="0 0 24 24"><path d="M14 6l-6 6 6 6"/></svg></div><b style="text-align:center">المتجر</b><div style="width:26px"></div></div>
   <div class="srch" style="top:66px;left:50px"><svg viewBox="0 0 24 24"><circle cx="11" cy="11" r="7"/><path d="M21 21l-4.5-4.5"/></svg><span class="t typed">AI Companion Robots</span></div>
   <div class="sq" style="position:absolute;left:14px;top:66px;width:30px;height:34px;border-radius:11px;background:#fdeedd;border-color:#fdeedd"><svg viewBox="0 0 24 24"><path d="M5 7h14M8 12h8M10 17h4"/></svg></div>
   <div class="chips"><span class="chip on">الكل</span><span class="chip">روبوتات</span><span class="chip">جماعي</span><span class="chip">مخفّض</span></div>
   <div class="body" style="top:140px"><div class="grid" id="grid"></div></div><div class="nav"><div class=""><svg viewBox="0 0 24 24"><path d="M4 11l8-7 8 7v9H4z"/></svg>الرئيسية</div><div class="on"><svg viewBox="0 0 24 24"><path d="M4 9h16v11H4zM4 9l2-5h12l2 5"/></svg>المتجر</div><div><div class="mid"><svg viewBox="0 0 24 24"><circle cx="9" cy="9" r="3.5"/><path d="M2.5 20a6.5 6.5 0 0113 0"/><circle cx="17" cy="8" r="2.5"/><path d="M17.5 14a5 5 0 014 6"/></svg></div><span style="color:#1a2433">معاً</span></div><div class=""><svg viewBox="0 0 24 24"><path d="M5 8h14l-1 12H6z"/><path d="M9 8a3 3 0 016 0"/></svg>السلة</div><div class=""><svg viewBox="0 0 24 24"><circle cx="12" cy="8" r="4"/><path d="M4 21a8 8 0 0116 0"/></svg>حسابي</div></div>
  </div>
  <div class="pscreen" id="pd">
   <div class="status"><span>9:41</span><span><u></u><u style="opacity:.4"></u></span></div>
   <div class="hero"><img src="product.jpg"></div>
   <div class="sq" style="position:absolute;right:14px;top:32px;z-index:3"><svg viewBox="0 0 24 24"><path d="M14 6l-6 6 6 6"/></svg></div>
   <div class="in">
    <h3>روبوت مرافق ذكي بالذكاء الاصطناعي</h3>
    <div class="rate"><span>★ 4.8</span>  AI Companion Robot</div>
    <div class="pbox"><b>1,490 <small>درهم</small></b><i>توصيل مجاني</i></div>
    <div class="gbar"><div class="ic"><svg viewBox="0 0 24 24"><circle cx="9" cy="9" r="3.5"/><path d="M2.5 20a6.5 6.5 0 0113 0"/><circle cx="17" cy="8" r="2.5"/><path d="M17.5 14a5 5 0 014 6"/></svg></div><div><b>اشترِ ضمن مجموعة ووفّر 25%</b><small>السعر الجماعي 1,118 درهم</small></div></div>
    <div class="qty"><span><u class="p">+</u>1<u>−</u></span>الكمية</div>
   </div>
   <div id="placed"><i></i>تم تأكيد طلبك · الدفع عند الاستلام</div>
   <div class="bot"><div id="b1">أضف للشنطة</div><div class="tot">الإجمالي<b>1,118 درهم</b></div></div>
  </div>
 </div>
</div>

<div class="scene" id="sT1"><div class="big" style="top:238px;font-size:104px;line-height:1.12">Everything<br><span id="yl">you love.</span></div><div id="ul"></div></div>
<div class="scene" id="sT2"><div id="ul2" style="position:absolute;height:6px;border-radius:3px;background:linear-gradient(90deg,#ffd43b,#f28a1e);opacity:0"></div><div class="big" id="a2a" style="top:300px;font-size:112px">From <span id="A1">A</span> to <span id="A2">A</span>.</div></div>

<div class="scene" id="sLogo" style="opacity:1">
 <div id="logo"><img id="logoWord" src="word.png"></div>
 <div id="slogan">شري مع <span>الجماعة</span></div>
</div>
<img id="arc" src="mark.png">
<div id="spark"></div><div id="rays"></div>
</div>
<script>
// ===== CONFIG: edit for each brand =====
const CFG={
  hasMark:true,          // false if the logo has no separate symbol (then word.png is the whole logo)
  markX:212/838,         // mark left edge, as a fraction of the wordmark width
  markW:595/838,         // mark width, as a fraction of the wordmark width
  markAspect:143/595,    // mark height / mark width
  markGap:2,             // px between mark bottom and wordmark top (negative = overlap)
  searchText:'AI Companion Robots'
};
const $=id=>document.getElementById(id);
const cl=x=>Math.max(0,Math.min(1,x));
const seg=(t,a,b)=>cl((t-a)/(b-a));
const io=x=>{x=cl(x);return x*x*(3-2*x)};
const eo=x=>{x=cl(x);return 1-Math.pow(1-x,3)};
const ei=x=>{x=cl(x);return x*x*x};
const lerp=(a,b,x)=>a+(b-a)*x;
const back=x=>{x=cl(x)-1;const s=1.7;return 1+(s+1)*x*x*x+s*x*x};
// products
const HEART='<svg viewBox="0 0 24 24"><path d="M12 20s-7-4.5-7-10a4 4 0 017-2 4 4 0 017 2c0 5.5-7 10-7 10z"/></svg>';
const P=[['روبوت مرافق ذكي · أبيض','1,490','',0,'جماعي','#f08a2c'],['روبوت مرافق ذكي · وردي','1,490','',300,'25%-','#2f8f5b'],['روبوت مكتبي تفاعلي','1,290','1,590',90,'',''],['روبوت مرافق · أخضر','1,390','',260,'جماعي','#f08a2c'],['روبوت مرافق ذكي · بنفسجي','1,490','',60,'',''],['روبوت مرافق للأطفال','990','',200,'15%-','#2f8f5b']];
const cardHTML=i=>`<div class="pic"><img src="product.jpg" style="filter:hue-rotate(${P[i][3]}deg)"></div><div class="hrt">${HEART}</div>${P[i][4]?`<span class="bdg" style="background:${P[i][5]}">${P[i][4]}</span>`:''}<b>${P[i][0]}</b><em>${P[i][1]} درهم${P[i][2]?`<s>${P[i][2]}</s>`:''}</em>`;
for(let i=0;i<4;i++){const d=document.createElement('div');d.className='card';d.innerHTML=cardHTML(i);$('grid').appendChild(d)}
const FP=[[-250,-40,-8],[-170,190,6],[190,-70,7],[230,150,-6],[-20,330,3],[40,-190,-4]];
FP.forEach((p,i)=>{const d=document.createElement('div');d.className='card fcard';d.innerHTML=cardHTML(i);$('floats').appendChild(d)});
const RAYS=[-150,-110,-70,-30,20,160,200,250];
RAYS.forEach(()=>{const r=document.createElement('div');r.className='ray';$('rays').appendChild(r)});
const route=$('route'),routeG=$('routeG');const RL=route.getTotalLength();
[route,routeG].forEach(r=>{r.style.strokeDasharray=RL});
// logo geometry: where the mark sits relative to the wordmark
const LG={x:400+480*CFG.markX,y:262+82-480*CFG.markW*CFG.markAspect+CFG.markGap,w:480*CFG.markW};
const arcH=w=>w*CFG.markAspect;
let A2A=null;
function arcSet(x,y,w,rev,op){const a=$('arc');a.style.left=x+'px';a.style.top=y+'px';a.style.width=w+'px';a.style.opacity=op;a.style.clipPath=`inset(-30px ${(1-rev)*100}% -30px -30px)`;}
function headPos(x,y,w,rev){ // point on arc stroke at reveal fraction
  const h=arcH(w);const u=rev*2-1;return [x+rev*w, y+h*0.12+ (u*u)*h*0.62];}
const TXT=CFG.searchText;
window.seek=function(t){
  if(!A2A){const s=$('sT2');s.style.opacity=1;const r1=$('A1').getBoundingClientRect(),r2=$('A2').getBoundingClientRect();
    const x1=r1.left+r1.width/2-18,x2=r2.left+r2.width/2+18;A2A={x:x1,w:x2-x1,y:r1.top+18-arcH(x2-x1)};s.style.opacity=0;}
  // ---------- global glow
  let g=0.9;
  if(t>7.2&&t<9.9)g=0; if(t>13.7&&t<17.4)g=0.25;
  $('glow').style.opacity=g;
  // ---------- LOGO intro (0-1.9) & outro (17.0-22.5)
  const sL=$('sLogo'),word=$('logoWord'),sp=$('spark');
  let arcOp=0,ax=LG.x,ay=LG.y,aw=LG.w,rev=1,spOp=0,rayK=0,hp=[0,0];
  let wordOp=0,wordSc=1,logoOp=0,slog=0;
  if(t<1.9){
    logoOp=1-seg(t,1.65,1.9);
    rev=io(seg(t,0.12,0.62)); arcOp=t>0.1?logoOp:0;
    // intro: arc drawn large in centre then settles
    const q=io(seg(t,0.62,0.95));
    aw=lerp(LG.w*1.25,LG.w,q); ax=640-aw/2+lerp(0,LG.x+LG.w/2-640,q); ay=lerp(300,LG.y,q);
    spOp=t>0.1&&t<0.7?1:0; rayK=seg(t,0.1,0.3)*(1-seg(t,0.5,0.75));
    hp=headPos(ax,ay,aw,rev);
    wordOp=seg(t,0.62,0.9)*logoOp; wordSc=lerp(1.12,1,eo(seg(t,0.62,1.1)));
  }else if(t>=15.25){
    logoOp=1;
    if(t<17.0){ // arc drawn over "From A to A"
      rev=io(seg(t,15.6,16.25)); ax=A2A.x;ay=A2A.y;aw=A2A.w;arcOp=t>15.58?1:0;
      spOp=(t>15.58&&t<16.3)?1:0; hp=headPos(ax,ay,aw,rev);
    }else{
      const q=io(seg(t,17.0,17.55));
      ax=lerp(A2A.x,LG.x,q);ay=lerp(A2A.y,LG.y,q);aw=lerp(A2A.w,LG.w,q);arcOp=1;
      wordOp=seg(t,17.45,17.8); wordSc=lerp(1.1,1,eo(seg(t,17.45,18.0)));
      slog=eo(seg(t,19.4,20.0));
    }
  }
  if(!CFG.hasMark){arcOp=0;spOp=0;rayK=0;if(t<1.9)wordOp=seg(t,0.2,0.6)*logoOp;else if(t>=16.9)wordOp=seg(t,17.0,17.5);
    const r2=$('a2a').getBoundingClientRect?document.querySelector('#a2a').getBoundingClientRect():null;const u2=io(seg(t,15.6,16.25));
    $('ul2').style.left=(640-260)+'px';$('ul2').style.top='440px';$('ul2').style.width=(520*u2)+'px';$('ul2').style.opacity=1;
    $('barc').style.display='none';}
  const zoom=t>=17.5?1+0.035*io(seg(t,17.5,22.5)):1;
  sL.style.opacity=(t<1.9||t>=17.0)?1:0;
  $('logo').style.transform=`scale(${zoom})`;$('logo').style.transformOrigin='240px 80px';
  word.style.opacity=wordOp;word.style.transform=`scale(${wordSc})`;
  $('slogan').style.opacity=slog;$('slogan').style.transform=`translateY(${(1-slog)*18}px)`;
  arcSet(ax,ay,aw,rev,arcOp);
  sp.style.opacity=spOp;sp.style.left=(hp[0]-13)+'px';sp.style.top=(hp[1]-13)+'px';
  [...$('rays').children].forEach((r,i)=>{const L=40+((i*37)%50);r.style.opacity=rayK;r.style.left=hp[0]+'px';r.style.top=hp[1]+'px';r.style.width=(L*rayK)+'px';r.style.transform=`rotate(${RAYS[i]}deg) translateX(${10+30*rayK}px)`;});
  // ---------- PHONE (1.75-7.3)
  const sP=$('sPhone');const pOp=seg(t,1.75,1.95)*(1-seg(t,7.05,7.3));sP.style.opacity=pOp;
  if(pOp>0){
    const rise=eo(seg(t,1.75,2.5));
    let sc=0.86,ty=(1-rise)*640+30,ox=0;
    // zoom into the search bar
    const z1=io(seg(t,2.5,3.0)), z1o=io(seg(t,3.9,4.3));
    sc=lerp(sc,3.0,z1); ty=lerp(ty,-70,z1);
    sc=lerp(sc,0.8,z1o); ty=lerp(ty,70,z1o);
    // grid close-up
    const z2=io(seg(t,4.7,5.1)); sc=lerp(sc,1.75,z2); ty=lerp(ty,-215,z2);
    // product page
    const z3=io(seg(t,5.25,5.6)); sc=lerp(sc,1.55,z3); ty=lerp(ty,-60,z3);
    const z4=io(seg(t,5.95,6.4)); sc=lerp(sc,2.2,z4); ty=lerp(ty,-530,z4);
    $('phone').style.transform=`translateY(${ty}px) scale(${sc})`;
    const n=Math.round(seg(t,2.85,3.7)*TXT.length);
    $('ph1').innerHTML=n>0?`<span class="typed" style="display:block">${TXT.slice(0,n)}<span class="caret" style="opacity:${(Math.floor(t*2.5)%2)?1:0.2}"></span></span>`:'ابحث عمّا تحتاجه...';
    const sg=seg(t,3.0,3.3)*(1-seg(t,3.75,3.9));$('suggs').style.opacity=sg;
    const res=seg(t,3.85,4.05);
    const pdq=seg(t,5.25,5.45);$('pd').style.opacity=pdq;$('pA').style.opacity=pdq>=1?0:res;$('pH').style.opacity=res>=1?0:1;
    const pr=Math.sin(seg(t,6.35,6.6)*Math.PI);
    $('b1').style.transform=`scale(${1-0.05*pr})`;$('b1').style.filter=`brightness(${1+0.25*pr})`;
    const pl=back(seg(t,6.6,6.9));$('placed').style.opacity=seg(t,6.6,6.75);$('placed').style.transform=`scale(${0.7+0.3*pl})`;
    // floating cards
    [...$('floats').children].forEach((c,i)=>{const a=eo(seg(t,4.0+i*0.04,4.45+i*0.04)),b=io(seg(t,4.6,4.85));
      const k=a*(1-b);c.style.opacity=k;const p=FP[i];
      c.style.left=(640-59+p[0]*(0.3+0.7*a)*(1-b))+'px';c.style.top=(250+p[1]*(0.3+0.7*a)*(1-b)+Math.sin(t*3+i)*6)+'px';
      c.style.transform=`rotate(${p[2]}deg) scale(${0.6+0.4*a})`;});
  }
  // ---------- MAP (7.15-9.95)
  const sM=$('sMap');const mOp=seg(t,7.15,7.4)*(1-seg(t,9.7,9.95));sM.style.opacity=mOp;
  if(mOp>0){const p=seg(t,7.3,9.6);const d=io(p);
    route.style.strokeDashoffset=RL*(1-d);routeG.style.strokeDashoffset=RL*(1-d);
    const pt=route.getPointAtLength(RL*d);$('van').setAttribute('cx',pt.x);$('van').setAttribute('cy',pt.y);
    const tilt=lerp(8,56,io(seg(t,7.2,8.0)));const sc=lerp(0.62,1.0,io(seg(t,7.2,8.2)));
    const px=lerp(0,-(pt.x-1300)*0.75,io(seg(t,7.6,8.4)));const py=-(pt.y-850)*0.4*io(seg(t,7.6,8.4));
    $('map').style.transform=`rotateX(${tilt}deg) scale(${sc}) translate(${px}px,${py}px)`;
    const tk=eo(seg(t,7.7,8.1));$('track').style.opacity=tk;$('track').style.transform=`translateY(${(1-tk)*-20}px)`;
    const f=lerp(0.33,0.66,seg(t,7.9,8.6))+0.3*seg(t,9.2,9.6);$('barFill').style.width=(cl(f)*100)+'%';
    [...document.querySelectorAll('#bar .dot')].forEach((dd,i)=>{dd.style.background=(f>=i*0.33-0.01)?'#f5a33a':'#555'});
  }
  // ---------- BOX (9.8-13.75)
  const sB=$('sBox');const bOp=seg(t,9.8,10.2)*(1-seg(t,13.4,13.75));sB.style.opacity=bOp;
  if(bOp>0){const s=eo(seg(t,9.9,10.75));const land=Math.sin(seg(t,10.75,11.05)*Math.PI);
    $('boxG').style.transform=`translate(${(1-s)*-900}px,${-land*10}px) scale(${lerp(1,1.07,io(seg(t,10.8,13.4)))})`;
    $('boxG').style.filter=`blur(${(1-s)*14+seg(t,13.2,13.6)*12}px)`;
    const r=io(seg(t,11.1,11.75));$('barc').style.clipPath=`inset(0 ${(1-r)*100}% 0 0)`;
    $('bword').style.opacity=seg(t,11.6,11.95);
    const nq=back(seg(t,12.35,12.75));$('notif').style.opacity=seg(t,12.35,12.5)*(1-seg(t,13.2,13.45));$('notif').style.transform=`translateY(${(1-nq)*-70}px)`;
  }
  // ---------- TEXT 1 (13.7-15.3)
  const s1=$('sT1');const o1=seg(t,13.7,14.0)*(1-seg(t,15.05,15.28));s1.style.opacity=o1;
  if(o1>0){const r=$('yl').getBoundingClientRect();const u=io(seg(t,14.25,14.8));
    $('ul').style.left=r.left+'px';$('ul').style.top=(r.bottom-6)+'px';$('ul').style.width=(r.width*u)+'px';
    s1.style.transform=`scale(${lerp(0.96,1.02,seg(t,13.7,15.3))})`;}
  // ---------- TEXT 2 (15.25-17.2)
  const s2=$('sT2');const o2=seg(t,15.25,15.5)*(1-seg(t,16.85,17.1));s2.style.opacity=o2;
};
seek(0);
</script></body></html>
```

## Template file: `scene.html`

```html
<!DOCTYPE html>
<html><head><meta charset="utf-8">
<link rel="stylesheet" href="emoji/index.css">
<style>
@font-face{font-family:"Barlow Condensed";font-weight:600;src:url(fonts/barlow-condensed-latin-600-normal.woff2)}
*{margin:0;padding:0;box-sizing:border-box}
html,body{width:1080px;height:1920px;overflow:hidden;background:#000}
#world{position:absolute;left:0;top:0;width:1080px;height:1920px;transform-origin:50% 55%}
#bg{position:absolute;left:-30px;top:-30px;width:1140px;height:1340px;object-fit:cover}
#desk{position:absolute;left:-30px;right:-30px;top:1290px;bottom:-30px;background:linear-gradient(180deg,#3a2a1c 0%,#241910 35%,#120c07 100%)}
#desk:before{content:"";position:absolute;inset:0;background:repeating-linear-gradient(92deg,rgba(255,255,255,.025) 0 3px,transparent 3px 70px)}
#monitor{position:absolute;left:84px;top:794px;width:912px;height:526px;border-radius:14px;background:#0a0a0c;box-shadow:0 30px 60px rgba(0,0,0,.55),0 0 0 2px #1d1d21}
#screen{position:absolute;left:6px;top:6px;width:900px;height:506px;overflow:hidden;border-radius:9px;background:#000}
#screen iframe{width:1280px;height:720px;border:0;transform:scale(.703125);transform-origin:0 0}
#glowM{position:absolute;left:60px;top:1300px;width:960px;height:120px;background:radial-gradient(ellipse at 50% 0,rgba(242,138,30,.22),rgba(0,0,0,0) 70%)}
#lid{position:absolute;left:196px;top:1262px;width:688px;height:446px;border-radius:20px 20px 6px 6px;background:#0b0b0d;box-shadow:0 0 0 2px #26262b,0 20px 40px rgba(0,0,0,.6)}
#ed{position:absolute;left:10px;top:10px;right:10px;bottom:12px;border-radius:10px;background:#17171b;overflow:hidden;font:600 9px Inter,sans-serif;color:#9a9aa3}
#ed .bar{height:18px;background:#101013;display:flex;align-items:center;gap:10px;padding:0 10px}
#ed .bar i{width:7px;height:7px;border-radius:50%;background:#ff5f57}#ed .bar i+i{background:#febc2e}#ed .bar i+i+i{background:#28c840}
#bins{position:absolute;left:12px;top:30px;display:grid;grid-template-columns:repeat(3,34px);gap:6px}
#bins b{height:22px;border-radius:3px;background:#2c2c33}
#pv{position:absolute;left:254px;top:28px;width:160px;height:90px;background:#000;overflow:hidden;border:1px solid #2c2c33}
#pv iframe{width:1280px;height:720px;border:0;transform:scale(.125);transform-origin:0 0}
#tl{position:absolute;left:0;right:0;top:136px;bottom:22px;background:#121215;border-top:1px solid #2a2a30;padding-top:20px}
.trk{height:19px;margin:3px 0 0 44px;position:relative;background:#19191d}
.trk span{position:absolute;top:1px;bottom:1px;border-radius:2px}
#ph{position:absolute;top:140px;bottom:22px;width:2px;background:#ff4d4d}
#dock{position:absolute;left:60px;right:60px;bottom:4px;height:14px;display:flex;gap:4px;justify-content:center}
#dock i{width:12px;height:12px;border-radius:3px}
#deck{position:absolute;left:96px;top:1706px;width:888px;height:260px;background:linear-gradient(180deg,#1b1b1f,#0c0c0e);clip-path:polygon(11.5% 0,88.5% 0,100% 100%,0 100%)}
#keys{position:absolute;left:120px;right:120px;top:26px;bottom:0;transform:perspective(600px) rotateX(38deg);transform-origin:50% 0;
 background:repeating-linear-gradient(90deg,#060607 0 42px,transparent 42px 48px),repeating-linear-gradient(0deg,#060607 0 40px,transparent 40px 46px);opacity:.9}
.cap{position:absolute;left:0;right:0;margin:0 auto;width:fit-content;background:#fff;color:#000;border-radius:22px;white-space:nowrap;display:flex;align-items:center;box-shadow:0 4px 14px rgba(0,0,0,.25)}
.cap .e{font-family:"Noto Color Emoji";font-weight:400;margin-left:14px}
#hook{top:652px;height:116px;padding:0 34px;font:600 78px "Barlow Condensed","Arial Narrow",sans-serif}
#cta{top:1512px;height:104px;padding:0 32px;font:700 40px Poppins,Inter,sans-serif}
#hook .e{font-size:70px}#cta .e{font-size:38px}
</style></head>
<body>
<div id="world">
 <img id="bg" src="bg.png">
 <div id="desk"></div>
 <div id="monitor"><div id="screen"><iframe src="ad.html"></iframe></div></div>
 <div id="glowM"></div>
 <div id="lid"><div id="ed">
   <div class="bar"><i></i><i></i><i></i><span>Ad edit &ndash; timeline v1</span></div>
   <div id="bins"><b></b><b></b><b></b><b></b><b></b><b></b></div>
   <div id="pv"><iframe src="ad.html"></iframe></div>
   <div id="tl"></div><div id="ph"></div>
   <div id="dock"></div>
 </div></div>
 <div id="deck"><div id="keys"></div></div>
</div>
<!-- ===== CAPTIONS: edit the two texts ===== -->
<div class="cap" id="hook">Made 100% by AI<span class="e">🤯</span></div>
<div class="cap" id="cta">Comment “AD” to get the prompt<span class="e">⬇️</span></div>
<script>
// ===== SCENE CONFIG =====
const SCENE={ bgPosition:'52% 40%',   // which part of the background image stays in view (CSS object-position)
              drift:1 };              // 0 = locked camera, 1 = gentle handheld movement
document.getElementById('bg').style.objectPosition=SCENE.bgPosition;
const cols=['#ff4fa3','#ffd43b','#39d98a','#4dabf7','#ff4fa3','#b197fc'];
const tl=document.getElementById('tl');let seed=7;const rnd=()=>(seed=(seed*16807)%2147483647)/2147483647;
for(let r=0;r<8;r++){const t=document.createElement('div');t.className='trk';let x=0;
  while(x<96){const w=6+rnd()*20;const s=document.createElement('span');s.style.left=x+'%';s.style.width=Math.min(w,98-x)+'%';
    s.style.background=r<2?'#3a2c1a':cols[(r+Math.floor(rnd()*3))%cols.length];s.style.opacity=r<2?1:.9;t.appendChild(s);x+=w+1+rnd()*4;}
  tl.appendChild(t);}
const dock=document.getElementById('dock');for(let i=0;i<22;i++){const d=document.createElement('i');d.style.background=`hsl(${(i*47)%360} 70% 55%)`;dock.appendChild(d);}
window.seekScene=function(t,dur){
  document.getElementById('ph').style.left=(54+ (t/dur)*600)+'px';
  const k=SCENE.drift;
  const dx=(Math.sin(t*0.9)*5+Math.sin(t*2.3+1)*2)*k, dy=(Math.cos(t*0.7)*6+Math.sin(t*1.9)*2)*k, rot=Math.sin(t*0.5)*0.25*k;
  document.getElementById('world').style.transform=`translate(${dx}px,${dy}px) rotate(${rot}deg) scale(${1.03})`;
};
seekScene(0,22.5);
</script></body></html>
```

## Template file: `render.js`

```js
// node render.js snap 1.2 5.8 12  -> stills s_<t>.jpg  |  node render.js full  -> out.mp4 (+ out_4x5.mp4)
// Optional in the work folder: timing.json (retime the ad to a voiceover) and voice.mp3 (merged into the output).
const path = require('path'), fs = require('fs'), { spawn } = require('child_process');
let pw; for (const p of ['playwright', 'playwright-core', '/opt/npm-tools/node_modules/playwright']) { try { pw = require(p); break; } catch (e) {} }
if (!pw) { console.error('playwright not found: npm i playwright-core'); process.exit(1); }
const CHROME = process.env.CHROME || ['/usr/bin/google-chrome', '/usr/bin/chromium', '/usr/bin/chromium-browser'].find(p => fs.existsSync(p));
const FPS = 30;
// timing.json: {"duration": 26.6, "anchors": [[videoSecond, adSecond], ...]} maps output time to ad time.
let DUR = 22.5, ANCH = null;
const TF = path.resolve(__dirname, 'timing.json');
if (fs.existsSync(TF)) { const j = JSON.parse(fs.readFileSync(TF, 'utf8')); DUR = j.duration; ANCH = j.anchors; }
const tmap = v => { if (!ANCH) return v; for (let k = 1; k < ANCH.length; k++) if (v <= ANCH[k][0]) { const a = ANCH[k - 1], b = ANCH[k]; return a[1] + (b[1] - a[1]) * (v - a[0]) / (b[0] - a[0]); } return ANCH[ANCH.length - 1][1]; };
(async () => {
  const browser = await pw.chromium.launch({ executablePath: CHROME, args: ['--no-sandbox', '--disable-gpu', '--allow-file-access-from-files'] });
  const page = await browser.newPage({ viewport: { width: 1080, height: 1920 } });
  await page.goto('file://' + path.resolve(__dirname, 'scene.html'));
  await page.waitForTimeout(1500);
  for (const f of page.frames()) await f.evaluate(() => document.fonts.ready);
  // Emoji in captions (<span class="e">) are drawn once on a blank page and swapped in as images,
  // because colour-emoji webfonts do not always paint inside file:// pages.
  const spans = await page.evaluate(() => [...document.querySelectorAll('.e')].map(e => [e.textContent, parseFloat(getComputedStyle(e).fontSize)]));
  if (spans.length) {
    const ep = await browser.newPage({ viewport: { width: 400, height: 300 }, deviceScaleFactor: 2 });
    for (let i = 0; i < spans.length; i++) {
      await ep.setContent(`<link rel="stylesheet" href="file://${path.resolve(__dirname, 'emoji/index.css')}"><span id="x" style="font:400 ${spans[i][1]}px 'Noto Color Emoji';line-height:1.2">${spans[i][0]}</span>`);
      await ep.waitForTimeout(700);
      const b64 = (await ep.locator('#x').screenshot({ omitBackground: true, type: 'png' })).toString('base64');
      await page.evaluate(([k, d, h]) => { const e = document.querySelectorAll('.e')[k]; e.innerHTML = `<img src="data:image/png;base64,${d}" style="height:${h * 1.2}px;display:block">`; }, [i, b64, spans[i][1]]);
    }
    await ep.close();
  }
  const seek = async t => {
    for (const f of page.frames()) await f.evaluate(([tt, ta, d]) => { if (window.seek) window.seek(ta); if (window.seekScene) window.seekScene(tt, d); }, [t, tmap(t), DUR]);
  };
  const mode = process.argv[2] || 'full';
  if (mode === 'snap') {
    for (const t of process.argv.slice(3)) { await seek(parseFloat(t)); await page.screenshot({ path: path.resolve(__dirname, `s_${t}.jpg`), type: 'jpeg', quality: 88 }); }
  } else {
    const tmp = path.resolve(__dirname, 'tmp_out.mp4'), out = path.resolve(__dirname, 'out.mp4');
    const ff = spawn('ffmpeg', ['-y', '-loglevel', 'error', '-f', 'image2pipe', '-framerate', String(FPS), '-c:v', 'mjpeg', '-i', '-',
      '-vf', 'scale=out_color_matrix=bt709:out_range=tv', '-c:v', 'libx264', '-preset', 'veryfast', '-crf', '18', '-pix_fmt', 'yuv420p',
      '-colorspace', 'bt709', '-color_primaries', 'bt709', '-color_trc', 'bt709', '-color_range', 'tv', '-movflags', '+faststart', tmp], { stdio: ['pipe', 'inherit', 'inherit'] });
    for (let f = 0; f < Math.round(FPS * DUR); f++) {
      await seek(f / FPS);
      const buf = await page.screenshot({ type: 'jpeg', quality: 92 });
      if (!ff.stdin.write(buf)) await new Promise(r => ff.stdin.once('drain', r));
    }
    ff.stdin.end();
    const code = await new Promise(r => ff.on('close', r));
    if (code !== 0) { process.exitCode = 1; await browser.close(); return; }
    const voice = path.resolve(__dirname, 'voice.mp3');
    if (fs.existsSync(voice)) {   // add the voiceover
      const tmp2 = path.resolve(__dirname, 'tmp_voice.mp4');
      const c2 = await new Promise(r => spawn('ffmpeg', ['-y', '-loglevel', 'error', '-i', tmp, '-i', voice, '-map', '0:v', '-map', '1:a', '-c:v', 'copy', '-c:a', 'aac', '-b:a', '192k', '-movflags', '+faststart', tmp2], { stdio: 'inherit' }).on('close', r));
      if (c2 !== 0) { process.exitCode = 1; await browser.close(); return; }
      fs.renameSync(tmp2, tmp);
    }
    fs.renameSync(tmp, out);
    // 4:5 copy (1080x1350) for Instagram/Facebook feed posts, which refuse 9:16 outside Reels
    const t45 = path.resolve(__dirname, 'tmp_4x5.mp4');
    const c3 = await new Promise(r => spawn('ffmpeg', ['-y', '-loglevel', 'error', '-i', out, '-vf', 'crop=1080:1350:0:330', '-c:v', 'libx264', '-preset', 'veryfast', '-crf', '18', '-pix_fmt', 'yuv420p', '-c:a', 'copy', '-movflags', '+faststart', t45], { stdio: 'inherit' }).on('close', r));
    if (c3 === 0) fs.renameSync(t45, path.resolve(__dirname, 'out_4x5.mp4')); else process.exitCode = 1;
  }
  await browser.close();
})();
```

## Template file: `align.py`

```python
# python3 align.py voice.mp3 script.txt   -> writes timing.json and prints where each line starts
# script.txt: the voiceover, one line per beat, in the order of BEATS below (10 lines).
# Finds pauses in the audio with ffmpeg, estimates each line's start from its share of the text,
# and snaps that estimate to the nearest pause. No transcription service is needed.
import json, re, subprocess, sys

BEATS = [0, 1.9, 3.9, 5.95, 6.35, 7.15, 9.85, 13.7, 15.25, 17.0]   # ad second where each line's beat starts
NAMES = ['logo', 'search', 'results + product', 'group bar', 'order confirmed', 'map', 'box',
         'text line 1', 'text line 2', 'final logo + slogan']

audio, script = sys.argv[1], sys.argv[2]
lines = [l.strip() for l in open(script, encoding='utf-8') if l.strip()]
if len(lines) != len(BEATS):
    sys.exit(f'script.txt has {len(lines)} lines; it needs {len(BEATS)}, one per beat: ' + ', '.join(NAMES))

def probe(cmd):
    return subprocess.run(cmd, capture_output=True, text=True).stderr

dur = float(subprocess.run(['ffprobe', '-v', 'error', '-show_entries', 'format=duration', '-of', 'csv=p=0', audio],
                           capture_output=True, text=True).stdout)
log = probe(['ffmpeg', '-hide_banner', '-nostats', '-i', audio, '-af', 'silencedetect=noise=-35dB:d=0.15', '-f', 'null', '-'])
starts = [float(x) for x in re.findall(r'silence_start: ([0-9.]+)', log)]
ends = [float(x) for x in re.findall(r'silence_end: ([0-9.]+)', log)]
speech_start = ends[0] if starts and starts[0] < 0.05 and ends else 0.0          # skip leading silence
speech_end = starts[-1] if starts and (not ends or starts[-1] > ends[-1]) else dur  # skip trailing silence
pauses = [e for e in ends if speech_start + 0.3 < e < speech_end - 0.3]          # candidate line starts

w = [len(re.sub(r'\s', '', l)) for l in lines]
total = sum(w)
est, acc = [], 0
for x in w:
    est.append(speech_start + (speech_end - speech_start) * acc / total)
    acc += x

# Choose one pause per line break, all together: stay close to the estimates and prefer long pauses
# (sentence breaks are longer than pauses inside a sentence).
plen = {}
for st, en in zip([x for x in starts if x > 0.05] if starts and starts[0] < 0.05 else starts, ends[1:] if starts and starts[0] < 0.05 else ends):
    plen[en] = en - st
cands = sorted(p for p in pauses)
n, m = len(lines) - 1, len(cands)
seg = (speech_end - speech_start) / len(lines)
def cost(i, p):   # line i+1 starting at pause p
    return ((p - est[i + 1]) / seg) ** 2 - 2.0 * min(plen.get(p, 0.15), 0.6)
INF = float('inf')
best = [[INF] * (m + 1) for _ in range(n + 1)]
back = [[None] * (m + 1) for _ in range(n + 1)]
for j in range(m + 1): best[0][j] = 0.0
for i in range(1, n + 1):
    for j in range(i, m + 1):
        take = best[i - 1][j - 1] + cost(i - 1, cands[j - 1]) if best[i - 1][j - 1] < INF else INF
        skip = best[i][j - 1]
        best[i][j], back[i][j] = (take, 'take') if take <= skip else (skip, 'skip')
if m >= n:
    picks, i, j = [], n, m
    while i > 0:
        if back[i][j] == 'take': picks.append(cands[j - 1]); i -= 1
        j -= 1
    found = [speech_start] + picks[::-1]
else:
    found = est[:]
    print(f'  only {m} pauses found for {n} line breaks: used estimates from text length; check by ear')
D = round(dur + 0.5, 2)
L = found[-1]
anchors = [[round(found[i], 2), BEATS[i]] for i in range(len(lines))]
anchors += [[round(L + 0.55, 2), 17.8], [round(L + 1.35, 2), 20.0], [D, 22.5]]
json.dump({'duration': D, 'anchors': anchors}, open('timing.json', 'w'))
for i, l in enumerate(lines):
    print(f'{found[i]:6.2f}s  {NAMES[i]:<20} {l[:50]}')
print(f'duration {D}s -> timing.json')
```