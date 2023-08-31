## Using the "help" command provides detailed command information

## Example
```shell
# View the list of files in the FONT.PAK
lucksystem pak \
  -s data/LB_EN/FONT.PAK \
  -L

# Extract all files from FONT.PAK to the "temp" directory
lucksystem pak extract \
  -i data/LB_EN/FONT.PAK \
  -o data/LB_EN/FONT.txt \
  --all data/LB_EN/temp

# Extract the 6th file (index starts from 0) from FONT.PAK
lucksystem pak extract \
  -i data/LB_EN/FONT.PAK \
  -o data/LB_EN/5 \
  --index 5

# Replace the corresponding file inside FONT.PAK with the file from the "temp" directory
lucksystem pak replace \
  -s data/LB_EN/FONT.PAK \
  -o data/LB_EN/FONT.out.PAK \
  -i data/LB_EN/temp

# Replace the file named "info32" in FONT.PAK
lucksystem pak replace \
  -s data/LB_EN/FONT.PAK \
  -o data/LB_EN/FONT.out.PAK \
  -i data/LB_EN/temp/info32 \
  --name info32

# Replace the files in FONT.PAK that have a listing in FONT.txt
lucksystem pak replace \
  -s data/LB_EN/FONT.PAK \
  -o data/LB_EN/FONT.out.PAK \
  -i data/LB_EN/FONT.txt \
  --list

# Extract CZ files to PNG images
lucksystem image export \
  -i data/LB_EN/FONT/明朝32 \
  -o data/LB_EN/0.png

# Import PNG images into CZ files
lucksystem image import \
  -s data/LB_EN/FONT/明朝32 \
  -i data/LB_EN/0.png \
  -o data/LB_EN/0.cz1

# Extract the image and character set txt for the 32nd "明朝" font
lucksystem font extract \
  -s data/Other/Font/明朝32 \
  -S data/Other/Font/info32 \
  -o data/Other/Font/明朝32_f.png \
  -O data/Other/Font/info32_f.txt

# Redraw the 32nd "明朝" font using a TTF (TrueType Font) file
lucksystem font edit \
  -s data/LB_EN/FONT/明朝32 \
  -S data/LB_EN/FONT/info32 \
  -f data/Other/Font/ARHei-400.ttf \
  -o data/Other/Font/明朝32 \
  -O data/Other/Font/info32 \
  -r

# Append the characters from "allchar.txt" to the end of the 32nd "明朝" font
lucksystem font edit \
  -s data/LB_EN/FONT/明朝32 \
  -S data/LB_EN/FONT/info32 \
  -f data/Other/Font/ARHei-400.ttf \
  -o data/Other/Font/明朝32 \
  -O data/Other/Font/info32 \
  -c data/Other/Font/allchar.txt \
  -a

# Replace the characters from "allchar.txt" at the 7106th character position of the 32nd "明朝" font
lucksystem font edit \
  -s data/LB_EN/FONT/明朝32 \
  -S data/LB_EN/FONT/info32 \
  -f data/Other/Font/ARHei-400.ttf \
  -o data/Other/Font/明朝32 \
  -O data/Other/Font/info32 \
  -c data/Other/Font/allchar.txt \
  -i 7105

```
