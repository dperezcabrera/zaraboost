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

