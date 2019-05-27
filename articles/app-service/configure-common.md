---
title: Portal - Azure App Service'te uygulamaları yapılandırma
description: Azure portalında bir App Service uygulaması için genel ayarlarını yapılandırmayı öğrenin.
keywords: Azure app service, web uygulaması, uygulama ayarları, ortam değişkenleri
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
ms.openlocfilehash: bcc970375120f76e4ec8a90f487d251296f92dba
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957918"
---
# <a name="configure-an-app-service-app-in-the-azure-portal"></a>Azure portalında bir App Service uygulaması yapılandırma

Bu konu başlığında, web uygulamaları, mobil arka uç veya API uygulamasını kullanarak genel ayarlarını yapılandırmak açıklanmaktadır [Azure Portal].

## <a name="configure-app-settings"></a>Uygulama ayarlarını yapılandırma

App Service uygulama ayarları gibi ortam değişkenlerini kullanın. İçinde [Azure Portal], uygulamanızın Yönetim sayfasına gidin. Uygulamanın sol menüde **yapılandırma** > **uygulama ayarları**.

![Uygulama Ayarları](./media/configure-common/open-ui.png)

ASP.NET ve ASP.NET Core geliştiricileri için App Service uygulama ayarlarında ayarı bunları ayarlamak gibidir `<appSettings>` içinde *Web.config*, ancak dışındaki geçersiz kılma değerleri App Service'te *Web.config*. Geliştirme Ayarları (örneğin yerel MySQL parola) tutabilirsiniz *Web.config*, ancak üretim gizli (örneğin Azure MySQL veritabanı parolasını) App Service'te güvenli. Yerel olarak hata ayıklama ve üretim dizilerinizin Azure'a dağıtıldığında kullanır, geliştirme ayarlarınızı aynı kodu kullanır.

Diğer dil yığınları, benzer şekilde, çalışma zamanında uygulama ayarları ortam değişkenleri olarak alın. Dil yığını belirli adımlar için bkz:

