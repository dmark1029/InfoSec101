# Microcorruption - Cusco
Author: [Kuldeep Singh Chouhan](https://github.com/kuldeep-singh-chouhan)

Challenge Page: [Microcorruption](https://microcorruption.com/)

## Walkthrough
This level is different from previous ones. Let's  start looking at the `main` function.
```4438 <main>
4438:  b012 0045      call	#0x4500 <login>
```
`main` directs to `login`
```4500 <login>
4500:  3150 f0ff      add	#0xfff0, sp
4504:  3f40 7c44      mov	#0x447c "Enter the password to continue.", r15
4508:  b012 a645      call	#0x45a6 <puts>
450c:  3f40 9c44      mov	#0x449c "Remember: passwords are between 8 and 16 characters.", r15
4510:  b012 a645      call	#0x45a6 <puts>
4514:  3e40 3000      mov	#0x30, r14
4518:  0f41           mov	sp, r15
451a:  b012 9645      call	#0x4596 <getsn>
451e:  0f41           mov	sp, r15
4520:  b012 5244      call	#0x4452 <test_password_valid>
4524:  0f93           tst	r15
4526:  0524           jz	#0x4532 <login+0x32>
4528:  b012 4644      call	#0x4446 <unlock_door>
452c:  3f40 d144      mov	#0x44d1 "Access granted.", r15
4530:  023c           jmp	#0x4536 <login+0x36>
4532:  3f40 e144      mov	#0x44e1 "That password is not correct.", r15
4536:  b012 a645      call	#0x45a6 <puts>
453a:  3150 1000      add	#0x10, sp
453e:  3041           ret
```
We can't find any security checks as such. Again the password is atmost of 16 digits. We can observe that `sp` points to the next address of that of last digit of password. When the `login` returns `sp` is returned. So, if we can deliberately put address of `unlock_door` after the password, door might get unlocked. But they need to placed according to [little endian format](https://www.geeksforgeeks.org/little-and-big-endian-mystery/).
Let the 16 digits be letter `i` and its HEX form is `69` so the password becomes `69696969696969696969696969696969`. But we also need to add `0x4446` at the end of this string in the little endian format. So final passsword will be `696969696969696969696969696969694644`.

This is it !!! We got our password.


## Password
`696969696969696969696969696969694644` in HEX.
