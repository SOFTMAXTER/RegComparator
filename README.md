# Python Script for Comparing Windows Registry Files (`.reg`)

This script compares two Windows Registry export files (`.reg`) and identifies differences, focusing on added keys, added values within existing keys, and modified values within existing keys. It allows for the exclusion of specific registry paths to filter out noise from frequently changing or irrelevant keys. The comparison of exclusion patterns is case-insensitive. [cite: 2]

The script's primary goal is to highlight what has been *added* or *changed* between two registry snapshots, rather than providing a list of deleted keys in the main report. [cite: 21]

## Features

* **Parses `.reg` files**: Reads and interprets the standard Windows Registry export format. [cite: 6, 7, 8, 9, 10, 11, 12]
* **Handles Multiple Encodings**: Attempts to read `.reg` files using `utf-16` and `utf-8` encodings. [cite: 6, 7, 8]
* **Compares Registry Data**: Identifies:
    * Keys added in the modified file. [cite: 14, 20]
    * Keys present in both but with modified values. [cite: 15, 22]
    * Values added to existing keys. [cite: 16, 23]
    * Values deleted from existing keys. [cite: 16, 24]
    * Values that have changed data. [cite: 16, 17, 25]
* **Exclusion Patterns**: Allows users to specify a list of registry key path prefixes (`EXCLUSION_PATTERNS`) to ignore during comparison. [cite: 3, 4, 5] This is useful for excluding volatile keys related to timestamps, MRU lists, system updates, etc.
* **Filtered Reporting**: The generated report focuses on *added* and *modified* keys/values after applying exclusions. [cite: 18, 19, 20, 21, 22, 23, 24, 25, 26]
* **Console Output**: Prints a summary of differences to the console.
* **File Report**: Saves a detailed report to a timestamped `.txt` file in the parent directory of the script (or the current working directory as a fallback if the script's path cannot be determined). [cite: 30, 32, 33, 34, 35, 36]

## Requirements

* Python 3.x
* Standard Python libraries: `re`, `sys`, `codecs`, `os`, `datetime`. [cite: 1] No external packages need to be installed.

## How to Use

1.  **Save the script**: Save the code as `comparar_reg.py` (or any other `.py` name).
2.  **Configure Exclusions (Optional)**:
    * Open `comparar_reg.py` in a text editor.
    * Modify the `EXCLUSION_PATTERNS` list near the beginning of the script. [cite: 3] Add or remove registry key path prefixes (as raw strings, e.g., `r"HKEY_CURRENT_USER\Software\MyApp\Temp"`).
3.  **Run the script**:
    * **Using command-line arguments (recommended)**:
        ```bash
        python comparar_reg.py <path_to_original.reg> <path_to_modified.reg>
        ```
        Example:
        ```bash
        python comparar_reg.py "C:\Users\Me\Desktop\reg_backup_original.reg" "C:\Users\Me\Desktop\reg_backup_modified.reg"
        ```
    * **Interactively**: If you run the script without command-line arguments, it will prompt you to enter the paths for the original and modified `.reg` files. [cite: 28]
        ```bash
        python comparar_reg.py
        ```

4.  **Review Output**:
    * **Console**: A summary of differences will be printed directly to your console.
    * **Report File**: A detailed text report named `registry_comparison_report_YYYYMMDD_HHMMSS.txt` will be saved. [cite: 33]
        * The script attempts to save this file in the **parent directory** of where the script itself is located. [cite: 30]
        * If the script's path cannot be determined (e.g., when run in certain bundled environments or an interactive interpreter without `__file__` defined), it will fall back to saving the report in the **current working directory**. [cite: 32]
        * Make sure the script has write permissions for the target directory. [cite: 36]

## Understanding the Report

The report is structured to show:

* **Comparison Summary**: Names of the files being compared and a list of active exclusion patterns. [cite: 19]
* **Added Keys**: Lists all new registry keys found in the modified file that were not in the original file (and are not excluded). [cite: 20]
    * For each added key, its values (if any) are also listed.
* **Modified Keys**: Lists keys that exist in both files but have differences in their values (and are not excluded). [cite: 22] For each modified key, it details:
    * `[+] Values Added`: Values that exist only in the modified version of the key. [cite: 23]
    * `[-] Values Deleted`: Values that existed in the original version of the key but are missing in the modified one. [cite: 24]
    * `[*] Values Modified`: Values that exist in both but have different data. [cite: 25] Both original and new data are shown.

If no differences (additions or modifications) are found after applying exclusions, a message indicating this will be displayed. [cite: 26, 27]

**Note on Deleted Keys**: While the script internally identifies keys present in the original file but absent in the modified one, these "deleted keys" are intentionally not included in the detailed report output to maintain focus on additions and modifications. [cite: 14, 21] Values deleted *within* a common (modified) key are reported. [cite: 16, 24]

## Contributing

Feel free to fork this repository, make improvements, and submit pull requests. If you encounter any bugs or have suggestions, please open an issue.

## License

This script is provided as-is. You are free to use, modify, and distribute it. Consider attributing the original source if you find it useful. (You might want to add a specific open-source license like MIT or Apache 2.0 here).