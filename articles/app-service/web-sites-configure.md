---
title: "Azure Uygulama Hizmeti’nde Web uygulamalarını yapılandırma"
description: "Azure App Services web uygulamasını yapılandırma"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: cephalin
ms.openlocfilehash: 9ec501d0a4e1c6165b83b5b590b87b0baa284423
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="configure-web-apps-in-azure-app-service"></a>Azure Uygulama Hizmeti’nde Web uygulamalarını yapılandırma

Bu konuda, bir web uygulamasını kullanarak yapılandırmak açıklanmaktadır [Azure Portal].

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a>Uygulama ayarları
1. İçinde [Azure Portal], web uygulaması dikey penceresini açın.
3. **Uygulama ayarları**’na tıklayın.

![Uygulama ayarları][configure01]

**Uygulama ayarları** dikey birkaç kategoriler altında gruplanmış ayarlara sahip.

### <a name="general-settings"></a>Genel ayarlar
**Framework sürümlerini**. Uygulamanız herhangi bu çerçeveleri kullanıyorsa bu seçenekleri ayarlayın: 

* **.NET framework**: .NET framework sürümünü ayarlayın. 
* **PHP**: PHP sürümünü ayarlayın veya **OFF** PHP devre dışı bırakmak için. 
* **Java**: Java sürümü seçin veya **OFF** Java devre dışı bırakmak için. Kullanım **Web kapsayıcısına** Tomcat ve Jetty sürümleri arasında seçmek için seçeneği.
* **Python**: Python sürümü seçin veya **OFF** Python devre dışı bırakmak için.

Teknik nedenlerle .NET, PHP ve Python seçenekleri uygulamanız için Java'yı etkinleştirme devre dışı bırakır.

<a name="platform"></a>
**Platform**. Web uygulamanızı 32 bit veya 64-bit bir ortamda çalışır olup olmadığını belirler. 64 bit ortamı temel veya standart modu gerektirir. Ücretsiz ve paylaşılan modları her zaman bir 32 bit ortamda çalıştırılabilir.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

