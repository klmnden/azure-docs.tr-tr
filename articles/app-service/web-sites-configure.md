---
title: Uygulamalar - Azure App Service'ı yapılandırma
description: Azure App Service'te bir uygulama yapılandırma
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: fb8dedac8b795ec127d7b4a14728d73c9397a1dd
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128441"
---
# <a name="configure-apps-in-azure-app-service"></a>Azure App Service'te uygulamaları yapılandırma

Bu konu başlığında, bir web uygulaması, mobil arka uç veya API uygulamasını kullanarak yapılandırmak açıklanmaktadır [Azure Portal].

## <a name="application-settings"></a>Uygulama ayarları
1. İçinde [Azure Portal], uygulama dikey penceresini açın.
2. **Uygulama ayarları**’na tıklayın.

![Uygulama Ayarları][configure01]

**Uygulama ayarları** çeşitli kategoriler altında gruplandırılmış ayarları dikey penceresinde bulunur.

### <a name="general-settings"></a>Genel ayarlar
**Framework sürümlerini**. Uygulamanız herhangi bu çerçeveler kullanıyorsa, bu seçenekleri ayarlayın: 

* **.NET framework**: .NET framework sürümünü ayarlayın. 
* **PHP**: PHP sürümünü ayarlayın veya **OFF** PHP devre dışı bırakmak için. 
* **Java**: Java sürümü seçin veya **OFF** Java devre dışı bırakmak için. Kullanım **Web kapsayıcısını** Tomcat ve Jetty sürümleri arasında seçmek için seçeneği.
* **Python**: Python sürümü seçin veya **OFF** Python devre dışı bırakmak için.

Teknik nedenlerle, uygulamanız için Java'yı etkinleştirme .NET, PHP ve Python seçenekleri devre dışı bırakır.

<a name="platform"></a>
**Platform**. 32 bit veya 64-bit bir ortamda, uygulamanızı çalıştıran olup olmadığını belirler. 64-bit ortam, temel veya standart katman gerektirir. Ücretsiz ve paylaşılan katmanın her zaman bir 32-bit ortamında çalıştırın.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

