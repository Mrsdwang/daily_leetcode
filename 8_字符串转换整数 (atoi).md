```
class Solution {
public:
    int myAtoi(string s) {
        int idx = 0;
        int len = s.size();
        for(int i=0;i<len;++i){
            if(isspace(s[i])) idx++;
            else break;
        }
        bool neg = false;
        bool numstart = false;
        bool sign = false;
        long res = 0;
        for(int i=idx;i<len;++i){
            if(s[i]>='0' && s[i]<='9'){
                if(!numstart && i!=0){
                    if(s[i-1] == '-') neg = true;
                } 
                numstart = true;
                if(res*10+(s[i]-'0') <= INT_MAX) res = res*10+(s[i]-'0');
                else {
                    if(neg) return INT_MIN;
                    else return INT_MAX;
                }
            }
            else{
                if(!numstart && (s[i]=='-'||s[i]=='+')){
                    if(sign) return 0;
                    sign = true;
                }
                else break;
            }
        }
        if(neg&&res-1>=INT_MAX) return INT_MIN;
        else if(res > INT_MAX) return INT_MAX;
        return neg? (int)(0-res):(int)res;
    }
};
```

在做这题的时候，第一次没搞清楚要求。举几个例子，如果+和-有一个不作为符号的时候就直接返回0，例如+-3，此时返回0。如果字母或其他符号出现在数字之前也返回0，例如abc123。对于-0032来说应该返回-32。

首先第一个for先把先导空格给跳过，找到第一个非空格的字符，该字符有可能是数字也有可能是字母或者符号。然后进入下一个for，就是用来找数字的了。在该for中有两种情况，要么是数字要么是其他的字符。我们通过numstart变量来判断是否已经找到数字，用neg变量来判断该数字是否为负的，用sign来判断是否找到一个-或+符号。如果此时遍历到的字符为数字，首先判断numstart是否为true，也即是否之前就遍历到数字了。如果numstart为false，那说明此时是第一次遇到数字，那么首先判断第一个数字前是否有‘-’符号，如果有说明是负数，neg赋值为true。如果numstart为true，说明此时需要获取该数字，那么就是通过res*10+s[i]将该字符转变为数字。但是这里可能出现溢出的情况，因此要首先判断是否大于int的最大值，没有溢出才进行赋值，如果溢出了，那就要判断是否为负数，根据正负来分别返回INT_MIN或者INT_MAX。如果此时遍历到的不是数字而是其他字符，那么也是先判断numstart是否为true，如果之前就已经有了数字，这时在遇到其他字符就应该终止转化了。如果没有开始记录数字，那么就判断这个字符是不是‘+’或者‘-’。这样做的原因是避免出现+-123的情况，出现两次符号是应该返回0的。

最后就判断是否为负数，并且超过了INT型的最小范围，或是超过了最大范围。
