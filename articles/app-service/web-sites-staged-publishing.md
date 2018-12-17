---
title: Hazırlık web uygulamaları - Azure App Service ortamları ayarlama | Microsoft Docs
description: Azure App service'taki web apps için hazırlanmış yayımlamayı kullanmayı öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 17bc8fe2e5ccd9888777e11f3ca98e6afefb56b7
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53277395"
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Azure App Service ortamlarında hazırlık ayarlama
<a name="Overview"></a>

Web uygulaması, Linux ve mobil arka uç API uygulamasına web uygulamasına dağıtırken [App Service](https://go.microsoft.com/fwlink/?LinkId=529714), varsayılan üretim yuvasına yerine ayrı bir dağıtım yuvası çalıştırıldığında dağıtabileceğiniz **standart**, **Premium**, veya **yalıtılmış** App Service planı katmanı. Dağıtım yuvaları kendi ana bilgisayar adları olan Canlı uygulamalardır. Uygulama içeriği ve yapılandırma öğelerinin, üretim yuvası dahil iki dağıtım yuvası arasında değişiklik yapılabilir. Uygulamanızı bir dağıtım yuvasına dağıtma, aşağıdaki faydaları vardır:

* Üretim yuvasıyla değiştirmeden önce hazırlık dağıtım yuvasındaki uygulama değişiklikleri doğrulayabilirsiniz.
* Bir uygulamayı ilk kez bir yuvasına dağıtma ve üretime geçirmeden yuvası tüm örneklerini üretime takas önce warmed olduğunu sağlar. Uygulamanızı dağıtırken bu kapalı kalma süresini ortadan kaldırır. Trafik yeniden yönlendirmesi sorunsuzdur ve değiştirme işlemleri sonucunda hiçbir istek bırakılır. Yapılandırarak bu iş akışının tamamı otomatikleştirilebilir [otomatik değiştirme](#Auto-Swap) zaman değiştirme öncesi doğrulama gerekli değildir.
* Bir değiştirme işleminden sonra yuvası ile önceden hazırlanmış uygulama artık önceki üretim uygulamasına sahiptir. Beklendiği gibi üretim yuvasına değişiklikleri varsa, "son bilinen iyi sitenizi" hemen almak için aynı değiştirme gerçekleştirebileceğiniz geri.

Her App Service planı katmanı farklı sayıda dağıtım yuvalarını destekler. Sayısının ölçeğini bulmak için uygulama katmanı destekleyen yuvası için bkz: [App Service limitleri](https://docs.microsoft.com/azure/azure-subscription-service-limits#app-service-limits). Farklı bir katmana uygulamanızı ölçeklendirmek için hedef katmana uygulamanızı zaten kullanıyor yuva sayısı desteklemesi gerekir. Uygulamanızı 5'ten fazla yuva varsa, örneğin, aşağı için ölçeği **standart** olduğundan, katman **standart** katman yalnızca 5 dağıtım yuvalarını destekler.

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a>Bir dağıtım yuvası Ekle
Uygulama çalıştırmalıdır **standart**, **Premium**, veya **yalıtılmış* katmanı, birden çok dağıtım yuvasını etkinleştirmeniz sırayla.

1. İçinde [Azure portalı](https://portal.azure.com/), uygulamanızın açın [kaynak dikey penceresinin](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Seçin **dağıtım yuvalarını** seçeneğini belirleyin, ardından tıklayın **yuva Ekle**.
   
    ![Yeni bir dağıtım yuvası ekle][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > Uygulama zaten değilse **standart**, **Premium**, veya **yalıtılmış* katmanı, aşamalı yayımlamayı etkinleştirmeye desteklenen katmanları belirten bir ileti alırsınız. Bu noktada, tercih yapma seçeneğine sahip ** **yükseltme** gidin **ölçek** sekmesinde devam etmeden önce uygulama.
   > 
   > 
3. İçinde **bir yuva Ekle** dikey penceresinde yuvası bir ad verin ve mevcut olan başka bir dağıtım yuvası uygulama yapılandırmasından kopyalamak seçin. Devam etmek için onay işaretine tıklayın.
   
    ![Yapılandırma Kaynağı][ConfigurationSource1]
   
    İlk kez bir yuvaya eklemek için yalnızca iki seçeneğiniz vardır: üretim ya da hiç varsayılan yuvasından kopya yapılandırma.
    Birkaç yuvaları oluşturduktan sonra üretim ortamında farklı bir yuvasından yapılandırma kopyalama olacaktır:
   
    ![Yapılandırma kaynağı][MultipleConfigurationSources]
4. Uygulamanızın kaynak dikey penceresinde **dağıtım yuvalarını**, Metrikler ve yapılandırma gibi herhangi bir uygulama kümesiyle Bu yuvanın kaynak dikey penceresini açmak için bir dağıtım yuvası'ye tıklayın. Yuva adını dağıtım yuvası görüntülediğiniz hatırlatmak için dikey pencerenin en üstünde gösterilir.
   
    ![Dağıtım yuvası başlığı][StagingTitle]
5. Uygulama URL'sini yuvanın dikey penceresinde tıklayın. Dağıtım yuvası kendi ana bilgisayar adı olan hem de dinamik uygulama dikkat edin. Dağıtım yuvası genel erişimi sınırlamak için bkz: [web erişimi engellemek için üretim dışı dağıtım yuvaları App Service Web uygulaması –](https://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Dağıtım yuvası oluşturulduktan sonra içerik yok. Farklı bir depo dalı veya tamamen farklı bir depoya yuvaya dağıtabilirsiniz. Yuvanın yapılandırmasını da değiştirebilirsiniz. İçerik güncelleştirmeleri için dağıtım yuvasıyla ilişkili yayımlama profilini veya dağıtım kimlik bilgilerini kullanın.  Örneğin, [bu yuva git ile yayımlama](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>

## <a name="which-settings-are-swapped"></a>Hangi ayarların değiştirilir?
Başka bir dağıtım yuvasından yapılandırma kopyaladığınızda, kopyalanan düzenlenebilir bir yapılandırmadır. Ayrıca, diğer yapılandırma öğeleri aynı yuva değiştirme işleminden sonra (Yuva belirli) kalır ancak bazı yapılandırma öğeleri (değil yuva belirli) arasında bir takas içeriği izler. Aşağıdaki listelerde, yuvaları takas olduğunda değiştirme ayarları gösterilir.

**Olacağı ayarları**:

* Framework sürümü, 32/64-bit, Web sockets gibi genel ayarları-
* Uygulama ayarları (yuvada şekilde yapılandırılabilir)
* Bağlantı dizeleri (yuvada şekilde yapılandırılabilir)
* İşleyici eşlemeleri
* İzleme ve tanılama ayarları
* WebJobs içeriği
* Karma bağlantılar

**Değil takas ayarları**:

* Yayımlama uç noktaları
* Özel Etki Alanı Adları
* SSL sertifikaları ve bağlamaları
* Ölçek ayarları
* WebJobs zamanlayıcılar

(Takas değil) yuvada için bir uygulama ayarı veya bağlantı dizesini yapılandırmak için erişim **uygulama ayarları** dikey penceresinde belirli bir yuva için seçip **yuva ayarı** kutusunu yapılandırma yuvada öğeler. Yuva olarak belirli bir yapılandırma öğesi işaretleme, o öğe arasında uygulama ile ilişkili tüm dağıtım yuvalarını swappable kurma etkisi vardır.

![Yuva ayarları][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a>Dağıtım yuvalarını değiştirme 
Dağıtım yuvaları takas edebilirsiniz **genel bakış** veya **dağıtım yuvalarını** uygulamanızın kaynak dikey penceresinin görünümü.

> [!IMPORTANT]
> Bir uygulamadan bir dağıtım yuvasını üretime taşır önce tüm yuvası olmayan belirli ayarları takas hedef sağlamak üzere tam olarak istediğiniz şekilde yapılandırıldığından emin olun.
> 
> 

1. Dağıtım yuvalarını değiştirmek için tıklayın **takas** uygulamasının komut çubuğunda veya dağıtım yuvasının komut çubuğunda düğme.
   
    ![Değiştirme düğmesi][SwapButtonBar]

2. Değiştirme hedefi ve takas kaynak düzgün ayarlandığından emin olun. Genellikle, değiştirme hedefi üretim yuvasıdır. Tıklayın **Tamam** işlem tamamlanamadı. İşlem sona erdiğinde, dağıtım yuvaları takas.

    ![Değiştirmeyi tamamla](./media/web-sites-staged-publishing/SwapImmediately.png)

    İçin **Önizleme ile değiştirme** değiştirme türü için bkz: [(birden çok aşamalı değiştirme) önizleme ile değiştirme](#Multi-Phase).  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a>(Birden çok aşamalı değiştirme) önizleme ile değiştirme

Önizleme ile değiştirme ya da birden çok aşamalı takas yuvası özgü yapılandırma öğelerinin bağlantı dizeleri gibi doğrulama basitleştirin.
Görev açısından kritik iş yükleri için doğrulamak uygulamayı üretim yuvanın yapılandırmasını uygulandığında beklendiği gibi davranan istediğiniz ve bu tür doğrulamasını gerçekleştirmelidir *önce* uygulamayı üretime taşınır. Önizleme ile değiştirme ihtiyacınız var.

> [!NOTE]
> Linux üzerinde web apps Önizleme ile değiştirme desteklenmez.

Kullanırken **Önizleme ile değiştirme** seçeneği (bkz [dağıtım yuvaları takas](#Swap)), App Service şunları yapar:

- Hedef yuvanın yuvanın (örneğin, üretim) var olan iş yükünü etkilemeyecek şekilde değişmeden kalmasını sağlar.
- Yapılandırma öğelerini hedef yuva, uygulama ayarları ve yuva özel bağlantı dizeleri dahil olmak üzere kaynak yuvaya uygular.
- Bu yapılandırma yukarıda sözü edilen öğeleri kullanılarak kaynak yuvaya üzerinde çalışan işlemleri yeniden başlatır.
- Takas tamamlandığında: Öncesi warmed yukarı kaynak yuvaya hedef yuvaya taşır. Hedef yuvanın el ile takas olduğu gibi kaynak yuvasına taşınır.
- Ne zaman değiştirmeyi iptal et: Kaynak yuvaya yapılandırma öğelerini, kaynak yuvaya yeniden uygular.

Uygulamanın hedef yuvanın yapılandırmasını ile tam olarak nasıl davranacağını önizleyebilirsiniz. Doğrulama tamamlandığında, takas ayrı bir adımda tamamlayın. Bu adım, kaynak yuvaya zaten istenen yapılandırma ile warmed ek bir avantajı vardır ve istemciler, kapalı kalma süresi deneyimi yok.  

Örnekleri birden çok aşamalı takas için Azure PowerShell cmdlet'leri için dağıtım yuvaları bölümü için Azure PowerShell cmdlet'leri dahil edilmiştir.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Otomatik Takas yapılandırın
Otomatik Takas sıfır hazırlıksız başlatma ve sıfır kapalı kalma süresi ile uygulamanızı uygulamasının son müşteriler için sürekli olarak dağıtmak istediğiniz DevOps senaryolarını kolaylaştırır. Bu yuvaya her kod güncelleştirme gönderdiğinizde bir dağıtım yuvası üretim ortamına otomatik takas için yapılandırıldığında, bunu zaten yuvada warmed sonra App Service otomatik olarak uygulama üretime değiştireceksiniz.

> [!IMPORTANT]
> Otomatik değiştirme için bir yuva etkinleştirdiğinizde, tam olarak (genellikle üretim yuvasına) hedef yuva için hedeflenen yapılandırma yuva yapılandırmadır emin olun.
> 
> 

> [!NOTE]
> Linux üzerinde web apps'te otomatik değiştirme desteklenmiyor.

Otomatik Takas yuva için yapılandırma çok kolaydır. Şu adımları uygulayın:

1. İçinde **dağıtım yuvalarını**, üretim dışı bir yuva seçin ve seçme **uygulama ayarları** Bu yuvanın kaynak dikey penceresinde.  
   
    ![][Autoswap1]
2. Seçin **üzerinde** için **otomatik değiştirme**, istenen hedef yuvada seçin **otomatik takas yuvası**, tıklatıp **Kaydet** komut çubuğunda. Tam olarak hedef yuva için hedeflenen yapılandırma yuva için yapılandırma olduğundan emin olun.
   
    **Bildirimleri** sekmesini yanıp yeşil **başarı** işlemi tamamlandıktan sonra.
   
    ![][Autoswap2]
   
   > [!NOTE]
   > Otomatik değiştirme için uygulamanızı test etmek için önce bir üretim dışı hedef yuvaya seçebilirsiniz **otomatik takas yuvası** özelliğiyle ilgili bilgi sahibi olma.  
   > 
   > 
3. Bu dağıtım yuvası için bir kod gönderimi yürütün. Kısa bir süre sonra otomatik takas olur ve güncelleştirme sırasında hedef yuvanın URL'si yansıtılır.

<a name="Rollback"></a>

## <a name="roll-back-a-production-app-after-swap"></a>Bir üretim uygulaması değiştirme işleminden sonra geri alma
Üretim yuvası takas sonra herhangi bir hata belirlenirse yuvaları hemen aynı iki yuvayı değiştirerek öncesi takas durumlarına geri alma.

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a>Takas önce özel Isınma
Kullanırken [Auto-Swap](#Auto-Swap), bazı uygulamalar, özel Isınma Eylemler gerekebilir. `applicationInitialization` Web.config dosyasındaki yapılandırma öğesi, bir istek almadan önce gerçekleştirilecek özel başlatma eylemleri belirtmenize olanak sağlar. Değiştirme işlemi, bu özel bir Isınma tamamlanmasını bekler. İşte bir örnek web.config parça.

    <system.webServer>
        <applicationInitialization>
            <add initializationPage="/" hostName="[app hostname]" />
            <add initializationPage="/Home/About" hostName="[app hostname]" />
        </applicationInitialization>
    </system.webServer>

## <a name="monitor-swap-progress"></a>Takas ilerlemeyi İzle

Bazı durumlarda, değiştirme işlemi, değiştirilirken uygulama uzun Isınma süresini olduğunda gibi tamamlanması biraz zaman alır. Değiştirme işlemleri hakkında daha fazla bilgi alabilirsiniz [etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) içinde [Azure portalında](https://portal.azure.com).

Portalın sol gezinti bölmesinde, uygulama sayfanızın seçin **etkinlik günlüğü**.

Bir değiştirme işlemi günlük sorgusu görünür `Slotsswap`. Genişletin ve ayrıntıları görmek için hataları ve suboperations birini seçin.

![Etkinlik günlüğü yuvası takas için](media/web-sites-staged-publishing/activity-log.png)

<a name="Delete"></a>

## <a name="delete-a-deployment-slot"></a>Dağıtım yuvasını Sil
Dağıtım yuvası dikey penceresinde, dağıtım yuvasındaki'nın dikey penceresini açın, **genel bakış** (varsayılan sayfası) tıklayıp **Sil** komut çubuğunda.  

![Dağıtım yuvasını Sil][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="automate-with-azure-powershell"></a>Azure PowerShell ile otomatik hale getirin

Azure PowerShell, Azure App Service'te dağıtım yuvalarını yönetmek için destek dahil olmak üzere Windows PowerShell aracılığıyla Azure yönetmek için cmdlet'ler sağlayan bir modüldür.

* Yükleme ve yapılandırma Azure PowerShell ve Azure PowerShell, Azure aboneliğiniz ile kimlik doğrulaması için bilgi [nasıl Microsoft Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).  

- - -
### <a name="create-a-web-app"></a>Web uygulaması oluşturma
```PowerShell
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a>Dağıtım yuvası oluştur
```PowerShell
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-preview-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a>Önizleme (birden çok aşamalı değiştirme) ile bir değiştirme başlatmak ve kaynak yuva için hedef yuva yapılandırmasını Uygula
```PowerShell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Bekleyen bir değiştirme (gözden geçirme ile değiştirme) iptal edin ve geri yükleme kaynağı yuvası yapılandırması
```PowerShell
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a>Dağıtım yuvalarını değiştirme
```PowerShell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

### <a name="monitor-swap-events-in-the-activity-log"></a>Etkinlik günlüğünde İzleyici değiştirme olayları
```PowerShell
Get-AzureRmLog -ResourceGroup [resource group name] -StartTime 2018-03-07 -Caller SlotSwapJobProcessor  
```

- - -
### <a name="delete-deployment-slot"></a>Dağıtım yuvasını Sil
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="automate-with-azure-cli"></a>Azure CLI ile otomatikleştirme

İçin [Azure CLI](https://github.com/Azure/azure-cli) bkz: dağıtım yuvaları için komutları [az webapp deployment slot](/cli/azure/webapp/deployment/slot).

## <a name="next-steps"></a>Sonraki adımlar
[Azure App Service Web uygulaması – üretim dışı dağıtım yuvalarını web erişimi engelle](https://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)  
[Linux üzerinde App Service'e Giriş](../app-service/containers/app-service-linux-intro.md)  
[Microsoft Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png

