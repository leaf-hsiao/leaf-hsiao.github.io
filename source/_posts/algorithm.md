---
title: 算法笔记
date: 2019-10-16 02:01:47
tags: Note
categories: Cheetsheet
---
正好有人借593的笔记，稍微整理了一下=-=（还是可能有些许错误的）
<!-- more -->

## Analysis of algorithms

$O(n)$

```cpp
for(int i = 0; i < n; i++){//O(n)
    f();//O(1)
}

//O(n)
for (int i = 0; i < n; i++) {
}
for (int i = 5; i < n; i++) {
}

//O(n)
for (int i = 5; i < n; i+=7) {
}

for i <- 1 to sqrt(n) //O(sqrt(n))

for i <- sqrt(n) to n //O(n)
    
for i <- n to n/2 //O(n)

for i <- n to n step n/2 //O(1)

1 < logn < sqrt(n) < n < n^2

for (int i = 1; i < n; i *= 3) { } //O(logn)
for (int i = 1; i < n; i = i * 3 + 8) { }//O(logn)
for (int i = 1; i < n; i *= 2) { // 1, 2, 4, 8, ….  log2(n) 
}
for (int i = 1; i < n; i *= 3) { // 1, 3, 9, 27, ….  log3(n)
}
for (int i = 1; i < sqrt(n); i *= 3 {}  //O(log sqrt(n))

//O(n*n) = O(n^2)
for (int i = 0; i < n; i++)  //O(n)
  for (int j = 0; j < n; j++) { //O(n)
  }
}

//O(n+n) = O(n)
for (int i = 0; i < n; i++)  //O(n)
}
for (int j = 0; j < n; j++) { //O(n)
}

//sum(1...n)=O(n^2)
for (int i = 0; i <= n; i++)  //O(n)
  for (int j = 0; j <= i; j++) { // 1 + 2 + 3 + … + n-1 + n 
  }
}

//O(n*sqrt(n))
for (int i = 0; i <= n; i++) { //O(n)
  for (int j = 0; j < sqrt(n); j++) { //O(sqrt(n))
  }
}

for (int i = 0; i <= n; i++) {  //O(n)
  for (int j = 0; j < n-sqrt(n); j++) { 
  }
}

//O(n log2n)
for (int i = 0; i < n; i++) { // O(n)
 for (int j = 1; j < n; j *= 2) { // O(log2n)
 }
}

//n(1+1/2+1/4+1/8+...)
//\sum(1/2^i)=1
//O(n)
for (int i = 1; i <= n; i *= 2) { // O(log2n)
 for (int j = 1; j < i; j++) { // O(1 + 2 + 4 + 8 + …. + n)
 }
}

//O(n+m) if n ≈ m  O(2n) = O(n)
for (int i = 0; i < n; i++) {  //O(n)
}
for (int j = 0; j < m; j++) {  //O(m)
    cout << i * j;	//O(1)
}

for (int i = 1; i < n; i *= 2)	//O(log n)
  for (int j = 1; j < n; j++)	//O(n)
    System.out.println(i*j);

for (int j = 1; j < n; j++)	//O(n)
  for (int i = 1; i < n; i *= 2)	//O(log n)
    System.out.println(i*j);

for (int i = 0; i < n; i++)  //O(n)
  for (int j = i; j < n; j++)	// n + (n-1) + (n-2) + … 1
    print(“x”);

sum ← 912851;  //O(1)
for i ← 1 to n		
   for j← 3 to i		//   0 + 0 + 1 + 2 + 3 +  … n-3 = O(n^2)
      sum ← sum + i*j;
   end
end

int f(int n) { 
  if (n <= 0)//O(1)
    return 1;
  return n * f(n-1);
}
f(5)=5*f(4)=5*4*f(3)...
```



```cpp
//Fibo
int fibo(int n) { //O(n)
  int a = 1, b = 1, c;
  for (int i = 0; i < n; i++) {
     c = a + b;
     a = b;
     b = c;
  }
   return c;
}

int fibo2(int n) { //O(2^n)
   if ( n <= 2)
      return 1;
   return fibo2(n-1) + fibo2(n-2);
}
```



