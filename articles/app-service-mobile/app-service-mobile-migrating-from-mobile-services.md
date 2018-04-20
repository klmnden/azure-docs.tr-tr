---
title: Bir uygulama hizmeti mobil uygulamaya Mobile Services geçirme
description: Bir mobil uygulama hizmeti Mobile Services uygulamanıza kolayca geçirmeyi öğrenme
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 07507ea2-690f-4f79-8776-3375e2adeb9e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: crdun
ms.openlocfilehash: e94f08b9b9dad20c6f47367c47eb49aea59f4bd8
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="article-top"></a>Mevcut Azure mobil hizmetiniz Azure App Service'e geçirme
İle [Azure uygulama hizmeti genel kullanılabilirliğini], Azure Mobile Services siteleri kolayca geçiş yapabilir yerinde Azure App Service'in tüm özelliklerden yararlanmak için.  Bu belgede, sitenizin Azure App Service için Azure Mobile Services geçirilirken beklenmesi gerekenler açıklanmaktadır.

## <a name="what-does-migration-do"></a>Geçiş sitenize ne yapar
Azure Mobile hizmetinizi geçişini kapatır mobil hizmetiniz bir [Azure App Service] kodunu etkilemeden uygulama.  Bildirim hub'ları, SQL veri bağlantısı, kimlik doğrulama ayarları, zamanlanmış işler ve etki alanı adı değişmeden kalır.  Azure mobil hizmeti kullanan mobil istemcilerin normal şekilde çalışmaya devam eder.  Azure App Service'e aktarılan sonra geçiş hizmetinizi yeniden başlatır.

