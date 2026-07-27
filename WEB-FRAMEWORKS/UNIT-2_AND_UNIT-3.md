# Study Notes: Unit 2 & Unit 3
## Web Technologies — Descriptive Exam Preparation

---

# UNIT 2: DYNAMIC DOCUMENTS WITH JAVASCRIPT

---

## 1. Introduction to Dynamic Documents

A **dynamic XHTML document** is one where the content, style, or structure can change after the page has loaded — without a page reload — using JavaScript and the DOM.

Using the DOM, JavaScript can:
- Move elements on the page
- Change styles (colors, fonts)
- Toggle element visibility
- Modify content dynamically

---

## 2. Element Positioning (CSS-P)

CSS Positioning (CSS-P) provides tools to place elements precisely on a web page.

### The `position` Property
| Value | Description |
|-------|-------------|
| `static` | Default. Elements placed left-to-right, top-to-bottom. `top`/`left` are ignored. |
| `relative` | Moves element relative to its normal static position (useful for superscripts/subscripts). |
| `absolute` | Places element at exact coordinates relative to its containing element. |

### The `top` and `left` Properties
- Positive `top` → pushes element **down**
- Negative `top` → pushes element **up**
- Positive `left` → pushes element **right**
- Negative `left` → pushes element **left**

### Example: Absolute Positioning
```html
<p style="position:absolute; left:100px; top:200px">
  This text appears at exactly (100, 200)
</p>
```

> **Exam Tip:** To dynamically move elements with JavaScript, `position` must be `absolute` or `relative` — never `static`.

---

## 3. Moving Elements with JavaScript

JavaScript can reposition elements by changing their `top` and `left` CSS properties at runtime.

### Example Program: mover.html
```html
<!DOCTYPE html>
<html>
<head>
<title>Move Element</title>
<script>
function moveImage() {
  var x = document.getElementById("xCoord").value;
  var y = document.getElementById("yCoord").value;
  var img = document.getElementById("myImage");
  img.style.left = x + "px";
  img.style.top  = y + "px";
}
</script>
</head>
<body>
  <img id="myImage" src="plane.jpg"
       style="position:absolute; left:50px; top:50px;" />

  X: <input type="text" id="xCoord" value="50" />
  Y: <input type="text" id="yCoord" value="50" />
  <button onclick="moveImage()">Move</button>
</body>
</html>
```

**Key Points:**
- Access element's style node via `element.style`
- Assign values like `"100px"` (must include unit `px`)
- `position` must be `absolute` or `relative` for this to work

---

## 4. Element Visibility

The `visibility` CSS property controls whether an element is shown or hidden (space is still occupied).

| Value | Effect |
|-------|--------|
| `visible` | Element is shown |
| `hidden` | Element is invisible but still occupies space |

### Example Program: showHide.html
```html
<!DOCTYPE html>
<html>
<head>
<script>
function toggleVisibility() {
  var img = document.getElementById("myImage");
  if (img.style.visibility === "hidden") {
    img.style.visibility = "visible";
  } else {
    img.style.visibility = "hidden";
  }
}
</script>
</head>
<body>
  <img id="myImage" src="logo.png"
       style="position:absolute; visibility:visible;" />
  <button onclick="toggleVisibility()">Show/Hide</button>
</body>
</html>
```

> **Note:** `display:none` removes the element from layout (no space). `visibility:hidden` hides but keeps space.

---

## 5. Changing Colors and Fonts

Colors and font properties are manipulated through the `style` property of an element node.

### Color Properties
| Property | Example Value |
|----------|---------------|
| `backgroundColor` | `"yellow"` |
| `color` (text color) | `"red"` |

### Font Properties
| Property | Example Value |
|----------|---------------|
| `fontFamily` | `"Arial"` |
| `fontSize` | `"18px"` |
| `fontWeight` | `"bold"` |
| `fontStyle` | `"italic"` |

### Example Program: dynColors.html
```html
<!DOCTYPE html>
<html>
<head>
<script>
function changeColors() {
  var bg = document.getElementById("bgColor").value;
  var fg = document.getElementById("fgColor").value;
  document.body.style.backgroundColor = bg;
  document.getElementById("myText").style.color = fg;
}
</script>
</head>
<body>
  <p id="myText">Watch my color change!</p>
  Background: <input type="text" id="bgColor" onchange="changeColors()" />
  Text Color: <input type="text" id="fgColor" onchange="changeColors()" />
</body>
</html>
```

### Example Program: Changing Fonts on Hover (dynLink.html)
```html
<a id="lnk" href="#"
   onmouseover="document.getElementById('lnk').style.fontSize='20px';
                document.getElementById('lnk').style.fontWeight='bold';"
   onmouseout="document.getElementById('lnk').style.fontSize='14px';
               document.getElementById('lnk').style.fontWeight='normal';">
  Hover Over Me
</a>
```

---

## 6. Dynamic Content

Dynamic content means **changing what is displayed** in the document without reloading.

The `value` property of text areas (or `innerHTML` for other elements) can be updated dynamically using mouse events.

### Example Program: dynValue.html
```html
<!DOCTYPE html>
<html>
<head>
<script>
var helpMessages = [
  "Enter your first name here.",
  "Enter your last name here.",
  "Enter your email address."
];

function showHelp(index) {
  document.getElementById("helpBox").value = helpMessages[index];
}
function clearHelp() {
  document.getElementById("helpBox").value = "";
}
</script>
</head>
<body>
  First Name: <input type="text" onmouseover="showHelp(0)" onmouseout="clearHelp()" /><br/>
  Last Name:  <input type="text" onmouseover="showHelp(1)" onmouseout="clearHelp()" /><br/>
  Email:      <input type="text" onmouseover="showHelp(2)" onmouseout="clearHelp()" /><br/>
  <br/>
  Help: <textarea id="helpBox" rows="2" cols="40"></textarea>
</body>
</html>
```

---

## 7. Stacking Elements (z-index)

The `z-index` CSS property controls **layering** of overlapping elements. The element with a **higher z-index** appears on top.

> z-index values are strings in JavaScript (e.g., `"10"`).

### Example Program: stacking.html
```html
<!DOCTYPE html>
<html>
<head>
<script>
function bringToFront(id) {
  document.getElementById("box1").style.zIndex = "1";
  document.getElementById("box2").style.zIndex = "1";
  document.getElementById(id).style.zIndex = "10";
}
</script>
</head>
<body>
  <div id="box1" onclick="bringToFront('box1')"
       style="position:absolute; left:50px; top:50px; width:100px;
              height:100px; background:red; z-index:1;">Box 1</div>

  <div id="box2" onclick="bringToFront('box2')"
       style="position:absolute; left:80px; top:80px; width:100px;
              height:100px; background:blue; z-index:1;">Box 2</div>
</body>
</html>
```

---

## 8. Locating the Mouse Cursor

Mouse event objects carry coordinate properties:

| Property | Description |
|----------|-------------|
| `clientX`, `clientY` | Position relative to the **browser window** |
| `screenX`, `screenY` | Position relative to the **entire screen** |

### Example Program: where.html
```html
<!DOCTYPE html>
<html>
<head>
<script>
function showCoords(e) {
  document.getElementById("output").innerHTML =
    "Window X: " + e.clientX + ", Window Y: " + e.clientY + "<br/>" +
    "Screen X: " + e.screenX + ", Screen Y: " + e.screenY;
}
</script>
</head>
<body onmousemove="showCoords(event)">
  <p>Move your mouse around this page.</p>
  <div id="output"></div>
</body>
</html>
```

---

## 9. Reacting to a Mouse Click

The `mousedown` and `mouseup` events are used when more precise click control is needed.

### Example Program: anywhere.html
```html
<!DOCTYPE html>
<html>
<head>
<script>
function placeImage(e) {
  var img = document.getElementById("myImg");
  img.style.left = e.clientX + "px";
  img.style.top  = e.clientY + "px";
  img.style.visibility = "visible";
}
</script>
</head>
<body onmousedown="placeImage(event)">
  <img id="myImg" src="star.png"
       style="position:absolute; visibility:hidden;" />
  <p>Click anywhere to place the image here!</p>
</body>
</html>
```

---

## 10. Slow Movement of Elements (setTimeout & setInterval)

### `setTimeout(code, milliseconds)`
Executes JavaScript code **once** after the given delay.

```javascript
setTimeout("moveText()", 20);  // runs moveText() after 20ms
```

### `setInterval(function, milliseconds)`
Executes a function **repeatedly** at a fixed interval.

```javascript
var timer = setInterval(moveText, 100); // every 100ms
clearInterval(timer);                   // stop it
```

