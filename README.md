# Instructions

1. The first time you're doing this:
    1. Open a terminal, go to a suitable directory.
    1. `git clone https://github.com/bmccary/math-teaching-preferences.git`
    1. That will make a directory called `math-teaching-preferences.
    1. The code will be in `/whatever/path/you/chose/math-teaching-preferences`. We will refer that directory as `$MTP`.
1. Subsequent times you're doing this:
    1. Open a terminal and change directory to `$MTP`.
    1. `git pull`
1. Send an email everyone:
    1. Attach `$MTP/tex/teaching-preferences.pdf`
    1. Tell them to fill-out the form, save it, and send it back to you.
    1. As the emails come in, put the PDFs in `$MTP`.
1. When you want an up-to-date spreadsheet of preferences:
    1. Open a terminal and change directory to `$MTP`.
    1. `scons`

