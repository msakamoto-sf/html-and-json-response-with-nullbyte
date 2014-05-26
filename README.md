html-and-json-response-with-nullbyte
====================================

HTML and JSON contents including 0x00 byte.

### include0x00.html

This plain HTML contents include 0x00 in title tag and body contents.
```html
...
<title>include(0x20)(0x00)0x00</title>
...
<body>
  hello,(0x20)(0x00)world.
</body>
...
```

 * chrome 35 : title and body contents both ignore 0x00.
 * Firefox 29 : 0x00 in title was broken. 0x00 in body contents was ignored.
 * IE 11 : (same to Firefox 29)
 * Opera 21 : (same to Chrome 35)

### include0x00_2.html

JavaScript string value include 0x00 in HTML.
```html
<body>
  <p id="placeholder"></p>
  <script>
    var data = '\u0041\u0042\u0000\u0043\u0044';
    var e = document.getElementById('placeholder');
    e.appendChild(document.createTextNode(data));
  </script>
</body>
```

 * chrome 35, Firefox 29, Opera 21 : ignores 0x00 in javascript string value.
 * IE 11 : __String after 0x00 (="CD") were ignored.__

### include0x00_3.json, include0x00_3a.html, include_0x00_3b.html

 * include0x00_3a.html load include0x00_3.json containing 0x00 in "data2" value by XMLHttpRequest, and parse by JSON.parse().
 * include0x00_3b.html load same json file by jQuery's getJSON().

```
{"data1": "ABCDEF", "data2": "ab(0x00)cdef", "data3": 123}
```

 * Result : include0x00_3a.html
   * chrome 35, Firefox 29, Operar 21, IE 11 : __0x00 triggers SyntaxError while JSON.parse().__
 * Result : include0x00_3b.html
   * chrome 35, Firefox 29, Operar 21, IE 11 : something wrong, no console error message.

### include0x00_4.json, include0x00_4.html

include0x00_4.html load include0x00_4.json containing 0x00 (JSON Unicode escaped) in "data2" value by jQuery's getJSON().

```
{"data1": "ABCDEF", "data2": "ab\u0000cdef", "data3": 123}
```

 * chrome 35, Firefox 29, Opera 21 : ignores 0x00 in "data2" value.
 * IE 11 : __String after 0x00 (="cdef") in "data2" were ignored.__

## CONCLUSION

 * Remove 0x00 (both raw and unicode/hex escaped) from HTML/JavaScript/JSON to avoid unintended contents rendering problems especially  for IE 11 environment.


