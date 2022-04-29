# All about Loan
## EMI calculation in Excel
EMI (Equated Monthly Installment) - monthly payment to borrower, which often time include interest value alongside principal value.

To calculate in Excel there are several functions to use:
- PMT() - EMI calculator
- IPMT() - installment calculator
- PPMT() - principal calculator

For example, given:
- Loan: $10 000
- Period: 5 years
- Interest rate: 10%

Calculate EMI, installement and principals.
- EMI: `PMT(10%;5;-10000) = $2 637,97/y or €212,47/mo`
- Installment: `IPMT(10%,1;5;-10000) = $1 000,00/y or €83,33/mo`
- Principal: `PPMT(10%;1;5;-10000) = $1 637,97/y or €129,14/mo`
- Total payment: `$2 637,97/y * 5 = $13 189,87`
- Total interset: `$13 189,87 - $10 000 = $3 189,87`
