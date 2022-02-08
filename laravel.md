# Laravel

Laravel 的構思的確和其他框架有很大不同，這也要求學習他的人必須熟練 php，並基礎紮實！
如果你覺得學 laravel 框架十分困難，那麼原因只有一個：你 php 基礎不好。

又稱作PHP教科書

### PHP版本需求
5.0 -> PHP>=5.4.0, PHP<=7.0.*
7 -> PHP>=7.2.5
8 -> PHP>=7.3

### 公司版本
Laravel -> 8.69.0 => PHP -> 7.3.29

### 執行環境
laradock

### 架構

View - Controller - Service - Repository - Model

### 開發注意事項
1.建立DB要有 migrations
2.要有Models
3.要有流程圖 (非必要)
4.實作至少一個 feature function 
5.實作至少一個 unit test

### RESTful API簡介

- get: 讀取資源

- post: 新增資料

- delete: 刪除資源

- put: 替換資源 (通常做替換一個資源功能)

- patch: 更新資源 (修改資源的部分內容)

***

## 安裝Laradock
1.下載並安裝docker(先確認系統安裝需求)

2.確認已安裝GIT

```shell
git --version
```

3.Command-line下選擇自己要的目錄(跟專案同層)

```shell
cd ~/project
```

4.安裝laradock(刪掉重裝的話注意已安裝的image)

```shell
git clone https://github.com/Laradock/laradock.git
```

5.進入目錄建立所需的.env檔案

```shell
cd laradock
cp .env.example .env
#修改存放專案目錄(單一專案)
vi .env
APP_CODE_PATH_HOST=../project/royce
```

6.建立nginx的laravel.test.conf 檔

```shell
cd ~/laradock/nginx/sites
cp laravel.conf.example laravel.test.conf
#修改nginx檔案(指向專案路徑，/var/www/=APP_CODE_PATH_HOST)
root /var/www/public;
```

7.啟動docker中的服務(第一次啟動會跑很久，workspace會自己同時啟動)

```shell
docker-compose up -d nginx mysql phpmyadmin
```

8.建立laravel專案(需透過docker下cmd，gitlab上已有則跳過)

```shell
docker-compose exec workspace bash #離開用exit
composer create-project laravel/laravel royce
#或
composer create-project --prefer-dist laravel/laravel royce
#或
composer create-project laravel/laravel --prefer-dist .
```

9.開啟瀏覽器確認可執行

```shell
http://127.0.0.1/ #laravel歡迎畫面
http://127.0.0.1:8081 #phpmyadmin登入畫面(server=mysql,username=root,password=root)
```

10.結束docker服務

```shell
docker-compose down #等同docker-compose stop
```

***
## 專案開發流程

```shell
#到gitlab將專案git clone下來(master)
#透過sourcetree切換到分支dev將專案下載下來
#開分支

#開啟終端機(windows推薦cmder)
#切換到laradock目錄
#運行服務
docker-compose up -d nginx mysql phpmyadmin
#進入docker的workspace container下指令
docker-compose exec workspace bash

#安裝相依套件
composer install
#產生專案.env檔案(若有可跳過)
cp .env.exmaple .env
#建laravel的key(若有可跳過)
php artisan key:generate
#產生jwt(json web tokens)私鑰(若有可跳過)
php artisan jwt:secret
#建立資料表(執行up()方法，先確認DB清單中已有laravel)
#若遇到錯誤可能要修改.env的DB_HOST=mysql,DB_PASSWORD=root
php artisan migrate
#離開workspace container
exit

#開始開發
#關閉運行的服務
docker-compose down

#確認完成後在gitlab上發出合併請求
#合併完成後再把遠端及本地分支砍掉
```

***
## 其它

### laravel修改預設時區

```shell
#\config\app.php
'timezone' => 'Asia/Taipei',
```



### laravel 測試

#### 簡介

Laravel 預設就支持用 PHPUnit 來做測試，並為你的應用配置好了 phpunit.xml 檔案。

