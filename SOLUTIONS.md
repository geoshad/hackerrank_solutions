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

The time conversion challenge requires a function that passes a `string` (`char*`) of time, in 12-hour AM/PM format, and have it be converted to 24-hour military time. To complete this task, I first checked if the time needed conversion, by seeing if the "AM/PM" position in the string array had an 'A' or a 'P'; times in PM would require converting.

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
    s[9] = '\0';
    
    return s;   
}
```
