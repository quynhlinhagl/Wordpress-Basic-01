# Wordpress Căn Bản 01
### I.Khái quát về Wordpress
**WordPress** là một hệ thống quản lý nội dung (Content Management System - CMS) mã nguồn mở, được viết bằng ngôn ngữ lập trình PHP và cơ sở dữ liệu MySQL.
**WordPress** được biết đến là một CMS miễn phí tuyệt vời, dễ sử dụng và phổ biến nhất trên thế giới.
**WordPress** là một nền tảng khả dụng và linh hoạt cho nhiều loại website. Từ việc dùng để viết blog, porfolio cá nhân đến các trang thương mại điện tử, cổng thông tin, diễn đàn thảo luận và những web tuyệt vời khác.


### II.Cấu tạo file nguồn (core file) của Wordpress
1. __Thư mục chính của WordPress__
- **wp-admin/**: Thư mục chứa tất cả các file cần thiết cho khu vực quản trị viên của WordPress, bao gồm các file quản lý giao diện và chức năng của bảng điều khiển.
-** wp-includes/**: Chứa các file và thư viện chính của WordPress để hệ thống hoạt động, bao gồm các chức năng, class, và các file cốt lõi như post.php, formatting.php, và plugin.php.
- **wp-content/**: Thư mục này chứa các nội dung người dùng có thể thay đổi hoặc mở rộng, như các theme, plugin, và uploads:
  + themes/: Lưu trữ các theme mà người dùng có thể chọn để thay đổi giao diện trang.
  + plugins/: Chứa các plugin giúp mở rộng tính năng cho trang web.
  + uploads/: Nơi lưu trữ các file người dùng tải lên, như hình ảnh và video

2. __Các file cốt lõi trong thư mục gốc__
- **wp-config.php**: File cấu hình chính của WordPress, chứa thông tin về cơ sở dữ liệu, các key bảo mật và các cấu hình khác.
- **.htaccess**: File cấu hình của máy chủ Apache, dùng để điều chỉnh cấu trúc đường dẫn tĩnh và các quy tắc bảo mật.
- **index.php**: File khởi động của WordPress, tải giao diện và nội dung trang.
- **wp-load.php**: Chuẩn bị môi trường WordPress và tải các file cần thiết.
- **wp-settings.php**: Cấu hình toàn bộ WordPress, bao gồm việc thiết lập các file cốt lõi và tải các theme, plugin.

3. __Các file đặc biệt khác__
- **xmlrpc.php**: Cho phép kết nối từ xa với WordPress, thường dùng cho các ứng dụng di động.
- **wp-cron.php**: Xử lý các tác vụ định kỳ, như kiểm tra cập nhật và các tác vụ tự động khác.
- **wp-login.php**: File xử lý quá trình đăng nhập của người dùng.
- **wp-signup.php** và **wp-activate.php**: Xử lý đăng ký và kích hoạt tài khoản, chủ yếu dùng cho multisite.

### III.Cách thức kết nối tới database.
Trong WordPress, kết nối đến cơ sở dữ liệu được thực hiện thông qua cấu hình trong tệp wp-config.php
``` bash
define( 'DB_NAME', 'database_name_here' );
define( 'DB_USER', 'username_here' );
define( 'DB_PASSWORD', 'password_here' );
define( 'DB_HOST', 'localhost' );
define( 'DB_CHARSET', 'utf8' );
define( 'DB_COLLATE', '' );
```
Trong đó:

* **DB_NAME**: Tên của cơ sở dữ liệu WordPress mà bạn muốn kết nối.
* **DB_USER**: Tên người dùng của cơ sở dữ liệu.
* **DB_PASSWORD**: Mật khẩu của người dùng cơ sở dữ liệu.
* **DB_HOST**: Địa chỉ máy chủ cơ sở dữ liệu. Nếu cơ sở dữ liệu nằm trên cùng máy với * * * WordPress, bạn có thể để là 'localhost'. Nếu không, hãy nhập địa chỉ máy chủ.
* **DB_CHARSET**: Bộ ký tự sử dụng cho cơ sở dữ liệu, thường là 'utf8'.
* **DB_COLLATE**: Phương thức đối chiếu (collation), thường để trống để sử dụng giá trị mặc định của MySQL.

### IV.Xử lý vòng lặp (loop) cho bài post cơ bản trong Wordpress.
1. __The Loop__
__The Loop__ là mã PHP được WordPress sử dụng để hiển thị bài đăng. Sử dụng The Loop, WordPress xử lý từng bài đăng để hiển thị trên trang hiện tại và định dạng bài đăng theo cách phù hợp với các tiêu chí được chỉ định trong thẻ The Loop. Bất kỳ mã HTML hoặc PHP nào trong The Loop sẽ được xử lý trên từng bài đăng.

Khi tài liệu WordPress nói rằng "Thẻ này phải nằm trong The Loop", chẳng hạn như đối với <strong><u>Template Tags</u></strong>  hoặc plugin cụ thể, thẻ sẽ được lặp lại cho mỗi bài đăng. Ví dụ, The Loop hiển thị thông tin sau theo mặc định cho mỗi bài đăng:
* **`Title (the_title())`**
* **`Time (the_time())`**
* **`Categories (the_category()).`**

2. __Sử dụng vòng lặp__
Vòng lặp này phải được đặt trong index.php của Theme và trong bất kỳ Template nào khác được sử dụng để hiển thị thông tin bài viết.
Vòng lặp bắt đầu ở đây:
```php
<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
```
Và kết thức ở đây:
```php
<?php endwhile; else : ?>
  <p><?php esc_html_e( 'Sorry, no posts matched your criteria.' ); ?></p>
<?php endif; ?>
```
Đây là cách sử dụng cú pháp thay thế của PHP cho các cấu trúc điều khiển và cũng có thể được diễn đạt như sau:
```php
<?php
  if ( have_posts() ) {
    while ( have_posts() ) {
      the_post();
      // Đăng nội dung ở đây
    }
  }
?>
```

3. __Ví dụ về vòng lặp__
Ví dụ này hiển thị từng bài đăng với Tiêu đề (được sử dụng làm liên kết đến Đường dẫn cố định của bài đăng), Thể loại và Nội dung. Nó cũng cho phép các bài đăng trong thể loại có ID thể loại '3' được định dạng khác nhau. Để thực hiện điều này, Thẻ mẫu in_category() được sử dụng.

```php
<!-- Start the Loop. -->
  <?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>

  <!-- Kiểm tra xem bài đăng hiện tại có nằm trong danh mục 3 không. -->
  <!-- Nếu có, hộp div sẽ được gán lớp CSS "post-cat-three". -->
  <!-- Nếu không, hộp div sẽ được gán lớp CSS "post". -->

  <?php if ( in_category( '3' ) ) : ?>
    <div class="post-cat-three">
  <?php else : ?>
    <div class="post">
  <?php endif; ?>

  <!-- Display the Title as a link to the Post's permalink. -->

  <h2><a href="<?php the_permalink(); ?>" rel="bookmark" title="Permanent Link to <?php the_title_attribute(); ?>"><?php the_title(); ?></a></h2>

  <!-- Hiển thị ngày tháng (định dạng 16 tháng 11 năm 2009) và liên kết đến các bài đăng khác của tác giả bài đăng này. -->

  <small><?php the_time('F jS, Y'); ?> by <?php the_author_posts_link(); ?></small>

  <!-- Hiển thị nội dung bài viết trong hộp div. -->

  <div class="entry">
    <?php the_content(); ?>
  </div>

  <!-- Hiển thị danh sách các danh mục bài đăng được phân tách bằng dấu phẩy. -->

  <p class="postmetadata"><?php _e( 'Posted in' ); ?> <?php the_category( ', ' ); ?></p>
  </div> <!-- closes the first div box -->

  <!-- Dừng vòng lặp (nhưng lưu ý "else:" - xem dòng tiếp theo). -->

  <?php endwhile; else : ?>

  <!-- "if" đầu tiên được kiểm tra để xem có bất kỳ Bài đăng nào để -->
  <!-- hiển thị. Phần "else" này cho biết phải làm gì nếu không có bài đăng nào. -->
  <p><?php esc_html_e( 'Sorry, no posts matched your criteria.' ); ?></p>

  <?php endif; ?>
```

### V.Cấu tạo Theme Wordpress - các file thiết yếu.
Trong WordPress, một theme cơ bản bao gồm các file thiết yếu để xác định giao diện và chức năng của trang web. Dưới đây là các file cơ bản và bắt buộc của một theme WordPress.

1. __style.css__
Đây là file bắt buộc cho mọi theme WordPress.
Chứa thông tin về theme (như tên theme, phiên bản, tác giả, mô tả) và định dạng giao diện.
Đặt ở thư mục gốc của theme.
Ví dụ tiêu đề trong style.css:
```css
/*
Theme Name: My Custom Theme
Theme URI: http://example.com
Author: Your Name
Author URI: http://example.com
Description: Mô tả ngắn về theme
Version: 1.0
License: GNU General Public License v2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html
Text Domain: mycustomtheme
*/
```
2. __index.php__
Đây là file bắt buộc và là file chính của theme.
Được sử dụng để hiển thị nội dung trang nếu không có template nào khác phù hợp (ví dụ: nếu không có home.php, page.php, single.php, thì WordPress sẽ dùng index.php).

3. __functions.php__
File này không bắt buộc, nhưng hầu như mọi theme đều có.
Cho phép bạn thêm các chức năng tùy chỉnh cho theme như thêm menu, widget, hỗ trợ hình ảnh nổi bật, tải script và stylesheet.
Là nơi bạn có thể thêm mã PHP tùy chỉnh cho theme.
Ví dụ trong functions.php:
```php
<?php
// Kích hoạt menu trong theme
function mytheme_register_menus() {
  register_nav_menus(
    array(
        'header-menu' => __( 'Header Menu' ),
        'footer-menu' => __( 'Footer Menu' )
    )
  );
}
add_action( 'init', 'mytheme_register_menus' );
```
4. __header.php__
Chứa phần đầu trang của theme, bao gồm các thẻ `<html>`, `<head>`, `<body>`, và header của trang.
Thường chứa mã để tải stylesheet, script và logo của trang web.
5. __footer.php__
Chứa phần chân trang của theme, bao gồm các thẻ đóng như `</body>` và `</html>`.
Thường là nơi hiển thị thông tin bản quyền, liên kết đến các trang mạng xã hội, và thêm các script ở cuối trang.
6. __sidebar.php__
Đây là file tùy chọn cho theme nhưng thường được sử dụng.
Chứa mã HTML cho thanh bên (sidebar) của trang, nơi hiển thị các widget như menu, liên kết, bài viết gần đây, v.v.
7. __single.php__
Template cho một bài viết đơn lẻ (single post).
Khi người dùng xem một bài viết cụ thể, WordPress sẽ sử dụng file này để hiển thị nội dung.
8. __page.php__
Template cho các trang đơn lẻ (page).
Khi người dùng xem một trang cụ thể, WordPress sẽ sử dụng file này để hiển thị nội dung của trang.
9. __archive.php__
Template để hiển thị các trang lưu trữ như lưu trữ theo chuyên mục, thẻ, tác giả, và ngày tháng.
Khi người dùng xem trang lưu trữ, WordPress sẽ sử dụng archive.php để hiển thị danh sách bài viết.
10. __search.php__
Template để hiển thị kết quả tìm kiếm.
Khi người dùng tìm kiếm trên trang, WordPress sẽ sử dụng search.php để hiển thị kết quả.
11. __404.php__
Template để hiển thị khi trang không được tìm thấy (lỗi 404).
Khi người dùng truy cập một trang không tồn tại, WordPress sẽ sử dụng 404.php.
12. __comments.php__
Template để hiển thị phần bình luận của bài viết hoặc trang.
Nếu theme hỗ trợ bình luận, comments.php sẽ hiển thị các bình luận và form bình luận.

Cấu trúc thư mục theme WordPress cơ bản:
```css
mytheme/
├── style.css
├── index.php
├── functions.php
├── header.php
├── footer.php
├── sidebar.php
├── single.php
├── page.php
├── archive.php
├── search.php
├── 404.php
└── comments.php
```

### VI.Nội dung hàm số `the_title()`, `get_the_title()` và sự khác nhau giữa chúng
Trong WordPress, cả hai hàm `the_title()` và `get_the_title()` đều được sử dụng để lấy tiêu đề của một bài viết, nhưng chúng có một số điểm khác biệt chính. Dưới đây là chi tiết về từng hàm và sự khác biệt giữa chúng.
1. **the_title()**
- Chức năng: Hàm `the_title()` sẽ hiển thị trực tiếp tiêu đề của bài viết.
- Sử dụng: Khi bạn muốn tiêu đề bài viết xuất hiện ngay lập tức ở vị trí bạn gọi hàm này.
- Trả về: Hàm này không trả về giá trị mà sẽ in (echo) tiêu đề trực tiếp ra màn hình.
Ví dụ:
```php
<?php the_title(); ?>
```
2. **get_the_title()**
- Chức năng: Hàm `get_the_title()` sẽ lấy tiêu đề của bài viết nhưng không hiển thị trực tiếp ra màn hình.
- Sử dụng: Khi bạn cần lấy tiêu đề để sử dụng trong một biến hoặc xử lý thêm trước khi hiển thị.
- Trả về: Hàm này sẽ trả về giá trị của tiêu đề (title) dưới dạng một chuỗi (string) để bạn có thể sử dụng lại hoặc tùy chỉnh trước khi hiển thị.
Ví dụ:
```php
<?php
$title = get_the_title();
echo '<h2>' . $title . '</h2>';
?>
```
**Sự khác biệt chính**
| Đặc điểm             | the_title()                                          | get_the_title()                                     |
|----------------------|------------------------------------------------------|-----------------------------------------------------|
| Chức năng            | In trực tiếp tiêu đề ra màn hình                     | Trả về tiêu đề dưới dạng chuỗi (string)             |
| Cách sử dụng         | Sử dụng khi muốn hiển thị tiêu đề ngay lập tức       | Sử dụng khi cần thao tác với tiêu đề trước khi hiển thị |
| Trả về giá trị       | Không, chỉ in (echo) trực tiếp                       | Có, trả về tiêu đề để sử dụng trong biến           |
| Ứng dụng             | Thường dùng trong các template để hiển thị nhanh chóng | Dùng khi cần kiểm soát hoặc tùy chỉnh trước khi hiển thị |

### VII.Nội dung hàm số `get_stylesheet_directory_uri()`, `get_stylesheet_directory()` và sự khác nhau giữa chúng
Trong WordPress, hai hàm `get_stylesheet_directory_uri()` và `get_stylesheet_directory()` được sử dụng để lấy đường dẫn đến thư mục theme con (child theme), nhưng có sự khác biệt về mục đích và định dạng kết quả như sau:
1. **`get_stylesheet_directory_uri()`**
- Chức năng: Hàm get_stylesheet_directory_uri() trả về URL đầy đủ đến thư mục theme con.
- Định dạng kết quả: Đường dẫn dưới dạng URL (ví dụ: https://example.com/wp-content/themes/ten-theme-con).
- Sử dụng: Thường được dùng khi cần liên kết đến các tệp CSS, JavaScript, hoặc các tệp khác qua URL.
- Kết quả: Hàm sẽ trả về đường dẫn URL đầy đủ đến thư mục theme con, cho phép sử dụng để liên kết đến tệp CSS trong theme con.
Ví dụ:
```php
<link rel="stylesheet" href="<?php echo get_stylesheet_directory_uri(); ?>/style.css">
```
2. **`get_stylesheet_directory()`**
- Chức năng: Hàm get_stylesheet_directory() trả về đường dẫn tuyệt đối (absolute path) đến thư mục theme con trên server.
- Định dạng kết quả: Đường dẫn dưới dạng hệ thống tập tin trên server (ví dụ: /var/www/html/wp-content/themes/ten-theme-con).
- Sử dụng: Thường được dùng khi cần thao tác với tệp tin trực tiếp trên server, ví dụ đọc hoặc ghi vào tệp.
- Kết quả: Hàm sẽ trả về đường dẫn tuyệt đối trên server để truy cập tệp custom-file.php trong theme con.
Ví dụ:
```php
$path_to_file = get_stylesheet_directory() . '/custom-file.php';
```
**Sự khác biệt chính**
| Đặc điểm              | get_stylesheet_directory_uri()                                       | get_stylesheet_directory()                                   |
|-----------------------|----------------------------------------------------------------------|-------------------------------------------------------------|
| **Chức năng**        | Trả về đường dẫn URL đến thư mục theme con                           | Trả về đường dẫn tuyệt đối trên server đến thư mục theme con |
| **Định dạng kết quả** | Đường dẫn URL (https://example.com/wp-content/themes/theme-con)      | Đường dẫn file hệ thống (/var/www/html/wp-content/themes/theme-con) |
| **Ứng dụng**         | Thường dùng để liên kết đến tệp qua URL                              | Thường dùng để truy cập tệp trên server                      |
