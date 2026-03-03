
## [Two sum](https://leetcode.com/problems/two-sum) 

### 1 способ: 

- брутфорс - проходимся двумя циклами по всему массиву и сравниваем значения, тогда временная сложность будет

```go
func twoSum(nums []int, target int) []int {
	for i := 0; i < len(nums); i++ {
		for j := i + 1; j < len(nums); j++ {
			if nums[i] + nums[j] == target {
				return []int{i, j}
			}
		}
	}
	
	return []int{}
}
```

time complexity:$$ O(n^2) $$
space complexity:$$ O(1) $$
### 2 способ: 

- хеш-мапа с двумя обходами, здесь мы проходим первый раз и заполняем словарь со значениями, во второй раз сравнимаем разность нашей цели и текущим элементом, если разность есть в словаре, то выводим индексы, если нет то идем далее, тут время 

```go
func twoSum(nums []int, target int) []int {
	dict := make(map[int]int)
	for i := 0; i < len(nums); i++ {
		dict[nums[i]] = i
	}
	
	for i := 0; i < len(nums); i++ {
		add := target - nums[i]
		_, ok := dict[add]
		if ok && i != dict[add] {
			return []int{i, dict[add]}
		}
	}
	
	return []int{}
}
```

time complexity:$$ O(n) $$
space complexity:$$ O(n) $$
### 3 способ:

- за один обход и добавляем значения если нет дополнения, если есть, то сразу выводим ответ. временная сложность и используемая память равны предыдущему способу

```go
func twoSum(nums []int, target int) []int {
    dict := make(map[int]int)
    for i:=0;i<len(nums);i++ {
        add := target - nums[i]
        _, ok := dict[add]
        if !ok {
            dict[nums[i]] = i
        } else {
            return []int{dict[add], i}
        }
    }

    return []int{}
}
```

time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/description/)

### 1 способ:

- чисто при помощи библиотек strings, unicode почистить строку от ненужных символов и потом еще одним циклом проходить по строке и сравнивать краевые элементы, если попадается первая не одинаковая пара, то возвращать false если же все прошлось то true 

```go
func isPalindrome(s string) bool {
	var cleaned strings.Builder
	for _, char := range strings.ToLower(s) {
		if unicode.IsLetter(char) || unicode.IsDigit(char) {
			cleaned.WriteRune(char)
		}
	}
	result := cleaned.String()
	for i := 0; i < len(result) / 2; i++ {
		if result[i] != result[len(result) - 1 - i] {
			return false
		}
	}
	
	return true
} 
```

time complexity:$$ O(n) $$
space complexity:$$ O(n) $$
### 2 способ:

- просто два указателя используем и ведем их пока пары сходятся, так же можно реализовать функцию проверки символа через ASCII таблицу

```go
func IsLetterOrNum(c byte) bool {
	return (c >= 'A' && c <= 'Z') || (c >= 'a' && c <= 'z') || (c >= '0' && c <= '9')
}

func isPalindrome(s string) bool {
	l, r := 0, len(s) - 1
	
	for l < r {
		for l < r && !IsLetterOrNum(s[l]) {
			l++
		}
		for l < r && !IsLetterOrNum(s[r]) {
			r--
		}
		
		if strings.ToLower(string(s[l])) != strings.ToLower(string(s[r])) {
			return false
		}
		l++
		r--
	}
	
	return true
}
```

