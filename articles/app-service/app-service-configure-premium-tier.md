---
title: PremiumV2 katmanı Azure App Service için yapılandırma | Microsoft Docs
description: Performans web, mobil ve Azure App Service'te API uygulaması için yeni PremiumV2 fiyatlandırma katmanı ölçeklendirme tarafından daha iyi öğrenin.
keywords: app service, azure app service, ölçek, ölçeklenebilir, app service planı, app service maliyeti
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: ff00902b-9858-4bee-ab95-d3406018c688
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: cephalin
ms.openlocfilehash: 4c157ed905b7dc48c886b26987c164ef9a47f3c3
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34714570"
---
# <a name="configure-premiumv2-tier-for-azure-app-service"></a>PremiumV2 katmanı Azure App Service için yapılandırma

Yeni **PremiumV2** fiyatlandırma katmanı size daha hızlı işlemcilerde, SSD depolama ve mevcut fiyatlandırma katmanlarına çiftleri bellek çekirdek oranı. Performans avantajı ile daha az örneklerinde uygulamalarınızı çalıştırarak paradan tasarruf. Bu makalede, bir uygulama oluşturmayı öğrenmek **PremiumV2** katmanı veya bir uygulamaya ölçeklendirin **PremiumV2** katmanı.

## <a name="prerequisites"></a>Önkoşullar

Bir web uygulamasına büyütme için **PremiumV2**, daha düşük bir fiyatlandırma katmanı çalışan Azure uygulama hizmeti bir Web uygulamanız gereken **PremiumV2**.

<a name="availability"></a>

## <a name="premiumv2-availability"></a>PremiumV2 kullanılabilirliği

PremiumV2 katmanı uygulama hizmeti için şu anda kullanılabilir _Windows_ yalnızca. Linux kapsayıcıları henüz desteklenmiyor.

PremiumV2 çoğu Azure bölgelerinde kullanılabilir ve artan zaten var. Bunu Bölgenizde kullanılabilir olup olmadığını görmek için aşağıdaki Azure CLI komutu Çalıştır [Azure bulut Kabuk](../cloud-shell/overview.md):

```azurecli-interactive
az appservice list-locations --sku P1V2
```

Uygulama oluşturma veya uygulama hizmeti planı oluşturma, ardından sırasında bir hata alırsanız **PremiumV2** büyük olasılıkla tercih bölgeniz için kullanılamaz.

<a name="create"></a>

## <a name="create-an-app-in-premiumv2-tier"></a>PremiumV2 katmanında bir uygulama oluşturun

Fiyatlandırma katmanı, bir uygulama hizmeti uygulamanın tanımlanan [uygulama hizmeti planı](azure-web-sites-web-hosting-plans-in-depth-overview.md) çalıştığı. Tek başına ya da Web uygulaması oluşturma işleminin bir parçası olarak, bir uygulama hizmeti planı oluşturabilirsiniz.

Uygulama hizmeti planında yapılandırırken <a href="https://portal.azure.com" target="_blank">Azure portal</a>seçin **fiyatlandırma katmanı**. 

Seçin **üretim**seçeneğini belirleyip **P1V2**, **P2V2**, veya **P3V2**, ardından **Uygula**.

![](media/app-service-configure-premium-tier/scale-up-tier-select.png)

> [!IMPORTANT] 
> Görmüyorsanız, **P1V2**, **P2V2**, ve **P3V2** ya da farklı seçenekler **PremiumV2** seçim, bölgenizde kullanılamıyor veya olduğunuz desteklemeyen bir Linux uygulama hizmeti planı yapılandırma **PremiumV2**.

## <a name="scale-up-an-existing-app-to-premiumv2-tier"></a>Var olan bir uygulamaya PremiumV2 katmanına ölçeklendirin

