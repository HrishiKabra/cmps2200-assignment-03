# CMPS 2200 Assignment 3
## Answers

**Name:** Hrishi Kabra


Place all written answers from `assignment-03.md` here for easier grading.

**1a)** Given a $N$ dollars, state a greedy algorithm for producing as few coins as possible that sum to $N$.

Given $N$ dollars and coin denominations of powers of 2, the algorithm would be to
- start with the largest coin denomination `d` that is lesser than or equal to N
- Update N to N - d
- Repeat with next smaller coin denomination until N = 0

```python
def Greedy_Coin_Algorithm(N):
    coins = []
    while N != 0:
        coin = 1
        while coin * 2 <= N:
            coin = coin * 2
        
        coins.append(coin)
        N = N - coin
```

Similar to how you convert decimal to binary 

**1b)** Prove that this algorithm is optimal by proving the greedy choice and optimal substructure properties.

Using the largest coin 2^i such that 2^i is <= N but it 2^i+1 is > N - to make this 2^i using smaller coins you need atleast two coins - 2^i-1 + 2^i-1 - however using just one coin of 2^i is more efficient so the optimal solution must use it - hence proving the greedy choice property
Also this is basically representing the coin in binary - every number has a unique binary representation - greedy algorithm corresponds to writing N in binary which uses the minimum number of coins needed

If we have an optimal solution for N that uses coin 2^i - then the solution for the remaining N-2^i must also be optimal - hence proving it has the optimal substructure property

**1c)** What is the work and span of your algorithm?

Work is O(log N) - we iterate through at most log_2 (N) coin denominations - each iteration has constant time operations

Span is also O(log N) - it is sequential so span is equal to work 


**2a)** You realize the greedy algorithm you devised above doesn't work in Fortuito. Give a simple counterexample that shows that the greedy algorithm does not produce the fewest number of coins.

Coin denominations are 1, 3, 4 and N = 6

Greedy Algorithm:
- Use coin 4 - 6-4 = 2
- Use coin 1 - 2-1 = 1
- Use coin 1 - 1-1 = 0
- total coins is 3

Optimal solution:
- use coin 3 - 6-3 = 3
- use coin 3 - 3-3=0
- total coins is 2

this shows that the greedy algorithm does not always give optimal solution for arbitrary denominations

**2b)** Since you paid attention in Algorithms class, you realize that while this problem does not have the greedy choice property it does have an optimal substructure property. State and prove this property.

Optimal substructure means that the optimal solution for N dollars is built from optimal solutions of smaller subproblems.

If we have an optimal solution for N dollars that uses coin D_i, then the solution for the remaining N - D_i dollars must also be optimal.

Proof by contradiction:
- Suppose we have an optimal solution for N that uses coin D_i
- But the solution for N - D_i is not optimal
- Then there exists a better solution for N - D_i that uses fewer coins
- We could combine this better solution with coin D_i to get a better solution for N
- This contradicts that our original solution for N was optimal
- Therefore, the solution for N - D_i must be optimal

C(N) = min over all coins D_i where D_i <= N of {1 + C(N - D_i)}

This means the minimum coins needed for N equals 1 (for the chosen coin) plus the minimum coins needed for the remaining amount after using that coin.



**2c)** Use this optimal substructure property to design a dynamic programming algorithm for this problem. If you used top-down or bottom-up memoization to avoid recomputing solutions to subproblems, what is the work and span of your approach?

bottom-up dynamic programming approach - create an array to store the best (minimum coins) result for each amount from 0 to N.

```python
def min_coins(denoms, N):
    best = [float('inf')] * (N + 1)
    best[0] = 0  # base case: 0 coins needed for 0 dollars
    for total in range(1, N + 1):
        for coin in denoms:
            if coin <= total:
                best[total] = min(best[total], 1 + best[total - coin])
    return best[N]
```

The algorithm works by:
- Starting with base case: best[0] = 0 (no coins needed for 0 dollars)
- For each amount from 1 to N, try each coin denomination
- If the coin fits, check if using it gives a better solution (fewer coins)
- Store the minimum coins needed for each amount

Work is O(N * k) where N is the target amount and k is the number of denominations - double loop - outer loop goes through N amounts, inner loop goes through k denominations

Span is O(N * k) - sequential -  each iteration depends on previous results -  outer loop must be sequential -  within each iteration we check all denominations sequentially