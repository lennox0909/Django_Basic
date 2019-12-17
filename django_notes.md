MTV:
1. Model : 描述你的資料類型
2. Template : 使用者看到網頁的形式
3. Views : 傳達資料(重點在於資料傳達的內容
4. Urls: 網址
'''
有了 Model，等於是擁有了資料庫雛形，接著在 View 處理 商業邏輯，取得我們想要顯示的資料，然後在 Template決定我們資料要怎麼擺飾! 最後再將 網址 與 view 作對應
'''
Models - 就是定義我們資料存放的型態，而且我們可以透過ORM的方式去取得
Template - 
View - 透過ORM的方式，去做存取資料的動作，接著再透過DICT MAPPING，去對應到我們 Template 定義的資料欄位，如此一來我們便能成功將資料顯示 (邏輯處理寫在 View)

Django Class-Based Views (CBV)

Render:
簡單來說，Django提供你很多方便你操作的函式，而render可以將我們要傳達的資料一併打包，再透過 HttpResponse 回傳到瀏覽器

DRY(Don't Repeat Yourself)

IoC: Inversion of Control 框架決定程式流程
ORM: Object-Relational Mapping
- 不使用資料庫的語法，e.g. SQL，便能夠達成操作資料庫的事情

WSGI: web server gateway interface
- 相容伺服器設定檔案

makemigrations: 創建新models (創建migration_name)
migrate: 創建資料庫 (依照migration_name創建db)

Database: https://ithelp.ithome.com.tw/articles/10200620
- CRUD: Create、Retrieve (Read)、Update、Delete 新增、讀取、更新、刪除
- 進入 Shell 操作 資料庫的方式也很簡單，鍵入 python manage.py shell，便能夠進入 Python Shell
- Django 在創建資料庫的時候，實際上背後偷偷地增加的 primary_key = id

