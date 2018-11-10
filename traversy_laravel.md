# Laravel From Scratch

## Part5: Models and Database Migration

### Model and Migration

-   Tạo Model:

        php artisan make:model Post -m

-   Thêm các trường vào model vừa tạo: 
    -   Vào file `database/migrations/2018_xx_xx_create_posts_table.php`
    -   Sửa hàm `up()`, thêm các trường khác vào nó (chú ý về mối quan hệ giữa kiểu dữ liệu sql và phương thức migrate tương ứng)

-   Sửa thông tin cho database:
    -   Vào file `.env`
    -   Sửa các biến `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`
    -   Kiểm tra username và password trong file `/opt/lampp/phpmyadmin/config.inc.php`

-   Sửa lỗi kích thước xâu khi migrate models:
    -   Vào file: `Providers/AppServiceProvider.php`
    -   Thêm: `use Illuminate\Support\Facades\Schema;`
    -   Thêm vào hàm `boot()`: `Schema::defaultStringLength(191);`

-   Migrate Model (kiểm tra kết quả trong csdl):

        php artisan migrate

-   Có thể dùng tinker để cập nhật cơ sở dữ liệu:

        php artisan tinker

### Create A Resource Controller

-   Đây là loại controller giúp quản lý các thao tác thông thường liên quan đến một trang nhất định

-   Tạo ra một resource controller:

        php artisan make:controller PostsController --resource

-   Các phương thức có sẵn của resource controller:
    -   `index()`: Hiển thị trang index
    -   `create()`: Hiển thị form để tạo ra một phần tử mới
    -   `store(Request)`: Thêm phần tử mới vào model
    -   `show(id)`: Hiển thị thông tin chi tiết một phần tử
    -   `edit(id)`: Hiển thị form chỉnh sửa một phần tử
    -   `update(Request, id)`: Cập nhật phần tử trong model
    -   `destroy(id)`: Hủy phần tử trong model

-   Tự động tạo route cho tất cả các trang trong resource page: Thêm vào file `routes/web.php`:

        Route::resource('posts', 'PostsController');

-   Xem danh sách tất cả các routes:

        php artisan route:list

## Part 6: Fetching Data With Eloquent

-   Nạp Model trong file controller:

        use App\Post;


