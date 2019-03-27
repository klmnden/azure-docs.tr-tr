---
title: PremiumV2 katmanını - Azure App Service yapılandırma | Microsoft Docs
description: Performans web, mobil ve API uygulaması Azure App service'taki yeni PremiumV2 fiyatlandırma katmanına ölçeklendirerek daha iyi öğrenin.
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
ms.date: 07/25/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: c85644e3cab39f9e0864af91722ee54aab6d59f3
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58486878"
---
# <a name="configure-premiumv2-tier-for-azure-app-service"></a>Azure App Service için PremiumV2 katmanını yapılandırma

Yeni **PremiumV2** fiyatlandırma katmanı, size daha hızlı işlemcilere, SSD depolamaya ve mevcut fiyatlandırma katmanları double bellek / çekirdek oranı. Performans avantajı ile uygulamalarınızı daha az örneklerinde çalıştırarak paradan tasarruf sağlayabilirsiniz. Bu makalede, bir uygulamada nasıl oluşturulacağını öğrenin **PremiumV2** katmanı veya bir uygulamaya ölçeğini **PremiumV2** katmanı.

## <a name="prerequisites"></a>Önkoşullar

Bir uygulamaya ölçek artırmayı **PremiumV2**, daha düşük bir fiyatlandırma katmanında çalıştıran bir Azure App Service uygulaması ihtiyacınız **PremiumV2**, ve uygulama PremiumV2 destekleyen bir App Service dağıtımı çalışıyor olması gerekir.

<a name="availability"></a>

## <a name="premiumv2-availability"></a>PremiumV2 kullanılabilirlik

**PremiumV2** katmanında, hem de App Service için kullanılabilir _Windows_ yanı _Linux_.

**PremiumV2** çok sayıda Azure bölgesinde kullanılabilir. Bunu kendi Bölgenizde kullanılabilir olup olmadığını görmek için aşağıdaki Azure CLI komutunu çalıştırın [Azure Cloud Shell](../cloud-shell/overview.md):

```azurecli-interactive
az appservice list-locations --sku P1V2
```

<a name="create"></a>

## <a name="create-an-app-in-premiumv2-tier"></a>PremiumV2 katmanı uygulama oluşturma

Bir App Service uygulaması fiyatlandırma katmanını tanımlanan [App Service planı](overview-hosting-plans.md) üzerinde çalıştığı. Tek başına veya uygulama oluşturmanın bir parçası olarak, bir App Service planı oluşturabilirsiniz.

App Service planında yapılandırırken <a href="https://portal.azure.com" target="_blank">Azure portalında</a>seçin **fiyatlandırma katmanı**. 

Seçin **üretim**, ardından **P1V2**, **P2V2**, veya **P3V2**, ardından **Uygula**.

![](media/app-service-configure-premium-tier/scale-up-tier-select.png)

