![](RackMultipart20210515-4-aquj9i_html_b02dcb41ed6d21e0.png)

1. **Ansible là gì ?, được dùng khi nào ?**

- Ansible là một công cụ cung cấp phần mềm nguồn mở, quản lý cấu hình và công cụ triển khai ứng dụng cho phép cơ sở hạ tầng dưới dạng mã ( &quot;Infrastructure As Code&quot;) một cách tự động. Nó chạy trên nhiều hệ thống giống Unix và có thể cấu hình cả các hệ thống giống Unix cũng như Microsoft Windows. Nó bao gồm ngôn ngữ khai báo riêng để mô tả cấu hình hệ thống.

![](RackMultipart20210515-4-aquj9i_html_4f83ba7a6f36ff3.png)

- Inventory : Một file INI chứa các thông tin về các server từ xa mà bạn quản lý (host 1 in group A and host 2 to N base on group B)
- Playbook : Một file YAML chứa một tập các công việc cần tự động hóa.

Ex: Công việc đầy tiên cần thực hiện là update web và tiếp theo là update version, start database theo một cách tuần tự.

![](RackMultipart20210515-4-aquj9i_html_37f9b1b1efe336e8.png)

- Ansible được sử dụng khi:

- Đảm bảo việc quản lý cấu hình của thiết bị, ứng dụng một cách hiệu quả, tức là quản lý đơn giản, kiểm soát được các cấu hình đúng và đủ hay chưa, chúng chạy có chính xác hay không.
- Tái sử dụng được các bước triển khai trước đó (các bước lặp đi lặp lại khi cài đặt, cấu hình máy chủ, cấu hình ứng dụng)
- Tự động hóa và áp dụng hàng loạt các việc trên hoàng loạt các server, hàng loạt các ứng dụng với các inventory và các playbook trong thời gian ngắn nhất.

1. **Cà**** i đặt Ansible:**

- Cài đặt Ansible sẽ phụ thuộc vào môi trường và mỗi môi trường sẽ có cách cài đặt khác nhau ([kham khảo](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)). Mỗi clustering chỉ cẩn một máy chủ ansible thực thi và ssh tới các node để triển khai.

![](RackMultipart20210515-4-aquj9i_html_608e873f48870b06.png)

- Để kiểm tra lại version:

![](RackMultipart20210515-4-aquj9i_html_507aee936f26704f.png)

- Cấu hình SSH Key và khai báo file inventory: Ansible hoạt động theo cơ chế agentless. Có nghĩa là không cần cài agent vào các node(client), thay vào đó ansible sẽ sử dụng việc điều khiển các node thông qua SSH. Do vầy sẽ có 2 cách để ansible có thể điều kiển các node:

- Sử dụng username, port của ssh để khai báo trong inventory.
- Sử dụng ssh keypair. Sẽ có cặp public key và private key trên node AnsibleServer và copy sang các node client ( [Examble of rsa key pair](https://news.cloud365.vn/10-phut-ansible-co-ban-phan-4-viet-playbook-tren-ansible/) )

1. **Ad**** -hoc command Ansible.**

- Khi vận hành hệ thống, sẽ có những tác vụ phải thực hiện đi thực hiện lại nhiều lần như: Reset OS, kiểm tra trạng thái của services hoặc là xem file log nào đó và đôi khi là thao tác copy một file nào đó giữa các node.
- Câu chuyện sẽ không có gì nếu ta chỉ có một vài máy chủ, nhưng giả sử ta cần thực hiện trên hàng loạt node thì lúc này chế độ ad-hoc command của ansible bắt đầu thể hiện được ưu thế của nó.
- Cú pháp chung của ad-hoc command:

![](RackMultipart20210515-4-aquj9i_html_2ff6f0769e8e3c7e.png)

- Thực hiện kiểm tra Ram trên tất cả các client

![](RackMultipart20210515-4-aquj9i_html_362c825a7053aa65.png)

![](RackMultipart20210515-4-aquj9i_html_2598630fdc6b1423.png)

- Thực hiện reboot hàng loạt các node

![](RackMultipart20210515-4-aquj9i_html_57eff294d729a2f8.png)

Trường hợp này host group sẽ chọn là ubuntu, tất các các node ubuntu sẽ thực hiện reboot. Nếu là all thì tất các các node sẽ đồng thời thực hiện reboot.

![](RackMultipart20210515-4-aquj9i_html_e239814b82988b6e.png)

- Thực hiện cài đặt gói ứng dụng hàng loạt

Ví dụ ta sẽ thực hiện cài đặt apache trên các máy có hệ điều hành Ubuntu:

![](RackMultipart20210515-4-aquj9i_html_d1d601945d214f88.png)

Và kết quả:

![](RackMultipart20210515-4-aquj9i_html_9d8020e984091eac.png)

- Sử dụng ad-hoc command để thu thập thông tin client.

![](RackMultipart20210515-4-aquj9i_html_adab42bfe9b9580b.png)

1. **Playbook trên Ansible**

- Playbook là file định dạng YAML chứa các mô tả chi tiết chỉ thị nhằm mục đích tự động hoá chúng trên server từ xa.
- Làm việc với các biến: {{package}} – biến mà sau này sử dụng trong các task (Task sẽ xác định một công việc đơn lẻ cần thực hiện) và biến này có thể mã hoá bằng ansible vault.

![](RackMultipart20210515-4-aquj9i_html_2190914f99adcef6.png)

- Sử dụng vòng lặp:

![](RackMultipart20210515-4-aquj9i_html_243329846ac0f9cd.png)

- Sử dụng các điều kiện: Các điều kiện được sử dụng để quyết định xem liệu task đó có được thực thi hay không

![](RackMultipart20210515-4-aquj9i_html_bb17ae7a45f50b4e.png)

- Làm việc với các Template: các teamplate được sử dụng để thiết lập các tệp cấu hình cho phép sử dụng các biến và các tính năng khác làm cho tệp trở nên linh hoạt hơn.
-

![](RackMultipart20210515-4-aquj9i_html_7cbd90840c6265e7.png)

![](RackMultipart20210515-4-aquj9i_html_1c3dd5e9c1567bb7.png)

Khi đó file HTML sẽ có kết quả:

Heyyy, I&#39;m Hong thai

- Khai báo kích hoạt các Handler

- Các handler được sử dụng để kích hoạt một trạng thái nào đó của service như restart hay stop.
- Các handler chỉ được thực thi khi được kích hoạt bởi một chỉ thị notify bên trong một task.

![](RackMultipart20210515-4-aquj9i_html_9408c1d9961c74ce.png)

- Để thực thi một playbook

![](RackMultipart20210515-4-aquj9i_html_b3b89e58783c1379.png)

![](RackMultipart20210515-4-aquj9i_html_fbfbdbd9a7406e1d.png)

That all basic about the playbook :P

1. **Ansible inventory**

- Một file INI chứa các thông tin về các server từ xa mà bạn quản lý. Examble:

![](RackMultipart20210515-4-aquj9i_html_64e335371a16c972.png)

- Cấu trúc thư mục:

![](RackMultipart20210515-4-aquj9i_html_7b34a9c5162156f.png)

1. **Ansible Vault:**

- Có thể mã hóa bất kỳ tệp dữ liệu có cấu trúc nào được Ansible sử dụng. Điều này có thể bao gồm các biến group\_vars/ hoặc host\_vars/ inventory, các biến được tải bởi include\_vars hoặc vars\_files hoặc các tệp biến được truyền trên dòng lệnh ansible-playbook với -e @ file.yml hoặc -e @ file.json.
- Vì tác vụ Ansible, trình xử lý và các đối tượng khác là dữ liệu nên chúng cũng có thể được mã hóa bằng vault. Nếu bạn không muốn tiết lộ những biến bạn đang sử dụng, bạn có thể giữ một tệp tác vụ riêng lẻ được mã hóa hoàn toàn.
- Để sử dụng vautl:

![](RackMultipart20210515-4-aquj9i_html_b40cc1cabb57f296.png)

- Create: dùng để tạo file và dùng để encrypt bởi vault.
- Encrypt: dùng để mã hoá file sử dụng vault
- Decrypt: dùng để giải mã file sử dụng vault password.
- Edit: mở và giải mã tệp vaulted hiện có trong trình chỉnh sửa, tệp này sẽ được mã hóa lại khi đóng.
- View: Mở file decrypt với file vault đã được mã hoá.
- Encrypt\_string: được sử dụng để mã hoá một chuỗi bí mật trong playbook và được mã hoá bằng vault.
- Rekey: Dùng để thay đổi khoá vault bằng một đoạn mã mới.

1. **Ưu điểm và nhược điểm khi sử dụng Ansible.**

1. **Ưu điểm của ansible:**

- Đơn giản để làm quen với ansible.

- Đề cập nhiều nhất trong số các ưu điểm của Ansible là tính đơn giản của nó. Sự đơn giản không chỉ dành cho các chuyên gia mà còn cho cả những người mới bắt đầu. Nó rất dễ học và do đó, người dùng có thể học cách sử dụng Ansible một cách nhanh chóng cùng với năng suất tốt hơn. Ansible nhận được sự hỗ trợ của tài liệu toàn diện và dễ hiểu.
- Do đó, bạn có thể học logic của các hoạt động Ansible và quy trình làm việc trong một khoảng thời gian giới hạn. Việc thiếu hệ thống phụ thuộc có thể ngụ ý rằng các tác vụ Ansible thực thi tuần tự và dừng lại khi xác định lỗi. Do đó, việc khắc phục sự cố trở nên dễ dàng hơn rất nhiều, ngay cả trong giai đoạn đầu của việc tìm hiểu về ansible.

- Sử dụng ngôn ngữ là python khiến cho các tác vụ trở nên đơn giản và dễ hiểu.

- Một trong những ưu điểm nổi bật của Ansible cũng liên quan đến ngôn ngữ mà nó được viết. Python là một ngôn ngữ con người có thể đọc được và dùng làm nền tảng cho Ansible. Nó cung cấp các phương tiện tốt hơn để thiết lập và chạy Ansible do sự hiện diện của các thư viện Python trên phần lớn các bản phân phối Linux theo mặc định.
- Python là một giải pháp thay thế lý tưởng cho các tác vụ quản trị và viết kịch bản, ngụ ý rằng sự phổ biến cao hơn giữa các kỹ sư và quản trị viên hệ thống. Một khía cạnh thú vị khác của Ansible là cơ sở của các mô-đun Ansible có thể cải thiện chức năng của nó. Các mô-đun Ansible có thể được viết bằng bất kỳ ngôn ngữ nào. Tuy nhiên, mối quan tâm quan trọng, trong trường hợp này, là mô-đun phải trả về dữ liệu ở định dạng JSON.

- Không phụ thuộc vào các Agents ( Các nodes )

- Sự bổ sung quan trọng tiếp theo trong số các lợi ích của Ansible đề cập đến tính chất không tác nhân của nó. Ansible quản lý tất cả các thông tin liên lạc giữa tác nhân chủ thông qua mô-đun SSH Chuẩn hoặc Paramiko. Do đó, Ansible không yêu cầu bất kỳ hình thức đại lý nào được cài đặt trên các hệ thống từ xa để đảm bảo quản lý. Kết quả là, chi phí bảo trì và sự suy giảm hiệu suất giảm đáng kể bởi lợi nhuận rất lớn với Ansible.

- Được viết bằng YAML

- Việc sử dụng Playbooks trong Ansible cũng là một lý do khác tạo nên những lợi thế lớn của Ansible. Playbook là các tệp cấu hình Ansible và ngôn ngữ để viết chúng là YAML. Trong trường hợp này, yếu tố thú vị là YAML là một giải pháp thay thế tốt hơn cho việc quản lý cấu hình và tự động hóa.

1. **Nhược điểm của Ansible**

- Hạn chế về giao diện người dùng.

- Mục đầu tiên trong các nhược điểm của Ansible là giao diện người dùng thô. Có thể có sự xung đột trong kết quả try vấn giữa GUI và các dòng lệnh.

- Thiếu sót về &quot;Khái niệm trạng thái&quot;

- Một đề cập nổi bật khác trong số những nhược điểm của Ansible là thiếu bất kỳ khái niệm nào về trạng thái. Ansible không có bất kỳ khái niệm nào về trạng thái giống như các công cụ tự động hóa khác như Puppet. Ansible không theo dõi các phần phụ thuộc và chỉ thực hiện các tác vụ tuần tự và dừng khi các tác vụ kết thúc, không thành công hoặc bất kỳ lỗi nào xảy ra.
- Những đặc điểm này không lý tưởng cho những người dùng muốn công cụ tự động hóa duy trì một danh mục chi tiết để đặt hàng. Danh mục có thể giúp đạt được trạng thái cụ thể mà không bị ảnh hưởng bởi những thay đổi trong điều kiện môi trường. Tuy nhiên, Ansible thiếu nó và thể hiện một bất lợi đáng gờm.

- Windows là hạn chế của Ansible.

- Điểm nổi bật tiếp theo trong số các nhược điểm của Ansible là hỗ trợ Windows được xây dựng một nửa không toàn vẹn,
- Việc hỗ trợ hạn chế cho Windows trong Ansible là một trong những trở ngại đáng kể với công cụ tự động hóa và quản lý cấu hình.

- Ansible và sự non trẻ:

- Ansible không có kinh nghiệm làm việc chính thức với các doanh nghiệp lớn như các đối thủ cạnh tranh của nó, chẳng hạn như Puppet and Chef.

- Ansible còn đang khá mới so với các đối thủ.

- Cuối cùng, bạn có thể lưu ý một trong những mục phổ biến nhất trong các ưu và nhược điểm của Ansible là một bước lùi nổi bật của Ansible. Ansible là sản phẩm mới trên thị trường, không giống như các đối thủ cạnh tranh nổi tiếng của nó. Kết quả là, nó không có một nhà phát triển hoặc cộng đồng người dùng lớn. Hơn nữa, sự hiện diện mới của Ansible trên thị trường ngụ ý khả năng xảy ra các lỗi chưa được phát hiện, các vấn đề phần mềm và các tình huống cạnh tranh.
