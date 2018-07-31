## 17 mẹo để sử dụng Composer hiệu quả

Mặc dù hầu hết những nhà phát triển về php đều biết cách sử dụng Composer,nhưng không phải tất cả họ biết cách sử dụng hiệu quả hoặc theo cách chuẩn nhất,Vì vậy, tôi đã quyết định tóm tắt những thứ quan trọng cho công việc hàng ngày của mình.

Triết lý của hầu hết các mẹo là "Chơi an toàn", có nghĩa là nếu có nhiều cách hơn để xử lý một cái gì đó, tôi sẽ sử dụng cách tiếp cận ít bị lỗi nhất.

## Mẹo 1: Đọc từ trang tài liệu
Đó là điều mà tôi thực sự nghĩ tới , Trang tài liệu rất tuyệt và chỉ mất một vài giờ để đọc nó  sẽ giúp bạn tiết kiệm đc 1 khoảng thời gian kha khá. Bạn sẽ rất ngạc nhiên về một vài thứ Composer có thể làm.

## Mẹo 2 : Hãy phân biệt sự khác nhau giữa 1 "project" và một "thư viện".
Đó là điều quan trọng phải biết,cho dù bạn tạo 1 project hay một thư viện.Mỗi loại sẽ yêu cầu những bài thực hành riêng biệt.
 
Thư viện là một gói có thể tái sử dụng , bạn có thể thêm nó như là một  phụ thuộc như là symfony/symfony, doctrine/orm or elasticsearch/elasticsearch.

Project thì thường là một ứng dụng,bị phụ thuộc vào một số thư viện.Thường thì nó không thể tái sử dụng (không có dự án nào khác yêu cầu nó như một phụ thuộc).Ví dụ điển hình là một trang web thương mại điện tử , hệ thống hỗ trợ khách hàng vv..

Tôi sẽ chỉ cách phân biệt giữa thư viện và một project ở mẹo dưới đây.

## Mẹo 3 : sử dụng các phiên bản phụ thuộc dành cho ứng dụng.
Nếu bạn khởi tạo một ứng dụng , bện nên sử dụng  phiên bản cụ thể nhất để định danh các phụ thuộc.Nếu bạn cần phân tích tệp YAML,bạn nên chỉ định các phụ thuộc như "symfony/yaml": "4.0.2".

Ngay cả khi thư tiện tuân theo Semantic Versioning , có thể sẽ có sự  phá vỡ các tương thích ở trong bản phụ hoặc bản vá.Ví dụ , nêu bạn đang sử dụng "symfony/symfony" : "^3.1" , có thể sẽ có vài thứ không được sử dụng nữa trong bản 3.2 và các bản test trong ứng dụng của bạn sẽ gặp lỗi.Hoặc một số các lỗi đã được sửa trong PHP_CodeSniffer và nó sẽ phát hiện các vấn đề về định dạng mới trong code của bạn, điều này có thể dẫn đến một bản code bị hỏng khi dựng lên.

Việc cập nhật các phụ thuộc nên được cân nhắc, không phải ngẫu nhiên. Một trong những lời khuyên dưới đây sẽ là rõ vấn đề này hơn.

Nghe thì có vẻ là hơi quá lên , nhưng nó sẽ ngăn các đồng nghiệp của bạn vô tình cập nhật tất cả các phụ thuộc khi thêm một thư viện mới vào dự án (mà bạn có thể bỏ lỡ trong khi xem xét code).

## Mẹo 4: Sử dụng phạm vi phiên bản cho các phụ thuộc thư viện
Nếu bạn khởi tạo một thư viện , bạn nên định nghĩa phạm vi phiên bản rộng nhất có thể.Nếu bạn tạo một thư viện sử dụng thư viện symfony/yaml
để phân tích YAML , bạn nên yêu cầu nó như sau:
"symfony/yaml": "^3.0 || ^4.0"
Nó có nghĩa là thư viện của bạn cần sử dụng symfony/yaml từ các phiên bản Symfony 3.x hoặc 4.x.Nó thực sự quan trọng , bởi vì hạn chế đó sẽ được chuyển vào ứng dụng từ thư viện của bạn.

