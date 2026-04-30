# level 17

Jatkossa voi kirjautua tällä salasanalla, ettei tarvi palata takaisin tasoon 16.

EReVavePLFHtFlFsjn3hyzMlvSuSAcRD

OHJE JA VINKKI: There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new & tarvittavia komentoja: cat, grep, ls, diff


## lunttilappu
- PIENI lunttilappu toimivana listana - mitä ollaan tähän menessä tehty
  - komentoja: whoami, ls ja jne & `$cat <file>`
  - jos on jotakin certificate avainta niin kokeilee ensin siinä Linux alla avautua seuraavaan tasoon ja jos ei tallentaa työasemaan-lokaaliselle ja siitä kirjautua sisään.
    - jos tallentaa lokaaliselle asemaan niin pitää purkkaa sitä ja määrittää oikeudet ensin ennen kuin ajaa seuraavan komento
    - malli komento:`$ssh -i .\private.key -p 2220 banditXX@bandit.labs.overthewire.org`

  - Bandith tallennettu tiedosto polku: `/etc/bandit_pass/banditXX`

  - etsiä tiedostoa jotakin `$find <SOMETHING>` & `$grep <something>`
    - luettava, tiedoston koon ja tiedosto on suoritettava: `$find -readable -size 1033c ! -executable`
    - etsiä tiedoston koon tavun väliltä: `$find -size +900c -size -1100c`
    - etsii tekstiä tiedostojen sisällöstä: `$grep <name> <file.txt>`

    - laskee .txt tiedoston tai tiedoston monta riviä sisällä on: `$wc -l <file.txt>`
    - lajitellaan tiedoston rivit aakkosjärjestyksessä `$sort data.txt | <grep/sort/uniq>`
    - enkoodata muutosta, on base32 ja -64: `$cat data.txt  | base64 -d`



## tarkistus ja testaukset

ihan pohjalla - saattin ratkaistettua, että komennosta käytettiin ($diff) että tarkistellaan molempien tiedoston eroa, mitä sisällä on muutetu

```
bandit17@bandit:~$ ls
passwords.new  passwords.old
bandit17@bandit:~$ whoami
bandit17
bandit17@bandit:~$ cat /etc/bandit_pass/bandit17
EReVavePLFHtFlFsjn3hyzMlvSuSAcRD
bandit17@bandit:~$ cat /etc/bandit_pass/bandit18
cat: /etc/bandit_pass/bandit18: Permission denied

bandit17@bandit:~$ diff passwords.old passwords.new
42c42
< KxOU4IzbXM8j8HeAWPAXTd1eC77mp1qV
---
> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

```

vastaus: x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

```

bandit17@bandit:~$ diff -u passwords.old passwords.new
--- passwords.old       2026-04-03 15:17:25.580141500 +0000
+++ passwords.new       2026-04-03 15:17:25.584253266 +0000
@@ -39,7 +39,7 @@
 RbNK7oUO0l50r8xbQFBMngXjP4PosdxI
 tA9dxz3SyAam2tiruMl4mB5CocAYTBP9
 I9IZK0CIWzeGdik4YD9bATBufq1fr9ON
-KxOU4IzbXM8j8HeAWPAXTd1eC77mp1qV
+x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
 SlNJjK3d31Mr3jtNETeMLcrZ7Yx2izqR
 c6xvvFLhpp6EsauHO5E5I2k5Jmslv49K
 4kHcF6SFl6wZQIQofBBlaFjvjL89X4UH
```

## diff komento

Se kertoo esimerkiksi:
- mitä rivejä puuttuu toisesta tiedostosta
- mitä rivejä on lisätty
- mitä rivejä on muutettu

- esim1. `$diff tiedosto1.txt tiedosto2.txt`
- esim2. `$diff -u tiedosto1.txt tiedosto2.txt`
- esim3. `$diff -r kansio1 kansio2` - vertaa kaikki tiedostot kansiossa 1 ja 2 ja ikään kuin läpikäynti koskien jos on alikansiot ja mitä tiedostoja alla onkaan.

