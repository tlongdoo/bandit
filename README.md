**Level 0** 

Command: ssh bandit0@bandit.labs.overthewire.org -p 2220

**Level 0 → Level 1**: 

Command: cat readme
*Đọc file Readme

Password : boJ9jbbUNNfktd78OOpsqOltutMc3MY1

**Level 1 → Level 2**
Command: cat ./-
*lệnh cat sẽ coi - là từ đồng nghĩa với stdin. Để tránh điều đó, cần cung cấp đường dẫn đầy đủ của tệp thay vì chỉ ghi tên tệp.

Password : CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9

**Level 2 → Level 3**
Command: cat 'spaces in this filename' 
         cat spaces\ in\ this\ filename
tên tệp có khoảng trống. 1 là ghi tên tệp trong ngoặc. 2 là trước mỗi khoảng trống dùng kí tự \ để hiểu sau đó là kí tự
Password : UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK

**Level 3 → Level 4**
Command: ls -a 
* -a : show ra toàn bộ file bao gồm cả file ẩn
Password : pIwrPrtPN36QITSp3EQaw936yaFoFgAB

**Level 4 → Level 5**
Command: file ./-file*
* lệnh file sẽ trả về định dang của các file để xem file nào ở đây là only human-readable
trong nhiều file.
Password : lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR

**Level 5 → Level 6** 
Command: find -readable ! -executable -size 1033c
tìm kiếm những file có thể đọc được(-readable), có kích thước 1033 bytes, 'c' ký hiệu ở đây là đơn vị bytes(-size 1033c), không thể thực thi (! -executable)
cat ./inhere/maybehere07/.file2
Password : P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU

**Level 6 → Level 7** 
Command: find / -user bandit7 -group bandit6 -size 33c
'/' để tìm kiếm từ thư mục gốc(root)
Password : z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S

**Level 7 → Level 8** 
Command: grep millionth data.txt
grep: Là lệnh tìm kiếm văn bản trong 1 tệp tin. Ở đây tìm kiếm 'millionth' trong file data.txt
Password : TESKZC0XvTetK0S9xNwm25STk5iWrBvP

**Level 8 → Level 9** 
Command: cat data.txt | sort | uniq -u
lênh sort sẽ sắp xếp file data theo thứ tự bảng chữ cái. Lệnh 'uniq -u' giữ lại chỉ những dòng không trùng lặp (unique). Tùy chọn -u chỉ hiển thị các dòng không lặp lại.
Password : EN632PlfYiZbn3PhVK3XOGSlNInNE00t

**Level 9 → Level 10**
Command: strings data.txt | grep =
lệnh 'strings' liệt kê ra toàn bộ xâu đọc được trong file data.txt, sau đó lệnh grep sẽ tìm kiếm tất cả đoạn strings có dấu =
Password : G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s

**Level 10 → Level 11**
Command: base64 -d data.txt
-d viết tắt cho deocde
Password : 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM

**Level 11 → Level 12**
rot13.com
Password : JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv

**Level 12 → Level 13**
Step 1. Tạo 1 đường riêng dưới /tmp, sau đó copy data.txt sang đường dẫn mới.
=> Chạy thẳng bị denied, tạo 1 đường dẫn riêng để dump file
Command: mkdir /tmp/tlongbandit
	 cp data.txt /tmp/tlongbandit/
Step 2. File ở đây đuôi .txt nhưng ở content file sẽ ở dạng hex. Do đó phải đổi về dạng binary(Để nguyên dạng hex có đổi đuôi cũng ko đổi định dạng)
Command: xxd -r data.txt > newdata.txt
Step 3. Sử dụng lệnh 'file newdata.txt' thấy được file ở dạng gzip. Nếu sử dụng gunzip ở đây sẽ xảy ra lỗi vì đuôi không đúng => Đổi đuôi về .tar.gz rồi gunzip, sẽ nhân được file 'newdata.tar'
Command: mv newdata.txt newdata.tar.gz
	 gunzip newdata.tar.gz
Step 4. File mới sẽ ở dạng bzip2. Đổi đuôi về '.bz2' rồi giải nén nhận được newdata
Command: mv mv newdata.tar newdata.bz2
	 gunzip bunzip2 newdata.bz2
Step 5. File mới sẽ ở dạng gzip. Đổi đuôi về 'tar.gz' rồi giải nén nhận được newdata.tar
Command: mv newdata newdata.tar.gz
	 gunzip newdata.tar.gz
