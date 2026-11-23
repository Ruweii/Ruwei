# Paid "3D Animated Shorts" skill (Mr omi, Skool) and community alternatives

Research date: 2026-11-23. Network note: skool.com, x.com, waveweaver.io and sebastianantony.com could not be fetched from this session (not on the network allowlist). Facts about those pages come from search-engine snippets and summaries, not from direct reads. YouTube page bodies also did not render; video metadata came from YouTube's oEmbed endpoint.

## 1. Who is the creator, which Skool community, and what is included?

### Takeaway
The creator is **"Mr omi"**: YouTube channel **@mr_omix** (channel ID UCRj1ZF-nLuCK5NFFo2Qkzyg), GitHub **mromidev**. Video _fIoJ_ywGfY is "How to Make Ultra-Realistic 3D Animated Shorts Using Claude (Full Workflow)". The paid skill is sold through his Skool community. Two Skool URLs exist. One, **skool.com/mr-omi-1113**, matches the $45/month price. The other, **"AI Creator Lab by Mr Omi" at skool.com/mromi**, was listed at $55/month and is the page that lists the 3D Animated Shorts skill as included. The price probably went up, since the pages say it "rises as new skills drop".

### Cited Findings
- oEmbed for _fIoJ_ywGfY returns title "How to Make Ultra-Realistic 3D Animated Shorts Using Claude (Full Workflow)", author "Mr omi", author_url https://www.youtube.com/@mr_omix — [YouTube oEmbed](https://www.youtube.com/oembed?url=https://www.youtube.com/watch?v=_fIoJ_ywGfY&format=json); [video](https://www.youtube.com/watch?v=_fIoJ_ywGfY)
- A search snippet of the video description says the creator gave Claude a channel's 50 most-viewed shorts and Claude measured all 47,709 frames to learn the style. The video was about 19 days old when indexed. — [YouTube](https://www.youtube.com/watch?v=_fIoJ_ywGfY) (snippet only)
- GitHub profile "Mr omi" (mromidev) links the YouTube channel UCRj1ZF-nLuCK5NFFo2Qkzyg and the Skool page https://www.skool.com/mr-omi-1113/about. It has 2 public repos and about 289 followers. — [GitHub mromidev](https://github.com/mromidev)
- Search snippet of skool.com/mr-omi-1113: titled "Mr Omi", **$45/month**, advertised as "$1.50/day", price rises as new skills are added, the 3D shorts skill shown as "coming soon" in the roadmap, about 28 members — [Skool mr-omi-1113](https://www.skool.com/mr-omi-1113/about) (snippet, not fetched)
- Search snippet of skool.com/mromi, "AI Creator Lab by Mr Omi": **$55/month**, about 301 members, includes a "3D Animated Shorts" skill for realistic 3D body/science shorts, and lists upcoming items such as Roblox animated shorts — [Skool mromi](https://www.skool.com/mromi/about) (snippet, not fetched)
- A personal blog post (about 15 days old when indexed) by someone who made a 3D short says they paid **$45/month** for "Mr. Oni's" Skool. The post says the 3D animated shorts skill "only works through his Skool connector", meaning it is delivered through a connector rather than as a downloadable SKILL.md. — [sebastianantony.com](https://www.sebastianantony.com/making-3d-animated-shorts/) (snippet only; the post spells the name "Oni")
- A third-party article says there is no official Claude–Skool connector because Skool has no public API, so any "Skool connector" is the creator's own or third-party tooling — [usecarly.com](https://www.usecarly.com/blog/claude-skool-integration/)
- A sister-name community, "Mr Miro's AI Cartoon Lab" (skool.com/mrmiro), also appeared in results. It is a different creator; do not confuse the two. — [Skool mrmiro](https://www.skool.com/mrmiro/about)

### Inferences
- The "$45/month" the user saw matches the mr-omi-1113 listing and the blog post. The $55 figure on skool.com/mromi is probably a later price after the 3D skill shipped, consistent with the "price rises as new skills drop" model. The mr-omi-1113 URL may be an older or duplicate group slug.
- "Delivered via connector" suggests the paid skill is gated behind a remote MCP/connector tied to membership, not a file you keep. If so, it stops working when you cancel. This claim rests on a single blog snippet.
- His free Blender repo uses a custom TCP bridge (`bridge.py` on 127.0.0.1:9876) rather than the standard blender-mcp. The paid skill likely uses the same approach, but this is unverified.

### Gaps
- Exact current price and a full list of what the Skool classroom contains (lessons, scripts, assets, support) could not be read; skool.com was blocked here.
- Whether "stitches" and "how long are your blood vessels" are bundled as scenes or templates in the paid skill is unconfirmed.
- No direct confirmation of the shirt reading "Mr. Omi", though every identifier (name, channel, video title) matches.

## 2. Free earlier skills on GitHub, and whether nocodehack/make-short is his

### Takeaway
Yes, Mr omi publishes free skills. **mromidev/claude-animated-shorts** is the cartoon-shorts skill from his previous video and is very popular (about 4.6k stars, about 963 forks). **mromidev/claude-blender-skills** holds his realistic Blender workflows (drone, forest, car films), but not the anatomy/science shorts. **nocodehack/make-short** belonged to a separate GitHub account, "nocodehack" (YouTube @nocodehack, "NoCodeHack"). No evidence ties it to Mr omi. The whole nocodehack account now returns 404.

### Cited Findings
- `claude-animated-shorts`: "Free Claude skill: viral animated story shorts (2D characters in a 3D world) with three.js + Remotion", about 4.6k stars and 963 forks — [GitHub mromidev](https://github.com/mromidev); [repo](https://github.com/mromidev/claude-animated-shorts)
- That skill makes 55–70 second vertical shorts with ink outlines, a painted town, lens blur and gliding cameras. It renders locally with no AI video generator. The stack is three.js in Remotion (`@remotion/three`), Node, Python 3 and ffmpeg. You install it to `~/.claude/skills/cartoon-3d-short/`. It is MIT-licensed and the README mentions no paid upsell. — [repo README](https://github.com/mromidev/claude-animated-shorts)
- `claude-blender-skills` (about 190 stars) contains three skills: `blender-film` (10s night FPV drone shot), `blender-forest` (realistic forest plus car), and `blender-car-film` (20s snowy-night film). Requirements are Blender 4.4+ (built on 5.2), a GPU for Cycles (RTX 5070 12GB used), Python 3, ffmpeg, numpy/scipy, and Poly Haven CC0 assets. It controls Blender through its own `tools/bridge.py` socket on 127.0.0.1:9876 plus `send.py`, not MCP. It is MIT-licensed and has no mention of Skool, Zack D Films, stitches or blood vessels. — [claude-blender-skills](https://github.com/mromidev/claude-blender-skills)
- Videos for those skills: [ZN-Ugd-4HDM](https://www.youtube.com/watch?v=ZN-Ugd-4HDM), [Udahf4a0UwQ](https://www.youtube.com/watch?v=Udahf4a0UwQ), [BagZ55ac4xs](https://www.youtube.com/watch?v=BagZ55ac4xs) — [claude-blender-skills README](https://github.com/mromidev/claude-blender-skills)
- nocodehack/make-short (indexed): "Free Claude Code skill: make animated YouTube Shorts (2D characters in a 3D Blender world) with a critic agent." MIT-licensed. — [search index of github.com/nocodehack/make-short](https://github.com/nocodehack/make-short) (now 404)
- The same org had `nocodehack-roblox-engine`, a "Claude Code skill that makes animated Roblox-style Shorts from one line", with release v1.15.0 on 17 Nov. It now returns 404, as does github.com/nocodehack itself. — [indexed repo](https://github.com/nocodehack/nocodehack-roblox-engine); [release v1.15.0](https://github.com/nocodehack/nocodehack-roblox-engine/releases/tag/v1.15.0)
- YouTube channel "NoCodeHack" exists at @nocodehack; its page details did not render — [YouTube @nocodehack](https://www.youtube.com/@nocodehack)

### Inferences
- make-short is a different project from mromidev's skill. It used Blender for the 3D world and added a critic agent, whereas mromidev uses three.js/Remotion. Its account and branding (NoCodeHack) are separate. The overlap in topic (cartoon shorts, Roblox shorts) is notable, because Mr omi's Skool also teases "Roblox animated shorts". Whether NoCodeHack is the same person under another brand cannot be confirmed; treat it as unrelated unless shown otherwise.
- The whole nocodehack account went 404 within days of a 17 Nov release. That points to account deletion, rename or suspension, not just one repo being removed.

### Gaps
- Could not query the GitHub users API (session restricted), so the nocodehack account's history or rename target is unknown.
- No archive copy was checked (Wayback not attempted).

## 3. Community discussion and free alternatives/clones (Reddit, X, HN, YouTube)

### Takeaway
Searches turned up **no relevant Reddit or Hacker News threads**. Discussion is happening mainly on GitHub (forks), X, YouTube and skill directories. The most direct free clone of the paid skill is a Spanish fork, **luispablovillasenor-afk/shorts-animados-claude**. In November 2026 it added a `realista/` Blender branch that recreates the realistic "stitches" scene on an arm and a leg. Several other free Zack-D-style skills exist, built on three.js, MPFB/Blender or AI video.

### Cited Findings
- **Free realistic clone:** shorts-animados-claude added "Nuevo (noviembre 2026)" a `realista/` folder based on the video "Cómo hacer shorts 3D ultra realistas con Claude". It contains a `REALISTA.md` guide and two full Blender scenes, an arm and a leg with stitches ("piel, herida, hilo y vello que se ven reales"), plus a 9s preview mp4. It needs Blender 5.x; some assets download separately. MIT-licensed, it credits Mr omi only for the cartoon base and does not mention Skool. — [shorts-animados-claude](https://github.com/luispablovillasenor-afk/shorts-animados-claude)
- **Ready-to-run copies and forks of the free cartoon skill:** [ClixClix/claude-animated-shorts](https://github.com/ClixClix/claude-animated-shorts) and [EssouliAnas/claude-animated-shorts](https://github.com/EssouliAnas/claude-animated-shorts) (one-command setup), [celticmatheus/claude-animated-shorts](https://github.com/celticmatheus/claude-animated-shorts) (Portuguese), [thuann-vn/claude-3d-animated-shorts](https://github.com/thuann-vn/claude-3d-animated-shorts) (rigged 3D characters, "RicksClips style"), [oddlyprovenai/oddly-shorts](https://github.com/oddlyprovenai/oddly-shorts) (derived skill with local voice/score/QA gate), [silenveritas/codex-cartoon-3d-short](https://github.com/silenveritas/codex-cartoon-3d-short) (Codex port) — sources: the linked repos
- **Zack-D-style free skills:**
  - artemnovitckii `/zach-d-films` skill: 3D cartoon "what if" explainers built entirely in three.js (no Blender, no models). Needs ffmpeg, Node and Python 3.9+, and renders WebGL in headless Chrome. Not affiliated with Zack D. — [SKILL.md](https://github.com/artemnovitckii/claude-animation-skills/blob/main/skills/zach-d-films/SKILL.md); [author page](https://novitckii.com/resources/animation-skills/)
  - Anil-matcha/zack-d-films-ai-video-generator: an agent skill covering script, character sheets, 3D keyframes, Veo 3.1 motion, voiceover and captions through MuAPI plus ffmpeg. It is AI video, not Blender, and costs API credits. — [repo](https://github.com/Anil-matcha/zack-d-films-ai-video-generator)
  - "Zack D Films Production Suite" and "Zack Higgsfield" skills (j-star-films-studios), a Higgsfield-MCP pipeline — [skillsdirectory](https://www.skillsdirectory.com/skills/j-star-films-studios-zack-d-films-production-suite); [Zack Higgsfield](https://www.skillsdirectory.com/skills/j-star-films-studios-zack-higgsfield)
  - A curated list of about 180 video skills for Claude Code/Codex, each security-graded — [awesome-claude-video-skills](https://github.com/zhuyansen/awesome-claude-video-skills)
- **X/Twitter:** Theo (@theoxmotion) posted that a Zack D. Films short got 255M views. He tried to rebuild the style with one Claude prompt, ran it on two models with differing results, and shared the full prompt for Claude Code in an empty folder. The prompt uses Blender plus the free MPFB add-on and needs a GPU. — [X post](https://x.com/theoxmotion/status/2120124748159996152) (snippet; x.com not fetchable)
- **YouTube:** A short, "30M views on 3D animations. Claude Makes 3D Stickman Videos With Code (Free GitHub Repo)", credits Mr omi's free skill — [YouTube short](https://www.youtube.com/shorts/R0A2BEfNq-8)
- **General Blender–Claude plumbing:** BlenderMCP (ahujasid's blender-mcp and forks) lets Claude create and modify objects and run arbitrary Python in Blender. It integrates Poly Haven, Sketchfab and Hyper3D Rodin, and Claude is noted as erratic with Poly Haven. — [blender-mcp fork](https://github.com/standardgalactic/blender-mcp); [Glama Blender MCP list](https://glama.ai/mcp/servers/integrations/blender)
- Skool post: "Claude Code + Blender = 3D Animations Without Opening Blender" (Claude writes Python, Blender renders headless) — [Automation Forge Skool](https://www.skool.com/automation-forge-7306/new-video-claude-code-blender-3d-animations-without-opening-blender)
- How-to article on 3D animated AI shorts with Claude Code — [aifire.co](https://www.aifire.co/p/how-to-make-3d-animated-ai-youtube-shorts-with-claude-code)

### Inferences
- The best free substitute for the paid realistic skill is a combination. Mr omi's free `claude-blender-skills` supplies the bridge and the lighting/render discipline. The Spanish `realista/` branch supplies the stitches scenes. MPFB supplies human bodies, which Theo's prompt also relies on. Anatomy scenes like "blood vessels" are not covered by any free repo found.
- The Reddit silence is likely a search-index limit and does not prove there is no discussion. A dedicated Reddit search (via the last30days skill) could still surface threads.

### Gaps
- No Reddit (r/ClaudeAI, r/blender, r/ClaudeCode, r/NewTubers, r/aivideo) or HN threads were found on this topic despite three query variants.
- The X post's full text, date and engagement could not be read.
- No public review or complaint threads about Mr omi's paid skill were found.

## 4. Other paid offerings for AI/Blender shorts pipelines

### Takeaway
There is a small market of paid "Claude skill" kits for 3D shorts on Skool, Payhip, Topmate and Hotmart, plus Gumroad Blender project files that mimic Zack D Films. Search found no Whop listings.

### Cited Findings
- Gumroad "Zyck Wealth": Zack-D-style Blender project files (anatomy animation, gold bar, course project file) — [store](https://zyckwealth.gumroad.com); [anatomy animation](https://zyckwealth.gumroad.com/l/anatomyanimation); [course project file](https://zyckwealth.gumroad.com/l/ZackDFilmCourseProjectFile)
- Skool "Mr Render": a free "Low-Poly 3D Shorts" skill in which Claude builds a PS1-style Blender scene, renders locally and adds sound and captions — [Skool mr-render](https://www.skool.com/mr-render-9528/classroom)
- Skool "The 3D Shortcut" Zero-to-Scene Blender course — [Skool](https://www.skool.com/the-3d-shortcut-6963/classroom)
- Topmate "3D Cartoon Shorts — Claude AI Skill + Setup Guide" (OUTTHINK), a .zip skill — [Topmate](https://topmate.io/out_think/2409753)
- Payhip "What-If" video kits: a three.js engine, HUD/captions tuned for TikTok safe zones, an exporter and frame-check tools. The search summary gave prices of $9.99–$100 across listings. — [payhip PNomt](https://payhip.com/b/PNomt); [payhip g5bum](https://payhip.com/b/g5bum)
- Hotmart "Explain It in 3D" — [Hotmart](https://hotmart.com/en/marketplace/products/explain-it-in-3d/S108323299D)
- Skillshare "Master Viral 3D Short Form Content in Blender" (Vladislav Sateev), a manual Blender course — [Skillshare](https://www.skillshare.com/en/classes/master-viral-3d-short-form-content-in-blender/2062676702)
- Higgsfield Academy "Blockbuster 4K" AI filmmaking pipeline course — [Higgsfield](https://higgsfield.ai/academy/courses/blockbuster-4k)

### Inferences
- Mr omi's $45–55/month subscription is at the high end compared with one-time kits ($10–100). Its differentiator is realistic Cycles-rendered Blender output, where most competitors use three.js or AI video.

### Gaps
- Individual Payhip, Topmate and Hotmart prices and contents were not verified page by page.
- Whop and Gumroad searches returned no Claude-skill-specific listings.
