## 1. Collecting items

We want to implement an algorithm to calculate the length of the largest subset of array elements where the maximum distance between the highest and lowest number is 1.

#### Example

`items = [2,4,3,4,2,4]`

In this list, there are two subsets matching the described criterion: `[2,2,3]` and `[3,4,4,4]`. The subset that has more items is the second. So the correct answer is `4`.
<br/>

#### Function description

Complete the getMaximumNumberItems function in the editor. 
getMaximumNumberItems has the following parameters:

   `items[]: an array of integers`

<br/>

#### Return

The maximum number of adjacent items.
<br/>

#### Input format

A line containing space-separated elements (integer) of a list, `items[n]` 

<br/>

#### Constraints

* 2 <= n <= 100
* 0 <= items[n] < 100
* The answer will be >= 1

<br/>
<br/>

#### Test Case 0

**Input 0**

`1 2 4 2 5`

**Output 0**

`3`

**Explanation 0**

We choose elements from the array: `{1, 2, 2}`. Each pair in the set has an absolute difference <= 1 (e.g: |1 - 2| = 1, |2 - 2| = 0, |2 - 1| = 1), so our answer is `3`.

<br/>

#### Test Case 1

**Input 1**

`4 5 6 7 3 9 4 5 9`

**Output 1**

`4`

**Explanation 1**

We choose elements from the array: `{4, 5, 4, 5}`. Each pair in the set has an absolute difference <= 1 (e.g: |4 - 5| = 1, |5 - 4| = 1, |4 - 4| = 0, |5 - 5| = 0), so our answer is `4`.


<br/>
<br/>

### Solution:

**Javascript (short)**
```js

function getMaximumNumberItems(items) {
    const occurrences = items.reduce((map, e) => map.set(e, (map.get(e) || 0) + 1), new Map());
    return [...occurrences.entries()].reduce((r, e) => Math.max(r,  e[1] + (occurrences.get(e[0] + 1) || 0)), 0);
}

```
<br/>
<br/>

**Java (short)**
```java

public static int getMaximumNumberItems(List<Integer> items) {
    var occurrences = items.stream().collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    return occurrences.entrySet().stream()
            .map(e -> e.getValue() + occurrences.getOrDefault(e.getKey() + 1, 0L))
            .max(Long::compare).orElse(0L).intValue();
}

```
<br/>
<br/>

**Python (short)**
```python

def getMaximumNumberItems(items):
    occurrences = collections.Counter(items)
    return max([occurrences[k]+occurrences[k+1] for k in occurrences])

```
    
<br/>
<br/>


**C# (short)**
```c#

public static int GetMaximumNumberItems(int[] items) {
    var occurrences = items.GroupBy(i => i).ToDictionary(g => g.Key, g => g.Count());
    return occurrences.Keys.Max(k => occurrences[k] + occurrences.GetValueOrDefault(k + 1, 0));
}

```
    
<br/>
<br/>

**C (short)**
```c

int getMaximumNumberItems(int items[], int n) {
    int occurrences[101] = {0};
    for(int i = 0; i < n; i++) {
        occurrences[items[i]]++;
    }
    int result = 0;
    for(int i = 0; i < 100; i++) {
        int length = occurrences[i] + occurrences[i + 1];
        if(length > result) {
            result = length;
        }
    }
    return result;
}

```
    
<br/>
<br/>

**C++ (short)**
```c++

int getMaximumNumberItems(int items[], int n) {
    unordered_map<int, int> occurrences;
    for(int i = 0; i < n; i++) {
        occurrences[items[i]]++;
    }
    int result = 0;
    for(auto &[value, count] : occurrences) {
        result = max(result, count + occurrences[value + 1]);
    }
    return result;
}

```
    
<br/>
<br/>

**PHP (short)**
```php

function getMaximumNumberItems($items) {
    $occurrences = array_count_values($items);
    $result = 0;
    foreach($occurrences as $value => $count) {
        $length = $count + ($occurrences[$value + 1] ?? 0);
        $result = max($result, $length);
    }
    return $result;
}

```
    
<br/>
<br/>

**Javascript**

```js

function getMaximumNumberItems(items) {
    let ocurrences = new Map();
    for (let i = 0 ; i < items.length; i++) {
        let value = items[i];
        if (ocurrences.has(value)) {
            let counter = ocurrences.get(value);
            ocurrences.set(value, counter + 1);
        } else {
            ocurrences.set(value, 1);
        }
    }
    let result = 0;
    for (const [ value, occurrence ] of ocurrences.entries()) {
        let length = occurrence;
        if (ocurrences.has(value + 1)) {
            length += ocurrences.get(value + 1);
        }
        result = Math.max(result, length);
    }
    return result;
}

```