### Example Program: moveText.html (Slow Movement using setTimeout)
```html
<!DOCTYPE html>
<html>
<head>
<script>
var currentX = 0;
var targetX  = 300;

function moveText() {
  var txt = document.getElementById("movingText");
  if (currentX < targetX) {
    currentX += 5;
    txt.style.left = currentX + "px";
    setTimeout("moveText()", 30);
  }
}
</script>
</head>
<body onload="setTimeout('moveText()', 100)">
  <p id="movingText"
     style="position:absolute; left:0px; top:100px; font-size:20px;">
    I am moving slowly!
  </p>
</body>
</html>
```

**Key Points:**
- CSS position strings like `"150px"` must be parsed to numbers before arithmetic: `parseInt("150px")` → `150`
- `setTimeout` calls itself recursively for animation effect

---

## 11. Dragging and Dropping Elements

Drag-and-drop is implemented by combining three mouse events: `mousedown`, `mousemove`, and `mouseup`.

### Logic Flow:
1. **`mousedown` (grabber):** Record click offset; assign `mousemove` and `mouseup` handlers.
2. **`mousemove`:** Update `top` and `left` based on new cursor position.
3. **`mouseup` (dropper):** Remove `mousemove` and `mouseup` handlers.

### Example Program: dragNDrop.html
```html
<!DOCTYPE html>
<html>
<head>
<script>
var offsetX, offsetY;

function grabber(e) {
  var elem = document.getElementById("dragBox");
  offsetX = e.clientX - parseInt(elem.style.left);
  offsetY = e.clientY - parseInt(elem.style.top);
  document.onmousemove = mover;
  document.onmouseup   = dropper;
}

function mover(e) {
  var elem = document.getElementById("dragBox");
  elem.style.left = (e.clientX - offsetX) + "px";
  elem.style.top  = (e.clientY - offsetY) + "px";
}

function dropper() {
  document.onmousemove = null;
  document.onmouseup   = null;
}
</script>
</head>
<body>
  <div id="dragBox"
       style="position:absolute; left:100px; top:100px;
              width:120px; height:60px; background:lightblue;
              cursor:move;"
       onmousedown="grabber(event)">
    Drag Me!
  </div>
</body>
</html>
```

---

# UNIT 3 — PART A: INTRODUCTION TO PHP

---

## 1. Origin and Uses of PHP

- Developed by **Rasmus Lerdorf in 1994**
- PHP = **Hypertext Preprocessor** (recursive acronym)
- A **server-side scripting language** embedded in XHTML pages
- File extensions: `.php`, `.phtml`, `.php3`
- Good support for **form processing**, **databases**, and **distributed object architectures**

---

## 2. Overview of PHP

When a `.php` document is requested:
1. Server detects the file extension
2. Passes document to the **PHP processor**
3. PHP processor generates HTML output
4. HTML output is sent to the browser (client **never sees PHP code**)

**Two modes of operation:**
- **Copy mode:** Plain HTML is copied directly to output
- **Interpret mode:** PHP code is executed; output sent to client

**Characteristics:**
- Dynamic typing (untyped variables)
- Associative arrays
- Pattern matching (POSIX and Perl regex)
- Extensive standard libraries

---

## 3. General Syntactic Characteristics

```php
<?php
  // PHP code goes here
  $name = "John";
  echo "Hello, $name!";
?>
```

- PHP code is enclosed between `<?php` and `?>`
- Include files: `include("filename.inc");`
- All **variable names begin with `$`**
- Variable names are **case-sensitive** (`$Name` ≠ `$name`)
- Keywords and function names are **NOT case-sensitive**
- Statements end with **semicolons**
- Comments: `//`, `#` (single-line), `/* */` (multi-line)

---

## 4. Primitives, Operations and Expressions

### Data Types
| Category | Types |
|----------|-------|
| Scalar | boolean, integer, double, string |
| Compound | array, object |
| Special | resource, NULL |

### Variables
```php
$x = 10;           // integer
$price = 3.14;     // double
$name = "Alice";   // string
$flag = TRUE;      // boolean
```

- Unassigned variables have value `NULL`
- `isset($var)` — returns true if variable is assigned
- `unset($var)` — sets variable back to unassigned state

