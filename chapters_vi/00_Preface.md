# **Lời nói đầu**

## Mục lục

- [**Lời nói đầu**](#lời-nói-đầu)
  - [**Ai nên đọc cuốn sách này**](#ai-nên-đọc-cuốn-sách-này)
  - [**Tại sao tôi viết cuốn sách này**](#tại-sao-tôi-viết-cuốn-sách-này)
  - [**Cách tiếp cận cuốn sách này**](#cách-tiếp-cận-cuốn-sách-này)
    - [**Ví dụ và Dự án thực hành**](#ví-dụ-và-dự-án-thực-hành)
  - [**Quy ước trình bày trong sách**](#quy-ước-trình-bày-trong-sách)
    - [**MẸO (TIP)**](#mẹo-tip)
    - [**GHI CHÚ (NOTE)**](#ghi-chú-note)
    - [**CẢNH BÁO (WARNING)**](#cảnh-báo-warning)
  - [**Sử dụng mã nguồn mẫu**](#sử-dụng-mã-nguồn-mẫu)
  - [**Nền tảng Học trực tuyến O’Reilly**](#nền-tảng-học-trực-tuyến-oreilly)
    - [**GHI CHÚ (NOTE)**](#ghi-chú-note-1)
  - [**Cách liên hệ với chúng tôi**](#cách-liên-hệ-với-chúng-tôi)
  - [**Lời cảm ơn**](#lời-cảm-ơn)

Hành trình đến với TypeScript của tôi không phải là một con đường thẳng tắp hay nhanh chóng. Thời còn đi học, tôi chủ yếu lập trình bằng Java, sau đó là C++, và giống như nhiều lập trình viên mới lớn lên cùng các ngôn ngữ định kiểu tĩnh (statically typed languages), tôi từng xem thường JavaScript, coi nó “chỉ” là một ngôn ngữ kịch bản cẩu thả mà người ta tiện tay ném vào các trang web.

Dự án đáng kể đầu tiên của tôi với JavaScript là một bản làm lại vui vẻ tựa game kinh điển _Super Mario Bros._ bằng HTML5/CSS/JavaScript thuần, và như bao dự án đầu tay khác, nó thực sự là một mớ hỗn độn. Khi mới bắt đầu dự án, theo bản năng, tôi không thích sự linh hoạt kỳ lạ và sự thiếu vắng các rào chắn an toàn của JavaScript. Chỉ đến giai đoạn cuối của dự án, tôi mới thực sự bắt đầu trân trọng các tính năng và những nét độc đáo của JavaScript: sự linh hoạt của một ngôn ngữ, khả năng kết hợp các hàm nhỏ một cách linh hoạt, và khả năng _chạy ngay tức thì_ trên trình duyệt người dùng chỉ trong vài giây sau khi tải trang.

Đến khi hoàn thành dự án đầu tiên đó, tôi đã hoàn toàn say mê JavaScript. Ban đầu, các công cụ phân tích tĩnh (static analysis - các công cụ phân tích mã nguồn của bạn mà không cần chạy nó) như TypeScript cũng khiến tôi có cảm giác e ngại. _JavaScript vốn dĩ rất thanh thoát và mượt mà_, tôi tự nhủ, _tại sao lại phải tự trói mình vào những cấu trúc và kiểu dữ liệu cứng nhắc?_ Phải chăng chúng ta đang quay trở lại thế giới của Java và C++ mà tôi vừa bỏ lại phía sau?

Thế nhưng khi quay lại các dự án cũ, tôi chỉ mất đúng 10 phút vật lộn để đọc lại mớ code JavaScript phức tạp, rắc rối ngày xưa là đã hiểu được mọi thứ có thể trở nên lộn xộn đến mức nào nếu không có static analysis. Quá trình dọn dẹp mớ code đó đã chỉ cho tôi thấy tất cả những chỗ mà lẽ ra tôi đã có thể hưởng lợi rất nhiều nếu có một cấu trúc chặt chẽ. Kể từ thời điểm đó, tôi đã bị cuốn hút vào việc đưa static analysis vào nhiều nhất có thể trong các dự án của mình.

Đã gần một thập kỷ trôi qua kể từ lần đầu tiên tôi tìm hiểu về TypeScript, và niềm say mê của tôi dành cho nó vẫn vẹn nguyên như ngày nào. Ngôn ngữ này vẫn đang không ngừng phát triển với các tính năng mới và ngày càng trở nên hữu ích hơn bao giờ hết trong việc mang lại _sự an toàn_ và _cấu trúc_ cho JavaScript.

Tôi hy vọng rằng qua việc đọc _Learning TypeScript_, bạn sẽ học được cách trân trọng TypeScript theo cách của tôi: không chỉ như một công cụ để tìm lỗi và lỗi chính tả—và chắc chắn không phải là một sự thay đổi hoàn toàn các mô hình code JavaScript—mà là JavaScript _kèm theo hệ thống kiểu (types)_: một hệ thống tuyệt đẹp để khai báo cách mà mã JavaScript của chúng ta nên hoạt động và giúp chúng ta kiên định tuân thủ theo nó.

## **Ai nên đọc cuốn sách này**

Nếu bạn đã có hiểu biết cơ bản về cách viết code JavaScript, có thể chạy các lệnh cơ bản trong terminal và quan tâm đến việc học TypeScript, thì cuốn sách này dành cho bạn.

Có thể bạn từng nghe nói rằng TypeScript giúp bạn viết được nhiều code JavaScript hơn mà ít lỗi hơn _(đúng vậy!)_ hoặc giúp tài liệu hóa code của bạn một cách rõ ràng để người khác dễ đọc _(cũng đúng luôn!)_. Có thể bạn đã thấy TypeScript xuất hiện trong rất nhiều tin tuyển dụng, hoặc trong một vai trò công việc mới mà bạn chuẩn bị đảm nhận.

Dù lý do của bạn là gì, chỉ cần bạn nắm vững các nguyên lý cơ bản của JavaScript—biến (variables), hàm (functions), closure/phạm vi (scope), và lớp (classes)—cuốn sách này sẽ dẫn dắt bạn từ chỗ chưa biết gì về TypeScript đến việc làm chủ các nền tảng và tính năng quan trọng nhất của ngôn ngữ. Khi đọc xong cuốn sách này, bạn sẽ hiểu được:

- Lịch sử và bối cảnh vì sao TypeScript lại hữu ích vượt trội so với JavaScript thuần (“vanilla” JavaScript)
- Cách một hệ thống kiểu (type system) mô hình hóa mã nguồn
- Cách một bộ kiểm tra kiểu (type checker) phân tích mã nguồn
- Cách sử dụng các chú thích kiểu (type annotations) chỉ dùng trong quá trình phát triển để cung cấp thông tin cho hệ thống kiểu
- Cách TypeScript kết hợp với các IDE (Integrated Development Environments) để cung cấp các công cụ khám phá mã nguồn và hiệu chỉnh (refactoring)

Và bạn sẽ có khả năng:

- Trình bày rõ ràng những lợi ích của TypeScript và các đặc tính chung của hệ thống kiểu trong ngôn ngữ này.
- Bổ sung các chú thích kiểu (type annotations) ở những nơi hữu ích trong mã nguồn của bạn.
- Biểu diễn các kiểu dữ liệu có độ phức tạp vừa phải bằng cách sử dụng khả năng suy luận kiểu tích hợp sẵn và cú pháp mới của TypeScript.
- Sử dụng TypeScript để hỗ trợ quá trình phát triển cục bộ khi tái cấu trúc mã nguồn.

## **Tại sao tôi viết cuốn sách này**

TypeScript là một ngôn ngữ vô cùng phổ biến trong cả ngành công nghiệp phần mềm lẫn cộng đồng mã nguồn mở:

- Khảo sát State of the Octoverse năm 2020 và 2021 của GitHub xếp TypeScript là ngôn ngữ phổ biến thứ 4 trên nền tảng, tăng từ vị trí thứ 7 trong năm 2018, 2019 và vị trí thứ 10 trong năm 2017.
- Khảo sát Developer Survey năm 2021 của StackOverflow xếp TypeScript là ngôn ngữ được yêu thích thứ 3 trên thế giới (với 72.73% người dùng yêu thích).
- Khảo sát State of JS năm 2020 cho thấy TypeScript liên tục đạt mức độ hài lòng và mức độ sử dụng cao ở cả vai trò công cụ xây dựng (build tool) lẫn biến thể của JavaScript.

Đối với các lập trình viên frontend, TypeScript được hỗ trợ toàn diện trong tất cả các thư viện và framework UI lớn, bao gồm Angular (vốn khuyến nghị mạnh mẽ việc dùng TypeScript), cũng như Gatsby, Next.js, React, Svelte và Vue. Đối với các lập trình viên backend, TypeScript biên dịch ra JavaScript có thể chạy tự nhiên trong Node.js; Deno, một runtime tương tự do chính tác giả của Node tạo ra, chú trọng việc hỗ trợ trực tiếp các tệp TypeScript.

Tuy nhiên, bất chấp sự hỗ trợ rộng rãi từ vô số dự án phổ biến này, tôi đã khá thất vọng trước sự thiếu vắng các tài liệu nhập môn chất lượng trên mạng khi mới bắt đầu học ngôn ngữ này. Rất nhiều nguồn tài liệu trực tuyến chưa làm tốt việc giải thích “hệ thống kiểu” là gì hoặc cách sử dụng nó ra sao. Chúng thường mặc định rằng người đọc đã có rất nhiều kiến thức nền tảng về cả JavaScript lẫn các ngôn ngữ định kiểu mạnh (strongly typed languages), hoặc chỉ được viết với những ví dụ mã nguồn rất sơ sài.

Việc không tìm thấy một cuốn sách O’Reilly với hình bìa con vật dễ thương giới thiệu về TypeScript cách đây nhiều năm là một điều đáng tiếc. Mặc dù hiện nay đã có những cuốn sách khác về TypeScript từ các nhà xuất bản bao gồm cả O’Reilly ra mắt trước cuốn này, nhưng tôi vẫn không tìm thấy một cuốn sách nào tập trung vào nền tảng của ngôn ngữ theo đúng cách mà tôi mong muốn: tại sao nó lại hoạt động như vậy và các tính năng cốt lõi của nó phối hợp với nhau ra sao. Một cuốn sách bắt đầu từ phần giải thích nền tảng của ngôn ngữ trước khi bổ sung từng tính năng một. Tôi rất hào hứng khi có thể mang đến một tài liệu nhập môn rõ ràng, toàn diện về các nguyên lý nền tảng của ngôn ngữ TypeScript dành cho những độc giả chưa quen thuộc với các nguyên tắc của nó.

## **Cách tiếp cận cuốn sách này**

_Learning TypeScript_ phục vụ hai mục đích:

- Bạn có thể đọc qua một lượt để hiểu toàn diện về TypeScript.
- Về sau, bạn có thể tra cứu lại nó như một tài liệu tham khảo thực tế về ngôn ngữ TypeScript.

Cuốn sách này nâng dần từ các khái niệm đến cách sử dụng thực tế qua ba phần chính:

- Phần I, “Khái niệm” (Concepts): JavaScript đã ra đời như thế nào, TypeScript bổ sung những gì cho nó, và nền tảng của một _hệ thống kiểu_ (type system) do TypeScript tạo ra.
- Phần II, “Tính năng” (Features): Làm sáng tỏ cách hệ thống kiểu tương tác với các thành phần chính của JavaScript mà bạn sẽ làm việc cùng khi viết mã TypeScript.
- Phần III, “Ứng dụng” (Usage): Sau khi bạn đã hiểu các tính năng tạo nên ngôn ngữ TypeScript, phần này sẽ hướng dẫn cách áp dụng chúng vào các tình huống thực tế để cải thiện trải nghiệm đọc và chỉnh sửa mã nguồn.

Tôi cũng đưa thêm Phần IV, “Điểm cộng thêm” (Extra Credit) ở cuối sách để đề cập đến các tính năng ít phổ biến hơn nhưng đôi khi vẫn rất hữu ích của TypeScript. Bạn không nhất thiết phải nắm thật sâu những tính năng này mới được coi là một lập trình viên TypeScript. Nhưng chúng đều là những khái niệm hữu ích mà bạn có thể sẽ gặp phải khi sử dụng TypeScript trong các dự án thực tế. Một khi bạn đã hiểu thấu đáo ba phần đầu tiên, tôi rất khuyến khích bạn tìm hiểu thêm phần điểm cộng này.

Mỗi chương đều bắt đầu bằng một bài thơ haiku để tạo cảm hứng cho nội dung và kết thúc bằng một câu chơi chữ (pun). Cộng đồng phát triển web nói chung và cộng đồng TypeScript nói riêng nổi tiếng là vui vẻ và luôn chào đón những người mới. Tôi đã cố gắng làm cho cuốn sách này trở nên thú vị, dễ chịu đối với những người học giống như tôi—những người không thích những bài viết dài dòng, khô khan.

### **Ví dụ và Dự án thực hành**

Không giống như nhiều tài liệu khác giới thiệu về TypeScript, cuốn sách này chủ ý tập trung vào việc giới thiệu các tính năng của ngôn ngữ bằng các ví dụ độc lập chỉ thể hiện đúng kiến thức mới, thay vì đi sâu vào các dự án quy mô vừa hoặc lớn. Tôi ưa chuộng phương pháp giảng dạy này vì nó đặt trọng tâm vào chính ngôn ngữ TypeScript trước hết. TypeScript hữu ích trên rất nhiều framework và nền tảng khác nhau—nhiều nền tảng trong số đó cập nhật API thường xuyên—vì vậy tôi không muốn đưa bất kỳ nội dung nào đặc thù cho riêng một framework hay nền tảng cụ thể vào cuốn sách này.

Tuy nhiên, khi học một ngôn ngữ lập trình, việc thực hành ngay các khái niệm sau khi được giới thiệu là vô cùng hữu ích. Tôi rất khuyến khích bạn nghỉ ngơi đôi chút sau mỗi chương để ôn luyện lại nội dung của chương đó. Mỗi chương đều kết thúc bằng một gợi ý truy cập vào phần tương ứng trên _https://learningtypescript.com_ và hoàn thành các bài tập, dự án được liệt kê ở đó.

## **Quy ước trình bày trong sách**

Các quy ước in ấn sau đây được sử dụng trong cuốn sách này:

_Chữ nghiêng (Italic)_  
Biểu thị các thuật ngữ mới, URL, địa chỉ email, tên tệp và phần mở rộng tệp.

_Chữ cùng độ rộng (Constant width / Monospace)_  
Được sử dụng cho các đoạn mã chương trình, cũng như trong các đoạn văn để chỉ các phần tử chương trình như tên biến hoặc tên hàm, kiểu dữ liệu, câu lệnh và từ khóa.

#### **MẸO (TIP)**

Biểu tượng này biểu thị một mẹo hoặc gợi ý hữu ích.

#### **GHI CHÚ (NOTE)**

Biểu tượng này biểu thị một ghi chú chung.

#### **CẢNH BÁO (WARNING)**

Biểu tượng này biểu thị một cảnh báo hoặc lưu ý thận trọng.

## **Sử dụng mã nguồn mẫu**

Tài liệu bổ trợ (mã nguồn mẫu, bài tập, v.v.) có sẵn để tải về tại https://learningtypescript.com .

Nếu bạn có câu hỏi kỹ thuật hoặc gặp sự cố khi sử dụng mã nguồn mẫu, vui lòng gửi email tới _bookquestions@oreilly.com_.

Cuốn sách này ở đây để giúp bạn hoàn thành công việc của mình. Nói chung, nếu mã nguồn mẫu được cung cấp cùng với cuốn sách này, bạn có thể sử dụng nó trong các chương trình và tài liệu của mình. Bạn không cần phải liên hệ với chúng tôi để xin phép trừ khi bạn sao chép một phần đáng kể của mã nguồn. Ví dụ, viết một chương trình sử dụng nhiều đoạn mã từ cuốn sách này không cần xin phép. Bán hoặc phân phối các ví dụ từ sách của O’Reilly thì cần phải xin phép. Trả lời một câu hỏi bằng cách trích dẫn cuốn sách này và dẫn lại mã mẫu không cần xin phép. Đưa một lượng đáng kể mã mẫu từ cuốn sách này vào tài liệu sản phẩm của bạn thì cần phải xin phép.

Chúng tôi đánh giá cao, nhưng nhìn chung không bắt buộc, việc ghi nhận nguồn gốc (attribution). Việc ghi nhận nguồn thường bao gồm tiêu đề, tác giả, nhà xuất bản và mã ISBN. Ví dụ: “_Learning TypeScript_ của Josh Goldberg (O’Reilly). Bản quyền 2022 Josh Goldberg, 978-1-098-11033-8.”

Nếu bạn cảm thấy việc sử dụng các ví dụ mã của mình nằm ngoài phạm vi sử dụng hợp lý hoặc sự cho phép được đưa ra ở trên, vui lòng liên hệ với chúng tôi tại _permissions@oreilly.com_.

## **Nền tảng Học trực tuyến O’Reilly**

#### **GHI CHÚ (NOTE)**

Trong hơn 40 năm qua, _O’Reilly Media_ đã cung cấp chương trình đào tạo về công nghệ và kinh doanh, kiến thức cũng như sự hiểu biết sâu sắc để giúp các công ty thành công.

Mạng lưới chuyên gia và những nhà đổi mới độc đáo của chúng tôi chia sẻ kiến thức và chuyên môn của họ thông qua sách báo, bài viết và nền tảng học trực tuyến của chúng tôi. Nền tảng học trực tuyến của O’Reilly cung cấp cho bạn quyền truy cập theo nhu cầu vào các khóa đào tạo trực tiếp, các lộ trình học tập chuyên sâu, môi trường lập trình tương tác và một bộ sưu tập khổng lồ văn bản và video từ O’Reilly cùng hơn 200 nhà xuất bản khác. Để biết thêm thông tin, vui lòng truy cập _http://oreilly.com_.

## **Cách liên hệ với chúng tôi**

Vui lòng gửi các ý kiến đóng góp và câu hỏi liên quan đến cuốn sách này tới nhà xuất bản:

O’Reilly Media, Inc.

1005 Gravenstein Highway North

Sebastopol, CA 95472

800-998-9938 (tại Hoa Kỳ hoặc Canada)

707-829-0515 (quốc tế hoặc địa phương)

707-829-0104 (fax)

Chúng tôi có một trang web dành riêng cho cuốn sách này, nơi chúng tôi liệt kê các đính chính (errata), ví dụ và bất kỳ thông tin bổ sung nào. Bạn có thể truy cập trang này tại https://oreil.ly/learning-typescript .

Gửi email tới _bookquestions@oreilly.com_ để đóng góp ý kiến hoặc đặt câu hỏi kỹ thuật về cuốn sách này.

Để biết tin tức và thông tin về sách và các khóa học của chúng tôi, hãy truy cập _https://oreilly.com_.

Tìm chúng tôi trên LinkedIn: https://linkedin.com/company/oreilly-media . Theo dõi chúng tôi trên Twitter: https://twitter.com/oreillymedia .

Xem chúng tôi trên YouTube: https://www.youtube.com/oreillymedia .

## **Lời cảm ơn**

Cuốn sách này là thành quả nỗ lực của cả một tập thể, và tôi xin chân thành cảm ơn tất cả những người đã biến nó thành hiện thực. Trước hết và trên hết là tổng biên tập phi thường của tôi, Rita Fernando, vì sự kiên nhẫn đáng kinh ngạc và sự hướng dẫn xuất sắc xuyên suốt hành trình viết sách. Xin gửi lời cảm ơn đặc biệt tới những thành viên còn lại của đội ngũ O’Reilly: Kristen Brown, Suzanne Huston, Clare Jensen, Carol Keller, Elizabeth Kelly, Cheryl Lenser, Elizabeth Oliver và Amanda Quinn. Tất cả các bạn đều tuyệt vời!

Xin gửi lời cảm ơn sâu sắc tới các chuyên gia phản biện kỹ thuật vì những hiểu biết sâu sắc về mặt sư phạm và chuyên môn TypeScript luôn ở đẳng cấp hàng đầu: Mike Boyle, Ryan Cavanaugh, Sara Gallagher, Michael Hoffman, Adam Reineke và Dan Vanderkam. Cuốn sách này sẽ không thể trọn vẹn nếu không có các bạn, và tôi hy vọng mình đã truyền tải thành công tinh thần trong tất cả những gợi ý tuyệt vời của các bạn!

Xin cảm ơn các đồng nghiệp và những người bạn đã dành thời gian đánh giá nhanh cuốn sách, giúp tôi nâng cao độ chính xác kỹ thuật và chất lượng hành văn: Robert Blake, Andrew Branch, James Henry, Adam Kaczmarek, Loren Sands-Ramshaw, Nik Stern và Lenz Weber-Tronic. Mọi góp ý đều vô cùng quý giá!

Cuối cùng, tôi muốn cảm ơn gia đình vì tình yêu và sự ủng hộ trong suốt những năm qua. Bố mẹ tôi, Frances và Mark, cùng anh trai Danny—cảm ơn mọi người đã để tôi dành nhiều thời gian với Lego, sách và trò chơi điện tử. Gửi tới người bạn đời Mariah Goldberg vì sự kiên nhẫn của cô ấy trong suốt những đợt tôi miệt mài chỉnh sửa và viết lách, và những chú mèo Luci, Tiny và Jerry của chúng tôi vì vẻ ngoài lông lá đặc biệt cùng sự đồng hành bên tôi.
