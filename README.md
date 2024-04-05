# 1-Implement a function that checks whether a given string is a palindrome or not.

fn is_palindrome(s: &str) -> bool {
    let s = s.to_lowercase(); // Convert to lowercase to ignore case
    let s = s.chars().filter(|c| c.is_alphanumeric()).collect::<String>(); // Remove non-alphanumeric characters
    let reversed = s.chars().rev().collect::<String>(); // Reverse the string
    s == reversed // Check if the original string is equal to its reverse
}

fn main() {
    let test_string = "A man, a plan, a canal, Panama!";
    if is_palindrome(test_string) {
        println!("{} is a palindrome.", test_string);
    } else {
        println!("{} is not a palindrome.", test_string);
    }
}

# 2-Given a sorted array of integers, implement a function that returns the index of the first occurrence of a given number

fn first_occurrence(arr: &[i32], target: i32) -> Option<usize> {
    let mut low = 0;
    let mut high = arr.len() - 1;
    
    while low <= high {
        let mid = low + (high - low) / 2;
        if arr[mid] == target && (mid == 0 || arr[mid - 1] != target) {
            return Some(mid);
        } else if arr[mid] < target {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    None
}

fn main() {
    let arr = vec![1, 2, 2, 3, 3, 3, 4, 4, 5];
    let target = 3;
    match first_occurrence(&arr, target) {
        Some(index) => println!("First occurrence of {} is at index {}", target, index),
        None => println!("{} not found in the array", target),
    }
}

# 3-Given a string of words, implement a function that returns the shortest word in the string.

fn shortest_word(s: &str) -> Option<&str> {
    s.split_whitespace()                       // Split the string into words
        .min_by_key(|word| word.len())        // Find the word with the shortest length
}

fn main() {
    let input_string = "This is a test string to find the shortest word.";
    match shortest_word(input_string) {
        Some(shortest) => println!("Shortest word: {}", shortest),
        None => println!("No words found in the string."),
    }
}

# 4-Implement a function that checks whether a given number is prime or not.

fn is_prime(n: u64) -> bool {
    if n <= 1 {
        return false; // 0 and 1 are not prime numbers
    }
    if n <= 3 {
        return true; // 2 and 3 are prime numbers
    }
    if n % 2 == 0 || n % 3 == 0 {
        return false; // Numbers divisible by 2 or 3 are not prime
    }
    let mut i = 5;
    while i * i <= n {
        if n % i == 0 || n % (i + 2) == 0 {
            return false; // If divisible by current or current + 2, it's not prime
        }
        i += 6; // All primes greater than 3 can be written in the form 6k ± 1
    }
    true // If not divisible by any number up to square root of n, it's prime
}

fn main() {
    let num = 29;
    if is_prime(num) {
        println!("{} is a prime number.", num);
    } else {
        println!("{} is not a prime number.", num);
    }
}

# 5-Given a sorted array of integers, implement a function that returns the median of the array.

fn find_median(arr: &[i32]) -> Option<f64> {
    let len = arr.len();
    if len == 0 {
        return None; // Return None if the array is empty
    }

    if len % 2 == 0 {
        // If the length of the array is even
        let mid = len / 2;
        Some((arr[mid - 1] + arr[mid]) as f64 / 2.0)
    } else {
        // If the length of the array is odd
        Some(arr[len / 2] as f64)
    }
}

fn main() {
    let arr = [1, 2, 3, 4, 5];
    println!("Median: {:?}", find_median(&arr)); // Output: Median: Some(3)

    let arr2 = [1, 2, 3, 4, 5, 6];
    println!("Median: {:?}", find_median(&arr2)); // Output: Median: Some(3.5)

    let arr3: [i32; 0] = [];
    println!("Median: {:?}", find_median(&arr3)); // Output: Median: None
}


# 6. Implement a function that finds the longest common prefix of a given set of strings.

fn longest_common_prefix(strings: &[String]) -> String {
    if strings.is_empty() {
        return String::new(); // If the input vector is empty, return an empty string
    }
    
    let first_string = &strings[0];
    let mut longest_prefix = String::new();

    'outer: for (i, char) in first_string.chars().enumerate() {
        for string in &strings[1..] {
            if let Some(c) = string.chars().nth(i) {
                if c != char {
                    break 'outer;
                }
            } else {
                break 'outer;
            }
        }
        longest_prefix.push(char);
    }

    longest_prefix
}

fn main() {
    let strings = vec![
        String::from("flower"),
        String::from("flow"),
        String::from("flight"),
    ];
    println!("Longest Common Prefix: {}", longest_common_prefix(&strings)); // Output: Longest Common Prefix: fl

    let strings2 = vec![
        String::from("dog"),
        String::from("racecar"),
        String::from("car"),
    ];
    println!("Longest Common Prefix: {}", longest_common_prefix(&strings2)); // Output: Longest Common Prefix: 
}

# 7. Implement a function that returns the kth smallest element in a given array.

fn kth_smallest(arr: &[i32], k: usize) -> Option<i32> {
    if k == 0 || k > arr.len() {
        return None; // Return None if k is out of bounds
    }
    
    let mut sorted_arr = arr.to_vec();
    sorted_arr.sort(); // Sort the array in ascending order
    
    Some(sorted_arr[k - 1]) // Return the kth smallest element
}

fn main() {
    let arr = [7, 10, 4, 3, 20, 15];
    let k = 3;
    match kth_smallest(&arr, k) {
        Some(val) => println!("{}th smallest element: {}", k, val), // Output: 3rd smallest element: 7
        None => println!("Invalid k value"),
    }

    let arr2 = [7, 10, 4, 3, 20, 15];
    let k2 = 6;
    match kth_smallest(&arr2, k2) {
        Some(val) => println!("{}th smallest element: {}", k2, val), // Output: Invalid k value
        None => println!("Invalid k value"),
    }
}

# 8. Given a binary tree, implement a function that returns the maximum depth of the tree.

// Definition of a binary tree node
#[derive(Debug)]
struct TreeNode {
    val: i32,
    left: Option<Box<TreeNode>>,
    right: Option<Box<TreeNode>>,
}

impl TreeNode {
    fn new(val: i32) -> Self {
        TreeNode { val, left: None, right: None }
    }
}

fn max_depth(root: Option<Box<TreeNode>>) -> i32 {
    match root {
        Some(node) => {
            let left_depth = max_depth(node.left);
            let right_depth = max_depth(node.right);
            1 + left_depth.max(right_depth)
        },
        None => 0,
    }
}

fn main() {
    // Example binary tree
    let root = Some(Box::new(TreeNode {
        val: 3,
        left: Some(Box::new(TreeNode {
            val: 9,
            left: None,
            right: None,
        })),
        right: Some(Box::new(TreeNode {
            val: 20,
            left: Some(Box::new(TreeNode {
                val: 15,
                left: None,
                right: None,
            })),
            right: Some(Box::new(TreeNode {
                val: 7,
                left: None,
                right: None,
            })),
        })),
    }));

    println!("Maximum depth of the tree: {}", max_depth(root)); // Output: Maximum depth of the tree: 3
}

# 9.Reverse a string in Rust
 
fn reverse_string(input: &str) -> String {
    input.chars().rev().collect::<String>()
}

fn main() {
    let input_string = "Hello, World!";
    let reversed_string = reverse_string(input_string);
    println!("Original: {}", input_string);
    println!("Reversed: {}", reversed_string);
}

# 10.Check if a number is prime in Rust

fn is_prime(num: u64) -> bool {
    if num <= 1 {
        return false;
    }
    if num <= 3 {
        return true;
    }
    if num % 2 == 0 || num % 3 == 0 {
        return false;
    }

    let mut i = 5;
    while i * i <= num {
        if num % i == 0 || num % (i + 2) == 0 {
            return false;
        }
        i += 6;
    }

    true
}

fn main() {
    let num = 17; // Change this number to test different values
    if is_prime(num) {
        println!("{} is a prime number", num);
    } else {
        println!("{} is not a prime number", num);
    }
}

# 11.Merge two sorted arrays in Rust by using rust developer

fn merge_sorted_arrays(arr1: &[i32], arr2: &[i32]) -> Vec<i32> {
    let mut merged_array = Vec::new();
    let (mut i, mut j) = (0, 0);

    while i < arr1.len() && j < arr2.len() {
        if arr1[i] < arr2[j] {
            merged_array.push(arr1[i]);
            i += 1;
        } else {
            merged_array.push(arr2[j]);
            j += 1;
        }
    }

    while i < arr1.len() {
        merged_array.push(arr1[i]);
        i += 1;
    }

    while j < arr2.len() {
        merged_array.push(arr2[j]);
        j += 1;
    }

    merged_array
}

fn main() {
    let arr1 = vec![1, 3, 5, 7, 9];
    let arr2 = vec![2, 4, 6, 8, 10];

    let merged_array = merge_sorted_arrays(&arr1, &arr2);

    println!("Merged Array: {:?}", merged_array);
}

# 12.Find the maximum subarray sum in Rust

fn max_subarray_sum(arr: &[i32]) -> i32 {
    let mut max_ending_here = arr[0];
    let mut max_so_far = arr[0];

    for &num in arr.iter().skip(1) {
        max_ending_here = i32::max(num, max_ending_here + num);
        max_so_far = i32::max(max_so_far, max_ending_here);
    }

    max_so_far
}

fn main() {
    let arr = vec![-2, 1, -3, 4, -1, 2, 1, -5, 4];
    let max_sum = max_subarray_sum(&arr);
    println!("Maximum subarray sum: {}", max_sum);
}


