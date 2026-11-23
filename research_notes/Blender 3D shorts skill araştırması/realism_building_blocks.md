# Realism Building Blocks for a Claude-Driven Photoreal 3D Explainer-Shorts Pipeline in Blender (Zack D. Films style)

Research date: 2026-11-23. Method: WebSearch plus GitHub fetch. Note: direct fetches of blender.org, polyhaven.com and static.makehumancommunity.org were blocked by this session's network allowlist, so some claims rely on search-result snippets of those pages rather than full-page reads. Items marked "unverified" come from general knowledge, not a source found in this session.

## 1. Blender MCP servers / add-ons (control layer)

### Takeaway
Two main options exist as of late 2026: the community ahujasid project (renamed `mcp-for-blender`, MIT, ~35k stars, which has Poly Haven, Sketchfab, Hyper3D Rodin, Hunyuan3D and other asset/AI integrations), and an official Blender Lab MCP server (Blender 5.1+, focused on scene analysis, scripting and manual search, not asset sourcing). For a shorts pipeline, the community server is the better asset front-end. Either way, most of the real work goes through `execute_blender_code` / bpy.

### Cited Findings
- ahujasid repo (now `ahujasid/mcp-for-blender`): 35.4k stars, 3.3k forks, MIT license. It describes itself as a "Community plugin to control Blender 3D with any LLM" and "not made by Blender." — [GitHub](https://github.com/ahujasid/blender-mcp)
- Features: a two-way socket bridge; create, modify and delete objects and materials; visual checks from multiple angles, cameras, wireframe and X-ray views, plus renders; arbitrary Blender Python execution. — [GitHub](https://github.com/ahujasid/blender-mcp)
- Integrations: Poly Haven (CC0 HDRIs, textures and models, no API key needed); Sketchfab search and import (needs an API key); Hyper3D Rodin (AI 3D generation, own key or a trial key); Hunyuan3D (Tencent Cloud). The README also lists Meshy, Tripo, Poly Pizza and World Labs. — [GitHub](https://github.com/ahujasid/blender-mcp)
- Tools: `execute_blender_code`, `look`, `get_scene_info`, `generate_3d`, `generate_gaussian_world`, `search_assets`, `import_asset`, `get_addon_status`, `disable_telemetry`, `record_trajectory_feedback`. — [GitHub](https://github.com/ahujasid/blender-mcp)
- The PyPI package was renamed from `blender-mcp` to `mcp-for-blender` (upgrade with `uvx mcp-for-blender@latest update`). A minimal anonymous usage record is collected by default. Content telemetry is opt-in. `DISABLE_TELEMETRY=true` turns off all telemetry. — [GitHub](https://github.com/ahujasid/blender-mcp)
- Official Blender Lab MCP server: hosted under Blender Lab (projects.blender.org / blender.org/lab), requires Blender 5.1+. You must enable "Allow Online Access" and start the bridge from the add-on panel. It targets scene analysis, debugging and scripting, and includes searching the bundled Blender manual. Reported versions: v1.0.0 on 27 Apr 2026, v1.0.2 and v1.0.3 in Sep 2026. Details come mostly from third-party blogs, one written by a vendor of a competing paid server. — [StraySpark comparison](https://www.strayspark.studio/blog/official-blender-mcp-server-comparison-2026); [DevelopersDigest](https://www.developersdigest.tech/mcp-servers/blender-mcp); [blender.org](https://www.blender.org/?p=96198)
- Claude's connector directory lists a Blender connector (added April 2026) that requires the add-on. — [claude.com/connectors/blender](https://claude.com/connectors/blender)
- Other alternatives seen on PyPI and GitHub: `blender-mcp-server`, `blendmcp`, `dcc-mcp-blender`, `blender-studio-pro-mcp`, emeryporter/blender-mcp. Not evaluated. — [PyPI blender-mcp-server](https://pypi.org/project/blender-mcp-server/); [PyPI blendmcp](https://pypi.org/project/blendmcp/1.4.0/); [PyPI dcc-mcp-blender](https://pypi.org/project/dcc-mcp-blender/0.1.38/); [GitHub emeryporter](https://github.com/emeryporter/blender-mcp)

### Inferences
- For a headless render farm, an agent can skip MCP entirely and run `blender -b scene.blend -P script.py`. MCP is most valuable for interactive scene building and the `look` screenshot feedback.
- Recommended skill design: use mcp-for-blender for asset search and import plus live viewport screenshots, and headless `blender -b` CLI calls for final renders. Disable telemetry in the skill's setup.

### Gaps
- I could not read the official Blender Lab MCP page directly (network blocked), so its exact tool list and license are unconfirmed. It is likely GPL, like other Blender projects, but that is unverified.

## 2. Open-source human characters with facial blendshapes

### Takeaway
MPFB (MakeHuman Plugin for Blender, v2.0.16 on extensions.blender.org, GPLv3 code, CC0 output) is the best free base. It loads a 54-key "faceunits01" ARKit-named face-unit pack plus viseme packs for lip sync, so ARKit-style 52-blendshape animation is effectively covered. CharMorph (GPLv3, CC0 and CC-BY bases) is the active MB-Lab successor. Use MB-Lab with caution because of its AGPL data license. For hero realism, free scanned people (Renderpeople free rigged models) work, and Mixamo supplies body animation.

### Cited Findings
- MPFB on extensions.blender.org: version 2.0.16, compatible with Blender 4.2 LTS and newer. The version history mentions fixes for Blender 5.1 glitches. The plugin is GPL-3.0-or-later (changed from GPL-2.0). — [extensions.blender.org MPFB](https://extensions.blender.org/add-ons/mpfb/); [versions](https://extensions.blender.org/add-ons/mpfb/versions/)
- Content created with MPFB/MakeHuman is CC0 and usable commercially, including in closed-source games. — [CG Channel](https://www.cgchannel.com/2025/03/check-out-open-source-blender-character-generation-plugin-mpfb-2); [GitHub mpfb2](https://github.com/makehumancommunity/mpfb2)
- MPFB 2.0.15 added a Face panel with three loadable shape-key packs: visemes01 (22 Microsoft-style keys), visemes02 (15 Meta/ARKit-style viseme keys such as viseme_aa and viseme_CH), and faceunits01 (54 ARKit face-unit keys such as browDownLeft and eyeBlinkLeft). Asset packs must be installed first. — [MPFB 2.0.15 release notes](https://static.makehumancommunity.org/mpfb/releases/release_2015.html) (via search snippet)
- MPFB integrates with the Lip Sync add-on: after loading visemes02, click "Assign Lip Sync shape keys". — [MPFB lipsync docs](https://static.makehumancommunity.org/mpfb/docs/lipsync.html) (via search snippet)
- One user review says it "does not work 4.5" for v2.0.15. This is anecdotal. — [extensions.blender.org reviews](https://extensions.blender.org/add-ons/mpfb/reviews/?score=3)
- Paid alternatives for generating ARKit 52 keys on any head mesh: Faceit and FaceKeys (Superhive). — [Faceit docs](https://faceit-doc.readthedocs.io/); [FaceKeys](https://superhivemarket.com/products/facekeys)
- CharMorph: the MB-Lab successor, GPLv3. v0.4 (March 2025) is compatible with Blender 4.4. Base characters are CC-BY or CC0 (Vitruvian is CC0). — [CG Channel](https://www.cgchannel.com/2025/03/check-out-new-open-source-3d-character-generator-charmorph/); [Digital Production](https://digitalproduction.com/2025/03/25/charmorph-0-4-open-source-character-creation-evolves/)
- MB-Lab is discontinued and archived, and its last version targets Blender 4.0. One source says its data is AGPL and that characters made with it inherit AGPLv3, a significant concern for commercial use. — [GitHub MB-Lab](https://github.com/animate1978/MB-Lab); [DeepWiki licensing](https://deepwiki.com/animate1978/MB-Lab/8-licensing-and-legal)
- Renderpeople free rigged models (e.g. "Eric Rigged 001", "Carla Rigged 001") are scan-based, retopologized, rigged, and licensed for commercial projects under Renderpeople's license. A Sketchfab copy shows CC-BY, so licenses conflict by distribution channel. — [CG Channel](https://www.cgchannel.com/2017/11/download-four-free-renderpeople-rigged-3d-characters); [Renderpeople free page](https://renderpeople.com/free-3d-model); [Sketchfab Eric](https://sketchfab.com/3d-models/eric-rigged-001-rigged-3d-business-man-a46bc9f67aaa415bb4f3241eef900e7f)
- Mixamo: per Adobe's FAQ, characters and animations are free and royalty-free for personal, commercial and non-profit use, including films. Community answers add that you may not redistribute raw files as the main product, and may not use them to train ML models. Bipedal humanoids only. — [Adobe Mixamo FAQ](https://helpx.adobe.com/creative-cloud/faq/mixamo-faq.html); [Adobe community FAQ](https://community.adobe.com/t5/mixamo-discussions/mixamo-faq-licensing-royalties-ownership-eula-and-tos/td-p/13234775)
- 3D Scan Store (Ten24) free head scans (multi-expression base mesh, male and female heads) are personal/non-commercial only. Commercial use needs contact with the company. — [3D Scan Store blog](https://www.3dscanstore.com/blog/multi-expression-basemesh); [CG Channel](https://www.cgchannel.com/2019/06/download-ten24s-free-hi-res-3d-scan-of-a-male-human-head/)

### Inferences
- Agent workflow: install MPFB via `bpy.ops.extensions` or the CLI, script a human with macros (age, weight, ethnicity), add the Rigify or default rig, load faceunits01 and visemes02, retarget a Mixamo FBX onto the rig, and drive ARKit keys from a JSON expression timeline. MPFB skin defaults are not photoreal. Plan to swap in a better SSS skin material.
- Commercial-safe stack: MPFB (CC0 output) + Mixamo + CharMorph CC0 base. Avoid MB-Lab and 3D Scan Store freebies for monetized shorts.

### Gaps
- I did not confirm whether faceunits01's 54 keys are exactly ARKit's 52 plus 2 extras, or how they are named. The MPFB pages could not be fetched.
- No AXYZ free-model information was found.
- Rigify face rig compatibility with MPFB was not verified.

## 3. Asset sources (scanned assets, anatomy, earth, sound)

### Takeaway
Poly Haven (CC0, public API) and ambientCG (CC0) cover HDRIs, PBR materials and some models. Kenney is CC0 but stylized, so it is a poor fit for photoreal. BodyParts3D is now CC BY 4.0, while derivatives like Z-Anatomy are CC BY-SA. NASA Blue Marble requires a credit line. Sonniss GDC bundles are royalty-free but exclude AI training. Sketchfab is per-model (filter for CC0/CC-BY).

### Cited Findings
- Poly Haven assets are all CC0. The public API at api.polyhaven.com is governed by separate API Terms. — [Poly Haven license](https://polyhaven.com/license)
- API terms conflict. The GitHub README and corporate page say the API is free for any purpose, including commercial, but building on the live API requires a "Powered by Poly Haven" credit. The official API page says commercial API use needs a custom license or sponsorship. All requests need a unique User-Agent. — [Poly-Haven/Public-API](https://github.com/Poly-Haven/Public-API); [Poly Haven corporate](https://polyhaven.com/corporate); [Poly Haven API page](https://polyhaven.com/our-api)
- ambientCG: all assets are CC0 1.0, attribution optional, commercial use allowed. — [ambientCG docs](https://docs.ambientcg.com/license)
- Kenney: all game assets are CC0, commercial use allowed. Don't use the Kenney logo. — [kenney.nl/support](https://kenney.nl/support)
- BodyParts3D: currently "CC Attribution 4.0 International", license updated 2025-02-27. Older releases were CC BY-SA 2.1 Japan. OBJ downloads are on dbarchive.biosciencedbc.jp. Z-Anatomy (built on BodyParts3D) is CC BY-SA 4.0. — [BodyParts3D license](https://dbarchive.biosciencedbc.jp/en/bodyparts3d/lic.html); [README](https://dbarchive.biosciencedbc.jp/data/bodyparts3d/LATEST/README_e.html); [download](https://dbarchive.biosciencedbc.jp/en/bodyparts3d/download.html); [Z-Anatomy](https://github.com/Z-Anatomy/Models-of-human-anatomy)
- NASA Blue Marble Next Generation: credit "NASA Earth Observatory". NASA insignia and logos are not public domain. The SVS page asks for a different credit (GSFC SVS). — [Earth Observatory Image Use](https://earthobservatory.nasa.gov/ImageUse); [SVS Blue Marble](https://svs.gsfc.nasa.gov/2915); [Blue Marble NG](https://science.nasa.gov/earth/earth-observatory/blue-marble-next-generation/)
- Sonniss GDC 2026 bundle: 347+ files, 7.47 GB, royalty-free WAV. Past bundles allowed commercial use without attribution. Since 2024 the license bans AI training. Sounds can't be redistributed unchanged. — [Bedroom Producers Blog 2026](https://bedroomproducersblog.com/2026/03/16/sonniss-gdc-2026-bundle/); [Rekkerd](https://rekkerd.org/sonniss-releases-gdc-2026-game-audio-bundle/); [Freesound forum](https://freesound.org/forum/legal-help-and-attribution-questions/42190)
- The Sketchfab integration is available in mcp-for-blender with an API key. — [GitHub](https://github.com/ahujasid/blender-mcp)

### Inferences
- The skill should keep a `licenses.json` manifest per project that records the asset ID, source, license and required credit, and auto-generate a description credit block (Poly Haven API credit, NASA, BodyParts3D CC BY).
- Freesound sounds are per-file CC0, CC-BY or CC-BY-NC (unverified in this session). The skill should filter the Freesound API by `license:"Creative Commons 0"` for commercial shorts.

### Gaps
- Freesound API license filters and Sketchfab's CC0 filter were not verified with sources in this session.

## 4. Reference video analysis tools

### Takeaway
All the proposed tools are free, scriptable CLIs or libraries, but this session found few primary sources for them (searches returned mostly Real-ESRGAN results). The standard stack: PySceneDetect for cuts and shot lengths, OpenCV `phaseCorrelate` on downscaled grayscale frames for global camera pan/zoom estimates, `ffmpeg -af ebur128` for integrated LUFS and loudness range, and Whisper word timestamps for words per second.

### Cited Findings
- No primary sources were retrieved for PySceneDetect, OpenCV phase correlation, ffmpeg ebur128 or Whisper in this session. The search returned off-target results. — (see Gaps)

### Inferences (unverified, from general knowledge)
- PySceneDetect (BSD-3): `scenedetect -i ref.mp4 detect-adaptive list-scenes` produces a CSV of cuts, from which you get average shot length and cut rhythm.
- OpenCV (Apache-2.0) `cv2.phaseCorrelate(prev, cur)` returns a sub-pixel (dx, dy) shift per frame pair. Integrate these to classify static, pan or push-in shots. Use log-polar transforms or optical flow for zoom.
- ffmpeg: `ffmpeg -i ref.mp4 -af ebur128=peak=true -f null -` prints integrated loudness (I, in LUFS), LRA and true peak. Shorts typically land around -14 LUFS (platform norms, unverified).
- OpenAI Whisper (MIT) or faster-whisper with `word_timestamps=True` gives word-level timing, so words/sec = words / speech duration.
- The agent writes a `reference_profile.json` (shot lengths, camera motion types, LUFS, WPS) that drives the shot plan and the critic rubric.

### Gaps
- Licenses and exact flags for PySceneDetect, Whisper and ffmpeg were not source-verified here. Re-check the GitHub READMEs (github.com/Breakthrough/PySceneDetect, github.com/openai/whisper) and ffmpeg-filters docs.

## 5. Photoreal look in Blender 4.x/5.x

### Takeaway
Use AgX (the default since Blender 4.0) for highlight rolloff and hue stability. Standard clips highlights hard, and Filmic is legacy. Optionally add the Punchy look, but watch highlights. In Blender 5.0, bloom lives only in the compositor (Glare node, Bloom mode), and the compositor ships effect templates (chromatic aberration, sensor noise, vignette). Lens Distortion is a core node. Scripts must use the engine ID `BLENDER_EEVEE`, not `BLENDER_EEVEE_NEXT`, in 5.0.

### Cited Findings
- Standard view is aimed at product photography where HDR values are mostly small speculars. Users report heavy clipping, but it gives a 1:1 match with scene values. — [Blender manual: Displays and Views](https://docs.blender.org/manual/fi/dev/render/color_management/displays_views.html); [Blender Artists](https://blenderartists.org/t/why-does-only-filmic-color-management-seem-to-handle-intense-emissive-colors-well/1573545)
- AgX replaced Filmic as the default in Blender 4.0, with better highlight detail and hue preservation, and a less saturated base. The Punchy look adds contrast and saturation and can push highlights toward clipping. AgX still struggles with very intense emissives. — [Blender 4.0 color management notes](https://wiki.blender.org/release_notes/4.0/color_management/); [PR #106355](https://projects.blender.org/blender/blender/pulls/106355); [Blender Artists AgX hue](https://blenderartists.org/t/agx-changes-hue/1527123)
- EEVEE bloom moved from render settings into the compositor Glare node. The Bloom mode is higher quality and more temporally stable than old EEVEE bloom. — [CG Cookie Blender 5.0](https://cgcookie.com/community/22683-blender-5-0); [devtalk real-time compositor](https://devtalk.blender.org/t/real-time-compositor-feedback-and-discussion/25018/537)
- The Blender 5.0 compositor provides eight effect templates, including chromatic aberration, sensor noise and vignette. The compositor is now a regular data block. — [Heise](https://heise.de/-11084290)
- Blender 5.0 renamed the EEVEE engine ID from BLENDER_EEVEE_NEXT to BLENDER_EEVEE. — [Blender 5.0 EEVEE release notes](https://developer.blender.org/docs/release_notes/5.0/eevee/)
- Paid add-ons for film looks (Compositor FX, Lazy Composer 2 Pro, Filmic Plus) exist, but core nodes suffice. — [Superhive Compositing FX](https://superhivemarket.com/products/compositing-fx)

### Inferences
- Recommended defaults the agent should set: `view_transform='AgX'`, look `None` or `Medium High Contrast`, exposure tuned so the subject is around 0.18 grey. Compositor chain: Glare (Bloom, low mix) → Lens Distortion (slight dispersion around 0.01) → vignette (template or Ellipse Mask + blur) → grain (sensor noise template) → output. Enable motion blur (shutter about 0.5) and DOF (real focal lengths 24–85mm), which are key cues of the "real camera" Zack D. Films look.
- Skin: Principled BSDF Subsurface Weight around 1 with Random Walk (Skin) and a reddish radius, scale in scene units. This is unverified here, so check the manual's Principled BSDF page.

### Gaps
- I did not retrieve the 5.x manual pages for the Principled BSDF SSS, motion blur or Lens Distortion nodes. Volume/fog settings were not sourced.

## 6. Render-time optimization (Cycles / EEVEE)

### Takeaway
Use Cycles GPU with the light tree on, adaptive sampling (noise threshold), and OpenImageDenoise (GPU-accelerated since 4.1) or OptiX. Rendering at 1080p and upscaling with Real-ESRGAN (BSD-3 code) is viable for vertical shorts. Running parallel Blender instances (frame ranges per GPU or per process) is a standard CLI pattern but was not source-verified here.

### Cited Findings
- The light tree samples lights by distance and intensity. It significantly reduces noise at a slightly higher per-sample cost and works best with physically correct lights (no custom falloff or ray-visibility tricks). — [Blender 5.0 manual: Sampling](https://docs.blender.org/manual/fi/5.0/render/cycles/render_settings/sampling.html); [3.5 Cycles notes](https://developer.blender.org/docs/release_notes/3.5/cycles/)
- Adaptive sampling reduces samples in low-noise areas. Noise threshold behavior changed in 3.0. — [Blender 4.5 manual: Sampling](https://docs.blender.org/manual/en/4.5/render/cycles/render_settings/sampling.html); [3.0 notes](https://wiki.blender.org/release_notes/3.0/cycles/)
- OpenImageDenoise typically gives the highest quality and has been GPU accelerated since 4.1. OptiX covers older NVIDIA GPUs. — [4.1 Cycles notes](https://wiki.blender.org/release_notes/4.1/cycles/); [Blender manual: Sampling](https://docs.blender.org/manual/en/4.5/render/cycles/render_settings/sampling.html)
- GPU setup: Preferences > System > Cycles Render Devices (CUDA, OptiX, HIP, oneAPI, Metal), then set the scene Device to GPU. When VRAM is full, Blender falls back to system memory, which is slower but usually still faster than CPU. — [GPU Rendering manual](https://docs.blender.org/manual/fi/4.4/render/cycles/gpu_rendering.html)
- Real-ESRGAN code and the RealESRGAN_x4plus weights are listed as BSD-3-Clause. Third-party packagings vary. — [Vast.ai model library](https://vast.ai/model/real-esrgan); [HF AI_upscalers](https://huggingface.co/easygoing0114/AI_upscalers)

### Inferences
- Agent render recipe: Cycles GPU; samples 128–512 with noise threshold 0.01–0.03; light tree on; OIDN denoise with albedo+normal passes; reduce max bounces (e.g. 4 diffuse, 4 glossy, 8 transmission); clamp indirect to around 10; persistent data on for animations; volumes via a cheap world-volume step size or replaced with a compositor mist pass. Render 1080x1920, optionally upscale with `realesrgan-ncnn-vulkan` (or skip it, since shorts are delivered at 1080x1920).
- Parallelism: `blender -b f.blend -E CYCLES -s 1 -e 120 -a -- --cycles-device OPTIX` per GPU, or several EEVEE instances on one GPU if VRAM allows. Use `--cycles-device` and `CUDA_VISIBLE_DEVICES` to pin. This is unverified in this session.
- EEVEE (5.0, ray-traced) is far faster for drafts and the critic loop. Use Cycles only for final hero shots.

### Gaps
- No benchmarks were found comparing EEVEE 5.x vs Cycles per-frame times for this style. Volume render settings were not sourced.

## 7. Existing Claude skills for critic/judge visual QA

### Takeaway
No Blender-specific render-critic skill was found. Several generic visual-judge skills exist (oh-my-claudecode `visual-verdict`, `lookdev-auto`, `screenshot-critique`, a render-critique-refine loop with a bpy reference). These are good templates, but a custom skill is needed.

### Cited Findings
- oh-my-claudecode `visual-verdict`: compares screenshots to reference images, with a pass threshold of 90+, and iterates until it reaches that. — [skills.sh visual-verdict](https://skills.sh/yeachan-heo/oh-my-claudecode/visual-verdict); [claudepluginhub](https://claudepluginhub.com/skills/markrmiller-oh-my-claudecode/visual-verdict)
- `lookdev-auto`: a vision or video model rates rendered variants in a loop, for "does it look right" questions. — [claudeskills.info lookdev-auto](https://claudeskills.info/skills/sickn33/agentic-awesome-skills/lookdev-auto/); [skillsmp](https://skillsmp.com/creators/sickn33/antigravity-awesome-skills/skills-lookdev-auto)
- A shared "producing creative work" skill set includes a render-critique-refine loop and a Blender/bpy reference. — [skillsmp shabaraba](https://skillsmp.com/creators/shabaraba/dotfiles/claude-skills-shared-producing-creative-work)
- Design-loop guidance: critics should judge rendered output, not code, and use binary verdicts because scores drift upward. — [The Design Loop guide](https://app.notion.com/p/The-Design-Loop-Free-Guide-3b8e8d6bd13781ff8bf2fc06fd5d0aac)
- Others: `screenshot-critique`, `render-debug-loop`, `visual-verify`. — [claudepluginhub screenshot-critique](https://claudepluginhub.com/skills/dzhng-skills/screenshot-critique); [skillsmp render-debug-loop](https://skillsmp.com/creators/jakildev/irredenengine/claude-skills-render-debug-loop); [skills.sh visual-verify](https://www.skills.sh/edwinhu/workflows/visual-verify)
- mcp-for-blender's `look` tool supplies multi-angle viewport and render images for a critic. — [GitHub](https://github.com/ahujasid/blender-mcp)

### Inferences
- Custom critic design: render a low-sample EEVEE contact sheet (first, middle and last frame per shot), then give a Claude subagent the reference-video frames plus `reference_profile.json` and a binary-checklist rubric (scale plausibility, interpenetration, clipped highlights from the histogram, plastic skin, missing contact shadows, camera motion matching the reference, text-safe area for 9:16). Loop up to N times. Add numeric checks (an ffmpeg `signalstats` clipping percentage, LUFS) as hard gates.

### Gaps
- None of these skills was installed or inspected, and quality is unknown. No skill combining reference-video profiling with Blender rendering was found, so a full free "Zack D. Films" skill does not appear to exist.
