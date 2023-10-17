

## When using this tool for translation work, please take note of the following:

- It's best to extract OPCODE from the executable file. If extraction is not possible, the following steps need to be taken:
  - Decompiling the script to obtain a script file consisting entirely of uint16 values, identifying operations that likely contain strings (typically very long and with continuous, large values).
  - Using the `opcode_dict` feature in the Plugin, mapping it to a plugin function and attempting to parse, extracting strings and translating them.
  - At this point, strings cannot be overly long, and they should be padded with **full-width spaces** (for half-width strings, use half-width spaces) to match the original length; otherwise, it may cause issues with the script after importing.
  - If you wish to modify any string arbitrarily, you need to identify the jump operations in the plugin files under [base](data/base), such as `IFN`, `FARCALL`, `JUMP`, and use `read_jump` to accurately parse the type and value of the jump.

- If you already have a complete set of OPCODE:
  - Decompile the script to obtain a script file consisting entirely of uint16 values, identifying operations that likely contain strings (typically very long and with continuous, large values).
  - Attempt to parse using the Plugin.
  - At this point, strings cannot be overly long, and they should be padded with **full-width spaces**(for half-width strings, use half-width spaces) to match the original length; otherwise, it may cause issues with the script after importing.
If you wish to modify any string arbitrarily, you need to parse the jump operations in the plugin files under [base](data/base), such as `IFN`, `FARCALL`, `JUMP`, and use `read_jump` to accurately parse the type and value of the jump.
Importing and exporting must use the same original SCRIPT.PAK, OPCODE, and plugins. Any modifications to the plugins require re-decompilation before importing is possible.

Recommendation: Write additional tools to extract the text to be translated from the decompiled script, use the tools to replace the translated text in the decompiled script after translation, and then import it to prevent unintentional modifications to game values.

## Using the "help" command provides detailed command information

## Example
```shell
# Decompile SCRIPT.PAK
lucksystem script decompile \
  -s D:/Game/LOOPERS/files/SCRIPT.PAK \
  -c UTF-8 \
  -O data/LOOPERS.txt \
  -p data/LOOPERS.py \
  -o D:/Game/LOOPERS/files/Export

# lucksystem script decompile -s D:/Game/LOOPERS/LOOPERS/files/src/SCRIPT.PAK -c UTF-8 -O data/LOOPERS.txt -p data/LOOPERS.py -o D:/Game/LOOPERS/LOOPERS/files/Export

# Import the modified decompiled script into SCRIPT.PAK
lucksystem script import \
  -s D:/Game/LOOPERS/files/SCRIPT.PAK \
  -c UTF-8 \
  -O data/LOOPERS.txt \
  -p data/LOOPERS.py \
  -i D:/Game/LOOPERS/files/Export \
  -o D:/Game/LOOPERS/files/Import/SCRIPT.PAK

# lucksystem script import -s D:/Game/LOOPERS/LOOPERS/files/src/SCRIPT.PAK -c UTF-8 -O data/LOOPERS.txt -p data/LOOPERS.py -i D:/Game/LOOPERS/LOOPERS/files/Export -o D:/Game/LOOPERS/LOOPERS/files/Import/SCRIPT.PAK


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
