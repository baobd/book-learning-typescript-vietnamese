# **Chương 8. Lớp (Classes)**

## Mục lục

- [**Chương 8. Lớp (Classes)**](#chương-8-lớp-classes)
  - [**Các phương thức của lớp (Class Methods)**](#các-phương-thức-của-lớp-class-methods)
  - [**Các thuộc tính của lớp (Class Properties)**](#các-thuộc-tính-của-lớp-class-properties)
    - [**Thuộc tính hàm (Function Properties)**](#thuộc-tính-hàm-function-properties)
    - [**Kiểm tra khởi tạo (Initialization Checking)**](#kiểm-tra-khởi-tạo-initialization-checking)
      - [**Các thuộc tính được gán chắc chắn (Definitely assigned properties)**](#các-thuộc-tính-được-gán-chắc-chắn-definitely-assigned-properties)
      - [**CẢNH BÁO (WARNING)**](#cảnh-báo-warning)
    - [**Thuộc tính tùy chọn (Optional Properties)**](#thuộc-tính-tùy-chọn-optional-properties)
    - [**Thuộc tính chỉ đọc (Read-Only Properties)**](#thuộc-tính-chỉ-đọc-read-only-properties)
      - [**CẢNH BÁO (WARNING)**](#cảnh-báo-warning-1)
  - [**Lớp với vai trò là Kiểu dữ liệu (Classes as Types)**](#lớp-với-vai-trò-là-kiểu-dữ-liệu-classes-as-types)
      - [**MẸO (TIP)**](#mẹo-tip)
  - [**Lớp và Giao diện (Classes and Interfaces)**](#lớp-và-giao-diện-classes-and-interfaces)
      - [**GHI CHÚ (NOTE)**](#ghi-chú-note)
    - [**Triển khai nhiều giao diện (Implementing Multiple Interfaces)**](#triển-khai-nhiều-giao-diện-implementing-multiple-interfaces)
  - [**Mở rộng một Lớp (Extending a Class)**](#mở-rộng-một-lớp-extending-a-class)
    - [**Khả năng gán của lớp kế thừa (Extension Assignability)**](#khả-năng-gán-của-lớp-kế-thừa-extension-assignability)
      - [**MẸO (TIP)**](#mẹo-tip-1)
    - [**Ghi đè hàm khởi tạo (Overridden Constructors)**](#ghi-đè-hàm-khởi-tạo-overridden-constructors)
    - [**Ghi đè phương thức (Overridden Methods)**](#ghi-đè-phương-thức-overridden-methods)
    - [**Ghi đè thuộc tính (Overridden Properties)**](#ghi-đè-thuộc-tính-overridden-properties)
  - [**Lớp trừu tượng (Abstract Classes)**](#lớp-trừu-tượng-abstract-classes)
  - [**Phạm vi hiển thị của thành viên (Member Visibility)**](#phạm-vi-hiển-thị-của-thành-viên-member-visibility)
      - [_`protected`_](#_protected_)
    - [**Các bổ từ trường tĩnh (Static Field Modifiers)**](#các-bổ-từ-trường-tĩnh-static-field-modifiers)
  - [**Tổng kết**](#tổng-kết)
    - [**MẸO (TIP)**](#mẹo-tip-2)

_Một số lập trình viên hướng hàm_

_Cố gắng không bao giờ dùng classes_

_Quá căng thẳng đối với tôi_

Thế giới JavaScript trong thời kỳ TypeScript ra đời và phát hành vào đầu những năm 2010 rất khác so với ngày nay. Các tính năng như arrow functions và các biến `let` / `const` mà sau này được chuẩn hóa trong ES2015 khi đó vẫn chỉ là những kỳ vọng xa vời ở phía chân trời. Babel lúc đó còn vài năm nữa mới có commit đầu tiên; các công cụ tiền nhiệm của nó như Traceur dùng để chuyển đổi cú pháp JavaScript mới hơn sang cú pháp cũ vẫn chưa đạt được sự chấp nhận rộng rãi.

Chiến lược tiếp thị ban đầu và bộ tính năng của TypeScript đã được điều chỉnh riêng cho thế giới đó. Ngoài việc kiểm tra kiểu, trình chuyển mã (transpiler) của nó cũng được nhấn mạnh—với classes là một ví dụ thường xuyên được đưa ra. Ngày nay, sự hỗ trợ class của TypeScript chỉ là một tính năng trong số rất nhiều tính năng để hỗ trợ toàn bộ các đặc tính ngôn ngữ JavaScript. TypeScript không khuyến khích cũng không ngăn cản việc sử dụng class hoặc bất kỳ mô hình JavaScript phổ biến nào khác.

## **Các phương thức của lớp (Class Methods)**

TypeScript nhìn chung hiểu các phương thức theo cùng một cách mà nó hiểu các hàm độc lập. Kiểu tham số mặc định là `any` trừ khi được cung cấp một kiểu hoặc giá trị mặc định; việc gọi phương thức đòi hỏi một số lượng đối số chấp nhận được; các kiểu trả về nhìn chung có thể được suy luận nếu hàm không phải là đệ quy.

Đoạn mã này định nghĩa một lớp `Greeter` với phương thức lớp `greet` nhận vào một tham số bắt buộc duy nhất có kiểu `string`:

```typescript
class Greeter {
greet(name: string) {

console.log(`${name}, do your stuff!`);
    }
}
new Greeter().greet("Miss Frizzle");  // Ok
new Greeter().greet();
//            ~~~~~
// Error: Expected 1 arguments, but got 0.
```

Hàm khởi tạo của lớp (constructor) được đối xử giống như các phương thức lớp thông thường đối với các tham số của chúng. TypeScript sẽ thực hiện kiểm tra kiểu để đảm bảo cung cấp đúng số lượng đối số với các kiểu chính xác cho các lời gọi phương thức.

Constructor của `Greeted` này cũng mong đợi tham số `message: string` của nó được cung cấp:

```typescript
class Greeted {
constructor(message: string) {
console.log(`As I always say: ${message}!`);
    }
}
new Greeted("take chances, make mistakes, get messy");

new Greeted();
// Error: Expected 1 arguments, but got 0.
```

Tôi sẽ đề cập đến các constructor trong ngữ cảnh của các lớp con (subclasses) sau trong chương này.

## **Các thuộc tính của lớp (Class Properties)**

Để đọc từ hoặc ghi vào một thuộc tính trên một lớp trong TypeScript, nó phải được khai báo rõ ràng trong lớp. Các thuộc tính của lớp được khai báo bằng cú pháp tương tự như interfaces: tên của chúng theo sau tùy chọn bởi một chú thích kiểu. TypeScript sẽ không cố gắng suy ra những thành viên nào có thể tồn tại trên một lớp từ các phép gán của chúng bên trong constructor.

Trong ví dụ này, `destination` được phép gán cho và truy cập trên các thể hiện của lớp `FieldTrip` vì nó được khai báo tường minh là một `string`. Phép gán `this.nonexistent` trong constructor không được phép vì lớp không khai báo thuộc tính `nonexistent`:

```typescript
class FieldTrip {
  destination: string;

  constructor(destination: string) {
    this.destination = destination;  // Ok
    console.log(`We're going to ${this.destination}!`);

    this.nonexistent = destination;
    //   ~~~~~~~~~~~
    // Error: Property 'nonexistent' does not exist on type 'FieldTrip'.
  }
}
```

Việc khai báo tường minh các thuộc tính của lớp cho phép TypeScript nhanh chóng hiểu được điều gì được phép hoặc không được phép tồn tại trên các thể hiện của lớp. Sau này, khi các thể hiện của lớp được sử dụng, TypeScript sẽ sử dụng hiểu biết đó để đưa ra lỗi kiểu nếu mã cố gắng truy cập một thành viên của một thể hiện lớp không được biết là có tồn tại, chẳng hạn như với `trip.nonexistent` ở phần tiếp theo này:

```typescript
const trip = new FieldTrip("planetarium");

trip.destination;  // Ok
trip.nonexistent;
//   ~~~~~~~~~~~
// Error: Property 'nonexistent' does not exist on type 'FieldTrip'.
```

### **Thuộc tính hàm (Function Properties)**

Hãy cùng tóm tắt lại một số nguyên tắc cơ bản về phạm vi và cú pháp phương thức JavaScript một chút, vì chúng có thể gây ngạc nhiên nếu bạn chưa quen với chúng. JavaScript chứa hai cú pháp để khai báo một thành viên trên một lớp là một hàm có thể gọi được: _method_ (phương thức) và _property_ (thuộc tính).

Tôi đã trình bày phương pháp method bằng cách đặt dấu ngoặc đơn sau tên thành viên, chẳng hạn như `myFunction() {}`. Cách tiếp cận method sẽ gán một hàm cho prototype của lớp, vì vậy tất cả các thể hiện của lớp đều sử dụng cùng một định nghĩa hàm.

Lớp `WithMethod` này khai báo một phương thức `myMethod` mà tất cả các thể hiện đều có thể tham chiếu tới:

```typescript
class WithMethod {
  myMethod() {}
}

new WithMethod().myMethod === new WithMethod().myMethod;  // true
```

Cú pháp khác là khai báo một thuộc tính có giá trị tình cờ là một hàm. Điều này tạo ra một hàm mới cho mỗi thể hiện của lớp, có thể hữu ích với các arrow functions `() =>` có phạm vi `this` luôn trỏ đến thể hiện của lớp (với chi phí thời gian và bộ nhớ khi tạo một hàm mới cho mỗi thể hiện lớp).

Lớp `WithProperty` này chứa một thuộc tính duy nhất có tên `myProperty` và kiểu `() => void` sẽ được tạo lại cho mỗi thể hiện của lớp:

```typescript
class WithProperty {
  myProperty: () => {}
}

new WithProperty().myProperty === new WithProperty().myProperty;  // false
```

Các thuộc tính hàm có thể được cung cấp các tham số và kiểu trả về bằng cách sử dụng cùng cú pháp như các phương thức lớp và hàm độc lập. Rốt cuộc, chúng là một giá trị được gán cho một thành viên lớp và giá trị đó tình cờ là một hàm.

Lớp `WithPropertyParameters` này có một thuộc tính `takesParameters` có kiểu `(input: boolean) => string`:

```typescript
class WithPropertyParameters {
takesParameters= (input: boolean) => input?"Yes":"No";
}

const instance = new WithPropertyParameters();
instance.takesParameters(true);  // Ok
instance.takesParameters(123);
//                       ~~~
// Error: Argument of type 'number' is not
// assignable to parameter of type 'boolean'.
```

### **Kiểm tra khởi tạo (Initialization Checking)**

Khi các thiết lập trình biên dịch nghiêm ngặt được bật, TypeScript sẽ kiểm tra xem mỗi thuộc tính được khai báo có kiểu không bao gồm `undefined` có được gán một giá trị trong constructor hay không. Việc kiểm tra khởi tạo nghiêm ngặt này rất hữu ích vì nó ngăn ngừa mã vô tình quên gán giá trị cho một thuộc tính của lớp.

Lớp `WithValue` sau đây không gán giá trị cho thuộc tính `unused` của nó, điều mà TypeScript nhận diện là một lỗi kiểu:

```typescript
class WithValue {
immediate = 0;  // Ok
later: number;  // Ok (set in the constructor)
mayBeUndefined: number | undefined;  // Ok (allowed to be undefined)
unused: number;
// Error: Property 'unused' has no initializer
// and is not definitely assigned in the constructor.
constructor() {
this.later = 1;
    }
}
```

Nếu không có kiểm tra khởi tạo nghiêm ngặt, một thể hiện của lớp có thể được phép truy cập một giá trị có thể là `undefined` mặc dù hệ thống kiểu nói rằng nó không thể như vậy.

Ví dụ này sẽ biên dịch suôn sẻ nếu việc kiểm tra khởi tạo nghiêm ngặt không diễn ra, nhưng mã JavaScript tạo ra sẽ bị crash tại runtime:

```typescript
class MissingInitializer {
property: string;
}
new MissingInitializer().property.length;
// TypeError: Cannot read property 'length' of undefined
```

Sai lầm tỷ đô lại tấn công một lần nữa!

Việc cấu hình kiểm tra khởi tạo thuộc tính nghiêm ngặt với tùy chọn trình biên dịch `strictPropertyInitialization` của TypeScript được đề cập trong Chương 13, “Các tùy chọn cấu hình”.

#### **Các thuộc tính được gán chắc chắn (Definitely assigned properties)**

Mặc dù kiểm tra khởi tạo nghiêm ngặt là hữu ích trong hầu hết thời gian, bạn có thể gặp một số trường hợp mà một thuộc tính của lớp được cố ý để chưa được gán sau constructor của lớp. Nếu bạn hoàn toàn chắc chắn một thuộc tính không nên áp dụng kiểm tra khởi tạo nghiêm ngặt, bạn có thể thêm dấu `!` sau tên của nó để vô hiệu hóa kiểm tra. Làm như vậy sẽ khẳng định với TypeScript rằng thuộc tính sẽ được gán một giá trị khác `undefined` trước lần sử dụng đầu tiên của nó.

Lớp `ActivitiesQueue` này được dự định khởi tạo lại bất kỳ số lần nào tách biệt khỏi constructor của nó, vì vậy thuộc tính `pending` của nó phải được khẳng định bằng dấu `!`:

```typescript
class ActivitiesQueue {
pending!:string[];  // Ok
initialize(pending: string[]) {
this.pending = pending;
    }
next() {
return this.pending.pop();
    }
}
const activities = new ActivitiesQueue();

activities.initialize(['eat', 'sleep', 'learn'])
activities.next();
```

#### **CẢNH BÁO (WARNING)**

Việc cần phải vô hiệu hóa kiểm tra khởi tạo nghiêm ngặt trên một thuộc tính của lớp thường là dấu hiệu cho thấy mã được thiết lập theo cách không thuận tiện cho việc kiểm tra kiểu. Thay vì thêm một khẳng định `!` và làm giảm tính an toàn kiểu cho thuộc tính, hãy cân nhắc tái cấu trúc lớp để không còn cần đến khẳng định đó nữa.

### **Thuộc tính tùy chọn (Optional Properties)**

Tương tự như interfaces, các lớp trong TypeScript có thể khai báo một thuộc tính là tùy chọn bằng cách thêm dấu `?` sau tên khai báo của nó. Các thuộc tính tùy chọn hoạt động gần giống như các thuộc tính có kiểu tình cờ là một union bao gồm `| undefined`. Kiểm tra khởi tạo nghiêm ngặt sẽ không bận tâm nếu chúng không được thiết lập rõ ràng trong constructor.

Lớp `MissingInitializer` này đánh dấu `property` của nó là tùy chọn, vì vậy nó được phép không được gán trong constructor của lớp bất kể kiểm tra khởi tạo thuộc tính nghiêm ngặt:

```typescript
class MissingInitializer {
property?: string;
}

new MissingInitializer().property?.length;  // Ok

new MissingInitializer().property.length;
// Error: Object is possibly 'undefined'.
```

### **Thuộc tính chỉ đọc (Read-Only Properties)**

Cũng giống như interfaces, các lớp trong TypeScript có thể khai báo một thuộc tính là chỉ đọc bằng cách thêm từ khóa `readonly` trước tên khai báo của nó. Từ khóa `readonly` tồn tại hoàn toàn trong hệ thống kiểu và bị loại bỏ khi biên dịch sang JavaScript.

Các thuộc tính được khai báo là `readonly` chỉ có thể được gán các giá trị ban đầu tại nơi chúng được khai báo hoặc bên trong constructor. Bất kỳ vị trí nào khác—bao gồm các phương thức trên chính lớp đó—chỉ có thể đọc từ các thuộc tính chứ không thể ghi vào chúng.

Trong ví dụ này, thuộc tính `text` trên lớp `Quote` được gán một giá trị trong constructor, nhưng các lần sử dụng khác gây ra lỗi kiểu:

```typescript
class Quote {
readonly text: string;
constructor(text: string) {
this.text= ;
    }
emphasize() {
this.text+="!";
//   ~~~~
// Error: Cannot assign to 'text' because it is a read-only property.
    }
}
const quote = new Quote(
"There is a brilliant child locked inside every student."
);
Quote.text = "Ha!";
// Error: Cannot assign to 'text' because it is a read-only property.
```

#### **CẢNH BÁO (WARNING)**

Người dùng bên ngoài mã của bạn, chẳng hạn như người sử dụng bất kỳ gói npm nào bạn đã xuất bản, có thể không tuân thủ các bổ từ `readonly`—đặc biệt nếu họ đang viết JavaScript và không có kiểm tra kiểu. Nếu bạn cần sự bảo vệ chỉ đọc thực sự, hãy cân nhắc sử dụng các trường private `#` và/hoặc các thuộc tính hàm `get()`.

Các thuộc tính được khai báo là `readonly` với giá trị ban đầu là một kiểu nguyên thủy có một nét kỳ quặc nhỏ so với các thuộc tính khác: chúng được suy luận là kiểu _literal_ hẹp hơn của giá trị nếu có thể, thay vì kiểu _primitive_ rộng hơn. TypeScript cảm thấy an tâm với việc thu hẹp kiểu ban đầu mạnh mẽ hơn vì nó biết giá trị sẽ không bị thay đổi sau này; điều này tương tự như các biến `const` nhận các kiểu hẹp hơn so với các biến `let`.

Trong ví dụ này, cả hai thuộc tính của lớp ban đầu đều được khai báo dưới dạng chuỗi ký tự cụ thể (string literal), vì vậy để mở rộng một trong số chúng thành `string`, cần phải có một chú thích kiểu:

```typescript
class RandomQuote {
readonly explicit: string = "Home is the nicest word there is.";
readonly implicit = "Home is the nicest word there is.";
constructor() {
if (Math.random () >0.5) {
this.explicit = "We start learning the minute we're born." // Ok;
this.implicit = "We start learning the minute we're born.";
// Error: Type '"We start learning the minute we're born."' is
// not assignable to type '"Home is the nicest word there is."'.
        }
    }
}
const quote = new RandomQuote();

quote.explicit;  // Type: string
quote.implicit;  // Type: "Home is the nicest word there is."
```

Việc mở rộng kiểu của một thuộc tính một cách tường minh không phải là điều cần thiết thường xuyên. Tuy nhiên, đôi khi nó có thể hữu ích trong trường hợp logic điều kiện trong các constructor giống như trong `RandomQuote`.

## **Lớp với vai trò là Kiểu dữ liệu (Classes as Types)**

Các lớp tương đối độc đáo trong hệ thống kiểu ở chỗ một khai báo lớp tạo ra cả một giá trị thời gian chạy—chính lớp đó—cũng như một kiểu dữ liệu có thể được sử dụng trong các chú thích kiểu.

Tên của lớp `Teacher` này được sử dụng để chú thích một biến `teacher`, cho TypeScript biết rằng nó chỉ nên được gán các giá trị có thể gán được cho lớp `Teacher`—chẳng hạn như các thể hiện của chính lớp `Teacher`:

```javascript
class Teacher {
sayHello() {
console.log("Take chances, make mistakes, get messy!");

    }
}
let teacher: Teacher;
teacher = new Teacher();  // Ok
teacher = "Wahoo!";
// Error: Type 'string' is not assignable to type 'Teacher'.
```

Điều thú vị là TypeScript sẽ coi bất kỳ kiểu đối tượng nào tình cờ bao gồm tất cả các thành viên giống như một lớp đều có thể gán được cho lớp đó. Điều này là do định kiểu cấu trúc của TypeScript chỉ quan tâm đến hình dạng của các đối tượng, chứ không quan tâm đến cách chúng được khai báo.

Ở đây, `withSchoolBus` nhận một tham số có kiểu `SchoolBus`. Điều đó có thể được thỏa mãn bởi bất kỳ đối tượng nào tình cờ có thuộc tính `getAbilities` có kiểu `() => string[]`, chẳng hạn như một thể hiện của lớp `SchoolBus`:

```typescript
class SchoolBus {
getAbilities() {
return ["magic", "shapeshifting"];
    }
}
function withSchoolBus(bus: SchoolBus) {
console.log(bus.getAbilities());
}
withSchoolBus(new SchoolBus());  // Ok
// Ok
withSchoolBus({
getAbilities: () => ["transmogrification"],
});
withSchoolBus({
getAbilities: () =>123,
//                  ~~~
// Error: Type 'number' is not assignable to type 'string[]'.
});
```

#### **MẸO (TIP)**

Trong hầu hết mã thực tế, các lập trình viên không truyền các giá trị đối tượng vào những nơi yêu cầu kiểu lớp. Hành vi kiểm tra cấu trúc này có vẻ bất ngờ nhưng không xuất hiện thường xuyên.

## **Lớp và Giao diện (Classes and Interfaces)**

Trong Chương 7, “Giao diện”, tôi đã chỉ cho bạn cách interfaces cho phép các lập trình viên TypeScript thiết lập các kỳ vọng cho các hình dạng đối tượng trong mã. TypeScript cho phép một lớp khai báo các thể hiện của nó tuân thủ một interface bằng cách thêm từ khóa `implements` sau tên lớp, theo sau là tên của một interface. Làm như vậy sẽ thông báo cho TypeScript biết rằng các thể hiện của lớp phải có thể gán được cho từng interface đó. Bất kỳ sự không khớp nào cũng sẽ bị bộ kiểm tra kiểu chỉ ra dưới dạng lỗi kiểu.

Trong ví dụ này, lớp `Student` triển khai chính xác interface `Learner` bằng cách bao gồm thuộc tính `name` và phương thức `study` của nó, nhưng `Slacker` bị thiếu `study` và do đó dẫn đến lỗi kiểu:

```typescript
interface Learner {
name: string;
study(hours: number): void;
}
class StudentimplementsLearner {
name: string;
constructor(name: string) {
this.name = name;
    }
study(hours: number) {
for (let i = 0; i<hours; i+=1) {
console.log("...studying...");
        }
    }
}
class SlackerimplementsLearner {
// ~~~~~~~

// Error: Class 'Slacker' incorrectly implements interface 'Learner'.

//  Property 'study' is missing in type 'Slacker'

//  but required in type 'Learner'.
name = "Rocky";
}
```

#### **GHI CHÚ (NOTE)**

Các interface nhằm mục đích được triển khai bởi các lớp là một lý do điển hình để sử dụng cú pháp phương thức để khai báo một thành viên interface dưới dạng hàm—như được sử dụng bởi interface `Learner`.

Việc đánh dấu một lớp là triển khai một interface không làm thay đổi bất kỳ điều gì về cách lớp đó được sử dụng. Nếu lớp đã tình cờ khớp với interface, bộ kiểm tra kiểu của TypeScript dù sao cũng sẽ cho phép các thể hiện của nó được sử dụng ở những nơi yêu cầu một thể hiện của interface. TypeScript thậm chí sẽ không suy luận kiểu của các phương thức hoặc thuộc tính trên lớp từ interface: nếu chúng ta đã thêm một phương thức `study(hours) {}` vào ví dụ `Slacker`, TypeScript sẽ coi tham số `hours` là một `any` ngầm định trừ khi chúng ta cung cấp cho nó một chú thích kiểu.

Phiên bản này của lớp `Student` gây ra các lỗi kiểu `any` ngầm định vì nó không cung cấp các chú thích kiểu trên các thành viên của nó:

```typescript
class StudentimplementsLearner {
name;
// Error: Member 'name' implicitly has an 'any' type.
study(hours) {
// Error: Parameter 'hours' implicitly has an 'any' type.
    }
}
```

Triển khai một interface hoàn toàn là một kiểm tra an toàn. Nó không tự động sao chép bất kỳ thành viên interface nào vào định nghĩa lớp cho bạn. Thay vào đó, việc triển khai một interface báo hiệu ý định của bạn cho bộ kiểm tra kiểu và làm nổi bật các lỗi kiểu trong định nghĩa lớp, thay vì xuất hiện muộn hơn ở nơi các thể hiện của lớp được sử dụng. Nó có mục đích tương tự như việc thêm một chú thích kiểu vào một biến ngay cả khi nó đã có giá trị ban đầu.

### **Triển khai nhiều giao diện (Implementing Multiple Interfaces)**

Các lớp trong TypeScript được phép khai báo là triển khai nhiều interface. Danh sách các interface được triển khai cho một lớp có thể là bất kỳ số lượng tên interface nào có dấu phẩy ở giữa.

Trong ví dụ này, cả hai lớp đều bắt buộc phải có ít nhất thuộc tính `grades` để triển khai `Graded` và thuộc tính `report` để triển khai `Reporter`. Lớp `Empty` có hai lỗi kiểu do không triển khai đúng cách một trong hai interface:

```typescript
interface Graded {
grades: number[];
}
interface Reporter {
report: () => string;
}
class ReportCardimplementsGraded, Reporter {
grades: number[];
constructor(grades: number[]) {
this.grades = grades;
    }
report() {
return this.grades.join(", ");
    }
}
class EmptyimplementsGraded, Reporter { }
// ~~~~~
// Error: Class 'Empty' incorrectly implements interface 'Graded'.
//   Property 'grades' is missing in type 'Empty'
//   but required in type 'Graded'.
// ~~~~~
// Error: Class 'Empty' incorrectly implements interface 'Reporter'.
//   Property 'report' is missing in type 'Empty'
//   but required in type 'Reporter'.
```

Trong thực tế, có thể có một số interface mà định nghĩa của chúng khiến một lớp không thể triển khai cả hai. Việc cố gắng khai báo một lớp triển khai hai interface xung đột sẽ dẫn đến ít nhất một lỗi kiểu trên lớp đó.

Các interface `AgeIsANumber` và `AgeIsNotANumber` sau đây khai báo các kiểu rất khác nhau cho thuộc tính `age`. Cả lớp `AsNumber` và lớp `NotAsNumber` đều không triển khai đúng cả hai:

```typescript
interface AgeIsANumber {
age: number;
}
interface AgeIsNotANumber {
age: () => string;
}
class AsNumberimplementsAgeIsANumber, AgeIsNotANumber {
age = 0;
// ~~~
// Error: Property 'age' in type 'AsNumber' is not assignable
// to the same property in base type 'AgeIsNotANumber'.
//   Type 'number' is not assignable to type '() => string'.
}
class NotAsNumberimplementsAgeIsANumber, AgeIsNotANumber {
age() {return ""; }
// ~~~
// Error: Property 'age' in type 'NotAsNumber' is not assignable
// to the same property in base type 'AgeIsANumber'.
//   Type '() => string' is not assignable to type 'number'.
}
```

Các trường hợp hai interface mô tả các hình dạng đối tượng rất khác nhau nhìn chung chỉ ra rằng bạn không nên cố gắng triển khai chúng bằng cùng một lớp.

## **Mở rộng một Lớp (Extending a Class)**

TypeScript bổ sung kiểm tra kiểu vào khái niệm JavaScript về một lớp mở rộng, hoặc tạo lớp con (subclass), từ một lớp khác. Để bắt đầu, bất kỳ phương thức hoặc thuộc tính nào được khai báo trên một lớp cơ sở (base class) sẽ có sẵn trên lớp con, còn được gọi là lớp dẫn xuất (derived class).

Trong ví dụ này, `Teacher` khai báo một phương thức `teach` có thể được sử dụng bởi các thể hiện của lớp con `StudentTeacher`:

```typescript
class Teacher {
teach() {
console.log("The surest test of discipline is its absence.");
    }
}
class StudentTeacherextendsTeacher {
learn() {
console.log("I cannot afford the luxury of a closed mind.");
    }
}
const teacher = new StudentTeacher();
teacher.teach();  // Ok (defined on base)
teacher.learn();  // Ok (defined on subclass)
teacher.other();
//     ~~~~~
// Error: Property 'other' does not exist on type 'StudentTeacher'.
```

### **Khả năng gán của lớp kế thừa (Extension Assignability)**

Các lớp con kế thừa các thành viên từ lớp cơ sở của chúng giống như các interface dẫn xuất mở rộng các interface cơ sở. Các thể hiện của lớp con có tất cả các thành viên của lớp cơ sở và do đó có thể được sử dụng ở bất kỳ nơi nào yêu cầu một thể hiện của lớp cơ sở. Nếu một lớp cơ sở không có tất cả các thành viên mà một lớp con có, thì nó không thể được sử dụng khi yêu cầu lớp con cụ thể hơn.

Các thể hiện của lớp `Lesson` sau đây không được phép sử dụng ở nơi yêu cầu các thể hiện của lớp dẫn xuất `OnlineLesson` của nó, nhưng các thể hiện dẫn xuất có thể được sử dụng để thỏa mãn lớp cơ sở hoặc lớp con:

```typescript
class Lesson {
subject: string;
constructor(subject: string) {
this.subject = subject;
    }
}

class OnlineLessonextendsLesson {
url: string;
constructor(subject: string, url: string) {
super(subject);
this.url = url;
    }
}
let lesson: Lesson;
lesson = new Lesson("coding");  // Ok
lesson = new OnlineLesson("coding", "oreilly.com");  // Ok
let online: OnlineLesson;
online = new OnlineLesson("coding", "oreilly.com");  // Ok
online = new Lesson("coding");
// Error: Property 'url' is missing in type
// 'Lesson' but required in type 'OnlineLesson'.
```

Theo định kiểu cấu trúc của TypeScript, nếu tất cả các thành viên trên một lớp con đã tồn tại trên lớp cơ sở của nó với cùng một kiểu, thì các thể hiện của lớp cơ sở vẫn được phép sử dụng thay cho lớp con.

Trong ví dụ này, `LabeledPastGrades` chỉ thêm một thuộc tính tùy chọn vào `PastGrades`, vì vậy các thể hiện của lớp cơ sở có thể được sử dụng thay cho lớp con:

```typescript
class PastGrades {
grades: number[] = [];
}
class LabeledPastGradesextendsPastGrades {
label?: string;
}
let subClass: LabeledPastGrades;
subClass = new LabeledPastGrades();  // Ok
subClass = new PastGrades();  // Ok
```

#### **MẸO (TIP)**

Trong hầu hết mã thực tế, các lớp con thường thêm thông tin kiểu bắt buộc mới lên trên lớp cơ sở của chúng. Hành vi kiểm tra cấu trúc này có vẻ bất ngờ nhưng không xuất hiện thường xuyên.

### **Ghi đè hàm khởi tạo (Overridden Constructors)**

Cũng như JavaScript thuần túy, các lớp con không bị TypeScript bắt buộc phải định nghĩa constructor của riêng chúng. Các lớp con không có constructor riêng sẽ sử dụng constructor từ lớp cơ sở của chúng một cách ngầm định.

Trong JavaScript, nếu một lớp con khai báo constructor của riêng mình, thì nó phải gọi constructor của lớp cơ sở thông qua từ khóa `super`. Constructor của lớp con có thể khai báo bất kỳ tham số nào bất kể lớp cơ sở của chúng yêu cầu gì. Bộ kiểm tra kiểu của TypeScript sẽ đảm bảo rằng lời gọi constructor của lớp cơ sở sử dụng đúng các tham số.

Trong ví dụ này, constructor của `PassingAnnouncer` gọi constructor cơ sở một cách chính xác với một đối số `number`, trong khi `FailingAnnouncer` gặp lỗi kiểu vì quên thực hiện lời gọi đó:

```typescript
class GradeAnnouncer {
message: string;
constructor(grade: number) {
this.message = grade>=65?"Maybe next time...":"You pass!";
    }
}
class PassingAnnouncerextendsGradeAnnouncer {
constructor() {
super(100);
    }
}
class FailingAnnouncerextendsGradeAnnouncer {
constructor() { }
// ~~~~~~~~~~~~~~~~~
// Error: Constructors for subclasses must contain a 'super' call.
}
```

Theo các quy tắc của JavaScript, constructor của một lớp con phải gọi constructor cơ sở trước khi truy cập `this` hoặc `super`. TypeScript sẽ báo cáo một lỗi kiểu nếu nó thấy `this` hoặc `super` được truy cập trước `super()`. Lớp `ContinuedGradesTally` sau đây tham chiếu sai đến `this.grades` trong constructor của nó trước khi gọi `super()`:

```typescript
class GradesTally {
grades: number[] = [];
addGrades(...grades: number[]) {
this.grades.push(...grades);
return this.grades.length;
    }
}
class ContinuedGradesTallyextendsGradesTally {
constructor(previousGrades: number[]) {
this.grades= [...previousGrades];
// Error: 'super' must be called before accessing
// 'this' in the constructor of a subclass.
super();
console.log("Starting with length", this.grades.length);  // Ok
    }
}
```

### **Ghi đè phương thức (Overridden Methods)**

Các lớp con có thể khai báo lại các phương thức mới có cùng tên với lớp cơ sở, miễn là phương thức trên lớp con có thể gán được cho phương thức trên lớp cơ sở. Hãy nhớ rằng, vì các lớp con có thể được sử dụng ở bất kỳ nơi nào lớp ban đầu được sử dụng, nên các kiểu của các phương thức mới phải sử dụng được thay cho các phương thức ban đầu.

Trong ví dụ này, phương thức `countGrades` của `FailureCounter` được cho phép vì nó có cùng tham số đầu tiên và kiểu trả về như phương thức `countGrades` của lớp cơ sở `GradeCounter`. Phương thức `countGrades` của `AnyFailureChecker` gây ra lỗi kiểu vì có sai kiểu trả về:

```typescript
class GradeCounter {
countGrades(grades: string[], let ter: string) {
return grades.filter(grade => grade === let ter).length;
    }
}
class FailureCounterextendsGradeCounter {
countGrades(grades: string[]) {
return super.countGrades(grades, "F");
    }
}
class AnyFailureCheckerextendsGradeCounter {
countGrades(grades: string[]) {
// Property 'countGrades' in type 'AnyFailureChecker' is not
// assignable to the same property in base type 'GradeCounter'.
//   Type '(grades: string[]) => boolean' is not assignable
//   to type '(grades: string[], letter: string) => number'.
//      Type 'boolean' is not assignable to type 'number'.
return super.countGrades(grades, "F") !==0;
    }
}

const counter: GradeCounter = new AnyFailureChecker();

// Expected type: number
// Actual type: boolean
const count = counter.countGrades(["A", "C", "F"]);
```

### **Ghi đè thuộc tính (Overridden Properties)**

Các lớp con cũng có thể khai báo lại các thuộc tính của lớp cơ sở của chúng một cách tường minh với cùng một tên, miễn là kiểu mới có thể gán được cho kiểu trên lớp cơ sở. Cũng như với các phương thức bị ghi đè, các lớp con phải khớp về mặt cấu trúc với các lớp cơ sở.

Hầu hết các lớp con khai báo lại các thuộc tính đều làm như vậy hoặc để làm cho các thuộc tính đó thành một tập con cụ thể hơn của một type union hoặc để làm cho các thuộc tính thành một kiểu kế thừa từ kiểu thuộc tính của lớp cơ sở.

Trong ví dụ này, lớp cơ sở `Assignment` khai báo `grade` của nó là `number | undefined`, trong khi lớp con `GradedAssignment` khai báo nó là một `number` bắt buộc phải luôn tồn tại:

```typescript
class Assignment {
grade?: number;
}
class GradedAssignmentextendsAssignment {
grade: number;

constructor(grade: number) {
super();
this.grade = grade;
    }
}
```

Mở rộng tập hợp các giá trị được phép của kiểu union của một thuộc tính là không được phép, vì làm như vậy sẽ khiến thuộc tính của lớp con không còn có thể gán cho kiểu thuộc tính của lớp cơ sở nữa.

Trong ví dụ này, `value` của `VagueGrade` cố gắng thêm `| string` lên trên kiểu `number` của lớp cơ sở `NumericGrade`, gây ra lỗi kiểu:

```typescript
class NumericGrade {
value = 0;
}
class VagueGradeextendsNumericGrade {
value = Math.random() >0.5?1: "...";
// Error: Property 'value' in type 'NumberOrString' is not
// assignable to the same property in base type 'JustNumber'.
//   Type 'string | number' is not assignable to type 'number'.
//     Type 'string' is not assignable to type 'number'.
}
const instance: NumericGrade = new VagueGrade();

// Expected type: number
// Actual type: number | string
instance.value;
```

## **Lớp trừu tượng (Abstract Classes)**

Đôi khi việc tạo một lớp cơ sở không tự khai báo việc triển khai một số phương thức mà thay vào đó mong đợi một lớp con cung cấp chúng có thể rất hữu ích. Việc đánh dấu một lớp là trừu tượng được thực hiện bằng cách thêm từ khóa `abstract` của TypeScript vào trước tên lớp và trước bất kỳ phương thức nào dự định là trừu tượng. Các khai báo phương thức trừu tượng đó bỏ qua việc cung cấp thân hàm trong lớp cơ sở trừu tượng; thay vào đó, chúng được khai báo theo cùng một cách như một interface.

Trong ví dụ này, lớp `School` và phương thức `getStudentTypes` của nó được đánh dấu là `abstract`. Do đó, các lớp con của nó—`Preschool` và `Absence`—được mong đợi sẽ triển khai `getStudentTypes`:

```typescript
abstract class School {
readonly name: string;
constructor(name: string) {
this.name = name;
    }
abstract getStudentTypes(): string[];
}
class PreschoolextendsSchool {
getStudentTypes() {
return ["preschooler"];
    }
}
class AbsenceextendsSchool { }
// ~~~~~~~
// Error: Nonabstract class 'Absence' does not implement
// inherited abstract member 'getStudentTypes' from class 'School'.
```

Một lớp trừu tượng không thể được khởi tạo trực tiếp, vì nó không có định nghĩa cho một số phương thức mà việc triển khai của nó có thể giả định là có tồn tại. Chỉ các lớp không trừu tượng (“concrete”) mới có thể được khởi tạo.

Tiếp tục ví dụ về `School`, việc cố gắng gọi `new School` sẽ dẫn đến lỗi kiểu TypeScript:

```typescript
let school: School;

school = new Preschool("Sunnyside Daycare");  // Ok

school = new School("somewhere else");
// Error: Cannot create an instance of an abstract class.
```

Các lớp trừu tượng thường được sử dụng trong các framework nơi người dùng được mong đợi sẽ điền vào các chi tiết của một lớp. Lớp này có thể được sử dụng làm chú thích kiểu để chỉ ra các giá trị phải tuân thủ lớp đó—như ví dụ trước về `school: School`—nhưng việc tạo các thể hiện mới phải được thực hiện với các lớp con.

## **Phạm vi hiển thị của thành viên (Member Visibility)**

JavaScript bao gồm khả năng bắt đầu tên của một thành viên lớp bằng dấu `#` để đánh dấu nó là một thành viên lớp “private”. Các thành viên lớp private chỉ có thể được truy cập bởi các thể hiện của lớp đó. Môi trường thực thi JavaScript thực thi tính riêng tư đó bằng cách ném ra lỗi nếu một vùng mã bên ngoài lớp cố gắng truy cập phương thức hoặc thuộc tính private.

Sự hỗ trợ lớp của TypeScript có trước tính năng riêng tư `#` thực sự của JavaScript, và trong khi TypeScript hỗ trợ các thành viên lớp private, nó cũng cho phép một tập hợp các định nghĩa về tính riêng tư sắc thái hơn một chút trên các phương thức và thuộc tính lớp chỉ tồn tại trong hệ thống kiểu. Các mức hiển thị thành viên của TypeScript đạt được bằng cách thêm một trong các từ khóa sau trước tên khai báo của một thành viên lớp:

- _`public` (mặc định)_: Được phép truy cập bởi bất kỳ ai, ở bất kỳ đâu
- _`protected`_: Chỉ được phép truy cập bởi chính lớp đó và các lớp con của nó
- _`private`_: Chỉ được phép truy cập bởi chính lớp đó

Các từ khóa này tồn tại hoàn toàn trong hệ thống kiểu. Chúng bị loại bỏ cùng với tất cả các cú pháp hệ thống kiểu khác khi mã được biên dịch sang JavaScript.

Ở đây, `Base` khai báo hai thành viên `public`, một `protected`, một `private`, và một private thực sự với `#truePrivate`. `Subclass` được phép truy cập các thành viên `public` và `protected` nhưng không được phép truy cập `private` hoặc `#truePrivate`:

```typescript
class Base {
isPublicImplicit = 0;
public isPublicExplicit = 1;
protected isProtected = 2;
private isPrivate = 3;
#truePrivate = 4;
}
class SubclassextendsBase {
examples() {
this.isPublicImplicit;  // Ok
this.isPublicExplicit;  // Ok
this.isProtected;  // Ok
this.isPrivate;
// Error: Property 'isPrivate' is private
// and only accessible within class 'Base'.
this.#truePrivate;
// Property '#truePrivate' is not accessible outside
// class 'Base' because it has a private identifier.
    }
}
new Subclass().isPublicImplicit;  // Ok
new Subclass().isPublicExplicit;  // Ok
new Subclass().isProtected;
//             ~~~~~~~~~~~
// Error: Property 'isProtected' is protected
// and only accessible within class 'Base' and its subclasses.
new Subclass().isPrivate;
//             ~~~~~~~~~~~
// Error: Property 'isPrivate' is private
// and only accessible within class 'Base'.
```

Sự khác biệt then chốt giữa các mức hiển thị thành viên của TypeScript và các khai báo private thực sự của JavaScript là các mức hiển thị của TypeScript chỉ tồn tại trong hệ thống kiểu, trong khi của JavaScript cũng tồn tại tại runtime. Một thành viên lớp TypeScript được khai báo là `protected` hoặc `private` sẽ biên dịch thành cùng một mã JavaScript như thể chúng được khai báo là `public` một cách tường minh hoặc ngầm định. Cũng như với interfaces và type annotations, các từ khóa visibility bị xóa bỏ khi xuất ra JavaScript. Chỉ các trường private `#` mới thực sự là private trong JavaScript thời gian chạy.

Các bổ từ hiển thị có thể được đánh dấu cùng với `readonly`. Để khai báo một thành viên vừa là `readonly` vừa có mức hiển thị tường minh, mức hiển thị sẽ đứng trước.

Lớp `TwoKeywords` này khai báo thành viên `name` của nó vừa là `private` vừa là `readonly`:

```typescript
class TwoKeywords {
private readonly name: string;
constructor() {
this.name = "Anne Sullivan";  // Ok
    }
log() {
console.log(this.name);  // Ok
    }
}
const two = new TwoKeywords();
two.name = "Savitribai Phule";
// ~~~~
// Error: Property 'name' is private and
// only accessible within class 'TwoKeywords'.
// ~~~~
// Error: Cannot assign to 'name'
// because it is a read-only property.
```

Lưu ý rằng không được phép trộn lẫn từ khóa hiển thị thành viên cũ của TypeScript với các trường private `#` mới của JavaScript. Các trường private luôn là private theo mặc định, vì vậy không cần phải đánh dấu thêm chúng bằng từ khóa `private`.

### **Các bổ từ trường tĩnh (Static Field Modifiers)**

JavaScript cho phép khai báo các thành viên trên chính lớp đó—thay vì các thể hiện của nó—bằng cách sử dụng từ khóa `static`. TypeScript hỗ trợ sử dụng từ khóa `static` độc lập và/hoặc với `readonly` và/hoặc với một trong các từ khóa hiển thị. Khi kết hợp, từ khóa hiển thị đứng đầu, sau đó là `static`, sau đó là `readonly`.

Lớp `Question` này kết hợp tất cả chúng lại với nhau để làm cho các thuộc tính `static prompt` và `answer` của nó vừa là `readonly` vừa là `protected`:

```typescript
class Question {
protected static readonly answer: "bash";
protected static readonly prompt=
"What's an ogre's favorite programming language?";
guess(getAnswer: (prompt: string) => string) {
const answer = getAnswer(Question.prompt);
// Ok
if (answer === Question.answer) {
console.log("You got it!");
        } else {
console.log("Try again...")
        }
    }
}
Question.answer;
//       ~~~~~~
// Error: Property 'answer' is protected and only
// accessible within class 'HasStatic' and its subclasses.
```

Việc sử dụng các bổ từ chỉ đọc và/hoặc hiển thị cho các trường lớp tĩnh rất hữu ích để hạn chế các trường đó không bị truy cập hoặc sửa đổi bên ngoài lớp của chúng.

## **Tổng kết**

Chương này đã giới thiệu vô số tính năng và cú pháp của hệ thống kiểu xung quanh classes:

- Khai báo và sử dụng các phương thức và thuộc tính của lớp
- Đánh dấu các thuộc tính là `readonly` và/hoặc tùy chọn
- Sử dụng tên lớp làm kiểu trong các chú thích kiểu
- Triển khai interfaces để thực thi hình dạng của thể hiện lớp
- Kế thừa lớp, cùng với các quy tắc về khả năng gán và ghi đè cho các lớp con
- Đánh dấu các lớp và phương thức là abstract
- Thêm các bổ từ hệ thống kiểu vào các trường của lớp

#### **MẸO (TIP)**

Bây giờ bạn đã đọc xong chương này, hãy thực hành những gì bạn đã học tại _https://learningtypescript.com/classes_.

_Tại sao các lập trình viên hướng đối tượng luôn mặc vest? Vì họ có đẳng cấp (got class)._
