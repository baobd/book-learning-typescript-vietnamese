# **Chương 4. Đối tượng (Objects)**

## Mục lục

- [**Chương 4. Đối tượng (Objects)**](#chương-4-đối-tượng-objects)
  - [**Kiểu đối tượng (Object Types)**](#kiểu-đối-tượng-object-types)
    - [**Khai báo kiểu đối tượng**](#khai-báo-kiểu-đối-tượng)
    - [**Bí danh kiểu đối tượng (Aliased Object Types)**](#bí-danh-kiểu-đối-tượng-aliased-object-types)
      - [**GHI CHÚ (NOTE)**](#ghi-chú-note)
  - [**Định kiểu theo cấu trúc (Structural Typing)**](#định-kiểu-theo-cấu-trúc-structural-typing)
    - [**Kiểm tra tính tương thích khi sử dụng (Usage Checking)**](#kiểm-tra-tính-tương-thích-khi-sử-dụng-usage-checking)
    - [**Kiểm tra thuộc tính dư thừa (Excess Property Checking)**](#kiểm-tra-thuộc-tính-dư-thừa-excess-property-checking)
    - [**Kiểu đối tượng lồng nhau (Nested Object Types)**](#kiểu-đối-tượng-lồng-nhau-nested-object-types)
      - [**MẸO (TIP)**](#mẹo-tip)
    - [**Thuộc tính tùy chọn (Optional Properties)**](#thuộc-tính-tùy-chọn-optional-properties)
  - [**Union của các kiểu đối tượng (Unions of Object Types)**](#union-của-các-kiểu-đối-tượng-unions-of-object-types)
    - [**Union kiểu đối tượng được suy luận (Inferred Object-Type Unions)**](#union-kiểu-đối-tượng-được-suy-luận-inferred-object-type-unions)
    - [**Union kiểu đối tượng tường minh (Explicit Object-Type Unions)**](#union-kiểu-đối-tượng-tường-minh-explicit-object-type-unions)
    - [**Thu hẹp kiểu đối tượng (Narrowing Object Types)**](#thu-hẹp-kiểu-đối-tượng-narrowing-object-types)
    - [**Union có gắn thẻ phân biệt (Discriminated Unions)**](#union-có-gắn-thẻ-phân-biệt-discriminated-unions)
  - [**Kiểu giao (Intersection Types)**](#kiểu-giao-intersection-types)
    - [**Những cạm bẫy của Intersection Types**](#những-cạm-bẫy-của-intersection-types)
      - [**Thông báo lỗi assignability dài dòng**](#thông-báo-lỗi-assignability-dài-dòng)
      - [**Kiểu never**](#kiểu-never)
  - [**Tổng kết**](#tổng-kết)
    - [**MẸO (TIP)**](#mẹo-tip-1)

_Các đối tượng literal_

_Một tập hợp các key và value_

_Mỗi thứ đều có kiểu riêng của mình_

Chương 3, “Unions và Literals” đã trình bày chi tiết về union types và literal types: làm việc với các kiểu nguyên thủy như `boolean` và các giá trị cụ thể của chúng như `true`. Những kiểu nguyên thủy đó chỉ mới chạm tới bề nổi của các hình dạng đối tượng phức tạp mà mã JavaScript thường xuyên sử dụng. TypeScript sẽ rất khó sử dụng nếu nó không thể biểu diễn được các đối tượng đó. Chương này sẽ trình bày cách mô tả các hình dạng đối tượng phức tạp và cách TypeScript kiểm tra khả năng gán (assignability) của chúng.

## **Kiểu đối tượng (Object Types)**

Khi bạn tạo một object literal bằng cú pháp `{...}`, TypeScript sẽ coi nó là một kiểu đối tượng mới, hay hình dạng kiểu (type shape), dựa trên các thuộc tính của nó. Kiểu đối tượng đó sẽ có cùng tên thuộc tính và kiểu nguyên thủy như các giá trị của đối tượng. Việc truy cập các thuộc tính của giá trị có thể được thực hiện bằng cú pháp `value.member` hoặc cú pháp tương đương `value['member']`.

TypeScript hiểu rằng biến `poet` sau đây có kiểu là một đối tượng với hai thuộc tính: `born`, có kiểu `number`, và `name`, có kiểu `string`. Việc truy cập các thành viên đó sẽ được cho phép, nhưng việc cố gắng truy cập bất kỳ tên thành viên nào khác sẽ gây ra lỗi kiểu vì tên đó không tồn tại:

```typescript
const poet = {
  born: 1935,
  name: "Mary Oliver",
};

poet['born'];  // Type: number
poet.name;  // Type: string
poet.end;
//   ~~~
// Error: Property 'end' does not exist on
// type '{ name: string; start: number; }'.
```

Object types là một khái niệm cốt lõi về cách TypeScript hiểu mã JavaScript. Mọi giá trị ngoại trừ `null` và `undefined` đều có một tập hợp các thành viên trong hình dạng kiểu nền tảng của nó, và do đó TypeScript phải hiểu kiểu đối tượng cho mọi giá trị để có thể kiểm tra kiểu cho nó.

### **Khai báo kiểu đối tượng**

Suy luận kiểu trực tiếp từ các đối tượng hiện có là rất tốt, nhưng cuối cùng bạn sẽ muốn có thể khai báo kiểu của một đối tượng một cách tường minh. Bạn sẽ cần một cách để mô tả hình dạng của một đối tượng tách biệt khỏi các đối tượng thỏa mãn nó.

Các kiểu đối tượng có thể được mô tả bằng cú pháp trông tương tự như object literals nhưng sử dụng các kiểu thay vì các giá trị cho các trường. Đó cũng chính là cú pháp mà TypeScript hiển thị trong các thông báo lỗi về khả năng gán kiểu.

Biến `poetLater` này có cùng kiểu như trước với `name: string` và `born: number`:

```typescript
let poetLater: {
born: number;
name: string;
};
// Ok
poetLater= {
born: 1935,
name: "Mary Oliver",
};
poetLater = "Sappho";
// Error: Type 'string' is not assignable to
// type '{ born: number; name: string; }'
```

### **Bí danh kiểu đối tượng (Aliased Object Types)**

Việc liên tục viết ra các kiểu đối tượng như `{ born: number; name: string; }` sẽ nhanh chóng trở nên nhàm chán. Cách phổ biến hơn là sử dụng type aliases để gán cho mỗi hình dạng kiểu một cái tên.

Đoạn mã trước có thể được viết lại với `type Poet`, điều này mang lại thêm lợi ích là làm cho thông báo lỗi assignability của TypeScript trở nên trực tiếp và dễ đọc hơn một chút:

```typescript
type Poet= {
born: number;
name: string;
};
let poetLater: Poet;
// Ok
poetLater= {
born: 1935,
name: "Sara Teasdale",
};
poetLater = "Emily Dickinson";
// Error: Type 'string' is not assignable to 'Poet'.
```

#### **GHI CHÚ (NOTE)**

Hầu hết các dự án TypeScript thích sử dụng từ khóa `interface` để mô tả các kiểu đối tượng, một tính năng mà tôi sẽ đề cập trong Chương 7, “Interfaces”. Các kiểu đối tượng có bí danh (aliased object types) và interfaces gần như giống hệt nhau: mọi thứ trong chương này cũng đều áp dụng cho interfaces.

Tôi đề cập đến các kiểu đối tượng này ngay bây giờ vì việc hiểu cách TypeScript diễn giải object literals là một phần quan trọng trong việc tìm hiểu hệ thống kiểu của TypeScript. Những khái niệm này sẽ tiếp tục đóng vai trò quan trọng khi chúng ta chuyển sang các tính năng trong phần tiếp theo của cuốn sách này.

## **Định kiểu theo cấu trúc (Structural Typing)**

Hệ thống kiểu của TypeScript được _định kiểu theo cấu trúc_ (structurally typed): nghĩa là bất kỳ giá trị nào tình cờ thỏa mãn một kiểu đều được phép sử dụng như một giá trị của kiểu đó. Nói cách khác, khi bạn khai báo một tham số hoặc biến có một kiểu đối tượng cụ thể, bạn đang thông báo cho TypeScript biết rằng bất kể bạn sử dụng (các) đối tượng nào, chúng cần phải có những thuộc tính đó.

Các kiểu đối tượng có bí danh `WithFirstName` và `WithLastName` sau đây đều chỉ khai báo một thành viên duy nhất có kiểu `string`. Biến `hasBoth` tình cờ có cả hai thành viên đó—mặc dù nó không được khai báo tường minh như vậy—vì vậy nó có thể được cung cấp cho các biến được khai báo là một trong hai kiểu đối tượng có bí danh:

```typescript
type WithFirstName= {
firstName: string;
};
type WithLastName= {
lastName: string;
};
const hasBoth= {
firstName: "Lucille",
lastName: "Clifton",
};
// Ok: `hasBoth` contains a `firstName` property of type `string`
let withFirstName: WithFirstName = hasBoth;

// Ok: `hasBoth` contains a `lastName` property of type `string`
let withLastName: WithLastName = hasBoth;
```

Định kiểu theo cấu trúc không giống với _duck typing_ (định kiểu vịt), bắt nguồn từ câu nói “Nếu nó trông giống con vịt và kêu quác quác như con vịt, thì nó có lẽ là một con vịt.”

- Định kiểu theo cấu trúc là khi có một hệ thống tĩnh kiểm tra kiểu—trong trường hợp của TypeScript, đó là bộ kiểm tra kiểu (type checker).
- Duck typing là khi không có gì kiểm tra kiểu đối tượng cho đến khi chúng được sử dụng tại thời gian chạy (runtime).

Tóm lại: _JavaScript_ là _duck typed_ trong khi _TypeScript_ là _structurally typed_.

### **Kiểm tra tính tương thích khi sử dụng (Usage Checking)**

Khi cung cấp một giá trị cho một vị trí được chú thích bằng một kiểu đối tượng, TypeScript sẽ kiểm tra xem giá trị đó có thể gán được cho kiểu đối tượng đó hay không. Để bắt đầu, giá trị phải có các thuộc tính bắt buộc của kiểu đối tượng. Nếu bất kỳ thành viên nào được yêu cầu trên kiểu đối tượng bị thiếu trong đối tượng, TypeScript sẽ đưa ra lỗi kiểu.

Kiểu đối tượng có bí danh `FirstAndLastNames` sau đây yêu cầu cả hai thuộc tính `first` và `last` phải tồn tại. Một đối tượng chứa cả hai thuộc tính đó được phép sử dụng trong một biến được khai báo là có kiểu `FirstAndLastNames`, nhưng một đối tượng không có chúng thì không:

```typescript
type FirstAndLastNames= {
first: string;
last: string;
};
// Ok
const hasBoth: FirstAndLastNames= {
first: "Sarojini",
last: "Naidu",
};
const hasOnlyOne: FirstAndLastNames= {
first: "Sappho"
};
// Property 'last' is missing in type '{ first: string; }'
// but required in type 'FirstAndLastNames'.
```

Các kiểu dữ liệu không khớp giữa hai bên cũng không được phép. Các kiểu đối tượng chỉ định cả tên của các thuộc tính bắt buộc và kiểu mà các thuộc tính đó được mong đợi. Nếu một thuộc tính của đối tượng không khớp kiểu, TypeScript sẽ báo cáo lỗi kiểu.

Kiểu `TimeRange` sau đây mong đợi thành viên `start` có kiểu `Date`. Đối tượng `hasStartString` đang gây ra lỗi kiểu vì `start` của nó lại là kiểu `string`:

```typescript
type TimeRange= {
start: Date;
};
const hasStartString: TimeRange= {
start: "1879-02-13",
// Error: Type 'string' is not assignable to type 'Date'.
};
```

### **Kiểm tra thuộc tính dư thừa (Excess Property Checking)**

TypeScript sẽ báo cáo lỗi kiểu nếu một biến được khai báo với một kiểu đối tượng và giá trị ban đầu của nó có nhiều trường hơn những gì kiểu của nó mô tả. Do đó, khai báo một biến có một kiểu đối tượng là một cách để yêu cầu bộ kiểm tra kiểu đảm bảo rằng nó chỉ có các trường dự kiến trên kiểu đó.

Biến `poetMatch` sau đây có chính xác các trường được mô tả trong kiểu đối tượng có bí danh `Poet`, trong khi `extraProperty` gây ra lỗi kiểu vì có thêm một thuộc tính thừa:

```typescript
type Poet= {
born: number;
name: string;
}
// Ok: all fields match what's expected in Poet
const poetMatch: Poet= {
born: 1928,
name: "Maya Angelou"
};
const extraProperty: Poet= {
activity: "walking",
born: 1935,
name: "Mary Oliver",
};
// Error: Type '{ activity: string; born: number; name: string; }'
// is not assignable to type 'Poet'.
//   Object literal may only specify known properties,
//   and 'activity' does not exist in type 'Poet'.
```

Lưu ý rằng việc kiểm tra thuộc tính dư thừa chỉ kích hoạt cho các object literals đang được tạo ra tại các vị trí được khai báo là một kiểu đối tượng. Việc cung cấp một object literal đã tồn tại từ trước sẽ bỏ qua kiểm tra thuộc tính dư thừa.

Biến `extraPropertyButOk` này không kích hoạt lỗi kiểu với kiểu `Poet` của ví dụ trước vì giá trị ban đầu của nó tình cờ khớp về mặt cấu trúc với `Poet`:

```typescript
const existingObject = {
  activity: "walking",
  born: 1935,
  name: "Mary Oliver",
};

const extraPropertyButOk: Poet = existingObject;  // Ok
```

Kiểm tra thuộc tính dư thừa sẽ kích hoạt ở bất kỳ nơi nào một đối tượng mới đang được tạo tại vị trí mong đợi nó khớp với một kiểu đối tượng—điều mà như bạn sẽ thấy trong các chương sau bao gồm các phần tử của mảng, các trường của class và các tham số hàm. Cấm các thuộc tính dư thừa là một cách khác mà TypeScript giúp đảm bảo mã của bạn sạch sẽ và hoạt động đúng như mong đợi. Các thuộc tính dư thừa không được khai báo trong kiểu đối tượng thường là tên thuộc tính bị gõ sai chính tả hoặc mã không còn sử dụng.

### **Kiểu đối tượng lồng nhau (Nested Object Types)**

Vì các đối tượng JavaScript có thể được lồng vào nhau như các thành viên của các đối tượng khác, nên các kiểu đối tượng của TypeScript phải có khả năng biểu diễn các kiểu đối tượng lồng nhau trong hệ thống kiểu. Cú pháp để làm điều đó cũng giống như trước nhưng với một kiểu đối tượng `{ ... }` thay vì tên kiểu nguyên thủy.

Kiểu `Poem` được khai báo là một đối tượng có thuộc tính `author` chứa `firstName: string` và `lastName: string`. Biến `poemMatch` có thể gán được cho `Poem` vì nó khớp với cấu trúc đó, trong khi `poemMismatch` thì không vì thuộc tính `author` của nó chứa `name` thay vì `firstName` và `lastName`:

```typescript
type Poem = {
  author: {
    firstName: string;
    lastName: string;
  };
  name: string;
};
// Ok
const poemMatch: Poem = {
  author: {
    firstName: "Sylvia",
    lastName: "Plath",
  },
  name: "Lady Lazarus",
};
const poemMismatch: Poem = {
  author: {
    name: "Sylvia Plath",
  },
// Error: Type '{ name: string; }' is not assignable
// to type '{ firstName: string; lastName: string; }'.
//   Object literal may only specify known properties, and 'name'
//   does not exist in type '{ firstName: string; lastName: string; }'.
  name: "Tulips",
};
```

Một cách khác để viết `type Poem` là trích xuất hình dạng của thuộc tính `author` thành kiểu đối tượng có bí danh riêng của nó, `Author`. Việc trích xuất các kiểu lồng nhau thành các type aliases riêng cũng giúp TypeScript đưa ra các thông báo lỗi kiểu giàu thông tin hơn. Trong trường hợp này, nó có thể thông báo `'Author'` thay vì `'{ firstName: string; lastName: string; }'`:

```typescript
type Author= {
firstName: string;
lastName: string;
};
type Poem= {
author: Author;
name: string;
};
const poemMismatch: Poem= {
author: {
name: "Sylvia Plath",

    },
// Error: Type '{ name: string; }' is not assignable to type 'Author'.
//     Object literal may only specify known properties,
//     and 'name' does not exist in type 'Author'.
name: "Tulips",
};
```

#### **MẸO (TIP)**

Nhìn chung, việc tách các kiểu đối tượng lồng nhau thành tên kiểu riêng như thế này là một ý kiến hay, vừa giúp mã dễ đọc hơn vừa giúp thông báo lỗi của TypeScript dễ hiểu hơn.

Bạn sẽ thấy trong các chương sau cách các thành viên của kiểu đối tượng có thể là các kiểu khác như mảng và hàm.

### **Thuộc tính tùy chọn (Optional Properties)**

Không phải tất cả các thuộc tính của kiểu đối tượng đều bắt buộc phải có trong đối tượng. Bạn có thể thêm dấu `?` trước dấu `:` trong chú thích kiểu của thuộc tính để biểu thị rằng đó là một thuộc tính tùy chọn.

Kiểu `Book` này chỉ yêu cầu thuộc tính `pages` và cho phép tùy chọn có thêm `author`. Các đối tượng tuân thủ theo nó có thể cung cấp `author` hoặc bỏ qua miễn là chúng cung cấp `pages`:

```typescript
type Book= {
author?: string;
pages: number;
};
// Ok
const ok: Book= {
author: "Rita Dove",
pages: 80,
};
const missing: Book= {
author: "Rita Dove",
};
// Error: Property 'pages' is missing in type
// '{ author: string; }' but required in type 'Book'.
```

Hãy nhớ rằng có sự khác biệt giữa các thuộc tính tùy chọn và các thuộc tính có kiểu tình cờ bao gồm `undefined` trong một union type. Một thuộc tính được khai báo là tùy chọn với `?` được phép không tồn tại. Một thuộc tính được khai báo là bắt buộc và có kiểu `| undefined` bắt buộc phải tồn tại, ngay cả khi giá trị là `undefined`.

Thuộc tính `editor` trong kiểu `Writers` sau đây có thể được bỏ qua khi khai báo biến vì nó có dấu `?` trong khai báo. Thuộc tính `author` không có dấu `?`, vì vậy nó bắt buộc phải tồn tại, ngay cả khi giá trị của nó chỉ là `undefined`:

```typescript
type Writers= {
author: string | undefined;
editor?: string;
};
// Ok: author is provided as undefined
const hasRequired: Writers= {
author: undefined,
};
const missingRequired: Writers= {};
//    ~~~~~~~~~~~~~~~
// Error: Property 'author' is missing in type
// '{}' but required in type 'Writers'.
```

Chương 7, “Interfaces” sẽ đề cập thêm về các loại thuộc tính khác, trong khi Chương 13, “Các tùy chọn cấu hình” sẽ mô tả các thiết lập độ nghiêm ngặt của TypeScript xung quanh các thuộc tính tùy chọn.

## **Union của các kiểu đối tượng (Unions of Object Types)**

Trong mã TypeScript, việc muốn mô tả một kiểu có thể là một hoặc nhiều kiểu đối tượng khác nhau có các thuộc tính hơi khác nhau là hoàn toàn hợp lý. Hơn nữa, mã của bạn có thể muốn thu hẹp kiểu giữa các kiểu đối tượng đó dựa trên giá trị của một thuộc tính.

### **Union kiểu đối tượng được suy luận (Inferred Object-Type Unions)**

Nếu một biến được cung cấp một giá trị ban đầu có thể là một trong nhiều kiểu đối tượng, TypeScript sẽ suy luận kiểu của nó là một union của các kiểu đối tượng. Kiểu union đó sẽ có một thành phần cấu thành cho mỗi hình dạng đối tượng có thể có. Mỗi thuộc tính có thể có trên kiểu sẽ xuất hiện trong mỗi thành phần cấu thành đó, mặc dù chúng sẽ là kiểu tùy chọn `?` trên bất kỳ kiểu nào không có giá trị ban đầu cho chúng.

Giá trị `poem` này luôn có thuộc tính `name` có kiểu `string`, và có thể có hoặc không có các thuộc tính `pages` và `rhymes`:

```typescript
const poem = Math.random() >0.5
? { name: "The Double Image", pages: 7 }
: { name: "Her Kind", rhymes: true };
// Type:
// {
//   name: string;
//   pages: number;
//   rhymes?: undefined;
// }
// |
// {
//   name: string;
//   pages?: undefined;
//   rhymes: boolean;
// }
poem.name;  // string
poem.pages;  // number | undefined
poem.rhymes;  // booleans | undefined
```

### **Union kiểu đối tượng tường minh (Explicit Object-Type Unions)**

Ngoài ra, bạn có thể chỉ định rõ ràng hơn về các kiểu đối tượng của mình bằng cách khai báo tường minh union của các kiểu đối tượng. Làm như vậy đòi hỏi phải viết thêm một chút mã nhưng mang lại lợi thế là cho bạn nhiều quyền kiểm soát hơn đối với các kiểu đối tượng của mình. Đáng chú ý nhất, nếu kiểu của một giá trị là một union của các kiểu đối tượng, hệ thống kiểu của TypeScript sẽ chỉ cho phép truy cập vào các thuộc tính tồn tại trên tất cả các kiểu union đó.

Phiên bản này của biến `poem` trước đó được định kiểu tường minh là một union type luôn có thuộc tính `name` cùng với `pages` hoặc `rhymes`. Việc truy cập `name` được phép vì nó luôn tồn tại, nhưng `pages` và `rhymes` không được đảm bảo chắc chắn sẽ tồn tại:

```typescript
type PoemWithPages= {
name: string;
pages: number;
};
type PoemWithRhymes= {
name: string;
rhymes: boolean;
};
type Poem = PoemWithPages | PoemWithRhymes;
const poem: Poem = Math.random() >0.5
? { name: "The Double Image", pages: 7 }
: { name: "Her Kind", rhymes: true };
poem.name;  // Ok
poem.pages;
//   ~~~~~
// Property 'pages' does not exist on type 'Poem'.
//   Property 'pages' does not exist on type 'PoemWithRhymes'.
poem.rhymes;
//   ~~~~~~
// Property 'rhymes' does not exist on type 'Poem'.
//   Property 'rhymes' does not exist on type 'PoemWithPages'.
```

Hạn chế quyền truy cập vào các thành viên có khả năng không tồn tại của các đối tượng là một điều tốt cho sự an toàn của mã nguồn. Nếu một giá trị có thể là một trong nhiều kiểu, các thuộc tính không tồn tại trên tất cả các kiểu đó không được đảm bảo chắc chắn sẽ tồn tại trên đối tượng.

Cũng giống như cách các union của kiểu literal và/hoặc kiểu nguyên thủy phải được thu hẹp kiểu để truy cập các thuộc tính không tồn tại trên tất cả các kiểu thành phần, bạn sẽ cần thu hẹp các union kiểu đối tượng đó.

### **Thu hẹp kiểu đối tượng (Narrowing Object Types)**

Nếu bộ kiểm tra kiểu thấy rằng một vùng mã chỉ có thể được chạy nếu một giá trị có kiểu union chứa một thuộc tính nhất định, nó sẽ thu hẹp kiểu của giá trị về chỉ những thành phần chứa thuộc tính đó. Nói cách khác, việc thu hẹp kiểu của TypeScript sẽ áp dụng cho các đối tượng nếu bạn kiểm tra hình dạng của chúng trong mã.

Tiếp tục ví dụ `poem` được định kiểu tường minh, việc kiểm tra xem `"pages" in poem` đóng vai trò như một type guard để TypeScript biết rằng đó là một `PoemWithPages`. Nếu `poem` không phải là `PoemWithPages`, thì nó chắc chắn phải là một `PoemWithRhymes`:

```typescript
if ("pages" in poem) {
  poem.pages;  // Ok: poem is narrowed to PoemWithPages
} else {
  poem.rhymes;  // Ok: poem is narrowed to PoemWithRhymes
}
```

Lưu ý rằng TypeScript sẽ không cho phép kiểm tra sự tồn tại theo tính chân lý như `if (poem.pages)`. Cố gắng truy cập một thuộc tính của một đối tượng có thể không tồn tại bị coi là một lỗi kiểu, ngay cả khi được sử dụng theo cách có vẻ giống như một type guard:

```typescript
if (poem.pages) { /* ... */ }
//       ~~~~~
// Property 'pages' does not exist on type 'PoemWithPages | PoemWithRhymes'.
//   Property 'pages' does not exist on type 'PoemWithRhymes'.
```

### **Union có gắn thẻ phân biệt (Discriminated Unions)**

Một dạng đối tượng có kiểu union phổ biến khác trong JavaScript và TypeScript là có một thuộc tính trên đối tượng cho biết đối tượng đó có hình dạng gì. Loại hình dạng kiểu này được gọi là một _discriminated union_ (union có phân biệt/gắn thẻ), và thuộc tính có giá trị chỉ ra kiểu của đối tượng được gọi là một _discriminant_ (thẻ phân biệt). TypeScript có thể thực hiện thu hẹp kiểu cho mã sử dụng type guard trên các thuộc tính discriminant.

Ví dụ: kiểu `Poem` này mô tả một đối tượng có thể là kiểu `PoemWithPages` mới hoặc kiểu `PoemWithRhymes` mới, và thuộc tính `type` cho biết nó là kiểu nào. Nếu `poem.type` là `"pages"`, thì TypeScript có thể suy luận rằng kiểu của `poem` phải là `PoemWithPages`. Nếu không có sự thu hẹp kiểu đó, cả hai thuộc tính đều không được đảm bảo tồn tại trên giá trị:

```typescript
type PoemWithPages= {
name: string;
pages: number;
type: 'pages';
};
type PoemWithRhymes= {
name: string;
rhymes: boolean;
type: 'rhymes';
};
type Poem = PoemWithPages | PoemWithRhymes;
const poem: Poem = Math.random() >0.5
? { name: "The Double Image", pages: 7, type: "pages" }
: { name: "Her Kind", rhymes: true, type: "rhymes" };
if (poem.type === "pages") {
console.log(`It's got pages: ${poem.pages}`);  // Ok
} else {
console.log(`It rhymes: ${poem.rhymes}`);
}
poem.type;  // Type: 'pages' | 'rhymes'
poem.pages;
//   ~~~~~
// Error: Property 'pages' does not exist on type 'Poem'.
//   Property 'pages' does not exist on type 'PoemWithRhymes'.
```

Discriminated unions là tính năng yêu thích nhất của tôi trong TypeScript vì chúng kết hợp một cách tuyệt đẹp một mô hình JavaScript thanh lịch phổ biến với khả năng thu hẹp kiểu của TypeScript. Chương 10, “Generics” và các dự án liên quan của nó sẽ giới thiệu thêm về việc sử dụng discriminated unions cho các thao tác dữ liệu generic.

## **Kiểu giao (Intersection Types)**

Các union types `|` của TypeScript đại diện cho kiểu của một giá trị có thể là một trong hai hoặc nhiều kiểu khác nhau. Giống như toán tử `|` thời gian chạy của JavaScript đóng vai trò đối trọng với toán tử `&` của nó, TypeScript cho phép biểu diễn một kiểu đồng thời là nhiều kiểu cùng một lúc: một _intersection type_ (kiểu giao) `&`. Các intersection types thường được sử dụng với các aliased object types để tạo ra một kiểu mới kết hợp nhiều kiểu đối tượng hiện có.

Các kiểu `Artwork` và `Writing` sau đây được sử dụng để tạo thành một kiểu `WrittenArt` kết hợp có các thuộc tính `genre`, `name`, và `pages`:

```typescript
type Artwork= {
genre: string;
name: string;
};
type Writing= {
pages: number;
name: string;
};
type WrittenArt = Artwork & Writing;
// Equivalent to:
// {
//   genre: string;
//   name: string;
//   pages: number;
// }
```

Các intersection types có thể được kết hợp với union types, điều này đôi khi rất hữu ích để mô tả các discriminated unions trong một kiểu duy nhất.

Kiểu `ShortPoem` này luôn có thuộc tính `author`, sau đó cũng là một discriminated union trên thuộc tính `type`:

```typescript
type ShortPoem= { author: string } & (
| { kigo: string; type: "haiku"; }
| { meter: number; type: "villanelle"; }
);
// Ok

const morningGlory: ShortPoem= {
author: "Fukuda Chiyo-ni",
kigo: "Morning Glory",
type: "haiku",
};
const oneArt: ShortPoem= {
author: "Elizabeth Bishop",
type: "villanelle",
};
// Error: Type '{ author: string; type: "villanelle"; }'
// is not assignable to type 'ShortPoem'.
//   Type '{ author: string; type: "villanelle"; }' is not assignable to
//   type '{ author: string; } & { meter: number; type: "villanelle"; }'.
//     Property 'meter' is missing in type '{ author: string; type: "villanelle"; }'
//     but required in type '{ meter: number; type: "villanelle"; }'.
```

### **Những cạm bẫy của Intersection Types**

Intersection types là một khái niệm hữu ích, nhưng rất dễ sử dụng chúng theo những cách gây bối rối cho chính bạn hoặc trình biên dịch TypeScript. Tôi khuyên bạn nên cố gắng giữ cho mã càng đơn giản càng tốt khi sử dụng chúng.

#### **Thông báo lỗi assignability dài dòng**

Các thông báo lỗi assignability từ TypeScript trở nên khó đọc hơn nhiều khi bạn tạo các intersection types phức tạp, chẳng hạn như loại được kết hợp với một union type. Đây sẽ là một chủ đề phổ biến với hệ thống kiểu của TypeScript (và các ngôn ngữ lập trình định kiểu nói chung): bạn càng viết phức tạp, thì thông báo từ bộ kiểm tra kiểu càng khó hiểu.

Trong trường hợp `ShortPoem` của đoạn mã trước, việc chia kiểu đó thành một chuỗi các aliased object types sẽ dễ đọc hơn nhiều để cho phép TypeScript in ra các tên đó:

```typescript
type ShortPoemBase= { author: string };
type Haiku = ShortPoemBase& { kigo: string; type: "haiku" };
type Villanelle = ShortPoemBase& { meter: number; type: "villanelle" };
type ShortPoem = Haiku | Villanelle;

const oneArt: ShortPoem= {
author: "Elizabeth Bishop",
type: "villanelle",
};
// Type '{ author: string; type: "villanelle"; }'
// is not assignable to type 'ShortPoem'.
//   Type '{ author: string; type: "villanelle"; }'
//   is not assignable to type 'Villanelle'.
//     Property 'meter' is missing in type '{ author: string; type: "villanelle"; }'
//     but required in type '{ meter: number; type: "villanelle"; }'.
```

#### **Kiểu never**

Intersection types cũng rất dễ bị lạm dụng và tạo ra một kiểu không thể tồn tại. Các kiểu nguyên thủy không thể được kết hợp với nhau như các thành phần cấu thành trong một intersection type vì một giá trị không thể đồng thời là nhiều kiểu nguyên thủy cùng một lúc. Cố gắng `&` hai kiểu nguyên thủy với nhau sẽ dẫn đến kiểu _never_, được đại diện bởi từ khóa `never`:

```typescript
type NotPossible = number & string;
// Type: never
```

Từ khóa và kiểu `never` là thứ mà các ngôn ngữ lập trình gọi là một _bottom type_ (kiểu đáy), hay kiểu rỗng. Một bottom type là một kiểu không thể có bất kỳ giá trị nào và không thể chạm tới được. Không có kiểu nào có thể được cung cấp cho một vị trí có kiểu là một bottom type:

```typescript
let notNumber: NotPossible = 0;
//  ~~~~~~~~~
// Error: Type 'number' is not assignable to type 'never'.
let notString: never = "";
//  ~~~~~~~~~
// Error: Type 'string' is not assignable to type 'never'.
```

Hầu hết các dự án TypeScript hiếm khi—nếu có—sử dụng kiểu `never`. Thỉnh thoảng nó xuất hiện để đại diện cho các trạng thái bất khả thi trong mã. Tuy nhiên, phần lớn thời gian, nó có khả năng là một sai sót do sử dụng sai intersection types. Tôi sẽ đề cập nhiều hơn về nó trong Chương 15, “Thao tác kiểu”.

## **Tổng kết**

Trong chương này, bạn đã mở rộng hiểu biết của mình về hệ thống kiểu TypeScript để có thể làm việc với các đối tượng:

- Cách TypeScript diễn giải các kiểu từ object type literals
- Mô tả các kiểu object literal, bao gồm các thuộc tính lồng nhau và tùy chọn
- Khai báo, suy luận và thu hẹp kiểu với các union của các kiểu object literal
- Discriminated unions và các thẻ phân biệt (discriminants)
- Kết hợp các kiểu đối tượng lại với nhau bằng intersection types

#### **MẸO (TIP)**

Bây giờ bạn đã đọc xong chương này, hãy thực hành những gì bạn đã học tại _https://learningtypescript.com/objects_.

_Một luật sư khai báo kiểu TypeScript của họ như thế nào?_

_“Tôi phản đối! (I object!)”_
