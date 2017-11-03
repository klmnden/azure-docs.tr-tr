---
title: "Mobil uygulamaları için Node.js arka uç sunucusu SDK ile çalışmaya nasıl | Microsoft Docs"
description: "Azure App Service Mobile Apps için Node.js arka uç sunucusu SDK ile çalışmayı öğrenin."
services: app-service\mobile
documentationcenter: 
author: elamalani
manager: elamalani
editor: 
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: ab1a9dfa71c4b633392ef839bb848347fdd26431
ms.sourcegitcommit: d6ad3203ecc54ab267f40649d3903584ac4db60b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2017
---
# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Azure Mobile Apps Node.js SDK'sını kullanma
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Bu makale, ayrıntılı bilgiler ve Azure App Service Mobile Apps Node.js arka ucu ile nasıl çalışılacağını gösteren örnekler sağlar.

## <a name="Introduction"></a>Giriş
Azure App Service Mobile Apps bir web uygulamasına mobile için iyileştirilmiş veri erişimi Web API ekleme yeteneği sağlar.  Azure App Service Mobile Apps SDK'sı, ASP.NET ve Node.js web uygulamaları için sağlanır.  SDK, aşağıdaki işlemleri sağlar:

* Veri erişimi için tablo işlemleri (okuma, INSERT, Update, Delete)
* Özel API işlemleri

İki işlem kimlik doğrulaması için Kurumsal kimlik için Facebook, Twitter, Google ve Microsoft yanı sıra Azure Active Directory gibi sosyal kimlik sağlayıcıları dahil olmak üzere Azure App Service tarafından izin verilen tüm kimlik sağlayıcıları üzerinden sağlar.

