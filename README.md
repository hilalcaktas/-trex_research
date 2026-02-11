# trex_research
## 1. Modern Yazılım Geliştirme Pratikleri

   ### -Git nedir? GitHub nedir?
   Git, bir versiyon kontrol sistemidir. Bir projenin bir çok kişi ile aynı anda yapılabilmesini sağlar. Dosyalarda değişiklik yapılmasını, projeyi bozarsak düzeltilebilmesini sağlar ve güncel tutmamızı sağlayan bir araçtır.
  GitHub, Git projelerini internette barındıran bir platformdur. Ekip çalışması yapmayı, Projeleri paylaşmayı sağlar. Farklı projeleri de indirerek değişiklik yapıp tekrar paylaşılabilir.
  
   ### Temel Git komutları: init, clone, add, commit, push, pull, branch, merge
   **init**: Bulunduğun klasörde yeni bir Git repository oluşturur. Git, dosya değişikliklerini takip etmeye başlar.
   **clone**: Uzak sunucudaki (GitHub) bir projeyi bilgisayarımıza indirir.
   **add**: Yaptığımız değişiklikleri Git`e ekler.
   **commit**: Yapılan değişiklikler local Repository’e kaydedilir.
   **push**: Bilgisayarımızda yaptığımız değişiklikleri uzak sunucuya (GitHub) gönderir.
   **pull**: remote repository’deki (GitHub) yeni değişiklikleri alıp local repository’ye (bilgisayardaki kaydedilen depoya) getirir.
   **branch**: Ana kodu bozmadan ayrı bir kolda proje üzerinde değişiklikler, geliştirmeler yapılır.
   **merge**: Farklı dallarda yapılan değişiklikleri tek bir kod tabanında birleştirir ve bu değişikliklerin uyumlu olmasını sağlar.
   
   ### Merge conflict nedir, nasıl çözülür?
   Merge conflict, aynı dosyanın aynı satırı iki branchte de değiştiyse mesela birince silinmişken birinde düzenlenmişse olur.
   hangisinden nasıl ilerlemek istediğimize biz karar veriyoruz. Çakışan dosyayı açarız hangisini kullanmak istiyorsak veya ikisini de birleştirmek istiyorsak gerksiz dosyaları sileriz. Düzenleyip dosyayı kaydederiz ve conflict çözülmüş olur.
   
   ### CI/CD nedir? Azure DevOps, GitHub Actions ile pipeline örnekleri
   CI/CD yazılımı otomatik olarak test etme, derleme ve yayınlama sürecidir. Hatalar erken yakalanır, insan hatası azalır, hızlı ve güvenli yayın yapılır.
  CI (Continuous Integration - Sürekli Entegrasyon) : bozuk kod main branche girmemesi için kod değişikliklerini otomatik olarak derler, test eder ve entegre eder.
  CD (Continuous Delivery - Sürekli Teslimat) : Kod değişikliklerini onay için üretime hazır ortamlara otomatik olarak iletir.
  CD (Continuous Deployment - Sürekli Dağıtım) :  Kod değişikliklerini otomatik olarak doğrudan müşterilere dağıtır.
   
   **Azure DevOps Pipeline Örneği (.NET)**
   ```
trigger:
- main

pool:
  vmImage: 'windows-latest'

steps:
- task: UseDotNet@2
  inputs:
    packageType: 'sdk'
    version: '8.x'

- script: dotnet restore
  displayName: 'Restore packages'

- script: dotnet build --configuration Release
  displayName: 'Build project'

- script: dotnet test
  displayName: 'Run tests'
 ```
(main branch’e push olunca otomatik çalışır, .NET projeyi derleyebilmesi ve çalıştırabilmesi için gerekli araçları yükler, paketleri indirir projeyi derler testleri çalıştırır.)

**GitHub Actions Pipeline Örneği (.NET)**
```
name: .NET CI Pipeline
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Repo'yu çek
      uses: actions/checkout@v4

    - name: .NET kur
      uses: actions/setup-dotnet@v4
      with:
        dotnet-version: '8.0.x'
. ya 
    - name: Restore
      run: dotnet restore

    - name: Build
      run: dotnet build --configuration Release --no-restore

    - name: Test
      run: dotnet test --no-build
 ```
 (main’e push veya pull request gelince çalışır, repository indirir, .NET kurar )
 
  **bir .NET projesinin nasıl uygulandığı**
.NET projesi geliştiririz kodlarımızı GitHub`a yükleriz ve CI/CD tetiklenir, önce CI devreye girer ve bozuk kodun projeye girmemesi için kontrollerini yapar. CI başarılı olursa CD devreye girer uygulama çalıştırılabilir hale getirilir ve yayınlanmaya hazır paket oluşur. Ya Continuous Delivery(sürekli teslimat) olur yayınlama butonuna basınca çalışır, ya da Continuous Deployment(sürekli dağıtım) otomatik olarak canlı ortama çıkar.

### Software Development Life Cycle (SDLC)
Yazılımın başlangıcından geliştirmesine bakımına test etmesine kadar yapılan bir sistemdir.

### Yazılım geliştirme sürecinin aşamaları (Planlama, analiz,geliştirme, test, dağıtım, bakım)
 **planlama**: başlangıç aşamasıdır, ne neden yapılacak diye düşünülür süre, maliyet, riskler görülür ve planlanır
 **analiz** : kullanıcı ihtiyaçlarına göre ne yapılacak projede ne olucak diye karar verilir
 **geliştirme** : planlar ve analizler kod üzerinde uygulanmaya başlanır 
 **test** : kullanıcıya hatalı yazılım gitmesin diye yazılım test edilir hatalar düzeltilir
 **dağıtım** : yazılım kullanılacak durumdadır ve kullanıcıların kullanacağı ortama alınır
 **bakım** : yazılımda yeni özellikler yapılır sürekli güncel bir sistem olur
 
 ### Agile/Scrum/Kanban metodolojileri
 **Agile** : projeyi küçük parçalar halinde yapılır
 **Scrum** : agile ile yapılan küçük projelerin yapıldığı zamandır 
 **Kanban** : diğerleri gibii zamana göre değildir. işi akışa göre yapar, pano üzerinde görülür (yapılacak, yapılıyor, bitti)
 
## 2. .NET Ekosistemi

### .NET nedir? Tarihçesi, amacı, neden kullanılır?
 Yerel olarak çalışabilen uygulamalar oluşturmayı sağlar(web, masasütü, mobil vs).
 2000 → Microsoft .NET’i duyurdu
 2002 → İlk sürüm (.NET Framework) yayınlandı
 Başlangıçta sadece Windows içindi
 2016 → .NET Core çıktı (çoklu platform: Windows, Linux, macOS)
 2020 sonrası → .NET 5, 6, 7, 8 ile tek çatı altında birleşti
 -amacı ise tek bir platformda bir sürü fakrlı uygulama yapabilmek
 
### .NET Framework, .NET Core ve .NET 7/8+ farkları
| Özellik            | .NET Framework     | .NET Core                  | .NET 7 / 8+                       |
| ------------------ | ------------------ | -------------------------- | --------------------------------- |
| Çalıştığı platform | Sadece Windows     | Windows, Linux, macOS      | Windows, Linux, macOS             |
| Güncelleme durumu  |  Yok(bakım modu)   |  Var                       |  Var                              |
| Yapı               | İlk .NET yapısı    | Modern ve yeniden yazılmış | .NET Core’un devamı, **tek çatı** |
| Performans         | Orta               | Yüksek                     | Çok yüksek                        |
| Açık kaynak        |  Hayır             |  Evet                      |  Evet                             |
| CLI (komut satırı) |  Zayıf             |  Güçlü                     |  Çok güçlü                        |
| Microservice uyumu |  Zayıf             |  İyi                       |  Mükemmel                         |
| Yeni proje için    |  Önerilmez         |  Artık eski                |  Önerilen                         |

### Platformlar arası çalışabilir mi? (Windows, Linux, macOS)
evet çalışılabilir zaten .NET diyince günümüzde platformlar arası yapıdan kast edilir ama .NET'in hangi sürümleri arasında olcağına göre değişir çünkü .NET Framework sadece eski ve windowsta çalışıyor

### Senkron ve Asenkron Programlama
**Senkron** : her şeyi sırayla yapar önce biri biter sonra diğerine geçilir
**Asenkron** : hepsini beklemeden yapar, biri uzun sürüyorsa o yapılırken diğerini de yapar

 ### async, await, Task, ConfigureAwait gibi anahtar kavramlar
  **async** : metodu asenkron yapar, bekleme yapar ama tek başına bir işe yaramaz await ile kullanılır
  **await** : async ile bekler, uzun süren işi bekletir ama bir yandan diğerleri akışa devam eder
  **Task** : işin durumunu ve sonucu hakkında bize biilgi verir devam eden iştir
  **ConfigureAwait** :işlemler bittikten sonra nerede devam edileceğini belirlenir
  
  ### arrow function (=>) ifadesinin C#’taki yeri
 genelde kısa metodlarda kullanılır tek satırlık gibi, dönüştürür anlamındadır.
 
## 3. Backend Geliştirme 

###  Backend nedir? Frontend ile farkları
**Backend** : arayüzün arkasındaki veri ve işlerin çalışmasını sağlayan taraftır. verileri kaydeder, güvenliği yüksektir, sunucuda çalışır, kullanıcıya görünmez
**Frontend** : kullanıcının gördüğü kullandığı arayüz kısmıdır. verileri gösterir,güvenliği düşüktür, tarayıcıda çalışır

### Web sunucusu nedir? API nedir? API türleri
Web sunucusu, internet üzerinden gelen isteklere yanıt verir.
API, iki yazılımın birbiriyle iletişim kurmasını sağlar.
--Kullanım Amaçlarına Göre API Türleri--
**Internal API** : belirli kişiler kullanabilir, yeniden kullanım ve üretim için kullanılır.
**Open API** : herkese açıktır.
**Partner API** : birbiriyle ortaklık yapıp iş yapan yerlerle arasında kullanılır
**Composite API** : birden çok veriyi bir araya getirir ve tek bir çağrıda birden çok uç noktaya erişilmesini sağlar.
--Mimari Yapıya Göre API Türleri--
**REST API** :modern web tabanlı uygulamalar oluşturmak için sıklıkla kullanılır. HTTP kullanıır.
**SOAP API** : sıkı güvenlikle veri aktarımı sağlar.
**GraphQL API** : istemcinin istedikleri şeyi onlara verir.
### HTTP nedir? HTTP metodları: GET, POST, PUT, DELETE
HTTP, istemci ve sunucu arasındaki iletişimi sağlar.
**GET** : sunucudan veri almak için kullanılır.
**POST** : yeni veri oluşturmak için kullanılır.
**PUT** : veriyi güncellemek için kullanılır.
**DELETE** : veriyi silmek için kullanılır.

### RESTful servislerin çalışma mantığı
RESTful, iki bilgisayar sisteminin internet üzerinden güvenli bir şekilde bilgi alışverişi yapmak için kullandığı bir arabirimdir.
 1. İstemci, sunucuya bir istek gönderir. İstemci, isteği sunucunun anlayacağı şekilde biçimlendirmek için API belgelerine uyar.
 2. Sunucu, istemcinin kimliğini doğrular ve istemcinin bu istekte bulunma hakkı olduğunu onaylar.
 3. Sunucu, isteği alır ve dahili olarak işler.
 4. Sunucu, istemciye bir yanıt verir. Yanıt, istemciye isteğin başarılı olup olmadığını söyleyen bilgileri içerir. Yanıt aynı zamanda, istemcinin talep ettiği bilgileri de içerir.

