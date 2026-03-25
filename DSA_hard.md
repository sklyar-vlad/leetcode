## [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring)

### 1 способ:

- скользящее окно и паттерн нашей строки

```go
func minWindow(s string, t string) string {
	if len(s) < len(t) { return "" }
	minLen, minStart, start := math.MaxInt32, 0, 0
	count := len(t)
	freqMap := [128]int{}
	
	for _, char := range t {
		freqMap[char]++
	}
	
	for end := 0; end < len(s); end++ {
		if freqMap[s[end]] > 0 {
			count--
		}
		freqMap[s[end]]--
		
		for count == 0 {
			if end-start+1 < minLen {
				minLen = end-start+1
				minStart = start
			}
			
			freqMap[s[start]]++
			if freqMap[s[start]] > 0 {
				count++
			}
			start++
		}
	}
	
	if minLen == math.MaxInt32 { return "" }
	return s[minStart:minStart+minLen]
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/description)

### 1 способ: 

- пишем вспомогательную функцию для сливания двух списков, а затем идем по списку списков и парами сливаем

```go
func mergeTwoLists(l1, l2 *ListNode) *ListNode  {
	dummy := &ListNode{}
	previous := dummy
	
	for l1 != nil && l2 != nil {
		if l1.Val < l2.Val {
			previous.Next = l1
			l1 = l1.Next
		} else {
			previous.Next = l2
			l2 = l2.Next
		}
		previous = previous.Next
	}
	
	if l1 != nil {
		previous.Next = l1
	} else {
		previous.Next = l2
	}
	
	return dummy.Next
}

func mergeKLists(lists []*ListNode) *ListNode {
	if len(lists) == 0 { return nil }
	
	for len(lists) > 1 {
		var mergedLists []*ListNode
		for i := 0; i < len(lists); i += 2 {
			l1 := lists[i]
			var l2 *ListNode
			if i+1 < len(lists) {
				l2 = lists[i+1]
			}
			mergedLists = append(mergedLists, mergeTwoLists(l1, l2))
		}
		lists = mergedLists
	}
	
	return lists[0]
}
```
time complexity:$$ O(n \cdot log(k)) $$
space complexity:$$ O(log(k)) $$

---
## [Find Medium from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/description)

```go
type MinHeap []int

func (h MinHeap) Len() int { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h MinHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MinHeap) Pop() interface{} { 
	x := (*h)[len(*h)-1]
	*h =(*h)[:len(*h)-1]
	return x
}

type MedianFinder struct {
	low, high MinHeap
}

func Constructor() MedianFinder {
	return MedianFinder{}
}

func (this *MedianFinder) AddNum(num int) {
	heap.Push(&this.low, -num)
	heap.Push(&this.high, -heap.Pop(&this.low).(int))
	
	if this.low.Len() < this.high.Len() {
		heap.Push(&this.low, -heap.Pop(&this.high).(int))
	}
}

func (this *MedianFinder) FindMedian() float64 {
	if this.low.Len() > this.high.Len() {
		return float64(-this.low[0])
	}
	return float64(-this.low[0]+this.high[0])/2
}
```
time complexity:$$ O(log(n)) $$
space complexity:$$ O(n) $$

---
## [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/description)

### 1 способ:

- двумя указателями сдвигаемся к центру и если у нас меньше левая стена то сравниваем текущий столбик с предыдущим и складываем разность и так же справа

```go
func trap(height []int) int {
	if len(height) == 0 { return 0 }
	result := 0
	left, right := 0, len(height)-1
	maxLeft, maxRight := height[0], height[len(height)-1]
	
	for left < right {
		if maxLeft < maxRight {
			left++
			maxLeft = max(maxLeft, height[left])
			result += maxLeft - height[left]
		} else {
			right--
			maxRight = max(maxRight, height[right])
			result += maxRight - height[right]
		}
	}
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/description)

### 1 способ:

-  при помощи дфс обходим весь список и собираем сумму

```go
func maxPathSum(root *TreeNode) int {
	maxSum := math.MinInt32
	dfs(root, &maxSum)
	return maxSum
}

func dfs(node *TreeNode, maxSum *int) int {
	if node == nil { return 0 }
	left := max(dfs(node.Left, maxSum), 0)
	right := max(dfs(node.Right, maxSum), 0)
	current := node.Val + left + right
	*maxSum = max(*maxSum, current)
	return node.Val + max(left, right)
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(h) $$

---
## [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/description)

```go
type Codec struct {}

func Constructor() Codec {
	return Codec{}
}

func dfs(node *TreeNode, result *[]string) {
	if node == nil { 
		*result = append(*result, "N")
		return
	}
	*result = append(*result, strconv.Itoa(node.Val))
	dfs(node.Left, result)
	dfs(node.Right, result)
}

