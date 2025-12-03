## 1. Program to Remove Duplicate Elements from an ArrayList

```java
import java.util.*;

public class RemoveDuplicates {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>(Arrays.asList(10, 20, 10, 30, 20, 40));

        ArrayList<Integer> uniqueList = new ArrayList<>();

        for (Integer num : list) {
            if (!uniqueList.contains(num)) {
                uniqueList.add(num);
            }
        }

        System.out.println("Original List: " + list);
        System.out.println("List without duplicates: " + uniqueList);
    }
}
````

---

## 2. Program to Find the Second Largest Number in an ArrayList

```java
import java.util.*;

public class SecondLargest {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>(Arrays.asList(50, 20, 40, 10, 60));

        Collections.sort(list);
        int secondLargest = list.get(list.size() - 2);

        System.out.println("ArrayList: " + list);
        System.out.println("Second Largest Element: " + secondLargest);
    }
}
```

---

## 3. Program to Check if an ArrayList is a Palindrome

```java
import java.util.*;

public class PalindromeList {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3, 2, 1));

        ArrayList<Integer> reversed = new ArrayList<>(list);
        Collections.reverse(reversed);

        if (list.equals(reversed)) {
            System.out.println("The list is a palindrome");
        } else {
            System.out.println("The list is NOT a palindrome");
        }
    }
}
```

---

## 4. Program to Merge Two ArrayLists Without Duplicates

```java
import java.util.*;

public class MergeLists {
    public static void main(String[] args) {
        ArrayList<Integer> list1 = new ArrayList<>(Arrays.asList(10, 20, 30));
        ArrayList<Integer> list2 = new ArrayList<>(Arrays.asList(20, 30, 40, 50));

        ArrayList<Integer> merged = new ArrayList<>(list1);

        for (Integer num : list2) {
            if (!merged.contains(num)) {
                merged.add(num);
            }
        }

        System.out.println("Merged List (No Duplicates): " + merged);
    }
}
```

---

## 5. Program to Find Frequency of Each Element in an ArrayList

```java
import java.util.*;

public class FrequencyCount {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>(Arrays.asList("apple", "banana", "apple", "orange", "banana"));

        HashMap<String, Integer> frequencyMap = new HashMap<>();

        for (String item : list) {
            frequencyMap.put(item, frequencyMap.getOrDefault(item, 0) + 1);
        }

        System.out.println("Frequency of Elements:");
        for (String key : frequencyMap.keySet()) {
            System.out.println(key + " -> " + frequencyMap.get(key));
        }
    }
}
```