Step 6. File mới ở dạng POSIX tar. Giải nén bằng lệnh 'tar -xf' nhận được file data5.bin
*-x: extract, -f: file
Command: tar -xf newdata.tar
Step 7. File data5.bin ở dạng POSIX tar. Giải nén bằng lệnh 'tar -xf' nhận được file data6.bin
Command: tar -xf data5.bin
Step 8. File data6.bin ở dạng bzip2. Đổi đuôi về '.bz2' rồi giải nén nhận được data6
Command: data6.bin data6.bz2
	 bunzip2 data6.bz2
Step 9. File data6 ở dạng POSIX tar. Giải nén bằng lệnh 'tar -xf' nhận được file data8.bin
Command: tar -xf data8.bin
Step 10. File data8.bin sẽ ở dạng gzip. Đổi đuôi về 'tar.gz' rồi giải nén nhận được data8.tar
Command: mv data8.bin data8.tar.gz
	 gunzip data8.tar.gz
Step 11. File data8.tar ở dạng ASCII text và đã có password
Command: cat data8.bin
Password : wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw

**Level 13 → Level 14**
Command: ssh -i sshkey.private bandit14@localhost -p 2220
* -i sshkey.private: Tùy chọn để chỉ định một tệp tin chứa khóa riêng tư (private key) để xác thực đối với kết nối SSH
* bandit14 : tên người dùng sẽ đăng nhập vào
* localhost: máy cá nhân
Password : fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq

**Level 14 → Level 15** 
Command: telnet localhost 30000
	 fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
kết nối đến cổng 30000, sau đó gửi pass của lv14 sẽ nhận được pass cho lv 15
*Sự khác nhau giữa Telnet và SSH:
Telnet thường sử dụng xác thực bằng mật khẩu, trong khi đó SSH nhiều phương thức xác thực hơn, bao gồm cả mật khẩu và public key, ...
=> Dữ liệu qua giao thức Telnet không được mã hoá, làm tăng nguy cơ bị đánh cắp thông tin. Ngược lại, SSH cung cấp mã hoá dữ liệu, sử dụng các phương thức mã hoá thông tin mạnh đảm bảo an toàn trong việc truyền nhận
Password : jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt

**Level 15 → Level 16**
Command : openssl s_client -connect localhost:30001
* openssl s_client: s_client là chương trình con của openSSL, được sử dụng để kết nối đến 1 máy chủ thông qua giao thức SSL/TLS.
Password : JQttfApK4SeyHwDlI9SXGR50qclOAil1

**Level 16 → Level 17**
Step 1: nmap -A -p 31000-32000 localhost 
* nmap :thực hiện lệnh quyét cổng
* -a : là option quét toàn diện, bao gồm cả HĐH, dịch vụ đang chạy,...
* -p 31000-32000: quét từ port 31000 đến 32000
* localhost : địa chỉ thực hiện quét
Sau khi chạy command, ta nhận ra port 31790 hiện thông báo '31790/tcp open  ssl/unknown', cổng này chạy dịch vụ không xác định(ssl/unknown)
Step 2: openssl s_client -connect localhost:31790
	cluFn7wTiGryunymYOu4RcffSxQluehd
Ở đây sẽ nhận được 1 cái RSA private key cho lv tiếp theo. Cop nó ra 1 file /tmp với tên key16.private
command: echo "{key}" > key16.private
Step 3: Giờ chạy lệnh ssh -i {key} bandit17@localhost thì gặp lỗi ' Unprotected private key file! ' vì chưa set quyền cho key nên ở đây key đã bị ignored.
Step 3 : Set quyền cho key 
Command: chmod 400 key16.private
* 400: 4(Read), 0(Write), 0(Execute)
Step 4 : ssh -i key16.private bandit17@localhost
Password: VwOSWtCA7lRKkTfbr2IDh6awj9RNZM5e

**Level 17 → Level 18**
Command: diff passwords.old passwords.new
* diff: Là lệnh để so sánh nội dung của hai tệp tin và hiển thị sự khác biệt
Kết quả sẽ hiển thị như sau:
' 42c42
< p6ggwdNHncnmCNxuAt0KtKVq185ZU7AW
---
> hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg'
Note kết quả:
'42': Dòng số 42 trong tệp tin passwords.old
'c' : (change) cho biết có sự thay đổi
'42': Dòng số 32 trong tệp tin passwords.new
Dòng 42 file old : p6ggwdNHncnmCNxuAt0KtKVq185ZU7AW
=> dòng 32 file new : hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg
Password: hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg

