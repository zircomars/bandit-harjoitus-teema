# level 19

cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

OHJE JA VIHJE;;  To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.


```
PS C:\Users\zhao-> ssh bandit19@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit19@bandit.labs.overthewire.org's password:

      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!

If you find any problems, please report them to the #wargames channel on
discord or IRC.

--[ Playing the games ]--

  This machine might hold several wargames.
  If you are playing "somegame", then:

    * USERNAMES are somegame0, somegame1, ...
    * Most LEVELS are stored in /somegame/.
    * PASSWORDS for each level are stored in /etc/somegame_pass/.

  Write-access to homedirectories is disabled. It is advised to create a
  working directory with a hard-to-guess name in /tmp/.  You can use the
  command "mktemp -d" in order to generate a random and hard to guess
  directory in /tmp/.  Read-access to both /tmp/ is disabled and to /proc
  restricted so that users cannot snoop on eachother. Files and directories
  with easily guessable or short names will be periodically deleted! The /tmp
  directory is regularly wiped.
  Please play nice:

    * don't leave orphan processes running
    * don't leave exploit-files laying around
    * don't annoy other players
    * don't post passwords or spoilers
    * again, DONT POST SPOILERS!
      This includes writeups of your solution on your blog or website!

--[ Tips ]--

  This machine has a 64bit processor and many security-features enabled
  by default, although ASLR has been switched off.  The following
  compiler flags might be interesting:

    -m32                    compile for 32bit
    -fno-stack-protector    disable ProPolice
    -Wl,-z,norelro          disable relro

  In addition, the execstack tool can be used to flag the stack as
  executable on ELF binaries.

  Finally, network-access is limited for most levels by a local
  firewall.

--[ Tools ]--

 For your convenience we have installed a few useful tools which you can find
 in the following locations:

    * gef (https://github.com/hugsy/gef) in /opt/gef/
    * pwndbg (https://github.com/pwndbg/pwndbg) in /opt/pwndbg/
    * gdbinit (https://github.com/gdbinit/Gdbinit) in /opt/gdbinit/
    * pwntools (https://github.com/Gallopsled/pwntools)
    * radare2 (http://www.radare.org/)

--[ More information ]--

  For more information regarding individual wargames, visit
  http://www.overthewire.org/wargames/

  For support, questions or comments, contact us on discord or IRC.

  Enjoy your stay!

bandit19@bandit:~$ whoami
bandit19
bandit19@bandit:~$ ls
bandit20-do
bandit19@bandit:~$ cat bandit20-do
ELFp4�54
         (444``���DD�� (�����DDP�tdP PP,,Q�tdR�td//lib/ld-linux.so.2GNUٵp�J�9�(���s3�GNU  �K��-S '_IO_stdin_usedexit__libc_sta>�����nexecvprintflibc.so.6GLIBC_2.0GLIBC_2.34__gmon_start__4ii
    S�����/��������t�Ѓ[��5��%��%h������%������h������%
                                                      h����1�^����PTR���t/jjQV������P�����$���f�f�f�f�f�f�f��f�f�f�f�f�f�f���$�f�f�f�f�f�f��=t$���t���h�Ѓ���.��&��.��&��&�-���������t ���tU���Ph�҃��Ít&Í�&���=u���h�����Í�&Í�&��늍L$����q�U��Q���ȃ8 �@��P��������
    j�����P�=��phA��������M�ɍa��S��������.�[�Run a command as another user.
  Example: %s whoami
env/usr/bin/env(����� ���D`���p6����zR|
                                     ����2zR|
                                            ���� 0D���F
                                                       J
                                                        tx?␦;*2$"(T����aD
                                                                         IuAu|N
                                                                               A�C
                                                                                  �P4
␦���o|�                                                                              �
      �
b
���aHN[ jy�������CC: (Ubuntu 13.3.0-6ubuntu2~24.04.1) 13.3.0 ��
  crt1.o__abi_tag__wrap_maincrtstuff.cderegister_tm_clones__do_global_dtors_auxcompleted.0__do_global_dtors_aux_fini_array_entryframe_dummy__frame_dummy_init_array_entrybandit20.c__FRAME_END___DYNAMIC__GNU_EH_FRAME_HDR_GLOBAL_OFFSET_TABLE___libc_start_main@GLIBC_2.34__x86.get_pc_thunk.bxprintf@GLIBC_2.0_edata_fini__data_start__gmon_start__exit@GLIBC_2.0__dso_handle_IO_stdin_usedexecv@GLIBC_2.0_end_dl_relocate_static_pie_fp_hw__bss_start__TMC_END___init.symtab.strtab.shstrtab.interp.note.gnu.build-id.note.ABI-tag.gnu.hash.dynsym.dynstr.gnu.version.gnu.version_r.rel.dyn.rel.plt.init.text.fini.rodata.eh_frame_hdr.eh_frame.init_array.fini_array.dynamic.got.got.plt.data.bss.comment�#��$� D���o� N

                                                                      �
                                                                      pV||b^���o�k���o�0z     � B$$� �  P�ppw��� N�PP ,�|| �/�/��/��/�0�00-H0p        2��4bandit19@bandit:~
```


## setuid

Setuid (lyhenne sanoista _"set user ID"_) on Unix/Linux-järjestelmissä tiedosto-oikeuksiin liittyvä ominaisuus. Se tarkoittaa, että kun tietty ohjelma suoritetaan, se ei toimi suorittavan käyttäjän oikeuksilla, vaan tiedoston omistajan oikeuksilla.

Miten se toimii käytännössä?

Normaalisti:
- > ohjelma → toimii sen käyttäjän oikeuksilla, joka sen käynnistää

Setuid päällä:
- > ohjelma → toimii tiedoston omistajan oikeuksilla

Miksi sitä käytetään?
- mahdollistaa tavallisille käyttäjille rajatun pääsyn root-tason toimintoihin
- ilman että annetaan täydet sudo-oikeudet

Setuid voi olla vaarallinen:
- jos ohjelmassa on bugi → hyökkääjä voi saada root-oikeudet
- siksi setuid-ohjelmia pitää olla mahdollisimman vähän

Esim 1. Miltä se näyttää?

- Kun listaat tiedostoja: `$ls -l`
- Saatat nähdä jotain tällaista: `-rwsr-xr-x 1 root root 54256 passwd`

Huomaa:
- s käyttäjän execute-kohdassa (rws)
- → tämä tarkoittaa setuid

Teknisesti tämä tarkoittaa sitä, että ohjelman prosessille asetetaan niin sanottu effective user ID (eUID) ohjelman omistajan mukaan. Vaikka käyttäjä pysyy samana, järjestelmä tarkistaa oikeudet tämän eUID:n perusteella. Näin ohjelma voi tehdä asioita, joihin tavallisella käyttäjällä ei muuten olisi lupaa.

Esimerkki tästä on Linuxin `passwd`-komento. Tavallinen käyttäjä voi vaihtaa oman salasanansa tällä komennolla, vaikka salasanat tallennetaan tiedostoon `/etc/shadow`, johon on normaalisti pääsy vain root-käyttäjällä. Tämä toimii siksi, että `passwd`-ohjelma on asetettu setuid-tilaan ja sen omistaja on root. Kun ohjelma suoritetaan, se saa tarvittavat oikeudet muokata kyseistä tiedostoa, mutta käyttäjä ei saa näitä oikeuksia suoraan itselleen.

Setuidin tarkoitus ei ole estää käyttäjiä käyttämästä komentoja, vaan tarjota turvallinen tapa suorittaa rajattuja, korkeampia oikeuksia vaativia toimintoja ilman, että käyttäjälle annetaan laajoja pääkäyttäjäoikeuksia. Tämä eroaa esimerkiksi sudo-komennosta, jonka avulla käyttäjälle voidaan antaa lupa suorittaa lähes mitä tahansa komentoja toisena käyttäjänä, usein rootina.

Tietoturvan näkökulmasta setuid on hyödyllinen mutta riskialtis mekanismi. Jos setuid-oikeuksilla varustetussa ohjelmassa on ohjelmointivirhe tai haavoittuvuus, hyökkääjä voi mahdollisesti hyödyntää sitä saadakseen korkeammat oikeudet järjestelmässä. Tällaista tilannetta kutsutaan nimellä Privilege Escalation. Tämän vuoksi kyberturvallisuudessa noudatetaan periaatetta Principle of Least Privilege, jonka mukaan ohjelmille ja käyttäjille annetaan vain ne oikeudet, jotka ovat välttämättömiä toiminnan kannalta.

Eettisessä hakkeroinnissa, eli Ethical Hacking-toiminnassa, setuid-ohjelmat ovat usein tarkastelun kohteena. Turvallisuustestaajat analysoivat, onko tällaisissa ohjelmissa heikkouksia, joiden avulla oikeuksia voisi nostaa luvattomasti. Tällainen testaaminen tehdään kuitenkin aina luvan kanssa ja tarkoituksena on parantaa järjestelmien turvallisuutta, ei murtaa niitä.

Setuid on tiedoston oikeusbitin asetus, ei suojausmenetelmä eikä salaus.
Se ei “lukitse” tiedostoa tietylle käyttäjälle, vaan määrittää mitä oikeuksia ohjelma käyttää suoritettaessa.

Kun setuid on asetettu:
- kuka tahansa, jolla on oikeus ajaa tiedosto, voi käynnistää sen
- mutta ohjelma toimii tiedoston omistajan oikeuksilla (usein root)

> Jotenkin parempi esim ja kirjoitettu, että tajutaan.
> Normaalisti Linuxissa ohjelma toimii sen käyttäjän oikeuksilla, joka sen suorittaa. Tämä tarkoittaa, että käyttäjä voi tehdä vain niitä asioita, joihin hänellä itsellään on oikeus.
>
> Kun tarkastellaan tiedoston oikeuksia komennolla `ls -l`, voidaan nähdä esimerkiksi muoto `-rwsr-xr-x`. Tässä rwx tarkoittaa omistajan oikeuksia ja kirjain `s` tarkoittaa, että setuid on asetettu.
>
> Setuid tarkoittaa sitä, että kun ohjelma suoritetaan, se toimii tiedoston omistajan oikeuksilla, eikä suorittavan käyttäjän oikeuksilla. Usein tiedoston omistaja on root, jolloin ohjelma saa ajon ajaksi root-oikeudet.
>
> Tämä ei kuitenkaan tarkoita, että käyttäjä itse saisi root-oikeudet, vaan ainoastaan ohjelma käyttää niitä rajatusti. Root-käyttäjä voi silti ajaa kyseisen ohjelman normaalisti ilman rajoituksia.


lukemista: 
- https://www.linux.fi/wiki/Setuid
- https://www.lenovo.com/fi/fi/glossary/setuid/?srsltid=AfmBOopEUxNb9hE4Y-q9goHurqoVxpH5BsakwX0H_EyLIfMyt7CXwDgo

---

```
bandit19@bandit:~$ setuids.bt bandit20-do
ERROR: bpftrace currently only supports running as the root user.
bandit19@bandit:~$ setuids.bt --help
USAGE:
    bpftrace [options] filename
    bpftrace [options] - <stdin input>
    bpftrace [options] -e 'program'

OPTIONS:
    -B MODE        output buffering mode ('full', 'none')
    -f FORMAT      output format ('text', 'json')
    -o file        redirect bpftrace output to file
    -e 'program'   execute this program
    -h, --help     show this help message
    -I DIR         add the directory to the include search path
    --include FILE add an #include file before preprocessing
    -l [search]    list probes
    -p PID         enable USDT probes on PID
    -c 'CMD'       run CMD and enable USDT probes on resulting process
    --usdt-file-activation
                   activate usdt semaphores based on file path
    --unsafe       allow unsafe builtin functions
    -q             keep messages quiet
    --info         Print information about kernel BPF support
    -k             emit a warning when a bpf helper returns an error (except read functions)
    -kk            check all bpf helper functions
    -V, --version  bpftrace version
    --no-warnings  disable all warning messages

TROUBLESHOOTING OPTIONS:
    -v                      verbose messages
    -vv                     more verbose messages (max 2)
    -d                      (dry run) debug info
    -dd                     (dry run) verbose debug info
    --emit-elf FILE         (dry run) generate ELF file with bpf programs and write to FILE
    --emit-llvm FILE        write LLVM IR to FILE.original.ll and FILE.optimized.ll

ENVIRONMENT:
    BPFTRACE_BTF                      [default: none] BTF file
    BPFTRACE_CACHE_USER_SYMBOLS       [default: auto] enable user symbol cache
    BPFTRACE_CPP_DEMANGLE             [default: 1] enable C++ symbol demangling
    BPFTRACE_DEBUG_OUTPUT             [default: 0] enable bpftrace's internal debugging outputs
    BPFTRACE_KERNEL_BUILD             [default: /lib/modules/$(uname -r)] kernel build directory
    BPFTRACE_KERNEL_SOURCE            [default: /lib/modules/$(uname -r)] kernel headers directory
    BPFTRACE_LOG_SIZE                 [default: 1000000] log size in bytes
    BPFTRACE_MAX_BPF_PROGS            [default: 512] max number of generated BPF programs
    BPFTRACE_MAX_CAT_BYTES            [default: 10k] maximum bytes read by cat builtin
    BPFTRACE_MAX_MAP_KEYS             [default: 4096] max keys in a map
    BPFTRACE_MAX_PROBES               [default: 512] max number of probes
    BPFTRACE_MAX_STRLEN               [default: 64] bytes on BPF stack per str()
    BPFTRACE_MAX_TYPE_RES_ITERATIONS  [default: 0] number of levels of nested field accesses for tracepoint args
    BPFTRACE_PERF_RB_PAGES            [default: 64] pages per CPU to allocate for ring buffer
    BPFTRACE_STACK_MODE               [default: bpftrace] Output format for ustack and kstack builtins
    BPFTRACE_STR_TRUNC_TRAILER        [default: '..'] string truncation trailer
    BPFTRACE_VMLINUX                  [default: none] vmlinux path used for kernel symbol resolution

EXAMPLES:
bpftrace -l '*sleep*'
    list probes containing "sleep"
bpftrace -e 'kprobe:do_nanosleep { printf("PID %d sleeping...\n", pid); }'
    trace processes calling sleep
bpftrace -e 'tracepoint:raw_syscalls:sys_enter { @[comm] = count(); }'
    count syscalls by process name
```


## ratkaisu muoto

cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

siis harjoituksen ideana saada oikeus seuraavan tasoon, että käyttäen **setuid** asetusta mitä tälle tiedostolle on asetettu.

- nyt nähdään tässä `s` on se setuid merkintä ja oikeus määritetty

```
bandit19@bandit:~$ ls
bandit20-do
bandit19@bandit:~$ ls -l
total 16

bandit19@bandit:~$ id
uid=11019(bandit19) gid=11019(bandit19) groups=11019(bandit19)

bandit19@bandit:~$ ./bandit20-do id
uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20) groups=11019(bandit19)
-rwsr-x--- 1 bandit20 bandit19 14888 Apr  3 15:17 bandit20-do
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

```