### String Type
```php
$sum = 12;
echo "The sum is $sum";    // → The sum is 12  (variable interpolated)
echo 'The sum is $sum';    // → The sum is $sum (no interpolation)
```

- Double-quoted: escape sequences interpreted, variables interpolated
- Single-quoted: no interpolation, no escape sequences

### Boolean Falsy Values
`0`, `""`, `"0"`, `NULL`, `0.0` → all evaluate to `FALSE`

### Arithmetic
```php
12 / 6;   // → 2 (integer)
12 / 5;   // → 2.4 (double, because remainder exists)
```

### String Operations
```php
$s1 = "Hello";
$s2 = " World";
$s3 = $s1 . $s2;     // Concatenation: "Hello World"
echo strlen($s3);    // → 11
echo $s1{1};         // → "e"  (character access)
```

---

## 5. Output

```php
print("Hello World");                              // simple string
printf("x = %5d is %s\n", $x, $size);             // formatted
// %5d → integer, min 5 chars wide
// %s  → string
// %f  → double
// %.2f → double with 2 decimal places
```

### Example: today.php
```php
<?php
  $today = date("l, F j, Y");
  print("<p>Today is $today</p>");
?>
```

---

## 6. Control Statements

### Selection (if-elseif-else)
```php
<?php
  $score = 75;
  if ($score >= 90) {
    echo "Grade: A";
  } elseif ($score >= 75) {
    echo "Grade: B";
  } elseif ($score >= 60) {
    echo "Grade: C";
  } else {
    echo "Grade: F";
  }
?>
```

### Switch
```php
<?php
  $day = "Mon";
  switch($day) {
    case "Mon": echo "Monday"; break;
    case "Tue": echo "Tuesday"; break;
    default:    echo "Other day";
  }
?>
```

### Loop Statements
```php
// while loop
$i = 1;
while ($i <= 5) {
  echo $i . "<br/>";
  $i++;
}

// for loop
for ($i = 1; $i <= 5; $i++) {
  echo $i . "<br/>";
}

// do-while loop
$i = 1;
do {
  echo $i . "<br/>";
  $i++;
} while ($i <= 5);
```

### Example: Sum of 1 to 100 (powers.php)
```php
<?php
  $sum = 0;
  for ($i = 1; $i <= 100; $i++) {
    $sum += $i;
  }
  echo "Sum = $sum";  // → Sum = 5050
?>
```

---

## 7. Arrays

PHP arrays combine **regular indexed arrays** and **associative (hash) arrays**.

### Array Creation
```php
// Numerically indexed
$fruits = array("apple", "banana", "cherry");
$fruits = array(23, 'xiv', "bob", 777);

// String indexed (associative)
$capitals = array("India" => "Delhi", "France" => "Paris");

// Direct assignment
$colors[] = "red";    // auto-index 0
$colors[] = "blue";   // auto-index 1
$colors["x"] = "xerxes";
```

### Accessing Elements
```php
echo $fruits[0];         // → apple
echo $capitals["India"]; // → Delhi
```

### Useful Array Functions
| Function | Description |
|----------|-------------|
| `array_keys($arr)` | Returns list of keys |
| `array_values($arr)` | Returns list of values |
| `array_key_exists($key, $arr)` | Checks if key exists |
| `is_array($arr)` | Returns true if it's an array |
| `implode(",", $arr)` | Joins array into string |
| `explode(",", $str)` | Splits string into array |
| `array_push($arr, $val)` | Appends value |
| `array_pop($arr)` | Removes and returns last element |
| `sort($arr)` | Sorts by value, reindexes |
| `asort($arr)` | Sorts by value, keeps keys |
| `ksort($arr)` | Sorts by key |
| `unset($arr[$key])` | Removes an element |

### Iterating with foreach
```php
<?php
  $lows = array("Mon" => 23, "Tue" => 18, "Wed" => 27);
  foreach ($lows as $day => $temp) {
    echo "The low on $day was $temp <br/>";
  }
?>
```

### Sequential Access Functions
- `next($arr)` — moves pointer forward, returns value
- `prev($arr)` — moves pointer backward
- `reset($arr)` — moves pointer to beginning
- `each($arr)` — returns current key/value pair, advances pointer

---

## 8. Functions

```php
function functionName($param1, $param2) {
  // body
  return $result;
}
```

**Key Rules:**
- Function names are **NOT case-sensitive**
- Parameters are passed **by value** by default
- Pass by reference: use `&` before parameter — `function foo(&$x)`
- Extra actual params → ignored; extra formal params → unassigned

