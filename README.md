# shell-scripting-assignment
**Question**:Write a shell script to change the values in a file(i.e sig.conf) according to the input passed to the script. The script should ask for all four inputs from the user & also validate the input. Below are the details of input. In full bracket options are given, you have to restrict the user pass single value for each input from the provided options in the full bracket

Input:-

1) Component Name [INGESTOR/JOINER/WRANGLER/VALIDATOR]
2) Scale [MID/HIGH/LOW]
3) View [Auction/Bid]
4) Count [single digit number]

Solution:

- **STEP 1**:

create a sig.conf file with some content in it like

Auction ; MID ; INGESTOR ; ETL ; vdopia-etl= 5

- **STEP 2**:

Read the input from the user by using following code:

#!/bin/bash

validate_input() {
  
  local input=$1
  
  local valid_options=$2
  
  if [[ ! " ${valid_options} " =~ " ${input} " ]]; then
    echo "Invalid input! Please enter one of: $valid_options"
    exit 1
  
  fi

}


#Component Name

read -p "Enter Component Name [INGESTOR/JOINER/WRANGLER/VALIDATOR]: " COMPONENT

validate_input "$COMPONENT" "INGESTOR JOINER WRANGLER VALIDATOR"


#Scale

read -p "Enter Scale [MID/HIGH/LOW]: " SCALE

validate_input "$SCALE" "MID HIGH LOW"


#View

read -p "Enter View [Auction/Bid]: " VIEW

validate_input "$VIEW" "Auction Bid"


#count

read -p "Enter Count [0-9]: " COUNT

if ! [[ "$COUNT" =~ ^[0-9]$ ]]; then
    echo "Invalid Count! Must be a single digit number."
    exit 1

fi

if [[ "$VIEW" == "Auction" ]]; then
    KEY="vdopiasample"
else
    KEY="vdopiasample-bid"
fi

#using the sed command to replace the line

PATTERN="$VIEW ; $SCALE ; $COMPONENT ; ETL ; $KEY= "


sed -i "/^$VIEW ; $SCALE ; $COMPONENT ; ETL ; $KEY=/ s/$KEY[0-9]/$KEY$COUNT/" sig.conf

echo "Configuration updated successfully!"


-**STEP 3**:CHANGE THE PERMISSION OF THE FILE

chmod +x update_conf.sh


-**STEP 4**:RUN THE SCRIPT

  ./update_conf.sh

  


