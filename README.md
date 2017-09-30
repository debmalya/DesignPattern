# DesignPattern
* Design Pattern , from the book 'Head-First-Design-Pattern'. 
* From the course "From the course: Programming Foundations: Design Patterns", provided by LinkedIn Learning.

## Design Pattern In Java SE 8  [1.8.0_91]
### Creational
How objects will be created ?

#### Singleton

Only will have one instance of the class across JVM. One JVM only one instance of the class.

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

Create object without exposing internal logic of its creation.

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

Encapsulates a group of individual group of factories.

* java.util.Calendar.getInstance()

In this example below, Timezone.getDefault() and Locale.getDefault(...) method encapsulated into single method getInstance().
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
Telescopic constructor. Use this pattern when we have to build a composite object.
* java.lang.StringBuilder.append()
```
    @Override
    public StringBuilder append(Object obj) {
        return append(String.valueOf(obj));
    }

    @Override
    public StringBuilder append(String str) {
        super.append(str);
        return this;
    }
    
    /**
     * Appends the specified string to this character sequence.
     * <p>
     * The characters of the {@code String} argument are appended, in
     * order, increasing the length of this sequence by the length of the
     * argument. If {@code str} is {@code null}, then the four
     * characters {@code "null"} are appended.
     * <p>
     * Let <i>n</i> be the length of this character sequence just prior to
     * execution of the {@code append} method. Then the character at
     * index <i>k</i> in the new character sequence is equal to the character
     * at index <i>k</i> in the old character sequence, if <i>k</i> is less
     * than <i>n</i>; otherwise, it is equal to the character at index
     * <i>k-n</i> in the argument {@code str}.
     *
     * @param   str   a string.
     * @return  a reference to this object.
     */
    public AbstractStringBuilder append(String str) {
        if (str == null)
            return appendNull();
        int len = str.length();
        ensureCapacityInternal(count + len);
        str.getChars(0, len, value, count);
        count += len;
        return this;
    }    
```
* java.lang.StringBuffer.append()
* java.nio.ByteBuffer.put() (also on CharBuffer, ShortBuffer, IntBuffer, LongBuffer, FloatBuffer, DoubleBuffer)
* javax.swing.GroupLayout.group.addComponent()
* all implements java.lang.Appendable()

#### Prototype 
Cloned to produce new objects
* java.lang.Object.clone()
```
 /**
     * Creates and returns a copy of this object.  The precise meaning
     * of "copy" may depend on the class of the object. The general
     * intent is that, for any object {@code x}, the expression:
     * <blockquote>
     * <pre>
     * x.clone() != x</pre></blockquote>
     * will be true, and that the expression:
     * <blockquote>
     * <pre>
     * x.clone().getClass() == x.getClass()</pre></blockquote>
     * will be {@code true}, but these are not absolute requirements.
     * While it is typically the case that:
     * <blockquote>
     * <pre>
     * x.clone().equals(x)</pre></blockquote>
     * will be {@code true}, this is not an absolute requirement.
     * <p>
     * By convention, the returned object should be obtained by calling
     * {@code super.clone}.  If a class and all of its superclasses (except
     * {@code Object}) obey this convention, it will be the case that
     * {@code x.clone().getClass() == x.getClass()}.
     * <p>
     * By convention, the object returned by this method should be independent
     * of this object (which is being cloned).  To achieve this independence,
     * it may be necessary to modify one or more fields of the object returned
     * by {@code super.clone} before returning it.  Typically, this means
     * copying any mutable objects that comprise the internal "deep structure"
     * of the object being cloned and replacing the references to these
     * objects with references to the copies.  If a class contains only
     * primitive fields or references to immutable objects, then it is usually
     * the case that no fields in the object returned by {@code super.clone}
     * need to be modified.
     * <p>
     * The method {@code clone} for class {@code Object} performs a
     * specific cloning operation. First, if the class of this object does
     * not implement the interface {@code Cloneable}, then a
     * {@code CloneNotSupportedException} is thrown. Note that all arrays
     * are considered to implement the interface {@code Cloneable} and that
     * the return type of the {@code clone} method of an array type {@code T[]}
     * is {@code T[]} where T is any reference or primitive type.
     * Otherwise, this method creates a new instance of the class of this
     * object and initializes all its fields with exactly the contents of
     * the corresponding fields of this object, as if by assignment; the
     * contents of the fields are not themselves cloned. Thus, this method
     * performs a "shallow copy" of this object, not a "deep copy" operation.
     * <p>
     * The class {@code Object} does not itself implement the interface
     * {@code Cloneable}, so calling the {@code clone} method on an object
     * whose class is {@code Object} will result in throwing an
     * exception at run time.
     *
     * @return     a clone of this instance.
     * @throws  CloneNotSupportedException  if the object's class does not
     *               support the {@code Cloneable} interface. Subclasses
     *               that override the {@code clone} method can also
     *               throw this exception to indicate that an instance cannot
     *               be cloned.
     * @see java.lang.Cloneable
     */
    protected native Object clone() throws CloneNotSupportedException;
```

