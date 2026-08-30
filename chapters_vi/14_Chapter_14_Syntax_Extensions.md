# **Phần IV. Điểm cộng thêm (Extra Credit)**

## Mục lục

- [**Phần IV. Điểm cộng thêm (Extra Credit)**](#phần-iv-điểm-cộng-thêm-extra-credit)
- [**Chương 14. Phần mở rộng cú pháp (Syntax Extensions)**](#chương-14-phần-mở-rộng-cú-pháp-syntax-extensions)
      - [**GHI CHÚ (NOTE)**](#ghi-chú-note)
  - [**Thuộc tính tham số của lớp (Class Parameter Properties)**](#thuộc-tính-tham-số-của-lớp-class-parameter-properties)
      - [**MẸO (TIP)**](#mẹo-tip)
  - [**Decorators thử nghiệm (Experimental Decorators)**](#decorators-thử-nghiệm-experimental-decorators)
      - [**MẸO (TIP)**](#mẹo-tip-1)
  - [**Kiểu liệt kê (Enums)**](#kiểu-liệt-kê-enums)
      - [**MẸO (TIP)**](#mẹo-tip-2)
      - [**CẢNH BÁO (WARNING)**](#cảnh-báo-warning)
    - [**Giá trị số tự động (Automatic Numeric Values)**](#giá-trị-số-tự-động-automatic-numeric-values)
      - [**CẢNH BÁO (WARNING)**](#cảnh-báo-warning-1)
    - [**Enums có giá trị chuỗi (String-Valued Enums)**](#enums-có-giá-trị-chuỗi-string-valued-enums)
      - [**MẸO (TIP)**](#mẹo-tip-3)
    - [**Const Enums**](#const-enums)
  - [**Không gian tên (Namespaces)**](#không-gian-tên-namespaces)
      - [**CẢNH BÁO (WARNING)**](#cảnh-báo-warning-2)
      - [**CẢNH BÁO (WARNING)**](#cảnh-báo-warning-3)
    - [**Export trong Namespace (Namespace Exports)**](#export-trong-namespace-namespace-exports)
    - [**Namespaces lồng nhau (Nested Namespaces)**](#namespaces-lồng-nhau-nested-namespaces)
    - [**Namespaces trong định nghĩa kiểu (Namespaces in Type Definitions)**](#namespaces-trong-định-nghĩa-kiểu-namespaces-in-type-definitions)
    - [**Ưu tiên Modules hơn Namespaces (Prefer Modules Over Namespaces)**](#ưu-tiên-modules-hơn-namespaces-prefer-modules-over-namespaces)
      - [**GHI CHÚ (NOTE)**](#ghi-chú-note-1)
  - [**Chỉ Import và Export Kiểu (Type-Only Imports and Exports)**](#chỉ-import-và-export-kiểu-type-only-imports-and-exports)
  - [**Tổng kết**](#tổng-kết)
    - [**MẸO (TIP)**](#mẹo-tip-4)

JavaScript đã tồn tại được vài thập kỷ cho đến thời điểm này, và mọi người đã làm khá nhiều điều kỳ lạ với nó. Cú pháp và hệ thống kiểu của TypeScript cần có khả năng đại diện cho tất cả những điều kỳ lạ đó để cho phép bất kỳ lập trình viên JavaScript nào cũng có thể làm việc với TypeScript. Do đó, có một số ngóc ngách của ngôn ngữ TypeScript không xuất hiện trong hầu hết các đoạn mã hàng ngày nhưng lại phù hợp, thậm chí cần thiết, để làm việc với một số loại dự án.

Tôi coi những phần này của ngôn ngữ như là “điểm cộng thêm” theo nghĩa là bạn có thể hoàn toàn tránh chúng mà vẫn là một lập trình viên TypeScript hiệu quả. Trên thực tế, đối với các kiểu logic được giới thiệu vào cuối phần này, tôi hy vọng bạn sẽ không cần phải sử dụng chúng thường xuyên—hoặc thậm chí không cần dùng đến.

# **Chương 14. Phần mở rộng cú pháp (Syntax Extensions)**

_“TypeScript không bổ sung thêm_

_vào môi trường thời gian chạy JavaScript.”_

_…đó chẳng lẽ chỉ là lời nói dối?!_

Khi TypeScript được phát hành lần đầu tiên vào năm 2012, các ứng dụng web đã phát triển về độ phức tạp nhanh hơn tốc độ mà JavaScript thuần túy bổ sung các tính năng hỗ trợ độ phức tạp sâu sắc đó. Ngôn ngữ biến thể JavaScript phổ biến nhất vào thời điểm đó, CoffeeScript, đã ghi dấu ấn bằng cách tách khỏi JavaScript thông qua việc giới thiệu các cấu trúc cú pháp mới và thú vị.

Ngày nay, việc mở rộng cú pháp JavaScript với các tính năng thời gian chạy mới dành riêng cho một ngôn ngữ siêu tập hợp (superset) như TypeScript được coi là một thực hành không tốt vì một số lý do:

- Quan trọng nhất, các phần mở rộng cú pháp thời gian chạy có thể xung đột với cú pháp mới trong các phiên bản JavaScript mới hơn.
- Chúng khiến các lập trình viên mới làm quen với ngôn ngữ khó hiểu được JavaScript kết thúc ở đâu và các ngôn ngữ khác bắt đầu từ đâu.
- Chúng làm tăng độ phức tạp của các trình chuyển mã (transpilers) nhận mã ngôn ngữ siêu tập hợp và phát ra JavaScript.

Vì vậy, với một trái tim nặng trĩu và sự tiếc nuối sâu sắc, tôi phải thông báo với bạn rằng các nhà thiết kế TypeScript ban đầu đã giới thiệu ba phần mở rộng cú pháp cho JavaScript trong ngôn ngữ TypeScript:

- Lớp (Classes), vốn đã được căn chỉnh phù hợp với các lớp JavaScript khi đặc tả được phê chuẩn
- Enums, một cú pháp tiện ích đơn giản tương tự như một đối tượng thuần túy gồm các khóa và giá trị
- Namespaces, một giải pháp ra đời trước các module hiện đại để cấu trúc và sắp xếp mã nguồn

#### **GHI CHÚ (NOTE)**

“Tội lỗi nguyên bản” của TypeScript về các phần mở rộng cú pháp thời gian chạy cho JavaScript may mắn thay không phải là một quyết định thiết kế mà ngôn ngữ này đưa ra kể từ những năm đầu. TypeScript không thêm các cấu trúc cú pháp thời gian chạy mới cho đến khi chúng có tiến triển đáng kể trong quá trình phê chuẩn để được thêm vào chính JavaScript.

Các lớp TypeScript cuối cùng trông và hoạt động gần như giống hệt các lớp JavaScript (phù!) ngoại trừ hành vi `useDefineForClassFields` (một tùy chọn cấu hình không được đề cập trong cuốn sách này) và các thuộc tính tham số (được đề cập ở đây). Enums vẫn được sử dụng trong một số dự án vì đôi khi chúng hữu ích. Hầu như không có dự án mới nào sử dụng namespaces nữa.

TypeScript cũng đã áp dụng một đề xuất thử nghiệm cho “decorators” của JavaScript mà tôi cũng sẽ đề cập đến.

## **Thuộc tính tham số của lớp (Class Parameter Properties)**

#### **MẸO (TIP)**

Tôi khuyên bạn nên tránh sử dụng các thuộc tính tham số của lớp trừ khi bạn đang làm việc trong một dự án sử dụng nhiều lớp hoặc một framework có lợi từ chúng.

Trong các lớp JavaScript, việc nhận một tham số trong constructor và gán ngay cho một thuộc tính của lớp là điều phổ biến.

Lớp `Engineer` này nhận một tham số `area` duy nhất thuộc kiểu `string` và gán nó cho một thuộc tính `area` thuộc kiểu `string`:

```typescript
class Engineer {
readonly area: string;

constructor(area: string) {

this.area = area;
console.log(`I work in the ${area} area.`);
    }
}
// Type: string
new Engineer("mechanical").area;
```

TypeScript bao gồm một cú pháp viết tắt để khai báo những loại “thuộc tính tham số” này: các thuộc tính được gán cho một thuộc tính thành viên cùng kiểu ở đầu constructor của lớp. Đặt `readonly` và/hoặc một trong các bổ từ quyền riêng tư—`public`, `protected`, hoặc `private`—phía trước tham số của constructor sẽ báo hiệu cho TypeScript khai báo một thuộc tính cùng tên và kiểu đó.

Ví dụ `Engineer` trước đó có thể được viết lại trong TypeScript bằng cách sử dụng thuộc tính tham số cho `area`:

```typescript
class Engineer {
constructor(readonly area: string) {
console.log(`I work in the ${area} area.`);
    }
}

// Type: string
new Engineer("mechanical").area;
```

Các thuộc tính tham số được gán ở ngay đầu constructor của lớp (hoặc sau lời gọi `super()` nếu lớp được dẫn xuất từ một lớp cơ sở). Chúng có thể được trộn lẫn với các tham số và/hoặc thuộc tính khác trên một lớp.

Lớp `NamedEngineer` sau đây khai báo một thuộc tính thông thường `fullName`, một tham số thông thường `name`, và một thuộc tính tham số `area`:

```typescript
class NamedEngineer {
fullName: string;
constructor(

name: string,
public area: string,
    ) {
this.fullName=`${name}, ${area} engineer`;
    }
}
```

Mã TypeScript tương đương không có thuộc tính tham số trông tương tự, nhưng có thêm một vài dòng mã để gán `area` một cách rõ ràng:

```typescript
class NamedEngineer {
fullName: string;
area: string;
constructor(
name: string,
area: string,
    ) {
this.area = area;
this.fullName=`${name}, ${area} engineer`;
    }
}
```

Thuộc tính tham số là một vấn đề đôi khi gây tranh cãi trong cộng đồng TypeScript. Hầu hết các dự án thích tránh chúng một cách tuyệt đối, vì chúng là phần mở rộng cú pháp thời gian chạy và do đó chịu những nhược điểm tương tự mà tôi đã đề cập trước đó. Chúng cũng không thể được sử dụng với cú pháp trường private `#` mới hơn của lớp.

Mặt khác, chúng khá tiện lợi khi được sử dụng trong các dự án rất ưa chuộng việc tạo lớp. Thuộc tính tham số giải quyết vấn đề tiện lợi khi cần khai báo tên và kiểu thuộc tính tham số hai lần, vốn là đặc thù của TypeScript chứ không phải JavaScript.

## **Decorators thử nghiệm (Experimental Decorators)**

#### **MẸO (TIP)**

Tôi khuyên bạn nên tránh sử dụng decorators nếu có thể cho đến khi một phiên bản ECMAScript được phê chuẩn với cú pháp decorator. Nếu bạn đang làm việc trong một phiên bản của một framework như Angular hoặc NestJS khuyến nghị sử dụng decorators TypeScript, tài liệu của framework đó sẽ hướng dẫn cách sử dụng chúng.

Nhiều ngôn ngữ khác có chứa lớp cho phép chú thích (decorating) các lớp đó và/hoặc các thành viên của chúng bằng một số loại logic thời gian chạy để sửa đổi chúng. Các hàm _Decorator_ là một đề xuất cho JavaScript để cho phép chú thích các lớp và thành viên bằng cách đặt dấu `@` và tên của một hàm ở phía trước.

Ví dụ: đoạn mã sau đây chỉ hiển thị cú pháp sử dụng một decorator trên một lớp `MyClass`:

```typescript
@myDecorator
class MyClass { /* ... */ }
```

Decorators vẫn chưa được phê chuẩn trong ECMAScript, vì vậy TypeScript không hỗ trợ chúng theo mặc định kể từ phiên bản 4.7.2. Tuy nhiên, TypeScript bao gồm tùy chọn trình biên dịch `experimentalDecorators` cho phép sử dụng phiên bản thử nghiệm cũ của chúng trong mã. Nó có thể được bật thông qua CLI `tsc` hoặc trong tệp TSConfig, như được hiển thị ở đây, giống như các tùy chọn trình biên dịch khác:

```json
{
"compilerOptions":{
"experimentalDecorators":true
}
}
```

Mỗi lần sử dụng decorator sẽ thực thi một lần, ngay khi thực thể mà nó đang trang trí được tạo ra. Mỗi loại decorator—accessor, class, method, parameter, và property—nhận được một tập hợp các đối số khác nhau mô tả thực thể mà nó đang trang trí.

Ví dụ: decorator `logOnCall` này được sử dụng trên phương thức lớp `Greeter` nhận vào chính lớp `Greeter`, khóa của thuộc tính (`"greet"`), và một đối tượng `descriptor` mô tả thuộc tính. Sửa đổi `descriptor.value` để ghi log trước khi gọi phương thức `greet` ban đầu trên lớp `Greeter` sẽ “trang trí” phương thức `greet`:

```typescript
function logOnCall(target: any, key: string, descriptor: PropertyDescriptor) {
const original = descriptor.value;
console.log("[logOnCall] I am decorating", target.constructor.name);

descriptor.value = function (...args: unknown[]) {
console.log(`[descriptor.value] Calling '${key}' with: `, ...args);
return original.call(this, ...args);
    }
}
class Greeter {
@logOnCall
greet(message: string) {
console.log(`[greet] Hello, ${message}!`);
    }
}
new Greeter().greet("you");
// Output log:
// "[logOnCall] I am decorating", "Greeter"
// "[descriptor.value] Calling 'greet' with:", "you"
// "[greet] Hello, you!"
```

Tôi sẽ không đi sâu vào các sắc thái và chi tiết cụ thể về cách hoạt động của `experimentalDecorators` cũ cho từng loại decorator có thể có. Hỗ trợ decorator của TypeScript là thử nghiệm và không phù hợp với các bản thảo mới nhất của đề xuất ECMAScript. Việc tự viết decorators của riêng bạn hiếm khi được biện minh trong bất kỳ dự án TypeScript nào.

## **Kiểu liệt kê (Enums)**

#### **MẸO (TIP)**

Tôi khuyên bạn không nên sử dụng enums trừ khi bạn có một tập hợp các literals lặp lại thường xuyên, tất cả đều có thể được mô tả bằng một tên chung, và mã nguồn sẽ dễ đọc hơn nhiều nếu chuyển sang enum.

Hầu hết các ngôn ngữ lập trình đều có khái niệm “enum”, hay kiểu liệt kê (enumerated type), để đại diện cho một tập hợp các giá trị có liên quan. Enums có thể được coi là một tập hợp các giá trị literal được lưu trữ trong một đối tượng với một tên thân thiện cho mỗi giá trị.

JavaScript không bao gồm cú pháp enum vì các đối tượng truyền thống có thể được sử dụng thay cho chúng. Ví dụ: trong khi các mã trạng thái HTTP có thể được lưu trữ và sử dụng dưới dạng số, nhiều lập trình viên thấy dễ đọc hơn khi lưu trữ chúng trong một đối tượng đặt khóa cho chúng theo tên thân thiện:

```typescript
const StatusCodes = {
  InternalServerError: 500,
  NotFound: 404,
  Ok: 200,
  // ...
} as const;

StatusCodes.InternalServerError;  // 500
```

Điều phức tạp với các đối tượng giống như enum trong TypeScript là không có cách nào tốt trong hệ thống kiểu để đại diện cho việc một giá trị phải là một trong các giá trị của chúng. Một phương pháp phổ biến là sử dụng các bổ từ kiểu `keyof` và `typeof` từ Chương 9, “Các bổ từ kiểu” để tạo ra một kiểu, nhưng đó là một lượng cú pháp đáng kể phải gõ ra.

Kiểu `StatusCodeValue` sau đây sử dụng giá trị `StatusCodes` trước đó để tạo một union type gồm các giá trị số mã trạng thái có thể có của nó:

```typescript
// Type: 200 | 404 | 500
type StatusCodeValue= (typeof StatusCodes)[keyof typeof StatusCodes];

let statusCodeValue: StatusCodeValue;

statusCodeValue = 200;  // Ok
statusCodeValue=-1;
// Error: Type '-1' is not assignable to type 'StatusCodeValue'.
```

TypeScript cung cấp một cú pháp `enum` để tạo một đối tượng có các giá trị literal thuộc kiểu `number` hoặc `string`. Bắt đầu bằng từ khóa `enum`, sau đó là tên của một đối tượng—theo quy ước ở dạng PascalCase—sau đó là một đối tượng `{}` chứa các khóa được phân tách bằng dấu phẩy trong enum. Mỗi khóa có thể tùy chọn sử dụng dấu `=` trước một giá trị ban đầu.

Đối tượng `StatusCodes` trước đó sẽ trông giống như enum `StatusCode` này:

```typescript
enum StatusCode {
  InternalServerError = 500,
  NotFound = 404,
  Ok = 200,
}

StatusCode.InternalServerError;  // 500
```

Cũng như với tên lớp, tên enum như `StatusCode` có thể được sử dụng làm tên kiểu trong một chú thích kiểu. Ở đây, biến `statusCode` có kiểu `StatusCode` có thể được cung cấp `StatusCode.Ok` hoặc một giá trị số:

```typescript
let statusCode: StatusCode;
statusCode = StatusCode.Ok;  // Ok
statusCode = 200;  // Ok
```

#### **CẢNH BÁO (WARNING)**

TypeScript cho phép bất kỳ số nào được gán cho một giá trị enum số như một sự tiện lợi nhưng phải đánh đổi một chút tính an toàn kiểu. `statusCode = -1` cũng sẽ được phép trong đoạn mã trước.

Enums biên dịch xuống một đối tượng tương đương trong JavaScript đầu ra đã biên dịch. Mỗi thành viên của chúng trở thành một khóa thành viên đối tượng với giá trị tương ứng và ngược lại.

`enum StatusCode` trước đó sẽ tạo ra đại khái mã JavaScript sau:

```javascript
var StatusCode;
(function (StatusCode) {
StatusCode[StatusCode["InternalServerError"] =500] =
"InternalServerError";
StatusCode[StatusCode["NotFound"] =404] ="NotFound";
StatusCode[StatusCode["Ok"] =200] ="Ok";
})(StatusCode|| (StatusCode= {}));
```

Enums là một chủ đề hơi gây tranh cãi trong cộng đồng TypeScript. Một mặt, chúng vi phạm nguyên tắc chung của TypeScript là không bao giờ thêm các cấu trúc cú pháp thời gian chạy mới vào JavaScript. Chúng đưa ra một cú pháp phi JavaScript mới để các lập trình viên phải học và có một vài điểm kỳ quặc xung quanh các tùy chọn như `preserveConstEnums`, được đề cập sau trong chương này.

Mặt khác, chúng khá hữu ích để khai báo rõ ràng các tập hợp giá trị đã biết. Enums được sử dụng rộng rãi trong cả kho lưu trữ mã nguồn của TypeScript và VS Code!

### **Giá trị số tự động (Automatic Numeric Values)**

Các thành viên enum không cần phải có giá trị ban đầu rõ ràng. Khi các giá trị bị bỏ qua, TypeScript sẽ bắt đầu giá trị đầu tiên bằng `0` và tăng mỗi giá trị tiếp theo lên `1`. Cho phép TypeScript chọn các giá trị cho các thành viên enum là một lựa chọn tốt khi giá trị không quan trọng ngoài việc là duy nhất và được liên kết với tên khóa.

Enum `VisualTheme` này cho phép TypeScript tự chọn các giá trị hoàn toàn, dẫn đến ba số nguyên:

```typescript
enum VisualTheme {
  Dark,  // 0
  Light,  // 1
  System,  // 2
}
```

JavaScript được phát ra trông giống hệt như thể các giá trị đã được thiết lập rõ ràng:

```javascript
var VisualTheme;
(function (VisualTheme) {
VisualTheme[VisualTheme["Dark"] =0] ="Dark";
VisualTheme[VisualTheme["Light"] =1] ="Light";
VisualTheme[VisualTheme["System"] =2] ="System";
})(VisualTheme|| (VisualTheme= {}));
```

Trong các enum có giá trị số, bất kỳ thành viên nào thiếu giá trị rõ ràng sẽ lớn hơn giá trị trước đó `1`.

Ví dụ: một enum `Direction` có thể chỉ quan tâm rằng thành viên `Top` của nó có giá trị là `1` và các giá trị còn lại cũng là các số nguyên dương:

```typescript
enum Direction {
  Top = 1,
  Right,
  Bottom,
  Left,
}
```

JavaScript đầu ra của nó cũng sẽ trông giống như thể các thành viên còn lại có các giá trị rõ ràng `2`, `3`, và `4`:

```javascript
var Direction;
(function (Direction) {
Direction[Direction["Top"] =1] ="Top";
Direction[Direction["Right"] =2] ="Right";
Direction[Direction["Bottom"] =3] ="Bottom";
Direction[Direction["Left"] =4] ="Left";
})(Direction|| (Direction= {}));
```

#### **CẢNH BÁO (WARNING)**

Việc sửa đổi thứ tự của một enum sẽ làm cho số bên dưới thay đổi. Nếu bạn lưu trữ các giá trị này ở đâu đó, chẳng hạn như cơ sở dữ liệu, hãy cẩn thận khi thay đổi thứ tự enum hoặc xóa một mục. Dữ liệu của bạn có thể đột nhiên bị hỏng vì số đã lưu sẽ không còn đại diện cho những gì mã của bạn mong đợi nữa.

### **Enums có giá trị chuỗi (String-Valued Enums)**

Enums cũng có thể sử dụng chuỗi cho các thành viên của chúng thay vì số. Enum `LoadStyle` này sử dụng các giá trị chuỗi thân thiện cho các thành viên của nó:

```typescript
enum LoadStyle {
AsNeeded = "as-needed",
Eager = "eager",
}
```

JavaScript đầu ra cho các enum có giá trị thành viên chuỗi trông tương tự về mặt cấu trúc như các enum có giá trị thành viên số:

```typescript
var LoadStyle;
(function (LoadStyle) {
LoadStyle["AsNeeded"] ="as-needed";
LoadStyle["Eager"] ="eager";
})(LoadStyle|| (LoadStyle= {}));
```

Các enum có giá trị chuỗi rất tiện lợi để tạo bí danh cho các hằng số dùng chung dưới các tên dễ đọc. Thay vì sử dụng một union type của các string literals, các enum có giá trị chuỗi cho phép tự động hoàn thành và đổi tên các thuộc tính đó mạnh mẽ hơn trong trình soạn thảo—như đã đề cập trong Chương 12, “Sử dụng các tính năng IDE”.

Một nhược điểm của các giá trị thành viên chuỗi là chúng không thể được TypeScript tính toán tự động. Chỉ những thành viên enum đứng sau một thành viên có giá trị số mới được phép tính toán tự động.

TypeScript sẽ có thể cung cấp giá trị ngầm định là `9001` trong `ImplicitNumber` của enum này vì giá trị thành viên trước đó là số `9000`, nhưng thành viên `NotAllowed` của nó sẽ đưa ra lỗi vì nó đứng sau một giá trị thành viên chuỗi:

```typescript
enum Wat {
FirstString = "first",
SomeNumber = 9000,
ImplicitNumber,  // Ok (value 9001)
AnotherString = "another",
NotAllowed,

// Error: Enum member must have initializer.
}
```

#### **MẸO (TIP)**

Về lý thuyết, bạn có thể tạo một enum có cả giá trị thành viên dạng số và chuỗi. Trong thực tế, enum đó có thể sẽ gây nhầm lẫn không cần thiết, vì vậy bạn có lẽ không nên làm như vậy.

### **Const Enums**

Vì các enum tạo ra một đối tượng thời gian chạy, việc sử dụng chúng tạo ra nhiều mã hơn so với chiến lược thay thế phổ biến là các unions của các giá trị literal. TypeScript cho phép khai báo các enum với bổ từ `const` ở phía trước để yêu cầu TypeScript bỏ qua định nghĩa đối tượng của chúng và việc tra cứu thuộc tính khỏi mã JavaScript đã biên dịch.

Enum `DisplayHint` này được sử dụng làm giá trị cho một biến `displayHint`:

```typescript
const enum DisplayHint {
  Opaque = 0,
  Semitransparent,
  Transparent,
}

let displayHint = DisplayHint.Transparent;
```

Mã JavaScript đã biên dịch đầu ra sẽ hoàn toàn thiếu khai báo enum và sẽ sử dụng một chú thích cho giá trị của enum:

```javascript
let displayHint = 2 /* DisplayHint.Transparent */;
```

Đối với các dự án vẫn muốn tạo các định nghĩa đối tượng enum, tùy chọn trình biên dịch `preserveConstEnums` thực sự tồn tại để giữ cho chính khai báo enum tồn tại. Các giá trị vẫn sẽ trực tiếp sử dụng literals thay vì truy cập chúng trên đối tượng enum.

Đoạn mã trước đó vẫn sẽ bỏ qua việc tra cứu thuộc tính trong đầu ra JavaScript đã biên dịch của nó:

```javascript
var DisplayHint;
(function (DisplayHint) {
DisplayHint[DisplayHint["Opaque"] =0] ="Opaque";
DisplayHint[DisplayHint["Semitransparent"] =1] ="Semitransparent";
DisplayHint[DisplayHint["Transparent"] =2] ="Transparent";
})(DisplayHint|| (DisplayHint= {}));

let displayHint = 2/* Transparent */;
```

`preserveConstEnums` có thể giúp giảm kích thước của mã JavaScript được phát ra, mặc dù không phải tất cả các cách chuyển mã TypeScript đều hỗ trợ nó. Xem Chương 13, “Các tùy chọn cấu hình” để biết thêm thông tin về tùy chọn trình biên dịch `isolatedModules` và thời điểm `const` enums có thể không được hỗ trợ.

## **Không gian tên (Namespaces)**

#### **CẢNH BÁO (WARNING)**

Trừ khi bạn đang viết các định nghĩa kiểu DefinitelyTyped cho một gói hiện có, đừng sử dụng namespaces. Namespaces không phù hợp với ngữ nghĩa module JavaScript hiện đại. Các phép gán thành viên tự động của chúng có thể làm cho mã trở nên khó hiểu. Tôi chỉ đề cập đến chúng vì bạn có thể bắt gặp chúng trong các tệp _.d.ts_.

Trước khi ECMAScript modules được phê chuẩn, các ứng dụng web thường đóng gói phần lớn mã đầu ra của chúng thành một tệp duy nhất được tải bởi trình duyệt. Những tệp đơn khổng lồ đó thường tạo ra các biến toàn cục để giữ các tham chiếu đến các giá trị quan trọng trên các khu vực khác nhau của dự án. Sẽ đơn giản hơn cho các trang web khi chỉ đưa vào một tệp đó thay vì thiết lập một trình tải module cũ như RequireJS—và thường có hiệu suất tải tốt hơn, vì nhiều máy chủ khi đó chưa hỗ trợ luồng tải xuống HTTP/2. Các dự án được tạo cho đầu ra tệp đơn cần một cách để sắp xếp các phần mã và các biến toàn cục đó.

Ngôn ngữ TypeScript đã cung cấp một giải pháp với khái niệm “internal modules” (các module nội bộ), hiện được gọi là namespaces. Một _namespace_ là một đối tượng có sẵn trên phạm vi toàn cục với nội dung được “export” có sẵn để gọi dưới dạng các thành viên của đối tượng đó. Namespaces được định nghĩa bằng từ khóa `namespace` theo sau bởi một khối mã `{}`. Mọi thứ trong khối namespace đó đều được đánh giá bên trong một bao đóng hàm (function closure). Namespace `Randomized` này tạo ra một biến `value` và sử dụng nó trong nội bộ:

```javascript
namespace Randomized {
const value = Math.random();
console.log(`My value is ${value}`);
}
```

JavaScript đầu ra của nó tạo ra một đối tượng `Randomized` và đánh giá nội dung của khối bên trong một hàm, vì vậy biến `value` không có sẵn bên ngoài namespace:

```javascript
var Randomized;
(function (Randomized) {
const value = Math.random();
console.log(`My value is ${value}`);
})(Randomized|| (Randomized= {}));
```

#### **CẢNH BÁO (WARNING)**

Namespaces và từ khóa `namespace` ban đầu được gọi lần lượt là “modules” và "`module`” trong TypeScript. Nhìn lại thì đó là một lựa chọn đáng tiếc do sự trỗi dậy của các trình tải module hiện đại và ECMAScript modules. Từ khóa `module` đôi khi vẫn được tìm thấy trong các dự án rất cũ, nhưng có thể—và nên—được thay thế an toàn bằng `namespace`.

### **Export trong Namespace (Namespace Exports)**

Tính năng then chốt của namespaces khiến chúng trở nên hữu ích là một namespace có thể “export” nội dung bằng cách biến chúng thành thành viên của đối tượng namespace. Các vùng mã khác sau đó có thể tham chiếu đến thành viên đó theo tên.

Ở đây, một namespace `Settings` export các giá trị `describe`, `name`, và `version` được sử dụng cả bên trong và bên ngoài namespace:

```typescript
namespace Settings {
  export const name = "My Application";
  export const version = "1.2.3";
  export function describe() {
    return `${Settings.name} at version ${Settings.version}`;
  }
  console.log("Initializing", describe());
}
console.log("Initialized", Settings.describe());
```

JavaScript đầu ra cho thấy rằng các giá trị luôn được tham chiếu như các thành viên của `Settings` (ví dụ: `Settings.name`) trong cả cách sử dụng bên trong và bên ngoài:

```javascript
var Settings;
(function (Settings) {
Settings.name = "My Application";
Settings.version = "1.2.3";
function describe() {
return`${Settings.name} at version ${Settings.version}`;
    }
Settings.describe = describe;
console.log("Initializing", describe());
})(Settings|| (Settings= {}));
console.log("Initialized", Settings.describe());
```

Bằng cách sử dụng một `var` cho đối tượng đầu ra và tham chiếu nội dung được export dưới dạng các thành viên của các đối tượng đó, namespaces theo thiết kế hoạt động rất tốt khi được phân chia trên nhiều tệp. Namespace `Settings` trước đó có thể được viết lại trên nhiều tệp:

```typescript
// settings/constants.ts
namespace Settings {
  export const name = "My Application";
  export const version = "1.2.3";
}
// settings/describe.ts
namespace Settings {
  export function describe() {
    return `${Settings.name} at version ${Settings.version}`;
  }
  console.log("Initializing", describe());
}
// index.ts
console.log("Initialized", Settings.describe());
```

Mã JavaScript đầu ra, khi được ghép lại với nhau, sẽ trông đại loại như sau:

```javascript
// settings/constants.ts
var Settings;
(function (Settings) {
Settings.name = "My Application";
Settings.version = "1.2.3";
})(Settings|| (Settings= {}));
// settings/describe.ts
(function (Settings) {
function describe() {
return`${Settings.name} at version ${Settings.version}`;
    }
Settings.describe = describe;
console.log("Initialized", describe());
})(Settings|| (Settings= {}));
console.log("Initialized", Settings.describe());
```

Trong cả hai dạng khai báo tệp đơn và nhiều tệp, đối tượng đầu ra tại thời gian chạy là một đối tượng có ba khóa. Đại khái là:

```javascript
const Settings = {
  describe: function describe() {
    return `${Settings.name} at version ${Settings.version}`;
  },
  name: "My Application",
  version: "1.2.3",
};
```

Điểm khác biệt chính khi sử dụng một namespace là nó có thể được chia tách trên các tệp khác nhau và các thành viên vẫn có thể tham chiếu lẫn nhau dưới tên của namespace.

### **Namespaces lồng nhau (Nested Namespaces)**

Các namespace có thể được “lồng nhau” ở các cấp độ không giới hạn bằng cách export một namespace từ bên trong một namespace khác hoặc đặt một hoặc nhiều dấu chấm `.` bên trong một tên.

Hai khai báo namespace sau đây sẽ hoạt động giống hệt nhau:

```typescript
namespace Root.Nested {
export const value1 = true;
}
namespace Root {
export namespace Nested {
export const value2 = true;
    }
}
```

Cả hai đều biên dịch thành mã giống nhau về mặt cấu trúc:

```javascript
(function (Root) {
let Nested;
    (function (Nested) {
Nested.value2 = true;
    })(Nested|| (Nested= {}));
})(Root|| (Root= {}));
```

Các namespace lồng nhau là một cách tiện dụng để thực thi sự phân định rõ ràng hơn giữa các phần trong các dự án lớn hơn được tổ chức bằng namespaces. Nhiều lập trình viên đã chọn sử dụng một namespace gốc theo tên dự án của họ—có lẽ bên trong một namespace cho công ty và/hoặc tổ chức của họ—và các namespace con cho từng khu vực chính của dự án.

### **Namespaces trong định nghĩa kiểu (Namespaces in Type Definitions)**

Phẩm chất có giá trị duy nhất cho namespaces ngày nay—và là lý do duy nhất khiến tôi quyết định đưa chúng vào cuốn sách này—là chúng có thể hữu ích cho các định nghĩa kiểu DefinitelyTyped. Nhiều thư viện JavaScript—đặc biệt là các thư viện web cổ điển như jQuery—được thiết lập để đưa vào trình duyệt web bằng thẻ `<script>` truyền thống, không phải module. Typings của chúng cần chỉ ra rằng chúng tạo ra một biến toàn cục có sẵn cho tất cả mã nguồn—một cấu trúc được nắm bắt hoàn hảo bởi namespaces.

Ngoài ra, nhiều thư viện JavaScript có khả năng chạy trong trình duyệt được thiết lập vừa để import trong các hệ thống module hiện đại hơn vừa để tạo ra một namespace toàn cục. TypeScript cho phép một định nghĩa kiểu module bao gồm một `export as namespace`, theo sau là một tên toàn cục, để chỉ ra rằng module cũng có sẵn trên phạm vi toàn cục dưới tên đó.

Ví dụ: tệp khai báo này cho một module export một `value` và có sẵn trên phạm vi toàn cục:

```typescript
// node_modules/@types/my-example-lib/index.d.ts
export const value: number;
export as namespace libExample;
```

Hệ thống kiểu sẽ biết rằng cả `import("my-example-lib")` và `window.libExample` đều sẽ trả về module đó, với thuộc tính `value` có kiểu `number`:

```typescript
// src/index.ts
import *as libExample from "my-example-lib";  // Ok
const value = window.libExample.value;  // Ok
```

### **Ưu tiên Modules hơn Namespaces (Prefer Modules Over Namespaces)**

Thay vì sử dụng namespaces, tệp _settings/constants.ts_ và tệp _settings/describe.ts_ của các ví dụ trước có thể được viết lại theo các tiêu chuẩn hiện đại với ECMAScript modules:

```typescript
// settings/constants.ts
export const name = "My Application";
export const version = "1.2.3";

// settings/describe.ts
import { name, version } from "./constants";
export function describe() {
  return `${Settings.name} at version ${Settings.version}`;
}

console.log("Initializing", describe());

// index.ts
import { describe } from "./settings/describe";
console.log("Initialized", describe());
```

Mã TypeScript được cấu trúc với namespaces không thể dễ dàng được tối ưu hóa loại bỏ mã thừa (tree-shaken) trong các công cụ build hiện đại như Webpack vì namespaces tạo ra các ràng buộc ngầm định thay vì khai báo rõ ràng giữa các tệp như cách ECMAScript modules làm. Nhìn chung rất nên viết mã thời gian chạy bằng ECMAScript modules chứ không phải TypeScript namespaces.

#### **GHI CHÚ (NOTE)**

Tính đến năm 2022, bản thân TypeScript được viết bằng namespaces, nhưng nhóm TypeScript đang làm việc để chuyển đổi sang modules. Ai biết được, có thể khi bạn đang đọc cuốn sách này, họ đã hoàn thành việc chuyển đổi đó rồi! Cùng hy vọng điều đó nhé.

## **Chỉ Import và Export Kiểu (Type-Only Imports and Exports)**

Tôi muốn kết thúc chương này bằng một lưu ý tích cực. Một tập hợp các phần mở rộng cú pháp cuối cùng, các lệnh import và export chỉ dành cho kiểu (type-only imports and exports), có thể khá hữu ích và không làm tăng thêm bất kỳ sự phức tạp nào cho JavaScript đầu ra được phát ra.

Trình chuyển mã của TypeScript sẽ loại bỏ các giá trị chỉ được sử dụng trong hệ thống kiểu khỏi các import và export trong tệp vì chúng không được sử dụng trong JavaScript thời gian chạy.

Ví dụ: tệp _index.ts_ sau đây tạo một biến `action` và một kiểu `ActivistArea`, sau đó export cả hai bằng một khai báo export độc lập. Khi biên dịch nó thành _index.js_, trình chuyển mã của TypeScript sẽ biết loại bỏ `ActivistArea` khỏi khai báo export độc lập đó:

```typescript
// index.ts
const action = { area: "people", name: "Bella Abzug", role: "politician" };

type ActivistArea = "nature" | "people";

export { action, ActivistArea };

// index.js
const action = { area: "people", name: "Bella Abzug", role: "politician" };

export { action };
```

Việc biết cách loại bỏ các kiểu được export lại như `ActivistArea` đó đòi hỏi sự hiểu biết về hệ thống kiểu TypeScript. Các trình chuyển mã như Babel chỉ hoạt động trên một tệp tại một thời điểm không có quyền truy cập vào hệ thống kiểu TypeScript để biết liệu mỗi tên có chỉ được sử dụng trong hệ thống kiểu hay không. Tùy chọn trình biên dịch `isolatedModules` của TypeScript, được đề cập trong Chương 13, “Các tùy chọn cấu hình”, giúp đảm bảo mã sẽ chuyển mã được trong các công cụ khác ngoài TypeScript.

TypeScript cho phép thêm bổ từ `type` vào trước các tên được import riêng lẻ hoặc toàn bộ đối tượng `{...}` trong các khai báo `export` và `import`. Làm như vậy sẽ chỉ ra rằng chúng chỉ nhằm mục đích sử dụng trong hệ thống kiểu. Việc đánh dấu default import của một gói là `type` cũng được phép.

Trong đoạn mã sau, chỉ có import và export `value` được giữ lại khi _index.ts_ được chuyển mã thành _index.js_ đầu ra:

```typescript
// index.ts
import { type TypeOne, value } from "my-example-types";
import type { TypeTwo } from "my-example-types";
import type DefaultType from "my-example-types";

export { type TypeOne, value };
export type { DefaultType, TypeTwo };

// index.js
import { value } from "my-example-types";
export { value };
```

Một số lập trình viên TypeScript thậm chí còn thích chọn sử dụng các type-only imports để làm rõ ràng hơn những import nào chỉ được sử dụng làm kiểu. Nếu một lệnh import được đánh dấu là type-only, việc cố gắng sử dụng nó như một giá trị thời gian chạy sẽ kích hoạt lỗi TypeScript.

`ClassOne` sau đây được import bình thường và có thể được sử dụng tại thời gian chạy, nhưng `ClassTwo` thì không thể vì nó được import dưới dạng một kiểu:

```typescript
import { ClassOne, type ClassTwo } from "my-example-types";

new ClassOne();  // Ok

new ClassTwo();
//  ~~~~~~~~
// Error: 'ClassTwo' cannot be used as a value
// because it was imported using 'import type'.
```

Thay vì thêm sự phức tạp vào JavaScript được phát ra, các type-only imports và exports làm cho các trình chuyển mã bên ngoài TypeScript hiểu rõ khi nào có thể xóa các phần mã. Do đó, hầu hết các nhà phát triển TypeScript không đối xử với chúng bằng sự ác cảm như đối với các phần mở rộng cú pháp trước đó được đề cập trong chương này.

## **Tổng kết**

Trong chương này, bạn đã làm việc với một số phần mở rộng cú pháp JavaScript có trong TypeScript:

- Khai báo các thuộc tính tham số của lớp trong constructor của lớp
- Sử dụng decorators để bổ sung các lớp và các trường của chúng
- Biểu diễn các nhóm giá trị bằng enums
- Sử dụng namespaces để tạo các nhóm trên nhiều tệp hoặc trong các định nghĩa kiểu
- Chỉ import và export kiểu (Type-only imports and exports)

#### **MẸO (TIP)**

Bây giờ bạn đã đọc xong chương này, hãy thực hành những gì bạn đã học tại _https://learningtypescript.com/syntax-extensions_.

_Bạn gọi chi phí hỗ trợ các phần mở rộng JavaScript cũ trong TypeScript là gì?_

_“Thuế tội lỗi (Sin tax / Syntax).”_