## Sorting 

### Selection Sort

```cpp
void selectionSort(int x[], int n) { //O(n2)
	for (int j = n - 1; j > 0; j--) {
		int pos = 0;
	  	for (int i = 1; i <= j; i++) {
	    	if (x[i] > x[pos])
	      		pos = i;
	  	}
		if (j != pos)
	    	swap(x[pos], x[j]);
	}
}
```

### Insertion Sort

```cpp
void InsertionSort(int x[], int n) {
	for (int i = 1; i < n; i++) {
		int temp = x[i];
		for (int j = i - 1; j >= 0; j--)
	    	if (x[j] > temp)
	        	x[j+1] = x[j];
	     	else {
	         	x[j+1] = temp;
	        	 break;
	       	}
	     }
	     x[0] = temp; //TODO: BAD edge case here
	}
}
```

worst case 1 + 2 + … + (n - 1) = $O(n^2)$

best case 1 + 1 + … + 1 = $\Omega(n)$

### Quick Sort

1. pick a pivot(a value in the middle)

2. 1. pivot ← (x[left] + x[right]) / 2
   2. pivot ← (x[left] + x[right] + x[(left+right)/2]) / 3
   3. pivot ← x[random] random is unpredictable  

3. Do not use…

4. 1. pivot ← x[left]
   2. pivot ← x[right]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          

```cpp
quicksort(int[] x, int left, int right) {
	if (right - left < 2)
		return;
	int pivot = (x[left] + x[right])/2;
	int i = left, j = right;
	
	while (i < j) {        //O(n)
        while (x[i] < pivot)
        	i++;
	    while (x[j] >= pivot)
	        j--;
	    if (i < j) {
	    	swap(x[i], x[j]);
	    }
	 }
	 //i == j
	 quicksort(x, left, i-1 );  // logn
	 quicksort(x, i, right );
}

quicksort(x, 0, x.length-1);
```

Worst case (pathological) $O(n^2)$

### Heap Sort

```cpp
makeheap(x)     
	for i ← n/2 to 0
	    makesubheap(x, i)
	end

makesubheap(x, i)
	if x[2i+1] > x[2i+2]
	    if x[i] < x[2i+1]
	    	swap(x[i], x[2i+1])
	    	makesubheap(x, 2i+1)
	    end
	    else
	    if x[i] < x[2i+2]
	        swap(x[i], x[2i+2])
	    	makesubheap(x, 2i+2)
	    end
	end

heapsort(x)
	makeheap(x)
	for i ← n-1 to 1
	    swap(x[0], x[i])
	    makesubheap(x, i-1)
	end
end
//makeheap = (n/2-1) log n + rebuild heap (n-1) log n = O(n log n)
```

### Merge Sort

work each time: n

number of iterations log n

O(n log n)

```cpp
// merge lists a,b of size n into list c of size 2n
merge(a, b, c) {
  i ← 0 // position in a
  j ← 0 // position in b
  k ← 0 // position in c
  while i < n and j < n
      if a[i] < b[j]
             c[k] ← a[i]
             i ← i + 1
      else
             c[k] ← b[j]
             j ← j + 1
      end
      k ← k + 1
  end
}
```

## Shuffling

 ```cpp
Fisher-Yates(x) // O(N)
    for i← x.length-1 to 0
        pick ← random(0, i)
        swap(x[i], x[pick])
    end
end
 ```

## Searching

### Linear Search

search(x, target)

$O(n),\Omega(1), avg = n/2$

### Binary Search

sort:O(n log n)

```cpp
binarySearch(x, target)
  L ← 0
  R ← x.length-1
  while (L < R)
      mid ← (L + R) / 2
      if x[mid] > target
        R ← mid - 1
      else if x[mid] < target
        L ← mid + 1
      else
        return mid
  end
  return NOTFOUND
end
```

### Golden Mean Search

L = 0

R = 19

S = (R-L) /  = 19 * .6 = 11.4 = 



function f(x) = 4 - x2

