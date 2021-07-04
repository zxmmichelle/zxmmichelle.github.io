# Question
**It's Mid-Autumn Festival, you prepare {N} moon cakes, and here's {M} people. Each person should at least get one moon cake, and after sorting the number that the moon cake each person has, the difference between maximum and sub-maximum should not greater than 3. How many ways to distribute the moon cakes?**

*p.s. {1,2,2} is as same as {2,1,2}, and you are not include in this question.*

# My answer
**we assume that there are 3 people and 6 moon cakes.**
- First of all, according to the question, reduce the size of problem, it would be {3,3}. 
- Secondly, asuming that the first person gets 0 moon cake, the second one gets 0 moon cake, the third one gets 3 moon cakes.
 Then, the first one gets 0 moon cake, the second one gets 1 moon cake, the third one gets 2 moon cakes and so on.
 We could build a table like that.
 
| the first one | the second one | the third one |  |
| ---- | ---- | ---- | ---- |
| 0 | 0 | 3 | ✔ |
| 0 | 1 | 2 | ✔ |
| 0 | 2 | 1 | Duplicated |
| 0 | 3 | 0 | Duplicated |
| 1 | 0 | 2 | Duplicated |
| 1 | 1 | 1 | ✔ |
| 1 | 2 | 0 | Duplicated |
| 2 | 0 | 1 | Duplicated |
| 2 | 1 | 0 | Duplicated |
| 3 | 0 | 0 | Duplicated |

> Now, it seems like {3,3} → {2,3}、{2,2}、{2,1}、{2,0}
>       
>                    {2,3} → {1,3}、{1,2}、{1,1}、{1,0}
>                     
>                    {2,2} → {1,2}、{1,1}、{1,0}
>                    
>                    {2,1} → {1,1}、{1,0}
>                   
>                    {2,0} → {1,0}
>  

- Thirdly, work out the data struction
> Because of _"after sorting the number that the moon cake each person has, the difference between maximum and sub-maximum should not greater than 3."_
> 
> The data struction should remember the sequence of moon cakes and deliver the current sequence to the next stage.
>
> So I choose **List<int[]>**

- Now, set the Recursive Method up! 
```java
public static List<int[]> loop(int m, int n, int[] cakeSequence, int currentIndx) {
    List<int[]> list = new ArrayList<int[]>();
    int[] copy = new int[cakeSequence.length];
    copy = Arrays.copyOf(cakeSequence, cakeSequence.length);

    if (Math.abs(n - copy[currentIndx - 1]) > 3) {
        return list;
    }

    if (m == 1) {
        copy[currentIndx] = n;
		    list.add(copy);
		    return list;
    }

    if (n == 1) {
        copy[currentIndx] = 1;
        for (int i = currentIndx + 1; i < copy.length; i++) {
            copy[i] = 0;
        }
        list.add(copy);
        return list;
    } else {
        for (int j = n; j >= 0; j--) {
            if (copy[currentIndx-1] - n + j > 3)
                continue;
            copy[currentIndx] = n - j;
            list.addAll(loop(m - 1, j, copy, currentIndx + 1));
        }
        return list;
    }
}
```
