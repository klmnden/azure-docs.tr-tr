---
title: "Bir expressroute bağlantı hattı değiştirin: PowerShell: Azure Klasik | Microsoft Docs"
description: "Bu makalede durumu, güncelleştirme veya silme denetlemek ve ExpressRoute Klasik dağıtım modeli hattınız yetkisini kaldırma için adımlarda size yol gösterir."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0134d242-6459-4dec-a2f1-4657c3bc8b23
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/08/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 457bb74fa15d31fecbf668038ac880cafb8a897d
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="modify-an-expressroute-circuit-using-powershell-classic"></a>PowerShell (Klasik) kullanarak bir expressroute bağlantı hattı değiştirme

> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure portalı](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Bu makalede ayrıca durumu, güncelleştirme veya silme denetleyin ve bir expressroute bağlantı hattı yetkisini kaldırma kullanmayı gösterir.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Azure Hizmet Yönetimi (SM) PowerShell modülleri en son sürümlerini'ndaki yönergeleri izleyin yükleme [Azure PowerShell cmdlet'leri ile çalışmaya başlama](/powershell/azure/overview) bilgisayarınızı kullanacak şekilde yapılandırma hakkında adım adım yönergeler için Azure PowerShell modülleri.

## <a name="get-the-status-of-a-circuit"></a>Devrenin durumunu Al

Herhangi bir zamanda bu bilgileri kullanarak alabilirsiniz `Get-AzureCircuit` cmdlet'i. Hiçbir parametre olmadan çağrıyı yapan tüm devreler listeler.

```powershell
Get-AzureDedicatedCircuit

Bandwidth                        : 200
CircuitName                      : MyTestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Standard
Status                           : Enabled

Bandwidth                        : 1000
CircuitName                      : MyAsiaCircuit
Location                         : Singapore
ServiceKey                       : #################################
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Standard
Status                           : Enabled
```

Çağrısına parametre olarak hizmet anahtarını geçirerek belirli bir expressroute bağlantı hattı hakkında bilgi alabilirsiniz.

```powershell
Get-AzureDedicatedCircuit -ServiceKey "*********************************"

Bandwidth                        : 200
CircuitName                      : MyTestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Standard
Status                           : Enabled
```

Aşağıdaki örnek çalıştırarak tüm parametrelerin ayrıntılı açıklamaları alabilirsiniz:

```powershell
get-help get-azurededicatedcircuit -detailed
```

## <a name="modify-a-circuit"></a>Bir bağlantı hattı değiştirme

Bağlantı etkilemeden belirli bir expressroute bağlantı hattı özelliklerini değiştirebilirsiniz.

Bunu kapalı kalma süresi olmadan aşağıdaki görevleri gerçekleştirebilirsiniz:

* Etkinleştirmek veya devre dışı bir expressroute bağlantı hattı için ExpressRoute premium eklentisi.
* Sağlanmış kapasite kullanılabilir bağlantı noktası, expressroute bağlantı hattı bant genişliğini artırır. Bir bağlantı hattının bant genişliğini önceki sürüme indirme desteklenmiyor. 
* Ölçüm plan sınırsız veri ölçülen verilerden değiştirin. Ölçüm plan sınırsız verilerden ölçülen verileri değiştirme desteklenmez.
* Etkinleştirme ve devre dışı *izin Klasik işlemleri*.

Başvurmak [ExpressRoute SSS](expressroute-faqs.md) sınırlar ve sınırlamalar hakkında daha fazla bilgi için.

### <a name="enable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisi etkinleştir

Aşağıdaki PowerShell cmdlet'ini kullanarak, varolan bağlantı hattınız için ExpressRoute premium eklentisi etkinleştirebilirsiniz:

```powershell
Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

Bandwidth                        : 1000
CircuitName                      : TestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Premium
Status                           : Enabled
```

Bağlantı hattınız şimdi etkin ExpressRoute premium eklentisi özellikleri sahip olur. Premium eklenti yetenek için fatura komutu başarıyla çalıştırıldı hemen başlar.

### <a name="disable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisi devre dışı bırak

> [!IMPORTANT]
> Standart bağlantı hattı için izin daha büyük olan kaynaklar kullanıyorsanız, bu işlemi başarısız olabilir.
> 
> 

#### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Standart Premium'dan düşürmek önce devresine bağlı sanal ağlar sayısı 10'dan az olduğundan emin olun. Bunu yapmazsanız, güncelleştirme isteği başarısız olursa ve, faturalandırılır premium oranları.
* Tüm sanal ağları diğer coğrafi bölgelerde bağlantısını gerekir. Yoksa, güncelleştirme isteği başarısız olur ve, faturalandırılır premium oranları.
* Yol tablosu özel eşleme için 4. 000'den az yolları olmalıdır. Rota tablosu boyutunuz 4.000 yolları büyükse, BGP oturumu bırakır ve 4.000 tanıtılan ön ek sayısı gider kadar yeniden iler hale olmaz.

#### <a name="to-disable-the-premium-add-on"></a>Premium eklentisi devre dışı bırakmak için

Aşağıdaki PowerShell cmdlet'ini kullanarak var olan bağlantı hattınız için ExpressRoute premium eklentisi devre dışı bırakabilirsiniz:

```powershell

Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

Bandwidth                        : 1000
CircuitName                      : TestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Standard
Status                           : Enabled
```

### <a name="update-the-expressroute-circuit-bandwidth"></a>Güncelleştirme ExpressRoute devresi bant genişliği

Denetleme [ExpressRoute SSS](expressroute-faqs.md) desteklenen sağlayıcınız için bant seçenekleri. Sunucudaki fiziksel bağlantı noktası (hattınız oluşturulduğu) izin verdiği sürece, varolan bağlantı hattınız boyutundan büyük herhangi bir boyutta seçebilirsiniz.

> [!IMPORTANT]
> Varolan bir bağlantı üzerinde yetersiz kapasite ise expressroute bağlantı hattı yeniden başlatmanız gerekebilir. Varsa hiçbir ek kapasite kullanılabilir o konumda bağlantı hattı yükseltemezsiniz.
>
> Bir expressroute bağlantı hattı kesintiye uğratmadan bant indiremezsiniz. Bant genişliği eski sürüme düşürmeyi expressroute bağlantı hattı yetkisini kaldırma ve yeni bir expressroute bağlantı hattı yeniden hazırlayana gerektirir.
> 
> 

#### <a name="resize-a-circuit"></a>Bir bağlantı hattı yeniden boyutlandırma

Gereksinim boyutu karar verdikten sonra bağlantı hattınız yeniden boyutlandırmak için aşağıdaki komutu kullanabilirsiniz:

```powershell
Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

Bandwidth                        : 1000
CircuitName                      : TestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Standard
Status                           : Enabled
```

Bağlantı hattınız Microsoft tarafında boyuta sahip sonra bu değişikliği eşleşecek şekilde kendi tarafında yapılandırmalar güncelleştirileceğini bağlantı sağlayıcınız başvurmanız gerekir. Üzerinde bu noktasından güncelleştirilmiş bant seçeneği için faturalama başlar.

Bağlantı hattı bant genişliğini artırma olduğunda aşağıdaki hata görürseniz, bu var. Varolan hattınız oluşturulduğu fiziksel bağlantı noktası üzerinde hiçbir yeterli bant genişliği sol anlamına gelir. Bağlantı hattı silin ve yeni bir bağlantı hattı ihtiyacınız boyutunun oluşturmanız gerekir.

```powershell
Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
update operation
At line:1 char:1
+ Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
  + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
```

## <a name="deprovision-and-delete-a-circuit"></a>Yetkisini kaldırma ve bir bağlantı hattı Sil

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Expressroute bağlantı hattı başarılı olması bu işlem için tüm sanal ağlardan bağlantısını gerekir. Bu işlem başarısız olursa, devresine bağlı sanal ağlar olup olmadığını denetleyin.
* Sağlama durumu ExpressRoute bağlantı hattı hizmet sağlayıcı ise **sağlama** veya **hazırlandı** kendi tarafında hattı yetkisini kaldırma için hizmet sağlayıcınıza birlikte çalışmalısınız. Kaynakları ayırabilir ve hizmet sağlayıcısı devre sağlama kaldırma işlemi tamamlandıktan ve bize bildiren kadar sizi faturalandırmak devam ediyoruz.
* Hizmet sağlayıcısı hattı sağlaması kaldırılıyor. sağlaması değilse (sağlama durumu hizmet sağlayıcısı kümesine **sağlanmadı**), bağlantı hattı sonra silebilirsiniz. Bağlantı hattı için fatura durdurur.

#### <a name="delete-a-circuit"></a>Bir bağlantı hattı Sil

Aşağıdaki komutu çalıştırarak, expressroute bağlantı hattı silebilirsiniz:

```powershell
Remove-AzureDedicatedCircuit -ServiceKey "*********************************"
```