L = -2        R = 1        D = R-L = 3

S = (R-L)/ 1.618 = 1.85

X = R - 1.85 = -.85

Y = L + 1.85 = -.15

L = -.85

S =(R-L)/ 1.618  = 1.143

X  = -.15

Y = L + S = -.85 + 1.143 = .293

R = .293

## Number Theoretic Algorithms

### gcd

```cpp
gcd(a,b)
    if b == 0
        return a
    end
    return gcd(b, a mod b)
end
```

$O(logn),\Omega(1)$

### lcm

```cpp
//O(log n)
lcm(a,b) = a * b / gcd(a,b)  // 1 + log(n)
```

### powermod

```cpp
//raise x to the power n
bruteforcepower(x, n)
    prod ← 1
    for x ← 1 to n
        prod ← prod * x
    end
    return prod
end

//O(log n)
//raise x to the power n
power(x, n)
  prod ← 1
  while n > 0
    if (n mod 2 != 0)
      prod ← prod * x
    end 
    x ← x * x
    n ← n / 2
  end
  return prod
end

//xn mod m
powermod(x, n, m)
  prod ← 1
  while n > 0
    if (n mod 2 != 0)
      prod ← prod * x mod m
    end
    x ← x * x mod m
    n ← n / 2
  end
  return prod
end
```

## Fermat

if a^p-1 mod p == 1⇒ p is probably prime

if a^p-1 mod p != 1⇒ p is definitely NOT prime

## Miller-Rabin

```cpp
MillerRabin(n, k)
  WitnessLoop: for i ← 1 to k // k trials
    a← random[2, n − 2]
    x ← a^d mod n
   if x = 1 or x = n − 1 then
      continue WitnessLoop
   for j ← 1 to s − 1
      x ← x2 mod n
      if x = 1 then
         return false composite  (Carmichael?)
      if x = n − 1 then
         continue WitnessLoop
    end
   return false (composite)
return probably prime

```

### brute force trial division

```
//O(n)		Ω(1)
bool isPrime(int n) {
  for (int i = 2; i < n/2; i++) {
    if (n % i == 0)
      return false;
  }
  return true;
}

//O(√n)		Ω(1)
bool isPrime(int n) {
  for (int i = 2; i <= sqrt(n); i++) {
    if (n % i == 0)
      return false;
  }
  return true;
}
```

### Eratosthenes' Sieve

```
ImprovedEratosthenes(n)
for i ← 2 to n
	isPrime[i] ← true
end
for i ← 4 to n step 2
	isPrime[i] ← false
end
for i ← 3 to n step 2
	if isPrime[i]
		for j ← i*i to n step 2i
			isPrime[j] ← false
		end
	end
end
```

## GrowArray

```cpp
class GrowArray {
private:
    int* data;
    int len;
    int capacity;
    void grow(){
        int* old = data;
        data = new int[2*capacity + 1];
        for (int i = 0; i < len; i++) // memcpy(data, old, len* sizeof(int))
            data[i] = old[i];
        delete old;
        capacity = 2 * capacity;
    }
public:
    GrowArray() { data = nullptr; len = 0; capacity = 0;}
    void addEnd(int v) { // O(1)
        if (len >= capacity)
            grow();
        data[len++] = v;
        cout<<v<<endl;
    }
};
 
int main() {
const int n = 1000000;
GrowArray a;
for (int i = 0; i < n; i++)
    a.addEnd(i);
}
```

## Linkedlist

```cpp
	class LinkedList {
	private:
	    class Node {
	        int val;
	        Node* next;
	        Node* prev;
	    };
	    Node* head;
	    Node* tail;
	public:
	    LinkedList() : head(nullptr), tail(nullptr) {} // O(1)
	    ~LinkedList() { // O(1)
	        while (head != nullptr) {
	            Node* temp = head;
	            head = head->next;
	            delete temp;
	        }
	    }
	    void addFirst(int v) { // O(1)
	        head = new Node {v, head, nullptr};
	        if (tail == null)
	            tail = head;
	        else
	            head->next->prev = head;
	    }
	    void addLast(int v) {
	        tail = new Node(v, nullptr, tail);
	        if (head == nullptr)
	            head = tail;
	        else
	            tail->prev->next = tail;
	    }
	    void removeFirst() {
	        Node* temp = head;
	        head = head->next;
	        delete temp;
	    }
	}

```

