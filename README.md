# Maximum units on a truck
## https://leetcode.com/problems/maximum-units-on-a-truck


You are assigned to put some amount of boxes onto one truck. You are given a 2D array boxTypes, where boxTypes[i] = [numberOfBoxesi, numberOfUnitsPerBoxi]:

numberOfBoxesi is the number of boxes of type i.
numberOfUnitsPerBoxi is the number of units in each box of the type i.

You are also given an integer truckSize, which is the maximum number of boxes that can be put on the truck. You can choose any boxes to put on the truck as long as the number of boxes does not exceed truckSize.

Return the maximum total number of units that can be put on the truck.
```
Example 1:

Input: boxTypes = [[1,3],[2,2],[3,1]], truckSize = 4
Output: 8
Explanation: There are:
- 1 box of the first type that contains 3 units.
- 2 boxes of the second type that contain 2 units each.
- 1 box of the third type that contain 1 unit.
You can take all the boxes of the first and second types, and one box of the third type.
The total number of units will be = (1 * 3) + (2 * 2) + (1 * 1) = 8.

Example 2:

Input: boxTypes = [[5,10],[2,5],[4,7],[3,9]], truckSize = 10
Output: 91
```

## Constraints:
```
1. 1 <= boxTypes.length <= 1000
2. 1 <= numberOfBoxesi, numberOfUnitsPerBoxi <= 1000
3. 1 <= truckSize <= 10^6
```


# Implementation 1 : PriorityQueue (Time Limit Exceeded)
Since we have to maximize the total number of units, the idea is to always take the box which have maximum number of units within it.
So we can put all the boxes in the priority queue, where the box which have more number of units within it, should have more priority.
When we take one box, we have to decrement the box count, and when the count reaches zero we should remove the box from the priority queue.
We should do this as long as queue is not empty and we can put more boxes within the truck.
```java
class Solution {
    public int maximumUnits(int[][] boxTypes, int truckSize) {
        if(boxTypes == null || boxTypes.length == 0)
            return 0;
        
        Queue<int[]> pq = new PriorityQueue<>((b1,b2) -> b2[1]-b1[1]);
        for(int[] box : boxTypes) {
            pq.add(new int[]{box[0], box[1]});
        }
        int maxUnits = 0;
        int boxAdded = 0;
        while(!pq.isEmpty() && boxAdded < truckSize) {
            int[] item = pq.remove();
            maxUnits += item[1];
            boxAdded++;
            if(item[0] > 1) {
                pq.add(new int[]{item[0] - 1, item[1]});
            }
        }
        return maxUnits;
    }
}
```

# Implementation 2 : PriorityQueue (Improvement)
In implementation 1 , we are `removing the top item and adding a new entry in priority queue with one less count` if count was greater than 1. In below approach we are
just updating the count if count was greater than 1, so we are not adding a new entry in priority queue. If count was not greater than 1, in that case we are removing the box from the queue.
```java
class Solution {
    public int maximumUnits(int[][] boxTypes, int truckSize) {
        if(boxTypes == null || boxTypes.length == 0)
            return 0;
        
        Queue<int[]> pq = new PriorityQueue<>((b1,b2) -> b2[1]-b1[1]);
        for(int[] box : boxTypes) {
            pq.add(new int[]{box[0], box[1]});
        }
        int maxUnits = 0;
        int boxAdded = 0;
        while(!pq.isEmpty() && boxAdded < truckSize) {
            int[] box = pq.peek();
            maxUnits += box[1];
            boxAdded++;
            if(box[0] > 1) {
                box[0] = box[0] - 1;
            } else {
                pq.remove();
            }
        }
        return maxUnits;
    }
}
```
