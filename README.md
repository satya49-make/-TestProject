## Below is a structured list of common Array problem patterns used in interviews 👇

## 1. Two Pointer Pattern

- Use two indexes moving from start/end.
- Common problems
- Reverse an array
- Remove duplicates from sorted array
- Container with most water
- Two Sum in sorted array
- Move zeros to end
```
Example
int left = 0;
int right = arr.length - 1;

while(left < right){
    int temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;
    left++;
    right--;
}
```
## 2. Sliding Window Pattern

- Used when dealing with subarray / contiguous range problems.
- Common problems
- Maximum sum subarray of size K
- Longest substring without repeating characters
- Minimum window substring
- Maximum average subarray
```
Example
int windowSum = 0;
for(int i=0;i<k;i++)
    windowSum += arr[i];

int maxSum = windowSum;

for(int i=k;i<arr.length;i++){
    windowSum += arr[i] - arr[i-k];
    maxSum = Math.max(maxSum, windowSum);
}
```
## 3. Prefix Sum Pattern

- Used when range queries are asked.
- Common problems
- Subarray sum equals K
- Range sum queries
- Count subarrays with sum K
```
Example
prefix[i] = prefix[i-1] + arr[i];

Subarray sum
sum(l,r) = prefix[r] - prefix[l-1]
```
## 4. Kadane's Algorithm Pattern

- Used for maximum / minimum subarray problems
- Common problems
- Maximum subarray sum
- Maximum product subarray
- Circular subarray sum
```
Example
int max = arr[0];
int current = arr[0];

for(int i=1;i<arr.length;i++){
    current = Math.max(arr[i], current + arr[i]);
    max = Math.max(max, current);
}
```
## 5. HashMap / Frequency Count Pattern

- Used when counting or tracking elements
- Common problems
- Two Sum
- First non-repeating element
- Subarray sum equals K
- Majority element
```
Example
Map<Integer,Integer> map = new HashMap<>();

for(int num : arr){
    map.put(num, map.getOrDefault(num,0)+1);
}
```
## 6. Sorting Based Pattern

- Sort first, then solve.
- Common problems
- Three sum
- Merge intervals
- Meeting rooms
- Find duplicates
```
Example
Arrays.sort(arr);
```
## 7. Binary Search Pattern

- Used when array is sorted or monotonic
- Common problems
- Search in sorted array
- First/last occurrence
- Peak element
- Rotated sorted array search
```
Example
while(left <= right){
    int mid = (left + right)/2;
}
```
## 8. Greedy Pattern

 Choose locally optimal solution Common problems
- Jump game
- Stock buy and sell
- Gas station problem
```
Example (stock profit):

for(int i=1;i<arr.length;i++){
    if(arr[i] > arr[i-1])
        profit += arr[i] - arr[i-1];
}
```
## 9. Merge Intervals Pattern

Used when ranges overlap
Common problems

- Merge intervals
- Insert interval
- Meeting rooms

## 10. Matrix / 2D Array Pattern

Arrays can also be 2D
Common problems

- Spiral matrix
- Rotate matrix
- Flood fill
- Number of islands

## 11. Cyclic Sort Pattern

Used when numbers are 1..N range
Common problems

- Find missing number
- Find duplicate number
- First missing positive

## 12. Backtracking / Recursion on Arrays

Used for subsets/permutations
Common problems

- Subsets
- Permutations
- Combination sum

## Most Asked Array Interview Questions
For 4–5 year Java developer, these are most common:

- Two Sum
- Best Time to Buy and Sell Stock
- Maximum Subarray
- Product of Array Except Self
- Rotate Array
- Move Zeroes
- Find Missing Number
- Merge Intervals
- Container With Most Water
- Subarray Sum Equals K

## How Interviewers Actually Ask
Instead of direct questions, they ask like:

- Find maximum profit from stock prices
- Find subarray whose sum equals K
- Find longest sequence
- Find duplicates in array without extra space
- Rotate array K times
- Find first missing positive

## ✅ Pro Tip (Important for you)
For 4–5 yrs backend developer interviews, focus on these 6 patterns first:

- Two Pointer
- Sliding Window
- HashMap / Prefix Sum
- Binary Search
- Kadane's Algorithm
- Greedy
