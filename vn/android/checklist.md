# ANDROID TO DO LIST

##	1. Bắt đầu một task

1. Kiểm tra nội dung ticket (trên redmine, backlog....) 
2. Kiểm tra estimate phù hợp chưa 
3. Phản hổi lại Leader nếu có vấn đề 
4. Cập nhật lại trạng thái ticket sang	"In Progress" 
	  
##	2.	Tạo branch làm việc

1. [Clone sourcecode về máy tính](#clone-sourcecode-về-máy-tính) *Bỏ qua nếu đã làm* 
2. [Tạo branch từ branch master or develop](#tạo-branch) (... branch chính của dự án)
3. [Tên branch là số ticket của task](#tạo-branch)

##3. Thực hiện task

1. Tuân thủ [Android coding standards](../README.md#Android)
2. Không nghiên cứu một vấn đề quá 2h. Thông báo cần hỗ trợ ngay
3. Thông báo ngay vấn đề mình gặp phải sớm sớm
4. Tại môi trường local(trên máy lập trình viên), tuyệt đối không được thay đổi code khi ở branch master。Nhất định phải thao tác trên branch khởi tạo để làm task

##4. Hoàn thành task

1. Check lại code và loại bỏ những đoạn code debug không cần thiết
2. [Tạo commit cho ticket](#tạo-commit), nội dung của commit là `refs #[Số ticket] [Nội dung ticket]` （Ví dụ: `refs #1234 Sửa lỗi cache`）
3. [Đồng bộ hóa từ Central Repository về Forked Repository](#Đồng-bộ-hóa-từ-central-repository-về-forked-repository) là `upstream` và `origin`
4. [Rebase và đẩy sourcecode lên Forked Repository](##rebase-và-đẩy-sourcecode-lên-forked-repository)
5. Tại origin trên Github（Bitbucket）、từ branch `task/1234` đã được push lên hãy gửi pull-request đối với branch master(develop) của upstream.
6. Tự review lại code 
7. Hãy gửi link URL của trang pull-request cho reviewer trên chatwork để tiến hành review code
8. Trong lúc chờ review và phản hồi từ reviewer thì làm task mới
9. [Fix comment và đẩy lại sourcecode lên Forked Repository](#fix-comment-và-đẩy-lại-sourcecode-lên-forked-repository) và thực hiện lại bước 6

##4. Kết thúc task

1. Sau khi sourcecode được merged tiến hành điền thời gian đã thực hiện vào ticket và chuyển trạng thái ticket về resolved trên readme hoặc backlog ....
2. Thực hiện task mới


# EXPLANATION 

Tài liệu có tham khảo các nội dung từ [Git Flow](../git/flow.md), [Android Coding Standards](../README.md#Android) .

#### Clone sourcecode về máy tính

- Trên Github (Bitbucket), fork Central Repository về tài khoản của mình（repository ở tài khoản của mình sẽ được gọi là Forked Repository）。
- Clone (tạo bản sao) Forked Repository ở môi trường local。Tại thời điểm này, Forked Repository sẽ được tự động đăng ký dưới tên là `origin`。

    ```sh
    $ git clone [URL của Forked Repository]
    ```
    
- Truy cập vào thư mục đã được tạo ra sau khi clone, đăng ký Central Repository dưới tên `upstream`。

    ```sh
    $ cd [thư mục được tạo ra]
    $ git remote add upstream [URL của Central Repository]
    ```

#### Tạo branch

Ví dụ ticket có số 1234 -> `task/1234`

```sh
$ git branch # <--- Kiểm tra xem mình đang ở branch nào
// Nếu branch master là branch để develop trực tiếp thì
$ git checkout master # <--- Không cần thiết nếu đang ở trên branch master
$ git checkout -b task/1234 # <--- Tạo branch task/1234
```

#### Tạo commit

Nội dung của commit là refs #[Số ticket] [Nội dung ticket] （Ví dụ: refs #1234 Sửa lỗi cache）

```sh
$ git add . 
$ git commit -m "refs #1234 Sửa lỗi cache"
```

#### Đồng bộ hóa từ Central Repository về Forked Repository

Central Repository và Forked Repository sẽ được gọi lần lượt là `upstream` và `origin`。

Đồng bộ hóa branch master(develop) tại local với upstream。

```sh
$ git checkout master
$ git pull upstream master
```
    
#### Rebase và đẩy sourcecode lên Forked Repository    

Sau khi đồng bộ hóa từ Central Repository về Forked Repository, ta tiến hành rebase với branch master(develop).

```sh
$ git checkout task/1234   <-- Chuyển về branch đang làm việc
$ git rebase master <-- Tiến hành rebase, có thể là develop
```

#### Khi xảy ra conflict trong quá trình rebase

Khi xảy ra conflict trong quá trình rebase, sẽ có hiển thị như dưới đây (tại thời điểm này sẽ bị tự động chuyển về một branch vô danh)

```sh
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: refs #1234 Sửa lỗi cache
Using index info to reconstruct a base tree...
Falling back to patching base and 3-way merge...
Auto-merging path/to/conflicting/file
CONFLICT (add/add): Merge conflict in path/to/conflicting/file
Failed to merge in the changes.
Patch failed at 0001 refs #1234 Sửa lỗi cache
The copy of the patch that failed is found in:
    /path/to/working/dir/.git/rebase-apply/patch

When you have resolved this problem, run "git rebase --continue".
If you prefer to skip this patch, run "git rebase --skip" instead.
To check out the original branch and stop rebasing, run "git rebase --abort".
```

- Hãy thực hiện fix lỗi conflict thủ công（những phần được bao bởi <<< và >>> ）。
Trong trường hợp muốn dừng việc rebase lại, hãy dùng lệnh `git rebase --abort`。

- Khi đã giải quyết được tất cả các conflict, tiếp tục thực hiện việc rebase bằng:

    ```sh
    $ git add .
    $ git rebase --continue
    ```

##### Đẩy sourcecode lên Forked Repository

```sh
$ git push origin task/1234
```

##### Fix comment và đẩy lại sourcecode lên Forked Repository

```sh
$ git add . <-- Add lại code vừa chỉnh sửa
$ git commit --amend
$ git push origin task/1234 -f
```