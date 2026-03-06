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


