---
title: "Hazırlık ortamları için Azure App Service'te web uygulamalarını ayarlama | Microsoft Docs"
description: "Azure App Service'deki web uygulamaları için hazırlanmış yayımlamayı kullanmayı öğrenin."
services: app-service
documentationcenter: 
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
ms.openlocfilehash: 55c023e8f6b41c17e85ba441f862a7682b2f2599
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Hazırlık Azure App Service ortamları ayarlama
<a name="Overview"></a>

Web uygulaması, Linux, mobil arka uç ve API uygulaması için web uygulaması dağıtırken [uygulama hizmeti](http://go.microsoft.com/fwlink/?LinkId=529714), varsayılan üretim yuvasına yerine ayrı bir dağıtım yuvası çalıştırırken dağıtabileceğiniz **standart** veya **Premium** uygulama hizmeti planı modu. Dağıtım yuvaları, kendi ana bilgisayar adları ile gerçekten Canlı uygulamalardır. Uygulama içeriği ve yapılandırmaları öğeleri üretim yuvası da dahil olmak üzere iki dağıtım yuvası arasında değişiklik yapılabilir. Bir dağıtım yuvası uygulamanıza dağıtımı aşağıdaki faydaları vardır:

* Üretim yuvasıyla değiştirmeden önce hazırlık dağıtım yuvasındaki uygulama değişikliklerini doğrulayabilirsiniz.
* Bir uygulama için bir yuva ilk dağıtma ve üretim ile değiştirmeden yuva tüm örneklerini üretime takas önce warmed olduğunu sağlar. Uygulamanızı dağıttığınızda bu kapalı kalma süresini ortadan kaldırır. Trafik yeniden yönlendirmesi sorunsuzdur ve değiştirme işlemleri sonucunda hiçbir istek bırakılır. Yapılandırarak bu iş akışının tamamı otomatikleştirilebilir [otomatik takas](#Auto-Swap) zaman öncesi takas doğrulama gerekli değildir.
* Değiştirme işleminden sonra önceden hazırlanmış uygulama yuvasıyla artık önceki üretim uygulamasına sahiptir. Beklendiği gibi üretim yuvasına değişiklikleri varsa, "son bilinen iyi sitenizi" hemen almak için aynı değiştirme işlemini gerçekleştirebilirsiniz geri.

Her bir uygulama hizmeti planı modu dağıtım yuvaları farklı sayıda destekler. Out sayısını bulmak için uygulamanızın modunu destekler yuvası için bkz: [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).

* Uygulamanız birden çok yuvası olduğunda, modunu değiştiremezsiniz.
* Ölçeklendirme için üretim dışı yuvası kullanılabilir değil.
* Bağlantılı kaynak yönetimi için üretim dışı yuvası desteklenmiyor. İçinde [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) üretim dışı yuvası geçici olarak başka bir uygulama hizmeti planı moda taşıyarak bu olası etkisini üretim yuvasına yalnızca önleyebilirsiniz. İki yuvaları takas önce üretim dışı yuvası yine aynı mod üretim yuvasıyla paylaşmalıdır olduğunu unutmayın.

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a>Bir dağıtım yuvası Ekle
Uygulamayı çalışır durumda **standart** veya **Premium** modu, birden çok dağıtım yuvasını etkinleştirmeniz sırayla.

1. İçinde [Azure Portal](https://portal.azure.com/), uygulamanızın açmak [kaynak dikey](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Seçin **dağıtım yuvası** seçeneğini ve ardından **yuva Ekle**.
   
    ![Yeni bir dağıtım yuvası ekle][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > Uygulama zaten değilse **standart** veya **Premium** modu, aşamalı yayımlamayı etkinleştirmeye ilişkin desteklenen modları belirten bir ileti alırsınız. Bu noktada, seçmek için seçeneğiniz **yükseltme** gidin **ölçek** devam etmeden önce uygulamanızın sekmesi.
   > 
   > 
3. İçinde **bir yuva eklemek** dikey penceresinde, yuva bir ad verin ve başka bir var olan dağıtım yuvası uygulama yapılandırmasından kopyalamak seçin. Devam etmek için onay işaretine tıklayın.
   
    ![Yapılandırma Kaynağı][ConfigurationSource1]
   
    İlk kez bir yuva eklemek için yalnızca iki seçenek olacaktır: üretim ya da hiç varsayılan yuvadan kopya yapılandırma.
    Birkaç yuvaları oluşturduktan sonra üretim farklı bir yuvadan yapılandırmayı kopyalama mümkün olacaktır:
   
    ![Yapılandırma kaynakları][MultipleConfigurationSources]
4. Uygulamanızın kaynak dikey penceresinde tıklayın **dağıtım yuvası**, ölçümleri ve diğer herhangi bir uygulama gibi yapılandırma kümesi ile Bu yuvanın kaynak dikey penceresini açmak için bir dağıtım yuvası'ye tıklayın. Yuva adı, dağıtım yuvasındaki görüntülüyorsanız anımsatmak için dikey pencerenin en üstünde gösterilir.
   
    ![Dağıtım yuvası başlığı][StagingTitle]
5. Uygulama URL'si yuvanın dikey penceresinde'ı tıklatın. Dağıtım yuvası kendi ana bilgisayar adı olan hem de dinamik uygulama dikkat edin. Dağıtım yuvası genel erişimi sınırlamak için bkz: [App Service Web uygulaması – web erişimi engellemek için üretim dışı dağıtım yuvası](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Dağıtım yuvası oluşturulduktan sonra içerik yok. Farklı depo dalından veya tamamen farklı bir depo yuvaya dağıtabilirsiniz. Yuvanın yapılandırmasını da değiştirebilirsiniz. İçerik güncelleştirmeleri için dağıtım yuvasıyla ilişkili yayımlama profilini veya dağıtım kimlik bilgilerini kullanın.  Örneğin, [bu yuva git ile yayımlamayı](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>

## <a name="configuration-for-deployment-slots"></a>Dağıtım yuvası için yapılandırma
Başka bir dağıtım yuvası yapılandırmasından kopyaladığınızda kopyalanan düzenlenebilir yapılandırmadır. Ayrıca, diğer yapılandırma öğeleri aynı yuvada değiştirme işleminden sonra (Yuva belirli) kalır ancak bazı yapılandırma öğeleri takas (değil yuva belirli) içeriği izler. Aşağıdaki listelerde yuvaları takas yükleyen değiştirecek yapılandırmasını gösterir.

**Değiştirilen ayarları**:

* -Framework sürümü, 32/64-bit, Web yuvaları gibi genel ayarları
* Uygulama ayarları (bir yuvaya kullanmayı yapılandırılabilir)
* Bağlantı dizeleri (bir yuvaya kullanmayı yapılandırılabilir)
* İşleyici eşlemeleri
* İzleme ve tanılama ayarları
* Web işleri içeriği

**Değil takas ayarları**:

* Yayımlama uç noktaları
* Özel Etki Alanı Adları
* SSL sertifikaları ve bağlamaları
* Ölçek ayarları
* Web işleri zamanlayıcılar

(Takas değil) bir yuva takılıyor için bir uygulama ayarı veya bağlantı dizesini yapılandırmak için erişim **uygulama ayarları** belirli bir yuva için dikey seçip **yuva ayarı** yapılandırma kutusu Yuva takılıyor öğeler. Yuva belirli o öğeye uygulamayla ilişkili tüm dağıtım yuvalarını arasında swappable oluşturma etkisi gibi bir yapılandırma öğesi işaretleme unutmayın.

![Yuva ayarları][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a>Dağıtım yuvalarını değiştirme 
Dağıtım yuvaları takas **genel bakış** veya **dağıtım yuvası** uygulamanızın kaynak dikey penceresinin görünümü.

> [!IMPORTANT]
> Üretime bir uygulamadan bir dağıtım yuvası takas önce tüm yuva olmayan belirli ayarları tam olarak değiştirme hedefi olması, istediğiniz biçimde yapılandırılmış olduğundan emin olun.
> 
> 

1. Dağıtım yuvaları takas etmek için tıklatın **takas** düğmesi uygulamanın komut çubuğunda veya dağıtım yuvasının komut çubuğunda.
   
    ![Değiştirme düğmesi][SwapButtonBar]

2. Takas kaynak ve hedef takas düzgün ayarlandığından emin olun. Genellikle, değiştirme hedefi üretim yuvasıdır. Tıklatın **Tamam** işlem tamamlanamadı. İşlem sona erdiğinde, dağıtım yuvaları takas.

    ![Değiştirmeyi tamamla](./media/web-sites-staged-publishing/SwapImmediately.png)

    İçin **Önizleme ile değiştirme** değiştirme türü için bkz: [(çok aşaması takas) önizleme ile değiştirme](#Multi-Phase).  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a>(Çok aşaması takas) önizleme ile değiştirme

Önizleme ile değiştirme ya da çok aşaması takas yuvası özel yapılandırma öğeleri, bağlantı dizeleri gibi doğrulama basitleştirin.
Kritik iş yükleri için doğrulamak uygulamayı üretim yuvanın yapılandırmasını uygulandığında beklendiği gibi davranan istediğiniz ve bu tür doğrulama gerçekleştirmelisiniz *önce* uygulama üretime taşınır. Önizleme ile değiştirme ihtiyacınız olur.

> [!NOTE]
> Linux üzerinde web uygulamalarında Önizleme ile değiştirme desteklenmez.

Kullandığınızda **Önizleme ile değiştirme** seçeneği (bkz [dağıtım yuvaları takas](#Swap)), uygulama hizmeti aşağıdakileri yapar:

- Bu yuva (örneğin, üretim) mevcut iş yüküne etkilenmez şekilde değişmeden hedef yuvaya tutar.
- Yapılandırma öğelerini hedef yuvaya, uygulama ayarları ve yuva özgü bağlantı dizeleri dahil olmak üzere kaynak yuvaya uygular.
- Bu daha önce bahsedilen yapılandırma öğeleri kullanarak kaynak yuvaya olan çalışan işlemleri yeniden başlatır.
- Takas tamamlandığında: hedef yuvaya öncesi warmed yukarı kaynak yuvaya taşır. Hedef yuvaya el ile değiştirme gibi kaynak yuvasına taşınır.
- İptal zaman değiştirme: kaynak yuvaya yapılandırma öğelerini kaynak yuvaya yeniden uygular.

Uygulamanın hedef yuvanın yapılandırma ile tam olarak nasıl davranacak önizleyebilirsiniz. Doğrulama tamamlandığında, ayrı bir adımda takas tamamlayın. Bu adım kaynak yuvaya zaten istenen yapılandırma ile warmed eklenen avantajı vardır ve istemcilerin kapalı kalma süresi karşılaşmazsınız.  

Örnekler için Azure PowerShell cmdlet'leri çok aşaması takas için kullanılabilir dağıtım yuvası bölümü için Azure PowerShell cmdlet'leri dahil edilmiştir.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Otomatik Takas yapılandırın
Otomatik Takas sürekli olarak uygulama son müşteriler için sıfır cold start ve sıfır kapalı kalma süresi ile uygulamanızı dağıtmak istediğiniz DevOps senaryolarını kolaylaştırır. Bu yuva için kod güncelleştirmenizi anında her zaman bir dağıtım yuvası üretim ortamına otomatik takas için yapılandırıldığında, onu zaten yuvada warmed sonra uygulama hizmeti otomatik olarak uygulama üretime değiştireceksiniz.

> [!IMPORTANT]
> Otomatik Takas yuvası için etkinleştirdiğinizde, tam olarak için hedef yuva (genellikle üretim yuvasına) yönelik yapılandırma yuvası yapılandırması olduğundan emin olun.
> 
> 

> [!NOTE]
> Otomatik Takas web uygulamaları Linux üzerinde desteklenmiyor.

Otomatik Takas yuvası için yapılandırma kolaydır. Aşağıdaki adımları izleyin:

1. İçinde **dağıtım yuvası**, bir üretim dışı yuva seçin ve **uygulama ayarları** Bu yuvanın kaynak dikey penceresinde.  
   
    ![][Autoswap1]
2. Seçin **üzerinde** için **otomatik takas**, istenen hedef yuvada seçin **otomatik takas yuvası**, tıklatıp **kaydetmek** komut çubuğunda. Tam olarak için hedef yuva hedeflenen yapılandırma yuvası için yapılandırma olduğundan emin olun.
   
    **Bildirimleri** flash yeşil sekmesi **başarı** işlemi tamamlandıktan sonra.
   
    ![][Autoswap2]
   
   > [!NOTE]
   > Otomatik Takas için uygulamanızı test etmek için önce bir üretim dışı hedef yuvada seçebilirsiniz **otomatik takas yuvası** özelliğiyle ilgili bilgi sahibi olma.  
   > 
   > 
3. Bu dağıtım yuvası için kod push yürütün. Otomatik Takas kısa bir süre sonra gerçekleşir ve güncelleştirme hedef yuvanın URL'de yansıtılır.

<a name="Rollback"></a>

## <a name="to-rollback-a-production-app-after-swap"></a>Bir üretim uygulaması takas sonra geri almak için
Bir yuva değişikliğinden sonra üretimde herhangi bir hata belirlenirse, yuvaları aynı iki hemen yuvayı değiştirerek öncesi takas durumlarına geri alma.

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a>Takas önce özel Isınma
Bazı uygulamalar özel Isınma Eylemler gerekebilir. `applicationInitialization` Yapılandırma öğesi web.config dosyasındaki bir isteği almadan önce gerçekleştirilecek özel başlatma eylemleri belirtmenize olanak verir. Değiştirme işlemi, bu özel Isınma tamamlanmasını bekler. Bir örnek web.config parçası değil.

    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostname="[app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>

## <a name="to-delete-a-deployment-slot"></a>Bir dağıtım yuvası silmek için
Bir dağıtım yuvası için dikey penceresinde dağıtım yuvanın dikey penceresini açın, **genel bakış** (varsayılan sayfa) tıklatıp **silmek** komut çubuğunda.  

![Bir dağıtım yuvası Sil][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="azure-powershell-cmdlets-for-deployment-slots"></a>Dağıtım yuvaları için Azure PowerShell cmdlet'leri
Azure PowerShell'i Azure Azure uygulama hizmeti dağıtım yuvalarında yönetmek için destek dahil olmak üzere Windows PowerShell aracılığıyla yönetmek için cmdlet'leri sağlayan bir modüldür.

* Yükleme ve yapılandırma Azure PowerShell ve Azure PowerShell'i Azure aboneliğiniz ile kimlik doğrulaması hakkında bilgi için bkz: [Microsoft Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).  

- - -
### <a name="create-a-web-app"></a>Web uygulaması oluşturma
```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a>Bir dağıtım yuvası oluşturma
```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-review-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a>Gözden geçirme (çok aşaması takas) ile değiştirme başlatmak ve kaynak yuvaya hedef yuvası yapılandırması Uygula
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Bekleyen bir değiştirme (gözden geçirme ile değiştirme) iptal kaynak yuvası yapılandırması ve geri yükleme
```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a>Dağıtım yuvalarını değiştirme
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="delete-deployment-slot"></a>Dağıtım yuvası Sil
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="azure-command-line-interface-azure-cli-commands-for-deployment-slots"></a>Dağıtım yuvaları için Azure komut satırı arabirimi (Azure CLI) komutları
Azure CLI, Azure uygulama hizmeti dağıtım yuvaları yönetmek için destek dahil olmak üzere ile çalışmak için platformlar arası komutları sağlar.

* Yükleme ve Azure CLI Azure aboneliğinize bağlanma hakkında bilgi de dahil olmak üzere Azure CLI yapılandırma hakkında yönergeler için bkz: [Azure CLI'yi yükleme ve yapılandırma](../cli-install-nodejs.md).
* Azure App Service'te Azure CLI için kullanılabilen komutları listelemek için arama `azure site -h`.

> [!NOTE] 
> İçin [Azure CLI 2.0](https://github.com/Azure/azure-cli) bkz: dağıtım yuvaları için komutları [az webapp dağıtım yuvası](/cli/azure/webapp/deployment/slot).

- - -
### <a name="azure-site-list"></a>Azure site listesi
Geçerli abonelik uygulamalar hakkında daha fazla bilgi için arama **azure site listesi**, aşağıdaki örnekte olduğu gibi.

`azure site list webappslotstest`

- - -
### <a name="azure-site-create"></a>Azure sitesi oluştur
Bir dağıtım yuvası oluşturmak için arama **azure sitesi oluşturmak** ve var olan bir uygulama adı ve oluşturma, aşağıdaki örnekte olduğu gibi için yuva adını belirtin.

`azure site create webappslotstest --slot staging`

Yeni yuva için kaynak denetimi etkinleştirmek için **--git** aşağıdaki örnekteki gibi seçeneği.

`azure site create --git webappslotstest --slot staging`

- - -
### <a name="azure-site-swap"></a>Azure site değiştirme
Üretim uygulamasına yuva güncelleştirilen dağıtım yapmak için kullanın **azure site takas** aşağıdaki örnekteki gibi bir değiştirme işlemi gerçekleştirmek için komutu. Üretim uygulamasına herhangi kesinti karşılaşmazsınız veya cold start yapılacaktır.

`azure site swap webappslotstest`

- - -
### <a name="azure-site-delete"></a>Azure site Sil
Artık gerekli olmadığında bir dağıtım yuvası silmek için kullanın **azure sitesi silme** aşağıdaki örnekteki gibi komutu.

`azure site delete webappslotstest --slot staging`

- - -
> [!NOTE]
> Bir web uygulamasını çalışırken görün. [App Service'i deneyin](https://azure.microsoft.com/try/app-service/) hemen bir kısa süreli başlangıç uygulaması oluşturun — gerekli herhangi bir kredi kartı veya bir taahhüt.
> 
> 

## <a name="next-steps"></a>Sonraki Adımlar
[Azure App Service Web uygulama – web erişimi engellemek için üretim dışı dağıtım yuvası](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[uygulama hizmeti Linux'ta giriş](../app-service/containers/app-service-linux-intro.md)
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

