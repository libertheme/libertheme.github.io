---
layout: blog
istop: true
title: "Introduction Liquid"
background-image: image/liquid-markdown.jpg
date:  2018-10-05 00:00:00 +0700
category: Liquid
tags:
- Liquid
---

Liquid có thể được phân loại thành **Object**, **Tags**, và **Filter**

# Objects
- Các đối tượng cho biết nơi hiển thị nội dung trên một trang. Đối tượng và tên được biểu thị bằng dấu ngoặc kép {{ và }}
``` Đầu vào
{{ page.title }}
```

``` Đầu ra
Introduction
```
> Trong trường hợp này, Liquid sẽ hiển thị nội dung của một đối tượng được gọi là `page.title` và đối tượng này chứa văn bản **Introduction**

# Tags
 - Tags tạo logic và luồng điều khiển cho các mẫu. Chúng được biểu thị bằng dấu ngoặc nhọn và dấu phần trăm: {% và %}
 - Đánh dấu được sử dụng trong tag không tạo ra bất kỳ văn bản hiển thị nào. Điều này có nghĩa là bạn có thể **gán biến** và ***tạo điều kiện*** và **vòng lặp** mà không hiển thị bất kỳ logic nào trên trang.
 
 ``` Đầu vào
 {% if user %}
    Hello {{ user.name}}
 {% endif %}
 ```

 ``` Đầu ra
 Hello Adam!
 ```

- **Tag có thể được phân loại thành ba loại:**
 ## 1. Control flow: cỏ thể thay đổi thông tin mà LIquid hiển thị bằng việc sử dụng programming logic:
- ### if: chỉ thực hiện một khối lệnh nếu một điều kiện nhất định là `true`

``` Input
{% if product.title == 'Awesome Shoes' %}
    These shoes are awesome!
{% endif %}
```

 ``` Output
 These shoes are awesome!
 ```

- ### unless: ngược lại với **if** - thực hiện một khối mã chỉ khi một điều kiện nhất định **không** được đáp ứng.

``` Input
{% unless product.title == 'Awesome Shoes' %}
    These shoes are not awesome.
{% endunless %}
```

``` Output
These shoes are not awesome.
```

Điều này tương đương với:
```
{% if product.title != 'Awesome Shoes' %}
    These shoes are not awesome.
{% endif %}
```

- ### elsif/else: Thêm nhiều điều kiện hơn trong một khối `if` hoặc `unless`:

``` Input
<!-- If customer.name = 'anonymous' -->
{% if costomer.name = 'kenvin' %}
    Hey Kevin!
{% elsif customer.name == 'anonymous' %}
    Hey Anonymous!
{% else %}
    Hi Stranger!
{% endif %}
```

```Output
Hey Anonymous!
```

- ### case/when: tạo một câu lệnh `switch` để so sánh một biến với các giá trị khác nhau. `case`  khởi tạo câu lệnh switch, và `when` so sánh các giá trị của nó.

```Input
{% assign handle = 'cake' %}
{% case handle %}
    {% when 'cake'%}
        This is a cake
    {% when 'cookie' %}
        This is a cookie
    {% else %}
        This is not a cake nor a cookie
    {% endcase %}
```

```Output
This is a cake
```

## 2. Iteration: Vòng lặp - chạy lặp lại các khối mã.

- ### for: lặp lại một khối mã
```Input
{% for product in collection.products %}
    {{ product.title }}
{% endfor %}
```

```Output
hat shirt pants
```
- **break** : làm cho vòng lặp dừng lại khi nó gặp thẻ **break**
```
{% for i in (1..5) %}
    {% if i == 4 %}
        {% break %}
    {% else %}
        {{ i }}
    {% endif %}
{% endfor %}
```

```Output
1 2 3
```
- **continue** : làm cho vòng lặp bỏ qua vòng lặp hiện tại khi nó gặp thẻ **continue**

```Input
{% for i in (1..5) %}
    {% if i == 4 %}
        {% continue %}
    {% else %}
        {{ i }}
    {% endif %}
{% endfor %}
```

```Output
1 2 3 5
```

- ### for (parameters):

- **limit**: giới hạn vòng lặp đến số vòng lặp chỉ định.

``` Input
<!-- if array = [1, 2, 3, 4, 5, 6] --->
{% for item in array limit:2 %}
    {{ item }}
    {% endfor %}
```

```Output
1 2
```

- **offset**: Bắt đầu vòng lặp tại chỉ mục được chỉ định

```Input
<!-- if array = [1, 2, 3, 4, 5, 6] -->
{% for item in array offset:2 %}
    {{ item }}
{% endfor %}
```

```
3 4 5 6
```

- **range**: Xác định một loạt các số để lặp qua. Phạm vi có thể được xác định bởi hai chữ số và số biến.

```Input
{% for i in (3..5) %}
    {{ i }}
{% endfor %}

{% assign num = 4 %}
{% for i in (1.. num) %}
    {{ i }}
{% endfor %}
```

```
3 4 5
1 2 3 4
```
 - **reversed**: Đảo ngược thứ tự của vòng lặp. Nó khác với `reverse`.
 
```Input
 <!-- if  array = [1, 2, 3, 4, 5, 6] -->
{% for item in array reversed %}
    {{ item }}
{% endfor %}
```

```Output
 6 5 4 3 2 1
```

