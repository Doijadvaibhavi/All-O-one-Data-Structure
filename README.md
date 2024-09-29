# All-O-one-Data-Structure

Design a data structure to store the strings' count with the ability to return the strings with minimum and maximum counts.

Implement the AllOne class:

AllOne() Initializes the object of the data structure.
inc(String key) Increments the count of the string key by 1. If key does not exist in the data structure, insert it with count 1.
dec(String key) Decrements the count of the string key by 1. If the count of key is 0 after the decrement, remove it from the data structure. It is guaranteed that key exists in the data structure before the decrement.
getMaxKey() Returns one of the keys with the maximal count. If no element exists, return an empty string "".
getMinKey() Returns one of the keys with the minimum count. If no element exists, return an empty string "".
Note that each function must run in O(1) average time complexity.

 

Example 1:

Input
["AllOne", "inc", "inc", "getMaxKey", "getMinKey", "inc", "getMaxKey", "getMinKey"]
[[], ["hello"], ["hello"], [], [], ["leet"], [], []]
Output
[null, null, null, "hello", "hello", null, "hello", "leet"]

Explanation
AllOne allOne = new AllOne();
allOne.inc("hello");
allOne.inc("hello");
allOne.getMaxKey(); // return "hello"
allOne.getMinKey(); // return "hello"
allOne.inc("leet");
allOne.getMaxKey(); // return "hello"
allOne.getMinKey(); // return "leet"
 

Constraints:

1 <= key.length <= 10

key consists of lowercase English letters.

It is guaranteed that for each call to dec, key is existing in the data structure.

At most 5 * 104 calls will be made to inc, dec, getMaxKey, and getMinKey.


# SOLUTION

Intuition:
The goal of the All O`one Data Structure is to keep track of string counts and efficiently retrieve the minimum and maximum counts. We need to ensure all operations — increment, decrement, and retrieval of the min/max key — operate in constant time, on average. This requires maintaining a balance between count tracking and efficient querying for min/max keys.

Approach:
Count Map: Use a hash map (unordered_map) to store the count of each string.
Sorted Set: Use a set to maintain a sorted order of counts and corresponding keys. The set will be based on a pair of (count, key), ensuring the counts are sorted, and if two strings have the same count, their lexicographical order is preserved.
Increment/Decrement Operations:
When incrementing, we update the count in the map and update the set by removing the old count and inserting the new count.
When decrementing, we do the reverse. If the count drops to zero, we remove the key from both the map and set.
GetMaxKey/GetMinKey: Since the set is sorted, the first element gives the minimum, and the last element gives the maximum.

Complexity:

Time Complexity: Each operation (inc, dec, getMaxKey, getMinKey) runs in O(1) on average, as insertion and deletion in a set (backed by a balanced binary search tree) are logarithmic but constant on average due to limited keys and operations.

Space Complexity: O(N), where N is the number of unique keys.

# JAVA CODE

class AllOne {

    private Map<String, Integer> count;
    
    private TreeSet<Pair<Integer, String>> set;

    public AllOne() {
    
        count = new HashMap<>();
        
        set = new TreeSet<>((a, b) -> a.getKey().equals(b.getKey()) ? a.getValue().compareTo(b.getValue()) : a.getKey() - b.getKey());
    }

    public void inc(String key) {
    
        int n = count.getOrDefault(key, 0);
        
        count.put(key, n + 1);
        
        set.remove(new Pair<>(n, key));
        
        set.add(new Pair<>(n + 1, key));
    }

    public void dec(String key) {
    
        int n = count.get(key);
        
        set.remove(new Pair<>(n, key));
        
        if (n == 1) count.remove(key);
        
        else {
        
            count.put(key, n - 1);
            
            set.add(new Pair<>(n - 1, key));
            
        }
    }

    public String getMaxKey() {
    
        return set.isEmpty() ? "" : set.last().getValue();
        
    }

    public String getMinKey() {
        return set.isEmpty() ? "" : set.first().getValue();
    }
    
}

Step-by-Step Detailed Explanation:

Initialization (AllOne constructor):

We initialize two structures:

count: a hash map to store the count of each string.
set: a sorted set or a tree structure to store pairs of (count, string). This will allow us to retrieve the minimum and maximum counts in constant time.
Increment Operation (inc method):

Retrieve the current count of the string from the count map (if it doesn't exist, treat it as 0).
Increment the count of the string in the count map.
Remove the old count of the string from the set (if it exists).
Insert the new count of the string back into the set.
This ensures the set remains up to date with the new count for the string.
Decrement Operation (dec method):

Retrieve the current count of the string.
Decrement the count of the string in the count map.
Remove the old count of the string from the set.
If the count of the string becomes 0, remove the string from the count map entirely. Otherwise, update the set with the new count.
Getting the Maximum Key (getMaxKey method):

The set stores the keys in a sorted order by their count. The last element of the set gives us the string with the maximum count.
If the set is empty, return an empty string.
Getting the Minimum Key (getMinKey method):

The set stores the keys in a sorted order by their count. The first element of the set gives us the string with the minimum count.
If the set is empty, return an empty string.
By maintaining both the count map and the set, we ensure that we can increment, decrement, and retrieve the min/max key in O(1) average time. This combination allows us to handle dynamic changes to the string counts while efficiently querying for the highest and lowest counts.

Visualization:
Imagine a scenario where we have strings, each with a count of how many times they've been incremented or decremented. To make this clearer, visualize it as a table and a sorted list.

Count Map (count):

Think of this as a table where each string has a corresponding count:
+-------+-------+
| Key   | Count |
+-------+-------+
| hello |   2   |
| leet  |   1   |
+-------+-------+
Sorted Set (set):

Visualize the set as a sorted list where each entry is a pair of (count, string). This allows us to easily find the smallest and largest counts:
(1, "leet")
(2, "hello")
The first element, (1, "leet"), represents the minimum count. The last element, (2, "hello"), represents the maximum count.
Increment Operation Example:

Let's say you call inc("hello"). This increments the count of "hello" from 2 to 3. The new table and set will look like this:
+-------+-------+
| Key   | Count |
+-------+-------+
| hello |   3   |
| leet  |   1   |
+-------+-------+

(1, "leet")
(3, "hello")
The set now reflects the updated count for "hello."
Decrement Operation Example:

If you call dec("hello"), the count of "hello" will decrease from 3 to 2, and the set will update accordingly:
+-------+-------+
| Key   | Count |
+-------+-------+
| hello |   2   |
| leet  |   1   |
+-------+-------+

(1, "leet")
(2, "hello")
Max and Min Key Retrieval:

After updates, the first element of the set gives you the minimum key ("leet"), and the last element gives you the maximum key ("hello").
This visualization helps illustrate how the count map and set work together to maintain and update counts, while allowing efficient retrieval of minimum and maximum keys.
