<img src="https://bogstandard.github.io/lapland-golf-club/assets/images/logo.svg" width="180" height="180" align="right">

# [Lapland Golf Club](https://bogstandard.github.io/lapland-golf-club/)

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

### How do I establish my own club?

Please speak with the [groundskeeper](https://github.com/bogstandard/) about this or copy this repo! Take a look at the `_config.yml` file and you should figure it out if you've got smarts. Hint; Use Github Pages with our custom theme.

![](https://bogstandard.github.io/lapland-golf-club/assets/images/forest.svg)

## What happened to the old Readme format of scores?

[Lapland Golf Club](https://bogstandard.github.io/lapland-golf-club/) has had a renovation & the readme format has been retired. While incredibly fun to build, the Bash based method of collating scores was unpleasant to operate; requiring a manual script run, commit & push each score update. It also required all players to stick within the same repository, which not everyone enjoys. You can still find the old Bash script [here](https://github.com/bogstandard/lapland-golf-club/blob/f39f6693e53bfd54307a1768f1ce94a868c69d80/scorekeeper.md) if you want something amusing to read (it disguised itself as markdown to avoid language detection!).

The club is now HTML and Javascript based, allowing a lot more freedom. Players only have to worry about their `.scorecard` and tell the club president where to find them once, the rest is automated. The club page automatically updates with the latest scores by scraping GitHub from within itself.

The repository the player chooses to play from now doesn't matter, they can play in a seperate repo if they like. So long as it's public the club will find & score it.

### Where is the code for the club?

The club operates using a custom Jekyll template, so it's reusable year to year. Checkout our `_config.yml` file for more insight.

This repo is an example of a **[Lapland Golf Course](https://github.com/bogstandard/lapland-golf-course) instance**.

