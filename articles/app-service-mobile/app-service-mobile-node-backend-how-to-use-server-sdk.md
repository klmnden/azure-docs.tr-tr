---
title: Mobile Apps için Node.js arka uç sunucu SDK'sı ile çalışma | Microsoft Docs
description: Azure App Service Mobile Apps Node.js arka uç sunucu SDK'sı ile çalışmayı öğrenin.
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
ms.openlocfilehash: 831f6b4bdc99e63859b390f8a9bb88d74301284e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62128109"
---
# <a name="how-to-use-the-mobile-apps-nodejs-sdk"></a>Mobile Apps Node.js SDK'sını kullanma

[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Bu makalede ayrıntılı bilgiler sağlar ve arka uç Azure App Service Mobile Apps özelliği, bir Node.js ile nasıl çalışılacağını gösteren örnekler.

## <a name="Introduction"></a>Giriş

Mobile Apps mobil için iyileştirilmiş veri erişim Web API'sini bir web uygulamasına ekleme yeteneği sağlar. ASP.NET ve Node.js web uygulamaları için Mobile Apps SDK'sı sağlanır. SDK'sı aşağıdaki işlemleri sağlar:

* Tablo işlemleri (okuma, ekleme, güncelleştirme ve silme) veri erişim
* Özel API işlemleri

Her iki işlemleri, Azure App Service veren tüm kimlik sağlayıcıları kimlik doğrulaması için sağlar. Bu sağlayıcıları, Facebook, Twitter, Google ve Microsoft yanı sıra Azure Active Directory gibi sosyal kimlik sağlayıcıları için Kurumsal kimlik içerir.

Her kullanım örneğine örnekleri bulabilirsiniz [github'daki örnekler dizini].

## <a name="supported-platforms"></a>Desteklenen platformlar

Mobile Apps Node.js SDK'sı, düğümün ve geçerli LTS yayın destekler. Şu anda, en son LTS düğüm v4.5.0 sürümüdür. Düğüm'ın diğer sürümlerinin çalışabilir ancak bu desteklenmez.

Mobile Apps Node.js SDK'sı iki veritabanı sürücüleri destekler: 

* Düğüm mssql sürücüsü, Azure SQL veritabanı ve yerel SQL Server örneklerini destekler.  
* Sqlite3 sürücüsü yalnızca tek bir örneği üzerinde SQLite veritabanlarını destekler.

### <a name="howto-cmdline-basicapp"></a>Komut satırını kullanarak temel bir Node.js arka ucu oluşturma

Her Mobile Apps Node.js arka ucu ExpressJS uygulamanın başlar. ExpressJS en popüler web hizmeti için Node.js kullanılabilir çerçevesidir. Bir temel oluşturabilirsiniz [Express] aşağıdaki gibi uygulama:

1. Bir komut veya PowerShell penceresinde, projeniz için bir dizin oluşturun:

        mkdir basicapp

1. Çalıştırma `npm init` paket yapısı başlatılamadı:

        cd basicapp
        npm init

   `npm init` Komutu bir proje başlatmak için soru sorar. Örnek bir çıktı görürsünüz:

   ![Npm init çıkış][0]

1. Yükleme `express` ve `azure-mobile-apps` npm deposu kitaplıklarından:

        npm install --save express azure-mobile-apps

1. Temel mobil sunucunun uygulamak için bir app.js dosyası oluşturun:

    ```javascript
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
    ```

Bu uygulamayı bir mobil için iyileştirilmiş Web API'si tek bir uç nokta ile oluşturur. (`/tables/TodoItem`) dinamik şemayı kullanarak bir temel SQL data store kimlik doğrulamasız erişimi sağlar. İstemci Kitaplığı hızlı başlangıçları izlemek için uygundur:

* [Android istemci hızlı başlangıç]
* [Apache Cordova istemci hızlı başlangıç]
* [iOS istemci hızlı başlangıç]
* [Windows Store istemcisi hızlı başlangıç]
* [Xamarin.iOS istemci hızlı başlangıç]
* [Xamarin.Android istemci hızlı başlangıç]
* [Xamarin.Forms istemci hızlı başlangıç]

Temel bu uygulamayla ilgili kodu bulabilirsiniz [basicapp örneği github'daki].

### <a name="howto-vs2015-basicapp"></a>Visual Studio 2015'i kullanarak bir Node.js arka ucu oluşturma

IDE içinden Node.js uygulamaları geliştirmek için bir uzantı Visual Studio 2015 gerektirir. Başlamak için Yükle [Visual Studio için Node.js araçları 1.1]. Yüklemeyi bitirdikten sonra bir Express 4.x uygulama oluşturun:

1. Açık **yeni proje** iletişim kutusu (gelen **dosya** > **yeni** > **proje**).
1. Genişletin **şablonları** > **JavaScript** > **Node.js**.
1. Seçin **temel Azure Node.js Express 4 uygulaması**.
1. Proje adını girin. **Tamam**’ı seçin.

   ![Visual Studio 2015 yeni proje][1]
1. Sağ **npm** düğümünü seçip alt **yükleme yeni npm paketleri**.
1. İlk Node.js uygulamanızı oluşturduktan sonra npm Kataloğu yenilemeniz gerekebilir. Seçin **Yenile** gerekirse.
1. Girin **azure mobile apps** arama kutusuna. Seçin **azure mobile apps 2.0.0** paketini ve ardından **yükleme paketi**.

   ![Yeni npm paketlerini yükle][2]
1. Seçin **Kapat**.
1. Mobile Apps SDK'sı için destek eklenecek şekilde app.js dosyasını açın. AT 6 at kitaplığının bir alt çizgi `require` ifadeleri, aşağıdaki kodu ekleyin:

    ```javascript
    var bodyParser = require('body-parser');
    var azureMobileApps = require('azure-mobile-apps');
    ```

    AT yaklaşık satır 27 art arda `app.use` ifadeleri, aşağıdaki kodu ekleyin:

    ```javascript
    app.use('/users', users);

    // Mobile Apps initialization
    var mobile = azureMobileApps();
    mobile.tables.add('TodoItem');
    app.use(mobile);
    ```

    Dosyayı kaydedin.

1. Ya da uygulamayı yerel olarak çalıştırma (API üzerinde sunulan `http://localhost:3000`) veya Azure'a yayımlayın.

### <a name="create-node-backend-portal"></a>Azure portalını kullanarak bir Node.js arka ucu oluşturma

Bir Mobile Apps arka ucu sağ oluşturabilirsiniz, [Azure portal]. Aşağıdaki adımları izleyin veya aşağıdaki istemci ve sunucu birlikte oluşturabilirsiniz [mobil uygulama oluşturma](app-service-mobile-ios-get-started.md) öğretici. Öğreticinin bu yönergeleri basitleştirilmiş bir sürümünü içerir ve kavram kanıtı projeler için idealdir.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Geri **başlama** bölmesi altında **tablo API'si Oluştur**, seçin **Node.js** arka uç dilinizi olarak.
Kutusunu seçin **bu tüm site içeriğinin üzerine yazacağını kabul ediyorum**ve ardından **Todoıtem tablosu oluştur**.

### <a name="download-quickstart"></a>Git kullanarak Node.js arka uç hızlı başlangıç kod projesini indirin

Portal kullanarak bir Node.js Mobile Apps arka ucu oluşturduğunuzda **Hızlı Başlangıç** bölmesinde bir Node.js projesi sizin için oluşturulan ve dağıtılan sitenize. Portalda, tablo ve API ekleme ve Node.js arka ucu için kod dosyaları düzenleme. Böylece ekleyin veya tablolar ve API'leri değiştirin ve sonra projeyi yeniden yayımlamanız arka uç projeyi indirmek için çeşitli dağıtım araçları da kullanabilirsiniz. Daha fazla bilgi için [Azure App Service Dağıtım Kılavuzu].

Aşağıdaki yordam, hızlı başlangıç proje kodu indirmek için bir Git deposu kullanır:

1. Zaten yapmadıysanız, Git, yükleyin. Git'i yüklemek için gerekli adımlar, işletim sistemleri arasında farklılık gösterir. İşletim sistemine özgü dağıtımları ve yükleme yönergeleri için bkz. [yükleme Git](https://git-scm.com/book/en/Getting-Started-Installing-Git).
2. Bkz: [deponuzu hazırlama](../app-service/deploy-local-git.md#prepare-your-repository) arka uç sitenizde Git deposunu etkinleştirmenize. Dağıtım kullanıcı adı ve parolayı not edin.
3. Mobile Apps arka ucu için bölmesinde Not **Git kopya URL'si** ayarı.
4. Yürütme `git clone` Git kopya URL'si kullanarak komutu. Gerektiğinde, aşağıdaki örnekte olduğu gibi parolanızı girin:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. Yerel dizine gözatın (`/todolist` önceki örnekte) ve proje dosyaları yüklendi dikkat edin. Todoitem.json dosyasında `/tables` dizin. Bu dosya, tablo üzerinde izinleri tanımlar. Ayrıca aynı dizinde todoitem.js dosyayı bulun. Bu tablo için CRUD işlemi betikleri tanımlar.
6. Proje dosyalarını eklemek için aşağıdaki komutları çalıştırın, değişiklikleri yaptıktan sonra kaydedin ve ardından değişiklikleri siteye karşıya yükleyin:

        $ git commit -m "updated the table script"
        $ git push origin master

   Projeye yeni dosyalar eklediğinizde, ilk kez çalıştırmanız gereken `git add .` komutu.

Site için site her yeni bir işleme gönderiminde yayımlanmadan.

### <a name="howto-publish-to-azure"></a>Node.js arka ucunuza Azure'a yayımlama

Microsoft Azure, Azure hizmeti için arka uç, Mobile Apps Node.js yayımlamak için birçok mekanizma sağlar. Bu mekanizmalar Visual Studio'ya entegre Dağıtım Araçları komut satırı araçlarını ve kaynak denetimine bağlı sürekli dağıtım seçenekleri içerir. Daha fazla bilgi için [Azure App Service Dağıtım Kılavuzu].

Azure App Service sahip tavsiyeler gözden geçirmeniz gereken Node.js uygulamaları için arka uç yayımlamadan önce:

* Nasıl yapılır [düğüm sürümü belirtin]
* Nasıl yapılır [düğüm modüllerini kullanın]

### <a name="howto-enable-homepage"></a>Uygulamanız için bir giriş sayfasını etkinleştir

Birçok uygulama, web ve mobil uygulamalar için kullanılan bir bileşimidir. Şu iki modelleri birleştirilecek ExpressJS framework kullanabilirsiniz. Bazı durumlarda, ancak yalnızca bir mobil arabirimi uygulamak isteyebilirsiniz. Bu, app service'kurmak olduğundan emin olmak için bir giriş sayfası sağlamak kullanışlı ve çalışır durumdadır. Kendi giriş sayfası belirtin veya geçici bir giriş sayfası etkinleştirin. Geçici bir giriş sayfası etkinleştirmek için Mobile Apps oluşturmak için aşağıdaki kodu kullanın:

```javascript
var mobile = azureMobileApps({ homePage: true });
```

Yalnızca kullanılabilen bu seçenek yerel olarak geliştirirken istiyorsanız, bu ayar azureMobile.js dosyanıza ekleyebilirsiniz.

## <a name="TableOperations"></a>Tablo işlemleri

Azure mobile apps Node.js sunucu SDK'sı, bir Web API'si olarak Azure SQL veritabanında depolanan veri tabloları kullanıma sunmak için mekanizmaları sağlar. Beş işlem olanakları sunar:

| İşlem | Açıklama |
| --- | --- |
| GET /tables/*tablename* |Tablodaki tüm kayıtları alın. |
| GET /tables/*tablename*/:id |Belirli bir kaydın tabloda alın. |
| POST /tables/*tablename* |Tabloya bir kayıt oluşturun. |
| Düzeltme eki /tables/*tablename*/:id |Bir kayıttaki tabloyu güncelleştirin. |
| DELETE /tables/*tablename*/:id |Tabloda bir kaydını silin. |

Bu Web API'sini destekleyen [OData] ve desteklemek için tablo şemasını genişletir [çevrimdışı veri eşitleme].

### <a name="howto-dynamicschema"></a>Dinamik şema kullanarak tabloları tanımlama

Bir tablo kullanabilmeniz için önce tanımlamanız gerekir. (Burada yer alan şemada sütunlar tanımlarsınız) statik bir şema kullanarak tablolar tanımlayabilirsiniz veya dinamik olarak (SDK'sı gelen istekleri temel alan şema denetimleri nerede). Buna ek olarak, tanımına JavaScript kodu ekleyerek Web API'si belirli yönlerini denetleyebilirsiniz.

En iyi uygulama, bir JavaScript dosyasında her tabloya tanımlamalısınız `tables` dizin ve ardından `tables.import()` tabloları yöntemi. Temel uygulama örnek genişletme, app.js dosyasını ayarlamanız:

```javascript
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
```

Tablodaki tanımlar. / tables/TodoItem.js:

```javascript
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// Additional configuration for the table goes here.

module.exports = table;
```

Tablolar varsayılan olarak dinamik şemayı kullanır. Dinamik şemanın kapatılması genel olarak kapatmak için ayarlanmış `MS_DynamicSchema` false Azure portalında uygulama ayarı.

Tam bir örnek bulabilirsiniz [github'da Yapılacaklar örneği].

### <a name="howto-staticschema"></a>Statik bir şema kullanarak tabloları tanımlama

Web API aracılığıyla kullanıma sunmak için sütunları açıkça tanımlayabilirsiniz. Azure mobile apps Node.js SDK'sı, sağladığınız listesine çevrimdışı veri eşitleme için gerekli tüm ek sütunları otomatik olarak ekler. Örneğin, iki sütunlu bir tablo hızlı istemci uygulamalarını gerektiren: `text` (dize) ve `complete` (Boole).  
Tablo tanımı JavaScript dosyasında tablo tanımlanabilir (bulunan `tables` dizin) gibi:

```javascript
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
```

Tabloları statik olarak tanımlarsanız, ayrıca çağırmalısınız `tables.initialize()` başlangıçta veritabanı şeması oluşturmak için yöntemi. `tables.initialize()` Yöntemi döndürür bir [promise] böylece veritabanı başlatılmadan önce web hizmeti isteklere hizmet yok.

### <a name="howto-sqlexpress-setup"></a>SQL Server Express'in yerel makinenizde geliştirme veri deposu olarak kullanın

Mobile Apps Node.js SDK'sı, kullanıma hazır veri sunulması için üç seçenek sunulur:

* Kullanım **bellek** kalıcı olmayan örnek deposu sağlamak için sürücü.
* Kullanım **mssql** sürücü geliştirme için bir SQL Server Express veri deposu sağlamak için.
* Kullanım **mssql** üretim için bir Azure SQL veritabanı veri deposu sağlamak için sürücü.

Mobile Apps Node.js SDK'sı kullanan [mssql Node.js paketi] kurmak ve SQL Server Express ve SQL veritabanına bir bağlantı kullanın. Bu paket, SQL Server Express örneğinizi TCP bağlantılarını etkinleştirmenizi gerektirir.

> [!TIP]
> Bellek sürücü özellikleri test için eksiksiz bir kümesini sağlamaz. Arka ucunuza yerel olarak test etmek isterseniz, bir SQL Server Express veri deposu ve mssql sürücü kullanılmasını öneririz.

1. İndirme ve yükleme [Microsoft SQL Server 2014 Express]. SQL Server 2014 Express ile araçları sürümü yüklediğinizden emin olun. 64-bit desteği açıkça gerektirmedikçe 32-bit sürümünü çalıştırırken daha az bellek tüketir.
1. SQL Server 2014 yapılandırma yöneticisini çalıştırın:

   a. Genişletin **SQL Server Ağ Yapılandırması** düğüm ağacı menüsünde.

   b. Seçin **SQLEXPRESS protokolleri**.

   c. Sağ **TCP/IP'yi** seçip **etkinleştirme**. Seçin **Tamam** açılır iletişim kutusunda.

   d. Sağ **TCP/IP'yi** seçip **özellikleri**.

   e. Seçin **IP adresleri** sekmesi.

   f. Bulma **IPAll** düğümü. İçinde **TCP bağlantı noktası** alanına **1433**.

      ![SQL Server Express için TCP/IP'yi yapılandırma][3]

   g. **Tamam**’ı seçin. Seçin **Tamam** açılır iletişim kutusunda.

   h. Seçin **SQL Server Hizmetleri** ağaç menüsünde.

   i. Sağ **SQL Server (SQLEXPRESS)** seçip **yeniden**.

   j. SQL Server 2014 Yapılandırma Yöneticisi'ni kapatın.

1. SQL Server 2014 Management Studio çalıştırın ve, yerel SQL Server Express örneğine bağlanın:

   1. Object Explorer Örneğinizde sağ tıklayıp **özellikleri**.
   1. Seçin **güvenlik** sayfası.
   1. Emin **SQL Server ve Windows kimlik doğrulaması modu** seçilir.
   1. **Tamam**’ı seçin.

      ![SQL Server Express kimlik doğrulamasını yapılandırma][4]
   1. Genişletin **güvenlik** > **oturumları** nesne Gezgini'nde.
   1. Sağ **oturumları** seçip **yeni oturum açma**.
   1. Bir oturum açma adı girin. **SQL Server kimlik doğrulaması**’nı seçin. Bir parola girin ve ardından aynı parolayı girin **parolayı onayla**. Parola Windows karmaşıklık gereksinimlerini karşılaması gerekir.
   1. **Tamam**’ı seçin.

      ![SQL Server Express için yeni kullanıcı ekleme][5]
   1. Yeni oturum açma bilgilerinizi sağ tıklayıp **özellikleri**.
   1. Seçin **sunucu rolleri** sayfası.
   1. Onay kutusunu seçin **dbcreator** sunucu rolü.
   1. **Tamam**’ı seçin.
   1. SQL Server 2015 Management Studio'yu kapatın.

Kullanıcı adı ve seçtiğiniz parolayı kaydettiğinizden emin olun. Ek sunucu rolleri veya veritabanı gereksinimlerinize bağlı olarak izinleri atamak gerekebilir.

Node.js uygulaması okuma `SQLCONNSTR_MS_TableConnectionString` bu veritabanı için bağlantı dizesi ortam değişkeni. Ortamınızda bu değişkeni ayarlayabilirsiniz. Örneğin, bu ortam değişkenini ayarlamak için PowerShell kullanabilirsiniz:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Veritabanına bir TCP/IP bağlantısı üzerinden erişir. Bağlantı için bir kullanıcı adı ve parola sağlayın.

### <a name="howto-config-localdev"></a>Projenizi yerel geliştirme için yapılandırın

Mobile Apps adlı bir JavaScript dosyasını okur *azureMobile.js* yerel dosya sisteminden. Bu dosya, üretimde Mobile Apps SDK'sını yapılandırmak için kullanmayın. Bunun yerine, **uygulama ayarları** içinde [Azure portal].

Bir yapılandırma nesnesi, azureMobile.js dosyasını dışarı aktarmanız gerekir. En yaygın ayarlar şunlardır:

* Veritabanı ayarları
* Tanılama günlüğüne kaydetme ayarlarını
* CORS ayarlarını değiştir

Bu örnekte **azureMobile.js** dosya önceki veritabanı ayarlarını uygular:

```javascript
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
```

Eklediğiniz öneririz **azureMobile.js** için **.gitignore** dosya (veya diğer kaynak kodu denetimi yoksayma dosyası) bulutta depolanan parolaları önlemek için. Her zaman üretim ayarlarında yapılandırdığınız **uygulama ayarları** içinde [Azure portal].

### <a name="howto-appsettings"></a>Mobil uygulamanız için uygulama ayarlarını yapılandırma

Çoğu ayarı azureMobile.js dosyasında eşdeğer uygulama ayarı olmayan [Azure portal]. Uygulamanızı yapılandırmak için aşağıdaki listeyi kullanın **uygulama ayarları**:

| Uygulama ayarı | azureMobile.js ayarı | Açıklama | Geçerli değerler |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |name |Uygulamanın adı |string |
| **MS_MobileLoggingLevel** |Logging.level |Günlüğe kaydedilecek ileti sayısı en düşük günlük düzeyi |hata, uyarı, bilgi, ayrıntılı, hata ayıklama, saçma |
| **MS_DebugMode** |Hata ayıklama |Etkinleştirir veya hata ayıklama modunu devre dışı bırakır |TRUE, false |
| **MS_TableSchema** |Data.Schema |SQL tabloları için varsayılan şema adı |dize (varsayılan: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Etkinleştirir veya hata ayıklama modunu devre dışı bırakır |TRUE, false |
| **MS_DisableVersionHeader** |Sürüm (çok tanımlanmamış ayarlanır) |X-ZUMO-Server-Version üstbilgi devre dışı bırakır |TRUE, false |
| **MS_SkipVersionCheck** |skipversioncheck |İstemci API sürümü denetimi devre dışı bırakır |TRUE, false |

Bir uygulama ayarı için:

1. [Azure Portal] oturum açın.
1. Seçin **tüm kaynakları** veya **uygulama hizmetleri**ve ardından mobil uygulamanızın adını seçin.
1. **Ayarları** bölmesi, varsayılan olarak açılır. Bu işaretlemezse **ayarları**.
1. Üzerinde **genel** menüsünde **uygulama ayarları**.
1. Kaydırma **uygulama ayarları** bölümü.
1. Uygulama ayarı zaten varsa, uygulama ayarının değerini düzenlemek için bir değer seçin.
   Uygulama ayarı mevcut değilse, uygulama ayarlarında girin **anahtarı** kutusu ve değer **değer** kutusu.
1. **Kaydet**’i seçin.

Çoğu uygulama ayarları değiştirme bir yeniden başlatma gerektirir.

### <a name="howto-use-sqlazure"></a>Depolama, üretim veri olarak SQL veritabanını kullan

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Bir veri deposu olarak Azure SQL veritabanı ile tüm Azure App Service uygulama türlerinde aynıdır. Bunu zaten yapmadıysanız, bir Mobile Apps arka ucu oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal] oturum açın.
1. Pencerenin üst sol seçin **+ yeni** düğmesi > **Web + mobil** > **mobil uygulama**ve, Mobile Apps arka ucu için bir ad belirtin.
1. İçinde **kaynak grubu** kutusuna, uygulamanızla aynı adı girin.
1. App Service planı varsayılan seçilidir. App Service planınızı değiştirmek istiyorsanız:

   a. Seçin **App Service planı** >  **+ Yeni Oluştur**.

   b. Yeni App Service planının adını belirtin ve uygun bir konum seçin.

   c. Hizmet için uygun bir fiyatlandırma katmanı seçin. Seçin **tümünü görüntüle** seçenekleri gibi daha fazla fiyatlandırma görünümüne **ücretsiz** ve **paylaşılan**.

   d. Tıklayın **seçin** düğmesi.

   e. Geri **App Service planı** bölmesinde **Tamam**.
1. **Oluştur**’u seçin.

Sağlama Mobile Apps arka ucu birkaç dakika sürebilir. Mobile Apps geri sonra son hazırlanır, portal açar **ayarları** Mobile Apps arka ucu için bölmesi.

Mevcut bir SQL veritabanını Mobile Apps arka ucunuza bağlanmak veya bir SQL veritabanı oluşturmayı seçebilirsiniz. Bu bölümde, bir SQL veritabanı oluşturacağız.

> [!NOTE]
> Mobile Apps arka ucu olarak zaten bir veritabanı aynı konumda varsa, bunun yerine seçebilirsiniz **varolan veritabanını kullan** ve bu veritabanını seçin. Daha yüksek gecikme nedeniyle farklı bir konumda bir veritabanının kullanılmasını önermeyiz.

1. Yeni Mobile Apps arka uçta, seçin **ayarları** > **mobil uygulama** > **veri** >  **+ Ekle**.
1. İçinde **veri bağlantısı ekleme** bölmesinde **SQL veritabanı - gerekli ayarları Yapılandır** > **yeni veritabanı oluştur**. Yeni veritabanı adını girin **adı** kutusu.
1. Seçin **sunucu**. İçinde **yeni sunucu** bölmesinde bir benzersiz sunucu adını girin **sunucu adı** kutusuna ve uygun Sunucu Yöneticisi oturum açma ve parola sağlayın. Emin **azure hizmetlerinin sunucuya erişmesine izin** seçilir. **Tamam**’ı seçin.

   ![Bir Azure SQL veritabanı oluşturma][6]
1. İçinde **yeni veritabanı** bölmesinde **Tamam**.
1. Geri **veri bağlantısı ekleme** bölmesinde **bağlantı dizesi**, oturum açma ve veritabanını oluştururken belirttiğiniz parolayı girin. Varolan bir veritabanını kullanırsanız, o veritabanı için oturum açma kimlik bilgilerini sağlayın. **Tamam**’ı seçin.
1. Geri **veri bağlantısı ekleme** bölmesinde tekrar seçin **Tamam** veritabanını oluşturmak için.

<!--- END OF ALTERNATE INCLUDE -->

Veritabanı oluşturma birkaç dakika sürebilir. Kullanım **bildirimleri** dağıtımın ilerleme durumunu izlemek için alan. Veritabanı başarılı olarak dağıtılmadıkça ilerleme değil. Veritabanı dağıtıldıktan sonra bir bağlantı dizesi, Mobile Apps arka uç uygulaması ayarları'nda SQL veritabanı örneği oluşturulur. Bu uygulama ayarında gördüğünüz **ayarları** > **uygulama ayarları** > **bağlantı dizeleri**.

### <a name="howto-tables-auth"></a>Tablolar için erişim için kimlik doğrulaması gerektir

App Service kimlik doğrulaması ile kullanmak istiyorsanız `tables` uç noktasını yapılandırmanız gerekir, App Service kimlik doğrulaması [Azure portal] ilk. Daha fazla bilgi için kullanmak istediğiniz kimlik sağlayıcısı için yapılandırma kılavuzuna bakın:

* [Azure Active Directory kimlik doğrulamasını yapılandırma]
* [Facebook kimlik doğrulamasını yapılandırma]
* [Google kimlik doğrulamasını yapılandırma]
* [Microsoft kimlik doğrulamasını yapılandırma]
* [Twitter kimlik doğrulamasını yapılandırma]

Her tablo tablo erişimi denetlemek için kullanabileceğiniz bir erişim özelliğine sahiptir. Aşağıdaki örnek, statik olarak tanımlanan bir tablo ile kimlik doğrulaması gerekli gösterir.

```javascript
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
```

Erişim özellik üç değerden birini alabilir:

* *Anonim* istemci uygulaması kimlik doğrulaması olmadan verileri okumak için izin verildiğini gösterir.
* *Kimliği doğrulanmış* istemci uygulaması isteği geçerli bir kimlik doğrulama belirteciyle göndermelisiniz gösterir.
* *devre dışı* Bu tablo şu anda devre dışı olduğunu belirtir.

Access özelliği tanımlanmamış kimliği doğrulanmamış erişime izin verilir.

### <a name="howto-tables-getidentity"></a>Kimlik doğrulaması talep tablolarınızı ile kullanma
Kimlik doğrulaması ayarlarken, istenen çeşitli beyanların ayarlayabilirsiniz. Bu talepler aracılığıyla normalde kullanılabilir olmayan `context.user` nesne. Ancak, bunları kullanarak alabileceğiniz `context.user.getIdentity()` yöntemi. `getIdentity()` Yöntemi, bir nesneye çözümler promise döndürür. Nesne kimlik doğrulama yöntemi tarafından Anahtarlanan (`facebook`, `google`, `twitter`, `microsoftaccount`, veya `aad`).

Örneğin, Microsoft hesabı kimlik doğrulaması ve e-posta adresi talep isteği ayarlama, aşağıdaki tabloda denetleyicisiyle kayıt e-posta adresi ekleyebilirsiniz:

```javascript
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
```

Hangi talepleri kullanılabilir olduğunu görmek için görüntülemek için bir web tarayıcısı kullanın `/.auth/me` sitenizin uç noktası.

### <a name="howto-tables-disabled"></a>Belirli bir tablo işlemleri erişimi devre dışı

Tablo üzerinde görünen ek olarak, erişim özelliği tek işlemler denetlemek için kullanılabilir. Dört işlem vardır:

* `read` RESTful edinin tablosunda bir işlemdir.
* `insert` Tablo üzerinde RESTful POST işlemdir.
* `update` Tablo üzerinde RESTful düzeltme eki işlemi var.
* `delete` Tablo üzerinde RESTful silme işlemi var.

Örneğin, bir salt okunur kimliği doğrulanmamış tablo sağlamak isteyebilirsiniz:

```javascript
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// Read-only table. Only allow READ operations.
table.read.access = 'anonymous';
table.insert.access = 'disabled';
table.update.access = 'disabled';
table.delete.access = 'disabled';

module.exports = table;
```

### <a name="howto-tables-query"></a>Tablo işlemleri ile kullanılan sorguyu Ayarla

Tablo işlemleri için genel bir gereksinim, verilerin kısıtlanmış bir görünümü sağlamaktır. Örneğin, yalnızca okuma veya kendi kayıtlarınızı güncelleştirme şekilde kimliği doğrulanmış kullanıcı kimliği ile etiketlenir bir tablo sağlayabilir. Aşağıdaki tablo tanımı, şu işlevleri sağlar:

```javascript
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
```

Normalde sorgu çalıştırma işlemleri kullanarak ayarlayabilirsiniz. bir sorgu özelliği olan bir `where` yan tümcesi. Sorgu özelliği bir [QueryJS] arka uç veri bir OData sorgusu bir şey dönüştürmek için kullanılan nesneyi işleyebilir. (Önceki bir gibi) basit eşitlik durumlarda, bir harita kullanabilirsiniz. Ayrıca, belirli SQL yan tümce ekleyebilirsiniz:

```javascript
context.query.where('myfield eq ?', 'value');
```

### <a name="howto-tables-softdelete"></a>Bir tabloda bir geçici silme Yapılandır

Geçici silme kayıtları silmez. Bunun yerine, bunları veritabanı içinde silinen sütun true olarak ayarlayarak silinmiş olarak işaretler. Mobil istemci SDK'sı kullanmadığı sürece Mobile Apps SDK'sı geçici olarak silinen kayıtlar sonuçları otomatik olarak kaldırır. `IncludeDeleted()`. Bir tablo için geçici silme yapılandırmak için `softDelete` özelliği tablo tanımı dosyasında:

```javascript
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
```

Kayıtları silmek için bir mekanizma oluşturmanız gerekir: bir istemci uygulaması, bir Web işi, bir Azure işlevine veya özel bir API.

### <a name="howto-tables-seeding"></a>Verilerle, veritabanının çekirdeğini oluşturma

Yeni bir uygulama oluştururken, temel bir tabloyu verilerle isteyebilirsiniz. Tablo tanımı JavaScript dosyasında şu şekilde bunu yapabilirsiniz:

```javascript
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
```

Tablo oluşturmak için Mobile Apps SDK'sı yalnızca kullandığınız veri üretme olur. Tablo veritabanında zaten varsa, veri tablosu içine eklenmiş olur. Dinamik şema açıksa, şema köklü verileri algılanır.

Açıkça çağırmanızı öneririz `tables.initialize()` hizmet çalışmaya başladığında tablo oluşturmak için yöntemi.

### <a name="Swagger"></a>Swagger desteğini etkinleştir
Mobile Apps ile yerleşik gelen [Swagger] destekler. Swagger desteğini etkinleştirmek için önce bir bağımlılık olarak swagger kullanıcı arabirimini yükleyin:

    npm install --save swagger-ui

Mobile Apps Oluşturucusu Swagger desteği daha sonra etkinleştirebilirsiniz:

```javascript
var mobile = azureMobileApps({ swagger: true });
```

Geliştirme sürümlerinde Swagger desteğini etkinleştirmek için büyük olasılıkla yalnızca istediğiniz. Kullanarak bunu yapabilirsiniz `NODE_ENV` uygulama ayarı:

```javascript
var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });
```

`swagger` Uç nokta http:// bulunduğu*yoursite*.azurewebsites.net/swagger. Swagger kullanıcı Arabirimi aracılığıyla erişebileceğiniz `/swagger/ui` uç noktası. Uygulamanın tamamında doğrulamasını seçerseniz, Swagger bir hata oluşturur. Kimliği doğrulanmamış istekler, Azure App Service kimlik doğrulama/yetkilendirme ayarları izin vermek en iyi sonuçlar için seçin ve ardından kimlik doğrulaması kullanarak kontrol `table.access` özelliği.

Yerel olarak geliştirmek için yalnızca Swagger destek istiyorsanız azureMobile.js dosyanıza Swagger seçeneği de ekleyebilirsiniz.

## <a name="a-namepushpush-notifications"></a><a name="push"/>Anında iletme bildirimleri gönderme

Tüm büyük platformlar arasında milyonlarca cihaza hedefli anında iletme bildirimleri gönderebilmek için Mobile Apps için Azure Notification hubs'ı ile tümleştirilir. Notification hubs'ı kullanarak anında iletme bildirim iOS, Android ve Windows için gönderebilirsiniz cihazlar. Tüm Notification Hubs ile yapabileceğiniz hakkında daha fazla bilgi için bkz: [Notification Hubs'a genel bakış](../notification-hubs/notification-hubs-push-notification-overview.md).

### <a name="send-push"></a>Anında iletme bildirimleri gönderme

Aşağıdaki kod nasıl kullanılacağını gösterir `push` kayıtlı iOS cihazlarınıza anında iletme bildirimi yayınlayın bildirim göndermek için nesne:

```javascript
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
```

İstemciden bir şablon anında iletme kaydı oluşturarak, bir şablon anında iletme iletisi tüm desteklenen platformlarda cihaz için bunun yerine gönderebilirsiniz. Aşağıdaki kod, bir şablon bildirimi göndermek nasıl gösterir:

```javascript
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
```

### <a name="push-user"></a>Etiketleri kullanarak kimliği doğrulanmış bir kullanıcıya anında iletme bildirimleri gönderme
Kimliği doğrulanmış bir kullanıcı için anında iletme bildirimleri kaydettiğinde, bir kullanıcı kimliği etiketi kayıt için otomatik olarak eklenir. Bu etiket kullanarak, belirli bir kullanıcı tarafından kaydedilen tüm cihazlara anında iletme bildirimleri gönderebilirsiniz. Aşağıdaki kodu şablon anında iletme bildirimi, o kullanıcı için her bir cihaz kaydı için gönderir ve isteği yapan kullanıcının SID'si alır:

```javascript
// Only do the push if configured.
if (context.push) {
    // Send a notification to the current user.
    context.push.send(context.user.id, payload, function (error) {
        if (error) {
            // Do something or log the error.
        }
    });
}
```

Kimliği doğrulanmış bir istemci anında iletme bildirimleri için kaydetme, kaydı denemeden önce söz konusu kimlik doğrulamasını tam olduğundan emin olun.

## <a name="CustomAPI"></a> Özel API'ler

### <a name="howto-customapi-basic"></a>Özel bir API tanımlama

Veri erişimi API'si yanı sıra `/tables` uç noktası, Mobile Apps, özel API kapsamı sağlayabilir. Özel API'ler, tablo tanımları benzer bir şekilde tanımlanır ve olanakları, kimlik doğrulama dahil olmak üzere aynı erişebilir.

App Service kimlik doğrulaması ile özel bir API kullanmak isterseniz, App Service kimlik doğrulaması yapılandırmalısınız [Azure portal] ilk. Daha fazla bilgi için kullanmak istediğiniz kimlik sağlayıcısı için yapılandırma kılavuzuna bakın:

* [Azure Active Directory kimlik doğrulamasını yapılandırma]
* [Facebook kimlik doğrulamasını yapılandırma]
* [Google kimlik doğrulamasını yapılandırma]
* [Microsoft kimlik doğrulamasını yapılandırma]
* [Twitter kimlik doğrulamasını yapılandırma]

Özel API'ler, tablo API'si kadar aynı şekilde tanımlanır:

1. Oluşturma bir `api` dizin.
1. API tanımı bir JavaScript dosyasında oluşturma `api` dizin.
1. İçeri aktarmak için alma yöntemini kullanmak `api` dizin.

Prototip önceden kullandığımız temel uygulama örneği temel API tanımı aşağıda verilmiştir:

```javascript
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
```

Bir örnek kullanarak sunucu tarih döndüren API alalım `Date.now()` yöntemi. Api/date.js dosyası aşağıda verilmiştir:

```javascript
var api = {
    get: function (req, res, next) {
        var date = { currentTime: Date.now() };
        res.status(200).type('application/json').send(date);
    });
};

module.exports = api;
```

Her parametre standart RESTful fiilleri biridir: Al, sonrası, düzeltme eki veya Sil. Standart yöntemdir [ExpressJS ara yazılımı] gerekli çıktı gönderen bir işlev.

### <a name="howto-customapi-auth"></a>Özel bir API'ye erişim için kimlik doğrulaması gerektir

Kimlik doğrulaması Mobile Apps SDK'sı aynı şekilde hem uygulayan `tables` uç noktası ve özel API'ler. Önceki bölümde geliştirilen API kimlik doğrulaması eklemek için Ekle bir `access` özelliği:

```javascript
var api = {
    get: function (req, res, next) {
        var date = { currentTime: Date.now() };
        res.status(200).type('application/json').send(date);
    });
};
// All methods must be authenticated.
api.access = 'authenticated';

module.exports = api;
```

Kimlik doğrulaması üzerinde belirli işlemler belirtebilirsiniz:

```javascript
var api = {
    get: function (req, res, next) {
        var date = { currentTime: Date.now() };
        res.status(200).type('application/json').send(date);
    }
};
// The GET methods must be authenticated.
api.get.access = 'authenticated';

module.exports = api;
```

İçin kullanılan aynı belirteci `tables` uç nokta kimlik doğrulaması gerektiren özel API'ler için kullanılmalıdır.

### <a name="howto-customapi-auth"></a>Büyük dosya yüklemeleri işleme

Mobile Apps SDK'sı kullanan [body-parser ara yazılım](https://github.com/expressjs/body-parser) kabul edin ve kod çözme gönderiminiz gövde içeriği. Daha büyük bir dosya yükler kabul edecek şekilde body-parser önceden yapılandırabilirsiniz:

```javascript
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
```

Base-64 kodlu Aktarımdan önce dosyasıdır. Bu kodlama gerçek karşıya yükleme (ve için hesap boyutu) boyutunu artırır.

### <a name="howto-customapi-sql"></a>Özel SQL deyimlerini yürütmek

Mobile Apps SDK'sı aracılığıyla istek nesnesi tüm bağlam erişim sağlar. Parametreli SQL deyimleri tanımlanan veri sağlayıcısı için kolayca yürütebilirsiniz:

```javascript
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
```

## <a name="Debugging"></a>Hata ayıklama, kolay tablolar ve kolay API'ler

### <a name="howto-diagnostic-logs"></a>Hata ayıklama, tanılama ve Mobile Apps sorunlarını giderme

Azure App Service çeşitli hata ayıklama ve sorun giderme teknikleri Node.js uygulamaları için sağlar.
Node.js Mobile Apps arka ucunuza gidermeye başlamak için aşağıdaki makalelere bakın:

* [Azure uygulama hizmeti izleme]
* [Azure App Service'te tanılama günlük kaydını etkinleştirme]
* [Visual Studio Azure App Service'te ilgili sorunları giderme]

Node.js uygulamaları çok çeşitli tanılama günlüğü araçları erişebilir. Dahili olarak, Mobile Apps Node.js SDK'sı kullanan [Winston] tanılama günlüğüne kaydedilecek. Günlük, hata ayıklama etkinleştirdiğinizde otomatik olarak etkinleşir modu veya set `MS_DebugMode` uygulama ayarı TRUE olarak [Azure portal]. Tanılama günlükleri oluşturulan günlüklerin görünür [Azure portal].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Azure portalında kolay tablolarla çalışma

Kolay tablolar oluşturmak ve tabloları doğrudan portalda çalışmak için kullanabilirsiniz. Kolay tablolar için veri kümesi CSV biçiminde karşıya yükleyebilirsiniz. Sistem özellik adlarını Mobile Apps arka ucu ile çakışan özellik adları (veri kümesinde, CSV)'i kullanamayacağınızı unutmayın. Sistem özellik adları şunlardır:
* createdAt
* updatedAt
* silindi
* version

Ayrıca, App Service Düzenleyicisi'ni kullanarak tablo işlemleri bile düzenleyebilirsiniz. Seçtiğinizde, **kolay tablolar** arka uç site ayarlarınızda ekleyebilir, değiştirebilir veya bir tablo silme. Tablodaki verileri de görebilirsiniz.

![Kolay tablolarla çalışma](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Aşağıdaki komutlar, komut çubuğunda bir tablo için kullanılabilir:

* **İzinleri Değiştir**: Okuma izinlerini değiştirmek, ekleme, güncelleştirme ve silme işlemleri tablosunda.
 Kimlik doğrulaması gerektirir ya da işlemi tüm erişimi devre dışı bırakmak için anonim erişime izin vermek için seçeneklerdir.
* **Komut dosyasını Düzenle**: Betik dosyası tablo için App Service Düzenleyicisi'nde açılır.
* **Şemayı yönetme**: Ekleme sütunlarını Sil veya tablosu dizini değiştirin.
* **Tabloyu Temizle**: Tüm veri satırları silerek var olan bir tabloyu kesin ancak şemanın değişmeden.
* **Satırları Sil**: Belirli veri satırlarının silin.
* **Akış günlükleri görünümü**: Siteniz için akış günlüğü hizmetine bağlanın.

### <a name="work-easy-apis"></a>Azure portalında kolay API'ler ile çalışma

Kolay API'ler oluşturmak ve özel API'ler doğrudan portalda çalışmak için kullanabilirsiniz. App Service Düzenleyicisi'ni kullanarak API betiklerini düzenleyebilirsiniz.

Seçtiğinizde, **kolay API'ler** arka uç site ayarlarınızda ekleyebilir, değiştirebilir veya özel bir API uç noktasını sil.

![Kolay API'ler ile çalışma](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Portalda, bir HTTP eylemi için erişim izinlerini değiştirmek, App Service Düzenleyicisi'nde API komut dosyasını düzenle veya akış günlüklerini görüntüleyin.

### <a name="online-editor"></a>App Service Düzenleyicisi'nde kod düzenleme

Azure portalını kullanarak Node.js arka uç komut dosyalarınızı App Service Düzenleyicisi'nde projeyi yerel bilgisayarınıza indirmek zorunda kalmadan düzenleyebilirsiniz. Çevrimiçi Düzenleyicisi'nde komut dosyalarını düzenlemek için:

1. Kendi Mobile Apps arka ucu için bölmesinde seçin **tüm ayarlar** > ya da **kolay tablolar** veya **kolay API'ler**. Bir tablo veya API'ı seçin ve ardından **betiği Düzenle**. Komut dosyası, App Service Düzenleyicisi'nde açılır.

   ![App Service Düzenleyicisi](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
1. Çevrimiçi düzenleyiciyi kod dosyasındaki değişikliklerinizi yapın. Siz yazarken değişiklikler otomatik olarak kaydedilir.

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
[Xamarin.iOS istemci hızlı başlangıç]: app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android istemci hızlı başlangıç]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms istemci hızlı başlangıç]: app-service-mobile-xamarin-forms-get-started.md
[Windows Store istemcisi hızlı başlangıç]: app-service-mobile-windows-store-dotnet-get-started.md
[Çevrimdışı veri eşitleme]: app-service-mobile-offline-data-sync.md
[Azure Active Directory kimlik doğrulamasını yapılandırma]: ../app-service/configure-authentication-provider-aad.md
[Facebook kimlik doğrulamasını yapılandırma]: ../app-service/configure-authentication-provider-facebook.md
[Google kimlik doğrulamasını yapılandırma]: ../app-service/configure-authentication-provider-google.md
[Microsoft kimlik doğrulamasını yapılandırma]: ../app-service/configure-authentication-provider-microsoft.md
[Twitter kimlik doğrulamasını yapılandırma]: ../app-service/configure-authentication-provider-twitter.md
[Azure App Service Dağıtım Kılavuzu]: ../app-service/deploy-local-git.md
[Azure uygulama hizmeti izleme]: ../app-service/web-sites-monitor.md
[Azure App Service'te tanılama günlük kaydını etkinleştirme]: ../app-service/troubleshoot-diagnostic-logs.md
[Visual Studio Azure App Service'te ilgili sorunları giderme]: ../app-service/troubleshoot-dotnet-visual-studio.md
[Düğüm sürümü belirtin]: ../nodejs-specify-node-version-azure-apps.md
[Düğüm modüllerini kullanın]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: https://expressjs.com/
[Swagger]: https://swagger.io/

[Azure portal]: https://portal.azure.com/
[OData]: https://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp örneği github'daki]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[github'da Yapılacaklar örneği]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[github'daki örnekler dizini]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Visual Studio için node.js araçları 1.1]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[MSSQL Node.js paketi]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: https://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS ara yazılımı]: https://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
