# Laravel Documentation

## The Basics

### Requests

**Truy cập đối tượng Request**

-   Khai báo và sử dụng đối tượng HTTP request:
    ```php
    use Illuminate\Http\Request
    function foo(Request $request){
        $url = $request->url();
    }
    ```
-   Thao tác với đối tượng HTTP request:
    -   Lấy ra đường dẫn của request: `$request->path()` (ví dụ `path(http://domain.com/foo/bar) -> foo/bar`)
    -   Kiểm tra đường dẫn khớp với một xâu: `$request->is('admin/*')`
    -   Lấy ra url của request: `$request->url()` (không kể xâu truy vấn) và `$request->fullUrl()` (kể cả câu truy vấn)
    -   Lấy phương thức của request: `$request->method()`
    -   Kiểm tra phương thức: `$request->isMethod('post')`

**Xử lý Input của Request**

-   Theo mặc định, tất cả các request đều được chuẩn hóa bằng hai middleware `TrimStrings` và `ConvertEmptyStringsToNull` trước khi chuyển cho server xử lý, có thể thay đổi cài đặt này trong lớp `App\Http\Kernel`

-   Lấy về tất cả input:
    ```php
    $input = $request->all();
    ```

-   Lấy về một giá trị input nào đó: 
    ```php
    $name = $request->input('name', 'Sally'); (Sally là giá trị fallback khi request không chứa input `name`)
    ```

-   Lấy về một phần tử trong một input có dạng mảng: 
    ```php
    $name = $request->input('products.0.name');

    $names = $request->input('products.*.name');
    ```

-   Lấy về input nằm trong xâu truy vấn:
    ```php
    $query = $request->query();
    ```
-   Lấy một phần input:
    ```php
    $input = $request->only(['username', 'password']);
    $input = $request->except(['credit_card']);
    ```
-   Kiểm tra request có input nào đó hay không:
    ```php
    if ($request->has('name')) {
        //
    }
    ```
-   Kiểm tra một input nào đó có giá trị khác rỗng hay không:
    ```php
    if ($request->filled('name')) {
        //
    }
    ```

**Lưu trữ input để sử dụng lại trong các request sau**

-   Sử dụng cơ chế flashing
-   Người dùng không cần phải dùng tính năng này một cách thủ công với các thao tác validation

**Cookies**

-   Lấy về cookie từ request
    ```php
    $value = $request->cookie('name');
    ```
-   Trả về cookie trong một response
    ```php
    return response('Hello World')->cookie(
        'name', 'value',
        $minutes,   // Thời gian hiệu lực của cookie tính theo phút
        $path,      // Không gian hiệu lực của cookie, gán bằng '/' khiến cookie hiệu lực trên toàn bộ domain
        $domain,    // Subdomain mà cookie có hiệu lực (ví dụ subdomain w2.example.com trên domain example.com)
        $secure,    // Sử dụng giao thức HTTPS để chuyển cookie
        $httpOnly   // true nếu cookie chỉ được phép truy cập từ giao thức HTTP
    );
    ```
-   Tạo một đối tượng cookie
    ```php
    $cookie = cookie('name', 'value', $minutes);
    return response('Hello World')->cookie($cookie);
    ```

**Files**

-   Lấy về các files đã upload
    ```php
    $file = $request->file('photo');
    ```
-   Kiểm tra file đã được upload thành công chưa
    ```php
    if ($request->file('photo')->isValid()) {
        //
    }
    ```
-   Lấy về đường dẫn và phần mở rộng của file
    ```php
    $path = $request->file('photo')->path();
    $extension = $request->file('photo')->extension();
    ```
-   Lưu một file đã upload
    ```php
    $path = $request->file('photo')->store('images');
    $path = $request->file('photo')->storeAs('images', 'filename.jpg');
    ```

