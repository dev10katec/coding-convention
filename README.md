## Mục lục

1. [Types](#types)
1. [References](#references)
1. [Objects](#objects)
1. [Arrays](#arrays)
1. [Destructuring](#destructuring)
1. [Strings](#strings)
1. [Functions](#functions)
1. [Arrow Functions](#arrow-functions)
1. [Classes & Constructors](#classes--constructors)
1. [Modules](#modules)
1. [Iterators and Generators](#iterators-and-generators)
1. [Properties](#properties)
1. [Variables](#variables)
1. [Hoisting](#hoisting)
1. [Comparison Operators & Equality](#comparison-operators--equality)
1. [Blocks](#blocks)
1. [Control Statements](#control-statements)
1. [Comments](#comments)
1. [Whitespace](#whitespace)
1. [Commas](#commas)
1. [Semicolons](#semicolons)
1. [Type Casting & Coercion](#type-casting--coercion)
1. [Naming Conventions](#naming-conventions)
1. [Accessors](#accessors)
1. [Events](#events)

## <a name="types">Types</a>

<a name="types--primitives"></a><a name="1.1"></a>
- [1.1](#types--primitives) **Kiểu nguyên thủy**: Khi bạn truy cập một giá trị kiểu nguyên thủy, bạn làm việc trực tiếp trên giá trị của nó.

    - `string`
    - `number`
    - `boolean`
    - `null`
    - `undefined`
    - `symbol`
    - `bigint`

  ``` javascript
  const foo = 1;
  let bar = foo;

  bar = 9;

  console.log(foo, bar); // => 1, 9
  ```

    - Sự thiếu hỗ trợ cho các `Symbol` và `BigInt` không thể được lấp đầy bởi các bộ trợ năng một cách toàn diện, do đó, chúng không nên được sử dụng khi hướng đến các trình duyệt/môi trường không có hỗ trợ sẵn.

<a name="types--complex"></a><a name="1.2"></a>
- [1.2](#types--complex) **Kiểu phức tạp**: Khi bạn truy cập một giá trị kiểu phức tạp, bạn làm việc trên tham chiếu giá trị của nó.

    - `object`
    - `array`
    - `function`

  ``` javascript
  const foo = [1, 2];
  const bar = foo;

  bar[0] = 9;

  console.log(foo[0], bar[0]); // => 9, 9
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="references">References</a>

<a name="references--prefer-const"></a><a name="2.1"></a>
- [2.1](#references--prefer-const) Sử dụng `const` đối với tất cả các tham chiếu; tránh sử dụng `var`. eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign.html)

  > Tại sao? Điều này đảm bảo rằng bạn không thể gán lại các tham chiếu, việc có thể gây ra các lỗi và gây khó khăn cho sự đọc hiểu mã nguồn.

  ``` javascript
  // bad
  var a = 1;
  var b = 2;

  // good
  const a = 1;
  const b = 2;
  ```

<a name="references--disallow-var"></a><a name="2.2"></a>
- [2.2](#references--disallow-var) Nếu bạn bắt buộc phải gán lại các tham chiếu, sử dụng `let`, thay vì `var`. eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html)

  > Tại sao? `let` thuộc phạm vi khối mà nó được khởi tạo, thay vì thuộc phạm vi hàm như `var`.

  ``` javascript
  // bad
  var count = 1;
  if (true) {
    count += 1;
  }

   // good, use the let.
  let count = 1;
  if (true) {
    count += 1;
  }
  ```

<a name="references--block-scope"></a><a name="2.3"></a>
- [2.3](#references--block-scope) Lưu ý rằng cả `let` và `const` đều thuộc phạm vi khối, còn `var` thuộc phạm vi hàm.

  ``` javascript
  // const and let only exist in the blocks they are defined in.
  {
    let a = 1;
    const b = 1;
    var c = 1;
  }
  console.log(a); // ReferenceError
  console.log(b); // ReferenceError
  console.log(c); //  Prints 1
  ```

  Trong đoạn mã trên, bạn có thể thấy rằng lỗi ReferenceError xảy ra khi truy cập `a` và `b`, trong khi `c` vẫn là số đã gán. Nguyên nhân là vì `a` và `b` thuộc phạm vi khối, còn `c` thuộc phạm vi hàm chứa đoạn mã trên.

**[⬆ về đầu trang](#table-of-contents)**

## <a name="objects">Objects</a>

<a name="objects--no-new"></a><a name="3.1"></a>
- [3.1](#objects--no-new) Sử dụng cú pháp nguyên văn `{}` để khởi tạo đối tượng. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)

  ``` javascript
  // bad
  const item = new Object();

  // good
  const item = {};
  ```

<a name="es6-computed-properties"></a><a name="3.4"></a>
- [3.2](#es6-computed-properties) Sử dụng các tên được tính của thuộc tính `[key()]` khi tạo các đối tượng có các tên của thuộc tính là động.

  > Tại sao? Chúng cho phép bạn định nghĩa tất cả các thuộc tính của một đối tượng cùng một chỗ.

  ```javascript
   function getKey(k) {
    return `a key named ${k}`;
   }
   // bad
   const obj = {
     id: 5,
     name: 'San Francisco',
   };
   obj[getKey('enabled')] = true;
   // good
   const obj = {
     id: 5,
     name: 'San Francisco',
     [getKey('enabled')]: true,
   };
   ```

<a name="es6-object-shorthand"></a><a name="3.5"></a>
- [3.3](#es6-object-shorthand) Sử dụng cú pháp định nghĩa phương thức rút gọn để định nghĩa các phương thức của đối tượng. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

  ``` javascript
  // bad
  const atom = {
    value: 1,

    addValue: function (value) {
      return atom.value + value;
    },
  };

  // good
  const atom = {
    value: 1,

    addValue(value) {
      return atom.value + value;
    },
  };
  ```

<a name="es6-object-concise"></a><a name="3.6"></a>
- [3.4](#es6-object-concise) Sử dụng cú pháp định nghĩa thuộc tính rút gọn để định nghĩa các thuộc tính của đối tượng. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

  > Tại sao? Nó ngắn gọn và súc tích.

  ``` javascript
  const lukeSkywalker = 'Luke Skywalker';

  // bad
  const obj = {
    lukeSkywalker: lukeSkywalker,
  };

  // good
  const obj = {
    lukeSkywalker,
  };
  ```

<a name="objects--grouped-shorthand"></a><a name="3.7"></a>
- [3.5](#objects--grouped-shorthand) Gom tất cả các thuộc tính rút gọn ở trên cùng khi khai báo đối tượng.

  > Tại sao? Điều này giúp bạn dễ dàng biết được thuộc tính nào sử dụng cú pháp rút gọn.

  ``` javascript
  const anakinSkywalker = 'Anakin Skywalker';
  const lukeSkywalker = 'Luke Skywalker';

  // bad
  const obj = {
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    lukeSkywalker,
    episodeThree: 3,
    mayTheFourth: 4,
    anakinSkywalker,
  };

  // good
  const obj = {
    lukeSkywalker,
    anakinSkywalker,
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    episodeThree: 3,
    mayTheFourth: 4,
  };
  ```

<a name="objects--quoted-props"></a><a name="3.8"></a>
- [3.6](#objects--quoted-props) Chỉ sử dụng dấu lược `' '` cho các thuộc tính có định danh không hợp lệ. eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props.html)

  > Tại sao? Nhìn chung, chúng ta sẽ thấy nó dễ đọc hơn nhiều. Nó cải thiện nhấn mạnh cú pháp, và nó cũng giúp việc tối ưu hóa bằng các trình thực thi JS hiệu quả hơn.

  ``` javascript
  // bad
  const bad = {
    'foo': 3,
    'bar': 4,
    'data-blah': 5,
  };

  // good
  const good = {
    foo: 3,
    bar: 4,
    'data-blah': 5,
  };
  ```

<a name="objects--prototype-builtins"></a>
- [3.7](#objects--prototype-builtins) Không gọi các phương thức `Object.prototype` một cách trực tiếp, ví dụ như `hasOwnProperty`, `propertyIsEnumerable`, và `isPrototypeOf`. eslint: [`no-prototype-builtins`](https://eslint.org/docs/rules/no-prototype-builtins)

  > Tại sao? Những phương thức này có thể bị thay thế bởi các thuộc tính của một đối tượng - như `{ hasOwnProperty: false }` - hoặc, đối tượng có thể là một đối tượng rỗng (`Object.create(null)`).

  ``` javascript
  // bad
  console.log(object.hasOwnProperty(key));

  // good
  console.log(Object.prototype.hasOwnProperty.call(object, key));

  // best
  const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
  console.log(has.call(object, key));
  /* or */
  import has from 'has'; // https://www.npmjs.com/package/has
  console.log(has.call(object, key));
  ```

<a name="objects--rest-spread"></a>
- [3.8](#objects--rest-spread) Ưu tiên sử dụng cú pháp liệt kê `...` so với [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) để tạo bản sao nhanh của một đối tượng. Sử dụng toán tử còn-lại `...` để tạo một đối tượng mới với một số thuộc tính đã bị loại bỏ. eslint: [`prefer-object-spread`](https://eslint.org/docs/rules/prefer-object-spread)

  ``` javascript
  // very bad
  const original = { a: 1, b: 2 };
  const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
  delete copy.a; // so does this

  // bad
  const original = { a: 1, b: 2 };
  const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

  // good
  const original = { a: 1, b: 2 };
  const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

  const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="arrays">Arrays</a>

<a name="arrays--literals"></a><a name="4.1"></a>
- [4.1](#arrays--literals) Sử dụng cú pháp nguyên văn `[]` để khởi tạo mảng. eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html)

  ``` javascript
  // bad
  const items = new Array();

  // good
  const items = [];
  ```

<a name="arrays--push"></a><a name="4.2"></a>
- [4.2](#arrays--push) Sử dụng [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push), thay vì phép gán, để thêm các mục cho một mảng.

  ``` javascript
  const someStack = [];

  // bad
  someStack[someStack.length] = 'abracadabra';

  // good
  someStack.push('abracadabra');
  ```

<a name="es6-array-spreads"></a><a name="4.3"></a>
- [4.3](#es6-array-spreads) Sử dụng toán tử liệt kê `...` để sao nhanh một mảng.

  ``` javascript
  // bad
  const len = items.length;
  const itemsCopy = [];
  let i;

  for (i = 0; i < len; i += 1) {
    itemsCopy[i] = items[i];
  }

  // good
  const itemsCopy = [...items];
  ```

<a name="arrays--from"></a>
<a name="arrays--from-iterable"></a><a name="4.4"></a>
- [4.4](#arrays--from-iterable) Để chuyển đổi một đối tượng khả duyệt thành một mảng, sử dụng toán tử liệt kê `...` thay vì [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

  ``` javascript
  const foo = document.querySelectorAll('.foo');

  // good
  const nodes = Array.from(foo);

  // best
  const nodes = [...foo];
  ```

<a name="arrays--from-array-like"></a>
- [4.5](#arrays--from-array-like) Sử dụng [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) để chuyển đổi một đối tượng giống-mảng thành một mảng.

  ``` javascript
  const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

  // bad
  const arr = Array.prototype.slice.call(arrLike);

  // good
  const arr = Array.from(arrLike);
  ```

<a name="arrays--mapping"></a>
- [4.6](#arrays--mapping) Sử dụng [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from), thay vì toán tử liệt kê `...`, để ánh xạ một đối tượng khả duyệt, vì nó không tạo ra một mảng trung gian.

  ``` javascript
  // bad
  const baz = [...foo].map(bar);

  // good
  const baz = Array.from(foo, bar);
  ```

<a name="arrays--callback-return"></a><a name="4.5"></a>
- [4.7](#arrays--callback-return) Sử dụng các lệnh `return` cho các hàm gọi lại dùng cho các phương thức của mảng. Được phép bỏ qua `return` nếu phần thân hàm chỉ gồm một câu lệnh trả về một biểu thức không có hiệu ứng phụ, theo quy tắc [8.2](#arrows--implicit-return). eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

  ``` javascript
  // good
  [1, 2, 3].map((x) => {
    const y = x + 1;
    return x * y;
  });

  // good
  [1, 2, 3].map((x) => x + 1);

  // bad - no returned value means `acc` becomes undefined after the first iteration
  [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
    const flatten = acc.concat(item);
  });

  // good
  [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
    const flatten = acc.concat(item);
    return flatten;
  });

  // bad
  inbox.filter((msg) => {
    const { subject, author } = msg;
    if (subject === 'Mockingbird') {
      return author === 'Harper Lee';
    } else {
      return false;
    }
  });

  // good
  inbox.filter((msg) => {
    const { subject, author } = msg;
    if (subject === 'Mockingbird') {
      return author === 'Harper Lee';
    }

    return false;
  });
  ```

<a name="arrays--bracket-newline"></a>
- [4.8](#arrays--bracket-newline) Sử dụng dấu ngắt dòng trước và sau các dấu đóng và mở ngoặc vuông nếu một mảng nằm trên nhiều dòng.

  ``` javascript
  // bad
  const arr = [
    [0, 1], [2, 3], [4, 5],
  ];

  const objectInArray = [{
    id: 1,
  }, {
    id: 2,
  }];

  const numberInArray = [
    1, 2,
  ];

  // good
  const arr = [[0, 1], [2, 3], [4, 5]];

  const objectInArray = [
    {
      id: 1,
    },
    {
      id: 2,
    },
  ];

  const numberInArray = [
    1,
    2,
  ];
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="destructuring">Destructuring</a>

<a name="destructuring--object"></a><a name="5.1"></a>
- [5.1](#destructuring--object) Sử dụng trích xuất đối tượng khi truy cập và sử dụng nhiều thuộc tính của một đối tượng. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

  > Tại sao? Trích xuất giúp việc tạo các tham chiếu đến các thuộc tính trở nên dễ dàng hơn và hạn chế việc truy cập một đối tượng lặp đi lặp lại. Việc truy cập đối tượng lặp đi lặp lại tạo ra nhiều đoạn mã trùng lặp hơn, cần phải đọc nhiều hơn, và tăng khả năng xảy ra nhầm lẫn. Trích xuất đối tượng cũng tạo nên một ý tưởng về cấu trúc của đối tượng được sử dụng trong khối, thay vì cần phải đọc toàn bộ khối để xác định những thuộc tính được sử dụng.

  ``` javascript
  // bad
  function getFullName(user) {
    const firstName = user.firstName;
    const lastName = user.lastName;

    return `${firstName} ${lastName}`;
  }

  // good
  function getFullName(user) {
    const { firstName, lastName } = user;
    return `${firstName} ${lastName}`;
  }

  // best
  function getFullName({ firstName, lastName }) {
    return `${firstName} ${lastName}`;
  }
  ```

<a name="destructuring--array"></a><a name="5.2"></a>
- [5.2](#destructuring--array) Hãy sử dụng trích xuất mảng. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

  ``` javascript
  const arr = [1, 2, 3, 4];

  // bad
  const first = arr[0];
  const second = arr[1];

  // good
  const [first, second] = arr;
  ```

<a name="destructuring--object-over-array"></a><a name="5.3"></a>
- [5.3](#destructuring--object-over-array) Sử dụng trích xuất đối tượng khi có nhiều giá trị trả về, thay vì trích xuất mảng.

  > Tại sao? Bạn có thể thêm các thuộc tính mới qua thời gian hay thay đổi thứ tự các thứ mà không lo làm hỏng các phép gọi trước đó.

  ``` javascript
  // bad
  function processInput(input) {
    // then a miracle occurs
    return [left, right, top, bottom];
  }

  // the caller needs to think about the order of return data
  const [left, __, top] = processInput(input);

  // good
  function processInput(input) {
    // then a miracle occurs
    return { left, right, top, bottom };
  }

  // the caller selects only the data they need
  const { left, top } = processInput(input);
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="strings">Strings</a>

<a name="strings--quotes"></a><a name="6.1"></a>
- [6.1](#strings--quotes) Sử dụng dấu lược cho các chuỗi. eslint: [`quotes`](https://eslint.org/docs/rules/quotes.html)

  ``` javascript
  // bad
  const name = "Capt. Janeway";

  // bad - template literals should contain interpolation or newlines
  const name = `Capt. Janeway`;

  // good
  const name = 'Capt. Janeway';
  ```

<a name="strings--line-length"></a><a name="6.2"></a>
- [6.2](#strings--line-length) Các chuỗi, dù khiến cho độ dài của dòng lớn hơn 100 ký tự, không nên được viết thành nhiều dòng sử dụng ghép chuỗi.

  > Tại sao? Các chuỗi bị chia nhỏ rất khó để làm việc cùng và khiến việc tìm kiếm trong mã nguồn trở nên khó hơn.

  ``` javascript
  // bad
  const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

  // bad
  const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

  // good
  const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
  ```

<a name="es6-template-literals"></a><a name="6.4"></a>
- [6.3](#es6-template-literals) Khi xây dựng các chuỗi theo một chu trình, sử dụng mẫu chuỗi thay vì ghép chuỗi. eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

  > Tại sao? Các mẫu chuỗi cho bạn một cú pháp súc tích, dễ đọc với các ngắt dòng và các tính năng ghép chuỗi phù hợp.

  ``` javascript
  // bad
  function sayHi(name) {
    return 'How are you, ' + name + '?';
  }

  // bad
  function sayHi(name) {
    return ['How are you, ', name, '?'].join();
  }

  // good
  function sayHi(name) {
    return `How are you, ${name}?`;
  }
  ```

<a name="strings--eval"></a><a name="6.5"></a>
- [6.4](#strings--eval) Không bao giờ sử dụng `eval()` cho một chuỗi, điều đó mở ra rất nhiều các lỗ hổng và rủi ro. eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)

<a name="strings--escaping"></a>
- [6.5](#strings--escaping) Không sử dụng các ký tự thoát trong một chuỗi khi không cần thiết. eslint: [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

  > Tại sao? Các dấu chéo ngược làm giảm tính khả đọc, vì thế chúng chỉ nên xuất hiện khi cần.

  ``` javascript
  // bad
  const foo = '\'this\' \i\s \"quoted\"';

  // good
  const foo = '\'this\' is "quoted"';
  const foo = `my name is '${name}'`;
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="functions">Functions</a>

<a name="functions--declarations"></a><a name="7.1"></a>
- [7.1](#functions--declarations) Sử dụng biểu thức hàm hữu danh thay vì khai báo hàm. eslint: [`func-style`](https://eslint.org/docs/rules/func-style)

  > Tại sao? Các khai báo hàm đều được kéo lên, đồng nghĩa với việc một hàm rất dễ có khả năng được sử dụng trước cả khi nó được định nghĩa trong tệp. Điều này làm giảm tính khả đọc và khả năng bảo trì. Nếu bạn thấy một hàm đủ lớn hoặc phức tạp đến mức ảnh hưởng đến việc đọc hiểu phần còn lại của tệp thì, có lẽ, nó nên được tách ra thành một mô-đun riêng! Đừng quên đặt tên cho biểu thức một cách rõ ràng, cho dù tên hàm có thể được suy ra từ tên biến chứa hàm đó (thường gặp ở các trình duyệt mới nhất hoặc các trình biên dịch như Babel). Điều này loại bỏ các nhận định liên quan đến ngăn xếp của một lỗi. ([Cuộc thảo luận](https://github.com/airbnb/javascript/issues/794))

  ``` javascript
  // bad
  function foo() {
    // ...
  }

  // bad
  const foo = function () {
    // ...
  };

  // good
  // lexical name distinguished from the variable-referenced invocation(s)
  const short = function longUniqueMoreDescriptiveLexicalFoo() {
    // ...
  };
  ```

<a name="functions--iife"></a><a name="7.2"></a>
- [7.2](#functions--iife) Đặt các biểu thức hàm gọi tức thời trong ngoặc. eslint: [`wrap-iife`](https://eslint.org/docs/rules/wrap-iife.html)

  > Tại sao? Một biểu thức hàm gọi tức thời mà một đơn vị riêng - đặt nó và dấu ngoặc dùng để gọi nó `()` trong ngoặc để biểu đạt nó một cách rõ ràng. Cũng cần biết là, trong cái thế giới mà mô-đun ngập tràn mọi nơi như bây giờ, bạn cũng chả mấy khi cần dùng đến biểu thức hàm gọi tức thời.

  ``` javascript
  // immediately-invoked function expression (IIFE)
  (function () {
    console.log('Welcome to the Internet. Please follow me.');
  }());
  ```

<a name="functions--in-blocks"></a><a name="7.3"></a>
- [7.3](#functions--in-blocks) Không bao giờ khai báo một hàm bên trong một khối không phải hàm (`if`, `while`, v.v.). Thay vào đó, hãy gán hàm cho một biến. Các trình duyệt đều sẽ cho phép bạn làm điều đó, nhưng tiếc là, cách mà chúng diễn dịch là khác nhau. eslint: [`no-loop-func`](https://eslint.org/docs/rules/no-loop-func.html)

<a name="functions--note-on-blocks"></a><a name="7.4"></a>
- [7.4](#functions--note-on-blocks) **Ghi chú:** ECMA-262 định nghĩa một `khối` là tập hợp một hoặc một vài câu lệnh. Một khai báo hàm không phải là một câu lệnh.

  ``` javascript
  // bad
  if (currentUser) {
    function test() {
      console.log('Nope!');
    }
  }

  // good
  let test;
  if (currentUser) {
    test = () => {
      console.log('Yup.');
    };
  }
  ```

<a name="functions--arguments-shadow"></a><a name="7.5"></a>
- [7.5](#functions--arguments-shadow) Không bao giờ đặt tên một tham số là `arguments`. Tham số này sẽ được ưu tiên hơn đối tượng `arguments` được cung cấp cho mỗi phạm vi hàm.

  ``` javascript
  // bad
  function foo(name, options, arguments) {
    // ...
  }

  // good
  function foo(name, options, args) {
    // ...
  }
  ```

<a name="es6-rest"></a><a name="7.6"></a>
- [7.6](#es6-rest) Không bao giờ sử dụng `arguments`, thay vào đó, hãy sử dụng cú pháp còn-lại `...`. eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

  > Tại sao? `...` định rõ các đối số mà bạn muốn lấy. Thêm nữa, kết quả của còn-lại là một mảng đúng nghĩa, thay vì chỉ là giống-mảng như `arguments`.

  ``` javascript
  // bad
  function concatenateAll() {
    const args = Array.prototype.slice.call(arguments);
    return args.join('');
  }

  // good
  function concatenateAll(...args) {
    return args.join('');
  }
  ```

<a name="es6-default-parameters"></a><a name="7.7"></a>
- [7.7](#es6-default-parameters) Sử dụng tham số mặc định thay vì làm biến đổi các đối số truyền vào hàm.

  ``` javascript
  // very bad
  function handleThings(opts) {
    // No! We shouldn’t mutate function arguments.
    // Double bad: if opts is falsy it'll be set to an object which may
    // be what you want but it can introduce subtle bugs.
    opts = opts || {};
    // ...
  }

  // still bad
  function handleThings(opts) {
    if (opts === void 0) {
      opts = {};
    }
    // ...
  }

  // good
  function handleThings(opts = {}) {
    // ...
  }
  ```

<a name="functions--default-side-effects"></a><a name="7.8"></a>
- [7.8](#functions--default-side-effects) Tránh gây ra hiệu ứng phụ với tham số mặc định.

  > Tại sao? Chúng khá là rối để có thể hình dung.

  ``` javascript
  var b = 1;
  // bad
  function count(a = b++) {
    console.log(a);
  }
  count();  // 1
  count();  // 2
  count(3); // 3
  count();  // 3
  ```

<a name="functions--defaults-last"></a><a name="7.9"></a>
- [7.9](#functions--defaults-last) Luôn để các tham số mặc định ở sau cùng. eslint: [`default-param-last`](https://eslint.org/docs/rules/default-param-last)

  ``` javascript
  // bad
  function handleThings(opts = {}, name) {
    // ...
  }

  // good
  function handleThings(name, opts = {}) {
    // ...
  }
  ```

<a name="functions--constructor"></a><a name="7.10"></a>
- [7.10](#functions--constructor) Không bao giờ sử dụng hàm tạo `Function` để tạo hàm. eslint: [`no-new-func`](https://eslint.org/docs/rules/no-new-func)

  > Tại sao? Tạo một hàm theo cách này cũng thực thi chuỗi giống như `eval()` vậy, thứ mà mở ra các lỗ hổng.

  ``` javascript
  // bad
  var add = new Function('a', 'b', 'return a + b');

  // still bad
  var subtract = Function('a', 'b', 'return a - b');
  ```

<a name="functions--signature-spacing"></a><a name="7.11"></a>
- [7.11](#functions--signature-spacing) Sử dụng các dấu cách giữa các bộ phận hàm. eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

  > Tại sao? Sự đồng nhất vẫn cứ là tốt, và bạn không cần phải thêm hoặc bớt dấu cách khi không đặt tên hàm.

  ``` javascript
  // bad
  const f = function(){};
  const g = function (){};
  const h = function() {};

  // good
  const x = function () {};
  const y = function a() {};
  ```

<a name="functions--mutate-params"></a><a name="7.12"></a>
- [7.12](#functions--mutate-params) Không bao giờ làm biến đổi các tham số. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

  > Tại sao? Việc can thiệp vào các đối tượng được truyền vào dưới dạng tham số có thể gây hiệu ứng phụ không mong muốn đối với biến tại tiến trình gọi.

  ``` javascript
  // bad
  function f1(obj) {
    obj.key = 1;
  }

  // good
  function f2(obj) {
    const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
  }
  ```

<a name="functions--reassign-params"></a><a name="7.13"></a>
- [7.13](#functions--reassign-params) Không bao giờ gán lại các tham số. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

  > Tại sao? Việc gán lại các tham số có thể dẫn tới hành vi không mong muốn, đặc biệt là khi truy cập đối tượng `arguments`. Nó cũng có thể gây ra một số vấn đề về tối ưu hóa, nhất là trong V8.

  ``` javascript
  // bad
  function f1(a) {
    a = 1;
    // ...
  }

  function f2(a) {
    if (!a) { a = 1; }
    // ...
  }

  // good
  function f3(a) {
    const b = a || 1;
    // ...
  }

  function f4(a = 1) {
    // ...
  }
  ```

<a name="functions--spread-vs-apply"></a><a name="7.14"></a>
- [7.14](#functions--spread-vs-apply) Ưu tiên sử dụng cú pháp liệt kê `...` để gọi các hàm bất định. eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

  > Tại sao? Nó nhìn sáng sủa hơn, bạn không cần phải đặt ngữ cảnh, và bạn cũng đâu thể dễ dàng kết hợp `new` với `apply`.

  ``` javascript
  // bad
  const x = [1, 2, 3, 4, 5];
  console.log.apply(console, x);

  // good
  const x = [1, 2, 3, 4, 5];
  console.log(...x);

  // bad
  new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

  // good
  new Date(...[2016, 8, 5]);
  ```

<a name="functions--signature-invocation-indentation"></a>
- [7.15](#functions--signature-invocation-indentation) Các hàm với các bộ phận hàm, hoặc các phép gọi, nằm trên nhiều dòng nên được căn đầu dòng như tất cả các danh sách nhiều dòng khác trong hướng dẫn này: với mỗi mục nằm trên một dòng riêng biệt, cùng với một dấu phẩy ngay sau mục cuối cùng. eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)

  ``` javascript
  // bad
  function foo(bar,
               baz,
               quux) {
    // ...
  }

  // good
  function foo(
    bar,
    baz,
    quux,
  ) {
    // ...
  }

  // bad
  console.log(foo,
    bar,
    baz);

  // good
  console.log(
    foo,
    bar,
    baz,
  );
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="arrow-functions">Arrow Functions</a>

<a name="arrows--use-them"></a><a name="8.1"></a>
- [8.1](#arrows--use-them) Khi bạn phải sử dụng một hàm vô danh (như khi cần truyền một hàm gọi lại trên cùng dòng), sử dụng ký pháp hàm mũi tên. eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing.html)

  > Tại sao? Nó tạo ra một hàm thực thi trên ngữ cảnh của `this`, thường là thứ bạn cần, và nó rất súc tích.

  > Khi nào thì không? Khi bạn có một hàm tương đối rắc rối, bạn cần phải chuyển lô-gíc của hàm đó sang biểu thức hàm hữu danh.

  ``` javascript
  // bad
  [1, 2, 3].map(function (x) {
    const y = x + 1;
    return x * y;
  });

  // good
  [1, 2, 3].map((x) => {
    const y = x + 1;
    return x * y;
  });
  ```

<a name="arrows--implicit-return"></a><a name="8.2"></a>
- [8.2](#arrows--implicit-return) Nếu như phần thân hàm chỉ gồm một câu lệnh trả về một [biểu thức](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) mà không có hiệu ứng phụ, bỏ qua dấu ngoặc nhọn và sử dụng trả về ngầm định. Nếu không, giữ nguyên dấu ngoặc và sử dụng lệnh `return`. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style.html)

  > Tại sao? Cú pháp tiện lợi. Nó dễ đọc khi nhiều hàm nối chuỗi nhau.

  ``` javascript
  // bad
  [1, 2, 3].map((number) => {
    const nextNumber = number + 1;
    `A string containing the ${nextNumber}.`;
  });

  // good
  [1, 2, 3].map((number) => `A string containing the ${number + 1}.`);

  // good
  [1, 2, 3].map((number) => {
    const nextNumber = number + 1;
    return `A string containing the ${nextNumber}.`;
  });

  // good
  [1, 2, 3].map((number, index) => ({
    [index]: number,
  }));

  // No implicit return with side effects
  function foo(callback) {
    const val = callback();
    if (val === true) {
      // Do something if callback returns true
    }
  }

  let bool = false;

  // bad
  foo(() => bool = true);

  // good
  foo(() => {
    bool = true;
  });
  ```

<a name="arrows--paren-wrap"></a><a name="8.3"></a>
- [8.3](#arrows--paren-wrap) Trong trường hợp biểu thức nằm trên nhiều dòng, nhóm nó trong ngoặc để dễ đọc hơn.

  > Tại sao? Nó cho thấy một cách rõ ràng điểm bắt đầu và kết thúc hàm.

  ``` javascript
  // bad
  ['get', 'post', 'put'].map((httpMethod) => Object.prototype.hasOwnProperty.call(
      httpMagicObjectWithAVeryLongName,
      httpMethod,
    )
  );

  // good
  ['get', 'post', 'put'].map((httpMethod) => (
    Object.prototype.hasOwnProperty.call(
      httpMagicObjectWithAVeryLongName,
      httpMethod,
    )
  ));
  ```

<a name="arrows--one-arg-parens"></a><a name="8.4"></a>
- [8.4](#arrows--one-arg-parens) Luôn sử dụng ngoặc tròn xung quanh các đối số để rõ ràng và nhất quán. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html)

  > Tại sao? Giảm thiểu sự khác biệt khi thêm và xóa các đối số.

  ``` javascript
  // bad
  [1, 2, 3].map(x => x * x);

  // good
  [1, 2, 3].map((x) => x * x);

  // good
  [1, 2, 3].map((number) => (
    `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
  ));

  // bad
  [1, 2, 3].map(x => {
    const y = x + 1;
    return x * y;
  });

  // good
  [1, 2, 3].map((x) => {
    const y = x + 1;
    return x * y;
  });
  ```

<a name="arrows--confusing"></a><a name="8.5"></a>
- [8.5](#arrows--confusing) Tránh gây dễ nhầm lẫn giữa cú pháp hàm mũi tên (`=>`) với các toán tử so sánh (`<=`, `>=`). eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

  ``` javascript
  // bad
  const itemHeight = (item) => item.height <= 256 ? item.largeSize : item.smallSize;

  // bad
  const itemHeight = (item) => item.height >= 256 ? item.largeSize : item.smallSize;

  // good
  const itemHeight = (item) => (item.height <= 256 ? item.largeSize : item.smallSize);

  // good
  const itemHeight = (item) => {
    const { height, largeSize, smallSize } = item;
    return height <= 256 ? largeSize : smallSize;
  };
  ```

<a name="whitespace--implicit-arrow-linebreak"></a>
- [8.6](#whitespace--implicit-arrow-linebreak) Cách đặt vị trí của phần thân hàm mũi tên với trả về ngầm định. eslint: [`implicit-arrow-linebreak`](https://eslint.org/docs/rules/implicit-arrow-linebreak)

  ``` javascript
  // bad
  (foo) =>
    bar;

  (foo) =>
    (bar);

  // good
  (foo) => bar;
  (foo) => (bar);
  (foo) => (
     bar
  )
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="classes--constructors">Classes & Constructors</a>

<a name="constructors--use-class"></a><a name="9.1"></a>
- [9.1](#constructors--use-class) Luôn sử dụng `class`. Tránh việc can thiệp trực tiếp vào `prototype`.

  > Tại sao? Cú pháp `class` súc tích, dễ hiểu và dễ hình dung.

  ``` javascript
  // bad
  function Queue(contents = []) {
    this.queue = [...contents];
  }
  Queue.prototype.pop = function () {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
  };

  // good
  class Queue {
    constructor(contents = []) {
      this.queue = [...contents];
    }
    pop() {
      const value = this.queue[0];
      this.queue.splice(0, 1);
      return value;
    }
  }
  ```

<a name="constructors--extends"></a><a name="9.2"></a>
- [9.2](#constructors--extends) Sử dụng `extends` cho thừa kế.

  > Tại sao? Nó là cách sẵn có để thừa kế nguyên mẫu mà không làm ảnh hưởng đến `instanceof`.

  ``` javascript
  // bad
  const inherits = require('inherits');
  function PeekableQueue(contents) {
    Queue.apply(this, contents);
  }
  inherits(PeekableQueue, Queue);
  PeekableQueue.prototype.peek = function () {
    return this.queue[0];
  };

  // good
  class PeekableQueue extends Queue {
    peek() {
      return this.queue[0];
    }
  }
  ```

<a name="constructors--chaining"></a><a name="9.3"></a>
- [9.3](#constructors--chaining) Các phương thức, mỗi khi có thể, hãy trả về `this` để tiện cho việc nối chuỗi phương thức.

  ``` javascript
  // bad
  Jedi.prototype.jump = function () {
    this.jumping = true;
    return true;
  };

  Jedi.prototype.setHeight = function (height) {
    this.height = height;
  };

  const luke = new Jedi();
  luke.jump(); // => true
  luke.setHeight(20); // => undefined

  // good
  class Jedi {
    jump() {
      this.jumping = true;
      return this;
    }

    setHeight(height) {
      this.height = height;
      return this;
    }
  }

  const luke = new Jedi();

  luke.jump()
    .setHeight(20);
  ```

<a name="constructors--tostring"></a><a name="9.4"></a>
- [9.4](#constructors--tostring) Có thể viết phương thức `toString()` tùy ý, chỉ cần đảm bản nó hoạt động hoàn hảo và không gây ra các hiệu ứng phụ.

  ``` javascript
  class Jedi {
    constructor(options = {}) {
      this.name = options.name || 'undefined';
    }

    getName() {
      return this.name;
    }

    toString() {
      return `Jedi - ${this.getName()}`;
    }
  }
  ```

<a name="constructors--no-useless"></a><a name="9.5"></a>
- [9.5](#constructors--no-useless) Các lớp có một hàm tạo mặc định nếu không được chỉ định. Một hàm tạo rỗng, hoặc chỉ trỏ đến lớp cha, là không cần thiết. eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

  ``` javascript
  // bad
  class Jedi {
    constructor() {}

    getName() {
      return this.name;
    }
  }

  // bad
  class Rey extends Jedi {
    constructor(...args) {
      super(...args);
    }
  }

  // good
  class Rey extends Jedi {
    constructor(...args) {
      super(...args);
      this.name = 'Rey';
    }
  }
  ```

<a name="classes--no-duplicate-members"></a>
- [9.6](#classes--no-duplicate-members) Tránh trùng lặp các thành viên của một lớp. eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

  > Tại sao? Với các khai báo thành viên bị lặp, khai báo cuối được tự động ưu tiên - việc có sự trùng lặp gần như chắc chắn là một lỗi.

  ``` javascript
  // bad
  class Foo {
    bar() { return 1; }
    bar() { return 2; }
  }

  // good
  class Foo {
    bar() { return 1; }
  }

  // good
  class Foo {
    bar() { return 2; }
  }
  ```

<a name="classes--methods-use-this"></a>
- [9.7](#classes--methods-use-this) Các phương thức của lớp nên sử dụng `this` hoặc được chuyển thành phương thức tĩnh, trừ trường hợp thư viện bên ngoài hoặc bộ khung phần mềm bắt buộc sử dụng phương thức không phải phương thức tĩnh. Một phương thức là phương thức của thực thể nên mang ý nghĩa rằng nó hoạt động khác nhau dựa trên những thuộc tính của đối tượng đích.  eslint: [`class-methods-use-this`](https://eslint.org/docs/rules/class-methods-use-this)

  ```javascript
  // bad
  class Foo {
    bar() {
      console.log('bar');
    }
  }

  // good - this is used
  class Foo {
    bar() {
      console.log(this.bar);
    }
  }

  // good - constructor is exempt
  class Foo {
    constructor() {
      // ...
    }
  }

  // good - static methods aren't expected to use this
  class Foo {
    static bar() {
      console.log('bar');
    }
  }
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="modules">Modules</a>

<a name="modules--use-them"></a><a name="10.1"></a>
- [10.1](#modules--use-them) Luôn sử dụng mô-đun (`import`/`export`) thay vì một hệ thống mô-đun phi chuẩn. Bạn luôn có thể dịch mã sang hệ thống mô-đun mà bạn thích.

  > Tại sao? Mô-đun là tương lai, hãy cùng sử dụng tương lai ngay lúc này.

  ``` javascript
  // bad
  const AirbnbStyleGuide = require('./AirbnbStyleGuide');
  module.exports = AirbnbStyleGuide.es6;

  // ok
  import AirbnbStyleGuide from './AirbnbStyleGuide';
  export default AirbnbStyleGuide.es6;

  // best
  import { es6 } from './AirbnbStyleGuide';
  export default es6;
  ```

<a name="modules--no-wildcard"></a><a name="10.2"></a>
- [10.2](#modules--no-wildcard) Không sử dụng ký tự đại diện để nhập.

  > Tại sao? Điều này đảm bảo bạn chỉ xuất mặc định một giá trị.

  ``` javascript
  // bad
  import * as AirbnbStyleGuide from './AirbnbStyleGuide';

  // good
  import AirbnbStyleGuide from './AirbnbStyleGuide';
  ```

<a name="modules--no-export-from-import"></a><a name="10.3"></a>
- [10.3](#modules--no-export-from-import) Và không xuất trực tiếp từ một lệnh nhập.

  > Tại sao? Mặc dù cấu trúc một dòng là súc tích, việc nhập một cách rõ ràng và xuất một cách rõ ràng làm cho mọi thứ nhất quán.

  ``` javascript
  // bad
  // filename es6.js
  export { es6 as default } from './AirbnbStyleGuide';

  // good
  // filename es6.js
  import { es6 } from './AirbnbStyleGuide';
  export default es6;
  ```

<a name="modules--no-duplicate-imports"></a>
- [10.4](#modules--no-duplicate-imports) Chỉ nhập từ một đường dẫn ở chung một chỗ.
  eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)
  > Tại sao? Có nhiều dòng nhập từ cùng một đường dẫn khiến mã nguồn trở nên khó bảo trì hơn.

  ``` javascript
  // bad
  import foo from 'foo';
  // … some other imports … //
  import { named1, named2 } from 'foo';

  // good
  import foo, { named1, named2 } from 'foo';

  // good
  import foo, {
    named1,
    named2,
  } from 'foo';
  ```

<a name="modules--no-mutable-exports"></a>
- [10.5](#modules--no-mutable-exports) Không xuất các ràng buộc có thể bị biến đổi.
  eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)
  > Tại sao? Sự biến đổi, nói chung, nên được tránh, nhưng đặc biệt là đối với trường hợp xuất các giá trị có thể bị biến đổi. Trong khi kỹ thuật này có thể là cần thiết trong một số trường hợp đặc biệt, nhìn chung, chỉ nên xuất các giá trị là hằng.

  ``` javascript
  // bad
  let foo = 3;
  export { foo };

  // good
  const foo = 3;
  export { foo };
  ```

<a name="modules--prefer-default-export"></a>
- [10.6](#modules--prefer-default-export) Trong các mô-đun chỉ có một địa chỉ xuất, ưu tiên xuất mặc định thay vì xuất hữu danh.
  eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)
  > Tại sao? Nhằm khuyến khích các tệp chỉ xuất một giá trị, giúp mã nguồn dễ đọc và dễ bảo trì.

  ``` javascript
  // bad
  export function foo() {}

  // good
  export default function foo() {}
  ```

<a name="modules--imports-first"></a>
- [10.7](#modules--imports-first) Đặt tất cả các lệnh `import` trên cùng.
  eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
  > Tại sao? Vì các lệnh `import` được kéo lên, việc đặt tất cả chúng ở trên cùng nhằm ngăn chặn các hành vi không đáng có.

  ``` javascript
  // bad
  import foo from 'foo';
  foo.init();

  import bar from 'bar';

  // good
  import foo from 'foo';
  import bar from 'bar';

  foo.init();
  ```

<a name="modules--multiline-imports-over-newlines"></a>
- [10.8](#modules--multiline-imports-over-newlines) Các lệnh nhập nhiều dòng nên được căn đầu dòng giống như các mảng hay đối tượng nguyên văn nhiều dòng. eslint: [`object-curly-newline`](https://eslint.org/docs/rules/object-curly-newline)

  > Tại sao? Các đấu ngoặc nhọn đều có cùng các quy tắc căn đầu dòng như tất cả mọi khối ngoặc nhọn trong bản định hướng này, cùng với như dấu phẩy ở cuối.

  ``` javascript
  // bad
  import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

  // good
  import {
    longNameA,
    longNameB,
    longNameC,
    longNameD,
    longNameE,
  } from 'path';
  ```

<a name="modules--no-webpack-loader-syntax"></a>
- [10.9](#modules--no-webpack-loader-syntax) Không cho phép cú pháp bộ tải Webpack trong các lệnh nhập mô-đun.
  eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)
  > Tại sao? Vì sử dụng cú pháp Webpack trong các lệnh nhập gom mã thành một bộ tổng hợp mô-đun. Ưu tiên sử dụng cú pháp bộ tải trong `webpack.config.js`.

  ``` javascript
  // bad
  import fooSass from 'css!sass!foo.scss';
  import barCss from 'style!css!bar.css';

  // good
  import fooSass from 'foo.scss';
  import barCss from 'bar.css';
  ```

<a name="modules--import-extensions"></a>
- [10.10](#modules--import-extensions) Không thêm phần mở rộng của tên tệp JavaScript. eslint: [`import/extensions`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/extensions.md)
  > Tạo sao? Việc thêm phần mở rộng của tệp khiến cho việc cải tiến mã nguồn trở nên khó khăn hơn, và tạo nên những chi tiết không cần thiết trong lệnh nhập mô-đun mỗi khi bạn sử dụng.

  ```javascript
  // bad
  import foo from './foo.js';
  import bar from './bar.jsx';
  import baz from './baz/index.jsx';
  // good
  import foo from './foo';
  import bar from './bar';
  import baz from './baz';
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="iterators-and-generators">Iterators and Generators</a>

<a name="iterators--nope"></a><a name="11.1"></a>
- [11.1](#iterators--nope) Không sử dụng các đối tượng duyệt. Ưu tiên sử dụng các hàm bậc cao hơn của JavaScript thay vì các vòng lặp như `for-in` hay `for-of`. eslint: [`no-iterator`](https://eslint.org/docs/rules/no-iterator.html) [`no-restricted-syntax`](https://eslint.org/docs/rules/no-restricted-syntax)

  > Tại sao? Điều này đảm bảo việc thực hiện quy tắc bất biến. Làm việc với các hàm thuần mà trả về các giá trị sẽ dễ tưởng tượng hơn so với các hiệu ứng phụ.

  > Sử dụng `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... để duyệt qua một mảng, và `Object.keys()` / `Object.values()` / `Object.entries()` để tạo một mảng để bạn có thể duyệt qua một đối tượng.

  ``` javascript
  const numbers = [1, 2, 3, 4, 5];

  // bad
  let sum = 0;
  for (let num of numbers) {
    sum += num;
  }
  sum === 15;

  // good
  let sum = 0;
  numbers.forEach((num) => {
    sum += num;
  });
  sum === 15;

  // best, (use the functional force)
  const sum = numbers.reduce((total, num) => total + num, 0);
  sum === 15;

  // bad
  const increasedByOne = [];
  for (let i = 0; i < numbers.length; i++) {
    increasedByOne.push(numbers[i] + 1);
  }

  // good
  const increasedByOne = [];
  numbers.forEach((num) => {
    increasedByOne.push(num + 1);
  });

  // best (use the functional force)
  const increasedByOne = numbers.map((num) => num + 1);
  ```

<a name="generators--nope"></a><a name="11.2"></a>
- [11.2](#generators--nope) Không sử dụng các hàm sinh trị `function*` vào thời điểm này.

  > Tại sao? Nó không thể được dịch mã sang ES5 một cách hoàn hảo.

<a name="generators--spacing"></a>
- [11.3](#generators--spacing) Nếu bạn bắt buộc phải dùng các hàm sinh trị, hoặc bạn bỏ qua [khuyến nghị của chúng tôi](#generators--nope), hãy đảm bảo rằng bạn sử dụng dấu cách giữa các bộ phận hàm một cách hợp lý. eslint: [`generator-star-spacing`](https://eslint.org/docs/rules/generator-star-spacing)

  > Tại sao? `function` và `*` là tạo thành một từ khóa riêng - `*` không phải là từ khóa điều chỉnh cho `function`, `function*` là một cấu tạo riêng biệt, khác với `function`.

  ``` javascript
  // bad
  function * foo() {
    // ...
  }

  // bad
  const bar = function * () {
    // ...
  };

  // bad
  const baz = function *() {
    // ...
  };

  // bad
  const quux = function*() {
    // ...
  };

  // bad
  function*foo() {
    // ...
  }

  // bad
  function *foo() {
    // ...
  }

  // very bad
  function
  *
  foo() {
    // ...
  }

  // very bad
  const wat = function
  *
  () {
    // ...
  };

  // good
  function* foo() {
    // ...
  }

  // good
  const foo = function* () {
    // ...
  };
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="properties">Properties</a>

<a name="properties--dot"></a><a name="12.1"></a>
- [12.1](#properties--dot) Sử dụng ký pháp chấm `.` để truy cập các thuộc tính. eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation.html)

  ``` javascript
  const luke = {
    jedi: true,
    age: 28,
  };

  // bad
  const isJedi = luke['jedi'];

  // good
  const isJedi = luke.jedi;
  ```

<a name="properties--bracket"></a><a name="12.2"></a>
- [12.2](#properties--bracket) Sử dụng ký pháp ngoặc `[]` để truy cập thuộc tính với một biến.

  ``` javascript
  const luke = {
    jedi: true,
    age: 28,
  };

  function getProp(prop) {
    return luke[prop];
  }

  const isJedi = getProp('jedi');
  ```

<a name="es2016-properties--exponentiation-operator"></a>
- [12.3](#es2016-properties--exponentiation-operator) Sử dụng toán tử lũy thừa `**` để tính các lũy thừa. eslint: [`no-restricted-properties`](https://eslint.org/docs/rules/no-restricted-properties).

  ``` javascript
  // bad
  const binary = Math.pow(2, 10);

  // good
  const binary = 2 ** 10;
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="variables">Variables</a>

<a name="variables--const"></a><a name="13.1"></a>
- [13.1](#variables--const) Luôn sử dụng `const` hoặc `let` để khai báo biến. Không làm như vậy sẽ dẫn đến các biến toàn cục. Chúng ta muốn tránh việc làm ô nhiễm không gian tên toàn cục. Đội trưởng Hành tinh đã cảnh báo chúng ta. eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

  ``` javascript
  // bad
  superPower = new SuperPower();

  // good
  const superPower = new SuperPower();
  ```

<a name="variables--one-const"></a><a name="13.2"></a>
- [13.2](#variables--one-const) Sử dụng một `const` hoặc `let` khai báo cho mỗi biến hoặc phép gán. eslint: [`one-var`](https://eslint.org/docs/rules/one-var.html)

  > Tại sao? Khai báo theo cách này giúp dễ thêm các khai báo mới, và bạn chẳng phải nghĩ về việc phải dùng `;` hay `,`. Bạn còn có thể bước qua mỗi khai báo trong trình gỡ lỗi, thay vì nhảy qua toàn bộ chúng trong một bước.

  ``` javascript
  // bad
  const items = getItems(),
      goSportsTeam = true,
      dragonball = 'z';

  // bad
  // (compare to above, and try to spot the mistake)
  const items = getItems(),
      goSportsTeam = true;
      dragonball = 'z';

  // good
  const items = getItems();
  const goSportsTeam = true;
  const dragonball = 'z';
  ```

<a name="variables--const-let-group"></a><a name="13.3"></a>
- [13.3](#variables--const-let-group) Nhóm tất cả các `const` và rồi nhóm tất cả các `let`.

  > Tại sao? Điều này hữu ích khi, sau đó, bạn sẽ cần gán lại một biến dựa trên các biến đã gán trước đó.

  ``` javascript
  // bad
  let i, len, dragonball,
      items = getItems(),
      goSportsTeam = true;

  // bad
  let i;
  const items = getItems();
  let dragonball;
  const goSportsTeam = true;
  let len;

  // good
  const goSportsTeam = true;
  const items = getItems();
  let dragonball;
  let i;
  let length;
  ```

<a name="variables--define-where-used"></a><a name="13.4"></a>
- [13.4](#variables--define-where-used) Chỉ gán biến khi cần, nhưng nhớ đặt chúng ở một nơi hợp lý.

  > Tại sao? `let` và `const` thuộc phạm vi khối, không phải phạm vi hàm.

  ``` javascript
  // bad - unnecessary function call
  function checkName(hasName) {
    const name = getName();

    if (hasName === 'test') {
      return false;
    }

    if (name === 'test') {
      this.setName('');
      return false;
    }

    return name;
  }

  // good
  function checkName(hasName) {
    if (hasName === 'test') {
      return false;
    }

    const name = getName();

    if (name === 'test') {
      this.setName('');
      return false;
    }

    return name;
  }
  ```

<a name="variables--no-chain-assignment"></a><a name="13.5"></a>
- [13.5](#variables--no-chain-assignment) Đừng nối chuỗi các phép gán. eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

  > Tại sao? Việc nối chuỗi các phép gán tạo ra các biến toàn cục ngầm định.

  ``` javascript
  // bad
  (function example() {
    // JavaScript interprets this as
    // let a = ( b = ( c = 1 ) );
    // The let keyword only applies to variable a; variables b and c become
    // global variables.
    let a = b = c = 1;
  }());

  console.log(a); // throws ReferenceError
  console.log(b); // 1
  console.log(c); // 1

  // good
  (function example() {
    let a = 1;
    let b = a;
    let c = a;
  }());

  console.log(a); // throws ReferenceError
  console.log(b); // throws ReferenceError
  console.log(c); // throws ReferenceError

  //  the same applies for `const`
  ```

<a name="variables--unary-increment-decrement"></a><a name="13.6"></a>
- [13.6](#variables--unary-increment-decrement) Tránh việc sử dụng các phép tăng và giảm một ngôi (`++`, `--`). eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

  > Tại sao? Theo như tài liệu của eslint, các phép tăng hoặc giảm một ngôi phụ thuộc vào Quy tắc thêm dấu chấm phẩy tự động và có thể gây ra các lỗi câm trong việc tăng hoặc giảm các giá trị trong một ứng dụng. Sự diễn đạt cũng trở nên rõ ràng hơn khi bạn biến đổi các giá trị với các lệnh, như `num += 1`, thay vì `num++` hay `num ++`. Việc không cho phép các lệnh tăng hoặc giảm một ngôi cũng giúp bạn tránh được các sự tiền tăng/tiền giảm các giá trị một cách không chủ ý, điều có thể cũng gây ra những hành vi không mong muốn cho chương trình của bạn.

  ``` javascript
  // bad

  const array = [1, 2, 3];
  let num = 1;
  num++;
  --num;

  let sum = 0;
  let truthyCount = 0;
  for (let i = 0; i < array.length; i++) {
    let value = array[i];
    sum += value;
    if (value) {
      truthyCount++;
    }
  }

  // good

  const array = [1, 2, 3];
  let num = 1;
  num += 1;
  num -= 1;

  const sum = array.reduce((a, b) => a + b, 0);
  const truthyCount = array.filter(Boolean).length;
  ```

<a name="variables--linebreak"></a>
- [13.7](#variables--linebreak) Tránh các dấu ngắt dòng trước và sau `=` trong một phép gán. Nếu phép gán của bạn vi phạm [`max-len`](https://eslint.org/docs/rules/max-len.html), hãy đặt giá trị trong ngoặc tròn. eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak.html).

  > Tại sao? Các dấu ngắt dòng quanh `=` có thể làm mờ nhạt giá trị trong phép gán.

  ``` javascript
  // bad
  const foo =
    superLongLongLongLongLongLongLongLongFunctionName();

  // bad
  const foo
    = 'superLongLongLongLongLongLongLongLongString';

  // good
  const foo = (
    superLongLongLongLongLongLongLongLongFunctionName()
  );

  // good
  const foo = 'superLongLongLongLongLongLongLongLongString';
  ```

<a name="variables--no-unused-vars"></a>
- [13.8](#variables--no-unused-vars) Không cho phép các biến không được sử dụng. eslint: [`no-unused-vars`](https://eslint.org/docs/rules/no-unused-vars)

  > Tại sao? Các biến được khai báo nhưng không được sử dụng ở khắp mọi nơi trong mã gần như chắc chắn là một lỗi do sự cải tiến mã nguồn chưa hoàn thiện. Những biến như vậy chiếm chỗ trong mã và có thể gây ra sự khó hiểu cho người đọc.

  ``` javascript
  // bad

  const some_unused_var = 42;

  // Write-only variables are not considered as used.
  const y = 10;
  y = 5;

  // A read for a modification of itself is not considered as used.
  const z = 0;
  z = z + 1;

  // Unused function arguments.
  function getX(x, y) {
      return x;
  }

  // good

  function getXPlusY(x, y) {
    return x + y;
  }

  const x = 1;
  const y = a + 2;

  alert(getXPlusY(x, y));

  // 'type' is ignored even if unused because it has a rest property sibling.
  // This is a form of extracting an object that omits the specified keys.
  // Đây là một cách để trích xuất một đối tượng mà bỏ qua một vài thuộc tính.
  const { type, ...coords } = data;
  // 'coords' is now the 'data' object without its 'type' property.
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="hoisting">Hoisting</a>

<a name="hoisting--about"></a><a name="14.1"></a>
- [14.1](#hoisting--about) Các khai báo bằng `var` được kéo lên đầu của phạm vi hàm gần nhất, còn phép gán thì không. Các khai báo bằng `const` và `let` thì mang trên mình một đặc tính khác là [Giai đoạn chết](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone). Điều này là quan trọng để biết tại sao [`typeof` không còn an toàn](https://web.archive.org/web/20200121061528/http://es-discourse.com/t/why-typeof-is-no-longer-safe/15).

  ```javascript
  // we know this wouldn’t work (assuming there
  // is no notDefined global variable)
  function example() {
    console.log(notDefined); // => throws a ReferenceError
  }

  // creating a variable declaration after you
  // reference the variable will work due to
  // variable hoisting. Note: the assignment
  // value of `true` is not hoisted.
  function example() {
    console.log(declaredButNotAssigned); // => undefined
    var declaredButNotAssigned = true;
  }

  // the interpreter is hoisting the variable
  // declaration to the top of the scope,
  // which means our example could be rewritten as:
  function example() {
    let declaredButNotAssigned;
    console.log(declaredButNotAssigned); // => undefined
    declaredButNotAssigned = true;
  }

  // using const and let
  function example() {
    console.log(declaredButNotAssigned); // => throws a ReferenceError
    console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
    const declaredButNotAssigned = true;
  }
    ```

<a name="hoisting--anon-expressions"></a><a name="14.2"></a>
- [14.2](#hoisting--anon-expressions) Các biểu thức hàm vô danh sẽ được kéo tên biến lên, nhưng không được kéo phép gán hàm.

  ``` javascript
  function example() {
    console.log(anonymous); // => undefined

    anonymous(); // => TypeError anonymous is not a function

    var anonymous = function () {
      console.log('anonymous function expression');
    };
  }
  ```

<a name="hoisting--named-expresions"></a><a name="hoisting--named-expressions"></a><a name="14.3"></a>
- [14.3](#hoisting--named-expressions) Biểu thức hàm hữu danh sẽ được kéo tên biến lên, nhưng không được kéo tên hàm và thân hàm.

  ```javascript
   function example() {
     console.log(named); // => undefined
     named(); // => TypeError named is not a function
     superPower(); // => ReferenceError superPower is not defined
     var named = function superPower() {
       console.log('Flying');
     };
   }
   // the same is true when the function name
   // is the same as the variable name.
   function example() {
     console.log(named); // => undefined
     named(); // => TypeError named is not a function
     var named = function named() {
       console.log('named');
     };
   }
    ```

<a name="hoisting--declarations"></a><a name="14.4"></a>
- [14.4](#hoisting--declarations) Khai báo hàm được kéo cả tên và thân hàm lên.

  ``` javascript
  function example() {
    superPower(); // => Flying

    function superPower() {
      console.log('Flying');
    }
  }
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="comparison-operators--equality">Comparison Operators & Equality</a>

<a name="comparison--eqeqeq"></a><a name="15.1"></a>
- [15.1](#comparison--eqeqeq) Sử dụng `===` và `!==` thay vì `==` và `!=`. eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

<a name="comparison--if"></a><a name="15.2"></a>
- [15.2](#comparison--if) Các câu lệnh điều kiện như lệnh `if` xét biểu thức của chúng bằng cách ép kiểu bằng một phương thức ảo`ToBoolean` và đều tuân theo những quy tắc đơn giản sau:

    - **Các đối tượng** tương đương với **true**
    - **Undefined** tương đương với **false**
    - **Null** tương đương với **false**
    - **Các boolean** tương đương với **giá trị của boolean**
    - **Các số** tương đương với **false** nếu là **+0, -0, hoặc NaN**, còn không sẽ là **true**
    - **Các chuỗi** tương đương vớiv**false** nếu là một chuỗi rỗng `''`, còn không sẽ là **true**

  ``` javascript
  if ([0] && []) {
     // true
     // an array (even an empty one) is an object, objects will evaluate to true
  }
  ```

<a name="comparison--shortcuts"></a><a name="15.3"></a>
- [15.3](#comparison--shortcuts) Sử dụng dạng rút gọn cho các boolean, nhưng dùng dạng so sánh cụ thể đối với chuỗi và số.

  ``` javascript
  // bad
  if (isValid === true) {
    // ...
  }

  // good
  if (isValid) {
    // ...
  }

  // bad
  if (name) {
    // ...
  }

  // good
  if (name !== '') {
    // ...
  }

  // bad
  if (collection.length) {
    // ...
  }

  // good
  if (collection.length > 0) {
    // ...
  }
  ```

<a name="comparison--moreinfo"></a><a name="15.4"></a>
- [15.4](#comparison--moreinfo) Để biết thêm chi tiết, xem bài viết [Truth Equality and JavaScript](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) bởi Angus Croll.

<a name="comparison--switch-blocks"></a><a name="15.5"></a>
- [15.5](#comparison--switch-blocks) Sử dụng các dấu ngoặc cho các khối của mệnh đề `case` và `default` nếu nó có chứa các khai báo (như `let`, `const`, `function`, và `class`). eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations.html)

  > Tại sao? Các khai báo tồn tại trong cả khối `switch` nhưng chỉ được khởi tạo khi được gán, mà nó chỉ xảy ra khi `case` của nó xảy ra. Điều này gây ra các lỗi khi mà nhiều mệnh đề `case` muốn định nghĩa cùng một thứ.

  ``` javascript
  // bad
  switch (foo) {
    case 1:
      let x = 1;
      break;
    case 2:
      const y = 2;
      break;
    case 3:
      function f() {
        // ...
      }
      break;
    default:
      class C {}
  }

  // good
  switch (foo) {
    case 1: {
      let x = 1;
      break;
    }
    case 2: {
      const y = 2;
      break;
    }
    case 3: {
      function f() {
        // ...
      }
      break;
    }
    case 4:
      bar();
      break;
    default: {
      class C {}
    }
  }
  ```

<a name="comparison--nested-ternaries"></a><a name="15.6"></a>
- [15.6](#comparison--nested-ternaries) Các toán tử ba ngôi không nên được đặt trong ngoặc và thường được viết trên một dòng riêng. eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary.html)

  ``` javascript
  // bad
  const foo = maybe1 > maybe2
    ? "baz"
    : value1 > value2 ? "baz" : null;

  // split into 2 separated ternary expressions
  const maybeNull = value1 > value2 ? 'baz' : null;
  
  // better
  const foo = maybe1 > maybe2
    ? 'bar'
    : maybeNull
    
  // best
  const foo = maybe1 > maybe2 ? 'baz' : maybeNull;
  ```

<a name="comparison--unneeded-ternary"></a><a name="15.7"></a>
- [15.7](#comparison--unneeded-ternary) Tránh các câu lệnh ba ngôi không đáng có. eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary.html)

  ``` javascript
  // bad
  const foo = a ? a : b;
  const bar = c ? true : false;
  const baz = c ? false : true;

  // good
  const foo = a || b;
  const bar = !!c;
  const baz = !c;
  ```

<a name="comparison--no-mixed-operators"></a>
- [15.8](#comparison--no-mixed-operators) Khi kết hợp các toán tử, nhớ đóng chúng trong ngoặc. Ngoại lệ duy nhất là các toán tử tiêu chuẩn: `+`, `-` và `**` vì chúng có thứ tự ưu tiên mà ai ai cũng hiểu. Chúng tôi khuyến khích việc sử dụng đóng ngoặc cho `/` và `*` vì thứ tự ưu tiên của chúng có thể bị nhầm lẫn khi chúng được sử dụng gần nhau. eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators.html)

  > Tại sao? Điều này cả thiện tính khả đọc và làm rõ ý định của nhà phát triển.

  ``` javascript
  // bad
  const foo = a && b < 0 || c > 0 || d + 1 === 0;

  // bad
  const bar = a ** b - 5 % d;

  // bad
  // one may be confused into thinking (a || b) && c
  if (a || b && c) {
    return d;
  }

  // bad
  const bar = a + b / c * d;

  // good
  const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

  // good
  const bar = a ** b - (5 % d);

  // good
  if (a || (b && c)) {
    return d;
  }

  // good
  const bar = a + (b / c) * d;
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="blocks">Blocks</a>

<a name="blocks--braces"></a><a name="16.1"></a>
- [16.1](#blocks--braces) Sử dụng các dấu ngoặc cho các khối nhiều dòng. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

  ``` javascript
  // bad
  if (test)
    return false;

  // good
  if (test) return false;

  // good
  if (test) {
    return false;
  }

  // bad
  function foo() { return false; }

  // good
  function bar() {
    return false;
  }
  ```

<a name="blocks--cuddled-elses"></a><a name="16.2"></a>
- [16.2](#blocks--cuddled-elses) Nếu bạn đang sử dụng các khối nhiều dòng với `if` và `else`, đặt `else` trên cùng dòng với dấu đóng ngoặc của khối `if`. eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html)

  ``` javascript
  // bad
  if (test) {
    thing1();
    thing2();
  }
  else {
    thing3();
  }

  // good
  if (test) {
    thing1();
    thing2();
  } else {
    thing3();
  }
  ```

<a name="blocks--no-else-return"></a><a name="16.3"></a>
- [16.3](#blocks--no-else-return) Nếu một khối `if` luôn thực hiện lệnh `return`, những khối `else` tiếp theo là không cần thiết. Một lệnh `return` trong một khối `else if` theo sau một khối `if` mà có chứa `return` có thể được tách thành nhiều khối `if`. eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

  ``` javascript
  // bad
  function foo() {
    if (x) {
      return x;
    } else {
      return y;
    }
  }

  // bad
  function cats() {
    if (x) {
      return x;
    } else if (y) {
      return y;
    }
  }

  // bad
  function dogs() {
    if (x) {
      return x;
    } else {
      if (y) {
        return y;
      }
    }
  }

  // good
  function foo() {
    if (x) {
      return x;
    }

    return y;
  }

  // good
  function cats() {
    if (x) {
      return x;
    }

    if (y) {
      return y;
    }
  }

  // good
  function dogs(x) {
    if (x) {
      if (z) {
        return y;
      }
    } else {
      return z;
    }
  }
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="control-statements">Control Statements</a>

<a name="control-statements"></a>
- [17.1](#control-statements) Nếu trong trường hợp lệnh điều khiển (`if`, `while`, v.v.) của bạn trở lên quá dài và vượt quá giới hạn độ dài dòng, mỗi (nhóm) điều kiện có thể được đặt ở một dòng mới. Toán tử lô-gíc nên được đặt ở đầu dòng.

  > Tại sao? Việc đặt các toán tử ở đầu dòng giúp các toán tử được căn đều và tuân theo cùng một mô hình với việc nối chuỗi phương thức. Điều này cũng cải thiện tính khả đọc vì khiến cho việc theo dõi một lô-gíc phức tạp trở nên đơn giản hơn.

  ``` javascript
  // bad
  if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
    thing1();
  }

  // bad
  if (foo === 123 &&
    bar === 'abc') {
    thing1();
  }

  // bad
  if (foo === 123
    && bar === 'abc') {
    thing1();
  }

  // bad
  if (
    foo === 123 &&
    bar === 'abc'
  ) {
    thing1();
  }

  // good
  if (
    foo === 123
    && bar === 'abc'
  ) {
    thing1();
  }

  // good
  if (
    (foo === 123 || bar === 'abc')
    && doesItLookGoodWhenItBecomesThatLong()
    && isThisReallyHappening()
  ) {
    thing1();
  }

  // good
  if (foo === 123 && bar === 'abc') {
    thing1();
  }
  ```

<a name="control-statement--value-selection"></a><a name="control-statements--value-selection"></a>
- [17.2](#control-statements--value-selection) Không sử dụng toán tử lựa chọn thay cho các câu lệnh điều khiển.

  ``` javascript
  // bad
  !isRunning && startRunning();

  // good
  if (!isRunning) {
    startRunning();
  }
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="comments">Comments</a>

<a name="comments--multiline"></a><a name="17.1"></a>
- [18.1](#comments--multiline) Sử dụng `/** ... */` cho các chú thích nhiều dòng.

  ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

<a name="comments--singleline"></a><a name="17.2"></a>
- [18.2](#comments--singleline) Sử dụng `//` cho các chú thích một dòng. Đặt các chú thích một dòng ở một dòng riêng, bên trên chủ đề của chú thích. Để một dòng trống trước chú thích trừ khi chú thích là dòng đầu tiên của một khối.

  ```javascript
    // bad
    const active = true;  // is current tab

    // good
    // is current tab
    const active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

<a name="comments--spaces"></a>
  - [18.3](#comments--spaces)  Bắt đầu tất cả các chú thích bằng một dấu cách để dễ đọc hơn. eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // bad
    //is current tab
    const active = true;

    // good
    // is current tab
    const active = true;

    // bad
    /**
     *make() returns a new element
     *based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

<a name="comments--actionitems"></a><a name="17.3"></a>
- [18.4](#comments--actionitems) Thêm `FIXME` hoặc `TODO` vào đầu chú thích giúp các nhà phát triển dễ dàng biết được rằng bạn đang chỉ ra một vấn đề cần được xem lại, hoặc bạn đang đề xuất cách giải quyết cho vấn đề nên mà được áp dụng. Các hành động có thể như `FIXME: -- cần xem xét về thứ này` hoặc `TODO: -- cần áp dụng`.

<a name="comments--fixme"></a><a name="17.4"></a>
- [18.5](#comments--fixme) Sử dụng `// FIXME:` để chú giải các vấn đề.

  ``` javascript
  class Calculator extends Abacus {
    constructor() {
      super();

      // FIXME: shouldn’t use a global here
      total = 0;
    }
  }
  ```

<a name="comments--todo"></a><a name="17.5"></a>
- [18.6](#comments--todo) Sử dụng `// TODO:` để chú giải các cách giải quyết cho các vấn đề.

  ``` javascript
  class Calculator extends Abacus {
    constructor() {
      super();

      // TODO: total should be configurable by an options param
      this.total = 0;
    }
  }
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="whitespace">Whitespace</a>

<a name="whitespace--spaces"></a><a name="18.1"></a>
- [19.1](#whitespace--spaces) Sử dụng các tab ngắn (dấu cách) đặt về 2 dấu cách. eslint: [`indent`](https://eslint.org/docs/rules/indent.html)

  ``` javascript
  // bad
  function foo() {
  ∙∙∙∙let name;
  }

  // bad
  function bar() {
  ∙let name;
  }

  // good
  function baz() {
  ∙∙let name;
  }
  ```

<a name="whitespace--before-blocks"></a><a name="18.2"></a>
- [19.2](#whitespace--before-blocks) Đặt 1 cách trước dấu mở ngoặc. eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks.html)

  ``` javascript
  // bad
  function test(){
    console.log('test');
  }

  // good
  function test() {
    console.log('test');
  }

  // bad
  dog.set('attr',{
    age: '1 year',
    breed: 'Bernese Mountain Dog',
  });

  // good
  dog.set('attr', {
    age: '1 year',
    breed: 'Bernese Mountain Dog',
  });
  ```

<a name="whitespace--around-keywords"></a><a name="18.3"></a>
- [19.3](#whitespace--around-keywords) Đặt 1 dấu cách trước dấu mở ngoặc tròn của các lệnh điều khiển (`if`, `while`, v.v.). Không đặt dấu cách giữa danh sách đối số và tên hàm trong các phép gọi và khai báo hàm. eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html)

  ``` javascript
  // bad
  if(isJedi) {
    fight ();
  }

  // good
  if (isJedi) {
    fight();
  }

  // bad
  function fight () {
    console.log ('Swooosh!');
  }

  // good
  function fight() {
    console.log('Swooosh!');
  }
  ```

<a name="whitespace--infix-ops"></a><a name="18.4"></a>
- [19.4](#whitespace--infix-ops) Đặt dấu cách trước và sau các toán tử. eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops.html)

  ``` javascript
  // bad
  const x=y+5;

  // good
  const x = y + 5;
  ```

<a name="whitespace--newline-at-end"></a><a name="18.5"></a>
- [19.5](#whitespace--newline-at-end) Kết thúc tệp với một dấu ngắt dòng. eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

  ``` javascript
  // bad
  import { es6 } from './AirbnbStyleGuide';
    // ...
  export default es6;
  ```

  ``` javascript
  // bad
  import { es6 } from './AirbnbStyleGuide';
    // ...
  export default es6;↵
  ↵
  ```

  ``` javascript
  // good
  import { es6 } from './AirbnbStyleGuide';
    // ...
  export default es6;↵
  ```
<a name="whitespace--after-blocks"></a><a name="18.7"></a>
- [19.6](#whitespace--after-blocks) Để một dòng trống sau mỗi khối và trước câu lệnh tiếp theo.

  ``` javascript
  // bad
  if (foo) {
    return bar;
  }
  return baz;

  // good
  if (foo) {
    return bar;
  }

  return baz;

  // bad
  const obj = {
    foo() {
    },
    bar() {
    },
  };
  return obj;

  // good
  const obj = {
    foo() {
    },

    bar() {
    },
  };

  return obj;

  // bad
  const arr = [
    function foo() {
    },
    function bar() {
    },
  ];
  return arr;

  // good
  const arr = [
    function foo() {
    },

    function bar() {
    },
  ];

  return arr;
  ```

<a name="whitespace--padded-blocks"></a><a name="18.8"></a>
- [19.7](#whitespace--padded-blocks) Không kê các khối với các dòng trống. eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks.html)

  ``` javascript
  // bad
  function bar() {

    console.log(foo);

  }

  // bad
  if (baz) {

    console.log(qux);
  } else {
    console.log(foo);

  }

  // bad
  class Foo {

    constructor(bar) {
      this.bar = bar;
    }
  }

  // good
  function bar() {
    console.log(foo);
  }

  // good
  if (baz) {
    console.log(qux);
  } else {
    console.log(foo);
  }
  ```

<a name="whitespace--no-multiple-blanks"></a>
- [19.8](#whitespace--no-multiple-blanks) Do not use multiple blank lines to pad your code. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

  <!-- markdownlint-disable MD012 -->
  ``` javascript
  // bad
  class Person {
    constructor(fullName, email, birthday) {
      this.fullName = fullName;


      this.email = email;


      this.setAge(birthday);
    }


    setAge(birthday) {
      const today = new Date();


      const age = this.getAge(today, birthday);


      this.age = age;
    }


    getAge(today, birthday) {
      // ..
    }
  }

  // good
  class Person {
    constructor(fullName, email, birthday) {
      this.fullName = fullName;
      this.email = email;
      this.setAge(birthday);
    }

    setAge(birthday) {
      const today = new Date();
      const age = getAge(today, birthday);
      this.age = age;
    }

    getAge(today, birthday) {
      // ..
    }
  }
  ```

<a name="whitespace--in-parens"></a><a name="18.9"></a>
- [19.9](#whitespace--in-parens) Không thêm các dấu cách trong dấu ngoặc tròn. eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens.html)

  ``` javascript
  // bad
  function bar( foo ) {
    return foo;
  }

  // good
  function bar(foo) {
    return foo;
  }

  // bad
  if ( foo ) {
    console.log(foo);
  }

  // good
  if (foo) {
    console.log(foo);
  }
  ```

<a name="whitespace--in-brackets"></a><a name="18.10"></a>
- [19.10](#whitespace--in-brackets) Không thêm các dấu cách trong các dấu ngoặc vuông. eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing.html)

  ``` javascript
  // bad
  const foo = [ 1, 2, 3 ];
  console.log(foo[ 0 ]);

  // good
  const foo = [1, 2, 3];
  console.log(foo[0]);
  ```

<a name="whitespace--in-braces"></a><a name="18.11"></a>
- [19.11](#whitespace--in-braces) Thêm các dấu cách giữa các dấu ngoặc nhọn. eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing.html)

  ``` javascript
  // bad
  const foo = {clark: 'kent'};

  // good
  const foo = { clark: 'kent' };
  ```

<a name="whitespace--max-len"></a><a name="18.12"></a>
- [19.12](#whitespace--max-len) Tránh các dòng mã có nhiều hơn 100 ký tự (kể cả khoảng trắng). Lưu ý: theo như [trên đây](#strings--line-length), các chuỗi được loại trừ bởi quy tắc này, và bạn không nên chia chúng ra. eslint: [`max-len`](https://eslint.org/docs/rules/max-len.html)

  > Tại sao? Điều này đảm bảo tính khả đọc và khả năng bảo trì.

  ``` javascript
  // bad
  const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

  // bad
  $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Chúc mừng!')).fail(() => console.log('You have failed this city.'));

  // good
  const foo = jsonData
    && jsonData.foo
    && jsonData.foo.bar
    && jsonData.foo.bar.baz
    && jsonData.foo.bar.baz.quux
    && jsonData.foo.bar.baz.quux.xyzzy;

  // good
  $.ajax({
    method: 'POST',
    url: 'https://airbnb.com/',
    data: { name: 'John' },
  })
    .done(() => console.log('Chúc mừng!'))
    .fail(() => console.log('You have failed this city.'));
  ```

<a name="whitespace--block-spacing"></a>
- [19.13](#whitespace--block-spacing) Đảm bảo sự nhất quán về dấu cách sau dấu mở ngoặc và trước ký tự đầu tiên sau nó trên cùng một dòng. Quy tắc này cũng yêu cầu sự nhất quán về dấu cách trước dấu đóng ngoặc và sau ký tự cuối cùng trước nó trên cùng một dòng. eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

  ``` javascript
  // bad
  function foo() {return true;}
  if (foo) { bar = 0;}

  // good
  function foo() { return true; }
  if (foo) { bar = 0; }
  ```

<a name="whitespace--comma-spacing"></a>
- [19.14](#whitespace--comma-spacing) Không sử dụng dấu cách trước dấu phẩy và phải sử dụng dấu cách sau dấu phẩy. eslint: [`comma-spacing`](https://eslint.org/docs/rules/comma-spacing)

  ``` javascript
  // bad
  const foo = 1,bar = 2;
  const arr = [1 , 2];

  // good
  const foo = 1, bar = 2;
  const arr = [1, 2];
  ```

<a name="whitespace--computed-property-spacing"></a>
- [19.15](#whitespace--computed-property-spacing) Không đặt dấu cách bên trong dấu ngoặc của thuộc tính được tính. eslint: [`computed-property-spacing`](https://eslint.org/docs/rules/computed-property-spacing)

  ``` javascript
  // bad
  obj[foo ]
  obj[ 'foo']
  const x = {[ b ]: a}
  obj[foo[ bar ]]

  // good
  obj[foo]
  obj['foo']
  const x = { [b]: a }
  obj[foo[bar]]
  ```

<a name="whitespace--func-call-spacing"></a>
- [19.16](#whitespace--func-call-spacing) Tránh sử dụng dấu cách giữa các hàm và phép gọi chúng. eslint: [`func-call-spacing`](https://eslint.org/docs/rules/func-call-spacing)

  ``` javascript
  // bad
  func ();

  func
  ();

  // good
  func();
  ```

<a name="whitespace--key-spacing"></a>
- [19.17](#whitespace--key-spacing) Đặt dấu cách giữa các tên và giá trị của các thuộc tính nguyên văn. eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

  ``` javascript
  // bad
  const obj = { foo : 42 };
  const obj2 = { foo:42 };

  // good
  const obj = { foo: 42 };
  ```

<a name="whitespace--no-trailing-spaces"></a>
- [19.18](#whitespace--no-trailing-spaces) Tránh các dấu cách ở cuối các dòng. eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

<a name="whitespace--no-multiple-empty-lines"></a>
- [19.19](#whitespace--no-multiple-empty-lines) Tránh để nhiều dòng trống liên tiếp, chỉ để một dòng trống ở cuối tệp, và không để dòng trống ở đầu tệp. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

  <!-- markdownlint-disable MD012 -->
  ``` javascript
  // bad - multiple empty lines
  const x = 1;


  const y = 2;

  // bad - 2+ newlines at end of file
  const x = 1;
  const y = 2;


  // bad - 1+ newline(s) at beginning of file

  const x = 1;
  const y = 2;

  // good
  const x = 1;
  const y = 2;

  ```
  <!-- markdownlint-enable MD012 -->

**[⬆ về đầu trang](#table-of-contents)**

## <a name="commas">Commas</a>

<a name="commas--leading-trailing"></a><a name="19.1"></a>
- [20.1](#commas--leading-trailing) Các dấu phẩy ở đầu: **Đừng!** eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style.html)

  ``` javascript
  // bad
  const story = [
      once
    , upon
    , aTime
  ];

  // good
  const story = [
    once,
    upon,
    aTime,
  ];

  // bad
  const hero = {
      firstName: 'Ada'
    , lastName: 'Lovelace'
    , birthYear: 1815
    , superPower: 'computers'
  };

  // good
  const hero = {
    firstName: 'Ada',
    lastName: 'Lovelace',
    birthYear: 1815,
    superPower: 'computers',
  };
  ```

<a name="commas--dangling"></a><a name="19.2"></a>
- [20.2](#commas--dangling) Thêm một dấu phẩy ở cuối: **Đúng đó!** eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle.html)

  > Tại sao? Điều này làm cho các so sánh git gọn gàng hơn. Ngoài ra, các trình dịch mã như Babel sẽ xóa các dấu phẩy ở cuối trong mã được dịch, có nghĩa là bạn không cần lo lắng về [vấn đề của dấu phẩy ở cuối](https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas) trên các trình duyệt cũ.

  ```diff
  // bad - git diff without trailing comma
  const hero = {
       firstName: 'Florence',
  -    lastName: 'Nightingale'
  +    lastName: 'Nightingale',
  +    inventorOf: ['coxcomb chart', 'modern nursing']
  };

  // good - git diff with trailing comma
  const hero = {
       firstName: 'Florence',
       lastName: 'Nightingale',
  +    inventorOf: ['coxcomb chart', 'modern nursing'],
  };
  ```

  ```  javascript
  // bad
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully'
  };

  const heroes = [
    'Batman',
    'Superman'
  ];

  // good
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully',
  };

  const heroes = [
    'Batman',
    'Superman',
  ];

  // bad
  function createHero(
    firstName,
    lastName,
    inventorOf
  ) {
    // does nothing
  }

  // good
  function createHero(
    firstName,
    lastName,
    inventorOf,
  ) {
    // dose nothing
  }

  // good (note that a comma must not appear after a "rest" element)
  function createHero(
    firstName,
    lastName,
    inventorOf,
    ...heroArgs
  ) {
    // không làm gì cả
  }

  // bad
  createHero(
    firstName,
    lastName,
    inventorOf
  );

  // good
  createHero(
    firstName,
    lastName,
    inventorOf,
  );

  // good (lưu ý là không được đặt dấu phẩy sau phần từ "còn-lại")
  createHero(
    firstName,
    lastName,
    inventorOf,
    ...heroArgs
  );
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="semicolons">Semicolons</a>

<a name="semicolons--required"></a><a name="20.1"></a>
- [21.1](#semicolons--required) **Dĩ nhiên.** eslint: [`semi`](https://eslint.org/docs/rules/semi.html)

  > Tại sao? Khi JavaScript gặp một dấu ngắt dòng mà không có dấu chấm phẩy, nó sử dụng một bộ quy tắc gọi là [Quy tắc thêm dấu chấm phẩy tự động](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion) để xác định xem dấu ngắt dòng có phải là kết thúc của một câu lệnh hay không, và (như cái tên gợi ý) đặt một dấu chấn phẩy vào mã của bạn, trước dấu ngắt dòng, nếu nó cho rằng nên làm vậy. Quy tắc thêm dấu chấm phẩy tự động có một vài hành vi lập dị, và mã của bạn sẽ hỏng nếu JavaScript hiểu sai dấu ngắt dòng của bạn. Những quy tắc này ngày càng trở nên phức tạp khi các tính năng mới được bổ sung vào JavaScript. Việc kết thúc các câu lệnh một cách rõ ràng và thiết lập trình phân tích mã của bạn bắt các lỗi thiếu dấu phẩy sẽ giúp bạn tránh được các vấn đề.

  ``` javascript
  // bad - raises exception
  const luke = {}
  const leia = {}
  [luke, leia].forEach((jedi) => jedi.father = 'vader')

  // bad - raises exception
  const reaction = "Không! Không thể nào!"
  (async function meanwhileOnTheFalcon() {
    // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
    // ...
  }())

  // bad - eturns `undefined` instead of the value on the next line - always happens when `return` is on a line by itself because of ASI!
  function foo() {
    return
      'search your feelings, you know it to be foo'
  }

  // good
  const luke = {};
  const leia = {};
  [luke, leia].forEach((jedi) => {
    jedi.father = 'vader';
  });

  // good
  const reaction = "Không! Không thể nào!";
  (async function meanwhileOnTheFalcon() {
    // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
    // ...
  }());

  // good
  function foo() {
    return 'search your feelings, you know it to be foo';
  }
  ```

  [Đọc thêm](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214).

**[⬆ về đầu trang](#table-of-contents)**

## <a name="type-casting--coercion">Type Casting & Coercion</a>

<a name="coercion--explicit"></a><a name="21.1"></a>
- [22.1](#coercion--explicit) Thực hiện ép kiểu ở đầu mỗi câu lệnh.

<a name="coercion--strings"></a><a name="21.2"></a>
- [22.2](#coercion--strings) Đối với các chuỗi: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

  ``` javascript
  // => this.reviewScore = 9;

  // bad
  const totalScore = new String(this.reviewScore); // typeof totalScore is "object" not "string"

  // bad
  const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

  // bad
  const totalScore = this.reviewScore.toString(); // isn’t guaranteed to return a string

  // good
  const totalScore = String(this.reviewScore);
  ```

<a name="coercion--numbers"></a><a name="21.3"></a>
- [22.3](#coercion--numbers) Đối với các số: Sử dụng `Number` để ép kiểu và `parseInt` luôn phải được dùng với một cơ số. eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

> Tại sao? Hàm `parseInt` sinh ra một số nguyên bằng cách diễn giải nội dung của một chuỗi dựa trên một cơ số đã định. Ký tự trống ở đầu chuỗi được bỏ qua. Nếu cơ số là `undefined` hoặc `0`, cơ số đó được ngầm định là `10`, trừ trường hợp số trong chuỗi bắt đầu bằng cặp ký tự `0x` hoặc `0X`, khi đó cơ số `16` được sử dụng. Điều này khác với ECMAScript 3, khi nó chỉ không khuyến khích (nhưng cho phép) sử dụng diễn giải số theo hệ bát phân. Nhiều trình duyệt chưa áp dụng theo điều trên kể từ 2013. Và, vì những trình duyệt cũ cũng cần được hỗ trợ, hãy luôn sử dụng một cơ số.

    ``` javascript
    const inputValue = '4';

    // bad
    const val = new Number(inputValue);

    // bad
    const val = +inputValue;

    // bad
    const val = inputValue >> 0;

    // bad
    const val = parseInt(inputValue);

    // good
    const val = Number(inputValue);

    // good
    const val = parseInt(inputValue, 10);
    ```

<a name="coercion--comment-deviations"></a><a name="21.4"></a>
- [22.4](#coercion--comment-deviations) Nếu, vì bất cứ lý do gì, bạn đang làm một điều gì đó thật điên và bạn gặp nghẽn cổ chai do `parseInt`, và bạn cần sử dụng phép dịch chuyển bit vì [các lý do hiệu suất](https://jsperf.com/coercion-vs-casting/3), nhớ để lại một chú thích để giải thích về thứ bạn đang làm và tại sao bạn làm vậy.

  ``` javascript
  // good
  /**
   * parseInt was the reason my code was slow.
   * Bitshifting the String to coerce it to a
   * Number made it a lot faster.
   */
  const val = inputValue >> 0;
  ```

<a name="coercion--bitwise"></a><a name="21.5"></a>
- [22.5](#coercion--bitwise) **Lưu ý:** Cẩn thận khi sử dụng các phép dịch chuyển bit. Các số được biểu diễn dưới dạng các [giá trị 64-bit](https://es5.github.io/#x4.3.19), nhưng các phép dịch chuyển bit luôn trả về một số nguyên 32-bit ([nguồn](https://es5.github.io/#x11.7)). Phép dịch chuyển bit cũng có thể dẫn đến các hành vi không mong đợi đối với các giá trị lớn hơn 32 bit. [Cuộc thảo luận](https://github.com/airbnb/javascript/issues/109). Số nguyên có dấu 32-bit lớn nhất là 2,147,483,647:

  ``` javascript
  2147483647 >> 0; // => 2147483647
  2147483648 >> 0; // => -2147483648
  2147483649 >> 0; // => -2147483647
  ```

<a name="coercion--booleans"></a><a name="21.6"></a>
- [22.6](#coercion--booleans) Đối với các boolean: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

  ``` javascript
  const age = 0;

  // bad
  const hasAge = new Boolean(age);

  // good
  const hasAge = Boolean(age);

  // best
  const hasAge = !!age;
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="naming-conventions">Naming Conventions</a>

<a name="naming--descriptive"></a><a name="22.1"></a>
- [23.1](#naming--descriptive) Tránh sử dụng các tên chỉ có một chữ cái. Hãy đặt những cái tên thật ý nghĩa. eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

  ``` javascript
  // bad
  function q() {
    // ...
  }

  // good
  function query() {
    // ...
  }
  ```

<a name="naming--camelCase"></a><a name="22.2"></a>
- [23.2](#naming--camelCase) Sử dụng camelCase khi đặt tên các đối tượng, các hàm và các thực thể. eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase.html)

  ``` javascript
  // bad
  const OBJEcttsssss = {};
  const this_is_my_object = {};
  function c() {}

  // good
  const thisIsMyObject = {};
  function thisIsMyFunction() {}
  ```

<a name="naming--PascalCase"></a><a name="22.3"></a>
- [23.3](#naming--PascalCase) Sử dụng PascalCase chỉ khi đặt tên các hàm tạo hay các lớp. eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html)

  ``` javascript
  // bad
  function user(options) {
    this.name = options.name;
  }

  const bad = new user({
    name: 'nope',
  });

  // good
  class User {
    constructor(options) {
      this.name = options.name;
    }
  }

  const good = new User({
    name: 'yup',
  });
  ```

<a name="naming--leading-underscore"></a><a name="22.4"></a>
- [23.4](#naming--leading-underscore) Không sử dụng các dấu gạch dưới ở đằng trước hoặc đằng sau. eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html)

  > Tại sao? JavaScript không có khái niệm về tính riêng tư khi nói đến các thuộc tính hay các phương thức. Tuy rằng một dấu gạch dưới đặt ở đằng trước là một quy ước chung mang nghĩa “riêng tư”, thực tế, các thuộc tính trên đều hoàn toàn công khai, và vì vậy, là các thành phần trong API của bạn. Quy ước này có thể khiến các nhà phát triển nghĩ, một cách sai lầm, rằng một sự thay đổi chẳng làm hỏng điều gì, hoặc không cần thiết phải kiểm thử. tl;dr: nếu bạn muốn thứ gì đó thật “riêng tư”, sự tồn tại của nó phải được giấu đi.

  ``` javascript
  // bad
  this.__firstName__ = 'Panda';
  this.firstName_ = 'Panda';
  this._firstName = 'Panda';

  // good
  this.firstName = 'Panda';

  // good, in environments where WeakMaps are available
  // xem https://kangax.github.io/compat-table/es6/#test-WeakMap
  const firstNames = new WeakMap();
  firstNames.set(this, 'Panda');
  ```

<a name="naming--self-this"></a><a name="22.5"></a>
- [23.5](#naming--self-this) Đừng lưu các tham chiếu đến `this`. Hãy sử dụng hàm mũi tên hoặc [Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

  ``` javascript
  // bad
  function foo() {
    const self = this;
    return function () {
      console.log(self);
    };
  }

  // bad
  function foo() {
    const that = this;
    return function () {
      console.log(that);
    };
  }

  // good
  function foo() {
    return () => {
      console.log(this);
    };
  }
  ```

<a name="naming--filename-matches-export"></a><a name="22.6"></a>
- [23.6](#naming--filename-matches-export) Phần tên của một tên tệp nên giống với địa chỉ xuất mặc định của tệp đó.

  ```javascript
    // file 1 contents
   class CheckBox {
     // ...
   }
   export default CheckBox;
   // file 2 contents
   export default function fortyTwo() { return 42; }
   // file 3 contents
   export default function insideDirectory() {}
   // in some other file
   // bad
   import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
   import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
   import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export
   // bad
   import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
   import forty_two from './forty_two'; // snake_case import/filename, camelCase export
   import inside_directory from './inside_directory'; // snake_case import, camelCase export
   import index from './inside_directory/index'; // requiring the index file explicitly
   import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly
   // good
   import CheckBox from './CheckBox'; // PascalCase export/import/filename
   import fortyTwo from './fortyTwo'; // camelCase export/import/filename
   import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
   // ^ supports both insideDirectory.js and insideDirectory/index.js
    ```

<a name="naming--camelCase-default-export"></a><a name="22.7"></a>
- [23.7](#naming--camelCase-default-export) Sử dụng camelCase khi xuất mặc định một hàm. Tên tệp nên trùng với tên hàm.

  ``` javascript
  function makeStyleGuide() {
    // ...
  }

  export default makeStyleGuide;
  ```

<a name="naming--PascalCase-singleton"></a><a name="22.8"></a>
- [23.8](#naming--PascalCase-singleton) Sử dụng PascalCase khi xuất mặc định một hàm tạo / lớp / đối tượng độc nhật / một thư viện các hàm / đối tượng trần.

  ``` javascript
  const AirbnbStyleGuide = {
    es6: {
    },
  };

  export default AirbnbStyleGuide;
  ```

<a name="naming--Acronyms-and-Initialisms"></a>
- [23.9](#naming--Acronyms-and-Initialisms) Các từ viết tắt nên được viết hoa hoặc viết thường toàn bộ.

  > Tại sao? Các tên dùng để đọc, không phải để giải thích thuật toán.

  ``` javascript
  // bad
  import SmsContainer from './containers/SmsContainer';

  // bad
  const HttpRequests = [
    // ...
  ];

  // good
  import SMSContainer from './containers/SMSContainer';

  // good
  const HTTPRequests = [
    // ...
  ];

  // good
  const httpRequests = [
    // ...
  ];

  // best
  import TextMessageContainer from './containers/TextMessageContainer';

  // best
  const requests = [
    // ...
  ];
  ```

<a name="naming--uppercase"></a>
- [23.10](#naming--uppercase) Bạn có chọn viết hoa một hằng chỉ khi hằng đó (1) được xuất, và (2) một lập trình viên có thể tin tưởng rằng nó (và các thuộc tính của nó) là bất biến.

  > Tại sao? Đây là một công cụ khác để hỗ trợ chúng ta trong các hoàn cảnh mà một lập trình viên có thể không chắc chắn là một biến có bị thay đổi hay chưa. UPPERCASE_VARIABLES đang cho lập trình viên đó biết là biến này (và thuộc tính của nó) không có thay đổi gì hết.
    - Thế còn các `const`? - Điều này là không cần thiết, vì việc viết hoa không nên được sử dụng cho các hằng ở trong cùng một tệp. Nó chỉ nên dùng cho các hằng được xuất.
    - Thế còn một đối tượng được xuất thì sao? - Chỉ viết hoa ở hàng cao nhất của đối tượng xuất (kiểu như: `EXPORTED_OBJECT.key`) và đảm bảo rằng những thuộc tính của nó không thay đổi.

  ``` javascript
  // bad
  const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

  // bad
  export const THING_TO_BE_CHANGED = 'should obviously not be uppercased';

  // bad
  export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables';

  // ---

  // allowed but does not supply semantic value
  export const apiKey = 'SOMEKEY';

  // better in most cases
  export const API_KEY = 'SOMEKEY';

  // ---

  // bad - unnecessarily uppercases key while adding no semantic value
  export const MAPPING = {
    KEY: 'value'
  };

  // good
  export const MAPPING = {
    key: 'value'
  };
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="accessors">Accessors</a>

<a name="accessors--not-required"></a><a name="23.1"></a>
- [24.1](#accessors--not-required) Các hàm truy cập cho các thuộc tình là không bắt buộc.

<a name="accessors--no-getters-setters"></a><a name="23.2"></a>
- [24.2](#accessors--no-getters-setters) Không sử dụng hàm đọc/hàm ghi của JavaScript vì chúng gây ra các hiệu ứng phụ không mong muốn và khó để kiểm thử, bảo trì, và hình dung. Thay vào đó, nếu bạn muốn tạo hàm truy cập, sử dụng `getVal()` và `setVal('hello')`.

  ``` javascript
  // bad
  class Dragon {
    get age() {
      // ...
    }

    set age(value) {
      // ...
    }
  }

  // good
  class Dragon {
    getAge() {
      // ...
    }

    setAge(value) {
      // ...
    }
  }
  ```

<a name="accessors--boolean-prefix"></a><a name="23.3"></a>
- [24.3](#accessors--boolean-prefix) Nếu thuộc tính/phương thức là một `boolean`, dùng `isVal()` hoặc `hasVal()`.

  ``` javascript
  // bad
  if (!dragon.age()) {
    return false;
  }

  // good
  if (!dragon.hasAge()) {
    return false;
  }
  ```

<a name="accessors--consistent"></a><a name="23.4"></a>
- [24.4](#accessors--consistent) Có thể dùng các hàm `get()` và `set()`, nhưng nhớ là phải nhất quán.

  ``` javascript
  class Jedi {
    constructor(options = {}) {
      const lightsaber = options.lightsaber || 'xanh dương';
      this.set('lightsaber', lightsaber);
    }

    set(key, val) {
      this[key] = val;
    }

    get(key) {
      return this[key];
    }
  }
  ```

**[⬆ về đầu trang](#table-of-contents)**

## <a name="standard-library">Standard Library</a>

[Thư viện tiêu chuẩn](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects)
chứa các hàm tiện ích không hoạt động đúng lắm nhưng vẫn tồn tại vì các lý do cũ.

<a name="standard-library--isnan"></a>
- [29.1](#standard-library--isnan) Sử dụng `Number.isNaN` thay vì hàm toàn cục `isNaN`.
  eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

  > Tại sao? Hàm toàn cục `isNaN` ép các giá trị không phải số thành số và trả về true cho tất cả những gì mà bị ép thành NaN.
  > Nếu đây là hành vi mong muốn, hãy khiến nó được biểu đạt rõ ràng.

  ``` javascript
  // bad
  isNaN('1.2'); // false
  isNaN('1.2.3'); // true

  // good
  Number.isNaN('1.2.3'); // false
  Number.isNaN(Number('1.2.3')); // true
  ```

<a name="standard-library--isfinite"></a>
- [29.2](#standard-library--isfinite) Sử dụng `Number.isFinite` thay vì hàm toàn cục `isFinite`.
  eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

  > Tại sao? Hàm toàn cục `isFinite` ép các giá trị không phải số thành số và trả về true cho tất cả những gì bị ép thành một số mà không phải là vô hạn.
  > Nếu đây là hành vi mong muốn, hãy khiến nó được biểu đạt rõ ràng.

  ``` javascript
  // bad
  isFinite('2e3'); // true

  // good
  Number.isFinite('2e3'); // false
  Number.isFinite(parseInt('2e3', 10)); // true
  ```

**[⬆ về đầu trang](#table-of-contents)**
