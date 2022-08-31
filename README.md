# Enums

## Learning Goals

- Explain enums
- Create and use enums in Java

## What is an Enum?

**Enumerations** or **enums** for short are a special kind of class in Java that
allows us to define a set of constants that can then be used in a program to
represent specific values.

A good example could be the days of the week. Say we want to represent the days
of the week in our program. We could create individual constants like this:

```java
public class Example {
   public static final int MONDAY = 0;
   public static final int TUESDAY = 1;
   public static final int WEDNESDAY = 2;
}
```

But a better way would be to create an enum called `Day` for a more structured
approach:

```java
public enum Day {
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY
}
```

An enum is defined like a class, except it uses the `enum` keyword instead of
the `class` keyword and implicitly extends the `java.lang.Enum` package.
Inside the `enum` definition, we list out the possible values for the enum,
which are by convention defined with names in all upper case - just like
constants. These values are called **enumeration constants**.

It should also be noted that each enumeration constant is associated with a
numeric value in the order that they are defined. For example, right now, the
days of the week we have defined in `Day` have the following values:

```plaintext
MONDAY = 0
TUESDAY = 1
WEDNESDAY = 2
THURSDAY = 3
FRIDAY = 4
SATURDAY = 5
SUNDAY = 6
```

If we were to have defined the days of the week starting with Sunday instead of
Monday, then Sunday would have a numeric value of 0 since the numeric values are
based on the order the enumeration constants are defined.

## How to use Enums

Let's consider we are trying to schedule a doctor's appointment. We could use
the enum `Day` to help determine what day of the week a patient is trying to
schedule an appointment:

```java
import java.util.Scanner;

public class DoctorOffice {

    private Day appointmentDay;

    DoctorOffice() {
        appointmentDay = Day.MONDAY;
    }

    public void scheduleAppointment(Day appointmentDay) {
        this.appointmentDay = appointmentDay;
        System.out.println("Your appointment is on " + appointmentDay);
    }

    public static void main(String[] args) {
        System.out.println("What day would you like to make your appointment?");

        Scanner scanner = new Scanner(System.in);
        String appointmentDay = scanner.nextLine();
        
        try {
            appointmentDay = appointmentDay.toUpperCase();

            DoctorOffice office = new DoctorOffice();
            office.scheduleAppointment(Day.valueOf(appointmentDay));
        } catch (IllegalArgumentException e) {
            System.out.println("Did not enter a day of the week");
        }
    }
}
```

In the class `DoctorOffice`, we have a private instance variable of our enum,
`appointmentDay` that we can assign to one of our enumeration constants. To
assign to one of our enumeration constants, we can either assign it using the
syntax:

```plaintext
<enumeration name>.<enumeration constant>
// Day.MONDAY or Day.TUESDAY
```

Or we could assign it to another `Day` type, like we do in the
`scheduleAppointment()` method.

In our `main()` method, when we ask the user what day they would like to
schedule their appointment, we use the `valueOf()` method. This is a method that
is part of the `java.lang.Enum` class that will return the `Day` type of the
`String` argument. For example, `Day.valueOf("WEDNESDAY")` would return the
enumeration constant of `WEDNESDAY`.

So if we were to run the code above, we might have an output like this:

```plaintext
What day would you like to make your appointment?
Monday
Your appointment is on MONDAY
```

We can also use the enum in if statements. For example, if we want to check
if the `appointmentDay` is Monday, we could do something like this:

```java
public void scheduleAppointment(Day appointmentDay) {
    // Example using enumerations in if statements
    if (appointmentDay == Day.MONDAY) {
        System.out.println("Hello Monday");
    }
}
```

Enumerations can also be used in switch statements as well:

```java
public void scheduleAppointment(Day appointmentDay) {
    // Example using enumerations in a switch statement
        switch (appointmentDay) {
            case MONDAY:
                System.out.println("Manic Monday!");
                break;
            case TUESDAY:
                System.out.println("It's Tuesday!");
                break;
            case WEDNESDAY:
                System.out.println("Middle of the week!");
                break;
            case THURSDAY:
                System.out.println("It's Thursday!");
                break;
            case FRIDAY:
                System.out.println("Freaky Friday!");
                break;
            case SATURDAY:
                System.out.println("Yay! It's Saturday!");
                break;
            case SUNDAY:
                System.out.println("Sunday Fun-Day!");
                break;
        }
}
```

Here are other important characteristics of Enums in Java:

1. Enums cannot be instantiated - they can only be referenced.
2. Enums can safely be compared using the `==` operator, as in the example above.
3. Enums are implicitly `static` and `final`.
4. All Enums implicitly extend `java.lang.Enum`, so they cannot extend anything
   else.
5. Enums can be declared outside or inside a class, but not inside a method.
