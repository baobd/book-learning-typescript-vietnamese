# **Bảng thuật ngữ (Glossary)**

## Mục lục

- [_ambient context_](#_ambient-context_)
- [_`any`_](#_any_)
- [_argument_](#_argument_)
- [_assertion, type assertion_](#_assertion-type-assertion_)
- [_assignable, assignability_](#_assignable-assignability_)
- [_billion-dollar mistake_](#_billion-dollar-mistake_)
- [_bottom type_](#_bottom-type_)
- [_call signature_](#_call-signature_)
- [_camel case_](#_camel-case_)
- [_class_](#_class_)
- [_compile_](#_compile_)
- [_conditional type_](#_conditional-type_)
- [_const assertion_](#_const-assertion_)
- [_constituent, constituent type_](#_constituent-constituent-type_)
- [_declaration file_](#_declaration-file_)
- [_decorator_](#_decorator_)
- [_DefinitelyTyped_](#_definitelytyped_)
- [_derived interface_](#_derived-interface_)
- [_discriminant_](#_discriminant_)
- [_discriminated union, discriminated type union_](#_discriminated-union-discriminated-type-union_)
- [_distributivity_](#_distributivity_)
- [_duck typed_](#_duck-typed_)
- [_dynamically typed, dynamic typing_](#_dynamically-typed-dynamic-typing_)
- [_emit, emitted Code_](#_emit-emitted-code_)
- [_enum_](#_enum_)
- [_evolving_ _`any`_](#_evolving_-_any_)
- [_extending an interface_](#_extending-an-interface_)
- [_function overload, overloaded function_](#_function-overload-overloaded-function_)
- [_generic_](#_generic_)
- [_generic type argument, type argument_](#_generic-type-argument-type-argument_)
- [_generic type parameter, type parameter_](#_generic-type-parameter-type-parameter_)
- [_global variable_](#_global-variable_)
- [_IDE, Integrated Development Environment_](#_ide-integrated-development-environment_)
- [_implementation signature_](#_implementation-signature_)
- [_implicit_ _`any`_](#_implicit_-_any_)
- [_interface_](#_interface_)
- [_interface merging_](#_interface-merging_)
- [_intersection type_](#_intersection-type_)
- [_JSDoc_](#_jsdoc_)
- [_literal_](#_literal_)
- [_mapped types_](#_mapped-types_)
- [_module_](#_module_)
- [_module resolution_](#_module-resolution_)
- [_namespace_](#_namespace_)
- [_`never`_](#_never_)
- [_non-null assertion_](#_non-null-assertion_)
- [_`null`_](#_null_)
- [_optional_](#_optional_)
- [_overload signature_](#_overload-signature_)
- [_override_](#_override_)
- [_parameter_](#_parameter_)
- [_parameter property_](#_parameter-property_)
- [_Pascal case_](#_pascal-case_)
- [_project references_](#_project-references_)
- [_primitive_](#_primitive_)
- [_privacy, private field_](#_privacy-private-field_)
- [_readonly_](#_readonly_)
- [_refactor_](#_refactor_)
- [_return type_](#_return-type_)
- [_Rick Roll_](#_rick-roll_)
- [_script_](#_script_)
- [_strict mode_](#_strict-mode_)
- [_strict null checking_](#_strict-null-checking_)
- [_structurally typed_](#_structurally-typed_)
- [_subclass_](#_subclass_)
- [_target_](#_target_)
- [_Thenable_](#_thenable_)
- [_top type_](#_top-type_)
- [_transpile_](#_transpile_)
- [_TSConfig_](#_tsconfig_)
- [_tuple_](#_tuple_)
- [_type_](#_type_)
- [_type annotation_](#_type-annotation_)
- [_type guard_](#_type-guard_)
- [_type narrowing_](#_type-narrowing_)
- [_type predicate_](#_type-predicate_)
- [_type system_](#_type-system_)
- [_`undefined`_](#_undefined_)
- [_union_](#_union_)
- [_`unknown`_](#_unknown_)
- [_visibility_](#_visibility_)
- [_void_](#_void_)

## _ambient context_

Một vùng trong mã nguồn nơi bạn có thể khai báo các kiểu (types) nhưng không thể khai báo phần triển khai (implementations). Thường được sử dụng để chỉ các tệp khai báo _.d.ts_. Xem thêm _declaration file_.

## _`any`_

Một kiểu dữ liệu được phép sử dụng ở bất kỳ đâu và có thể nhận bất kỳ giá trị nào. `any` có thể hoạt động như một top type, theo nghĩa là bất kỳ kiểu nào cũng có thể được cung cấp cho một vị trí có kiểu `any`. Hầu hết thời gian, bạn có thể muốn sử dụng `unknown` để đảm bảo an toàn kiểu chính xác hơn.

Xem thêm `unknown`, _top type_.

## _argument_

Một đối số được cung cấp dưới dạng đầu vào, dùng để chỉ một giá trị được truyền vào một hàm. Đối với các hàm, _argument_ (đối số) là giá trị được truyền cho một lời gọi, trong khi _parameter_ (tham số) là giá trị bên trong hàm. Xem thêm _parameter_.

## _assertion, type assertion_

Một khẳng định kiểu với TypeScript rằng một giá trị thuộc về một kiểu khác với những gì TypeScript mong đợi.

## _assignable, assignability_

Khả năng gán: liệu một kiểu có được phép sử dụng thay cho một kiểu khác hay không.

## _billion-dollar mistake_

Sai lầm tỷ đô: thuật ngữ ngành hấp dẫn chỉ việc nhiều hệ thống kiểu cho phép các giá trị như `null` được sử dụng ở những nơi yêu cầu một kiểu khác. Được đặt ra bởi Tony Hoare để ám chỉ mức độ thiệt hại mà nó gây ra. Xem thêm _strict null checking_.

## _bottom type_

Kiểu đáy: một kiểu không có giá trị khả dĩ nào—tập hợp rỗng của các kiểu. Không có kiểu nào có thể gán được cho kiểu đáy. TypeScript cung cấp từ khóa `never` để chỉ ra kiểu đáy.

Xem thêm `never`.

## _call signature_

Chữ ký cuộc gọi: mô tả của hệ thống kiểu về cách một hàm có thể được gọi. Bao gồm danh sách các tham số và một kiểu trả về.

## _camel case_

Quy ước đặt tên trong đó chữ cái đầu tiên của mỗi từ ghép sau từ đầu tiên trong một tên được viết hoa, như camelCase. Đây là quy ước cho tên của các thành viên trong nhiều cấu trúc hệ thống kiểu TypeScript, bao gồm các thành viên của lớp và giao diện.

## _class_

Lớp: cú pháp tiện ích của JavaScript bao bọc các hàm gán vào một prototype. TypeScript cho phép làm việc với các lớp JavaScript.

## _compile_

Biên dịch: chuyển đổi mã nguồn sang một định dạng khác. TypeScript bao gồm một trình biên dịch, ngoài việc kiểm tra kiểu, còn biến mã nguồn TypeScript thành JavaScript và/hoặc các tệp khai báo.

Xem thêm _transpile_.

## _conditional type_

Kiểu điều kiện: một kiểu giải quyết thành một trong hai kiểu có thể, dựa trên một kiểu hiện có.

## _const assertion_

Khẳng định const: cú pháp viết tắt khẳng định kiểu `as const` yêu cầu TypeScript sử dụng dạng literal, chỉ đọc (read-only) cụ thể nhất có thể của kiểu giá trị.

## _constituent, constituent type_

Thành phần, kiểu thành phần: một trong các kiểu cấu thành nên một kiểu giao (intersection) hoặc kiểu hợp (union).

## _declaration file_

Tệp khai báo: một tệp có phần mở rộng _.d.ts_. Các tệp khai báo tạo ra một ambient context, nghĩa là chúng chỉ có thể khai báo các kiểu mà không thể khai báo phần triển khai.

Xem thêm _ambient context_.

## _decorator_

Một đề xuất thử nghiệm của JavaScript cho phép chú thích một lớp hoặc thành viên lớp bằng một hàm được đánh dấu bằng ký tự `@`. Làm như vậy sẽ khiến hàm đó được chạy trên lớp hoặc thành viên lớp đó khi được tạo ra.

## _DefinitelyTyped_

Kho lưu trữ khổng lồ chứa các định nghĩa kiểu do cộng đồng biên soạn cho các gói (gọi tắt là DT). Nó chứa hàng ngàn định nghĩa _.d.ts_ cùng với tự động hóa xung quanh việc đánh giá các đề xuất thay đổi và xuất bản các bản cập nhật. Các định nghĩa đó được xuất bản dưới dạng các gói thuộc tổ chức `@types/` trên npm, chẳng hạn như `@types/react`.

## _derived interface_

Giao diện dẫn xuất: một interface mở rộng ít nhất một interface khác, được gọi là base interface (interface cơ sở). Làm như vậy sẽ sao chép tất cả các thành viên của base interface vào derived interface.

## _discriminant_

Trường phân biệt: một thành viên của một discriminated union có cùng tên nhưng khác kiểu trong mỗi thành phần.

## _discriminated union, discriminated type union_

Một union gồm các kiểu trong đó một thành viên “discriminant” tồn tại với cùng tên nhưng có giá trị khác nhau trong mỗi kiểu thành phần. Việc kiểm tra giá trị của discriminant hoạt động như một hình thức thu hẹp kiểu (type narrowing).

## _distributivity_

Tính phân phối: một thuộc tính của các kiểu điều kiện trong TypeScript khi được cung cấp các kiểu mẫu union: kiểu kết quả của chúng sẽ là một union của việc áp dụng kiểu điều kiện đó cho từng thành phần (các kiểu trong union type). `ConditionalType<T | U>` giống như `Conditional<T> | Conditional<U>`.

## _duck typed_

Định kiểu vịt: một cụm từ phổ biến mô tả cách hệ thống kiểu của JavaScript hoạt động. Nó xuất phát từ câu nói: “Nếu nó trông giống con vịt và kêu quác quác như con vịt, nó có thể là con vịt.” Nó có nghĩa là JavaScript cho phép bất kỳ giá trị nào được truyền vào bất kỳ đâu; nếu một đối tượng được yêu cầu một thành viên không tồn tại, kết quả sẽ là `undefined`.

Xem thêm _structurally typed_.

## _dynamically typed, dynamic typing_

Định kiểu động: một phân loại ngôn ngữ lập trình vốn không bao gồm trình kiểm tra kiểu. Các ví dụ về ngôn ngữ lập trình định kiểu động bao gồm JavaScript và Ruby.

## _emit, emitted Code_

Xuất mã: đầu ra từ một trình biên dịch, chẳng hạn như các tệp _.js_ thường được tạo ra khi chạy `tsc`. Các tệp JavaScript và/hoặc khai báo được xuất ra từ trình biên dịch TypeScript có thể được kiểm soát bởi các tùy chọn trình biên dịch của nó.

## _enum_

Kiểu liệt kê: một tập hợp các giá trị literal được lưu trữ trong một đối tượng với một tên thân thiện cho mỗi giá trị. Enums là một ví dụ hiếm hoi về phần mở rộng cú pháp đặc thù của TypeScript cho JavaScript thuần túy.

## _evolving_ _`any`_

Một trường hợp đặc biệt của `any` ngầm định dành cho các biến không có chú thích kiểu hoặc giá trị khởi tạo ban đầu. Kiểu của chúng sẽ được phát triển (evolve) dần thành bất kỳ thứ gì chúng được sử dụng cùng.

Xem thêm _implicit `any`_.

## _extending an interface_

Mở rộng giao diện: khi một interface khai báo rằng nó mở rộng một interface khác. Làm như vậy sẽ sao chép tất cả các thành viên của interface ban đầu vào interface mới. Xem thêm _interface_.

## _function overload, overloaded function_

Nạp chồng hàm: một cách để mô tả một hàm có thể được gọi với các tập hợp tham số hoàn toàn khác nhau.

## _generic_

Kiểu tổng quát: cho phép thay thế một kiểu khác nhau cho một cấu trúc mỗi khi một lần sử dụng mới của cấu trúc đó được tạo ra. Lớp, giao diện và bí danh kiểu đều có thể được chuyển thành generic.

## _generic type argument, type argument_

Đối số kiểu generic, đối số kiểu: một kiểu được cung cấp dưới dạng tham số kiểu cho một cấu trúc generic.

## _generic type parameter, type parameter_

Tham số kiểu generic, tham số kiểu: một kiểu được thay thế cho một generic. Các tham số kiểu generic có thể được cung cấp với các đối số kiểu khác nhau cho mỗi thể hiện của cấu trúc nhưng sẽ duy trì tính nhất quán bên trong thể hiện đó.

## _global variable_

Biến toàn cục: một biến tồn tại trong phạm vi toàn cục, chẳng hạn như `setTimeout` trong các môi trường như trình duyệt, Deno và Node.

## _IDE, Integrated Development Environment_

Môi trường phát triển tích hợp: chương trình cung cấp công cụ cho nhà phát triển bên trên trình soạn thảo văn bản cho mã nguồn. Các IDE thường đi kèm với trình gỡ lỗi (debuggers), tô sáng cú pháp và các plugin hiển thị các cảnh báo từ ngôn ngữ lập trình như lỗi kiểu. Cuốn sách này sử dụng VS Code cho các ví dụ IDE của nó, nhưng các IDE khác bao gồm Atom, Emacs, Vim, Visual Studio và WebStorm.

## _implementation signature_

Chữ ký triển khai: chữ ký cuối cùng được khai báo trên một hàm nạp chồng, được sử dụng cho các tham số triển khai của nó.

Xem thêm _function overload_.

## _implicit_ _`any`_

Khi TypeScript không thể suy luận ngay kiểu của một thuộc tính lớp, tham số hàm hoặc biến, nó sẽ ngầm định giả định kiểu là `any`. Các kiểu `any` ngầm định cho các thuộc tính lớp và tham số hàm có thể được cấu hình thành lỗi kiểu bằng cách sử dụng tùy chọn trình biên dịch `noImplicitAny`.

## _interface_

Giao diện: một tập hợp các thuộc tính được đặt tên. TypeScript sẽ biết một giá trị được khai báo thuộc kiểu của một interface cụ thể sẽ có các thuộc tính đã khai báo của interface đó.

## _interface merging_

Hợp nhất giao diện: một thuộc tính của interface khi nhiều interface có cùng tên được khai báo trong cùng một phạm vi, chúng kết hợp thành một interface duy nhất thay vì gây ra lỗi kiểu về xung đột tên. Điều này thường được các tác giả định nghĩa kiểu sử dụng nhiều nhất để bổ sung các interface toàn cục như `Window`.

## _intersection type_

Kiểu giao: một kiểu sử dụng toán tử `&` để chỉ ra rằng nó có tất cả các thuộc tính của cả hai thành phần của nó.

## _JSDoc_

Một tiêu chuẩn cho các khối chú thích `/** ... */` mô tả các đoạn mã như lớp, hàm và biến. Thường được sử dụng trong các dự án JavaScript để mô tả sơ lược các kiểu dữ liệu.

## _literal_

Một giá trị được biết đến là một thể hiện cụ thể duy nhất của một kiểu nguyên thủy.

## _mapped types_

Kiểu ánh xạ: một kiểu nhận vào một kiểu khác và thực hiện một số thao tác trên mỗi thành viên của kiểu đó. Nói cách khác, nó _ánh xạ_ từ các thành viên của một kiểu thành một tập hợp các thành viên mới.

## _module_

Một tệp có `export` hoặc `import` ở cấp cao nhất. Đây thường là các tệp trong mã nguồn của bạn hoặc các tệp trong các gói `node_modules/`. Xem thêm _script_.

## _module resolution_

Phân giải module: tập hợp các bước được sử dụng để xác định tệp mà một lệnh import module giải quyết thành. Trình biên dịch TypeScript có thể chỉ định điều này bằng tùy chọn trình biên dịch `moduleResolution`.

## _namespace_

Không gian tên: một cấu trúc cũ trong TypeScript tạo ra một đối tượng có sẵn trên toàn cầu với nội dung được “export” có sẵn để gọi dưới dạng các thành viên của đối tượng đó. Namespaces là một ví dụ hiếm hoi về phần mở rộng cú pháp đặc thù của TypeScript cho JavaScript thuần túy. Ngày nay, chúng chủ yếu được sử dụng trong các tệp khai báo _.d.ts_.

## _`never`_

Kiểu TypeScript đại diện cho bottom type: một kiểu không thể có giá trị khả dĩ nào.

Xem thêm _bottom type_.

## _non-null assertion_

Khẳng định không null: cú pháp viết tắt `!` khẳng định rằng một kiểu không phải là `null` hoặc `undefined`.

## _`null`_

Một trong hai kiểu nguyên thủy trong JavaScript đại diện cho sự thiếu vắng giá trị. `null` đại diện cho sự thiếu vắng giá trị có chủ ý, trong khi `undefined` đại diện cho sự thiếu vắng giá trị nói chung.

Xem thêm `undefined`.

## _optional_

Tùy chọn: một tham số hàm, thuộc tính lớp hoặc thành viên của một interface hoặc kiểu đối tượng không bắt buộc phải được cung cấp. Được biểu thị bằng cách đặt dấu `?` sau tên của nó, hoặc đối với các tham số hàm và thuộc tính lớp, được biểu thị thay thế bằng cách cung cấp một giá trị mặc định với dấu `=`.

## _overload signature_

Chữ ký nạp chồng: một trong các chữ ký được khai báo trên một hàm nạp chồng để mô tả một cách mà nó có thể được gọi.

Xem thêm _function overload_.

## _override_

Ghi đè: khai báo lại một thuộc tính trên một đối tượng giao diện dẫn xuất hoặc lớp con vốn đã tồn tại trên lớp cơ sở.

## _parameter_

Tham số: một đầu vào nhận được, thường đề cập đến những gì một hàm khai báo. Đối với các hàm, _argument_ (đối số) là giá trị được truyền cho một lời gọi, trong khi _parameter_ (tham số) là giá trị bên trong hàm.

Xem thêm _argument_.

## _parameter property_

Thuộc tính tham số: phần mở rộng cú pháp TypeScript để khai báo một thuộc tính được gán cho một thuộc tính thành viên cùng kiểu ở đầu constructor của một lớp.

## _Pascal case_

Quy ước đặt tên trong đó chữ cái đầu tiên của mỗi từ ghép trong một tên được viết hoa, như PascalCase. Quy ước cho tên của nhiều cấu trúc hệ thống kiểu TypeScript, bao gồm generics, interfaces và type aliases.

## _project references_

Tham chiếu dự án: một tính năng của các tệp cấu hình TypeScript trong đó chúng có thể tham chiếu các dự án của các tệp cấu hình khác như các phụ thuộc. Điều này cho phép bạn sử dụng TypeScript như một bộ điều phối build để thực thi một cây phụ thuộc dự án.

## _primitive_

Kiểu nguyên thủy: một kiểu dữ liệu bất biến được tích hợp sẵn trong JavaScript và không phải là một đối tượng. Chúng gồm: `null`, `undefined`, `boolean`, `string`, `number`, `bigint`, và `symbol`.

## _privacy, private field_

Quyền riêng tư, trường private: một tính năng của JavaScript trong đó các thành viên lớp có tên bắt đầu bằng `#` chỉ có thể được truy cập bên trong chính lớp đó.

## _readonly_

Chỉ đọc: một tính năng của hệ thống kiểu TypeScript trong đó việc thêm từ khóa `readonly` vào trước một thành viên lớp hoặc đối tượng chỉ ra rằng nó không thể được gán lại giá trị.

## _refactor_

Tái cấu trúc: một sự thay đổi đối với mã nguồn giữ nguyên hầu hết hoặc tất cả các hành vi của nó. Dịch vụ ngôn ngữ TypeScript có thể thực hiện một số tái cấu trúc trên mã nguồn khi được yêu cầu, chẳng hạn như chuyển các dòng mã phức tạp thành một biến `const`.

## _return type_

Kiểu trả về: kiểu phải được trả về bởi một hàm. Nếu tồn tại nhiều câu lệnh `return` trong hàm với các kiểu khác nhau, nó sẽ là một union của tất cả các kiểu có thể đó. Nếu hàm không thể trả về, nó sẽ là `never`.

## _Rick Roll_

Một meme trên internet nơi người dùng bị lừa nghe và/hoặc xem video âm nhạc kinh điển “Never Gonna Give You Up” của Rick Astley. Tôi đã giấu một vài liên kết như vậy trong cuốn sách này.

Xem thêm _https://oreil.ly/rickroll_

## _script_

Bất kỳ tệp mã nguồn nào không phải là một module.

Xem thêm _module_.

## _strict mode_

Chế độ nghiêm ngặt: một tập hợp các tùy chọn trình biên dịch làm tăng mức độ nghiêm ngặt và số lượng kiểm tra mà bộ kiểm tra kiểu TypeScript thực hiện. Điều này có thể được bật cho `tsc` với cờ `--strict` và trong các tệp TSConfig với tùy chọn `"strict": true`.

## _strict null checking_

Kiểm tra null nghiêm ngặt: một chế độ nghiêm ngặt cho TypeScript trong đó `null` và `undefined` không còn được phép cung cấp cho các kiểu không bao gồm chúng một cách tường minh.

Xem thêm _billion-dollar mistake_.

## _structurally typed_

Định kiểu theo cấu trúc: một hệ thống kiểu trong đó bất kỳ giá trị nào tình cờ thỏa mãn một kiểu đều được phép sử dụng như một thể hiện của kiểu đó.

Xem thêm _duck typed_.

## _subclass_

Lớp con: một lớp mở rộng (extends) một lớp khác, được gọi là lớp cơ sở (base class). Làm như vậy sẽ sao chép các thành viên của prototype lớp cơ sở sang prototype của lớp con.

## _target_

Tùy chọn trình biên dịch TypeScript để chỉ định mã JavaScript cần được chuyển mã ngược về mức hỗ trợ cú pháp nào, chẳng hạn như `"es5"` hoặc `"es2017"`. Mặc dù `target` mặc định là `"es3"` vì lý do tương thích ngược, bạn nên sử dụng cú pháp JavaScript mới nhất có thể theo (các) nền tảng mục tiêu của mình, vì việc hỗ trợ các tính năng JavaScript mới hơn trong các môi trường cũ hơn đòi hỏi phải tạo ra nhiều mã JavaScript hơn.

## _Thenable_

Một đối tượng JavaScript có phương thức `.then` nhận vào tối đa hai hàm callback và trả về một Thenable khác. Thường được triển khai nhất bởi lớp `Promise` tích hợp sẵn, nhưng các lớp và đối tượng do người dùng định nghĩa cũng có thể hoạt động như một Thenable.

## _top type_

Kiểu đỉnh: một kiểu có thể đại diện cho bất kỳ kiểu nào có thể có trong một hệ thống.

Xem thêm `any`, `unknown`.

## _transpile_

Chuyển mã: một thuật ngữ chỉ việc biên dịch biến đổi mã nguồn từ một ngôn ngữ lập trình mà con người có thể đọc được sang một ngôn ngữ khác. TypeScript bao gồm một trình biên dịch chuyển đổi mã nguồn TypeScript _.ts_ / _.tsx_ thành các tệp _.js_, đôi khi được gọi là quá trình chuyển mã (transpilation).

Xem thêm _compile_.

## _TSConfig_

Tệp cấu hình JSON cho TypeScript. Phổ biến nhất có tên là _tsconfig.json_ hoặc theo mẫu _tsconfig.*.json_. Các trình soạn thảo như VS Code sẽ đọc từ tệp _tsconfig.json_ trong một thư mục để xác định các tùy chọn cấu hình dịch vụ ngôn ngữ TypeScript.

## _tuple_

Một mảng có kích thước cố định trong đó mỗi phần tử được cung cấp một kiểu dữ liệu rõ ràng. Ví dụ: `[number, string | undefined]` là một tuple có kích thước hai trong đó phần tử đầu tiên có kiểu `number` và phần tử thứ hai có kiểu `string | undefined`.

## _type_

Kiểu dữ liệu: sự hiểu biết về những thành viên và khả năng mà một giá trị có. Đây có thể là các kiểu nguyên thủy như `string`, literals như `123`, hoặc các hình dạng phức tạp hơn như hàm và đối tượng.

## _type annotation_

Chú thích kiểu: một chú thích sau một tên được sử dụng để chỉ ra kiểu của nó. Bao gồm dấu `:` và tên của một kiểu.

## _type guard_

Bộ bảo vệ kiểu: một đoạn logic thời gian chạy có thể được hiểu trong hệ thống kiểu để chỉ cho phép một số logic nhất định nếu một giá trị thuộc về một kiểu cụ thể.

## _type narrowing_

Thu hẹp kiểu: khi TypeScript có thể suy luận một kiểu cụ thể hơn cho một giá trị bên trong một khối mã được bảo vệ bởi một type guard.

## _type predicate_

Vị từ kiểu: một hàm có kiểu trả về được chú thích để hoạt động như một type guard. Các hàm type predicate trả về một giá trị `boolean` cho biết liệu một giá trị có thuộc một kiểu hay không.

## _type system_

Hệ thống kiểu: tập hợp các quy tắc về cách một ngôn ngữ lập trình hiểu các cấu trúc trong một chương trình có thể có những kiểu nào.

## _`undefined`_

Một trong hai kiểu nguyên thủy trong JavaScript đại diện cho sự thiếu vắng giá trị. `null` đại diện cho sự thiếu vắng giá trị có chủ ý, trong khi `undefined` đại diện cho sự thiếu vắng giá trị nói chung.

Xem thêm `null`.

## _union_

Kiểu hợp: một kiểu mô tả một giá trị có thể là hai hoặc nhiều kiểu có thể. Được biểu thị bằng dấu gạch đứng `|` giữa mỗi kiểu có thể.

## _`unknown`_

Khái niệm TypeScript đại diện cho top type. `unknown` không cho phép truy cập thành viên tùy ý mà không thực hiện thu hẹp kiểu.

Xem thêm `any`, _top type_.

## _visibility_

Phạm vi hiển thị: chỉ định xem một thành viên lớp có hiển thị với mã bên ngoài lớp hay không. Được chỉ định trước khai báo của thành viên với các từ khóa `public`, `protected`, và `private`. Visibility và các từ khóa của nó ra đời trước tính năng bảo mật thành viên `#` thực sự của JavaScript và chỉ tồn tại trong hệ thống kiểu TypeScript. Xem thêm _privacy_.

## _void_

Một kiểu chỉ ra việc thiếu giá trị trả về từ một hàm, được biểu thị bằng từ khóa `void` trong TypeScript. Các hàm được coi là trả về `void` nếu chúng không có câu lệnh `return` nào trả về một giá trị.
