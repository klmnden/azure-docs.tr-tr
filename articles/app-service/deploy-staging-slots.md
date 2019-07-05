---
title: Hazırlık ortamları Azure App service'taki web apps için ayarlama | Microsoft Docs
description: Azure App service'taki web apps için hazırlanmış yayımlamayı kullanmayı öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
writer: cephalin
manager: jpconnoc
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2019
ms.author: cephalin
ms.openlocfilehash: fd488d475e24bc1aeebfa49b9d81b04ebae449ff
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445607"
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Azure App Service ortamlarında hazırlık ayarlama
<a name="Overview"></a>

Web uygulaması, web uygulaması Linux, mobil arka ucuna veya API uygulamasına dağıtırken [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714), çalıştırmakta olduğunuz zaman ayrı bir dağıtım yuvası yerine varsayılan üretim yuvasına kullanabilirsiniz **standart**, **Premium**, veya **yalıtılmış** App Service planı katmanı. Dağıtım yuvaları kendi ana bilgisayar adları olan Canlı uygulamalardır. Uygulama içeriği ve yapılandırma öğelerinin, üretim yuvası dahil iki dağıtım yuvası arasında değişiklik yapılabilir. 

Uygulamanızı üretim dışı bir yuvaya dağıtmak için aşağıdaki faydaları vardır:

