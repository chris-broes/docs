# Sauce Labs Docs

> This repo contains the markdown documentation for Sauce Labs.

## Editing Markdown

Every [markdown](http://daringfireball.net/projects/markdown/) document in the `markdown` folder of this repo has a required metadata section which should be valid a JavaScript object containing the `title`, `description`(used in the meta description), `category`(used in the nav and URL), and the `index`(used to determine the document's position in the navigation relative to other documents in the same category). You can optionally include an `image` for a doc which will be used in the nav, search results, and title section on the doc's page. The supported version of markdown is GitHub flavored markdown as described [here](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

### Naming conventions

All markdown file names should be lowercase, dasherized, and be placed inside the corresponding category folder. For example a doc with the category `Tutorials` entitled `Sample Tutorial` would be located at `markdown/tutorials/sample-tutorial.md`. Images relating to docs are found in the same relative location as markdown files. A main doc `image` would be located at `markdown/tutorials/sample-tutorial.png` while an image in the content of the doc would be located at `markdown/tutorials/sample-tutorial/content-image.png`.

## Custom Markdown processing

Any code block containing `sauceUsername` or `sauceAccessKey` will either get replaced by the logged-in user's credentials or be replaced by `YOUR_USERNAME`, and `YOUR_ACCESS_KEY` respectively. If the user is logged out, there will be a button present when hovering over code blocks prompting the user to login. Additionally, code blocks with a programming language defined will have a button present on hover to allow users to copy code to their clipboard.
