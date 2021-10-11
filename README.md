# LeetCode_Solved


[1365. How Many Numbers Are Smaller Than the Current Number](https://leetcode.com/problems/how-many-numbers-are-smaller-than-the-current-number/).


class Solution {
    func smallerNumbersThanCurrent(_ nums: [Int]) -> [Int] {
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
        
    }
}

// Since these for statements take much longer in time complexity than a single for statement, writing multiple for statements is more efficient in terms of time complexity.

class Solution {
    func smallerNumbersThanCurrent(_ nums: [Int]) -> [Int] {
        
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
    }
}