### Structural
#### Adapter
* java.io.InputStreamReader(InputStream)
* java.io.OutputStreamWriter(OutputStream)
* javax.xml.bind.annotation.adapters.XmlAdapter.marshal()

#### Bridge
* LinkedHashMap - which returns an unmodifiable linked map which doesn't clone the items but uses them. 
* java.util.Collections.newSetFromMap()

#### Composite
* java.util.Map.putAll()
* java.util.List.addAll()
* java.util.Set.addAll()
* java.nio.ByteBuffer.put(buffer)

#### Decorator
* java.io.InputStream, OutputStream, Reader, Writer, 
* java.util.Collections.synchronizedXXX() and unmodifiableXXX()

** FileInputStream.read() -> Decorated with buffer in BufferedInputStream.read() -> then further decorated in LineNumberReader.read()
```
    /**
     * Reads the next byte of data from this input stream. The value
     * byte is returned as an {@code int} in the range
     * {@code 0} to {@code 255}. If no byte is available
     * because the end of the stream has been reached, the value
     * {@code -1} is returned. This method blocks until input data
     * is available, the end of the stream is detected, or an exception
     * is thrown.
     * <p>
     * The {@code read} method of
     * {@code LineNumberInputStream} calls the {@code read}
     * method of the underlying input stream. It checks for carriage
     * returns and newline characters in the input, and modifies the
     * current line number as appropriate. A carriage-return character or
     * a carriage return followed by a newline character are both
     * converted into a single newline character.
     *
     * @return     the next byte of data, or {@code -1} if the end of this
     *             stream is reached.
     * @exception  IOException  if an I/O error occurs.
     * @see        java.io.FilterInputStream#in
     * @see        java.io.LineNumberInputStream#getLineNumber()
     */
    @SuppressWarnings("fallthrough")
    public int read() throws IOException {
        int c = pushBack;

        if (c != -1) {
            pushBack = -1;
        } else {
            c = in.read();
        }

        switch (c) {
          case '\r':
            pushBack = in.read();
            if (pushBack == '\n') {
                pushBack = -1;
            }
          case '\n':
            lineNumber++;
            return '\n';
        }
        return c;
    }
```

#### Facade
* javax.faces.context.FacesContext
* Flyweight
* java.lang.Integer.valueOf()

#### Proxy
* java.lang.reflect.Proxy

### Behavioral
#### Chain of Responsibility 
* java.util.logging.Logger.log()
* javax.servlet.Filter.doFilter()
#### Command
* All implementation of java.lang.Runnable  javax.swing.Action
#### Interpreter
* java.util.Pattern
* java.text.Normalizer
* javax.text.Format
* javax.el.ELResolver
#### Iterator
* java.util.Iterator
* java.util.Enumeration

#### Mediator
* java.util.Timer
* java.util.concurrent.Executor.execute
* java.util.concurrent.ExecutorService (the invokeXXX and submit methods)
* java.lang.reflect.Method.invoke
#### Momento
* java.util.Date
* java.io.Serializable
#### Observer (Publish/Subscriber)
* java.util.Observer
* java.util.Observable
* java.util.EventListener
* javax.servlet.http.HttpSessionBindingListener
* javax.servlet.http.HttpSessionAttributeListener
* javax.faces.event.PhaseListener
#### State
* All implementations of java.util.Iterator
* javax.faces.LifeCycle.execute
#### Strategy
* java.util.Comparator.compare(), executed by among others Collections.sort()
* javax.servlet.http.HttpServlet the service and doXXX() methods
* javax.servlet.Filter.doFilter

#### Template
* all non abstract methods of java.io.InputStream, java.io.OutputStream, java.io.Reader
* all non abstract methods of java.util.AbstractList, java.util.AbstractSet, java.util.AbstractMap.
#### Visitor
* javax.lang.model.element.AnnotationValue
* javax.lang.model.element.Element and ElementVisitor
* javax.lang.model.type.TypeMirror  and TypeVisitor

#### Marker Interface / Tagging interface Pattern
* Serializable, Cloneable, 


