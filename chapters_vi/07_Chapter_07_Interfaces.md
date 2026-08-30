# **Chương 7. Giao diện (Interfaces)**

## Mục lục

- [**Chương 7. Giao diện (Interfaces)**](#chương-7-giao-diện-interfaces)
  - [**Bí danh kiểu so với Giao diện (Type Aliases Versus Interfaces)**](#bí-danh-kiểu-so-với-giao-diện-type-aliases-versus-interfaces)
      - [**MẸO (TIP)**](#mẹo-tip)
  - [**Các loại thuộc tính (Types of Properties)**](#các-loại-thuộc-tính-types-of-properties)
      - [**MẸO (TIP)**](#mẹo-tip-1)
    - [**Thuộc tính tùy chọn (Optional Properties)**](#thuộc-tính-tùy-chọn-optional-properties)
    - [**Thuộc tính chỉ đọc (Read-Only Properties)**](#thuộc-tính-chỉ-đọc-read-only-properties)
    - [**Hàm và phương thức (Functions and Methods)**](#hàm-và-phương-thức-functions-and-methods)
    - [**Chữ ký gọi (Call Signatures)**](#chữ-ký-gọi-call-signatures)
    - [**Chữ ký chỉ số (Index Signatures)**](#chữ-ký-chỉ-số-index-signatures)
      - [**Chữ ký chỉ số dạng số (Numeric index signatures)**](#chữ-ký-chỉ-số-dạng-số-numeric-index-signatures)
    - [**Interfaces lồng nhau (Nested Interfaces)**](#interfaces-lồng-nhau-nested-interfaces)
  - [**Mở rộng giao diện (Interface Extensions)**](#mở-rộng-giao-diện-interface-extensions)
    - [**Ghi đè thuộc tính (Overridden Properties)**](#ghi-đè-thuộc-tính-overridden-properties)
    - [**Kế thừa nhiều giao diện (Extending Multiple Interfaces)**](#kế-thừa-nhiều-giao-diện-extending-multiple-interfaces)
  - [**Hợp nhất giao diện (Interface Merging)**](#hợp-nhất-giao-diện-interface-merging)
    - [**Xung đột tên thành viên (Member Naming Conflicts)**](#xung-đột-tên-thành-viên-member-naming-conflicts)
  - [**Tổng kết**](#tổng-kết)
    - [**MẸO (TIP)**](#mẹo-tip-2)

_Tại sao chỉ sử dụng những hình dạng kiểu tích hợp sẵn nhàm chán_

_Khi chúng ta có thể tự tạo ra hình dạng của riêng mình!_

Tôi đã đề cập trong Chương 4, “Đối tượng” rằng mặc dù type aliases cho các kiểu đối tượng `{ ... }` là một cách để mô tả hình dạng đối tượng, TypeScript cũng bao gồm tính năng “interface” mà nhiều lập trình viên ưa thích. Interfaces là một cách khác để khai báo hình dạng đối tượng với một cái tên liên kết. Interfaces về nhiều mặt tương tự như các aliased object types nhưng nhìn chung được ưa chuộng hơn vì thông báo lỗi dễ đọc hơn, hiệu năng trình biên dịch nhanh hơn và khả năng tương tác tốt hơn với các class.

## **Bí danh kiểu so với Giao diện (Type Aliases Versus Interfaces)**

Dưới đây là tóm tắt nhanh về cú pháp mà một aliased object type sẽ mô tả một đối tượng có `born: number` và `name: string`:

```typescript
type Poet = {
  born: number;
  name: string;
};
```

Dưới đây là cú pháp tương đương cho một interface:

```typescript
interface Poet {
  born: number;
  name: string;
}
```

Hai cú pháp này gần như giống hệt nhau.

#### **MẸO (TIP)**

Các lập trình viên TypeScript thích dấu chấm phẩy thường đặt chúng sau type aliases và không đặt sau interfaces. Sở thích này phản ánh sự khác biệt giữa việc khai báo một biến có dấu `;` so với khai báo một class hoặc function không có dấu chấm phẩy.

Việc kiểm tra khả năng gán (assignability) và các thông báo lỗi của TypeScript đối với interfaces cũng hoạt động và trông giống hệt như đối với các kiểu đối tượng. Các lỗi assignability sau đây khi gán cho biến `valueLater` sẽ gần như tương tự bất kể `Poet` là một interface hay một type alias:

```typescript
let valueLater: Poet;
// Ok
valueLater= {
born: 1935,
name: 'Sara Teasdale',
};
valueLater = "Emily Dickinson";
// Error: Type 'string' is not assignable to 'Poet'.
valueLater= {
born: true,
// Error: Type 'boolean' is not assignable to type 'number'.
name: 'Sappho'
};
```

Tuy nhiên, có một vài khác biệt then chốt giữa interfaces và type aliases:

- Như bạn sẽ thấy sau này trong chương này, interfaces có thể “hợp nhất” (merge) lại với nhau để được bổ sung mở rộng—một tính năng đặc biệt hữu ích khi làm việc với mã của bên thứ ba như các đối tượng toàn cục tích hợp sẵn hoặc các gói npm.
- Như bạn sẽ thấy trong chương tiếp theo, Chương 8, “Classes”, interfaces có thể được sử dụng để kiểm tra kiểu cấu trúc của các khai báo class trong khi type aliases thì không thể.
- Interfaces nhìn chung xử lý nhanh hơn đối với bộ kiểm tra kiểu của TypeScript: chúng khai báo một kiểu có tên có thể được lưu vào bộ nhớ đệm (cache) nội bộ dễ dàng hơn, thay vì việc sao chép-và-dán động một object literal mới theo cách mà type aliases thực hiện.
- Vì interfaces được coi là các đối tượng có tên thay vì một bí danh cho một object literal không tên, nên các thông báo lỗi của chúng có nhiều khả năng dễ đọc hơn trong các trường hợp biên phức tạp.

Vì hai lý do sau cùng và để duy trì tính nhất quán, phần còn lại của cuốn sách này và các dự án liên quan sẽ mặc định sử dụng interfaces thay vì aliased object shapes. Tôi khuyên bạn nên sử dụng interfaces bất cứ khi nào có thể (nghĩa là cho đến khi bạn cần các tính năng như union types từ type aliases).

## **Các loại thuộc tính (Types of Properties)**

Các đối tượng JavaScript trong thực tế có thể rất đa dạng và phong phú, bao gồm getters và setters, các thuộc tính chỉ thỉnh thoảng mới tồn tại, hoặc chấp nhận bất kỳ tên thuộc tính tùy ý nào. TypeScript cung cấp một tập hợp các công cụ hệ thống kiểu cho interfaces để giúp chúng ta mô hình hóa sự đa dạng đó.

#### **MẸO (TIP)**

Bởi vì interfaces và type aliases hoạt động rất giống nhau, các loại thuộc tính sau đây được giới thiệu trong chương này cũng đều có thể sử dụng được với các aliased object types.

### **Thuộc tính tùy chọn (Optional Properties)**

Cũng như với các kiểu đối tượng, không phải tất cả các thuộc tính của interface đều bắt buộc phải có trong đối tượng. Bạn có thể chỉ ra rằng một thuộc tính của interface là tùy chọn bằng cách thêm dấu `?` trước dấu `:` trong chú thích kiểu của nó.

Interface `Book` này chỉ yêu cầu thuộc tính `pages` và cho phép tùy chọn có thêm `author`. Các đối tượng tuân thủ theo nó có thể cung cấp `author` hoặc bỏ qua miễn là chúng cung cấp `pages`:

```typescript
interface Book {
author?: string;
pages: number;
};
// Ok
const ok: Book= {
author: "Rita Dove",
pages: 80,
};
const missing: Book= {
pages: 80
};
// Error: Property 'author' is missing in type
// '{ pages: number; }' but required in type 'Book'.
```

Những lưu ý tương tự xung quanh sự khác biệt giữa các thuộc tính tùy chọn và các thuộc tính có kiểu tình cờ bao gồm `undefined` trong một union type cũng áp dụng cho interfaces cũng như các kiểu đối tượng. Chương 13, “Các tùy chọn cấu hình” sẽ mô tả các thiết lập độ nghiêm ngặt của TypeScript xung quanh các thuộc tính tùy chọn.

### **Thuộc tính chỉ đọc (Read-Only Properties)**

Đôi khi bạn có thể muốn chặn người dùng interface của bạn gán lại các thuộc tính của các đối tượng tuân thủ interface đó. TypeScript cho phép bạn thêm bộ bổ từ `readonly` trước tên thuộc tính để chỉ ra rằng một khi đã được thiết lập, thuộc tính đó không được phép gán sang một giá trị khác. Những thuộc tính `readonly` này có thể được đọc bình thường, nhưng không thể được gán lại cho bất kỳ giá trị mới nào.

Ví dụ: thuộc tính `text` trong interface `Page` dưới đây trả về một `string` khi được truy cập, nhưng gây ra lỗi kiểu nếu được gán một giá trị mới:

```typescript
interface Page {
readonly text: string;
}
function read(page: Page) {
// Ok: reading the text property doesn't attempt to modify it
console.log(page.text);
page.text+="!";

//   ~~~~
// Error: Cannot assign to 'text'
// because it is a read-only property.
}
```

Lưu ý rằng bộ bổ từ `readonly` chỉ tồn tại trong hệ thống kiểu và chỉ áp dụng cho việc sử dụng interface đó. Nó sẽ không áp dụng cho một đối tượng trừ khi đối tượng đó được sử dụng ở một vị trí khai báo nó thuộc interface đó.

Trong phần tiếp theo của ví dụ `Page`, thuộc tính `text` được phép sửa đổi bên ngoài hàm vì đối tượng cha của nó không được sử dụng tường minh như một `Page` cho đến khi vào bên trong hàm. `pageIsh` được phép sử dụng như một `Page` vì một thuộc tính có thể ghi có thể gán được cho một thuộc tính `readonly` (các thuộc tính có thể thay đổi có thể đọc được, đó là tất cả những gì một thuộc tính `readonly` cần):

```typescript
const pageIsh= {
text: "Hello, world!",
};
// Ok: pageIsh is an inferred object type with text, not a Page
pageIsh.text+="!";

// Ok: read takes in Page, which happens to
// be a more specific version of pageIsh's type
read(pageIsh);
```

Việc khai báo biến `pageIsh` với chú thích kiểu tường minh `: Page` sẽ thông báo cho TypeScript biết rằng thuộc tính `text` của nó là `readonly`. Tuy nhiên, kiểu được suy luận của nó thì không phải là `readonly`.

Các thành viên interface chỉ đọc là một cách tiện dụng để đảm bảo các vùng mã không vô tình sửa đổi các đối tượng mà chúng không được phép sửa đổi. Tuy nhiên, hãy nhớ rằng chúng chỉ là cấu trúc hệ thống kiểu và không tồn tại trong mã JavaScript đầu ra đã biên dịch. Chúng chỉ bảo vệ khỏi sự sửa đổi trong quá trình phát triển với bộ kiểm tra kiểu TypeScript.

### **Hàm và phương thức (Functions and Methods)**

Trong JavaScript, việc các thành viên đối tượng là các hàm là rất phổ biến. Do đó, TypeScript cho phép khai báo các thành viên interface là các kiểu hàm đã được đề cập trong Chương 5, “Hàm”. TypeScript cung cấp hai cách khai báo các thành viên interface dưới dạng hàm:

- Cú pháp _Method_ (phương thức): khai báo rằng một thành viên của interface là một hàm được dự định gọi như một thành viên của đối tượng, chẳng hạn `member(): void`
- Cú pháp _Property_ (thuộc tính): khai báo rằng một thành viên của interface bằng với một hàm độc lập, chẳng hạn `member: () => void`

Hai dạng khai báo này tương ứng với hai cách bạn có thể khai báo một đối tượng JavaScript có một hàm.

Cả hai thành viên `method` và `property` được hiển thị ở đây đều là các hàm có thể được gọi không có tham số và trả về một `string`:

```typescript
interface HasBothFunctionTypes {
property: () => string;
method(): string;
}
const hasBoth: HasBothFunctionTypes= {
property: () =>"",
method() {
return "";
  }
};
hasBoth.property();  // Ok
hasBoth.method();  // Ok
```

Cả hai dạng đều có thể nhận bộ bổ từ tùy chọn `?` để chỉ ra rằng chúng không nhất thiết phải được cung cấp:

```typescript
interface OptionalReadonlyFunctions {
  optionalProperty?: () => string;
  optionalMethod?(): string;
}
```

Khai báo phương thức và thuộc tính hầu như có thể được sử dụng thay thế cho nhau. Những khác biệt chính giữa chúng mà tôi sẽ đề cập trong cuốn sách này là:

- Methods không thể được khai báo là `readonly`; properties thì có thể.
- Việc hợp nhất giao diện (sẽ đề cập sau trong chương này) đối xử với chúng khác nhau.
- Một số thao tác được thực hiện trên các kiểu được đề cập trong Chương 15, “Thao tác kiểu” đối xử với chúng khác nhau.

Các phiên bản tương lai của TypeScript có thể bổ sung tùy chọn để nghiêm ngặt hơn về sự khác biệt giữa methods và property functions.

Hiện tại, quy tắc phong cách chung mà tôi đề xuất là:

- Sử dụng method function nếu bạn biết hàm bên dưới có thể tham chiếu đến `this`, phổ biến nhất là đối với các instance của class (được đề cập trong Chương 8, “Classes”).
- Sử dụng property function trong các trường hợp khác.

Đừng lo lắng nếu bạn nhầm lẫn giữa hai loại này hoặc không hiểu rõ sự khác biệt. Nó sẽ hiếm khi ảnh hưởng đến mã của bạn trừ khi bạn có chủ đích về phạm vi của `this` và dạng nào bạn chọn.

### **Chữ ký gọi (Call Signatures)**

Interfaces và các kiểu đối tượng có thể khai báo _call signatures_ (chữ ký gọi), là mô tả của hệ thống kiểu về cách một giá trị có thể được gọi giống như một hàm. Chỉ các giá trị có thể được gọi theo cách mà call signature khai báo mới có thể gán được cho interface—tức là một hàm có các tham số và kiểu trả về có thể gán được. Một call signature trông tương tự như một kiểu hàm, nhưng có dấu hai chấm `:` thay vì dấu mũi tên `=>`.

Các kiểu `FunctionAlias` và `CallSignature` sau đây đều mô tả các tham số hàm và kiểu trả về giống nhau:

```typescript
type FunctionAlias = (input: string) => number;

interface CallSignature {
  (input: string): number;
}
// Type: (input: string) => number
const typedFunctionAlias: FunctionAlias= (input) => input.length;  // Ok
// Type: (input: string) => number
const typedCallSignature: CallSignature= (input) => input.length;  // Ok
```

Call signatures có thể được sử dụng để mô tả các hàm có thêm một số thuộc tính do người dùng định nghĩa trên chúng. TypeScript sẽ nhận diện một thuộc tính được thêm vào một khai báo hàm như là việc bổ sung vào kiểu của khai báo hàm đó.

Khai báo hàm `keepsTrackOfCalls` sau đây được gán một thuộc tính `count` có kiểu `number`, làm cho nó có thể gán được cho interface `FunctionWithCount`. Do đó, nó có thể được gán cho đối số `hasCallCount` có kiểu `FunctionWithCount`. Hàm ở cuối đoạn mã không được gán `count`:

```typescript
interface FunctionWithCount {
count: number;
  (): void;
}
let hasCallCount: FunctionWithCount;
function keepsTrackOfCalls() {
keepsTrackOfCalls.count+=1;
console.log(`I've been called ${keepsTrackOfCalls.count} times!`);
}
keepsTrackOfCalls.count = 0;
hasCallCount = keepsTrackOfCalls;  // Ok
function doesNotHaveCount() {
console.log("No idea!");
}
hasCallCount = doesNotHaveCount;
// Error: Property 'count' is missing in type
// '() => void' but required in type 'FunctionWithCalls'
```

### **Chữ ký chỉ số (Index Signatures)**

Một số dự án JavaScript tạo các đối tượng dùng để lưu trữ các giá trị dưới bất kỳ khóa `string` tùy ý nào. Đối với các đối tượng “vật chứa” (container) này, việc khai báo một interface có một trường cho mọi khóa có thể có là không thực tế hoặc không thể thực hiện được.

TypeScript cung cấp một cú pháp gọi là _index signature_ (chữ ký chỉ số) để chỉ ra rằng các đối tượng của một interface được phép nhận bất kỳ khóa nào và trả về một kiểu nhất định dưới khóa đó. Chúng thường được sử dụng nhiều nhất với các khóa dạng string vì việc tra cứu thuộc tính đối tượng JavaScript chuyển đổi các khóa thành chuỗi một cách ngầm định. Một index signature trông giống như một định nghĩa thuộc tính thông thường nhưng có một kiểu sau khóa và các dấu ngoặc vuông mảng bao quanh chúng, chẳng hạn `{ [i: string]: ... }`. Interface `WordCounts` này được khai báo là cho phép bất kỳ khóa `string` nào với một giá trị `number`. Các đối tượng thuộc kiểu đó không bị ràng buộc phải nhận bất kỳ khóa cụ thể nào—miễn là giá trị là một `number`:

```typescript
interface WordCounts {
  [i: string]: number;
}
const counts: WordCounts= {};
counts.apple = 0;  // Ok
counts.banana = 1;  // Ok
counts.cherry = false;
// Error: Type 'boolean' is not assignable to type 'number'.
```

Index signatures rất tiện lợi cho việc gán giá trị cho một đối tượng nhưng không hoàn toàn an toàn về kiểu. Chúng chỉ ra rằng một đối tượng sẽ luôn trả về một giá trị bất kể thuộc tính nào đang được truy cập.

Giá trị `publishDates` này trả về `Frankenstein` như một `Date` một cách an toàn nhưng lại đánh lừa TypeScript nghĩ rằng `Beloved` của nó đã được định nghĩa mặc dù nó là `undefined`:

```typescript
interface DatesByName {
  [i: string]: Date;
}

const publishDates: DatesByName= {
Frankenstein: new Date("1 January 1818"),
};

publishDates.Frankenstein;  // Type: Date
console.log(publishDates.Frankenstein.toString());  // Ok

publishDates.Beloved;  // Type: Date, but runtime value of undefined!
console.log(publishDates.Beloved.toString());  // Ok in the type system, but...
// Runtime error: Cannot read property 'toString'
// of undefined (reading publishDates.Beloved)
```

Khi có thể, nếu bạn đang muốn lưu trữ các cặp key-value và các khóa không được biết trước, nhìn chung sẽ an toàn hơn nếu sử dụng một `Map`. Phương thức `.get` của nó luôn trả về một kiểu có `| undefined` để chỉ ra rằng khóa đó có thể không tồn tại. Chương 9, “Các bổ từ kiểu” sẽ thảo luận về việc làm việc với các generic container classes như `Map` và `Set`.

**Kết hợp các thuộc tính và index signatures**

Interfaces có thể bao gồm các thuộc tính được đặt tên rõ ràng và các index signatures `string` bao quát toàn bộ, với một điều kiện: kiểu của mỗi thuộc tính được đặt tên phải có thể gán được cho kiểu của index signature bao quát đó. Bạn có thể coi việc kết hợp chúng như việc thông báo cho TypeScript biết rằng các thuộc tính được đặt tên cung cấp một kiểu cụ thể hơn, và bất kỳ thuộc tính nào khác sẽ rơi về kiểu của index signature.

Ở đây, `HistoricalNovels` khai báo rằng tất cả các thuộc tính đều có kiểu `number`, và ngoài ra thuộc tính `Oroonoko` bắt buộc phải tồn tại ngay từ đầu:

```typescript
interface HistoricalNovels {
Oroonoko: number;
  [i: string]: number;
}
// Ok
const novels: HistoricalNovels= {
Outlander: 1991,
Oroonoko: 1688,
};
const missingOroonoko: HistoricalNovels= {
Outlander: 1991,
};

// Error: Property 'Oroonoko' is missing in type
// '{ Outlander: number; }' but required in type 'HistoricalNovels'.
```

Một mẹo hệ thống kiểu phổ biến với các thuộc tính kết hợp và index signatures là sử dụng một kiểu thuộc tính literal cụ thể hơn cho thuộc tính được đặt tên so với kiểu nguyên thủy của index signature. Miễn là kiểu của thuộc tính được đặt tên có thể gán được cho kiểu của index signature—điều này đúng với một literal và một primitive—TypeScript sẽ cho phép điều đó.

Ở đây, `ChapterStarts` khai báo rằng một thuộc tính dưới `preface` phải là `0` và tất cả các thuộc tính khác có kiểu `number` tổng quát hơn. Điều đó có nghĩa là bất kỳ đối tượng nào tuân thủ theo `ChapterStarts` đều phải có thuộc tính `preface` bằng `0`:

```typescript
interface ChapterStarts {
preface: 0;
  [i: string]: number;
}
const correctPreface: ChapterStarts= {
preface: 0,
night: 1,
shopping: 5
};
const wrongPreface: ChapterStarts= {
preface: 1,
// Error: Type '1' is not assignable to type '0'.
};
```

#### **Chữ ký chỉ số dạng số (Numeric index signatures)**

Mặc dù JavaScript chuyển đổi các khóa tra cứu thuộc tính đối tượng thành chuỗi một cách ngầm định, nhưng đôi khi chúng ta chỉ muốn cho phép các số làm khóa cho một đối tượng. Các index signatures của TypeScript có thể sử dụng kiểu `number` thay vì `string` nhưng với cùng điều kiện như các thuộc tính được đặt tên: kiểu của chúng phải có thể gán được cho index signature `string` bao quát.

Interface `MoreNarrowNumbers` sau đây sẽ được cho phép vì `string` có thể gán được cho `string | undefined`, nhưng `MoreNarrowStrings` thì không vì `string | undefined` không thể gán được cho `string`:

```typescript
// Ok
interface MoreNarrowNumbers {
  [i: number]: string;
  [i: string]: string | undefined;
}
// Ok
const mixesNumbersAndStrings: MoreNarrowNumbers= {
0: '',
key1: '',
key2: undefined,
}
interface MoreNarrowStrings {
  [i: number]: string | undefined;
// Error: 'number' index type 'string | undefined'
// is not assignable to 'string' index type 'string'.
  [i: string]: string;
}
```

### **Interfaces lồng nhau (Nested Interfaces)**

Cũng giống như các kiểu đối tượng có thể được lồng vào nhau như các thuộc tính của các kiểu đối tượng khác, các kiểu interface cũng có thể có các thuộc tính bản thân chúng là các kiểu interface (hoặc kiểu đối tượng).

Interface `Novel` này chứa một thuộc tính `author` phải thỏa mãn một kiểu đối tượng inline và một thuộc tính `setting` phải thỏa mãn interface `Setting`:

```typescript
interface Novel {
author: {
name: string;
    };
setting: Setting;
}
interface Setting {
place: string;
year: number;
}
let myNovel: Novel;
// Ok
myNovel= {

author: {
name: 'Jane Austen',
    },
setting: {
place: 'England',
year: 1812,
    }
};
myNovel= {
author: {
name: 'Emily Brontë',
    },
setting: {
place: 'West Yorkshire',
    },
// Error: Property 'year' is missing in type
// '{ place: string; }' but required in type 'Setting'.
};
```

## **Mở rộng giao diện (Interface Extensions)**

Đôi khi bạn có thể có nhiều interface trông tương tự như nhau. Một interface có thể chứa tất cả các thành viên giống như của một interface khác, kèm theo một vài thành viên bổ sung.

TypeScript cho phép một interface _mở rộng_ (extend) một interface khác, khai báo việc sao chép tất cả các thành viên của interface kia. Một interface có thể được đánh dấu là mở rộng một interface khác bằng cách thêm từ khóa `extends` sau tên của nó (interface “dẫn xuất”), theo sau là tên của interface cần mở rộng (interface “cơ sở”). Làm như vậy sẽ thông báo cho TypeScript biết rằng tất cả các đối tượng tuân thủ interface dẫn xuất cũng phải có tất cả các thành viên của interface cơ sở.

Trong ví dụ sau, interface `Novella` kế thừa từ `Writing` và do đó yêu cầu các đối tượng phải có ít nhất cả thuộc tính `pages` của `Novella` và `title` của `Writing`:

```typescript
interface Writing {
title: string;
}

interface Novella extends Writing {
pages: number;
}
// Ok
let myNovella: Novella= {
pages: 195,
title: "Ethan Frome",
};
let missingPages: Novella= {
// ~~~~~~~~~~~~
// Error: Property 'pages' is missing in type
// '{ title: string; }' but required in type 'Novella'.
title: "The Awakening",
}
let extraProperty: Novella= {
// ~~~~~~~~~~~~~
// Error: Type '{ genre: string; name: string; strategy: string; }'
// is not assignable to type 'Novella'.
//   Object literal may only specify known properties,
//   and 'genre' does not exist in type 'Novella'.
pages: 300,
strategy: "baseline",
style: "Naturalism"
};
```

Interface extensions là một cách tinh tế để biểu thị rằng một loại thực thể trong dự án của bạn là một tập cha (nó bao gồm tất cả các thành viên của) một thực thể khác. Chúng cho phép bạn tránh việc phải gõ lại cùng một đoạn mã trên nhiều interface để thể hiện mối quan hệ đó.

### **Ghi đè thuộc tính (Overridden Properties)**

Các interface dẫn xuất có thể _ghi đè_ (override), hoặc thay thế các thuộc tính từ interface cơ sở của chúng bằng cách khai báo lại thuộc tính đó với một kiểu khác. Bộ kiểm tra kiểu của TypeScript sẽ thực thi rằng một thuộc tính bị ghi đè phải có thể gán được cho thuộc tính cơ sở của nó. Nó làm như vậy để đảm bảo rằng các thể hiện của kiểu interface dẫn xuất vẫn có thể gán được cho kiểu interface cơ sở.

Hầu hết các interface dẫn xuất khai báo lại các thuộc tính đều làm như vậy hoặc để làm cho các thuộc tính đó thành một tập con cụ thể hơn của một type union hoặc để làm cho các thuộc tính thành một kiểu kế thừa từ kiểu của interface cơ sở.

Ví dụ: kiểu `WithNullableName` này được chuyển đổi thành non-nullable trong `WithNonNullableName` một cách hợp lệ. Tuy nhiên, `WithNumericName` không được phép vì `number | string` không thể gán được cho `string | null`:

```typescript
interface WithNullableName {
name: string | null;
}
interface WithNonNullableName extends WithNullableName {
name: string;
}
interface WithNumericName extends WithNullableName {
name: number | string;
}
// Error: Interface 'WithNumericName' incorrectly
// extends interface 'WithNullableName'.
//   Types of property 'name' are incompatible.
//     Type 'string | number' is not assignable to type 'string | null'.
//       Type 'number' is not assignable to type 'string'.
```

### **Kế thừa nhiều giao diện (Extending Multiple Interfaces)**

Các interface trong TypeScript được phép khai báo là mở rộng từ nhiều interface khác. Bất kỳ số lượng tên interface nào được phân tách bằng dấu phẩy đều có thể được sử dụng sau từ khóa `extends` theo sau tên của interface dẫn xuất. Interface dẫn xuất sẽ nhận tất cả các thành viên từ tất cả các interface cơ sở. Ở đây, `GivesBothAndEither` có ba phương thức: một phương thức của riêng nó, một từ `GivesNumber`, và một từ `GivesString`:

```typescript
interface GivesNumber {
giveNumber(): number;
}
interface GivesString {
giveString(): string;
}

interface GivesBothAndEither extends GivesNumber, GivesString {
giveEither(): number | string;
}
function useGivesBoth(instance: GivesBothAndEither) {
instance.giveEither();  // Type: number | string
instance.giveNumber();  // Type: number
instance.giveString();  // Type: string
}
```

Bằng cách đánh dấu một interface mở rộng từ nhiều interface khác, bạn vừa có thể giảm bớt sự trùng lặp mã vừa giúp các hình dạng đối tượng dễ dàng được tái sử dụng trên các khu vực mã khác nhau.

## **Hợp nhất giao diện (Interface Merging)**

Một trong những tính năng quan trọng của interfaces là khả năng _hợp nhất_ (merge) với nhau. Hợp nhất interface có nghĩa là nếu hai interface được khai báo trong cùng một phạm vi với cùng một tên, chúng sẽ kết hợp thành một interface lớn hơn dưới tên đó với tất cả các trường được khai báo.

Đoạn mã này khai báo một interface `Merged` với hai thuộc tính: `fromFirst` và `fromSecond`:

```typescript
interface Merged {
fromFirst: string;
}
interface Merged {
fromSecond: number;
}
// Equivalent to:
// interface Merged {
//   fromFirst: string;
//   fromSecond: number;
// }
```

Hợp nhất interface không phải là một tính năng được sử dụng thường xuyên trong quá trình phát triển TypeScript hàng ngày. Tôi khuyên bạn nên tránh nó khi có thể, vì việc hiểu mã nguồn nơi một interface được khai báo ở nhiều nơi có thể khá khó khăn.

Tuy nhiên, việc hợp nhất interface đặc biệt hữu ích cho việc bổ sung các interface từ các gói bên ngoài hoặc các interface toàn cục tích hợp sẵn như `Window`. Ví dụ: khi sử dụng các tùy chọn trình biên dịch TypeScript mặc định, việc khai báo một interface `Window` trong một tệp với thuộc tính `myEnvironmentVariable` sẽ làm cho `window.myEnvironmentVariable` trở nên khả dụng:

```typescript
interface Window {
myEnvironmentVariable: string;
}
window.myEnvironmentVariable;  // Type: string
```

Tôi sẽ đề cập sâu hơn về các định nghĩa kiểu trong Chương 11, “Tệp khai báo” và các tùy chọn kiểu toàn cục của TypeScript trong Chương 13, “Các tùy chọn cấu hình”.

### **Xung đột tên thành viên (Member Naming Conflicts)**

Lưu ý rằng các interface được hợp nhất không được phép khai báo cùng một tên thuộc tính nhiều lần với các kiểu khác nhau. Nếu một thuộc tính đã được khai báo trong một interface, một interface hợp nhất sau đó bắt buộc phải sử dụng cùng một kiểu.

Trong interface `MergedProperties` này, thuộc tính `same` được cho phép vì nó giống nhau trong cả hai khai báo, nhưng `different` là một lỗi vì có kiểu khác nhau:

```typescript
interface MergedProperties {
same: (input: boolean) => string;
different: (input: string) => string;
}
interface MergedProperties {
same: (input: boolean) => string;  // Ok
different: (input: number) => string;
// Error: Subsequent property declarations must have the same type.
// Property 'different' must be of type '(input: string) => string',
// but here has type '(input: number) => string'.
}
```

Tuy nhiên, các interface được hợp nhất có thể định nghĩa một phương thức có cùng tên và chữ ký khác nhau. Làm như vậy sẽ tạo ra một hàm nạp chồng (function overload) cho phương thức đó. Interface `MergedMethods` này tạo ra một phương thức `different` có hai nạp chồng:

```typescript
interface MergedMethods {
different(input: string): string;
}
interface MergedMethods {
different(input: number): string;  // Ok
}
```

## **Tổng kết**

Chương này đã giới thiệu cách các kiểu đối tượng có thể được mô tả bằng interfaces:

- Sử dụng interfaces thay vì type aliases để khai báo các kiểu đối tượng
- Các loại thuộc tính interface khác nhau: tùy chọn, chỉ đọc, hàm và phương thức
- Sử dụng index signatures cho các thuộc tính đối tượng bao quát
- Tái sử dụng interfaces bằng cách sử dụng các interface lồng nhau và kế thừa `extends`
- Cách các interface có cùng tên có thể hợp nhất lại với nhau

Tiếp theo sẽ là một cú pháp JavaScript gốc để thiết lập nhiều đối tượng có cùng các thuộc tính: classes.

#### **MẸO (TIP)**

Bây giờ bạn đã đọc xong chương này, hãy thực hành những gì bạn đã học tại _https://learningtypescript.com/objects-and-interfaces_.

_Tại sao interfaces lại là những tài xế giỏi?_

_Vì chúng rất giỏi trong việc nhập làn / hợp nhất (merging)._