Trong trường hợp có 2 thư viện với các yêu cầu xung đột nhau.Ví dụ một cái thì yêu cầu ~3.1.0 và cái khác yêu cầu ~3.2.0 thì việc cài đặt sẽ thất bại.

## Mẹo 5: Bạn nên commit composer.lock lên trên git trong ứng dụng
Nếu bạn tạo một project , bạn nên chắc chắn là sẽ commit composer.lock lên git . Điều này đảm bảo rằng tất cả mọi người - bạn, đồng nghiệp của bạn, máy chủ CI và máy chủ sản xuất của bạn - đang chạy ứng dụng với cùng các phiên bản phụ thuộc.

Thoạt nhìn, nó nghe có vẻ không cần thiết - bạn đã sử dụng một phiên bản cụ thể trong ràng buộc, như đã đề cập trong mẹo 3. Nhưng không, cũng có phụ thuộc trong các phụ thuộc của bạn không bị ràng buộc bởi các ràng buộc này (ví dụ: symfony / console phụ thuộc vào symfony / polyfill-mbstring). Vì vậy, nếu không commit composer.lock, bạn sẽ không nhận được cùng một tập hợp các phụ thuộc.

## Mẹo 6: Đẩy composer.lock vào trong .gitignore trong thư viện
Nếu bạn tạo một thư viện ( Giả sử gọi nó là acme/my-library) , bạn không nên commit tệp composer.lock. Nó không có bất kỳ ảnh hưởng nào đối với các dự án đang sử dụng thư viện của bạn.

Hãy tưởng tượng rằng acme/my-library sử dụng monolog / monolog như một sự phụ thuộc. Nếu bạn đã commit một composer.lock, mthì những ai phát triển acme/my-library sẽ sử dụng phiên bản cũ hơn của Monolog. Nhưng khi thư viện hoàn tất, và bạn sử dụng nó trong một dự án thực, một phiên bản mới hơn của Monolog có thể được cài đặt, và nó có thể không tương thích với thư viện. Nhưng bạn đã không nhận thấy nó trước đây, chình là bởi composer.lock!

Tốt nhất là đẩy composer.lock vào trong tệp .gitignore của bạn như thế thì bạn sẽ không lo lắng về vấn đề commit nhầm nữa.

Nếu bạn muốn chắc chắn rằng thư viện đã tương thích với các phiên bản phụ thuộc của nó , hãy đọc mẹo tiếp theo.

## Mẹo 7: Chạy Travis CI mà được dựng nên bởi các phiên bản khác nhau của các phụ thuộc

> Lời khuyên này chỉ áp dụng đối với các thư viện (bởi bạn sử dụng  các phiên bản cụ thể cho ứng dụng).

Nếu bạn đang xây dựng một thư viện mã nguồn mở, có thể bạn đang sử dụng Travis CI để chạy các bản build của nó.

Mặc định, Composer sẽ cài đặt các phiên bản mới nhất có thể của các phụ thuộc mà được khai báo trong `composer.json`. Nó có nghĩa rằng, đối với điều kiện phụ thuộc `^ 3.0 || ^ 4.0`, bản build sẽ luôn sử dụng phiên bản mới nhất của bản v4. Do bản 3.0 chưa bao giờ được kiểm thử, thư viện có thể sẽ không tương thích với nó và điều đó sẽ làm cho user của bạn thất vọng.

May mắn thay, Composer cung cấp một tính năng để cài đặt các phiên bản thấp nhất có thể của các phụ thuộc `--prefer-low` (nên được sử dụng với` --prefer-stable` để phòng ngừa việc cài đặt các phiên bản không ổn định).

