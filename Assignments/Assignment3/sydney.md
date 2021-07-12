# Microcorruption - Sydney
Author: [Kuldeep Singh Chouhan](https://github.com/kuldeep-singh-chouhan)

Challenge Page: [Microcorruption](https://microcorruption.com/)

## Walkthrough
As we can see, there are a lot of functions in the Disassembly section, but we will start looking at the `main` function.
```4438 <main>
4438:  3150 9cff      add	#0xff9c, sp
443c:  3f40 b444      mov	#0x44b4 "Enter the password to continue.", r15
4440:  b012 6645      call	#0x4566 <puts>
4444:  0f41           mov	sp, r15
4446:  b012 8044      call	#0x4480 <get_password>
444a:  0f41           mov	sp, r15
444c:  b012 8a44      call	#0x448a <check_password>
4450:  0f93           tst	r15
4452:  0520           jnz	#0x445e <main+0x26>
4454:  3f40 d444      mov	#0x44d4 "Invalid password; try again.", r15
4458:  b012 6645      call	#0x4566 <puts>
445c:  093c           jmp	#0x4470 <main+0x38>
445e:  3f40 f144      mov	#0x44f1 "Access Granted!", r15
4462:  b012 6645      call	#0x4566 <puts>
4466:  3012 7f00      push	#0x7f
446a:  b012 0245      call	#0x4502 <INT>
446e:  2153           incd	sp
4470:  0f43           clr	r15
4472:  3150 6400      add	#0x64, sp
```
Here, `check_password` seems interesting. Let's go to the `check_password` section.
```448a <check_password>
448a:  bf90 403c 0000 cmp	#0x3c40, 0x0(r15)
4490:  0d20           jnz	$+0x1c
4492:  bf90 7443 0200 cmp	#0x4374, 0x2(r15)
4498:  0920           jnz	$+0x14
449a:  bf90 7a6d 0400 cmp	#0x6d7a, 0x4(r15)
44a0:  0520           jne	#0x44ac <check_password+0x22>
44a2:  1e43           mov	#0x1, r14
44a4:  bf90 5526 0600 cmp	#0x2655, 0x6(r15)
44aa:  0124           jeq	#0x44ae <check_password+0x24>
44ac:  0e43           clr	r14
44ae:  0f4e           mov	r14, r15
44b0:  3041           ret
```
It can be observed that `check_password` is comparing the password we enter with particular characters present in the memory dump.
```
448a:  bf90 403c 0000 cmp	#0x3c40, 0x0(r15)
4492:  bf90 7443 0200 cmp	#0x4374, 0x2(r15)
449a:  bf90 7a6d 0400 cmp	#0x6d7a, 0x4(r15)
44a4:  bf90 5526 0600 cmp	#0x2655, 0x6(r15)
```
Looking at memory dump we can find those particular characters.
```
4480:   3e40 6400 b012 5645 3041 bf90 403c 0000   >@d...VE0A..@<..
4490:   0d20 bf90 7443 0200 0920 bf90 7a6d 0400   . ..tC... ..zm..
44a0:   0520 1e43 bf90 5526 0600 0124 0e43 0f4e   . .C..U&...$.C.N
```
So, the potential password is made of `@<` `tC` `zm` `U&` .


This is it !!! We got our password.


## Password
`@<tCzmU&` in ASCII text or `403c74437a6d5526` in HEX.
