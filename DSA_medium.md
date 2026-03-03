## [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters)
### 1 способ:

- при помощи скользящего окна и вспомогательного массива где будем указывать последний индекс встречающегося символа

```go
func lengthOfLongestSubstring(s string) int {
	seen := make([]int, 128)
	start, result := 0, 0
	for end := 0; end < len(s); end++ {
		char := s[end]
		if seen[char] > start {
			start = seen[char]
		}
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
space complexity:$$ O(n) $$

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
