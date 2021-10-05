: '
```bash
'
# Why is this a .md file when its Bash?: to avoid GitHub's language detection
#
# To run: bash scorekeeper.md
# To run real mean: bash scorekeeper.md && git commit . -m "Score keeping." && git push
#
# How .scorecard works:
# - Player puts a .scorecard file in their directory
# - Player lists filepaths to be scored relative to their Directory
# - Format is.. [free text]: [filepath]
#   eg.  Day One Part One: day-one/answer.ext
# - An optional status comment can be set on the file by starting the first line with a hash (#)
#   This optional status comment must be single line and only the first line.
#
# What I do: scan the directories and append a scores table & breakdown to the bottom
# Stuff to be aware of:
# - I overwrite the existing README.md
# - If you want a blankline in the README, make sure you put a space on it
# - If you want a backslash in the README, don't let it be the last character of the line
# - I don't score newlines & trim leading & trailing whitespace from lines
#   because of this sometimes a score may be 1 or 2 out from what your text editor says
#   I am correct.
#
# To Test: setup a dummy player dir Keeper/ and set PRODUCTION to a falsy value

PRODUCTION=0


tally() {

# use the unique bottom of the scores title to find where to whack em
scores_indicator="@@        |____/ \___\___/|_|  \___||___/    @@ ";
# chomp thru the readme to find the score section
# IFS=$'\n' lines=($(<README.template.md));
# for line in ${lines[@]}; do
#   echo -e "${line}";
#   if [ "$line" == "$scores_indicator" ]; then break; fi;
# done;
lines=( )
while IFS= read -r line; do
   echo -e "${line}";
   if [ "$line" == "$scores_indicator" ]; then break; fi;
done <README.template.md




# tally up the scores!!
trim() {
    local trimmed="$1";
    # Strip leading spaces.
    while [[ $trimmed == ' '* ]]; do
       trimmed="${trimmed## }";
    done;
    # Strip trailing spaces.
    while [[ $trimmed == *' ' ]]; do
        trimmed="${trimmed%% }";
    done;
    echo -e "$trimmed"
}

chr() {
  [ "$1" -lt 256 ] || return 1
  printf "\\$(printf '%03o' "$1")"
}

ord() {
  LC_CTYPE=C printf '%d' "'$1"
}

tiny() {
    sed 'y/0123456789/⁰¹²³⁴⁵⁶⁷⁸⁹/' <<< "$1"
}

score_file() {
  score=0;
  IFS=$'\n' file_lines=($(< $1));
  for file_line in ${file_lines[@]}; do
    if [[ $file_line = '//'* ]]; then continue;
    elif  [[ $file_line = '#'* ]]; then continue;
    fi;
    # Strip leading space.
    file_line="$(trim $file_line)";
    line_score=${#file_line};
    score=$[score+line_score];
  done;
  echo -e $score;
}

recurse() {
 for i in "$1"/*;do

    player=$(echo -e "$i" | cut -d "/" -f2);
    player_index=$(ord $player);

    if [ "$player" == "Keeper" ]&&((PRODUCTION)); then continue; fi;

    # its a dir
    if [ -d "$i" ];then
        recurse "$i";

    # its a file
    elif [ -f "$i" ]; then
        if [ "$1" != "." ]; then # its not a root file

          filename="$(basename "$i")"
          if [[ $i == *.txt ]]; then continue; fi; # skip txt
          if [[ $i == *.md ]]; then continue; fi; # skip md
          if [[ $filename =~ (.*)?\.(.*) ]]; then :; else continue; fi; # skip extensionless files

          local score=$(score_file $i);
          players[$player_index]=$player;
          player_totals[$player_index]=$[player_totals[$player_index] + score];
          tiny_running_total=$(tiny ${player_totals[$player_index]});
          echo -e "! ${i} scores [${score}] ⁽${tiny_running_total}⁾";
        fi;
    fi;
 done;
}


# do the magic
echo -e "\n\n@@ Scorecard Breakdown: @@";
echo -e "# Colour indicates effect on running average score!"
for d in */ ; do
  if [[ -d "$d" && ! -L "$d" ]]; then # its a normal dir
    player="$(basename "$d")";

    if [ "$player" == "Keeper" ]&&((PRODUCTION)); then continue; fi;

    player_index=$(ord $player);
    scorecard_path="${d}.scorecard";

    if [ -f "$scorecard_path" ]; then # .scorecard exists

      echo -e "\n! ${player}";

      IFS=$'\n' sc_lines=($(< $scorecard_path));
      line_number=1;
      for l in ${sc_lines[@]}; do

        # check for a file comment
        if [ $line_number == 1 ] && [ "${l::1}" == "#" ];
        then
          echo $l;
          continue;
        fi;

        IFS=: chunks=($l);
        file_name=$(trim ${chunks[1]});
        file_path="${d}${file_name}";

        if [ -f "$file_path" ]; then # referenced file exists
          score=$(score_file $file_path);
          players[$player_index]=$player;
          ((player_file_count[$player_index]++));
          player_totals[$player_index]=$[player_totals[$player_index] + score];
          tiny_running_total=$(tiny ${player_totals[$player_index]});
          running_avg=$[player_totals[$player_index] / line_number];
          if ((running_avg < player_running_avgs[$player_index])); then
            symbol='+'
          else
            if [ $line_number == 1 ]; then
              symbol=' '
            else
              symbol='-'
            fi;
          fi;
          player_running_avgs[$player_index]=$running_avg
          tiny_running_avg=$(tiny $running_avg)

          echo -e "${symbol}  ${chunks[0]} ⟶ ${file_path} scores [${score}]     ⁽ᵃᵛᵍ ${tiny_running_avg}⁾";
          ((++line_number))
        fi;

      done;

    fi;
  fi;
done


# loop final scores
echo -e "\n\n@@ Scorecard Totals: @@";
for i in ${!players[@]};do

  if [ "${players[$i]}" == "Keeper" ]&&((PRODUCTION)); then continue; fi;

  symbol="+";
  for opp in ${!players[@]};do
    if [ "${players[$opp]}" == "Keeper" ]&&((PRODUCTION)); then continue; fi;
    if (("${player_totals[$opp]}" < "${player_totals[$i]}")); then symbol="!"; fi;
  done;

  echo -e "${symbol} ${players[$i]} scores a total [${player_totals[$i]}]";
done;


# loop final avgs
echo -e "\n\n@@ Scorecard Averages: @@";
for i in ${!players[@]};do
  if [ "${players[$i]}" == "Keeper" ]&&((PRODUCTION)); then continue; fi;
  player_avgs[$i]=$[player_totals[$i] / player_file_count[$i]];

  symbol="+";
  for opp in ${!players[@]};do
    if [ "${players[$opp]}" == "Keeper" ]&&((PRODUCTION)); then continue; fi;
    if [[ "${player_avgs[$opp]}" < "${player_avgs[$i]}" ]]; then symbol="!"; fi;
  done;

  echo -e "${symbol} ${players[$i]} ⟶ [${player_totals[$i]}] ÷ [${player_file_count[$i]}] files ⟶ [${player_avgs[$i]}]";
done;


# make totals 0 again
players=();
player_totals=();


# do the magic
echo -e "\n\n@@ Global Breakdown: @@";
echo -e "+ This covers all files, including those which do not";
echo -e "+ appear on scorecards. This is for informational";
echo -e "+ purposes only and is not regarded in any way as ";
echo -e "+ counting towards scores or averages. ";
recurse .
echo -e "+ note: this ignores txt, md and extensionless files!";


# last things!
dt=$(date '+%d/%m/%Y %H:%M:%S');
echo -e "\n\n\nUpdated ${dt} Local time\n\`\`\`";
}

#tally;
tally > README.md
cp -fr README.md index.md
