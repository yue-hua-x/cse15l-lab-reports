# part 1: bugs 🐞
bug: `ArrayExamples reversed`  
failure inducing input:
```
  @Test
  public void testReversed1() {
    int[] input1 = {1, 2, 3};
    assertArrayEquals(new int[]{3, 2, 1}, ArrayExamples.reversed(input1));
  }
```
non-failure inducing input:
```
  @Test
  public void testReversed2() {
    int[] input1 = { };
    assertArrayEquals(new int[]{ }, ArrayExamples.reversed(input1));
  }
```
symptom:
![image](https://github.com/yue-hua-x/cse15l-lab-reports/assets/146787492/c7923d78-6530-45dc-a4a6-fa2826b5e42b)

bug:
```
//before
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }
```

```
//after
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
  }
```
# part 2: researching `find`
for all of these examples, i just used the [man page](https://man7.org/linux/man-pages/man1/find.1.html).  
## option 1: `-path`
the `-path` option finds all files such that the path of the file matches the pattern passed after `-path`.
