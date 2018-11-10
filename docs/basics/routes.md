# Laravel Documentation

## The Basics

### Routing

**Cơ bản về Route**

-   Router nằm ở file routes/web.php

-   Cú pháp của một route:

    ```php
    Route::{method}('{route}', '{controller}@{method}');
    ```

-   Trong đó:
    -   `method`={get, post, put, patch, delete, options}: Phương thức truy cập đến route này
    -   `route`: Đường dẫn của page
    -   `controller`: Controller thực hiện xử lý request tới route này
    -   `method`: Phương thức trên controller xử lý request

-   Xử lý yêu cầu tới route qua nhiều method khác nhau:
    -   `match(['get', 'post'], ...)`: Phương thức là get hoặc post
    -   `any`: Bất kỳ phương thức nào

-   CSRF Protection: Phòng chống tấn công CSRF tới site qua các phương thức `POST`, `PUT` hoặc `DELETE`
    ```html
    <form method="POST" action="/profile">
        @csrf
        ...
    </form>
    ```

-   Chuyển hướng tới route khác:
    ```php
    Route::redirect('/here', '/there', 301);
    ```

-   Truyền tham số vào route:
    ```php
    Route::get('user/{id}', function ($id) {
        return 'User '.$id;
    });
    ```

-   Ràng buộc cho tham số của route:
    ```php
    Route::get('user/{id}', function ($id) {
        //
    })->where('id', '[0-9]+');

    ```
-   Ràng buộc toàn cục: Định nghĩa ràng buộc cho một pattern trong tất cả các route trong file `RouteServiceProvider`
    ```php
    public function boot()
    {
        Route::pattern('id', '[0-9]+'); //Tất cả các tham số {id} trong các routes đều phải là số

        parent::boot();
    }
    ```

**Đặt tên cho routes**

-   Đặt tên cho route:
    ```php
    Route::get('user/profile/{id}', 'UserProfileController@show')->name('profile');
    ```
-   Sử dụng route đã được đặt tên:
    -   Tạo URL từ route:
    ```php
    $url = route('profile', ['id'=>1]);
    ```
    -   Redirect đến route:
    ```php
    return redirect()->route('profile');
    ```
    
**Phân nhóm cho routes**

-   Định nghĩa nhóm các routes sử dụng chung một middleware:
    ```php
    Route::middleware(['first', 'second'])->group(function () {
        Route::get('/', function () {
            // Uses first & second Middleware
        });

        Route::get('user/profile', function () {
            // Uses first & second Middleware
        });
    });
    ```
-   Tương tự, định nghĩa các controllers trong cùng một namespace:
    ```php
    Route::namespace('Admin')->group(function () {
        // Controllers Within The "App\Http\Controllers\Admin" Namespace
    });
    ```
-   Định tuyến trong sub domain `account`:
    ```php
    Route::domain('{account}.myapp.com')->group(function () {
        Route::get('user/{id}', function ($account, $id) {
            //
        });
    });
    ```
-   Thêm tiền tố cho các route trong nhóm:
    ```php
    Route::prefix('admin')->group(function () {
        Route::get('users', function () {
            // Matches The "/admin/users" URL
        });
    });
    ```
-   Thêm tiền tố cho TÊN của route trong nhóm:
    ```php
    Route::name('admin.')->group(function () {
        Route::get('users', function () {
            // Route assigned name "admin.users"...
        })->name('users');
    });
    ```
**Gắn route với model**

-   Có thể yêu cầu Laravel trả về đối tượng model tương ứng khi yêu cầu được gửi tới một route với tham số là `id` của đối tượng:
    ```php
    Route::get('api/users/{user}', function (App\User $user) {
        return $user->email;
    });
    ```
-   Đọc thêm: https://laravel.com/docs/5.7/routing#route-model-binding

**Route dự phòng**

-   Định nghĩa route mặc định được sử dụng nếu như không có route nào match với địa chỉ URI trong request:
    ```php
    Route::fallback(function () {
        //
    });
    ```
**Tần suất truy cập tới route**

-   Hạn định số lần truy cập tối đa/số phút sử dụng middleware:
    ```php
    Route::middleware('auth:api', 'throttle:60,1')->group(function () {
        Route::get('/user', function () {
            // Tối đa 60 truy cập mỗi phút đến route /user
        });
    });
    ```
**Form Method Spoofing**

-   Thêm hỗ trợ `PUT`, `PATCH` hoặc `DELETE` cho html form:
    ```html
    <form action="/foo/bar" method="POST">
        @method('PUT')
        @csrf
    </form>
    ```
**Truy cập các thuộc tính của route hiện tại**

    ```php
    $route = Route::current();

    $name = Route::currentRouteName();

    $action = Route::currentRouteAction();
    ```


