<!-- $theme: gaia -->
<!-- *template: invert -->

Zeppelin Helium : Spell
===  

#### [1ambda](https://github.com/1ambda)
#### ZEPL
##### March 2, 2017

---

<!-- page_number: true -->
<!-- *template: invert -->

### What is Spell?

Frontend interpreter ==runs on browser== not in backend. 

- **pluggable**
- **written in javascript**
- **can be display system**
- available in **0.8.0-SNAPSHOT** ([ZEPPELIN #1940](https://github.com/apache/zeppelin/pull/1940))

---

<!-- *page_number: false -->
<!-- *template: invert -->

### What?! :flushed:

---

<!-- *template: invert -->


#### Background: ==Interpreter==

![center](https://raw.githubusercontent.com/apache/zeppelin/336df5617b3a2ca43a8fe7b600508d8e8b0b9a25/docs/assets/themes/zeppelin/img/interpreter.png)

---

<!-- *template: invert -->

#### Background: ==Interpreter== (cont.)

* Interpreter consumes paragraph text and 
render output.
* Each interpreter has ==magic==. For example,
 the [markdown interpreter](https://github.com/apache/zeppelin/blob/master/markdown/src/main/java/org/apache/zeppelin/markdown/Markdown.java#L39) uses `%markdown`


```go
%spark 

println("Hello, Zeppelin!")
```

```md
%markdown

## Hello, Zeppelin
```

---

<!-- *template: invert -->

#### Background: ==Display System==

- Display system renders result which already translated by backend interpreters **on browser** 
([Basic Display System](http://zeppelin.apache.org/docs/0.7.0/displaysystem/basicdisplaysystem.html#basic-display-system-in-apache-zeppelin): `%html`, `%table`, `%angular`)

```go
%spark 

val total = 195
println(s"%html <h3>result is ${total}</h3>") 

// will be interpreted by spark interpreter
// `%html <h3>total value is 195<h3>`
```

- Can we do better?  (e.g `%markdown` display type)

---

<!-- *template: invert -->

### Motivation: Spell

- [How can i pass variables from spark to markdown in Zeppelin](http://stackoverflow.com/questions/41543593/zeppelin-pass-variable-from-spark-to-markdown-to-generate-dynamic-narrative-te)
- Do we really need ==backend== markdown interpreter? ([awesome js markdown library](https://markdown-it.github.io/))
- What if we implement `%markdown` as a **frontend** interpreter?
- Then, **==we can also use them as display system==** because it runs on browser like `%html` :smirk:

---

<!-- *template: invert -->


### This is Spell :sunglasses:

Frontend interpreter runs on browser not in backend. 

- **pluggable**: can be installed / removed easily 
([Helium Online Registry](http://zeppelin.apache.org/helium_packages.html))
- **written in javascript**: can utilise exisiting libraries
([flowchart](http://flowchart.js.org/), [sigmajs](http://sigmajs.org/), [vega](http://vega.github.io/vega-editor/index.html?mode=vega), [papaparse](http://papaparse.com/), ...)
- **can be display system as well** ([ZEPPELIN-2089](https://issues.apache.org/jira/browse/ZEPPELIN-2089))

---

<!-- *page_number: false -->
<!-- *template: gaia -->

### DEMO

#### ==1.== zeppelin-flowchart-spell
#### ==2.== zeppelin-csv-spell
#### ==3.== zeppelin-json-spell
#### ==4.== zeppelin-translator-spell

---

<!-- *template: invert -->

### I wanna create my own spell :hushed:

- [Doc: How to create new spell](http://zeppelin.apache.org/docs/0.8.0-SNAPSHOT/development/writingzeppelinspell.html)
- [Basic Examples](https://github.com/apache/zeppelin/tree/master/zeppelin-examples)
- [Published Spells](http://zeppelin.apache.org/helium_packages.html)
- [Configurtion Support: #1982](https://github.com/apache/zeppelin/pull/1982)

- Ideas: `%slack`, [`%sigma`](http://sigmajs.org/), [`%vega`](http://vega.github.io/vega-editor/index.html?mode=vega)

---

<!-- *template: gaia -->
<!-- *page_number: false -->

### DEMO
#### ==1.== zeppelin-echo-spell
#### ==2.== zeppelin-markdown-spell

---

<!-- *template: invert -->

### Creating Spell: ==interpret()==

- every spell takes paragraph text (==string==)
- returns ==SpellResult==
- [spell-base.js](https://github.com/1ambda/zeppelin/blob/dbc4f10fd3ee556d5e38cb4f6e3966661eaf69a9/zeppelin-web/src/app/spell/spell-base.js#L37-L39)
- example: [zeppelin-echo-spell](https://github.com/1ambda/zeppelin-echo-spell/blob/37703288cb1a9bd1af1d90bef907d8bcbef78fae/index.js#L24-#L32)

```javascript
interpret(paragraphText) {
  return new SpellResult(paragraphText)
}
```

---

<!-- *template: invert -->

### Creating Spell: ==SpellResult==

- ==SpellResult(data, displayType)==
- [displayType](https://github.com/apache/zeppelin/blob/0589e27e7bb84ec81e1438bcbf3f2fd80ee5a963/zeppelin-web/src/app/spell/spell-result.js#L26-#L32) can be optional (default is ==`%text`==)


```go
import {
    SpellBase,
    SpellResult,
    DefaultDisplayType,
} from 'zeppelin-spell';

interpret(paragraphText) {
  const html = `<h2>${paragraphText}</h2>`
  return new SpellResult(html, DefaultDisplayType.HTML)
}
```

---

<!-- *template: invert -->

### Creating Spell: ==SpellResult== (cont.)

- ==SpellResult== supports multiple results: ==add()==
- See also: [Multiple paragraph results (PR #1658)](https://github.com/apache/zeppelin/pull/1658)

```go
interpret(paragraphText) {
   const html = '<h4>Hello</h4>'
   const text = 'Spell'

   return new SpellResult()
    .add(html, DefaultDisplayType.HTML)
    .add(text, DefaultDisplayType.TEXT /** optional */)
}
```

---

<!-- *template: invert -->

### Creating Spell: ==SpellResult== (cont.)

- pass ==Promise== for **API call** (async)
- psss ==Function== which takes elem id to **draw DOM**
- [default display types](https://github.com/apache/zeppelin/blob/0589e27e7bb84ec81e1438bcbf3f2fd80ee5a963/zeppelin-web/src/app/spell/spell-result.js#L26-#L32)


|data|displayType|example|
|:--:|:-:|:--|
|==String==|ALL (except `%element`)|[markdown-spell](https://github.com/apache/zeppelin/blob/336df5617b3a2ca43a8fe7b600508d8e8b0b9a25/zeppelin-examples/zeppelin-example-spell-markdown/index.js#L34-L40)
|==Promise==|ALL (except `%element`)|[translator-spell](https://github.com/apache/zeppelin/blob/336df5617b3a2ca43a8fe7b600508d8e8b0b9a25/zeppelin-examples/zeppelin-example-spell-translator/index.js#L47)
|==Function==|ELEMENT (`%element`)|[flowchart-spell](https://github.com/apache/zeppelin/blob/336df5617b3a2ca43a8fe7b600508d8e8b0b9a25/zeppelin-examples/zeppelin-example-spell-flowchart/index.js#L38-L41)

---

<!-- *template: invert -->

### Creating Spell: ==Configuration== 

- Actually, ==interpret()== takes ==config== as the second argument

```javascript
interpret(paragraphText, config) { ... }
```

- Define config specification in [package.json](https://github.com/1ambda/zeppelin/blob/dbc4f10fd3ee556d5e38cb4f6e3966661eaf69a9/zeppelin-examples/zeppelin-example-spell-echo/zeppelin-example-spell-echo.json#L24-L30) like

```json
  "config": {
    "repeat": {
      "type": "number",
      "description": "How many times to repeat",
      "defaultValue": 1
    }
  },
```

---

<!-- *template: invert -->

### (unresolved) Helium, Spell Issues

we need your help :sob:


- [ZEPPELIN-2089](https://issues.apache.org/jira/browse/ZEPPELIN-2089): Use spell as display system with backend interpreters
- [ZEPPELIN-2088](https://issues.apache.org/jira/browse/ZEPPELIN-2088): Evaluate helium bundles one by one
- [ZEPPELIN-2122](https://issues.apache.org/jira/browse/ZEPPELIN-2122): Add execution time to paragraphs executed by spell
- [Other Issues](https://issues.apache.org/jira/issues/?filter=-2&jql=project%20%3D%20ZEPPELIN%20AND%20(text%20~%20helium%20OR%20text%20~%20spell)%20and%20status%20%3D%20Open%20and%20assignee%20%3D%20empty%20ORDER%20BY%20createdDate%20DESC)

---

<!-- *template: invert -->

### References

- Image: [Zeppelin Interpreter Architecture](http://zeppelin.apache.org/docs/latest/development/writingzeppelininterpreter.html)

### Resources

- Slide: [github.com/1ambda/talk/2017/fly-zeppelin](https://github.com/1ambda/talk/tree/master/2017/fly-zeppelin)

---

<!-- *page_number: false -->
<!-- *template: gaia -->


# Thanks :smiley: 
