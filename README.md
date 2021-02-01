# Preamble

This is a fork of an [existing sample](https://github.com/ghammock/LaTeX_Listings_JavaScript_ES6). Main adjustments are completion of the keywords and different layout of the compilation (to allow as well a pagebreak).

# LaTeX Listings &mdash; JavaScript & ES6

The LaTeX [listings](https://ctan.org/pkg/listings?lang=en) package does not include (by default) a language specification for JavaScript/ECMAScript.  However, the package provides the capability to create custom languages and styles based on built-in methods.  These methods were used to create listings languages for JavaScript and ES6 (ECMAScript 2015).

# Basic JavaScript

First, the JavaScript version 1.1 definition is built in a way to allow for modifications to be built upon the basic language.  Reference the [JavaScript v1.1 specification](http://hepunx.rl.ac.uk/~adye/jsspec11/titlepg2.htm).

## The code
```latex
\lstdefinelanguage{JavaScript}{
  morekeywords=[1]{break, continue, delete, else, for, function, if, in,
    new, return, this, typeof, var, void, while, with},
  % Literals, primitive types, and reference types.
  morekeywords=[2]{false, null, true, boolean, number, undefined,
    Array, Boolean, Date, Math, Number, String, Object},
  % Built-ins.
  morekeywords=[3]{eval, parseInt, parseFloat, escape, unescape},
  sensitive,
  morecomment=[s]{/*}{*/},
  morecomment=[l]//,
  morecomment=[s]{/**}{*/}, % JavaDoc style comments
  morestring=[b]',
  morestring=[b]"
}[keywords, comments, strings]
```

## Example

### Markup

```tex
\begin{lstlisting}[style=JavaScript, caption={JavaScript Listing}]
(function () {
  var user = getCookie("username");
  document.getElementById("#date-field").innerHTML = new Date();
  document.getElementById("#greetings").innerHTML =
    "<p>Hello, " + user.name + ".</p>";
})()

function getCookie(cname) {
  var name = cname + "=";
  var decodedCookie = decodeURIComponent(document.cookie);
  var ca = decodedCookie.split(';');
  for(var i = 0; i < ca.length; ++i) {
    var c = ca[i];
    while (c.charAt(0) == ' ') {
      c = c.substring(1);
    }
    if (c.indexOf(name) == 0) {
      return c.substring(name.length, c.length);
    }
  }
  return "";
}
\end{lstlisting}
```

### Resulting Typeset
![Javascript Listing Example](img/js_listing_example.png)

# ECMAScript 2015 (ES6)

ES6 adds additional keywords and interpolated string capability.  So these need to be reflected in the language defintion for `listings`.  The `ECMAScript2015` dialect of the `JavaScript` language uses the base language and adds the additional keywords and string interpolation.

There is an alias to map the language `ES6` to the `ECMAScript2015` dialect such that `language=ES6` is the same as `language=[ECMAScript2015]JavaScript`.

```tex
\lstalias[]{ES6}[ECMAScript2015]{JavaScript}
```

## The code
```latex
\lstdefinelanguage[ECMAScript2015]{JavaScript}[]{JavaScript}{
  morekeywords=[1]{await, async, case, catch, class, const, default, do,
    enum, export, extends, finally, from, implements, import, instanceof,
    let, static, super, switch, throw, try},
  morestring=[b]` % Interpolation strings.
}
```

## Example

### Markup
```tex
//Option 1 - Use try catch within the function
async function doubleAndAdd(a, b) {
	try {
		a = await doubleAfter1Sec(a);
		b = await doubleAfter1Sec(b);
	} catch (e) {
		return NaN; //return something
	}
	return a + b;
}

//New usage:
doubleAndAdd('one', 2).then(console.log); // NaN
doubleAndAdd(1, 2).then(console.log); // 6

function doubleAfter1Sec(param) {
	return new Promise((resolve, reject) => {
		setTimeout(function() {
			let val = param * 2;
			isNaN(val) ? reject(NaN) : resolve(val);
		}, 1000);
	});
}


	// Old Syntax
	function oldOne() {
		console.log('Hello World..!');
	}
	// New Syntax in ES6
	var newOne = () => {
		console.log('Hello World..!');
	}
```

### Resulting Typeset
![ES6 Listing Example](img/es6_sample.png)

# Styling the language
The `listings` package also has the built-in capacity for custom styling the language definitions.  The styles presented in the typeset images were generated using:

```tex
% Requires package: color.
\definecolor{mediumgray}{rgb}{0.3, 0.4, 0.4}
\definecolor{mediumblue}{rgb}{0.0, 0.0, 0.8}
\definecolor{forestgreen}{rgb}{0.13, 0.55, 0.13}
\definecolor{darkviolet}{rgb}{0.58, 0.0, 0.83}
\definecolor{royalblue}{rgb}{0.25, 0.41, 0.88}
\definecolor{crimson}{rgb}{0.86, 0.8, 0.24}

\lstdefinestyle{JSES6Base}{
	basicstyle=\ttfamily\small,
	breakatwhitespace=true,
	breaklines=true,
	captionpos=b,
	columns=fullflexible,
	commentstyle=\color{mediumgray}\upshape,
	emph={},
	emphstyle=\color{crimson},
	extendedchars=true,  % requires inputenc
	identifierstyle=\color{black},
	keepspaces=true,
	keywordstyle=\color{mediumblue},
	keywordstyle={[2]\color{darkviolet}},
	keywordstyle={[3]\color{royalblue}},
	numbers=left,
	numbersep=2pt,
	numberstyle=\tiny\color{black},
	rulecolor=\color{red},
	showlines=true,
	showspaces=false,
	showstringspaces=false,
	showtabs=false,
	stringstyle=\color{forestgreen},
	tabsize=2,
	title=\lstname,
	upquote=true  % requires textcomp
}

\lstdefinestyle{JavaScript}{
  language=JavaScript,
  style=JSES6Base
}
\lstdefinestyle{ES6}{
  language=ES6,
  style=JSES6Base
}
```
