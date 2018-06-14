# codeshare1: intro

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
which you are currently reading. There is also audio available
for most of these overviews. The advantage of audio recording
is an ability to concentrate your eyes on code while listening to explanation.</p>

<p>All code snippets contain comments that should make it possible
to interpret the message properly even without reading the overview notes.
This makes it possible to use these pages as a source of code recipes.</p>

</details></td><td><details><summary>Ru</summary>
<p>Все документы из этой серии структурированы следующим образом:
последовательность сниппетов кода, к каждому из которых прилагается
описание, которое вы в данный момент читаете.
К большинству таких описаний доступна аудио версия.
Преимущество аудиозаписи в том, что вы можете продолжать концентрировать
ваш взгляд на коде, воспринимая пояснения к нему через слух.</p>

<p>В коде содержатся комментарии, которые позволяют разобраться в нём
без использования пояснительных записей, что позволяет использовать
данные страницы как источник рецептов.</p>
</details></td></tr></table>

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
    // Two ways to create binary expression:
    x := strparse.Expr(`1+2`)
    y := &ast.BinaryExpr{
        Op: token.ADD,
        X:  &ast.BasicLit{Kind: token.INT, Value: "1"},
        Y:  &ast.BasicLit{Kind: token.INT, Value: "2"},
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

</details></td><td><details><summary>Ru</summary>
<p>В этом примере используются два пакета из <a href="https://github.com/go-toolsmith">go-toolsmith</a>: <a href="https://github.com/go-toolsmith/strparse">strparse</a>
и <a href="https://github.com/go-toolsmith/astequal">astequal</a>.</p>

<p>Пакет strparse упрощает создание простых AST элементов и является простой обёрткой
вызовов parser.ParseExpr и parser.ParseFile из пакета go/parser. Особенно полезен для примеров и тестов.</p>

<p>Пакет astequal позволяет сравнивать два AST элемента на равенство.
В отличие от reflect.DeepEqual, он не сравнивает позиции, отсюда разный
результат сравнения, наблюдаемый в коде выше.</p>
</details></td></tr></table>