<br/>
<br/>

**Java**
```java

public static int getMaximumNumberItems(List<Integer> items) {
    var occurrences = new HashMap<Integer, Integer>();
    for (var value : items) {
        if (occurrences.containsKey(value)) {
            var counter = occurrences.get(value);
            occurrences.put(value, counter + 1);
        } else {
            occurrences.put(value, 1);
        }
    }
    int result = 0;
    for (var entry : occurrences.entrySet()) {
       int length = entry.getValue();
       if (occurrences.containsKey(entry.getKey() + 1)) {
           length += occurrences.get(entry.getKey() + 1);
       }
       result = Math.max(result, length);
    }
    return result;
}

```
<br/>
<br/>

**Python**
```python

def getMaximumNumberItems(items):
    occurrences = {}
    for value in items:
        if value in occurrences:
            occurrences[value] += 1
        else:
            occurrences[value] = 1
        
    result = 0
    for value in occurrences:
        length = occurrences[value];
        if value + 1 in occurrences:
            length +=  occurrences[value+1] 
        result = max(result, length)
    return result

```
<br/>
<br/>

**Go**
```go

func getMaximumNumberItems(items []int) int {
	occurrences := make(map[int]int)
	for _, item := range items {
		occurrences[item]++
	}

	maxCount := 0
	for value, count := range occurrences {
		maxCount = max(maxCount, count+occurrences[value+1])
	}
	return maxCount
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

```
<br/>
<br/>

**Rust**
```rust

fn get_maximum_number_items(items: &[i32]) -> i32 {
    let mut occurrences = HashMap::new();
    for &item in items.iter() {
        *occurrences.entry(item).or_insert(0) += 1;
    }

    occurrences.keys().cloned().map(|k| occurrences[&k] + *occurrences.get(&(k+1)).unwrap_or(&0)).max().unwrap_or(0)
}

```

<br/>
<br/>

**Ruby**
```ruby

def get_maximum_number_items(items)
  occurrences = items.group_by(&:itself).transform_values(&:count)
  occurrences.keys.map { |k| occurrences[k] + occurrences[k + 1].to_i }.max
end

```

<br/>
<br/>

**Groovy**
```groovy

def getMaximumNumberItems(items) {
    def occurrences = items.groupBy { it }.collectEntries { [(it.key): it.value.size()] }
    occurrences.keySet().collect { k -> occurrences[k] + (occurrences[k + 1] ?: 0) }.max()
}

```

<br/>
<br/>

**Kotlin**
```kotlin

fun getMaximumNumberItems(items: List<Int>): Int {
    val occurrences = items.groupingBy { it }.eachCount()
    return occurrences.keys.map { k -> occurrences[k]!! + (occurrences[k + 1] ?: 0) }.maxOrNull() ?: 0
}

```
<br/>
<br/>

**Swift**
```swift

func getMaximumNumberItems(_ items: [Int]) -> Int {
    let occurrences = Dictionary(items.map { ($0, 1) }, uniquingKeysWith: +)
    return occurrences.keys.map { key in
        return occurrences[key, default: 0] + occurrences[key + 1, default: 0]
    }.max() ?? 0
}

```


<br/>
<br/>

**Pascal**
```pascal

function GetMaximumNumberItems(Items: array of Integer): Integer;
var
    Occurrences: TFPList;
    I, Value, MaxCount: Integer;
begin
    Occurrences := TFPList.Create;
    try
        for I := 0 to High(Items) do
        begin
            Value := Items[I];
            if Occurrences.IndexOf(Pointer(Value)) = -1 then
                Occurrences.Add(Pointer(1))
            else
                Occurrences[Occurrences.IndexOf(Pointer(Value))] := Pointer(Integer(Occurrences[Occurrences.IndexOf(Pointer(Value))]) + 1);
        end;

        MaxCount := 0;
        for I := 0 to Occurrences.Count - 1 do
        begin
            Value := Integer(Occurrences[I]);
            if Occurrences.IndexOf(Pointer(Value + 1)) <> -1 then
                Value := Value + Integer(Occurrences[Occurrences.IndexOf(Pointer(Value + 1))]);
            if Value > MaxCount then
                MaxCount := Value;
        end;

        Result := MaxCount;
    finally
        Occurrences.Free;
    end;
end;
```
