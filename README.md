# DesignPattern
* Design Pattern , from the book 'Head-First-Design-Pattern'. 
* From the course "From the course: Programming Foundations: Design Patterns", provided by LinkedIn Learning.

## Design Pattern In Java SE 8  [1.8.0_91]
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
```
private DesktopPeer peer;

    /**
     * Suppresses default constructor for noninstantiability.
     */
    private Desktop() {
        peer = Toolkit.getDefaultToolkit().createDesktopPeer(this);
    }
    /**
     * Returns the <code>Desktop</code> instance of the current
     * browser context.  On some platforms the Desktop API may not be
     * supported; use the {@link #isDesktopSupported} method to
     * determine if the current desktop is supported.
     * @return the Desktop instance of the current browser context
     * @throws HeadlessException if {@link
     * GraphicsEnvironment#isHeadless()} returns {@code true}
     * @throws UnsupportedOperationException if this class is not
     * supported on the current platform
     * @see #isDesktopSupported()
     * @see java.awt.GraphicsEnvironment#isHeadless
     */
    public static synchronized Desktop getDesktop(){
        if (GraphicsEnvironment.isHeadless()) throw new HeadlessException();
        if (!Desktop.isDesktopSupported()) {
            throw new UnsupportedOperationException("Desktop API is not " +
                                                    "supported on the current platform");
        }

        sun.awt.AppContext context = sun.awt.AppContext.getAppContext();
        Desktop desktop = (Desktop)context.get(Desktop.class);

        if (desktop == null) {
            desktop = new Desktop();
            context.put(Desktop.class, desktop);
        }

        return desktop;
    }

```

#### Factory
* java.lang.Object.toString()
```
    /**
     * Returns a string representation of the object. In general, the
     * {@code toString} method returns a string that
     * "textually represents" this object. The result should
     * be a concise but informative representation that is easy for a
     * person to read.
     * It is recommended that all subclasses override this method.
     * <p>
     * The {@code toString} method for class {@code Object}
     * returns a string consisting of the name of the class of which the
     * object is an instance, the at-sign character `{@code @}', and
     * the unsigned hexadecimal representation of the hash code of the
     * object. In other words, this method returns a string equal to the
     * value of:
     * <blockquote>
     * <pre>
     * getClass().getName() + '@' + Integer.toHexString(hashCode())
     * </pre></blockquote>
     *
     * @return  a string representation of the object.
     */
    public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }

```
* java.lang.Class.forName()
```
    /**
     * Returns the {@code Class} object associated with the class or
     * interface with the given string name.  Invoking this method is
     * equivalent to:
     *
     * <blockquote>
     *  {@code Class.forName(className, true, currentLoader)}
     * </blockquote>
     *
     * where {@code currentLoader} denotes the defining class loader of
     * the current class.
     *
     * <p> For example, the following code fragment returns the
     * runtime {@code Class} descriptor for the class named
     * {@code java.lang.Thread}:
     *
     * <blockquote>
     *   {@code Class t = Class.forName("java.lang.Thread")}
     * </blockquote>
     * <p>
     * A call to {@code forName("X")} causes the class named
     * {@code X} to be initialized.
     *
     * @param      className   the fully qualified name of the desired class.
     * @return     the {@code Class} object for the class with the
     *             specified name.
     * @exception LinkageError if the linkage fails
     * @exception ExceptionInInitializerError if the initialization provoked
     *            by this method fails
     * @exception ClassNotFoundException if the class cannot be located
     */
    @CallerSensitive
    public static Class<?> forName(String className)
                throws ClassNotFoundException {
        Class<?> caller = Reflection.getCallerClass();
        return forName0(className, true, ClassLoader.getClassLoader(caller), caller);
    }
```
#### Abstract Factory 
* java.util.Calendar.getInstance()
```
    /**
     * Gets a calendar using the default time zone and locale. The
     * <code>Calendar</code> returned is based on the current time
     * in the default time zone with the default
     * {@link Locale.Category#FORMAT FORMAT} locale.
     *
     * @return a Calendar.
     */
    public static Calendar getInstance()
    {
        return createCalendar(TimeZone.getDefault(), Locale.getDefault(Locale.Category.FORMAT));
    }

```
* java.util.Arrays.asList();
```
    /**
     * Returns a fixed-size list backed by the specified array.  (Changes to
     * the returned list "write through" to the array.)  This method acts
     * as bridge between array-based and collection-based APIs, in
     * combination with {@link Collection#toArray}.  The returned list is
     * serializable and implements {@link RandomAccess}.
     *
     * <p>This method also provides a convenient way to create a fixed-size
     * list initialized to contain several elements:
     * <pre>
     *     List&lt;String&gt; stooges = Arrays.asList("Larry", "Moe", "Curly");
     * </pre>
     *
     * @param <T> the class of the objects in the array
     * @param a the array by which the list will be backed
     * @return a list view of the specified array
     */
    @SafeVarargs
    @SuppressWarnings("varargs")
    public static <T> List<T> asList(T... a) {
        return new ArrayList<>(a);
    }

```
* java.util.ResourceBundle.getBundle()
* java.sql.DriverManager.getConnection()
* java.sql.Statement.executeQuery()
* java.text.NumberFormat.getInstance()
* java.lang.management.ManagementFactory
* java.nio.charset.Charset.forName()
* javax.xml.parsers.DocumentBuilderFactory.newInstance()
* javax.xml.transform.TransformerFactory.newInstance()
* javax.xml.parsers.DocumentBuilderFactory.newInstance()
* javax.xml.xpath.XPathFactory.newInstance()
* java.net.URLStreamHandlerFactory.createURLStreamHandler(String) returns singleton object per protocol

#### Builder
* java.lang.StringBuilder.append()
* java.lang.StringBuffer.append()
* java.nio.ByteBuffer.put() (also on CharBuffer, ShortBuffer, IntBuffer, LongBuffer, FloatBuffer, DoubleBuffer)
* javax.swing.GroupLayout.group.addComponent()
* all implements java.lang.Appendable()

#### Prototype 
* java.lang.Object.clone()
```
```
### Structural
### Behavioral
* Marker Interface / Tagging interface Pattern
** Serializable, Cloneable, 


