# Laravel Documentation

## The Basics

### Controllers

**Giới thiệu**
-   Controller giúp gom nhóm các thao tác xử lý request liên quan với nhau trong các phương thức của nó, thay vì để chúng là các Closures function trong router

**Cơ bản về controllers**

-   Định nghĩa một controller sử dụng artisan command:

php artisan make:controller Name [-m|-r|-i]

-   Trong đó:
    -   `-m`: Tạo model tương ứng
    -   `-r`: Tạo resource controller (sẽ đề cập ở dưới)
    -   `-i`: Tạo invokable controller (chỉ xử lý một hành động duy nhất, có duy nhất phương thức `__invoke($id)`)

**Controller Middleware**

-   Ta cũng có thể gán middleware cho một route bên trong constructor của controller xử lý route đó:
    ```php
    class UserController extends Controller
    {
        /**
         * Instantiate a new controller instance.
         *
         * @return void
         */
        public function __construct()
        {
            // Chỉ ra route này dùng middleware 'auth'
            $this->middleware('auth');
            
            // Chỉ dùng middleware cho phương thức 'index'
            $this->middleware('log')->only('index');
            
            // Không dùng middleware cho phương thức 'store'
            $this->middleware('subscribed')->except('store');
        }
    }
    ```

-   Cũng có thể gán middleware theo cú pháp closure:
    ```php
    $this->middleware(function ($request, $next) {
        // ...

        return $next($request);
    });
    ```

**Resource Controllers**

-   Là mẫu controller dùng để quản lý các trang liên quan đến một loại "tài nguyên" nào đó của hệ thống (product, user, ...)
-   Định nghĩa ra các trang/phương thức quản lý trang cho loại tài nguyên này, bao gồm:
    -   index
    -   create
    -   store
    -   show
    -   edit
    -   update
    -   destroy
-   Tạo route đến các trang này:
    ```php
    Route::resource('route', 'ResourceController');
    ```
-   Chỉtạo route đến một vài trang: dùng cú pháp `->only([])` và `->except([])`

-   Đọc thêm: https://laravel.com/docs/5.7/controllers#resource-controllers



