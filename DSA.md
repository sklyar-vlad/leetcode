
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

time complexity: $$ O(n^2) $$
space complexity: $$ O(1) $$
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

time complexity: $$ O(n) $$
space complexity: $$ O(n) $$
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

time complexity: $$ O(n) $$
space complexity: $$ O(n) $$

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

time complexity: $$ O(n) $$
space complexity: $$ O(n) $$
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
space complexity:$$O(1)$$

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

time complexity:$$O(n)$$
space complexity:$$O(1)$$

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

time complexity:$$O((m+n)log(m+n))$$
space complexity:$$O(1)$$
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

time complexity:$$O(n+m)$$
space complexity:$$O(1)$$

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

time complexity:$$O(n \cdot log(n))$$
space complexity:$$O(1)$$
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

time complexity:$$O(n)$$
space complexity:$$O(1)$$

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

time complexity:$$O(n)$$
space complexity:$$O(1)$$

---
