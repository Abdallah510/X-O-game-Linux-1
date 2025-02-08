#here we initialize the score of the two players and the dimention of the grid and the mark
score1=0
score2=0
dim=0
mark=
#here we declare the grid
declare -A grid
#this is the main function that runs the program
main(){
echo "please enter player one name with X"
read player1
echo "please enter player two name with O"
read player2
echo "please enter the number of moves you which to end the game on"
read duration

initializeGrid

turn=0
while [ "$turn" -lt "$duration" ]
do
echo "$player1 score=$score1"
echo "$player2 score=$score2"
echo "$player1 trun"
mark=X
display
moveHandle
calculateScore
echo"$player1 score=$sore1"
echo"$player2 score=$sore2"
echo "$player2 trun"
mark=O
display
moveHandle
calculateScore
turn=$((turn+1))
done
echo "$player1 score=$score1"
echo "$player2 score=$score2"
display
}
calculateScore(){
player="$mark"
opponent=
playerScore=0
if [ "$mark" = "X" ]
then
opponent=O
else
opponent=X
fi
#to check for diagonal alignment
for ((i=0 ; i<"$dim" ; i++))
do
row=""
for ((j=0 ; j<"$dim" ; j++))
do
row+="${grid[$i,$j]}"
done
if [ "$row" = "$player$player$player" ]
then
playerScore=$((playerScore+2))
elif [ "$row" = "$opponent$opponent$opponent" ]
then
playerScore=$((playerScore-3))
fi
done
#to check for vertical alignment
for ((i=0 ; i<"$dim" ; i++))
do
col=""
for ((j=0 ; j<"$dim" ; j++))
do
col+="${grid[$j,$i]}"
done
if [ "$col" = "$player$player$player" ]
then
playerScore=$((playerScore+2))
elif [ "$col" = "$opponent$opponent$opponent" ]
then
playerScore=$((playerScore-3))
fi
done
#to check for diagonal alignment form left to right
diagonal1=""
for ((i=0 ; i<"$dim" ; i++))
do
diagonal1+="${grid[$i,$i]}"
done
if [ "$diagonal1" = "$player$player$player" ]
then
playerScore=$((playerScore+2))
elif [ "$diagonal1" = "$opponent$opponent$opponent" ]
then
playerScore=$((playerScore-3))
fi
diagonal2=""
for ((i=0 ; i<"$dim" ; i++))
do
diagonal2+="${grid[$i,$(($dim-1-$i))]}"
done
if [ "$diagonal2" = "$player$player$player" ]
then
playerScore=$((playerScore+2))
elif [ "$diagonal2" = "$opponent$opponent$opponent" ]
then
playerScore=$((playerScore-3))
fi

if [ "$player" = "X" ]
then
score1=$((score1+playerScore))
else
score2=$((score2+playerScore))
fi
}
moveHandle(){
echo "choose a move"
echo "move1:place mark one empty cell"
echo "move2:remove mark"
echo "move3:exchange row"
echo "move4:exchane column"
echo "move5:exchange position"
read move
case "$move"
in
1)move1;;
2)move2;;
3)move3;;
4)move4;;
5)move5;;
*)echo "not a valid option"
moveHandle;;
esac
}
move1(){
echo "enter row[0-$(($dim-1))] and column[0-$(($dim-1))] you wish to put your mark in"
read row col
if [ "$row" -ge "$dim" -o "$row" -lt 0 ]
then
echo "invlaid coordinates"
move1
return
elif [ "$col" -ge "$dim" -o "$col" -lt 0 ]
then
echo "invlaid coordinates"
move1
return
fi
if [ "${grid[$row,$col]}" != " " ]
then
echo "cell is not empty"
moveHandle
return
fi
grid["$row","$col"]="$mark"
}