單元測試(Unit Tests)是針對你的程式碼中非常少，而且相對獨立的部分來進行的測試。實際上，大部分單元測試都是針對單個方法進行的。單元測試不會啟動你的應用因此無法取用應用資料庫或框架的服務

功能測試(Feature Tests)則是針對大部分的程式碼進行的測試，包括多個物件之間的交互，甚至是對 JSON 端點的完整 HTTP 請求等等。總的來說，你大部分的測試都應該是功能測試，這些測試能讓你確保你的系統能夠正常運作

Feature 和 Unit 目錄中都提供一個 ExampleTest.php 測試範例文件。安裝一個新的 Laravel 應用之後，在命令行下運行 phpunit 或者是 test 命令，即可運行測試

#### 環境

你可以隨意創建其它必要的測試環境配置。testing 環境變數可以在 phpunit.xml 文件中修改，但是在運行測試之前，請確保使用以下命令來清除配置的緩存！

```shell
php artisan config:clear
```

使用 Artisan 命令 make:test 創建一個新的測試用例

```shell
#在 Feature 目錄下創建一個測試類別
php artisan make:test UserTest

#在 Unit 目錄下創建一個測試類別
php artisan make:test UserTest --unit
```

#### 建立測試用設定檔

複製 .env 檔用以生成 .env.testing 檔案，內容大致可相同，一般來說根據需求將資料庫設定改成另一個資料庫，複製.env中APP_URL及JWT_SECRET設定值過去

```shell
#注意以下設定
DB_CONNECTION=sqlite
DB_DATABASE=:memory:
```

#### 執行測試

```shell
#預設執行全部測試
#其它指令可參考常用指令
php artisan test
```

### laravel Fake 使用

```php
#安裝
#composer require fzaninotto/faker
#可通过在 config/app.php 增加如下配置使其支持中文
#'faker_locale' => 'zh_CN',
#Faker\Provider\Base
$randomDigit = $faker->randomDigit;//生成0-9之间的随机数
$randomDigitNotNull = $faker->randomDigitNotNull;//生成1-9之间的随机数
$randomNumber = $faker->randomNumber(5, true);//生成5位整数，true表示严格模式，即只能5位
$randomFloat = $faker->randomFloat(2, 0, 10);//生成浮点数，两位小数点，范围是0-10之间
$numberBetween = $faker->numberBetween(0, 100);//生成随机整数，范围是0-100之间
$randomLetter = $faker->randomLetter;//返回a-z之间任意的一个小写字符
$randomElements = $faker->randomElements(['a', 'b', 'c', 'd'], 2);//返回数组中的随机两个元素
$randomElement = $faker->randomElement(['aa', 'bb', 'cc', 'dd']);//随机返回数组中的一个元素
$suffle = $faker->shuffle('hello, world'); //将字串中的字符打乱返回
$suffle = $faker->shuffle(['aa', 'bb', 'cc', 'dd']); //将数组中的元素打乱返回
$numerify = $faker->numerify('Hello #####');//#####替换为随机数字，输出类似：Hello 03501
$lexify = $faker->lexify('Hello ???');//???替换为3个随机小写字符，输出类似：Hello krg
$bothify = $faker->bothify('hello ##??');//#替换为随机数字,?替换为随机小写字符.输出类似：hello 15cr
$asciify = $faker->asciify('hello *****');//*替换为随机字符，输出类似：hello 5Ynt[
$regexify = $faker->regexify('[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}');//根据正则表达式返回字串
```


***
## 錯誤處理

### docker無法正常啟動-port被占用

請先停止XAMPP的mysql服務，然後關閉，別直接離開
或是改用其他port

### docker無法正常啟動-phpmyadmin無法登入

若原因為: `Different lower_case_table_names settings for server ('2') and data dictionary ('0').` 

