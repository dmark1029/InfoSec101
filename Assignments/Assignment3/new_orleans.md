# Microcorruption - New Orleans

Author: [Kuldeep Singh Chouhan](https://github.com/kuldeep-singh-chouhan)

Challenge Page: [Microcorruption](https://microcorruption.com/)

## Walkthrough
As we can see, there are a lot of functions in the Disassembly section, but we will start looking at the `main` function.
```4438 <main>
4438:  3150 9cff      add	#0xff9c, sp
443c:  b012 7e44      call	#0x447e <create_password>
4440:  3f40 e444      mov	#0x44e4 "Enter the password to continue", r15
4444:  b012 9445      call	#0x4594 <puts>
4448:  0f41           mov	sp, r15
444a:  b012 b244      call	#0x44b2 <get_password>
444e:  0f41           mov	sp, r15
4450:  b012 bc44      call	#0x44bc <check_password>
4454:  0f93           tst	r15
4456:  0520           jnz	#0x4462 <main+0x2a>
4458:  3f40 0345      mov	#0x4503 "Invalid password; try again.", r15
445c:  b012 9445      call	#0x4594 <puts>
4460:  063c           jmp	#0x446e <main+0x36>
4462:  3f40 2045      mov	#0x4520 "Access Granted!", r15
4466:  b012 9445      call	#0x4594 <puts>
446a:  b012 d644      call	#0x44d6 <unlock_door>
446e:  0f43           clr	r15
4470:  3150 6400      add	#0x64, sp
```
Here, `create_password` and `check_password` seems interesting. Let's go to the `create_password` section.
```447e <create_password>
447e:  3f40 0024      mov	#0x2400, r15
4482:  ff40 4300 0000 mov.b	#0x43, 0x0(r15)
4488:  ff40 2400 0100 mov.b	#0x24, 0x1(r15)
448e:  ff40 6100 0200 mov.b	#0x61, 0x2(r15)
4494:  ff40 5100 0300 mov.b	#0x51, 0x3(r15)
449a:  ff40 6200 0400 mov.b	#0x62, 0x4(r15)
44a0:  ff40 7900 0500 mov.b	#0x79, 0x5(r15)
44a6:  ff40 6800 0600 mov.b	#0x68, 0x6(r15)
44ac:  cf43 0700      mov.b	#0x0, 0x7(r15)
44b0:  3041           ret
```
It can be observed that `create_password` is moving some bytes using `mov.b` at addresses starting from `0x2400`. Now, take a look at `check_password`.
```44bc <check_password>
44bc:  0e43           clr	r14
44be:  0d4f           mov	r15, r13
44c0:  0d5e           add	r14, r13
44c2:  ee9d 0024      cmp.b	@r13, 0x2400(r14)
44c6:  0520           jne	#0x44d2 <check_password+0x16>
44c8:  1e53           inc	r14
44ca:  3e92           cmp	#0x8, r14
44cc:  f823           jne	#0x44be <check_password+0x2>
44ce:  1f43           mov	#0x1, r15
44d0:  3041           ret
44d2:  0f43           clr	r15
44d4:  3041           ret
```
It can be clearly seen that `cmp.b` operations are being performed at the address seen at `create_password`, `0x2400`. So, the password we enter will be checked against the bytes stored at addresses starting from `0x2400`.
Now, let's create a break point at 
```447e:  3f40 0024      mov	#0x2400, r15```, continue by hitting `c` in the console.
We can now see `2400:   0000 0000 0000 0000   ........` in memory dump.
Remove the break point and make a new break point at `44ac:  cf43 0700      mov.b	#0x0, 0x7(r15)` and hit `c` to continue. Memory dump gets updated, `2400:   4324 6151 6279 6800   C$aQbyh.` (last part `.` in password is actually NULL character, so our password is `C$aQbyh`)
This is it !!! We got our password.


## Password
`C$aQbyh` in ASCII text or `4324615162796800` in HEX.