**Level 18 → Level 19**
Ở đây khi kết nối đến bandit18, lập tức kết nối sẽ bị đóng, có thể ở đây lệnh exit đã đượct thực thi. 
Command : ssh bandit18@bandit.labs.overthewire.org -p 2220 -t /bin/sh
* Tùy chọn -t trong lệnh ssh được sử dụng để force pseudo-tty allocation, tạo 1 terminal ảo
* /bin/sh là một shell thực thi có sẵn. Trong trường hợp này, /bin/sh được sử dụng làm shell
Password: awhqfNnAbc1naukrpqDYcF95h7HoMTrC

**Level 19 → Level 20**
Khi chạy lệnh file ta nhận thấy file bandit20-do có dạng setuid 
*Set user ID, nó được thực thi với quyền của ở lv20
Command : ./bandit20-do cat /etc/bandit_pass/bandit20
* Thực thi tệp tin bandit20-do sau đó đọc file password
Password: VxCazJaVykI6W36BkBU0mJTCM8rR95XT

**Level 20 → Level 21**
Step 1 : nmap localhost
Thực hiện quét các cổng xem cổng nào hiện đang open
Step 2 : echo "VxCazJaVykI6W36BkBU0mJTCM8rR95XT" | nc -l -p 60000
* Gửi chuỗi sử dụng Netcat để thiết lập kết nối mạng, sau đó lắng nghe kết nối ở cổng 60000 để nhận dữ liệu từ máy khác
Step 3 : ./suconnect 60000
* Sử dụng suconnect để kết nối đến cổng 60000, sau đó đọc dữ liệu đã được gửi, nếu matches password lv 20 sẽ trả về password lv 21
Password: NvEJF7oVjkddltPSrdKEFOllh9V1IBcq

**Level 21 → Level 22**
Step 1 : cat cronjob_bandit22
Theo như gợi ý ở, hãy di chuyển đến path /etc/cron.d và để ý thấy file cronjob_bandit22. Sau khi đọc file, di chuyển đến path /usr/bin/cronjob_bandit22.sh
Đọc file này và nhận thấy password sẽ nhằm trong file /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
Command: cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
Password: WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff

**Level 22 → Level 23**
Giống như lv trước, đọc file cronjob_bandit23
Command : echo I am user bandit23 | md5sum | cut -d ' ' -f 1
Nhận được tên file sau khi băm md5, di chuyển vào đọc file sẽ nhận được pass
Password: QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G

**Level 23 → Level 24**
Ở file shell script cronjob_bandit24, script ở đây sẽ thực hiện thực thi và xoá tât cả script trong /var/spool/bandit24/foo
Giống như những lv trước, nhưng lv này chúng ta phải tự tạo shell script sau đó copy vào trong /var/spool/bandit24/foo để cron chạy script của chúng ta đọc password rồi trả về kêt quả . 
Command: vi tlbd23.sh
* tạo file trong /tmp
Command:#!/bin/sh
	cat /etc/bandit_pass/bandit24 >> /tmp/tlbandit23/bandit24pass
* add script vào file tlbd23.sh
	chmod 777 tlbd23.sh
* cấp quyền ghi cho tlbd23.sh
	cp tlbd23.sh /var/spool/bandit24/foo
* sao chép tệp vào đường dẫn và chờ thực thi, sau đó sẽ gửi lại file bandit24pass chứa password
Password: VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar

**Level 24 → Level 25**
Lv này yêu cầu gửi Pass đến cổng 30002 và 1 mã pin 4 số. Ta sẽ dùng Brute-Force để check mã pin này
Script : 
#!/bin/bash

for i in {0000..9999}
do
	echo "VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar $i"
done
Command : ./tlbandit24.sh | nc localhost 30002 | grep -v "Wrong" 
* ./ thực thi tệp tlbandit24.sh
* netcat tạo 1 kết nối đến cổng 30002
* grep -v sẽ loại bỏ mọi dòng chứa 'Wrong'
Password: p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d

**Level 25 → Level 26**
Thử ssh đến lv 26 bằng private key chúng ta nhận thấy conncection bị closed
Step 1: Sau 1 vài thao tác tìm kiếm, chúng ta thấy được shell cho lv này không lưu ở  /bin/bash mà thay vào đó là /usr/bin/showtext
' #!/bin/sh
 more ~/text.txt
 exit 0'
