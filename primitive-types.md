% Простые типы

The Rust language has a number of types that are considered ‘primitive’. This
means that they’re built-in to the language. Rust is structured in such a way
that the standard library also provides a number of useful types built on top
of these ones, as well, but these are the most primitive.

# Booleans

Rust has a built in boolean type, named `bool`. It has two values, `true` and `false`:

```rust
let x = true;

let y: bool = false;
```

A common use of booleans is in [`if` statements][if].

[if]: if.html

You can find more documentation for `bool`s [in the standard library
documentation][bool].

[bool]: ../std/primitive.bool.html

# `char`

The `char` type represents a single Unicode scalar value. You can create `char`s
with a single tick: (`'`)

```rust
let x = 'x';
let two_hearts = '💕';
```

Unlike some other languages, this means that Rust’s `char` is not a single byte,
but four.

You can find more documentation for `char`s [in the standard library
documentation][char].

[char]: ../std/primitive.char.html

# Numeric types

Rust has a variety of numeric types in a few categories: signed and unsigned,
fixed and variable, floating-point and integer.

These types consist of two parts: the category, and the size. For example,
`u16` is an unsigned type with sixteen bits of size. More bits lets you have
bigger numbers.

If a number literal has nothing to cause its type to be inferred, it defaults:

```rust
let x = 42; // x has type i32

let y = 1.0; // y has type f64
```

Here’s a list of the different numeric types, with links to their documentation
in the standard library:

* [i16](../std/primitive.i16.html)
* [i32](../std/primitive.i32.html)
* [i64](../std/primitive.i64.html)
* [i8](../std/primitive.i8.html)
* [u16](../std/primitive.u16.html)
* [u32](../std/primitive.u32.html)
* [u64](../std/primitive.u64.html)
* [u8](../std/primitive.u8.html)
* [isize](../std/primitive.isize.html)
* [usize](../std/primitive.usize.html)
* [f32](../std/primitive.f32.html)
* [f64](../std/primitive.f64.html)

Let’s go over them by category:

## Signed and Unsigned

Integer types come in two varieties: signed and unsigned. To understand the
difference, let’s consider a number with four bits of size. A signed, four-bit
number would let you store numbers from `-8` to `+7`. Signed numbers use
‘two’s compliment representation’. An unsigned four bit number, since it does
not need to store negatives, can store values from `0` to `+15`.

Unsigned types use a `u` for their category, and signed types use `i`. The `i`
is for ‘integer’. So `u8` is an eight-bit unsigned number, and `i8` is an
eight-bit signed number. 

## Fixed size types

Fixed size types have a specific number of bits in their representation. Valid
bit sizes are `8`, `16`, `32`, and `64`. So, `u32` is an unsigned, 32-bit integer,
and `i64` is a signed, 64-bit integer.

## Variable sized types

Rust also provides types whose size depends on the size of a pointer of the
underlying machine. These types have ‘size’ as the category, and come in signed
and unsigned varieties. This makes for two types: `isize` and `usize`.

## Floating-point types

Rust also two floating point types: `f32` and `f64`. These correspond to 
IEEE-754 single and double precision numbers.

# Массивы

В Rust, как и во многих других языках программирования, есть типы-списки для
представления последовательностей неких вещей. Самый простой из них - это
*массив*, то есть список элементов одного и того же типа, имеющий фиксированный
размер. Массивы неизменяемы по умолчанию.

```rust
let a = [1, 2, 3]; // a: [i32; 3]
let mut m = [1, 2, 3]; // m: [i32; 3]
```

Массивы имеют тип `[T; N]`. О значении `T` мы поговорим позже, когда будем
рассматривать дженерики. `N` - это константа времени компиляции, задающая длину
массива.

Для инициализации всех элементов массива одним и тем же значением есть
специальный синтаксис. В этом примере каждый элемент `a` будет инициализирован
значением `0`:

```rust
let a = [0; 20]; // a: [i32; 20]
```

Вы можете получить число элементов массива `a` с помощью метода `a.len()`:

```rust
let a = [1, 2, 3];

println!("Число элементов в a: {}", a.len());
```

Вы можете получить определённый элемент массива с помощью *индекса*:

```rust
let names = ["Graydon", "Brian", "Niko"]; // names: [&str; 3]

println!("Второе имя: {}", names[1]);
```