### Stack

```cpp
class Stack {
private:
    LinkedList impl; //  addFirst O(1), addLast O(n), removeFirst O(1), removeLast O(n)
    GrowArray impl; //  addFirst O(n) addLast O(1), removeFirst O(n), removeLast O(1)
public:
    void push(int v) { impl.addFirst(v); }
    int pop() { return impl.removeFirst()}
    bool isEmpty() const {}
    int size( const {})
};

```

### Queue

```cpp
class Queue {
private:
    linkedList 2 templ; //  has head and tail
public:
    void enqueue(int v) {impl.addLast(v)} //  O(1)
    int dequeue() { impl.removeFirst()} //  O(1)
    bool isEmpty() const { return impl,isEmpty();}
}
```



## Trees

### Binary tree

```cpp
class BinaryTree {
private:
  class Node {
  public:
    int val;
    Node* parent;
    Node* left;                // int* a,b; a is pointer to int, b is just int
    Node* right;
    Node(int v, Node* L, Node* r) : val(v), left(L), right(R) {}
  };
  Node * root;
public:
  void add(int v) {
    if (root == nullptr) {
      root = new Node(v, nullptr, nullptr);
      return;
    }
  … }

inorder(node n)
    if n == null
        return
	inorder(n.left)
    print (val)
    inorder(n.right)
end

preorder(node n)
    if n == null
        return
    print (val)
	preorder(n.left)
    preorder(n.right)
end

postorder(node n)
    if n == null
        return
	postorder(n.left)
    postorder(n.right)
    print (val)
end
```

### Trie

```cpp
class Trie{
private:
    class Node{
        Node* next[26];
        bool isWord;
    }
    Node root;
public:
    Trie() {}// set up the empty trie root contains all null pointers 
    ~Trie() {
        Node* p = root;
        for (int i = 0; i < 26; i++)
            if (p->next[i] != nullptr)
                deleteNode(p->next[i]);
    }
    
    void insert(String word) {
        Node* temp = root;
        for (int i = 0; i < word.size; i++)
            int index = word[i] - 'a';
            if temp->next[index] == nullptr
                temp->next[index] = new Node();
        	temp = temp->next[index];
        temp->isWord = true;
    }
    
    bool containsPrefix(String word) {
        Node* temp = root;
        for (i = 0; i < word.size; i++) {
            int index = word[i] - 'a';
            if (temp->next[index] == nullptr)
                return false;
            temp = temp->next[index];
        }
        return true;
    }
    bool contains(String word) {
        Node* temp = root;
        for (i = 0; to word.size) {
            int index = word[i] - 'a';
            if (temp->next[index] == nullptr)
                return false;
            temp = temp->next[index];
        }
        return temp->isWord;
    }
}
}
```

## Hashing

### Hash functions

### Linear probing

> Linear probing is a strategy for resolving collisions, by placing the new key into the closest following empty cell.

```pseudocode
add(k)
    if 2*used >= capacity
        grow()
    pos ← hash(k)
    while table[pos] != EMPTY
        pos++
        if pos >= table.length
            pos = 0
        end
    end
    used++
    table[pos] ← k
end

find(k)
	pos ← hash(k)
	while table[pos] != EMPTY
		if table[pos] == k
			return true
		end
		pos++
		if pos >= table.length
			pos = 0
		end
	end
	return false
end

bool ← remove(k)
	pos ← hash(k)
	while table[pos] != 0
		if table[pos] == k
			table[pos] ← 0
				// redo everyone potential
				// until next 0
			return
		end
		pos++
		if pos >= table.length
			pos = 0
		end
	end
	return false
end
```

### Quadratic probing*

### Linear chaining