* Chúng ta nhận thấy sẽ bị log out ngay khi chúng ta vừa log in do dòng code 'exit 0'
Step 2: Ở đây, giải pháp sẽ là thu nhỏ terminal xuống để connect hiện ra more để ta có thể sử dụng 'vim' để nhập lệnh (press 'v')
Command: :e /etc/bandit_pass/bandit26
* e để edit password file của bandit26, sau đó chúng ta nhận đc password vì ở đây chúng ta đã log vào lv26, khi chưa kịp log out thì chúng ta sẽ check password trước.
Password: c7GvcKlw9mC7aUQaPx7nwFstuAIBw1o1


**Level 26 → Level 27**
Giống lv 19, thực thi tệp bandit27-do để có quyền với lv27, sau đó lấy password
Command: ./bandit27-do cat /etc/bandit_pass/bandit27
Password: YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS

**Level 27 → Level 28**
Lv này yêu cầu clone git ở địa chỉ ssh://bandit27-git@localhost/home/bandit27-git/repo thông qua port 2220
Command: git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
* add port 2220 bằng -p sẽ lỗi, add bằng localhost:2220
Password: AVanL161y9rsbcJIsFHuw35rjaOM19nR

**Level 28 → Level 29**
Giống như lv trước, ta thực hiện clone git về, nhưng ở đây file README không cho password nữa, mà thay vào đó passwuord bị ẩn đi
Command : git log
* xem lịch sử commit
Command : git checkout f08b9cc63fa1a4602fb065257633c2dae6e5651b
* Sau khi thực hiện lệnh git trên, HEAD đã được chuyển đến commit mới và có thể xem password ở file README
Password: tQKvmcwNYcFS6vmPHIUSI3ShmsrQZK8S

**Level 29 → Level 30**
Giống lv trước, clone git đầu tiên
Nhưng ở lv này : <no passwords in production!>
Command: git branch -a
* Ở đây sẽ sử dụng lệnh branch để list toàn bộ nhánh, ngoài nhánh HEAD thì gồm 3 nhánh: dev, sploits-dev, master
git check từng nhánh sẽ thấy được password ở nhánh dev 
Password: xbhV3HpNGlTIdnjUrdAlPzc2L6y9EOnS

**Level 30 → Level 31**
Lv này thậm chí file README không có gì, các lệnh git commit và git brand cũng không cho thêm thông tin gì
Command : git tag
*Hiển thị danh sách các tag hiện tại trong repository, ở đây có 1 tag là 'secret'
	  git show secret
* Show ra thông tin chi tiết về tag, ở đây ta có được password
Password: OoffzGDlzhAlerFJ2cAiz1D41JW1Mhmt

**Level 31 → Level 32**
Clone git về ta được file README: 
" This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master "
File hướng dẫn ta cụ thể các bước, việc chúng ta cần làm là push file lên repository với tên key.txt, content là 'May I come in?', và nhánh là master
Command: echo "May I come in?" > key.txt
* tạo file key.txt với content 'May I come in?'
	 git add key.txt
* đưa tệp key vào hàng chờ, nhưng ở đây file bị ignore do file '.gitignore'. Sử dụng 'git add -f key.txt' để bypass.
	 git commit -m "upload"
* Tạo commit trên repository  với thông điệp "upload"
	 git push origin master
* Push từ repository local lên repository remote nhánh master như yêu cầu, ta nhận được password
Password: rmCBvG56y58BXzv98yZGdO7ATVL5dW8y

**Level 32 → Level 33**
Ở lv này, khi log in vào ta sẽ gặp 1 UPPERCASE SHELL và ghi nhập bất cứ lệnh nào nó sẽ đều bị viết hoa dẫn đến việc 'command not found'. 
Command : $0
* Uppercase shell có thể sẽ cung cấp các parameters để chuyển đổi command của ta thành uppercase. Thực thi $0 sẽ chạy lại shell mà không có các parameters.
Việc còn lại đơn giản, khi các command không bị uppercase, ta ở đây đã có đặc quyền của bandit33, chỉ cần xem mật khẩu 'cat /etc/bandit_pass/bandit33'
Passowrd: odHo63fHiFqcWWJG9rLiLDtPm45KzUKy

**Level 33 → Level 34**
Level cuối cùng, có 1 file README:
"Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you could play some of our other wargames.

If you have an idea for an awesome new level, please let us know!"
~ Chúc mừng phá đảo lv cuối! ~
-THE END-

