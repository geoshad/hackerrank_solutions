# Solutions

My notes on HackerRank challenges, 2023. Each challenge is scored by the accuracy of its output, and is tested against various cases. Challenges may be completed in one's programming language of choice; I am using C, due to its heavy use in my university studies.

## Plus Minus 

Plus Minus involves counting the positive, negative, and zero elements in an array, and asks that the final output display the ratios, to the sixth decimal place, of these elements occurring. Finding the solution required cycling through the array via a `for` loop, and checking `if` a given element was less than, more than, or equal to zero, and adding to their given counts.

```
void plusMinus(int arr_count, int* arr) {
    // initialize floats for counting values in array
    float pos_count = 0;
    float neg_count = 0;
    float zero_count = 0;
    
    for (int i = 0; i < arr_count; i++) {
        if (arr[i] > 0) {
            pos_count++;
        } else if (arr[i] < 0) {
            neg_count++;
        } else {
            zero_count++;
        }
    }
    
    // print array ratios with 6 digits after decimal
    printf("%.6f\n", (pos_count/arr_count));
    printf("%.6f\n", (neg_count/arr_count));
    printf("%.6f\n", (zero_count/arr_count));
}
```

## Mini-Max Sum

This problem involves finding the minimum and maximum numbers than can be calculated by summing four out of five given integers. Before calculations, I sorted the array to be in ascending order, so that finding the required integers is more convenient. From there, the minimum sum could be found by cycling through the array from lowest to highest (-1), whilst the maximum sum could be found by cycling from highest to lowest (+1). These sums are then printed on separate lines as per the output requirements.

```
void miniMaxSum(int arr_count, int* arr) {
    int temp = 0;
    long int min = 0, max = 0;
    
    // sort array in ascending order
    for (int i = 0; i < arr_count; i++) {
        for (int j = i + 1; j < arr_count; j++) {
            if (arr[i] > arr[j]) {
                temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp; 
            }
        }
    }
    
    // find min
    for (int i = 0; i < arr_count; i++) {
        min = min + arr[i - 1];
    }
    
    // find max
    for (int j = arr_count; j > 0; j--) {
        max = max + arr[j];
    }
    
    // print values (no lines)
    printf("%lu ", min);
    printf("%lu ", max);
}
```

## Time Conversion

The time conversion challenge requires a function that passes a `string` (`char*`) of time, in 12-hour AM/PM format, and have it returned as a converted, 24-hour military time. 

To complete this task, I first checked if the time needed conversion, by seeing if the "AM/PM" position in the string array had an 'A' or a 'P'; times in PM would require converting. Only the first two indexes in the string array, i.e., the hour, would need to be manipulated, also ensuring that the original "AM/PM" is ignored.

```
char* timeConversion(char* s) {
    bool not_military;

    // check time format of given time
    if (s[8] == 'A') {
        not_military = false;
    } else if (s[8] == 'P') {
        not_military = true;
    }
    
    if(not_military == true)
    {
        if ((s[0] == 0) && (s[1] >= '8')) {
            s[0] +=2;
            s[1] +=2;
        } else if ((s[0] == '1') && (s[1] == '2')) {
            s[0] = '1';
            s[1] = '2';
        } else {
            s[0] += 1;
            s[1] += 2;
        }
    } else {
        if ((s[0] == '1') && (s[1] == '2')) {
            s[0] = '0';
            s[1] = '0';
        }
    }

    s[8] = '\0';
    return s;   
}
```

## Mock Test 1: Find the Median

The first test encountered required a function to pass in an array of integers, and return the median integer. As the example hinted at sorting the array, I applied the same sorting algorithm from the Mini-Max Sum challenge. Once the array was in ascending order, I created a conditional statement to check if the array count was odd or even, by applying a modulo operation. If odd, the median would be `(n+1)/2`; if even, the median would be `(n/2) + (n+1/2)`. As the array count is one count more than the array's indexes, we can adjust this algorithm as needed. 

