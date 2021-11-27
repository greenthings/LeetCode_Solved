# LeetCode_Solved


[1365. How Many Numbers Are Smaller Than the Current Number](https://leetcode.com/problems/how-many-numbers-are-smaller-than-the-current-number/).



        var count : Int = 0 // Variables to contain numbers.
        var result : [Int] = [] // The result array.
        
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


Best solution

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
