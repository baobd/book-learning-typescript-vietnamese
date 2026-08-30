# **Chương 9. Các bổ từ kiểu (Type Modifiers)**

## Mục lục

- [**Chương 9. Các bổ từ kiểu (Type Modifiers)**](#chương-9-các-bổ-từ-kiểu-type-modifiers)
  - [**Kiểu đỉnh (Top Types)**](#kiểu-đỉnh-top-types)
    - [**any, một lần nữa**](#any-một-lần-nữa)
    - [**unknown**](#unknown)
  - [**Vị từ kiểu (Type Predicates)**](#vị-từ-kiểu-type-predicates)
  - [**Toán tử kiểu (Type Operators)**](#toán-tử-kiểu-type-operators)
    - [**keyof**](#keyof)
    - [**typeof**](#typeof)
      - [**keyof typeof**](#keyof-typeof)
  - [**Khẳng định kiểu (Type Assertions)**](#khẳng-định-kiểu-type-assertions)
      - [**GHI CHÚ (NOTE)**](#ghi-chú-note)
    - [**Khẳng định các kiểu lỗi bắt được (Asserting Caught Error Types)**](#khẳng-định-các-kiểu-lỗi-bắt-được-asserting-caught-error-types)
    - [**Khẳng định không null (Non-Null Assertions)**](#khẳng-định-không-null-non-null-assertions)
    - [**Những lưu ý về Type Assertion**](#những-lưu-ý-về-type-assertion)
      - [**Khẳng định so với khai báo (Assertions versus declarations)**](#khẳng-định-so-với-khai-báo-assertions-versus-declarations)
      - [**Khả năng gán của khẳng định (Assertion assignability)**](#khả-năng-gán-của-khẳng-định-assertion-assignability)
  - [**Khẳng định const (Const Assertions)**](#khẳng-định-const-const-assertions)
    - [**Từ Literals đến Primitives**](#từ-literals-đến-primitives)
    - [**Các đối tượng chỉ đọc (Read-Only Objects)**](#các-đối-tượng-chỉ-đọc-read-only-objects)
  - [**Tổng kết**](#tổng-kết)
    - [**MẸO (TIP)**](#mẹo-tip)

_Các kiểu của các kiểu từ các kiểu._

_“Đó là những chú rùa xếp chồng lên nhau tới tận đáy,”_

_Anders thường thích nói vậy._

Đến đây, bạn đã đọc tất cả về cách hệ thống kiểu của TypeScript hoạt động với các cấu trúc JavaScript hiện có như mảng, lớp và đối tượng. Trong chương này và Chương 10, “Generics”, tôi sẽ tiến thêm một bước nữa vào chính hệ thống kiểu và giới thiệu các tính năng tập trung vào việc viết các kiểu chính xác hơn, cũng như các kiểu dựa trên các kiểu khác.

## **Kiểu đỉnh (Top Types)**

Tôi đã đề cập đến khái niệm _bottom type_ (kiểu đáy) trong Chương 4, “Đối tượng” để mô tả một kiểu không thể có bất kỳ giá trị nào và không thể chạm tới được. Trong lý thuyết kiểu, hoàn toàn hợp lý khi điều ngược lại cũng tồn tại. Đúng vậy!

Một _top type_ (kiểu đỉnh), hay universal type, là một kiểu có thể đại diện cho bất kỳ giá trị nào có thể có trong một hệ thống. Các giá trị của tất cả các kiểu khác đều có thể được cung cấp cho một vị trí có kiểu là một top type. Nói cách khác, tất cả các kiểu đều có thể gán được cho một top type.

### **any, một lần nữa**

Kiểu `any` có thể hoạt động như một top type, theo nghĩa là bất kỳ kiểu nào cũng có thể được cung cấp cho một vị trí có kiểu `any`. `any` thường được sử dụng khi một vị trí được phép chấp nhận dữ liệu thuộc bất kỳ kiểu nào, chẳng hạn như các tham số cho `console.log`:

```typescript
let anyValue: any;
anyValue = "Lucille Ball";  // Ok
anyValue = 123;  // Ok

console.log(anyValue);  // Ok
```

Vấn đề với `any` là nó thông báo rõ ràng cho TypeScript không thực hiện kiểm tra kiểu trên khả năng gán hoặc các thành viên của giá trị đó. Sự thiếu an toàn đó rất hữu ích nếu bạn muốn nhanh chóng bỏ qua bộ kiểm tra kiểu của TypeScript, nhưng việc vô hiệu hóa kiểm tra kiểu làm giảm tính hữu ích của TypeScript đối với giá trị đó. Ví dụ, lời gọi `name.toUpperCase()` dưới đây chắc chắn sẽ bị crash, nhưng vì `name` được khai báo là `any`, TypeScript không báo cáo bất kỳ cảnh báo kiểu nào:

```typescript
function greetComedian(name: any) {
// No type error...
console.log(`Announcing ${name.toUpperCase()}!`);
}

greetComedian({ name: "Bea Arthur" });

// Runtime error: name.toUpperCase is not a function
```

Nếu bạn muốn chỉ ra rằng một giá trị có thể là bất cứ thứ gì, kiểu `unknown` an toàn hơn nhiều.

### **unknown**

Kiểu `unknown` trong TypeScript là top type thực sự của nó. `unknown` tương tự như `any` ở chỗ tất cả các đối tượng đều có thể được truyền vào các vị trí có kiểu `unknown`. Điểm khác biệt then chốt với `unknown` là TypeScript hạn chế hơn nhiều về các giá trị có kiểu `unknown`:

- TypeScript không cho phép truy cập trực tiếp các thuộc tính của các giá trị có kiểu `unknown`.
- `unknown` không thể gán được cho các kiểu không phải là top type (`any` hoặc `unknown`).

Việc cố gắng truy cập một thuộc tính của một giá trị có kiểu `unknown`, như trong đoạn mã sau, sẽ khiến TypeScript báo cáo lỗi kiểu:

```typescript
function greetComedian(name: unknown) {
console.log(`Announcing ${name.toUpperCase()}!`);
//                        ~~~~

// Error: Object is of type 'unknown'.
}
```

Cách duy nhất TypeScript cho phép mã truy cập các thành viên trên một biến có kiểu `unknown` là nếu kiểu của giá trị đó được thu hẹp, chẳng hạn như sử dụng `instanceof` hoặc `typeof`, hoặc bằng một type assertion.

Đoạn mã này sử dụng `typeof` để thu hẹp `name` từ `unknown` thành `string`:

```typescript
function greetComedianSafety(name: unknown) {
if (typeof name === "string") {
console.log(`Announcing ${name.toUpperCase()}!`);  // Ok
    } else {
console.log("Well, I'm off.");
    }
}

greetComedianSafety("Betty White");  // Logs: 4
greetComedianSafety({});  // Does not log
```

Hai hạn chế đó làm cho `unknown` trở thành một kiểu an toàn hơn nhiều để sử dụng so với `any`. Bạn nên luôn ưu tiên sử dụng `unknown` thay vì `any` khi có thể.

## **Vị từ kiểu (Type Predicates)**

Trước đây tôi đã chỉ cho bạn cách các cấu trúc JavaScript như `instanceof` và `typeof` có thể được sử dụng để thu hẹp các kiểu. Điều đó hoàn toàn ổn khi sử dụng trực tiếp tập hợp các kiểm tra hạn chế đó, nhưng nó sẽ bị mất đi nếu bạn bọc logic đó bằng một hàm.

Ví dụ, hàm `isNumberOrString` này nhận vào một giá trị và trả về một boolean cho biết giá trị đó là một `number` hay `string`. Con người chúng ta có thể suy ra rằng `value` bên trong câu lệnh `if` do đó phải là một trong hai kiểu đó vì `isNumberOrString(value)` trả về true, nhưng TypeScript thì không. Tất cả những gì nó biết là `isNumberOrString` trả về một boolean—chứ không phải là nó nhằm mục đích thu hẹp kiểu của một đối số:

```typescript
function isNumberOrString(value: unknown) {

return ['number', 'string'].includes(typeof value);

}

function logValueIfExists(value: number | string | null | undefined) {
if (isNumberOrString(value)) {
// Type of value: number | string | null | undefined
value.toString();
// Error: Object is possibly undefined.
    } else {
console.log("Value does not exist: ", value);
    }
}
```

TypeScript có một cú pháp đặc biệt cho các hàm trả về một boolean nhằm mục đích chỉ ra liệu một đối số có phải là một kiểu cụ thể hay không. Điều này được gọi là một _vị từ kiểu_ (type predicate), đôi khi còn được gọi là một “type guard do người dùng định nghĩa”: bạn, lập trình viên, đang tạo ra type guard của riêng mình tương tự như `instanceof` hoặc `typeof`. Type predicates thường được sử dụng để chỉ ra liệu một đối số được truyền vào dưới dạng tham số có phải là kiểu cụ thể hơn kiểu của tham số đó hay không.

Kiểu trả về của type predicate có thể được khai báo là tên của một tham số, từ khóa `is`, và một kiểu nào đó:

```typescript
function typePredicate(input: WideType): input is NarrowType;
```

Chúng ta có thể thay đổi hàm trợ giúp của ví dụ trước để có kiểu trả về tường minh nêu rõ `value is number | string`. Khi đó, TypeScript sẽ có thể suy luận rằng các khối mã chỉ có thể tiếp cận được nếu `value is number | string` là `true` phải có `value` thuộc kiểu `number | string`. Ngoài ra, các khối mã chỉ có thể tiếp cận được nếu `value is number | string` là `false` phải có `value` thuộc kiểu `null | undefined`:

```typescript
function isNumberOrString(value: unknown): value is number | string {
return ['number', 'string'].includes(typeof value);
}

function logValueIfExists(value: number | string | null | undefined) {
if (isNumberOrString(value)) {
// Type of value: number | string
value.toString();  // Ok
    } else {
// Type of value: null | undefined

console.log("value does not exist: ", value);
    }
}
```

Bạn có thể coi một type predicate không chỉ trả về một boolean, mà còn đưa ra một dấu hiệu cho thấy đối số thuộc về kiểu cụ thể hơn đó.

Type predicates thường được sử dụng để kiểm tra xem một đối tượng đã được biết là một thể hiện của một interface có phải là thể hiện của một interface cụ thể hơn hay không.

Ở đây, interface `StandupComedian` chứa thông tin bổ sung bên trên `Comedian`. Type guard `isStandupComedian` có thể được sử dụng để kiểm tra xem một `Comedian` nói chung có cụ thể là một `StandupComedian` hay không:

```typescript
interface Comedian {
funny: boolean;
}
interface StandupComedianextendsComedian {
routine: string;
}
function isStandupComedian(value: Comedian): value is StandupComedian {
return 'routine'invalue;
}
function workWithComedian(value: Comedian) {
if (isStandupComedian(value)) {
// Type of value: StandupComedian
console.log(value.routine);  // Ok
    }
// Type of value: Comedian
console.log(value.routine);
//                ~~~~~~~
// Error: Property 'routine' does not exist on type 'Comedian'.
}
```

Hãy cẩn thận: vì type predicates cũng thu hẹp kiểu trong trường hợp false, bạn có thể nhận được kết quả bất ngờ nếu một type predicate kiểm tra nhiều hơn chỉ là kiểu của đầu vào.

Type predicate `isLongString` này trả về `false` nếu tham số `input` của nó là `undefined` hoặc một `string` có độ dài nhỏ hơn `7`. Do đó, câu lệnh `else` (trường hợp false của nó) bị thu hẹp lại khi nghĩ rằng `text` bắt buộc phải có kiểu `undefined`:

```typescript
function isLongString(input: string | undefined): input is string {
return!!(input&&input.length>=7);
}
function workWithText(text: string | undefined) {
if (isLongString(text)) {
// Type of text: string
console.log("Long text: ", text.length);
    } else {
// Type of text: undefined
console.log("Short text: ", text?.length);
//                               ~~~~~~
// Error: Property 'length' does not exist on type 'never'.
    }
}
```

Các type predicates làm nhiều hơn việc xác minh kiểu của một thuộc tính hoặc giá trị rất dễ bị lạm dụng. Tôi thường khuyên bạn nên tránh chúng khi có thể. Các type predicates đơn giản là đủ cho hầu hết các trường hợp.

## **Toán tử kiểu (Type Operators)**

Không phải tất cả các kiểu đều có thể được biểu diễn chỉ bằng một từ khóa hoặc tên của một kiểu hiện có. Đôi khi có thể cần tạo một kiểu mới kết hợp cả hai, thực hiện một số phép biến đổi trên các thuộc tính của một kiểu hiện có.

### **keyof**

Các đối tượng JavaScript có thể có các thành viên được truy xuất bằng các giá trị động, thường (nhưng không nhất thiết) có kiểu `string`. Biểu diễn các khóa này trong hệ thống kiểu có thể phức tạp. Việc sử dụng một kiểu nguyên thủy bao quát như `string` sẽ cho phép các khóa không hợp lệ đối với giá trị vùng chứa.

Đó là lý do tại sao TypeScript khi sử dụng các thiết lập cấu hình nghiêm ngặt hơn—được đề cập trong Chương 13, “Các tùy chọn cấu hình”—sẽ báo lỗi trên `ratings[key]` như trong ví dụ tiếp theo. Kiểu `string` cho phép các giá trị không được phép làm thuộc tính trên interface `Ratings`, và `Ratings` không khai báo một index signature để cho phép bất kỳ khóa `string` nào:

```typescript
interface Ratings {
audience: number;
critics: number;
}
function getRating(ratings: Ratings, key: string): number {
return ratings[key];
//     ~~~~~~~~~~~
// Error: Element implicitly has an 'any' type because expression
// of type 'string' can't be used to index type 'Ratings'.
//   No index signature with a parameter of
//   type 'string' was found on type 'Ratings'.
}
const ratings: Ratings= { audience: 66, critic: 84 };
getRating(ratings, 'audience');  // Ok

getRating(ratings, 'not valid');  // Ok, but shouldn't be
```

Một lựa chọn khác là sử dụng một union type của các literals cho các khóa được phép. Điều đó sẽ chính xác hơn trong việc giới hạn đúng chỉ các khóa tồn tại trên giá trị vùng chứa:

```typescript
function getRating(ratings: Ratings, key: 'audience' | 'critic'): number {
return ratings[key];  // Ok
}
const ratings: Ratings= { audience: 66, critic: 84 };
getCountLiteral(ratings, 'audience');  // Ok
getCountLiteral(ratings, 'not valid');
//                       ~~~~~~~~~~~
// Error: Argument of type '"not valid"' is not
// assignable to parameter of type '"audience" | "critic"'.
```

Tuy nhiên, điều gì sẽ xảy ra nếu interface có hàng tá thành viên trở lên? Bạn sẽ phải gõ từng khóa của các thành viên đó vào union type và luôn cập nhật chúng. Thật là phiền toái.

TypeScript thay vào đó cung cấp toán tử `keyof` nhận vào một kiểu hiện có và trả về một union của tất cả các khóa được phép trên kiểu đó. Đặt nó phía trước tên của một kiểu ở bất kỳ nơi nào bạn có thể sử dụng một kiểu, chẳng hạn như một chú thích kiểu.

Ở đây, `keyof Ratings` tương đương với `'audience' | 'critic'` nhưng viết nhanh hơn nhiều và sẽ không cần phải cập nhật thủ công nếu interface `Ratings` có thay đổi:

```typescript
function getCountKeyof(ratings: Ratings, key: keyof Ratings): number {
return ratings[key];  // Ok
}

const ratings: Ratings= { audience: 66, critic: 84 };

getCountKeyof(ratings, 'audience');  // Ok

getCountKeyof(ratings, 'not valid');
//                     ~~~~~~~~~~~
// Error: Argument of type '"not valid"' is not
// assignable to parameter of type 'keyof Ratings'.
```

`keyof` là một tính năng tuyệt vời để tạo ra các union types dựa trên các khóa của các kiểu hiện có. Nó cũng kết hợp rất tốt với các toán tử kiểu khác trong TypeScript, cho phép một số mô hình rất tiện lợi mà bạn sẽ thấy sau trong chương này và Chương 15, “Thao tác kiểu”.

### **typeof**

Một toán tử kiểu khác do TypeScript cung cấp là `typeof`. Nó trả về kiểu của một giá trị được cung cấp. Điều này có thể hữu ích nếu kiểu của giá trị quá phức tạp để viết thủ công.

Ở đây, biến `adaptation` được khai báo có cùng kiểu với `original`:

```typescript
const original= {
medium: "movie",
title: "Mean Girls",
};
let adaptation: type oforiginal;
if (Math.random() >0.5) {
adaptation= { ...original, medium: "play" };  // Ok
} else {
adaptation= { ...original, medium: 2 };
//                          ~~~~~~
// Error: Type 'number' is not assignable to type 'string'.
}
```

Mặc dù toán tử _kiểu_ `typeof` trông giống hệt toán tử `typeof` _thời gian chạy_ được sử dụng để trả về một chuỗi mô tả kiểu của một giá trị, nhưng hai toán tử này là khác nhau. Chúng chỉ trùng hợp sử dụng cùng một từ. Hãy nhớ rằng: toán tử JavaScript là một toán tử thời gian chạy trả về tên chuỗi của một kiểu. Phiên bản TypeScript, vì là một toán tử kiểu, chỉ có thể được sử dụng trong các kiểu và sẽ không xuất hiện trong mã đã biên dịch.

#### **keyof typeof**

`typeof` truy xuất kiểu của một giá trị, và `keyof` truy xuất các khóa được phép trên một kiểu. TypeScript cho phép nối hai từ khóa này lại với nhau để truy xuất ngắn gọn các khóa được phép trên kiểu của một giá trị. Kết hợp lại với nhau, toán tử kiểu `typeof` trở nên vô cùng hữu ích khi làm việc với các thao tác kiểu `keyof`.

Trong ví dụ này, hàm `logRating` nhằm mục đích nhận vào một trong các khóa của giá trị `ratings`. Thay vì tạo một interface, mã sử dụng `keyof typeof` để chỉ ra rằng `key` phải là một trong các khóa trên kiểu của giá trị `ratings`:

```typescript
const ratings = {
  imdb: 8.4,
  metacritic: 82,
};
function logRating(key: keyof typeof ratings) {
  console.log(ratings[key]);
}
logRating("imdb");  // Ok
logRating("invalid");
//        ~~~~~~~~~
// Error: Argument of type '"missing"' is not assignable
// to parameter of type '"imdb" | "metacritic"'.
```

Bằng cách kết hợp `keyof` và `typeof`, chúng ta tiết kiệm được công sức phải viết ra—và phải cập nhật—các kiểu đại diện cho các khóa được phép trên các đối tượng không có kiểu interface tường minh.

## **Khẳng định kiểu (Type Assertions)**

TypeScript hoạt động tốt nhất khi mã của bạn được “định kiểu mạnh” (strongly typed): tất cả các giá trị trong mã của bạn đều có các kiểu được biết chính xác. Các tính năng như top types và type guards cung cấp các cách để đưa mã phức tạp vào sự hiểu biết của bộ kiểm tra kiểu TypeScript. Tuy nhiên, đôi khi việc thông báo chính xác 100% cho hệ thống kiểu biết cách mã của bạn hoạt động là không thể thực hiện một cách hợp lý.

Ví dụ, `JSON.parse` cố tình trả về top type `any`. Không có cách nào để thông báo an toàn cho hệ thống kiểu biết rằng một giá trị chuỗi cụ thể được cung cấp cho `JSON.parse` sẽ trả về bất kỳ kiểu giá trị cụ thể nào. (Như chúng ta sẽ thấy trong Chương 10, “Generics”, việc thêm một kiểu generic vào `parse` chỉ được sử dụng một lần cho một kiểu trả về sẽ vi phạm thực hành tốt nhất được gọi là Quy tắc Vàng của Generics.)

TypeScript cung cấp một cú pháp để ghi đè hiểu biết của hệ thống kiểu về kiểu của một giá trị: một “khẳng định kiểu” (type assertion), còn được gọi là “ép kiểu” (type cast). Trên một giá trị được dự định là một kiểu khác, bạn có thể đặt từ khóa `as` theo sau là một kiểu. TypeScript sẽ tuân theo khẳng định của bạn và đối xử với giá trị đó như kiểu đó.

Trong đoạn mã này, có khả năng kết quả trả về từ `JSON.parse` được dự định là một kiểu như `string[]`, `[string, string]`, hoặc `["grace", "frankie"]`. Đoạn mã sử dụng các type assertions cho ba dòng mã để chuyển kiểu từ `any` sang một trong những kiểu đó:

```typescript
const rawData=`["grace", "frankie"]`;
// Type: any
JSON.parse(rawData);
// Type: string[]
JSON.parse(rawData) as string[];
// Type: [string, string]
JSON.parse(rawData) as [str in g, string];
// Type: ["grace", "frankie"]
JSON.parse(rawData) as ["grace", "frankie"];
```

Type assertions chỉ tồn tại trong hệ thống kiểu TypeScript. Chúng bị loại bỏ cùng với tất cả các cú pháp hệ thống kiểu khác khi biên dịch sang JavaScript. Mã trước đó sẽ trông như thế này khi được biên dịch sang JavaScript:

```typescript
const rawData=`["grace", "frankie"]`;
// Type: any
JSON.parse(rawData);
// Type: string[]
JSON.parse(rawData);
// Type: [string, string]
JSON.parse(rawData);
// Type: ["grace", "frankie"]
JSON.parse(rawData);
```

#### **GHI CHÚ (NOTE)**

Nếu bạn đang làm việc với các thư viện hoặc mã cũ hơn, bạn có thể thấy một cú pháp ép kiểu khác trông giống như `<type>item` thay vì `item as type`. Vì cú pháp này không tương thích với cú pháp JSX và do đó không hoạt động trong các tệp _.tsx_, nên nó không được khuyến khích.

Thực hành tốt nhất trong TypeScript nói chung là tránh sử dụng type assertions khi có thể. Tốt nhất là mã của bạn nên được định kiểu đầy đủ và không cần can thiệp vào hiểu biết của TypeScript về các kiểu của nó bằng cách sử dụng các assertions. Nhưng đôi khi sẽ có những trường hợp mà type assertions là hữu ích, thậm chí là cần thiết.

### **Khẳng định các kiểu lỗi bắt được (Asserting Caught Error Types)**

Xử lý lỗi là một nơi khác mà type assertions có thể phát huy tác dụng. Nhìn chung không thể biết một lỗi bắt được trong một khối `catch` sẽ có kiểu gì vì mã trong khối `try` có thể ném ra bất kỳ đối tượng nào khác ngoài dự kiến của bạn. Hơn nữa, mặc dù thực hành tốt nhất trong JavaScript là luôn ném ra một thể hiện của lớp `Error`, một số dự án thay vào đó lại ném ra các string literals hoặc các giá trị bất ngờ khác.

Nếu bạn hoàn toàn tin tưởng rằng một vùng mã sẽ chỉ ném ra một thể hiện của lớp `Error`, bạn có thể sử dụng một type assertion để coi một khẳng định bị bắt là một `Error`. Đoạn mã này truy cập thuộc tính `message` của một `error` bị bắt mà nó giả định là một thể hiện của lớp `Error`:

```javascript
try {
// (code that may throw an error)
} catch (error) {
console.warn("Oh no!", (errorasError).message);
}
```

Nhìn chung sẽ an toàn hơn nếu sử dụng một hình thức thu hẹp kiểu như kiểm tra `instanceof` để đảm bảo lỗi bị ném ra là kiểu lỗi mong đợi. Đoạn mã này kiểm tra xem lỗi bị ném ra có phải là một thể hiện của lớp `Error` hay không để biết nên log thông báo đó hay chính lỗi đó:

```javascript
try {
// (code that may throw an error)
} catch (error) {
console.warn("Oh no!", errorinstanceofError?error.message : error);
}
```

### **Khẳng định không null (Non-Null Assertions)**

Một trường hợp sử dụng phổ biến khác cho type assertions là loại bỏ `null` và/hoặc `undefined` khỏi một biến mà chỉ về mặt lý thuyết, chứ không phải trên thực tế, có thể bao gồm chúng. Tình huống đó phổ biến đến mức TypeScript bao gồm một cú pháp viết tắt cho nó. Thay vì viết ra `as` và kiểu đầy đủ của bất kỳ thứ gì ngoại trừ `null` và `undefined`, bạn có thể sử dụng dấu `!` để biểu thị điều tương tự. Nói cách khác, khẳng định không-null `!` khẳng định rằng kiểu không phải là `null` hoặc `undefined`.

Hai type assertions sau đây là giống hệt nhau ở chỗ cả hai đều mang lại `Date` chứ không phải `Date | undefined`:

```typescript
// Inferred type: Date | undefined
let maybeDate = Math.random() >0.5
?undefined
:new Date();
// Asserted type: Date
maybeDateasDate;
// Asserted type: Date
maybeDate!;
```

Các khẳng định không null đặc biệt hữu ích với các API như `Map.get` trả về một giá trị hoặc `undefined` nếu giá trị đó không tồn tại.

Ở đây, `seasonCounts` là một `Map<string, number>` thông thường. Chúng ta biết rằng nó chứa một khóa `"I Love Lucy"`, vì vậy biến `knownValue` có thể sử dụng dấu `!` để loại bỏ `| undefined` khỏi kiểu của nó:

```typescript
const seasonCounts = new Map([
    ["I Love Lucy", 6],
    ["The Golden Girls", 7],
]);
// Type: string | undefined
const maybeValue = seasonCounts.get("I Love Lucy");
console.log(maybeValue.toUpperCase());
//          ~~~~~~~~~~
// Error: Object is possibly 'undefined'.
// Type: string

const knownValue = seasonCounts.get("I Love Lucy")!;

console.log(knownValue.toUpperCase());  // Ok
```

### **Những lưu ý về Type Assertion**

Type assertions, giống như kiểu `any`, là một lối thoát cần thiết cho hệ thống kiểu của TypeScript. Do đó, cũng giống như kiểu `any`, chúng nên tránh bất cứ khi nào có thể một cách hợp lý. Thường thì việc có các kiểu chính xác hơn đại diện cho mã của bạn sẽ tốt hơn là việc làm cho việc khẳng định trên kiểu của một giá trị trở nên dễ dàng hơn. Những khẳng định đó thường sai—hoặc đã sai tại thời điểm viết, hoặc chúng trở nên sai sau này khi codebase thay đổi.

Ví dụ, giả sử ví dụ `seasonCounts` thay đổi theo thời gian để có các giá trị khác nhau trong map. Khẳng định không null của nó có thể vẫn làm cho mã vượt qua kiểm tra kiểu của TypeScript, nhưng có thể có một lỗi thời gian chạy:

```typescript
const seasonCounts = new Map([
  ["Broad City", 5],
  ["Community", 6],
]);

// Type: string
const knownValue = seasonCounts.get("I Love Lucy")!;

console.log(knownValue.toUpperCase());  // No type error, but...
// Runtime TypeError: Cannot read property 'toUpperCase' of undefined.
```

Type assertions nhìn chung nên được sử dụng hạn chế, và chỉ khi bạn hoàn toàn chắc chắn rằng làm như vậy là an toàn.

#### **Khẳng định so với khai báo (Assertions versus declarations)**

Có sự khác biệt giữa việc sử dụng một chú thích kiểu để khai báo kiểu của một biến so với việc sử dụng một type assertion để thay đổi kiểu của một biến có giá trị ban đầu. Bộ kiểm tra kiểu của TypeScript thực hiện kiểm tra khả năng gán trên giá trị ban đầu của biến đối với chú thích kiểu của biến khi cả hai đều tồn tại. Tuy nhiên, một type assertion lại yêu cầu TypeScript bỏ qua một số kiểm tra kiểu của nó.

Mã sau đây tạo ra hai đối tượng có kiểu `Entertainer` với cùng một thiếu sót: thiếu thành viên `acts`. TypeScript có thể bắt được lỗi trong biến `declared` nhờ vào chú thích kiểu `: Entertainer` của nó. Nó không thể bắt được lỗi trên biến `asserted` vì sự xuất hiện của type assertion:

```typescript
interface Entertainer {
acts: string[];
name: string;
}
const declared: Entertainer= {
name: "Moms Mabley",
};
// Error: Property 'acts' is missing in type
// '{ one: number; }' but required in type 'Entertainer'.
const asserted= {
name: "Moms Mabley",
} as Entertainer;  // Ok, but...
// Both of these statements would fail at runtime with:
// Runtime TypeError: Cannot read properties of undefined (reading
'toPrecision')
console.log(declared.acts.join(", "));
console.log(asserted.acts.join(", "));
```

Do đó, rất nên sử dụng một chú thích kiểu hoặc cho phép TypeScript tự suy luận kiểu của biến từ giá trị ban đầu của nó.

#### **Khả năng gán của khẳng định (Assertion assignability)**

Type assertions chỉ nhằm mục đích là một lối thoát nhỏ, dành cho các tình huống mà kiểu của một giá trị nào đó hơi không chính xác. TypeScript sẽ chỉ cho phép các type assertions giữa hai kiểu nếu một trong hai kiểu có thể gán được cho kiểu kia. Nếu type assertion là giữa hai kiểu hoàn toàn không liên quan, thì TypeScript sẽ nhận thấy và báo cáo lỗi kiểu.

Ví dụ, việc chuyển từ một kiểu nguyên thủy này sang một kiểu nguyên thủy khác là không được phép, vì các kiểu nguyên thủy không có liên quan gì đến nhau:

```typescript
let myValue = "Stella!" as number;
//            ~~~~~~~~~~~~~~~~~~~
// Error: Conversion of type 'string' to type 'number'
// may be a mistake because neither type sufficiently
// overlaps with the other. If this was intentional,
// convert the expression to 'unknown' first.
```

Nếu bạn bắt buộc phải chuyển một giá trị từ một kiểu sang một kiểu hoàn toàn không liên quan, bạn có thể sử dụng một khẳng định kiểu kép (double type assertion). Đầu tiên ép giá trị sang một top type—`any` hoặc `unknown`—và sau đó ép kết quả đó sang kiểu không liên quan:

```typescript
let myValueDouble = "1337" as unknown as number;  // Ok, but... eww.
```

Các khẳng định kiểu kép `as unknown as...` là nguy hiểm và hầu như luôn là dấu hiệu của một điều gì đó không chính xác trong các kiểu của mã xung quanh. Sử dụng chúng như một lối thoát khỏi hệ thống kiểu có nghĩa là hệ thống kiểu có thể không bảo vệ được bạn khi những thay đổi đối với mã xung quanh gây ra sự cố cho đoạn mã trước đó từng hoạt động. Tôi chỉ giảng giải các double type assertions như một lời cảnh báo để giúp giải thích hệ thống kiểu, chứ không khuyến khích việc sử dụng chúng.

## **Khẳng định const (Const Assertions)**

Trong Chương 4, “Đối tượng”, tôi đã giới thiệu cú pháp `as const` để chuyển một kiểu mảng có thể thay đổi thành một kiểu tuple chỉ đọc và đã hứa sẽ sử dụng nó nhiều hơn sau này trong cuốn sách. Thời điểm đó chính là bây giờ!

Các khẳng định const nhìn chung có thể được sử dụng để chỉ ra rằng bất kỳ giá trị nào—mảng, kiểu nguyên thủy, giá trị đối tượng, bất cứ thứ gì—đều phải được đối xử như phiên bản hằng số, bất biến (immutable) của chính nó. Cụ thể, `as const` áp dụng ba quy tắc sau cho bất kỳ kiểu nào nó nhận được:

- Các mảng được đối xử như các `readonly` tuples, không phải các mảng có thể thay đổi.
- Các literals được đối xử như chính literals đó, không phải các kiểu nguyên thủy tổng quát tương đương của chúng.
- Các thuộc tính trên các đối tượng được coi là `readonly`.

Bạn đã thấy các mảng trở thành tuples, chẳng hạn như với mảng này được khẳng định là một tuple:

```typescript
// Type: (number | string)[]
[0, ''];
// Type: readonly [0, '']
[0, ''] as const;
```

Hãy cùng tìm hiểu sâu hơn về hai thay đổi còn lại mà `as const` tạo ra.

### **Từ Literals đến Primitives**

Hệ thống kiểu hiểu một giá trị literal là chính giá trị literal cụ thể đó thay vì mở rộng nó thành kiểu nguyên thủy tổng quát có thể rất hữu ích.

Ví dụ, tương tự như các hàm trả về tuples, việc một hàm được biết là tạo ra một literal cụ thể thay vì một primitive tổng quát có thể rất hữu ích. Các hàm này cũng trả về các giá trị có thể làm cho cụ thể hơn—ở đây, kiểu trả về của `getNameConst` là kiểu cụ thể hơn `"Maria Bamford"` thay vì `string` tổng quát:

```typescript
// Type: () => string
const getName= () =>"Maria Bamford";

// Type: () => "Maria Bamford"
const getNameConst= () =>"Maria Bamford" as const;
```

Việc có các trường cụ thể trên một giá trị là các literals cụ thể hơn cũng có thể hữu ích. Nhiều thư viện phổ biến yêu cầu một trường discriminant trên một giá trị phải là một literal cụ thể để các kiểu trong mã của họ có thể đưa ra các suy luận cụ thể hơn trên giá trị đó. Ở đây, biến `narrowJoke` có `style` thuộc kiểu `"one-liner"` thay vì `string`, vì vậy nó có thể được cung cấp ở một vị trí cần kiểu `Joke`:

```typescript
interface Joke {
quote: string;
style: "story" | "one-liner";
}

function tellJoke(joke: Joke) {
if (joke.style === "one-liner") {
console.log(joke.quote);
    } else {
console.log(joke.quote.split("
"));
    }
}
// Type: { quote: string; style: "one-liner" }
const narrowJoke= {
quote: "If you stay alive for no other reason do it for spite.",
style: "one-liner" as const,
};
tellJoke(narrowJoke);  // Ok
// Type: { quote: string; style: string }
const wideObject= {
quote: "Time flies when you are anxious!",
style: "one-liner",
};
tellJoke(wideObject);
// Error: Argument of type '{ quote: string; style: string; }'
// is not assignable to parameter of type 'LogAction'.
//   Types of property 'style' are incompatible.
//     Type 'string' is not assignable to type '"story" | "one-liner"'.
```

### **Các đối tượng chỉ đọc (Read-Only Objects)**

Các object literals chẳng hạn như những đối tượng được sử dụng làm giá trị ban đầu của một biến nhìn chung sẽ mở rộng kiểu của các thuộc tính theo cùng cách mà các giá trị ban đầu của các biến `let` mở rộng. Các giá trị chuỗi như `'apple'` trở thành các primitives như `string`, các mảng được định kiểu là các mảng thay vì tuples, v.v. Điều này có thể bất tiện khi một số hoặc tất cả các giá trị đó sau này được dự định sử dụng ở một nơi yêu cầu kiểu literal cụ thể của chúng.

Tuy nhiên, việc khẳng định một value literal bằng `as const` sẽ chuyển kiểu được suy luận sang dạng cụ thể nhất có thể. Tất cả các thuộc tính thành viên đều trở thành `readonly`, literals được coi là kiểu literal của chính chúng thay vì kiểu nguyên thủy tổng quát của chúng, mảng trở thành read-only tuples, v.v. Nói cách khác, việc áp dụng khẳng định const cho một value literal làm cho value literal đó trở nên bất biến (immutable) và áp dụng đệ quy cùng một logic khẳng định const cho tất cả các thành viên của nó.

Ví dụ, giá trị `preferencesMutable` sau đây được khai báo không có `as const`, vì vậy tên của nó là kiểu nguyên thủy `string` và nó được phép sửa đổi. Tuy nhiên, `favoritesConst` được khai báo với `as const`, vì vậy các giá trị thành viên của nó là literals và không được phép sửa đổi:

```typescript
function describePreference(preference: "maybe" | "no" | "yes") {
switch (preference) {
case "maybe":
return "I suppose...";
case "no":
return "No thanks.";
case "yes":
return "Yes please!";
    }
}
// Type: { movie: string, standup: string }
const preferencesMutable= {
movie: "maybe"
standup: "yes",
};
describePreference(preferencesMutable.movie);
//                 ~~~~~~~~~~~~~~~~~~~~~~~~
// Error: Argument of type 'string' is not assignable
// to parameter of type '"maybe" | "no" | "yes"'.
preferencesMutable.movie = "no";  // Ok
// Type: readonly { readonly movie: "maybe", readonly standup: "yes" }
const preferencesReadonly= {
movie: "maybe"
standup: "yes",
} as const;
describePreference(preferencesReadonly.movie);  // Ok
preferencesReadonly.movie = "no";
//                  ~~~~~
// Error: Cannot assign to 'movie' because it is a read-only property.
```

## **Tổng kết**

Trong chương này, bạn đã sử dụng các bổ từ kiểu để lấy các đối tượng và/hoặc các kiểu hiện có và biến chúng thành các kiểu mới:

- Top types: `any` có tính dễ dãi cao và `unknown` có tính hạn chế cao
- Các toán tử kiểu: sử dụng `keyof` để lấy các khóa của một kiểu và/hoặc `typeof` để lấy kiểu của một giá trị
- Sử dụng—và khi nào không nên sử dụng—type assertions để lén lút thay đổi kiểu của một giá trị
- Thu hẹp các kiểu bằng cách sử dụng các khẳng định `as const`

#### **MẸO (TIP)**

Bây giờ bạn đã đọc xong chương này, hãy thực hành những gì bạn đã học tại _https://learningtypescript.com/type-modifiers_.

_Tại sao kiểu literal lại cứng đầu như vậy?_

_Vì nó có một tư duy hạn hẹp (narrow mind)._
