# **Chương 10. Kiểu tổng quát (Generics)**

## Mục lục

- [**Chương 10. Kiểu tổng quát (Generics)**](#chương-10-kiểu-tổng-quát-generics)
  - [**Các hàm generic (Generic Functions)**](#các-hàm-generic-generic-functions)
      - [**CẢNH BÁO (WARNING)**](#cảnh-báo-warning)
    - [**Các kiểu gọi generic tường minh (Explicit Generic Call Types)**](#các-kiểu-gọi-generic-tường-minh-explicit-generic-call-types)
    - [**Nhiều tham số kiểu trong hàm (Multiple Function Type Parameters)**](#nhiều-tham-số-kiểu-trong-hàm-multiple-function-type-parameters)
      - [**MẸO (TIP)**](#mẹo-tip)
  - [**Giao diện generic (Generic Interfaces)**](#giao-diện-generic-generic-interfaces)
    - [**Suy luận kiểu giao diện generic (Inferred Generic Interface Types)**](#suy-luận-kiểu-giao-diện-generic-inferred-generic-interface-types)
  - [**Lớp generic (Generic Classes)**](#lớp-generic-generic-classes)
    - [**Các kiểu lớp generic tường minh (Explicit Generic Class Types)**](#các-kiểu-lớp-generic-tường-minh-explicit-generic-class-types)
    - [**Kế thừa các lớp generic (Extending Generic Classes)**](#kế-thừa-các-lớp-generic-extending-generic-classes)
    - [**Triển khai các giao diện generic (Implementing Generic Interfaces)**](#triển-khai-các-giao-diện-generic-implementing-generic-interfaces)
    - [**Phương thức generic (Method Generics)**](#phương-thức-generic-method-generics)
    - [**Generics trên thành viên tĩnh (Static Class Generics)**](#generics-trên-thành-viên-tĩnh-static-class-generics)
  - [**Bí danh kiểu generic (Generic Type Aliases)**](#bí-danh-kiểu-generic-generic-type-aliases)
    - [**Discriminated Unions generic (Generic Discriminated Unions)**](#discriminated-unions-generic-generic-discriminated-unions)
  - [**Các bổ từ generic (Generic Modifiers)**](#các-bổ-từ-generic-generic-modifiers)
    - [**Giá trị mặc định cho Generic (Generic Defaults)**](#giá-trị-mặc-định-cho-generic-generic-defaults)
  - [**Ràng buộc kiểu generic (Constrained Generic Types)**](#ràng-buộc-kiểu-generic-constrained-generic-types)
    - [**keyof và các tham số kiểu có ràng buộc (keyof and Constrained Type Parameters)**](#keyof-và-các-tham-số-kiểu-có-ràng-buộc-keyof-and-constrained-type-parameters)
  - [**Promises**](#promises)
    - [**Tạo Promises (Creating Promises)**](#tạo-promises-creating-promises)
    - [**Hàm Async (Async Functions)**](#hàm-async-async-functions)
  - [**Sử dụng Generics đúng cách (Using Generics Right)**](#sử-dụng-generics-đúng-cách-using-generics-right)
      - [**CẢNH BÁO (WARNING)**](#cảnh-báo-warning-1)
    - [**Quy tắc vàng của Generics (The Golden Rule of Generics)**](#quy-tắc-vàng-của-generics-the-golden-rule-of-generics)
    - [**Quy ước đặt tên cho Generic (Generic Naming Conventions)**](#quy-ước-đặt-tên-cho-generic-generic-naming-conventions)
  - [**Tổng kết**](#tổng-kết)
    - [**MẸO (TIP)**](#mẹo-tip-1)

_Các biến bạn khai báo trong hệ thống kiểu?_

_Một thế giới (được định kiểu) hoàn toàn mới!_

Tất cả các cú pháp kiểu bạn đã học cho đến nay đều được dự định sử dụng với các kiểu được biết hoàn toàn khi chúng đang được viết. Tuy nhiên, đôi khi một đoạn mã có thể được thiết kế để hoạt động với nhiều kiểu khác nhau tùy thuộc vào cách nó được gọi.

Hãy xem hàm `identity` này trong JavaScript nhằm mục đích nhận một đầu vào thuộc bất kỳ kiểu nào có thể và trả về chính đầu vào đó dưới dạng đầu ra. Bạn sẽ mô tả kiểu tham số và kiểu trả về của nó như thế nào?

```typescript
function identity(input) {
  return input;
}
identity("abc");
identity(123);
identity({ quote: "I think your self emerges more clearly over time." });
```

Chúng ta có thể khai báo `input` là `any`, nhưng khi đó kiểu trả về của hàm cũng sẽ là `any`:

```typescript
function identity(input: any) {
return input;
}
let value = identity(42);  // Type of value: any
```

Với việc `input` được phép là bất kỳ đầu vào nào, chúng ta cần một cách để nói rằng có một mối quan hệ giữa kiểu của `input` và kiểu mà hàm trả về. TypeScript nắm bắt các mối quan hệ giữa các kiểu bằng cách sử dụng _generics_ (kiểu tổng quát).

Trong TypeScript, các cấu trúc như hàm có thể khai báo bất kỳ số lượng _tham số kiểu_ (type parameters) generic nào: các kiểu được xác định cho mỗi lần sử dụng cấu trúc generic. Các tham số kiểu này được sử dụng làm kiểu trong cấu trúc để đại diện cho một kiểu nào đó có thể khác nhau trong mỗi thể hiện của cấu trúc. Các tham số kiểu có thể được cung cấp với các kiểu khác nhau, được gọi là các _đối số kiểu_ (type arguments), cho mỗi thể hiện của cấu trúc nhưng sẽ duy trì tính nhất quán bên trong thể hiện đó.

Các tham số kiểu thường có tên gồm một chữ cái như `T` và `U` hoặc tên dạng PascalCase như `Key` và `Value`. Trong tất cả các cấu trúc được đề cập trong chương này, generics có thể được khai báo bằng các dấu ngoặc `<` và `>`, chẳng hạn như `someFunction<T>` hoặc `SomeInterface<T>`.

## **Các hàm generic (Generic Functions)**

Một hàm có thể được chuyển thành generic bằng cách đặt một bí danh cho tham số kiểu, được bọc trong dấu ngoặc nhọn, ngay trước dấu ngoặc đơn của các tham số. Tham số kiểu đó sau đó sẽ có sẵn để sử dụng trong các chú thích kiểu tham số, chú thích kiểu trả về và các chú thích kiểu bên trong thân hàm.

Phiên bản sau đây của `identity` khai báo một tham số kiểu `T` cho tham số `input` của nó, cho phép TypeScript suy luận rằng kiểu trả về của hàm là `T`. TypeScript sau đó có thể suy luận một kiểu khác nhau cho `T` mỗi khi `identity` được gọi:

```typescript
function identity<T>(input: T) {
return input;
}

const numeric = identity("me");  // Type: "me"
const stringy = identity(123);  // Type: 123
```

Các arrow functions cũng có thể là generic. Các khai báo generic của chúng cũng được đặt ngay trước dấu `(` trước danh sách tham số của chúng.

Arrow function sau đây có chức năng giống hệt như khai báo trước đó:

```typescript
const identity=<T>(input: T) => input;

identity(123);  // Type: 123
```

#### **CẢNH BÁO (WARNING)**

Cú pháp cho các arrow functions generic có một số hạn chế trong các tệp _.tsx_, vì nó xung đột với cú pháp JSX. Xem Chương 13, “Các tùy chọn cấu hình” để biết các giải pháp thay thế cũng như cách cấu hình hỗ trợ JSX và React.

Việc thêm các tham số kiểu vào các hàm theo cách này cho phép chúng được tái sử dụng với các đầu vào khác nhau trong khi vẫn duy trì tính an toàn kiểu và tránh các kiểu `any`.

### **Các kiểu gọi generic tường minh (Explicit Generic Call Types)**

Hầu hết thời gian khi gọi các hàm generic, TypeScript sẽ có thể suy luận các type arguments dựa trên cách hàm đang được gọi. Ví dụ, trong các hàm `identity` của các ví dụ trước, bộ kiểm tra kiểu của TypeScript đã sử dụng một đối số được cung cấp cho `identity` để suy luận đối số kiểu của tham số hàm tương ứng.

Thật không may, cũng như với các thành viên của lớp và các kiểu biến, đôi khi không có đủ thông tin từ lời gọi hàm để thông báo cho TypeScript biết đối số kiểu của nó sẽ giải quyết thành gì. Điều này thường xảy ra nếu một cấu trúc generic được cung cấp một cấu trúc generic khác mà các đối số kiểu của nó chưa được biết.

TypeScript sẽ mặc định giả định kiểu `unknown` cho bất kỳ đối số kiểu nào mà nó không thể suy luận.

Ví dụ, hàm `logWrapper` sau đây nhận vào một callback với kiểu tham số được đặt thành tham số kiểu `Input` của `logWrapper`. TypeScript có thể suy luận đối số kiểu nếu `logWrapper` được gọi với một callback khai báo rõ ràng kiểu tham số của nó. Tuy nhiên, nếu kiểu tham số là ngầm định, TypeScript không có cách nào biết `Input` nên là gì:

```typescript
function logWrapper<Input>(callback: (input: Input) => void) {
return (input: Input) => {
console.log("Input: ", input);
callback(input);
    };
}
// Type: (input: string) => void
logWrapper((input: string) => {
console.log(input.length);
});
// Type: (input: unknown) => void
logWrapper((input) => {
console.log(input.length);
//                ~~~~~~
// Error: Property 'length' does not exist on type 'unknown'.
});
```

Để tránh việc mặc định về `unknown`, các hàm có thể được gọi với một đối số kiểu generic tường minh để nói rõ cho TypeScript biết đối số kiểu đó nên là gì thay thế. TypeScript sẽ thực hiện kiểm tra kiểu trên lời gọi generic để đảm bảo tham số được yêu cầu khớp với những gì được cung cấp làm đối số kiểu.

Ở đây, hàm `logWrapper` được thấy trước đó được cung cấp một `string` tường minh cho generic `Input` của nó. TypeScript sau đó có thể suy luận rằng tham số `input` của callback thuộc kiểu generic `Input` giải quyết thành kiểu `string`:

```typescript
// Type: (input: string) => void
logWrapper<string>((input) => {
console.log(input.length);
});
logWrapper<string>((input: boolean) => {
//             ~~~~~~~~~~~~~~~~~~~~~~~
// Argument of type '(input: boolean) => void' is not
// assignable to parameter of type '(input: string) => void'.
//   Types of parameters 'input' and 'input' are incompatible.

//     Type 'string' is not assignable to type 'boolean'.

});
```

Giống như các chú thích kiểu tường minh trên các biến, các đối số kiểu tường minh luôn có thể được chỉ định trên một hàm generic nhưng thường không cần thiết. Nhiều lập trình viên TypeScript thường chỉ chỉ định chúng khi cần thiết.

Cách sử dụng `logWrapper` sau đây chỉ định rõ ràng `string` cho cả đối số kiểu và kiểu tham số hàm. Cả hai đều có thể được lược bỏ bớt một:

```typescript
// Type: (input: string) => void
logWrapper<string>((input: string) => { /* ... */ });
```

Cú pháp `Name<Type>` để chỉ định một đối số kiểu sẽ giống nhau cho các cấu trúc generic khác xuyên suốt chương này.

### **Nhiều tham số kiểu trong hàm (Multiple Function Type Parameters)**

Các hàm có thể định nghĩa bất kỳ số lượng tham số kiểu nào, được phân tách bằng dấu phẩy. Mỗi lần gọi hàm generic có thể giải quyết tập hợp giá trị riêng của nó cho từng tham số kiểu.

Trong ví dụ này, `makeTuple` khai báo hai tham số kiểu và trả về một giá trị có kiểu là một tuple chỉ đọc với phần tử thứ nhất, sau đó là phần tử thứ hai:

```typescript
function makeTuple<First, Second>(first: First, second: Second) {
  return [first, second] as const;
}

let tuple = makeTuple(true, "abc");  // Type of value: readonly [boolean, string]
```

Lưu ý rằng nếu một hàm khai báo nhiều tham số kiểu, các lời gọi đến hàm đó phải khai báo rõ ràng hoặc không có kiểu generic nào hoặc tất cả các kiểu generic đó. TypeScript chưa hỗ trợ việc chỉ suy luận một số kiểu của một lời gọi generic.

Ở đây, `makePair` cũng nhận vào hai tham số kiểu, vì vậy hoặc không có kiểu nào trong số chúng hoặc cả hai đều phải được chỉ định rõ ràng:

```typescript
function makePair<Key, Value>(key: Key, value: Value) {
return { key, value };
}
// Ok: neither type argument provided
makePair("abc", 123);  // Type: { key: string; value: number }

// Ok: both type arguments provided
makePair<string, number>("abc", 123);  // Type: { key: string; value: number }
makePair<"abc", 123>("abc", 123);  // Type: { key: "abc"; value: 123 }

makePair<string>("abc", 123);
//       ~~~~~~
// Error: Expected 2 type arguments, but got 1.
```

#### **MẸO (TIP)**

Cố gắng không sử dụng quá một hoặc hai tham số kiểu trong bất kỳ cấu trúc generic nào. Cũng như với các tham số hàm thời gian chạy, bạn càng sử dụng nhiều thì mã càng khó đọc và khó hiểu.

## **Giao diện generic (Generic Interfaces)**

Các interface cũng có thể được khai báo là generic. Chúng tuân theo các quy tắc generic tương tự như các hàm: chúng có thể có bất kỳ số lượng tham số kiểu nào được khai báo giữa `<` và `>` sau tên của chúng. Kiểu generic đó sau này có thể được sử dụng ở nơi khác trong phần khai báo của chúng, chẳng hạn như trên các kiểu thành viên.

Khai báo `Box` sau đây có một tham số kiểu `T` cho một thuộc tính. Việc tạo một đối tượng được khai báo là một `Box` với một đối số kiểu sẽ thực thi rằng thuộc tính `inside: T` khớp với đối số kiểu đó:

```typescript
interface Box<T> {
inside: T;
}
let stringyBox: Box<string>= {
inside: "abc",
};
let numberBox: Box<number>= {
inside: 123,

}

let incorrectBox: Box<number>= {
inside: false,
// Error: Type 'boolean' is not assignable to type 'number'.
}
```

Một sự thật thú vị: các phương thức tích hợp sẵn của `Array` được định nghĩa trong TypeScript dưới dạng một generic interface! `Array` sử dụng một tham số kiểu `T` để đại diện cho kiểu dữ liệu được lưu trữ bên trong một mảng. Các phương thức `pop` và `push` của nó trông đại loại như sau:

```typescript
interface Array<T> {
// ...
/**
     * Removes the last element from an array and returns it.
     * If the array is empty, undefined is returned and the array is not
modified.
     */
pop(): T | undefined;
/**
     * Appends new elements to the end of an array,
     * and returns the new length of the array.
     * @param items new elements to add to the array.
     */
push(...items: T[]): number;
// ...
}
```

### **Suy luận kiểu giao diện generic (Inferred Generic Interface Types)**

Cũng như với các hàm generic, các đối số kiểu của generic interface có thể được suy luận từ cách sử dụng. TypeScript sẽ cố gắng hết sức để suy luận các đối số kiểu từ các kiểu giá trị được cung cấp cho một vị trí được khai báo là nhận vào một kiểu generic.

Hàm `getLast` này khai báo một tham số kiểu `Value` sau đó được sử dụng cho tham số `node` của nó. TypeScript sau đó có thể suy luận `Value` dựa trên kiểu của bất kỳ giá trị nào được truyền vào dưới dạng đối số. Nó thậm chí có thể báo cáo một lỗi kiểu khi một đối số kiểu được suy luận không khớp với kiểu của một giá trị. Việc cung cấp cho `getLast` một đối tượng không bao gồm `next`, hoặc có đối số kiểu `Value` được suy luận cùng kiểu, là được phép. Tuy nhiên, việc không khớp giữa `value` của đối tượng được cung cấp và `next.value` là một lỗi kiểu:

```typescript
interface LinkedNode<Value> {
next?: LinkedNode<Value>;
value: Value;
}
function getLast<Value>(node: LinkedNode<Value>): Value {
return node.next?getLast(node.next) :node.value;
}
// Inferred Value type argument: Date
let lastDate = getLast({
value: new Date("09-13-1993"),
});
// Inferred Value type argument: string
let lastFruit = getLast({
next: {
value: "banana",
    },
value: "apple",
});
// Inferred Value type argument: number
let lastMismatch = getLast({
next: {
value: 123
    },
value: false,
//  ~~~~~
// Error: type 'boolean' is not assignable to type 'number'.
});
```

Lưu ý rằng nếu một interface khai báo các tham số kiểu, bất kỳ chú thích kiểu nào tham chiếu đến interface đó đều phải cung cấp các đối số kiểu tương ứng. Ở đây, việc sử dụng `CrateLike` là không chính xác vì không đưa vào một đối số kiểu:

```typescript
interface CrateLike<T> {
contents: T;
}

let missingGeneric: CrateLike= {

//              ~~~~~~~~~
// Error: Generic type 'Crate<T>' requires 1 type argument(s).
inside: "??"
};
```

Sau này trong chương này, tôi sẽ chỉ ra cách cung cấp các giá trị mặc định cho các tham số kiểu để giải quyết yêu cầu này.

## **Lớp generic (Generic Classes)**

Các lớp, giống như interfaces, cũng có thể khai báo bất kỳ số lượng tham số kiểu nào để sau này được sử dụng trên các thành viên. Mỗi thể hiện của lớp có thể có một tập hợp các đối số kiểu khác nhau cho các tham số kiểu của nó.

Lớp `Secret` này khai báo các tham số kiểu `Key` và `Value`, sau đó sử dụng chúng cho các thuộc tính thành viên, kiểu tham số constructor, và kiểu tham số cùng kiểu trả về của một phương thức:

```typescript
class Secret<Key, Value> {
key: Key;
value: Value;
constructor(key: Key, value: Value) {
this.key = key;
this.value = value;
    }
getValue(key: Key): Value | undefined {
return this.key === key
?this.value
            : undefined;
    }
}

const storage = new Secret(12345, "luggage");  // Type: Secret<number, string>
storage.getValue(1987);  // Type: string | undefined
```

Cũng như với generic interfaces, các chú thích kiểu sử dụng một lớp phải chỉ ra cho TypeScript biết bất kỳ kiểu generic nào trên lớp đó là gì. Sau này trong chương này, tôi sẽ chỉ ra cách cung cấp các giá trị mặc định cho các tham số kiểu để giải quyết yêu cầu này cho các lớp nữa.

### **Các kiểu lớp generic tường minh (Explicit Generic Class Types)**

Việc khởi tạo các lớp generic tuân theo các quy tắc suy luận đối số kiểu giống như khi gọi các hàm generic. Nếu đối số kiểu có thể được suy luận từ kiểu của một tham số đối với constructor của lớp, chẳng hạn như `new Secret(12345, "luggage")` trước đó, TypeScript sẽ sử dụng kiểu được suy luận. Mặt khác, nếu một đối số kiểu của lớp không thể được suy luận từ các đối số được truyền vào constructor của nó, đối số kiểu sẽ mặc định là `unknown`.

Lớp `CurriedCallback` này khai báo một constructor nhận vào một hàm generic. Nếu hàm generic có kiểu đã biết—chẳng hạn như từ một chú thích kiểu đối số kiểu tường minh—thì đối số kiểu `Input` của thể hiện lớp có thể được xác định từ nó. Mặt khác, đối số kiểu `Input` của thể hiện lớp sẽ mặc định là `unknown`:

```typescript
class CurriedCallback<Input> {
#callback: (input: Input) => void;
constructor(callback: (input: Input) => void) {
this.#callback= (input: Input) => {
console.log("Input: ", input);
callback(input);
        };
    }
call(input: Input) {
this.#callback(input);
    }
}
// Type: CurriedCallback<string>
new CurriedCallback((input: string) => {
console.log(input.length);
});
// Type: CurriedCallback<unknown>
new CurriedCallback((input) => {
console.log(input.length);
//                ~~~~~~

// Error: Property 'length' does not exist on type 'unknown'.
});
```

Các thể hiện của lớp cũng có thể tránh việc mặc định về `unknown` bằng cách cung cấp (các) đối số kiểu tường minh giống như các lời gọi hàm generic khác.

Ở đây, `CurriedCallback` từ phần trước giờ đây được cung cấp một `string` tường minh cho đối số kiểu `Input` của nó, vì vậy TypeScript có thể suy luận rằng tham số kiểu `Input` của callback giải quyết thành `string`:

```typescript
// Type: CurriedCallback<string>
new CurriedCallback<string>((input) => {
console.log(input.length);
});
new CurriedCallback<string>((input: boolean) => {
//                       ~~~~~~~~~~~~~~~~~~~~~~
// Argument of type '(input: boolean) => void' is not
// assignable to parameter of type '(input: string) => void'.
//   Types of parameters 'input' and 'input' are incompatible.
//     Type 'string' is not assignable to type 'boolean'.
});
```

### **Kế thừa các lớp generic (Extending Generic Classes)**

Các lớp generic có thể được sử dụng làm lớp cơ sở theo sau từ khóa `extends`. TypeScript sẽ không cố gắng suy luận các đối số kiểu cho lớp cơ sở từ cách sử dụng. Bất kỳ đối số kiểu nào không có giá trị mặc định sẽ cần phải được chỉ định bằng một chú thích kiểu tường minh.

Lớp `SpokenQuote` sau đây cung cấp `string[]` làm đối số kiểu `T` cho lớp cơ sở `Quote<T>` của nó:

```typescript
class Quote<T> {
lines: T;
constructor(lines: T) {
this.lines = lines;
    }
}
class SpokenQuote extends Quote<string[]> {

speak() {
console.log(this.lines.join("
"));
    }
}
new Quote("The only real failure is the failure to try.").lines;  // Type:
string
new Quote([4, 8, 15, 16, 23, 42]).lines;  // Type: number[]
new SpokenQuote([
"Greed is so destructive.",
"It destroys everything",
]).lines;  // Type: string[]
new SpokenQuote([4, 8, 15, 16, 23, 42]);
//              ~~~~~~~~~~~~~~~~~~~~~~
// Error: Argument of type 'number' is not
// assignable to parameter of type 'string'.
```

Các lớp dẫn xuất generic có thể truyền đối số kiểu của chính chúng qua lớp cơ sở. Tên kiểu không nhất thiết phải trùng khớp; ví dụ, `AttributedQuote` này truyền một đối số kiểu `Value` có tên khác cho lớp cơ sở `Quote<T>`:

```typescript
class AttributedQuote<Value> extends Quote<Value> {
speaker: string
constructor(value: Value, speaker: string) {
super(value);
this.speaker = speaker;
    }
}
// Type: AttributedQuote<string>
// (extending Quote<string>)
new AttributedQuote(
"The road to success is always under construction.",
"Lily Tomlin",
);
```

### **Triển khai các giao diện generic (Implementing Generic Interfaces)**

Các lớp generic cũng có thể triển khai generic interfaces bằng cách cung cấp cho chúng bất kỳ tham số kiểu cần thiết nào. Điều này hoạt động tương tự như việc kế thừa một lớp cơ sở generic: bất kỳ tham số kiểu nào trên interface cơ sở đều phải được khai báo bởi lớp.

Ở đây, lớp `MoviePart` chỉ định đối số kiểu `Role` của interface `ActingCredit` là `string`. Lớp `IncorrectExtension` gây ra cảnh báo kiểu vì `role` của nó có kiểu `boolean` mặc dù nó cung cấp `string[]` làm đối số kiểu cho `ActingCredit`:

```typescript
interface ActingCredit<Role> {
role: Role;
}
class MoviePart implements ActingCredit<string> {
role: string;
speaking: boolean;
constructor(role: string, speaking: boolean) {
this.role = role;
this.speaking = speaking;
    }
}
const part = new MoviePart("Miranda Priestly", true);

part.role;  // Type: string
class IncorrectExtension implements ActingCredit<string> {
role: boolean;
//    ~~~~~~~
// Error: Property 'role' in type 'IncorrectExtension' is not
// assignable to the same property in base type 'ActingCredit<string>'.
//   Type 'boolean' is not assignable to type 'string'.
}
```

### **Phương thức generic (Method Generics)**

Các phương thức của lớp có thể khai báo các kiểu generic của riêng chúng tách biệt khỏi thể hiện lớp của chúng. Mỗi lời gọi đến một phương thức lớp generic có thể có một đối số kiểu khác nhau cho từng tham số kiểu của nó.

Lớp generic `CreatePairFactory` này khai báo một kiểu `Key` và bao gồm một phương thức `createPair` cũng khai báo một kiểu generic `Value` riêng biệt. Kiểu trả về cho `createPair` sau đó được suy luận là `{ key: Key, value: Value }`:

```typescript
class CreatePairFactory<Key> {
key: Key;
constructor(key: Key) {
this.key = key;
    }
createPair<Value>(value: Value) {
return { key: this.key, value };
    }
}
// Type: CreatePairFactory<string>
const factory = new CreatePairFactory("role");
// Type: { key: string, value: number }
const numberPair = factory.createPair(10);
// Type: { key: string, value: string }
const stringPair = factory.createPair("Sophie");
```

### **Generics trên thành viên tĩnh (Static Class Generics)**

Các thành viên tĩnh của một lớp tách biệt khỏi các thành viên thể hiện và không được liên kết với bất kỳ thể hiện cụ thể nào của lớp. Chúng không có quyền truy cập vào bất kỳ thể hiện lớp nào hoặc thông tin kiểu cụ thể cho bất kỳ thể hiện lớp nào. Do đó, trong khi các phương thức lớp tĩnh có thể khai báo các tham số kiểu của riêng chúng, chúng không thể truy cập bất kỳ tham số kiểu nào được khai báo trên một lớp.

Ở đây, một lớp `BothLogger` khai báo một tham số kiểu `OnInstance` cho phương thức `instanceLog` của nó và một tham số kiểu `OnStatic` riêng biệt cho phương thức tĩnh `staticLog` của nó. Phương thức tĩnh không thể truy cập `OnInstance` của thể hiện vì `OnInstance` được khai báo cho các thể hiện của lớp:

```typescript
class BothLogger<OnInstance> {
instanceLog(value: OnInstance) {
console.log(value);
return value;
    }

static staticLog<OnStatic>(value: OnStatic) {
let fromInstance: OnInstance;
//                ~~~~~~~~~~
// Error: Static members cannot reference class type arguments.
console.log(value);
return value;
    }
}
const logger = new BothLogger<number[]>;
logger.instanceLog([1, 2, 3]);  // Type: number[]
// Inferred OnStatic type argument: boolean[]
BothLogger.staticLog([false, true]);

// Explicit OnStatic type argument: string
BothLogger.staticLog<string>("You can't change the music of your soul.");
```

## **Bí danh kiểu generic (Generic Type Aliases)**

Một cấu trúc cuối cùng trong TypeScript có thể được chuyển thành generic với các đối số kiểu là type aliases. Mỗi type alias có thể được cung cấp bất kỳ số lượng tham số kiểu nào, chẳng hạn như kiểu `Nullish` này nhận vào một `T`:

```typescript
type Nullish<T>=T | null | undefined;
```

Các generic type aliases thường được sử dụng với các hàm để mô tả kiểu của một hàm generic:

```typescript
type CreatesValue<Input, Output> = (input: Input) => Output;

// Type: (input: string) => number
let creator: CreatesValue<string, number>;
creator = text => text.length;  // Ok
creator = text => text.toUpperCase();
//                ~~~~~~~~~~~~~~~~~~
// Error: Type 'string' is not assignable to type 'number'.
```

### **Discriminated Unions generic (Generic Discriminated Unions)**

Tôi đã đề cập trong Chương 4, “Đối tượng” rằng discriminated unions là tính năng yêu thích nhất của tôi trong toàn bộ TypeScript vì chúng kết hợp một cách tuyệt đẹp một mô hình JavaScript thanh lịch phổ biến với khả năng thu hẹp kiểu của TypeScript. Cách sử dụng yêu thích nhất của tôi cho discriminated unions là thêm một đối số kiểu để tạo ra một kiểu “result” generic đại diện cho kết quả thành công với dữ liệu hoặc thất bại với một lỗi.

Kiểu generic `Result` này có một discriminant `succeeded` bắt buộc phải được sử dụng để thu hẹp kết quả xem nó là thành công hay thất bại. Điều này có nghĩa là bất kỳ thao tác nào trả về một `Result` đều có thể chỉ ra kết quả lỗi hoặc dữ liệu, và được đảm bảo rằng người sử dụng sẽ cần phải kiểm tra xem kết quả có thành công hay không:

```typescript
type Result<Data>=FailureResult | SuccessfulResult<Data>;
interface FailureResult {
error: Error;
succeeded: false;
}
interface SuccessfulResult<Data> {
data: Data;
succeeded: true;
}
function handleResult(result: Result<string>) {
if (result.succeeded) {
// Type of result: SuccessfulResult<string>
console.log(`We did it! ${result.data}`);
    } else {
// Type of result: FailureResult
console.error(`Awww... ${result.error}`);
    }
result.data;
//     ~~~~
// Error: Property 'data' does not exist on type 'Result<string>'.
//   Property 'data' does not exist on type 'FailureResult'.
}
```

Kết hợp lại với nhau, các kiểu generic và kiểu discriminated cung cấp một cách tuyệt vời để mô hình hóa các kiểu có thể tái sử dụng như `Result`.

## **Các bổ từ generic (Generic Modifiers)**

TypeScript bao gồm cú pháp cho phép bạn sửa đổi hành vi của các tham số kiểu generic.

### **Giá trị mặc định cho Generic (Generic Defaults)**

Cho đến nay tôi đã nói rằng nếu một kiểu generic được sử dụng trong một chú thích kiểu hoặc làm cơ sở cho một lớp `extends` hoặc `implements`, nó phải cung cấp một đối số kiểu cho mỗi tham số kiểu. Bạn có thể tránh việc phải cung cấp các đối số kiểu một cách tường minh bằng cách đặt dấu `=` theo sau bởi một kiểu mặc định sau khai báo của tham số kiểu. Giá trị mặc định sẽ được sử dụng trong bất kỳ kiểu tiếp theo nào mà đối số kiểu không được khai báo rõ ràng và không thể suy luận được. Ở đây, interface `Quote` nhận vào một tham số kiểu `T` mặc định là `string` nếu không được cung cấp. Biến `explicit` thiết lập rõ ràng `T` thành `number` trong khi `implicit` và `mismatch` đều giải quyết thành `string`:

```typescript
interface Quote<T = string> {
value: T;
}
let explicit: Quote<number>= { value: 123 };
let implicit: Quote= { value: "Be yourself. The world worships the original."
};
let mismatch: Quote= { value: 123 };
//                                     ~~~
// Error: Type 'number' is not assignable to type 'string'.
```

Các tham số kiểu cũng có thể mặc định về các tham số kiểu đứng trước trong cùng một khai báo. Vì mỗi tham số kiểu giới thiệu một kiểu mới cho khai báo, chúng có sẵn làm giá trị mặc định cho các tham số kiểu sau trong khai báo đó.

Kiểu `KeyValuePair` này có thể có các kiểu khác nhau cho các generics `Key` và `Value` của nó nhưng mặc định giữ chúng giống nhau—mặc dù vì `Key` không có giá trị mặc định, nó vẫn cần phải suy luận được hoặc được cung cấp:

```typescript
interface KeyValuePair<Key, Value = Key> {
key: Key;
value: Value;
}
// Type: KeyValuePair<string, string>
let allExplicit: KeyValuePair<string, number>= {
key: "rating",
value: 10,
};
// Type: KeyValuePair<string>
let oneDefaulting: KeyValuePair<string>= {
key: "rating",
value: "ten",
};
let firstMissing: KeyValuePair= {
//            ~~~~~~~~~~~~
// Error: Generic type 'KeyValuePair<Key, Value>'
// requires between 1 and 2 type arguments.
key: "rating",
value: 10,
};
```

Hãy nhớ rằng tất cả các tham số kiểu mặc định phải đứng cuối cùng trong danh sách khai báo của chúng, tương tự như các tham số hàm mặc định. Các kiểu generic không có giá trị mặc định không được phép đứng sau các kiểu generic có giá trị mặc định.

Ở đây, `inTheEnd` được cho phép vì tất cả các kiểu generic không có giá trị mặc định đều đứng trước các kiểu generic có giá trị mặc định. `inTheMiddle` gặp vấn đề vì một kiểu generic không có giá trị mặc định lại đứng sau các kiểu có giá trị mặc định:

```typescript
function inTheEnd<First, Second, Third = number, Fourth = string>() {}  // Ok
function inTheMiddle<First, Second = boolean, Third = number, Fourth>() {}
//                                                         // ~~~~~~
// Error: Required type parameters may not follow optional type parameters.
```

## **Ràng buộc kiểu generic (Constrained Generic Types)**

Theo mặc định, các kiểu generic có thể được cung cấp bất kỳ kiểu nào trên đời: classes, interfaces, primitives, unions, bất cứ thứ gì. Tuy nhiên, một số hàm chỉ được dự định hoạt động với một tập hợp các kiểu hạn chế.

TypeScript cho phép một tham số kiểu tự khai báo là cần phải _mở rộng_ (extend) một kiểu: nghĩa là nó chỉ được phép làm bí danh cho các kiểu có thể gán được cho kiểu đó. Cú pháp để ràng buộc một tham số kiểu là đặt từ khóa `extends` sau tên của tham số kiểu, theo sau là một kiểu để ràng buộc nó vào.

Ví dụ, bằng cách tạo một interface `WithLength` để mô tả bất kỳ thứ gì có `length: number`, sau đó chúng ta có thể cho phép hàm generic của mình nhận bất kỳ kiểu nào có `length` cho generic `T` của nó. Strings, arrays, và giờ đây thậm chí cả các đối tượng tình cờ có `length: number` đều được cho phép, trong khi các hình dạng kiểu như `Date` bị thiếu `length` dạng số đó sẽ dẫn đến lỗi kiểu:

```typescript
interface WithLength {
length: number;
}
function logWithLength<T extends WithLength>(input: T) {
console.log(`Length: ${input.length}`);
return input;
}
logWithLength("No one can figure out your worth but you.");  // Type: string
logWithLength([false, true]);  // Type: boolean[]
logWithLength({ length: 123 });  // Type: { length: number }

logWithLength(new Date());
//            ~~~~~~~~~~
// Error: Argument of type 'Date' is not
// assignable to parameter of type 'WithLength'.
//   Property 'length' is missing in type
//   'Date' but required in type 'WithLength'.
```

Tôi sẽ đề cập thêm các thao tác kiểu bạn có thể thực hiện với generics trong Chương 15, “Thao tác kiểu”.

### **keyof và các tham số kiểu có ràng buộc (keyof and Constrained Type Parameters)**

Toán tử `keyof` được giới thiệu trong Chương 9, “Các bổ từ kiểu” cũng hoạt động rất tốt với các tham số kiểu có ràng buộc. Việc sử dụng `extends` và `keyof` cùng nhau cho phép một tham số kiểu bị ràng buộc vào các khóa của một tham số kiểu đứng trước. Đó cũng là cách duy nhất để chỉ định khóa của một kiểu generic.

Hãy xem phiên bản đơn giản hóa này của phương thức `get` từ thư viện Lodash phổ biến. Nó nhận vào một giá trị vùng chứa, được định kiểu là `T`, và một tên `key` của một trong các khóa của `T` để lấy từ `container`. Vì tham số kiểu `Key` bị ràng buộc là một `keyof T`, TypeScript biết hàm này được phép trả về `T[Key]`:

```typescript
function get<T, Key extends keyof T>(container: T, key: Key) {
return container[key];
}
const roles= {
favorite: "Fargo",
others: ["Almost Famous", "Burn After Reading", "Nomadland"],
};
const favorite = get(roles, "favorite");  // Type: string
const others = get(roles, "others");  // Type: string[]
const missing = get(roles, "extras");
//                         ~~~~~~~~
// Error: Argument of type '"extras"' is not assignable
// to parameter of type '"favorite" | "others"'.
```

Nếu không có `keyof`, sẽ không có cách nào để định kiểu chính xác cho tham số generic `key`.

Lưu ý tầm quan trọng của tham số kiểu `Key` trong ví dụ trước. Nếu chỉ có `T` được cung cấp làm tham số kiểu, và tham số `key` được phép là bất kỳ `keyof T` nào, thì kiểu trả về sẽ là kiểu union của tất cả các giá trị thuộc tính trong `Container`. Khai báo hàm kém cụ thể hơn này không chỉ ra cho TypeScript biết rằng mỗi lời gọi có thể có một `key` cụ thể thông qua một đối số kiểu:

```typescript
function get<T>(container: T, key: keyof T) {
return container[key];
}
const roles= {
favorite: "Fargo",
others: ["Almost Famous", "Burn After Reading", "Nomadland"],
};
const found = get(roles, "favorite");  // Type: string | string[]
```

Hãy chắc chắn khi viết các hàm generic để biết khi nào kiểu của một tham số phụ thuộc vào kiểu của một tham số đứng trước. Bạn sẽ thường cần sử dụng các tham số kiểu có ràng buộc cho các kiểu tham số chính xác trong các trường hợp đó.

## **Promises**

Bây giờ bạn đã thấy cách generics hoạt động, cuối cùng đã đến lúc nói về một tính năng cốt lõi của JavaScript hiện đại dựa trên các khái niệm của chúng: Promises! Để tóm tắt lại, một Promise trong JavaScript đại diện cho một điều gì đó có thể vẫn đang chờ xử lý (pending), chẳng hạn như một yêu cầu mạng. Mỗi Promise cung cấp các phương thức để đăng ký callbacks trong trường hợp hành động đang chờ xử lý “resolves” (hoàn thành thành công) hoặc “rejects” (ném ra lỗi).

Khả năng của Promise trong việc biểu diễn các hành động tương tự trên bất kỳ kiểu giá trị tùy ý nào là một sự phù hợp tự nhiên đối với generics của TypeScript. Promises được biểu diễn trong hệ thống kiểu TypeScript dưới dạng một lớp `Promise` với một tham số kiểu duy nhất đại diện cho giá trị được resolve cuối cùng.

### **Tạo Promises (Creating Promises)**

Constructor `Promise` được định kiểu trong TypeScript là nhận vào một tham số duy nhất. Kiểu của tham số đó dựa trên một tham số kiểu được khai báo trên lớp `Promise` generic. Một dạng rút gọn sẽ trông đại loại như sau:

```typescript
class PromiseLike<Value> {
constructor(
executor: (
resolve: (value: Value) => void,

reject: (reason: unknown) => void,
        ) => void,
    ) { /* ... */ }
}
```

Việc tạo một Promise nhằm mục đích cuối cùng là resolve với một giá trị thường đòi hỏi phải khai báo rõ ràng đối số kiểu của Promise. TypeScript sẽ mặc định giả định kiểu tham số là `unknown` nếu không có đối số kiểu generic tường minh đó. Việc cung cấp tường minh một đối số kiểu cho constructor `Promise` sẽ cho phép TypeScript hiểu kiểu được resolve của thể hiện Promise tạo ra:

```typescript
// Type: Promise<unknown>
const resolvesUnknown = new Promise((resolve) => {
setTimeout(() => resolve("Done!"), 1000);
});
// Type: Promise<string>
const resolvesString = new Promise<string>((resolve) => {
setTimeout(() => resolve("Done!"), 1000);
});
```

Phương thức generic `.then` của Promise giới thiệu một tham số kiểu mới đại diện cho giá trị được resolve của Promise mà nó trả về.

Ví dụ, đoạn mã sau tạo một Promise `textEventually` resolve với một giá trị `string` sau một giây, cũng như một `lengthEventually` đợi thêm một giây nữa để resolve với một `number`:

```typescript
// Type: Promise<string>
const textEventually = new Promise<string>((resolve) => {
setTimeout(() => resolve("Done!"), 1000);
});
// Type: Promise<number>
const lengthEventually = textEventually.then((text) => text.length)
```

### **Hàm Async (Async Functions)**

Bất kỳ hàm nào được khai báo trong JavaScript với từ khóa `async` đều trả về một `Promise`. Nếu một giá trị được trả về bởi một hàm `async` trong JavaScript không phải là Thenable (một đối tượng có phương thức `.then()`; trong thực tế hầu như luôn là Promise), nó sẽ được bọc trong một `Promise` như thể `Promise.resolve` đã được gọi trên nó. TypeScript nhận diện điều này và sẽ suy luận kiểu trả về của một hàm `async` luôn là một `Promise` cho bất kỳ giá trị nào được trả về. Ở đây, `lengthAfterSecond` trả về một `Promise<number>` trực tiếp, trong khi `lengthImmediately` được suy luận là trả về một `Promise<number>` vì nó là `async` và trực tiếp trả về một `number`:

```typescript
// Type: (text: string) => Promise<number>
async function lengthAfterSecond(text: string) {
await new Promise((resolve) => setTimeout(resolve, 1000))
return text.length;
}
// Type: (text: string) => Promise<number>
async function lengthImmediately(text: string) {
return text.length;
}
```

Do đó, bất kỳ kiểu trả về nào được khai báo thủ công trên một hàm `async` luôn phải là kiểu `Promise`, ngay cả khi hàm không đề cập rõ ràng đến Promises trong phần triển khai của nó:

```typescript
// Ok
async function givesPromiseForString(): Promise<string> {
return "Done!";
}
async function givesString(): string {
//                        ~~~~~~
// Error: The return type of an async function
// or method must be the global Promise<T> type.
return "Done!";
}
```

## **Sử dụng Generics đúng cách (Using Generics Right)**

Như trong các triển khai `Promise<Value>` trước đó trong chương này, mặc dù generics có thể mang lại cho chúng ta rất nhiều sự linh hoạt trong việc mô tả các kiểu trong mã, chúng có thể trở nên khá phức tạp rất nhanh chóng. Các lập trình viên mới làm quen với TypeScript thường trải qua một giai đoạn lạm dụng generics đến mức làm cho mã trở nên khó hiểu và quá phức tạp để làm việc cùng. Thực hành tốt nhất trong TypeScript nói chung là chỉ sử dụng generics khi cần thiết, và phải rõ ràng về những gì chúng được sử dụng khi áp dụng chúng.

#### **CẢNH BÁO (WARNING)**

Hầu hết mã bạn viết trong TypeScript không nên sử dụng generics quá nhiều đến mức gây bối rối. Tuy nhiên, các kiểu cho các thư viện tiện ích, đặc biệt là các module dùng chung, đôi khi có thể cần sử dụng chúng rất nhiều. Hiểu về generics đặc biệt hữu ích để có thể làm việc hiệu quả với các kiểu tiện ích đó.

### **Quy tắc vàng của Generics (The Golden Rule of Generics)**

Một bài kiểm tra nhanh có thể giúp chỉ ra xem một tham số kiểu có cần thiết cho một hàm hay không là nó phải được sử dụng ít nhất hai lần. Generics mô tả các mối quan hệ giữa các kiểu, vì vậy nếu một tham số kiểu generic chỉ xuất hiện ở một nơi duy nhất, nó không thể nào đang định nghĩa mối quan hệ giữa nhiều kiểu.

Mỗi tham số kiểu hàm nên được sử dụng cho một tham số và sau đó cũng cho ít nhất một tham số khác và/hoặc kiểu trả về của hàm.

Ví dụ, hàm `logInput` này sử dụng tham số kiểu `Input` của nó đúng một lần duy nhất, để khai báo tham số `input` của nó:

```typescript
function logInput<Input extends string>(input: Input) {
console.log("Hi!", input);
}
```

Không giống như các hàm `identity` ở phần trước của chương, `logInput` không làm bất cứ điều gì với tham số kiểu của nó chẳng hạn như trả về hoặc khai báo thêm các tham số khác. Do đó, việc khai báo tham số kiểu `Input` đó không có nhiều ý nghĩa. Chúng ta có thể viết lại `logInput` mà không cần nó:

```typescript
function logInput(input: string) {
console.log("Hi!", input);
}
```

Cuốn sách _Effective TypeScript_ của Dan Vanderkam (O'Reilly, 2019) chứa một số mẹo tuyệt vời về cách làm việc với generics, bao gồm một phần có tiêu đề “The Golden Rule of Generics” (Quy tắc Vàng của Generics). Tôi rất khuyến khích bạn đọc _Effective TypeScript_ và phần đó đặc biệt nếu bạn thấy mình đang mất nhiều thời gian vật lộn với generics trong mã của mình.

### **Quy ước đặt tên cho Generic (Generic Naming Conventions)**

Quy ước đặt tên tiêu chuẩn cho các tham số kiểu trong nhiều ngôn ngữ, bao gồm cả TypeScript, là mặc định gọi đối số kiểu đầu tiên là “T” (viết tắt của “type” hoặc “template”) và nếu có các tham số kiểu tiếp theo, sẽ gọi chúng là “U”, “V”, v.v.

Nếu một số thông tin ngữ cảnh được biết về cách đối số kiểu dự kiến được sử dụng, quy ước đôi khi mở rộng sang việc sử dụng chữ cái đầu tiên của thuật ngữ cho cách sử dụng đó: ví dụ, các thư viện quản lý trạng thái có thể gọi một generic state là “S”. “K” và “V” thường chỉ các keys và values trong các cấu trúc dữ liệu.

Thật không may, việc đặt tên cho một đối số kiểu bằng một chữ cái có thể gây khó hiểu giống như việc đặt tên cho một hàm hoặc biến chỉ bằng một ký tự:

```typescript
// L và V rốt cuộc là cái gì thế?!
function labelBox<L, V>(l: L, v: V) { /* ... */ }
```

Khi mục đích của một generic không rõ ràng từ một chữ cái đơn lẻ `T`, tốt nhất là nên sử dụng các tên kiểu generic có tính mô tả cho biết kiểu đó được sử dụng để làm gì:

```typescript
// Rõ ràng hơn rất nhiều.
function labelBox<Label, Value>(label: Label, value: Value) { /* ... */ }
```

Bất cứ khi nào một cấu trúc có nhiều tham số kiểu, hoặc mục đích của một đối số kiểu đơn lẻ không rõ ràng ngay lập tức, hãy cân nhắc sử dụng các tên được viết đầy đủ để dễ đọc thay vì các từ viết tắt một chữ cái.

## **Tổng kết**

Trong chương này, bạn đã làm cho các lớp, hàm, giao diện và bí danh kiểu trở thành “generic” bằng cách cho phép chúng hoạt động với các tham số kiểu:

- Sử dụng các tham số kiểu để đại diện cho các kiểu khác nhau giữa các lần sử dụng của một cấu trúc
- Cung cấp các đối số kiểu tường minh hoặc ngầm định khi gọi các hàm generic
- Sử dụng generic interfaces để đại diện cho các kiểu đối tượng generic
- Thêm các tham số kiểu vào các lớp, và điều đó ảnh hưởng như thế nào đến các kiểu của chúng
- Thêm các tham số kiểu vào type aliases, đặc biệt là với discriminated type unions
- Sửa đổi các tham số kiểu generic với các giá trị mặc định (`=`) và các ràng buộc (`extends`)
- Cách Promises và các hàm `async` sử dụng generics để đại diện cho luồng dữ liệu bất đồng bộ
- Các thực hành tốt nhất với generics, bao gồm Quy tắc Vàng và các quy ước đặt tên của chúng

Đến đây là kết thúc phần _Các tính năng (Features)_ của cuốn sách này. Xin chúc mừng: bây giờ bạn đã biết tất cả các tính năng kiểm tra kiểu và cú pháp quan trọng nhất trong hệ thống kiểu TypeScript cho hầu hết các dự án!

Phần tiếp theo, _Sử dụng (Usage)_, đề cập đến cách cấu hình TypeScript để chạy trên dự án của bạn, tương tác với các phụ thuộc bên ngoài và tinh chỉnh việc kiểm tra kiểu cũng như JavaScript được xuất ra. Đó là những tính năng quan trọng để sử dụng TypeScript trên các dự án của riêng bạn.

Có một số thao tác kiểu linh tinh khác có sẵn trong cú pháp TypeScript. Bạn không cần phải hiểu đầy đủ về chúng để làm việc trong hầu hết các dự án TypeScript—nhưng chúng rất thú vị và hữu ích để biết. Tôi đã đưa chúng vào Phần IV, “Điểm cộng thêm (Extra Credit)” sau Phần III, “Sử dụng (Usage)” như một món quà thú vị nếu bạn có thời gian.

#### **MẸO (TIP)**

Bây giờ bạn đã đọc xong chương này, hãy thực hành những gì bạn đã học tại _https://learningtypescript.com/generics_.

_Tại sao generics lại khiến các lập trình viên tức giận?_

_Vì họ luôn luôn phải gõ các đối số (typing arguments)._
