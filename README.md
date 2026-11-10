# Codealong 9 — Clean Up Your CSS

**Course:** Web Design 1: Concepts & Coding
**Week:** 9
**Time:** most of class

---

## What we're doing

Your page has a grid, flexbox, a media query, and real content. It works. Tonight we make it **consistent** and **usable by everyone**.

Most pages at this point have the same problems: the same blue typed in five slightly different ways, margins of `13px` here and `20px` there, links you can't see when you tab to them, and gray text that's hard to read. None of these break the page. All of them make it feel unfinished.

We'll practice five tools on a small page:

1. **Custom properties**: name your colors and spacing once, then reuse them.
2. **A spacing scale**: a few sizes instead of random numbers.
3. **Images that fit**: let images size themselves instead of forcing a height.
4. **Contrast**: checking that text is actually readable.
5. **Focus styles**: making the page work with a keyboard.

Then we'll run an accessibility audit and read the results together.

You'll work in a separate practice repo, so nothing here can break your project.

---

## Setup — Make your own copy

1. Open the template repo: https://github.com/profJoey/touro-f26-codealong-clean-up-css
2. Click the green **Use this template** button → **Create a new repository**.
3. Name it `week9-codealong`. Leave it **Public**. Click **Create repository**.
4. In your new repo, click **Code** → **Codespaces** → **Create codespace on main**.
5. When it opens, right-click `index.html` → **Open with Live Server**.

**You should see:** the Brightside page again. It looks fine at first glance. Open `style.css` and scroll through it. Count how many different blues you can find.

---

## Part 1 — Custom properties

### Step 1 — Find the repeats

Look through `style.css` for colors. You'll find things like:

```css
header { background: #1d4ed8; }
.button { background: #1e4fd8; }
a { color: #1d4fd9; }
```

These are meant to be the same blue, but they're all slightly different. If the client asks for a new brand color, you'd have to find and change every one, and you'd probably miss some.

### Step 2 — Name them once

At the **very top** of `style.css`, add:

```css
:root {
  /* Colors */
  --color-primary: #1d4ed8;
  --color-text: #1f2937;
  --color-background: #ffffff;
  --color-muted: #4b5563;

  /* Spacing */
  --space-s: 0.5rem;
  --space-m: 1rem;
  --space-l: 2rem;
  --space-xl: 4rem;

  /* Type */
  --font-body: system-ui, sans-serif;
  --font-heading: Georgia, serif;
}
```

- `:root` is the whole page. Anything defined here can be used anywhere.
- Custom property names always start with two dashes: `--`.
- The names describe the **job**, not the value. `--color-primary`, not `--blue`. If the brand color changes to green, the name still makes sense.

### Step 3 — Use them

Replace the hardcoded values with `var()`:

```css
header { background: var(--color-primary); }
.button { background: var(--color-primary); }
a { color: var(--color-primary); }
```

**Now try this:** change `--color-primary` to `#15803d` (green) and save.

**You should see:** the header, buttons, and links all turn green at once. Change it back.

That's the whole point. One change, everywhere.

---

## Part 2 — A spacing scale

### Step 4 — Find the random numbers

Search `style.css` for `margin` and `padding`. You'll find `12px`, `15px`, `18px`, `20px`, `25px`... No one can tell the difference between 15 and 18, but the page ends up feeling slightly off.

### Step 5 — Snap to the scale

Replace every spacing value with one of your four sizes:

```css
section {
  padding: var(--space-l);
}

.page {
  gap: var(--space-m);
}

.feature h3 {
  margin-bottom: var(--space-s);
}
```

Rule of thumb: **small things inside a box get small spacing, space between big sections gets big spacing.** If you can't decide between two sizes, pick the bigger one.

**You should see:** the page looks calmer. Nothing moved much, but the gaps now repeat in a rhythm.

> **Where's `gap`?** Remember from last week: on a grid or flex container, use `gap` instead of margins on the children. Use your spacing variables there too.

### Step 6 — Remove leftover margins

Look at the three feature boxes. The right edge of the last box doesn't line up with the hero above it.

Find the `.feature` rule:

```css
.feature {
  grid-column: span 4;
  margin-right: 17px;
}
```

The `.page` grid already puts a `gap` between the boxes. The `margin-right` is left over from before the page was a grid, so now the spacing is doubled and the last box comes up short. Delete the `margin-right` line.

**You should see:** all three feature boxes line up with the hero's right edge.