### Variable Scope
```php
$globalVar = 100;

function myFunc() {
  global $globalVar;    // declares use of global variable
  echo $globalVar;      // now accessible
}
```

### Static Variables
```php
function counter() {
  static $count = 0;   // retained across calls
  $count++;
  echo $count;
}
counter(); // 1
counter(); // 2
counter(); // 3
```

### Example Function Program
```php
<?php
function factorial($n) {
  if ($n <= 1) return 1;
  return $n * factorial($n - 1);
}
echo factorial(5);  // → 120
?>
```

---

## 9. Pattern Matching

PHP supports both **POSIX** and **Perl-Compatible Regular Expressions (PCRE)**.

### Key Functions
| Function | Description |
|----------|-------------|
| `preg_match($pattern, $string)` | Returns 1 if match found |
| `preg_split($pattern, $string)` | Splits string by pattern |
| `preg_replace($pattern, $replacement, $string)` | Replace matches |

### Example Programs
```php
<?php
// Check if string is a valid email
$email = "test@example.com";
if (preg_match("/^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/", $email)) {
  echo "Valid email";
} else {
  echo "Invalid email";
}
?>
```

```php
<?php
// Split string on spaces or commas
$str = "apple, banana orange, cherry";
$words = preg_split("/[\s,]+/", $str);
foreach ($words as $w) echo $w . "<br/>";
?>
```

---

## 10. Form Handling

PHP accesses form data through superglobal arrays:
- `$_POST["fieldname"]` — for `method="post"` forms
- `$_GET["fieldname"]` — for `method="get"` forms

### Example: HTML Form (order.html)
```html
<form action="order.php" method="post">
  Name: <input type="text" name="custName" /><br/>
  Quantity: <input type="text" name="qty" /><br/>
  <input type="submit" value="Order" />
</form>
```

### Example: PHP Processor (order.php)
```php
<?php
  $name = $_POST["custName"];
  $qty  = $_POST["qty"];
  $price = 5.99;
  $total = $qty * $price;
  printf("<p>Order for %s: %d items = $%.2f</p>", $name, $qty, $total);
?>
```

---

## 11. Cookies

**HTTP is stateless** — the server treats each request independently. Cookies help maintain state.

A **cookie** is a key/value pair stored on the browser, sent back with every request to the same server.

### Setting a Cookie
```php
<?php
// Must be called BEFORE any HTML output
setcookie("username", "Alice", time() + 86400);  // expires in 1 day
setcookie("lastVisit", date("Y-m-d"));
?>
```

### Reading a Cookie
```php
<?php
if (isset($_COOKIE["username"])) {
  echo "Welcome back, " . $_COOKIE["username"];
} else {
  echo "Hello, new visitor!";
}
?>
```

### Deleting a Cookie
```php
setcookie("username", "", time() - 3600);  // set expiry in the past
```

**Security Notes:**
- Cookies are only returned to the server that created them
- Browsers allow users to disable/delete cookies
- Systems depending on cookies fail if cookies are disabled

---

## 12. Session Tracking

Sessions allow maintaining state across multiple pages during a user visit. PHP generates a **session ID** to identify each session.

```php
<?php
session_start();  // Must be first line, before any output

// Store data in session
$_SESSION["user"]  = "Alice";
$_SESSION["cart"]  = array("item1", "item2");

// Read session data
echo "Hello " . $_SESSION["user"];

// Destroy session
session_destroy();
?>
```

**Session vs Cookies:**
| Feature | Cookie | Session |
|---------|--------|---------|
| Storage | Client (browser) | Server |
| Security | Less secure | More secure |
| Lifetime | Set by programmer | Until browser closes (default) |
| Size limit | ~4KB | Larger |

---

# UNIT 3 — PART B: XML

---

## 1. Introduction to XML

- **XML** = eXtensible Markup Language
- A **simplified version of SGML** (Standard Generalized Markup Language)
- A **meta-markup language** — used to define other markup languages
- XML is NOT a replacement for HTML; XHTML is an XML-based version of HTML
- **XML is a universal data interchange language**
- In XML, tags carry the **meaning** of their content
- Markup languages defined with XML are called **XML applications**

