:boom: **NEW:** As of March 7, 2014, CodeAreaFX has been renamed to RichTextFX, to reflect the fact that the project now supports much more than just syntax highlighting.  
:boom: **IMPORTANT:** Package `codearea.control` has been renamed to `org.fxmisc.richtext` in the HEAD of the master branch. All builds starting from March 7, 2014 will reflect this change. Please, update your source code.  
:boom: **NEW:** RichTextFX snapshots are now deployed to the Sonatype repository. [See below](#use-richtextfx-in-your-project)

RichTextFX
==========

RichTextFX provides a text area for JavaFX with API to style ranges of text. It is intended as a base for rich-text editors and code editors with syntax highlighting.


Flavors
-------

### StyleClassedTextArea

`StyleClassedTextArea` lets you assign style classes to ranges of text. You can define the style classes in your stylesheet.

Example.java:

```java
area.setStyleClass(from, to, "red");
```

example.css:

```css
.red { -fx-fill: red; }
```

This renders the text in the range `[from, to)` in red.

Note that the style classes are assigned to instances of [Text](http://download.java.net/jdk8/jfxdocs/javafx/scene/text/Text.html), so you can specify any [CSS properties applicable to a Text node](http://docs.oracle.com/javafx/2/api/javafx/scene/doc-files/cssref.html#text).

#### CodeArea

`CodeArea` is a variant of `StyleClassedTextArea` that uses a fixed width font by default, making it a convenient base for source code editors. `CodeArea` is used in the [Java Keywords demo](#1-automatic-highlighting-of-java-keywords) below.


### InlineCssTextArea

`InlineCssTextArea` lets you specify inline CSS for a range of text.

```java
area.setStyle(from, to, "-fx-font-weight: bold;");
```

Again, you can use any CSS properties applicable to a `Text` node.


### InlineStyleTextArea

`InlineStyleTextArea<S>` is a more general version of `InlineCssTextArea`. In the end, there is still inline CSS assigned to `Text` nodes, but instead of using the CSS string directly, you use an instance of your custom style representation `S` and provide a way (function) to convert `S` to CSS string in `InlineStyleTextArea` constructor.

```java
class MyStyleInfo {
    boolean bold;
    boolean italic;
    
    String toCss() {
        return "-fx-font-weight: " + (bold ? "bold" : "normal") + ";"
               + "-fx-font-style: " + (italic ? "italic" : "normal") + ";";
    }
}

InlineStyleTextArea<MyStyleInfo> area =
        new InlineStyleTextArea<>(new MyStyleInfo(), styleInfo -> styleInfo.toCss());
```

The first constructor argument is the default style to use for ranges of text where you don't set the style explicitly. The second constructor argument is the function to convert the custom style representation to CSS.

You then assign an instance of your custom style representation to a range of text.

```java
MyStyleInfo styleInfo = ...;

area.setStyle(from, to, styleInfo);
```

You appreciate the benefits of this approach over `InlineCssTextArea` when you need to query the style used at a given position in text - you get back an instance of your style representation instead of a CSS string.

```java
MyStyleInfo styleInfo = area.getStyleAt(charIndex);
```

`InlineStyleTextArea` is used in the [Rich-text demo](#2-rich-text-editor) below.


Requirements
------------

[JDK8](https://jdk8.java.net/download.html) is required, because [TextFlow](http://download.java.net/jdk8/jfxdocs/javafx/scene/text/TextFlow.html), introduced in JavaFX 8.0, is used to render each line. Also, there's a heavy use of lambdas, defender methods and the stream API in the code base.


Demos
-----

### 1. Automatic highlighting of Java keywords

![Screenshot of the JavaKeywords demo](https://googledrive.com/host/0B4a5AnNnZhkbYlVlbVprYnhPdVk/java-keywords.png)

#### Run using the pre-built JAR

[Download](https://googledrive.com/host/0B4a5AnNnZhkbZ3dRam5ONHJGOHM/downloads/) the pre-built "fat" JAR file and run

    java -cp richtextfx-demos-fat-yyyymmdd.jar org.fxmisc.richtext.demo.JavaKeywords

#### Run from the source repo

    gradle JavaKeywords

#### Source code

[JavaKeywords.java](https://github.com/TomasMikula/RichTextFX/blob/master/richtextfx-demos/src/main/java/org/fxmisc/richtext/demo/JavaKeywords.java)


### 2. Rich-text editor

![Screenshot of the RichText demo](https://googledrive.com/host/0B4a5AnNnZhkbYlVlbVprYnhPdVk/rich-text.png)

#### Run using the pre-built JAR
[Download](https://googledrive.com/host/0B4a5AnNnZhkbZ3dRam5ONHJGOHM/downloads/) the pre-built "fat" JAR file and run

    java -cp richtextfx-demos-fat-yyyymmdd.jar org.fxmisc.richtext.demo.RichText

#### Run from the source repo

    gradle RichText

#### Source code

[RichText.java](https://github.com/TomasMikula/RichTextFX/blob/master/richtextfx-demos/src/main/java/org/fxmisc/richtext/demo/RichText.java)

Source code of this demo is dedicated to the public domain.


Use RichTextFX in your project
------------------------------

### Method 1: as a managed dependency

Snapshot releases are deployed to Sonatype snapshot repository with these Maven coordinates

| Group ID            | Artifact ID | Version        |
| :-----------------: | :---------: | :------------: |
| org.fxmisc.richtext | richtextfx  | 1.0.0-SNAPSHOT |

#### Gradle example

```groovy
repositories {
    maven {
        url 'https://oss.sonatype.org/content/repositories/snapshots/' 
    }
}

dependencies {
    compile group: 'org.fxmisc.richtext', name: 'richtextfx', version: '1.0.0-SNAPSHOT'
}
```

#### Sbt example

```scala
resolvers += "Sonatype OSS Snapshots" at "https://oss.sonatype.org/content/repositories/snapshots"

libraryDependencies += "org.fxmisc.richtext" % "richtextfx" % "1.0.0-SNAPSHOT"
```


### Method 2: as an unmanaged dependency

Download the latest [JAR](https://oss.sonatype.org/content/repositories/snapshots/org/fxmisc/richtext/richtextfx/1.0.0-SNAPSHOT/) or [fat JAR (including dependencies)](https://googledrive.com/host/0B4a5AnNnZhkbZ3dRam5ONHJGOHM/downloads/) and place it on your classpath.


License
-------

[GPLv2 with the Classpath Exception](http://openjdk.java.net/legal/gplv2+ce.html)


Links
-----

[API Documentation](http://www.fxmisc.org/richtext/javadoc/org/fxmisc/richtext/package-summary.html)  
[Dropped Features](https://github.com/TomasMikula/RichTextFX/wiki/Dropped-Features)  
[Known Issues](https://github.com/TomasMikula/RichTextFX/wiki/Known-Issues)  
[Java 8 Development Gotchas](https://github.com/TomasMikula/RichTextFX/wiki/Java-8-Development-Gotchas)