### JSON veri formatı ve kullanım amacı
Sunucu ile web uygulaması arasında veri taşır.

   JSON Örneği: Kişi Bilgileri

    {
     "ad": "Ali",
     "soyad": "Veli",
     "yas": 30,
     "sehir": "İstanbul",
     "hobiler": ["kitap okumak", "spor yapmak", "seyahat etmek"]
    }

{}: Bir JSON nesnesini (object) temsil eder.
"ad": "Ali": "ad" anahtarına karşılık gelen değer "Ali"dir. Bu, bir key-value (anahtar-değer)
çiftini gösterir.
"hobiler": ["kitap okumak", "spor yapmak", "seyahat etmek"]: "hobiler" anahtarı, bir dizi
(array) değere karşılık gelir. Bu dizi içindeki her bir eleman, Ali'nin hobilerinden biridir.

### SOAP ve GraphQL nedir, REST’ten farkları
SOAP, güvenlik ve işlem yönetimi gerektiren uygulamalar arasında veri alışverişi yapmak için kullanıllır.
GraphQL, istemcinin hangi veriyi istediğine göre veri alımı yapar.

| Özellik         | SOAP     | REST   | GraphQL    |
| --------------- | -------- | ------ | ---------- |
| Tür             | Protokol | Mimari | Sorgu dili |
| Veri formatı    | XML      | JSON   | JSON       |
| Performans      | Düşük    | Yüksek | Yüksek     |
| Esneklik        | Düşük    | Orta   | Yüksek     |
| Öğrenme         | Zor      | Kolay  | Orta       |

## 4. ASP.NET

### ASP.NET ve ASP.NET Core nedir? Avantajları, farkları
**ASP.NET** : Web uygulamalarını geliştirmek için kullanılır. 
Avantajları:
  1. kod tekrarını azaltır, geliştirme sürecini hızlandırır
  2. kullanıcı deneyimini iyileştirir
  3. bakım ve yönetim kolaylığı sağlar
  4. geniş bir geliştirici topluluğuna sahiptir

**ASP.NET Core** : Platform bağımsız çalışan ASP.NET'in sürümüdür
Avanatajları:
  1. açık kodlu bir yapıya sahip
  2. projenizde yapacağınız değişikliklerde daha esnek olabilir
  3. uygulama başka platforma taşınsa da çalışmaya devam eder
     
Farkları:

| Özellik              | ASP.NET        | ASP.NET Core          |
| -------------------- | -------------- | --------------------- |
| Çalıştığı platform   | Sadece Windows | Windows, Linux, macOS |
| .NET altyapısı       | .NET Framework | .NET (Core, 6/7/8)    |
| Performans           | Orta           | Çok yüksek            |
| Açık kaynak          |   Hayır        |    Evet               |
| Dependency Injection | Kısıtlı        | Yerleşik              |
| Cloud / Docker       | Zayıf          | Çok güçlü             |
| Konfigürasyon        | web.config     | appsettings.json      |
| Middleware           | Yok            | **Var**               |

### MVC nedir, ne için kullanılır?
MVC (Model-View-Controller), kodu 3 ayrı sorumluluğa ayıran yazılımdır. yazılım geliştirme süreçlerinde uygulamanın veri, kullanıcı arayüzü ve kontrol akışı gibi temel bileşenlerini birbirinden ayırarak daha temiz bir yapı kurulmasını sağlar.

### Middleware nedir, nasıl çalışır?
Middleware, farklı uygulamalar, sistemler ve veritabanları arasında etkileşim ve veri akışı sağlayan bulut hizmetleridir. Uygulamalar ve veriler arasında bir köprü görevi görür. Sistemler arasında kesintisiz iletişim sağlar, veri akışını düzenler.
Farklı uygulamaların ve hizmetlerin JSON, REST, XML ,SOAP veya web hizmetleri gibi yaygın mesajlaşma çerçeveleri üzerinden iletişim kurmasını sağlayarak işlev görür.

### Dependency Injection (DI) nedir, neden önemlidir?
Bir sınıfın/nesnenin bağımlılıklardan kurtulmasını amaçlayan ve o nesneyi olabildiğince bağımsızlaştıran bir programlama tekniği
Bağımlılık oluşturacak nesneleri direkt olarak kullanmak yerine, bu nesneleri dışardan verilmesiyle sistem içerisindeki bağımlılığı minimize etmek amaçlanır. Böylece bağımlılık bulunan sınıf üzerindeki değişikliklerden korunmuş oluruz.

public class KullaniciController : Controller
{
    private readonly IKullaniciService _kullaniciService;

    public KullaniciController(IKullaniciService kullaniciService)
    {
        _kullaniciService = kullaniciService;
    }

    public IActionResult Index()
    {
        var kullanici = _kullaniciService.KullaniciGetir();
        return Content(kullanici);
    }
}
### Katmanlı Mimari (Layered Architecture)
Uygulamayı sorumluluklarına göre ayrı katmanlara bölerek geliştirir
### Presentation, Business, Data Access katmanları
**Presentation Layer** : Bu katman kullanıcı ile etkileşimin yapıldığı katmandır. Burası Windows form da olabilir, Web’te olabilir veya Bir Consol uygulamasida olabilir. Burada temel amac kullaniciya verileri göstermek ve kullanıcıdan gelen verileri Business Katmanı ile Data Access’e iletmektir.
**Business Layer** : Bu katmanda iş yüklerimizi yazıyoruz.Kullanıcıdan gelen veriler öncelikle Business katmanına gider oradan işlenerek Data Access katmanına aktarılır. Bu katmanda ayrıca bu verilere kimlerin erişeceğini belirtiyoruz.
**Data Access Layer** : Bu katmanda sadece veritabanı işlemleri yapılmaktadır. Bu katmanın görevi veriyi ekleme, silme, güncelleme ve veritabanından çekme işlemidir. B

