# Math
## Combinations and Permutations
1. Selection = Combination
2. Arrangement = Permutation
3. OR -> "+" (addition)
4. AND -> "*" (multiplication)

Examples:<br>
Q: Form a 3-digit number from 1,2,3,4, such that repetition of any number is allowed. How many cases of 3-digit numbers can be formed?<br>
A: 4(from 1-4) * 4(from 1-4) * 4(from 1-4) = 4^3=64

Q: Form a 3-digit number from 1,2,3,4, such that no repetition of any number is allowed. How many cases of 3-digit numbers can be formed?<br>
A: 2(from 1-4 but exluding 2 digits) * 3(from 1-4 but exluding 1 digit) * 4(from 1-4) = 2 * 3 * 4=24

Q: Form a 3-digit number from 1,2,3,4,5 such that:
  1) no repetition of any number is allowed
  2) repetition is allowed
How many cases of 3-digit numbers can be formed?<br>
A:
1) 5(from 1-5) * 5(from 1-5) * 5(from 1-5) = 5^3=125
2) 3(from 1-5 but exluding 3 digits) * 4(from 1-5 but excluding 2 digits) * 5(from 1-5) = 3 * 4 * 5 = 60
