显示当月日历

```java
import java.text.DateFormatSymbols;
import java.util.*;
public class JavaUsePredefinedClass {
    public static void main(String[] args) {
        /*
        下面将显示当前月的日历
         */
        Locale.setDefault(Locale.ITALY);
        GregorianCalendar d = new GregorianCalendar();
        int today = d.get(Calendar.DAY_OF_MONTH);
        int month = d.get(Calendar.MONTH);
        int weekday = d.get(Calendar.DAY_OF_WEEK);

        d.set(Calendar.DAY_OF_MONTH, 1);
        int firstDayOfWeek = d.getFirstDayOfWeek();
        int indent =0 ;
        while(weekday != firstDayOfWeek) {
            d.add(Calendar.DAY_OF_MONTH, 1);
            weekday = d.get(Calendar.DAY_OF_WEEK);
            indent++;
        }
        String[] weekdayNames = new DateFormatSymbols().getShortWeekdays();
        do {
            System.out.printf("%4s", weekdayNames[weekday]);
            d.add(Calendar.DAY_OF_MONTH, 1);
            weekday = d.get(Calendar.DAY_OF_WEEK);
        } while (weekday != firstDayOfWeek);
        System.out.println();
        System.out.print("                ");


        d.set(Calendar.DAY_OF_MONTH, 1);
        do {
            int day = d.get(Calendar.DAY_OF_MONTH);
            System.out.printf("%3d", day);

            if (day == today) System.out.print("*");
            else System.out.print(" ");

            d.add(Calendar.DAY_OF_MONTH, 1);
            weekday = d.get(Calendar.DAY_OF_WEEK);

            if (weekday == firstDayOfWeek) System.out.println();
        } while (d.get(Calendar.MONTH) == month);

        if (weekday != firstDayOfWeek) System.out.println();
    }
}
```
输出结果

```
 lun mar mer gio ven sab dom

                  1   2   3 
  4   5   6   7   8   9  10 
 11  12* 13  14  15  16  17 
 18  19  20  21  22  23  24 
 25  26  27  28  29  30 
```

GregorianCalendar类来写日历还是比较麻烦，Core Java 书上的代码有错误并需要修改。

另一种用Date类来写:

```java
import java.time.*;
public class CalendarTest {
public static void main(String[] args) {
      LocalDate date = LocalDate.now();
      int month = date.getMonthValue();
      int today = date.getDayOfMonth();

      date = date.minusDays(today - 1); // Set to start of month
      DayOfWeek weekday = date.getDayOfWeek();
      int value = weekday.getValue(); // 1 = Monday, ... 7 = Sunday

      System.out.println("Mon Tue Wed Thu Fri Sat Sun");
      for (int i = 1; i < value; i++)
         System.out.print("    ");
      while (date.getMonthValue() == month) {
         System.out.printf("%3d", date.getDayOfMonth());
         if (date.getDayOfMonth() == today)
            System.out.print("*");
         else
            System.out.print(" ");
         date = date.plusDays(1);
         if (date.getDayOfWeek().getValue() == 1) System.out.println();
      }
      if (date.getDayOfWeek().getValue() != 1) System.out.println();
   }
   }
```
代码行数少，实现简单轻松。

