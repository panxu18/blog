#### KMP算法

KMP算法能解决问题为：给一个主串S，模式串P，计算S中是否包含P，以及出现的位置。

当模式串P和主串S从S[i]匹配到长度L时失配，如果使用暴力匹配则需要从S[i+1]重新匹配，匹配位置在主串上出现回退。当模式串和主串S匹配长度L失配，那么从S[i]开始长度为L-1的子串与P的前L-1个字符是相同的，所以实际上重新匹配时模式串P和主串的S[i+1]匹配等价于模式串P和模式串的子串P[1]的匹配。利用这个现象可以提前计算出关于模式串P的一些匹配规则，可以加快匹配速度。

KMP算法计算的是失配数组fail[i]，即S[i]与P[j]匹配不上之后，下一个需要匹配的是S[i]和P[fail[j]]。fail[i]和fail[i+1]之间存在联系，例如fail[i]=5，那么就有两种情况

1. 如果P[i]与P[5]相同，fail[i+1] = 6。
2. P[i]与P[5]不相同，那么接着比较P[i]和P[fail[5]]。

算法实现

```java
public class KMP {

   /**
     * 计算公共前后缀
     */
    private int[] getPrefix(char[] arr) {
        int[] fail = new int[arr.length];
        int[] prefix = new int[arr.length];
        fail[0] = -1;
        int j = -1;
        for (int i = 0; i < arr.length - 1; i++) {
            while (j != -1 && arr[i] != arr[j]) {
                j = fail[j];
            }
            prefix[i] = j;
            fail[i + 1] = arr[i + 1] == arr[++j] ? fail[j] : j;
        }
        // 计算prefix[arr.length-1]
        while (j != -1 && arr[arr.length - 1] != arr[j]) {
            j = fail[j];
        }
        prefix[arr.length - 1] = j;
        return prefix;
    }

    /**
     * 计算失败指针
     */
    private static int[] getFail(char[] arr) {
        int[] fail = new int[arr.length];
        fail[0] = -1;
        int j = -1;
        for (int i = 0; i < arr.length - 1; i++) {
            while (j != -1 && arr[i] != arr[j]) {
                j = fail[j];
            }
            fail[i + 1] = arr[i + 1] == arr[j] ? fail[j] : j;
        }
        return fail;
    }

    private static int search(char[] source, char[] pattern, int[] fail) {
        int i = 0, j = 0;
        while (i < source.length && j < pattern.length) {
            if (j == -1 || source[i] == pattern[j]) {
                i++; j++;
            } else {
                j = fail[j];
            }
        }
        if (j >= pattern.length) {
            return i - pattern.length;
        } else {
            return -1;
        }
    }

    public static int kmp(String source, String pattern) {
        char[] sourceArr = source.toCharArray();
        char[] patternArr = pattern.toCharArray();
        if (sourceArr.length < patternArr.length) {
            char[] temp = sourceArr;
            sourceArr = patternArr;
            patternArr = temp;
        }
        int[] prefix = getFail(patternArr);
        return search(sourceArr, patternArr, prefix);
    }

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String source = in.nextLine();
        String pattern = in.nextLine();
        System.out.println(kmp(source, pattern));
    }
}
```

#### 扩展KMP

扩展KMP解决的问题为：给一个主串S，一个模式串P，找出主串S中所有匹配子串。

扩展KMP计算的是所有后缀子串的公共前缀，即对所有的S[i, N]与P的公共前缀，同样根据字符匹配过程中的规律，假设从S[i]开始与P比较的公共前缀长度为L，那么S[i+1]-S[i+L]与P[1]-P[L]是一样的，因此实际上就是P[1]-P[L]与模式串的比较，同样的S[i+2]-S[i+L]对应的是P[2]-P[L]。所以这部分匹配就是模式串自身的匹配，可以提前计算匹配结果，根据预处理的prefix数组，可以减少匹配次数。

首先对模式串计算所有后缀子串的公共前缀prefix[i]，上面分析过了，在计算prefix[i+k]时，P[i+k]-P[i+L] 与P[k]-P[L]是相同的，所以有以下情况：

1. prefix[k] + k < L, 匹配的子串不超过P[i+L]，那么prefix[i+k]=prefix[k] 。
2. prefix[k] + k >= L,P[i+k]-P[L]都能匹配，那么需要从P[i+L]和P[L-k]开始匹配。

算法实现

```java
public class EXKMP {

    private static int[] getPrefix(char[] pattern) {
        int[] prefix = new int[pattern.length];
        prefix[0] = pattern.length;

        int pos = 1;
        int len = 0;
        while (pos + len < pattern.length && pattern[pos + len] == pattern[len]) {
            len++;
        }
        prefix[pos] = len;

        for (int i = 2; i < pattern.length; i++) {
            if (i < pos + len && prefix[i - pos] + i < pos + len) {
                prefix[i] = prefix[i - pos];
            } else {
                len = Math.max(pos + len - i, 0);
                while (i + len < pattern.length && pattern[i + len] == pattern[len]) {
                    len++;
                }
                pos = i;
                prefix[pos] = len;
            }
        }
        return prefix;
    }

    private static int[] search(char[] source, char[] pattern, int[] prefix) {
        int[] extend = new int[source.length];
        int pos = 0;
        int len = 0;
        while (pos + len < source.length && len < pattern.length && source[pos + len] == pattern[len]) {
            len++;
        }
        extend[pos] = len;

        for (int i = 1; i < source.length; i++) {
            if (i < pos + len && prefix[i - pos] + i < pos + len) {
                extend[i] = prefix[i - pos];
            } else {
                len = Math.max(pos + len - i, 0);
                while (i + len < source.length && len < pattern.length && source[i + len] == pattern[len]) {
                    len++;
                }
                pos = i;
                extend[pos] = len;
            }
        }
        return extend;
    }

}
```