time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Move Zeroes](https://leetcode.com/problems/move-zeroes)

### 1 способ:

- при помощи двух указателей, левый указатель будет указывать границу тех чисел где нет нулей, а правый указатель идет до нуля, а потом меняет местами ноль и не ноль под двумя указателями

```go
func moveZeroes(nums []int) {
	l := 0
	for r := 0; r < len(nums); r++ {
		if nums[r] != 0 {
			nums[l], nums[r] = nums[r], nums[l]
			l++
		}
	}
}
```

time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---

## [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/) 

### 1 способ:

- тут идея просто начать отсчет с нулей в первом массиве и начать с первого элемента второго массива и вставлять в первый массив числа из второго, затем применить сортировку

```go
func merge(nums1 []int, m int, nums2 []int, n int) {
	for i, j = 0, m; i < n; i, j = i+1, j+1 {
		nums1[j] = nums2[i]
	}
	
	sort.Ints(nums1)
}
```

time complexity:$$ O((m+n) \cdot log(m+n)) $$
space complexity:$$ O(1) $$
### 2 способ:

- мы имеем три указателя, один указывает на конец реальных значений первого массива, второй указывает на конец второго массива и третий указывает на конец всего первого массива, затем начинаем из второго массива сравнивать с последним элементом первого, если из второго массива элемент больше то ставим в конец первого массива и так сравниваем и переставляем

```go
func merge(nums1 []int, m int, nums2 []int, n int) {
	k := m + n - 1
	p1 := m - 1
	p2 := n - 1
	
	for p2 >= 0 {
		if p1 >= 0 && nums1[p1] > nums2[p2] {
			nums1[k] = nums1[p1]
			p1--
		} else {
			nums1[k] = nums2[p2]
			p2--
		}
		k--
	}
}
```

time complexity:$$ O(n+m) $$
space complexity:$$ O(1) $$

---
## [Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array)

### 1 способ:

- в тупую создать новый массив, записать в него квадраты каждого числа и затем отсортировать при помощи библиотеки sort 

```go
func sortedSquares(nums []int) []int {
	result := make([]int, len(nums))
	for i := 0; i < len(nums); i++ {
		result[i] = nums[i] * nums[i]
	}
	sort.Ints(result)
	return result
}
```

time complexity:$$ O(n \cdot log(n)) $$
space complexity:$$ O(1) $$
### 2 способ:

- решать двумя указателями, так как у нас изначально массив отсортирован и есть отрицательные значения, то максимальные числа скорее всего буду иметь наибольший модуль, поэтому надо сравнивать краевые значения, у кого модуль выше того ставить в конец массива и затем возводить в квадрат

```go
func Abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}

func sortedSquares(nums []int) []int {
	result := make([]int, len(nums))
	l, r := 0, len(nums) - 1
	
	for i := len(nums) - 1; i >= 0; i-- {
		if Abs(nums[l]) > Abs(nums[r]) {
			result[i] = nums[l]*nums[l]
			l++
		} else {
			result[i] = nums[r]*nums[r]
			r--
		}
	}
	
	return result
}
```

time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description)

### 1 способ:

- пробегаемся двумя указателями по массиву, один указывает куда мы кладем уникальное число, второй бежим и сравнивает предыдущее с текущим, если разные то записываем под индексом на котором стоит первый указатель

```go
func removeDuplicates(nums []int) int {
	index := 1
	
	for i := 1; i < len(nums); i++ {
		if nums[i] != nums[i-1] {
			nums[index] = nums[i]
			index++
		}
	}
	
	return index
}
```

time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)

### 1 способ:

-  создать хеш-мапу которая обозначает видели ли мы это число в другом массиве или нет, пробегаемся по первому массиву заполняем хеш-мапу, затем пробегаемся по второму, если там есть значения которые которые мы сохранили в хешмапу, значит добавляем в результат через аппенд

```go
func intersection(nums1 []int, nums2 []int) []int {
	seen := make(map[int]bool)
	result := make([]int, 0)
	
	for i := 0; i < len(nums1); i++ {
		seen[nums1[i]] = true
	}
	
	for i := 0; i < len(nums2); i++ {
		if seen[nums2[i]] == true {
			result = append(result, nums2[i])
			seen[nums2[i]] = false
		}
	}
	
	return result
}
```

time complexity:$$ O(n+m) $$
space complexity:$$ O(n) $$

---
## [Missing Number](https://leetcode.com/problems/missing-number/description/)

### 1 способ:

- Гениальное решение, просто считаем сумму арифметической прогрессии, а затем вычитаем все значения из суммы которые лежат в массиве, оставшийся результат это и есть недостающее число

```go
func missingNumber(nums []int) int {
	result := (len(nums) * (len(nums) + 1)) / 2
	
	for _, value := range nums {
		result -= value
	}

	return value
}
```