Laradock数据cache问题，更改MySQL版本及其重建
1.關閉服務
2.修改.env中MYSQL_VERSION從latest改成5.7(預設是8)
3.刪除資料庫資料 `rm -rf ~/.laradock/data/mysql`
4.重建mysql: `docker-compose build mysql`
5.重新啟動服務: `docker-compose up -d nginx mysql phpmyadmin`

若還是無法解決，可能是PHP 7和MySQL 8的authentication方法不一致的问题
1.啟動服務: `docker-compose up -d nginx mysql phpmyadmin`
2.查看mysql的container id: `docker ps`
3.進入mysql的docker container: `docker exec -it {替換剛剛的id)} bash` 
4.登入mysql: `mysql -u root -p`
5.修改用戶密碼: 
`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root'`
`ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';`
`ALTER USER 'default'@'%' IDENTIFIED WITH mysql_native_password BY 'secret';`
6.登入phpmyadmin


### 戳API 出現 401 Unauthorized (公司用)

先去註冊一組帳號，再去登入獲得token，然後在目標API的Authorization加上參數(過期需重新拿)



### API body參數接收不到

postman body參數要放在x-www-form-urlencoded底下

### 下指令 php artisan jwt:secret 出現 There are no commands defined in the "jwt" namespace.

```shell=
#/composer.json補上下列參數
#"require": {
#中間省略
"tymon/jwt-auth": "^1.0.2"
#},
```

***

## 常用指令

### Aglio

```shell=
#Default theme
aglio -i input.apib -o output.html

#Built-in color scheme
aglio --theme-variables slate -i index.apib -o index.html
```

### Composer

```shell=
#相依套件安裝
#透過laradock去安裝
composer install
#執行環境的PHP版本差異請用(強制執行容易有錯誤，請確認下指令的目錄)
composer install --ignore-platform-reqs

#相依套件更新
composer update

#刪除migration後需執行
composer dump-autoload
```

### Docker

```shell=
#查看git版本
git --version

#查看docker版本
docker --version

#查看docker所啟動的服務
docker ps

#啟動docker服務，需在laradock目錄下運行(需確認docker有啟動)
#nginx可換成apache2
docker-compose up -d nginx mysql phpmyadmin

#進入docker的workspace container下指令
#離開用exit
docker-compose exec workspace bash 

#關閉啟動的服務
docker-compose down
```

### Laravel

```shell=
#查看Laravel使用版本
php artisan --version

#清除快取(更改.env後不起作用)
php artisan config:cache

#清除配置的緩存
php artisan config:clear

#建laravel的key
php artisan key:generate

#產生jwt(json web tokens)私鑰
php artisan jwt:secret

#建立controller
php artisan make:controller UserController

#建立model
php artisan make:model ErpUser

#新增資料表
php artisan make:migration create_erp_users_table --create=erp_users

#新增欄位到資料表(會花點時間)
#執行前需確保已執行composer require doctrine/dbal
php artisan make:migration add_status_to_ship_orders --table=ship_orders

#建立假資料工廠
php artisan make:factory CommodityFactory
#指定model
php artisan make:factory CommodityFactory --model=Commodity

#建立資料填充
php artisan make:seeder CommoditySeeder

#在Feature目錄下創建一個測試類別
php artisan make:test BoatStuffingRepositoryTest

#在Unit目錄下創建一個測試類別
php artisan make:test BoatStuffingRepositoryTest --unit

#建立資料表(執行up()方法)
php artisan migrate

#清除資料表(執行down()方法)
php artisan migrate:rollback

#還原所有遷移
php artisan migrate:reset

#重新建立(執行migrate:reset+migrate)
php artisan migrate:fresh
#重新建立並填充假資料
php artisan migrate:refresh --seed

#執行資料填充
php artisan db:seed
#使用--class選項來單獨運行一個特別指定的seeder類別
php artisan db:seed --class=CommoditySeeder

#執行測試
php artisan test
#執行特定file
php artisan test --filter=BoatStuffingRepositoryTest
#執行特定file的function
php artisan test --filter=BoatStuffingRepositoryTest::testUpdateRcvItemList
```
