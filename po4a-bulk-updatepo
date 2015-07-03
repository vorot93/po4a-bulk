#!/bin/bash

# Copyright (C) 2015 Artem Vorotnikov
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU Lesser General Public License as published by
# the Free Software Foundation; either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU Lesser General Public License for more details.
#
# You should have received a copy of the GNU Lesser General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.
#

if [ "$1" = "--help" ]
then
  echo -e "\n$0 [SOURCE DIR] [SOURCE FORMAT] [SOURCE EXTENSION] [PO DIR] LINGUAS\n"
  echo -e "Bulk PO update via po4a-updatepo.\n"
  echo -e "\n$0 allows you to update gettext translations in accordance with master files.\n"
  echo -e "\nOperands (mandatory):"
  echo -e "\n   SOURCE DIR            - directory containing source (i.e. untranslated) files."
  echo -e "\n   SOURCE FORMAT         - format of the files to parse. See po4a-gettextize --help-format for more info."
  echo -e "\n   SOURCE EXTENSION      - extension of the files to parse."
  echo -e "\n   PO DIR                - path of gettext translations. Expected structure must match SOURCE DIR except while being nested in language directory and having .po for each translation file appended. That is, for every SOURCE DIR/PATH/FILE.EXT there should be PO DIR/LANG/PATH/FILE.EXT.po"
  echo -e "\n   LINGUAS               - list of languages to process. Please use parentheses."
  echo -e "\nUsage example:"
  echo -e "\n   $0 project/source asciidoc adoc project/po 'de fr ru'\n"
  echo -e "\nAvailable options:"
  echo -e "\n   --help - show help"
  echo -e ""
  echo -e "\nThis program is a part of po4a-bulk package that provides easy mass manipulation for groups of files.\n"
  exit
fi

SRCDIR=$1
SRCFORMAT=$2
SRCEXT=$3
PODIR=$4
LINGUAS=$5

if !([ -r "$SRCDIR" ] && [ "$SRCFORMAT" != "" ] && [ "$SRCEXT" != "" ] && [ "$PODIR" != "" ])
then
  echo "Invalid operand(s)."
  echo "Try $0 --help for more information."
  exit
elif !([ "$LINGUAS" != "" ])
then
  echo "No languages specified"
  echo "Try $0 --help for more information."
  exit
fi

echo -e "\nYour selected locales: $LINGUAS\n"

FILES=$(find $SRCDIR -printf "%P\n" | grep .$SRCEXT)

for lang in $LINGUAS
do
  for file in $FILES
  do
    basepath=$(echo $file | sed "s|.$SRCEXT||")
    po="$lang/$basepath.${SRCEXT}.po"

    echo "Updating ${PODIR}/$po from $basepath.${SRCEXT}"
    po4a-updatepo -M UTF-8 -f ${SRCFORMAT} -m $SRCDIR/$file -p $PODIR/$po
  done
done
