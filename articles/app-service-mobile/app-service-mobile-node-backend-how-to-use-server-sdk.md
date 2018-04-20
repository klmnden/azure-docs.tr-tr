---
title: Mobil uygulamaları için Node.js arka uç sunucusu SDK ile çalışmaya nasıl | Microsoft Docs
description: Azure App Service Mobile Apps için Node.js arka uç sunucusu SDK ile çalışmayı öğrenin.
services: app-service\mobile
documentationcenter: ''
author: elamalani
manager: elamalani
editor: ''
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 335186deccaa82b9a8d262d62dd8ce5d620446b6
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="how-to-use-the-mobile-apps-nodejs-sdk"></a>Mobile Apps Node.js SDK'sını kullanma
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Bu makalede ayrıntılı bilgiler sağlar ve bir Node.js ile nasıl çalışacağınızı örnekler arka uç, Azure App Service Mobile Apps özelliğidir.

## <a name="Introduction"></a>Giriş
Mobile Apps bir web uygulamasına mobile için iyileştirilmiş veri erişimi Web API ekleme yeteneği sağlar. Mobile Apps SDK'sı, ASP.NET ve Node.js web uygulamaları için sağlanır. SDK, aşağıdaki işlemleri sağlar:

* Tablo işlemleri (okuma, Ekle, Güncelleştir, Sil) veri erişimi
* Özel API işlemleri

İki işlem kimlik doğrulaması için Azure uygulama hizmeti sağlayan tüm kimlik sağlayıcıları sağlar. Bu sağlayıcıları kuruluş kimliği için Facebook, Twitter, Google ve Microsoft yanı sıra Azure Active Directory gibi sosyal kimlik sağlayıcıları içerir.

