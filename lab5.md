# part 1: debugging scenario
## student post:
i'm having some issues testing my code; my files (and the test) compile and run fine when i do it manually, but when i run my testing script i get this output:
```
[lroot@server lab_rept5]$ bash ./test.sh 
Error: Main method not found in class TestListExamples, please define the main method as:
   public static void main(String[] args)
or a JavaFX application class must extend javafx.application.Application
```
my bash script looks like this:
```
CPATH='.:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar'

javac -cp $CPATH *.java
java -cp $CPATH TestListExamples
```
i'm not sure what i'm doing wrong? it clearly compiles, because I can find the class files, but it's not running and i don't know why.
## ta response:
the error message states that your class is missing a Main method. when you run your test file, are you supposed to run it directly? (hint: where's your JUnitCore runner?)
## end result:
modified bash script:
```
CPATH='.:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar'

javac -cp $CPATH *.java
java -cp $CPATH org.junit.runner.JUnitCore TestListExamples
```
end result:
```
[lroot@server lab_rept5]$ bash ./test.sh 
JUnit version 4.13.2
.
Time: 0.012

OK (1 test)
```
bug description: they were missing `org.junit.runner.JUnitCore` when running their tests, so they didn't have a `Main` method to run. 
## overall setup:
File/directory structure (not including class files):
```
lab_rept_5/
|-- lib/
|   |-- hamcrest-core-1.3.jar
|   `-- junit-4.13.2.jar
|-- ListExamples.java
|-- TestListExamples.java
`-- test.sh
```
ListExamples.java
```
import java.util.ArrayList;
import java.util.List;

interface StringChecker { boolean checkString(String s); }

class ListExamples {

  // Returns a new list that has all the elements of the input list for which
  // the StringChecker returns true, and not the elements that return false, in
  // the same order they appeared in the input list;
  static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(s);
      }   
    }   
    return result;
  }


  // Takes two sorted list of strings (so "a" appears before "b" and so on),
  // and return a new list that has all the strings in both list in sorted order.
  static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }   
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }   
    }   
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }   
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index2 += 1;
    }   
    return result;
  }


}

```
TestListExamples.java
```
import static org.junit.Assert.*;
import org.junit.*;
import java.util.Arrays;
import java.util.List;

class IsMoon implements StringChecker {
  public boolean checkString(String s) {
    return s.equalsIgnoreCase("moon");
  }
}

public class TestListExamples {
  @Test(timeout = 500)
  public void testMergeRightEnd() {
    List<String> left = Arrays.asList("a", "b", "c");
    List<String> right = Arrays.asList("a", "d");
    List<String> merged = ListExamples.merge(left, right);
    List<String> expected = Arrays.asList("a", "a", "b", "c", "d");
    assertEquals(expected, merged);
  }
}

```
test.sh (initial)
```
CPATH='.:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar'

javac -cp $CPATH *.java
java -cp $CPATH TestListExamples
```
test.sh (final)
```
CPATH='.:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar'

javac -cp $CPATH *.java
java -cp $CPATH org.junit.runner.JUnitCore TestListExamples
```
commands to trigger the bug:
```
[lroot@server lab_rept5]$ bash ./test.sh 
```
how to fix the bug: inserting `org.junit.runner.JUnitCore` between the class path and the test class name in the command to run the tests fixed the bug.
# part 2: reflection
i learned a fair bit about jdb! i started programming in C/C++ and learned enough of gdb to get by, albeit with some difficulty, as i was really only familiar with `break`, `print`, and `step`. i didn't know that you could continue to the next breakpoint, or make the debugger print anything more involved than just the value of a variable (ex: `print hashed` vs. `print hashed.length`).
