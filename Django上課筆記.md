# Django

## Web服務三層式架構
![](https://i.imgur.com/lu362iH.png)
Client透過url去訪問server內的Django寫的網頁應用程式(透過wsgi)，如果有資料需求，再去和DB load或update data。

## MTV、MVC架構
* ### MTV簡介
Django是採用MTV架構，M是Models、T是Templates，V是Views。

![](https://i.imgur.com/tYKR33H.png)

client端的browser透過urls去調用views的function，views再到透過model去DB取資料，取完資料後再透過調用templates(即html)讓client可以在browser看到內容。
** model中的每個class都對應到db中的table，透過ORM去DB取data。(Django會將ORM轉成sql)

* ### MTV架構的優點
    • 分離商業邏輯與UI
    • 前後端可獨立作業
    • 擁有更多彈性
    • 較容易維護
    • 降低複雜度
    
## 安裝Django

```
pip install django
```
```
pip freeze|grep django
```

## 創建專案
先cd到想創建專案的資料夾後執行下方指令
```
django-admin startproject project .
```

測試sever 
```
python manage.py runserver 8000
```

## 設定資料庫

對應的db檔名在settings.py的db內去設定，執行後會執行一連串db創建的過程
```python
DATABASES = {
 'default': {
 'ENGINE': 'django.db.backends.sqlite3',
 'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
 }
}

```

```
python manage.py migrate
```

## 建置app
**Django的主要架構分為project和app兩層架構。
app層讓架構分明以外，可作為可拔插的元件，供任何project使⽤。**


記得先cd到想要的位置下
```
python manage.py startapp app資料夾名稱
```
到settings.py去加上app

```python
INSTALLED_APPS = (
 'django.contrib.admin',
 'django.contrib.auth',
 'django.contrib.contenttypes',
 'django.contrib.sessions',
 'django.contrib.messages',
 'django.contrib.staticfiles',
 'app資料夾名稱',
)
```
## 後台管理介⾯
Django內含的sqlite3，預設就有user和group兩個table，方便網頁服務的管理。

`python manage.py createsuperuser`
設定user, password,e-mail
輸入網址：127.0.0.1:8000/admin/


## ADDING MODEL
Django的model其實就是一個透過Django實現的DB的Table。
django model field就是Table的欄位。

要在app層底下的model.py去做建立，範例如下。去建立一個儲存user po文結果的table。
```python
from django.db import models
from django.utils import timezone
# Create your models here.

class Post(models.Model):

    author = models.ForeignKey('auth.User',
                               on_delete=models.CASCADE,
                               null=False)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(default=timezone.now)

    def publish(self):
        self.created_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```
資料庫欄位屬性null，blank要設定，不然不允許空白和null

## MODEL MIGRATION

在創建或修改model後要輸入下列指令
```
python manage.py makemigrations 
```

如果MIGRATE model後想要回去原本的版本
```
python manage.py migrate app資料夾名稱 想回去的版本號 
```

EX.  `python manage.py migrate blog 0001 `


## template
Variables {{}}
{{key}} ---> 對應到render中第三個參數(dict格式)的key

tags {% logic %}用來寫邏輯
{if ... than ... }


## Urls、Views
用戶透過urls來訪問我們的server，而server透過urls的對應找到相應的app，再透過app內的views內的function去完成商業邏輯或透過model去DB要資料。

project下的urls.py檔 -----> app下的urls.py檔 ----> views的function

* project下的urls.py檔
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    # 用戶透過 https:// xxxxxxxx/blog 會去對應到 app下的urls.py檔
    path('blog/', include('blog.urls')),
]
```
* app下的urls.py檔

```python
from django.urls import path
from .views import post_list,post_form,post_record, add_record

urlpatterns = [
    #blog/ 這個url會去對應到 blog/views下的post_list這個function
    path("",post_list,name="post_list"),
    
    #blog/add_record 這個url會去對應到 blog/views下的add_record這個function
    path("add_record", add_record,name="add_record"),
    path("<int:id>",post_record, name='post_record'),
]
```

* app下的views檔

有些function會透過model去db要data，也有透過import別的.py檔的class去做別的事情，例如:
生成一個form
```python
from django.shortcuts import render
from .models import Post
from .forms import PostForm
from django.contrib.auth import get_user_model
from django.shortcuts import redirect

from django.views.decorators.csrf import csrf_exempt
me = get_user_model().objects.get(username='lionel771128')
# Create your views here.
def post_list(request):
    posts = Post.objects.all().order_by('-created_date')
    post_form = PostForm()
    return render(request, 'blog/post_list.html', locals())
    # return render(request, 'blog/post_list.html', {'posts': posts})
@csrf_exempt
def add_record(request):
    if request.POST:
        title = request.POST['title']
        text = request.POST['text']
        Post.objects.create(author=me, title=title, text=text)
    return redirect('/blog')
```

## CSRF、templates繼承、deploy、自行轉換
[請看pdf檔案](https://drive.google.com/open?id=17LZfl5H3vYFE_qVB7v6Wtp5ZtOThIVE2)
