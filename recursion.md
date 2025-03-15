# How to think recursively

1. Base Case
2. Assume kr lena ki choti problem ke liye chal jayega. Isko question nahi krna h. Choti problem ka solution recursion de dega.
3. Choti problem ka result use krke bari problem ko solve kr lena.

# Note

- PMI ki tarah sochna h.
  1. Base case.
  2. f(n-1) is true [Choti Problem apne aap ho jayegi] (Induction Hypothesis).
  3. Using f(n-1) solve for f(n). (Induction Step)
- Extended Form of PMI
  1. Base case: Prove F(0) or F(1) is true.
  2. Assume F(i) is true for all 0<= i <= k. (Induction Hypothesis)
  3. Use Induction Hypothesis (Step 2) to prove F(k+1) is true.

# Find the length of string using recursion

```
int strLength(string str) {
    if (str.size() == 0) return 0;
    int smallAns = strLength(str.substr(1));
    return smallAns + 1;
}
```

# Remove all 'x' from string using recursion

```
string removeX(string str) {
    if (str.size() == 0) return "";
    if (str[0] != 'x') {

        return str[0] + removeX(str.substr(1));

    }
    else {
        int i;
        for (i = 1; i < str.size(); i++) str[i - 1] = str[i];
        str.pop_back();
        return removeX(str);
    }
}
void solve() {
    string str; cin >> str;
    str = removeX(str);
    cout << str;
}
```

# Given a string S, remove consecutive duplicates from it recursively.

```
#include<bits/stdc++.h>
using namespace std;
string rd(string str) {
    if (str.size() == 0) return "";
    if (str.size() == 1) return str;
    char cur = str[0];
    if (str[1] == cur) {
        return rd(str.substr(1));
    }

    return cur + rd(str.substr(1));
}
int main(){

    int t; cin>>t;
    while(t--){
        string str; cin>>str;
        str=rd(str);
        cout<<str<<'\n';
    }
    return 0;
}
```

# Subsequences

Method 1

```
int subs(string str, vector<string>&output) {
    if (str == "" or str.size() == 0) {
        output.push_back(str);
        return 1;
    }

    char cur = str[0];
    int smallAns = subs(str.substr(1), output);
    for (int i = 0; i < smallAns; i++) output.push_back(cur + output[i]);

    return output.size();
}
void solve() {
    string str; cin >> str;
    vector<string> output;
    int l = subs(str, output);
    for (int i = 0; i < l; i++) cout << output[i] << '\n';
}
```

Method 2

```
void subs(string input, string output) {
    if (input == "") {
        cout << output << '\n';
        return;
    }
    subs(input.substr(1), output); // Not includes first char
    // Includes first char
    output += input[0];
    subs(input.substr(1), output);

}
void solve() {
    string input; cin >> input;
    int pos = 0;
    string output = "";
    subs(input, output);

}
```

# KeyPad

## Problem

Given an integer n, using phone keypad find out all the possible strings that can be made using digits of input n.
Return empty string for numbers 0 and 1.
Note : 1. The order of strings are not important.

## Sample Input

```
1
23
```

## Sample Output

```
ad
ae
af
bd
be
bf
cd
ce
cf
```

Method 1

```
#include<bits/stdc++.h>
using namespace std;
#define int long long
string keyPad[] = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
int kp(int n, vector<string>&output) {
    if n == 0 or n==1 or n==10 or n==11) {
        output.push_back("");
        return 1;
    }
    if(n%10!=0 and n%10!=1){
        int smallAns = kp(n / 10, output);
    string cur = keyPad[n % 10];
    if (cur == "") return output.size();
    for (int i = 0; i < smallAns; i++) {
        int k = 0;
        for (k = 0; k < cur.size() - 1; k++) {
            output.push_back(output[i] + cur[k]);
        }
        output[i] = output[i] + cur[k];
    }
    }
    return output.size();
}
int32_t main(){

   int t; cin>>t;
    while(t--){
    int n; cin>>n;

    vector<string> output;
    int l = kp(n, output);
    if(l==0) cout<<endl;
    for (int i = 0; i < l; i++) cout << output[i] << '\n';
    }
    return 0;
}
```

Method 2

```
string keyPad[] = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
void kp(int n, string output) {
    if (n == 0 or n == 1 or n == 10 or n == 11) {
        cout << output << '\n';
        return;
    }
    int d = n % 10;
    if (d == 0 or d == 1) kp(n / 10, output);
    else {
        string cur = keyPad[d];
        for (int i = 0; i < cur.size(); i++) kp(n / 10, cur[i] + output);
    }
}
void printKeypad(int num){

     string output="";
     kp(num, output);

}
```

# MergeSort

```
#include<bits/stdc++.h>
using namespace std;
void merge(vector<int>&arr,int s,int mid,int e){
    int n1=mid-s+1,n2=e-mid;
    vector<int>a(n1),b(n2);
    for(int i=0; i<n1; i++) a[i]=arr[s+i];
    for(int i=0; i<n2; i++) b[i]=arr[mid+1+i];
    int i=0,j=0,k=s;
    while(i<n1 and j<n2){
        if(a[i]<b[j]) arr[k++]=a[i++];
        else arr[k++]=b[j++];
    }
    while(i<n1) arr[k++]=a[i++];
    while(j<n2) arr[k++]=b[j++];
}
void mergesort(vector<int>&a ,int s,int e){
    if(s>=e) return;
    int mid=s+(e-s)/2;
    mergesort(a,s,mid);
    mergesort(a,mid+1,e);
    merge(a,s,mid,e);
}
int main(){

   int t; cin>>t;
	while(t--){
        int n; cin>>n;
        vector<int>a(n);
        for(int i=0; i<n; i++) cin>>a[i];
        mergesort(a,0,n-1);
        for(int i=0; i<n; i++) cout<<a[i]<<" ";
        cout<<'\n';
    }
    return 0;
}
```

# QuickSort

```
#include<bits/stdc++.h>
using namespace std;
int partition(vector<int>&a,int s,int e){
    int pivot=a[e];
    int i=s-1;
    for(int j=s; j<=e-1; j++){
        if(a[j]<pivot){
            i++;
            swap(a[i],a[j]);
        }
    }
    swap(a[i+1],a[e]);
    return i+1;
}
void quicksort(vector<int>&a,int s,int e){
    if(s>=e) return;
    int pos=partition(a,s,e);
    quicksort(a,s,pos-1);
    quicksort(a,pos+1,e);
}
int main(){

    int t; cin>>t;
    while(t--){
        int n; cin>>n;
        vector<int>a(n);
        for(int i=0; i<n; i++) cin>>a[i];
        quicksort(a,0,n-1);
        for(int i=0; i<n; i++) cout<<a[i]<<" ";
        cout<<endl;
    }
    return 0;
}
```