**Web yuvaları**. Ayarlama **ON** WebSocket Protokolü; etkinleştirmek için web uygulamanızın kullanıyorsa, örneğin, [ASP.NET SignalR] veya [Socket.IO](https://socket.io/).

<a name="alwayson"></a>
**Always On**. Varsayılan olarak, belirli bir süre için boşta olmaları durumunda web uygulamaları kaldırılır. Bu kaynakların tasarrufu sistem sağlar. Temel veya standart modunda etkinleştirebilirsiniz **her zaman açık** uygulama korumak için her zaman yüklenir. Sürekli Webjob'lar uygulamanızın çalıştırdığı veya çalıştığı Web işleri tetiklenen CRON ifade kullanılarak, etkinleştirmeniz gerekir **her zaman açık**, ya da web işleri güvenilir bir şekilde çalışmayabilir.

**Yönetilen ardışık düzen sürüm**. IIS ayarlar [ardışık düzen modu]. IIS daha eski bir sürümü gerektiren eski bir uygulama yoksa bu kümesi tümleşik (varsayılan) bırakın.

**Otomatik Takas**. Otomatik Takas için bir dağıtım yuvası etkinleştirirseniz, bu yuva için bir güncelleştirme bastığınızda uygulama hizmeti otomatik olarak web uygulaması üretime değiştireceksiniz. Daha fazla bilgi için bkz: [hazırlama yuvası için Azure App Service'te web uygulamalarını dağıtma](web-sites-staged-publishing.md).

### <a name="debugging"></a>Hata ayıklama
**Uzaktan hata ayıklama**. Uzaktan hata ayıklamayı etkinleştirir. Etkinleştirildiğinde, web uygulamanızı doğrudan bağlanmak için Visual Studio uzaktan hata ayıklayıcı kullanabilirsiniz. Uzaktan hata ayıklama 48 saat boyunca etkin kalır. 

### <a name="app-settings"></a>Uygulama ayarları
Bu bölümde, web ad/değer çiftleri içeren uygulama başlangıç yükler. 

* .NET yapılandırmanızı eklenen .NET uygulamaları için bu ayarları `AppSettings` çalışma zamanında mevcut ayarları geçersiz kılar. 
* PHP, Python, Java ve düğüm uygulamalar, bu ayarlar çalışma zamanında ortam değişkenleri olarak erişebilir. Her uygulama ayarı için iki ortam değişkenin oluşturulur; bir uygulama ayarı giriş ve APPSETTING_ önekine sahip başka bir tarafından belirtilen ada sahip. Her ikisi de aynı değeri içerir.

### <a name="connection-strings"></a>Bağlantı dizeleri
Bağlantılı kaynaklar için bağlantı dizelerini. 

.NET uygulamaları için bu bağlantı dizeleri .NET yapılandırmanızı eklenmiş `connectionStrings` çalışma zamanında girdilerinin burada anahtar eşittir bağlantılı veritabanı adı geçersiz kılma ayarları. 

PHP, Python, Java ve düğüm uygulamalar için bu ayarlar bağlantı türü ile önek zamanında ortam değişkenleri olarak kullanılabilir. Ortam değişkeni önekleri aşağıdaki gibidir: 

* SQL sunucusu:`SQLCONNSTR_`
* MySQL:`MYSQLCONNSTR_`
* SQL veritabanı:`SQLAZURECONNSTR_`
* Özel:`CUSTOMCONNSTR_`

Örneğin, bir MySql bağlantı dizesi olarak adlandırılmışsa `connectionstring1`, ortam değişkeni erişilebilecek `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Varsayılan belgeler
Varsayılan belge bir Web sitesinin kök URL'sindeki görüntülenen bir web sayfasıdır.  Listedeki ilk eşleşen dosya kullanılır. 

Web uygulamaları rota URL'sine bağlı yerine, bu nedenle bir varsayılan belge statik içerik vardır; bu durumda hizmet olan modülleri kullanabilir.    

### <a name="handler-mappings"></a>İşleyici eşlemeleri
Bu alan, belirli dosya uzantıları için istekleri işlemek üzere özel betik işleyicileri eklemek için kullanın. 

* **Uzantı**. , *.Php veya handler.fcgi gibi işlenecek dosya uzantısı. 
* **Betik işleyici yolu**. Betik işleyici mutlak yolu. Dosya uzantısı ile eşleşen dosyaları isteklerine betik işleyici tarafından işlenir. Yolun kullanmak `D:\home\site\wwwroot` , uygulamanızın kök dizinine başvurmak için.
* **Ek bağımsız değişkenler**. Betik işleyici için isteğe bağlı komut satırı bağımsız değişkenleri 

### <a name="virtual-applications-and-directories"></a>Sanal uygulamalar ve dizinler
Sanal uygulamaları ve dizinleri yapılandırmak için her sanal dizini ve Web sitesi köküne ilişkin karşılık gelen fiziksel yolu belirtin. İsteğe bağlı olarak seçebileceğiniz **uygulama** bir sanal dizin bir uygulama olarak işaretlemek için onay kutusunu.

## <a name="enabling-diagnostic-logs"></a>Tanılama günlüklerini etkinleştirme
Tanılama günlüklerini etkinleştirmek için:

1. Web uygulamanız için dikey penceresinde tıklayın **tüm ayarları**.
2. Tıklatın **tanılama günlükleri**. 

Bir web uygulamasından tanılama günlüklerini yazma seçenekleri, günlük kaydı destekler: 

* **Uygulama günlüğü**. Uygulama günlüklerini dosya sistemine yazar. Lasts 12 saatlik bir dönem için günlüğe kaydetme. 

**Düzey**. Uygulama günlüğü etkinleştirildiğinde, bu seçenek olacaktır bilgi miktarını (hata, uyarı, bilgi veya ayrıntılı) kaydedilen belirtir.

**Web sunucusu günlüğü**. Günlükleri, W3C Genişletilmiş günlük dosyası biçiminde kaydedilir. 

**Ayrıntılı hata iletileri**. .Htm dosyaları kaydeder ayrıntılı hata iletileri. Dosyalar /LogFiles/DetailedErrors altında kaydedilir. 

**Başarısız istek izleme**. Günlükleri XML dosyaları için istekleri başarısız oldu. Dosyalar/LogFiles/altında W3SVC kaydedilir*xxx*, burada xxx benzersiz bir tanımlayıcıdır. Bu klasör bir XSL dosyası ve bir veya daha fazla XML dosyalarını içerir. Biçimlendirme ve XML dosyalarının içeriğini filtreleme için işlevsellik sağlar çünkü XSL dosyasını karşıdan yüklemek emin olun.

Günlük dosyalarını görüntülemek için FTP kimlik bilgileri, şu şekilde oluşturmanız gerekir:

1. Web uygulamanız için dikey penceresinde tıklayın **tüm ayarları**.
2. Tıklatın **dağıtım kimlik bilgileri**.
3. Bir kullanıcı adı ve parola girin.
4. **Kaydet**’e tıklayın.

![Dağıtım kimlik bilgilerini ayarlama][configure03]

Tam FTP kullanıcı adı "app\username" olduğu *uygulama* web uygulamanızın adıdır. Kullanıcı adı web uygulaması dikey penceresinde altında listelenen **Essentials**.

![FTP dağıtımı kimlik bilgileri][configure02]

## <a name="other-configuration-tasks"></a>Diğer yapılandırma görevleri
### <a name="ssl"></a>SSL
Temel veya standart modunda özel bir etki alanı için SSL sertifikalarını karşıya yükleyebilirsiniz. [Web uygulaması için HTTPS'yi etkinleştir] daha fazla bilgi için bkz. 

Karşıya yüklenen sertifikalarınızı görüntülemek için **tüm ayarları** > **özel etki alanları ve SSL**.

### <a name="domain-names"></a>Etki alanı adları
Web uygulamanız için özel etki alanı adlarını ekleyin. Daha fazla bilgi için bkz: [yapılandırma Azure App Service'te bir web uygulaması için bir özel etki alanı adı].

Etki alanı adlarını görüntülemek için **tüm ayarları** > **özel etki alanları ve SSL**.

### <a name="deployments"></a>Dağıtımlar
* Sürekli dağıtım ayarlayın. Bkz: [kullanarak Azure App Service'te Web uygulamalarını dağıtmak için Git](app-service-deploy-local-git.md).
* Dağıtım yuvaları. Bkz: [Azure App Service'te Web uygulamalarını için hazırlama ortamlarını dağıtmak].

Dağıtım yuvaları görüntülemek için **tüm ayarları** > **dağıtım yuvası**.

### <a name="monitoring"></a>İzleme
Temel veya standart modunda en çok üç coğrafi olarak dağıtılmış konumlardan HTTP veya HTTPS uç kullanılabilirliğini test edebilirsiniz. HTTP yanıt kodu bir hata (4xx veya 5xx) ya da 30 saniyeden fazla yanıt alır, izleme testi başarısız olur. Bir uç nokta izleme testlerinin belirtilen tüm konumlarda başarılı olması kullanılabilir olarak kabul edilir. 

Daha fazla bilgi için bkz: [nasıl yapılır: izleme web uç noktası durumu].

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin] sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Azure App Service'te özel etki alanı adını yapılandırma]
* [Azure uygulama hizmetinde bir uygulama için HTTPS'yi etkinleştir]
* [Bir web uygulamasını Azure App Service'te ölçeklendirme]
* [Azure App Service'deki Web uygulamaları için izleme temelleri]

<!-- URL List -->

[ASP.NET SignalR]: http://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Azure App Service'te özel etki alanı adını yapılandırma]: ./app-service-web-tutorial-custom-domain.md
[Azure App Service'te Web uygulamalarını için hazırlama ortamlarını dağıtmak]: ./web-sites-staged-publishing.md
[Azure uygulama hizmetinde bir uygulama için HTTPS'yi etkinleştir]: ./app-service-web-tutorial-custom-ssl.md
[nasıl yapılır: izleme web uç noktası durumu]: http://go.microsoft.com/fwLink/?LinkID=279906
[Azure App Service'deki Web uygulamaları için izleme temelleri]: ./web-sites-monitor.md
[ardışık düzen modu]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Bir web uygulamasını Azure App Service'te ölçeklendirme]: ./web-sites-scale.md
[App Service’i Deneyin]: https://azure.microsoft.com/try/app-service/

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
