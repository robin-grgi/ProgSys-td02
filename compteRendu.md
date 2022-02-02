# TD n° 2 - Bibliothèques Statiques et Dynamiques sous Unix

## Exercice 1
```bubble.c```, ```insertion.c```, ```merge.c``` etc. sont les différentes imlplémentations des méthodes de tri mentionnées dans le sujet du TD. Ces implémentations qont utilisées via la librairie ```sort.h``` et le choix de la méthode de tri pour l'exécution du main a lieu dans le Makefile.

## Exercice 2
Les librairies linux-vsdo, libc et ld-linux-x86-64 sont utilisées
```
[robin@robin-recoil3gtx15 td02]$ ldd tri_bubble-basicExe.exe 
        linux-vdso.so.1 (0x00007ffe6bf47000)
        libc.so.6 => /usr/lib/libc.so.6 (0x00007f0310457000)
        /lib64/ld-linux-x86-64.so.2 => /usr/lib64/ld-linux-x86-64.so.2 (0x00007f0310646000)
```

## Exercice 3

```
[robin@robin-recoil3gtx15 td02]$ make test
*** Début des tests des programmes générés
Lancement du programme: tri_bubble-basicExe.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 1710
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_bubble-staticExe.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 1594
Sorted array:335 383 386 421 492 649 777 793 886 915 

*** Fin des tests
[robin@robin-recoil3gtx15 td02]$ ldd tri_bubble-staticExe.exe 
        n'est pas un exécutable dynamique


-rwxr-xr-x 1 robin robin  23800  2 févr. 10:03 tri_bubble-basicExe.exe
-rwxr-xr-x 1 robin robin 809440  2 févr. 10:34 tri_bubble-staticExe.exe
```

Les résultats sont bien identiques et l'exécutable static est plus gros car toutes les librairies y sont directement intégrées sans utiliser de simples références.
## Exercice 4:

```
[robin@robin-recoil3gtx15 td02]$ make test
*** Début des tests des programmes générés
Lancement du programme: tri_bubble-basicExe.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 1319
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_bubble-staticExe.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 1355
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_bubble-staticLib.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 1403
Sorted array:335 383 386 421 492 649 777 793 886 915

[robin@robin-recoil3gtx15 td02]$ ls -l
total 932
...
-rwxr-xr-x 1 robin robin  23800  2 févr. 10:03 tri_bubble-basicExe.exe
-rwxr-xr-x 1 robin robin 809440  2 févr. 10:34 tri_bubble-staticExe.exe
-rwxr-xr-x 1 robin robin  22856  2 févr. 11:17 tri_bubble-staticLib.exe
...

[robin@robin-recoil3gtx15 td02]$ nm tri_bubble-staticLib.exe 
000000000000039c r __abi_tag
                 U atoi@GLIBC_2.2.5
000000000000407c B __bss_start
                 U clock_gettime@GLIBC_2.17
000000000000407c b completed.0
                 w __cxa_finalize@GLIBC_2.2.5
0000000000004068 D __data_start
0000000000004068 W data_start
0000000000001100 t deregister_tm_clones
0000000000001170 t __do_global_dtors_aux
0000000000003de8 d __do_global_dtors_aux_fini_array_entry
0000000000004070 D __dso_handle
0000000000003df0 d _DYNAMIC
000000000000407c D _edata
0000000000004088 B _end
                 U exit@GLIBC_2.2.5
00000000000018c8 T _fini
                 U fprintf@GLIBC_2.2.5
00000000000011c0 t frame_dummy
0000000000003de0 d __frame_dummy_init_array_entry
0000000000002314 r __FRAME_END__
                 U fwrite@GLIBC_2.2.5
0000000000004000 d _GLOBAL_OFFSET_TABLE_
                 w __gmon_start__
00000000000020e0 r __GNU_EH_FRAME_HDR
0000000000001000 t _init
0000000000003de8 d __init_array_end
0000000000003de0 d __init_array_start
0000000000002000 R _IO_stdin_used
                 w _ITM_deregisterTMCloneTable
                 w _ITM_registerTMCloneTable
00000000000018c0 T __libc_csu_fini
0000000000001850 T __libc_csu_init
                 U __libc_start_main@GLIBC_2.2.5
00000000000011c9 T main
                 U printf@GLIBC_2.2.5
0000000000001718 T print_list
                 U putchar@GLIBC_2.2.5
                 U rand@GLIBC_2.2.5
0000000000001130 t register_tm_clones
00000000000013c8 t Scan_Args
0000000000004078 d Size_Array
0000000000001775 T sort
                 U srand@GLIBC_2.2.5
                 U __stack_chk_fail@GLIBC_2.4
00000000000010d0 T _start
                 U stderr@GLIBC_2.2.5
00000000000016ae T timer_end
0000000000001668 T timer_start
00000000000014f9 t time_subtract
0000000000004080 D __TMC_END__
000000000000149f t Usage
0000000000004080 b Verbose
```

Les résultats sont toujours identiques, l'executable static avec les références aux librairies est le moins volumineux car il ne référence pas les librairies inutilisées comme foo et bar qui n'apparaissent pas lors de l'exécution de la commande 

## Exercice 5
Il faut utiliser la commande gcc avec les options -shared -Wl -soname et -o avec la syntaxe makefile suivante :

```$(CC) -shared -Wl,-soname,$@ -o $@ $^```

## Exercice 6

```
[robin@robin-recoil3gtx15 td02]$ export LD_LIBRARY_PATH=.:~/lib

[robin@robin-recoil3gtx15 td02]$ make test
*** Début des tests des programmes générés
Lancement du programme: tri_bubble-basicExe.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 1796
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_insertion-basicExe.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 3273
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_merge-basicExe.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 3093
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_quick-basicExe.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 1849
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_bubble-staticExe.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 3106
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_insertion-staticExe.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 1678
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_merge-staticExe.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 25117
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_quick-staticExe.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 1915
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_bubble-staticLib.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 2838
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_insertion-staticLib.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 1125
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_merge-staticLib.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 2965
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_quick-staticLib.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 3998
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_bubble-dynamicLib.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 2133
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_insertion-dynamicLib.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 4775
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_merge-dynamicLib.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 3679
Sorted array:335 383 386 421 492 649 777 793 886 915 

Lancement du programme: tri_quick-dynamicLib.exe
Array to sort:383 886 777 915 793 335 386 492 649 421 
Time taken for sorting (nanoseconds): 1969
Sorted array:335 383 386 421 492 649 777 793 886 915 
```