### Service & Repository pattern
 **Service layer**: Bu katman, bu modeldeki iletişim noktasıdır. Bu katman, verilen istek için gerçek iş mantığını gerçekleştirir ve "tek birimlik iş nesnesiyle ilgili işlemler" için depo katmanından sorgulama yapar,birden fazla depoya erişebilir.Bu katmanda çok birimli iş nesnesi işlemleri veya herhangi bir iş mantığı işlemi (örneğin, yanıt biçimlendirme, iki farklı tek birimli iş nesnesi işlemi sonucunu birleştirme, veriler üzerinde hesaplamalar yapma) gerçekleşir.
 
**Repository layer**: Veriyle ilgili tüm işlemleri gizler ve servis katmanının veri nesneleriyle doğrudan etkileşim kurmasını engeller.

### Clean Architecture
Clean Architecture,yazılımların farklı katmanlarının birbirinden bağımsız olmasını sağlayan, esnek, sürdürülebilir ve test edilebilir bir yazılım tasarımı yaklaşımıdır.
### Domain, Application, Infrastructure, API katmanları
**Domain**: Domain yani mülk katmanı bütün yapıda kullanılacak varlıkları (Entity) barındırır.
**Application**:Uygulama katmanı içerisinde Domain katmanında bulunan varlıkların (Entity) sorgulanması, eklenmesi, silinmesi gibi işlemlerin sözleşmeleri (Interface) yer alır.
**Infrastructer**:Altyapı katmanı olarak kullanılmaktadır. Bu katman içerisinde Veri tabanı nesneleri, dependency injection (bağımlılık ekleme) gibi nesneler barınmaktadır.
**Presentation**:Sunum katmanı olan bu alanda projenin hangi platformda sunulacağı barınır.

### Bağımlılıkların dışa akması ilkesi
Bu ilke Clean Architecture’ın temel taşıdır. Bağımlılıklar daima dış katmanlardan iç katmanlara doğru olmalıdır.İç katmanlar (Domain, Application) dış katmanları (API, Infrastructure) tanımaz ama dış katmanlar iç katmanları bilir ve kullanır.

