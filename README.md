# How to Make Manga with AI: Muse, the PixAI API, and MangaAgentsStudio-A

Learn how to make manga with AI. I used Muse and PixAI to turn a short story into a finished manga page, from panel script to English lettering and real costs.

<img width="1672" height="941" alt="Header for a tutorial on how to make manga with AI, showing a black-and-white manga page where a young wizard leans out of a panel pouring water from a teapot, a black cat with a raised paw sits on the page, and a small fluffy mascot holds a pencil." src="https://github.com/user-attachments/assets/42571e10-ad1e-4103-9f28-31abb72d02cc" />

*See how to make manga with AI as I turn a simple tea-making mishap into a five-panel page with Muse and PixAI.*

Nowadays, it's possible to turn a one-line story idea into a lettered manga page in an evening. I wanted to see how to make manga with AI myself, so I tried it with a wizard, a teapot, and a very unimpressed cat:

<img width="2000" height="3000" alt="Five-panel black-and-white manga page of a young wizard flooding his kitchen with a tea spell while his black cat watches, with English speech balloons." src="https://github.com/user-attachments/assets/38ac5357-1941-4390-b155-61b7ef4bf8db" />

*My finished one-page manga, made with AI by planning in Muse, drawing in PixAI, and lettering with the MangaAgentsStudio-A toolkit.*