* Üretim yuvasıyla değiştirmeden önce hazırlık dağıtım yuvasındaki uygulama değişiklikleri doğrulayabilirsiniz.
* Bir uygulamayı ilk kez bir yuvasına dağıtma ve üretime geçirmeden yuvası tüm örneklerini üretime takas önce warmed emin emin olur. Uygulamanızı dağıtırken bu kapalı kalma süresini ortadan kaldırır. Trafik yeniden yönlendirmesi sorunsuzdur ve değiştirme işlemleri nedeniyle istek bırakılır. Yapılandırarak bu tüm iş akışını otomatik hale getirebilirsiniz [otomatik takas](#Auto-Swap) değiştirme öncesi doğrulama değil gerekli olmadığında.
* Bir değiştirme işleminden sonra yuvası ile önceden hazırlanmış uygulama artık önceki üretim uygulamasına sahiptir. Üretim yuvasına değişiklikler beklediğiniz gibi değilse, "son bilinen iyi sitenizi" hemen almak için aynı değiştirme gerçekleştirebileceğiniz geri.

Her App Service planı katmanı farklı sayıda dağıtım yuvalarını destekler. Dağıtım yuvalarını kullanarak ek ücret yoktur. Sayısının ölçeğini bulmak için uygulama katmanı destekleyen yuvası için bkz: [App Service limitleri](https://docs.microsoft.com/azure/azure-subscription-service-limits#app-service-limits). 

Farklı bir katmana uygulamanızı ölçeklendirmek için hedef katmanı, uygulamanızı zaten kullanıyor yuva sayısı desteklediğinden emin olun. Uygulamanızın beşten fazla yuva varsa, örneğin, aşağı için ölçeklendirilemez **standart** olduğundan, katman **standart** katman yalnızca beş dağıtım yuvalarını destekler. 

<a name="Add"></a>

## <a name="add-a-slot"></a>Bir yuva Ekle
Uygulama çalıştırmalıdır **standart**, **Premium**, veya **yalıtılmış** katmanı, birden çok dağıtım yuvasını etkinleştirmeniz sırayla.

1. İçinde [Azure portalında](https://portal.azure.com/), uygulamanızın açın [kaynak sayfası](../azure-resource-manager/manage-resources-portal.md#manage-resources).

2. Sol bölmede seçin **dağıtım yuvalarını** > **yuva Ekle**.
   
    ![Yeni bir dağıtım yuvası Ekle](./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png)
   
   > [!NOTE]
   > Uygulamayı hala kullanımda değilse **standart**, **Premium**, veya **yalıtılmış** katmanı, aşamalı yayımlamayı etkinleştirmeye desteklenen katmanları belirten bir ileti alırsınız. Bu noktada, tercih yapma seçeneğine sahip **yükseltme** gidin **ölçek** sekmesinde devam etmeden önce uygulama.
   > 

3. İçinde **bir yuva Ekle** iletişim kutusu, yuva bir ad verin ve başka bir dağıtım yuvası bir uygulama yapılandırmasından kopyalamak seçin. Seçin **Ekle** devam etmek için.
   
    ![Yapılandırma kaynağı](./media/web-sites-staged-publishing/ConfigurationSource1.png)
   
    Mevcut bir yuva yapılandırmasından kopyalayabilirsiniz. Uygulama ayarları, bağlantı dizeleri, dil framework sürümü, web yuvaları, HTTP sürümü ve platform bit genişliği kopyalanabilir ayarları içerir.

4. Yuva eklendikten sonra seçin **kapatmak** iletişim kutusunu kapatın. Yeni yuva artık gösterilir **dağıtım yuvalarını** sayfası. Varsayılan olarak, **trafik %** 0 yeni bir yuva için üretim yuvasına yönlendirilen tüm müşteri trafiği ile ayarlanır.

5. Bu yuvanın kaynak sayfasını açmak için yeni dağıtım yuvası seçin.
   
    ![Dağıtım yuvası başlığı](./media/web-sites-staged-publishing/StagingTitle.png)

    Herhangi bir App Service uygulaması gibi bir yönetim sayfası hazırlama yuvasına sahiptir. Yuvanın yapılandırmasını değiştirebilirsiniz. Yuva adını dağıtım yuvası görüntülediğiniz hatırlatmak için sayfanın en üstünde gösterilir.

6. Yuvanın kaynak sayfasında uygulama URL'sini seçin. Dağıtım yuvası kendi ana bilgisayar adına sahip ve aynı zamanda canlı bir uygulamadır. Dağıtım yuvası genel erişimi sınırlamak için bkz: [Azure App Service IP kısıtlamaları](app-service-ip-restrictions.md).

Başka bir yuvaya ayarlarından kopyalama olsa bile yeni dağıtım yuvası, içerik yok. Örneğin, [bu yuva Git ile yayımlama](app-service-deploy-local-git.md). Farklı bir depo dalı ya da farklı bir depoya yuvaya dağıtabilirsiniz. 

<a name="AboutConfiguration"></a>

## <a name="what-happens-during-a-swap"></a>Geçiş sırasında ne olur?

### <a name="swap-operation-steps"></a>Takas işlemi adımları

App Service, (genellikle gelen hazırlama yuvasını üretim yuvasına) iki Yuvalar, hedef yuvadaki kesinti yaşamak değil emin olmak için şunları yapar:

1. Aşağıdaki ayarlar (örneğin, üretim yuvası) hedef yuvadan kaynak yuvaya tüm örnekleri için geçerlidir: 
    - [Yuva özel](#which-settings-are-swapped) uygulama ayarlarının ve bağlantı dizeleri, varsa.
    - [Sürekli dağıtım](deploy-continuous-deployment.md) etkinleştirilirse ayarları.
    - [App Service kimlik doğrulaması](overview-authentication-authorization.md) etkinleştirilirse ayarları.
    
    Tüm durumlarda tüm örnekleri yeniden başlatmak için kaynak yuvasındaki tetikleyin. Sırasında [Önizleme ile değiştirme](#Multi-Phase), bu ilk aşama sonunu işaretler. Değiştirme işlemi duraklatıldı ve kaynak yuvaya hedef yuvanın ayarlarla düzgün çalıştığını doğrulayabilirsiniz.

1. Her örnek kendi yeniden tamamlamak için kaynak yuvasındaki bekleyin. Herhangi bir örneğine yeniden başlatılamazsa, değiştirme işlemi kaynak yuvaya yapılan tüm değişiklikler geri döner ve işlemi durdurur.

1. Varsa [yerel önbellek](overview-local-cache.md) olan etkin bir HTTP isteği için uygulama kökü ("/") kaynak yuvaya her bir örneği üzerinde yaparak yerel önbellek başlatma tetikleyin. Her örnek herhangi bir HTTP yanıt dönene kadar bekleyin. Yerel önbellek başlatma başka bir yeniden başlatma her örneğinde neden olur.

1. Varsa [otomatik takas](#Auto-Swap) ile etkin [özel Isınma](#Warm-up), tetikleyici [uygulama başlatma](https://docs.microsoft.com/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) yaparak uygulama kök ("/") kaynak her örneğinde bir HTTP isteği Yuva.

    Varsa `applicationInitialization` değil, belirtilen kaynak yuvaya her örneğinde uygulama kökü için bir HTTP isteği tetikler. 
    
    Herhangi bir HTTP yanıt örneği döndürürse, warmed dikkate almıştır.

1. Kaynak yuvaya tüm örneklerinde başarıyla warmed, iki iki yuvaları için yönlendirme kuralları geçerek Yuvalar. Bu adımdan sonra hedef yuvadaki (örneğin, üretim yuvası) kaynak yuvaya önceden warmed uygulama vardır.

1. Artık kaynak yuvaya öncesi takas uygulama daha önce hedef yuvada verildiğine göre tüm ayarlarını uygulama ve örnekleri yeniden başlatılıyor aynı işlemi gerçekleştirin.

Herhangi bir noktada değiştirme işlemi, kaynak yuva değiştirildi uygulama başlatma, tüm iş gerçekleşir. Kaynak yuvaya yüklenirken, hedef yuvadaki çevrimiçi kalır, burada değiştirme başarılı veya başarısız bakılmaksızın warmed ve hazır. Bir hazırlama yuvasını üretim yuvasıyla takas etmek için üretim yuvasına her zaman hedef yuva olduğundan emin olun. Bu şekilde değiştirme işlemi, üretim uygulamanızı etkilemez.

### <a name="which-settings-are-swapped"></a>Hangi ayarların değiştirilir?
Başka bir dağıtım yuvasından yapılandırma kopyaladığınızda, kopyalanan düzenlenebilir bir yapılandırmadır. Diğer yapılandırma öğeleri aynı yuva değiştirme işleminden sonra (Yuva belirli) kalır ancak bazı yapılandırma öğeleri takas arasında (değil yuva özel), içeriği izleyin. Aşağıdaki listelerde, yuvaları takas olduğunda değiştirme ayarları gösterilir.

**Olacağı ayarları**:

* Framework sürümü, 32/64-bit, web sockets gibi genel ayarlar
* Uygulama ayarları (yuvada şekilde yapılandırılabilir)
* Bağlantı dizeleri (yuvada şekilde yapılandırılabilir)
* İşleyici eşlemeleri
* İzleme ve tanılama ayarları
* Ortak Sertifikalar
* WebJobs içeriği
* Karma bağlantılar *
* Sanal ağ tümleştirmesi *
* Hizmet uç noktalarını *
* Azure Content Delivery Network'e *

Bir yıldız işareti (*) ile özellikleri yuvaya Yapışkan yapılacak planlanmaktadır. 

**Takas olmayan ayarlarını**:

* Uç noktalarını yayımlama
* Özel etki alanı adları
* Özel sertifikaları ve SSL bağlamaları
* Ölçek ayarları
* WebJobs zamanlayıcılar
* IP kısıtlamaları
* Her Zaman Açık
* Protokol ayarları (HTTPS, TLS sürümü, istemci sertifikaları)
* Tanılama günlüğü ayarları
* Çıkış noktaları arası kaynak paylaşımı (CORS)

<!-- VNET and hybrid connections not yet sticky to slot -->

(Takas değil) belirli yuvada için bir uygulama ayarı veya bağlantı dizesini yapılandırmak için Git **yapılandırma** o yuva için sayfa. Ekleme veya bir ayarı düzenleyin ve ardından **dağıtım yuvası ayarı**. Bu onay kutusunu işaretleyerek, App Service ayarı swappable olmadığını söyler. 

![Yuva ayarı](./media/web-sites-staged-publishing/SlotSetting.png)

<a name="Swap"></a>

## <a name="swap-two-slots"></a>İki Yuvalar 
Uygulamanızın üzerinde dağıtım yuvaları takas edebilirsiniz **dağıtım yuvalarını** sayfası ve **genel bakış** sayfası. Yuva değiştirme ile ilgili teknik ayrıntılar için bkz. [değiştirme sırasında neler](#AboutConfiguration).

> [!IMPORTANT]
> Bir uygulamadan bir dağıtım yuvasını üretime taşır önce üretim, hedef yuvadaki olduğundan ve kaynak yuvaya tüm ayarları tam olarak üretim ortamında olmasını istediğiniz şekilde yapılandırıldığından emin olun.
> 
> 

Dağıtım yuvalarını değiştirmek için:

1. Uygulamanızın Git **dağıtım yuvalarını** sayfasından seçim yapıp **takas**.
   
    ![Değiştirme düğmesi](./media/web-sites-staged-publishing/SwapButtonBar.png)

    **Takas** iletişim kutusu, değiştirilecek seçilen kaynak ve hedef yuvalarda ayarları gösterir.

2. İstenen seçin **kaynak** ve **hedef** yuvası. Genellikle, hedefi üretim yuvasıdır. Ayrıca, seçin **kaynak değişiklikleri** ve **hedef değişiklikleri** sekmeler ve yapılandırma değişikliklerini beklendiğini doğrulayın. İşlemi tamamladığınızda, size hemen seçerek Yuvalar **takas**.

    ![Değiştirmeyi Tamamla](./media/web-sites-staged-publishing/SwapImmediately.png)

    Takas gerçekten gerçekleşmeden önce hedef yuvadaki yeni ayarlar ile nasıl çalışır görmek için seçmeyin **takas**, ancak yönergeleri [Önizleme ile değiştirme](#Multi-Phase).

3. İşlemi tamamladığınızda, seçerek iletişim kutusunu kapatmak **kapatmak**.

Herhangi bir sorun varsa, bkz: [sorun giderme takasları](#troubleshoot-swaps).

<a name="Multi-Phase"></a>

### <a name="swap-with-preview-multi-phase-swap"></a>(Birden çok aşamalı değiştirme) önizleme ile değiştirme

> [!NOTE]
> Linux üzerinde web apps'te Önizleme ile değiştirme desteklenmez.

Hedef yuva olarak üretim ile takas önce uygulama değiştirildi ayarlarla çalıştığını doğrulayın. Kaynak yuvaya ayrıca görev açısından kritik uygulamalar için tercih edilir değiştirme tamamlanmadan önce warmed.

Önizleme ile değiştirme gerçekleştirdiğinizde, App Service aynı gerçekleştirir [takas işlemi](#AboutConfiguration) ancak, ilk adımdan sonra duraklatır. Ardından, takas tamamlamadan önce hazırlama yuvasına sonucuna doğrulayabilirsiniz. 

Değiştirmeyi iptal ederseniz, App Service kaynak yuva için yapılandırma öğeleri yeniden uygular.

Önizleme ile değiştirme için:

1. bağlantısındaki [dağıtım yuvaları takas](#Swap) ancak select **Önizleme ile değiştirme gerçekleştir**.

    ![Önizleme ile değiştirme](./media/web-sites-staged-publishing/SwapWithPreview.png)

    İletişim kutusu, kaynak yuvaya yapılandırmada 1. Aşama içinde nasıl değiştiğini ve kaynak ve hedef yuvanın Aşama 2 nasıl değiştiğini gösterir.

2. Takas başlatmaya hazır olduğunuzda seçin **Başlat takas**.

    1\. Aşama tamamlandıktan sonra iletişim kutusunda bildirim alırsınız. Değiştirme kaynağı yuvasındaki giderek Önizleme `https://<app_name>-<source-slot-name>.azurewebsites.net`. 

3. Bekleyen değiştirme işlemini tamamlamak hazır olduğunuzda **değiştirmeyi Tamamla** içinde **değiştirme eylemi** seçip **değiştirmeyi Tamamla**.

    Bekleyen bir değiştirme iptal etmek için işaretleyin **iptal takas** yerine.

4. İşlemi tamamladığınızda, seçerek iletişim kutusunu kapatmak **kapatmak**.

Herhangi bir sorun varsa, bkz: [sorun giderme takasları](#troubleshoot-swaps).

Birden çok aşamalı değiştirmeyi otomatik hale getirmek için bkz: [PowerShell ile otomatikleştirme](#automate-with-powershell).

<a name="Rollback"></a>

## <a name="roll-back-a-swap"></a>Bir takas geri alma
Hatalar (örneğin, üretim yuvası) hedef yuvada yuva değiştirme işleminden sonra meydana gelirse, yuva hemen aynı iki yuvayı değiştirerek öncesi takas durumlarına geri yükleyin.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Otomatik Takas yapılandırın

> [!NOTE]
> Linux üzerinde web apps'te otomatik değiştirme desteklenmez.

Otomatik değiştirme, uygulama müşterileri için sürekli olarak sıfır kaldırmanıza ve sıfır kapalı kalma süresi ile uygulamanızı dağıtmak için istediğiniz Azure DevOps senaryolarını kolaylaştırır. Otomatik Takas etkinleştirildiğinde bir yuvadan üretim ortamına, kod değişikliklerinizi bu yuva, App Service otomatik olarak her itme yapışınızda [uygulamayı üretime taşır](#swap-operation-steps) kaynak yuvaya warmed, sonra.

   > [!NOTE]
   > Otomatik Takas için üretim yuvasına yapılandırmadan önce bir üretim dışı hedef yuva üzerinde otomatik takas test göz önünde bulundurun.
   > 

Otomatik Takas yapılandırmak için:

1. Uygulamanızın kaynak sayfasına gidin. Seçin **dağıtım yuvalarını** >  *\<istenen kaynak yuvaya >*  > **yapılandırma**  >  **Genel ayarlar**.
   
2. İçin **etkin otomatik takas**seçin **üzerinde**. İstenen hedef yuvanın seçip **otomatik takas dağıtım yuvası**seçip **Kaydet** komut çubuğunda. 
   
    ![Yapılandırma otomatik takas seçimleri](./media/web-sites-staged-publishing/AutoSwap02.png)

3. Bir kod gönderimi kaynak yuvaya yürütün. Kısa bir süre sonra otomatik takas olur ve güncelleştirme sırasında hedef yuvanın URL'si yansıtılır.

Herhangi bir sorun varsa, bkz: [sorun giderme takasları](#troubleshoot-swaps).

<a name="Warm-up"></a>

## <a name="specify-custom-warm-up"></a>Özel Isınma belirtin
Kullanırken [otomatik takas](#Auto-Swap), bazı uygulamalar takas önce özel Isınma Eylemler gerekebilir. `applicationInitialization` Web.config yapılandırma öğesinde özel başlatma eylemleri belirtmenize olanak sağlar. [Takas işlemi](#AboutConfiguration) hedef yuvasıyla değiştirmeden önce tamamlamak bu özel Isınma bekler. İşte bir örnek web.config parça.

    <system.webServer>
        <applicationInitialization>
            <add initializationPage="/" hostName="[app hostname]" />
            <add initializationPage="/Home/About" hostName="[app hostname]" />
        </applicationInitialization>
    </system.webServer>

Özelleştirme hakkında daha fazla bilgi için `applicationInitialization` öğesi bkz [en yaygın dağıtım yuvası takas hataları ve nasıl düzeltileceğini](https://ruslany.net/2017/11/most-common-deployment-slot-swap-failures-and-how-to-fix-them/).

Isınma davranışla birini veya ikisini de özelleştirebilirsiniz [uygulama ayarları](configure-common.md):

- `WEBSITE_SWAP_WARMUP_PING_PATH`: Sitenizi sıcak için ping yolu. Bu uygulama ayarının değeri olarak bir eğik çizgi ile başlayan bir özel yol belirterek ekleyin. `/statuscheck` bunun bir örneğidir. Varsayılan değer `/` şeklindedir. 
- `WEBSITE_SWAP_WARMUP_PING_STATUSES`: Isınma işlemi için geçerli HTTP yanıt kodları. HTTP kodlarının virgülle ayrılmış bir listesi ile bu uygulama ayarı ekleyin. Bir örnek `200,202` . Döndürülen durum kodu listede yoksa, Isınma ve değiştirme işlemleri durdurulur. Varsayılan olarak, tüm yanıt kodları geçerlidir.

Herhangi bir sorun varsa, bkz: [sorun giderme takasları](#troubleshoot-swaps).

## <a name="monitor-a-swap"></a>Bir takas İzleyicisi

Varsa [takas işlemi](#AboutConfiguration) tamamlanması uzun zaman alıyor değiştirme işlemi hakkında bilgi alabilirsiniz [etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Portalında, sol bölmede, uygulamanızın kaynak sayfasında **etkinlik günlüğü**.

Bir değiştirme işlemi günlük sorgusu görünür `Swap Web App Slots`. Genişletin ve ayrıntıları görmek için hataları ve suboperations birini seçin.

## <a name="route-traffic"></a>Trafiği yönlendirme

Varsayılan olarak, tüm istemci isteklerini uygulamanın üretim URL'si (`http://<app_name>.azurewebsites.net`) üretim yuvasına yönlendirilir. Trafiğin bir kısmını başka bir yuvaya yönlendirebilirsiniz. Kullanıcı geri bildirimi için yeni bir güncelleştirme gerekiyor, ancak üretim yayınlamaya hazır değilseniz bu özellik yararlıdır.

### <a name="route-production-traffic-automatically"></a>Üretim trafiği otomatik olarak yönlendirin

Üretim trafiği otomatik olarak yönlendirmek için:

1. Uygulamanızın kaynak sayfasına gidin ve seçin **dağıtım yuvalarını**.

2. İçinde **trafik %** sütun yuvasının istediğiniz yönlendirmek istediğiniz yönlendirmek için toplam trafik miktarı temsil etmek için (0 ile 100 arasında) bir yüzde belirtin. **Kaydet**’i seçin.

    ![Trafik yüzdesi ayarlama](./media/web-sites-staged-publishing/RouteTraffic.png)

Ayar kaydedildikten sonra belirtilen istemcilerin yüzdesi üretim dışı yuvasına rastgele yönlendirilir. 

Bir istemci belirli bir yuvaya otomatik olarak yönlendirilir sonra onun "sabitlenmiş" bu yuvaya ömrü, istemci oturumun. İstemci tarayıcısına oturumunuz sabitlenmiş için bakarak hangi yuvası gördüğünüz `x-ms-routing-name` tanımlama bilgisi, HTTP üst bilgilerinde. "Hazırlama" yuvasını yönlendirilmiş bir istek tanımlama bilgisi varsa `x-ms-routing-name=staging`. Üretim yuvasına yönlendirilmiş bir istek tanımlama bilgisi varsa `x-ms-routing-name=self`.

### <a name="route-production-traffic-manually"></a>El ile üretim trafiği yönlendirin

App Service, otomatik trafik yönlendirme ek olarak, belirli bir yuvaya istekleri yönlendirebilir. Kullanıcılarınızın kabul etmek veya dışında beta uygulamanızı iyileştirilmiş istediğinizde bu kullanışlıdır. El ile üretim trafiği yönlendirmek için kullandığınız `x-ms-routing-name` sorgu parametresi.

Beta uygulamanızı dışında iyileştirilmiş kullanıcılara izin vermek için örneğin, bu bağlantıyı sayfanıza üzerinde koyabilirsiniz:

```HTML
<a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>
```

Dize `x-ms-routing-name=self` üretim yuvasına belirtir. İstemci tarayıcısı bağlantı eriştikten sonra üretim yuvasına yeniden yönlendirilir. Her bir sonraki istekte `x-ms-routing-name=self` sabitler üretim yuvasına oturum tanımlama bilgisi.

Beta uygulamanıza kabul et kullanıcılar izin vermek için aynı sorgu parametresi olmayan üretim yuvası adını ayarlayın. Bir örneği aşağıda verilmiştir:

```
<webappname>.azurewebsites.net/?x-ms-routing-name=staging
```

Varsayılan olarak, yeni bir yuva yönlendirme kuralını verilen `0%`gri gösterilmiştir. Açıkça ayarlarsanız bu değer `0%` (siyah metinde görünür), kullanıcılarınıza hazırlama yuvasına el ile kullanarak erişebilirsiniz `x-ms-routing-name` sorgu parametresi. Ancak, yönlendirme yüzde 0 olarak ayarlandığından bunlar yuvaya otomatik olarak yönlendirilmesini olmaz. Bu, burada, "hazırlama, ortak yuvadan yuvası değişiklikleri test etmek dahili ekipler verirken gizleyebilirsiniz" Gelişmiş bir senaryodur.

<a name="Delete"></a>

## <a name="delete-a-slot"></a>Bir yuvasını Sil

Uygulamanızın kaynak sayfasına gidin. Seçin **dağıtım yuvalarını** >  *\<silmek için yuva >*  > **genel bakış**. Seçin **Sil** komut çubuğunda.  

![Dağıtım yuvasını Sil](./media/web-sites-staged-publishing/DeleteStagingSiteButton.png)

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="automate-with-powershell"></a>PowerShell ile otomatikleştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure PowerShell, Azure App Service'te dağıtım yuvalarını yönetmek için destek dahil olmak üzere Windows PowerShell aracılığıyla Azure yönetmek için cmdlet'ler sağlayan bir modüldür.

Yükleme ve yapılandırma Azure PowerShell ve Azure PowerShell, Azure aboneliğiniz ile kimlik doğrulaması için bilgi [nasıl Microsoft Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).  

---
### <a name="create-a-web-app"></a>Web uygulaması oluşturma
```powershell
New-AzWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

---
### <a name="create-a-slot"></a>Yuva oluşturun
```powershell
New-AzWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

---
### <a name="initiate-a-swap-with-a-preview-multi-phase-swap-and-apply-destination-slot-configuration-to-the-source-slot"></a>Bir önizleme (birden çok aşamalı değiştirme) ile bir değiştirme başlatın ve kaynak yuvaya hedef yuva yapılandırmasını Uygula
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

---
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-the-source-slot-configuration"></a>Bekleyen bir değiştirme (gözden geçirme ile değiştirme) iptal edin ve geri yükleme kaynağı yuvası yapılandırması
```powershell
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

---
### <a name="swap-deployment-slots"></a>Dağıtım yuvalarını değiştirme
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

### <a name="monitor-swap-events-in-the-activity-log"></a>Etkinlik günlüğünde İzleyici değiştirme olayları
```powershell
Get-AzLog -ResourceGroup [resource group name] -StartTime 2018-03-07 -Caller SlotSwapJobProcessor  
```

---
### <a name="delete-a-slot"></a>Bir yuvasını Sil
```powershell
Remove-AzResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

---
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="automate-with-the-cli"></a>CLI ile otomatikleştirme

İçin [Azure CLI](https://github.com/Azure/azure-cli) bkz: dağıtım yuvaları için komutları [az webapp deployment slot](/cli/azure/webapp/deployment/slot).

## <a name="troubleshoot-swaps"></a>Takasları sorunlarını giderme

Herhangi bir hata sırasında oluşursa bir [yuvası takas](#AboutConfiguration), günlüğe kaydedilir *D:\home\LogFiles\eventlog.xml*. Ayrıca, uygulamaya özgü hata günlüğüne de kaydedilir.

Bazı yaygın takas hatalar şunlardır:

- Uygulama kökü için bir HTTP isteği zaman aşımına uğradı. Değiştirme işlemi 5 kata kadar her HTTP isteği ve yeniden denemeler 90 saniye bekler. Tüm yeniden deneme aşımına değiştirme işlemi durdurulur.

- Uygulama içeriği için yerel önbelleği belirtilen yerel disk kotasını aştığında yerel önbellek başlatma başarısız olabilir. Daha fazla bilgi için [yerel önbelleğe genel bakış](overview-local-cache.md).

- Sırasında [özel Isınma](#Warm-up), HTTP istekleri yapılan dahili olarak (dış URL yoluyla olmadan). Belirli URL yeniden yazma kuralları ile devredebilirsiniz *Web.config*. Örneğin, Yönlendirme etki alanı adlarını veya HTTPS zorlama için kuralları, uygulama kodu ulaşmasını Isınma istekleri engelleyebilirsiniz. Bu sorunu geçici olarak çözmek için aşağıdaki iki koşul ekleyerek, yeniden yazma kuralları değiştirin:

    ```xml
    <conditions>
      <add input="{WARMUP_REQUEST}" pattern="1" negate="true" />
      <add input="{REMOTE_ADDR}" pattern="^100?\." negate="true" />
      ...
    </conditions>
    ```
- Özel bir Isınma, URL yeniden yazma kuralları hala HTTP isteklerini engelleyebilirsiniz. Bu sorunu geçici olarak çözmek için aşağıdaki koşul ekleyerek, yeniden yazma kuralları değiştirin:

    ```xml
    <conditions>
      <add input="{REMOTE_ADDR}" pattern="^100?\." negate="true" />
      ...
    </conditions>
    ```
- Bazı [IP kısıtlama kuralları](app-service-ip-restrictions.md) değiştirme işlemi, uygulamanız için HTTP istekleri göndermesini engelleyebilir. IPv4 adres aralıkları ile başlayan `10.` ve `100.` dağıtımınıza dahili. Bunların uygulamanıza bağlanmasına izin vermelidir.

## <a name="next-steps"></a>Sonraki adımlar
[Üretim dışı yuvaları erişimi engelle](app-service-ip-restrictions.md)
