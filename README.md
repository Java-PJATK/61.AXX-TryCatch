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
// AYC-Excpts/Excpts.java
 
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

```

and then we use it

## Listing 64 AYN-CheckedExc/CheckedExc.java

```java

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

Next [62.AYC-Excpts](https://github.com/Java-PJATK/62.AYC-Excpts)
