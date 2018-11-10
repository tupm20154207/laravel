# Laravel Documentation

## The Basics

### Session

**Giới thiệu**

-   Cấu hình session
    -   File cấu hình nằm ở địa chỉ `config/session.php`
    -   Danh sách session drivers được hỗ trợ:
        -   `file(default)`: Lưu session trong file `storage/framework/sessions`
        -   `cookie`: Lưu session trong cookie được mã hóa
        -   `database`: Lưu session trong CSDL quan hệ
        -   `memcached/redis`
        -   `array`: Lưu session bằng một PHP array trong bộ nhớ
    -   Tạo ra session database nếu dùng database driver:
    ```php
    Schema::create('sessions', function ($table) {
        $table->string('id')->unique();
        $table->unsignedInteger('user_id')->nullable();
        $table->string('ip_address', 45)->nullable();
        $table->text('user_agent')->nullable();
        $table->text('payload');
        $table->integer('last_activity');
    });
    ```

    ```bash
    php artisan session:table
    php artisan migrate
    ```
-   Sử dụng Session