While the below solution works as intended, it did not pass one of the test cases due to long runtime (~2 seconds); I wonder how to improve its efficiency?

```
int findMedian(int arr_count, int* arr) {
    int temp = 0;
    int median;
    
    for (int i = 0; i < arr_count; i++) {
        for (int j = i + 1; j < arr_count; j++) {
            if (arr[i] > arr[j]) {
                temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
    }
    
    if (arr_count % 2) {
        median = (arr[arr_count/2] + arr[arr_count/2+1])/2;
    } else {
        median = arr[arr_count/2];
    }
    
    return median;
}
```

## Lonely Integer

The following challenge requires a function wherein an array of integers is passed through. Each value occurs twice, excepting for one; this unique element is the one we want returned. 

Initially, I was very unsure of how this was be calculated, but recalling past experiences in Python, the bitwise `XOR` operator could be used to find the value occurring an odd number of times. So, to find this element, a `for` loop cycles through the array and checks for the unique value - a much simpler solution than expected!

```
int lonelyinteger(int a_count, int* a) {
    int unique = 0;
    
    // cycle through array
    for (int i = 0; i < a_count; i++) {
        unique ^= a[i];
    } 
    
    return unique;
}
```

## Diagonal Difference

Diagonal difference is a challenge that asks for function which takes a given square matrix, and calculates the absolute difference between the sums of its diagonals. This would require calculating the sum of the left-to-right diagonal, minus the sum of right-to-left diagonal. 

To find the left diagonals, `int i` would traverse through the matrix array in order to find the values at `[0][0]`, `[1][1]` and `[2][2]`. The `left` variable would add these values to its sum until the end is reached. Conversely, the right diagonal would require summing the values at `[2][0]`, `[1][1]` and `[0][2]` by traversing backwards with `int j`. Finding the difference is simply a matter of subtracting right from left, but requires the absolute value, so `abs()` is applied.

```
int diagonalDifference(int arr_rows, int arr_columns, int** arr) {
    int difference = 0, left = 0, right = 0;

    for (int i = 0; i < arr_rows; i++) {
        left += arr[i][i];
    }
    
    for (int j = arr_rows - 1; j >= 0; j--) {
        right += arr[j][arr_columns - j - 1];
    }
    
    difference = left - right;
    return abs(difference);
}
```

## Counting Sort 1

This challenge requires that for a given a list of integers, count and return the number of times each value appears as an array of integers. The array given is always of size 100, but enough memory should be allocated, to account for any array size. The array is traversed in a `for` loop, and `result` acts an array counting values as it goes through the indexes of `arr`. The array counting the frequency of values is then returned.

```
int* countingSort(int arr_count, int* arr, int* result_count) {
    int* result = malloc(sizeof(int)*arr_count);
    
    for (int i = 0; i < arr_count; i++){
        result[arr[i]] += 1;
    }
    
    *result_count = 100;
    return result;
}
```

## Zig Zag Sequence

The aim of this challenge is simply to debug a given function, `findZigZagSequence`, until it works as intended and succeeds in all test cases. This function permutes an array so it is rearranged into a "zig zag" sequence, wherein the first `k` elements are in ascending order, and the last `k` elements are in descending order. Funnily, there was no code for the C module, so I opted for Python 3 in its stead. The original function is as follows:

```
def findZigZagSequence(a, n):
    a.sort()
    mid = int((n + 1)/2)
    a[mid], a[n-1] = a[n-1], a[mid]

    st = mid + 1
    ed = n - 1
    while(st <= ed):
        a[st], a[ed] = a[ed], a[st]
        st = st + 1
        ed = ed + 1

    for i in range (n):
        if i == n-1:
            print(a[i])
        else:
            print(a[i], end = ' ')
    return
```

The following changes were made: 

1. Line 3: `mid = int((n + 1)/2)` → ` mid = int((n + 1)/2) - 1`
2. Line 7: `ed = n - 1` → `ed = n - 2`
3. Line 11: `ed = ed + 1` → `ed = ed - 1`
