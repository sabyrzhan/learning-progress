# Math
## Combinations and Permutations
1. Selection = Combination
2. Arrangement = Permutation
3. OR -> "+" (addition)
4. AND -> "*" (multiplication)

Examples:<br>
**Q: Form a 3-digit number from 1,2,3,4, such that repetition of any number is allowed. How many cases of 3-digit numbers can be formed?<br>**
A: 4(from 1-4) * 4(from 1-4) * 4(from 1-4) = 4^3=64

**Q: Form a 3-digit number from 1,2,3,4, such that no repetition of any number is allowed.<br>**
A: 2(one of 1-4) * 3(one of 1-4) * 4(one of 1-4) = 2 * 3 * 4=24

**Q: Form a 3-digit number from 1,2,3,4,5 such that 1) no repetition of any number is allowed 2) repetition is allowed<br>**
A:
1) 5(from 1-5) * 5(from 1-5) * 5(from 1-5) = 5^3=125
2) 3(from 1-5 but exluding 3 digits) * 4(from 1-5 but excluding 2 digits) * 5(from 1-5) = 3 * 4 * 5 = 60

**Q: Form a 3-digit number from 0,3,4,5,1, such that 1) no repetition 2) repitition of any number is allowed.<br>**
A:
1. 4(3,5,4,1) * 5(0,3,4,5,1) * 5(0,3,4,5,1) = 100
2. 4(3,5,4,1) * 4(3,5,4,1) * 3(3,5,4,1) = 48

**Q: Form a 3-digit number from 1,2,3,4,5, such that 5 should come once or twice.<br>**
A:<br>
*Once:<br>*
4(1,2,3,4) * 4(1,2,3,4) * 1(5) = 16<br>
4(1,2,3,4) * 1(5) * 4(1,2,3,4) = 16<br>
1(5) * 4(1,2,3,4) * 4(1,2,3,4) = 16<br>
16 + 16 + 16 = 48

*Twice:<br>*
4(1,2,3,4) * 1(5) * 1(5) = 4<br>
1(5) * 1(5) * 4(1,2,3,4) = 4<br>
1(5) * 4(1,2,3,4) * 1(5) = 4<br>
4 + 4 + 4 = 12

**Q: Form a 4-digit number from 1,2,3..9, such that the number cannot start from 0 and no repetition allowed.<br>**
A:<br>
9(one of 1-9) * 9(one of 0-9) * 8(one of 0-9) * 7(one of 0-9) = 4528

**Q: Generate 3-letter password from uppercase letters(ABC), lowercase letters(abc) and 0-9 numbers.<br>**
A:<br>
(3(ABC) * 3(abc) * 3(0-9)) * 3!(arrangements) = 3 * 3 * 3 * 3 * 2 * 1 = 162

**Q: Generate 3-letter password with following conditions<br>**
**- one numeral from 0-9**<br>
**- one uppercase english alphabet**<br>
**- one lowercase english alphabet**<br>
A:<br>
10 * 26 * 26 * 3! = 40560

**Q: How many different words can be formed from the letters of the word COMPLIANCE when:<br>**
**- all the letters are taken**<br>
**- the letter 'G' always occupy the 1st place**<br>
**- letter 'P' and 'E' should occupy 1st and last place respectively**<br>

A:<br>
1) All 10 letters are unique: 10! = 1 * 2 * 3 * 4 * 5 * 6 * 7 * 8 * 9 * 10 = 3 628 800
2) 1 * 9! = 362 880
3) 1 * 8! * 1 = 40 320



