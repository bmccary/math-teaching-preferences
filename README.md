# Instructions

The preferences form is [this PDF](https://bmccary.github.io/utd-math-teaching-preferences/teaching-preferences.pdf).

1. The first time you're doing this:
    1. Open a terminal, go to a suitable directory.
    1. `git clone https://github.com/bmccary/utd-math-teaching-preferences.git`
    1. That will make a directory called `utd-math-teaching-preferences`.
    1. The code will be in `/whatever/path/you/chose/utd-math-teaching-preferences`. 
    1. We will refer that directory as `$MTP`.
    1. `scons` 
1. Send an email everyone:
    1. Attach `$MTP/tex/teaching-preferences.pdf`
    1. Tell them to fill-out the form, save it, and send it back to you.
    1. As the emails come in, put the PDFs in `$MTP`.
1. When you want an up-to-date spreadsheet of preferences:
    1. Open a terminal and change directory to `$MTP`.
    1. `scons`
    1. `preferences.csv` will be up-to-date.

# File Structure

| File Name | Input/Output/Constant | Description |
| --- | --- | --- |
| `$MTP/john-johnson.pdf` | Input | A PDF you received in your email and put in `$MTP`. |
| `$MTP/john-johnson.txt` | Output | A TXT file generated from `$MTP/john-johnson.pdf`. |
| `$MTP/jack-jackson.pdf` | Input | A PDF you received in your email and put in `$MTP`. |
| `$MTP/jack-jackson.txt` | Output | A TXT file generated from `$MTP/jack-jackson.pdf`. |
| `$MTP/config.json` | Input | A TXT file which contains configuration options. |
| `$MTP/LICENSE` | Constant | A TXT file which contains the license (MIT) for this program. |
| `$MTP/preferences.csv` | Output | A CSV file which contains the preferences from all of `$MTP/*.pdf`. |
| `$MTP/README.md` | Constant | The file you're currently reading. |
| `$MTP/SConstruct` | Constant | Part of the program. |
| `$MTP/SConscript` | Constant | Part of the program. |
| `$MTP/tex/config.sty` | Output | A TeX .sty file generated from the contents of `config.json`. |
| `$MTP/tex/courses-table.tex` | Output | A TeX file generated from the contents of `config.json`. |
| `$MTP/tex/Makefile` | Constant | Part of the program. |
| `$MTP/tex/teaching-preferences.pdf` | Output | The blank, unfilled PDF form you mail to everyone. |
| `$MTP/tex/teaching-preferences.tex` | Constant | The main TeX file. |
| `$MTP/tex/teaching-preferences-table.tex` | Output | A TeX file generated from the contents of `config.json`. |

