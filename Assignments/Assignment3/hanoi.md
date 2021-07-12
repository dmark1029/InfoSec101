# Microcorruption - Hanoi
Author: [Kuldeep Singh Chouhan](https://github.com/kuldeep-singh-chouhan)

Challenge Page: [Microcorruption](https://microcorruption.com/)

## Walkthrough
As we can see, there are a lot of functions in the Disassembly section, but we will start looking at the `main` function.
```4438 <main>
4438:  b012 2045      call	#0x4520 <login>
443c:  0f43           clr	r15
```
`main` directs to `login`
```4520 <login>
4520:  c243 1024      mov.b	#0x0, &0x2410
4524:  3f40 7e44      mov	#0x447e "Enter the password to continue.", r15
4528:  b012 de45      call	#0x45de <puts>
452c:  3f40 9e44      mov	#0x449e "Remember: passwords are between 8 and 16 characters.", r15
4530:  b012 de45      call	#0x45de <puts>
4534:  3e40 1c00      mov	#0x1c, r14
4538:  3f40 0024      mov	#0x2400, r15
453c:  b012 ce45      call	#0x45ce <getsn>
4540:  3f40 0024      mov	#0x2400, r15
4544:  b012 5444      call	#0x4454 <test_password_valid>
4548:  0f93           tst	r15
454a:  0324           jz	$+0x8
454c:  f240 0e00 1024 mov.b	#0xe, &0x2410
4552:  3f40 d344      mov	#0x44d3 "Testing if password is valid.", r15
4556:  b012 de45      call	#0x45de <puts>
455a:  f290 1900 1024 cmp.b	#0x19, &0x2410
4560:  0720           jne	#0x4570 <login+0x50>
4562:  3f40 f144      mov	#0x44f1 "Access granted.", r15
4566:  b012 de45      call	#0x45de <puts>
456a:  b012 4844      call	#0x4448 <unlock_door>
456e:  3041           ret
4570:  3f40 0145      mov	#0x4501 "That password is not correct.", r15
4574:  b012 de45      call	#0x45de <puts>
4578:  3041           ret
```
Though, there are a lot of functions being called in `login`, they do not provide any check for password. There is a `cmp.b` comparing value `#0x19` with some byte in memory dump at address `0x2410`. This might be the security check.
Initially we can't find any such place in the dump. Let's give a test password `passwordpass` in the console (as the password is of length 8-16). Now, the memorary can be accessed.
```
2400:   7061 7373 776f 7264   password
2408:   7061 7373 0000 0000   pass....
2410:   0000 0000 0000 0000   ........
```
`2400` and `2408` row stores the 16 digit password and `0x2410` stores the digit which comes after the 16th digit of password. So, if we bruteforce the next digit in the password itself door could be unlocked.
As ASCII form of `0x19` is unreadable, we can put password in HEX form. Let the 16 digits be letter `i` and its HEX form is `69` so the password becomes `69696969696969696969696969696969`. But we also need 17th character, whose HEX would be `19`. So final passsword will be `6969696969696969696969696969696919`.

This is it !!! We got our password.


## Password
`6969696969696969696969696969696919` in HEX.