```pseudocode
LinearChaining.add(k)
	pos ← hash(k)
	p ← table[pos].head
	while p != null
		if p.k == k
			return
		p = p.next
	end
	table[pos].addStart(k)
end

LinearChaning.find(k)
	pos ← hash(k)
	p ← table[pos].head
	while p!= null
		if p.k == k
			return true
		p = p.next
	end
	return false
end		
```

### Perfect hashing

## Matrices

### Representation (rectangular, diagonal, tridiagonal)

```cpp
class Matrix{
private:
    int row,cols;
    double* m;
public:
    Matrix(int rows, int cols, double val) : rows(rows), cols(cols){
        for (int i = 0; i < row * cols; i++)
            m[i] = val;
    }
    ~Matrix(){
        delete []m
    }
}
```

```pseudocode
\\Lower Triangular Matrix
get(i, j)
	if(j > i)
		return 0
	return m[i*(i+1)/2+j]

\\Diagonal Matrix
get(i, j)
	if(i == j)
		return m[i]
	return 0
	
\\TriDiagonal Matrix
get(i, j)
	if(abs(j-i) > 1)
		return 0;
	return m[2i+j]
```

### Addition

```pseudocode
addition(a, b)
	if a.rows != b.rows OR a.cols != b.cols
		ERROR
	end
	for i ← 0 to rows*cols
		ans[i] = a[i] + b[i]
	end
	return ans
end
```

### Multiplication

```ps
mult(a, b)
	if a.cols != b.rows
		ERROR
	end
	for k ← 0 to a.rows-1
		for i  ←  0 to b.cols - 1
			sum ← 0
			for j ← 0 to a.cols - 1
				sum ← sum + a(k,j)*b(j,i)
			end
			ans(k,i) ← sum
		end
	end
end
```

### Solving systems (Gauss-Jordan Elimination, partial pivoting)

```pseudocode
partialPivot(A,i)
	max ← A(i,i)
	maxpos ← i
	for j ← i+1 to rows-1
		if A(j,i) > max
			max ← A(j,i)
			maxpos ← j
		end
	end
	//swap rows
GaussianElimination(A,i)
	for i ← 0 to rows-2
		partialPivot(A,i)
		for j ← i+1 to rows-1
			s ← - A(j,i)/A(i,i)
			A(j,i) ← 0
			for k ← i+1 to cols - 1
				A(j,k) += s * m(i,j)
			end
		end
	end
```

### Gram-Schmidt orthogonalization

```cpp
void subtractProj(int i, int j){
    double dot = 0;
    for (int k = 0; k < rows; k++)
        dot += (*this)(k,i) * (*this)(k,j);
    //<u.v>
    for (int k = 0; k < rows; k++)
        (*this)(k,j) -= dot * (*this)(k,j);
}
gramschmidt(){
    int n = cols;
    double mag = 0;
    for (int i = 0; i < n; i++)
        mag += (*this)(i,0) * (*this)(i,0);
    mag = 1 / sqrt(mag);
    for (int i = 0; i < n; i++)
        (*this)(i,0) *= mag;
    for(int j = 1; j < n; j++){
        for (int i = 0; i < j; i++)
            subtractProj(i,j);
        double mag = 0;
        for (int i = 0; i < n; i++)
            mag += (*this)(i,j) * (*this)(i,j);
        mag = 1/sqrt(mag);
        for (int i = 0; i < n; i++)
            (*this)(i,j) *= mag;
    }
    return *this;  
}
```

## Strings

### Boyer-Moore

```c++
void boyerMoore(const char s[], const char target[]){
    int jump[256];
    int n = strlen(s);
    int len = strlen(target);
    for (int i = 0; i < 256; i++)
        jump[i] = len;
    for (int i = 0; i < len; i++)
        jump[target[i]] = len - 1 - i;
    for (int i = len - 1; i<n;){
        int offset = jump[s[i]];
        if(offset > 0)
            i += offset;
        else{
            for (int j = i - len +1, k = 0; j < i; j++, k++)
                if(target[k] != s[j])
                    goto notfound;
            cout << "Found: " < i-len +1<<endl;
        notfound:
            i+=len;
        }
}
    
for i =0 to 255
    table[i] = len
for i = 0 to len
    table[target[i]] = len - i - 1
i = len-1
while (i < n)
    jump ← table[search[i]]
    if (jump == 0) { // possible match
        // compare the word letter by letter

    }
    i ← i + jump
end
```

