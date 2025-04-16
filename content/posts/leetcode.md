+++
title = 'Leetcode Problems'
date = 2025-02-28T15:35:52-07:00
draft = false
tags = ["project"]
readTime = true
toc = true
+++

> Here are a few Leetcode/Leetcode-esque problems that I have been working on over the past couple of months

## 1. Reverse Vowels of a String (Leetcode #345)

[Original Problem](https://leetcode.com/problems/reverse-vowels-of-a-string)

### Intuition

My first thought was I need to record the index of the location of each value in the string, and then reverse the array, and rebuild the string.

### Approach

I went through the string, checked if the character was a vowel, and if so appended it's index to an array. I then reversed that array, and rebuilt the string, replacing the vowels one by one with the vowel at the reversed index of the original string

### Complexity

-   Time complexity: _O(n)_
-   Space complexity: _O(n)_

### Code

```python3
class Solution:
    def reverseVowels(self, s: str) -> str:
        vowel_indices = []
        # find indices of all vowels
        for i, c in enumerate(s):
            c_lower = c.lower();
            if c_lower == 'a' or c_lower == 'e' or c_lower == 'i' or c_lower =='o' or c_lower =='u':
                vowel_indices.append(i)
        reverse_vowel_indices = vowel_indices[::-1]
        new_string = s
        for k, value in enumerate(reverse_vowel_indices):
            new_string = new_string[:value] + s[vowel_indices[k]] + new_string[value + 1:]
        return new_string
```

## 2. Two Sum (Leetcode #1)

[Original Problem](https://leetcode.com/problems/two-sum/)

### Intuition

My approach here was pretty naïve, as it turns out. All I have to do is loop through the list, and for each item `i` check if any of the items in the list after can add with my`i` to equal the target.

### Approach

Pretty simple, nested for loops, and it returns if it finds a sum that equals the target.

### Complexity

-   Time complexity: _O(n^2^)_
-   Space complexity: _O(1)_

## Code

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        i = 0
        j = 1
        while (i < len(nums) - 1):
            j = i + 1
            while (j < len(nums)):
                if (nums[i] + nums[j] == target):
                    return i, j
                j += 1
            i += 1

```

## 3. Best Time to Buy and Sell Stock (Leetcode #121)

[Original Problem](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### Intuition

My first intuition is that I wanted to figure out how to only have to loop through the list once.

### Approach

We just need to hold onto the lowest value we come across, along with the highest profit we have ever come across. In this approach, it is OK if we find a lower buy price later on and store it, even if it never results in a higher profit, because we are just returning the highest profit. For example, if we have an array `[2, 10, 1, 8]`, the profit will still return as 8, even if the lowest price was set to 1 at the end of the run.

### Complexity

-   Time complexity: _O(n)_
-   Space complexity: _O(1)_

## Code

```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        buy = prices[0]
        profit = 0
        i = 1
        while i < len(prices):
            if prices[i] < buy:
                buy = prices[i]
            elif prices[i] - buy > profit:
                profit = prices[i] - buy
            i += 1
        return profit

```

## 4. Valid Sudoku (Leetcode #36)

[Original Problem](https://leetcode.com/problems/valid-sudoku/)

### Intuition

My first thought was to get an array for every column, row, and box, and check if there are any duplicates in any of those entities.

### Approach

For each type of check, column, row, and box, I wrote a function that returned a one-dimensional array of the requested entity to check, and then I had a universal function that could check if an array had any duplicates. If there were any duplicates, I immediately returned false.

### Complexity

-   Time complexity:
    -   Because the sudoku grid is a fixed size, _O(1)_
-   Space complexity:
    -   Again, because the sudoku grid is a fixed size, _O(1)_

### Code

```js
/**
 * @param {character[][]} board
 * @return {boolean}
 */
var isValidSudoku = function (board) {
    const columns = [];
    for (let col = 0; col < board[0].length; col++) {
        if (hasDuplicateValue(columnForBoard(board, col))) {
            return false;
        }
    }

    for (let row = 0; row < board.length; row++) {
        if (hasDuplicateValue(board[row])) {
            return false;
        }
    }
    for (let row = 0; row < board.length / 3; row++) {
        for (let col = 0; col < board[row].length / 3; col++) {
            if (hasDuplicateValue(squareForBoard(board, row, col))) {
                return false;
            }
        }
    }
    return true;
};

function hasDuplicateValue(array) {
    let existingNumbers = [];
    for (let i = 0; i < array.length; i++) {
        value = array[i];
        if (existingNumbers[value] === 1 && value !== ".") {
            return true;
        } else {
            existingNumbers[value] = 1;
        }
    }
    return false;
}

function columnForBoard(board, columnIndex) {
    const column = [];
    for (let row = 0; row < board.length; row++) {
        column.push(board[row][columnIndex]);
    }
    return column;
}

function rowForBoard(board, rowIndex) {
    return board[rowIndex];
}

function squareForBoard(board, rowIndex, columnIndex) {
    const square = [];
    rowIndex = rowIndex * 3;
    columnIndex = columnIndex * 3;
    for (let row = rowIndex; row < rowIndex + 3; row++) {
        for (let column = columnIndex; column < columnIndex + 3; column++) {
            square.push(board[row][column]);
        }
    }
    return square;
}
```

## 5. Majority Element (Leetcode #169)

[Original Problem](https://leetcode.com/problems/majority-element/)

### Intuition

So for this, I wanted to aim to only go through the numbers once. I also wanted to return as early as possible, which is the moment the count of any number exceeds the length of the array divided by 2.

### Approach

I created a hash map, then looped through the array of numbers. Any time I encountered a given number, I incremented the count at that number's key in the hash map by 1. I then checked if that number's count exceeded the threshold to be the majority. If so, I returned, if not, I continued

### Complexity

-   Time complexity: _O(n)_
-   Space complexity: _O(n)_

### Code

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function (nums) {
    const length = nums.length;
    const threshold = length / 2;
    const counts = {};

    for (let i = 0; i < length; i++) {
        counts[nums[i]] = 1 + (counts[nums[i]] || 0);
        if (counts[nums[i]] >= threshold) {
            return nums[i];
        }
    }
};
```

## 6. Roman to Integer (Leetcode #13)

[Original Problem](https://leetcode.com/problems/roman-to-integer/)

### Intuition

I first recognized that each Roman number maps directly to an Arabic numeral value, I also recognized that I would add together all the values, except in the case where a Roman numeral was a smaller order/value than the next numeral, in which case I would subtract that value (i.e. `VI` is `5 + 1`, whereas `IV` is `-1 + 5`).

### Approach

I first created a map of of the Roman numerals to their integer value. For this problem, the maximum Roman numeral was limited, so this map was not very large. I then looped through the string, checked the value in the map against the value of the next character in the string. If the value was less, we subtract from the total, if not, we add to the total

### Complexity

-   Time complexity:
    -   This is a bit complicated. Since the largest value of the Roman numeral passed into this function was 3999, technically the time complexity is _O(1)_, as the input will never be longer than MMMCMXCIX. However, assuming this map was expanded and the algorithm infinite, the time complexity is _O(n)_
-   Space complexity: _O(1)_

### Code

```js
/**
 * @param {string} s
 * @return {number}
 */
var romanToInt = function (s) {
    let romanMap = { I: 1, V: 5, X: 10, L: 50, C: 100, D: 500, M: 1000 };
    let total = 0;
    for (let j = 0; j < s.length; j++) {
        if (romanMap[s[j]] < romanMap[s[j + 1]]) {
            total -= romanMap[s[j]];
        } else {
            total += romanMap[s[j]];
        }
    }
    return total;
};
```

## 7. Valid Parentheses (Leetcode #20)

[Original Problem](https://leetcode.com/problems/valid-parentheses/)

### Intuition

This problem requires checking if brackets are properly matched and closed in the correct order. A stack is perfect for this because we need to match the most recently opened bracket with its closing pair.

### Approach

1. We use a stack to keep track of opening brackets
2. We use a hash map to store the matching pairs of brackets
3. For each character:
    - If it's an opening bracket, push to stack
    - If it's a closing bracket:
        - Check if it matches the last opening bracket in stack
        - If matches, pop the stack
        - If doesn't match or stack is empty, return false
4. Finally, check if stack is empty (all brackets were properly closed)

### Complexity

-   Time complexity: _O(n)_ where n is the length of the string, as we need to process each character once
-   Space complexity: _O(n)_ in worst case where all characters are opening brackets, they all get stored in the stack

### Code

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function (s) {
    const stack = [];
    const hash = { ")": "(", "]": "[", "}": "{" }; // Matching pairs

    for (const char of s) {
        if (char in hash) {
            if (stack.length && stack[stack.length - 1] === hash[char]) {
                stack.pop();
            } else {
                return false;
            }
        } else {
            stack.push(char);
        }
    }
    return stack.length === 0;
};
```

## 8. Bridge Repair Part 1 ( Advent of Code 2024)

> Note: This is the first [Advent of Code](https://adventofcode.com) problem I have in here, so I'll briefly explain. Advent of Code is a popular Christmas-themed coding challenge that I decided to start, but never got around to finishing, until now

[Original Problem](https://adventofcode.com/2024/day/7)

### Intuition

In reading through this problem, I realized it was very similar to the [last problem](#7-valid-parentheses-leetcode-20) I had just done. I needed to find all of the possible ways to perform operations on a list of numbers, although this time I had the simpler constraints of using only addition and multiplication, as well as evaluating left to right. I knew I should probably try to use recursion to solve this problem.

### Approach

My first objective: Get a list of all possible results from the list of values. (Recursive function)

1. Base case is if the list of values passed in has only one element.
2. Then, loops through the result of a recursive call, passing in the subproblem of the original array up to, but not including the last element of the array. This ensures that the values are evaluated left-to-right
3. For each of those results, append the result to the last element of the array and the result + the last element of the array
4. Then I just have to check if my expected result from my input is in the array of results!

### Complexity

-   Time complexity: `O(2^n^)`, where `n` is the total number of integers passed in
-   Space complexity: Also `O(2^n^)`

### Code

```python
def allPossibleResults(values: list) -> list:
    if len(values) == 1:
        return values
    results = []
    for result in allPossibleResults(values[:-1]):
        results.append(result + values[-1])
        results.append(result * values[-1])
    return results

def day1(input: list) -> int:
    valid = 0
    for line in input:
        result = int(line.split(":")[0])
        values = list(map(int, line.split(":")[1].strip().split(" ")))
        if (result in allPossibleResults(values)):
            valid = valid + result
    return valid

```

## 9. Bridge Repair Part 2 ( Advent of Code 2024)

[Original Problem](https://adventofcode.com/2024/day/7#part2)

### Intuition

This one is pretty brief. My intuition was simply to modify my `allPossibleResults` function from the previous problem to add the concatenation.

### Approach

1. Cast the numbers as strings
2. Add together
3. Cast back to int

### Complexity

-   Time complexity: Same as previous problem
-   Space complexity: Same as previous problem

### Code

```python
def allPossibleResults2(values: list) -> list:
    if len(values) <= 1:
        return values
    results = []
    for result in allPossibleResults2(values[:-1]):
        results.append(result + values[-1])
        results.append(result * values[-1])
        results.append(int(str(result) + str(values[-1])))
    return results

def day2(input=input) -> int:
    valid = 0
    for line in input:
        result = int(line.split(":")[0])
        values = list(map(int, line.split(":")[1].strip().split(" ")))
        if (result in allPossibleResults2(values)):
            valid = valid + result
    return valid
```

## 10. Resonant Collinearity Part 1 (Advent of Code)

[Original Problem](https://adventofcode.com/2024/day/8)

### Intuition

This problem requires keeping track of a pattern and offset in a grid, and then identifying what spots are "antinodes" according to that offset. My first thought was that a map would be a great way to do this, with the key being a string of the coordinates that I have marked as "antinodes", and the number of unique "antinodes" that I find will just be the number of unique keys in my map.

### Approach

1. Go through each character in the input
2. For each one, if the first `antenna_map[character]` offset does not exist, create it as an array and add the coordinates of that character to the array
3. If it does already exist in the `antenna_map`, then we have a "resonance" that creates "antinodes". Those antinodes are found at offsets from these characters. I need to calculate these offsets to find the antinodes, and then add the found antinodes' coordinates to a `unique_antinode_map`, so they are only counted as an antinode once.
4. Return the length of the `unique_antinode_map`.

### Complexity

-   Time complexity: This one is complicated. The worst case would be if every character in the input was the same, meaning that they are all antennae and have a relationship with every other character in the input, each one creating antinodes. In this worst case, the complexity would be `O((m x n)^2)`.
-   Space complexity: Again the worst case would be if all characters were the same, and the space would be `O(m x n)`.

### Code

```
def distance_between(left, right):
    return left[0] - right[0] + left[1] - right[1]

class FrequencyRelationship:
    coordinates: tuple
    # offset from first item of tuple to second item of tuple
    offset: tuple

    def __init__(self, coordinates):
        self.coordinates = coordinates
        self.offset= (coordinates[0][0] - coordinates[1][0], coordinates[0][1] - coordinates[1][1])

    def distance(self) -> int:
        return self.offset[0] + self.offset[1]

    def antinodes(self) -> tuple:
        antinode1 = (self.coordinates[0][0] + self.offset[0], self.coordinates[0][1] + self.offset[1])
        antinode2 = (self.coordinates[1][0] - self.offset[0], self.coordinates[1][1] - self.offset[1])
        return [antinode1, antinode2]

def part1(input=input) -> int:
    # unique_antinode_map - hash map of coordinates, if true, an antinode is there. will get passed into the add_antinodes function for each relationship that exists
    unique_antinode_map = {}
    # go through input
    # find locations of each frequency (use a map? then each object in the map is an array of coordinates?)
    # use a map then each object in the map is an array of coordinates
    # also have an antinode map

    antenna_map = {}

    for y, line in enumerate(input):
        for x, char in enumerate(line):
            if char != '.':
                if char not in antenna_map:
                    antenna_map[char] = []
                else:
                    for coordinate in antenna_map[char]:
                        new_relationship = FrequencyRelationship((coordinate, (x,y)))
                        for antinode in new_relationship.antinodes():
                            if antinode[0] in range(len(line)) and antinode[1] in range(len(input)):
                                unique_antinode_map[str(antinode[0]) + ',' + str(antinode[1])] = True
                antenna_map[char].append((x,y))

    # go through each coordinate. for each one, calculate offset and "distance" to each other coordinate in the array
    # find an efficient way to store these offsets and distances
        # plot antinodes
    # an antinode is placed wherever the offset is the same as between two nodes of the same frequency, and the distance is

    return len(unique_antinode_map)
```

## 11. Resonant Collinearity Part 2 (Advent of Code)

[Original Problem](https://adventofcode.com/2024/day/8#part2)

### Intuition

This one is very similar to part 1, except now the antinodes extend on forever according to the offset between the "antennae". This means that instead of finding one "antinode" on each side, I needed to loop and keep looking for antinodes until I hit the edge of the grid.

### Approach

Mostly the same as before, except now, with each found relationship, I use the offset of the relationship and loop, continuing to add antinodes to the list until I hit the edge of the grid of characters

### Complexity

-   Time complexity: Increases because for each relationship, we now are looping, so it becomes `O((m x n)^3)
-   Space complexity: The same as part 1

### Code

```
class FrequencyRelationship:
    # previous code from part 1...

    def expanded_antinodes(self, width, height) -> list:
        # while the current_antinode[0] is within the width and the current_antinode[1] is within the height
        # append the current_antinode to the list
        # then add the offset to the current_antinode
        (antinode1, antinode2) = self.antinodes()
        antinode_list = []
        current = antinode1
        while 0 <= current[0] < width and 0 <= current[1] < height:
            antinode_list.append(current)
            current = (current[0] + self.offset[0], current[1] + self.offset[1])
        current = antinode2
        while 0 <= current[0] < width and 0 <= current[1] < height:
            antinode_list.append(current)
            current = (current[0] - self.offset[0], current[1] - self.offset[1])

       # Also need to check in opposite directions from the original antenna positions
        current = (self.coordinates[0][0] - self.offset[0], self.coordinates[0][1] - self.offset[1])
        while 0 <= current[0] < width and 0 <= current[1] < height:
            antinode_list.append(current)
            current = (current[0] - self.offset[0], current[1] - self.offset[1])

        current = (self.coordinates[1][0] + self.offset[0], self.coordinates[1][1] + self.offset[1])
        while 0 <= current[0] < width and 0 <= current[1] < height:
            antinode_list.append(current)
            current = (current[0] + self.offset[0], current[1] + self.offset[1])

        return antinode_list

def part2(input=input) -> int:
    unique_antinode_map = {}
    antenna_map = {}

    for y, line in enumerate(input):
        for x, char in enumerate(line):
            if char != '.':
                if char not in antenna_map:
                    antenna_map[char] = []
                else:
                    for coordinate in antenna_map[char]:
                        new_relationship = FrequencyRelationship((coordinate, (x,y)))
                        for antinode in new_relationship.expanded_antinodes(len(line), len(input)):
                            if antinode[0] in range(len(line)) and antinode[1] in range(len(input)):
                                unique_antinode_map[str(antinode[0]) + ',' + str(antinode[1])] = True
                antenna_map[char].append((x,y))
    return len(unique_antinode_map)
```

Thanks for reading! I've had a lot of fun with these; I hope to refine them in the future.

If you want to take a look at specifically my Advent of Code solutions as I keep working on them, go to my [repo](https://github.com/algabl/AdventofCode) for them.
