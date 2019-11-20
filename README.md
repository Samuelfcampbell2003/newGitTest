#!/bin/bash

# moonphase--Reports the phase of the moon (really the percentage of
#    illumination) for today or a specified date

# Format of Moongiant.com query:
#   http://www.moongiant.com/phase/MM/DD/YYYY

# If no date is specified, use "today" as a special value.

if [$# -eq 0]; then
   thedate="today"
else
  # Date specified. Let's check whether it's in the right format.
   mon="$(echo $1 | cut -d/ -f1)"
   day="$(echo $1 | cut -d/ -f2)"
  year="$(echo $1 | cut -d/ -f3)"

 if [-z "$year" -o -z "$day"]; then # Zero length?
  echo "Error: valid date format is MM/DD/YYYY"
  exit 1
fi
  thedate="$1" # No error checking = dangerous
fi

url="http://www.moongiant.com/phase/$thedate"
 pattern="Illumination:"
 phase="$(curl -s "$url" | grep "$pattern" | tr ',' '\
' | grep "$pattern" | sed 's/[^0-9]//g')"

# Site output format is "Illumination: <span>NN%\n<\/span>"

if ["$thedate" = "today"]; then
   echo "Today the moon is ${phase}% illuminated."
else
   echo "On $thedate the moon = ${phase}% illuminated."
fi

exit 0