**Why XML was developed:**
- HTML defines a fixed set of tags — no extensibility
- SGML is too large, complex, and expensive
- SGML requires a formal definition with each new language

---

## 2. XML Syntax

### Basic Rules
- XML declaration at the top: `<?xml version="1.0" encoding="utf-8"?>`
- Every document has a **single root element**
- Every element that can contain content must have a **closing tag**
- Empty elements: `<elementName />`
- XML is **case-sensitive**: `<Name>` ≠ `<name>`
- Attribute values must be in **quotes**
- Comments: `<!-- comment here -->`

### Example XML Document
```xml
<?xml version="1.0" encoding="utf-8"?>
<ad>
  <year>1960</year>
  <make>Cessna</make>
  <model>172</model>
  <location>
    <city>Chicago</city>
    <state>IL</state>
  </location>
</ad>
```

### Attribute vs. Nested Element
```xml
<!-- Option 1: Name as attribute -->
<person name="Sneha M"></person>

<!-- Option 2: Name as nested element -->
<person>
  <name>Sneha M</name>
</person>

<!-- Option 3: Fully structured -->
<person>
  <name>
    <first>Sneha</first>
    <middle></middle>
    <last>M</last>
  </name>
</person>
```

**Rule:** Use nesting to describe structure; use attributes for identifiers.

---

## 3. XML Document Structure

An XML document may be accompanied by:

1. **Schema file** — defines tag set and syntactic rules (DTD or XML Schema)
2. **Style file** — for display (CSS or XSLT)

### CDATA Sections
```xml
<description>
  <![CDATA[
    This content is NOT parsed: <b>bold</b> & "quotes" work here.
  ]]>
</description>
```
Used when content contains characters like `<` and `&` that would otherwise need escaping.

---

## 4. Document Type Definitions (DTD)

A **DTD** is a set of structural rules that define the valid structure of an XML document.

**Four DTD Keywords:**
| Keyword | Purpose |
|---------|---------|
| `ELEMENT` | Defines element structure |
| `ATTLIST` | Defines attributes of elements |
| `ENTITY` | Defines reusable content |
| `NOTATION` | Defines data type notations |

### Declaring Elements
```dtd
<!ELEMENT memo (from, to, date, re)>
```

**Modifiers for child elements:**
| Symbol | Meaning |
|--------|---------|
| `+` | One or more |
| `*` | Zero or more |
| `?` | Zero or one (optional) |
| _(none)_ | Exactly once |

```dtd
<!ELEMENT person (parent+, age, spouse?, sibling*)>
```

**Leaf node (content) types:**
```dtd
<!ELEMENT city (#PCDATA)>   <!-- parsable character data -->
<!ELEMENT image EMPTY>      <!-- no content -->
<!ELEMENT anything ANY>     <!-- any content allowed -->
```

### Declaring Attributes
```dtd
<!ATTLIST element-name attribute-name attribute-type default-value>

<!-- Example -->
<!ATTLIST person
          id    CDATA  #REQUIRED
          age   CDATA  #IMPLIED
          type  CDATA  "human">
```

**Default value types:**
- A literal value — used when no value provided
- `#REQUIRED` — attribute must always be given
- `#IMPLIED` — attribute is optional, no default
- `#FIXED value` — attribute always has this fixed value

### Declaring Entities
```dtd
<!ENTITY st "Sachin Tendulkar">
```
Used in XML as: `&st;` → expands to "Sachin Tendulkar"

### Internal vs External DTD
```xml
<!-- Internal DTD -->
<!DOCTYPE memo [
  <!ELEMENT memo (from, to, date, body)>
  <!ELEMENT from (#PCDATA)>
  <!ELEMENT to   (#PCDATA)>
  <!ELEMENT date (#PCDATA)>
  <!ELEMENT body (#PCDATA)>
]>

<!-- External DTD -->
<!DOCTYPE memo SYSTEM "memo.dtd">
```

### Complete Example with DTD and XML
```xml
<?xml version="1.0"?>
<!DOCTYPE book [
  <!ELEMENT book (title, author, year)>
  <!ELEMENT title  (#PCDATA)>
  <!ELEMENT author (#PCDATA)>
  <!ELEMENT year   (#PCDATA)>
]>
<book>
  <title>Web Technologies</title>
  <author>Sebesta</author>
  <year>2008</year>
</book>
```

---

## 5. Namespaces

When combining XML tag sets from different sources, **naming conflicts** can arise. Namespaces resolve this.