### Finite State*

```c++
class FiniteStateMachine {
private:
	int currentState;
	int next[NUMSTATES][256];
public:
	FiniteStateMachine() : currentState(0) {
		for (int i = 0; i < 256; i++)
			next[0][i] = 1;
		for (int i = 'a'; i <= 'z'; i++)
			next[0][i] = 0;
		for (int i = 'A'; i <= 'Z'; i++)
			next[0][i] = 0;
		

	}
	void exec(const char inp[]) {
		for (int i = 0; inp[i] != '\0'; i++) {
			currentState = next[currentState][inp[i]];
			if (currentState == 1)
				return; // get out when we reach the end state
	}
};
```

### LCS

```c++
int LCS(string s, string t){
    int m = s.size();
    int n = t.size();
    int dp[m+1][n+1];
    for(int i = 0; i < m + 1; i++){
        for(int j = 0; j < n + 1; j++){
            if(i==0||j==0){
                d[i][j]=0;
            }
            else if(s[i-1]==t[j-1]){
                dp[i][j] = dp[i-1][j-1] + 1;
            }else{
                dp[i][j] = max(dp[i-1][j],dp[i][j-1]);
            }
        }
    }
    return dp[m][n];
}
```



## Graphs

### Representation: list or matrix

![img](https://docs.google.com/drawings/d/sWyhI6FrcgmwhtdawJFsCZg/image?w=624&h=107&rev=90&ac=1&parent=1QRQ5hBbBiKXkq0xePaM74Dbq7uui8nGUhUarYntk3wY)

find out if v1 is adjacent to v2 = O(E) = O($V^2$)        Ω(1)

find out the set of vertices connected to v is O(E)    

Storage: O(E)  Ω(1)

![img](https://docs.google.com/drawings/d/szZxUtrrzDPlGnxXMe6Hl3Q/image?w=199&h=284&rev=37&ac=1&parent=1QRQ5hBbBiKXkq0xePaM74Dbq7uui8nGUhUarYntk3wY)

Storage: O(E)   Ω(V)

is vertex k adjacent to vertex m?  O(V)     Ω(1)

find all vertices adjacent to vertex k?  O(V)   Ω(1)

```pseudocode
isAdjacent(from,to)   //O(V)
allAdjacent(from) //O(V), Ω(1) 
```



![img](https://docs.google.com/drawings/d/s_S39ZP6f8cbZoeQk9bNpbQ/image?w=201&h=178&rev=49&ac=1&parent=1QRQ5hBbBiKXkq0xePaM74Dbq7uui8nGUhUarYntk3wY)

Space complexity: O(E) = O(V2)      Ω(V2)        = Θ(V2)

is vertex k adjacent to vertex m?  O(1)

find all vertices adjacent to k? O(V)

```pseudocode
isAdjacent(from,to)   //O(1)
if from < to
        return isAdajacent(to, from)
return m[from][to]
end

allAdjacent(from) //O(V), Ω(V)  ⇒ θ(V)
```

```c++
class MatrixGraph {
  private double [] w;
  private int V;
  public boolean isAdjacent(int i, int j) { return !isInf(w[i * V + j]); } //O(1)
  public boolean[] adjacent(int v) { … } // O(V) Ω(V)
}

class EdgeListGraph {
  private static class Edge {
     int to;
     double w;
  }
  private LinkedList<Edge>[] v;
  public boolean isAdjacent(int i, int j) { … } //O(V)    Ω(1)
  public boolean[] adjacent(int i) { … } // O(V)        Ω(1)
}

```



### DFS

O(V2) worst case (matrix representation, ALWAYS)

Ω(1) best case (with initial vertex not connected to anyone, list)

Ω(V) best case (with initial vertex not connected to anyone, matrix)

```pseudocode
g.DFS2(v,visited) //O(V^2) Ω(1) with list impl
	visited[v] ← true
	print v
	foreach v2 in adjacent(v)// for matrix O(v), for linkedlist O(v)  Ω(1)
		if NOT visited[v2]
			g.DFS2(v2, visited)
		end
	end
end
g.DFS(v)
	visited ← [false, false, ...,] //O(v)
	g.DFS2(v,visited)
end

//iteratively
g.DFS(v)
	visited ← [false, false, ...,] //O(v)
	stack ← empty
	stack.push(v)
	visited[v] ← true
	while NOT stack.empty()
		v ← stack.pop();
		print v
		foreach v2 in adjacent(v)
			if NOT visited[v2]
				stack.push(v2)
				visited[v2] ← true
			end
		end
	end
end
```

### BFS

```pseudocode
g.BFS(v)
	visited ← [false, false, ...,] //O(v)
	queue ← empty
	queue.enqueue(v)
	visited[v] ← true
	while NOT queue.empty()
		v ← queue.dequeue();
		print v
		foreach v2 in adjacent(v)
			if NOT visited[v2]
				queue.enqueue(v2)
				visited[v2] ← true
			end
		end
	end
end
```



### IsConnected

```pseudocode
bool ← g.isConnected() //O(V^2)
	visited ← g.DFS(r)
	for i ← 0 to V
		if NOT visited[i]
			return false
	end
	return true
end
```



### Bellman-Ford//O(VE)

```pseudocode
BellmanFord(start, end)
	cost ← [∞, ∞, ...]
	cost[start] ← 0
	for v ← 0 to V-1
		for each edge e from v
			if cost[v2] > cost[v] + adjacent[v2]
			cost[v2] ← cost[v] + adjacent[v2]
			add to the list to recompute costs from v2
		end
	end
end
```

### Floyd-Warshall//O(V^3)

```pseudocode
dist ← |V| × |V| array of minimum distances initialized to ∞

for each vertex v
	dist[v][v] ← 0
for each edge (u,v)
	dist[u][v] ← w(u,v) //the weight of the edge (u,v)
for k from 1 to V
	for i from 1 to V
		for j from 1 to V
			if dist[i][j] > dist[i][k] + dist[k][j]
				dist[i][j] ← dist[i][k] + dist[k][j]
			end
		end
	end
end
```

### Prim//O(V^2)

```pseudocode
Prim(v)
	visited ← new Vector(V, false)
	visited[v] ← true
	for i ← 1 to V-1
		for all edges out of the visited set(V,2V,3V, ...) to V-1
			min ← ∞
			if isAdjacent(v,a) and NOT visited[a]
				if getWeight(v,a) < min
					min ← getWeight(v,a)
					minA ← a
				end
			end
		end
		visited[minA] ← trme
		v ← minA
	end
end
```

### Kruskal//O(E log E)

https://en.wikipedia.org/wiki/Kruskal%27s_algorithm

### TSP*

## Backtracking

### Basic

### Heap's

```cpp
#include <iostream>
using namespace std;

class Permute {
private:
	int* p;
	int N;
public:
	Permute(int x[], int N) : p(x), N(N) {
		//generate(N-1);
		heaps(N-1);
	}
	void generate(int n) {
		if (n == 0) {
			doit();
			return; // What Sedgewick forgot!!!!
		}
		for (int c = 0; c <= n; c++) {
			swap(p[c], p[n]);
			generate(n-1);
			swap(p[c], p[n]);
		}
	}
	void heaps(int n) {
		if (n == 0) {
			doit();
			return; // What Sedgewick forgot!!!!
		}
		for (int c = 0; c <= n; c++) {
			generate(n-1);
			swap(n % 2 ? p[0] : p[c] , p[n]);
		}
	}

	void doit() const {
		for (int i = 0; i < N; i++)
			cout << p[i];
		cout << '\n';
	}
};


int main() {
  int x[] = {1, 2, 3, 4};
	Permute perm(x, 4);
}
```

