# Django-sitemap
sitemap
![image](https://github.com/AsadbekNurmamatov2002/Django-sitemap/assets/144318530/d2b68ea2-ad72-4aa2-b9f9-3d810bc97805)

### Django blog ilovasida sayt xaritasini yaratish bosqichlari
Django ishlab chiquvchilarga o'z saytlari va sayt xaritasi tizimidan foydalangan holda dinamik ravishda sayt xaritalarini yaratishga imkon beradi. Blog ilovamizda sayt xaritasini yaratish uchun beshta oddiy qadamni bajaramiz.

__1-qadam: Bularni INSTALLED_APPS bo‘limingizga qo‘shing__
>     INSTALLED_APPS = [
>         …
>         'django.contrib.sites',
>         'django.contrib.sitemaps',
>      ]
>
>      SITE_ID = 1


Biz saytlar ramkasini va sayt xaritalari ilovasini loyihamiz sozlamalariga qo‘shamiz. __SITE_ID__ ushbu Django loyihasida qancha veb-saytlar mavjudligini aniqlaydi.

__2-qadam: Ma'lumotlar bazasini yangilash__
Biz buni terminalda migratsiyani ishga tushirish orqali qilamiz
>      python3 manage.py makemigrations
>      python3 manage.py migrate

![image](https://github.com/AsadbekNurmamatov2002/Django-sitemap/assets/144318530/376b2e4f-4816-4698-a5a7-b0ee41fd483c)

__3-qadam: Django sitemap.py faylini yarating__

Bu ilova darajasidagi katalogda amalga oshiriladi. urls.py da GenericSitemap sinfidan foydalanishdan farqli ravishda alohida sayt xaritasi faylini yaratish bizga koʻproq nazorat qilish va uning mavjud usullariga kirish imkonini beradi, ulardan baʼzilari ushbu loyihada qoʻllaniladi.

>       from django.contrib.sitemaps import Sitemap
>       from .models import Post
> 
>       class PostSitemap(Sitemap):
>           changefreq = 'weekly'
>           priority = 0.8
>           protocol = 'http'
>
>       def items(self):
>          return Post.objects.all()
>
>       def lastmod(self, obj):
>          return obj.last_modified
>
>       def location(self, obj):
>          return f'/blog/{obj.slug}'
> 
Biz yuqorida import qilingan sinfini kengaytiruvchi PostSitemap sayt xaritasi sinfini yaratamiz. Qo'shilgan atributlar quyidagicha izohlanadi:Sitemap


__changefreq__-o‘z-o‘zidan tushunarli. Jiddiy blogger o'z blogini haftada kamida ikki marta yangilab turishi kerak. Shuning uchun biz har hafta o'rnatamiz. Boshqa variantlarga soatlik, kunlik, oylik va yillik kiradi.
__priority__ -sahifalarni veb-saytingizdagi boshqa xabarlar uchun ahamiyati bo'yicha tartiblaydi. U 0 dan 1 gacha.
__protocol http__ yoki __https__ boʻlishi mumkin boʻlgan veb-sayt protokoliga ishora qiladi. Blogimiz hali ham mahalliy serverda joylashganligi sababli biz http dan foydalanamiz.

Usullar quyidagicha izohlanadi:

1. __items()__ usuli biz sayt xaritasiga kiritmoqchi bo‘lgan barcha namunaviy obyektlarni qaytaradi.
2. __lastmod()__ usuli post yaratilgan yoki o‘zgartirilgan sanani qaytaradi.
3. __location()__ usuli URL manzilini qaytaradi. Biz slugni tanlaymiz, chunki u Post modelida aniqlangan va post sarlavhasi asosida o‘zini to‘ldirishga o‘rnatiladi. Shunday qilib, har qanday post uchun URL endi bo'ladihttp://127.0.0.1:8000. /blog/<title_of_post>

__4-qadam: Sayt xaritasi URL manzilini qo‘shing__

>        from .sitemaps import PostSitemap
>        from django.contrib.sitemaps.views import sitemap
>
>        sitemaps = {
>          'blog': PostSitemap
>        }
>
>        urlpatterns = [
>             …
>            path('sitemap.xml', sitemap, {'sitemaps': sitemaps}, name='django.contrib.sitemaps.views.sitemap'),
>         ]
> 

__5-qadam: Domen nomini o‘zgartiring Sites__

Administrator interfeysiga o‘ting (quyida skrinshotga qarang) va example.com tugmasini bosing.

Endi biz mahalliy serverni ishga tushirish va http://127.0.0.1:8000/sitemap.xml sahifasiga o‘tish orqali sayt xaritasi ko‘rsatilishini tekshirishimiz mumkin.
![image](https://github.com/AsadbekNurmamatov2002/Django-sitemap/assets/144318530/1f4707d8-4f03-4c6b-9c0e-92040f94b12a)

