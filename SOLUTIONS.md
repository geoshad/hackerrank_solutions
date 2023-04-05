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