func (this *Codec) serialize(root *TreeNode) string {
	var result []string
	dfs(root, &result)
	return strings.Join(result, ",")
}

func build(values []string, index *int) *TreeNode {
	if values[*index] == "N" {
		*index++
		return nil
	}
	value, _ := strconv.Atoi(values[*index])
	*index++
	
	node := &TreeNode{Val: value}
	node.Left = build(values, index)
	node.Right = build(values, index)
	
	return node
}

func (this *Codec) deserialize(data string) *TreeNode {
	values := strings.Split(data, ",")
	index := 0
	return build(values, &index)
}
```

---
## [Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/description)

### 1 способ:

- строим гистрограмму для каждой строки и затем берем максимальную по величине

```go
func maximalRectangle(matrix [][]byte) int {
	if len(matrix) == 0 { return 0 }
	columns := len(matrix[0])
	heights := make([]int, columns)
	maxArea := 0
	
	for _, row := range matrix {
		updateHeights(row, heights)
		area := largestRectangleArea(heights)
		maxArea = max(maxArea, area)
	}
	
	return maxArea
}

func updateHeights(row []byte, heights []int) {
	for i := 0; i < len(row); i++ {
		if row[i] == '1' {
			heights[i]++
		} else {
			heights[i] = 0
		}
	}
}

func largestRectangleArea(heights []int) int {
	stack := make([]int, 0)
	maxArea := 0
	heights = append(heights, 0)
	
	for i := 0; i < len(heights); i++ {
		for len(stack) > 0 && heights[stack[len(stack)-1]] > heights[i] {
			top := stack[len(stack)-1]
			stack = stack[:len(stack)-1]
			height := heights[top]
			width := i
			if len(stack) > 0 {
				width = i - stack[len(stack)-1] - 1
			}
			maxArea = max(maxArea, height*width)
		}
		stack = append(stack, i)
	}
	
	return maxArea
}
```
time complexity:$$ O(n \cdot m) $$
space complexity:$$ O(m) $$

---
## [Median of Two Sorted Array](https://leetcode.com/problems/median-of-two-sorted-arrays/description/)

### 1 способ:

- просто двумя указателями идем по двум массивам чей элемент тот и добавляем и сдвигаем указатель а потом возвращаем середину

```go
func findMedianSortedArrays(nums1, nums2 []int) int {
	merged := make([]int, 0, len(nums1)+len(nums2))
	i, j := 0, 0
	
	for i < len(nums1) && j < len(nums2) {
		if nums1[i] < nums2[j] {
			merged = append(merged, nums1[i])
			i++
		} else {
			merged = append(merged, nums2[j])
			j++
		}
	}
	
	for i < len(nums1) {
		merged = append(merged, nums1[i])
		i++
	}
	for j < len(nums2) {
		merged = append(merged, nums2[j])
		j++
	}
	
	middle := len(merged) / 2
	
	if len(merged) % 2 == 0 {
		return (float64(merged[middle-1]) + float64(merged[middle])) / 2.0
	} else {
		return float64(merged[middle])
	}
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Word Search II](https://leetcode.com/problems/word-search-ii)

### 1 способ:

- при помощи префиксного дерева и прохода в глубину

```go
var directions = [4][2]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}

func findWords(board [][]byte, words []string) []string {
	trie := new(TrieNode)
	
	for _, word := range words {
		trie.insert(word)
	}
	
	result := make([]string, 0, len(words))
	
	for i, row := range board {
		for j, char := range row {
			index := char - 'a'
			
			if trieNode := trie.Children[index]; trieNode != nil {
				dfs(board, i, j, trie, &result)
			}
		}
	}
	
	return result
}

func dfs(board [][]byte, i, j int, node *TrieNode, result *[]string) {
	letter := board[i][j]
	node = node.Children[letter - 'a']
	if node == nil {
		return
	} else if node.Word != "" {
		*result = append(*result, node.Word)
		node.Word = ""
	}
	
	board[i][j] = '.'
	
	for _, direct := range directions {
		x, y := i+direct[0], j+direct[1]
		
		if x >= 0 && y >= 0 && x < len(board) && y < len(board[0]) && board[x][y] != '.' {
			dfs(board, x, y, node, result)
		}
	}
	
	board[i][j] = letter
}

type TrieNode struct {
	Children [26]*TrieNode
	Word string
}

func (node *TrieNode) insert(word string) {
	for _, char := range word {
		index := char - 'a'
		
		if node.Children[index] == nil {
			node.Children[index] = new(TrieNode)
		}
		
		node = node.Children[index]
	}
	
	node.Word = word
}
```
time complexity:$$ O(W \cdot L + M \cdot N \cdot 4^L) $$
space complexity
trie:$$ O(W \cdot L) $$
dfs:$$ O(L) $$