File cấu hình `.travis.yml` được cập nhật có thể sẽ như sau:
    
    language: php
    
    php:
      - 7.1
      - 7.2
    
    env:
      matrix:
        - PREFER_LOWEST="--prefer-lowest --prefer-stable"
        - PREFER_LOWEST=""
    
    before_script:
      - composer update $PREFER_LOWEST
    
    script:
      - composer ci

Hãy xem nó tại [my mhujer/fio-api-php library](https://github.com/mhujer/fio-
api-php / blob / master / .travis.yml) và [the build matrix on Travis
CI](https://travis-ci.org/mhujer/fio-api-php)

Mặc dù giải pháp này sẽ bắt được hầu hết các bản không tương thích, hãy nhớ rằng có nhiều cách kết hợp các phụ thuộc giữa phiên bản thấp nhất và mới nhất. Và chúng cũng có thể không tương thích với nhau.


## Mẹo 8: Sắp xếp các package trong require và require-dev theo tên

Một thói quen tốt là sắp xếp các package trong `require` và` require-dev` theo tên. Điều này có thể ngăn ngừa những merge conflict không cần thiết khi rebase một nhánh. Bởi nếu bạn đã thêm một package vào cuối danh sách trong hai branch,
sẽ có luôn có xung đột khi merge.

Đây là một công việc nhàm chán khi phải làm thủ công, vì vậy tốt nhất là [cấu hình nó] (https://getcomposer.org/doc/06-config.md#sort-packages) trong
`composer.json`:

    {
    ...
        "config": {
            "sort-packages": true
        },
    ...
    }
    
Lần tới, nếu bạn `require` một package mới, nó sẽ được thêm vào vị trí thích hợp (và không phải ở cuối).

## Mẹo 9: Đừng merge `composer.lock` khi đang rebase hoặc merge

Nếu bạn thêm một phụ thuộc mới vào `composer.json` (và` composer.lock`) và
trước khi nhánh của bạn được merge, có một phụ thuộc khác được thêm vào trong master, bạn cần phải rebase nhánh của bạn. Và bạn sẽ nhận được một merge conflct trong `composer.lock`.

Bạn không bao giờ nên giải quyết xung đột này bằng tay, bởi vì file `composer.lock` chứa một hàm băm của các phụ thuộc được định nghĩa trong
`composer.json`. Vì vậy, ngay cả khi bạn giải quyết xung đột, file lock vẫn sẽ không chính xác.

Tốt nhất là tạo `.gitattributes` tại gốc của project bằng dòng sau, nó có nghĩa là git sẽ không merge `composer.lock`:
    
    /composer.lock -merge

Bạn có thể khắc phục vấn đề này bằng cách sử dụng các branch có tính năng ngắn hạn như được đề xuất trong [Trunk Based Development] (https://trunkbaseddevelopment.com/) (thực ra bạn cũng nên thực hành điều này ngay lúc này). Khi bạn có một branch ngắn hạn, được merge ngay lập tức, nguy cơ xảy ra xung đột khi merge trong `composer.lock` được giảm thiểu tối đa. Bạn có thể thậm chí tạo branch chỉ để thêm dependency và merge nó ngay lập tức.

Nhưng phải làm gì nếu bạn gặp phải merge conflict trong file `composer.lock` khi rebase? Hãy giải quyết bằng cách sử dụng phiên bản từ master, nhờ đó những thay đổi sẽ chỉ xuất hiện trong `composer.json` (các package mới được thêm vào). Và sau đó chạy `composer update --lock`, nó sẽ cập nhật file `composer.lock` với các thay đổi từ `composer.json`. Bây giờ bạn có thể stage file đã được `composer.lock` đã được cập nhật và có thể tiếp tục với rebase.

## Mẹo 10: Hiểu được sự khác nhau giữa `require` và `require-dev`

Điều quan trọng là ta biết được sự khác biệt giữa `require` và` require-dev`.

Các package cần thiết để chạy ứng dụng hoặc thư viện phải được định nghĩa trong `require` (ví dụ: Symfony, Doctrine, Twig, Guzzle, ...) Nếu bạn đang
tạo một thư viện, hãy cẩn thận về những thứ bạn viêt vào `require`. Bởi vì mỗi
dependency tại phần này cũng là dependency của ứng dụng mà sử dụng thư viện.

Các package cần thiết để phát triển ứng dụng (hoặc thư viện) nên được định nghĩa trong `require-dev` (ví dụ: PHPUnit, PHP_CodeSniffer, PHPStan).

## Mẹo 11: Update dependency một cách an toàn

Tôi cho rằng chúng ta có thể nhất trí về việc các dependency nên được update thường xuyên. Những gì tôi muốn thảo luận ở đây là việc update các dependency nên được thực hiện một cách rõ ràng và thận trọng. Không nên thực hiện nó theo kiểu "tiện thể thì làm" kèm với các công việc khác. Nếu bạn sửa đổi một cái gì đó và đồng thời cập nhật một số thư viện, bạn không thể dễ dàng
biết được liệu ứng dụng đã bị hỏng do bạn sửa đổi hay do việc cập nhật đã gây nên.

Bạn có thể sử dụng lệnh `composer outdated` để xem dependency nào có thể
được cập nhật. Bạn cũng có thể thêm tùy chọn `--direct` (hoặc` -D`) để chỉ liệt kê những dependency được khai báo trong `composer.json`. Cũng có một tùy chọn `-m` để liệt kê những phiên bản cập nhật nhỏ.

** Đối với mỗi dependency đã bị lỗi thời, hãy làm theo các bước sau **:

  1. Tạo một branch mới
  2. Cập nhật phiên bản dependency trong `composer.json` thành phiên bản mới nhất
  3. Chạy `composer update phpunit / phpunit --with-dependencies` (thay ` phpunit / phpunit` bằng thư viện bạn đang cập nhật)
  4. Kiểm tra CHANGELOG trong repository thư viện trên Github để xem có bất kỳ thay đổi lớn nào không. Nếu có, hãy cập nhật ứng dụng
  5. Kiểm tra ứng dụng tại local (Nếu bạn đang sử dụng Symfony, bạn có thể thấy các cảnh báo lỗi trong Debug Bar)
  6. Commit các thay đổi (`composer.json`,` composer.lock` và bất cứ điều gì khác cần thiết để phiên bản mới có thể chạy)
  7. Đợi CI build xong
  8. Merge và deploy

Đôi khi, bạn nên cập nhật nhiều dependency cùng một lúc, ví dụ: khi bạn
đang cập nhật Doctrine hoặc Symfony. Trong trường hợp này, bạn có thể liệt kê tất cả trong câu lệnh update sau:
    
    composer update symfony/symfony symfony/monolog-bundle --with-dependencies
    
Hoặc bạn có thể sử dụng ký tự đại diện để cập nhật tất cả các dependency từ một
namespace cụ thể:

    composer update symfony/* --with-dependencies
    
Tôi biết rằng những điều này nghe có vẻ thật tẻ nhạt, nhưng chỉ thỉnh thoảng bạn mới nên cập nhật các dependency, như vậy sẽ an toàn hơn.

Một cách tắt là cập nhật tất cả các `require-dev` dependency cùng một lúc (nếu chúng không yêu cầu thay đổi trong code, nếu không thì tôi đề xuất sử dụng các branch khác nhau để review code dễ dàng hơn).

## Mẹo 12: Bạn có thể định nghĩa các kiểu dependency khác trong `composer.json`

Ngoài việc định nghĩa các thư viện như là dependency, bạn cũng có thể định nghĩa những thứ khác ở đó.

Bạn có thể định nghĩa, phiên bản PHP nào mà ứng dụng / thư viện của bạn hỗ trợ:
    
    "require": {
        "php": "7.1.* || 7.2.*",
    },

Bạn cũng có thể xác định extension nào là cần thiết cho ứng dụng / thư viện. Điều này cực kỳ hữu ích khi bạn cố gắng docker hóa ứng dụng của mình hoặc đồng nghiệp mới đang cài đặt ứng dụng lần đầu tiên.

    "require": {
        "ext-mbstring": "*",
        "ext-pdo_mysql": "*",
    },
    
(Bạn nên sử dụng `*` cho các biến thể của phiên bản vì [chúng đôi khi không có tính nhất quán] (https://getcomposer.org/doc/01-basic-usage.md#platform-
packages)).

## Mẹo 13: Validate `composer.json` trong quá trình build CI

`composer.json` và` composer.lock` phải luôn được giữ đồng bộ. Vì thế, sẽ thật lý tưởng nếu có thể tự động kiểm tra cho nó. Chỉ cần thêm đoạn này như một phần
của giai đoạn khi bạn build script và nó sẽ đảm bảo rằng `composer.lock` được đồng bộ với `composer.json`:

    composer validate --no-check-all --strict
    
## Mẹo 14: Sử dụng một Composer plugin trong PHPStorm

Có một [composer.json plugin cho PHPStorm (https://plugins.jetbrains.com/plugin/7631-php-composer-json-
support). Khi ta sửa `composer.json` bằng tay, autocompletion và một vài validation sẽ được hỗ trợ.

Nếu bạn đang sử dụng IDE khác (hoặc chỉ là một trình soạn thảo code), bạn có thể thiết lập validation đối với [JSON schema của nó] (https://getcomposer.org/schema.json).

## Mẹo 15: Chỉ rõ phiên bản PHP production trong `composer.json`

Nếu bạn giống tôi và đôi khi bạn [chạy các phiên bản PHP tiền phát hành ở local] (https://blog.martinhujer.cz/php-7-2-is-due-in-november-whats-new/),
bạn sẽ có nguy cơ cập nhật các dependency tới phiên bản mà sẽ không hoạt động trong production. Ngay lúc này, tôi đang sử dụng PHP 7.2.0, có nghĩa rằng tôi có thể cài đặt thư viện mà không hoạt động trên 7.1. Khi production chạy 7.1,
cài đặt sẽ thất bại.

Nhưng không cần phải lo lắng, có một cách dễ dàng để giải quyết. Chỉ cần chỉ rõ phiên bản PHP ở production trong phần `config` của` composer.json` như sau:

    "config": {
        "platform": {
            "php": "7.1"
        }
    }
    
Đừng nhầm lẫn với phần `require`, chúng hoạt động khác nhau đó. Ứng dụng của bạn có thể chạy trên 7.1 hoặc 7.2 và đồng thời chỉ định 7.1 làm nền tảng (có nghĩa là các dependency sẽ luôn được cập nhật thành phiên bản tương thích với 7.1):

    "require": {
        "php": "7.1.* || 7.2.*"
    },
    "config": {
        "platform": {
            "php": "7.1"
        }
    },
    
## Mẹo 16: Sử dụng các package private từ  self-hosted Gitlab 

Chúng tôi khuyên bạn nên sử dụng `vcs` làm kiểu của repository và Composer nên xác định cách phù hợp để fetch các package. Ví dụ, nếu bạn thêm một fork từ Github, nó sẽ sử dụng API của nó để tải file .zip thay vì clone toàn bộ repo.

Điều này lại phức tạp hơn đối với một cài đặt cho private Gitlab. Nếu bạn sử dụng `vcs` như một kiểu repo, Composer sẽ phát hiện rằng dó là một cài đặt Gitlab
và sẽ cố tải xuống package bằng API (điều này yêu API key. Do tôi không muốn thiết lập nó, vì vậy tôi đã quyết định thiết lập như sau (sử dụng SSH cho
việc clone):

Đầu tiên chỉ định repo với kiểu `git`:
 
    "repositories": [
        {
            "type": "git",
            "url": "git@gitlab.mycompany.cz:package-namespace/package-name.git"
        }
    ]
    
Sau đó sử dụng package như bình thường:
  
    "require": {
        "package-namespace/package-name": "1.0.0"
    }

## Mẹo 17: Làm sao để tạm thời sử dụng một branch với bugfix từ fork

Nếu bạn tìm thấy bug trong một số thư viện public và bạn sửa nó trong fork của bạn trên Github, bạn cần phải cài đặt thư viện từ repo này thay vì từ repo chính gốc (hoặc trừ khi lỗi bug được sửa và merge và phiên bản đã sửa được phát hành).

Điều đó có thể được thực hiện một cách dễ dàng với [inline aliasing] (https://getcomposer.org/doc/articles/aliases.md#require-inline-alias):

    {
        "repositories": [
            {
                "type": "vcs",
                "url": "https://github.com/you/monolog"
            }
        ],
        "require": {
            "symfony/monolog-bundle": "2.0",
            "monolog/monolog": "dev-bugfix as 1.0.x-dev"
        }
    }
    
Bạn có thể kiểm tra bản sửa lỗi ở local trước khi push nó bằng cách [sử dụng `path` làm kiểu repo] (https://getcomposer.org/doc/05-repositories.md#path).

## Cập nhật vào ngày 2018-01-08:

Sau khi xuất bản bài viết, tôi đã nhận được đề xuất về một số lời khuyên khác. Sau đây là những lời khuyên đó:

## Mẹo 18: Cài đặt prestissimo để tăng tốc độ cài đặt các package

Có một plugin Composer [hirak / prestissimo] (https://github.com/hirak/prestissimo) có thể tăng tốc độ cài đặt dependency bằng cách tải chúng một cách song song.

Và điều tuyệt nhất là gì? Bạn chỉ cần cài đặt nó một cách global một lần, và nó sẽ tự chạy cho tất cả các project khác:

    composer global require hirak/prestissimo

## Mẹo 19: Kiểm thử phiên bản nếu bạn chưa chắc chắn

Việc viết các ràng buộc về phiên bản một cách chính xác đôi khi khá khó khăn ngay cả sau khi đọc [tài liệu] (https://getcomposer.org/doc/articles/versions.md#writing-
ràng buộc phiên bản).

May mắn thay, [Packagist Semver Checker] (https://semver.mwl.be/) giúp
bạn có thể kiểm tra phiên bản nào phù hợp với ràng buộc đã chỉ định. Thay vì chỉ
phân tích ràng buộc của phiên bản, nó tải dữ liệu từ Packagist để hiển thị các phiên bản thực tế được phát hành.

Xem [kết quả cho
`symfony / symfony: ^ 3.1`] (https://semver.mwl.be/#?package=symfony%2Fsymfony&version=%5E3.1&minimum-stability=stable).

## Mẹo 20: Sử dụng authoritative class map trong production

Bạn nên [tạo authoritative class
map](https://getcomposer.org/doc/articles/autoloader-
optimization.md#optimization-level-2-a-authoritative-class-maps) trong
production. Nó sẽ tăng tốc độ load class bằng cách include mọi thứ trong class-map và bỏ qua các kiểm tra của bất kỳ filesystem nào.

Bạn có thể thực hiện điều này bằng cách chạy lệnh sau như một phần của bản build của production:

    composer dump-autoload --classmap-authoritative
    
## Mẹo 21: Cấu hình `autoload-dev` cho các test

Nếu bạn không muốn include các file test trong production class map (bởi lý do về kích thước file và bộ nhớ). Bạn có thể thực hiện điều này bằng cách cấu hình `autoload-dev` (tương tự như `autoload`):
 
    "autoload": {
        "psr-4": {
            "Acme\\": "src/"
        }
    },
    "autoload-dev": {
        "psr-4": {
            "Acme\\": "tests/"
        }
    },
    
## Mẹo 22: Hãy thử sử dụng Composer scripts

Composer scripts là một tool nhỏ gon để tạo các build script. Tôi đã viết [một bài riêng về vấn đề này](/have-you-tried-composer-scripts/).