- [ASP.NET Core](containers/configure-language-dotnetcore.md#access-environment-variables)
- [Node.js](containers/configure-language-nodejs.md#access-environment-variables)
- [PHP](containers/configure-language-php.md#access-environment-variables)
- [Python](containers/how-to-configure-python.md#access-environment-variables)
- [Java](containers/configure-language-java.md#data-sources)
- [Ruby](containers/configure-language-ruby.md#access-environment-variables)
- [Özel kapsayıcılar](containers/configure-custom-container.md#configure-environment-variables)

Uygulama ayarları depolandığında her zaman şifrelenir (şifrelenmiş bekleyen).

> [!NOTE]
> Uygulama ayarları da çözümlenebilir gelen [Key Vault](/azure/key-vault/) kullanarak [Key Vault başvuran](app-service-key-vault-references.md).

### <a name="show-hidden-values"></a>Gizli değerleri göster

Varsayılan olarak, uygulama ayarları için değerleri portalında güvenlik için gizlidir. Gizli bir uygulama ayarının değerini görmek için tıklayın **değer** söz konusu ayarın alanı. Tüm uygulama ayarlarının değerleri görmek için tıklayın **Göster değeri** düğmesi.

### <a name="add-or-edit"></a>Ekle veya Düzenle

Yeni bir uygulama ayarı eklemek için tıklatın **yeni uygulama ayarı**. İletişim kutusunda, şunları yapabilirsiniz [ayarı geçerli yuvada](deploy-staging-slots.md#which-settings-are-swapped).

Bir ayarı düzenlemek için tıklayın **Düzenle** işlecin sağ tarafındaki düğmesi.

İşiniz bittiğinde tıklayın **güncelleştirme**. Tıklamayı unutmayın **Kaydet** geri **yapılandırma** sayfası.

> [!NOTE]
> Varsayılan Linux kapsayıcısı veya özel bir Linux kapsayıcısı, tüm iç içe geçmiş JSON anahtar yapısında uygulama ayarı adı ister `ApplicationInsights:InstrumentationKey` App Service ' yapılandırılması gerekir `ApplicationInsights__InstrumentationKey` anahtar adı. Herhangi diğer bir deyişle, `:` tarafından değiştirilmelidir `__` (çift alt çizgi).
>

### <a name="edit-in-bulk"></a>Toplu düzenleme

Ekleme ya da toplu uygulama ayarlarını düzenlemek için tıklayın **düzenleme Gelişmiş** düğmesi. İşiniz bittiğinde tıklayın **güncelleştirme**. Tıklamayı unutmayın **Kaydet** geri **yapılandırma** sayfası.

Uygulama ayarları, aşağıdaki JSON biçimlendirmeye sahip:

```json
[
  {
    "name": "<key-1>",
    "value": "<value-1>",
    "slotSetting": false
  },
  {
    "name": "<key-2>",
    "value": "<value-2>",
    "slotSetting": false
  },
  ...
]
```

## <a name="configure-connection-strings"></a>Bağlantı dizelerini yapılandırma

İçinde [Azure Portal], uygulamanın Yönetim sayfasına gidin. Uygulamanın sol menüde **yapılandırma** > **uygulama ayarları**.

![Uygulama Ayarları](./media/configure-common/open-ui.png)

ASP.NET ve ASP.NET Core geliştiricileri için App Service ayarı bağlantı dizelerini ayarlama bölümünde gibidir `<connectionStrings>` içinde *Web.config*, ancak dışındaki App Service'te ayarlanan değerlerle geçersiz *Web.config*. Geliştirme Ayarları (örn. veritabanı dosyası) tutabilirsiniz *Web.config* ve üretim gizli (örneğin SQL veritabanı kimlik bilgileri) App Service'te güvenli. Yerel olarak hata ayıklama ve üretim dizilerinizin Azure'a dağıtıldığında kullanır, geliştirme ayarlarınızı aynı kodu kullanır.

Diğer dil yığınları için kullanmak en iyisidir [uygulama ayarları](#configure-app-settings) değerlere erişmek için bağlantı dizelerini değişken anahtarlarında özel biçimlendirme gerektirdiğinden'ın bunun yerine, sipariş. İşte bir özel durum, ancak: kendi bağlantı dizelerini uygulamanızın yapılandırma, belirli bir Azure veritabanı türlerini uygulamanızla birlikte yedeklenir. Daha fazla bilgi için [ne yedeklenir](manage-backup.md#what-gets-backed-up). Ardından bu otomatik yedeklemeyi gerekmiyorsa, uygulama ayarlarını kullanın.

Çalışma zamanında, bağlantı dizeleri ile aşağıdaki bağlantı türlerini öneki, ortam değişkenleri olarak kullanılabilir:

* SQL Server: `SQLCONNSTR_`
* MySQL: `MYSQLCONNSTR_`
* SQL veritabanı: `SQLAZURECONNSTR_`
* Özel: `CUSTOMCONNSTR_`

Örneğin, bir MySql bağlantı dizesi adlı *connectionstring1* ortam değişkeni erişilebilen `MYSQLCONNSTR_connectionString1`. Dil yığını belirli adımlar için bkz:

- [ASP.NET Core](containers/configure-language-dotnetcore.md#access-environment-variables)
- [Node.js](containers/configure-language-nodejs.md#access-environment-variables)
- [PHP](containers/configure-language-php.md#access-environment-variables)
- [Python](containers/how-to-configure-python.md#access-environment-variables)
- [Java](containers/configure-language-java.md#data-sources)
- [Ruby](containers/configure-language-ruby.md#access-environment-variables)
- [Özel kapsayıcılar](containers/configure-custom-container.md#configure-environment-variables)

Bağlantı dizeleri depolandığında her zaman şifrelenir (şifrelenmiş bekleyen).

> [!NOTE]
> Bağlantı dizelerini de gelen çözülmüş olabilir [Key Vault](/azure/key-vault/) kullanarak [Key Vault başvuran](app-service-key-vault-references.md).

### <a name="show-hidden-values"></a>Gizli değerleri göster

Varsayılan olarak, bağlantı dizeleri için değerleri portalında güvenlik için gizlidir. Gizli bir bağlantı dizesi değerini görmek için tıklamanız **değer** bu dizenin alan. Tüm bağlantı dizeleri değerleri görmek için tıklayın **Göster değeri** düğmesi.

### <a name="add-or-edit"></a>Ekle veya Düzenle

Yeni bir bağlantı dizesi eklemek için tıklatın **yeni bağlantı dizesi**. İletişim kutusunda, şunları yapabilirsiniz [bağlantı dizesini geçerli yuvada](deploy-staging-slots.md#which-settings-are-swapped).

Bir ayarı düzenlemek için tıklayın **Düzenle** işlecin sağ tarafındaki düğmesi.

İşiniz bittiğinde tıklayın **güncelleştirme**. Tıklamayı unutmayın **Kaydet** geri **yapılandırma** sayfası.

### <a name="edit-in-bulk"></a>Toplu düzenleme

Eklemek veya toplu bağlantı dizelerini düzenlemek için tıklayın **düzenleme Gelişmiş** düğmesi. İşiniz bittiğinde tıklayın **güncelleştirme**. Tıklamayı unutmayın **Kaydet** geri **yapılandırma** sayfası.

Aşağıdaki JSON biçimlendirme bağlantı dizelerine sahipsiniz:

```json
[
  {
    "name": "name-1",
    "value": "conn-string-1",
    "type": "SQLServer",
    "slotSetting": false
  },
  {
    "name": "name-2",
    "value": "conn-string-2",
    "type": "PostgreSQL",
    "slotSetting": false
  },
  ...
]
```

<a name="platform"></a>
<a name="alwayson"></a>

## <a name="configure-general-settings"></a>Genel ayarları yapılandırma

İçinde [Azure Portal], uygulamanın Yönetim sayfasına gidin. Uygulamanın sol menüde **yapılandırma** > **uygulama ayarları**.

![Genel ayarlar](./media/configure-common/open-general.png)

Burada, uygulama için bazı ortak ayarları yapılandırabilirsiniz. Bazı ayarlar gerektirir [daha yüksek bir fiyatlandırma katmanları kadar ölçek](web-sites-scale.md).

- **Yığın ayarları**: Dil ve SDK sürümlerine dahil olmak üzere uygulamayı çalıştırmak için yazılım yığını. Linux uygulamaları ve özel kapsayıcı uygulamaları için isteğe bağlı bir başlangıç komutu ya da dosya ayarlayabilirsiniz.
- **Platform ayarları**: Bir barındırma platformu için ayarları yapılandırmanıza olanak sağlayan dahil olmak üzere:
    - **Bit genişliği**: 32 bit veya 64-bit.
    - **WebSocket Protokolü**: İçin [ASP.NET SignalR] veya [socket.io](https://socket.io/), örneğin.
    - **Her zaman açık**: Hiçbir trafik olduğunda bile uygulama tutun. Sürekli WebJobs için veya bir CRON ifadesi kullanarak tetiklenen WebJobs için etkinleştirmeniz gerekir.
    - **Yönetilen ardışık düzen sürümü**: IIS [ardışık düzen modu]. Ayarlayın **Klasik** IIS daha eski bir sürümünü gerektiren eski bir uygulama varsa.
    - **HTTP sürümü**: Kümesine **2.0** desteğini etkinleştirmek için [HTTPS/2](https://wikipedia.org/wiki/HTTP/2) protokolü.
    > [!NOTE]
    > Çoğu modern tarayıcılar HTTP/2 protokolüne TLS üzerinden trafiği şifrelenmemiş HTTP/1.1 kullanmaya devam ederken yalnızca destekler. İstemci emin olmak için tarayıcılar HTTP/2 ile uygulamanıza ya da bağlama [bir App Service sertifikası satın alma](web-sites-purchase-ssl-web-site.md) uygulamanın özel etki alanı veya [bir üçüncü taraf sertifika bağlama](app-service-web-tutorial-custom-ssl.md).
    - **ARR benzeşimi**: Çok örnekli bir dağıtımda, istemci oturumunun ömrü aynı örneğe yönlendirildiğinden emin olun. Bu seçeneği ayarlamak **kapalı** durum bilgisiz uygulamalar için.
- **Hata ayıklama**: İçin uzaktan hata ayıklamayı etkinleştirme [ASP.NET](troubleshoot-dotnet-visual-studio.md#remotedebug), [ASP.NET Core](/visualstudio/debugger/remote-debugging-azure), veya [Node.js](containers/configure-language-nodejs.md#debug-remotely) uygulamalar. Bu seçenek 48 saat sonra otomatik olarak devre dışı bırakır.
- **Gelen istemci sertifikaları**: istemci sertifikaları iste [karşılıklı kimlik doğrulaması](app-service-web-configure-tls-mutual-auth.md).

## <a name="configure-default-documents"></a>Varsayılan belgeleri yapılandırma

Bu ayar yalnızca Windows uygulamaları için desteklenir.

İçinde [Azure Portal], uygulamanın Yönetim sayfasına gidin. Uygulamanın sol menüde **yapılandırma** > **varsayılan belgeler**.

![Genel ayarlar](./media/configure-common/open-documents.png)

Varsayılan belge kök URL'si için bir Web sitesi görüntülenir bir web sayfasıdır. Listedeki ilk eşleşen dosya kullanılır. Yeni bir varsayılan belge eklemek için tıklatın **yeni belge**. Tıklamayı unutmayın **Kaydet**.

Uygulama yönlendirmek yerine statik içeriğe hizmet URL'sini temel alarak modüllerini kullanıyorsa, varsayılan belgeler için gerek yoktur.

## <a name="configure-path-mappings"></a>Yolu eşlemelerini yapılandırma

İçinde [Azure Portal], uygulamanın Yönetim sayfasına gidin. Uygulamanın sol menüde **yapılandırma** > **yolu eşlemeleri**.

![Genel ayarlar](./media/configure-common/open-path.png)

**Yolu eşlemeleri** sayfasında işletim sistemi türüne göre farklı şeyler gösterir.

### <a name="windows-apps-uncontainerized"></a>Windows uygulamaları (uncontainerized)

Windows uygulamaları için IIS işleyici eşlemeleri ve sanal uygulamalar ve dizinler özelleştirebilirsiniz.

İşleyici eşlemeleri belirli dosya uzantıları için istekleri işleyecek bir özel betik işleyicileri eklemenize olanak sağlar. Özel bir işleyici eklemek için tıklatın **yeni işleyici**. İşleyicisi aşağıdaki gibi yapılandırın:

- **Uzantı**. İstediğiniz gibi işlemek için dosya uzantısı  *\*.php* veya *handler.fcgi*.
- **Betik işleyici**. Betik işleyici, mutlak yolu. Dosya uzantısı ile eşleşen dosyaları için istekleri betik işleyici tarafından işlenir. Yolu kullanmak `D:\home\site\wwwroot` uygulamanızın kök dizini belirtmek için.
- **Bağımsız değişkenler**. Betik işleyici için isteğe bağlı komut satırı değişkenleri.

Her uygulamanın varsayılan kök yolu vardır (`/`) eşlenmiş `D:\home\site\wwwroot`, varsayılan olarak, kodunuzun dağıtıldığı. Uygulama kök farklı bir klasörde ise veya deponuzda birden fazla uygulama varsa, düzenleme yapabilmeniz veya sanal uygulamalar ve dizinler buraya ekleyin. Tıklayın **yeni bir sanal uygulama veya dizin**.

Sanal uygulamaları ve dizinleri yapılandırmak için her bir sanal dizin ve Web sitesi köküne karşılık gelen fiziksel yolu belirtin (`D:\home`). İsteğe bağlı olarak seçebileceğiniz **uygulama** bir sanal dizin bir uygulama olarak işaretlemek için onay kutusu.

### <a name="containerized-apps"></a>Kapsayıcı uygulamaları

Yapabilecekleriniz [kapsayıcılı uygulamanız için özel depolama ekleme](containers/how-to-serve-content-from-azure-storage.md). Tüm Linux uygulamaları ve App Service üzerinde çalışan Windows ve Linux özel kapsayıcılar kapsayıcılı uygulamaları içerir. Tıklayın **yeni Azure depolama bağlama** ve özel depolama alanınızı aşağıdaki gibi yapılandırın:

- **Ad**: Görünen adı.
- **Yapılandırma seçenekleri**: **Temel** veya **Gelişmiş**.
- **Depolama hesapları**: İstediğiniz depolama hesabı ile kapsayıcı.
- **Depolama türü**: **Azure BLOB'ları** veya **Azure dosyaları**.
  > [!NOTE]
  > Windows kapsayıcı uygulamaları, yalnızca Azure dosyaları destekler.
- **Depolama kapsayıcısı**: Temel yapılandırma için istediğiniz kapsayıcı.
- **Paylaşım adı**: Gelişmiş yapılandırma için dosya paylaşımı adı.
- **Erişim anahtarı**: Gelişmiş yapılandırma için erişim anahtarı.
- **Bağlama yolunu**: Özel depolama bağlamak kapsayıcınızdaki mutlak yolu.

Daha fazla bilgi için [Linux üzerinde App Service'te Azure Depolama'dan içerik hizmet](containers/how-to-serve-content-from-azure-storage.md).

## <a name="configure-language-stack-settings"></a>Dil yığını ayarlarını yapılandırma

Linux uygulamaları için bkz:

- [ASP.NET Core](containers/configure-language-dotnetcore.md)
- [Node.js](containers/configure-language-nodejs.md)
- [PHP](containers/configure-language-php.md)
- [Python](containers/how-to-configure-python.md)
- [Java](containers/configure-language-java.md)
- [Ruby](containers/configure-language-ruby.md)

## <a name="configure-custom-containers"></a>Özel kapsayıcılar'ı yapılandırma

Bkz: [Azure App Service için özel bir Linux kapsayıcısını yapılandırın](containers/configure-custom-container.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure App Service'te özel etki alanı adını yapılandırma]
- [Azure App Service ortamlarında hazırlık ayarlama]
- [Azure App Service'te bir uygulama için HTTPS'yi etkinleştirme]
- [Tanılama günlüklerini etkinleştirme](troubleshoot-diagnostic-logs.md)
- [Azure App Service'te bir uygulama ölçeklendirme]
- [Azure App Service içindeki temel izleme]
- [ApplicationHost.xdt ile applicationHost.config ayarlarını değiştir](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)

<!-- URL List -->

[ASP.NET SignalR]: https://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Azure App Service'te özel etki alanı adını yapılandırma]: ./app-service-web-tutorial-custom-domain.md
[Azure App Service ortamlarında hazırlık ayarlama]: ./deploy-staging-slots.md
[Azure App Service'te bir uygulama için HTTPS'yi etkinleştirme]: ./app-service-web-tutorial-custom-ssl.md
[How to: Monitor web endpoint status]: https://go.microsoft.com/fwLink/?LinkID=279906
[Azure App Service içindeki temel izleme]: ./web-sites-monitor.md
[Ardışık Düzen modu]: https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Azure App Service'te bir uygulama ölçeklendirme]: ./web-sites-scale.md
