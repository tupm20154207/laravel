# Laravel Documentation

## The Basics

### Middleware

**Giới thiệu**

-   Middleware có tác dụng lọc các HTTP request gửi tới ứng dụng
-   Các ví dụ:
    -   Kiểm tra xác thực người dùng, nếu người dùng chưa đăng nhập thì redirect tới trang đăng nhập, ngược lại thì cho phép request được xử lý
    -   CORS middlewares giúp thêm headers cho tất cả các gói tin HTTP responses
    -   Logging middleware giúp ghi log tất cả các request gửi tới hệ thống
    -   Phòng chống tấn công CSRF
-   Thư mục chứa middlewares: `app/HTTP/Middleware`

**Định nghĩa Middleware**

-   Tạo một middleware:
    ```php
    php artisan make:middleware CheckAge
    ```

-   Chặn các route có trường `age` nhỏ hơn 18:
    ```php
    <?php

    namespace App\Http\Middleware;

    use Closure;

    class CheckAge
    {
        public function handle($request, Closure $next)
        {
            if ($request->age >= 18) {
                return redirect('home');
            }

            return $next($request);
        }
    }
    ```
-   Middleware có thể được chạy trước hoặc sau khi request đã được xử lý:
    -   Trước:
        -   Xử lý trên đối tượng `$request`
        -   return `$next($request)`
    -   Sau:
        -   Lấy về  `$response=$next($request)`
        -   Xử lý trên `$response`
        -   Trả về `$response`

**Đăng ký middlewares cho routes**

-   Danh sách tên của middlewares trong file `app/Http/Kernel.php`:
    ```php
        protected $routeMiddleware = [
        'auth' => \App\Http\Middleware\Authenticate::class,
        'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
        'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
        'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
        'can' => \Illuminate\Auth\Middleware\Authorize::class,
        'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
        'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
        'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
        'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,
    ];
    ```
-   Gán middleware cho route:
    ```php
    Route::get('admin/profile', function () {
        //
    })->middleware('auth');
    ```
-   Nhóm các middlewares sử dụng cùng nhau trong file `app/Http/Kernel.php`:
```php
    protected $middlewareGroups = [
        'web' => [
            \App\Http\Middleware\EncryptCookies::class,
            \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
            \Illuminate\Session\Middleware\StartSession::class,
            \Illuminate\View\Middleware\ShareErrorsFromSession::class,
            \App\Http\Middleware\VerifyCsrfToken::class,
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
        ],

        'api' => [
            'throttle:60,1',
            'auth:api',
        ],
    ];
    ```
**Middlewares chứa tham số**

-   Định nghĩa:
    ```php
    ...
        public function handle($request, Closure $next, $param1, $param2)
        {
            // Do something
            return $next($request);
        }
    ```

-   Sử dụng:
    ```php
    Route::put('post/{id}', function ($id) {
        //
    })->middleware('role:param1, param2');
    ```

**Terminable Middlewares**

-   Là các middlewares thực hiện nhiệm vụ "dọn dẹp" sau khi response đã được chuẩn bị và sẵn sàng gửi cho người dùng
-   Ví dụ "session" middleware thực hiện ghi thông tin session ra database
    ```php
    ...
        public function handle($request, Closure $next, $param1, $param2){}
        public function terminate($request, $response){
            // Store the session data
        }
    ```


