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
A: 2(from 1-4 but exluding 2 digits) * 3(from 1-4 but exluding 1 digit) * 4(from 1-4) = 2 * 3 * 4=24

**Q: Form a 3-digit number from 1,2,3,4,5 such that 1) no repetition of any number is allowed 2) repetition is allowed<br>**
A:
1) 5(from 1-5) * 5(from 1-5) * 5(from 1-5) = 5^3=125
2) 3(from 1-5 but exluding 3 digits) * 4(from 1-5 but excluding 2 digits) * 5(from 1-5) = 3 * 4 * 5 = 60

**Q: Form a 3-digit number from 0,3,4,5,1, such that 1) no repetition 2) repitition of any number is allowed.<br>**
A:
1. 4(3,5,4,1) * 5(0,3,4,5,1) * 5(0,3,4,5,1) = 100
2. 4(3,5,4,1) * 4(3,5,4,1) * 3(5,4,1) = 48

**Q: Form a 3-digit number from 1,2,3,4,5, such that 5 should come once or twice.<br>**
A:<br>
*Once:*
4(1,2,3,4) * 4(1,2,3,4) * 1(5) = 16<br>
4(1,2,3,4) * 1(5) * 4(1,2,3,4) = 16<br>
1(5) * 4(1,2,3,4) * 4(1,2,3,4) = 16<br>
16 + 16 + 16 = 48

*Twice:*
4(1,2,3,4) * 1(5) * 1(5) = 4<br>
1(5) * 1(5) * 4(1,2,3,4) = 4<br>
1(5) * 4(1,2,3,4) * 1(5) = 4<br>
4 + 4 + 4 = 12