**Problem Example:** A furniture catalog has `<table>` (furniture) and XHTML also has `<table>` (HTML table) — conflict!

### Namespace Declaration
```xml
<element_name xmlns[:prefix]="URI">
```

```xml
<!-- Using a namespace prefix -->
<birds xmlns:bd="http://www.birds.org/names/species">
  <bd:sparrow>House Sparrow</bd:sparrow>
</birds>
```

### Multiple Namespaces
```xml
<states xmlns="http://www.states-info.org/states"
        xmlns:cap="http://www.states-info.org/state-capitals">
  <state>
    <name>South Dakota</name>
    <population>754844</population>
    <capital>
      <cap:name>Pierre</cap:name>
      <cap:population>12429</cap:population>
    </capital>
  </state>
</states>
```

**Key Points:**
- Default namespace (no prefix) applies to unprefixed elements
- Attributes are NOT included in namespaces
- URI is just a unique string — does not need to be a real URL

---

## 6. XML Schemas

### Disadvantages of DTD (why schemas were created)
1. DTDs are written in a **different syntax** from XML (not XML themselves)
2. DTDs do not support **data type constraints** (everything is text)
3. Two syntaxes to learn — one for documents, one for structure

### XML Schema Advantages
- Written **in XML itself**
- Supports **data types** (integer, date, string, etc.)
- Supports **complex types** with attributes and nested elements

### Schema Namespace Declaration
```xml
<xsd:schema
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  targetNamespace="http://cs.uccs.edu/planeSchema"
  xmlns="http://cs.uccs.edu/planeSchema"
  elementFormDefault="qualified">
```

### Simple vs Complex Types
- **Simple type:** Only text content, no child elements
- **Complex type:** Can have child elements and/or attributes

### Example XML Schema
```xml
<?xml version="1.0"?>
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">

  <xsd:element name="book">
    <xsd:complexType>
      <xsd:sequence>
        <xsd:element name="title"  type="xsd:string"/>
        <xsd:element name="author" type="xsd:string"/>
        <xsd:element name="year"   type="xsd:integer"/>
        <xsd:element name="price"  type="xsd:decimal"/>
      </xsd:sequence>
    </xsd:complexType>
  </xsd:element>

</xsd:schema>
```

---

## 7. Displaying Raw XML Documents

When a plain XML file is opened in a browser (like Firefox), the browser displays the raw XML tree — noting that no style information is associated.

```xml
<?xml version="1.0"?>
<catalog>
  <plane>
    <year>1977</year>
    <make>Cessna</make>
    <model>Skyhawk</model>
  </plane>
</catalog>
```
Opening this in Firefox shows a collapsible tree — no styling.

---

## 8. Displaying XML Documents with CSS

An XML-stylesheet processing instruction links an XML document to a CSS file.

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/css" href="planes.css"?>
<catalog>
  <plane>
    <year>1977</year>
    <make>Cessna</make>
  </plane>
</catalog>
```

### planes.css
```css
catalog {
  display: block;
  font-family: Arial;
}
plane {
  display: block;
  border: 1px solid black;
  margin: 10px;
  padding: 10px;
}
year {
  display: block;
  font-weight: bold;
  color: blue;
}
make {
  display: block;
  color: green;
}
```

---

## 9. XSLT Style Sheets

**XSL (Extensible Stylesheet Language)** family:
- **XSLT** — transforms XML documents into other formats
- **XPath** — selects parts of an XML document
- **XSL-FO** — specifies formatting for printed pages

### How XSLT Works
```
XML Document  +  XSLT Stylesheet  →  XSLT Processor  →  Output (XHTML/text/etc.)
```

Link XSLT stylesheet to XML document:
```xml
<?xml-stylesheet type="text/xsl" href="stylesheet.xsl"?>
```

### XSLT Structure
```xml
<xsl:stylesheet
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
    xmlns="http://www.w3.org/1999/xhtml"
    version="1.0">

  <xsl:template match="/">
    <!-- Template content applied when root is matched -->
  </xsl:template>

</xsl:stylesheet>
```

### XSLT Templates and XPath
- `match="/"` — matches the root node
- `match="plane"` — matches all `<plane>` elements
- `<xsl:value-of select="year"/>` — outputs content of `<year>` element
- `<xsl:apply-templates select="plane"/>` — applies templates to child elements

### Example: xslplane.xml
```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="xslplane.xsl"?>
<plane>
  <year>1977</year>
  <make>Cessna</make>
  <model>Skyhawk</model>
  <color>Light blue and white</color>