> [!IMPORTANT] 
> Görmüyorsanız **P1V2**, **P2V2**, ve **P3V2** seçeneklerle veya seçenekleri gri, çıkış, ardından **PremiumV2** büyük olasılıkla kullanılabilir değil App Service planı içeren temel bir App Service dağıtımı. Bkz: [desteklenmeyen kaynak grubunda ve bölgede bileşiminden ölçeği](#unsupported) daha fazla ayrıntı için.

## <a name="scale-up-an-existing-app-to-premiumv2-tier"></a>Mevcut bir uygulamaya PremiumV2 katmanı ölçeğini

Var olan bir uygulamayı ölçeklendirme önce **PremiumV2** emin olun, katmanı **PremiumV2** kullanılabilir. Bilgi için [PremiumV2 kullanılabilirlik](#availability). Kullanılabilir durumda değilse, bkz. [desteklenmeyen kaynak grubunda ve bölgede bileşiminden ölçeği](#unsupported).

Barındırma ortamınıza bağlı olarak büyütme ek adımlar gerektirebilir. 

İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a>, App Service uygulaması sayfanızın açın.

App Service uygulaması sayfanızın sol gezinti bölmesinde seçin **ölçeği Artır (App Service planı)**.

![](media/app-service-configure-premium-tier/scale-up-tier-portal.png)

Seçin **üretim**, ardından **P1V2**, **P2V2**, veya **P3V2**, ardından **Uygula**.

![](media/app-service-configure-premium-tier/scale-up-tier-select.png)

İşleminiz başarıyla tamamlanırsa şimdi olduğunu uygulamanızın genel bakış sayfasını gösterir bir **PremiumV2** katmanı.

![](media/app-service-configure-premium-tier/finished.png)

### <a name="if-you-get-an-error"></a>Bir hata iletisi alırsanız

Temel alınan App Service dağıtımı PremiumV2 desteklemiyorsa bazı App Service planları için PremiumV2 katmanı ölçeği olamaz.  Bkz: [desteklenmeyen kaynak grubunda ve bölgede bileşiminden ölçeği](#unsupported) daha fazla ayrıntı için.

<a name="unsupported"></a>

## <a name="scale-up-from-an-unsupported-resource-group-and-region-combination"></a>Desteklenmeyen kaynak grubunda ve bölgede bileşiminden ölçeği artırma

Uygulamanız bir App Service dağıtımı içinde çalışıyorsa burada **PremiumV2** kullanılamıyor veya uygulamanız bir bölgede şu anda çalışıyorsa desteklemediği **PremiumV2**, faydalanmak için uygulamanızı yeniden dağıtmanız gerekir avantajlarından **PremiumV2**.  İki seçeneğiniz vardır:

- Oluşturma bir **yeni** kaynak grubunda ve ardından oluşturun bir **yeni** uygulaması ve App Service planı'nda **yeni** oluşturma işlemi sırasında istediğiniz Azure bölgeyi seçerek, kaynak grubu.  **Gerekir** seçin **PremiumV2** plana zaman yeni app service planı oluşturulur.  Bu kaynak grubu, App Service planı birleşimi sağlar ve Azure bölgesi destekleyen bir App Service dağıtımı oluşturulan App Service planı sonuçlanır **PremiumV2**.  Ardından uygulama kodunuz, yeni oluşturulan uygulama ve app service planı içinde yeniden dağıtın. İsterseniz, daha sonra ölçeklendirme yapabilen App Service planı gelen **PremiumV2** kaydetmek için maliyetleri ve yine de başarılı bir şekilde yeniden gelecekte kullanarak arka ölçeği oluşturabileceksiniz **PremiumV2**.
- Uygulamanız zaten var çalışıyorsa **Premium** katmanı, uygulamanızı tüm uygulama ayarları, bağlantı dizeleri ve dağıtım yapılandırması kullanan yeni bir app service planı içine kopyalayabilirsiniz sonra **PremiumV2**.

    ![](media/app-service-configure-premium-tier/clone-app.png)

    İçinde **uygulama kopyalama** sayfasında, bir App Service planı kullanarak oluşturabileceğiniz **PremiumV2** bölgede istediğiniz ve uygulama ayarları ve kopyalamak için istediğiniz yapılandırmayı belirtin.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

Uygulama oluşturmaya otomatikleştirebilirsiniz **PremiumV2** katmanı kullanarak betiklerle [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure CLI

Aşağıdaki komut, bir App Service planı oluşturur. _P1V2_. Cloud Shell içinde çalıştırabilirsiniz. Seçeneklerini `--sku` P1V2, olan _P2V2_, ve _P3V2_.

```azurecli-interactive
az appservice plan create \
    --resource-group <resource_group_name> \
    --name <app_service_plan_name> \
    --sku P1V2
```

### <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıdaki komut, bir App Service planı oluşturur. _P1V2_. Seçeneklerini `-WorkerSize` olan _küçük_, _orta_, ve _büyük_.

```powershell
New-AzAppServicePlan -ResourceGroupName <resource_group_name> `
    -Name <app_service_plan_name> `
    -Location <region_name> `
    -Tier "PremiumV2" `
    -WorkerSize "Small"
```
## <a name="more-resources"></a>Diğer kaynaklar

[Azure'da uygulamanın ölçeğini](web-sites-scale.md)  
[Örnek sayısını el ile veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md)
