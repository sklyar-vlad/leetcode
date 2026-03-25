## [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters)

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
## [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii)

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

---
## [Container With Most Water](https://leetcode.com/problems/container-with-most-water/description)

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
space complexity:$$ O(1) $$

---
## [Jump Game](https://leetcode.com/problems/jump-game)

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
				k--
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

```go
func longestConsecutive(nums []int) int {
	if len(nums) == 0 { return 0 }
	
	seen := make(map[int]bool)
	for _, num := range nums {
		seen[num] = true
	}
	result := 1
	
	for num := range seen {
		if !seen[num-1] {
			current := num 
			template := 1
			
			for seen[current+1] {
				current++
				template++
			}
			
			result = max(result, template)
		}
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Perfect Squares](https://leetcode.com/problems/perfect-squares/description/)

```go
func numSquares(n int) int {
	seen := make([]int, n+1)
	seen[0] = 0
	for i := 1; i <= n; i++ {
		seen[i] = math.MaxInt32
	}
	
	for i := 1; i <= n; i++ {
		for j := 1; j*j <= i; j++ {
			seen[i] = min(seen[i], seen[i-j*j]+1)
		}
	}	
	
	return seen[n]
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Unique Paths](https://leetcode.com/problems/unique-paths/description)

```go 
func uniquePaths(m, n int) int {
	result := 1
	for i := 1; i <= m-1; i++ {
		result = result * (n-1+i)/i
	}
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Decode Ways](https://leetcode.com/problems/decode-ways/description)

```go
func isValid(code, length int) bool {
	if length == 1 {
		return code >= 1 && code <= 9
	} else {
		return code >= 10 && code <= 26
	}
}

func countOfDecoding(i int, s string, seen []int) int {
	if i >= len(s) { return 1 }
	if seen[i] != -1 { return seen[i] }
	seen[i] = 0
	if isValid(int(s[i]) - '0', 1) {
		seen[i] += countOfDecoding(i+1, s, seen)
	} 
	if i < len(s)-1 && isValid((int(s[i] - '0'))*10 + int(s[i+1] - '0'), 2) {
		seen[i] += countOfDecoding(i+2, s, seen)
	} 
	
	return seen[i]
}

func numDecodings(s string) int {
	seen := make([]int, len(s))
	for i := range seen {
		seen[i] = -1
	}
	return countOfDecoding(0, s, seen)
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Word Break](https://leetcode.com/problems/word-break)

```go
func wordBreak(s string, wordDict []string) bool {
	wordMap := make(map[string]bool)
	maxLen := 0
	for _, word := range wordDict {
		wordMap[word] = true
		maxLen = max(maxLen, len(word))
	}
	
	seen := make([]bool, len(s)+1)
	seen[0] = true
	
	for i := 1; i <= len(s); i++ {
		for j := i-1; j >= 0; j-- {
			if i-j > maxLen { break }
			if seen[j] && wordMap[s[j:i]] {
				seen[i] = true
				break
			}
		}
	}
	
	return seen[len(s)]
}
```
time complexity:$$ O(n \cdot min(wordDict, len(s))) $$
space complexity:$$ O(n) $$

---
## [House Robber](https://leetcode.com/problems/house-robber/description)

```go 
func rob(nums []int) int {
	if len(nums) == 1 { return nums[0] }
	l1, l2 := nums[0], max(nums[0], nums[1])
	for i := 2; i < len(nums); i++ {
		l2, l1 = max(l1+nums[i], l2), l2
	}
	return l2
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/description)

```go
func longestPalindrome(s string) string {
	template := "^#" + strings.Join(strings.Split(s, ""), "#") + "#$"
	seen := make([]int, len(template))
	center, right := 0, 0
	
	for i := 1; i < len(template)-1; i++ {
		if i < right {
			seen[i] = min(right-i, seen[center*2-i])
		}
		for template[i-1-seen[i]] == template[i+1+seen[i]] {
			seen[i]++
		}
		if seen[i] + i > right {
			center, right = i, i + seen[i]
		}
	}
	
	maxLen, centerIndex := 0, 0
	for i, value := range seen {
		if maxLen < value {
			maxLen = value
			centerIndex = i
		}
	}
	
	return s[(centerIndex-maxLen)/2:(centerIndex+maxLen)/2]
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/description)

```go
func countSubstrings(s string) int {
	template := "^#" + strings.Join(strings.Split(s, ""), "#") + "#$"
	seen := make([]int, len(template))
	result, center, right := 0, 0, 0
	
	for i := 1; i < len(template)-1; i++ {
		if i < right {
			seen[i] = min(right-i, seen[center*2 - i])
		}
		for template[i-1-seen[i]] == template[i+1+seen[i]] {
			seen[i]++
		}
		if seen[i] + i > right {
			center, right = i, seen[i] + i
		}
		result += (seen[i] + 1)/2
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/description)

```go
func longestCommonSubsequence(text1, text2 string) int {
	seen := make([][]int, len(text1)+1)
	for i := 0; i < len(text1)+1; i++ {
		seen[i] = make([]int, len(text2)+1)
	}
	
	for i := 1; i < len(text1)+1; i++ {
		for j := 1; j < len(text2)+1; j++ {
			if text1[i-1] == text2[j-1] {
				seen[i][j] = seen[i-1][j-1] + 1
			} else {
				seen[i][j] = max(seen[i-1][j], seen[i][j-1])
			}
		}
	}
	
	return seen[len(text1)][len(text2)]
}
```
time complexity:$$ O(n \cdot m) $$
space complexity:$$ O(n \cdot m) $$

---
## [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence)

```go
func lengthOfLIS(nums []int) int {
	result := make([]int, 0)
	for i := 0; i < len(nums); i++ {
		if len(result) == 0 || result[len(result)-1] < nums[i] {
			result = append(result, nums[i])
		} else {
			index := binarySearch(result, nums[i])
			result[index] = nums[i]
		}
	}
	
	return len(result)
}

func binarySearch(nums []int, target int) int {
	left, right := 0, len(nums)-1
	for left <= right {
		middle := (left+right)/2
		if nums[middle] < target {
			left = middle + 1
		} else {
			right = middle - 1
		}
	}
	
	return left
}
```
time complexity:$$ O(n \cdot log(n)) $$
space complexity:$$ O(n) $$

---
## [Coin Change](https://leetcode.com/problems/coin-change/description)

```go
func coinChange(coins []int, amount int) int {
	seen := make([]int, amount+1)
	for templateSum := 1; templateSum <= amount; templateSum++ {
		seen[templateSum] = math.MaxInt32
		for _, coin := range coins {
			if templateSum-coin >= 0 && seen[templateSum-coin] != math.MaxInt32 {
				seen[templateSum] = min(seen[templateSum], seen[templateSum-coin]+1)
			}
		}
	}
	
	if seen[amount] == math.MaxInt32 {
		return -1
	}
	return seen[amount]
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Simplify Path](https://leetcode.com/problems/simplify-path/description)

```go
func simplifyPath(path string) string {
	afterSplit := strings.Split(path, "/")
	result := make([]string, 0)
	for _, element := range afterSplit {
		if element == "." || element == "" { continue }
		if element == ".." {
			if len(result) >= 1 {
				result = result[:len(result)-1]
			} 
		} else {
			result = append(result, element)
		}
	}
	
	return "/" + strings.Join(result, "/")
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation)

```go
func evalRPN(tokens []string) int {
	operators := map[string]func(int, int) int {
		"+": func(a, b int) int { return a + b },
		"-": func(a, b int) int { return a - b },
		"/": func(a, b int) int { return a / b },
		"*": func(a, b int) int { return a * b },
	}
	stack := make([]int, 0, len(tokens))
	for _, token := range tokens {
		if operator, ok := operators[token]; ok {
			a, b := stack[len(stack)-2], stack[len(stack)-1]
			stack = append(stack[:len(stack)-2], operator(a, b))
		} else {
			num, _ := strconv.Atoi(token)
			stack = append(stack, num)
		}
	}
	
	return stack[0]
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Implement Rand10() Using Rand7()](https://leetcode.com/problems/implement-rand10-using-rand7/description)

```go
func rand10() int {
	for {
		num := (rand7()-1)*7 + rand7()
		if num <= 40 {
			return (num-1)%10 + 1
		}
	}
}
```
time complexity:$$ O(1) $$
space complexity:$$ O(1) $$

---
## [Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers)

```go
func getSum(a, b int) int {
	for b != 0 {
		sum := a ^ b
		carry := (a & b) << 1
		a = sum
		b = carry
	}
	
	return a
}
```
time complexity:$$ O(1) $$
space complexity:$$ O(1) $$

---
## [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/description)

```go
var pool [100000]Trie
var pId int

type Trie struct {
	nodes [26]*Trie
	isEnd bool
}

func Constructor() Trie {
	pId = 0
	return Trie{}
}

func newNode() *Trie {
	pId++
	node := &pool[pId]
	node.nodes = [26]*Trie{}
	node.isEnd = false
	return &pool[pId]
}

func (this *Trie) Insert(word string) {
	current := this
	for _, char := range word {
		index := char - 'a'
		if current.nodes[index] == nil {
			current.nodes[index] = newNode()
		}
		current = current.nodes[index]
	}
	
	current.isEnd = true
}

func (this *Trie) Find(word string) *Trie {
	current := this
	
	for _, char := range word {
		index := char - 'a'
		if current.nodes[index] == nil {
			return nil
		}
		current = current.nodes[index]
	}
	
	return current
}

func (this *Trie) Search(word string) bool {
	node := this.Find(word)
	return node != nil && node.isEnd
}

func (this *Trie) StartsWith(prefix string) bool {
	return this.Find(prefix) != nil
}
```