```
bandit17@bandit:~$ diff --help
Usage: diff [OPTION]... FILES
Compare FILES line by line.

Mandatory arguments to long options are mandatory for short options too.
      --normal                  output a normal diff (the default)
  -q, --brief                   report only when files differ
  -s, --report-identical-files  report when two files are the same
  -c, -C NUM, --context[=NUM]   output NUM (default 3) lines of copied context
  -u, -U NUM, --unified[=NUM]   output NUM (default 3) lines of unified context
  -e, --ed                      output an ed script
  -n, --rcs                     output an RCS format diff
  -y, --side-by-side            output in two columns
  -W, --width=NUM               output at most NUM (default 130) print columns
      --left-column             output only the left column of common lines
      --suppress-common-lines   do not output common lines

  -p, --show-c-function         show which C function each change is in
  -F, --show-function-line=RE   show the most recent line matching RE
      --label LABEL             use LABEL instead of file name and timestamp
                                  (can be repeated)

  -t, --expand-tabs             expand tabs to spaces in output
  -T, --initial-tab             make tabs line up by prepending a tab
      --tabsize=NUM             tab stops every NUM (default 8) print columns
      --suppress-blank-empty    suppress space or tab before empty output lines
  -l, --paginate                pass output through 'pr' to paginate it

  -r, --recursive                 recursively compare any subdirectories found
      --no-dereference            don't follow symbolic links
  -N, --new-file                  treat absent files as empty
      --unidirectional-new-file   treat absent first files as empty
      --ignore-file-name-case     ignore case when comparing file names
      --no-ignore-file-name-case  consider case when comparing file names
  -x, --exclude=PAT               exclude files that match PAT
  -X, --exclude-from=FILE         exclude files that match any pattern in FILE
  -S, --starting-file=FILE        start with FILE when comparing directories
      --from-file=FILE1           compare FILE1 to all operands;
                                    FILE1 can be a directory
      --to-file=FILE2             compare all operands to FILE2;
                                    FILE2 can be a directory

  -i, --ignore-case               ignore case differences in file contents
  -E, --ignore-tab-expansion      ignore changes due to tab expansion
  -Z, --ignore-trailing-space     ignore white space at line end
  -b, --ignore-space-change       ignore changes in the amount of white space
  -w, --ignore-all-space          ignore all white space
  -B, --ignore-blank-lines        ignore changes where lines are all blank
  -I, --ignore-matching-lines=RE  ignore changes where all lines match RE

  -a, --text                      treat all files as text
      --strip-trailing-cr         strip trailing carriage return on input

  -D, --ifdef=NAME                output merged file with '#ifdef NAME' diffs
      --GTYPE-group-format=GFMT   format GTYPE input groups with GFMT
      --line-format=LFMT          format all input lines with LFMT
      --LTYPE-line-format=LFMT    format LTYPE input lines with LFMT
    These format options provide fine-grained control over the output
      of diff, generalizing -D/--ifdef.
    LTYPE is 'old', 'new', or 'unchanged'.  GTYPE is LTYPE or 'changed'.
    GFMT (only) may contain:
      %<  lines from FILE1
      %>  lines from FILE2
      %=  lines common to FILE1 and FILE2
      %[-][WIDTH][.[PREC]]{doxX}LETTER  printf-style spec for LETTER
        LETTERs are as follows for new group, lower case for old group:
          F  first line number
          L  last line number
          N  number of lines = L-F+1
          E  F-1
          M  L+1
      %(A=B?T:E)  if A equals B then T else E
    LFMT (only) may contain:
      %L  contents of line
      %l  contents of line, excluding any trailing newline
      %[-][WIDTH][.[PREC]]{doxX}n  printf-style spec for input line number
    Both GFMT and LFMT may contain:
      %%  %
      %c'C'  the single character C
      %c'\OOO'  the character with octal code OOO
      C    the character C (other characters represent themselves)

  -d, --minimal            try hard to find a smaller set of changes
      --horizon-lines=NUM  keep NUM lines of the common prefix and suffix
      --speed-large-files  assume large files and many scattered small changes
      --color[=WHEN]       color output; WHEN is 'never', 'always', or 'auto';
                             plain --color means --color='auto'
      --palette=PALETTE    the colors to use when --color is active; PALETTE is
                             a colon-separated list of terminfo capabilities

      --help               display this help and exit
  -v, --version            output version information and exit

FILES are 'FILE1 FILE2' or 'DIR1 DIR2' or 'DIR FILE' or 'FILE DIR'.
If --from-file or --to-file is given, there are no restrictions on FILE(s).
If a FILE is '-', read standard input.
Exit status is 0 if inputs are the same, 1 if different, 2 if trouble.

Report bugs to: bug-diffutils@gnu.org
GNU diffutils home page: <https://www.gnu.org/software/diffutils/>
General help using GNU software: <https://www.gnu.org/gethelp/>
```

