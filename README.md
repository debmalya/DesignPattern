# DesignPattern
* Design Pattern , from the book 'Head-First-Design-Pattern'. 
* From the course "From the course: Programming Foundations: Design Patterns", provided by LinkedIn Learning.

## Design Pattern In Java
### Creational
#### Singleton
* java.lang.Runtime.getRuntime()
```
private static Runtime currentRuntime = new Runtime();

    /**
     * Returns the runtime object associated with the current Java application.
     * Most of the methods of class <code>Runtime</code> are instance
     * methods and must be invoked with respect to the current runtime object.
     *
     * @return  the <code>Runtime</code> object associated with the current
     *          Java application.
     */
    public static Runtime getRuntime() {
        return currentRuntime;
    }

    /** Don't let anyone else instantiate this class */
    private Runtime() {}

```
* java.awt.Desktop.getDeskTop()

#### Factory
#### Abstract Factory 
#### Builder
#### Prototype 
### Structural
### Behavioral

