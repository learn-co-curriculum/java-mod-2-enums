# Enums

## Learning Goals

- Explain enums
- Create and use enums in Java

## Introduction

Enums are a special kind of class in Java that allows you to define constants
that can then be used in your program to represent specific values.

In the previous example, we used "FLAT", "SLIGHT" and "HEAVY" to represent the
type of grade curving algorithm we wanted to use. There are 2 issues with the
way we set up and used those values in our previous example:

1. The literal text for each value is used in 2 different classes, which means
   it has to match or else the logic will break down. This is error-prone and
   could lead to bugs that are very difficult to find.
2. There is no way for the compiler to tell us that we're using an incorrect
   value for the "grade curving" algorithm

We can address both issues with an Enum:

```java
    public enum GradeCurvingAlgo {
        FLAT, SLIGHT, HEAVY
    }
```

## Enum Walkthrough

An enum is defined like a class, except it uses the `enum` keyword instead of
the `class` keyword. Inside the `enum` definition, you will list out the
possible values for the `enum`, which are by convention defined with names in
all upper case.

Once we have the `enum` defined, we will use those values instead of the
`String` literal we were using in the previous example.

This is the modified section of `StudentGradeTranslator`:

```java
    public enum GradeCurvingAlgo {
        FLAT, SLIGHT, HEAVY
    }

    public StudentGradeTranslator() {
        this.gradeCalculator = new FlatCurveGradeCalculator();
    }

    public StudentGradeTranslator(GradeCurvingAlgo gradingMethod) {
        if (gradingMethod == null) {
            this.gradeCalculator = new FlatCurveGradeCalculator();
        } else if (gradingMethod == GradeCurvingAlgo.FLAT) {
            this.gradeCalculator = new FlatCurveGradeCalculator();
        } else if (gradingMethod == GradeCurvingAlgo.SLIGHT) {
            this.gradeCalculator = new SlightCurveGradeCalculator();
        } else if (gradingMethod == GradeCurvingAlgo.HEAVY) {
            this.gradeCalculator = new HeavyCurveGradeCalculator();
        }
    }
```

And this is the modified version of `InnerClassRunner`:

```java
        StudentGradeTranslator gradeTranslator = new StudentGradeTranslator(StudentGradeTranslator.GradeCurvingAlgo.SLIGHT);
```

As you can see, we couldn't use an invalid value for the "curving algorithm"
anymore because the Java compiler would tell us right away that the value we're
trying to use is not defined in the `enum` we are referencing.

Here are other important characteristics of Enums in Java:

1. Enums cannot be instantiated - they can only be referenced as outlined above
2. Enums can safely be compared using the `==` operator, as in the example above
3. Enums are implicitly `static` and `final` - we will explain what this means
   in a later section
4. All Enums implicitly extend `java.lang.Enum`, so they cannot extend anything
   else
5. Enums can be declared outside or inside a class, but not inside a method

In addition to the above, you can also specify and reference values for Enums,
so that you can associate a value with each constant. In our example, we can
take advantage of this feature to create a label for each grade curving
algorithm:

```java
    public enum GradeCurvingAlgo {
        FLAT("Flat grading, so 90 = A and so on"),
        SLIGHT("Slight curving of the grades, so the grade line is moved down by 5 points so that 85 = A and so on"),
        HEAVY("Heavy curving of the grades, so the grade line is moved down by 10 points so that 80 = A and so on");

        String label;

        private GradeCurvingAlgo(String label) {
            this.label = label;
        }
    }
```

We also change the constructor to keep track of the active grading algorithm, so
we can easily reference it when we need to:

```java
    GradeCalculator gradeCalculator;
    GradeCurvingAlgo activeGradeAlgo;

    public enum GradeCurvingAlgo {
        FLAT("Flat grading, so 90 = A and so on"),
        SLIGHT("Slight curving of the grades, so the grade line is moved down by 5 points so that 85 = A and so on"),
        HEAVY("Heavy curving of the grades, so the grade line is moved down by 10 points so that 80 = A and so on");

        String label;

        private GradeCurvingAlgo(String label) {
            this.label = label;
        }
    }

    public StudentGradeTranslator() {
        this.gradeCalculator = new FlatCurveGradeCalculator();
        this.activeGradeAlgo = GradeCurvingAlgo.FLAT;
    }

    public StudentGradeTranslator(GradeCurvingAlgo gradingMethod) {
        if (gradingMethod == null) {
            this.gradeCalculator = new FlatCurveGradeCalculator();
            activeGradeAlgo = GradeCurvingAlgo.FLAT;
        } else {
            if (gradingMethod == GradeCurvingAlgo.FLAT) {
                this.gradeCalculator = new FlatCurveGradeCalculator();
            } else if (gradingMethod == GradeCurvingAlgo.SLIGHT) {
                this.gradeCalculator = new SlightCurveGradeCalculator();
            } else if (gradingMethod == GradeCurvingAlgo.HEAVY) {
                this.gradeCalculator = new HeavyCurveGradeCalculator();
            }
            activeGradeAlgo = gradingMethod;
        }
    }
```

This can then be used by anyone using the `enum` by referencing the new variable
just like you would for a class:

```java
        // get all the student and their grades using each entry
        System.out.println("List of students and their grades (" + gradeTranslator.activeGradeAlgo.label + "): ");
        for (Map.Entry<String, String> studentGrade: studentGrades.entrySet()) {
           System.out.println(studentGrade.getKey() + "'s grade is " +
           gradeTranslator.getLetterGrade(Integer.parseInt(studentGrade.getValue())));
           System.out.println("Passing grade status: " + gradeTranslator.isPassingGrade(Integer.parseInt(studentGrade.getValue())));
        }

```
