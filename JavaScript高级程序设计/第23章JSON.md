# 第23章 JSON
## 23.1 语法
JSON语法支持三种类型的值。
- 简单值：字符串、数值、布尔值和null可以在JSON中使用，特殊值undefined不可以。
- 对象：第一种复杂数据类型，对象表示有序键值对。每个值可以是简单值，也可以是复杂类型。
- 数组：第二种复杂数据类型，数据表示可以通过数值索引访问的值的有序列表。数组的值可以是任意类型，包括简单值、对象，甚至其他数组。  

JSON没有变量、函数或对象实例的概念。JSON的所有记号都只为表示结构化数据。  
:warning:JSON字符串必须使用双引号（单引号会导致语法错误），JSON中的对象属性名必须始终带双引号。手动编写JSON时漏掉这些双引号或者使用单引号是常见错误。布尔值和null本身也是有效的JSON值。  
## 23.2 解析与序列化
JSON对象有两个方法：stringify()和parse()。stringify()将JavaScript序列化为JSON字符串，parse()将JSON解析为JavaScript值。  
在序列化JavaScript对象时，所有函数和原型成员都会被忽略。值为undefined的任何属性都会被跳过。最终得到的就是所有实例属性均为有效JSON数据类型的表示。  
### 序列化选项
JSON.stringify()方法除了要传入序列化的对象外，还可以接收两个参数。这两个参数可以用于指定其他序列化JavaScript对象的方式。第一个参数是过滤器，可以是数组或函数；第二个参数是用于缩进结果JSON字符串的选项。单独或组合使用这些参数可以更好地控制JSON序列化。  
#### 过滤结果
如果第二个参数是一个数组，那么JSON.stringify()返回的结果只会包含该数组中列出的对象属性。
```
let book = {
    title: "Professional JavaScript",
    authors: [
        'Nicholas C. Zakas',
        "Matt Frisbie"
    ],
    edition: 4,
    year: 2017,
};

let jsonText1 = JSON.stringify(book, ['title', 'edition']);
console.log(jsonText1);
//{"title":"Professional JavaScript","edition":4}
```
如果第二个参数是一个函数，提供的函数接收两个参数：属性名key和属性值value。可以根据这个key决定要对相应属性执行什么操作。这个key始终是字符串，只是在值不属于某个键值对时是空字符串。  
为了改变对象的序列化，返回的指就是相应key应该包含的结果。注意，:warning:返回undefined会导致属性被忽略。  
```
let book = {
    title: "Professional JavaScript",
    authors: [
        'Nicholas C. Zakas',
        "Matt Frisbie"
    ],
    edition: 4,
    year: 2017,
};

let jsonText2 = JSON.stringify(book, (key, value) => {
    switch (key) {
        case 'authors':
            return value.join(',');
        case 'year':
            return 5000;
        case 'edition':
            return undefined;
        default:
            return value;
    }
});
console.log(jsonText2);
//{"title":"Professional JavaScript","authors":"Nicholas C. Zakas,Matt Frisbie","year":5000}
```
注意，函数过滤器会应用到要序列化的对象所包含的所有对象，因此如果数组中包含多个具有这些属性的对象，则序列化之后每个对象都只会剩下上面这些属性。  
#### 字符串缩进
JSON.stringify()方法的第三个参数控制缩进和空格。在这个参数是数值时，表示每一级缩进的空格数。
```
let book = {
    title: "Professional JavaScript",
    authors: [
        'Nicholas C. Zakas',
        "Matt Frisbie"
    ],
    edition: 4,
    year: 2017,
};
let jsonText3 = JSON.stringify(book, null, 4);
console.log(jsonText3);
/*
{
    "title": "Professional JavaScript",
    "authors": [
        "Nicholas C. Zakas",
        "Matt Frisbie"
    ],
    "edition": 4,
    "year": 2017
}
*/
```
注意，除了缩进，JSON.stringify()方法还为方便阅读
