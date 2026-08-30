# **Chương 15. Các thao tác kiểu (Type Operations)**

## Mục lục

- [**Chương 15. Các thao tác kiểu (Type Operations)**](#chương-15-các-thao-tác-kiểu-type-operations)
      - [**CẢNH BÁO (WARNING)**](#cảnh-báo-warning)
  - [**Kiểu ánh xạ (Mapped Types)**](#kiểu-ánh-xạ-mapped-types)
    - [**Mapped Types từ các kiểu khác (Mapped Types from Types)**](#mapped-types-từ-các-kiểu-khác-mapped-types-from-types)
      - [**Mapped types và chữ ký (Mapped types and signatures)**](#mapped-types-và-chữ-ký-mapped-types-and-signatures)
    - [**Thay đổi các bổ từ (Changing Modifiers)**](#thay-đổi-các-bổ-từ-changing-modifiers)
      - [**GHI CHÚ (NOTE)**](#ghi-chú-note)
      - [**GHI CHÚ (NOTE)**](#ghi-chú-note-1)
    - [**Mapped Types tổng quát (Generic Mapped Types)**](#mapped-types-tổng-quát-generic-mapped-types)
  - [**Kiểu điều kiện (Conditional Types)**](#kiểu-điều-kiện-conditional-types)
    - [**Kiểu điều kiện tổng quát (Generic Conditional Types)**](#kiểu-điều-kiện-tổng-quát-generic-conditional-types)
    - [**Tính phân phối của kiểu (Type Distributivity)**](#tính-phân-phối-của-kiểu-type-distributivity)
    - [**Các kiểu được suy luận (Inferred Types)**](#các-kiểu-được-suy-luận-inferred-types)
    - [**Mapped Conditional Types**](#mapped-conditional-types)
  - [**never**](#never)
    - [**never và các phép giao và hợp (never and Intersections and Unions)**](#never-và-các-phép-giao-và-hợp-never-and-intersections-and-unions)
    - [**never và Kiểu điều kiện (never and Conditional Types)**](#never-và-kiểu-điều-kiện-never-and-conditional-types)
    - [**never và Mapped Types**](#never-và-mapped-types)
  - [**Kiểu Template Literal (Template Literal Types)**](#kiểu-template-literal-template-literal-types)
    - [**Các kiểu thao tác chuỗi nội tại (Intrinsic String Manipulation Types)**](#các-kiểu-thao-tác-chuỗi-nội-tại-intrinsic-string-manipulation-types)
    - [**Các khóa Template Literal (Template Literal Keys)**](#các-khóa-template-literal-template-literal-keys)
    - [**Ánh xạ lại các khóa của Mapped Type (Remapping Mapped Type Keys)**](#ánh-xạ-lại-các-khóa-của-mapped-type-remapping-mapped-type-keys)
  - [**Các thao tác kiểu và độ phức tạp (Type Operations and Complexity)**](#các-thao-tác-kiểu-và-độ-phức-tạp-type-operations-and-complexity)
  - [**Tổng kết**](#tổng-kết)
    - [**MẸO (TIP)**](#mẹo-tip)

_Các điều kiện, các ánh xạ_

_Với sức mạnh to lớn đối với các kiểu_

_đi kèm với sự bối rối lớn lao_

TypeScript cung cấp cho chúng ta những mức độ sức mạnh tuyệt vời để định nghĩa các kiểu trong hệ thống kiểu. Ngay cả các bổ từ logic từ Chương 10, “Kiểu tổng quát (Generics)” cũng trở nên mờ nhạt khi so sánh với các khả năng của các thao tác kiểu trong chương này. Khi bạn hoàn thành chương này, bạn sẽ có thể kết hợp, đối sánh và sửa đổi các kiểu dựa trên các kiểu khác—cung cấp cho bạn những cách mạnh mẽ để biểu diễn các kiểu trong hệ thống kiểu.

#### **CẢNH BÁO (WARNING)**

Hầu hết các kiểu hoa mỹ này là những kỹ thuật mà nhìn chung bạn không muốn sử dụng quá thường xuyên. Bạn sẽ muốn hiểu chúng cho các trường hợp mà chúng hữu ích, nhưng hãy coi chừng: chúng có thể rất khó đọc khi bị lạm dụng. Hãy tận hưởng nó nhé!

## **Kiểu ánh xạ (Mapped Types)**

TypeScript cung cấp cú pháp để tạo một kiểu mới dựa trên các thuộc tính của một kiểu khác: nói cách khác, _ánh xạ_ (mapping) từ kiểu này sang kiểu khác. Một _mapped type_ (kiểu ánh xạ) trong TypeScript là một kiểu nhận vào một kiểu khác và thực hiện một số thao tác trên mỗi thuộc tính của kiểu đó.

Mapped types tạo ra một kiểu mới bằng cách tạo một thuộc tính mới dưới mỗi khóa trong một tập hợp các khóa. Chúng sử dụng cú pháp tương tự như index signatures, nhưng thay vì sử dụng kiểu khóa tĩnh với `:` như `[i: string]`, chúng sử dụng một kiểu được tính toán từ kiểu kia với `in` như `[K in OriginalType]`:

```typescript
type NewType = {
  [K in OriginalType]: NewProperty;
};
```

Một trường hợp sử dụng phổ biến cho mapped types là tạo một đối tượng có các khóa là từng string literal trong một union type hiện có. Kiểu `AnimalCounts` này tạo ra một kiểu đối tượng mới trong đó các khóa là từng giá trị từ union type `Animals` và mỗi giá trị có kiểu `number`:

```typescript
type Animals = "alligator" | "baboon" | "cat";
type AnimalCounts= {
    [K in Animals]: number;
};
// Equivalent to:
// {
//   alligator: number;
//   baboon: number;
//   cat: number;
// }
```

Mapped types dựa trên các literals hiện có của các unions là một cách tiện lợi để tiết kiệm không gian khi khai báo các interfaces lớn. Nhưng mapped types thực sự tỏa sáng khi chúng có thể tác động lên các kiểu khác và thậm chí thêm hoặc bớt các bổ từ khỏi các thành viên.

### **Mapped Types từ các kiểu khác (Mapped Types from Types)**

Mapped types thường tác động lên các kiểu hiện có bằng cách sử dụng toán tử `keyof` để lấy các khóa của kiểu hiện có đó. Bằng cách hướng dẫn một kiểu ánh xạ qua các khóa của một kiểu hiện có, chúng ta có thể _ánh xạ_ từ kiểu hiện có đó sang một kiểu mới.

Kiểu `AnimalCounts` này cuối cùng giống hệt với kiểu `AnimalCounts` từ trước bằng cách ánh xạ từ kiểu `AnimalVariants` sang một kiểu tương đương mới:

```typescript
interface AnimalVariants {
alligator: boolean;
baboon: number;
cat: string;

}
type AnimalCounts= {
    [K in keyof AnimalVariants]: number;
};
// Equivalent to:
// {
//   alligator: number;
//   baboon: number;
//   cat: number;
// }
```

Các khóa kiểu mới được ánh xạ qua một `keyof`—được đặt tên là `K` trong các đoạn mã trước—được biết là các khóa của kiểu ban đầu. Điều đó có nghĩa là mỗi giá trị thành viên mapped type được phép tham chiếu đến giá trị thành viên tương ứng của kiểu ban đầu dưới cùng một khóa.

Nếu đối tượng ban đầu là `SomeName` và ánh xạ là `[K in keyof SomeName]`, thì mỗi thành viên trong mapped type sẽ có thể tham chiếu đến giá trị thành viên tương đương của `SomeName` dưới dạng `SomeName[K]`.

Kiểu `NullableBirdVariants` này lấy kiểu `BirdVariants` ban đầu và thêm `| null` vào mỗi thành viên:

```typescript
interface BirdVariants {
dove: string;
eagle: boolean;
}
type NullableBirdVariants= {
    [K in keyof BirdVariants]: BirdVariants[K] |null,
};
// Equivalent to:
// {
//   dove: string | null;
//   eagle: boolean | null;
// }
```

Thay vì phải sao chép từng trường từ một kiểu ban đầu sang bất kỳ số lượng kiểu nào khác một cách tốn công, mapped types cho phép bạn định nghĩa một tập hợp các thành viên một lần và tái tạo hàng loạt các phiên bản mới của chúng bao nhiêu lần tùy thích.

#### **Mapped types và chữ ký (Mapped types and signatures)**

Trong Chương 7, “Giao diện”, tôi đã giới thiệu rằng TypeScript cung cấp hai cách khai báo các thành viên interface dưới dạng hàm:

- Cú pháp _Phương thức_ (Method), như `member(): void`: khai báo rằng một thành viên của interface là một hàm được thiết kế để được gọi như một thành viên của đối tượng
- Cú pháp _Thuộc tính_ (Property), như `member: () => void`: khai báo rằng một thành viên của interface bằng với một hàm độc lập

Mapped types không phân biệt giữa cú pháp phương thức và thuộc tính trên các kiểu đối tượng. Mapped types coi các phương thức như các thuộc tính trên các kiểu ban đầu.

Kiểu `ResearcherProperties` này chứa cả hai thành viên `property` và `method` của `Researcher`:

```typescript
interface Researcher {
researchMethod(): void;
researchProperty: () => string;
}
type JustProperties<T>= {
    [K in keyof T]: T[K];
};
type ResearcherProperties = JustProperties<Researcher>;
// Equivalent to:
// {
//   researchMethod: () => void;
//   researchProperty: () => string;
// }
```

Sự phân biệt giữa methods và properties không xuất hiện thường xuyên trong hầu hết mã TypeScript thực tế. Rất hiếm khi tìm thấy một trường hợp sử dụng thực tế của một mapped type nhận vào một kiểu lớp.

### **Thay đổi các bổ từ (Changing Modifiers)**

Mapped types cũng có thể thay đổi các bổ từ kiểm soát truy cập—`readonly` và tính tùy chọn `?`—trên các thành viên của kiểu ban đầu. `readonly` hoặc `?` có thể được đặt trên các thành viên của mapped types bằng cách sử dụng cú pháp giống như các interface thông thường. Kiểu `ReadonlyEnvironmentalist` sau đây tạo ra một phiên bản của interface `Environmentalist` với tất cả các thành viên được gán `readonly`, trong khi `OptionalReadonlyEnvironmentalist` tiến thêm một bước và tạo ra một phiên bản khác thêm `?` vào tất cả các thành viên `ReadonlyEnvironmentalist`:

```typescript
interface Environmentalist {
area: string;
name: string;
}
type ReadonlyEnvironmentalist= {
readonly [K in keyof Environmentalist]: Environmentalist[K];
};
// Equivalent to:
// {
//   readonly area: string;
//   readonly name: string;
// }
type OptionalReadonlyEnvironmentalist= {
    [K in keyof ReadonlyEnvironmentalist]?: ReadonlyEnvironmentalist[K];
};
// Equivalent to:
// {
//   readonly area?: string;
//   readonly name?: string;
// }
```

#### **GHI CHÚ (NOTE)**

Kiểu `OptionalReadonlyEnvironmentalist` cũng có thể được viết bằng cách khác là `readonly [K in keyof Environmentalist]?: Environmentalist[K]`.

Việc xóa các bổ từ được thực hiện bằng cách thêm dấu `-` trước bổ từ trong một kiểu mới. Thay vì viết `readonly` hoặc `?:`, bạn có thể viết lần lượt là `-readonly` hoặc `-?:`.

Kiểu `Conservationist` này chứa các thành viên tùy chọn `?` và/hoặc `readonly` được làm cho có thể ghi trong `WritableConservationist` và sau đó cũng bắt buộc trong `RequiredWritableConservationist`:

```typescript
interface Conservationist {
name: string;
catchphrase?: string;
readonly born: number;
readonly died?: number;
}
type WritableConservationist= {
-readonly [K in keyof Conservationist]: Conservationist[K];
};
// Equivalent to:
// {
//   name: string;
//   catchphrase?: string;
//   born: number;
//   died?: number;
// }
type RequiredWritableConservationist= {
    [K in keyof WritableConservationist]-?: WritableConservationist[K];
};
// Equivalent to:
// {
//   name: string;
//   catchphrase: string;
//   born: number;
//   died: number;
// }
```

#### **GHI CHÚ (NOTE)**

Kiểu `RequiredWritableConservationist` cũng có thể được viết bằng cách khác là `-readonly [K in keyof Conservationist]-?: Conservationist[K]`.

### **Mapped Types tổng quát (Generic Mapped Types)**

Toàn bộ sức mạnh của mapped types đến từ việc kết hợp chúng với generics, cho phép một loại ánh xạ duy nhất được tái sử dụng trên các kiểu khác nhau.

Mapped types có thể truy cập `keyof` của bất kỳ tên kiểu nào trong phạm vi của chúng, bao gồm một tham số kiểu trên chính mapped type đó.

Generic mapped types thường hữu ích để biểu diễn cách dữ liệu biến đổi khi nó truyền qua một ứng dụng. Ví dụ: có thể mong muốn một khu vực của ứng dụng có thể nhận vào các giá trị của các kiểu hiện có nhưng không được phép sửa đổi dữ liệu.

Kiểu generic `MakeReadonly` này nhận vào bất kỳ kiểu nào và tạo ra một phiên bản mới với bổ từ `readonly` được thêm vào tất cả các thành viên của nó:

```typescript
type MakeReadonly<T>= {
readonly [K in keyof T]: T[K];
}
interface Species {
genus: string;
name: string;
}
type ReadonlySpecies = MakeReadonly<Species>;
// Equivalent to:
// {
//   readonly genus: string;
//   readonly name: string;
// }
```

Một biến đổi khác mà các lập trình viên thường cần biểu diễn là một hàm nhận vào một phần bất kỳ của một interface và trả về một thể hiện được điền đầy đủ của interface đó.

Kiểu `MakeOptional` và hàm `createGenusData` sau đây cho phép cung cấp bất kỳ phần nào của interface `GenusData` và nhận lại một đối tượng có các giá trị mặc định được điền vào:

```typescript
interface GenusData {
family: string;
name: string;
}
type MakeOptional<T>= {
    [K in keyof T]?: T[K];

}
// Equivalent to:
// {
//   family?: string;
//   name?: string;
// }
/**
 * Spreads any {overrides} on top of default values for GenusData.
 */
function createGenusData(overrides?: MakeOptional<GenusData>): GenusData {
return {
family: 'unknown',
name: 'unknown',
        ...overrides,
    }
}
```

Một số thao tác được thực hiện bởi generic mapped types hữu ích đến mức TypeScript cung cấp các utility types cho chúng ngay khi xuất xưởng. Ví dụ: làm cho tất cả các thuộc tính trở thành tùy chọn có thể đạt được bằng cách sử dụng kiểu `Partial<T>` tích hợp sẵn. Bạn có thể tìm thấy danh sách các kiểu tích hợp đó trên _https://www.typescriptlang.org/docs/handbook/utility-types.html_.

## **Kiểu điều kiện (Conditional Types)**

Ánh xạ các kiểu hiện có sang các kiểu khác rất tiện lợi, nhưng chúng ta vẫn chưa thêm các điều kiện logic vào hệ thống kiểu. Hãy làm điều đó ngay bây giờ.

Hệ thống kiểu của TypeScript là một ví dụ về một _ngôn ngữ lập trình logic_ (logic programming language). Nó cho phép tạo ra các cấu trúc mới (types) dựa trên việc kiểm tra logic các kiểu trước đó. Nó làm như vậy với khái niệm về một _kiểu điều kiện_ (conditional type): một kiểu giải quyết thành một trong hai kiểu có thể, dựa trên một kiểu hiện có. Cú pháp kiểu điều kiện trông giống như các toán tử ba ngôi (ternaries):

```typescript
LeftType extends RightType ? IfTrue : IfFalse
```

Kiểm tra logic trong một kiểu điều kiện luôn là xem liệu kiểu bên trái có _extends_ (mở rộng), hay có thể gán được cho kiểu bên phải hay không.

Kiểu điều kiện `CheckStringAgainstNumber` sau đây kiểm tra xem `string extends number`—hay nói cách khác, liệu kiểu `string` có thể gán được cho kiểu `number` hay không. Không thể, vì vậy kiểu kết quả là trường hợp “if false”: `false`:

```typescript
// Type: false
type CheckStringAgainstNumber = string extends number ? true : false;
```

Phần lớn phần còn lại của chương này sẽ liên quan đến việc kết hợp các tính năng khác của hệ thống kiểu với các kiểu điều kiện. Khi các đoạn mã trở nên phức tạp hơn, hãy nhớ rằng: mỗi kiểu điều kiện thuần túy là một phần của logic boolean. Mỗi kiểu nhận vào một kiểu nào đó và mang lại một trong hai kết quả có thể.

### **Kiểu điều kiện tổng quát (Generic Conditional Types)**

Các kiểu điều kiện có thể kiểm tra bất kỳ tên kiểu nào trong phạm vi của chúng, bao gồm cả tham số kiểu trên chính kiểu điều kiện đó. Điều đó có nghĩa là bạn có thể viết các kiểu generic có thể tái sử dụng để tạo ra các kiểu mới dựa trên bất kỳ kiểu nào khác.

Biến kiểu `CheckStringAgainstNumber` trước đó thành một generic `CheckAgainstNumber` sẽ mang lại một kiểu là `true` hoặc `false` dựa trên việc kiểu trước đó có thể gán được cho `number` hay không. `string` vẫn không phải là true, trong khi `number` và `0 | 1` đều là true:

```typescript
type CheckAgainstNumber<T> = T extends number ? true : false;
// Type: false
type CheckString = CheckAgainstNumber<'parakeet'>;

// Type: true
type CheckString1 = CheckAgainstNumber<1891>;
// Type: true
type CheckString2 = CheckAgainstNumber<number>;
```

Kiểu `CallableSetting` sau đây hữu ích hơn một chút. Nó nhận vào một generic `T` và kiểm tra xem `T` có phải là một hàm hay không. Nếu `T` là hàm, thì kiểu kết quả là `T`—như với `GetNumbersSetting` trong đó `T` là `() => number[]`. Ngược lại, kiểu kết quả là một hàm trả về `T`, như với `StringSetting` trong đó `T` là `string`, và vì vậy kiểu kết quả là `() => string`:

```typescript
type CallableSetting<T>=
T extends () => any
  ? T
  : () => T
// Type: () => number[]
type GetNumbersSetting = CallableSetting<() => number[]>;
// Type: () => string
type StringSetting = CallableSetting<string>;
```

Các kiểu điều kiện cũng có thể truy cập các thành viên của các kiểu được cung cấp với cú pháp tra cứu thành viên đối tượng. Chúng có thể sử dụng thông tin đó cả trong mệnh đề `extends` và/hoặc trong các kiểu kết quả.

Một mô hình được các thư viện JavaScript sử dụng rất phù hợp với các generic conditional types là thay đổi kiểu trả về của một hàm dựa trên một đối tượng tùy chọn được cung cấp cho hàm.

Ví dụ: nhiều hàm cơ sở dữ liệu hoặc tương đương có thể sử dụng một thuộc tính như `throwIfNotFound` để thay đổi hàm nhằm ném ra lỗi thay vì trả về `undefined` nếu không tìm thấy giá trị. Kiểu `QueryResult` sau đây mô hình hóa hành vi đó bằng cách mang lại kiểu hẹp hơn `string` thay vì `string | undefined` nếu `throwIfNotFound` của tùy chọn được biết cụ thể là `true`:

```typescript
interface QueryOptions {
throwIfNotFound: boolean;
}
type QueryResult<Options extends QueryOptions>=
Options["throwIfNotFound"] extends true?string: string | undefined;

declare function retrieve<Options extends QueryOptions>(
key: string,
options?: Options,
): Promise<QueryResult<Options>>;
// Returned type: string | undefined

await retrieve("Biruté Galdikas");

// Returned type: string | undefined
await retrieve("Jane Goodall", { throwIfNotFound: Math.random() >0.5 });

// Returned type: string
await retrieve("Dian Fossey", { throwIfNotFound: true });
```

Bằng cách kết hợp một kiểu điều kiện với một tham số kiểu generic, hàm `retrieve` đó chính xác hơn trong việc thông báo cho hệ thống kiểu biết cách nó sẽ thay đổi luồng điều khiển của chương trình.

### **Tính phân phối của kiểu (Type Distributivity)**

Các kiểu điều kiện _phân phối_ (distribute) qua các unions, nghĩa là kiểu kết quả của chúng sẽ là một union của việc áp dụng kiểu điều kiện đó cho từng thành phần (các kiểu trong union type). Nói cách khác, `ConditionalType<T | U>` giống như `Conditional<T> | Conditional<U>`.

Tính phân phối kiểu là một khái niệm hơi trừu tượng để giải thích nhưng lại rất quan trọng đối với cách các kiểu điều kiện hoạt động với các unions.

Hãy xem xét kiểu `ArrayifyUnlessString` sau đây chuyển đổi tham số kiểu `T` của nó thành một mảng trừ khi `T extends string`. `HalfArrayified` tương đương với `string | number[]` vì `ArrayifyUnlessString<string | number>` giống với `ArrayifyUnlessString<string> | ArrayifyUnlessString<number>`:

```typescript
type ArrayifyUnlessString<T> = T extends string ? T : T[];

// Type: string | number[]
type HalfArrayified = ArrayifyUnlessString<string | number>;
```

Nếu các kiểu điều kiện của TypeScript không phân phối qua các unions, `HalfArrayified` sẽ là `(string | number)[]` vì `string | number` không thể gán được cho `string`. Nói cách khác, các kiểu điều kiện áp dụng logic của chúng cho từng thành phần của một union type, chứ không phải toàn bộ union type.

### **Các kiểu được suy luận (Inferred Types)**

Truy cập các thành viên của các kiểu được cung cấp hoạt động tốt cho thông tin được lưu trữ như một thành viên của một kiểu, nhưng nó không thể nắm bắt các thông tin khác như các tham số hàm hoặc các kiểu trả về. Các kiểu điều kiện có thể truy cập các phần tùy ý của điều kiện của chúng bằng cách sử dụng từ khóa `infer` bên trong mệnh đề extends của chúng. Đặt từ khóa `infer` và một tên mới cho một kiểu bên trong một mệnh đề extends có nghĩa là kiểu mới đó sẽ có sẵn bên trong trường hợp true của kiểu điều kiện.

Kiểu `ArrayItems` này nhận vào một tham số kiểu `T` và kiểm tra xem `T` có phải là một mảng của một kiểu `Item` mới nào đó hay không. Nếu có, kiểu kết quả là `Item`; nếu không, nó là `T`:

```typescript
type ArrayItems<T>=
T extends (infer Item)[]
  ? Item
  : T;
// Type: string
type StringItem = ArrayItems<string>;
// Type: string
type StringArrayItem = ArrayItems<string[]>;
// Type: string[]
type String2DItem = ArrayItems<string[][]>;
```

Các kiểu được suy luận cũng có thể hoạt động để tạo ra các kiểu điều kiện đệ quy. Kiểu `ArrayItems` đã thấy trước đó có thể được mở rộng để lấy kiểu phần tử của một mảng có số chiều bất kỳ theo cách đệ quy:

```typescript
type ArrayItemsRecursive<T>=
T extends (infer Item)[]
  ? ArrayItemsRecursive<Item>
  : T;
// Type: string
type StringItem = ArrayItemsRecursive<string>;
// Type: string
type StringArrayItem = ArrayItemsRecursive<string[]>;

// Type: string
type String2DItem = ArrayItemsRecursive<string[][]>;
```

Lưu ý rằng trong khi `ArrayItems<string[][]>` mang lại `string[]`, thì `ArrayItemsRecursive<string[][]>` lại mang lại `string`. Khả năng cho phép các kiểu generic đệ quy cho phép chúng tiếp tục áp dụng các sửa đổi—chẳng hạn như lấy kiểu phần tử của một mảng ở đây.

### **Mapped Conditional Types**

Mapped types áp dụng một thay đổi cho mọi thành viên của một kiểu hiện có. Các kiểu điều kiện áp dụng một thay đổi cho một kiểu hiện có đơn lẻ. Kết hợp lại với nhau, chúng cho phép áp dụng logic điều kiện cho từng thành viên của một kiểu mẫu generic.

Kiểu `MakeAllMembersFunctions` này biến mỗi thành viên không phải hàm của một kiểu thành một hàm:

```typescript
type MakeAllMembersFunctions<T>= {
    [K in keyof T]: T[K] extends (...args: any[]) => any
?T[K]
: () => T[K]
};
type MemberFunctions = MakeAllMembersFunctions<{
alreadyFunction: () => string,
notYetFunction: number,
}>;
// Type:
// {
//   alreadyFunction: () => string,
//   notYetFunction: () => number,
// }
```

Mapped conditional types là một cách tiện lợi để sửa đổi tất cả các thuộc tính của một kiểu hiện có bằng cách sử dụng một số kiểm tra logic.

## **never**

Trong Chương 4, “Đối tượng”, tôi đã giới thiệu kiểu `never`, một bottom type, có nghĩa là nó không thể có giá trị nào có thể và không thể chạm tới được. Thêm một chú thích kiểu `never` vào đúng vị trí có thể yêu cầu TypeScript quyết liệt hơn trong việc phát hiện các đường dẫn mã không bao giờ được chạm tới trong hệ thống kiểu cũng như trong các ví dụ trước về mã thời gian chạy.

### **never và các phép giao và hợp (never and Intersections and Unions)**

Một cách khác để mô tả bottom type `never` là đó là một kiểu không thể tồn tại. Điều đó mang lại cho `never` một số hành vi thú vị với các kiểu giao `&` (intersection) và hợp `|` (union):

- `never` trong một kiểu giao `&` sẽ thu gọn kiểu giao lại chỉ còn `never`.
- `never` trong một kiểu hợp `|` sẽ bị bỏ qua.

Các kiểu `NeverIntersection` và `NeverUnion` này minh họa những hành vi đó:

```typescript
type NeverIntersection = never & string;  // Type: never
type NeverUnion = never | string;  // Type: string
```

Đặc biệt, hành vi bị bỏ qua trong các union types làm cho `never` trở nên hữu ích để lọc bỏ các giá trị khỏi các kiểu điều kiện và mapped types.

### **never và Kiểu điều kiện (never and Conditional Types)**

Các kiểu điều kiện generic thường sử dụng `never` để lọc bỏ các kiểu khỏi unions. Bởi vì `never` bị bỏ qua trong unions, kết quả của một điều kiện generic trên một union các kiểu sẽ chỉ là những kiểu không phải là `never`.

Kiểu điều kiện generic `OnlyStrings` này lọc ra các kiểu không phải là chuỗi, vì vậy kiểu `RedOrBlue` lọc bỏ `0` và `false` khỏi union:

```typescript
type OnlyStrings<T> = T extends string ? T : never;

type RedOrBlue = OnlyStrings<"red" | "blue" | 0 | false>;
// Equivalent to: "red" | "blue"
```

`never` cũng thường được kết hợp với các inferred conditional types khi tạo các tiện ích kiểu cho các kiểu generic. Các suy luận kiểu với `infer` phải nằm trong trường hợp true của một kiểu điều kiện, vì vậy nếu trường hợp false không bao giờ được dự định sử dụng, `never` là một kiểu phù hợp để đặt ở đó.

Kiểu `FirstParameter` này nhận vào một kiểu hàm `T`, kiểm tra xem nó có phải là một hàm với `arg: infer Arg` hay không, và trả về `Arg` đó nếu đúng:

```typescript
type FirstParameter<T extends (...args: any[]) => any> =
  T extends (arg: infer Arg) => any
    ? Arg
    : never;

type GetsString = FirstParameter<
    (arg0: string) => void
>;  // Type: string
```

Việc sử dụng `never` trong trường hợp false của kiểu điều kiện cho phép `FirstParameter` trích xuất kiểu tham số đầu tiên của hàm.

### **never và Mapped Types**

Hành vi của `never` trong unions cũng làm cho nó hữu ích để lọc ra các thành viên trong mapped types. Có thể lọc ra các khóa của một đối tượng bằng cách sử dụng ba tính năng hệ thống kiểu sau:

- `never` bị bỏ qua trong unions.
- Mapped types có thể ánh xạ các thành viên của các kiểu.
- Các kiểu điều kiện có thể được sử dụng để chuyển các kiểu thành `never` nếu đáp ứng một điều kiện.

Kết hợp cả ba điều đó lại với nhau, chúng ta có thể tạo ra một mapped type thay đổi mỗi thành viên của kiểu ban đầu thành khóa ban đầu hoặc thành `never`. Sau đó, việc yêu cầu các thành viên của kiểu đó với `[keyof T]` sẽ tạo ra một union của tất cả các kết quả mapped type đó, lọc bỏ `never`.

Kiểu `OnlyStringProperties` sau đây biến mỗi thành viên `T[K]` thành khóa `K` nếu thành viên đó là chuỗi, hoặc thành `never` nếu không phải:

```typescript
type OnlyStringProperties<T>= {
  [K in keyof T]: T[K] extends string?K : never;
}[keyof T];
interface AllEventData {
participants: string[];
location: string;
name: string;
year: number;
}
type OnlyStringEventData = OnlyStringProperties<AllEventData>;
// Equivalent to: "location" | "name"
```

Một cách đọc khác cho kiểu `OnlyStringProperties<T>` là nó lọc bỏ tất cả các thuộc tính không phải `string` (chuyển chúng thành `never`), sau đó trả về tất cả các khóa còn lại (`[keyof T]`).

## **Kiểu Template Literal (Template Literal Types)**

Bây giờ chúng ta đã đề cập rất nhiều về các kiểu điều kiện và/hoặc kiểu ánh xạ. Hãy chuyển sang các kiểu ít chuyên sâu về logic hơn và tập trung vào chuỗi một lúc. Cho đến nay tôi đã đưa ra hai chiến lược để định kiểu các giá trị chuỗi:

- Kiểu nguyên thủy `string`: dành cho khi giá trị có thể là bất kỳ chuỗi nào trên đời
- Các kiểu literal như `""` và `"abc"`: dành cho khi giá trị chỉ có thể là kiểu duy nhất đó (hoặc một union của chúng)

Tuy nhiên, đôi khi bạn có thể muốn chỉ ra rằng một chuỗi khớp với một mẫu chuỗi nào đó: một phần của chuỗi đã được biết, nhưng một phần thì chưa. Hãy đến với _các kiểu template literal_ (template literal types), một cú pháp TypeScript để chỉ ra rằng một kiểu chuỗi tuân thủ một mẫu. Chúng trông giống như các chuỗi template literal—do đó có tên gọi này—nhưng có các kiểu nguyên thủy hoặc các unions của các kiểu nguyên thủy được nội suy vào.

Kiểu template literal này chỉ ra rằng chuỗi phải bắt đầu bằng `"Hello"` nhưng có thể kết thúc bằng bất kỳ chuỗi nào (`string`). Các tên bắt đầu bằng `"Hello"` chẳng hạn như `"Hello, world!"` đều khớp, nhưng `"World! Hello!"` hoặc `"hi"` thì không:

```typescript
type Greeting = `Hello${string}`;

let matches: Greeting = "Hello, world!";  // Ok
let outOfOrder: Greeting = "World! Hello!";
//  ~~~~~~~~~~
// Error: Type '"World! Hello!"' is not assignable to type '`Hello ${string}`'.
let missingAltogether: Greeting = "hi";
//  ~~~~~~~~~~~~~~~~~
// Error: Type '"hi"' is not assignable to type '`Hello ${string}`'.
```

Các kiểu string literal—và các unions của chúng—có thể được sử dụng trong việc nội suy kiểu thay vì kiểu nguyên thủy `string` bao quát để giới hạn các kiểu template literal vào các mẫu chuỗi hẹp hơn. Các kiểu template literal có thể khá hữu ích để mô tả các chuỗi phải khớp với một tập hợp hạn chế các chuỗi được phép.

Ở đây, `BrightnessAndColor` chỉ khớp với các chuỗi bắt đầu bằng một `Brightness`, kết thúc bằng một `Color`, và có dấu gạch nối `-` ở giữa:

```typescript
type Brightness = "dark" | "light";
type Color = "blue" | "red";
type BrightnessAndColor = `${Brightness}-${Color}`;
// Equivalent to: "dark-red" | "light-red" | "dark-blue" | "light-blue"

let colorOk: BrightnessAndColor = "dark-blue";  // Ok

let colorWrongStart: BrightnessAndColor = "medium-blue";
//  ~~~~~~~~~~~~~~~
// Error: Type '"medium-blue"' is not assignable to type
// '"dark-blue" | "dark-red" | "light-blue" | "light-red"'.
let colorWrongEnd: BrightnessAndColor = "light-green";
//  ~~~~~~~~~~~~~
// Error: Type '"light-green"' is not assignable to type
// '"dark-blue" | "dark-red" | "light-blue" | "light-red"'.
```

Nếu không có kiểu template literal, chúng ta sẽ phải viết ra tất cả bốn sự kết hợp của `Brightness` và `Color` một cách tốn công. Điều đó sẽ trở nên cồng kềnh nếu chúng ta thêm nhiều string literals hơn vào một trong hai kiểu đó! TypeScript cho phép các kiểu template literal chứa bất kỳ kiểu nguyên thủy nào (ngoại trừ `symbol`) hoặc một union của chúng: `string`, `number`, `bigint`, `boolean`, `null`, hoặc `undefined`.

Kiểu `ExtolNumber` này cho phép bất kỳ chuỗi nào bắt đầu bằng `"much "`, bao gồm một chuỗi trông giống như một số, và kết thúc bằng `" wow"`:

```typescript
type ExtolNumber=`much ${number} wow`;
function extol(extolee: ExtolNumber) { /* ... */ }
extol('much 0 wow');  // Ok
extol('much -7 wow');  // Ok
extol('much 9.001 wow');  // Ok
extol('much false wow');
//    ~~~~~~~~~~~~~~~~
// Error: Argument of type '"much false wow"' is not
// assignable to parameter of type '`much ${number} wow`'.
```

### **Các kiểu thao tác chuỗi nội tại (Intrinsic String Manipulation Types)**

Để hỗ trợ làm việc với các kiểu chuỗi, TypeScript cung cấp một tập hợp nhỏ các kiểu tiện ích generic nội tại (nghĩa là: chúng được tích hợp sẵn trong TypeScript) nhận vào một chuỗi và áp dụng một số thao tác cho chuỗi đó. Tính đến TypeScript 4.7.2, có bốn kiểu:

- `Uppercase`: Chuyển đổi một kiểu string literal thành chữ hoa.
- `Lowercase`: Chuyển đổi một kiểu string literal thành chữ thường.
- `Capitalize`: Chuyển đổi ký tự đầu tiên của kiểu string literal thành chữ hoa.
- `Uncapitalize`: Chuyển đổi ký tự đầu tiên của kiểu string literal thành chữ thường.

Mỗi kiểu này có thể được sử dụng như một kiểu generic nhận vào một chuỗi. Ví dụ: sử dụng `Capitalize` để viết hoa chữ cái đầu tiên trong một chuỗi:

```typescript
type FormalGreeting = Capitalize<"hello.">;  // Type: "Hello."
```

Các kiểu thao tác chuỗi nội tại này có thể khá hữu ích để thao tác các khóa thuộc tính trên các kiểu đối tượng.

### **Các khóa Template Literal (Template Literal Keys)**

Các kiểu template literal là điểm trung gian giữa `string` nguyên thủy và string literals, điều đó có nghĩa là chúng vẫn là chuỗi. Chúng có thể được sử dụng ở bất kỳ nơi nào khác mà bạn có thể sử dụng string literals.

Ví dụ: bạn có thể sử dụng chúng làm index signature trong một mapped type. Kiểu `ExistenceChecks` này có một khóa cho mỗi chuỗi trong `DataKey`, được ánh xạ với `check${Capitalize<DataKey>}`:

```typescript
type DataKey = "location" | "name" | "year";
type ExistenceChecks= {
    [K in `check${Capitalize<DataKey>}`]: () => boolean;
};
// Equivalent to:
// {
//   checkLocation: () => boolean;
//   checkName: () => boolean;
//   checkYear: () => boolean;
// }
function checkExistence(checks: ExistenceChecks) {
checks.checkLocation();  // Type: boolean
checks.checkName();  // Type: boolean
checks.checkWrong();
//     ~~~~~~~~~~
// Error: Property 'checkWrong' does not exist on type 'ExistenceChecks'.
}
```

### **Ánh xạ lại các khóa của Mapped Type (Remapping Mapped Type Keys)**

TypeScript cho phép bạn tạo các khóa mới cho các thành viên của mapped types dựa trên các thành viên ban đầu bằng cách sử dụng các kiểu template literal. Đặt từ khóa `as` theo sau bởi một kiểu template literal cho index signature trong một mapped type sẽ thay đổi các khóa của kiểu kết quả để khớp với kiểu template literal. Làm như vậy cho phép mapped type có một khóa khác nhau cho mỗi thuộc tính được ánh xạ trong khi vẫn tham chiếu đến giá trị ban đầu.

Ở đây, `DataEntryGetters` là một mapped type có các khóa là `getLocation`, `getName`, và `getYear`. Mỗi khóa được ánh xạ tới một khóa mới bằng kiểu template literal. Mỗi giá trị được ánh xạ là một hàm có kiểu trả về là một `DataEntry` sử dụng khóa `K` ban đầu làm đối số kiểu:

```typescript
interface DataEntry<T> {
key: T;
value: string;
}
type DataKey = "location" | "name" | "year";
type DataEntryGetters= {
    [K in DataKey as `get${Capitalize<K>}`]: () => DataEntry<K>;
};
// Equivalent to:
// {
//   getLocation: () => DataEntry<"location">;
//   getName: () => DataEntry<"name">;
//   getYear: () => DataEntry<"year">;
// }
```

Ánh xạ lại khóa có thể được kết hợp với các thao tác kiểu khác để tạo ra các mapped types dựa trên các hình dạng kiểu hiện có. Một sự kết hợp thú vị là sử dụng `keyof typeof` trên một đối tượng hiện có để tạo một mapped type từ kiểu của đối tượng đó.

Kiểu `ConfigGetter` này dựa trên kiểu `config`, nhưng mỗi trường là một hàm trả về config ban đầu, và các khóa được sửa đổi từ khóa ban đầu:

```typescript
const config = {
  location: "unknown",
  name: "anonymous",
  year: 0,
};
type LazyValues = {
  [K in keyof typeof config as `${K}Lazy`]: () => Promise<typeof config[K]>;
};
// Equivalent to:
// {
//   location: Promise<string>;
//   name: Promise<string>;
//   year: Promise<number>;
// }
async function  withLazyValues(configGetter: LazyValues) {
  await configGetter.locationLazy;  // Resultant type: string
  await configGetter.missingLazy();
  //                 ~~~~~~~~~~~
  // Error: Property 'missingLazy' does not exist on type 'LazyValues'.
};
```

Lưu ý rằng trong JavaScript, các khóa đối tượng có thể thuộc kiểu `string` hoặc `Symbol`—và các khóa `Symbol` không thể sử dụng làm kiểu template literal vì chúng không phải là kiểu nguyên thủy. Nếu bạn cố gắng sử dụng một khóa kiểu template literal được ánh xạ lại trong một kiểu generic, TypeScript sẽ đưa ra cảnh báo rằng `symbol` không thể được sử dụng trong một kiểu template literal:

```typescript
type TurnIntoGettersDirect<T>= {
    [K in keyof T as `get${K}`]: () => T[K]
//                     ~
// Error: Type 'keyof T' is not assignable to type
// 'string | number | bigint | boolean | null | undefined'.
//   Type 'string | number | symbol' is not assignable to type
//   'string | number | bigint | boolean | null | undefined'.
//     Type 'symbol' is not assignable to type
//     'string | number | bigint | boolean | null | undefined'.
};
```

Để vượt qua hạn chế đó, bạn có thể sử dụng kiểu giao `string &` để thực thi rằng chỉ những kiểu có thể là chuỗi mới được sử dụng. Vì `string & symbol` mang lại `never`, toàn bộ template string sẽ thu gọn thành `never` và TypeScript sẽ bỏ qua nó:

```typescript
const someSymbol = Symbol("");
interface HasStringAndSymbol {
StringKey: string;
    [someSymbol]: number;
}
type TurnIntoGetters<T>= {
    [K in keyof T as `get${string & K}`]: () => T[K]
};
type GettersJustString = TurnIntoGetters<HasStringAndSymbol>;
// Equivalent to:
// {
//     getStringKey: () => string;
// }
```

Hành vi của TypeScript trong việc lọc bỏ các kiểu `never` khỏi unions một lần nữa chứng minh tính hữu ích của nó!

## **Các thao tác kiểu và độ phức tạp (Type Operations and Complexity)**

_Gỡ lỗi khó gấp đôi so với việc viết mã ngay từ đầu. Do đó, nếu bạn viết mã khéo léo nhất có thể, theo định nghĩa, bạn không đủ thông minh để gỡ lỗi nó._

—Brian Kernighan

Các thao tác kiểu được mô tả trong chương này nằm trong số những tính năng hệ thống kiểu mạnh mẽ và tiên tiến nhất trong bất kỳ ngôn ngữ lập trình nào hiện nay. Hầu hết các lập trình viên vẫn chưa đủ quen thuộc với chúng để có thể gỡ lỗi các trường hợp sử dụng phức tạp đáng kể của chúng. Các công cụ phát triển tiêu chuẩn ngành như các tính năng IDE mà tôi đề cập trong Chương 12, “Sử dụng các tính năng IDE” thường không được tạo ra để trực quan hóa các thao tác kiểu nhiều lớp được sử dụng cùng nhau.

Nếu bạn thấy cần phải sử dụng các thao tác kiểu, xin vui lòng—vì lợi ích của bất kỳ lập trình viên nào phải đọc mã của bạn, bao gồm cả chính bạn trong tương lai—cố gắng giữ chúng ở mức tối thiểu nếu có thể. Sử dụng các tên dễ đọc giúp người đọc hiểu mã khi họ đọc. Để lại các chú thích mô tả cho bất cứ điều gì bạn nghĩ rằng người đọc trong tương lai có thể gặp khó khăn.

## **Tổng kết**

Trong chương này, bạn đã mở khóa sức mạnh thực sự của TypeScript bằng cách thao tác trên các kiểu trong hệ thống kiểu của nó:

- Sử dụng mapped types để biến đổi các kiểu hiện có thành các kiểu mới
- Đưa logic vào các thao tác kiểu với các kiểu điều kiện (conditional types)
- Tìm hiểu cách `never` tương tác với intersections, unions, conditional types và mapped types
- Biểu diễn các mẫu của kiểu chuỗi bằng cách sử dụng các kiểu template literal
- Kết hợp các kiểu template literal và mapped types để sửa đổi các khóa kiểu

#### **MẸO (TIP)**

Bây giờ bạn đã đọc xong chương này, hãy thực hành những gì bạn đã học tại _https://learningtypescript.com/type-operations_.

_Khi bạn bị lạc trong hệ thống kiểu, bạn sử dụng cái gì?_

_Một kiểu ánh xạ (A mapped type / Bản đồ)!_
