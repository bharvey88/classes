# Teach these classes

Every class on this site is free to copy, change, and teach. The license ([CC BY 4.0](https://github.com/bharvey88/classes/blob/main/LICENSE)) asks one thing: keep a credit link back to [the project](https://github.com/bharvey88/classes) somewhere.

You don't need to be a programmer. If you can edit a document in a web browser, you can run your own version of a class at your makerspace, library, or user group.

## What "forking" means

These classes live on GitHub, a site where people share projects as folders of plain text files. A **fork** is your own copy of a project, attached to your own account.

Your fork is fully yours. Change the gear list, swap my stories for yours, rename the whole thing. My original stays untouched, and you don't need my permission. That's the point of publishing it this way.

## Make your copy (about 5 minutes)

1. **Create a free GitHub account** at [github.com/signup](https://github.com/signup) if you don't have one.
2. **[Click here to fork the classes repo](https://github.com/bharvey88/classes/fork)**, then press the green **Create fork** button. GitHub copies everything into your account.

That's it. Everything from here on happens inside your copy.

## Edit a class

Each class is one folder under `docs/`. For Intro to Home Assistant that's `docs/intro-to-home-assistant/`, and the two files that matter are:

- `outline.md`: the speaker outline, what you say and when
- `handout.md`: the take-home guide attendees keep

To edit one, open it on GitHub and click the **pencil icon** in the top right. The files are Markdown, which is just text with a little punctuation for formatting: `#` makes a heading, `**bold**` makes bold, `-` makes a bullet. You'll pick it up by looking at what's already there.

When you're done, click **Commit changes**. That's "save" in GitHub speak.

Swap in your own venue, your own demo devices, your own automation stories. The class lands because the examples are real, so make them yours.

One rule: keep `outline.md` and `handout.md` plain Markdown, no HTML or theme-specific syntax. The same files also become the printable docx and PDF versions, and anything fancier turns into garbage in print.

## Publish your website

Your fork can have its own free site like this one:

1. In your fork, go to **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to **GitHub Actions**.
3. Go to the **Actions** tab. If GitHub asks you to enable workflows, do it, then run the **Deploy site** workflow (or just commit any edit).

A few minutes later your site is live at `https://YOUR-USERNAME.github.io/classes/`. (If you own a domain and want the site on it, add it under **Settings → Pages → Custom domain** once the site is up.)

One more polish step: edit `mkdocs.yml` (in the top folder of your fork) and change `site_url`, `repo_url`, and `repo_name` to point at your fork instead of mine. The site works without this, but its "edit" links will point at me until you do.

## Get printable handouts

Every time you commit a change to a class, GitHub rebuilds that class's Word and PDF files automatically and attaches them to a rolling release (for example [`intro-ha`](https://github.com/bharvey88/classes/releases/tag/intro-ha) on my copy; your fork gets its own). Download the handout from your fork's **Releases** page, print it, hand it out.

The [repo README](https://github.com/bharvey88/classes#readme) covers local previews and the rest of the plumbing, if you want it.

## Stuck?

[Open an issue](https://github.com/bharvey88/classes/issues) on the original project and I'll try to help. And if you teach one of these, tell me how it went. I'd genuinely like to hear.
