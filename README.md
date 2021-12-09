<img src="https://bogstandard.github.io/lapland-golf-club/assets/images/logo.svg" width="180" height="180" align="right">

# 🌲 [Lapland Golf Club](https://bogstandard.github.io/lapland-golf-club/)

[Here](https://bogstandard.github.io/lapland-golf-club/) we'll display your golfing scores. 

Contact your local club president if you'd like to join or attach your portrait. These scores will update automatically, so long as you & other players keep your `.scorecard` file up to date!

Click on a row name to display the player's solution!

### How to play

Put a `.scorecard` file in the directory you'll be golfing from & fill it out as you go. Here is [an example](https://github.com/bogstandard/lapland-golf-club/blob/main/Keeper/.scorecard) for a fictional player named _Keeper_.

Within your `.scorecard` you'll list your submissions in a format like below, lines beginning with `@` or `//` or `#` are ignored:

```
@ Bogstandard
# Awaiting the frost
Day One: scorekeeper.test
Day Two: example-day/example-file.test
```

Then provide your local club president with a link to your `.scorecard` in your public repository & they'll do the rest! Your `.scorecard` doesn't need to be in the same repository as the club!

# How do I establish my own club?

Please checkout the **[Lapland Golf Course](https://github.com/bogstandard/lapland-golf-course)** repo or speak with the [groundskeeper](https://github.com/bogstandard/) about this, or even just copy this repo!

The club operates using a custom Jekyll template running on GitHub Pages, so it's reusable year to year. Checkout our `_config.yml` file for more insight.

The rough steps for establishing a club are as follows:
1. Make an empty repo.
2. Set the repo to be a Jekyll based GitHub Pages instance via repo settings.
3. Edit the `_config.yml` file to match the one here, you can always add players any time later.
4. **Optional:** Edit `index.md` to reflect your own choice of club brief.
5. **Optional:** Add any `{day}.png` (eg `24.png`) images you want for the Advent flap to the directory `assets/images/advents/` in the repo.


**Players can play from their own repos but they must be public.**

If your club isn't pulling in the latest updates to the Lapland Golf Course theme then edit the `index.md` file in some way, commit & push your changes then wait a few minutes for GitHub to rebuild the static HTML.

## How do scores update?

The page uses Javascript to dynamically check player's `.scorecard` files on page load, so once they're added to the club by the owner via `_config.yml` their scores will update automatically as they update their scorecards. No one needs to do anything after they're added except play codegolf & keep the scorecards up to date.

![](https://bogstandard.github.io/lapland-golf-club/assets/images/forest.svg)

## What happened to the old Readme format of scores?

[Lapland Golf Club](https://bogstandard.github.io/lapland-golf-club/) has had a renovation & the readme format has been retired. While incredibly fun to build, the Bash based method of collating scores was unpleasant to operate; requiring a manual script run, commit & push each score update. It also required all players to stick within the same repository, which not everyone enjoys. You can still find the old Bash script [here](https://github.com/bogstandard/lapland-golf-club/blob/f39f6693e53bfd54307a1768f1ce94a868c69d80/scorekeeper.md) if you want something amusing to read (it disguised itself as markdown to avoid language detection!).

The club is now HTML and Javascript based, allowing a lot more freedom. Players only have to worry about their `.scorecard` and tell the club president where to find them once, the rest is automated. The club page automatically updates with the latest scores by scraping GitHub from within itself.

The repository the player chooses to play from now doesn't matter, they can play in a seperate repo if they like. So long as it's public the club will find & score it.

### Where is the code for the club?

This repo is an example of a **[Lapland Golf Course](https://github.com/bogstandard/lapland-golf-course) instance**. Everything except customisation options is stored under the theme repo, anything here is custom to this specific instance.

### This is all static??

It's 95% clientside, the only backend is handled by GitHubs internal Jekyll engine, the rest is stretching the limits of GitHub's allowance for within GitHub domain XHR/fetch requests.