Three tools shared the work. Muse, Meta's personal AI agent, planned the story and ran each step from chat. PixAI, an anime-focused AI image generator, drew every panel. A free community toolkit called MangaAgentsStudio-A handled the page layout and the English lettering. These are three separate projects, not an official bundle, and I connected Muse to PixAI the same way PixAI's [official Muse × PixAI API guide](https://blog.pixai.art/en/muse-pixai-api-guide/) describes.

I made the page on November 23, 2026. It took 7 generated images to get 5 finished panels, and my PixAI balance confirms the whole project used 28,000 credits. Most steps took about a minute each.

<img width="1810" height="1860" alt="Close-up of three manga panels showing a teapot erupting as a wizard shouts, the soaked wizard asking a black cat for help, and the cat resting a paw on the teapot." src="https://github.com/user-attachments/assets/4b20a8fd-03aa-46bd-81e5-aa72c837a50f" />

*The second half of the page up close, where the flood peaks and the cat steps in.*

*Disclosure: This article was created in collaboration with PixAI. It also includes my PixAI referral link. If you sign up through it and complete the qualifying steps, we both receive bonus credits, and I may earn extra credits if you top up later. It costs you nothing.*

Let's break down each step, including the parts that needed a second try.

## What You Need to Make Manga with AI Using Muse and PixAI

Before you generate a single panel, there are five things you need in place to make manga with AI this way:

- a Muse account
- PixAI API access and enough credits
- a PixAI model that works through the API
- the manga toolkit, plus the software it runs on
- a way to add English lettering and export the page

Here's how I handled each one.

### Muse, PixAI API Access and Credits

I used Muse on the web with its Free plan. On the PixAI side, I have a Starter membership, which lets you create an API key straight away.

You can also request access by email if you're not a member, and PixAI's [guide to getting API access](https://platform.pixai.art/en/docs/quick-start/enroll-in-api) explains both routes. An API key is the password-like credential that lets Muse send image requests to PixAI on your behalf. I created a fresh key for this project and pasted it into a private file myself, so it never appeared in the chat.

Every image you generate uses PixAI credits, its in-app currency. In this project, each panel costs 4,000 credits, and PixAI's [pricing guide](https://blog.pixai.art/en/pixai-pricing-guide-free-credits-membership-and-commercial-use/) explains free daily credits and what each membership adds.

### Why This Isn't a One-Click AI Manga Maker

The part that turns separate images into a manga page is [MangaAgentsStudio-A](https://github.com/potechitakusan/MangaAgentsStudio-A-Eng), a community project by potechitakusan. It isn't an app you open and click through. It's a kit of instructions and Python scripts for an AI agent to follow, covering everything from panel layouts to speech balloons. You bring your own agent and your own image generator, and it isn't made by Meta or PixAI.

I used the English edition, version 0.5.4 (commit `4c9bcee`, October 26, 2026). It's localized for left-to-right reading and horizontal English text, which saved me from adapting Japanese settings. One catch is that the kit officially targets other AI agents, not Muse, so a few steps needed adjusting along the way.

### Setting Up the Toolkit in Muse

My first instruction asked Muse to clone the kit from GitHub. Muse stopped partway through, explaining that a cloned repository can run code during normal git processing, and asked whether to continue. Instead, I had it download the same files as a ZIP:

```text
Don't complete the git clone. Remove the empty manga-kit folder. Instead, download the main branch as a ZIP from https://github.com/potechitakusan/MangaAgentsStudio-A-Eng/archive/refs/heads/main.zip and extract it into a folder named manga-kit without running anything in it. Then read distribution-version.txt, README.md and AGENTS.md, and look up the latest commit hash and date for the main branch on GitHub. Report the version, the commit hash and date, and summarize the requirements and how to create a new project. Do not run any scripts yet.
```

<img width="1571" height="1125" alt="Muse chat showing a request to clone the MangaAgentsStudio-A English repository and Muse's reply explaining it paused the clone after an execution-risk check." src="https://github.com/user-attachments/assets/f6d7c13a-eadd-4026-967d-c906307779fb" />

*Muse paused the git clone after a safety check, explaining that cloned repositories can run code, so I downloaded the toolkit as a ZIP instead.*

From there, setup went quickly. Muse confirmed its workspace already had Bash, Python 3.12, and Pillow, the image library the kit uses for lettering. It then created a separate project for the manga, which the kit requires so you never work inside the master copy:

```text
Approved. Run bash scripts/New-MangaProject.sh --project-name 'manga-page'. From now on, treat that project folder as your working folder for this manga, since you can't reopen in it the way Codex does. List its main folders and files, and tell me whether it has a .env file and which variable names it expects. Do not print any key values.
```

That last part was one of the adjustments for Muse. The kit tells you to reopen your agent inside the new project folder, which Muse can't do, so I asked it to treat that folder as home instead. Once I'd added my key, Muse ran the kit's built-in key check, which confirms the key works without generating anything or using credits.

### Choosing the Model and English Lettering Font

The kit defaults to PixAI's Tsubaki.2 model, generating four images per request for complex panels. I switched to Tsubaki.3 in Pro mode with one image per request instead.

I'd already tested Tsubaki.3 through the API in earlier projects, and four images per request would have multiplied the cost of every panel. The kit's scripts send everything through [PixAI's API](https://platform.pixai.art/en/docs), so Muse made the change in the project's settings files. It also passed the single-image setting on every run, since the scripts otherwise switch to four images for two-character panels.

Before generating any art, I also tested the lettering. The kit doesn't include fonts, so it fell back to DejaVu Sans, which was readable but looked plain. I asked Muse to add [Comic Neue](https://fonts.google.com/specimen/Comic+Neue), a free comic-style font under the SIL Open Font License, and set it as the dialogue font.

<img width="1270" height="630" alt="Two test speech balloons side by side, one in DejaVu Sans with text wrapped onto three lines and one in Comic Neue with text on two lines." src="https://github.com/user-attachments/assets/d764127d-2483-4a2e-b330-468ceeb95d49" />

*I tested English lettering before generating any art. The kit's fallback font was readable, but Comic Neue with a manual line break looked more like a comic and cleared the wrap warning.*

With the tools ready, it was time to write the story.

## Turn Your Story Idea into a Manga Panel Script

If you're learning how to make manga with AI, the script matters more than the prompts, so I gave Muse a deliberately rough idea and let it build the plan first.

### My Story Idea and Muse's First Draft

Here's the instruction I sent:

```text
Let's start the manga in the manga-page project, using the kit's planning workflow. Here's my story idea:

a young wizard tries to make tea with magic and floods the kitchen, then his cat fixes it with one paw

Turn it into a one-page script with 4–6 panels, read left to right and top to bottom. For each panel, give: what happens, which character appears, the framing and key visual details, the dialogue or narration (short English lines, using \n for deliberate line breaks), and what the reader needs to understand before moving on. Suggest a panel layout from the kit's templates. Save the script in the project's story docs and show me the full script. Do not generate any images yet.
```

Muse followed the kit's own planning order, from a short brief to a text storyboard to a layout. It chose one of the kit's templates with five panels, arranged in two rows of two and a wide final panel, so the punchline gets the most room. The script was strong, with a clear setup, a flood, a plea for help, and a deadpan ending.

<img width="1578" height="1270" alt="Muse chat with a story idea about a wizard flooding his kitchen, followed by Muse confirming a saved five-panel script, synopsis, and layout files." src="https://github.com/user-attachments/assets/a53affba-5a34-4eb6-b5f2-e77ec0e70e93" />

*Muse turned my one-line idea into a five-panel script and saved it in the project's story folder, along with a synopsis and a layout record.*

Here's the approved plan, reading left to right:

> **Panel 1 (top left):** Morning. The wizard shuffles in, yawning, holding an empty teapot and his wand, and eyes the tap across the room. The cat rests on the windowsill.<br>
> Dialogue: "Tea first. / The tap is / so far away."<br>
> Reader learns: he's too lazy to fetch water, so he'll use magic. A cat lives here.
>
> **Panel 2 (top right):** Close-up. He points his wand at the teapot and casts a filling spell. The pot is full, but the water keeps coming.<br>
> Dialogue: "Fill, bubble, / steep... and pour!"<br>
> Reader learns: he never said "stop".
>
> **Panel 3 (middle left):** The spell erupts. Water bursts from the teapot on the counter and rains down. He recoils, shouting.<br>
> Dialogue: "Stop! / I said / STOP—!" Sound effect: SPLOOSH<br>
> Reader learns: the spell is out of control, and the kitchen is flooding.
>
> **Panel 4 (middle right):** Later. He stands soaked in ankle-deep water, palms up, asking the cat for help. The cat sits dry on the counter and slowly raises one paw.<br>
> Dialogue: "Okay. / New plan. / ...Help?" Sound effect: plip... plip...<br>
> Reader learns: he's given up, and the cat already has a plan.
>
> **Panel 5 (bottom, full width):** The kitchen is dry. The cat sits with one paw resting on the teapot lid. The wizard holds a steaming cup of tea and stares.<br>
> Dialogue: "All that magic... / and you used / one paw?"<br>
> Reader learns: the cat stopped the spell with the smallest possible touch.

### What I Changed Before Approving the Script

Muse's first draft asked for more than an image generator can reliably hold across five panels, so I made four edits. The final panel originally showed the water flowing back into the teapot, so I cut it down to the aftermath.

The dialogue already tells you what happened. I also removed a lost slipper, a fixed kitchen layout for every wide shot, and the side of the cat's white paw. Each of these was a small detail that would need to match perfectly in every panel, and in my experience, those are exactly the details AI images drift on.

## Create a Character Reference for Consistent AI Manga Panels

Next, Muse wrote a compact character reference: the features that must stay the same in every panel, plus a reusable description that starts each prompt. I used PixAI's [guide to building a consistent character sheet](https://blog.pixai.art/en/oc-vtuber-character-sheet-generation-guide-building-a-consistent-ai-character-with-pixai/) as a model for separating fixed features from details that can change.

For the art itself, I used Tsubaki.3, one of [PixAI's anime image models](https://eap.pixai.art/go/abirami), with black-and-white ink and screentone shading to give the page a classic manga look.

### The Character Rules I Carried into Every Prompt

I made three changes to Muse's first version. I removed color words like "chestnut hair" and "amber eyes," since they can push a black-and-white prompt toward color. I gave the wizard one tall cowlick, so he'd be easy to recognize in every panel.

I also took "in his right hand" out of the wand description, because left and right details are easy for AI to flip. Here's the final description that opened every prompt for each character:

```text
a young wizard in his early twenties, messy dark hair with one tall cowlick sticking up, wearing a rumpled open robe over striped pajamas, black-and-white manga style, monochrome ink, screentone shading, clean line art
```

```text
a small solid black cat with pale eyes, black-and-white manga style, monochrome ink, screentone shading, clean line art
```

### Testing the Characters Before Generating Panels

Before spending credits on real panels, I generated one test image of both characters. The style was exactly what I wanted, and the wizard matched his description.

The cat didn't. It was supposed to have one white front paw, but it came out with two, plus a white chest and a white back foot. Muse caught this in its own check, without being asked.

I decided to simplify. The cat became solid black, since the story's "one paw" punchline works without a white one. When I first wrote "no white markings" into the prompt, the kit's prompt checker flagged it, because mentioning "white" at all can make the model draw it. So I moved "white markings, white paws, white chest" into the negative prompt instead, which is the list of things the model should avoid.

<img width="1832" height="1254" alt="Two black-and-white manga images side by side, the first showing a black cat with white paws and chest by a window and the second showing a solid black cat raising a paw on a kitchen counter." src="https://github.com/user-attachments/assets/a390a529-fd5f-4974-87bf-f88671d2b4bc" />

*My first character test gave the cat extra white paws and a white chest.*

That fixed the cat, so I moved on to the real panels.

## Generate Manga Panels with PixAI Through Muse

The kit generates each panel as a separate image, which is how this Muse PixAI workflow turns a script into a page. That also means you can replace one weak panel without redoing the rest.

### Start with One Representative Panel

Rather than generating everything at once, I started with Panel 4, the hardest one. It has both characters, the raised paw and the flooded kitchen, so if it worked, the others probably would too.

Muse matched the image's shape to Panel 4's square space on the page and left blank areas for the speech balloon and sound effect. The kit also added a few tags of its own to the prompt, such as "1boy, 1other" and "masterpiece, best quality," so this is exactly what PixAI received:

```text
1boy, 1other, the wizard standing in floodwater on the left side, the cat sitting dry on the counter on the right side at the same eye level, facing each other, a large plain empty area of blank wall in the upper left above the wizard's head, kept completely clear of objects, an empty stretch of calm water surface across the lower foreground, kept clear, boy, a young wizard in his early twenties, messy dark hair with one tall cowlick sticking up, wearing a rumpled open robe over striped pajamas, soaked and dripping wet, standing in ankle-deep floodwater, turning toward the cat with both palms raised in appeal, holding a thin willow wand with a curled tip in one hand, pleading expression, other, a small solid black cat with pale eyes, sitting upright and completely dry on the kitchen counter, calmly watching the wizard, slowly raising one front paw and holding it up, still and silent, medium two-shot, eye-level side view, both faces and the raised front paw readable in one line, a quiet pause, nothing splashing, one teacup floating on the water near the wizard's knees, cozy cottage kitchen interior, a counter, a tap, a window, the floor covered in ankle-deep still water, black-and-white manga style, monochrome ink, screentone shading, clean line art, masterpiece, best quality
```

And this was the negative prompt:

```text
text, speech bubble, panel border, watermark, white markings, white paws, white chest
```

I generated it with Tsubaki.3 in Pro mode at a square 1:1 size (1024×1024), with one image per request and Prompt Helper turned off. The solid black cat held, and both characters matched the reference. The wand tip came out straight instead of curled, but nothing affected the story, so I kept it.

### Generating the Remaining Panels

For the other four panels, Muse turned each script beat into a prompt the same way, using the character descriptions word for word. Panels 1 to 3 were square, and Panel 5 used a wide 16:9 shape for the full-width bottom panel.

I reviewed all four prompts before approving them, and the kit wouldn't send anything without an explicit credit confirmation and a cost note. Each file was named after its panel, from `p01-01` to `p01-05`, so matching images to the page was straightforward. Each image took about a minute, so the batch of four took around four to five minutes.

Muse checked every result against the script and the character reference. The characters held up well, but three panels had small staging differences.

The cat in Panel 1 was awake instead of asleep, a tap sat prominently beside the teapot in Panel 2, and the wizard in Panel 5 held his cup in one hand with a blank expression. None of these broke the story, so I kept those panels. The kit didn't report a credit cost for these requests, so I checked my PixAI balance after each round to confirm it.

<img width="1767" height="1210" alt="Grid of seven black-and-white manga images of a wizard and a black cat, labeled character test, Panels 1 to 5, and a Panel 3 retake, with two marked as rejected." src="https://github.com/user-attachments/assets/c83fd4f8-0b91-4429-a8dc-68dcfc0948db" />

*I generated seven images to get five finished panels, rejecting the character test and the first Panel 3.*

### Replacing a Panel That Missed the Script

Panel 3 was the one real miss. Fixing a panel through the API means regenerating it, since PixAI's Edit tools [aren't available through the API](https://platform.pixai.art/en/docs/quick-start/limits).

The prompt described the teapot erupting like a fountain, and PixAI drew a literal stone garden fountain in the middle of the kitchen, with no counter in sight. I asked Muse to remove the word everywhere, describe the teapot sitting on the counter with water shooting up from its spout, and add "fountain, stone basin, garden fountain" to the negative prompt.

Here's the exact prompt Muse sent for the retake, with the kit's tags at the start and end:

```text
1boy, solo, the erupting teapot in the center of the frame, the wizard recoiling on the left side with his face clear of the water jet, a wide empty band across the very top of the frame kept completely clear, plain empty space in the upper left above the wizard's head kept clear, a young wizard in his early twenties, messy dark hair with one tall cowlick sticking up, wearing a rumpled open robe over striped pajamas, recoiling in shock with both arms thrown up, mouth open shouting, sleeves soaked to the elbows, holding a thin willow wand with a curled tip pointing the wrong way, medium shot, eye-level view, pulled back to show the full height of the jet, the teapot sitting on the kitchen counter, a tall jet of water shooting straight up from its spout, hitting the ceiling and raining down onto the counter and floor, the first puddle spreading across the floor, cozy cottage kitchen interior, a counter, a tap, a window, black-and-white manga style, monochrome ink, screentone shading, clean line art, masterpiece, best quality
```

And the negative prompt:

```text
text, speech bubble, panel border, watermark, fountain, stone basin, garden fountain
```

The cat isn't in this panel, so the white-marking terms from Panel 4's negative prompt aren't needed here. The other settings matched Panel 4, which means Tsubaki.3 in Pro mode at a square 1:1 size, with one image per request and Prompt Helper turned off.

The retake put the eruption back on the counter where it belonged. The water bursts from the lid rather than the spout, but once "SPLOOSH" is lettered across the top, the panel reads clearly, so I kept it.

Muse also pointed out that the kit's batch script quietly assigned a new random seed for the retake, the number that controls a generation's randomness, which is worth knowing if you're trying to reproduce a result.

<img width="1770" height="1052" alt="Two manga panels of a shouting wizard side by side, the first with a teapot balanced on a large stone fountain and the second with a teapot erupting on a kitchen counter." src="https://github.com/user-attachments/assets/3110ff25-df32-4f7a-87dd-5fc564b343b2" />

*Including the word fountain in the prompt gave me a stone garden fountain in the middle of the kitchen.*

With all five panels approved, the last job was turning them into a page.

## Assemble the Page and Add English Dialogue

This is the stage that makes it feel like manga rather than a set of illustrations, and it's the part most guides on how to make manga with AI skip.

### Panel Layout, Cropping and Grayscale

Muse placed the five panels into the layout template, reading left to right and top to bottom. Three of the panels had a slight warm tint, so it converted every panel to pure grayscale to keep the page consistent.

Panel 5 came out at 16:9, while its space on the page is wider, so Muse cropped it from the bottom and kept the empty space at the top for the speech balloon.

### Lettering Speech Balloons in English

None of the text on this page was generated by PixAI. The kit's documentation is clear that dialogue should never go into an image prompt, so every balloon was added afterward by the kit's `typeset_manga.py` script.

Muse used Comic Neue, kept the line breaks from the script, pointed each balloon's tail at the wizard, and used a spiky shout balloon for Panel 3. The script has no option for text outside a balloon, so the "SPLOOSH" and "plip... plip..." sound effects were added as separate overlays in Comic Neue Bold.

### Checking and Exporting the Finished Page

The first version of the page had four errors. Balloons sat on the wizard's face in four panels, and in Panel 3 the shout balloon covered his head completely. Muse moved and resized them across four more versions.

In the last one, I asked it to move "plip... plip..." off the wizard's leg. The final check showed no errors and three warnings that some balloons sit outside the page's print margin, which only matters if you're printing. I exported the page as a 2000×3000 PNG and read it through at normal size to check spelling, balloon order, and anything covered by text.

<img width="1914" height="1552" alt="Two versions of the same five-panel manga page side by side, the first with a speech balloon covering the wizard's head in Panel 3 and the second with all balloons placed clear of his face." src="https://github.com/user-attachments/assets/605530cd-9d1c-4c3e-831d-7336455520c4" />

*The first build placed speech balloons over the wizard's face, so Muse moved and resized them across four more builds until every face was clear.*

## What It Took: Credits, Time and Corrections

Here's a breakdown of what it took to finish the page:

- **Finished artwork:** The final result is 1 page with 5 panels, read left to right.
- **Images generated:** I generated 7 images, including the character test and the Panel 3 retake, and rejected 2 of them.
- **Retries:** I retook Panel 3 once, and the key check hit 1 network error that worked on the second try.
- **Page builds:** It took 5 builds before every balloon was clear of the characters.
- **PixAI credits:** The project used 28,000 credits in total, at 4,000 per image, and my balance went from 900,500 to 872,500 to confirm it.
- **Muse usage:** Muse used 4% of the Free plan's weekly limit, the first time any of my Muse tests registered on it.
- **Extra costs:** There were none, since the toolkit and Comic Neue are both free.
- **Time:** Each setup or planning step took about a minute, and each image took about a minute to generate.

<img width="1187" height="455" alt="Muse usage settings showing the Free plan with 4% of the weekly limit used and a reset date of November 27." src="https://github.com/user-attachments/assets/16ec4db2-55f7-4fc2-b8b5-0ec4a7ff9a29" />

*After the whole manga project, Muse's Free plan showed 4% of its weekly limit used, the first time any of my tests registered on it.*

The finished page and the work behind it aren't the same number. Five panels took seven images because I tested the characters first and replaced one panel. Your own cost will depend on the model, the settings, and how many revisions you need. If you're learning how to make AI comics on a budget, testing one panel before committing to the rest is the easiest way to keep that number down.

## How to Make Manga with AI: FAQ

Here are quick answers to the questions I had going in:

- **Do I need drawing or coding experience?** You don't need either one. Muse ran every script, and I never drew anything myself. You'll still need to review the script, the prompts, and each panel.
- **Can this workflow keep characters consistent?** It kept them mostly consistent, but not automatically. Simplifying the designs and checking every panel kept both characters recognizable, and my first test still needed a redesign.
- **Can I make the whole page in one generation?** I didn't test that approach. I generated each panel separately, which let me replace Panel 3 without redoing the whole page.
- **Which parts need manual review?** You'll need to review the script, every prompt, each generated panel, and the final balloon placement. Muse checked its own work well, but its first page still put balloons over the wizard's face.

## Try It Yourself: Make Your First Four-Panel Manga with AI

If you want to try this, start smaller than you think. The easiest way to learn how to make manga with AI is to create one original character image on PixAI, write a four-panel story around them, and generate a single test panel before you commit to the full page.

That one panel will tell you whether your character holds up, which is where most of my corrections came from. If you'd like to develop something longer afterward, PixAI's guide to going [from a character idea to a finished manga zine](https://blog.pixai.art/en/from-character-idea-to-finished-manga-zine-a-tsubaki-3-workflow/) covers a separate, larger workflow.

If you're new to PixAI, you can sign up through [my referral link](https://pixai.art/referral?refCode=QCZS8EZS&utm_source=referral&ref_entry=bonus_quest&ref_platform=web&share_copy_id=SC-EN-LINK-T5). Once you've verified your email (for email sign-ups), claimed PixAI's Daily Reward on two separate days, and generated your first image, we each get 50,000 credits.

I also earn 10% back in credits if you top up later. Signing up doesn't unlock API access on its own, but those credits are plenty for testing a character on the website first. You can read the [full referral terms](https://blog.pixai.art/en/pixai-referral-program/) before signing up.
