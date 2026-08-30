**Phần II. Các tính năng (Features)**

# **Chương 5. Hàm (Functions)**

## Mục lục

- [**Chương 5. Hàm (Functions)**](#chương-5-hàm-functions)
  - [**Các tham số hàm (Function Parameters)**](#các-tham-số-hàm-function-parameters)
    - [**Các tham số bắt buộc (Required Parameters)**](#các-tham-số-bắt-buộc-required-parameters)
      - [**GHI CHÚ (NOTE)**](#ghi-chú-note)
    - [**Các tham số tùy chọn (Optional Parameters)**](#các-tham-số-tùy-chọn-optional-parameters)
    - [**Các tham số mặc định (Default Parameters)**](#các-tham-số-mặc-định-default-parameters)
    - [**Các tham số còn lại (Rest Parameters)**](#các-tham-số-còn-lại-rest-parameters)
  - [**Kiểu trả về (Return Types)**](#kiểu-trả-về-return-types)
    - [**Kiểu trả về tường minh (Explicit Return Types)**](#kiểu-trả-về-tường-minh-explicit-return-types)
  - [**Kiểu hàm (Function Types)**](#kiểu-hàm-function-types)
    - [**Dấu ngoặc đơn trong kiểu hàm (Function Type Parentheses)**](#dấu-ngoặc-đơn-trong-kiểu-hàm-function-type-parentheses)
    - [**Suy luận kiểu tham số (Parameter Type Inferences)**](#suy-luận-kiểu-tham-số-parameter-type-inferences)
    - [**Bí danh kiểu hàm (Function Type Aliases)**](#bí-danh-kiểu-hàm-function-type-aliases)
  - [**Các kiểu trả về khác (More Return Types)**](#các-kiểu-trả-về-khác-more-return-types)
    - [**Trả về kiểu void (Void Returns)**](#trả-về-kiểu-void-void-returns)
    - [**Trả về kiểu never (Never Returns)**](#trả-về-kiểu-never-never-returns)
      - [**GHI CHÚ (NOTE)**](#ghi-chú-note-1)
  - [**Nạp chồng hàm (Function Overloads)**](#nạp-chồng-hàm-function-overloads)
      - [**CẢNH BÁO (WARNING)**](#cảnh-báo-warning)
    - [**Tính tương thích của Call-Signature (Call-Signature Compatibility)**](#tính-tương-thích-của-call-signature-call-signature-compatibility)
  - [**Tổng kết**](#tổng-kết)
    - [**MẸO (TIP)**](#mẹo-tip)

_Các đối số của hàm_

_Vào đầu này, ra đầu kia_

_Dưới dạng một kiểu trả về_

Trong Chương 2, “Hệ thống kiểu”, bạn đã thấy cách sử dụng các chú thích kiểu (type annotations) để chú thích giá trị của các biến. Bây giờ, bạn sẽ thấy cách làm tương tự với các tham số hàm và kiểu trả về—và tại sao điều đó lại hữu ích.

## **Các tham số hàm (Function Parameters)**

Hãy xem xét hàm `sing` sau đây nhận vào một tham số `song` và log nó ra console:

```javascript
function sing(song) {
  console.log(`Singing: ${song}!`);
}
```

Lập trình viên viết hàm `sing` đã dự định cung cấp kiểu giá trị nào cho tham số `song`?

Nó có phải là một `string` không? Nó có phải là một đối tượng có phương thức `toString()` được override không? Đoạn mã này có bị lỗi không? _Ai mà biết được?!_

Nếu không có thông tin kiểu tường minh được khai báo, chúng ta có thể không bao giờ biết được—TypeScript sẽ coi nó là kiểu `any`, nghĩa là kiểu của tham số có thể là bất cứ thứ gì.

Cũng như với các biến, TypeScript cho phép bạn khai báo kiểu của các tham số hàm bằng một chú thích kiểu (type annotation). Bây giờ chúng ta có thể sử dụng một `: string` để thông báo cho TypeScript biết rằng tham số `song` có kiểu `string`:

```typescript
function sing(song: string) {
  console.log(`Singing: ${song}!`);
}
```

Tốt hơn nhiều rồi: bây giờ chúng ta đã biết `song` được dự định là kiểu gì!

Lưu ý rằng bạn không nhất thiết phải thêm các chú thích kiểu chuẩn chỉnh vào các tham số hàm để mã của bạn có cú pháp TypeScript hợp lệ. TypeScript có thể cảnh báo bạn bằng các lỗi kiểu, nhưng mã JavaScript được xuất ra vẫn sẽ chạy. Đoạn mã trước đó bị thiếu khai báo kiểu trên tham số `song` vẫn sẽ được chuyển đổi từ TypeScript sang JavaScript. Chương 13, “Các tùy chọn cấu hình” sẽ đề cập đến cách cấu hình các cảnh báo của TypeScript về các tham số có kiểu `any` ngầm định như `song`.

### **Các tham số bắt buộc (Required Parameters)**

Không giống như JavaScript, ngôn ngữ cho phép các hàm được gọi với bất kỳ số lượng đối số nào, TypeScript giả định rằng tất cả các tham số được khai báo trên một hàm đều là bắt buộc. Nếu một hàm được gọi với sai số lượng đối số, TypeScript sẽ phản đối dưới dạng một lỗi kiểu. Việc đếm đối số của TypeScript sẽ phát huy tác dụng nếu một hàm được gọi với quá ít hoặc quá nhiều đối số.

Hàm `singTwo` này yêu cầu hai tham số, vì vậy việc truyền một đối số và việc truyền ba đối số đều không được phép:

```typescript
function singTwo(first: string, second: string) {
  console.log(`${first} / ${second}`);
}
// Logs: "Ball and Chain / undefined"
singTwo("Ball and Chain");
//      ~~~~~~~~~~~~~~~~
// Error: Expected 2 arguments, but got 1.
// Logs: "I Will Survive / Higher Love"
singTwo("I Will Survive", "Higher Love");  // Ok
// Logs: "Go Your Own Way / The Chain"
singTwo("Go Your Own Way", "The Chain", "Dreams");
//                                      ~~~~~~~~
// Error: Expected 2 arguments, but got 3.
```

Việc thực thi rằng các tham số bắt buộc phải được cung cấp cho một hàm giúp tăng cường tính an toàn kiểu bằng cách đảm bảo tất cả các giá trị đối số dự kiến đều tồn tại bên trong hàm. Việc không đảm bảo các giá trị đó tồn tại có thể dẫn đến hành vi không mong muốn trong mã, chẳng hạn như hàm `singTwo` trước đó log ra `undefined` hoặc bỏ qua một đối số.

#### **GHI CHÚ (NOTE)**

_Parameter_ (tham số) đề cập đến khai báo của một hàm về những gì nó mong đợi nhận được dưới dạng một đối số. _Argument_ (đối số) đề cập đến một giá trị được cung cấp cho một tham số trong một lời gọi hàm. Trong ví dụ trước, `first` và `second` là các parameters, trong khi các chuỗi như `"Dreams"` là các arguments.

### **Các tham số tùy chọn (Optional Parameters)**

Hãy nhớ lại rằng trong JavaScript, nếu một tham số hàm không được cung cấp, giá trị đối số của nó bên trong hàm mặc định là `undefined`. Đôi khi các tham số hàm không nhất thiết phải được cung cấp, và mục đích sử dụng dự kiến của hàm là dành cho giá trị `undefined` đó. Chúng ta sẽ không muốn TypeScript báo cáo các lỗi kiểu vì không cung cấp đối số cho các tham số tùy chọn đó. TypeScript cho phép chú thích một tham số là tùy chọn bằng cách thêm dấu `?` trước dấu `:` trong chú thích kiểu của nó—tương tự như các thuộc tính kiểu đối tượng tùy chọn.

Các tham số tùy chọn không cần phải được cung cấp cho các lời gọi hàm. Do đó, kiểu của chúng luôn có `| undefined` được thêm vào dưới dạng một union type.

Trong hàm `announceSong` sau đây, tham số `singer` được đánh dấu là tùy chọn. Kiểu của nó là `string | undefined`, và nó không cần phải được cung cấp bởi những nơi gọi hàm. Nếu `singer` được cung cấp, nó có thể là một giá trị `string` hoặc `undefined`:

```typescript
function announceSong(song: string, singer?: string) {
  console.log(`Song: ${song}`);

  if (singer) {
    console.log(`Singer: ${singer}`);
  }
}

announceSong("Greensleeves");  // Ok
announceSong("Greensleeves", undefined);  // Ok
announceSong("Chandelier", "Sia");  // Ok
```

Các tham số tùy chọn này luôn có khả năng ngầm định là `undefined`. Trong đoạn mã trước, `singer` bắt đầu với kiểu `string | undefined`, sau đó được thu hẹp về chỉ `string` bởi câu lệnh `if`.

Các tham số tùy chọn không giống với các tham số có union types tình cờ bao gồm `| undefined`. Các tham số không được đánh dấu là tùy chọn bằng dấu `?` luôn phải được cung cấp, ngay cả khi giá trị là `undefined` một cách tường minh.

Tham số `singer` trong hàm `announceSongBy` này bắt buộc phải được cung cấp tường minh. Nó có thể là một giá trị `string` hoặc `undefined`:

```typescript
function announceSongBy(song: string, singer: string | undefined) { /* ... */
}

announceSongBy("Greensleeves");
// Error: Expected 2 arguments, but got 1.

announceSongBy("Greensleeves", undefined);  // Ok
announceSongBy("Chandelier", "Sia");  // Ok
```

Bất kỳ tham số tùy chọn nào cho một hàm đều phải là các tham số nằm ở cuối cùng. Việc đặt một tham số tùy chọn trước một tham số bắt buộc sẽ kích hoạt lỗi cú pháp của TypeScript:

```typescript
function announceSinger(singer?: string, song: string) {}
//                                       ~~~~
// Error: A required parameter cannot follow an optional parameter.
```

### **Các tham số mặc định (Default Parameters)**

Các tham số tùy chọn trong JavaScript có thể được cung cấp một giá trị mặc định bằng dấu `=` và một giá trị trong phần khai báo của chúng. Đối với các tham số tùy chọn này, vì một giá trị đã được cung cấp theo mặc định, nên kiểu TypeScript của chúng không có union `| undefined` được thêm vào một cách ngầm định bên trong hàm. TypeScript vẫn sẽ cho phép hàm được gọi với các đối số bị thiếu hoặc là `undefined` cho các tham số đó.

Khả năng suy luận kiểu của TypeScript hoạt động tương tự đối với các giá trị tham số hàm mặc định như đối với các giá trị biến ban đầu. Nếu một tham số có giá trị mặc định và không có chú thích kiểu, TypeScript sẽ suy luận kiểu của tham số dựa trên giá trị mặc định đó.

Trong hàm `rateSong` sau đây, `rating` được suy luận là có kiểu `number`, nhưng lại là một `number | undefined` tùy chọn trong mã gọi hàm:

```typescript
function rateSong(song: string, rating = 0) {
  console.log(`${song} gets ${rating}/5 stars!`);
}
rateSong("Photograph");  // Ok
rateSong("Set Fire to the Rain", 5);  // Ok
rateSong("Set Fire to the Rain", undefined);  // Ok
rateSong("At Last!", "100");
//                   ~~~~~
// Error: Argument of type '"100"' is not assignable
// to parameter of type 'number | undefined'.
```

### **Các tham số còn lại (Rest Parameters)**

Một số hàm trong JavaScript được tạo ra để có thể được gọi với bất kỳ số lượng đối số nào. Toán tử spread `...` có thể được đặt trên tham số cuối cùng trong khai báo hàm để biểu thị rằng bất kỳ đối số “còn lại” (rest) nào được truyền vào hàm bắt đầu từ tham số đó đều phải được lưu trữ trong một mảng duy nhất.

TypeScript cho phép khai báo kiểu của các tham số rest này tương tự như các tham số thông thường, ngoại trừ có thêm cú pháp `[]` ở cuối để chỉ ra rằng đó là một mảng các đối số.

Ở đây, `singAllTheSongs` được phép nhận 0 hoặc nhiều đối số có kiểu `string` cho tham số rest `songs` của nó:

```typescript
function singAllTheSongs(singer: string, ...songs: string[]) {
  for (const song of songs) {
    console.log(`${song}, by ${singer}`);
  }
}
singAllTheSongs("Alicia Keys");  // Ok
singAllTheSongs("Lady Gaga", "Bad Romance", "Just Dance", "Poker Face");  // Ok
singAllTheSongs("Ella Fitzgerald", 2000);
//                                 ~~~~
// Error: Argument of type 'number' is not
// assignable to parameter of type 'string'.
```

Tôi sẽ đề cập đến việc làm việc với mảng trong TypeScript trong Chương 6, “Mảng”.

## **Kiểu trả về (Return Types)**

TypeScript rất nhạy bén: nếu nó hiểu tất cả các giá trị có thể được trả về bởi một hàm, nó sẽ biết hàm đó trả về kiểu gì. Trong ví dụ này, `singSongs` được TypeScript hiểu là trả về một `number`:

```typescript
// Type: (songs: string[]) => number
function singSongs(songs: string[]) {
  for (const song of songs) {
    console.log(`${song}`);
  }
  return songs.length;
}
```

Nếu một hàm chứa nhiều câu lệnh `return` với các giá trị khác nhau, TypeScript sẽ suy luận kiểu trả về là một union của tất cả các kiểu được trả về có thể có.

Hàm `getSongAt` này sẽ được suy luận là trả về `string | undefined` vì hai giá trị được trả về tiềm năng của nó lần lượt có kiểu là `string` và `undefined`:

```typescript
// Type: (songs: string[], index: number) => string | undefined
function getSongAt(songs: string[], index: number) {
  return index < songs.length
    ? songs[index]
    : undefined;
}
```

### **Kiểu trả về tường minh (Explicit Return Types)**

Cũng như với các biến, tôi thường khuyên bạn không cần phải bận tâm khai báo tường minh kiểu trả về của các hàm bằng các chú thích kiểu. Tuy nhiên, có một vài trường hợp mà việc này có thể đặc biệt hữu ích cho các hàm:

- Bạn có thể muốn bắt buộc các hàm có nhiều giá trị trả về tiềm năng luôn phải trả về cùng một kiểu giá trị.
- TypeScript sẽ từ chối suy luận kiểu trả về của các hàm đệ quy (recursive functions).
- Nó có thể tăng tốc độ kiểm tra kiểu của TypeScript trong các dự án rất lớn—nghĩa là những dự án có hàng trăm tệp TypeScript trở lên.

Các chú thích kiểu trả về trong khai báo hàm được đặt sau dấu ngoặc đóng `)` theo sau danh sách tham số.

Đối với khai báo hàm thông thường, vị trí đó nằm ngay trước dấu `{`:

```typescript
function singSongsRecursive(songs: string[], count = 0): number {
  return songs.length ? singSongsRecursive(songs.slice(1), count + 1) : count;
}
```

Đối với các arrow functions (còn được gọi là lambdas), vị trí đó nằm ngay trước dấu `=>`:

```typescript
const singSongsRecursive = (songs: string[], count = 0): number =>
  songs.length ? singSongsRecursive(songs.slice(1), count + 1) : count;
```

Nếu một câu lệnh `return` trong một hàm trả về một giá trị không thể gán cho kiểu trả về của hàm, TypeScript sẽ đưa ra một cảnh báo assignability. Ở đây, hàm `getSongRecordingDate` được khai báo tường minh là trả về `Date | undefined`, nhưng một trong các câu lệnh return của nó lại cung cấp một `string` một cách không chính xác:

```typescript
function getSongRecordingDate(song: string): Date | undefined {
  switch (song) {
    case "Strange Fruit":
      return new Date('April 20, 1939');  // Ok
    case "Greensleeves":
      return "unknown";
// Error: Type 'string' is not assignable to type 'Date'.
    default:
      return undefined;  // Ok
  }
}
```

## **Kiểu hàm (Function Types)**

JavaScript cho phép chúng ta truyền các hàm đi xung quanh như các giá trị. Điều đó có nghĩa là chúng ta cần một cách để khai báo kiểu của một tham số hoặc biến được dự định để chứa một hàm.

Cú pháp kiểu hàm trông tương tự như một arrow function, nhưng có một kiểu dữ liệu thay vì thân hàm.

Kiểu của biến `nothingInGivesString` này mô tả một hàm không có tham số và trả về một giá trị `string`:

```typescript
let nothingInGivesString: () => string;
```

Kiểu của biến `inputAndOutput` này mô tả một hàm có một tham số `string[]`, một tham số tùy chọn `count`, và trả về một giá trị `number`:

```typescript
let inputAndOutput: (songs: string[], count?: number) => number;
```

Các kiểu hàm thường được sử dụng để mô tả các tham số callback (các tham số được dự định để gọi như các hàm).

Ví dụ, đoạn mã `runOnSongs` sau đây khai báo kiểu của tham số `getSongAt` của nó là một hàm nhận vào một `index: number` và trả về một `string`. Việc truyền `getSongAt` khớp với kiểu đó, nhưng `logSong` thất bại vì nhận vào một `string` làm tham số của nó thay vì một `number`:

```typescript
const songs = ["Juice", "Shake It Off", "What's Up"];
function runOnSongs(getSongAt: (index: number) => string) {
  for (let i = 0; i < songs.length; i += 1) {
    console.log(getSongAt(i));
  }
}
function getSongAt(index: number) {
  return `${songs[index]}`;
}
runOnSongs(getSongAt);  // Ok
function logSong(song: string) {
  return `${song}`;
}
runOnSongs(logSong);
//         ~~~~~~~
// Error: Argument of type '(song: string) => string' is not
// assignable to parameter of type '(index: number) => string'.
//   Types of parameters 'song' and 'index' are incompatible.
//     Type 'number' is not assignable to type 'string'.
```

Thông báo lỗi cho `runOnSongs(logSong)` là một ví dụ về lỗi assignability bao gồm một vài mức độ chi tiết. Khi phàn nàn rằng hai kiểu hàm không thể gán cho nhau, TypeScript thường sẽ đưa ra ba mức độ chi tiết với mức độ cụ thể tăng dần:

1. Mức thụt lề đầu tiên in ra hai kiểu hàm.
2. Mức thụt lề tiếp theo chỉ định phần nào không khớp nhau.
3. Mức thụt lề cuối cùng là thông báo lỗi assignability chính xác của phần không khớp nhau.

Trong đoạn mã trước, các mức độ đó là:

1. Kiểu của `logSong`: `(song: string) => string` là kiểu được cung cấp đang được gán cho nơi tiếp nhận `getSongAt: (index: number) => string`
2. Tham số `song` của `logSong` đang được gán cho tham số `index` của `getSongAt`
3. Kiểu `number` của `song` không thể gán cho kiểu `string` của `index`

#### **MẸO (TIP)**

Các lỗi nhiều dòng của TypeScript thoạt nhìn có vẻ nản lòng. Việc đọc qua chúng từng dòng một và hiểu những gì mỗi phần đang truyền tải sẽ giúp bạn nắm bắt lỗi dễ dàng hơn rất nhiều.

### **Dấu ngoặc đơn trong kiểu hàm (Function Type Parentheses)**

Kiểu hàm có thể được đặt ở bất kỳ nơi nào mà một kiểu khác có thể được sử dụng. Điều đó bao gồm cả union types.

Trong union types, dấu ngoặc đơn có thể được sử dụng để chỉ ra phần nào của chú thích là giá trị trả về của hàm hoặc là union type bao quanh:

```typescript
// Type is a function that returns a union: string | undefined
let returnsStringOrUndefined: () => string | undefined;

// Type is either undefined or a function that returns a string
let maybeReturnsString: (() => string) | undefined;
```

Các chương sau giới thiệu nhiều cú pháp kiểu hơn sẽ hiển thị các vị trí khác nơi các kiểu hàm phải được bọc trong dấu ngoặc đơn.

### **Suy luận kiểu tham số (Parameter Type Inferences)**

Sẽ rất rườm rà nếu chúng ta phải khai báo kiểu tham số cho mọi hàm chúng ta viết, bao gồm cả các hàm inline được sử dụng làm tham số. May mắn thay, TypeScript có thể suy luận kiểu của các tham số trong một hàm được cung cấp cho một vị trí có kiểu đã được khai báo.

Biến `singer` này được biết là một hàm nhận vào một tham số có kiểu `string`, vì vậy tham số `song` trong hàm sau đó được gán cho `singer` được biết là một `string`:

```typescript
let singer: (song: string) => string;
singer = function (song) {
  // Type of song: string
  return `Singing: ${song.toUpperCase()}!`;  // Ok
};
```

Các hàm được truyền dưới dạng đối số cho các tham số có kiểu tham số hàm cũng sẽ được suy luận kiểu tham số của chúng.

Ví dụ: các tham số `song` và `index` ở đây được TypeScript suy luận lần lượt là `string` và `number`:

```typescript
const songs = ["Call Me", "Jolene", "The Chain"];
// song: string
// index: number
songs.forEach((song, index) => {
  console.log(`${song} is at index ${index}`);
});
```

### **Bí danh kiểu hàm (Function Type Aliases)**

Bạn còn nhớ type aliases từ Chương 3, “Unions và Literals” không? Chúng cũng có thể được sử dụng cho các kiểu hàm.

Kiểu `StringToNumber` này đặt bí danh cho một hàm nhận vào một `string` và trả về một `number`, có nghĩa là sau này nó có thể được sử dụng để mô tả kiểu của các biến:

```typescript
type StringToNumber = (input: string) => number;
let stringToNumber: StringToNumber;
stringToNumber = (input) => input.length;  // Ok
stringToNumber = (input) => input.toUpperCase();
//                          ~~~~~~~~~~~~~~~~~~~
// Error: Type 'string' is not assignable to type 'number'.
```

Tương tự, các tham số hàm bản thân chúng có thể được định kiểu bằng các bí danh tình cờ tham chiếu đến một kiểu hàm.

Hàm `usesNumberToString` này có một tham số duy nhất bản thân nó là kiểu hàm có bí danh `NumberToString`:

```typescript
type NumberToString = (input: number) => string;

function usesNumberToString(numberToString: NumberToString) {
  console.log(`The string is: ${numberToString(1234)}`);
}
usesNumberToString((input) => `${input}! Hooray!`);  // Ok
usesNumberToString((input) => input * 2);
//                            ~~~~~~~~~
// Error: Type 'number' is not assignable to type 'string'.
```

Type aliases đặc biệt hữu ích cho các kiểu hàm. Chúng có thể tiết kiệm rất nhiều không gian theo chiều ngang thay vì phải liên tục viết ra các tham số và/hoặc kiểu trả về.

## **Các kiểu trả về khác (More Return Types)**

Bây giờ, hãy cùng xem xét hai kiểu trả về nữa: `void` và `never`.

### **Trả về kiểu void (Void Returns)**

Một số hàm không được dự định để trả về bất kỳ giá trị nào. Chúng hoặc không có câu lệnh `return` nào hoặc chỉ có các câu lệnh `return` không trả về giá trị. TypeScript cho phép sử dụng từ khóa `void` để chỉ kiểu trả về của một hàm như vậy không trả về gì.

Các hàm có kiểu trả về là `void` không được phép trả về một giá trị. Hàm `logSong` này được khai báo là trả về `void`, vì vậy nó không được phép trả về một giá trị:

```typescript
function logSong(song: string | undefined): void {
  if (!song) {
    return;  // Ok
  }

  console.log(`${song}`);

  return true;
  // Error: Type 'boolean' is not assignable to type 'void'.
}
```

`void` có thể hữu ích như kiểu trả về trong một khai báo kiểu hàm. Khi được sử dụng trong một khai báo kiểu hàm, `void` chỉ ra rằng bất kỳ giá trị nào được trả về từ hàm đó đều sẽ bị bỏ qua.

Ví dụ: biến `songLogger` này đại diện cho một hàm nhận vào một `song: string` và không trả về giá trị nào:

```typescript
let songLogger: (song: string) => void;
songLogger = (song) => {
  console.log(`${song}`);
};
songLogger("Heart of Glass");  // Ok
```

Lưu ý rằng mặc dù tất cả các hàm JavaScript đều trả về `undefined` theo mặc định nếu không có giá trị thực tế nào được trả về, nhưng `void` không giống với `undefined`. `void` có nghĩa là kiểu trả về của một hàm sẽ bị bỏ qua, trong khi `undefined` là một giá trị literal được trả về. Cố gắng gán một giá trị có kiểu `void` cho một giá trị có kiểu bao gồm `undefined` là một lỗi kiểu:

```typescript
function returnsVoid() {
  return;
}
let lazyValue: string | undefined;
lazyValue = returnsVoid();
// Error: Type 'void' is not assignable to type 'string | undefined'.
```

Sự phân biệt giữa kiểu trả về `undefined` và `void` đặc biệt hữu ích cho việc bỏ qua bất kỳ giá trị nào được trả về từ một hàm được truyền vào một vị trí có kiểu được khai báo là trả về `void`. Ví dụ: phương thức tích hợp sẵn `forEach` trên mảng nhận vào một callback trả về `void`. Các hàm được cung cấp cho `forEach` có thể trả về bất kỳ giá trị nào chúng muốn. `records.push(record)` trong hàm `saveRecords` sau đây trả về một `number` (giá trị được trả về từ phương thức `.push()` của mảng), nhưng vẫn được phép là giá trị trả về cho arrow function được truyền vào `newRecords.forEach`:

```typescript
const records: string[] = [];

function saveRecords(newRecords: string[]) {
  newRecords.forEach(record => records.push(record));
}

saveRecords(['21', 'Come On Over', 'The Bodyguard'])
```

Kiểu `void` không phải là JavaScript. Nó là một từ khóa TypeScript được sử dụng để khai báo kiểu trả về của các hàm. Hãy nhớ rằng, nó là dấu hiệu cho thấy giá trị trả về của hàm không được dự định để sử dụng, chứ không phải là một giá trị bản thân nó có thể được trả về.

### **Trả về kiểu never (Never Returns)**

Một số hàm không chỉ không trả về giá trị nào, mà còn không bao giờ được dự định sẽ trả về. Các hàm không bao giờ trả về là những hàm luôn ném ra lỗi hoặc chạy một vòng lặp vô hạn (hy vọng là có chủ ý!).

Nếu một hàm được dự định là không bao giờ trả về, việc thêm một chú thích kiểu `: never` tường minh cho biết rằng bất kỳ mã nào phía sau lời gọi hàm đó sẽ không bao giờ được chạy. Hàm `fail` này luôn chỉ ném ra lỗi, vì vậy nó có thể giúp phân tích luồng điều khiển của TypeScript trong việc thu hẹp kiểu của `param` thành `string`:

```typescript
function fail(message: string): never {
  throw new Error(`Invariant failure: ${message}.`);
}

function workWithUnsafeParam(param: unknown) {
  if (typeof param !== "string") {
    fail(`param should be a string, not ${typeof param}`);
  }

  // Here, param is known to be type string
  param.toUpperCase();  // Ok
}
```

#### **GHI CHÚ (NOTE)**

`never` không giống với `void`. `void` dành cho một hàm không trả về gì. `never` dành cho một hàm không bao giờ trả về.

## **Nạp chồng hàm (Function Overloads)**

Một số hàm JavaScript có thể được gọi với các tập hợp tham số hoàn toàn khác nhau mà không thể biểu diễn chỉ bằng các tham số tùy chọn và/hoặc rest parameters. Những hàm này có thể được mô tả bằng một cú pháp TypeScript gọi là _overload signatures_ (chữ ký nạp chồng): khai báo các phiên bản khác nhau về tên hàm, tham số và kiểu trả về nhiều lần trước một _implementation signature_ (chữ ký triển khai) cuối cùng và thân của hàm.

Khi xác định xem có phát ra lỗi cú pháp cho một lời gọi hàm nạp chồng hay không, TypeScript sẽ chỉ nhìn vào các overload signatures của hàm. Chữ ký triển khai chỉ được sử dụng bởi logic nội bộ của hàm.

Hàm `createDate` này được dự định gọi hoặc với một tham số `timestamp` hoặc với ba tham số—`month`, `day` và `year`. Việc gọi với một trong hai số lượng đối số đó đều được cho phép, nhưng việc gọi với hai đối số sẽ gây ra lỗi kiểu vì không có overload signature nào cho phép hai đối số. Trong ví dụ này, hai dòng đầu tiên là các overload signatures, và dòng thứ ba là implementation signature:

```typescript
function createDate(timestamp: number): Date;
function createDate(month: number, day: number, year: number): Date;
function createDate(monthOrTimestamp: number, day?: number, year?: number) {
  return day === undefined || year === undefined
    ? new Date(monthOrTimestamp)
    : new Date(year, monthOrTimestamp, day);
}

createDate(554356800);  // Ok
createDate(7, 27, 1987);  // Ok

createDate(4, 1);

// Error: No overload expects 2 arguments, but overloads
// do exist that expect either 1 or 3 arguments.
```

Các overload signatures, cũng như các cú pháp hệ thống kiểu khác, sẽ bị xóa bỏ khi biên dịch TypeScript sang JavaScript đầu ra.

Hàm trong đoạn mã trước sẽ biên dịch thành đoạn JavaScript gần giống như sau:

```typescript
function createDate(monthOrTimestamp, day, year) {
  return day === undefined || year === undefined
    ? new Date(monthOrTimestamp)
    : new Date(year, monthOrTimestamp, day);
}
```

#### **CẢNH BÁO (WARNING)**

Function overloads nhìn chung được sử dụng như một giải pháp cuối cùng cho các kiểu hàm phức tạp, khó mô tả. Tốt hơn hết là bạn nên giữ cho các hàm đơn giản và tránh sử dụng function overloads khi có thể.

### **Tính tương thích của Call-Signature (Call-Signature Compatibility)**

Chữ ký triển khai được sử dụng cho việc triển khai của một hàm nạp chồng là thứ mà việc triển khai hàm sử dụng cho các kiểu tham số và kiểu trả về. Do đó, kiểu trả về và từng tham số trong các overload signatures của một hàm phải có thể gán được cho tham số ở cùng chỉ số trong implementation signature của nó. Nói cách khác, implementation signature phải tương thích với tất cả các overload signatures.

Implementation signature của hàm `format` này khai báo tham số đầu tiên của nó là một `string`. Trong khi hai overload signatures đầu tiên tương thích vì cũng có kiểu `string`, thì overload signature thứ ba có kiểu `() => string` lại không tương thích:

```typescript
function format(data: string): string;  // Ok
function format(data: string, needle: string, haystack: string): string;  // Ok
function format(getData: () => string): string;
//       ~~~~~~
// This overload signature is not compatible with its implementation signature.

function format(data: string, needle?: string, haystack?: string) {
  return needle && haystack ? data.replace(needle, haystack) : data;
}
```

## **Tổng kết**

Trong chương này, bạn đã thấy cách các tham số và kiểu trả về của một hàm có thể được suy luận hoặc khai báo tường minh trong TypeScript:

- Khai báo kiểu tham số hàm với type annotations
- Khai báo các tham số tùy chọn, giá trị mặc định và rest parameters để thay đổi hành vi của hệ thống kiểu
- Khai báo kiểu trả về của hàm với type annotations
- Mô tả các hàm không trả về giá trị có thể sử dụng được bằng kiểu `void`
- Mô tả các hàm không bao giờ trả về bằng kiểu `never`
- Sử dụng function overloads để mô tả các chữ ký gọi hàm khác nhau

#### **MẸO (TIP)**

Bây giờ bạn đã đọc xong chương này, hãy thực hành những gì bạn đã học tại _https://learningtypescript.com/functions_.

_Điều gì làm nên một dự án TypeScript tốt?_

_Nó hoạt động (functions) tốt._