time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Jewels and Stones](https://leetcode.com/problems/jewels-and-stones/description/)

### 1 способ:

- короче создать хеш-мапу с просмотренными символами из драгоценностей, затем бежать по списку камней и если это значение есть в хеш-мапе то увеличивать счетчик

```go
func numJewelsInStones(jewels string, stones string) int {
	seen := make(map[byte]bool)
	result := 0
	
	for i := 0; i < len(jewels); i++ {
		seen[jewels[i]] = true
	}
	
	for i := 0; i < len(stones); i++ {
		if seen[stones[i]] == true {
			result++
		}
	}
	
	return result
}
```
time complexity:$$ O(n+m) $$
space complexity:$$ O(n) $$

---
## [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/description/)

### 1 способ:

- тоже создать хеш-мапу где вести подсчет какие символы встретились, затем вторым циклом пробегаться заново по строке и первый символ который в хеш-мапе будет только один раз его и вернуть

```go
func firstUniqChar(s string) int {
	seen := make(map[rune]int)
	
	for _, char := range s {
		seen[char]++
	}
	
	for idx, char := range s {
		if seen[char] == 1 {
			return idx
		}
	}
	
	return -1
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Consecutive Characters](https://leetcode.com/problems/consecutive-characters/description/)

### 1 способ:

- короч создаем переменную куда будем складывать наибольшее значение и левый указатель который будет указывать на начало строки нашей максимальной, затем циклом идем по строке, если дошли до конца или у нас символы различаются, то мы перезаписываем переменную мощности и сдвигаем левый указатель на следующий индекс где у нас закончилась строка

```go
func maxPower(s string) int {
	if len(s) == 0 { return 0 }
	power, l := 1, 0
	
	for idx := range s {
		if idx == len(s) - 1 || s[idx] != s[idx+1] {
			power = max(power, idx - l + 1)
			l = idx + 1
		}
	}
	
	return power
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Add Strings](https://leetcode.com/problems/add-strings/description/)

### 1 способ:

- реализовываем сложение по принципу сложения в столбик с переменной под перенос разряда и отсчет с конца числа

```go
func addStrings(num1 string, num2 string) string {
	i, j := len(num1) - 1, len(num2) - 1
	carry := 0
	result := ""
	
	for i >= 0 || j >= 0 {
		x, y := 0, 0
		if i >= 0 {
			x = int(num1[i] - '0')
		}
		if j >= 0 {
			y = int(num2[j] - '0')
		}
		sum := x + y + carry
		carry = sum / 10
		result = strconv.Itoa(sum%10) + result
		i--
		j--
	}
	if carry != 0 {
		result = strconv.Itoa(carry) + result
	}
	
	return result
	
}
```
time complexity:$$ O(max(n, m)) $$
space complexity:$$ O(max(n, m)) $$

---
## [Reverse Words in a String III](https://leetcode.com/problems/reverse-words-in-a-string-iii/description/)

## 1 способ:

- парсим список слов в массив через split, затем каждое слово кастуем в руны и двумя указателями за один цикл меняем местами элементы, а затем обратно в аски кидаем и возвращаем в строку через join

```go
func reverseWords(s string) string {
	words := strings.Split(s, " ")
	for idx, word := range words {
		runes := []rune(word)
		for i, j := 0, len(runes) - 1; i < j; i, j = i+1, j-1 {
			runes[i], runes[j] = runes[j], runes[i]
		}
		words[idx] = string(runes)
	}
	
	return strings.Join(words, " ")
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Valid Parentheses](https://leetcode.com/problems/valid-parentheses)

### 1 способ: 

-  решать стеком, и хешмапой. Идем по нашей строке, если попадается закрывающаяся, то проверяем является ли последняя скобка в нашем стеке подходящей, если нет то возвращаем сразу фолс, если да то срезаем стек до нуля. Если скобка открывающаяся то добавляем в стек

```go
func isValid(s string) bool {
	stack := []rune{}
	brackets := map[rune]rune{']':'[', ')':'(', '}':'{'}
	
	for _, char := range s {
		if bracket, ok := brackets[char]; ok {
			if len(stack) > 0 && stack[len(stack) - 1] == bracket {
				stack = stack[:len(stack) - 1]
			} else {
				return false
			}
		} else {
			stack = append(stack, char)
		}
	}
	
	return len(stack) == 0
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/description/)

### 1 способ:

- рекурсивно сравниваем значение текущих узлов, что меньше тому следующему присваиваем результат функции этой же но без текущего узла, а затем делаем возврат значения. Если у нас какой-то из списков закончился, то возвращаем просто другой список полностью.

```go
func mergeTwoLists(list1 *ListNode, list2 *ListNode) *ListNode {
	if list1 == nil { return list2 }
	if list2 == nil { return list1 }
	
	if list1.Val < list2.Val {
		list1.Next = mergeTwoLists(list1.Next, list2)
		return list1
	} else {
		list2.Next = mergeTwoLists(list1, list2.Next)
		return list2
	}
}
```
time complexity:$$ O(n + m) $$
space complexity:$$ O(n + m) $$

---
## [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list)

### 1 способ:

- с помощью трех указателей разворачиваем стрелки в списке, создаем вспомогательный указатель на текущий узел, на следующий и на предыдущий, сначала надо в следующий сохранить следующий узел чтобы не потерять его когда привязку от текущего мы перенесем со следующего на предыдущий

```go
func reverseList(head *ListNode) *ListNode {
	var previous *ListNode
	current := head 
	
	for current != nil {
		next := current.Next
		
		current.Next = previous
		
		previous = current
		current = next
	}
	
	return previous
}
```
time complexity: $$ O(n) $$
space complexity: $$ O(1) $$

---
## [Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls)

### 1 способ:

- просто реализовать добавление через типы в стандартной библиотеке, пинг через аппенд работает, а удаление истекших запросов через цикл без счетчика

```go
type RecentCounter struct {
	requests []int
}

func Constructor() RecentCounter {
	return RecentCounter{}
}

func (this *RecentCounter) Ping(t int) int {
	this.requests = append(this.requests, t)
	
	for this.requests[0] < t - 3000 {
		this.requests = this.requests[1:]
	}
	
	return len(this.requests)
}
```
time complexity:$$ O(1) $$
space complexity:$$ O(W) $$

### 2 способ:

- мы вместо того чтобы срезать слайс каждую итерацию цикла, мы найдем нашу границу запросов через бинарный поиск и затем вычтем эту часть

```go
type RecentCounter struct {
	requests []int
}

func Constructor() RecentCounter {
	return RecentCounter{}
}

func (this *RecentCounter) Ping(t int) int {
	this.requests = append(this.requests, t)
	useless := binarysearch(this.requests, t - 3000)
	return len(this.requests) - useless
}

func binarysearch(nums []int, target int) int {
	l, r := 0, len(nums) - 1
	
	for l <= r {
		mid := (l + r) / 2
		if num := nums[mid]; num == target {
			return mid
		} else if num < target {
			l = mid + 1
		} else {
			r = mid - 1
		}
	}
	
	return l
}
```
time complexity:$$ O(log(n))$$
space complexity:$$ O(W) $$

---
## [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/description/)

### 1 способ:

- реализация очереди через стек, мы просто реализуем путем переливания из одного в другой и достаем из второго уже все элементы для методов Peak и Pop

```go
type MyQueue struct {
	stackPush, stackPop []int
}

func Constructor() MyQueue {
	return MyQueue{}
}

func (this *MyQueue) Push(x int) {
	this.stackPush = append(this.stackPush, x)
}

func (this *MyQueue) Pop() {
	ans := this.Peek()
	this.stackPop = this.stackPop[:len(this.stackPop)-1]
	return ans
}

func (this *MyQueue) Peek() int {
	if len(this.stackPop) == 0 {
		for len(this.stackPush) != 0 {
			this.stackPop = append(this.stackPop, this.stackPush[len(this.stackPush) - 1])
			this.stackPush = this.stackPush[:len(this.stackPush) - 1]
		}
	}
	
	return this.stackPop[len(this.stackPop) - 1]
}

func (this *MyQueue) Empty() bool {
	return len(this.stackPop) == 0 && len(this.stackPush) == 0
}
```
time complexity:$$ O(1) $$
space complexity:$$ O(n) $$

---
## [Symmetric Tree](https://leetcode.com/problems/symmetric-tree)

### 1 способ:

- при помощи рекурсии проверять сначала внешние узлы затем внутрение и так обходить каждую ветку

```go
func isSymmetric(root *TreeNode) bool {
	if root == nil {
		return false
	}
	return isMirror(root.Left, root.Right)
}

func isMirror(left, right *TreeNode) bool {
	if left == nil || right == nil {
		return left == right
	}
	
	return (left.Val == right.Val) && isMirror(left.Left, right.Right) && isMirror(left.Right, right.Left)
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(h) $$

---
## [Range sum of BST](https://leetcode.com/problems/range-sum-of-bst)

### 1 способ:

- в тупую обходим все дерево, причем помним про свойство бинарного дерева, что по левую ветку всегда меньшее число, поэтому если значение узла меньше диапазона то берем правую ветку, ну и рекурсивно пробегаемся по всем узлам и складываем значения

```go
func rangeSumBST(root *TreeNode, low int, high int) int {
	if root == nil {
		return 0
	}
	if root.Val < low {
		return rangeSumBST(root.Right, low, high)
	}
	if root.Val > high {
		return rangeSumBST(root.Left, low, high)
	}
	
	return root.Val + rangeSumBST(root.Left, low, high) + rangeSumBST(root.Right, low, high)
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(h) $$

---
## [Summary Ranges](https://leetcode.com/problems/summary-ranges/)

### 1 способ:

- одним циклом пробегаемся по числам, устанавливаем указатель на то число с которого начинаем диапазон, затем если у нас последующие значения составляют ряд то просто скипаем пока не ломается ряд, затем проверяем если указатель и текущий индекс равны значит число вообще одинокое стоит и мы его просто записываем, если же нет, то склеиваем значение под указателем на начале и стрелочку и под индексом, затем перекидываем указатель на следующий от индекса элемент и возвращаем наш резалт после всего

```go
func summaryRanges(nums []int) []string {
	if len(nums) == 0 {
		return nil
	}
	
	result := []string{}
	head := 0
	
	for i := range nums {
		if i < len(nums) - 1 && nums[i]+1 == nums[i+1] {
			continue
		}
		if head == i {
			result = append(result, strconv.Itoa(nums[i]))
		} else {
			template := strconv.Itoa(nums[head]) + "->" + strconv.Itoa(nums[i])
			result = append(result, template)
		}
		
		head = i + 1
	}
	
	return result
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Is Subsequence](https://leetcode.com/problems/is-subsequence/description/)

### 1 способ:

- двумя указателями пробегаемся по двум строкам находим одинаковые элементы добавляем и если в конце длина строки первой равна индексу значит нашли все подходящие элементы

```go
func isSubsequence(s string, t string) bool {
	i, j := 0, 0
	
	for i < len(s) && j < len(t) {
		if s[i] == t[j] {
			i++
		}
		j++
	}
	
	return len(s) == i
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Contains Duplicate](https://leetcode.com/problems/contains-duplicate)

### 1 способ:

- пробегаемся циклом по числам и инкрементируем в словарик, если это уже больше одного раза инкрементировалось значит дубликаты есть, если мы добежали до конца значит их нет

```go
func containsduplicate(nums []int) bool {
	seen := make(map[int]int)
	
	for _, value := range nums {
		seen[value]++
		if seen[value] > 1 { return true }
	}
	
	return false
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(n) $$

---
## [Reverse Bits](https://leetcode.com/problems/reverse-bits/)

### 1 способ:

- приводим все к типу uint32 и затем путем переливания битов записываем в новую переменную перевернутое число

```go
func reverseBits(n int) int {
	num := uint32(n)
	var res uint32
	
	for i := 0; i < 32; i++ {
		res = (res << 1) | (num & 1)
		num >>= 1
	}
	
	return int(res)
}
```
time complexity:$$ O(1) $$
space complexity:$$ O(1) $$

---
## [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits)

### 1 способ:

- просто делаем побитовый сдвиг и если крайнее число равно 1 то прибавляем к результату

```go
func hammingWeight(n int) int {
	result := 0
	for i := 0; i < 32; i++ {
		if (n & 1) == 1 {
			result ++
		}
		n >>= 1
	}
	
	return result
}
```
time complexity:$$ O(1) $$
space complexity:$$ O(1) $$

---
## [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle)

### 1 способ:

- метод черепахи и зайчика, один скипает два узла, другой один, если зацикленный список то они встретятся ибо скорость сближения один узел, если заец дойдет до конца значит не зацикленный

```go
func hasCycle(head *ListNode) bool {
	fast := head
	slow := head
	
	for fast != nil && fast.Next != nil {
		slow = slow.Next
		fast = fast.Next.Next
		
		if slow == fast {
			return true
		}
	}
	
	return false
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(1) $$

---
## [Same Tree](https://leetcode.com/problems/same-tree)

### 1 способ:

- при помощи рекурсивной функции просто проверяем значение каждого узла чтобы одни и те же узлы разных деревьев были равны

```go
func isSameTree(p, q *TreeNode) bool {
	if p == nil && q == nil {
		return true
	}
	if p == nil || q == nil {
		return false
	}
	if p.Val != q.Val {
		return false
	}
	
	return isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(h) $$

---
## [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree)

### 1 способ: 

- просто дфс проходимся рекурсивно по дереву и ищем максимальную длину каждого узла

```go
func maxDepth(root *TreeNode) int {
	if root == nil { return 0 }
	
	return 1 + max(maxDepth(root.Left), maxDepth(root.Right))
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(h) $$

---
## [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

### 1 способ:

- рекурсивно так же

```go
func invertTree(root *TreeNode) *TreeNode {
	if root != nil {
		root.Left, root.Right = invertTree(root.Right), invertTree(root.Left)
	}
	
	return root
}
```
time complexity:$$ O(n) $$
space complexity:$$ O(h) $$

---
## [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/description/)

### 1 способ: