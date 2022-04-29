# General Excel
## Relative, absolute and mixed references
- Relative - no locked rows or columns, which means automatically incremented in case of dragging. Generally you refer the value by row and column. For example, `A1` or `B3`.
- Absolute - locaked to value on specific row and column. To refer to the locked value you put `$` sign in front of column or row value. For example, `$A$1`.
- Mixed - either of the row or column is locked. You put `$` sign in front of either column or row name. For example, `$A1` - locked to A column, `B$1` - locked to 1st row.

## What-If?
What-If is used to calculate the value based on other values which are also calculated with functions. So it automatically adjusts other's value to suite the condition. The "What-If" function is located in "Data" tab. It consists from following required fields:
* Set cell - cell location
* To value - target cell value
* By chaning cell - another cell which needs to be updated. This cell also has to use functions to calculate its value.
