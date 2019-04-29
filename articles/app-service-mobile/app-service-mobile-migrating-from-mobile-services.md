---
title: Bir App Service mobil uygulaması mobil Hizmetler'den geçiş
description: Bir App Service Mobile Apps, Mobile Services uygulamanıza kolayca geçirmeyi öğrenin
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
ms.openlocfilehash: dfc5e2923215b1669b0a3300653ad0cae7379655
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122481"
---
# <a name="article-top"></a>Var olan Azure mobil hizmetinizi Azure App Service'e geçirme
İle [Azure App Service'in genel kullanılabilirlik], Azure mobil Hizmetleri'ne sitelerine yerinde Azure App Service'in tüm özelliklerinden yararlanmak için kolayca da geçirilebilir.  Bu belgede, sitenizin Azure App Service için Azure Mobile Services geçirilirken beklenmesi gerekenler açıklanmaktadır.

## <a name="what-does-migration-do"></a>Geçiş sitenize ne yapar
Azure mobil hizmetiniz geçişini kapatır mobil hizmetinizde bir [Azure App Service] uygulama kodunu etkilemeden.  Notification Hubs, SQL veri bağlantısı, kimlik doğrulama ayarları, zamanlanmış işleri ve etki alanı adı değişmeden kalır.  Azure mobil hizmetinizi kullanarak mobil istemciler normal şekilde çalışmaya devam eder.  Azure App Service'e devredildikten sonra geçiş hizmetinizi yeniden başlatır.

[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Sitenizi neden geçirmeniz gerekir
Microsoft Azure App Service özelliklerinden yararlanmak için Azure mobil hizmetinizi geçirme öneren:

* [WebJobs] dahil yeni ana bilgisayara özellikleri ve [Özel etki alanı adları].
* İzleme ve sorun giderme [Application Insights].
* Yerleşik DevOps araçları da dahil olmak üzere [hazırlama yuvası], geri alma ve üretimde test etme.
* [Otomatik ölçeklendirme], Yük Dengeleme, ve [Performans izleme].

Azure App Service avantajları hakkında daha fazla bilgi için bkz. [Mobil hizmetler ile App Service] konu.

## <a name="before-you-begin"></a>Başlamadan önce
Herhangi bir ana iş sitenizde başlamadan önce SQL veritabanı ve mobil hizmeti betikleri yedeklemelisiniz.

## <a name="migrating-site"></a>Sitelerinizi geçirme
Geçiş işlemi, tek bir Azure bölgesi içindeki tüm siteleri geçirir.

Sitenizi geçirmek için:

1. Oturum [Klasik Azure Portalı].
2. Geçirmek istediğiniz bölgede bir mobil hizmeti seçin.
3. Tıklayın **App Service'e geçirme** düğmesi.

   ![Geçiş düğmesi][0]
4. App Service iletişim kutusunda geçirme okuyun.
5. Mobil hizmetinizin adını sağlanan kutuya girin.  Örneğin, etki alanı adınızı contoso.azure-mobile.net varsa enter *contoso* sağlanan kutuya.
6. Onay düğmesine tıklayın.

Geçiş etkinlik izleyicisinin durumunu izleyin. Siteniz olarak listelenip listelenmediğini *geçirme* Azure Klasik Portalı'nda.

  ![Geçiş etkinliğini izleme][1]

Her geçiş 3'ten herhangi bir yere Geçirilmekte olan mobil hizmet başına 15 dakika sürebilir.  Sitenizin, geçiş sırasında kullanılabilir durumda kalır.
Geçiş işleminin sonunda, sitenizi yeniden başlatılır.  Site birkaç saniye sürebilir yeniden başlatma işlemi sırasında kullanılamaz.

## <a name="finalizing-migration"></a>Geçiş Tamamlanıyor
Bir mobil istemci geçiş işleminin sonunda sitenizden sınamayı planlayın.  Mobil istemciyi değişiklik yapmadan tüm ortak istemci eylemlerini gerçekleştirebilir emin olun.  

### <a name="update-app-service-tier"></a>Uygun bir App Service fiyatlandırma katmanı seçin
Azure App Service'e geçirdikten sonra fiyatlandırma daha fazla esneklik var.

1. [Azure Portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** ardından geçirilen mobil hizmetinizde adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Tıklayın **App Service planı** Ayarlar menüsünden.
5. Tıklayın **fiyatlandırma katmanı** Döşe.
6. Gereksinimlerinize uygun kutucuğuna tıklayın, ardından tıklatın **seçin**.  I gerekebilir **tümünü görüntüle** kullanılabilir fiyatlandırma katmanlarına görmek için.

Bir başlangıç noktası olarak aşağıdaki katmanları öneririz:

| Mobil hizmet fiyatlandırma katmanı | App Service fiyatlandırma katmanı |
|:--- |:--- |
| Ücretsiz |F1 Ücretsiz |
| Temel |Temel B1 |
| Standart |S1 Standart |

Fiyatlandırma katmanı uygulamanız için sağ seçme içinde önemli ölçüde esneklik yoktur.  Başvurmak [App Service fiyatlandırması] , yeni bir App Service planının fiyatlandırma hakkında tam Ayrıntılar için.

> [!TIP]
> App Service standart katman da dahil olmak üzere kullanmak isteyebilirsiniz birçok özellik erişimi içerir [hazırlama yuvası], otomatik yedeklemeler ve otomatik ölçeklendirme.  Varken yeni özellikleri kontrol edin!
>
>

### <a name="review-migration-scheduler-jobs"></a>Gözden geçirilen Scheduler işleri
Scheduler işleri geçişten sonra yaklaşık 30 dakika kadar görünür olmayacaktır.  Zamanlanan işler arka planda çalışmaya devam eder.
Zamanlanan işleriniz tekrar görünür olduktan sonra görüntülemek için:

1. [Azure Portal]’da oturum açın.
2. Seçin **Gözat >**, girin **zamanlama** içinde *filtre* kutusuna ve ardından **Scheduler koleksiyonları**.

Ücretsiz zamanlayıcı işleri kullanılabilir geçiş sonrası sınırlı sayıda vardır.  Kullanımınızı gözden geçirin ve [Azure Zamanlayıcı planları].

### <a name="configure-cors"></a>Gerekirse CORS'yi yapılandırın
Çıkış noktaları arası kaynak paylaşımı, farklı bir etki alanındaki bir Web API'sine erişmek bir Web sitesi izin vermek için bir tekniktir.  Azure mobil hizmetler ile ilişkili bir Web sitesi kullanıyorsanız, CORS geçişin bir parçası yapılandırmanız gerekir.  Azure Mobile Services özel olarak mobil cihazlardan erişiyorsanız, daha sonra CORS dışında nadir durumlarda yapılandırılması gerekmez.

Geçirilen CORS ayarlarınızı olarak kullanılabilir **MS_CrossDomainWhitelist** uygulama ayarı.  Siteniz için App Service CORS tesis geçirmek için:

1. [Azure Portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** ardından geçirilen mobil hizmetinizde adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Tıklayın **CORS** API menüsünde.
5. Tüm izin verilen çıkış noktaları her birinden sonra Enter tuşuna basarak sağlanan kutuya girin.
6. İzin verilen çıkış noktaları listesi doğru olduğunda, Kaydet düğmesine tıklayın.

> [!TIP]
> Bir Azure App Service'ı kullanmanın avantajlarını aynı sitede web siteniz ve mobil hizmeti çalıştırabilirsiniz biridir.  Daha fazla bilgi için [sonraki adımlar](#next-steps) bölümü.
>
>

### <a name="download-publish-profile"></a>Yeni bir yayımlama profilini indirin
Azure App Service'e geçirilmesi sitenizi yayımlama profilini değiştirilir.  Sitenizi Visual Studio'dan yayımlamak istediğiniz yeni bir yayımlama profili gerekir.  Yeni Yayımlama profilini indirmek için:

1. [Azure Portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** ardından geçirilen mobil hizmetinizde adına tıklayın.
3. Tıklayın **yayımlama profili Al**.

PublishSettings dosyasını bilgisayarınıza indirilir.  Normalde adlı *sitename*. PublishSettings.  Yayımlama ayarlarını, varolan bir projeye içeri aktarın:

1. Visual Studio ve Azure mobil hizmet projenizi açın.
2. Projenize sağ tıklayın **Çözüm Gezgini** seçip **Yayımla...**
3. **İçeri Aktar**'a tıklayın
4. Tıklayın **Gözat** ve seçin, indirilen yayımlama ayarları dosyası.  **Tamam**’a tıklayın.
5. Tıklayın **bağlantıyı doğrula** yayımlama ayarları iş emin olmak için.
6. Tıklayın **Yayımla** sitenizi yayımlamak için.

## <a name="working-with-your-site"></a>Site geçiş sonrası ile çalışma
Yeni App Service içinde ile çalışmaya başlamak [Azure portal] geçiş sonrası.  Aşağıdakileri gerçekleştirmek için kullanılan belirli işlemleri üzerindeki bazı notlardır [Klasik Azure portalı], App Service eşdeğeri ile birlikte.

### <a name="publishing-your-site"></a>İndirme ve geçirilen sitenizi yayımlama
Sitenizi ve WebDeploy, TFS, Mercurial, GitHub ve FTP gibi çeşitli farklı sistemler ile yeniden git veya ftp kullanılabilir.  Dağıtım kimlik bilgilerini, sitenin geri kalanını ile geçirilir.  Dağıtım kimlik bilgilerinizi ayarlanmamıştır veya bunları anımsamıyorsanız, bunları sıfırlayabilir:

1. [Azure Portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** ardından geçirilen mobil hizmetinizde adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Tıklayın **dağıtım kimlik bilgileri** yayımlama menüsü.
5. Yeni dağıtım kimlik bilgileri kutulara girin, ardından Kaydet düğmesine tıklayın.

Git ile site kopyalama veya GitHub, TFS veya Mercurial otomatik dağıtımları ayarlamak için bu kimlik bilgilerini kullanabilirsiniz.  Daha fazla bilgi için [Azure App Service dağıtım belgelerine] bakın.

### <a name="appsettings"></a>Uygulama ayarları
Geçirilen bir mobil hizmetin çoğu ayarları, uygulama ayarları kullanılabilir.  Uygulama ayarları listesi alabilirsiniz [Azure portal].
Uygulama ayarlarını görüntülemek veya değiştirmek için:

1. [Azure Portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** ardından geçirilen mobil hizmetinizde adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Tıklayın **uygulama ayarları** genel menüsünde.
5. Uygulama ayarları bölümüne gidin ve uygulama ayarınızı bulun.
6. Değeri uygulama ayarının değerini düzenlemek için tıklayın.  Tıklayın **Kaydet** değeri kaydetmek için.

Aynı anda birden çok uygulama ayarlarını güncelleştirebilirsiniz.

> [!TIP]
> Aynı değere sahip iki adet uygulama ayarı vardır.  Örneğin, görebileceğiniz *tanıtıcı, ApplicationKey* ve *MS\_tanıtıcı, ApplicationKey*.  Aynı anda hem uygulama ayarlarını güncelleştirin.
>
>

### <a name="authentication"></a>Kimlik doğrulaması
Tüm kimlik doğrulama ayarları, sitenizde geçirilen uygulama ayarları olarak kullanılabilir.  Kimlik doğrulama ayarlarınızı güncelleştirmek için uygun uygulama ayarlarını değiştirmeniz gerekir.  Aşağıdaki tabloda, kimlik doğrulama sağlayıcısı için uygun uygulama ayarları gösterilmektedir:

| Sağlayıcı | İstemci Kimliği | İstemci Gizli Anahtarı | Diğer ayarlar |
|:--- |:--- |:--- |:--- |
| Microsoft Hesabı |**MS\_MicrosoftClientID** |**MS\_MicrosoftClientSecret** |**MS\_MicrosoftPackageSID** |
| Facebook |**MS\_FacebookAppID** |**MS\_FacebookAppSecret** | |
| Twitter |**MS\_TwitterConsumerKey** |**MS\_TwitterConsumerSecret** | |
| Google |**MS\_GoogleClientID** |**MS\_GoogleClientSecret** | |
| Azure AD |**MS\_AadClientID** | |**MS\_AadTenants** |

Not: **MS\_AadTenants** (Mobile Services Portalı'nda "İzin verilen kiracılar" alanları) Kiracı etki alanlarının virgülle ayrılmış bir liste olarak depolanır.

> [!WARNING]
> **Ayarlar menüsünde kimlik doğrulama mekanizmaları kullanma**
>
> Azure App Service sağlayan ayrı bir "kod yok" kimlik doğrulaması ve yetkilendirme sistemi altında *kimlik doğrulama / yetkilendirme* Ayarlar menüsünden ve (kullanım dışı) *Mobil kimlik doğrulaması* seçeneği Ayarlar menüsü altında.  Bu seçenekler, geçirilmiş bir Azure mobil hizmeti ile uyumlu değildir.  Yapabilecekleriniz [sitenizi yükseltme](app-service-mobile-net-upgrading-from-mobile-services.md) Azure App Service kimlik doğrulaması yararlanmak için.
>
>

### <a name="easytables"></a>Veri
*Veri* sekmesinde mobil hizmetler tarafından değiştirilmiştir *kolay tablolar* Azure portalındaki.  Kolay tablolar erişmek için:

1. [Azure Portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** ardından geçirilen mobil hizmetinizde adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Tıklayın **kolay tablolar** mobil menüsünde.

Tıklayarak bir tablo ekleyebilirsiniz **Ekle** düğmesine veya bir tablo adına tıklayarak mevcut tablolarınızı erişim.  Bu dikey pencereden yapabileceğiniz çeşitli işlemler vardır dahil olmak üzere:

* Tablo izinleri değiştirme
* Operasyonel betiklerinizi düzenleme
* Tablo şemasını yönetme
* Tablo silme
* İçindekiler temizleme
* Tablodaki belirli satırları silme

### <a name="easyapis"></a>API
*API* sekmesinde mobil hizmetler tarafından değiştirilmiştir *kolay API'ler* Azure portalındaki.  Kolay API'ler erişmek için:

1. [Azure Portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** ardından geçirilen mobil hizmetinizde adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Tıklayın **kolay API'ler** mobil menüsünde.

Geçirilen Apı'lerinizi zaten dikey penceresinde listelenir.  Bu dikey pencereden bir API de ekleyebilirsiniz.  Belirli bir API'yi yönetmek için API'ı tıklatın.
Yeni dikey pencerede, izinleri ayarlayabilir ve için API betiklerini Düzenle.

### <a name="on-demand-jobs"></a>Scheduler işleri
Scheduler iş koleksiyonları bölümde tüm zamanlayıcı işleri kullanılabilir.  Zamanlayıcı işlerinizi erişmek için:

1. [Azure Portal]’da oturum açın.
2. Seçin **Gözat >**, girin **zamanlama** içinde *filtre* kutusuna ve ardından **Scheduler koleksiyonları**.
3. Siteniz için iş koleksiyonu seçin.  İsimlendirilmiştir *sitename*-işler.
4. Tıklayın **ayarları**.
5. Tıklayın **zamanlayıcı işlerinin** Yönet altında.

Zamanlanmış işler, geçiş işleminden önce belirtilen sıklıkta listelenir.  İsteğe bağlı işleri devre dışı.  İsteğe bağlı işi çalıştırmak için:

1. Çalıştırmak istediğiniz işi seçin.
2. Gerekirse, **etkinleştirme** işini etkinleştirmek için.
3. Tıklayın **ayarları**, ardından **zamanlama**.
4. Bir yineleme seçin **kez**, ardından **Kaydet**

İsteğe bağlı işlerinizi bulunan `App_Data/config/scripts/scheduler post-migration`.  Tüm isteğe bağlı işleri [Web işleri] dönüştürmenizi öneririz veya [İşlevler].  Yeni bir zamanlayıcı işleri [WebJobs] olarak yazma veya [İşlevler].

### <a name="notification-hubs"></a>Bildirim hub'ları
Mobil hizmetler, anında iletme bildirimleri için Notification hubs'ı kullanır.  Aşağıdaki uygulama ayarlarını, bildirim hub'ı, geçişten sonra mobil hizmetinize bağlamak için kullanılır:

| Uygulama ayarı | Açıklama |
|:--- |:--- |
| **MS\_PushEntityNamespace** |Bildirim hub'ı Namespace |
| **MS\_NotificationHubName** |Bildirim hub'ı adı |
| **MS\_NotificationHubConnectionString** |Bildirim hub'ı bağlantı dizesi |
| **MS\_NamespaceName** |MS_PushEntityNamespace için bir diğer ad |

Bildirim Hub'ınız aracılığıyla yönetilen [Azure portal].  (Bu uygulama ayarlarını kullanarak bulabilirsiniz) bildirim hub'ı adını not edin:

1. [Azure Portal]’da oturum açın.
2. Seçin **Gözat**>, ardından **bildirim hub'ları**
3. Mobil hizmetle ilişkili bildirim hub'ı adına tıklayın.

> [!NOTE]
> Bildirim HUb'ınıza bir "Mixed" türü ise görünür değil.  Bildirim hub'ları hem bildirim hub'ları hem de eski Service Bus özellikleri kullanan "Karma" yazın.  [Karma ad alanlarınıza Dönüştür] devam etmeden önce.  Dönüştürme işlemi tamamlandıktan sonra bildirim hub'ınıza görünür [Azure portal].
>
>

Daha fazla bilgi için gözden [Notification Hubs] belgeleri.

> [!TIP]
> Bildirim hub'ları yönetimi özellikleri [Azure portal] Önizleme aşamasında olan.  [Klasik Azure portalı] tüm Notification Hubs'ı yönetmek için kullanılabilir durumda kalır.
>
>

### <a name="legacy-push"></a>Eski gönderim ayarları
Anında iletme, mobil hizmetinize bildirim hub'ları üzerinde giriş önce yapılandırdıysanız, kullanmakta olduğunuz *eski anında iletme*.  Anında iletme kullanıyorsanız ve yapılandırmanızda listelenen bir bildirim hub'ı görmüyorsanız durumunda kullanıyorsanız büyük olasılıkla *eski anında iletme*.  Bu özellik, diğer özellikleri ile geçirilir.  Ancak, geçiş tamamlandıktan hemen sonra Notification hubs'ı yükseltmeniz önerilir.

Bu arada, tüm eski gönderim ayarları (APNS sertifikasını, önemli özel durum ile) uygulama ayarlarında kullanılabilir.  APNS sertifikasını, uygun dosya sisteminde değiştirerek güncelleştirin.

### <a name="app-settings"></a>Diğer uygulama ayarları
Aşağıdaki ek uygulama ayarları, mobil hizmetinizden geçirilir ve altında *ayarları* > *uygulama ayarları*:

| Uygulama ayarı | Açıklama |
|:--- |:--- |
| **MS\_MobileServiceName** |Uygulamanızın adı |
| **MS\_MobileServiceDomainSuffix** |Etki alanı ön eki. yani azure-mobile.net |
| **MS\_tanıtıcı, ApplicationKey** |Uygulama anahtarı |
| **MS\_MasterKey** |Uygulama ana anahtarınız |

Uygulama anahtarı ve ana anahtarı özgün mobil hizmetinizden uygulama anahtarlarını aynıdır.  Özellikle, uygulama anahtarı, mobil API bunların kullanılması doğrulamak için mobil istemciler tarafından gönderilir.

### <a name="cliequivalents"></a>Komut satırı eşdeğerleri
Böyle kullanabileceğinizi *azure mobil* Azure Mobile Services sitenizi yönetmek için komutu.  Bunun yerine, birçok işlev ile değiştirilmiştir *azure site* komutu.  Sık kullanılan komutlar eşdeğerleri bulmak için aşağıdaki tabloyu kullanın:

| *Azure mobil* komutu | Eşdeğer *Azure sitesine* komutu |
|:--- |:--- |
| Mobil konumları |site konumu listesi |
| Mobil listesi |site listesi |
| Mobil show *adı* |Site show *adı* |
| Mobil yeniden *adı* |Site yeniden *adı* |
| Mobil yeniden dağıtma *adı* |Site dağıtımı yeniden dağıtma *Commitıd* *adı* |
| Mobil anahtar kümesi *adı* *türü* *değeri* |Site appsetting Sil *anahtarı* *adı* <br/> Site uygulama ayarı ekleme *anahtarı*=*değer* *adı* |
| Mobil config listesi *adı* |Site appsetting listesi *adı* |
| Mobil yapılandırmasını al *adı* *anahtarı* |Site appsetting show *anahtarı* *adı* |
| Mobil config set *adı* *anahtarı* |Site appsetting Sil *anahtarı* *adı* <br/> Site uygulama ayarı ekleme *anahtarı*=*değer* *adı* |
| Mobil etki alanı listesi *adı* |site etki alanı listesi *adı* |
| Mobil etki alanı ekleme *adı* *etki alanı* |site etki alanı ekleme *etki alanı* *adı* |
| Mobil etki alanı silme *adı* |site etki alanı silme *etki alanı* *adı* |
| Mobil ölçek show *adı* |Site show *adı* |
| Mobil ölçek değişikliği *adı* |Site ölçek modu *modu* *adı* <br /> Site ölçek örneğini *örnekleri* *adı* |
| Mobil uygulama ayarı listesi *adı* |Site appsetting listesi *adı* |
| Mobil uygulama ayarı ekleme *adı* *anahtarı* *değeri* |Site uygulama ayarı ekleme *anahtarı*=*değer* *adı* |
| Mobil uygulama ayarı silmek *adı* *anahtarı* |Site appsetting Sil *anahtarı* *adı* |
| Mobil uygulama ayarı show *adı* *anahtarı* |Site appsetting Sil *anahtarı* *adı* |

Kimlik doğrulaması veya anında iletme bildirimi ayarları uygun uygulama ayarı güncelleştirerek güncelleştirin.
Dosyaları düzenleyebilir ve sitenizi ftp veya git aracılığıyla yayımlayın.

### <a name="diagnostics"></a>Tanılama ve günlüğe kaydetme
Tanılama günlüğüne kaydetme normal bir Azure App Service'te devre dışı bırakıldı.  Tanılama günlüğüne kaydetmeyi etkinleştirmek için:

1. [Azure Portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** ardından geçirilen mobil hizmetinizde adına tıklayın.
3. Varsayılan ayarlar dikey penceresi açılır.
4. Seçin **tanılama günlükleri** Özellikler menüsü altında.
5. Tıklayın **ON** günlükleri için: **Uygulama günlüğü (dosya sistemi)**, **ayrıntılı hata iletileri**, ve **başarısız istek izleme**
6. Tıklayın **dosya sistemi** için Web sunucusu günlüğü
7. **Kaydet**’e tıklayın

Günlükleri görüntülemek için:

1. [Azure Portal]’da oturum açın.
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** ardından geçirilen mobil hizmetinizde adına tıklayın.
3. Tıklayın **Araçları** düğmesi
4. Seçin **günlük Stream** GÖZLEMLE menüsü altında.

Böylece, oluşturulan gibi günlükleri penceresinde görüntülenir.  Ayrıca, dağıtım kimlik bilgilerinizi kullanarak daha sonra çözümlemek için günlükleri indirebilirsiniz. Daha fazla bilgi için [Logging] belgeleri.

## <a name="known-issues"></a>Bilinen sorunlar
### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Bir site kesintisi neden geçirilmiş bir mobil uygulama kopya siliniyor
Azure PowerShell kullanarak, geçirilen mobil hizmetinize kopyalayın ve ardından kopya silebilirsiniz, üretim hizmetiniz için DNS girişi kaldırıldı.  Sitenizi olup artık Internet'ten erişilebilir.  

Çözüm: Sitenizi kopyalamak istiyorsanız, portal üzerinden bunu.

### <a name="changing-webconfig-does-not-work"></a>Web.config değiştiriliyor çalışmıyor
Bir ASP.NET sitesi varsa, değişikliklerini `Web.config` dosya uygulanmadı.  Azure App Service uygun bir yapılar `Web.config` Mobile Services çalışma zamanı desteği başlatılırken dosya.  Bir XML dönüşümü dosyası kullanarak (örneğin, özel üst bilgiler) belirli ayarları geçersiz kılabilirsiniz.  Bir dosya oluşturun olarak adlandırılan `applicationHost.xdt` -bu dosya düştüğünden gerekir `D:\home\site` Azure hizmeti üzerinde dizin.  Karşıya yükleme `applicationHost.xdt` Kudu kullanarak doğrudan veya bir özel dağıtım betiği aracılığıyla dosya.  Aşağıdaki örnek bir belgeyi gösterir:

```xml
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

Daha fazla bilgi için [XDT dönüştürme örnekleri] github'daki belgeler.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Geçirilen mobil hizmetler için Traffic Manager eklenemez.
Traffic Manager profili oluşturduğunuzda, doğrudan geçirilen bir mobil hizmet profiline seçemezsiniz.  "Dış uç noktası." kullanın  Dış uç noktası yalnızca PowerShell aracılığıyla eklenebilir.  Daha fazla bilgi için [Traffic Manager öğretici](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Sonraki Adımlar
Uygulamanızı App Service'e geçirilir, kullanabileceğiniz daha da fazla özellik vardır:

* Dağıtım [hazırlama yuvası] sitenizde değişiklikler aşama ve gerçekleştirmenizi sağlayan / B testi.
* İsteğe bağlı zamanlanmış işleri için değiştirme [WebJobs] sağlayın.
* Yapabilecekleriniz [sürekli dağıtım] GitHub, TFS veya Mercurial sitenizi bağlayarak sitenizi.
* Kullanabileceğiniz [Application Insights] sitenizi izlemek için.
* Bir Web sitesi ve bir mobil API'si aynı kodun işlevi görür.

### <a name="upgrading-your-site"></a>Azure Mobile Apps SDK'sı için Mobile Services sitesi yükseltiliyor
* Node.js tabanlı sunucu projeleri için yeni [Mobile Apps Node.js SDK'sı] birçok yeni özellik sunar. Örneğin, artık yerel geliştirme ve hata ayıklama, 0,10 yukarıda herhangi bir Node.js sürümünü kullanın ve Express.js Ara yazılımların ile özelleştirme.
* İçin. AĞ tabanlı sunucu projeleri, yeni [Mobile Apps SDK'sı NuGet paketleri](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) NuGet bağımlılıklar hakkında daha fazla esnekliğe sahipsiniz.  Bu paketleri, yeni App Service kimlik doğrulamasını desteklemek ve ile herhangi bir ASP.NET projesi oluşturun. Yükseltme hakkında daha fazla bilgi edinmek için [, mevcut .NET Mobile Services'ı App Service'a yükseltme](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[App Service fiyatlandırması]: https://azure.microsoft.com/pricing/details/app-service/
[Application Insights]: ../azure-monitor/app/app-insights-overview.md
[Otomatik ölçeklendirme]: ../app-service/web-sites-scale.md
[Azure App Service]: ../app-service/overview.md
[Klasik Azure Portalı]: https://manage.windowsazure.com
[Azure portal]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/regions/
[Azure Zamanlayıcı planları]: ../scheduler/scheduler-plans-billing.md
[sürekli dağıtım]: ../app-service/deploy-continuous-deployment.md
[Karma ad alanlarınıza Dönüştür]: https://azure.microsoft.com/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: https://curl.haxx.se/
[Özel etki alanı adları]: ../app-service/app-service-web-tutorial-custom-domain.md
[Fiddler]: https://www.telerik.com/fiddler
[Azure App Service'in genel kullanılabilirlik]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hybrid Connections]: ../app-service/app-service-hybrid-connections.md
[Logging]: ../app-service/troubleshoot-diagnostic-logs.md
[Mobile Apps Node.js SDK'sı]: https://github.com/azure/azure-mobile-apps-node
[Mobil hizmetler ile App Service]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Notification Hubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Performans izleme]: ../app-service/web-sites-monitor.md
[Postman]: https://www.getpostman.com/
[Hazırlama yuvası]: ../app-service/deploy-staging-slots.md
[VNet]: ../app-service/web-sites-integrate-with-vnet.md
[XDT dönüştürme örnekleri]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[İşlevler]: ../azure-functions/functions-overview.md