move2(){
echo "enter row[0-$(($dim-1))] and column[0-$(($dim-1))] you wish to remove your mark in"
read row col
if [ "$row" -ge "$dim" -o "$row" -lt 0 ]
then
echo "invlaid coordinates"
move2
return
elif [ "$col" -ge "$dim" -o "$col" -lt 0 ]
then
echo "invlaid coordinates"
move2
return
fi
if [ "${grid[$row,$col]}" = " " ]
then
echo "cell is empty"
moveHandle
return
fi
if [ "${grid[$row,$col]}" != "$mark" ]
then
echo "not your mark to remove"
move2
return
fi
grid["$row","$col"]=" "
if [ "$mark" = "X" ]
then
score1=$((scoor1+1))
else
score2=$((scoor2+1))
fi
}
move3(){
echo "enter raw numbers[1-$dim] you want to exchange using the rxy format"
read input
if [ "${input:0:1}" != "r" ]
then
echo "invlid formate"
move3
return
fi
if [[ "${input:1:1}" =~ [0-9] ]];
then
:
else
echo "invlid formate"
move3
return
fi
if [[ "${input:2:1}" =~ [0-9] ]];
then
:
else
echo "invlid formate"
move3
return
fi
row1=${input:1:1}
row2=${input:2:1}
if [ "$row1" -eq "$row2" ]
then
echo "rows cant be equal"
move3
return
fi
if [ "$row1" -gt "$dim" -o "$row1" -lt 1 ]
then
echo "invlid coordinates"
move3
return
fi
if [ "$row2" -gt "$dim" -o "$row2" -lt 1 ]
then
echo "invlid coordinates"
move3
return
fi
for ((i=0 ; i<"$dim" ; i++))
do
temp="${grid[$(($row1-1)),$i]}"
grid[$(("$row1"-1)),"$i"]="${grid[$(($row2-1)),$i]}"
grid[$(("$row2"-1)),"$i"]="$temp"
done
if [ "$mark" = "X" ]
then
score1=$((scoor1-1))
else
score2=$((scoor2-1))
fi
}
move4(){
echo "enter column numbers[1-$dim] you want to exchange using the cxy format"
read input
if [ "${input:0:1}" != "c" ]
then
echo "invlid formate"
move4
return
fi
if [[ "${input:1:1}" =~ [0-9] ]];
then
:
else
echo "invlid formate"
move4
return
fi
if [[ "${input:2:1}" =~ [0-9] ]];
then
:
else
echo "invlid formate"
move4
return
fi
col1=${input:1:1}
col2=${input:2:1}
if [ "$col1" -eq "$col2" ]
then
echo "columns cant be equal"
move4
return
fi
if [ "$col1" -gt "$dim" -o "$col1" -lt 1 ]
then
echo "invlid coordinates"
move4
return
fi
if [ "$col2" -gt "$dim" -o "$col2" -lt 1 ]
then
echo "invlid coordinates"
move4
return
fi
for ((i=0 ; i<"$dim" ; i++))
do
temp="${grid[$i,$(($col1-1))]}"
grid["$i",$(("$col1"-1))]="${grid[$i,$(($col2-1))]}"
grid["$i",$(("$col2"-1))]="$temp"
done
if [ "$mark" = "X" ]
then
score1=$((scoor1-1))
else
score2=$((scoor2-1))
fi
}
move5(){
echo "enter row[0-$(($dim-1))] and column[0-$(($dim-1))] numbers you want to exchange using the exyuv format"
read input
if [ "${input:0:1}" != "e" ]
then
echo "invlid formate"
move5
return
fi
if [[ "${input:1:1}" =~ [0-9] ]];
then
:
else
echo "invlid formate"
move5
return
fi
if [[ "${input:2:1}" =~ [0-9] ]];
then
:
else
echo "invlid formate"
move5
return
fi
if [[ "${input:3:1}" =~ [0-9] ]];
then
:
else
echo "invlid formate"
move5
return
fi
if [[ "${input:4:1}" =~ [0-9] ]];
then
:
else
echo "invlid formate"
move5
return
fi
row1=${input:1:1}
col1=${input:2:1}
row2=${input:3:1}
col2=${input:4:1}
if [ "$row1" -eq "$row2" -a "$col1" -eq "$col2" ]
then
echo "cells cant be equal"
move5
return
fi
if [ "$col1" -ge "$dim" -o "$col2" -ge "$dim" ]
then
echo "invlid coordinates"
move5
return
fi
if [ "$row1" -ge "$dim" -o "$row1" -ge "$dim" ]
then
echo "invlid coordinates"
move5
return
fi
if [ "${grid[$row1,$col1]}" = "${grid[$row2,$col2]}" ]
then
echo "cant change your own mark"
move5
return
fi
temp="${grid[$row1,$col1]}"
grid["$row1","$col1"]="${grid[$row2,$col2]}"
grid["$row2","$col2"]="$temp"
if [ "$mark" = "X" ]
then
score1=$((scoor1-2))
else
score2=$((scoor2-2))
fi
}
initializeGrid(){
echo "if you want to start an empty game press 1 if you want to load a game press 2"
read gameType
if [ "$gameType" -eq 1 ]
then
echo  "enter the dimention of the grid, options are:3,4,5"
read dim
if [ "$dim" -ne 3 -a "$dim" -ne 4 -a "$dim" -ne 5 ]
then
echo "invalid dimention"
initializeGrid
return
fi
for ((i=0 ; i<dim ; i++))
do
for ((j=0 ; j<dim ;j++))
do
grid["$i","$j"]=" "
done
done
elif [ "$gameType" -eq 2 ]
then
loadFile
echo "file loaded succesfully"
else
echo "invalid option"
initializeGrid
fi
}

loadFile(){
echo "enter name of file you wish to get the game from"
read file
if [ ! -e "$file" ]
then
echo "file does not exist"
initializeGrid
return
else
j=0
while IFS='|' read -r -a row
do
dim="${#row[@]}"
for((i=0 ; i<"${#row[@]}" ; i++))
do
if [ "${row[$i]}" != "" ]
then
grid["$j","$i"]="${row[$i]}"
else
grid["$j","$i"]=""
fi
done
j=$((j+1))
done < "$file"
fi
}

display(){
for ((i=0 ; i<"$dim" ; i++))
do
for ((j=0 ; j<"$dim" ;j++))
do
if [ "$j" -eq $(("$dim"-1)) ]
then
printf "%s" "${grid[$i,$j]}"
else
printf "%s|" "${grid[$i,$j]}"
fi
done
printf "\n"
done
}
main