# Google Photos JSON Matcher
## Description
This repository contains Python scripts that matches image files (jpg, jpeg, png) downloaded from Google Photos with their associated JSON files. The scripts uses a CSV file generated from ExifTool to find the matching JSON files.


## Installation

1. Clone the repository: `git clone https://github.com/redninja2/Google-Photos-JSON-Extractor.git`
2. Install required libraries: `pip install pandas`

## Usage

1. Go to section Evaluate Results first, so you can get a "before" vs "after" comparison. 
2. Generate a CSV file using ExifTool with the following command (replace directory with your directory): `exiftool -common -r -csv "/mnt/e/takeout-20230312T190809Z-001/Takeout/Google Photos/" > out.csv` 
3. Run the script: `python json_matcher.py`
4. Run the script: `python metadata_loader.py`
5. Run the command: `find /mnt/e/takeout-20230312T190809Z-001/Takeout/Google\ Photos/ -type f \( -iname '*.jpg' -o -iname '*.jpeg' -o -iname '*.png' \) -print0 | xargs -0 exiftool -csv=out_with_metadata.csv -overwrite_original -tagsfromfile @ -DateTimeOrigininal -csv-delimiter ,`
Note: You may receive a lot of warnings such as "Warning: No writeable tags set from ...". This is (probably) normal. I'm not an exiftool pro... There's probably a better way to do this, but this command worked for me. 

The script will output a CSV file with the matched image and JSON files.

## Evaluate Results

1. Generate a CSV file, once again: `exiftool -common -r -csv "/mnt/e/takeout-20230312T190809Z-001/Takeout/Google Photos/" > results.csv`
2. Run the script: `python evaluate_results.py`
3. Make changes to the scripts as necessary and repeat the steps. 

## Misc. Commands
- Apply metadata from csv to files in single directory: `exiftool -csv=final_v3.csv -tagsfromfile @ -DateTimeOriginal -overwrite_original /mnt/e/takeout-20230312T190809Z-001/Takeout/Google\ Photos/Photos\ from\ 2022/`

# JSON file matching logic
###### Description generated using ChatGPT- may not be 100% accurate. 
The script uses the following naming conventions to match a JSON file to an image file:

If the image file name ends with parentheses and a number (e.g., "IMG_1667(1).jpg"), the corresponding JSON file name should be "IMG_1667.jpg(1).json". The number in parentheses should be removed from the image file name and included in the JSON file name, and the extension should be changed to ".json".

If the image file name ends with "-edited" (e.g., "Snapchat-1068254512-edited.jpg"), the corresponding JSON file name should be "Snapchat-1068254512.jpg.json". The "-edited" portion of the image file name should be removed, and the extension should be changed to ".json".

If the image file name does not match either of the above conventions, the corresponding JSON file name should simply be the image file name with ".json" appended to the end.

The script searches for JSON files in the same directory as the image files, using the naming conventions above to construct the file name. It first searches for files that match the full file name of the image, then for files that match the base name (without the extension), and finally for files that match the base name with parentheses and a number.
If no matching JSON file is found, the script will move on to the next image file.

## License

This project is licensed under the [MIT License](LICENSE).




