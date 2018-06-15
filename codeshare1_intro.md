# codeshare1: intro

### Codeshare format quick overview

```go
// This is first document from "codeshare" series.
//
// See "En" and "Ru" controls right below this snippet?
// Open either of them and you'll get description (the overview).
// Every overview includes both audio and text.
```
<table><tr><td><details><summary>En</summary>
<p>All documents from this series are structured this way:
sequence of code snippets with associated overview texts,
which you are currently reading (or listening). There is also audio available
for most of these overviews. The advantage of audio recording
is an ability to concentrate your eyes on code while listening to explanation.</p>

<p>All code snippets contain comments that should make it possible
to interpret the message properly even without reading the overview notes.
This makes it possible to use these pages as a source of code recipes.</p>
</details></td><td><details><summary>Ru</summary>
<p>Все документы из этой серии структурированы следующим образом:
последовательность сниппетов кода, к каждому из которых прилагается
описание, которое вы в данный момент читаете (или слушаете).
К большинству таких описаний доступна аудио версия.
Преимущество аудиозаписи в том, что вы можете продолжать концентрировать
ваш взгляд на коде, воспринимая пояснения к нему через слух.</p>

<p>В коде содержатся комментарии, которые позволяют разобраться в нём
без использования пояснительных записей, что позволяет использовать
данные страницы как источник рецептов.</p>
</details></td></tr></table>

### strparse and astequal packages

```go
package main

import (
    "fmt"
    "go/ast"
    "go/token"
    "reflect"

    "github.com/go-toolsmith/astequal"
    "github.com/go-toolsmith/strparse"
)

func main() {
    // Both x and y hold same structurally equal AST.
    x := strparse.Expr(`1 + 2`)
    y := &ast.BinaryExpr{
        Op: token.ADD, // Binary "+" operator
        X:  &ast.BasicLit{Kind: token.INT, Value: "1"}, // args[0]: 1
        Y:  &ast.BasicLit{Kind: token.INT, Value: "2"}, // args[1]: 2
    }

    fmt.Println(reflect.DeepEqual(x, y)) // => false
    fmt.Println(astequal.Expr(x, y))     // => true
}
```
<table><tr><td><details><summary>En</summary>
<p>In this example we're using two packages from <a href="https://github.com/go-toolsmith">go-toolsmith</a>: <a href="https://github.com/go-toolsmith/strparse">strparse</a>
and <a href="https://github.com/go-toolsmith/astequal">astequal</a>.</p>

<p>strparse package makes it easier to create simple AST nodes.
Basically, it's a simple wrapper around parser.ParseExpr and parser.ParseFile from
go/parser package. Its especially useful for tests and examples.</p>

<p>astequal package defines AST node equallity operations.
Unlike reflect.DeepEqual, it does not compare source positions, which
leads to different results in the snippet above.</p>

<p>No all code snippets from this series are like this.
Most examples contain only minimal amount of code required to describe a single idea.</p>
</details></td><td><details><summary>Ru</summary>
<p>В этом примере используются два пакета из <a href="https://github.com/go-toolsmith">go-toolsmith</a>: <a href="https://github.com/go-toolsmith/strparse">strparse</a>
и <a href="https://github.com/go-toolsmith/astequal">astequal</a>.</p>

<p>Пакет strparse упрощает создание простых AST элементов и является простой обёрткой
вызовов parser.ParseExpr и parser.ParseFile из пакета go/parser. Особенно полезен для примеров и тестов.</p>

<p>Пакет astequal позволяет сравнивать два AST элемента на равенство.
В отличие от reflect.DeepEqual, он не сравнивает позиции, отсюда разный
результат сравнения, наблюдаемый в коде выше.</p>

<p>Не все примеры из этой серии выглядят таким образом.
Большая часть примеров кода содержит лишь минимально необходимые элементы для описания одной идеи.</p>
</details></td></tr></table>

### astequal vs reflect.DeepEqual

```go
const statement = `{ f1(); f2() }`
x := strparse.Stmt(statement)
y := strparse.Stmt(statement)

// Sometimes reflect.DeepEqual works as you might expect.
// But usually you can't depend on it. See overview for explanation.
fmt.Println(reflect.DeepEqual(x, y)) // => true
astequal.Stmt(x, y)                  // => true
```

<table><tr><td><details><summary>En</summary>
<p>reflect.DeepEqual will correctly work only for nodes that have identical attributes.
Usually, this property satisfied when comparing AST created from the very same source
and for artifically created nodes.</p>

<p>In addition to token.Pos, there is also an ast.Object that makes getting predictable results
from reflect.DeepEqual problematic. Newer programs usually rely on types.Object,
but parser does initialize ast.Ident.Obj with these objects nonetheless.
This leads to another class of "unequal" nodes.</p>

<p>Because of these reasons it's impossible to correctly compare two AST nodes
with reflect.DeepEqual for syntactical equallity.</p>

<p>astequal does just that, compares for syntacitcal equallity.
What has the same AST structure treated as identical.</p>
</details></td><td><details><summary>Ru</summary>
<p>reflect.DeepEqual будет корректно работать только для случаев, когда у двух деревьев
совпадают все атрибуты. Обычно, это свойство соблюдается при сравнении AST, созданных
из одинаковых исходников и для искуственно созданных элементов.</p>

<p>Кроме позиций token.Pos, предсказуемой работе reflect.DeepEqual мешают ast.Object.
В новых программах, обычно, используют types.Object, но парсер всё равно заполняет
ast.Ident.Obj этими объектами. Это приводит к другому классу "неодинаковых" узлов.</p>

<p>Из-за этих причин корректно сравнить два AST элемента на синтаксическое равенство
через reflect.DeepEqual невозможно.</p>

<p>astequal сравнивает именно на синтаксическое равенство.
То, что имеет одинаковую AST структуру, считается идентичным.</p>
</details></td></tr></table>

### Comparing printed representation and performance

```go
var emptyFileSet = token.NewFileSet()

func nodeBytes(x ast.Node) []byte {
    var buf bytes.Buffer
    printer.Fprint(&buf, emptyFileSet, x)
    return buf.Bytes()
}

func equalNode(x, y ast.Node) bool {
    // Works even if x and y source positions differ.
    // Also ignores ast.Object-related issues.
    // But, it's even slower than reflect.DeepEqual.
    return bytes.Equal(nodeBytes(x), nodeBytes(y))
}
```
<table><tr><td><details><summary>En</summary>
<p>Comparing nodes printed representations is a more correct way to
do syntactical equallity check. This method works, but it's not optimal from performance point of view.</p>

<pre>
BenchmarkEqualExpr/astequal.Expr-8      5000000    325 ns/op     0 B/op    0 allocs/op
BenchmarkEqualExpr/astequal.Node-8      5000000    408 ns/op     0 B/op    0 allocs/op
BenchmarkEqualExpr/reflect.DeepEqual-8   100000  18088 ns/op  5094 B/op   78 allocs/op
BenchmarkEqualExpr/printer.Fprint-8       30000  42740 ns/op  8320 B/op  364 allocs/op
</pre>

<p>100+ times improvement is usually good enough to consider alternatives.</p>
</details></td><td><details><summary>Ru</summary>
<p>Более корретным способом проверить на синтаксическое равенство является сравнение
печатаемого представления элементов. Этот подход работает, но не является оптимальным с точки зрения производительности.</p>

<pre>
BenchmarkEqualExpr/astequal.Expr-8      5000000    325 ns/op     0 B/op    0 allocs/op
BenchmarkEqualExpr/astequal.Node-8      5000000    408 ns/op     0 B/op    0 allocs/op
BenchmarkEqualExpr/reflect.DeepEqual-8   100000  18088 ns/op  5094 B/op   78 allocs/op
BenchmarkEqualExpr/printer.Fprint-8       30000  42740 ns/op  8320 B/op  364 allocs/op
</pre>

