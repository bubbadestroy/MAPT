# MAPT

PDF of Mobile App Penetration Testing - sourced selflearning.io/

Tools Used:

## Firefox: Enable Toolbars
alt -v - Toolbars - Menu Bar & Bookmark Toolbar

Addon: note, too many addons can/will destroy system resources quickly, especially with multiple page tabs *[1]

https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/

https://addons.mozilla.org/en-US/firefox/addon/fireshot/

convert entire current source page to pdf = ctrl + shift + Y


## other pdf tools

https://www.astro.caltech.edu/observatories/coo/solicit/mergePDF.html

Bookmarked to toolbar:

https://pdfmyurl.com



## See Also: nix command line 

https://askubuntu.com/questions/942727/convert-website-to-pdf-recursively


Save a list of Web pages as PDF file

    First install wkhtmltopdf conversion tool (this tool requires desktop environment; source):

    sudo apt install wkhtmltopdf 

    Then create a file that contains a list of URLs of multiple target web pages (each on new line). Let's call this file url-list.txt and let's place it in ~/Downloads/PDF/. For example its content could be:

    https://askubuntu.com/users/721082/tarek
    https://askubuntu.com/users/566421/pa4080

    And then run the next command, that will generate a PDF file for each site URL, located into the directory where the command is executed:

    while read i; do wkhtmltopdf "$i" "$(echo "$i" | sed -e 's/https\?:\/\///' -e 's/\//-/g' ).pdf"; done < ~/Downloads/PDF/url-list.txt

    The result of this command - executed within the directory ~/Downloads/PDF/ - is:

    ~/Downloads/PDF/$ ls -1 *.pdf
    askubuntu.com-users-566421-pa4080.pdf
    askubuntu.com-users-721082-tarek.pdf

    Merge the output files by the next command, executed in the above directory (source):

    gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite -dPDFSETTINGS=/prepress -sOutputFile=merged-output.pdf $(ls -1 *.pdf)

    The result is:

    ~/Downloads/PDF/$ ls -1 *.pdf
    askubuntu.com-users-566421-pa4080.pdf
    askubuntu.com-users-721082-tarek.pdf
    merged-output.pdf

Save an entire Website as PDF file

    First we must create a file (url-list.txt) that contains URL map of the site. Run these commands (source):

    TARGET_SITE="https://www.yahoo.com/"
    wget --spider --force-html -r -l2 "$TARGET_SITE" 2>&1 | grep '^--' | awk '{ print $3 }' | grep -v '\.\(css\|js\|png\|gif\|jpg\)$' > url-list.txt

    Then we need go through the steps from the above section.

Create a script that will Save an entire Website as PDF file (recursively)

    To automate the process we can bring all together in a script file.

    Create an executable file, called site-to-pdf.sh:

    mkdir -p ~/Downloads/PDF/
    touch ~/Downloads/PDF/site-to-pdf.sh
    chmod +x ~/Downloads/PDF/site-to-pdf.sh
    nano ~/Downloads/PDF/site-to-pdf.sh

    The script content is:

    #!/bin/sh
    TARGET_SITE="$1"
    wget --spider --force-html -r -l2 "$TARGET_SITE" 2>&1 | grep '^--' | awk '{ print $3 }' | grep -v '\.\(css\|js\|png\|gif\|jpg\|txt\)$' > url-list.txt
    while read i; do wkhtmltopdf "$i" "$(echo "$i" | sed -e 's/https\?:\/\///' -e 's/\//-/g' ).pdf"; done < url-list.txt
    gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite -dPDFSETTINGS=/prepress -sOutputFile=merged-output.pdf $(ls -1 *.pdf)

    Copy the above content and in nano use: Shift+Insert for paste; Ctrl+O and Enter for save; Ctrl+X for exit.

    Usage:

    enter image description here

The answer to the original question:
Convert multiple PHP files to one PDF (recursively)

    First install the package enscript, which is a 'regular file to pdf' conversion tool:

    sudo apt update && sudo apt install enscript

    Then run the next command, that will generate file called output.pdf, located into directory where the command is executed, which will contains the content of all php files within /path/to/folder/ and its sub-directories:

    find /path/to/folder/ -type f -name '*.php' -exec printf "\n\n{}\n\n" \; -exec cat "{}" \; | enscript -o - | ps2pdf - output.pdf

    Example, from my system, that generated this file:

    find /var/www/wordpress/ -type f -name '*.php' -exec printf "\n\n{}\n\n" \; -exec cat "{}" \; | enscript -o - | ps2pdf - output.pdf

shareimprove this answer
edited Oct 30 '18 at 18:19
answered Aug 3 '17 at 17:48
pa4080
21.3k88 gold badges5252 silver badges100


*[1] 
## on that note, tabs in general are a security vulerabililty. 
## As are addons. Extensions. Mods. Permissions. Certificates. Forks. Spoons. Sporks. Especially Sporks.
## Anything not stock and up to date, including rooting, mods, addons, extensions, non-oem software... is a potential vulnerability. That includes third party anti-virus and anti-malware. CC-Cleaner for example.. 
## tldr; beyond the scope of this readme. 
