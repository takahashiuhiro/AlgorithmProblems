# URL:[Problem - A - Codeforces](https://codeforces.com/contest/1819/problem/A)

# Problem

You have a non-negative array $a$, length of $a$ is $n$ and we define $MEX$(a)  is the smallist non-negative integer that it not in $a$. You are allowed to perform the following operation exactly once: choose some non-empty subsegment $a_l,a_{l+1},…,a_r$ of the array a and a non-negative integer k, and assign value k to all elements of the array on the chosen subsegment.

# Input

Each test contains multiple test cases. The first line contains the number of test cases t (1≤t≤50000) — the number of test cases. The description of the test cases follows.

The first line of each test case contains a single integer n (1≤n≤200000) — the number of elements of array a.

The second line of each test case contains n integers $a_1,a_2,…,a_n (0≤a_i≤10^9)$ — elements of array a.

It is guaranteed that the sum n over all test cases does not exceed 200000.

# Output

For each test case, print "Yes" if you can increase MEX(a) by exactly one by performing the operation from the statement exactly once, otherwise print "No".

You can output the answer in any case (upper or lower). For example, the strings "yEs", "yes", "Yes", and "YES" will be recognized as positive responses.

# Example

#### Input

4
3
1 2 1
4
0 2 2 0
4
3 2 0 2
1
0

#### Output

Yes
Yes
No
No



# Solution

We defined tmex(a) = mex(a) + 1, and there are 2 cases in this solution.

case 1: tmex(a) not in a

In this case, we only need to find res elements  in [0,tmex] and the element > tmex

case 2: tmex(a) in a

in this case, we should define a map to save the range of elements in array a, and then check if the range of tmex covers the range of an element in [0, TMEX)

# Accept Code #[202334516](https://codeforces.com/contest/1819/submission/202334516 "Source")

```
#include <bits/stdc++.h>
using namespace std;

int main()
{
    int T;
    cin>>T;
    int tu[200001];
    int pd[200001];
    map<int,vector<int>>mp;
    while(T--)
    {
        int n;
        cin>>n;
        memset(tu,0,sizeof(int)*n);
        memset(pd,0,sizeof(int)*n);
        mp.clear();
        for(int a=1;a<=n;a++)
        {
            cin>>tu[a];
            pd[a] = tu[a];
        }
        sort(pd+1,pd+1+n);
        int mex = 0;
        for(int a=1;a<=n;a++)
        {
            if(mex == pd[a])
            {
                mex++;
            }
        }
        for(int a=1;a<=n;a++)
        {
            if(!mp[tu[a]].size())
            {
                mp[tu[a]].push_back(a);
                mp[tu[a]].push_back(a);
            }
            mp[tu[a]][0] = min(mp[tu[a]][0],a);
            mp[tu[a]][1] = max(mp[tu[a]][1],a);
            mp[tu[a]].push_back(a);
        } 
        int tmex = mex+1;
        int fg = 0;
        for(int a=1;a<=n;a++)
        {
            if(tmex == tu[a])
            {
                fg = 1;
                break;
            }
        }
        int res = 0;
        if(!fg)
        {
            for(int a=1;a<=n;a++)
            {
                if((tu[a]<tmex && mp[tu[a]][1] != mp[tu[a]][0])||(tu[a]>tmex))
                {
                    cout<<"yes"<<endl;
                    res = 1;
                    break;
                }
            }
            if(res)continue;
            cout<<"no"<<endl;
            continue;
        }
        for(int a = mp[tmex][0];a<=mp[tmex][1];a++)
        {
            if(tu[a]<tmex)
            {
                if(mp[tu[a]][0] >= mp[tmex][0] && mp[tu[a]][1] <= mp[tmex][1])
                {
                    cout<<"no"<<endl;
                    res = 1;
                    break;
                }
            }
        }
        if(res)continue;
        cout<<"yes"<<endl;
        continue;
    }
}
```