</plane>
```

### Example: xslplane.xsl (Single Record)
```xml
<?xml version="1.0"?>
<xsl:stylesheet
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
    xmlns="http://www.w3.org/1999/xhtml"
    version="1.0">

  <xsl:template match="/">
    <html>
      <body>
        <h2>Aircraft Information</h2>
        <xsl:apply-templates select="plane"/>
      </body>
    </html>
  </xsl:template>

  <xsl:template match="plane">
    <p>
      <b>Year:</b>  <xsl:value-of select="year"/>  <br/>
      <b>Make:</b>  <xsl:value-of select="make"/>  <br/>
      <b>Model:</b> <xsl:value-of select="model"/> <br/>
      <b>Color:</b> <xsl:value-of select="color"/> <br/>
    </p>
  </xsl:template>

</xsl:stylesheet>
```

### Example: Processing Multiple Records with for-each
```xml
<!-- xslplanes.xsl — handles multiple <plane> elements -->
<?xml version="1.0"?>
<xsl:stylesheet
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
    xmlns="http://www.w3.org/1999/xhtml"
    version="1.0">

  <xsl:template match="/">
    <html>
      <body>
        <h2>Aircraft Catalog</h2>
        <table border="1">
          <tr>
            <th>Year</th><th>Make</th><th>Model</th>
          </tr>
          <xsl:for-each select="catalog/plane">
            <xsl:sort select="year" data-type="number"/>
            <tr>
              <td><xsl:value-of select="year"/></td>
              <td><xsl:value-of select="make"/></td>
              <td><xsl:value-of select="model"/></td>
            </tr>
          </xsl:for-each>
        </table>
      </body>
    </html>
  </xsl:template>

</xsl:stylesheet>
```

---

## Quick Reference: Key Exam Points

### Unit 2 Summary
| Topic | Key Concept |
|-------|-------------|
| Positioning | `position: absolute/relative/static` |
| Moving | Change `style.left` and `style.top` (add `"px"`) |
| Visibility | `style.visibility = "visible"/"hidden"` |
| Colors/Fonts | `style.backgroundColor`, `style.fontSize` etc. |
| Dynamic Content | Change `element.value` or `element.innerHTML` |
| Stacking | `style.zIndex` — higher value = on top |
| Mouse location | `e.clientX`, `e.clientY` |
| Slow movement | `setTimeout("fn()", ms)` and `setInterval(fn, ms)` |
| Drag & Drop | mousedown → grabber; mousemove → mover; mouseup → dropper |

### Unit 3 (PHP) Summary
| Topic | Key Concept |
|-------|-------------|
| Variables | Start with `$`, case-sensitive |
| String concat | `.` operator |
| Output | `print()` or `printf()` |
| Arrays | `array()`, `foreach`, `sort/asort/ksort` |
| Functions | `function name($params) { return val; }` |
| Pattern matching | `preg_match()`, `preg_split()` |
| Forms | `$_POST["name"]` or `$_GET["name"]` |
| Cookies | `setcookie()`, `$_COOKIE[]` |
| Sessions | `session_start()`, `$_SESSION[]` |

### Unit 3 (XML) Summary
| Topic | Key Concept |
|-------|-------------|
| Declaration | `<?xml version="1.0" encoding="utf-8"?>` |
| DTD elements | `<!ELEMENT name (content)>` |
| DTD attributes | `<!ATTLIST elem attr CDATA #REQUIRED>` |
| DTD entities | `<!ENTITY name "value">` → used as `&name;` |
| Namespaces | `xmlns:prefix="URI"` |
| XML Schema | Written in XML; supports data types |
| CSS display | `<?xml-stylesheet type="text/css" href="file.css"?>` |
| XSLT | `<?xml-stylesheet type="text/xsl" href="file.xsl"?>` |
| XSLT template | `<xsl:template match="element">` |
| Output value | `<xsl:value-of select="child"/>` |
| Loop | `<xsl:for-each select="path">` |
| Sort | `<xsl:sort select="field" data-type="number"/>` |

---

*Notes compiled from course material — Chapters 6, 11, and 7 (Sebesta, Web Programming)*
