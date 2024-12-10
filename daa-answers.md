# Design and Analysis of Algorithms
## Complete Answer Sheet

### Q1) 
#### a) Job Scheduling using Greedy Approach [9 marks]

Algorithm for Job Scheduling:
```
1. Sort all jobs in descending order of profit
2. Initialize result sequence with first job
3. For each remaining job:
   - Find latest empty slot before deadline
   - If found, add job to that slot
```

For the given problem:
First, sort jobs by profit: J2(100) > J1(60) > J4(40) > J3(20) = J5(20)

Processing sequence:
1. J2: deadline 1, slot 1 assigned
2. J1: deadline 2, slot 2 assigned
3. J4: deadline 2, no slot available (skip)
4. J3: deadline 3, slot 3 assigned
5. J5: deadline 1, no slot available (skip)

Optimal sequence: J2 → J1 → J3
Maximum profit = 100 + 60 + 20 = 180

#### b) Knapsack Problem using Dynamic Programming [9 marks]

Let's solve this step by step:

1) Create table K[i][w] where i is the number of items and w is the current capacity
```
K[i][w] = max(K[i-1][w],                  // don't include item
              K[i-1][w-wi] + pi)          // include item
```

Dynamic Programming Table:
```
    0   1   2   3   4   5  (Weight)
0   0   0   0   0   0   0
1   0   0  12  12  12  12  (Item 1: W=2, P=12)
2   0  10  12  22  22  22  (Item 2: W=1, P=10)
3   0  10  12  22  22  32  (Item 3: W=3, P=20)
4   0  10  15  25  27  37  (Item 4: W=2, P=15)
```

Optimal solution: Value = 37
Selected items: 
- Item 4 (W=2, P=15)
- Item 3 (W=3, P=20)
- Item 2 (W=1, P=10)

### Q3) 
#### a) Traveling Salesman Problem using Branch and Bound [9 marks]

Initial Distance Matrix:
```
   A   B   C   D
A  0  10  15  20
B  10  0  35  25
C  15  35  0  30
D  20  25  30  0
```

Solution steps:
1. Initial lower bound calculation:
   For each row, subtract minimum element
   For each column, subtract minimum element

2. Branch and Bound tree:
   Root → A
   Level 1: A→B, A→C, A→D
   Continue based on minimum cost path

Final optimal tour: A → B → D → C → A
Total cost = 10 + 25 + 30 + 15 = 80

#### b) Backtracking [8 marks]

Principle:
- Systematically search for a solution to a problem among all available options
- Build candidates incrementally and abandon candidates ("backtrack") when determined that candidate cannot lead to a valid solution

Control Abstraction:
```
void backtrack(node v) {
    if (solution(v)) 
        process_solution(v);
    else {
        for each child u of v {
            if (valid(u)) {
                backtrack(u);
            }
        }
    }
}
```

Time Analysis:
- Worst case: O(b^m) where b is branching factor and m is maximum depth
- Best case: O(1) if solution found immediately
- Average case depends on problem constraints

Example: N-Queens Problem
```
bool isSafe(board[][], row, col, N) {
    // Check row on left side
    // Check upper diagonal
    // Check lower diagonal
    return true if safe, false otherwise
}

bool solveNQ(board[][], col, N) {
    if (col >= N)
        return true
    
    for (row = 0; row < N; row++) {
        if (isSafe(board, row, col, N)) {
            board[row][col] = 1
            if (solveNQ(board, col + 1, N))
                return true
            board[row][col] = 0
        }
    }
    return false
}
```

### Q5) 
#### a) Amortized Analysis and Aggregate Method [9 marks]

Amortized Analysis:
- Average performance of each operation in worst-case sequence of operations
- Differs from average-case analysis as it guarantees the average performance of each operation in the worst case

Aggregate Method:
1. Calculate total cost T(n) of sequence of n operations
2. Divide by n to obtain amortized cost per operation
3. Amortized cost = T(n)/n

Example: Dynamic Array Resizing
- Initial size: 1
- Double size when full
- Insert n elements:
  - Resizing costs: 1 + 2 + 4 + 8 + ... + n/2
  - Total cost = O(n)
  - Amortized cost per operation = O(1)

#### b) Potential Function Method [9 marks]

Potential Function Method:
- Φ(Di) represents potential of data structure after i operations
- amortized_cost = actual_cost + Φ(Di) - Φ(Di-1)

For Stack Operations:

PUSH:
- Actual cost: O(1)
- Potential change: +1
- Amortized cost = 1 + 1 = 2

POP:
- Actual cost: O(1)
- Potential change: -1
- Amortized cost = 1 - 1 = 0

MULTIPOP(k):
- Actual cost: min(k, s) where s is stack size
- Potential change: -min(k, s)
- Amortized cost = min(k, s) - min(k, s) = 0

### Q7) 
#### a) i) Distributed Minimum Spanning Tree [5 marks]

Algorithm (GHS Algorithm):
1. Each node starts as a fragment
2. Each fragment finds its minimum weight outgoing edge
3. Fragments merge along minimum weight edges
4. Process continues until single fragment remains

Key steps:
```
1. Initialize:
   - Each node is a fragment
   - Fragment ID = node ID
   - Level = 0

2. Find minimum outgoing edge:
   - Each node finds minimum weight edge to other fragments
   - Send TEST message to neighboring nodes

3. Merge fragments:
   - Use CONNECT messages to merge fragments
   - Update fragment IDs and levels

4. Continue until no outgoing edges remain
```

#### a) ii) Rabin-Karp String Matching [5 marks]

Algorithm:
```
RabinKarpSearch(pattern P, text T, prime q):
    n = length(T)
    m = length(P)
    d = number of characters in alphabet
    h = d^(m-1) mod q
    p = 0 // hash value for pattern
    t = 0 // hash value for text window
    
    // Calculate hash value for pattern and first window
    for i = 0 to m-1:
        p = (d*p + P[i]) mod q
        t = (d*t + T[i]) mod q
        
    // Slide pattern over text
    for i = 0 to n-m:
        if p == t:
            check characters
        if i < n-m:
            t = (d*(t - T[i]*h) + T[i+m]) mod q
```

#### b) Multithreaded Algorithms [7 marks]

1. Parallel Loops:
```
#pragma omp parallel for
for (int i = 0; i < n; i++) {
    // parallel work
}
```

2. Race Conditions:
- Prevention methods:
  - Mutex locks
  - Atomic operations
  - Critical sections
```
#pragma omp critical
{
    // critical section
}
```

3. Performance Analysis:
- Speedup = T1/Tp
  where T1 = sequential time
        Tp = parallel time with p processors
- Efficiency = Speedup/p
- Overhead = pTp - T1
