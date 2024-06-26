# AXX-TryCatch
Page 103 Listing 61 AXX-TryCatch/TryCatch.java 

Cont. from [Section 11. Exceptions 60.AXW-ScanExcept](https://github.com/Java-PJATK/60.AXW-ScanExcept/tree/main)

In the example below, we read data from the user and use (unchecked) exception of type `NumberFormatException` to ensure that the data is valid:

## Listing 61 AXX-TryCatch/TryCatch.java

```java
import javax.swing.JOptionPane;

public class TryCatch {
    public static void main(String[] args) {
        boolean noGood = true;
        int     number = 0;

        while (noGood) {
            String s = JOptionPane.showInputDialog(
                null,"Enter an integer","Entering data",
                JOptionPane.QUESTION_MESSAGE);

            if (s == null)       break;
            if (s.length() == 0) continue;
            try {
                number = Integer.parseInt(s);
                noGood = false;
            } catch(NumberFormatException e) {
                JOptionPane.showMessageDialog(
                    null,"<html>This is not an integer!" +
                         "<br />Try again!!!",
                    "ERROR",JOptionPane.ERROR_MESSAGE);
            }
        }

        System.out.println(
            noGood ?
                "Program cancelled" : "Entered: " + number);
    }
}
```

The following example demonstrates the use of two different unrelated exceptions:

## Listing 62 AYC-Excpts/Excpts.java

```java
import javax.swing.JOptionPane;

public class Excpts {
    public static void main(String[] args) {
        boolean noGood = true;
        int     number = 0;

        while (noGood) {
            String s = JOptionPane.showInputDialog(
                null,"Enter two integers","Entering data",
                JOptionPane.QUESTION_MESSAGE);

            if (s == null) break;
            s = s.trim();
            if (s.length() == 0) continue;

            int spac = s.indexOf(' ');
            if (spac < 0) {
                JOptionPane.showMessageDialog(
                    null,"<html>Wrong data!" +
                         "<br />Try again!!!",
                    "ERROR",JOptionPane.ERROR_MESSAGE);
                continue;
            }

            try {
                int n1 = Integer.parseInt(
                                    s.substring(0,spac));
                int n2 = Integer.parseInt(
                             s.substring(spac+1).trim());
                number = n1/n2;
                noGood = false;

            } catch(NumberFormatException e) {
                JOptionPane.showMessageDialog(
                    null,"<html>This were not integers!" +
                         "<br />Try again!!!",
                    "ERROR",JOptionPane.ERROR_MESSAGE);
                System.err.println("EXCEPTION " +
                    e.getMessage() + '\n' + "STACK TRACE:");
                e.printStackTrace();

            } catch(ArithmeticException e) {
                JOptionPane.showMessageDialog(
                    null,"<html>Division by zero!" +
                         "<br />Try again!!!",
                    "ERROR",JOptionPane.ERROR_MESSAGE);
                System.err.println("EXCEPTION " +
                    e.getMessage() + '\n' + "STACK TRACE:");
                e.printStackTrace();
            }
        }

        System.out.println(
            noGood ?
                "Program cancelled"
              : "Result of division: " + number);
    }
}
```

The last example illustrates custom (user defined) exceptions: we define our own type of exception:

## Listing 63 AYN-CheckedExc/MyUncheckedException.java

```java
public class MyUncheckedException
                    extends IllegalArgumentException {
    MyUncheckedException() {
        super();
    }
    MyUncheckedException(String message) {
        super(message);
    }
}
```

and then we use it

## Listing 64 AYN-CheckedExc/CheckedExc.java

```java
public class CheckedExc {

    public static void main(String[] args) {

        try {
            goSleep(5*1000);
        } catch(InterruptedException ignored) {
            System.err.println("Interrupted");
        } finally {
            System.err.println("AFTER SLEEP");
        }

          // handling all exceptions
        try {
            goSleep(-1);
        } catch(InterruptedException e) {
            System.err.println("Interrupted");
        } catch(Exception e) {
            System.err.println("Handling exception: " +
                                       e.getMessage());
        } finally {
            System.err.println("GOING ON");
        }

          // here MyUncheckedException is  n o t  handled
          // so the program  w i l l  crash (but 'finally'
          // clause will be executed anyway)
        try {
            goSleep(-1);
        } catch(InterruptedException e) {
            System.err.println("Interrupted");
        } finally {
            System.err.println("QUITTING");
        }
    }

    private static void goSleep(int time)
                            throws InterruptedException {
        if (time < 0)
            throw new MyUncheckedException("Negative time");

        System.out.println(
                "Going to sleep for " + time +"ms");
        Thread.sleep(time);
        System.out.println("Waking up");
    }
}
```

The program prints:

```java
Going to sleep for 3000ms
Waking up
AFTER SLEEP
Handling exception: Negative time
GOING ON
QUITTING
Exception in thread "main" MyUncheckedException: Negative time
    at CheckedExc.goSleep(CheckedExc.java:40)
    at CheckedExc.main(CheckedExc.java:29)
```

As we can see, the string "`QUITTING`" was printed by the `finally` clause even though there was an unhandled exception and the program crashed.

## 11.6 Assertions

Another way of dealing with possible errors in our programs is by using _assertions_ (line 34 of the listing [60](https://github.com/Java-PJATK/60.AXW-ScanExcept/) on page 102). After the keyword assert, we specify a condition (something with boolean value) and, after a colon, a message (which is in fact optional) 

```java
assert boolExp : message;
```

At runtime, the value of `boolExp` is evaluated and if found `false`, the program will print the message and terminate by throwing an exception of type `AssertionError`.

In our example, this is what will happen when the file is missing and the variable `scfile` is null`.

By default, assertions are disabled at runtime, but can be enabled by `-ea` switch (<sup>10</sup>It is also possible to enable/disable assertions selectively for certain packages or individual classes.) when running a program. 

Because assertions are sometimes _on_ and sometimes _off_, there should be no side effects of using them. Normally, assertions check conditions that we are almost sure should always hold. 

If, however, the condition is _not_ met, and an _unchecked_ exception of type `AssertionError` is thrown, we should never even
try to handle it, because it indicates a serious flaw in the program that just must be corrected.

Next [62.AYC-Excpts](https://github.com/Java-PJATK/62.AYC-Excpts)