Индексы нумеруются с нуля, как и в большинстве языков программирования, поэтому
мы получаем первое имя с помощью `names[0]`, а второе - с помощью `names[1]`.
Пример выше печатает `Второе имя: Brian`. Если вы попытаетесь использовать
индекс, который не входит в массив, вы получите ошибку: при доступе к массивам
происходит проверка границ во время исполнения программы. Такая ошибочная
попытка доступа - источник многих проблем в других языках системного
программирования.

Вы можете найти больше информации о массивах (`array`) в [документации к
стандартной библиотеке][array].

[array]: ../std/primitive.array.html

# Срезы

*Срез* - это ссылка на (или "проекция" в) массив. Они полезны, когда нужно
обеспечить безопасный, эффективный доступ к части массива без копирования.
Например, возможно вам нужно сослаться на единственную строку файла, считанного
в память. Из-за своей ссылочной природы, срезы создаются не напрямую, а из
существующих переменных. У срезов есть длина, они могут быть изменяемы или нет,
и во многих случаях они ведут себя как массивы:

```rust
let a = [0, 1, 2, 3, 4];
let middle = &a[1..4]; // Срез a: только элементы 1, 2, и 3
```

Срезы имеют тип `&[T]`. О значении `T` мы поговорим позже, когда будем
рассматривать [дженерики][generics].

[generics]: generics.html

Вы можете найти больше информации о срезах (`slice`) в [документации к
стандартной библиотеке][slice].

[slice]: ../std/primitive.slice.html

# `str`

Тип `str` в Rust являются наиболее простым типом строк. Это [безразмерный
тип][dst], поэтому сам по себе он не очень полезен, но он становится полезным
при использовании ссылки, [`&str`][strings]. Таким образом, мы просто
остановимся на этом.

[dst]: unsized-types.html
[strings]: strings.html

Вы можете найти больше информации о строках (`str`) в [документации к
стандартной библиотеке][str].

[str]: ../std/primitive.str.html

# Кортежи

Кортеж - это упорядоченный список фиксированного размера. Вроде такого:

```rust
let x = (1, "привет");
```

Этот кортеж из двух элементов создан с помощью скобок и запятой между
элементами. Вот тот же код, но с аннотациями типов:

```rust
let x: (i32, &str) = (1, "привет");
```

Как вы можете видеть, тип кортежа выглядит как сам кортеж, но места элементов
занимают типы. Внимательные читатели также отметят, что кортежи гетерогенны: в
этом кортеже одновременно хранятся значения типов `i32` и `&str`. В языках
системного программирования строки немного более сложны, чем в других языках.
Пока вы можете читать `&str` как *срез строки*. Мы вскоре узнаем об этом больше.

Доступ к полям кортежа можно получить с помощью *деконструирующего let*. Вот
пример:

```rust
let (x, y, z) = (1, 2, 3);

println!("x это {}", x);
```

Помните, я [говорил][let], что левая часть оператора `let` более полезна, чем
просто присваивание имени? Об этом я и говорил. Мы можем написать слева от `let`
образец, и, если он совпадает со значением справа, произойдёт присваивание имён
сразу нескольким значениям. В данном случае, `let` "деконструирует" или
"разбивает" кортеж, и присваивает его части трём именам.

[let]: variable-bindings.html

Это очень удобный шаблон программирования, и мы ещё не раз увидим его.

Некоторые вещи можно делать с кортежами как с единым целым, без разбиения. Можно
присваивать один кортеж другому, если они содержат значения одинаковых типов и
имеют одинаковую [арность][arity]. Арность кортежей одинакова, когда их длина
совпадает.

[arity]: ./glossary.html#arity

```rust
let mut x = (1, 2); // x: (i32, i32)
let y = (2, 3); // y: (i32, i32)

x = y;
```

Вы можете найти больше информации о кортежах (`tuple`) в [документации к
стандартной библиотеке][tuple].

[tuple]: ../std/primitive.tuple.html

# Функции

Функции тоже имеют тип! Это выглядит следующим образом:

```
fn foo(x: i32) -> i32 { x }

let x: fn(i32) -> i32 = foo;
```

В данном примере `x` - ‘это указатель на функцию‘, которая принимает в качестве
аргумента `i32` и возвращает `i32`.
