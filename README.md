# LeetCode_Solved


[1365. How Many Numbers Are Smaller Than the Current Number](https://leetcode.com/problems/how-many-numbers-are-smaller-than-the-current-number/).



        var count : Int = 0 // Variables to contain numbers.
        var result : [Int] = [] // The result array.
        
	//If the number is small while circulating, the count increases one by one
        for num in nums{
            for i in 0..<nums.count{
                if num > nums[i] {
                    count += 1
                }
            }
            result.append(count)
            count = 0
        }
         
        return result

```
// Since these for statements take much longer in time complexity than a single for statement, writing multiple for statements is more efficient in terms of time complexity.

        var count = Array(repeating: 0, count: 101)
        var result = Array(repeating: 0, count:nums.count)

        for i in 0..<nums.count {
            count[nums[i]] += 1
        }
        for i in 1...100 { 
            count[i] += count[i - 1]
        }
        for i in 0..<nums.count { 
            if nums[i] == 0 { 
                result[i] = 0
            } else { 
                result[i] = count[nums[i] - 1]
            }
        }
        return result
```

```
// Best 
class Solution {
    func smallerNumbersThanCurrent(_ nums: [Int]) -> [Int] {
        
      /*
      [] -> []
      [Int] -> [Int]
      
      input: [8,1,2,2,3]      
      
      output: [4,0,1,1,3]
      
      1. sorted array by value in ascending order & remap index
          values = [1, 2, 2, 3, 8]
          indices[value] = min(indices[value], i)
      2. input.map { indices[$0]! } is the answer
      */
      let indices = nums.sorted().enumerated().reversed().reduce(into: [Int: Int]()) { $0[$1.element] = $1.offset }
      return nums.map { indices[$0]! }
    }
}	
```

[617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/).
```
// Definition of TreeNode
//
// public class TreeNode {
//       public var val: Int
//       public var left: TreeNode?
//       public var right: TreeNode?
//      public init() { self.val = 0; self.left = nil; self.right = nil; }
//     public init(_ val: Int) { self.val = val; self.left = nil; self.right = nil; }
//       public init(_ val: Int, _ left: TreeNode?, _ right: TreeNode?) {
//           self.val = val
//           self.left = left
//           self.right = right
//       }
//   }


// Mine
class Solution {
    func mergeTrees(_ root1: TreeNode?, _ root2: TreeNode?) -> TreeNode? {
        // When one tree is empty, the opposite is returned.
        if root1 == nil{
            return root2
        }
        
        if root2 == nil{
            return root1
        }
        
        // total overlapped values and build node
        let value = root1!.val + root2!.val 
        let node = TreeNode(value)
        
        node.left = mergeTrees(root1?.left, root2?.left)
        node.right = mergeTrees(root1?.right, root2?.right)
        
        return node
    }
}

//Best solution

class Solution {
    func mergeTrees(_ root1: TreeNode?, _ root2: TreeNode?) -> TreeNode? {
        guard let root1 = root1 else {
            return root2
        }
        guard let root2 = root2 else {
            return root1
        }
        //The function was performed in addition to the existing value without declaring a new variable.
        root1.val += root2.val
        
        root1.left = mergeTrees(root1.left, root2.left)
        root1.right = mergeTrees(root1.right, root2.right)
        return root1
    }
}
```


[136. Single Number](https://leetcode.com/problems/single-number/).
```
//Mine
class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        //Remove duplicate numbers.
        var numsSet: Set<Int> = []
        
        //If there is a value equal to the set, remove the value.
        for num in nums{
            
// Insert method returns false or true if the added value already exists in the set.
            if numsSet.insert(num).0 == false {
            numsSet.remove(num)
            }
            
        }
        return numsSet.first!
    }
}


```

[543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/).

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?

 *     public init() { self.val = 0; self.left = nil; self.right = nil; }

 *     public init(_ val: Int) { self.val = val; self.left = nil; self.right = nil; }

 *     public init(_ val: Int, _ left: TreeNode?, _ right: TreeNode?) {
 *         self.val = val
 *         self.left = left
 *         self.right = right
 *     }

 * }
 */
//Mine
class Solution {
    
    // Global value of longest length 
    var longest = 0
    
    func diameterOfBinaryTree(_ root: TreeNode?) -> Int {
        //Depth-first search
        dfs(root)
        // Return longest length
        return longest
    }
    
    func dfs(_ node: TreeNode?)->Int{
        // If empty node return 0
        guard let node = node else{
            return 0
        }

        let left = dfs(node.left)
        print(node.left)
       
        let right = dfs(node.right)
        print(node.right)
        

        let sum = left + right
        if longest < sum {
            longest = sum
        }
        
        // Returns a larger value between right and left
        return max(left,right) + 1
    }
}






//Best
class Solution {
	
	private var ans = 0
	
	private func solve(_ root: TreeNode?) -> Int {
		if let root = root {
			let lh = solve(root.left)
			let rh = solve(root.right)
			ans = max(ans, lh + rh)
			return 1 + max(lh, rh)
		} else {
			return 0
		}
	}
	
	func diameterOfBinaryTree(_ root: TreeNode?) -> Int {
		ans = 0
		solve(root)
		return ans
	}
}
```

[283. Move Zeroes](https://leetcode.com/problems/move-zeroes/).

```
//Mine
class Solution {
    func moveZeroes(_ nums: inout [Int]) {
        // know original array count
        let original = nums.count
        
        // know array count from which 0 is removed
        nums = nums.filter { $0 != 0 }
        let differ = original -  nums.count
        
        // append 0s
        nums.append(contentsOf: Array.init(repeating: 0, count: differ))             
    }
}



// Mine(tried)
        var count: Int = 0
        
        for num in nums{
            if num == 0{
                count += 1
            }
        }
        
        nums = nums.filter { $0 != 0 }
        
        for i in 0...count - 1{
            nums.append(0)
        }
//         if nums.contains(0) == false{
//             return 
//         }
//         nums = nums.sorted()
    
//         for num in nums{
//             if nums.first == 0 {
//                 nums.removeFirst()
//                 nums.append(0)
//             }else{
//                 break
//             }
//         }
        
        //nums = nums.filtier{ $0 != 0 }
        
          
        //  for num in nums{
        //     if nums.first == 0 {
        //         nums.removeFirst()
        //         nums.append(0)
        //     }else{
        //         nums.removeFirst()
        //     }
        // }
             
        // for i in 0...nums.count - 1{
        //     print(i)
        //     print(nums[i])
        //     if nums[i] == 0 {
        //         print(nums[i])
        //         nums.append(nums[i])
        //         print(nums)
        //         nums.remove(at:i)    
        //         print(nums)
        //     }
        // }
        
        //  for i in 0...nums.count - 1{
        //     if nums[i] == 0 {
        //         nums.remove(at:i)
        //         nums.append(0)
        //     }
        // }
        
        // Try codes
        // for num in nums{
        //   if num == 0{
        //       nums.remove(object: num)
        //       nums.append(num)
        //   }
        // }



//Best
class Solution {
    func moveZeroes(_ nums: inout [Int]) {
        var writeIdx = 0
        // Move non-zero items
        for num in nums where num != 0 {
            nums[writeIdx] = num
            writeIdx += 1
        }
        // Fill the remaining with zero
        for i in writeIdx..<nums.count {
            nums[i] = 0
        }
    }
}

class Solution {
    func moveZeroes(_ nums: inout [Int]) {
        guard !nums.isEmpty else { return }

        var j = 0

        for i in 0..<nums.endIndex {
            if nums[i] != 0 {
                nums[j] = nums[i]
                if j != i {
                    nums[i] = 0
                }
                j += 1
            }
        }

    }
}
```

