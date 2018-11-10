# Laravel Documentation

## The Basics

### URL Generation

**Cơ bản**

-   Tạo ra một đối tượng URL:
    ```php
    $url = url("/posts/{$post->id}");
    ```
-   Truy cập đến địa chỉ URL hiện tại:
    ```php
    // Get the current URL without the query string...
    echo url()->current();
    // Get the current URL including the query string...
    echo url()->full();
    // Get the full URL for the previous request...
    echo url()->previous();
    ```
**Cơ chế sinh URL bằng hàm băm**
-   Cách thức hoạt động:
    -   Người dùng gửi một truy vấn tới địa chỉ với các tham số query: ['id'->'1', 'foo'->'bar']
    -   Hệ thống băm các tham số query thành một địa chỉ URL và gán URL này cho trang đích
    -   Người dùng truy cập đến địa chỉ này để tới trang đích, các thông tin về query không bị lộ vì đã được băm

-   Tạo địa chỉ URL bằng hàm băm:
    ```php
    use Illuminate\Support\Facades\URL;
    return URL::signedRoute('unsubscribe', ['user' => 1]);
    ```
-   Kiểm tra tính hợp lệ của URL được băm:
    -   Thêm `ValidateSignature` vào danh sách middlewares:
    ```php
    protected $routeMiddleware = [
        'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
    ];
    ```
    -   Gán middleware cho route:
    ```php
    Route::post('/unsubscribe/{user}', function (Request $request) {
        // ...
    })->name('unsubscribe')->middleware('signed');
    ```

**URL từ Controller Action**

-   Sinh URL từ một phương thức trong controller:
    ```php
    $url = action('HomeController@index');
    ```
