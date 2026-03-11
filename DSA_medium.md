## [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters)
### 1 способ:

- при помощи скользящего окна и вспомогательного массива где будем указывать последний индекс встречающегося символа

```go
func lengthOfLongestSubstring(s string) int {
	seen := make([]int, 128)
	start, result := 0, 0
	for end := 0; end < len(s); end++ {
		char := s[end]
		start = max(start, seen[char])
		result = max(result, end-start+1)
		seen[char] = end + 1
	}
	
	return result
}
```

time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/description)

### 1 способ:

- сначала проверяем что у нас строка по которой будем искать аннаграмы длиннее чем строка паттерн, затем создаем два массива которые будут иметь длину равную количеству символов, и заполняем наш паттерн через разность с символом 'a' который дает индекс латинской буквы в массиве, и так же заполняем первое окно нашей строки под проверку, затем бежим циклом окном проверяем эти массивы если они равны значит индекс подходит, если нет то идем дальше и проверяем что у нас не зашли за границу строки и меняем положение окна.

```go
func findAnagrams(s string, p string) []int {
	if len(s) < len(p) { return nil }
	
	var pattern, memory [26]int
	result := make([]int, 0)
	
	for i := range p {
		pattern[p[i] - 'a']++
		memory[s[i] - 'a']++
	}
	
	for i := 0; i < len(s)-len(p)+1; i++ {
		if pattern == memory {
			result = append(result, i)
		}
		if i + len(p) < len(s) {
			memory[s[i] - 'a']--
			memory[s[i+len(p)] - 'a']++
		}
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Permutation in String](https://leetcode.com/problems/permutation-in-string)

### 1 способ:

- та такой же способ что и в предыдущей задаче, через скользящее окно и вспомогательные массивы

```go
func checkInclusion(s1 string, s2 string) bool {
	if len(s2) < len(s1) { return false }
	
	var pattern, memory [26]int
	
	for i := range s1 {
		pattern[s1[i] - 'a']++
		memory[s2[i] - 'a']++
	}
	
	for i := 0; i < len(s2) - len(s1) + 1; i++ {
		if pattern == memory { return true }
		if i + len(s1) < len(s2) {
			memory[s2[i] - 'a']--
			memory[s2[i + len(s1)] - 'a']++
		}
	}
	
	return false
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Max Consecutive Ones |||](https://leetcode.com/problems/max-consecutive-ones-iii)

### 1 способ:

- скользящим окном, пока k достаточно на нулей, то увеличиваем окно, когда недостаточно то левую границу перебираем

```go
func longestOnes(nums []int, k int) int {
	l, zeros, result := 0, 0, 0
	
	for r := 0; r < len(nums); r++ {
		if nums[r] == 0 {
			zeros++
		}
		
		for k < zeros {
			if nums[l] == 0 {
				zeros--
			}
			l++
		}
		
		result = max(result, r - l  + 1)
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Longest Subarray of 1's After Deleting One Element](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/description)

### 1 способ:

- точно такой же способ при помощи скользящего окна

```go
func longestSubarray(nums []int) int {
	l, k, result := 0, 0, 0
	
	for r := 0; r < len(nums); r++ {
		if nums[r] == 0 {
			k ++
		}
		if k > 1 {
			if nums[l] == 0 {
				k--
			}
			l++
		}
		
		result = max(result, r - l + 1)
	}
	
	return result - 1
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement)

### 1 способ:

- скользящее окно и вспомогательный массив, мы узнаем счетчик максимально повторяющегося элемента, затем если оно  у нас превышает лимит то сдвигаем левую рамку окна

```go
func characterReplacement(s string, k int) int {
	seen := make(map[byte]int)
	l, result, maxFreq := 0, 0, 0
	for r := 0; r < len(s); r++ {
		seen[s[r]]++
		maxFreq = max(maxFreq, seen[s[r]])
		
		if (r-l+1) - maxFreq > k {
			seen[s[l]]--
			l++
		}
		
		result = max(result, r - l + 1)
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Subarray sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k)

### 1 способ: 

- при помощи метода префиксных сумм и хеш-мапой в которой мы будем хранить все состояния нашей префиксной суммы, если у нас разность преф суммы на какой-то итерации и наше искомое число равны, значит добавляем к нашему результату количество таких сумм

```go
func subarraySum(nums []int, k int) int {
	seen := make(map[int]int)
	seen[0] = 1
	result, prefixSum := 0, 0
	
	for _, num := range nums {
		prefixSum += num
		if counter, ok := seen[prefixSum - k]; ok {
			result += counter
		}
		seen[prefixSum]++
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum)

### 1 способ: 

- при помощи метода префиксных сумм записывать в хеш-мапу остатки префиксной суммы в мапу и затем проверять что разница индексов более 2

```go
func checkSubarraySum(nums []int, k int) bool {
	seen := make(map[int]int)
	seen[0] = -1
	prefixSum := 0
	
	for i, num := range nums {
		prefixSum += num
		if value, ok := seen[prefixSum % k]; ok {
			if i - value >= 2 {
				return true
			}
		} else {
			seen[prefixSum % k] = i
		}
	}
	
	return false
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/description)

### 1 способ:

-  при помощи петода префиксных сумм мы сохраняем сначала первым циклом произведения всех элементов слева, а затем проходимся вторым циклом с конца до начала и дополняем произведениями элементов справа

```go
func productExceptSelf(nums []int) []int {
	result := make([]int, len(nums))
	helper := 1
	
	for i := 0; i < len(nums); i++ {
		result[i] = helper
		helper *= nums[i]
	}
	
	helper = 1
	
	for i := len(nums) - 1; i >= 0; i-- {
		result[i] *= helper
		helper *= nums[i]
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$
extra space complexity:$$ O(1) $$

---
## [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/description)

### 1 способ:

- просто проходим по массиву и сохраняем префиксную сумму, если она больше нашего изначального максимум, то обновляем максимум. Если префикс сумма стала меньше нуля, то смысла ее нести за собой нет, мы ее обнуляем

```go
func maxSubArray(nums []int) int {
	result, prefixSum := nums[0], 0
	
	for _, num := range nums {
		prefixSum += num
		result = max(result, prefixSum)
		if prefixSum < 0 { prefixSum = 0 }
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/description)

### 1 способ:

- короче завести три переменные, максимальное произведение, минимальное произведение и наш ответ, идти по массиву и сохранять если у нас произведение меняется

```go
func maxProduct(nums []int) int {
	productMax, productMin, result := nums[0], nums[0], nums[0]
	
	for i := 1; i < len(nums); i++ {
		num := nums[i]
		templateMax := max(num, max(num*productMax, num*productMin))
		productMin = min(num, min(num*productMax, num*productMin))
		productMax = templateMax
		result = max(result, productMax)
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

## [Container With Most Water](https://leetcode.com/problems/container-with-most-water/description)

### 1 способ:

- двумя указателями сужаем окно, за объем берем произведение расстояния между колоннами и высотой минимальной стены, а затем двигаем внутрь указатель от меньшей стены и обновляем макс

```go
func maxArea(height []int) int {
	left, right, result := 0, len(height)-1, 0
	
	for left < right {
		minHeight := min(height[left], height[right])
		area := minHeight * (right - left)
		result = max(result, area)
		
		if height[left] < height[right] {
			left++
		} else {
			right--
		}
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Partition Labels](https://leetcode.com/problems/partition-labels)

### 1 способ: 

- сохраняем последний индекс каждого символа в мапе, затем второй раз пробегаемся и раздвигаем окно до последнего символа если i == end то сохраняем в наш массив, и передвигаем левый указатель в самый край права

```go
func partitionLabels(s string) []int {
	seen := make(map[rune]int)
	
	for i, char := range s {
		seen[char] = i
	}
	
	result := make([]int, 0)
	start, end := 0, 0
	
	for i, char := range s {
		end = max(end, seen[char])
		if i == end {
			result = append(result, end-start+1)
			start = i+1
		}
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [String Compression](https://leetcode.com/problems/string-compression)

### 1 способ: 

- мы создаем несколько указателей, пробегаемся по строке и считаем сколько каких у нас символов, как только досчитали то если просто один символ то просто добавляем, если же нет, то еще пробегаемся циклом по нашему счетчику и записываем все цифры в конце возвращаем длину строки

```go
func compress(chars []byte) int {
	i, index := 0, 0
	for i < len(chars) {
		char := chars[i]
		count := 0
		for i < len(chars) && chars[i] == char {
			count++
			i++
		}
		if count == 1 {
			chars[index] = char
			index++
		} else {
			chars[index] = char
			index++
			for _, digit := range []byte(strconv.Itoa(count)) {
				chars[index] = digit
				index++
			}
		}
	}
	
	return index
}
```
time complexity:$$ O(n) $$
space complexity:$$ o(1) $$

---
## [Jump Game](https://leetcode.com/problems/jump-game)

### 1 способ: 

- здесь мы жадным алгоритмом считаем наш максимум по прыжкам и затем если мы дошли до момента что до текущего индекса нельзя допрыгнуть макс меньше то возвращаем фолс

```go
func canJump(nums []int) bool {
	maxJump := 0
	for i := 0; i < len(nums); i++ {
		if i > maxJump { return false }
		maxJump = max(maxJump, nums[i]+i)
	}
	
	return true
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Maximize Distance to Closest Person](https://leetcode.com/problems/maximize-distance-to-closest-person)

### 1 способ:

- пробегаемся по всем местам и обрабатываем случаи, либо самое крайнее левое место, либо самое крайнее правое, либо ровно посередине от чуваков

```go
func maxDistToClosest(seats []int) int {
	result, previous := 0, -1
	for i := 0; i < len(seats); i++ {
		if seats[i] == 1 {
			if previous == -1 {
				result = i
			} else {
				result = max(result, (i - previous) / 2)
			}
			previous = i
		}
	}
	result = max(result, len(seats)-previous-1)
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals)

### 1 способ: 

- сначала сортируем по концу интервалов, затем те интервалы которые начинаются до самого первого конца, удаляем и увеличиваем счетчик, а те которые идут после обновляем счетчик

```go
func eraseOverlapIntervals(intervals [][]int) int {
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][1] < intervals[j][1]
	})
	
	lastEnd := intervals[0][1]
	count := 0
	
	for i := 1; i < len(intervals); i++ {
		if intervals[i][0] < lastEnd {
			count++
		} else {
			lastEnd = intervals[i][1]
		}
	}
	
	return count
}
```
time complexity:$$ O(n \cdot log(n)) $$
space complexity:$$ O(1) $$

---
## [Merge Intervals](https://leetcode.com/problems/merge-intervals)

### 1 способ: 

- сортируем интервалы по их началу, добавляем в наш список самый первый а затем проверяем со следующим если начало после конца то добавляем в список, если нет то увеличиваем первый интервал

```go
func sortIntervals(intervals [][]int) {
	sort.Slice(intervals, func (i, j int) bool {
		return intervals[i][0] < intervals[j][0]
	})
}

func merge(intervals [][]int) [][]int {
	if len(intervals) <= 1 {
		return intervals
	}
	
	sortIntervals(intervals)
	
	result := make([][]int, 0, len(intervals))
	result = append(result, intervals[0])
	
	for _, interval := range intervals[1:] {
		if top := result[len(result)-1]; top[1] < interval[0] {
			result = append(result, interval)
		} else if interval[1] > top[1] {
			top[1] = interval[1]
		}
	}

	return result
}
```
time complexity:$$ O(n \cdot log(n)) $$
space complexity:$$ O(n) $$

---
## [Insert Interval](https://leetcode.com/problems/insert-interval/description)

### 1 способ: 

- делаем три цикла, в первом цикле добавляешь в конечный массив те интервалы которые заканчиваются до нового интервала, второй цикл проверяет если интервал начался до того как закончился новый, то проверяем и расширяем границы нового интервала этими интервалами, третий цикл добавляет оставшиеся интервалы

```go
func insert(intervals [][]int, newInterval []int) [][]int {
	result := make([][]int, len(intervals)+1)
	i := 0
	
	for i < len(intervals) && intervals[i][1] < newInterval[0] {
		result = append(result, intervals[i])
		i++
	}
	
	for i < len(intervals) && intervals[i][0] <= newInterval[1] {
		newInterval[0] = min(newInterval[0], intervals[i][0])
		newInterval[1] = max(newInterval[1], intervals[i][1])
		i++
	}
	
	result = append(result, newInterval)
	
	for i < len(intervals) {
		result = append(result, intervals[i])
		i++
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Interval List Intersections](https://leetcode.com/problems/interval-list-intersections)

### 1 способ:

-  идти по двум спискам, брать максимальный минимум от каждого и минимальный максимум от каждого, если начало меньше равно конца то добавляем этот интервал в наш результат, затем если конец равен концу одного из списков то мы передвигаем указатель

```go
func getInterval(A, B []int) (int, int) {
	start := max(A[0], B[0])
	end := min(A[1], B[1])
	return start, end
}

func intervalIntersection(firstList [][]int, secondList [][]int) [][]int {
	result := make([][]int, 0)
	
	for i, j := 0, 0; i < len(firstList) && j < len(secondList); {
		start, end := getInterval(firstList[i], secondList[j])
		
		if start <= end {
			result = append(result, []int{start, end})
		}
		if firstList[i][1] == end {
			i++
		}
		if secondList[j][1] == end {
			j++
		}
	}
	
	return result
}
```
time complexity:$$ O(n + m) $$
space complexity:$$ O(min(n, m)) $$

---
## [Group Anagrams](https://leetcode.com/problems/group-anagrams)

### 1 способ:

- заводим словарь в котором по ключу будем хранить паттерн, те слова которые удовлетворяют паттерну кладем под этот ключ в словаре

```go
func groupAnagrams(strs []string) [][]string {
	seen := map[[26]int][]string{}
	for _, str := range strs {
		key := [26]int{}
		
		for i := 0; i < len(str); i++ {
			key[str[i] - 'a']++
		}
		seen[key] = append(seen[key], str)
	}
	result := [][]string{}
	
	for _, value := range seen {
		result = append(result, value)
	}
	
	return result
}
```
time complexity:$$ O(n \cdot m) $$
space complexity:$$ O(n \cdot m) $$

---
## [Top K Frequent](https://leetcode.com/problems/top-k-frequent-elements/description)

### 1 способ: 

- создаем словарь где по значению записываем сколько раз встретилось в массиве, затем создаем двумерный массив где по индексу повтора будут лежать эти значения ну и будем идти с конца этого массива и сохранять в новый уже самые частые символы

```go
func topKFrequent(nums []int, k int) []int {
	seen := make(map[int]int)
	
	for _, num := range nums {
		seen[num]++
	}
	
	bucket := make([][]int, len(nums)+1)
	
	for key, value := range seen {
		bucket[value] = append(bucket[value], key)
	}
	
	result := make([]int, 0, k)
	for i := len(bucket) - 1; i >= 0; i-- {
		for _, num := range bucket[i] {
			if k > 0 {
				result = append(result, num)
			}
		}	
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [LRU Cache](https://leetcode.com/problems/lru-cache)

### 1 способ:

- через двусвязный список и хеш-мапу

```go
type LRUCache struct {
	Head, Tail *Node
	Mp map[int]*Node
	Capacity int
}

func newLRUCache(head, tail *Node, cap int) LRUCache {
	return LRUCache{
		Head: head,
		Tail: tail,
		Mp: make(map[int]*Node),
		Capacity: cap,
	}
}

type Node struct {
	Next, Prev *Node
	Key, Value int
}

func newNode(key, value int) *Node {
	return &Node{
		Key: key,
		Value: value,
	}
}

func Constructor(capacity int) LRUCache {
	head, tail := newNode(0, 0), newNode(0, 0)
	
	head.Next = tail
	tail.Prev = head
	
	return newLRUCache(head, tail, capacity)
}

func (this *LRUCache) Get(key int) int {
	if n, ok := this.Mp[key]; ok {
		this.remove(n)
		this.insert(n)
		return n.Value
	}
	
	return -1
}

func (this *LRUCache) Put(key int, value int) {
	if _, ok := this.Mp[key]; ok {
		this.remove(this.Mp[key])
	}
	
	if len(this.Mp) == this.Capacity {
		this.remove(this.Tail.Prev)
	}
	
	this.insert(newNode(key, value))
}

func (this *LRUCache) remove(node *Node) {
	delete(this.Mp, node.Key)
	node.Prev.Next = node.Next
	node.Next.Prev = node.Prev
}

func (this *LRUCache) insert(node *Node) {
	this.Mp[node.Key] = node
	next := this.Head.Next
	this.Head.Next = node
	node.Prev = this.Head
	
	next.Prev = node
	node.Next = next
}
```

---
## [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/description)

### 1 способ: 

- через массивчик и хеш-мапу

```go
type RandomizedSet struct {
	arr []int
	size int
	set map[int]int
}

func Constructor() RandomizedSet {
	return RandomizedSet{
		arr: make([]int, 0),
		size: 0,
		set: make(map[int]int),
	}
}

func (this *RandomizedSet) Insert(val int) bool {
	if _, ok := this.set[val]; ok {
		return false
	}
	
	this.arr = append(this.arr, val)
	this.set[val] = this.size
	this.size++
	
	return true
}

func (this *RandomizedSet) Remove(val int) bool {
	index, ok := this.set[val]
	if !ok {
		return false
	}
	
	lastElement := this.arr[this.size-1]
	this.arr[index] = lastElement
	this.set[lastElement] = index
	
	this.arr = this.arr[:this.size-1]
	delete(this.set, val)
	this.size--
	return true
}

func (this *RandomizedSet) GetRandom() int {
	index := rand.Intn(this.size)
	return this.arr[index]
}
```

---
## [Flatten Nested List Iterator](https://leetcode.com/problems/flatten-nested-list-iterator)

### 1 способ: 

- рекурсивный вызов стуктурой самой себя

```go
type NestedIterator struct {
	index int
	nestedIterator *NestedIterator
	nestedList []*NestedInteger
}

func Constructor(nestedList []*NestedInteger) *NestedIterator {
	return &NestedIterator{
		index: -1,
		nestedList: nestedList,		
	}
}

func (this *NestedIterator) Next() int {
	if nestedIterator := this.nestedIterator; nestedIterator != nil {
		return nestedIterator.Next()
	}
	
	this.nestedList[this.index].GetInteger()
}

func (this *NestedIterator) HasNext() bool {
	if nestedIterator := this.nestedIterator; nestedIterator != nil {
		if nestedIterator.HasNext() {
			return true
		}
		this.nestedIterator = nil
	}
	
	this.index++
	if this.index == len(this.nestedList) {
		return false
	}
	
	if this.nestedList[this.index].isInteger() {
		return true
	}
	
	this.nestedIterator = Constructor(this.nestedList[this.index].GetList())
	return this.HasNext()
}
```

---
##  [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

### 1 способ:

- сложение в столбик как и в sum strings с переносом остатка в уме

```go
func addTwoNumbers(l1, l2 *ListNode) *ListNode {
	dummy, carry := new(ListNode), 0
	for node := dummy; l1 != nil || l2 != nil || carry > 0; node = node.Next {
		if l1 != nil {
			carry += l1.Val
			l1 = l1.Next
		}
		if l2 != nil {
			carry += l2.Val
			l2 = l2.Next
		}
		node.Next = &ListNode{carry%10, nil}
		carry /= 10
	}
	
	return dummy.Next
}
```
time complexity:$$ O(max(n,m)) $$
space complexity:$$ O(max(n,m)) $$

---
## [Remove Nth Node from End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list)

### 1 способ: 

- используем два указателя, когда мы прошли первым указателям n чисел, то начинаем идти вместе со вторым, тогда между ними разница будет n и второй в конце будет указывать на узел который стоит перед удаляемым


```go 
func removeNthFromEnd(head *ListNode, n int) *ListNode {
	dummyHead := &ListNode{-1, head}
	current, previous := dummyHead, dummyHead
	for current.Next != nil {
		if n <= 0 {
			previous = previous.Next
		}
		current = current.Next
		n--
	}
	nthNode := previous.Next
	previous.Next = nthNode.Next
	return dummyHead.Next
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Reorder List](https://leetcode.com/problems/reorder-list/)

### 1 способ:

- мы двумя указателями идем по списку, когда один доходит до конца, другой останавливается на середине, затем мы переворачиваем весь оставшийся конец и затем идем от конца и начала списка и зигзагом сшиваем

```go
func reorderList(head *ListNode) {
	if head == nil || head.Next == nil {
		return
	}
	
	slow, fast := head, head
	for fast != nil && fast.Next != nil {
		fast = fast.Next.Next
		slow = slow.Next
	}
	
	var previous *ListNode
	for slow != nil {
		next := slow.Next
		slow.Next = previous
		previous = slow
		slow = next
	}
	
	first := head
	for previous.Next != nil {
		first.Next, first = previous, first.Next
		previous.Next, previous = first, previous.Next
	}
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/description)

### 1 способ:

- бинарным поиском проверяем отрезки, если левый меньше середины значит он упорядочен, если нет, значит правый упорядочен и ищем в них

```go
func search(nums []int, target int) int {
	left, right := 0, len(nums)-1
	for left <= right {
		mid := (left + right)/2
		
		if nums[mid] == target {
			return mid
		}
		if nums[left] <= nums[mid] {
			if nums[left] <= target && target <= nums[mid] {
				right = mid-1
			} else {
				left = mid+1
			}
		} else {
			if nums[mid] <= target && target <= nums[right] {
				left = mid+1
			} else {
				right = mid-1
			}
		}
	}
	
	return -1
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/)

### 1 способ: 

- через бинарный поиск сужаем наше окно до тех пор пока не найдем излом

```go
func findMin(nums []int) int {
	left, right := 0, len(nums)-1
	
	for left < right {
		mid := (left + right) / 2
		if nums[mid] > nums[right] {
			left = mid+1
		} else {
			right = mid
		}
	}
	
	return nums[left]
}
```
time complexity:$$ O(log(n)) $$
space complexity:$$ O(1) $$

---
## [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description)

### 1 способ: 

- рекурсивно проходим по самому левому, если не находим ответ то идем вправо и вот так перебираем значения и уменьшаем счетчик, если счетчик 0, то мы нашли ответ, просто перебираем ветви слева направо

```go
func kthSmallest(root *TreeNode, k int) int {
	return InOrderTravel(root, &k)
}

func InOrderTravel(node *TreeNode, k *int) int {
	result := 0
	if node == nil { return 0 }
	result = max(result, InOrderTravel(node.Left, k))
	*k--
	if *k == 0 { return node.Val }
	result = max(result, InOrderTravel(node.Right, k))
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(h) $$

---
## [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree)

### 1 способ:

- делаем дфс, и задаем границы и создаем некий коридор чтобы значения узла находилось между этими границами иначе дерево не валидно

```go
func isValidBST(root *TreeNode) bool {
	return isValid(root, nil, nil)
}

func isValid(node, mini, maxi *TreeNode) bool {
	if node == nil { return true }
	if mini != nil && node.Val <= mini.Val { return false }
	if maxi != nil && node.Val >= maxi.Val { return false }
	return isValid(node.Left, mini, node) && isValid(node.Right, node, maxi)
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(h) $$

---
## [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal)

### 1 способ:

- с помощью бфс проходим и передаем через указатели текущий уровень и наш двумерный массив, и добавляем значения по индексам = уровень

```go
func levelOrder(root *TreeNode) [][]int {
	result, level := [][]int{}, 0
	traversal(root, &result, &level)
	return result
}

func traversal(node *TreeNode, result *[][]int, level *int) {
	if node == nil { return }
	if len(*result) < *level + 1 {
		*result = append(*result, []int{node.Val})
	} else {
		(*result)[*level] = append((*result)[*level], node.Val)
	}
	*level++
	traversal(node.Left, result, level)
	traversal(node.Right, result, level)
	*level--
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(h) $$

---
## [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description)

### 1 способ:

- у нас всегда первый узел в preorder это корневой узелй у поддерева, поэтому мы потом ищем по какому индексу хранится в inorder корневой узел, где слева от узла вся левая ветка, справа вся правая ветка и тогда строим дерево рекурсивно передавая массива

```go
func buildTree(preorder, inorder []int) *TreeNode {
	seen := make(map[int]int)
	for index, value := range inorder {
		seen[value] = index
	}
	return build(preorder, inorder, 0, seen)
}

func build(preorder, inorder []int, shift int, seen map[int]int) *TreeNode {
	if len(preorder) == 0 { return nil }
	
	index := seen[preorder[0]] - shift
	
	return &TreeNode{
		Val: preorder[0],
		Left: build(preorder[1:index+1], inorder[:index], shift, seen),
		Right: build(preorder[index+1:], inorder[index+1:], shift+index+1, seen),
	}
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree)

### 1 способ:

- при помощи дфс находим корневой узел, просто погружаемся влево и вправо, как только у нас нашелся рез рекурсия вернет корневой узел и просто возвращаем где нашлось

```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
	if root == nil { return nil }
	if root == p || root == q { return root }
	
	left := lowestCommonAncestor(root.Left, p, q)
	right := lowestCommonAncestor(root.Right, p, q)
	
	if left != nil && right != nil { return root }
	if left != nil {
		return left
	} else {
		return right
	}
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree)

### 1 способ:

- если оба узла меньше корня то идти влево по дереву, если больше то вправо, если разошлись то вернуть корень)

```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
	for root != nil {
		if p.Val < root.Val && q.Val < root.Val {
			root = root.Left
		} else if p.Val > root.Val && q.Val > root.Val {
			root = root.Right
		} else {
			return root
		}
	}
	return nil
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Number of Islands](https://leetcode.com/problems/number-of-islands/description)

### 1 способ: 

- идти двойным циклом по всему массиву, если нашли 1 то запускаем dfs алгоритм и затапливаем весь остров путем замены 1 на 0

```go
func numIslands(grid [][]byte) int {
	result := 0
	for x := 0; x < len(grid); x++ {
		for y := 0; y < len(grid[0]); y++ {
			if grid[x][y] == '1' {
				result++
				DeepFirstSearch(x, y, grid)
			}
		}
	}
	return result
}

func DeepFirstSearch(x, y int, grid [][]byte) {
	if x < 0 || x > len(grid)-1 || y < 0 || y > len(grid[0])-1 || grid[x][y] == '0' {
		return
	}
	grid[x][y] = '0'
	DeepFirstSearch(x+1, y, grid)
	DeepFirstSearch(x-1, y, grid)
	DeepFirstSearch(x, y+1, grid)
	DeepFirstSearch(x, y-1, grid)
}
```
time complexity:$$ O(n \cdot m) $$
space complexity:$$ O(n \cdot m) $$

---
## [Clone Graph](https://leetcode.com/problems/clone-graph)

### 1 способ:

- дфс делаем и добавляем не пройденный узел в список соседей

```go
func cloneGraph(node *Node) *Node {
	if node == nil { return nil }
	copies := make([]*Node, 101)
	dfs(node, copies)
	return copies[node.Val]
}

func dfs(node *Node, copies []*Node) {
	newNode := new(Node)
	newNode.Val = node.Val
	copies[node.Val] = newNode
	
	for _, neighbor := range node.Neighbors {
		if copies[neighbor.Val] == nil {
			dfs(neighbor, copies)
		}
		newNode.Neighbors = append(newNode.Neighbors, copies[neighbor.Val])
	}
}
```
time complexity:$$ O(V+E) $$
space complexity:$$ O(E) $$

---
## [Course Schedule](https://leetcode.com/problems/course-schedule)

### 1 способ:

- короче завести два массива, по индексу 0 мы будем класть все курсы которые нам становятся доступны после прохождения курса 0 и тд, во втором массиве мы считаем сколько курсов нужно пройти для каждого курса

```go
func canFinish(numCourses int, prerequisites [][]int) bool {
	graph := make([][]int, numCourses)
	degree := make([]int, numCourses)
	
	for _, prerequisite := range prerequisites {
		graph[prerequisite[1]] = append(graph[prerequisite[1]], prerequisite[0])
		degree[prerequisite[0]]++
	}
	
	bfs := make([]int, 0)
	for course, d := range degree {
		if d == 0 {
			bfs = append(bfs, course)
		}
	}
	
	for i := 0; i < len(bfs); i++ {
		course := bfs[i]
		for _, j := range graph[course] {
			degree[j]--
			if degree[j] == 0 {
				bfs = append(bfs, j)
			}
		}
	}
	if len(bfs) == numCourses {
		return true
	}
	
	return false
}
```
time complexity:$$ O(V+E) $$
space complexity:$$ O(V+E) $$

---
## [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/description)

### 1 способ:

- пиздец

```go
func pacificAtlantic(heights [][]int) [][]int {
	if len(heights) == 0 { return nil }
	
	result := [][]int{}
	row, column := len(heights), len(heights[0])
	
	pacific, atlantic := make([][]bool, row), make([][]bool, row)
	
	for i := 0; i < row; i++ {
		pacific[i] = make([]bool, column)
		atlantic[i] = make([]bool, column)
	}
	
	for i := 0; i < column; i++ {
		dfs(0, i, heights, pacific, heights[0][i])
		dfs(row-1, i, heights, atlantic, heights[row-1][i])
	}
	
	for i := 0; i < row; i++ {
		dfs(i, 0, heights, pacific, heights[i][0])
		dfs(i, column-1, heights, atlantic, heights[i][column-1])
	}
	
	for i := 0; i < row; i++ {
		for j := 0; j < column; j++ {
			if pacific[i][j] && atlantic[i][j] {
				result = append(result, []int{i, j})
			}
		}
	}
	return result
}

func dfs(i, j int, heights [][]int, visited [][]bool, prevHeight int) {
	row, column := len(heights), len(heights[0])
	
	if i < 0 || i >= row || j < 0 || j >= column || heights[i][j] < prevHeight || visited[i][j] {
		return
	}
	
	visited[i][j] = true
	
	dfs(i+1, j, heights, visited, heights[i][j])
	dfs(i-1, j, heights, visited, heights[i][j])
	dfs(i, j+1, heights, visited, heights[i][j])
	dfs(i, j-1, heights, visited, heights[i][j])
}
```
time complexity:$$ O(n \cdot m) $$
space complexity:$$ O(n \cdot m) $$

---
## [Generate Parentheses](https://leetcode.com/problems/generate-parentheses)

### 1 способ:

- рекурсивно записываем открывающиеся скобки до тех пор пока их меньше входного параметра и записываем закрывающиеся если их меньше чем открывающихся

```go
func generateParenthesis(n int) []string {
	result := []string{}
	backtrack("", 0, 0, &result, n)
	return result
}

func backtrack(current string, open, close int, result *[]string, n int) {
	if len(current) == n*2 {
		*result = append(*result, current)
		return
	}
	
	if open < n {
		backtrack(current+"(", open+1, close, result, n)
	}
	if close < open {
		backtrack(current+")", open, close+1, result, n)
	}
}
```
time complexity:$$ O(4^n / n \sqrt n) $$ сложность порядка числа Каталана
space complexity:$$ O(n) $$

---
## [Combination Sum](https://leetcode.com/problems/combination-sum/description)

### 1 способ:

-  Идея заключается в рекурсивном обходе дерева решений (backtracking), где на каждом шаге мы либо добавляем текущее число в комбинацию и уменьшаем искомую сумму, либо переходим к следующему числу, «откатываясь» назад при превышении лимита или достижении цели.

```go
func combinationSum(candidates []int, target int) [][]int {
	result := make([][]int, 0)
	current := make([]int, 0)
	sort.Ints(candidates)
	findCombinations(0, target, candidates, current, &result)
	return result
}

func findCombinations(index, target int, candidates, current []int, result *[][]int) {
	if target == 0 {
		combination := make([]int, len(current))
		copy(combination, current)
		*result = append(*result, combination)
		return
	}
	
	for i := index; i < len(candidates); i++ {
		if candidates[i] > target {
			break
		}
		current = append(current, candidates[i])
		findCombinations(i, target-candidates[i], candidates, current, result)
		current = current[:len(current)-1]
	}
}
```
time complexity:$$ O(N^{T/M}) $$
space complexity:$$ O(T/M) $$

---
## [Word Search](https://leetcode.com/problems/word-search)

### 1 способ:

- при помощи дфс просто проверяем буквы во всех направлениях

```go
func exist(board [][]byte, word string) bool {
	if len(board) == 0 || len(board[0]) == 0 || len(word) == 0 {
		return false
	}
	
	for x := 0; x < len(board); x++ {
		for y := 0; y < len(board[0]); y++ {
			if board[x][y] == word[0] && dfs(x, y, board, word) {
				return true
			}
		}
	}
	return false
}

func dfs(x, y int, board [][]byte, word string) bool {
	if len(word) == 0 { return true }
	if x < 0 || y < 0 || x >= len(board) || y >= len(board[0]) || board[x][y] != word[0] {
		return false
	}
	
	template := board[x][y]
	board[x][y] = '#'
	
	result := dfs(x+1, y, board, word[1:]) ||
			  dfs(x-1, y, board, word[1:]) ||
			  dfs(x, y+1, board, word[1:]) ||
			  dfs(x, y-1, board, word[1:])
			  
	board[x][y] = template
	return result
}
```
time complexity:$$ O(N \cdot M \cdot 3^L) $$space complexity:$$ O(N \cdot M) $$

---
## [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/description)

### 1 способ:

- 