<p>Разница более чем в 100 раз обычно является достаточной для рассмотрения альтернативы.</p>
</details></td></tr></table>

### Deep copy of AST

```go
x := strparse.Expr(`1 + 2`).(*ast.BinaryExpr)
y := astcopy.BinaryExpr(x)       // Clone x expression into y
fmt.Println(astequal.Expr(x, y)) // => true

// Now modify x and make sure y is not modified.
z := astcopy.BinaryExpr(y)
x.Op = token.SUB
fmt.Println(astequal.Expr(y, z)) // => true
fmt.Println(astequal.Expr(x, y)) // => false
```
<table><tr><td><details><summary>En</summary>
<p><a href="https://github.com/go-toolsmith/astcopy">astcopy</a> is another simple yet useful package.
It allows you to perform ast.Node deep copy with a single call.</p>

<p>You may want to clone AST when you need to get modified object without mutating original tree.</p>

<p>astcopy does copy associated comments, but ignores ast.Object elements.
In cause your tool needs to copy these objects, please, tell us about it in <a href="https://github.com/go-toolsmith/astcopy/issues/1">issue#1</a></p>
</details></td><td><details><summary>Ru</summary>
<p>Ещё одним простым и полезным пакетом является <a href="https://github.com/go-toolsmith/astcopy">astcopy</a>.
С его помощью можно копировать любой ast.Node.</p>

<p>Клонировать деревья может быть полезно при желании получить модифицированный объект,
не затрагивая при этом исходное дерево.</p>

<p>astcopy копирует ассоциированные комментарии, но игнорирует ast.Object элементы.
В случае, если в ваших задачах требуется копирование этих объектов,
расскажите нам об этом в <a href="https://github.com/go-toolsmith/astcopy/issues/1">issue#1</a></p>
</details></td></tr></table>

### nil vs empty slice matters

```go
func main() {
	proto := strparse.Expr(`func(x, y int) (a, b string)`).(*ast.FuncType)

	x := astcopy.FuncType(proto)
	x.Results.List[0].Names = nil // Nil names slice
	astfmt.Println(x)             // => func(x, y int) string

	y := astcopy.FuncType(proto)
	y.Results.List[0].Names = []*ast.Ident{} // Empty names slice
	astfmt.Fprintf(y)                        // => func(x, y int) (string)
}
```
<table><tr><td><details><summary>En</summary>
<p>Sometimes you need to pay extra attention for slices when working with AST.
Empty and nil slices may lead to a different behavior.
Code abode demonstrates how it affects <a href="https://golang.org/pkg/go/printer/">go/printer</a> package.
Note the extra surrounding parenthesis around output parameters.</p>

<p>When copying AST slice, you should do early nil check to avoid
returning empty slice as a copy of nil slice.
By the way, astcopy package provides several convenience functions for slice copying that
do the proper right for you.</p>

<p>You may had also noticed a href="https://github.com/go-toolsmith/astfmt">astfmt</a> package usage.
It makes AST printing and convertions of it to string easier.</p>
</details></td><td><details><summary>Ru</summary>
<p>В некоторых случаях при работе с AST следует уделять особое внимание слайсам.
Пустой и nil слайсы могут вести к разному поведению.
Код выше демонстрирует это на примере пакета <a href="https://golang.org/pkg/go/printer/">go/printer</a>.
Обратите внимание на наличие/отсутствие скобок вокруг возвращаемых параметров.</p>

<p>При копировании слайсов стоит делать раннюю проверку на nil, чтобы
избежать возвращения копии nil слайса в виде пустого слайса.
Кстати, astcopy предоставляет функции для копирования слайсов, которые учитывают эти особенности.</p>

<p>Вы также могли заметить использование нового пакета, <a href="https://github.com/go-toolsmith/astfmt">astfmt</a>.
Он позволяет с лёгкостью печатать AST элементы и приводить их к строковому представлению.</p>
</details></td></tr></table>