- ### cycle (parameters): 
>`cycle` chấp nhận một tham số được gọi là nhóm `cycle` trong trường hợp cần nhiều khối `cycle` trong một mẫu. Nếu không có tên nào được cung cấp cho cycle group, thì giả sử rằng multiple calls có cùng tham số là một nhóm.

- ### tablerow: Tạo một bảng HTML được bao trong thẻ <table> và </table>

```Input
<table>
{% tablerow product in collection.products %}
    {{ product.title }}
{% endtablerow %}
</table>
```

```
<table>
  <tr class="row1">
    <td class="col1">
      Cool Shirt
    </td>
    <td class="col2">
      Alien Poster
    </td>
    <td class="col3">
      Batman Poster
    </td>
    <td class="col4">
      Bullseye Shirt
    </td>
    <td class="col5">
      Another Classic Vinyl
    </td>
    <td class="col6">
      Awesome Jeans
    </td>
  </tr>
</table>
```

- ### tablerow (parameters)

- **cols** : Xác định có bao nhiêu cột mà bảng nên có.
```Input
{% tablerow product in collection.products cols:2 %}
  {{ product.title }}
{% endtablerow %}
```

```Output
<table>
  <tr class="row1">
    <td class="col1">
      Cool Shirt
    </td>
    <td class="col2">
      Alien Poster
    </td>
  </tr>
  <tr class="row2">
    <td class="col1">
      Batman Poster
    </td>
    <td class="col2">
      Bullseye Shirt
    </td>
  </tr>
  <tr class="row3">
    <td class="col1">
      Another Classic Vinyl
    </td>
    <td class="col2">
      Awesome Jeans
    </td>
  </tr>
</table>
```

- **limit**: thoát khỏi tablerow sau một chỉ mục cụ thể.
```Input
{% tablerow product in collection.products cols:2 limit:3 %}
  {{ product.title }}
{% endtablerow %}
```

- **offset** : bắt đầu một tablerow sau một chỉ mục cụ thể
```Input
{% tablerow product in collection.products cols:2 offset:3 %}
  {{ product.title }}
{% endtablerow %}
```

- **range**: Xác định một loạt các số để lặp qua

```
<!--variable number example-->

{% assign num = 4 %}
<table>
{% tablerow i in (1..num) %}
  {{ i }}
{% endtablerow %}
</table>

<!--literal number example-->

<table>
{% tablerow i in (3..5) %}
  {{ i }}
{% endtablerow %}
</table>
```

- ## 3. Variable: Variable tags create new Liquid variables

- ### assign: Tạo một biến mới

```Input
{% assign my_variable = false %}
{% if my_variable != true %}
  This statement is valid.
{% endif %}
```

```Output
    This stament is valid.
```

> Bọc một chuỗi giá trị trong dấu ngoặc kép `""` để lưu dưới dạng string.

```Input
{% assign foo = "bar" %}
{{ foo }}
```

```Output
bar
```

- ### capture: 'Capture- chụp chuỗi có trong dấu ngoặc "" và gán nó cho một biến. Biến được tạo thông qua `{% capture %}` là strings.

```Input

{% capture my_variable %}I am being captured.{% endcapture %}
{{ my_variable }}
```

```Output
I am being captured.
```

> Sử dụng `capture`, bạn có thể tạo chuỗi phức tạp bằng cách sử dụng các biến khác sử dụng `assign`.

```Input
{% assign favorite_food = 'pizza' %}
{% assign age = 35 %}

{% capture about_me %}
I am {{ age }} and my favorite food is {{ favorite_food }}.
{% endcapture %}

{{ about_me }}
```

```
I am 35 and my favourite food is pizza.
```

- ## increment: Tạo một biến số mới và tăng giá trị của nó lên một lần mỗi lần nó được gọi. Giá trị ban đầu là 0.

```Input
{% increment my_counter %}
{% increment my_counter %}
{% increment my_counter %}
```

```
0
1
2
```
 >Các biến được tạo thông qua thẻ `increment` độc lập với các biến được tạo thông qua `assign` hoặc `capture`.
 .
- Trong ví dụ bên dưới, một biến có tên “var” được tạo thông qua `assign`. Thẻ `increment` sau đó được sử dụng nhiều lần trên một biến có cùng tên.
- Lưu ý rằng thẻ `increment` không ảnh hưởng đến giá trị của "var" đã được tạo thông qua `assign`.
```Input
{% assign var = 10 %}
{% increment var %}
{% increment var %}
{% increment var %}
{{ var }}
```

```Output
0
1
2
10
```

- ### decrement: Tạo một biến số mới, và giảm giá trị của nó một lần mỗi khi nó được gọi. Giá trị ban đầu là -1.

```Input
{% decrement variable %}
{% decrement variable %}
{% decrement variable %}
```

```Output
-1
-2
-3
```

> Giống như `increment`, các biến được khai báo bên trong `decrement` là độc lập với các biến được tạo thông qua `assign` hoặc `capture`.

# Filter
- **Bộ lọc** thay đổi đầu ra của một đối tượng Liquid. Chúng được sử dụng trong một đầu ra và được phân cách bởi a |.
```Input
{{ "/my/fancy/url" | append: ".html" }}
```

```Out
/my/fancy/url.html
```

- Nhiều bộ lọc có thể được sử dụng trên một đầu ra. Chúng được áp dụng từ trái sang phải.

```Input
{{ "adam!" | capitalize | prepend: "Hello " }}
```

```Output
Hello Adam!
