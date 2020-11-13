#include<iostream>
#include<cstdlib>
#include<iomanip>
#include<ctime>
#include<chrono>
using namespace std;

template<class T>
void swap(T* arr1, T* arr2) {
    T temp;
    temp = *arr1;
    *arr1 = *arr2;
    *arr2 = temp;
}
template <class T>
void selectSort(T a[], int n) {
    int it, i, mi;
    for (it = 1; it < n; it++) {
        mi = n - it;

        for (i = 0; i < n - it; i++) {
            if (a[mi] < a[i]) {
                mi = i;
            }
        }
        if (mi != n - it) {
            swap(&a[mi], &a[n - it]);
        }
    }
}
template<class T>
void bubbleSort(T a[], int n) {
    int it, i;
    for (it = 1; it < n; it++) {
        for (i = 0; i < n - it; i++) {
            if (a[i] > a[i + 1])
                swap(&a[i], &a[i + 1]);
        }
    }
}

template<class T>
void insertSort(T a[], int n) {
    //i -1st idx of unsorted region
    //loc -idx of insertion in sorted region
    for (int i = 1; i < n; i++) {
        T next = a[i];
        int loc = i;
        while (loc > 0 && a[loc - 1] > next) {
            a[loc] = a[loc - 1];
            loc--;
        }
        a[loc] = next;
    }
}
template<class T>
int partition(T arr[], int low, int high)
{
    int pivot = arr[high]; // pivot  
    int i = (low - 1); // Index of smaller element  

    for (int j = low; j <= high - 1; j++)
    {
        // If current element is smaller than the pivot  
        if (arr[j] < pivot)
        {
            i++; // increment index of smaller element  
            swap(&arr[i], &arr[j]); //swap 
        }
    }
    swap(&arr[i + 1], &arr[high]);
    return (i + 1);
}
template <class T>
void quickSort(T a[], int f, int l) {
    if (f < l) {
        int pIdx = partition(a, f, l);
        quickSort(a, f, pIdx - 1);
        quickSort(a, pIdx + 1, l);
    }
}


template<class T>
void merge(T arr[], int l, int m, int r)
{
    int a = m - l + 1; 
    int b = r - m; 
    //create size a for second half
    //create size b for first half 
    // temp arrays  
    T* L = new T[a];
    T* R = new T[b];

    // Copy data to temp arrays L[] and R[]  
    for (int i = 0; i < a; i++)
        L[i] = arr[l + i];
    for (int j = 0; j < b; j++)
        R[j] = arr[m + 1 + j];

    // Merge the temp arrays back into arr[l..r] 

    // Initial index of first half 
    int i = 0;

    // Initial index of second half 
    int j = 0;

    // Initial index of merged subarray 
    int k = l;

    while (i < a && j < b) //while index of both arrays are less than the sizes 
    {
        if (L[i] <= R[j]) // if element on right array is less than element on left array 
        {
            arr[k] = L[i]; //move to temporary array 
            i++;
        }
        else
        {
            arr[k] = R[j]; //move to temporary array 
            j++;
        }
        k++;
    }

    // Copy the rest of L array 
    while (i < a)
    {
        arr[k] = L[i];
        i++;
        k++;
    }

    // Copy the rest of R array 
    while (j < b)
    {
        arr[k] = R[j];
        j++;
        k++;
    }
}

// l is for left index and r is  
// right index of the sub-array 
// of arr to be sorted 
template<class T>
void mergeSort(T arr[], int l, int r)
{
    if (l < r)
    {
       
        int m = l + (r - l) / 2;

        //sort first and second half  
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);

        merge(arr, l, m, r); //merge 
    }
}

int* intgenerator(int arr[], int size)  //takes in array already created and size, initalize array elements
{
    srand(time(0)); //set a seed 
    for (int i = 0; i < size; ++i) {
        arr[i] = rand() % size + 1; // /set each arr element to random int from 1 to size 
    }
    return arr;
}

int main() {

    int* arr1; //make int arr pointer for dynamic allocation
    int j; //counter

    for (j = 1; j < 6; j++) { // for each iteration of increase power of 10 
        cout << "For " << 10 * pow(10, j) << endl;
        if (j == 5) {
            cout << "For " << ((10 * pow(10, j)) / 4) << endl;
        }
        for (int i = 1; i < 5; i++) {
                                                              
                arr1 = new int[10 * pow(10, j)];                        
                intgenerator(arr1, (10 * pow(10, j)));
   
            auto start = std::chrono::high_resolution_clock::now();

            switch (i) { //print out each algorithim
            case 1:
                selectSort(arr1, (10 * pow(10, j)));                 //divide by last input size of 250k
                cout << "selectSort: ";
                break;
            case 2:
                bubbleSort(arr1, (10 * pow(10, j)));
                cout << "bubbleSort: ";
                break;
            case 3:
                insertSort(arr1, (10 * pow(10, j)) );
                cout << "insertSort: ";
                break;
            case 4:
                quickSort(arr1, 0, (10 * pow(10, j)) );
                cout << "quickSort: ";
                break;
            case 5:
                mergeSort(arr1, 0, (10 * pow(10, j)));
                cout << "mergeSort: ";
                break;

            }
            auto finish = std::chrono::high_resolution_clock::now();
            std::chrono::duration<double> elapsed = finish - start;
            std::cout << elapsed.count() << "s" << endl;
        }
    }

    system("pause");
    return 0;
}