**Web yuvaları**. Ayarlama **ON** WebSocket Protokolü; etkinleştirmek için örneğin, uygulamanızın kullandığı [ASP.NET SignalR] veya [socket.io](https://socket.io/).

<a name="alwayson"></a>
**Her zaman açık**. Varsayılan olarak, belirli bir süre için boşta olmaları durumunda uygulamalar kaldırılır. Bu kaynak tasarrufu yapmak sistemi sağlar. Temel veya standart modunda etkinleştirebilirsiniz **Always On** uygulama korumak için her zaman yüklenir. Sürekli WebJobs uygulamanız çalışırken veya çalıştırmaları WebJobs tetiklenen bir CRON ifadesi kullanarak, etkinleştirmeniz gereken **Always On**, ya da web işleri güvenilir bir şekilde çalışmayabilir.

**Yönetilen ardışık düzen sürümü**. IIS ayarlar [ardışık düzen modu]. Bu IIS daha eski bir sürümünü gerektiren eski bir uygulama olmadığı sürece tümleşik (varsayılan) bırakın.

**HTTP sürümü**. Kümesine **2.0** desteğini etkinleştirmek için [HTTPS/2](https://wikipedia.org/wiki/HTTP/2) protokolü. 

> [!NOTE]
> Çoğu modern tarayıcılar HTTP/2 protokolüne TLS üzerinden trafiği şifrelenmemiş HTTP/1.1 kullanmaya devam ederken yalnızca destekler. İstemci emin olmak için tarayıcılar HTTP/2 ile uygulamanıza ya da bağlama [bir App Service sertifikası satın alma](web-sites-purchase-ssl-web-site.md) uygulamanızın özel etki alanı veya [bir üçüncü taraf sertifika bağlama](app-service-web-tutorial-custom-ssl.md).

**ARR benzeşimi**. Bir uygulamada, out birden çok sanal makine örneklerine, ARR benzeşimi tanımlama bilgilerini istemci oturumunun ömrü aynı örneğine yönlendirilir garanti ölçeklendirilir. Durum bilgisiz uygulamaların performansını artırmak için bu seçeneği belirlemek **kapalı**.   

**Otomatik Takas**. Otomatik Takas için bir dağıtım yuvası etkinleştirirseniz, bu yuva için bir güncelleştirme gönderdiğinizde App Service otomatik olarak uygulama üretime değiştireceksiniz. Daha fazla bilgi için [hazırlama yuvaları Azure App Service'teki uygulamalar için dağıtma](deploy-staging-slots.md).

### <a name="debugging"></a>Hata ayıklama
**Uzaktan hata ayıklama**. Uzaktan hata ayıklamasını etkinleştirir. Etkin olduğunda, doğrudan uygulamanızın bağlanmak için Visual Studio uzaktan hata ayıklayıcıyı kullanabilirsiniz. Uzaktan hata ayıklama 48 saat boyunca etkin kalır. 

### <a name="app-settings"></a>Uygulama ayarları
Bu bölüm, uygulamanız başlatıldığında yükleyecek ad/değer çiftleri içerir. 

* .NET uygulamaları için .NET yapılandırmanızın bu ayarları eklenmiş `AppSettings` çalışma zamanında mevcut ayarları geçersiz kılar. 
* App Service Linux veya, kapsayıcılar için Web App için sizin adınıza json anahtar yapısı iç içe ister `ApplicationInsights:InstrumentationKey` sahip olması gerekir `ApplicationInsights__InstrumentationKey` anahtar adı. Bu nedenle herhangi bir fark `:` tarafından değiştirilmelidir `__` (yani çift alt çizgi).
* PHP, Python, Java ve Node uygulamaları, bu ayarlar çalışma zamanında ortam değişkenleri olarak erişebilir. Her uygulama ayarı için iki ortam değişkenlerini oluşturulur; bir uygulama ayarı girişi ve başka bir önek APPSETTING_ tarafından belirtilen ada sahip. Her ikisi de aynı değeri içerecek.

Uygulama ayarları depolandığında her zaman şifrelenir (şifrelenmiş bekleyen).

Uygulama ayarlarını kullanarak Key Vault çözülmüş olabilir [Key Vault başvuran](app-service-key-vault-references.md).

### <a name="connection-strings"></a>Bağlantı dizeleri
Bağlı kaynaklar için bağlantı dizelerini. 

.NET uygulamaları için bu bağlantı dizeleri .NET yapılandırmanızın eklenmiş `connectionStrings` çalışma zamanında, girişlerin nereye anahtar eşittir bağlantılı veritabanı adı ayarları. 

PHP, Python, Java ve Node uygulamaları için bu ayarlar bağlantı türü ile önek zamanında ortam değişkenleri olarak kullanılabilir. Ortam değişkeni önekleri aşağıdaki gibidir: 

* SQL Server: `SQLCONNSTR_`
* MySQL: `MYSQLCONNSTR_`
* SQL veritabanı: `SQLAZURECONNSTR_`
* Özel: `CUSTOMCONNSTR_`

Örneğin, bir MySql bağlantı dizesi olarak adlandırılmışsa `connectionstring1`, ortam değişkeni erişilebilecek `MYSQLCONNSTR_connectionString1`.

Bağlantı dizeleri depolandığında her zaman şifrelenir (şifrelenmiş bekleyen).

Bağlantı dizeleri çözümlenebilir Key Vault kullanarak [Key Vault başvuran](app-service-key-vault-references.md).

### <a name="default-documents"></a>Varsayılan belgeler
Varsayılan belge kök URL'si için bir Web sitesi görüntülenir bir web sayfasıdır.  Listedeki ilk eşleşen dosya kullanılır. 

URL rota tabanlı yerine, bu nedenle hiçbir varsayılan belge statik içerik sunan bu durumda var olan modülleri uygulamaları kullanabilir.    

### <a name="handler-mappings"></a>İşleyici eşlemeleri
Belirli dosya uzantıları için istekleri işleyecek bir özel betik işleyicileri eklemek için bu alanı kullanın. 

* **Uzantı**. *.Php veya handler.fcgi gibi ele alınması için dosya uzantısı. 
* **Betik işleyici yolu**. Betik işleyici mutlak yolu. Dosya uzantısı ile eşleşen dosyaları isteklerine betik işleyici tarafından işlenir. Yolu kullanmak `D:\home\site\wwwroot` uygulamanızın kök dizini belirtmek için.
* **Ek bağımsız değişkenler**. Betik işleyici için isteğe bağlı komut satırı bağımsız değişkenleri 

### <a name="virtual-applications-and-directories"></a>Sanal uygulamalar ve dizinler
Sanal uygulamaları ve dizinleri yapılandırmak için her bir sanal dizin ve Web sitesi köküne karşılık gelen fiziksel yolu belirtin. İsteğe bağlı olarak seçebileceğiniz **uygulama** bir sanal dizin bir uygulama olarak işaretlemek için onay kutusu.

## <a name="enabling-diagnostic-logs"></a>Tanılama günlüklerini etkinleştirme
Tanılama günlüklerini etkinleştirmek için:

1. Uygulamanızın dikey penceresinde **tüm ayarlar**.
2. **Tanılama günlükleri**’ne tıklayın. 

Günlük destekleyen bir web uygulamasından tanılama günlükleri yazmak için seçenekleri: 

* **Uygulama günlüğü**. Uygulama günlükleri, dosya sistemine yazma. 12 saat boyunca bağlanabilmesini günlüğe kaydetme. 

**Düzey**. Uygulama günlüğü etkinleştirildiğinde, bu seçenek olacak bilgi miktarını (hata, uyarı, bilgi veya ayrıntılı) kaydedilen belirtir.

**Web sunucusu günlüğü**. Günlükleri W3C Genişletilmiş günlük dosyası biçiminde kaydedilir. 

**Ayrıntılı hata iletileri**. .Htm dosyaları kaydeder ayrıntılı hata iletileri. Dosyaların altında /LogFiles/DetailedErrors kaydedilir. 

**Başarısız istek izlemeyi**. Günlükleri, XML dosyaları için istekleri başarısız oldu. Dosyalar/LogFiles/altında W3SVC kaydedilir*xxx*, burada xxx benzersiz bir tanımlayıcıdır. Bu klasör, XSL dosyası ve bir veya daha fazla XML dosyalarını içerir. Biçimlendirme ve XML dosyalarının içeriğini filtreleme için işlevsellik sağlar çünkü XSL dosyası indirme girişiminde emin olun.

Günlük dosyalarını görüntülemek için FTP kimlik bilgileri, şu şekilde oluşturmanız gerekir:

1. Uygulamanızın dikey penceresinde **tüm ayarlar**.
2. Tıklayın **dağıtım kimlik bilgileri**.
3. Bir kullanıcı adı ve parola girin.
4. **Kaydet**’e tıklayın.

![Dağıtım kimlik bilgilerini ayarlama][configure03]

Tam FTP kullanıcı adı "app\username" olduğu *uygulama* uygulamanızın adıdır. Kullanıcı adı uygulaması dikey penceresinde, altında listelenen **Essentials**.

![FTP dağıtım kimlik bilgileri][configure02]

## <a name="other-configuration-tasks"></a>Diğer yapılandırma görevleri
### <a name="ssl"></a>SSL
Temel veya standart modunda özel etki alanı için SSL sertifikaları karşıya yükleyebilirsiniz. Daha fazla bilgi için [bir uygulama için HTTPS'yi etkinleştir](app-service-web-tutorial-custom-ssl.md). 

Karşıya yüklenen sertifikaların görüntülemek için tıklayın **tüm ayarlar** > **özel etki alanları ve SSL**.

### <a name="domain-names"></a>Etki alanı adları
Uygulamanız için özel etki alanı adlarını ekleyin. Daha fazla bilgi için [Azure App Service'te bir uygulama için bir özel etki alanı adı yapılandırma](app-service-web-tutorial-custom-domain.md).

Etki alanı adlarınızı görüntülemek için tıklayın **tüm ayarlar** > **özel etki alanları ve SSL**.

### <a name="deployments"></a>Dağıtımlar
* Sürekli dağıtım ayarlayın. Bkz: [kullanarak Azure App Service'te uygulamaları dağıtmak için Git](deploy-local-git.md).
* Dağıtım yuvaları. Bkz: [Azure App Service için hazırlama ortamlarını dağıtma].

Dağıtım yuvaları görüntülemek için tıklayın **tüm ayarlar** > **dağıtım yuvalarını**.

### <a name="monitoring"></a>İzleme
Temel veya standart modunda Üçe kadar coğrafi olarak dağıtılmış konumlardan HTTP veya HTTPS uç kullanılabilirliğini test edebilirsiniz. İzleme testi başarısız olursa HTTP yanıt kodunu (4xx veya 5xx) bir hata veya 30 saniyeden uzun yanıt alır. Bir uç nokta izleme testlerinin belirtilen tüm konumlarda başarılı olması için kullanılabilir olarak kabul edilir. 

Daha fazla bilgi için [Nasıl yapılır: Web uç noktası durumunu izleme].

## <a name="next-steps"></a>Sonraki adımlar
* [Azure App Service'te özel etki alanı adını yapılandırma]
* [Azure App Service'te bir uygulama için HTTPS'yi etkinleştirme]
* [Azure App Service'te bir uygulama ölçeklendirme]
* [Azure App Service içindeki temel izleme]
* [ApplicationHost.xdt ile applicationHost.config ayarlarını değiştir](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)

<!-- URL List -->

[ASP.NET SignalR]: https://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Azure App Service'te özel etki alanı adını yapılandırma]: ./app-service-web-tutorial-custom-domain.md
[Azure App Service için hazırlama ortamlarını dağıtma]: ./deploy-staging-slots.md
[Azure App Service'te bir uygulama için HTTPS'yi etkinleştirme]: ./app-service-web-tutorial-custom-ssl.md
[Nasıl yapılır: Web uç noktası durumunu izleme]: https://go.microsoft.com/fwLink/?LinkID=279906
[Azure App Service içindeki temel izleme]: ./web-sites-monitor.md
[Ardışık Düzen modu]: https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Azure App Service'te bir uygulama ölçeklendirme]: ./web-sites-scale.md

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