[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Sitenizi neden geçirmelisiniz
Microsoft Azure uygulama hizmeti özelliklerden yararlanmak için Azure mobil hizmetiniz geçirmek öneren:

* [WebJobs] dahil yeni ana bilgisayara özellikleri ve [özel etki alanı adlarını].
* İzleme ve sorunlarını giderme [Application Insights].
* Yerleşik DevOps araçları dahil olmak üzere [yuvaları hazırlama], geri alma ve üretim sınama.
* [Otomatik ölçek], Yük Dengeleme, ve [performans izleme].

Azure uygulama hizmeti yararları hakkında daha fazla bilgi için bkz: [Mobile Services vs. Uygulama hizmeti] konu.

## <a name="before-you-begin"></a>Başlamadan önce
Sitenizde herhangi bir ana iş başlamadan önce SQL veritabanı ve mobil hizmeti betikleri yedeklemeniz gerekir.

## <a name="migrating-site"></a>Sitelerinizin geçirme
Geçiş işlemi, tek bir Azure bölge içindeki tüm siteleri geçirir.

Sitenizi geçirmek için:

1. Oturum [Klasik Azure portalı].
2. Mobil hizmet, geçirmek istediğiniz bölgeyi seçin.
3. Tıklatın **App Service'e geçirme** düğmesi.

   ![Geçiş düğmesi][0]
4. App Service iletişim geçiş okuyun.
5. Mobil hizmetinizi adını sağlanan kutuya girin.  Örneğin, etki alanı adınızı contoso.azure mobile.net ise, enter *contoso* sağlanan kutuya.
6. Onay işareti düğmesine tıklayın.

Etkinlik izleyicisinin geçişte durumunu izleyin. Siteniz olarak listelenen *geçirme* Azure Klasik Portalı'nda.

  ![Geçiş etkinliğini izleme][1]

Her geçiş herhangi bir yere 3 veya Geçirilmekte olan mobil hizmeti başına 15 dakika sürebilir.  Sitenizi geçiş sırasında kullanılabilir olarak kalır.
Sitenizin, geçiş işleminin sonunda yeniden başlatılır.  Site birkaç saniye sürebilir yeniden başlatma işlemi sırasında kullanılamaz.

## <a name="finalizing-migration"></a>Geçiş Tamamlanıyor
Mobil istemci geçiş işleminin sonunda sitenizden sınamayı planlayın.  Mobil istemci için bir değişiklik yapılmadan tüm ortak istemci eylemleri gerçekleştirebilir emin olun.  

### <a name="update-app-service-tier"></a>Uygun bir uygulama hizmeti fiyatlandırma katmanı seçin
Azure App Service'e geçirdikten sonra fiyatlandırma daha fazla esneklik bulunur.

1. [Azure portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** geçirilen mobil hizmetiniz adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Tıklatın **App Service planı** Ayarlar menüsünden.
5. Tıklatın **fiyatlandırma katmanı** döşeme.
6. Gereksinimlerinize uygun kutucuğa tıklayın ve ardından **seçin**.  I gerekebilir **tüm görüntüle** mevcut fiyatlandırma katmanlarına görmek için.

Bir başlangıç noktası olarak aşağıdaki katmanlarda öneririz:

| Mobil hizmet fiyatlandırma katmanı | Uygulama hizmeti fiyatlandırma katmanı |
|:--- |:--- |
| Ücretsiz |F1 Ücretsiz |
| Temel |B1 Basic |
| Standart |S1 Standart |

Fiyatlandırma katmanı, uygulamanız için sağ seçerken büyük oranda esneklik yoktur.  Başvurmak [App Service fiyatlandırması] yeni App Service fiyatlandırması hakkında tam bilgi.

> [!TIP]
> App Service standart katmanı kullanmak isteyebilirsiniz birçok özellik erişimi de dahil olmak üzere içeren [yuvaları hazırlama], otomatik yedeklemeler ve otomatik ölçeklendirme.  Varken yeni özelliklerini kullanıma bakın!
>
>

### <a name="review-migration-scheduler-jobs"></a>Geçirilen zamanlayıcı işlerinin gözden geçirin
Zamanlayıcı işlerinin geçişten sonra yaklaşık 30 dakika kadar görünür olmaz.  Zamanlanan işler arka planda çalışmaya devam eder.
Zamanlanan işleriniz yeniden görünür sonra görüntülemek için:

1. [Azure portal]’da oturum açın.
2. Seçin **Gözat >**, girin **zamanlama** içinde *filtre* kutusuna ve ardından **Zamanlayıcı koleksiyonları**.

Ücretsiz zamanlayıcı işleri kullanılabilir geçiş sonrası sınırlı sayıda vardır.  Kullanımınızı gözden geçirin ve [Azure Zamanlayıcı planları].

### <a name="configure-cors"></a>Gerekirse CORS'yi yapılandırın
Çıkış noktaları arası kaynak paylaşımı, farklı bir etki alanındaki bir Web API erişmek bir Web sitesi izin vermek için bir tekniktir.  İlişkili bir Web sitesi ile Azure Mobile Services kullanıyorsanız, CORS geçiş işleminin bir parçası olarak yapılandırmanız gerekir.  Yalnızca mobil aygıtlardan Azure Mobile Services erişiyorsanız, ardından CORS dışında nadir durumlarda yapılandırılması gerekmez.

Geçirilen CORS ayarlarınızı kullanılabilir olarak **MS_CrossDomainWhitelist** uygulama ayarı.  Siteniz için App Service CORS tesis geçirmek için:

1. [Azure portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** geçirilen mobil hizmetiniz adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Tıklatın **CORS** API menüsünde.
5. Tüm izin verilen çıkış noktası her birinden sonra Enter tuşuna basarak sağlanan kutuya girin.
6. İzin verilen çıkış noktası listesini doğru olduğunda Kaydet düğmesine tıklayın.

> [!TIP]
> Bir Azure uygulama hizmeti kullanmanın yararları aynı sitede web siteniz ve mobil hizmet çalıştırabilirsiniz biridir.  Daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü.
>
>

### <a name="download-publish-profile"></a>Yeni bir yayımlama profili indirin
Yayımlama profili, sitenizin Azure App Service'e geçirilirken değiştirilir.  Sitenizden Visual Studio'dan yayımlamak isterseniz, yeni bir yayımlama profili gerekir.  Yeni Yayımlama profilini indirmek için:

1. [Azure portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** geçirilen mobil hizmetiniz adına tıklayın.
3. Tıklatın **Get yayımlama profili**.

PublishSettings dosyasını bilgisayarınıza yüklenir.  Normalde adlı *sitename*. PublishSettings.  Yayımlama ayarlarını varolan projenize içeri aktarın:

1. Visual Studio ve Azure mobil hizmet projenizi açın.
2. Projenize sağ **Çözüm Gezgini** seçip **Yayımla...**
3. **İçeri Aktar**'a tıklayın
4. Tıklatın **Gözat** ve seçin, indirilen yayımlama ayarları dosyası.  **Tamam**’a tıklayın.
5. Tıklatın **bağlantıyı doğrula** yayımlama ayarları çalışmasını sağlamak için.
6. Tıklatın **Yayımla** sitenizi yayımlamak için.

## <a name="working-with-your-site"></a>Site geçiş sonrası ile çalışma
Yeni uygulama hizmetiniz ile çalışmaya başlamak [Azure portal] geçiş sonrası.  Bazı notlar gerçekleştirmek için kullanılan belirli işlemler şunlardır [Klasik Azure portalı], kendi uygulama hizmeti eşdeğer birlikte.

### <a name="publishing-your-site"></a>İndirme ve geçirilen sitenizi yayımlama
Sitenizin git veya ftp aracılığıyla kullanılabilir ve WebDeploy, TFS, Mercurial, GitHub ve FTP dahil olmak üzere çeşitli farklı mekanizmaları ile yeniden.  Dağıtım kimlik bilgileri, sitenin geri kalanını ile geçirilir.  Dağıtım kimlik bilgilerinizi ayarlamamış veya bunları hatırlamıyorsanız, bunları sıfırlayabilirsiniz:

1. [Azure portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** geçirilen mobil hizmetiniz adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Tıklatın **dağıtım kimlik bilgileri** yayımlama menüsü.
5. Yeni dağıtım kimlik bilgileri kutulara girin, sonra kaydetme düğmesini tıklatın.

Git ile site kopyalama veya GitHub, TFS veya Mercurial otomatik dağıtımları ayarlamak için bu kimlik bilgileri kullanabilirsiniz.  Daha fazla bilgi için [Azure uygulama hizmeti dağıtım belgeleri] bakın.

### <a name="appsettings"></a>Uygulama ayarları
Geçirilen bir mobil hizmet için çoğu ayarları aracılığıyla uygulama ayarlarını kullanılabilir.  Uygulama ayarlarından listesini almak [Azure portal].
Uygulama ayarlarını görüntülemek veya değiştirmek için:

1. [Azure portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** geçirilen mobil hizmetiniz adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Tıklatın **uygulama ayarları** genel menüsünde.
5. Uygulama ayarları bölümüne gidin ve uygulama ayarını bulun.
6. Değeri düzenlemek için uygulama ayarının değeri'ni tıklatın.  Tıklatın **kaydetmek** değeri kaydetmek için.

Aynı anda birden çok uygulama ayarlarını güncelleştirebilirsiniz.

> [!TIP]
> Aynı değere sahip iki uygulama ayarları vardır.  Örneğin, görebileceğiniz *ApplicationKey* ve *MS\_ApplicationKey*.  Aynı anda hem uygulama ayarlarını güncelleştirin.
>
>

### <a name="authentication"></a>Kimlik doğrulaması
Tüm kimlik doğrulama ayarları, uygulama ayarları geçirilen siteniz olarak kullanılabilir.  Kimlik doğrulama ayarlarınızı güncelleştirmek için uygun uygulama ayarları değiştirmeniz gerekir.  Aşağıdaki tabloda, kimlik doğrulama sağlayıcısı için uygun uygulaması ayarları gösterilmektedir:

| Sağlayıcı | İstemci Kimliği | İstemci Parolası | Diğer ayarları |
|:--- |:--- |:--- |:--- |
| Microsoft Hesabı |**MS\_MicrosoftClientID** |**MS\_MicrosoftClientSecret** |**MS\_MicrosoftPackageSID** |
| Facebook |**MS\_FacebookAppID** |**MS\_FacebookAppSecret** | |
| Twitter |**MS\_TwitterConsumerKey** |**MS\_TwitterConsumerSecret** | |
| Google |**MS\_GoogleClientID** |**MS\_GoogleClientSecret** | |
| Azure AD |**MS\_AadClientID** | |**MS\_AadTenants** |

Not: **MS\_AadTenants** Kiracı etki alanı ("İzin verilen kiracılar" alanları Mobile Services Portalı'nda) virgülle ayrılmış bir liste olarak depolanır.

> [!WARNING]
> **Kimlik doğrulama mekanizmaları ayarlar menüsünde kullanmayın**
>
> Azure uygulama hizmeti sağlayan ayrı bir "kod yok" kimlik doğrulama ve yetkilendirme sistemi altında *kimlik doğrulama / yetkilendirme* ayarlar menüsünü ve (kullanım dışı) *Mobil kimlik doğrulama* ayarları menüsündeki seçeneği.  Bu seçenekleri geçirilen bir Azure mobil hizmeti ile uyumlu değil.  Yapabilecekleriniz [sitenizi yükseltme](app-service-mobile-net-upgrading-from-mobile-services.md) Azure App Service kimlik doğrulama yararlanmak için.
>
>

### <a name="easytables"></a>Veri
*Veri* Mobile Services sekmesinde tarafından değiştirildi *kolay tabloları* Azure portalı içinde.  Kolay tabloları erişmek için:

1. [Azure portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** geçirilen mobil hizmetiniz adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Tıklatın **kolay tabloları** mobil menüsünde.

Bir tablo tıklayarak ekleyebilirsiniz **Ekle** düğmesini veya bir tablo adı tıklayarak mevcut tablolarınızı erişim.  Bu dikey pencereden yapabileceğiniz çeşitli işlemler vardır dahil olmak üzere:

* Tablo izinleri değiştirme
* İşletimsel komut dosyalarını düzenleme
* Tablo şemasını yönetme
* Tablo silme
* İçindekiler temizleme
* Tablonun belirli satırları silme

### <a name="easyapis"></a>API
*API* Mobile Services sekmesinde tarafından değiştirildi *kolay API'leri* Azure portalı içinde.  Kolay API'leri erişmek için:

1. [Azure portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** geçirilen mobil hizmetiniz adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Tıklatın **kolay API'leri** mobil menüsünde.

Geçirilen Apı'lerinizi zaten dikey penceresinde listelenir.  Bu dikey pencereden bir API de ekleyebilirsiniz.  Belirli bir API yönetmek için API'ı tıklatın.
Yeni dikey penceresinden izinleri ayarlama ve komut dosyaları için API düzenleyin.

### <a name="on-demand-jobs"></a>Zamanlayıcı işleri
Tüm zamanlayıcı işlerinin Zamanlayıcı İş koleksiyonları bölümü kullanılabilir.  Zamanlayıcı işlerinin erişmek için:

1. [Azure portal]’da oturum açın.
2. Seçin **Gözat >**, girin **zamanlama** içinde *filtre* kutusuna ve ardından **Zamanlayıcı koleksiyonları**.
3. Siteniz için iş koleksiyonu seçin.  Bu adlı *sitename*-işler.
4. Tıklatın **ayarları**.
5. Tıklatın **zamanlayıcı işlerinin** Yönet altında.

Zamanlanan işler geçişten önce belirtilen sıklığı birlikte listelenmiştir.  İsteğe bağlı işleri devre dışı bırakılır.  Bir isteğe bağlı işi çalıştırmak için:

1. Çalıştırmak istediğiniz işi seçin.
2. Gerekirse, **etkinleştirmek** iş etkinleştirilemedi.
3. Tıklatın **ayarları**, ardından **zamanlama**.
4. Bir yinelenmesini seçin **kez**, ardından **Kaydet**

İsteğe bağlı işleriniz bulunan `App_Data/config/scripts/scheduler post-migration`.  Tüm isteğe bağlı işleri [WebJobs] için dönüştürme öneririz veya [işlevler].  Yeni zamanlayıcı işlerinin [WebJobs] olarak yazma veya [işlevler].

### <a name="notification-hubs"></a>Bildirim hub'ları
Mobile Services anında iletme bildirimleri için Notification Hubs'ı kullanır.  Aşağıdaki uygulama ayarları, bildirim hub'ı geçişten sonra mobil hizmetinize bağlamak için kullanılır:

| Uygulama ayarı | Açıklama |
|:--- |:--- |
| **MS\_PushEntityNamespace** |Bildirim hub'ı Namespace |
| **MS\_NotificationHubName** |Bildirim hub'ı adı |
| **MS\_NotificationHubConnectionString** |Bildirim hub'ı bağlantı dizesi |
| **MS\_NamespaceName** |MS_PushEntityNamespace için diğer ad |

Bildirim Hub'ınız aracılığıyla yönetilen [Azure portal].  (Bu uygulama ayarlarını kullanarak bulabilirsiniz) bildirim hub'ı adını not edin:

1. [Azure portal]’da oturum açın.
2. Seçin **Gözat**> seçeneğini belirleyip **bildirim hub'ları**
3. Mobil hizmetle ilişkili bildirim hub'ı adına tıklayın.

> [!NOTE]
> Bildirim HUb'ınızı "Karma" türü ise, görünür değil.  Bildirim hub'ları bildirim hub'ları ve eski Service Bus özelliklerini kullanma "Karma" yazın.  [Karma alanlarınızın Dönüştür] devam etmeden önce.  Dönüştürme işlemi tamamlandıktan sonra bildirim hub'ınızı görünür [Azure portal].
>
>

Daha fazla bilgi için gözden [bildirim hub'ları] belgeleri.

> [!TIP]
> Bildirim hub'ları yönetimi özellikleri [Azure portal] olan hala önizlemede.  [Klasik Azure portalı] tüm bildirim hub'ı yönetmek için kullanılabilir durumda kalır.
>
>

### <a name="legacy-push"></a>Eski itme ayarları
Anında iletme, mobil hizmetinize bildirim hub'ları üzerinde giriş önce yapılandırdıysanız, kullanmakta olduğunuz *eski itme*.  Anında iletme kullanıyorsanız ve bir bildirim hub'ı yapılandırmanızda listelenen görüyor musunuz durumunda kullanmakta olduğunuz olasıdır *eski itme*.  Bu özellik ile diğer tüm özelliklerini geçirilir.  Ancak, geçiş tamamlandıktan hemen sonra Notification Hubs yükseltmenizi öneririz.

Bu arada, tüm eski itme ayarlarla (APNS sertifikasını, önemli özel durum) uygulama ayarlarında kullanılabilir.  APNS sertifikasını, dosya sistemi uygun dosyayı değiştirerek güncelleştirin.

### <a name="app-settings"></a>Diğer uygulama ayarları
Aşağıdaki ek uygulama ayarları, mobil hizmetinizden geçirilen ve altında kullanılabilir *ayarları* > *uygulama ayarları*:

| Uygulama ayarı | Açıklama |
|:--- |:--- |
| **MS\_MobileServiceName** |Uygulama adı |
| **MS\_MobileServiceDomainSuffix** |Etki alanı öneki. yani azure-mobile.net |
| **MS\_ApplicationKey** |Uygulama anahtarı |
| **MS\_MasterKey** |Uygulama ana anahtarı |

Uygulama anahtarı ve ana anahtar özgün mobil hizmetinizden uygulama anahtarlarına aynıdır.  Özellikle, uygulama anahtar kullanımları mobil API doğrulamak için mobil istemciler tarafından gönderilir.

### <a name="cliequivalents"></a>Komut satırı eşdeğerleri
Artık kullanabilirsiniz *azure mobil* Azure Mobile Services sitenizi yönetmek için komutu.  Bunun yerine, birçok işlevini ile değiştirilmiştir *azure site* komutu.  Sık kullanılan komutlar eşdeğerlerini bulmak için aşağıdaki tabloyu kullanın:

| *Azure Mobile* komutu | Eşdeğer *Azure Site* komutu |
|:--- |:--- |
| Mobil konumları |site konumu listesi |
| Mobil listesi |site listesi |
| Mobil Göster *adı* |Site Göster *adı* |
| Mobil yeniden *adı* |Site yeniden *adı* |
| Mobil dağıtın *adı* |Site dağıtım dağıtın *commitId* *adı* |
| Mobil anahtar kümesi *adı* *türü* *değeri* |Site appsetting silme *anahtar* *adı* <br/> Site appsetting eklemek *anahtar*=*değeri* *adı* |
| Mobil config listesi *adı* |Site appsetting listesinin *adı* |
| Mobil config alma *adı* *anahtarı* |Site appsetting Göster *anahtar* *adı* |
| Mobil yapılandırma kümesi *adı* *anahtarı* |Site appsetting silme *anahtar* *adı* <br/> Site appsetting eklemek *anahtar*=*değeri* *adı* |
| Mobil etki alanı listesi *adı* |site etki alanı listesi *adı* |
| Mobil etki alanı ekleme *adı* *etki alanı* |Site, etki alanı ekleme *etki alanı* *adı* |
| Mobil etki alanı delete *adı* |site etki alanı delete *etki alanı* *adı* |
| Mobil ölçek Göster *adı* |Site Göster *adı* |
| Mobil ölçek değişiklik *adı* |Site ölçek modu *modu* *adı* <br /> Site ölçek örnekleri *örnekleri* *adı* |
| Mobil appsetting listesi *adı* |Site appsetting listesinin *adı* |
| Mobil appsetting eklemek *adı* *anahtar* *değeri* |Site appsetting eklemek *anahtar*=*değeri* *adı* |
| Mobil appsetting silme *adı* *anahtarı* |Site appsetting silme *anahtar* *adı* |
| Mobil appsetting Göster *adı* *anahtarı* |Site appsetting silme *anahtar* *adı* |

Kimlik doğrulaması veya anında iletme bildirimi ayarlarını uygun uygulama ayarı güncelleştirerek güncelleştirin.
Dosyaları düzenleyebilir ve sitenizi ftp veya git üzerinden yayımlayın.

### <a name="diagnostics"></a>Tanılama ve günlüğe kaydetme
Tanılama günlük normalde bir Azure App Service'te devre dışıdır.  Tanılama günlük kaydını etkinleştirmek için:

1. [Azure portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** geçirilen mobil hizmetiniz adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Seçin **tanılama günlüklerini** Özellikler menüsü altında.
5. Tıklatın **ON** günlükleri için: **uygulama günlüğü (dosya sistemi)**, **ayrıntılı hata iletileri**, ve **başarısız istek izleme**
6. Tıklatın **dosya sistemi** için Web sunucusu günlüğü
7. **Kaydet**’e tıklayın

Günlükleri görüntülemek için:

1. [Azure portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** geçirilen mobil hizmetiniz adına tıklayın.
3. Tıklatın **Araçları** düğmesi
4. Seçin **günlük akışı** GÖZLEMLE menüsünün altında.

Bunlar oluşturulduğunda günlükleri penceresinde görüntülenir.  Dağıtım kimlik bilgilerinizi kullanarak daha sonraki analizler için günlükleri de indirebilirsiniz. Daha fazla bilgi için bkz: [günlüğü] belgeleri.

## <a name="known-issues"></a>Bilinen sorunlar
### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Geçirilen mobil uygulama kopya silme site kesintiye neden olur
Azure PowerShell kullanarak geçirilen mobil hizmetinizi kopyalama, ardından kopya silin, üretim hizmetiniz için DNS girişi kaldırılır.  Sitenizi olduğu artık Internet'ten erişilebilir.  

Çözüm: sitenizi kopyalamak isterseniz, portalı üzerinden bunu.

### <a name="changing-webconfig-does-not-work"></a>Web.config değiştirme çalışmıyor
Bir ASP.NET sitesi varsa, değişikliklerini `Web.config` dosya uygulanmaz.  Azure uygulama hizmeti uygun bir derlemeler `Web.config` Mobile Services çalışma zamanı desteği başlatılırken dosya.  Bir XML dönüştürme dosyası kullanarak (örneğin, özel üstbilgiler) belirli ayarları geçersiz kılabilirsiniz.  Bir dosya oluşturun olarak adlandırılan `applicationHost.xdt` -bu dosyanın sonunda `D:\home\site` Azure hizmeti üzerinde dizin.  Karşıya yükleme `applicationHost.xdt` dosya aracılığıyla özel dağıtım komut dosyası veya Kudu kullanarak doğrudan.  Aşağıdaki örnek bir belgeyi gösterir:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Daha fazla bilgi için bkz: [XDT dönüştürme örnekleri] belgeler GitHub üzerinde.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Trafik Yöneticisi için geçirilen Mobile Services eklenemez.
Bir Traffic Manager profili oluşturduğunuzda, doğrudan geçirilen bir mobil hizmet profiline seçemezsiniz.  "Dış uç noktası." kullanın  Dış uç noktası yalnızca PowerShell aracılığıyla eklenebilir.  Daha fazla bilgi için bkz: [trafik Yöneticisi Öğreticisi](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanızı App Service'e geçirilir, kullanabileceğiniz daha da fazla özellikleri vardır:

* Dağıtım [yuvaları hazırlama] değişiklikleri sitenize aşama ve gerçekleştirmenizi sağlayan / B testi.
* [WebJobs] isteğe bağlı zamanlanan işleri için yenileme sağlar.
* Yapabilecekleriniz [sürekli olarak dağıtma] GitHub, TFS veya Mercurial sitenizi bağlayarak sitenizi.
* Kullanabileceğiniz [Application Insights] sitenizi izlemek için.
* Bir Web sitesi ve bir mobil API aynı kodundan işlevi görür.

### <a name="upgrading-your-site"></a>Azure Mobile Apps SDK'sı için Mobile Services sitesi yükseltiliyor
* Node.js tabanlı sunucu projeleri için yeni [Mobile Apps Node.js SDK'sı] birçok yeni özellik sağlar. Örneğin, artık yerel geliştirme ve hata ayıklama yapın, 0,10 yukarıda herhangi bir Node.js sürümünü kullanın ve Express.js Ara yazılımların ile özelleştirme.
* İçin. NET tabanlı sunucu projeleri, yeni [Mobile Apps SDK NuGet paketlerini](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) NuGet bağımlılıklar hakkında daha fazla esneklik bulunur.  Bu paketleri yeni uygulama hizmeti kimlik doğrulamayı desteklemek ve ile tüm ASP.NET projesi oluşturun. Yükseltme hakkında daha fazla bilgi edinmek için [varolan .NET Mobil hizmetinizi App Service'e yükseltme](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[Uygulama hizmeti fiyatlandırması]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Application Insights]: ../application-insights/app-insights-overview.md
[Otomatik ölçek]: ../app-service/web-sites-scale.md
[Azure App Service]: ../app-service/app-service-web-overview.md
[Klasik Azure portalı]: https://manage.windowsazure.com
[Azure portal]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure Zamanlayıcı planları]: ../scheduler/scheduler-plans-billing.md
[sürekli olarak dağıtma]: ../app-service/app-service-continuous-deployment.md
[Karma alanlarınızın Dönüştür]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[özel etki alanı adlarını]: ../app-service/app-service-web-tutorial-custom-domain.md
[Fiddler]: http://www.telerik.com/fiddler
[Azure uygulama hizmeti genel kullanılabilirliğini]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hybrid Connections]: ../app-service/app-service-hybrid-connections.md
[günlüğü]: ../app-service/web-sites-enable-diagnostic-log.md
[Mobile Apps Node.js SDK'sı]: https://github.com/azure/azure-mobile-apps-node
[Mobile Services vs. Uygulama hizmeti]: app-service-mobile-value-prop-migration-from-mobile-services.md
[bildirim hub'ları]: ../notification-hubs/notification-hubs-push-notification-overview.md
[performans izleme]: ../app-service/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[yuvaları hazırlama]: ../app-service/web-sites-staged-publishing.md
[VNet]: ../app-service/web-sites-integrate-with-vnet.md
[XDT dönüştürme örnekleri]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[işlevler]: ../azure-functions/functions-overview.md