Var olan bir uygulamaya ölçeklendirme önce **PremiumV2** katmanı, olduğundan emin olun **PremiumV2** Bölgenizde kullanılabilir. Bilgi için bkz: [PremiumV2 kullanılabilirlik](#availability). Bölgenizde kullanılabilir değilse, bkz: [desteklenmeyen bölgesinden ölçeği](#unsupported).

Barındırma ortamınıza bağlı olarak ölçeklendirmeyi ek adımlar gerekebilir. 

İçinde <a href="https://portal.azure.com" target="_blank">Azure portal</a>, uygulama hizmeti uygulaması sayfanızı açın.

Uygulama hizmeti uygulaması sayfanızı sol gezinti bölmesinde seçin **(uygulama hizmeti planı) ölçeklendirme**.

![](media/app-service-configure-premium-tier/scale-up-tier-portal.png)

Seçin **üretim**seçeneğini belirleyip **P1V2**, **P2V2**, veya **P3V2**, ardından **Uygula**.

![](media/app-service-configure-premium-tier/scale-up-tier-select.png)

İşleminiz başarıyla tamamlanırsa, uygulamanızın genel bakış sayfasında, şimdi gösterir bir **PremiumV2** katmanı.

![](media/app-service-configure-premium-tier/finished.png)

### <a name="if-you-get-an-error"></a>Bir hata iletisi alırsanız

Bazı uygulama hizmeti planları PremiumV2 katmanı ölçeği olamaz. Büyütme işlemi bir hata veriyorsa, uygulamanız için yeni bir uygulama hizmeti planı gerekiyor.

Oluşturma bir _Windows_ uygulama hizmeti planı aynı bölge ve kaynak grubunda var olan uygulama hizmeti uygulamanızı olarak. Bölümündeki adımları izleyin [PremiumV2 katmanında bir uygulama oluşturun](#create) ayarlamak için **PremiumV2** katmanı. Aynı yapılandırmayı genişletmek istiyorsanız, var olan uygulama hizmeti planına (örnekleri, otomatik ölçeklendirme vb. sayısı) kullanın.

Uygulama hizmeti uygulaması sayfanızı yeniden açın. Uygulama hizmetiniz sol gezinti bölmesinde seçin **değişiklik uygulama hizmeti planı**.

![](media/app-service-configure-premium-tier/change-plan.png)

Oluşturduğunuz uygulama hizmeti planı seçin.

![](media/app-service-configure-premium-tier/select-plan.png)

Değiştirme işlemi tamamlandıktan sonra uygulamanızı çalıştıran **PremiumV2** katmanı.

<a name="unsupported"></a>

## <a name="scale-up-from-an-unsupported-region"></a>Desteklenmeyen bir bölgesinden ölçeği

Uygulamanızı bir bölgede çalıştırıyorsa nerede **PremiumV2** yararlanmak için farklı bir bölgeye uygulamanızı taşıyabilirsiniz henüz kullanılamıyor **PremiumV2**. İki seçeneğiniz vardır:

- Yeni bir uygulama oluştur **PremiumV2** planlama ve uygulama kodunuz yeniden dağıtın. Bölümündeki adımları izleyin [PremiumV2 katmanında bir uygulama oluşturun](#create) ayarlamak için **PremiumV2** katmanı. Dilerseniz, aynı genişleme yapılandırmasını var olan uygulama hizmeti planına (örnekleri, otomatik ölçeklendirme vb. sayısı) kullanın.
- Uygulamanızı zaten var olan çalıştırıyorsa **Premium** katmanı sonra tüm uygulama ayarları, bağlantı dizeleri ve dağıtım yapılandırması ile uygulamanızı kopyalayabilirsiniz.

    ![](media/app-service-configure-premium-tier/clone-app.png)

    İçinde **kopya uygulama** sayfasında, bölge ve kopyalamak istediğiniz ayarları belirtin bir uygulama hizmeti planı oluşturabilirsiniz.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

Uygulama oluşturma otomatikleştirebilirsiniz **PremiumV2** katmanı kullanarak kodlarıyla [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure CLI

Aşağıdaki komut bir uygulama hizmeti planında oluşturur _P1V2_. Bulut Kabuğu'nda çalıştırabilirsiniz. Seçeneklerini `--sku` P1V2, olan _P2V2_, ve _P3V2_.

```azurecli-interactive
az appservice plan create \
    --resource-group <resource_group_name> \
    --name <app_service_plan_name> \
    --sku P1V2
```

### <a name="azure-powershell"></a>Azure PowerShell

Aşağıdaki komut bir uygulama hizmeti planında oluşturur _P1V2_. Seçeneklerini `-WorkerSize` olan _küçük_, _orta_, ve _büyük_.

```PowerShell
New-AzureRmAppServicePlan -ResourceGroupName <resource_group_name> `
    -Name <app_service_plan_name> `
    -Location <region_name> `
    -Tier "PremiumV2" `
    -WorkerSize "Small"
```
## <a name="more-resources"></a>Diğer kaynaklar

[Bir Azure uygulamasında ölçeklendirin](web-sites-scale.md)  
[Örnek sayısını el ile veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md)