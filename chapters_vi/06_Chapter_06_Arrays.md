# **Chương 6. Mảng (Arrays)**

## Mục lục

- [**Chương 6. Mảng (Arrays)**](#chương-6-mảng-arrays)
  - [**Kiểu mảng (Array Types)**](#kiểu-mảng-array-types)
      - [**GHI CHÚ (NOTE)**](#ghi-chú-note)
    - [**Kiểu mảng và kiểu hàm**](#kiểu-mảng-và-kiểu-hàm)
    - [**Mảng kiểu kết hợp (Union-Type Arrays)**](#mảng-kiểu-kết-hợp-union-type-arrays)
    - [**Mảng có kiểu any tiến hóa (Evolving Any Arrays)**](#mảng-có-kiểu-any-tiến-hóa-evolving-any-arrays)
    - [**Mảng đa chiều (Multidimensional Arrays)**](#mảng-đa-chiều-multidimensional-arrays)
  - [**Các phần tử của mảng (Array Members)**](#các-phần-tử-của-mảng-array-members)
    - [**Lưu ý: Các phần tử không an toàn tuyệt đối (Caveat: Unsound Members)**](#lưu-ý-các-phần-tử-không-an-toàn-tuyệt-đối-caveat-unsound-members)
      - [**GHI CHÚ (NOTE)**](#ghi-chú-note-1)
  - [**Spreads và Rests**](#spreads-và-rests)
    - [**Phép trải mảng (Spreads)**](#phép-trải-mảng-spreads)
    - [**Trải các tham số Rest (Spreading Rest Parameters)**](#trải-các-tham-số-rest-spreading-rest-parameters)
  - [**Bộ dữ liệu cố định (Tuples)**](#bộ-dữ-liệu-cố-định-tuples)
    - [**Khả năng gán của Tuple (Tuple Assignability)**](#khả-năng-gán-của-tuple-tuple-assignability)
      - [**Tuples dưới dạng các tham số rest**](#tuples-dưới-dạng-các-tham-số-rest)
    - [**Suy luận kiểu Tuple (Tuple Inferences)**](#suy-luận-kiểu-tuple-tuple-inferences)
      - [**Các kiểu tuple tường minh**](#các-kiểu-tuple-tường-minh)
      - [**Tuples với khẳng định const (Const asserted tuples)**](#tuples-với-khẳng-định-const-const-asserted-tuples)
      - [**GHI CHÚ (NOTE)**](#ghi-chú-note-2)
  - [**Tổng kết**](#tổng-kết)
    - [**MẸO (TIP)**](#mẹo-tip)

_Mảng và tuple_

_Một linh hoạt và một cố định_

_Hãy chọn chuyến phiêu lưu của bạn_

Các mảng trong JavaScript cực kỳ linh hoạt và có thể chứa bất kỳ sự pha trộn giá trị nào bên trong:

```typescript
const elements = [true, null, undefined, 42];

elements.push("even", ["more"]);
// Value of elements: [true, null, undefined, 42, "even", ["more"]]
```

Tuy nhiên, trong hầu hết các trường hợp, từng mảng JavaScript riêng lẻ thường chỉ nhằm mục đích chứa một kiểu giá trị cụ thể. Việc thêm các giá trị thuộc kiểu khác có thể gây nhầm lẫn cho người đọc mã, hoặc tệ hơn, là kết quả của một lỗi có thể gây ra sự cố trong chương trình.

TypeScript tôn trọng thực hành tốt nhất là duy trì một kiểu dữ liệu cho mỗi mảng bằng cách ghi nhớ kiểu dữ liệu nào ban đầu ở bên trong mảng, và chỉ cho phép mảng thao tác trên loại dữ liệu đó.

Trong ví dụ này, TypeScript biết mảng `warriors` ban đầu chứa các giá trị có kiểu `string`, vì vậy trong khi việc thêm các giá trị kiểu `string` được cho phép, thì việc thêm bất kỳ kiểu dữ liệu nào khác là không được phép:

```typescript
const warriors = ["Artemisia", "Boudica"];

// Ok: "Zenobia" is a string
warriors.push("Zenobia");
warriors.push(true);
//            ~~~~
// Argument of type 'boolean' is not assignable to parameter of type 'string'.
```

Bạn có thể coi việc TypeScript suy luận kiểu của một mảng từ các phần tử ban đầu của nó tương tự như cách nó hiểu các kiểu biến từ các giá trị ban đầu của chúng. TypeScript nhìn chung cố gắng hiểu các kiểu dự định trong mã của bạn từ cách các giá trị được gán, và mảng cũng không ngoại lệ.

## **Kiểu mảng (Array Types)**

Cũng như các khai báo biến khác, các biến dùng để lưu trữ mảng không nhất thiết phải có giá trị ban đầu. Các biến có thể bắt đầu bằng `undefined` và nhận một giá trị mảng sau đó.

TypeScript sẽ muốn bạn cho nó biết những kiểu giá trị nào được dự định đưa vào mảng bằng cách cung cấp cho biến một chú thích kiểu. Chú thích kiểu cho một mảng yêu cầu kiểu của các phần tử trong mảng theo sau bởi `[]`:

```typescript
let arrayOfNumbers: number[];

arrayOfNumbers= [4, 8, 15, 16, 23, 42];
```

#### **GHI CHÚ (NOTE)**

Các kiểu mảng cũng có thể được viết bằng cú pháp như `Array<number>` được gọi là _class generics_ (generic của lớp). Hầu hết các lập trình viên thích cách viết đơn giản hơn là `number[]`. Các lớp được đề cập trong Chương 8, “Classes”, và generics được đề cập trong Chương 10, “Generics”.

### **Kiểu mảng và kiểu hàm**

Các kiểu mảng là một ví dụ về cấu trúc cú pháp mà ở đó các kiểu hàm có thể cần dấu ngoặc đơn để phân biệt phần nào nằm trong kiểu hàm hoặc không. Dấu ngoặc đơn có thể được sử dụng để chỉ ra phần nào của chú thích là giá trị trả về của hàm hoặc là kiểu mảng bao quanh.

Kiểu `createStrings` ở đây, vốn là một kiểu hàm, không giống với `stringCreators`, vốn là một kiểu mảng:

```typescript
// Type is a function that returns an array of strings
let createStrings: () => string[];

// Type is an array of functions that each return a string
let stringCreators: (() => string)[];
```

### **Mảng kiểu kết hợp (Union-Type Arrays)**

Bạn có thể sử dụng một union type để chỉ ra rằng mỗi phần tử của một mảng có thể là một trong nhiều kiểu được chọn.

Khi sử dụng các kiểu mảng với unions, dấu ngoặc đơn có thể cần được sử dụng để chỉ ra phần nào của chú thích là nội dung của mảng hoặc kiểu union bao quanh. Việc sử dụng dấu ngoặc đơn trong các kiểu mảng union là rất quan trọng—hai kiểu sau đây hoàn toàn không giống nhau:

```typescript
// Type is either a number or an array of strings
let stringOrArrayOfNumbers: string | number[];

// Type is an array of elements that are each either a number or a string
let arrayOfStringOrNumbers: (string | number)[];
```

TypeScript sẽ hiểu từ khai báo của một mảng rằng nó là một mảng kiểu union nếu nó chứa nhiều hơn một kiểu phần tử. Nói cách khác, kiểu của các phần tử trong mảng là sự kết hợp (union) của tất cả các kiểu có thể có cho các phần tử trong mảng.

Ở đây, `namesMaybe` có kiểu `(string | undefined)[]` vì nó có cả các giá trị `string` và một giá trị `undefined`:

```typescript
// Type is (string | undefined)[]
const namesMaybe = [
  "Aqualtune",
  "Blenda",
  undefined,
];
```

### **Mảng có kiểu any tiến hóa (Evolving Any Arrays)**

Nếu bạn không đưa một chú thích kiểu vào một biến ban đầu được đặt thành một mảng rỗng, TypeScript sẽ coi mảng đó là `any[]` tiến hóa, nghĩa là nó có thể nhận bất kỳ nội dung nào. Giống như các biến `any` tiến hóa, chúng ta không thích các mảng `any[]` tiến hóa. Chúng làm mất đi một phần lợi ích của bộ kiểm tra kiểu TypeScript bằng cách cho phép bạn thêm các giá trị có thể không chính xác.

Mảng `values` này bắt đầu bằng việc chứa các phần tử `any`, tiến hóa để chứa các phần tử `string`, sau đó lại tiến hóa để bao gồm các phần tử `number | string`:

```typescript
// Type: any[]
let values= [];
// Type: string[]
values.push('');
// Type: (number | string)[]
values[0] =0;
```

Cũng như với các biến, việc cho phép các mảng có kiểu `any` tiến hóa—và việc sử dụng kiểu `any` nói chung—làm mất đi một phần mục đích kiểm tra kiểu của TypeScript. TypeScript hoạt động tốt nhất khi nó biết các giá trị của bạn được dự định là kiểu gì.

### **Mảng đa chiều (Multidimensional Arrays)**

Một mảng 2 chiều, hoặc một mảng chứa các mảng, sẽ có hai cặp dấu “[]”:

```typescript
let arrayOfArraysOfNumbers: number[][];

arrayOfArraysOfNumbers= [

  [1, 2, 3],
  [2, 4, 6],
  [3, 6, 9],
];
```

Một mảng 3 chiều, hoặc một mảng chứa các mảng của các mảng, sẽ có ba cặp dấu “[]”. Mảng 4 chiều có bốn cặp dấu “[]”. Mảng 5 chiều có năm cặp dấu “[]”. Bạn có thể đoán được điều này tiếp tục diễn ra như thế nào đối với mảng 6 chiều và hơn thế nữa.

Các kiểu mảng đa chiều này không giới thiệu bất kỳ khái niệm mới nào cho các kiểu mảng. Hãy coi một mảng 2 chiều là việc lấy kiểu ban đầu, vốn tình cờ có `[]` ở cuối, và thêm một `[]` vào sau nó.

Mảng `arrayOfArraysOfNumbers` này có kiểu `number[][]`, cũng có thể được biểu diễn bằng `(number[])[]`:

```typescript
// Type: number[][]
let arrayOfArraysOfNumbers: (number[])[];
```

## **Các phần tử của mảng (Array Members)**

TypeScript hiểu việc truy cập dựa trên chỉ số thông thường để lấy các phần tử của một mảng sẽ trả về một phần tử thuộc kiểu của mảng đó.

Mảng `defenders` này có kiểu `string[]`, vì vậy `defender` là một `string`:

```typescript
const defenders= ["Clarenza", "Dina"];
// Type: string
const defender = defenders[0];
```

Các phần tử của mảng có kiểu union bản thân chúng cũng chính là kiểu union đó. Ở đây, `soldiersOrDates` có kiểu `(string | Date)[]`, vì vậy biến `soldierOrDate` có kiểu `string | Date`:

```typescript
const soldiersOrDates = ["Deborah Sampson", new Date(1782, 6, 3)];

// Type: Date | string
const soldierOrDate = soldiersOrDates[0];
```

### **Lưu ý: Các phần tử không an toàn tuyệt đối (Caveat: Unsound Members)**

Hệ thống kiểu của TypeScript được biết là về mặt kỹ thuật _không an toàn tuyệt đối_ (unsound): nó có thể nhận diện hầu hết các kiểu một cách chính xác, nhưng đôi khi hiểu biết của nó về kiểu của các giá trị có thể không chính xác. Mảng đặc biệt là một nguồn gây ra sự không an toàn tuyệt đối trong hệ thống kiểu. Theo mặc định, TypeScript giả định tất cả các truy cập phần tử mảng đều trả về một phần tử của mảng đó, mặc dù trong JavaScript, việc truy cập một phần tử mảng với chỉ số lớn hơn độ dài của mảng sẽ trả về `undefined`. Đoạn mã này không đưa ra bất kỳ cảnh báo nào với các thiết lập trình biên dịch TypeScript mặc định:

```typescript
function withElements(elements: string[]) {
  console.log(elements[9001].length);  // No type error
}

withElements(["It's", "over"]);
```

Chúng ta với tư cách là người đọc có thể suy ra rằng nó sẽ bị crash tại runtime với lỗi “`Cannot read property 'length' of undefined`”, nhưng TypeScript cố tình không kiểm tra xem các phần tử mảng được truy xuất có thực sự tồn tại hay không. Nó thấy `elements[9001]` trong đoạn mã có kiểu `string`, chứ không phải `undefined`.

#### **GHI CHÚ (NOTE)**

TypeScript có một cờ `--noUncheckedIndexedAccess` giúp các truy vấn mảng bị hạn chế hơn và an toàn về kiểu hơn, nhưng nó khá nghiêm ngặt và hầu hết các dự án không sử dụng nó. Tôi không đề cập đến nó trong cuốn sách này. Chương 12, “Sử dụng các tính năng IDE” liên kết đến các tài nguyên giải thích sâu về tất cả các tùy chọn cấu hình của TypeScript.

## **Spreads và Rests**

Bạn có nhớ các tham số rest `...` cho các hàm từ Chương 5, “Hàm” không? Các tham số rest và phép trải mảng (array spreading), cả hai đều sử dụng toán tử `...`, là những cách chính để tương tác với các mảng trong JavaScript. TypeScript hiểu cả hai cách này.

### **Phép trải mảng (Spreads)**

Các mảng có thể được ghép lại với nhau bằng cách sử dụng toán tử spread `...`. TypeScript hiểu mảng kết quả sẽ chứa các giá trị có thể đến từ bất kỳ mảng đầu vào nào.

Nếu các mảng đầu vào cùng kiểu, mảng đầu ra sẽ có cùng kiểu đó. Nếu hai mảng thuộc các kiểu khác nhau được trải cùng nhau để tạo ra một mảng mới, mảng mới sẽ được hiểu là một mảng kiểu union của các phần tử thuộc một trong hai kiểu ban đầu.

Ở đây, mảng `conjoined` được biết là chứa cả các giá trị có kiểu `string` và các giá trị có kiểu `number`, vì vậy kiểu của nó được suy luận là `(string | number)[]`:

```typescript
// Type: string[]
const soldiers= ["Harriet Tubman", "Joan of Arc", "Khutulun"];
// Type: number[]
const soldierAges= [90, 19, 45];
// Type: (string | number)[]
const conjoined= [...soldiers, ...soldierAges];
```

### **Trải các tham số Rest (Spreading Rest Parameters)**

TypeScript nhận diện và sẽ thực hiện kiểm tra kiểu đối với thực hành JavaScript về việc trải `...` một mảng dưới dạng một tham số rest. Các mảng được sử dụng làm đối số cho các tham số rest phải có cùng kiểu mảng với tham số rest.

Hàm `logWarriors` dưới đây chỉ nhận các giá trị `string` cho tham số rest `...names` của nó. Việc trải một mảng có kiểu `string[]` được cho phép, nhưng một mảng `number[]` thì không:

```typescript
function logWarriors(greeting: string, ...names: string[]) {
  for (const name of names) {
    console.log(`${greeting}, ${name}!`);
  }
}
const warriors= ["Cathay Williams", "Lozen", "Nzinga"];
logWarriors("Hello", ...warriors);
const birthYears= [1844, 1840, 1583];
logWarriors("Born in", ...birthYears);
//                     ~~~~~~~~~~~~~
// Error: Argument of type 'number' is not
// assignable to parameter of type 'string'.
```

## **Bộ dữ liệu cố định (Tuples)**

Mặc dù các mảng JavaScript có thể có bất kỳ kích thước nào trên lý thuyết, nhưng đôi khi việc sử dụng một mảng có kích thước cố định—còn được gọi là một _tuple_—là rất hữu ích. Các mảng tuple có một kiểu đã biết cụ thể tại mỗi chỉ số, có thể cụ thể hơn so với kiểu union của tất cả các phần tử có thể có trong mảng. Cú pháp khai báo kiểu tuple trông giống như một array literal, nhưng có các kiểu dữ liệu thay cho các giá trị phần tử.

Ở đây, mảng `yearAndWarrior` được khai báo là một kiểu tuple với một `number` ở chỉ số 0 và một `string` ở chỉ số 1:

```typescript
let yearAndWarrior: [number, string];
yearAndWarrior= [530, "Tomyris"];  // Ok
yearAndWarrior= [false, "Tomyris"];
//                ~~~~~
// Error: Type 'boolean' is not assignable to type 'number'.
yearAndWarrior= [530];
// Error: Type '[number]' is not assignable to type '[number, string]'.
//   Source has 1 element(s) but target requires 2.
```

Tuples thường được sử dụng trong JavaScript cùng với tính năng phân rã mảng (array destructuring) để có thể gán nhiều giá trị cùng một lúc, chẳng hạn như thiết lập hai biến thành các giá trị ban đầu dựa trên một điều kiện duy nhất.

Ví dụ: TypeScript nhận diện ở đây rằng `year` sẽ luôn là một `number` và `warrior` sẽ luôn là một `string`:

```typescript
// year type: number
// warrior type: string
let [year, warrior] =Math.random() >0.5
? [340, "Archidamia"]
: [1828, "Rani of Jhansi"];
```

### **Khả năng gán của Tuple (Tuple Assignability)**

Các kiểu tuple được TypeScript đối xử như cụ thể hơn so với các kiểu mảng có độ dài biến đổi. Điều đó có nghĩa là các kiểu mảng có độ dài biến đổi không thể gán được cho các kiểu tuple.

Ở đây, mặc dù con người chúng ta có thể thấy `pairLoose` có `[boolean, number]` bên trong, nhưng TypeScript lại suy luận nó thành kiểu `(boolean | number)[]` tổng quát hơn:

```typescript
// Type: (boolean | number)[]
const pairLoose= [false, 123];

const pairTupleLoose: [boolean, number] =pairLoose;
//    ~~~~~~~~~~~~~~
// Error: Type '(number | boolean)[]' is not
// assignable to type '[boolean, number]'.
//   Target requires 2 element(s) but source may have fewer.
```

Nếu bản thân `pairLoose` đã được khai báo là một `[boolean, number]`, thì việc gán giá trị của nó cho `pairTupleLoose` sẽ được phép. Các tuple có độ dài khác nhau cũng không thể gán cho nhau, vì TypeScript lưu giữ thông tin về số lượng phần tử có trong tuple trong các kiểu tuple.

Ở đây, `tupleTwoExtra` phải có chính xác hai phần tử, vì vậy mặc dù `tupleThree` bắt đầu bằng các phần tử chính xác, phần tử thứ ba của nó ngăn nó có thể gán được cho `tupleTwoExtra`:

```typescript
const tupleThree: [boolean, number, string] = [false, 1583, "Nzinga"];

const tupleTwoExact: [boolean, number] = [tupleThree[0], tupleThree[1]];
const tupleTwoExtra: [boolean, number] = tupleThree;
//    ~~~~~~~~~~~~~
// Error: Type '[boolean, number, string]' is
// not assignable to type '[boolean, number]'.
//   Source has 3 element(s) but target allows only 2.
```

#### **Tuples dưới dạng các tham số rest**

Bởi vì các tuple được xem là các mảng có thông tin kiểu cụ thể hơn về độ dài và kiểu phần tử, chúng có thể đặc biệt hữu ích cho việc lưu trữ các đối số được truyền vào một hàm. TypeScript có thể cung cấp kiểm tra kiểu chính xác cho các tuple được truyền dưới dạng các tham số rest `...`.

Ở đây, các tham số của hàm `logPair` được định kiểu là `string` và `number`. Việc cố gắng truyền vào một giá trị có kiểu `(string | number)[]` dưới dạng các đối số sẽ không an toàn về kiểu vì nội dung có thể không khớp: chúng có thể đều cùng một kiểu, hoặc mỗi thứ một kiểu nhưng sai thứ tự. Tuy nhiên, nếu TypeScript biết giá trị đó là một tuple `[string, number]`, nó hiểu rằng các giá trị khớp nhau:

```typescript
function logPair(name: string, value: number) {
  console.log(`${name} has ${value}`);
}
const pairArray = ["Amage", 1];

logPair(...pairArray);
// Error: A spread argument must either have a
// tuple type or be passed to a rest parameter.
const pairTupleIncorrect: [number, string] = [1, "Amage"];

logPair(...pairTupleIncorrect);
// Error: Argument of type 'number' is not
// assignable to parameter of type 'string'.
const pairTupleCorrect: [string, number] = ["Amage", 1];

logPair(...pairTupleCorrect);  // Ok
```

Nếu bạn thực sự muốn sáng tạo với các tuple tham số rest, bạn có thể kết hợp chúng với các mảng để lưu trữ danh sách các đối số cho nhiều lời gọi hàm. Ở đây, `trios` là một mảng các tuple, trong đó mỗi tuple cũng có một tuple cho phần tử thứ hai của nó. `trios.forEach(trio => logTrio(...trio))` được biết là an toàn vì mỗi `...trio` tình cờ khớp với các kiểu tham số của `logTrio`. Tuy nhiên, `trios.forEach(logTrio)` không thể gán được vì điều đó đang cố gắng truyền toàn bộ `[string, [number, boolean]]` làm tham số đầu tiên, vốn có kiểu `string`:

```typescript
function logTrio(name: string, value: [number, boolean]) {
  console.log(`${name} has ${value[0]} (${value[1]})`);
}

const trios: [string, [number, boolean]][] = [
  ["Amanitore", [1, true]],
  ["Æthelflæd", [2, false]],
  ["Ann E. Dunwoody", [3, false]]
];

trios.forEach(trio => logTrio(...trio));  // Ok

trios.forEach(logTrio);
//            ~~~~~~~
// Argument of type '(name: string, value: [number, boolean]) => void'
// is not assignable to parameter of type
// '(value: [string, [number, boolean]], ...) => void'.
//   Types of parameters 'name' and 'value' are incompatible.
//     Type '[string, [number, boolean]]' is not assignable to type 'string'.
```

### **Suy luận kiểu Tuple (Tuple Inferences)**

TypeScript nhìn chung coi các mảng được tạo là mảng có độ dài biến đổi, chứ không phải tuple. Nếu nó thấy một mảng được sử dụng làm giá trị ban đầu của một biến hoặc giá trị trả về cho một hàm, thì nó sẽ giả định một mảng có kích thước linh hoạt thay vì một tuple có kích thước cố định.

Hàm `firstCharAndSize` sau đây được suy luận là trả về `(string | number)[]`, chứ không phải `[string, number]`, vì đó là kiểu được suy luận cho array literal được trả về của nó:

```typescript
// Return type: (string | number)[]
function firstCharAndSize(input: string) {
  return [input[0], input.length];
}

// firstChar type: string | number
// size type: string | number
const [firstChar, size] = firstCharAndSize("Gudit");
```

Có hai cách phổ biến trong TypeScript để chỉ ra rằng một giá trị phải là một kiểu tuple cụ thể hơn thay vì một kiểu mảng tổng quát: các kiểu tuple tường minh và các khẳng định `const` (const assertions).

#### **Các kiểu tuple tường minh**

Các kiểu tuple có thể được sử dụng trong các chú thích kiểu, chẳng hạn như chú thích kiểu trả về cho một hàm. Nếu hàm được khai báo là trả về một kiểu tuple và trả về một array literal, thì array literal đó sẽ được suy luận là một tuple thay vì một mảng có độ dài biến đổi tổng quát hơn.

Phiên bản hàm `firstCharAndSizeExplicit` này nêu rõ ràng rằng nó trả về một tuple gồm một `string` và một `number`:

```typescript
// Return type: [string, number]
function firstCharAndSizeExplicit(input: string): [string, number] {
  return [input[0], input.length];
}

// firstChar type: string
// size type: number
const [firstChar, size] = firstCharAndSizeExplicit("Cathay Williams");
```

#### **Tuples với khẳng định const (Const asserted tuples)**

Việc gõ các kiểu tuple trong các chú thích kiểu tường minh có thể gây phiền toái vì những lý do tương tự như khi gõ bất kỳ chú thích kiểu tường minh nào. Đó là cú pháp bổ sung để bạn viết và cập nhật khi mã thay đổi.

Như một giải pháp thay thế, TypeScript cung cấp toán tử `as const` được gọi là một _khẳng định const_ (const assertion) có thể được đặt sau một giá trị. Các khẳng định const thông báo cho TypeScript sử dụng dạng literal, chỉ đọc (read-only) cụ thể nhất có thể của giá trị khi suy luận kiểu của nó. Nếu một khẳng định được đặt sau một array literal, nó sẽ chỉ ra rằng mảng đó phải được đối xử như một tuple:

```typescript
// Type: (string | number)[]
const unionArray = [1157, "Tomoe"];

// Type: readonly [1157, "Tomoe"]
const readonlyTuple = [1157, "Tomoe"] as const;
```

Lưu ý rằng các khẳng định `as const` còn đi xa hơn việc chuyển từ mảng có kích thước linh hoạt sang tuple có kích thước cố định: chúng cũng chỉ ra cho TypeScript biết rằng tuple là chỉ đọc (read-only) và không thể được sử dụng ở vị trí mong đợi được phép sửa đổi giá trị.

Trong ví dụ này, `pairMutable` được phép sửa đổi vì nó có kiểu tuple tường minh truyền thống. Tuy nhiên, `as const` làm cho giá trị không thể gán được cho `pairAlsoMutable` có thể thay đổi, và các phần tử của hằng số `pairConst` không được phép sửa đổi:

```typescript
const pairMutable: [number, string] = [1157, "Tomoe"];
pairMutable[0] = 1247;  // Ok
const pairAlsoMutable: [number, string] = [1157, "Tomoe"] as const;
//    ~~~~~~~~~~~~~~~
// Error: The type 'readonly [1157, "Tomoe"]' is 'readonly'
// and cannot be assigned to the mutable type '[number, string]'.
const pairConst = [1157, "Tomoe"] as const;
pairConst[0] = 1247;
//        ~
// Error: Cannot assign to '0' because it is a read-only property.
```

Trong thực tế, các read-only tuples rất tiện lợi cho các giá trị trả về của hàm. Các giá trị được trả về từ các hàm trả về một tuple thường được destructure ngay lập tức, vì vậy việc tuple ở dạng read-only không cản trở việc sử dụng hàm.

Hàm `firstCharAndSizeAsConst` này trả về một `readonly [string, number]`, nhưng mã sử dụng chỉ quan tâm đến việc lấy các giá trị từ tuple đó:

```typescript
// Return type: readonly [string, number]
function firstCharAndSizeAsConst(input: string) {
  return [input[0], input.length] as const;
}
// firstChar type: string
// size type: number
const [firstChar, size] = firstCharAndSizeAsConst("Ching Shih");
```

#### **GHI CHÚ (NOTE)**

Các đối tượng chỉ đọc và các khẳng định `as const` được đề cập sâu hơn trong Chương 9, “Các bổ từ kiểu (Type Modifiers)”.

## **Tổng kết**

Trong chương này, bạn đã làm việc với việc khai báo mảng và truy xuất các phần tử của chúng:

- Khai báo các kiểu mảng bằng `[]`
- Sử dụng dấu ngoặc đơn để khai báo mảng các hàm hoặc kiểu union
- Cách TypeScript hiểu các phần tử mảng là kiểu của mảng
- Làm việc với `...` spreads và rests
- Khai báo các kiểu tuple để biểu diễn các mảng có kích thước cố định
- Sử dụng các chú thích kiểu hoặc các khẳng định `as const` để tạo các tuple

#### **MẸO (TIP)**

Bây giờ bạn đã đọc xong chương này, hãy thực hành những gì bạn đã học tại _https://learningtypescript.com/arrays_.

_Cấu trúc dữ liệu yêu thích của một tên cướp biển là gì?_

_Arrrrr-ays! (Mảngggg!)_
