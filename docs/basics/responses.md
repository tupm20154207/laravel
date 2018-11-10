# Laravel Documentation

## The Basics

### Responses

**Tạo một Response**

-   Tạo một response với tham số là view và mã trạng thái
    ```php
    $response = response('Hello World', 200)
    ```
-   Gán header cho responses
    ```php
    $response->withHeaders([
        'Content-Type' => $type,
        'X-Header-One' => 'Header Value',
        'X-Header-Two' => 'Header Value',
    ]);
    ```
-   Gán cookies cho responses
    ```php
    $response->cookie('name', 'value', $minutes);
    ```

**Chuyển hướng**

-   Chuyển hướng đến một trang khác:
    ```php
    Route::get('dashboard', function () {
        return redirect('home/dashboard');
    });
    ```
-   Chuyển hướng đến trang trước đó:
    ```php
    Route::post('user/profile', function () {
        // Validate the request...
        return back()->withInput();
    });
    ```
-   Chuyển hướng tới một route đã được đặt tên:
    ```php
    return redirect()->route('login');
    ```
-   Chuyển hướng kèm theo tham số:
    ```php
    // For a route with the following URI: profile/{id}
    return redirect()->route('profile', ['id' => 1]);
    ```
-   Hoặc thay vì truyền tham số `id`, ta có thể truyền chính model luôn:
    ```php
    return redirect()->route('profile', [$user]);
    ```
-   Chuyển hướng cho một phương thức khác trong controller xử lý:
    ```php
    return redirect()->action(
        'UserController@profile', ['id' => 1]
    );
    ```
-   Chuyển hướng tới một địa chỉ nằm ngoài site:
    ```php
    return redirect()->away('https://www.google.com')
    ```
**Các loại Response khác**

-   JSON Responses
    ```php
    return response()->json([
        'name' => 'Abigail',
        'state' => 'CA'
    ]);
    ```
-   File Downloads: Buộc browser phải tải về một file nào đó (chú ý tên file phải tuân theo chuẩn ascii)
    ```php
    return response()->download($pathToFile);
    return response()->download($pathToFile, $name, $headers);
    return response()->download($pathToFile)->deleteFileAfterSend();
    ```