![1_swy_Um8nP-x0yB5PUdzscg](https://github.com/user-attachments/assets/e3117f73-c7a0-4efb-aebd-b04c29990a25)

## 5. Veritabanı ve ORM
 ###  SQL nedir? 
  Veritabanlarındaki verileri depolamak, değiştirmek ve almak için kullanılan standart bir dildir.
### İlişkisel ve ilişkisel olmayan veri tabanları arasındaki farklar
**İlişkisel Veritabanları (Relational / SQL)**
Tablo yapısı kullanır (satır–sütun)
Tablolar arasında ilişki (relation) vardır
Önceden tanımlı şema (schema) zorunludur
SQL dili ile sorgulanır
ACID kurallarına uyar
Veri tutarlılığı yüksektir
Karmaşık sorgular kolaydır
Esneklik düşüktür
Büyük ve dağınık verilerde ölçekleme zordu

**İlişkisel Olmayan Veritabanları (NoSQL)**
Tablo zorunluluğu yok
Şema esnektir veya yoktur
Büyük ve dağınık veriler için uygundur
Yatay ölçekleme güçlüdür
ACID yerine çoğunlukla BASE yaklaşımı
Çok hızlı
Esnek veri yapısı
Büyük veri ve gerçek zamanlı sistemler için uygun

| Özellik        | İlişkisel (SQL) | İlişkisel Olmayan (NoSQL)   |
| -------------- | --------------- | --------------------------- |
| Veri Yapısı    | Tablolar        | Esnek (JSON, key-value vb.) |
| Şema           | Zorunlu         | Esnek                       |
| İlişkiler      | Var             | Yok / sınırlı               |
| Ölçekleme      | Dikey           | Yatay                       |
| Performans     | Orta            | Yüksek                      |
| Tutarlılık     | Yüksek (ACID)   | Esnek (BASE)                |
| Kullanım Alanı | Finans, ERP     | Big Data, IoT, sosyal medya |

### ORM nedir? Entity Framework Core nedir?
**ORM**: OOP tabanlı programlama dillerinde veri tabanı tabloları ile bu tabloları temsil eden ürünlerin eşleştirilmesi ve bu nesnelerin üzerinden veri tabanında saklananken ihtiyaç odaklı sorguların otomatik olarak oluşturulmasını sağlayan bir yaklaşımdır.
**Entity Framework Core**:ORM yaklaşımını benimsemiş ve .NET platformu için geliştirilmiş bir ORM aracıdır. Kod üzerinde İlişiki sorgular, Görünüm, Saklı Prosedür, Fonksiyon, Geçici Tablo, Kısıtlama ve Sıra oluşturmamızı ve kullanmamızı sağlar.

### DbContext nedir, nasıl kullanılır?
DbContext Entity Framework Core'da veritabanı bağlantısını yöneten ve varlıklarınızın örneklerini sorgulamak ve kaydetmek için bir geçit görevi gören merkezi bir sınıftır.
public class UserService
{
    private readonly AppDbContext _context;

    public UserService(AppDbContext context)
    {
        _context = context;
    }

    public void AddUser(User user)
    {
        _context.Users.Add(user);
        _context.SaveChanges();
    }
}
SaveChanges():Yapılan tüm değişiklikleri veritabanına yazar

### LINQ nedir? En çok kullanılan LINQ ifadeleri
**LINQ**: veri koleksiyonlarını sorgulamak, verileri filtrelemek veya dönüştürmek gibi işlemleri kolaylaştıran güçlü bir teknolojidir. Liste, dizi, koleksiyon, veritabanı gibi veriler üzerinde SQL yazar gibi ama C# ile sorgu yapmanı sağlar.

**Where — Filtreleme**
Bir koleksiyon içinden belirli koşula uyan elemanları seçmek için kullanılır.

    LINQ
    var activeUsers = context.Users
       .Where(u => u.IsActive)
       .ToList();

    SQL
    SELECT * FROM Users
    WHERE IsActive = 1;

**Select — Dönüştürme**
Koleksiyonun her bir elemanını başka bir forma dönüştürmek için kullanılır.

    LINQ
    var names = context.Users
    .Select(u => u.Name)
    .ToList();

    SQL
    SELECT Name FROM Users;

 **OrderBy / OrderByDescending — Sıralama**
Koleksiyondaki elemanları küçükten büyüğe ya da büyükten küçüğe sıralar.

    LINQ
    var sortedDesc = context.Users
    .OrderByDescending(u => u.Age)
    .ToList();

    SQL
    SELECT * FROM Users
    ORDER BY Age DESC;

**Any — Var mı?**
Koleksiyonda belirli koşula uyan en az bir eleman olup olmadığını kontrol eder.

     LINQ
    bool exists = context.Users.Any(u => u.City == "İstanbul");

    SQL
    SELECT CASE 
    WHEN EXISTS (
        SELECT 1 FROM Users WHERE City = 'İstanbul'
    )
    THEN 1 ELSE 0
      END;

**Count — Eleman Sayısı**
Koleksiyondaki toplam eleman sayısını veya şartı sağlayanların sayısını döner.

    LINQ
    int count = context.Users.Count();

    SQL
    SELECT COUNT(*) FROM Users;


**GroupBy — Gruplama**
 Belirli bir özelliğe göre gruplama yapar.
 
    LINQ
    var groupByCity = context.Users
    .GroupBy(u => u.City)
    .Select(g => new {
        City = g.Key,
        Count = g.Count()
    })
    .ToList();

    SQL
    SELECT City, COUNT(*) AS Count
    FROM Users
    GROUP BY City;

### Code-First ve Database-First yaklaşımı nedir?
**Code-First yaklaşımı**
Önce kodu yazarsın, veritabanı koddan üretilir.Kod merkezlidir,Kod üzerinden doğrudan veri tabanı güncellemesi oluşturulabilir ve değişiklikler kolayca yönetilebilir.Kod üzerinden doğrudan veri tabanı güncellemesi oluşturulabilir ve değişiklikler kolayca yönetilebilir.

**Database-First yaklaşımı**
Önce veritabanı vardır, kod veritabanından üretilir.Veri yapısı nettir,Entity Framework, veritabanındaki tabloları baz alarak kod tarafında sınıfları otomatik olarak oluşturur.

| Özellik           | Code-First | Database-First |
| ----------------- | ---------- | -------------- |
| Başlangıç noktası | Kod        | Veritabanı     |
| Migration         | Var        | Sınırlı        |
| Yeni projeler     | Yapılır    | Yapılmaz       |
| Legacy sistemler  | Yok        | Var            |
| Esneklik          | Yüksek     | Düşük          |
| Domain odaklılık  | Yüksek     | Düşük          |

### Temel SQL sorguları: SELECT, INSERT, UPDATE, DELETE
**SELECT** : Veri tabanındaki verileri sorgulamak için kullanılır. Bu komut veri tabanındaki belirli bir tablodan belirli bir sütunu seçmek için kullanılır.

    SELECT * FROM Users;
    (Users tablosundaki tüm kayıtları getirir.)

WHERE: Komutu ile kayıtları filtrelemek için kullanılır.Yalnızca belirli koşulları sağlayan kayıtları çıkarmak için kullanılır.

    SELECT * FROM Users
    WHERE Age > 18;

**INSERT INTO** : Veritabanındaki tablolarımıza yeni veriler seçmek için kullanılır

    INSERT INTO Users (Name, Age, City)
    VALUES ('Ali', 30, 'İstanbul');
    (Users tablosuna yeni bir kullanıcı ekler)

**UPDATE** : Veritabanındaki bir kaydı güncellemek için kullanılır.

    UPDATE Users
    SET City = 'Ankara'
    WHERE Id = 1;
    (WHERE yoksa tüm tablo güncellenir!)

**DELETE**: Veritabanındaki verileri silmemizi sağlar.

    DELETE FROM Users
    WHERE Id = 1;
    (WHERE yoksa tüm tablo silinir!)

## 6. Güvenlik ve Performans
### Authentication vs Authorization nedir?
**Authentication (AuthN)**: Bir kişinin veya bir şeyin iddia ettiği gibi olduğunu doğrulayan bir süreçtir. Kimlik doğrulama, doğruluğunu kanıtlama anlamına gelmektedir.Arka planda, girdiğiniz kullanıcı adı ve parolayı veritabanında kayıtlı olan bilgilerle karşılaştırır. Gönderdiğiniz bilgilerle eşleşme sağlanırsa, sistem sizi geçerli bir kullanıcı olarak kabul eder ve erişim izni verir. 

**Authorization (authZ)**: Kimlik doğrulamasının ardından kullanıcının neye erişebileceğini ve ne tür işlemleri yapabileceğini belirler.Bir kullanıcı veya sistem bileşenine erişim kontrolü sağlamak için kullanılır ve bir kullanıcının yetkilerini tanımlamak için izinleri belirler. İzinler, kullanıcının veya sistemin belirli kaynaklara veya hizmetlere hangi koşullar altında erişebileceğini belirler. AuthZ süreci, genellikle belirli roller, gruplar veya izin seviyeleriyle ilişkilendirilmiş politikalar ve kurallar kullanır. Bu politika ve kurallar, yetkilendirme işlemlerini yönetmek ve uygulamak için kullanılır. Kullanıcıların sadece belirli kaynaklara veya hizmetlere erişmelerine izin verilirken, diğer kaynaklara veya hizmetlere erişimleri engellenebilir.

### JWT (JSON Web Token) nedir, nasıl çalışır?
JWT, kullanıcının doğrulanması, web servis güvenliği, bilgi güvenliği gibi birçok konuda kullanılabilir. 

**Header(Başlık)**
JWT’de kullanılacak bu kısım JSON formatında yazılmakta ve 2 alandan oluşmaktadır. Bunlar token tipi ve imzalama için kullanılacak algoritmanın adı.

**Payload(Veri)**
Bu kısım ‘claim’leri içerir. Bu kısımda tutulan veriler ile token istemci ve sunucu arasında eşsiz olur. Bu tutulan claim bilgileri de bu eşsizliği sağlar.
 1. Registered(Kayıtlı) claims: JWT tarafından önceden reserve edilmiş 3 harf uzunluğunda claimlerdir. Yani bu ayarlanmış belli claim isimlerini diğer claimlerde kullanamazsınız.
 2. Public (Açık) claims: İsteğe bağlı, açık yayınlanan claimlerdir.
 3. Private (Gizli) claims: Tarafların kendi aralarında bilgi taşımak için kullandığı gizli claimlerdir.

**Signature(İmza)**
Bu kısım tokenın son kısmıdır. Bu kısmın oluşturulabilmesi için header, payload ve gizli anahtar(secret) gereklidir. İmza kısmı ile veri bütünlüğü garanti altına alınır. Burada kullandığımız gizli anahtar Header kısmında belirttiğimiz algoritma için kullanılır. Header ve Payload kısımları bu gizli anahtar ile imzalanır

### OAuth, OAuth2.0, OpenIddict, OpenID nedir? Aralarındaki ilişki 

**OAuth**: Bir uygulamanın kullanıcının şifresini almadan başka bir servise erişmesine izin vermesini sağlayan protokoldür.

**OAuth 2.0**: OAuth’un modern ve yaygın kullanılan versiyonudur.Günümüzde kullanılan standarttır.Token (Access Token) mantığıyla çalışır.Mobil, web, API için uygundur.

**OpenID**: Kimlik doğrulama (authentication) için geliştirilmiş eski bir standarttır.

**OpenIddict**: .NET projelerinde:
OAuth 2.0
OpenID Connect
standartlarını uygulamanı sağlayan bir kütüphanedir.

| Kavram         | Ne yapar?                          |
| -------------- | ---------------------------------- |
| OAuth          | Yetkilendirme                      |
| OAuth 2.0      | Modern yetkilendirme standardı     |
| OpenID         | Kimlik doğrulama (eski)            |
| OpenIddict     | .NET’te OAuth/OIDC implementasyonu |

### Performans artımı için ne yapılabilir? (AsNoTracking, IAsyncEnumerable, caching, profiling, redis)
**AsNoTracking()**: EF Core’un Change Tracking (veritabanından çekilen nesneleri hafızada tutar ve neyin INSERT / UPDATE / DELETE edileceğine karar verir.) mekanizmasını kapatır. daha az bellek kullanır ve hızlı sorgu yapar

     var users = context.Users
                     .AsNoTracking()
                     .ToList();


**IAsyncEnumerable**: Veriyi parça parça (streaming) çeker, hepsini RAM’e yüklemez. düşük bellek kullanır

    await foreach (var user in context.Users.AsAsyncEnumerable())
    {
    Console.WriteLine(user.Name);
    }

**Caching (Önbellekleme)**: Sık okunan ama nadir değişen verileri tekrar DB’den çekmemek 

**Profiling**: Performans sorununu ölçelerek bulur

**Redis**: Bellekte çalışan, anahtar-değer yapısına dayalı bir veri deposudur. Sunucular arası paylaşım yapabilir

### OWASP Top 10
Web uygulamalarında güvvenlik açıkları listesi
### Web uygulamalarında en yaygın güvenlik açıklar         
  1. Injection
      Güvenilmeyen veriler, bir komut veya sorgunun bir parçası olarak bir yorumlayıcıya gönderildiğinde ortaya çıkar. Saldırganın verileri, yorumlayıcıyı istenmeyen komutları yürütmesi veya uygun yetkilendirme olmadan verilere erişmesi için kandırabilir.
  2. Broken Authentication
     Kimlik doğrulama ve oturum yönetimiyle ilgili işlevler, yanlış uygulandığında, saldırganların parolaları, anahtar sözcükleri ve oturumları tehlikeye atmasına olanak tanır.
  3. Sensitive Data Exposure
    Çoğu web uygulaması ve API, finans, sağlık hizmetleri ve PII(kişisel olarak tanımlabilir bilgiler) gibi hassas verileri gerektiği gibi koruyamaz. Çalmak isteyenler kredi kartı, kimlik hırsızlığı gibi şeyler çalabilir
  4. XML External Entities (XXE)
    Birçok eski veya kötü yapılandırılmış XML işlemci, XML belgelerindeki harici entity referanslarını değerlendirir. Bu kullanarak dahili dosyaları ifşa etmek için kullanılabilir
  5. Broken Access Control
    Kimliği doğrulanmış kullanıcıların ne yapmasına izin verildiğine ilişkin kısıtlamalar genellikle düzgün bir şekilde uygulanmaz. Hırsızlar diğer kullanıcı verilerine erişebilir
  6. Security Misconfiguration
     Güvenli olmayan varsayılan yapılandırmaların, eksik veya geçici yapılandırmaların, açık bulut depolamanın, yanlış yapılandırılmış HTTP headerının ve hassas bilgiler içeren ayrıntılı hata mesajlarının bir sonucudur.
  7. Cross-Site Scripting (XSS)
     Bir uygulama, yeni bir web sayfasında uygun doğrulama veya çıkış olmadan, güvenilir olmayan veriler içerdiğinde ortaya çıkar
  8. Insecure Deserialization
     Genellikle uzaktan kod yürütülmesine yol açar
  9. Using Components with Known Vulnerabilities
     Kütüphaneler, frameworksler ve diğer yazılım modülleri gibi bileşenler, uygulama ile aynı ayrıcalıklarla çalışır. Savunmasız bir bileşenden yararlanılırsa, bu tür bir saldırı, ciddi veri kaybını veya sunucunun ele geçirilmesini kolaylaştırabilir.
  10. Insufficient Logging & Monitoring
       Yetersiz günlük kaydı(insufficient logging) ve izleme(monitoring), olay yanıtı(incident response) ile eksik veya etkisiz entegrasyon ile birleştiğinde, saldırganların sistemlere daha fazla saldırmasına, kalıcılığı sürdürmesine, daha fazla sisteme dönmesine ve verileri kurcalamasına, çıkarmasına veya yok etmesine izin verir.

### SQL Injection, XSS, CSRF, Broken Auth gibi başlıkların kısa
tanımı
**SQL Injection**: kullanıcıdan alınan veriler sql sorgularına dehil edilerek yetkisiz erişim sağlanır
**XSS**: Zararlı JavaScript kodlarının uygulama üzerinden kullanıcı tarayıcısında çalıştırılması
**CSRF** : Web sitesinin açığından yararlanarak web sitesi kullanıcılarının istekleri dışında sanki o kullanıcıymış gibi erişerek işlem yapılması
**Broken Authentication**: Kimlik doğrulama ve oturum yönetiminin hatalı uygulanması nedeniyle hesapların ele geçirilmesi.
      
 ### ASP.NET Core ile alınabilecek önlemler (örnek: model validation, input sanitization)
 **Model Validation**: kullanıcıdan gelen veriiler kurallara uygun mu diye bakar
 **Input Sanitization**: Zararlı karakterleri temizler ve XSS saldılarını önler
 **Parametreli Sorgular / EF Core**: Parametreli sorgu üretir
 **Anti-CSRF**: Kullanıcı adı üzerinden sahte istek gönderilmesini önler
 **Authentication & Authorization**: Kimlik doğrulama ve ne yetkisi olduğu ayırmını yapar
 **HTTPS Zorunluluğu**: verilerin şifreli olmasını sağlar
 
## 7. Logging ve Hata Yönetim
 ### Neden loglama yapılır? Log seviyesi nedir?
 Yazılım geliştiricilerin hataları tespit etmek ve gidermek açısından önemli bir kaynaktır.Loglar bir yazılım sistemi içerisinde işleyişin ayak izleridir 
Log seviyesi, kaydedilecek logların önem derecesini belirler.
Her seviye, sistemin farklı bir durumunu temsil eder.
| Seviye          | Anlamı                             |
| --------------- | ---------------------------------- |
| **Trace**       | En detaylı, adım adım bilgi        |
| **Debug**       | Geliştirme sırasında hata ayıklama |
| **Information** | Normal sistem akışı                |
| **Warning**     | Potansiyel problem                 |
| **Error**       | İşlemi bozan hata                  |
| **Critical**    | Sistem çalışamaz durumda           |

### ASP.NET Core'da logging altyapısı
Uygulamanın en yaygın özelliklerinden biri, raporlama yeteneğidir. Bunlar yalnızca bir uygulamadan kaynaklanan sorunları gidermeye yardımcı olmakla kalmaz, aynı zamanda işlerin nasıl gittiğini takip etmeye ve sorunları daha kolay bir şekilde ortaya çıkarmak ve durdurmak için yardımcı olur
**ILoggerFactory**
ILoggerFactory, uygun bir ILogger türü örneği oluşturmak ve ayrıca ILoggerProvider örneğini eklemek için gerekli startup.cs sınıfında kullanılacak olan bir interface’dir

**ILoggerProvider**
Logger sınıfını oluşturmak ve yönetmek görevini üstlenen interface’dir.

**ILogger**
Loglamayı yapacak, Logger sınıfında kullanılacak olan interface’dir.

### Global exception handling nasıl yapılır?
Uygulamada oluşan tüm hataların tek bir merkezden yakalanıp yönetilmesini sağlar. Oluşan tüm hataların tek bir merkezden yakalanıp yönetilir

Middleware, ASP.NET Core’da HTTP request–response pipeline’ına giren ve isteği işleyip bir sonrakine ileten bileşendir.

UseExceptionHandler, ASP.NET Core’un hazır global exception handling middleware’idir.Uygulamada oluşan yakalanmamış hataları tek noktadan yönetir.

### UseExceptionHandler ve ILogger nasıl kullanılır?
**UseExceptionHandler**: Uygulamada oluşan yakalanmamış hataları global olarak yakalamak ve kullanıcıya güvenli cevap göndermek

    var builder = WebApplication.CreateBuilder(args);
    var app = builder.Build();
 
    app.UseExceptionHandler("/Error");

    app.MapControllers();
    app.Run();

**ILogger**: Uygulama içindeki olayları ve hataları loglamak için kullanılır.

    public class UserController : ControllerBase
    {
     private readonly ILogger<UserController> _logger;

    public UserController(ILogger<UserController> logger)
    {
        _logger = logger;
    }

    [HttpGet]
    public IActionResult Get()
    {
        _logger.LogInformation("User listesi istendi");

        try
        {
            throw new Exception("Test hata");
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Hata oluştu");
            throw; // Global handler yakalar
        }
    }
    }
    //(throw; → hatayı UseExceptionHandler’a gönderir)

## 8. Yazılım Geliştirme Prensipleri
### SOLID prensipleri: Her biri için kısa açıklama ve örnek
SOLID yazılım prensipleri; geliştirilen yazılımın esnek, yeniden kullanılabilir, sürdürülebilir ve anlaşılır olmasını sağlayan, kod tekrarını önler 

- **S — Single-responsibility principle**
Bir sınıf yalnızca bir amaç uğruna değiştirilebilir, o da o sınıfa yüklenen sorumluluktur, yani bir sınıfın yapması gereken yalnızca bir işi olması gerekir

- **O — Open-closed principle**
Bir sınıf ya da fonksiyon halihazırda var olan özellikleri korumalı ve değişikliğe izin vermemelidir. Yani davranışını değiştirmiyor olmalı ve yeni özellikler kazanabiliyor olmalıdır

- **L — Liskov substitution principle**
Kodlarımızda herhangi bir değişiklik yapmaya gerek duymadan alt sınıfları, türedikleri(üst) sınıfların yerine kullanabilmeliyiz

- **I — Interface segregation principle**
Sorumlulukların hepsini tek bir arayüze toplamak yerine daha özelleştirilmiş birden fazla arayüz oluşturmalıyı

- **D — Dependency Inversion Principle**
Sınıflar arası bağımlılıklar olabildiğince az olmalıdır özellikle üst seviye sınıflar alt seviye sınıflara bağımlı olmamalıdır

### Design Patterns: Singleton, Repository, Factory
- **Singleton** : Bir sınıftan uygulama boyunca yalnızca tek bir nesne oluşturulmasını sağlar. Bir sınıfın tek bir instance’ının olmasını ve her yerden erişilmesini sağlar.

- **Repository** : Veri erişim katmanını soyutlayarak iş mantığını (business) veritabanından ayırmak. Veri erişim işlemlerini tek bir noktada toplayan tasarım desenidir.

- **Factory** : Nesne oluşturma işlemini merkezi bir yapıdan yapmak. Hangi nesnenin oluşturulacağını istemciden gizleyen tasarım desenidir.

### Clean Code nedir, neden önemlidir?
Kodun temiz anlaşılabilir olması ve kod yazan geliştiricinin dışında ekipteki kodun kolay şekilde anlaşılabilmesi ve geliştirilebilmesidir.

**Okunabilirlik**: Kod daha çabuk anlaşılır yeni gelen geliştirici projeyi çabuk anlar
**Değiştirilebilirlik**: Kod kolayca değiştirilebiliyor yeni özellikler eklenebiliyor
**Genişletilebilirlik**: Kod basitçe bozulmadan genişletirilebilir
**Sürdürülebilirlik**: Kod bozulmadan bakımı yapılabiliyor

### Yazılım Mimari Desenleri
 
### Layered, Clean Architecture, Microservices, Event-Driven, Hexagonal Architecture (Ports & Adapters)

- **Layered Architecture**: Uygulamanın sorumluluklerına göre katmanlar haline gelmesi
- **Clean Architecture**: Framework ve dış bağımlılıklardan bağımsız hale getirmesi ve bağımlılıkları merkeze doğru yönlendirerek sürdürülebilir ve test edilebilir sistemler kurmayı amaçlar
- **Microservices Architecture**: Uygulamayı bağımsız küçük servisler halinde bilünmesi
- **Event-Driven Architecture**: Olaylar üzerinden asenkron iletişim kurar
- **Hexagonal Architecture (Ports & Adapters)** : Uygulama çekirdeğini dış dünyadan tamamen koparır

| Mimari                           | Temel Amaç            | Avantajlar                      | Dezavantajlar               | Uygun Senaryo                      |
| -------------------------------- | --------------------- | ------------------------------- | --------------------------- | ---------------------------------- |
| **Layered**                      | Katmanlı düzen        | Basit, anlaşılır                | Katmanlar arası bağımlılık  | Küçük–orta CRUD projeleri          |
| **Clean Architecture**           | İş kurallarını koruma | Test edilebilir, sürdürülebilir | Kurulum karmaşık            | Uzun ömürlü projeler               |
| **Microservices**                | Bağımsız servisler    | Ölçeklenebilir, bağımsız deploy | Dağıtık sistem karmaşıklığı | Büyük ve yüksek trafikli sistemler |
| **Event-Driven**                 | Asenkron iletişim     | Gevşek bağlılık, performans     | Debug zor                   | Gerçek zamanlı sistemler           |
| **Hexagonal (Ports & Adapters)** | Bağımlılık izolasyonu | Kolay test, esnek yapı          | Öğrenme eğrisi              | DB/UI değişken projeler            |
| **Monolithic**                   | Tek uygulama          | Kolay geliştirme                | Ölçeklenemez                | Küçük projeler                     |
| **MVC**                          | UI ayrımı             | Düzenli arayüz                  | UI odaklı                   | Web uygulamaları                   |

### Hangi senaryoda hangi mimari tercih edilir?

| Senaryo / İhtiyaç                 | Proje Özellikleri                   | Tercih Edilen Mimari             | Neden Bu Mimari?                    |
| --------------------------------- | ----------------------------------- | -------------------------------- | ----------------------------------- |
| **Küçük ve basit uygulama**       | Az kullanıcı, basit CRUD, kısa süre | **Monolithic / Layered**         | Hızlı geliştirme, düşük karmaşıklık |
| **Web arayüzlü uygulama**         | UI odaklı, controller–view yapısı   | **MVC**                          | UI ve iş mantığı ayrımı             |
| **Kurumsal ve uzun ömürlü proje** | İş kuralları yoğun, sık değişim     | **Clean Architecture**           | İş mantığı korunur, bakım kolay     |
| **Test edilebilirlik öncelikli**  | Unit test, mock ihtiyacı            | **Hexagonal (Ports & Adapters)** | Dış bağımlılıklar izole edilir      |
| **Yüksek trafik**                 | Binlerce kullanıcı, performans      | **Microservices**                | Bağımsız ölçekleme                  |
| **Büyük ekipli projeler**         | Birden fazla ekip                   | **Microservices**                | Servis bazlı ekipler                |
| **Asenkron işlemler**             | Bildirim, sipariş, log              | **Event-Driven**                 | Gevşek bağlı, hızlı iletişim        |
| **Gerçek zamanlı sistem**         | Anlık veri akışı                    | **Event-Driven**                 | Olay tabanlı mimari                 |
| **Değişken altyapı**              | DB / UI / API değişebilir           | **Hexagonal**                    | Adaptör değişir, core sabit         |
| **Hızlı MVP / PoC**               | Deneme projesi                      | **Monolithic**                   | En hızlı başlangıç                  |
