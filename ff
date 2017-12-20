#!/bin/bash

# Written by Giovanni Giglio (@GioGiglio)

FILES_PATH=""

function on_entry_found {
	FILES_PATH="$FILES_PATH $1"
}


function on_files_found {
	local lenght=$(echo $FILES_PATH | wc -w) 
	if [ $lenght -eq 1 ]; then
		echo "File found!"
		echo "$FILES_PATH"
		on_file_found $FILES_PATH
		exit
	else	
		echo "Multiple files found!"
		echo "Please select one"
		select opt in ${FILES_PATH[@]}; do
			if [ -z "$opt" ]; then
				echo "Insert a valid option!"
			else
				on_file_found $opt
				exit
			fi
		done
	fi
}
		

function on_file_found {
	OPTIONS="Navigate Open Cat 
Exit"
	select opt in $OPTIONS;
	do
	if [ "$opt" = "Navigate" ]; then
		local path=$(dirname $1)
		xdg-open $path &> /dev/null
		exit

	elif [ "$opt" = "Open" ]; then
		xdg-open $1 &> /dev/null
		exit

	elif [ "$opt" = "Cat" ]; then
		echo 
		cat $1
		echo 

	elif [ "$opt" = "Exit" ]; then
		exit
	else
		echo Insert a valid option!
	fi
	done
}

function search_in_dir {
	local dir=$1
	local searched_file=$2
	cd $dir
	search_file $searched_file
	cd ..
}

function search_file {
	searched_file=$1
    	local current_dir_files=$(ls)
    	for file in $current_dir_files;
    	do

	if [ "$searched_file" = "$file" ]; then
		on_entry_found "$(pwd)/$file"
	fi

	if [ -d "$file" ]; then
		search_in_dir $file $searched_file
	fi
	done
}

if [ -z "$1" ]; then
	printf "Insert filename: "
	read filename
else
	filename=$1
fi

cd /home/
search_file $filename

if [ -z "$FILES_PATH" ]; then
	echo "File not found!"
	exit
else
	on_files_found
fi
