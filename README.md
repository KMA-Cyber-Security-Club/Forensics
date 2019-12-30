~Author:Dangkhai~
***<h2>0.Steganography</h2>***

  `[+] Là gì:` Là kỹ thuật giấu các thông điệp,tin nhắn, hình ảnh,bla bla dưới dạng một thông điệp,hình ảnh ,bla bla khác. Nhằm mục đích chỉ dành cho những người có kỹ năng, mật khẩu,... xem được.</br>
  `[+]Link tham khảo:` [Tại đây](https://whitehat.vn/threads/forensic-6-steganography.2409/) </br>
     <h3>Các mức độ:</h3>
        <h4>Cơ bản:</h4>
                - Dùng các tool Stegsolve,binwalk,exiftool,strings,steghide,audacity,sonic visualiser,wireshark... để xem các thông điệp đơn giản,không dùng các cách encrypt hoặc kỹ thuật steg khác để ẩn thông tin.</br>
   > Cần nắm rõ, thành thạo xử dụng các công cụ, đặc biệt hiểu rõ đặc điểm khi nào dùng tool a hoặc tool b.</br>
   
 <h4>Nâng cao:</h4>
 - Thông điệp được ẩn bằng các cách phức tạp hơn.</br>
                
 `[+]ví dụ:`</br>
          I.Dùng các mã màu rgb để tương đương với mã char ẩn thông điệp,crypto để mã hóa thông điệp 2 x n lớp. </br>
          II.Thay đổi signature hình ảnh hoặc thay đổi tất cả các byte hex của tệp, xor với 1 key nào đó. </br>
          III... </br>
   > Cần hiểu rõ các khái niệm ví dụ, RGB, decode cơ bản, khóa GPB,header signature,... </br>
   > Thông thạo một số ngôn ngữ lập trình, coding giải quyết các vấn đề tìm thông điệp khi các công cụ cơ bản không áp dụng được. </br>
  
 `[+] Các solution:`
* https://medium.com/@FourOctets/ctf-tidbits-part-1-steganography-ea76cc526b40
* https://github.com/shiltemann/CTF-writeups-public/blob/master/IceCTF-2018/writeup.md
* https://medium.com/@kamransaifullah786/hsctf-6-forensics-challenges-solutions-162da84549db
* https://shankaraman.wordpress.com/category/ctf/stegano/
* https://dominicbreuker.com/post/stego_book_of_secrets/ </br>

> Đọc 3 page đầu google về từ khóa này!</br>
   
***<h2>1.Image (ổ đĩa)</h2>***
  `[+] Là gì:` Giống như 1 phần ổ đĩa, và bạn có nhiệm vụ khai thác những dữ liệu bên trong . </br>
  `[+] Cách xử lý:`Khi được cung cấp file ổ đĩa, cần mount(gắn) file ổ đĩa như ổ đĩa thông thường trong máy tính, dùng các công cụ như autopsy,FTK,.. để recover ỗ đĩa. </br>
  `[+] Các solution:`
  * https://www.jaiminton.com/Defcon/DFIR-2019/
  * https://codisec.com/whitehat11-wyginwys/
  * http://periciadigitaldf.blogspot.com/ </br>
   > Đọc 3 page đầu google về từ khóa này!</br>
   
***<h2>2.Network Forensic</h2>***
  `[+] Là gì:` Khai thác các thông tin được truy xuất qua mạng, các tập tin phân tích thường là các tập tin pcap đã snap bằng wireshark,tshark,... </br>
  `[+] Cần:` Hiểu về các kiến thức network: Protocol,method,tcp,ip,...,sử dụng thành thạo một số công cụ như wireshark,tshark,... </br>
  <h3>Các mức độ:</h3>
  <h4>Cơ bản:</h4>
      - Các dạng bài sử dụng giao thức không mã hóa http để trao đổi, chúng ta cần follow các gói tin đó để tìm ra thông điệp, hoặc các thông điệp được ẩn giấu mà không dùng kĩ thuật gì cao siêu.</br>
  <h4>Nâng cao:</h4>
      - Sử dụng giao thức mã hóa https,tcp,dhcp,... Các gói tin truyền tải đều có thể chứa thông điệp, cần follow kĩ,nâng cao hơn là các dạng bài dùng các khóa công khai,khóa bí mật xyz đễ giải mã các thông điệp bị mã hóa. </br>
      
   `[+] Các solution:`
  * https://infamoussyn.wordpress.com/2013/07/16/cysca-13-network-forensic-question-writeup/
  * https://leigh-annegalloway.com/44con-ctf-writeup/
  * https://github.com/ctfs/write-ups-2016/tree/master/su-ctf-2016/forensics/network-forensics-200/ </br>
   > Đọc 3 page đầu google về từ khóa này!</br>
   
***<h2>3.Memory Forensic</h2>***
  `[+] Là gì:` Là sử dụng kiến trúc quản lý bộ nhớ trong máy tính để ánh xạ, trích xuất các tập tin đang thực thi và đang lưu tạm trong bộ nhớ, các bộ nhớ RAM được trích xuất dưới dạng image.</br>
  `[+]: Cần:` Hiểu về kiến trúc máy tính,các hàm system, cách thức máy tính hoạt động...</br>
  `[+]: Các dạng hay gặp:`</br>
      - Thông điệp được ẩn trong các tệp được lưu trong máy... </br>
      - Thông điệp được ẩn trên các truy cập mạng của nạn nhân... </br>
      - Thông được được ẩn trong các tiến trình đã dùng của nạn nhân... <br>
      -...</br>
   `[+] Các solution:`
  * https://malware.news/t/grehack-2017-write-up-forensic-400/17085
  * https://medium.com/hackstreetboys/hsb-presents-otterctf-2018-memory-forensics-write-up-c3b9e372c36c
  * https://www.rootusers.com/google-ctf-2016-forensic-for1-write-up/
  * https://www.rootusers.com/trend-micro-ctf-2017-forensic-200-write/
  * https://minhtt159.wordpress.com/2018/01/30/acebear-ctf-2018-misc-forensics-write-up/
  * https://rawsec.ml/en/seccon-2016-100-forensics-memory-analysis/
  * https://medium.com/@melanijan93/write-up-memory-forensics-in-the-def-con-dfir-ctf-c2b50ed62c6b </br>
   > Đọc 5 page đầu google về từ khóa này!</br>
    
---</br>
 `~[*]Kết :~ 
 -Mảng này không phãi chỉ cần đọc nhiều mà cần làm nhiều để rèn luyện. Mảng này cần khá nhiều kiến thức kinh nghiệm để biết rằng vói bài này thì cần dùng cách nào để forensics.
 -Con đường tốt nhất để hiểu: Hãy tìm hiểu tại sao người ra đề tạo ra được file ẩn thông điệp đó, hoặc tự ra đề cho chính bẩn thân.`</br>
:smile:<h1>---CẢM ƠN - CHÚC MAY MẮN ---<h1>:smile:
