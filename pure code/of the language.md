# abastract classes


They are basically a cross between an interface and a regular class in that abstract classes may have multiple methods that are fully declared but a couple abstract methods.

these classes/methods will have information regarding what the object does rather than how it does it.

an abstract method is declared just like an interface method is declared.

Abstract classes are never instantiated they are only inherited from other classes and then the abstract methods are defined in the child class.

if one method is declared abstract the entire class must be declared abstract. 




# super keyword


basically it says to assign the parameters in the child object to the properties defined in the constructor of the parent. 

just think of a kid being asked to be picked up by its parent.
<br/>
<br/>

# lambdas
**these guys are used primarly for defining the implementation of a functional interface**(an interface with only one method)
see this [page](https://www.tutorialspoint.com/java8/java8_lambda_expressions.htm) for more details

# generic classes/methods
T**hese are for when you want to create an algorithm that works for a wide variety of objects.** For example if you want to create a sort method that can be used for multiple types of arrays, not just Doubles or Strings, you can use a generic object.

here's how you can implement it.

```
public class GenericMethodTest {
   // generic method printArray
   public static < E > void printArray( E[] inputArray ) {
      // Display array elements
      for(E element : inputArray) {
         System.out.printf("%s ", element);
      }
      System.out.println();
   }

   public static void main(String args[]) {
      // Create arrays of Integer, Double and Character
      Integer[] intArray = { 1, 2, 3, 4, 5 };
      Double[] doubleArray = { 1.1, 2.2, 3.3, 4.4 };
      Character[] charArray = { 'H', 'E', 'L', 'L', 'O' };

      System.out.println("Array integerArray contains:");
      printArray(intArray);   // pass an Integer array

      System.out.println("\nArray doubleArray contains:");
      printArray(doubleArray);   // pass a Double array

      System.out.println("\nArray characterArray contains:");
      printArray(charArray);   // pass a Character array
   }
}
```
<br>

# enums
basically an array of const variables. A good example is the days of the week or compass directions. because they are constants the name's of the enum's type fields should be in upper case letters.

**enums are great for when you need to represent a fixed set of constants.**

defining an enum

```
public enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY,
    THURSDAY, FRIDAY, SATURDAY 
}
```
calling an enum

```
Day day;
    
    public EnumTest(Day day) {
        this.day = day;
    }
    
    public void tellItLikeItIs() {
        switch (day) {
            case MONDAY:
                System.out.println("Mondays are bad.");
                break;
                    
            case FRIDAY:
                System.out.println("Fridays are better.");
                break;
                         
            case SATURDAY: case SUNDAY:
                System.out.println("Weekends are best.");
                break;
                        
            default:
                System.out.println("Midweek days are so-so.");
                break;
        }
    }
    
    public static void main(String[] args) {
        EnumTest firstDay = new EnumTest(Day.MONDAY);
        firstDay.tellItLikeItIs();
```
the enum types are variables but they are final variables.

# anonymous classes

help make code more consise. They enable you to decalare and instantiate a class at the same time.

**you should use anonymous classes when you need to use a local class only once**