Her kullanım örneğine ilgili örnekleri bulabilirsiniz [örnekler dizini github'da].

## <a name="supported-platforms"></a>Desteklenen platformlar
Mobile Apps Node.js SDK'sı geçerli LTS sürüm düğümünün ve daha sonra destekler. Şu anda, en son LTS düğümü v4.5.0 sürümüdür. Düğüm'ın diğer sürümleri çalışabilir ancak desteklenmez.

Mobile Apps Node.js SDK'sı iki veritabanı sürücüleri destekler: 

* Düğüm mssql sürücüsü Azure SQL Database ve yerel SQL Server örnekleri destekler.  
* Sqlite3 sürücü üzerinde yalnızca tek bir örneği SQLite veritabanlarını destekler.

### <a name="howto-cmdline-basicapp"></a>Komut satırını kullanarak basit bir Node.js arka ucu oluşturma
Her mobil uygulamaları Node.js arka ucu bir ExpressJS uygulamayı başlatır. ExpressJS en popüler web service Node.js için kullanılabilir çerçevesidir. Temel bir oluşturabileceğiniz [Express] şekilde uygulama:

1. Bir komut veya PowerShell penceresi projeniz için bir dizin oluşturun:

        mkdir basicapp
2. Çalıştırma `npm init` paket yapısı başlatılamadı:

        cd basicapp
        npm init

   `npm init` Komutu bir dizi proje başlatılamadı sorular sorar. Örnek çıktı bakın:

   ![Npm init çıktı][0]
3. Yükleme `express` ve `azure-mobile-apps` npm depodan kitaplıklar:

        npm install --save express azure-mobile-apps
4. Temel mobil sunucu uygulamak için bir app.js dosyası oluşturun:

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table.
        mobile.tables.add('TodoItem');

        // Add the Mobile API so it is accessible as a Web API.
        app.use(mobile);

        // Start listening on HTTP.
        app.listen(process.env.PORT || 3000);

Bu uygulama bir mobil iyileştirilmiş Web API ile tek bir uç noktası oluşturur (`/tables/TodoItem`) dinamik şemasını kullanarak kimliği doğrulanmamış bir temel alınan SQL veri deposuna erişimi sağlar. İstemci Kitaplığı quickstarts izlemek için uygundur:

* [Android istemci hızlı başlangıç]
* [Apache Cordova istemci hızlı başlangıç]
* [iOS istemci hızlı başlangıç]
* [Windows mağazası istemci hızlı başlangıç]
* [Xamarin.iOS istemcisi hızlı başlangıç]
* [Xamarin.Android istemcisi hızlı başlangıç]
* [Xamarin.Forms istemci hızlı başlangıç]

Bu temel uygulamada kodu bulabilirsiniz [basicapp örnek github'da].

### <a name="howto-vs2015-basicapp"></a>Visual Studio 2015 kullanarak bir Node.js arka ucu oluşturma
Visual Studio 2015 IDE içinden Node.js uygulamaları geliştirmek için uzantı gerektirir. Başlatmak için Yükle [Visual Studio için Node.js araçları 1.1]. Yüklemeyi bitirdikten sonra bir Express 4.x uygulaması oluşturun:

1. Açık **yeni proje** iletişim kutusu (gelen **dosya** > **yeni** > **proje**).
2. Genişletme **şablonları** > **JavaScript** > **Node.js**.
3. Seçin **temel Azure Node.js Express 4 uygulama**.
4. Proje adı girin. **Tamam**’ı seçin.

   ![Visual Studio 2015 yeni proje][1]
5. Sağ **npm** düğümü ve select **yükleme yeni npm paket**.
6. İlk Node.js uygulamanızı oluşturduktan sonra npm katalog yenilemeniz gerekebilir. Seçin **yenileme** gerekiyorsa.
7. Girin **azure mobile apps** arama kutusuna. Seçin **azure mobile apps 2.0.0** paketini ve ardından **paket yükleme**.

   ![Yeni npm paket yüklemek için][2]
8. Seçin **Kapat**.
9. Mobile Apps SDK'sı için destek eklemek için app.js dosyasını açın. AT 6 at kitaplığı alt çizgi `require` ifadeleri, aşağıdaki kodu ekleyin:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

   AT yaklaşık satır 27 art arda `app.use` ifadeleri, aşağıdaki kodu ekleyin:

        app.use('/users', users);

        // Mobile Apps initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

   Dosyayı kaydedin.
10. Uygulamayı yerel olarak çalıştırın (API üzerinde sunulan http://localhost:3000) veya Azure'a yayımlayabilirsiniz.

### <a name="create-node-backend-portal"></a>Azure portalı kullanarak bir Node.js arka ucu oluşturma
Mobile Apps arka plan sağ oluşturabilirsiniz, [Azure portal]. Aşağıdaki adımları tamamlayın veya izleyerek bir istemci ve sunucu birlikte oluşturabilirsiniz [mobil uygulama oluşturma](app-service-mobile-ios-get-started.md) Öğreticisi. Öğreticinin bu yönergeleri basitleştirilmiş bir sürümünü içerir ve kavram kanıtı projeleri için en iyisidir.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Geri **başlama** bölmesi altında **tablo API Oluştur**, seçin **Node.js** arka uç dilinizi olarak.
Kutusunu seçin **bu tüm site içeriğinin üzerine yazacağını kabul ediyorum**ve ardından **Todoıtem tablosu oluştur**.

### <a name="download-quickstart"></a>Git kullanarak Node.js arka uç hızlı başlangıç kod projesi indirme
Portal kullanarak bir Node.js Mobile Apps arka uç oluşturduğunuzda **Hızlı Başlangıç** bölmesinde, bir Node.js projesi için oluşturduğunuz ve sitenize dağıtılabilir. Portalda, tablolar ve API'ları ekleyin ve Node.js arka ucu için kod dosyaları düzenleyin. Böylece ekleyin veya tablolar ve API'ları değiştirmek ve projeyi yeniden yayımlamanız arka uç projesi indirmek için çeşitli dağıtım araçları da kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure uygulama hizmeti Dağıtım Kılavuzu'nu]. 

Aşağıdaki yordam, hızlı başlangıç projesi kodu indirmek için bir Git deposu kullanır:

1. Zaten yapmadıysanız Git'i, yükleyin. Git yüklemek için gereken adımlar, işletim sistemleri arasında farklılık gösterir. İşletim sistemine özgü dağıtımları ve yükleme yönergeleri için bkz: [yükleme Git](http://git-scm.com/book/en/Getting-Started-Installing-Git).
2. Bkz: [deponuz hazırlama](../app-service/app-service-deploy-local-git.md#prepare-your-repository) arka uç sitenizin Git deposunu etkinleştirmek için. Dağıtım kullanıcı adı ve parola not edin.
3. Mobile Apps arka uç için bölmesinde Not **Git kopyalama URL'si** ayarı.
4. Yürütme `git clone` Git kopyalama URL'si kullanarak komutu. Gerektiğinde, aşağıdaki örnekte olduğu gibi parolanızı girin:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. Yerel dizine gözatın (`/todolist` önceki örnekte) ve proje dosyalarını yüklendi dikkat edin. Todoitem.json dosyasında bulun `/tables` dizin. Bu dosya, tablo üzerinde izinleri tanımlar. Ayrıca aynı dizinde todoitem.js dosyasını bulun. Tablo için CRUD işlemi betikleri tanımlar.
6. Proje dosyalarını eklemek için aşağıdaki komutları çalıştırın, değişiklikleri yaptıktan sonra kaydetmek ve siteye değişiklikleri karşıya yükle:

        $ git commit -m "updated the table script"
        $ git push origin master

   Projeye yeni dosyalar eklediğinizde, ilk kez çalıştırmanız gereken `git add .` komutu.

İşlemeleri yeni bir dizi siteye gönderilen her zaman site yeniden yayımlanacak.

### <a name="howto-publish-to-azure"></a>Node.js arka ucunuz için Azure yayımlama
Microsoft Azure Mobile Apps Node.js yayımlamak için birçok mekanizma arka uç Azure hizmeti sağlar. Bu düzenekler Visual Studio'ya tümleşik dağıtım araçları, komut satırı araçları ve kaynak denetimine bağlı sürekli dağıtım seçenekleri içerir. Daha fazla bilgi için bkz: [Azure uygulama hizmeti Dağıtım Kılavuzu'nu].

Azure uygulama hizmeti olan belirli önerileri gözden geçirmeniz gereken Node.js uygulamaları için arka uç yayımlamadan önce:

* Nasıl yapılır [düğümü sürüm belirtin]
* Nasıl yapılır [düğümü modüllerini kullanma]

### <a name="howto-enable-homepage"></a>Uygulamanız için bir giriş sayfası etkinleştir
Birçok uygulama, web ve mobil uygulamaları birleşimidir. İki modelleri birleştirmek için ExpressJS çerçevesini kullanabilirsiniz. Bazı durumlarda, ancak yalnızca bir mobil arabirimi uygulamak isteyebilirsiniz. Uygulama hizmeti çalışır durumda olduğundan emin olmak için bir giriş sayfası sağlamak kullanışlı ve çalışır durumdadır. Giriş sayfası sağlayın veya geçici bir giriş sayfası etkinleştirin. Geçici bir giriş sayfası etkinleştirmek için mobil uygulamaları oluşturmak için aşağıdaki kodu kullanın:

    var mobile = azureMobileApps({ homePage: true });

Yalnızca kullanılabilen bu seçenek yerel olarak geliştirirken istiyorsanız, bu ayar, azureMobile.js dosyasına ekleyebilirsiniz.

## <a name="TableOperations"></a>Tablo işlemleri
Azure mobile apps Node.js sunucusu SDK'sı bir Web API olarak Azure SQL veritabanında depolanan veri tabloları kullanıma sunmak için mekanizma sağlar. Beş işlemleri sağlar:

| İşlem | Açıklama |
| --- | --- |
| GET /tables/*tablename* |Tüm kayıtların tabloda alın. |
| GET /tables/*tablename*/:id |Belirli bir kayıt tabloda alın. |
| POST /tables/*tablename* |Bir kayıt tablosu oluşturun. |
| Düzeltme eki /tables/*tablename*/:id |Tablodaki bir kaydın güncelleştirin. |
| DELETE /tables/*tablename*/:id |Tablodaki kaydını silin. |

Bu Web API'sini destekleyen [OData] ve desteklemek için tablo şemasını genişletir [çevrimdışı veri eşitlemeye].

### <a name="howto-dynamicschema"></a>Dinamik bir şema kullanarak tabloları tanımlayın
Bir tablo kullanmadan önce tanımlamanız gerekir. (Burada yer alan şemada sütunları tanımlayın) statik şeması kullanarak tabloları tanımlayabilirsiniz veya dinamik olarak (SDK gelen istekleri temel alan şema denetimleri yerlerde). Ayrıca, JavaScript kodu tanımına ekleyerek Web API belirli yönlerini kontrol edebilir.

En iyi uygulama, bir JavaScript dosyasında her tablo tanımlamanız gerekir `tables` dizin ve ardından `tables.import()` tabloları yöntemi. Basic uygulaması örneği genişletme, app.js dosya ayarlamanız:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed.
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined.
    mobile.tables.initialize().then(function () {
        // Add the Mobile API so it is accessible as a Web API.
        app.use(mobile);

        // Start listening on HTTP.
        app.listen(process.env.PORT || 3000);
    });

Tabloda tanımlayın. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here.

    module.exports = table;

Tablolar, varsayılan olarak dinamik bir şema kullanın. Dinamik şema genel olarak kapatmak için ayarlayın `MS_DynamicSchema` false Azure portalında uygulama ayarı.

Tam bir örnek bulabilirsiniz [Yapılacaklar örneği github'daki].

### <a name="howto-staticschema"></a>Statik bir şema kullanarak tabloları tanımlayın
Web API kullanıma sunmak için sütunları açıkça tanımlayabilirsiniz. Azure mobile apps Node.js SDK'sı çevrimdışı veri eşitlemeye sağladığınız listesine için gerekli ek sütunları otomatik olarak ekler. Örneğin, hızlı başlangıç istemci uygulamaları iki sütun içeren bir tablo gerektirir: `text` (dize) ve `complete` (Boole).  
Tablo tablo tanımı JavaScript dosyasında tanımlanabilir (bulunan `tables` dizin) gibi:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table.
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

    module.exports = table;

Tabloları statik olarak tanımlarsanız, ayrıca çağırmalısınız `tables.initialize()` yöntemi başlangıçta veritabanı şeması oluşturulamadı. `tables.initialize()` Yöntemi döndürür bir [promise] böylece veritabanı başlatılmadan önce web hizmeti isteklerinin sunmuyor.

### <a name="howto-sqlexpress-setup"></a>SQL Server Express'in yerel makinenizde geliştirme veri deposu olarak kullanın
Mobile Apps Node.js SDK'sı veri kutudan çıktığında hizmet veren için üç seçenek sunar:

* Kullanım **bellek** sürücü kalıcı olmayan örnek depolama sağlar.
* Kullanım **mssql** geliştirme için bir SQL Server Express veri deposu sağlamak için sürücü.
* Kullanım **mssql** üretim için bir Azure SQL veritabanı veri deposu sağlamak için sürücü.

Mobile Apps Node.js SDK'sı [mssql Node.js paket] kurmak ve SQL Server Express ve SQL veritabanına bir bağlantı kullanın. Bu paket, SQL Server Express örneğinizi TCP bağlantılarını etkinleştirmenizi istemektedir.

> [!TIP]
> Bellek sürücüsü eksiksiz test olanaklarının sağlamaz. Arka uç yerel olarak test etmek isterseniz, bir SQL Server Express veri deposu ve mssql sürücü kullanılmasını öneririz.
>
>

1. İndirme ve yükleme [Microsoft SQL Server 2014 Express]. SQL Server 2014 Express araçları Edition'la yüklediğinizden emin olun. 64-bit desteği açıkça gerekmiyorsa 32-bit sürümünü çalışırken daha az bellek kullanır.
2. SQL Server 2014 Yapılandırma Yöneticisi'ni çalıştırın:

   a. Genişletme **SQL Server Ağ Yapılandırması** ağaç menü düğümünde.

   b. Seçin **SQLEXPRESS protokolleri**.

   c. Sağ **TCP/IP'yi** seçip **etkinleştirmek**. Seçin **Tamam** açılan iletişim kutusunda.

   d. Sağ **TCP/IP'yi** seçip **özellikleri**.

   e. Seçin **IP adreslerini** sekmesi.

   f. Bul **IPAll** düğümü. İçinde **TCP bağlantı noktası** alanına, **1433**.

      ![SQL Server Express için TCP/IP'yi yapılandırma][3]

   g. **Tamam**’ı seçin. Seçin **Tamam** açılan iletişim kutusunda.

   h. Seçin **SQL Server Hizmetleri** ağaç menüsünde.

   i. Sağ **SQL Server (SQLEXPRESS)** seçip **yeniden**.

   j. SQL Server 2014 Yapılandırma Yöneticisi'ni kapatın.
3. SQL Server 2014 Management Studio'yu çalıştırın ve, yerel SQL Server Express örneğine bağlan:

   1. Nesne Gezgini'nde örneğinizi sağ tıklatıp **özellikleri**.
   2. Seçin **güvenlik** sayfası.
   3. Emin **SQL Server ve Windows kimlik doğrulaması modu** seçilir.
   4. **Tamam**’ı seçin.

      ![SQL Server Express kimlik doğrulamasını yapılandırma][4]
   5. Genişletme **güvenlik** > **oturumları** nesne Gezgini'nde.
   6. Sağ **oturumları** seçip **yeni oturum açma**.
   7. Bir oturum açma adı girin. **SQL Server kimlik doğrulaması**’nı seçin. Bir parola girin ve sonra aynı parolayı girin **parolayı onayla**. Parola Windows karmaşıklık gereksinimlerini karşılaması gerekir.
   8. **Tamam**’ı seçin.

      ![SQL Server Express için yeni bir kullanıcı ekleyin][5]
   9. Yeni oturum açma sağ tıklatıp **özellikleri**.
   10. Seçin **sunucu rolleri** sayfası.
   11. Onay kutusunu seçin **dbcreator** sunucu rolü.
   12. **Tamam**’ı seçin.
   13. SQL Server 2015 Management Studio'yu kapatın.

Kullanıcı adı ve seçtiğiniz parolayı kaydettiğinizden emin olun. Ek sunucu rollerini veya veritabanı gereksinimlerinize bağlı olarak izinleri atamak gerekebilir.

Node.js uygulaması okuma `SQLCONNSTR_MS_TableConnectionString` bu veritabanı için bağlantı dizesi için ortam değişkeni. Ortamınızda bu değişkeni ayarlayabilirsiniz. Örneğin, bu ortam değişkenini ayarlamak için PowerShell kullanın:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Veritabanına bir TCP/IP bağlantısı üzerinden erişir. Bağlantı için bir kullanıcı adı ve parola sağlayın.

### <a name="howto-config-localdev"></a>Projeniz yerel geliştirme için yapılandırma
Mobile Apps okur adlandırılan bir JavaScript dosyası *azureMobile.js* yerel dosya sisteminden. Bu dosya, Mobile Apps SDK'sı üretimde yapılandırmak için kullanmayın. Bunun yerine, kullanın **uygulama ayarları** içinde [Azure portal]. 

Bir yapılandırma nesnesi azureMobile.js dosyasını dışarı aktarmanız gerekir. En yaygın ayarlar şunlardır:

* Veritabanı ayarları
* Tanılama günlük ayarları
* Alternatif CORS ayarları

Bu örnek azureMobile.js dosyası önceki veritabanı ayarlarını uygular:

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

AzureMobile.js .gitignore dosyanıza ekleyin öneririz (veya diğer kaynak kodu denetimi dosya yoksay) bulutta depolanan parolaları önlemek için. Her zaman üretim ayarlarında yapılandırdığınız **uygulama ayarları** içinde [Azure portal].

### <a name="howto-appsettings"></a>Mobil uygulamanız için uygulama ayarlarını yapılandırın
Bir eşdeğer uygulama ayarı azureMobile.js dosyasındaki çoğu ayarları sahip [Azure portal]. Uygulamanızı yapılandırmak için aşağıdaki listeyi kullanın **uygulama ayarları**:

| Uygulama ayarı | azureMobile.js ayarı | Açıklama | Geçerli değerler |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |ad |Uygulama adı |string |
| **MS_MobileLoggingLevel** |Logging.level |En küçük günlük düzeyi günlüğe kaydedilecek ileti sayısı |hata, uyarı, bilgi, ayrıntılı, hata ayıklama, saçma |
| **MS_DebugMode** |hata Ayıkla |Etkinleştirir ya da hata ayıklama modunu devre dışı bırakır |TRUE, false |
| **MS_TableSchema** |Data.Schema |SQL tablolarının varsayılan şema adı |dize (varsayılan: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Etkinleştirir ya da hata ayıklama modunu devre dışı bırakır |TRUE, false |
| **MS_DisableVersionHeader** |(çok undefined olarak ayarlanır) sürümü |X-ZUMO-Server-Version üstbilgi devre dışı bırakır |TRUE, false |
| **MS_SkipVersionCheck** |skipversioncheck |İstemcisi API sürümü denetimi devre dışı bırakır |TRUE, false |

Bir uygulama ayarı ayarlamak için:

1. [Azure portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri**ve ardından mobil uygulamanızı adını seçin.
3. **Ayarları** bölmesi varsayılan olarak açılır. Bu işaretlemezse **ayarları**.
4. Üzerinde **genel** menüsünde, select **uygulama ayarları**.
5. Kaydırma **uygulama ayarları** bölümü.
6. Uygulamanızı ayarı zaten varsa, değeri düzenlemek için uygulama ayarının değerini seçin.
   Uygulama ayarı mevcut değilse uygulama ayarında girin **anahtar** kutusu ve değeri **değeri** kutusu.
8. **Kaydet**’i seçin.

Çoğu uygulama ayarlarını değiştirme hizmetini yeniden başlatma gerektirir.

### <a name="howto-use-sqlazure"></a>Kullanım SQL veritabanı, üretim verileri olarak depolama
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Bir veri deposu olarak Azure SQL veritabanı kullanan tüm Azure App Service uygulama türlerine aynıdır. Siz bunu zaten yapmadıysanız, mobil uygulama arka ucu oluşturmak için aşağıdaki adımları izleyin:

1. [Azure portal]’da oturum açın.
2. Pencerenin üst sol seçin **+ yeni** düğmesi > **Web + mobil** > **mobil uygulama**ve mobil uygulamalarınızı arka uç için bir ad sağlayın.
3. İçinde **kaynak grubu** kutusuna, uygulamanızı aynı adı girin.
4. Uygulama hizmeti planı varsayılan seçilidir. Uygulama hizmeti planınızı değiştirmek istiyorsanız:

   a. Seçin **uygulama hizmeti planı** > **+ Yeni Oluştur**. 
   
   b. Yeni uygulama hizmeti planının adı sağlayın ve uygun bir konum seçin. 
   
   c. Hizmet için uygun bir fiyatlandırma katmanı seçin. Seçin **tüm görüntüle** daha seçenekleri gibi fiyatlandırma görünümüne **serbest** ve **paylaşılan**. 
   
   d. Tıklatın **seçin** düğmesi. 
   
   e. Geri **uygulama hizmeti planı** bölmesinde, **Tamam**.
5. **Oluştur**’u seçin. 

Mobile Apps sağlama arka uç birkaç dakika sürebilir. Mobile Apps geri sonra son sağlandığına, portal açar **ayarları** Mobile Apps arka uç için bölmesi.

Mevcut bir SQL veritabanını, Mobile Apps arka ucuna bağlanmak veya yeni bir SQL veritabanı oluşturma seçebilirsiniz. Bu bölümde, bir SQL veritabanı oluşturun.

> [!NOTE]
> Mobile Apps arka uç gibi bir veritabanı aynı konumda zaten varsa, bunun yerine seçebileceğiniz **varolan veritabanını kullan** sonra bu veritabanını seçin. Bir veritabanı farklı bir konumda kullanımını daha yüksek gecikme nedeniyle öneririz yok.
>
>

1. Yeni mobil uygulama arka ucu içinde seçin **ayarları** > **mobil uygulama** > **veri** > **+ Ekle**.
2. İçinde **veri bağlantısı Ekle** bölmesinde, **SQL veritabanı - gerekli ayarları Yapılandır** > **yeni bir veritabanı oluşturmak**. Yeni veritabanı adını girin **adı** kutusu.
3. Seçin **Server**. İçinde **yeni sunucu** bölmesinde, bir benzersiz sunucu adı girin **sunucu adı** kutusuna ve uygun bir sunucu yönetici oturum açma ve parola sağlayın. Emin **azure hizmetlerinin sunucuya erişmesine izin** seçilir. **Tamam**’ı seçin.

   ![Bir Azure SQL veritabanı oluşturma][6]
4. İçinde **yeni veritabanı** bölmesinde, **Tamam**.
5. Geri **veri bağlantısı Ekle** bölmesinde, **bağlantı dizesi**, oturum açma ve veritabanı oluştururken belirttiğiniz parolayı girin. Varolan bir veritabanını kullanıyorsanız, bu veritabanı için oturum açma kimlik bilgilerini sağlayın. **Tamam**’ı seçin.
6. Geri **veri bağlantısı Ekle** bölmesinde tekrar select **Tamam** veritabanı oluşturmak için.

<!--- END OF ALTERNATE INCLUDE -->

Veritabanı oluşturma, birkaç dakika sürebilir. Kullanım **bildirimleri** dağıtımın ilerleme durumunu izlemek için alan. Veritabanı başarıyla dağıtıldı kadar ilerleme değil. Veritabanı dağıtıldıktan sonra bir bağlantı dizesi Mobile Apps arka uç uygulama ayarlarınızı SQL veritabanı örneğinde için oluşturulur. Bu uygulama ayarını görebilirsiniz **ayarları** > **uygulama ayarları** > **bağlantı dizeleri**.

### <a name="howto-tables-auth"></a>Tablolara erişimi kimlik doğrulaması iste
App Service kimlik doğrulaması ile kullanmak istiyorsanız, `tables` uç noktası, App Service kimlik doğrulaması yapılandırmanız gerekir [Azure portal] ilk. Daha fazla bilgi için kullanmak istediğiniz kimlik sağlayıcısı için Yapılandırma Kılavuzu'na bakın:

* [Azure Active Directory kimlik doğrulamasını yapılandırma]
* [Facebook kimlik doğrulamasını yapılandırma]
* [Google kimlik doğrulamasını yapılandırma]
* [Microsoft kimlik doğrulamasını yapılandırma]
* [Twitter kimlik doğrulamasını yapılandırma]

Her tablo tablo erişimi denetlemek için kullanabileceğiniz bir erişim özelliğine sahiptir. Aşağıdaki örnek, kimlik doğrulaması gerekli statik olarak tanımlanmış bir tablo gösterir.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table.
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

    // Require authentication to access the table.
    table.access = 'authenticated';

    module.exports = table;

Access özelliği üç değerden birini gerçekleştirebilirsiniz:

* *Anonim* istemci uygulamanın kimlik doğrulaması olmadan veri okuma izni olup olmadığını gösterir.
* *Kimliği doğrulanmış* istemci uygulamasının geçerli kimlik doğrulama belirteci isteği ile göndermelidir gösterir.
* *devre dışı* Bu tablo şu anda devre dışı olduğunu gösterir.

Access özelliği tanımsız ise, kimliği doğrulanmamış erişim verilir.

### <a name="howto-tables-getidentity"></a>Kimlik doğrulaması talep tablolarınızı ile kullanma
Kimlik doğrulama ayarlarken, istenen çeşitli talep ayarlayabilirsiniz. Bu talep normalde aracılığıyla kullanılabilir olmayan `context.user` nesnesi. Ancak, siz bunları kullanarak alabilirsiniz `context.user.getIdentity()` yöntemi. `getIdentity()` Yöntemi bir nesneye çözümler promise döndürür. Nesne kimlik doğrulama yöntemiyle anahtarlanır (`facebook`, `google`, `twitter`, `microsoftaccount`, veya `aad`).

Örneğin, Microsoft hesabı kimlik doğrulaması ve e-posta adresi talep isteği ayarlama, aşağıdaki tabloda denetleyicisiyle kayıt e-posta adresi ekleyebilirsiniz:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition.
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

    // Configure specific code when the client does a request.
    // READ: only return records that belong to the authenticated user.
    table.read(queryContextForEmail);

    // CREATE: add or overwrite the userId based on the authenticated user.
    table.insert(addEmailToContext);

    // UPDATE: only allow updating of records that belong to the authenticated user.
    table.update(queryContextForEmail);

    // DELETE: only allow deletion of records that belong to the authenticated user.
    table.delete(queryContextForEmail);

    module.exports = table;

Hangi talepleri kullanılabilir olduğunu görmek için görüntülemek için web tarayıcısını kullanın `/.auth/me` uç noktası, sitenizin.

### <a name="howto-tables-disabled"></a>Belirli bir tablo işlemlerine erişim devre dışı bırak
Tabloda görünen ek olarak, access özelliği ayrı işlemlerini denetlemek için kullanılabilir. Dört işlemleri şunlardır:

* `read` Tablo üzerinde RESTful alma işlemi var.
* `insert` Tablo üzerinde RESTful POST işlemdir.
* `update` Tablo üzerinde RESTful düzeltme eki işlemi var.
* `delete` Tablo üzerinde RESTful DELETE işlemi var.

Örneğin, bir salt okunur kimliği doğrulanmamış tablo sağlamak isteyebilirsiniz:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-only table. Only allow READ operations.
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Tablo işlemleriyle kullanılan sorgu Ayarla
Verileri sınırlı bir görünümünü sağlamak için tablo işlemleri için ortak gerekli değildir. Örneğin, yalnızca okuma veya kendi kayıtlarını güncelleştirmek, kimliği doğrulanmış kullanıcı kimliği ile etiketlenmiş bir tablo sağlayabilir. Aşağıdaki tablo tanımı bu işlevleri sağlar:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table.
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table.
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved.
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user.
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Normalde bir sorguyu çalıştırma işlemleri kullanarak ayarlayabilirsiniz bir sorgu özelliğe sahip bir `where` yan tümcesi. Sorgu özelliği bir [QueryJS] arka uç veri OData sorgusu şey dönüştürmek için kullanılan nesne işleyebilir. (Gibi önceki bir) basit eşitlik durumlar için bir harita kullanabilirsiniz. Belirli SQL yan tümceleri de ekleyebilirsiniz:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Bir tabloda bir geçici silme Yapılandır
Bir geçici silme kayıtları silmez. Bunun yerine, bunları veritabanı içinde silinen sütun true değerine ayarlayarak silinmiş olarak işaretler. Mobil istemci SDK'sını kullanmadıkça Mobile Apps SDK'sı geçici olarak silinen kayıtlar sonuçlarından otomatik olarak kaldırır. `IncludeDeleted()`. Bir tablo için geçici silme yapılandırmak için ayarlayın `softDelete` tablo tanımı dosyasında özellik:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table.
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

    // Turn on soft delete.
    table.softDelete = true;

    // Require authentication to access the table.
    table.access = 'authenticated';

    module.exports = table;

Kayıtları silme için bir mekanizma kurmanız gerekir: bir istemci uygulaması, bir Web işi, bir Azure işlevi veya özel bir API.

### <a name="howto-tables-seeding"></a>Veritabanınızı verilerle çekirdek
Yeni bir uygulama oluştururken, veri içeren bir tablo oluşturmak isteyebilirsiniz. Tablo tanımı JavaScript dosyası içinde şu şekilde bunu yapabilirsiniz:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table.
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

    // Require authentication to access the table.
    table.access = 'authenticated';

    module.exports = table;

Yalnızca bir tablo oluşturmak için Mobile Apps SDK'sı kullanmış olduğunuz verilerin dengeli olur. Tablo veritabanında zaten varsa, hiçbir veri tabloya eklenen. Dinamik şema açıksa, şema köklü verileri algılanır.

Açıkça çağırın öneririz `tables.initialize()` hizmet çalışmaya başladığında tablo oluşturmak için yöntem.

### <a name="Swagger"></a>Swagger desteğini etkinleştir
Mobile Apps ile yerleşik gelen [Swagger] destekler. Swagger desteğini etkinleştirmek için önce bir bağımlılık olarak swagger kullanıcı arabirimini yükleyin:

    npm install --save swagger-ui

Ardından, Mobile Apps Oluşturucusu Swagger desteği etkinleştirebilirsiniz:

    var mobile = azureMobileApps({ swagger: true });

Geliştirme sürümlerinde Swagger desteğini etkinleştirmek için büyük olasılıkla yalnızca istiyorsanız. Kullanarak bunu yapabilirsiniz `NODE_ENV` uygulama ayarı:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

`swagger` Http:// son nokta bulunduğu*yoursite*.azurewebsites.net/swagger. Swagger kullanıcı Arabirimi aracılığıyla erişebilirsiniz `/swagger/ui` uç noktası. kimlik doğrulaması, tüm uygulamanızda gerektirecek şekilde seçerseniz, Swagger bir hata oluşturur. Azure App Service kimlik doğrulama/yetkilendirme ayarları kimliği doğrulanmamış isteklere izin vermek en iyi sonuçlar için seçin ve ardından kimlik doğrulaması kullanarak kontrol `table.access` özelliği.

Yerel olarak geliştirmek için yalnızca Swagger destek istiyorsanız, Swagger seçeneği azureMobile.js dosyanıza ekleyebilirsiniz.

## <a name="a-namepushpush-notifications"></a><a name="push">Anında iletme bildirimleri
Tüm önde gelen platformlarda milyonlarca cihaza için hedeflenen anında iletme bildirimleri gönderebilmek için mobil uygulamalar Azure Notification Hubs ile tümleşir. Bildirim hub'ları kullanarak, anında iletme bildirimlerini iOS, Android ve Windows gönderebilirsiniz aygıtlar. Tüm Notification Hubs ile yapabileceğiniz hakkında daha fazla bilgi için bkz: [Notification Hubs'a genel bakış](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Anında iletme bildirimleri gönderme
Aşağıdaki kodu nasıl kullanılacağını gösterir `push` nesne kayıtlı iOS cihazlara bir yayın anında iletme bildirimi göndermek için:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured.
    if (context.push) {
        // Send a push notification by using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

İstemciden bir şablon İtme kaydı oluşturarak, bir şablon anında iletme iletisi tüm desteklenen platformlarda aygıtlara yerine gönderebilirsiniz. Aşağıdaki kod, şablon bildirim göndermek gösterilmektedir:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured.
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


### <a name="push-user"></a>Kimliği doğrulanmış bir kullanıcı etiketleri kullanarak anında iletme bildirimleri gönderme
Kimliği doğrulanmış bir kullanıcı için anında iletme bildirimleri kaydolduğunda, bir kullanıcı kimliği etiketi kayıt için otomatik olarak eklenir. Bu etiket kullanarak, belirli bir kullanıcı tarafından kaydedilen tüm cihazlara anında iletme bildirimleri gönderebilirsiniz. Aşağıdaki kod istekte ve o kullanıcı için her aygıt kaydı için bir şablon anında iletme bildirimi gönderir kullanıcının SID alır:

    // Only do the push if configured.
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Kimliği doğrulanmış bir istemci anında iletme bildirimleri için kaydetme, kaydı çalışmadan önce kimlik doğrulamasının tam olduğundan emin olun.

## <a name="CustomAPI"></a> Özel API'leri
### <a name="howto-customapi-basic"></a>Özel bir API tanımlayın
Veri erişim API üzerinden yanı sıra `/tables` uç noktası, Mobile Apps özel API kapsamı sağlayabilir. Özel API'leri tablo tanımları benzer bir şekilde tanımlanır ve kimlik doğrulaması dahil tesisler aynı erişebilir.

App Service kimlik doğrulaması ile özel bir API kullanmak isterseniz, App Service kimlik doğrulaması yapılandırmalısınız [Azure portal] ilk. Daha fazla bilgi için kullanmak istediğiniz kimlik sağlayıcısı için Yapılandırma Kılavuzu'na bakın:

* [Azure Active Directory kimlik doğrulamasını yapılandırma]
* [Facebook kimlik doğrulamasını yapılandırma]
* [Google kimlik doğrulamasını yapılandırma]
* [Microsoft kimlik doğrulamasını yapılandırma]
* [Twitter kimlik doğrulamasını yapılandırma]

Özel API'ları, tabloları API kadar aynı şekilde tanımlanır:

1. Oluşturma bir `api` dizin.
2. API tanımı JavaScript dosyasını içinde oluşturmak `api` dizin.
3. İçeri aktarmak için alma yöntemini kullanmak `api` dizin.

Daha önce kullanılan basic uygulama örneği temel alarak API tanımı prototip şöyledir:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the custom API.
    mobile.api.import('./api');

    // Add the Mobile API so it is accessible as a Web API.
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Bir örnek kullanılarak sunucu tarihi döndürür API atalım `Date.now()` yöntemi. Api/date.js dosya şöyledir:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Her bir parametreyi standart RESTful fiillerden biri: GET, POST, düzeltme eki veya silme. Standart bir yöntemdir [ExpressJS Ara] zorunlu çıkış gönderir işlevi.

### <a name="howto-customapi-auth"></a>Özel bir API erişimi için kimlik doğrulaması gerektirir
Kimlik doğrulama Mobile Apps SDK'sı aynı şekilde her ikisi için uygulayan `tables` uç noktasını ve özel API'leri. Önceki bölümde geliştirilen API kimlik doğrulaması eklemek için Ekle bir `access` özelliği:

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

İçin kullanılan aynı belirteci `tables` uç noktası kimlik doğrulaması gerektiren özel API'leri için kullanılmalıdır.

### <a name="howto-customapi-auth"></a>Tanıtıcı büyük dosyayı yükler
Mobile Apps SDK'sı [gövde ayrıştırıcı Ara](https://github.com/expressjs/body-parser) kabul etmek ve gönderme işleminiz gövdesi içeriği kod çözme için. Daha büyük dosya yüklemeleri kabul etmek için gövde-ayrıştırıcı önceden yapılandırabilirsiniz:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling.
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the custom API.
    mobile.api.import('./api');

    // Add the Mobile API so it is accessible as a Web API.
    app.use(mobile);

    // Start listening on HTTP.
    app.listen(process.env.PORT || 3000);

Base-64 kodlanmış Aktarımdan önce dosyasıdır. Bu kodlama, gerçek karşıya yükleme (ve için hesap boyutu) boyutunu artırır.

### <a name="howto-customapi-sql"></a>Özel SQL deyimlerini yürütmek
Mobile Apps SDK'sı istek nesnesi aracılığıyla tüm içeriği erişim sağlar. Parametreli SQL deyimlerini tanımlanmış veri sağlayıcısına kolayca çalıştırabilirsiniz:

    var api = {
        get: function (request, response, next) {
            // Check for parameters. If not there, pass on to a later API call.
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query. Anything that the mssql
            // driver can handle is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query. The context for Mobile Apps is available through
            // request.azureMobile. The data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Hata ayıklama, kolay tablolar ve kolay API'leri
### <a name="howto-diagnostic-logs"></a>Hata ayıklama, tanılama ve Mobile Apps sorunlarını giderme
Azure uygulama hizmeti birkaç hata ayıklama ve sorun giderme tekniklerini Node.js uygulamaları için sağlar.
Node.js Mobile Apps arka uç sorunlarını giderme çalışmaya başlamak için aşağıdaki makalelere bakın:

* [Azure uygulama hizmeti izleme]
* [Azure App Service'te tanılama günlük kaydını etkinleştir]
* [Visual Studio'da Azure uygulama hizmeti sorunlarını giderme]

Node.js uygulamalarını çok çeşitli tanılama günlük araçları erişimi. Mobile Apps Node.js SDK'sı düzenlenmemeli [Winston] tanılama günlük için. Günlüğü otomatik olarak etkin hata ayıklama etkinleştirdiğinizde modu veya kümesi `MS_DebugMode` uygulama ayarı true [Azure portal]. Oluşturulan günlüklerin görünür tanılama günlüklerine [Azure portal].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Azure portalında kolay tabloları ile çalışma
Oluşturma ve tabloları doğrudan portalda çalışmak için kolay tabloları kullanabilir. CSV biçiminde kolay tablolara veri kümesi yükleyebilirsiniz. Sistem özellik adlarını Mobile Apps arka ucu ile çakışan özellik adları (veri kümesinde, CSV) kullanamayacağınızı unutmayın. Sistemi özellik adları şunlardır:
* CreatedAt
* updatedAt
* silindi
* sürüm

Uygulama hizmeti Düzenleyicisi'ni kullanarak, tablo işlemleri bile düzenleyebilirsiniz. Seçtiğinizde, **kolay tabloları** arka uç site ayarlarınızı ekleyebilir, değiştirebilir veya bir tablo silme. Tablodaki verileri de görebilirsiniz.

![Kolay tablolarla çalışma](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Aşağıdaki komutlar bir tablo için komut çubuğunda kullanılabilir:

* **İzinleri Değiştir**: Okuma izinlerini değiştirmek, ekleme, güncelleştirme ve silme işlemleri tablo üzerinde.
 Kimlik doğrulaması gerektiren ya da işlemi tüm erişimi devre dışı bırakmak için anonim erişime izin verecek şekilde seçeneklerdir.
* **Komut dosyasını Düzenle**: Tablo için komut dosyası App Service Düzenleyicisi'nde açılır.
* **Şemayı yönetmek**: ekleyin veya sütunları Sil ya da tablo dizini değiştirin.
* **Düz tablo**: tüm veri satırları silerek varolan bir tabloyu kesin ancak şemanın değişmeden.
* **Satırları Sil**: veri tek tek satırları silme.
* **Akış günlükleri Görünüm**: siteniz için akış günlüğü hizmetine bağlanın.

### <a name="work-easy-apis"></a>Azure portalında kolay API'leri ile çalışma
Oluşturma ve özel API'lerini doğrudan portalda çalışmak için kolay API'leri kullanın. Uygulama hizmeti Düzenleyicisi'ni kullanarak API komut dosyalarını düzenleyebilirsiniz.

Seçtiğinizde, **kolay API'leri** arka uç site ayarlarınızı ekleyebilir, değiştirebilir veya özel bir API uç noktasını silmek.

![Kolay API'leri ile çalışma](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Portalda, bir HTTP eylemi için erişim izinlerini değiştirmek, API komut dosyası App Service Düzenleyicisi'ni düzenleyin veya akış günlükleri görüntüleyin.

### <a name="online-editor"></a>Kodu App Service Düzenleyicisi'nde Düzenle
Azure Portalı'nı kullanarak, yerel bilgisayarınıza projeye yüklemeye gerek kalmadan Node.js arka uç komut dosyalarınızı App Service Düzenleyicisi'ni düzenleyebilirsiniz. Çevrimiçi düzenleyicisinde komut dosyalarını düzenlemek için:

1. Mobile Apps arka uç bölmesinde seçin **tüm ayarları** > ya da **kolay tablolar** veya **kolay API'leri**. Bir tablo veya API seçin ve ardından **Düzenle betik**. Komut dosyası App Service Düzenleyicisi'nde açar.

   ![App Service Düzenleyicisi](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
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
[Windows Store istemcisi hızlı başlangıç]: app-service-mobile-windows-store-dotnet-get-started.md
[çevrimdışı veri eşitlemeye]: app-service-mobile-offline-data-sync.md
[Azure Active Directory kimlik doğrulamasını yapılandırma]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook kimlik doğrulamasını yapılandırma]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[Google kimlik doğrulamasını yapılandırma]: ../app-service/app-service-mobile-how-to-configure-google-authentication.md
[Microsoft kimlik doğrulamasını yapılandırma]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter kimlik doğrulamasını yapılandırma]: ../app-service/app-service-mobile-how-to-configure-twitter-authentication.md
[Azure uygulama hizmeti Dağıtım Kılavuzu'nu]: ../app-service/app-service-deploy-local-git.md
[Azure uygulama hizmeti izleme]: ../app-service/web-sites-monitor.md
[Azure App Service'te tanılama günlük kaydını etkinleştir]: ../app-service/web-sites-enable-diagnostic-log.md
[Visual Studio'da Azure uygulama hizmeti sorunlarını giderme]: ../app-service/web-sites-dotnet-troubleshoot-visual-studio.md
[düğümü sürüm belirtin]: ../nodejs-specify-node-version-azure-apps.md
[düğümü modüllerini kullanma]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure portal]: https://portal.azure.com/
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
