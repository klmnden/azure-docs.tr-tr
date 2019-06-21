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
ms.openlocfilehash: cbf287aef2c1792033a198070da605014a7b6281
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272847"
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Azure App Service ortamlarında hazırlık ayarlama
<a name="Overview"></a>

Web uygulaması, Linux ve mobil arka uç API uygulamasına web uygulamasına dağıtırken [App Service](https://go.microsoft.com/fwlink/?LinkId=529714), varsayılan üretim yuvasına yerine ayrı bir dağıtım yuvası çalıştırıldığında dağıtabileceğiniz **standart**, **Premium**, veya **yalıtılmış** App Service planı katmanı. Dağıtım yuvaları kendi ana bilgisayar adları olan Canlı uygulamalardır. Uygulama içeriği ve yapılandırma öğelerinin, üretim yuvası dahil iki dağıtım yuvası arasında değişiklik yapılabilir. Uygulamanızı üretim dışı bir yuvaya dağıtmak için aşağıdaki faydaları vardır:

* Üretim yuvasıyla değiştirmeden önce hazırlık dağıtım yuvasındaki uygulama değişiklikleri doğrulayabilirsiniz.
* Bir uygulamayı ilk kez bir yuvasına dağıtma ve üretime geçirmeden yuvası tüm örneklerini üretime takas önce warmed emin emin olur. Uygulamanızı dağıtırken bu kapalı kalma süresini ortadan kaldırır. Trafik yeniden yönlendirmesi sorunsuzdur ve değiştirme işlemleri nedeniyle istek bırakılır. Yapılandırarak bu iş akışının tamamı otomatikleştirilebilir [otomatik değiştirme](#Auto-Swap) değiştirme öncesi doğrulama değil gerekli olmadığında.
* Bir değiştirme işleminden sonra yuvası ile önceden hazırlanmış uygulama artık önceki üretim uygulamasına sahiptir. Üretim yuvasına değişiklikler beklediğiniz gibi değilse, "son bilinen iyi sitenizi" hemen almak için aynı değiştirme gerçekleştirebileceğiniz geri.

Her App Service planı katmanı farklı sayıda dağıtım yuvalarını destekler ve dağıtım yuvalarını kullanarak ek ücret yoktur. Sayısının ölçeğini bulmak için uygulama katmanı destekleyen yuvası için bkz: [App Service limitleri](https://docs.microsoft.com/azure/azure-subscription-service-limits#app-service-limits). Farklı bir katmana uygulamanızı ölçeklendirmek için hedef katmana uygulamanızı zaten kullanıyor yuva sayısı desteklemesi gerekir. Uygulamanızın beşten fazla yuva varsa, örneğin, aşağı için ölçeklendirilemez **standart** olduğundan, katman **standart** katman yalnızca beş dağıtım yuvalarını destekler. 

<a name="Add"></a>

## <a name="add-slot"></a>Yuva Ekle
Uygulama çalıştırmalıdır **standart**, **Premium**, veya **yalıtılmış** katmanı, birden çok dağıtım yuvasını etkinleştirmeniz sırayla.

1. İçinde [Azure portalında](https://portal.azure.com/), uygulamanızın açın [kaynak sayfası](../azure-resource-manager/manage-resources-portal.md#manage-resources).

2. Sol gezinti bölmesinde seçin **dağıtım yuvalarını** seçeneğini belirleyin, ardından tıklayın **yuva Ekle**.
   
    ![Yeni bir dağıtım yuvası Ekle](./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png)
   
   > [!NOTE]
   > Uygulamayı hala kullanımda değilse **standart**, **Premium**, veya **yalıtılmış** katmanı, aşamalı yayımlamayı etkinleştirmeye desteklenen katmanları belirten bir ileti alırsınız. Bu noktada, tercih yapma seçeneğine sahip **yükseltme** gidin **ölçek** sekmesinde devam etmeden önce uygulama.
   > 

3. İçinde **bir yuva Ekle** iletişim kutusunda, yuva bir ad verin ve mevcut olan başka bir dağıtım yuvası uygulama yapılandırmasından kopyalamak seçin. Tıklayın **Ekle** devam etmek için.
   
    ![Yapılandırma kaynağı](./media/web-sites-staged-publishing/ConfigurationSource1.png)
   
    Tüm mevcut yuvasından yapılandırma kopyalayabilirsiniz. Uygulama ayarları, bağlantı dizeleri, dil framework sürümü, web yuvaları, HTTP sürümü ve platform bit genişliği kopyalanabilir ayarları içerir.

4. Yuva eklendikten sonra tıklayın **kapatmak** iletişim kutusunu kapatmak için. Yeni yuva artık gösterilen **dağıtım yuvalarını** sayfası. Varsayılan olarak, **trafik %** 0 yeni bir yuva için üretim yuvasına yönlendirilen tüm müşteri trafiği ile ayarlanır.

5. Bu yuvanın kaynak sayfasını açmak için yeni dağıtım yuvasına tıklayın.
   
    ![Dağıtım yuvası başlığı](./media/web-sites-staged-publishing/StagingTitle.png)

    Herhangi bir App Service uygulaması gibi bir yönetim sayfası hazırlama yuvasına sahiptir. Yuvanın yapılandırmasını değiştirebilirsiniz. Yuva adını dağıtım yuvası görüntülediğiniz hatırlatmak için sayfanın en üstünde gösterilir.

6. Uygulama URL'sini yuvanın kaynak sayfasında tıklayın. Dağıtım yuvası, kendi ana bilgisayar adı var ve ayrıca canlı bir uygulamadır. Dağıtım yuvası genel erişimi sınırlamak için bkz: [Azure App Service IP kısıtlamaları](app-service-ip-restrictions.md).

Başka bir yuvaya ayarlarından kopyalama olsa bile yeni dağıtım yuvası, içerik yok. Örneğin, [bu yuva git ile yayımlama](app-service-deploy-local-git.md). Farklı bir depo dalı ya da farklı bir depoya yuvaya dağıtabilirsiniz. 

<a name="AboutConfiguration"></a>

## <a name="what-happens-during-swap"></a>Geçiş sırasında ne olur?

[İşlem adımı takas](#swap-operation-steps)
[hangi ayarların takas?](#which-settings-are-swapped)

### <a name="swap-operation-steps"></a>Takas işlemi adımları

App Service, (genellikle gelen hazırlama yuvasını üretim yuvasına) iki Yuvalar, hedef yuvadaki kesinti yaşamak değil emin olmak için şunları yapar:

1. Aşağıdaki ayarlar (örneğin, üretim yuvası) hedef yuvadan kaynak yuvaya tüm örnekleri için geçerlidir: 
    - [Yuva özel](#which-settings-are-swapped) uygulama ayarlarının ve bağlantı dizeleri, varsa.
    - [Sürekli dağıtım](deploy-continuous-deployment.md) etkinleştirilirse ayarları.
    - [App Service kimlik doğrulaması](overview-authentication-authorization.md) etkinleştirilirse ayarları.
    Yukarıdaki durumların herhangi birinde tüm örnekleri yeniden başlatmak için kaynak yuvasındaki tetikler. Sırasında [Önizleme ile değiştirme](#Multi-Phase), bu yeri değiştirme işlemi duraklatıldı ve kaynak yuvaya hedef yuvanın ayarlarla düzgün çalıştığını doğrulamak için ilk aşama sonunu işaretler.

1. Her örnek kendi yeniden tamamlamak için kaynak yuvasındaki bekleyin. Herhangi bir örneğine yeniden başlatılamazsa, değiştirme işlemi kaynak yuvaya yapılan tüm değişiklikler geri döner ve işlemi durdurur.

1. Varsa [yerel önbellek](overview-local-cache.md) olan etkin bir HTTP isteği için uygulama kökü ("/") kaynak yuvaya her bir örneği üzerinde ve herhangi bir HTTP yanıt her örneği dönene kadar bekleyin yaparak yerel önbellek başlatma tetikleyin. Yerel önbellek başlatma başka bir yeniden başlatma her örneğinde neden olur.

1. Varsa [otomatik takas](#Auto-Swap) ile etkin [özel Isınma](#custom-warm-up), tetikleyici [uygulama başlatma](https://docs.microsoft.com/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) yaparak uygulama kök ("/") kaynak her örneğinde bir HTTP isteği Yuva. Herhangi bir HTTP yanıt örneği döndürürse, warmed dikkate almıştır.

    Hayır ise `applicationInitialization` olduğundan, belirtilen kaynak yuvaya her örneğinde uygulama kökü için bir HTTP isteği tetikler. Herhangi bir HTTP yanıt örneği döndürürse, warmed dikkate almıştır.

1. Kaynak yuvaya tüm örneklerinde başarıyla warmed, iki iki yuvaları için yönlendirme kuralları geçerek Yuvalar. Bu adımdan sonra hedef yuvadaki (örneğin, üretim yuvası) kaynak yuvaya önceden warmed uygulama vardır.

1. Artık kaynak yuvaya öncesi takas uygulama daha önce hedef yuvada verildiğine göre tüm ayarlarını uygulama ve örnekleri yeniden başlatılıyor aynı işlemi gerçekleştirin.

Herhangi bir noktada değiştirme işlemi, kaynak yuva değiştirildi uygulama başlatma, tüm işler gerçekleştirilir. Kaynak yuvaya yüklenirken, hedef yuvadaki çevrimiçi kalır hazır ve warmed yeri değiştirme başarılı veya başarısız yukarı, ne olursa olsun. Bir hazırlama yuvasını üretim yuvasıyla takas etmek için üretim yuvasına her zaman hedef yuva olduğundan emin olun. Bu şekilde, üretim uygulamanızın değiştirme işlemi tarafından etkilenmez.

### <a name="which-settings-are-swapped"></a>Hangi ayarların değiştirilir?
Başka bir dağıtım yuvasından yapılandırma kopyaladığınızda, kopyalanan düzenlenebilir bir yapılandırmadır. Ayrıca, diğer yapılandırma öğeleri aynı yuva değiştirme işleminden sonra (Yuva belirli) kalır ancak bazı yapılandırma öğeleri (değil yuva belirli) arasında bir takas içeriği izleyin. Aşağıdaki listelerde, yuvaları takas olduğunda değiştirme ayarları gösterilir.

**Olacağı ayarları**:

* Framework sürümü, 32/64-bit, Web sockets gibi genel ayarları-
* Uygulama ayarları (yuvada şekilde yapılandırılabilir)
* Bağlantı dizeleri (yuvada şekilde yapılandırılabilir)
* İşleyici eşlemeleri
* İzleme ve tanılama ayarları
* Ortak Sertifikalar
* WebJobs içeriği
* Karma bağlantılar *
* VNet tümleştirmesi *
* Hizmet uç noktalarını *
* Azure CDN *

Özellikleri ile işaretlenmiş bir * yuvaya Yapışkan yapılacak planlanmaktadır. 

**Takas olmayan ayarlarını**:

* Uç noktalarını yayımlama
* Özel etki alanı adları
* Özel sertifikaları ve SSL bağlamaları
* Ölçek ayarları
* WebJobs zamanlayıcılar
* IP kısıtlamaları
* Her Zaman Açık
* Protokol ayarları (HTTP**S**, TLS sürümü, istemci sertifikaları)
* Tanılama günlüğü ayarları
* CORS

<!-- VNET and hybrid connections not yet sticky to slot -->

(Takas değil) belirli yuvada için bir uygulama ayarı veya bağlantı dizesini yapılandırmak için gidin **yapılandırma** sayfasında bu yuva için eklemek veya bir ayarı düzenleyin ve ardından seçin **dağıtım yuvası ayarı**kutusu. Bu onay kutusunu seçerek, App Service ayarı swappable olmadığını söyler. 

![Yuva ayarı](./media/web-sites-staged-publishing/SlotSetting.png)

<a name="Swap"></a>

## <a name="swap-two-slots"></a>İki Yuvalar 
Uygulamanızın dağıtım yuvaları takas edebilirsiniz **dağıtım yuvalarını** sayfası ve **genel bakış** sayfası. Yuva değiştirme ile ilgili teknik ayrıntılar için bkz. [değiştirme sırasında ne olur?](#what-happens-during-swap)

> [!IMPORTANT]
> Bir uygulamadan bir dağıtım yuvasını üretime taşır önce üretim, hedef yuvadaki olduğundan ve tüm ayarlarını kaynak yuvaya üretimde sağlamak tam olarak istediğiniz şekilde yapılandırıldığından emin olun.
> 
> 

Dağıtım yuvalarını değiştirmek için aşağıdaki adımları izleyin:

1. Git, uygulamanızın **dağıtım yuvalarını** sayfasında ve tıklayın **takas**.
   
    ![Değiştirme düğmesi](./media/web-sites-staged-publishing/SwapButtonBar.png)

    **Takas** iletişim ayarları değiştirilecek seçilen kaynak ve hedef yuvalarda gösterir.

2. İstenen seçin **kaynak** ve **hedef** yuvası. Genellikle, hedefi üretim yuvasıdır. Ayrıca, **kaynak değişiklikleri** ve **hedef değişiklikleri** sekmeler ve yapılandırma değişikliklerini beklendiğini doğrulayın. İşiniz bittiğinde, hemen tıklayarak Yuvalar **takas**.

    ![Değiştirmeyi Tamamla](./media/web-sites-staged-publishing/SwapImmediately.png)

    Takas gerçekten gerçekleşmeden önce hedef yuvadaki yeni ayarlar ile nasıl çalışır görmek için tıklamayın **takas**, ancak yönergeleri [Önizleme ile değiştirme](#Multi-Phase).

3. İşlemi tamamladığınızda, tıklayarak iletişim kutusunu kapatmak **kapatmak**.

Bir sorun yaşarsanız bkz [sorun giderme takasları](#troubleshoot-swaps).

<a name="Multi-Phase"></a>

### <a name="swap-with-preview-multi-phase-swap"></a>(Birden çok aşamalı değiştirme) önizleme ile değiştirme

> [!NOTE]
> Linux üzerinde web apps'te Önizleme ile değiştirme desteklenmez.

Takas gerçekleşmeden önce hedef yuva olarak üretime geçirmeden önce takas ayarlarla uygulama çalıştırmaları doğrulayın. Kaynak yuvaya de ayrıca görev açısından kritik uygulamalar için tercih edilir değiştirme tamamlanmadan önce warmed.

Önizleme ile değiştirme gerçekleştirdiğinizde, App Service aynı gerçekleştirir [takas işlemi](#what-happens-during-swap) ancak, ilk adımdan sonra duraklatır. Ardından, takas tamamlamadan önce hazırlama yuvasına sonucuna doğrulayabilirsiniz. 

Değiştirmeyi iptal ederseniz, App Service kaynak yuvaya kaynak yuvaya yapılandırma öğelerini yeniden uygular.

Önizleme ile değiştirme için aşağıdaki adımları izleyin.

1. bağlantısındaki [dağıtım yuvaları takas](#Swap) ancak select **Önizleme ile değiştirme gerçekleştir**.

    ![Önizleme ile değiştirme](./media/web-sites-staged-publishing/SwapWithPreview.png)

    İletişim kutusu, kaynak yuvaya yapılandırmada 1. Aşama içinde nasıl değiştiğini ve kaynak ve hedef yuvanın Aşama 2 nasıl değiştiğini gösterir.

2. Hazır olduğunuzda takas başlatmak tıklatın **Başlat takas**.

    1\. Aşama tamamlandıktan sonra iletişim kutusunda bildirim alırsınız. Değiştirme kaynağı yuvasındaki giderek Önizleme `https://<app_name>-<source-slot-name>.azurewebsites.net`. 

3. Hazır bekleyen değiştirme işlemini tamamlamak bittiğinde **değiştirmeyi Tamamla** içinde **değiştirme eylemi** tıklatıp **değiştirmeyi Tamamla**.

    Bekleyen bir değiştirme iptal etmek için işaretleyin **iptal takas** yerine tıklatıp **iptal takas**.

4. İşlemi tamamladığınızda, tıklayarak iletişim kutusunu kapatmak **kapatmak**.

Bir sorun yaşarsanız bkz [sorun giderme takasları](#troubleshoot-swaps).

Birden çok aşamalı değiştirme otomatikleştirmek için PowerShell ile otomatikleştirme bakın.

<a name="Rollback"></a>

## <a name="roll-back-swap"></a>Değiştirme geri alma
Hatalar (örneğin, üretim yuvası) hedef yuvada yuva değiştirme işleminden sonra meydana gelirse, yuva hemen aynı iki yuvayı değiştirerek öncesi takas durumlarına geri yükleyin.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Otomatik Takas yapılandırın

> [!NOTE]
> Linux üzerinde web apps'te otomatik değiştirme desteklenmez.

Otomatik Takas için uygulamanın son müşterilere sürekli olarak sıfır hazırlıksız başlatma ve sıfır kapalı kalma süresi ile uygulamanızı dağıtmak için istediğiniz DevOps senaryolarını kolaylaştırır. Otomatik Takas etkinleştirildiğinde bir yuvadan üretim ortamına, kod değişikliklerinizi bu yuva, App Service otomatik olarak her itme yapışınızda [uygulamayı üretime taşır](#swap-operation-steps) kaynak yuvaya warmed, sonra.

   > [!NOTE]
   > Otomatik Takas için üretim yuvasına yapılandırmadan önce bir üretim dışı hedef yuva üzerinde otomatik takas ilk test göz önünde bulundurun.
   > 

Otomatik Takas yapılandırmak için aşağıdaki adımları izleyin:

1. Uygulamanızın kaynak sayfasına gidin. Seçin **dağıtım yuvalarını** >  *\<istenen kaynak yuvaya >*  > **yapılandırma**  >  **Genel ayarlar**.
   
2. İçinde **etkin otomatik takas**seçin **üzerinde**, istenen hedef yuvada seçip **otomatik takas dağıtım yuvası**, tıklatıp **Kaydet** içinde Komut çubuğu. 
   
    ![](./media/web-sites-staged-publishing/AutoSwap02.png)

3. Bir kod gönderimi kaynak yuvaya yürütün. Kısa bir süre sonra otomatik takas olur ve güncelleştirme sırasında hedef yuvanın URL'si yansıtılır.

Bir sorun yaşarsanız bkz [sorun giderme takasları](#troubleshoot-swaps).

<a name="Warm-up"></a>

## <a name="custom-warm-up"></a>Özel Isınma
Kullanırken [Auto-Swap](#Auto-Swap), bazı uygulamalar takas önce özel Isınma Eylemler gerekebilir. `applicationInitialization` Web.config yapılandırma öğesinde gerçekleştirilecek özel başlatma eylemleri belirtmenize olanak sağlar. [Takas işlemi](#what-happens-during-swap) hedef yuvasıyla değiştirmeden önce tamamlamak bu özel Isınma bekler. İşte bir örnek web.config parça.

    <system.webServer>
        <applicationInitialization>
            <add initializationPage="/" hostName="[app hostname]" />
            <add initializationPage="/Home/About" hostName="[app hostname]" />
        </applicationInitialization>
    </system.webServer>

Özelleştirme hakkında daha fazla bilgi için `applicationInitialization` öğesi bkz [en yaygın dağıtım yuvası takas hataları ve nasıl düzeltileceğini](https://ruslany.net/2017/11/most-common-deployment-slot-swap-failures-and-how-to-fix-them/).

Bir veya daha fazlasını Isınma davranışını özelleştirebilirsiniz [uygulama ayarları](configure-common.md):

- `WEBSITE_SWAP_WARMUP_PING_PATH`: Isınma için sitenizin ping yolu. Bu uygulama ayarının değeri olarak bir eğik çizgi ile başlayan bir özel yol belirterek ekleyin. Örneğin, `/statuscheck`. Varsayılan değer `/` şeklindedir. 
- `WEBSITE_SWAP_WARMUP_PING_STATUSES`: Isınma işlemi için geçerli HTTP yanıt kodları. HTTP kodlarının virgülle ayrılmış bir listesi ile bu uygulama ayarı ekleyin. Örneğin: `200,202` . Döndürülen durum kodu listede değilse Isınma ve değiştirme işlemlerini durdurulur. Varsayılan olarak, tüm yanıt kodları geçerlidir.

Bir sorun yaşarsanız bkz [sorun giderme takasları](#troubleshoot-swaps).

## <a name="monitor-swap"></a>İzleyici değiştirme

Varsa [takas işlemi](#what-happens-during-swap) tamamlanması uzun zaman alıyor değiştirme işlemi hakkında bilgi alabilirsiniz [etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Portalında, sol taraftaki gezinti bölmesinde, uygulamanızın kaynak sayfasını seçin **etkinlik günlüğü**.

Bir değiştirme işlemi günlük sorgusu görünür `Swap Web App Slots`. Genişletin ve ayrıntıları görmek için hataları ve suboperations birini seçin.

## <a name="route-traffic"></a>Trafiği yönlendirme

Varsayılan olarak, tüm istemci isteklerini uygulamanın üretim URL'si (`http://<app_name>.azurewebsites.net`) üretim yuvasına yönlendirilir. Trafiğin bir kısmını başka bir yuvaya yönlendirebilirsiniz. Kullanıcı geri bildirimi için yeni bir güncelleştirme gerekiyor, ancak üretim yayınlamaya hazır değilseniz bu özellik yararlıdır.

### <a name="route-production-traffic-automatically"></a>Üretim trafiği otomatik olarak yönlendirin

Üretim trafiği otomatik olarak yönlendirmek için bu adımları izleyin:

1. Uygulamanızın kaynak sayfasına gidin ve seçin **dağıtım yuvalarını**.

2. İçinde **trafik %** sütun yuvasının istediğiniz yönlendirmek istediğiniz yönlendirmek için toplam trafik miktarı temsil etmek için (0 ile 100 arasında) bir yüzde belirtin. **Kaydet**’e tıklayın.

    ![](./media/web-sites-staged-publishing/RouteTraffic.png)

Ayar kaydedildikten sonra belirtilen istemcilerin yüzdesi üretim dışı yuvasına rastgele yönlendirilir. 

Bir istemci belirli bir yuvaya otomatik olarak yönlendirilir sonra onun "sabitlenmiş" bu yuvaya ömrü, istemci oturumun. İstemci tarayıcısına oturumunuz sabitlenmiş için bakarak hangi yuvası gördüğünüz `x-ms-routing-name` tanımlama bilgisi, HTTP üst bilgilerinde. "Hazırlama" yuvasını yönlendirilmiş bir istek tanımlama bilgisi varsa `x-ms-routing-name=staging`. Üretim yuvasına yönlendirilmiş bir istek tanımlama bilgisi varsa `x-ms-routing-name=self`.

### <a name="route-production-traffic-manually"></a>El ile üretim trafiği yönlendirin

App Service, otomatik trafik yönlendirme ek olarak, belirli bir yuvaya istekleri yönlendirebilir. Kullanıcılarınız için iyileştirilmiş içine veya çevirme beta uygulamanızın olmasını istediğinizde bu kullanışlıdır. El ile üretim trafiği yönlendirmek için kullandığınız `x-ms-routing-name` sorgu parametresi.

Beta uygulamanızı dışında iyileştirilmiş kullanıcılara izin vermek için örneğin, bu bağlantıyı web sayfanızın koyabilirsiniz:

```HTML
<a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>
```

Dize `x-ms-routing-name=self` üretim yuvasına belirtir. İstemci tarayıcısı bağlantı erişen sonra yalnızca üretim yuvasına yönlendirilir ancak sonraki her istekte `x-ms-routing-name=self` sabitler üretim yuvasına oturum tanımlama bilgisi.

Beta uygulamanıza kabul et kullanıcılar izin vermek için aynı sorgu parametresi örneğin üretim dışı yuvası adını ayarlayın:

```
<webappname>.azurewebsites.net/?x-ms-routing-name=staging
```

Varsayılan olarak, yeni bir yuva yönlendirme kuralını verilen `0%`gri gösterilmiştir. Bu değeri açıkça ayarlayarak `0%` (siyah metinde görünür), kullanıcılarınıza hazırlama yuvasına el ile kullanarak erişebilirsiniz `x-ms-routing-name` sorgu parametresi, ancak değil yönlendirilecek yuvaya otomatik olarak Yönlendirme yüzde 0 olarak ayarlanmış olduğundan. Bu, burada, "hazırlama, ortak yuvadan yuvası değişiklikleri test etmek dahili ekipler verirken gizleyebilirsiniz" Gelişmiş bir senaryodur.

<a name="Delete"></a>

## <a name="delete-slot"></a>Yuvasını Sil

Uygulamanızın kaynak sayfasına gidin. Seçin **dağıtım yuvalarını** >  *\<silmek için yuva >*  > **genel bakış**. Tıklayın **Sil** komut çubuğunda.  

![Dağıtım yuvasını Sil](./media/web-sites-staged-publishing/DeleteStagingSiteButton.png)

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="automate-with-powershell"></a>PowerShell ile otomatikleştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure PowerShell, Azure App Service'te dağıtım yuvalarını yönetmek için destek dahil olmak üzere Windows PowerShell aracılığıyla Azure yönetmek için cmdlet'ler sağlayan bir modüldür.

Yükleme ve yapılandırma Azure PowerShell ve Azure PowerShell, Azure aboneliğiniz ile kimlik doğrulaması için bilgi [nasıl Microsoft Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).  

---
### <a name="create-web-app"></a>Web uygulaması oluşturma
```powershell
New-AzWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

---
### <a name="create-slot"></a>Yuva oluşturun
```powershell
New-AzWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

---
### <a name="initiate-swap-with-preview-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a>(Birden çok aşamalı değiştirme) önizleme ile değiştirme başlatmak ve kaynak yuva için hedef yuva yapılandırmasını Uygula
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

---
### <a name="cancel-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Swap (gözden geçirme ile değiştirme) bekleyen iptal etme ve kaynak yuva yapılandırması geri yükleme
```powershell
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

---
### <a name="swap-deployment-slots"></a>Dağıtım yuvalarını değiştirme
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

### <a name="monitor-swap-events-in-the-activity-log"></a>Etkinlik günlüğü değiştirme olay izleme
```powershell
Get-AzLog -ResourceGroup [resource group name] -StartTime 2018-03-07 -Caller SlotSwapJobProcessor  
```

---
### <a name="delete-slot"></a>Yuvasını Sil
```powershell
Remove-AzResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

---
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="automate-with-cli"></a>CLI ile otomatikleştirme

İçin [Azure CLI](https://github.com/Azure/azure-cli) bkz: dağıtım yuvaları için komutları [az webapp deployment slot](/cli/azure/webapp/deployment/slot).

## <a name="troubleshoot-swaps"></a>Takasları sorunlarını giderme

Herhangi bir hata sırasında oluşursa bir [yuvası takas](#what-happens-during-swap), günlüğe kaydedilir *D:\home\LogFiles\eventlog.xml*, uygulamaya özgü hata günlüğünün yanı sıra.

Bazı yaygın takas hatalar şunlardır:

- Uygulama kökü için bir HTTP isteği zaman aşımına uğradı. Değiştirme işlemi 5 kata kadar her HTTP isteği ve yeniden denemeler 90 saniye bekler. Tüm yeniden deneme aşımına uğrarsa, değiştirme işlemi iptal edildi.

- Uygulama içeriği için yerel önbelleği belirtilen yerel disk kotasını aştığında yerel önbellek başlatma başarısız olabilir. Daha fazla bilgi için [yerel önbelleğe genel bakış](overview-local-cache.md).

- Sırasında [özel Isınma](#custom-warm-up), dahili olarak (dış URL yoluyla olmadan) HTTP isteklerinin yapılma ve belirli URL ile başarısız kuralları yazabilirsiniz *Web.config*. Örneğin, Yönlendirme etki alanı adlarını veya HTTPS zorlama için kuralları, uygulama kodunu hiç ulaşmasını Isınma istekleri engelleyebilirsiniz. Bu sorunu geçici olarak çözmek için aşağıdaki iki koşul ekleyerek, yeniden yazma kuralları değiştirin:

    ```xml
    <conditions>
      <add input="{WARMUP_REQUEST}" pattern="1" negate="true" />
      <add input="{REMOTE_ADDR}" pattern="^100?\." negate="true" />
      ...
    </conditions>
    ```
- Özel Isınma, HTTP isteklerini hala URL yeniden yazma kuralları tarafından tutulabilir. Bu sorunu geçici olarak çözmek için aşağıdaki koşul ekleyerek, yeniden yazma kuralları değiştirin:

    ```xml
    <conditions>
      <add input="{REMOTE_ADDR}" pattern="^100?\." negate="true" />
      ...
    </conditions>
    ```
- Bazı [IP kısıtlama kuralları](app-service-ip-restrictions.md) değiştirme işlemi, uygulamanız için HTTP istekleri göndermesini engelleyebilir. IPv4 adres aralıkları ile başlayan `10.` ve `100.` dağıtımınıza iç ve uygulamanıza bağlanmasına izin.

## <a name="next-steps"></a>Sonraki adımlar
[Üretim dışı yuvaları erişimi engelle](app-service-ip-restrictions.md)