Her kullanım örneğine ilgili örnekleri bulabilirsiniz [örnekler dizini github'da].

## <a name="supported-platforms"></a>Desteklenen platformlar
Azure Mobile Apps düğümü SDK geçerli LTS sürüm düğümünün ve daha sonra destekler.  Makalenin yazıldığı sırada son LTS düğümü v4.5.0 sürümüdür.  Düğüm'ın diğer sürümleri çalışabilir, ancak desteklenmez.

Azure Mobile Apps düğümü SDK iki veritabanı sürücülerini destekler - düğüm mssql sürücü SQL Azure ve yerel SQL Server örnekleri destekler.  Sqlite3 sürücü üzerinde yalnızca tek bir örneği SQLite veritabanlarını destekler.

### <a name="howto-cmdline-basicapp"></a>Nasıl yapılır: komut satırını kullanarak bir temel Node.js arka ucu oluşturma
Her Azure App Service Mobile uygulama Node.js arka ucu bir ExpressJS uygulamayı başlatır.  ExpressJS en popüler web service Node.js için kullanılabilir çerçevesidir.  Temel bir oluşturabileceğiniz [Express] şekilde uygulama:

1. Bir komut veya PowerShell penceresi projeniz için bir dizin oluşturun.

        mkdir basicapp
2. Paket yapısı başlatmak için npm init çalıştırın.

        cd basicapp
        npm init

    Npm init komutu bir dizi proje başlatılamadı sorular sorar.  Örnek çıktı bakın:

    ![Npm init çıktı][0]
3. Hızlı ve azure mobile apps kitaplıkları npm depodan yükleyin.

        npm install --save express azure-mobile-apps
4. Temel mobil sunucu uygulamak için bir app.js dosyası oluşturun.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Bu uygulama ile tek bir uç noktası mobil iyileştirilmiş Webapı oluşturur (`/tables/TodoItem`) dinamik Şeması'nı kullanarak bir arka plandaki SQL veri deposu kimlik doğrulamasız erişim sağlar.  İstemci Kitaplığı hızlı başlangıçlar izlemek için uygundur:

* [Android istemci hızlı başlangıç]
* [Apache Cordova istemci hızlı başlangıç]
* [iOS istemci hızlı başlangıç]
* [Windows mağazası istemci hızlı başlangıç]
* [Xamarin.iOS istemcisi hızlı başlangıç]
* [Xamarin.Android istemcisi hızlı başlangıç]
* [Xamarin.Forms istemci hızlı başlangıç]

Bu temel uygulamada kodu bulabilirsiniz [basicapp örnek github'da].

### <a name="howto-vs2015-basicapp"></a>Nasıl yapılır: Visual Studio 2015 ile birlikte bir düğüm arka ucu oluşturma
Visual Studio 2015 IDE içinden Node.js uygulamaları geliştirmek için uzantı gerektirir.  Başlatmak için Yükle [Visual Studio için Node.js araçları 1.1].  Visual Studio için Node.js araçları yüklendikten sonra bir Express 4.x uygulaması oluşturun:

1. Açık **yeni proje** iletişim (gelen **dosya** > **yeni** > **proje...** ).
2. Genişletme **şablonları** > **JavaScript** > **Node.js**.
3. Seçin **temel Azure Node.js Express 4 uygulama**.
4. Proje adı girin.  *Tamam*’a tıklayın.

    ![Visual Studio 2015 yeni proje][1]
5. Sağ **npm** düğümü ve select **yüklemek yeni npm paket...** .
6. İlk Node.js uygulamanızı oluşturma npm katalog yenilemeniz gerekebilir.  Tıklatın **yenileme** gerekiyorsa.
7. Girin *azure mobile apps* arama kutusuna.  Tıklatın **azure mobile apps 2.0.0** paketini ve ardından **paket yükleme**.

    ![Yeni npm paket yüklemek için][2]
8. **Kapat**’a tıklayın.
9. Açık *app.js* Azure Mobile Apps SDK'sı için destek eklemek için dosya.  6 at kitaplığı alt satırında deyimleri gerektirir, aşağıdaki kodu ekleyin:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Yaklaşık satırında 27 diğer app.use deyimleri sonra aşağıdaki kodu ekleyin:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Dosyayı kaydedin.
10. (API http://localhost: 3000 üzerinde sunulur) uygulamayı yerel olarak çalıştırın ya da Azure'a yayımlayacaksınız.

### <a name="create-node-backend-portal"></a>Nasıl yapılır: Azure portalını kullanarak bir Node.js arka ucu oluşturma
Bir mobil uygulama arka uç hakkı oluşturabilirsiniz [Azure Portal]. Aşağıdaki adımları izleyin veya izleyerek bir istemci ve sunucu birlikte oluşturabilirsiniz [mobil uygulama oluşturma](app-service-mobile-ios-get-started.md) Öğreticisi. Öğreticinin bu yönergeleri basitleştirilmiş bir sürümünü içerir ve kavram projeleri kanıtı için en iyisidir.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Geri *başlama* dikey altında **tablo API Oluştur**, seçin **Node.js** olarak, **arka uç dilinizi**.
Onay kutusunu için "**bu tüm site içeriğini. üzerine yazacağını kabul ediyorum**", ardından **Todoıtem tablosu oluştur**.

### <a name="download-quickstart"></a>Nasıl yapılır: Git kullanarak Node.js arka uç hızlı başlangıç kod projesi indirme
Portalı kullanarak bir Node.js mobil uygulama arka ucu oluşturduğunuzda **Hızlı Başlangıç** dikey penceresinde, bir Node.js projesi için oluşturduğunuz ve sitenize dağıtılır. Tabloları ve API'ları ekleyin ve Portalı'nda Node.js arka ucu için kod dosyaları düzenleyin. Böylece ekleyin veya tablolar ve API'ları değiştirmek sonra projeyi yeniden yayımlamanız arka uç projesini indirmek için çeşitli dağıtım araçları da kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure App Service Deployment Guide]. Aşağıdaki yordam, hızlı başlangıç projesi kodu indirmek için bir Git deposu kullanır.

1. Zaten yapmadıysanız Git'i, yükleyin. Git yüklemek için gereken adımlar, işletim sistemleri arasında farklılık gösterir. Bkz: [yükleme Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) işletim sistemine özgü dağıtımları ve yükleme yönergeleri için.
2. Adımları [uygulama hizmeti uygulama havuzu etkinleştirmek](../app-service/app-service-deploy-local-git.md#Step3) dağıtım kullanıcı adı ve parola Not yapmadan arka uç sitenizin Git deposunu etkinleştirmek için.
3. Mobil uygulama arka ucu için dikey penceresinde, Not **Git kopyalama URL'si** ayarı.
4. Yürütme `git clone` Git kullanarak komutu kopyalama, aşağıdaki örnekte olduğu gibi gerekli olduğunda parolanızı girmeden URL:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. Olan önceki örnekte /todolist yerel dizinine göz atın ve proje dosyalarını yüklendi dikkat edin. Bulun `todoitem.json` dosyasını `/tables` dizin.  Bu dosya, tablo üzerinde izinleri tanımlar.  Ayrıca bulmak `todoitem.js` , CRUD işlemi tanımlar aynı dizinde dosya tablosu için komut dosyası.
6. Proje dosyalarına değişiklikleri yaptıktan sonra Ekle, yürütme ve ardından değişiklikleri siteye yüklemek için aşağıdaki komutları yürütün:

        $ git commit -m "updated the table script"
        $ git push origin master

    Projeye yeni dosyalar eklediğinizde, önce yürütülecek gerekir `git add .` komutu.

İşlemeleri yeni bir dizi siteye gönderilen her zaman site yeniden yayımlanacak.

### <a name="howto-publish-to-azure"></a>Nasıl yapılır: Node.js arka ucu için Azure yayımlama
Microsoft Azure, Azure App Service Mobile Apps Node.js arka uç Azure hizmetine yayımlamak için birçok mekanizma sağlar.  Bunlar, Visual Studio'ya tümleşik dağıtım araçları, komut satırı araçları ve kaynak denetimine bağlı sürekli dağıtım seçenekleri kullanılarak içerir.  Bu konu hakkında daha fazla bilgi için bkz: [Azure App Service Deployment Guide].

Azure uygulama hizmeti dağıtmadan önce gözden geçirmeniz gereken Node.js uygulama için ilgili bir uzmana sahiptir:

* Nasıl yapılır [düğümü sürüm belirtin]
* Nasıl yapılır [düğümü modüllerini kullanma]

### <a name="howto-enable-homepage"></a>Nasıl yapılır: bir giriş sayfası, uygulamanız için etkinleştirme
Birçok uygulama, web ve mobil uygulamaları birleşimidir ve ExpressJS framework, iki modelleri birleştirmenize izin verir.  Bazı durumlarda, ancak yalnızca bir mobil arabirimi uygulamak isteyebilirsiniz.  Uygulama hizmeti emin olmak için bir giriş sayfası çalışır durumda sağlamak kullanışlıdır.  Giriş sayfası sağlayın veya geçici bir giriş sayfası etkinleştirin.  Geçici bir giriş sayfasını etkinleştirmek için Azure Mobile Apps örneği oluşturmak için aşağıdakileri kullanın:

    var mobile = azureMobileApps({ homePage: true });

Yalnızca kullanılabilen bu seçenek yerel olarak geliştirirken istiyorsanız, bu ayar ekleyebilirsiniz, `azureMobile.js` dosya.

## <a name="TableOperations"></a>Tablo işlemleri
Azure mobile apps Node.js sunucusu SDK'sı bir Webapı olarak Azure SQL veritabanında depolanan veri tabloları kullanıma sunmak için mekanizma sağlar.  Beş işlem sağlanır.

| İşlem | Açıklama |
| --- | --- |
| GET /tables/*tablename* |Tüm kayıtların tabloda Al |
| GET /tables/*tablename*/:id |Belirli bir kayıt tabloda Al |
| POST /tables/*tablename* |Tabloda bir kayıt oluşturun |
| Düzeltme eki /tables/*tablename*/:id |Tablodaki bir kaydı güncelleştirmeye |
| DELETE /tables/*tablename*/:id |Tabloyu kayıt silme |

Bu Webapı destekleyen [OData] ve desteklemek için tablo şemasını genişletir [çevrimdışı veri eşitlemeye].

### <a name="howto-dynamicschema"></a>Nasıl yapılır: dinamik bir şema kullanarak tabloları tanımlayın
Bir tablo kullanılabilmesi için tanımlanmalıdır.  Tabloları (burada şeması içindeki sütun tanımlar Geliştirici) statik bir şema veya dinamik olarak tanımlanabilir (burada SDK gelen istekleri temel alan şema denetler). Ayrıca, geliştirici tanımına Javascript kodu ekleyerek Webapı belirli yönlerini kontrol edebilir.

En iyi uygulama, tablolar dizindeki Javascript dosyasında her tablo tanımlamanız sonra tabloları tables.import() yöntemi kullanın.  Temel uygulama genişletme, app.js dosya ayarlanması:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Tabloda tanımlayın. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Tablolar, varsayılan olarak dinamik şema kullanın.  Dinamik şema genel etkinleştirmek için uygulama ayarı **MS_DynamicSchema** Azure portalındaki false.

Tam bir örnek bulabilirsiniz [Yapılacaklar örneği github'daki].

### <a name="howto-staticschema"></a>Nasıl yapılır: statik Şeması'nı kullanarak tabloları tanımlayın
Açıkça Webapı kullanıma sunmak için sütunları tanımlayabilirsiniz.  Azure mobile apps Node.js SDK'sı çevrimdışı veri eşitlemeye sağladığınız listesine için gerekli ek sütunlar otomatik olarak ekler.  Örneğin, hızlı başlangıç istemci uygulamaları iki sütun içeren bir tablo gerektirir: metin (dize) ve (Boole) tamamlayın.  
Tablonun (tablolar dizininde bulunur) tablo tanımı JavaScript dosyasında şu şekilde tanımlanabilir:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Tabloları statik olarak tanımlarsanız, başlangıçta veritabanı şeması oluşturmak için tables.initialize() yöntemini de çağırmalıdır.  Tables.initialize() yöntemi döndürür bir [Promise] böylece web hizmeti isteklerinin başlatılmakta veritabanı önce sunmuyor.

### <a name="howto-sqlexpress-setup"></a>Nasıl yapılır: yerel makinenizde geliştirme veri deposu olarak kullanmak SQL Express'i
Azure Mobile Apps AzureMobile uygulamaları düğümü SDK veri kutudan çıktığında hizmet veren için üç seçenek sunar: SDK, veri kutudan çıktığında hizmet veren için üç seçenek sağlar:

* Kullanım **bellek** kalıcı olmayan örnek depolama sağlamak için sürücü
* Kullanım **mssql** geliştirme için bir SQL Express veri deposu sağlamak için sürücü
* Kullanım **mssql** üretim için bir Azure SQL veritabanı veri deposu sağlamak için sürücü

Azure Mobile Apps Node.js SDK'sı [mssql Node.js paket] kurmak ve SQL Express ve SQL veritabanına bir bağlantı kullanın.  Bu paket, SQL Express örneği üzerinde TCP bağlantılarını etkinleştirmenizi istemektedir.

> [!TIP]
> Bellek sürücüsü eksiksiz test olanaklarının sağlamaz.  Arka ucunu yerel olarak test etmek isterseniz, SQL Express veri deposu ve mssql sürücü kullanılmasını öneririz.
>
>

1. İndirme ve yükleme [Microsoft SQL Server 2014 Express].  Araçlar sürümü ile SQL Server 2014 Express yükle emin olun.  64-bit desteği açıkça gerekmiyorsa 32-bit sürümünü çalışırken daha az bellek kullanır.
2. SQL Server 2014 Yapılandırma Yöneticisi'ni çalıştırın.

   1. Genişletme **SQL Server Ağ Yapılandırması** sol ağaç menü düğümünde.
   2. Tıklatın **SQLEXPRESS protokolleri**.
   3. Sağ **TCP/IP'yi** seçip **etkinleştirmek**.  Tıklatın **Tamam** açılan iletişim kutusunda.
   4. Sağ **TCP/IP'yi** seçip **özellikleri**.
   5. Tıklatın **IP adreslerini** sekmesi.
   6. Bul **IPAll** düğümü.  İçinde **TCP bağlantı noktası** alanına, **1433**.

          ![Configure SQL Express for TCP/IP][3]
   7. **Tamam** düğmesine tıklayın.  Tıklatın **Tamam** açılan iletişim kutusunda.
   8. Tıklatın **SQL Server Hizmetleri** sol ağaç menüsünde.
   9. Sağ **SQL Server (SQLEXPRESS)** seçip **yeniden başlatın**
   10. SQL Server 2014 Yapılandırma Yöneticisi'ni kapatın.
3. SQL Server 2014 Management Studio'yu çalıştırın ve yerel SQL Express örneği için Bağlan

   1. Nesne Gezgini'nde örneğinizi sağ tıklatıp **özellikleri**
   2. Seçin **güvenlik** sayfası.
   3. Olun **SQL Server ve Windows kimlik doğrulaması modu** seçilir
   4. **Tamam**’a tıklayın.

          ![Configure SQL Express Authentication][4]
   5. Genişletme **güvenlik** > **oturumları** nesne Gezgini'nde
   6. Sağ **oturumları** seçip **yeni oturum açma...**
   7. Bir oturum açma adı girin.  **SQL Server kimlik doğrulaması**’nı seçin.  Bir parola girin ve ardından aynı parolayı girmeniz **parolayı onayla**.  Parola Windows karmaşıklık gereksinimlerini karşılaması gerekir.
   8. **Tamam**’a tıklayın.

          ![Add a new user to SQL Express][5]
   9. Yeni oturum açma sağ tıklatıp **özellikleri**
   10. Seçin **sunucu rolleri** sayfası
   11. Yanındaki kutuyu işaretleyin **dbcreator** sunucu rolü
   12. **Tamam**’a tıklayın.
   13. SQL Server 2015 Management Studio'yu kapatın

Kullanıcı adı ve parola seçtiğiniz kayıt emin olun.  Ek sunucu rollerini veya belirli veritabanı gereksinimlerinize bağlı olarak izinleri atamanız gerekebilir.

Node.js uygulaması okuma **SQLCONNSTR_MS_TableConnectionString** bu veritabanı için bağlantı dizesi için ortam değişkeni.  Ortamınızda bu değişkeni ayarlayabilirsiniz.  Örneğin, bu ortam değişkenini ayarlamak için PowerShell kullanın:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Veritabanına bir TCP/IP bağlantısı üzerinden erişmek ve bağlantı için bir kullanıcı adı ve parola sağlayın.

### <a name="howto-config-localdev"></a>Nasıl yapılır: projeniz yerel geliştirme için yapılandırma
Azure Mobile Apps okur adlandırılan bir JavaScript dosyası *azureMobile.js* yerel dosya sistemi gelen.  Bu dosyayı kullanmayın üretimde Azure Mobile Apps SDK'sını yapılandırmak-uygulaması ayarları içinde kullanmak [Azure Portal] yerine.  *AzureMobile.js* dosyasına bir yapılandırma nesnesi.  En yaygın ayarlar şunlardır:

* Veritabanı ayarları
* Tanılama günlük ayarları
* Alternatif CORS ayarları

Örnek *azureMobile.js* önceki veritabanı ayarlarını uygulama dosya izler:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Eklediğiniz öneririz *azureMobile.js* için *.gitignore* dosya (veya diğer kaynak kodu denetimi dosyayı yoksay) bulutta depolanan parolaları önlemek için.  Her zaman uygulama ayarlarında üretim ayarlarını yapılandırmak [Azure Portal].

### <a name="howto-appsettings"></a>: Mobil uygulamanız için uygulama ayarlarını yapılandır
Çoğu ayarlarında *azureMobile.js* dosyanız eşdeğer bir uygulama ayarı [Azure Portal].  Uygulama ayarlarında Uygulamanızı yapılandırmak için aşağıdaki listeye bakın:

| Uygulama ayarı | *azureMobile.js* ayarı | Açıklama | Geçerli Değerler |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |ad |Uygulama adı |Dize |
| **MS_MobileLoggingLevel** |Logging.level |En küçük günlük düzeyi günlüğe kaydedilecek ileti sayısı |hata, uyarı, bilgi, ayrıntılı, hata ayıklama, saçma |
| **MS_DebugMode** |Hata ayıklama |Etkinleştirme veya devre dışı bırak hata ayıklama modu |TRUE, false |
| **MS_TableSchema** |Data.Schema |SQL tablolarının varsayılan şema adı |dize (varsayılan: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Etkinleştirme veya devre dışı bırak hata ayıklama modu |TRUE, false |
| **MS_DisableVersionHeader** |(çok undefined olarak ayarlanır) sürümü |X-ZUMO-Server-Version üstbilgi devre dışı bırakır |TRUE, false |
| **MS_SkipVersionCheck** |skipversioncheck |İstemcisi API sürümü denetimi devre dışı bırakır |TRUE, false |

Bir uygulama ayarı ayarlamak için:

1. [Azure Portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** mobil uygulamanızın adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır. ' I içermiyorsa tıklatırsanız **ayarları**.
4. Tıklatın **uygulama ayarları** genel menüsünde.
5. Uygulama ayarları bölümüne gidin.
6. Uygulamanızı ayarı zaten varsa, değeri düzenlemek için uygulama ayarının değeri'ı tıklatın.
7. Uygulama ayarı mevcut değilse, uygulama ayarı anahtar kutusuna ve değer kutusundaki değeri girin.
8. Tamamlandıktan sonra tıklatın **kaydetmek**.

Çoğu uygulama ayarlarını değiştirme hizmetini yeniden başlatma gerektirir.

### <a name="howto-use-sqlazure"></a>Nasıl yapılır: SQL Database üretim veri deposu olarak kullanın
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Bir veri deposu olarak Azure SQL veritabanı kullanan tüm Azure App Service uygulama türlerine aynıdır. Henüz yapmadıysanız, mobil uygulama arka ucu oluşturmak için aşağıdaki adımları izleyin.

1. [Azure Portal]’da oturum açın.
2. Üst pencerenin sol, tıklama **+ yeni** düğmesi > **Web + mobil** > **mobil uygulama**, mobil uygulama arka ucu için bir ad sağlayın.
3. İçinde **kaynak grubu** kutusuna, uygulamanızı aynı adı girin.
4. Varsayılan uygulama hizmeti planı seçilir.  Uygulama hizmeti planınızı değiştirmek istiyorsanız, uygulama hizmeti planı tıklayarak bunu yapabilirsiniz > **+ Yeni Oluştur**.  Yeni uygulama hizmeti planının adı sağlayın ve uygun bir konum seçin.  Fiyatlandırma Katmanı'nı tıklatın ve hizmet için uygun bir fiyatlandırma katmanı seçin. Seçin **tüm görüntüle** daha seçenekleri gibi fiyatlandırma görünümüne **serbest** ve **paylaşılan**.  Fiyatlandırma katmanı seçildiğinde tıklatın **seçin** düğmesi.  Geri **uygulama hizmeti planı** dikey penceresinde tıklatın **Tamam**.
5. **Oluştur**'a tıklayın. Bir mobil uygulama arka ucu sağlama birkaç dakika sürebilir.  Mobil uygulama arka ucu sağlandıktan sonra portal açar **ayarları** mobil uygulama arka ucu için dikey.

Mobil uygulama arka ucu oluşturulduktan sonra mevcut bir SQL veritabanını, mobil uygulamanızın arka ucuna bağlanmak veya yeni bir SQL veritabanı oluşturmak seçebilirsiniz.  Bu bölümde, bir SQL veritabanı oluşturun.

> [!NOTE]
> Mobil uygulama arka ucu ile aynı konumda zaten bir veritabanınız varsa, bunun yerine seçebileceğiniz **varolan veritabanını kullan** sonra bu veritabanını seçin. Bir veritabanı farklı bir konumda kullanımını daha yüksek gecikme nedeniyle önerilmez.
>
>

1. Yeni mobil uygulama arka ucunda tıklatın **ayarları** > **mobil uygulama** > **veri** > **+ Ekle**.
2. İçinde **veri bağlantısı Ekle** dikey penceresinde tıklatın **SQL veritabanı - gerekli ayarları Yapılandır** > **yeni bir veritabanı oluşturmak**.  Yeni veritabanı adını girin **adı** alan.
3. Tıklatın **Server**.  İçinde **yeni sunucu** dikey penceresinde bir benzersiz sunucu adı girin **sunucu adı** alan ve uygun bir sağlamak **Sunucu Yöneticisi oturum açma** ve **parola**.  Olun **azure hizmetlerinin sunucuya erişmesine izin** denetlenir.  **Tamam** düğmesine tıklayın.

    ![Bir Azure SQL veritabanı oluşturma][6]
4. Üzerinde **yeni veritabanı** dikey penceresinde tıklatın **Tamam**.
5. Geri **veri bağlantısı Ekle** dikey penceresinde, select **bağlantı dizesi**, oturum açma ve veritabanı oluşturulurken sağlanan parola girin.  Varolan bir veritabanını kullanıyorsanız, bu veritabanı için oturum açma kimlik bilgilerini sağlayın.  Girilen sonra tıklayın **Tamam**.
6. Geri **veri bağlantısı Ekle** dikey penceresinde tekrar tıklatın **Tamam** veritabanı oluşturulamıyor.

<!--- END OF ALTERNATE INCLUDE -->

Veritabanı oluşturma, birkaç dakika sürebilir.  Kullanım **bildirimleri** dağıtımın ilerleme durumunu izlemek için alan.  Veritabanı başarıyla dağıtıldığını kadar ilerleme değil.  Başarılı bir şekilde dağıtıldığında, bir bağlantı dizesi mobil arka uç uygulaması ayarları SQL veritabanı örneğinde için oluşturulur.  Bu uygulama ayarını görebilirsiniz **ayarları** > **uygulama ayarları** > **bağlantı dizeleri**.

### <a name="howto-tables-auth"></a>Nasıl yapılır: tablolara erişimi kimlik doğrulaması iste
Tabloları noktayla App Service kimlik doğrulaması kullanmak isterseniz, App Service kimlik doğrulaması yapılandırmalısınız [Azure Portal] ilk.  Bir Azure uygulama hizmetinde kimlik doğrulaması yapılandırma hakkında daha fazla ayrıntı için kullanmak istediğiniz kimlik sağlayıcısı için Yapılandırma Kılavuzu gözden geçirin:

* [Azure Active Directory kimlik doğrulaması yapılandırma]
* [Facebook kimlik doğrulaması yapılandırma]
* [Google kimlik doğrulamasını yapılandırma]
* [Microsoft Authentication yapılandırma]
* [Twitter kimlik doğrulamasını yapılandırma]

Her tablo tablosuna erişimi denetlemek için kullanılan bir erişim özelliğine sahiptir.  Aşağıdaki örnek, kimlik doğrulaması gerekli statik olarak tanımlanmış bir tablo gösterir.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Access özelliği üç değerden birini alabilir

* *Anonim* istemci uygulamanın kimlik doğrulaması olmadan veri okuma izni olup olmadığını gösterir
* *Kimliği doğrulanmış* istemci uygulamasının geçerli kimlik doğrulama belirteci isteği ile göndermelidir gösterir
* *devre dışı* Bu tablo şu anda devre dışı olduğunu gösterir

Access özelliği tanımsız ise, kimliği doğrulanmamış erişim verilir.

### <a name="howto-tables-getidentity"></a>Nasıl yapılır: tablolarınızı ile kimlik doğrulaması talep kullanın
Kimlik doğrulama ayarlarken, istenen çeşitli talep ayarlayabilirsiniz.  Bu talep normalde aracılığıyla kullanılabilir olmayan `context.user` nesnesi.  Ancak, bunlar kullanılarak alınabilir `context.user.getIdentity()` yöntemi.  `getIdentity()` Yöntemi bir nesneye çözümler Promise döndürür.  Nesne, kimlik doğrulama yöntemi (facebook, google, twitter, microsoftaccount veya aad) anahtarlanır.

Örneğin, Microsoft Account kimlik doğrulaması ve istek e-posta adresi talep ayarlarsanız, aşağıdaki tabloda denetleyicisiyle kayıt e-posta adresi ekleyebilirsiniz:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Hangi talepleri kullanılabilir olduğunu görmek için görüntülemek için web tarayıcısını kullanın `/.auth/me` uç noktası, sitenizin.

### <a name="howto-tables-disabled"></a>Nasıl yapılır: belirli bir tablo işlemlerine erişim devre dışı bırak
Tabloda görünen ek olarak, access özelliği ayrı işlemlerini denetlemek için kullanılabilir.  Dört işlemleri şunlardır:

* *Okuma* tablo üzerinde RESTful alma işlemi
* *INSERT* tablo üzerinde RESTful POST işlemi
* *Güncelleştirme* tablo üzerinde RESTful düzeltme eki işlemi
* *silme* tablo üzerinde RESTful silme işlemi

Örneğin, bir salt okunur kimliği doğrulanmamış tablo sağlamak isteyebilirsiniz:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Nasıl yapılır: Tablo işlemleriyle kullanılan sorgu Ayarla
Verileri sınırlı bir görünümünü sağlamak için tablo işlemleri için ortak gerekli değildir.  Örneğin, yalnızca okuma veya kendi kayıtlarını güncelleştirmek, kimliği doğrulanmış kullanıcı kimliği ile etiketlenmiş bir tablo sağlayabilir.  Aşağıdaki tablo tanımı bu işlevleri sağlar:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Normalde bir sorgu yürütme işlemleri bulunduğu yerle ayarlayabilirsiniz bir sorgu özelliğe sahip yan tümcesi. Sorgu özelliği bir [QueryJS] OData sorgusu, veri arka uç işleyebilecek bir şey için dönüştürmek için kullanılan nesne.  Bir harita (gibi önceki bir) basit eşitlik durumlar için kullanılabilir. Belirli SQL yan tümceleri de ekleyebilirsiniz:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Nasıl yapılır: bir tablo üzerinde geçici silme Yapılandır
Geçici silme kayıtları silmez.  Bunun yerine, bunları veritabanı içinde silinen sütun true değerine ayarlayarak silinmiş olarak işaretler.  Mobil istemci SDK'sı IncludeDeleted() kullanmadıkça Azure Mobile Apps SDK'sı sonuçlarından geçici olarak silinen kayıtları otomatik olarak kaldırır.  Bir tablo için geçici silme yapılandırmak için ayarlayın `softDelete` tablo tanımı dosyasında özellik:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Kayıt - bir istemci uygulamasından bir Webjob'un Azure işlevi aracılığıyla veya özel bir API aracılığıyla ya da Temizleme için bir mekanizma oluşturmanız gerekir.

### <a name="howto-tables-seeding"></a>Nasıl yapılır: veri veritabanınızla çekirdek
Yeni bir uygulama oluştururken, veri içeren bir tablo oluşturmak isteyebilirsiniz.  Bu gibi tablo tanımı JavaScript dosyası içinde yapılabilir:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Tablo Azure Mobile Apps SDK tarafından oluşturulduğunda verileri dengeli yalnızca yapılır.  Tablo içinde veritabanı zaten varsa, hiçbir veri tabloya eklenen.  Dinamik şema açıksa, şema köklü verileri algılanır.

Açıkça çağırın öneririz `tables.initialize()` hizmet çalışmaya başladığında tablo oluşturmak için yöntem.

### <a name="Swagger"></a>Nasıl yapılır: Swagger desteğini etkinleştir
Azure App Service Mobile Apps ile yerleşik gelen [Swagger] destekler.  Swagger desteğini etkinleştirmek için önce bir bağımlılık olarak swagger kullanıcı arabirimini yükleyin:

    npm install --save swagger-ui

Yüklendikten sonra Azure Mobile Apps Oluşturucusu Swagger desteği etkinleştirebilirsiniz:

    var mobile = azureMobileApps({ swagger: true });

Geliştirme sürümlerinde Swagger desteğini etkinleştirmek için büyük olasılıkla yalnızca istiyorsanız.  Bunu yararlanarak yapmak `NODE_ENV` uygulama ayarı:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Swagger uç nokta http:// bulunduğu*yoursite*.azurewebsites.net/swagger.  Swagger kullanıcı Arabirimi aracılığıyla erişebilirsiniz `/swagger/ui` uç noktası.  kimlik doğrulaması, tüm uygulamanızda gerektirecek şekilde seçerseniz, Swagger bir hata oluşturur.  Azure App Service kimlik doğrulaması üzerinden kimlik doğrulamasız isteklere izin vermek en iyi sonuçlar için Seç / yetkilendirme ayarları, ardından Denetim kimlik doğrulaması kullanarak `table.access` özelliği.

Swagger seçeneğine de ekleyebilirsiniz, `azureMobile.js` yalnızca Swagger desteği yerel olarak geliştirirken istiyorsanız, dosya.

## <a name="a-namepushpush-notifications"></a><a name="push">Anında iletme bildirimleri
Tüm önde gelen platformlarda milyonlarca cihaza için hedeflenen anında iletme bildirimleri göndermenizi etkinleştirmek için Azure Notification Hubs ile Mobile Apps tümleştirir. Bildirim hub'ları kullanarak iOS, anında iletme bildirimleri gönderebilir, Android ve Windows cihazları. Tüm Notification Hubs ile yapabileceğiniz hakkında daha fazla bilgi için bkz: [Notification Hubs'a genel bakış](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Nasıl yapılır: anında iletme bildirimleri gönderme
Aşağıdaki kod itme nesne kayıtlı iOS cihazlara bir yayın anında iletme bildirimi göndermek için nasıl kullanılacağını gösterir:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

İstemciden bir şablon İtme kaydı oluşturarak, bir şablon anında iletme iletisi tüm desteklenen platformlarda aygıtlara yerine gönderebilirsiniz. Aşağıdaki kod, şablon bildirim göndermek gösterilmektedir:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


### <a name="push-user"></a>Nasıl yapılır: etiketleri kullanarak kimliği doğrulanmış bir kullanıcı için anında iletme bildirimleri gönderme
Kimliği doğrulanmış bir kullanıcı için anında iletme bildirimleri kaydolduğunda, bir kullanıcı kimliği etiketi kayıt için otomatik olarak eklenir. Bu etiket kullanarak, belirli bir kullanıcı tarafından kaydedilen tüm cihazlara anında iletme bildirimleri gönderebilirsiniz. Aşağıdaki kod, isteği yapan kullanıcı SID'si alır ve bu kullanıcı için her aygıt kaydı için bir şablon anında iletme bildirimi gönderir:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Kimliği doğrulanmış bir istemci anında iletme bildirimleri için kaydolurken kayıt denemeden önce kimlik doğrulamasının tam olduğundan emin olun.

## <a name="CustomAPI"></a>Özel API'leri
### <a name="howto-customapi-basic"></a>Nasıl yapılır: özel bir API tanımlayın
Veri erişimi yanı sıra API /tables uç noktası aracılığıyla, Azure Mobile Apps özel API kapsamı sağlayabilir.  Özel API'leri tablo tanımları benzer bir şekilde tanımlanır ve kimlik doğrulaması dahil tesisler aynı erişebilir.

Özel API ile App Service kimlik doğrulaması kullanmak isterseniz, App Service kimlik doğrulaması yapılandırmalısınız [Azure Portal] ilk.  Bir Azure uygulama hizmetinde kimlik doğrulaması yapılandırma hakkında daha fazla ayrıntı için kullanmak istediğiniz kimlik sağlayıcısı için Yapılandırma Kılavuzu gözden geçirin:

* [Azure Active Directory kimlik doğrulaması yapılandırma]
* [Facebook kimlik doğrulaması yapılandırma]
* [Google kimlik doğrulamasını yapılandırma]
* [Microsoft Authentication yapılandırma]
* [Twitter kimlik doğrulamasını yapılandırma]

Özel API'ları, tabloları API kadar aynı şekilde tanımlanır.

1. Oluşturma bir **API** dizini
2. API tanımı JavaScript dosyasını içinde oluşturmak **API** dizin.
3. İçeri aktarmak için alma yöntemini kullanmak **API** dizin.

Daha önce kullandık basic uygulama örneği temel alarak prototip API tanımı aşağıda verilmiştir.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Örneği kullanarak sunucu tarihi döndürür API atalım *Date.now()* yöntemi.  Api/date.js dosya şöyledir:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Her bir parametreyi standart RESTful fiiller - GET, POST, düzeltme eki veya DELETE biridir.  Standart bir yöntemdir [ExpressJS Ara] zorunlu çıkış gönderir işlevi.

### <a name="howto-customapi-auth"></a>Nasıl yapılır: özel bir API erişimi için kimlik doğrulaması gerektirir
Azure Mobile Apps SDK'sı kimlik doğrulaması için de aynı şekilde tablolar endpoint ve özel API'leri uygular.  Önceki bölümde geliştirilen API kimlik doğrulaması eklemek için Ekle bir **erişim** özelliği:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Ayrıca, kimlik doğrulama belirli işlemleri belirtebilirsiniz:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Tabloları uç noktası için kullanılan aynı belirteci kimlik doğrulaması gerektiren özel API'leri için kullanılması gerekir.

### <a name="howto-customapi-auth"></a>Nasıl yapılır: işlemek büyük dosya yüklemeleri
Azure Mobile Apps SDK'sı [gövde ayrıştırıcı Ara](https://github.com/expressjs/body-parser) kabul etmek ve gönderme işleminiz gövdesi içeriği kod çözme için.  Daha büyük dosya yüklemeleri kabul etmek için gövde-ayrıştırıcı önceden yapılandırabilirsiniz:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Base-64 kodlanmış Aktarımdan önce dosyasıdır.  Bu gerçek karşıya yükleme boyutu artar (ve bu nedenle boyut için hesabı gerekir).

### <a name="howto-customapi-sql"></a>Nasıl yapılır: özel SQL deyimlerini yürütmek
Azure Mobile Apps SDK'sı kolayca tanımlanmış veri sağlayıcısı için parametreli SQL deyimlerini yürütmek sağlayarak tüm içeriği istek nesnesi aracılığıyla erişmesine izin verir:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Hata ayıklama, kolay tablolar ve kolay API'leri
### <a name="howto-diagnostic-logs"></a>Nasıl yapılır: hata ayıklama, tanılama ve Azure Mobile apps sorunlarını giderme
Azure uygulama hizmeti birkaç hata ayıklama ve sorun giderme tekniklerini Node.js uygulamaları için sağlar.
Node.js mobil arka uç sorunlarını giderme çalışmaya başlamak için aşağıdaki makalelere bakın:

* [Bir Azure uygulama hizmeti izleme]
* [Azure uygulama hizmetinde tanılama günlük kaydını etkinleştir]
* [Visual Studio'da bir Azure uygulama hizmeti sorunlarını giderme]

Node.js uygulamalarını çok çeşitli tanılama günlük araçları erişimi.  Azure Mobile Apps Node.js SDK'sı düzenlenmemeli [Winston] tanılama günlük için.  Günlüğü otomatik olarak etkin hata ayıklama modunu etkinleştirme veya ayarlayarak **MS_DebugMode** uygulama ayarı true [Azure Portal]. Oluşturulan günlüklerin görünmez tanılama günlüklerine [Azure Portal].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Nasıl yapılır: Azure portalında kolay tabloları ile çalışma
Portaldaki kolay tabloları oluşturma ve tabloları doğrudan portalda çalışmak olanak tanır. CSV biçiminde kolay tablolara veri kümesi yükleyebilirsiniz. Çakışan özellikleri adlarında (CSV kümenizi) ile Azure Mobile Apps arka uç sistem özellikleri adlarını kullanamayacağınızı unutmayın. Sistem özellikleri adları şunlardır:
* CreatedAt
* updatedAt
* silindi
* Sürüm

Uygulama hizmeti Düzenleyicisi'ni kullanarak tablo işlemleri bile düzenleyebilirsiniz. Tıkladığınızda **kolay tabloları** arka uç site ayarlarınızda ekleyebilir, değiştirebilir veya bir tablo silme. Tablodaki verileri de görebilirsiniz.

![Kolay tablolarla çalışma](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Aşağıdaki komutlar bir tablo için komut çubuğunda kullanılabilir:

* **İzinleri Değiştir** - Okuma izinlerini değiştirmek, Ekle, Güncelleştir ve tablosu üzerinde işlem silin.
  Kimlik doğrulaması gerektiren ya da işlemi tüm erişimi devre dışı bırakmak için anonim erişime izin verecek şekilde seçeneklerdir.
* **Komut dosyasını Düzenle** -komut dosyası tablosu için uygulama hizmeti Düzenleyicisi'nde açılır.
* **Şemayı yönetmek** - ekleme veya sütunları silin veya tablo dizini değiştirin.
* **Düz tablo** -var olan bir tabloyu kesen değişmeden tüm veri satırları silme ancak şemanın bırakarak olabilir.
* **Satırları Sil** -tek tek veri satırı silin.
* **Akış günlükleri Görünüm** -siteniz için akış günlüğü hizmetine bağlanır.

### <a name="work-easy-apis"></a>Nasıl yapılır: Azure portalında kolay API'leri ile çalışma
Portaldaki kolay API'leri oluşturmanıza ve özel API'lerini doğrudan portalda çalışmak olanak verir. Uygulama hizmeti Düzenleyicisi'ni kullanarak API betikleri düzenleyebilirsiniz.

Tıkladığınızda **kolay API'leri** arka uç site ayarlarınızda ekleyebilir, değiştirebilir veya özel bir API uç noktasını silmek.

![Kolay API'leri ile çalışma](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Portalda, belirli bir HTTP eylemi için erişim izinlerini değiştirmek, API komut dosyası App Service Düzenleyicisi'ni düzenleyin veya akış günlükleri görüntüleyin.

### <a name="online-editor"></a>Nasıl yapılır: kod App Service Düzenleyicisi'nde Düzenle
Azure portalı proje yerel bilgisayarınıza indirmek zorunda kalmadan Node.js arka uç komut dosyalarınızı App Service Düzenleyicisi'ni düzenlemenizi sağlar. Çevrimiçi düzenleyicisinde komut dosyalarını düzenlemek için:

1. Mobil uygulama arka uç dikey penceresinde tıklayın **tüm ayarları** > ya da **kolay tablolar** veya **kolay API'leri**, bir tablo veya API'ı tıklatın **Düzenle betik**. Komut dosyası App Service Düzenleyicisi'nde açılır.

    ![Uygulama hizmeti Düzenleyicisi](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. Çevrimiçi düzenleyicisini kod dosyasında istediğiniz değişiklikleri yapın. Siz yazarken değişiklikler otomatik olarak kaydedilir.

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Android istemci hızlı başlangıç]: app-service-mobile-android-get-started.md
[Apache Cordova istemci hızlı başlangıç]: app-service-mobile-cordova-get-started.md
[iOS istemci hızlı başlangıç]: app-service-mobile-ios-get-started.md
[Xamarin.iOS istemcisi hızlı başlangıç]: app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android istemcisi hızlı başlangıç]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms istemci hızlı başlangıç]: app-service-mobile-xamarin-forms-get-started.md
[Windows mağazası istemci hızlı başlangıç]: app-service-mobile-windows-store-dotnet-get-started.md
[çevrimdışı veri eşitlemeye]: app-service-mobile-offline-data-sync.md
[Azure Active Directory kimlik doğrulaması yapılandırma]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook kimlik doğrulaması yapılandırma]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[Google kimlik doğrulamasını yapılandırma]: ../app-service/app-service-mobile-how-to-configure-google-authentication.md
[Microsoft Authentication yapılandırma]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter kimlik doğrulamasını yapılandırma]: ../app-service/app-service-mobile-how-to-configure-twitter-authentication.md
[Azure App Service Deployment Guide]: ../app-service/app-service-deploy-local-git.md
[Bir Azure uygulama hizmeti izleme]: ../app-service/web-sites-monitor.md
[Azure uygulama hizmetinde tanılama günlük kaydını etkinleştir]: ../app-service/web-sites-enable-diagnostic-log.md
[Visual Studio'da bir Azure uygulama hizmeti sorunlarını giderme]: ../app-service/web-sites-dotnet-troubleshoot-visual-studio.md
[düğümü sürüm belirtin]: ../nodejs-specify-node-version-azure-apps.md
[düğümü modüllerini kullanma]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp örnek github'da]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[Yapılacaklar örneği github'daki]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[örnekler dizini github'da]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Visual Studio için Node.js araçları 1.1]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js paket]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Ara]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
