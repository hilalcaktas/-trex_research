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
.NET Framework    ---    .NET Core           ---      .NET 7/8+
windowsda çalışır ---  windows,linux,macOS   ---   windows,linux,macOS
güncelleme yok    ---   güncelleme var       ---    güncelleme var
ilk sürüm         ---  modern ve yenilenmiş  ---  modern yenilenmiştir .NET Core’un devamı ve tek çatı altındaki hali
orta performans   --- yüksek performans      ---   çok yüksek performans 
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
## 3. Backend Geliştirme Temelleri
###  Backend nedir? Frontend ile farkları
**Backend** : arayüzün arkasındaki veri ve işlerin çalışmasını sağlayan taraftır. verileri kaydeder, güvenliği yüksektir, sunucuda çalışır, kullanıcıya görünmez
**Frontend** : kullanıcının gördüğü kullandığı arayüz kısmıdır. verileri gösterir,güvenliği düşüktür, tarayıcıda çalışır
### Web sunucusu nedir? API nedir? API türleri
Web sunucusu, internet üzerinden gelen isteklere yanıt verir
API, iki yazılımın birbiriyle iletişim kurmasını sağlar
--Kullanım Amaçlarına Göre API Türleri--
**Internal API** : belirli kişiler kullanabilir, yeniden kullanım ve üretim için kullanılır
**Open API** : herkese açıktır
**Partner API** : birbiriyle ortaklık yapıp iş yapan yerlerle arasında kullanılır
**Composite API** : birden çok veriyi bir araya getirir ve tek bir çağrıda birden çok uç noktaya erişilmesini sağlar
--Mimari Yapıya Göre API Türleri--
**REST API** :modern web tabanlı uygulamalar oluşturmak için sıklıkla kullanılır. HTTP kullanıır
**SOAP API** : sıkı güvenlikle veri aktarımı sağlar
**GraphQL API** : istemcinin istedikleri şeyi onlara verir
### HTTP nedir? HTTP metodları: GET, POST, PUT, DELETE
HTTP, istemci ve sunucu arasındaki iletişimi sağlar
**GET** : sunucudan veri almak için kullanılır
**POST** : yeni veri oluşturmak için kullanılır
**PUT** : veriyi güncellemek için kullanılır
**DELETE** : veriyi silmek için kullanılır
### RESTful servislerin çalışma mantığı