> **When you switch something to flex or grid, look for leftover margins on the children.** `gap` replaces them. If you keep both, the spacing doubles.

---

## Part 3 — Images that fit

### Step 7 — The one image rule every page needs

Make your browser window narrow, about phone width.

**You should see:** the hero image sticks out past the right edge and the page scrolls sideways. An image shows at its full size (this one is 1200px wide) unless you tell it otherwise.

Where the comment says `STEP 7`, near the top of `style.css`, add:

```css
img {
  max-width: 100%;
  height: auto;
  display: block;
}
```

- `max-width: 100%` means "never wider than your container."
- `height: auto` keeps the image's shape when it shrinks.
- `display: block` removes a small gap that browsers leave under images.

Put this rule at the top of every project, next to `:root`.

### Step 8 — Take out the fixed height

The image fits now, but it looks stretched. Find `.hero img`:

```css
.hero img {
  display: block;
  height: 600px;
  ...
}
```

`height: 600px` overrides your `height: auto`. As the image gets narrower it stays 600px tall, so the picture gets squished. Delete the `height` line (and the extra `display: block`, since your `img` rule handles that now).

**You should see:** the image keeps its shape at every window width.

> **Don't set a fixed `height` on images or sections.** Let the content decide how tall the box is. If you need an image to be a certain shape, use `aspect-ratio: 16 / 9;` with `object-fit: cover;` instead of a pixel height.

---

## Part 4 — Contrast

### Step 9 — Check the gray text

Find the `.tagline` paragraph under the hero. It's light gray (`#a3a3a3`) on white.

1. Right-click the tagline → **Inspect**.
2. In the Styles panel, click the little color square next to `color`.
3. Look for **Contrast ratio**. You'll see a number and a ✗ or ✓.

**You should see:** around **2.5**, with a ✗. Body text needs at least **4.5** to pass.

### Step 10 — Fix it

Change the tagline color to your muted variable:

```css
.tagline {
  color: var(--color-muted);
}
```

Check the contrast ratio again. It should now pass.

> **Light gray text looks "designy," but a lot of people can't read it:** anyone with low vision, anyone on a phone in sunlight, anyone on a cheap screen. If the contrast check fails, make the color darker.

---

## Part 5 — Focus styles

### Step 11 — Put the mouse down

Click in the address bar, then press **Tab** over and over. Watch the page.

**You should see:** you can't tell where you are. The starter CSS has `outline: none` on links and buttons, which hides the focus ring. Someone who uses a keyboard instead of a mouse is now lost.

### Step 12 — Bring it back, better

Delete the `outline: none` rule. Then add:

```css
a:hover,
.button:hover {
  opacity: 0.85;
}

a:focus-visible,
.button:focus-visible {
  outline: 3px solid var(--color-primary);
  outline-offset: 3px;
}
```

- `:hover` is for the mouse.
- `:focus-visible` is for the keyboard. It only shows when someone is tabbing, so mouse users don't see a ring every time they click.

Tab through the page again. **You should see:** a clear outline around each link and button as you reach it.

---

## Part 6 — Run an audit

### Step 13 — Lighthouse

1. Open your Live Server preview in its own Chrome tab.
2. Right-click → **Inspect** → find the **Lighthouse** tab (it may be under `>>`).
3. Check **Accessibility** only. Click **Analyze page load**.

**You should see:** a score out of 100 and a list of problems. Read through them together. Common ones:

| Lighthouse says | What it means |
|---|---|
| Image elements do not have `[alt]` attributes | An image is missing `alt` text |
| Background and foreground colors do not have a sufficient contrast ratio | Some text is too light |
| Heading elements are not in a sequentially-descending order | You skipped a level, like `h1` → `h3` |
| Links do not have a discernible name | A link has no text, like an icon with no label |

Fix one item from the list and run it again.

> **A 100 doesn't mean your page is perfect,** and a 90 doesn't mean it's broken. Lighthouse catches the easy things. You still have to tab through your page and read it yourself.

---

## Before you leave

- [ ] I can explain what `:root` and `var()` do
- [ ] Changing one variable changed the color everywhere
- [ ] My spacing uses only the four scale sizes
- [ ] No leftover margins on children of a grid or flex container
- [ ] Images shrink with the window and keep their shape (no fixed `height`)
- [ ] I checked a contrast ratio in DevTools
- [ ] I can tab through the page and see where I am
- [ ] I ran Lighthouse and fixed one thing

Next: do the same thing to your own project in **Assignment 9**.
