---
title: 'Oluşturma ve bir Azure expressroute değiştirme: CLI | Microsoft Docs'
description: Bu makalede, oluşturmak, sağlamak, doğrulayın, güncelleştirme, silme ve CLI kullanarak bir expressroute bağlantı hattı yetkisini kaldırma açıklar.
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: cd4e31336fd0e90b13f1c3984de89f24e65b052b
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
ms.locfileid: "23933252"
---
# <a name="create-and-modify-an-expressroute-circuit-using-cli"></a>Oluşturma ve CLI kullanarak bir expressroute bağlantı hattı değiştirme


Bu makalede, komut satırı arabirimi (CLI) kullanarak bir Azure expressroute oluşturmayı açıklar. Bu makalede ayrıca durumu, güncelleştirme veya silme denetleyin ve bir bağlantı hattı yetkisini kaldırma kullanmayı gösterir. ExpressRoute bağlantı hatları ile çalışmak için farklı bir yöntem kullanmak istiyorsanız, aşağıdaki listeden makaleyi seçebilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure portalı](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
> 

## <a name="before-you-begin"></a>Başlamadan önce

* Başlamadan önce, CLI komutlarının en son sürümünü (2.0 veya üzeri) yükleyin. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI 2.0’ı yükleme](/cli/azure/install-azure-cli) ve [Azure CLI 2.0’ı Kullanmaya Başlama](/cli/azure/get-started-with-azure-cli).
* Gözden geçirme [Önkoşullar](expressroute-prerequisites.md) ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.

## <a name="create"></a>Oluşturma ve bir expressroute bağlantı hattı sağlama

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Azure hesabınızda oturum açın ve aboneliğinizi seçin

Yapılandırmanızı başlamak için Azure hesabınızda oturum açın. Aşağıdaki örnekler bağlanmanıza yardımcı olması için kullanın:

```azurecli
az login
```

Hesapla ilişkili abonelikleri kontrol edin.

```azurecli
az account list
```

Bir expressroute bağlantı hattı oluşturmak istediğiniz aboneliği seçin.

```azurecli
az account set --subscription "<subscription ID>"
```

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. Desteklenen sağlayıcılar, konumları ve bant genişlikleri listesini alma

Bir expressroute bağlantı hattı oluşturmadan önce desteklenen bağlantı sağlayıcıları, konumları ve bant seçeneklerini listesi gerekir. CLI komut 'az ağ hızlı rota listesi-service-providers', sonraki adımlarda kullanacağınız bu bilgileri döndürür:

```azurecli
az network express-route list-service-providers
```

Yanıt aşağıdaki örneğe benzer:

```azurecli
[
  {
    "bandwidthsOffered": [
      {
        "offerName": "50Mbps",
        "valueInMbps": 50
      },
      {
        "offerName": "100Mbps",
        "valueInMbps": 100
      },
      {
        "offerName": "200Mbps",
        "valueInMbps": 200
      },
      {
        "offerName": "500Mbps",
        "valueInMbps": 500
      },
      {
        "offerName": "1Gbps",
        "valueInMbps": 1000
      },
      {
        "offerName": "2Gbps",
        "valueInMbps": 2000
      },
      {
        "offerName": "5Gbps",
        "valueInMbps": 5000
      },
      {
        "offerName": "10Gbps",
        "valueInMbps": 10000
      }
    ],
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/expressRouteServiceProviders/",
    "location": null,
    "name": "AARNet",
    "peeringLocations": [
      "Melbourne",
      "Sydney"
    ],
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "tags": null,
    "type": "Microsoft.Network/expressRouteServiceProviders"
  },
```

Yanıt bağlantı sağlayıcınız listelenip listelenmediğini denetleyin. Bir bağlantı hattı oluştururken ihtiyacınız olacak aşağıdaki bilgileri not edin:

* Ad
* PeeringLocations
* BandwidthsOffered

Artık bir expressroute bağlantı hattı oluşturmak hazırsınız.

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute bağlantı hattı oluşturma

> [!IMPORTANT]
> ExpressRoute bağlantı hattınız bir hizmet anahtarı verilen andan itibaren faturalandırılır. Bağlantı sağlayıcı bağlantı hattı sağlamak hazır olduğunda bu işlemi gerçekleştirin.
> 
> 

Bir kaynak grubu zaten yoksa, expressroute bağlantı hattı oluşturmadan önce bir oluşturmanız gerekir. Aşağıdaki komutu çalıştırarak, bir kaynak grubu oluşturabilirsiniz:

```azurecli
az group create -n ExpressRouteResourceGroup -l "West US"
```

Aşağıdaki örnek 200 MB/sn expressroute bağlantı hattı üzerinden Equinix Silikon Vadisi'nde oluşturulacağını gösterir. Farklı bir sağlayıcı ve farklı ayarlar kullanıyorsanız, bu bilgileri isteğiniz yaptığınızda değiştirin. 

SKU ailesi ve doğru SKU katmanı belirttiğinizden emin olun:

* SKU katmanı, bir ExpressRoute standart ya da bir ExpressRoute premium Eklentisi etkin olup olmadığını belirler. Standart SKU veya 'Premium' premium eklenti için almak için ' Standart' belirtebilirsiniz.
* SKU ailesi faturalama türü belirler. Ölçülen veri planı ve 'Unlimiteddata' için 'Metereddata' için bir sınırsız veri planı belirtebilirsiniz. Faturalama 'Metereddata' türünden 'Unlimiteddata' için değiştirebilirsiniz, ancak 'Unlimiteddata' türünden 'Metereddata' için değiştirilemiyor.


ExpressRoute bağlantı hattınız bir hizmet anahtarı verilen andan itibaren faturalandırılır. Aşağıdaki örnek yeni bir hizmet anahtarı için bir istek verilmiştir:

```azurecli
az network express-route create --bandwidth 200 -n MyCircuit --peering-location "Silicon Valley" -g ExpressRouteResourceGroup --provider "Equinix" -l "West US" --sku-family MeteredData --sku-tier Standard
```

Yanıt hizmet anahtarını içerir.

### <a name="4-list-all-expressroute-circuits"></a>4. Tüm ExpressRoute bağlantı hatları listesi

Oluşturduğunuz tüm expressroute bağlantı hatları listesini almak için 'az ağ hızlı rota listesi' komutunu çalıştırın. Bu komutu kullanarak bu bilgileri dilediğiniz zaman alabilir. Tüm devreler listelemek için hiçbir parametre çağırmaya.

```azurecli
az network express-route list
```

Hizmet anahtarınız listelenen *ServiceKey* yanıtının alan.

```azurecli
"allowClassicOperations": false,
"authorizations": [],
"circuitProvisioningState": "Enabled",
"etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
"gatewayManagerEtag": null,
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
"location": "westus",
"name": "MyCircuit",
"peerings": [],
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup",
"serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
"serviceProviderNotes": null,
"serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
},
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

Kullanarak komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamaları alabilirsiniz '-h' parametresi.

```azurecli
az network express-route list -h
```

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. Hizmet anahtarı sağlamak için bağlantı sağlayıcınızı Gönder

'ServiceProviderProvisioningState' hizmet sağlayıcı tarafında sağlama geçerli durumu hakkında bilgi sağlar. Durum Microsoft tarafında durumunu sağlar. Daha fazla bilgi için bkz: [iş akışları makale](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Yeni bir expressroute bağlantı hattı oluşturduğunuzda, bağlantı hattı şu durumda olur:

```azurecli
"serviceProviderProvisioningState": "NotProvisioned"
"circuitProvisioningState": "Enabled"
```

Bağlantı sağlayıcı onu sizin için etkinleştirme sürecinde olduğunda bağlantı hattı için şu durum değiştirir:

```azurecli
"serviceProviderProvisioningState": "Provisioning"
"circuitProvisioningState": "Enabled"
```

Bir expressroute bağlantı hattı kullanabilmek için şu durumda olmalıdır:

```azurecli
"serviceProviderProvisioningState": "Provisioned"
"circuitProvisioningState": "Enabled
```

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Durum ve hattı anahtar durumunu düzenli aralıklarla denetleyin

Durum ve hattı anahtar durumunu denetleme, sağlayıcınız hattınız etkin olduğunda bilmenizi sağlar. Bağlantı hattı yapılandırıldıktan sonra 'ServiceProviderProvisioningState' 'Hazırlandı ', aşağıdaki örnekte gösterildiği gibi görünür:

```azurecli
az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
```

Yanıt aşağıdaki örneğe benzer:

```azurecli
"allowClassicOperations": false,
"authorizations": [],
"circuitProvisioningState": "Enabled",
"etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
"gatewayManagerEtag": null,
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
"location": "westus",
"name": "MyCircuit",
"peerings": [],
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup",
"serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
"serviceProviderNotes": null,
"serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
},
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

### <a name="7-create-your-routing-configuration"></a>7. Yönlendirme yapılandırması oluşturma

Adım adım yönergeler için bkz: [expressroute bağlantı hattı yönlendirme yapılandırması](howto-routing-cli.md) oluşturup hattı eşlemeler değiştirmek için makale.

> [!IMPORTANT]
> Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen sunan bir hizmet sağlayıcısı kullanıyorsanız, Katman 3 Hizmetleri (genellikle bir IP VPN, MPLS gibi), bağlantı sağlayıcınız yönlendirmeyi sizin için yönetir ve yapılandırır.
> 
> 

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. ExpressRoute bağlantı hattına bir sanal ağı bağlama

Ardından, bir sanal ağ, expressroute bağlantı hattına bağlayın. Kullanım [ExpressRoute bağlantı hatları için sanal ağları bağlama](howto-linkvnet-cli.md) makalesi.

## <a name="modify"></a>Bir expressroute bağlantı hattı değiştirme

Bağlantı etkilemeden belirli bir expressroute bağlantı hattı özelliklerini değiştirebilirsiniz. Kapalı kalma süresi olmadan aşağıdaki değişiklikleri yapabilirsiniz:

* Etkinleştirmek veya expressroute bağlantı hattı için ExpressRoute premium eklentisi devre dışı bırakın.
* Sağlanmış kapasite kullanılabilir bağlantı noktası, expressroute bağlantı hattı bant genişliğini artırabilirsiniz. Ancak, bir bağlantı hattının bant genişliğini önceki sürüme indirme desteklenmiyor. 
* Sınırsız veri ölçülen verilerden ölçüm planı değiştirebilirsiniz. Ancak, ölçüm plan sınırsız verilerden ölçülen veri değiştirilmesi desteklenmiyor.
* Etkinleştirme ve devre dışı *izin Klasik işlemleri*.

Sınırlar ve sınırlamalar hakkında daha fazla bilgi için bkz: [ExpressRoute SSS](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisi etkinleştirmek için

Aşağıdaki komutu kullanarak, varolan bağlantı hattınız için ExpressRoute premium eklentisi etkinleştirebilirsiniz:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Premium
```

Bağlantı hattı şimdi etkin ExpressRoute premium eklentisi özellikleri vardır. Komut başarıyla çalıştırıldı hemen premium eklenti özellik faturalama başlayın.

### <a name="to-disable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisi devre dışı bırakmak için

> [!IMPORTANT]
> Standart bağlantı hattı için izin daha büyük olan kaynaklar kullanıyorsanız, bu işlemi başarısız olabilir.
> 
> 

ExpressRoute premium eklentisi devre dışı bırakmadan önce aşağıdaki ölçütleri anlama:

* Standart Premium'dan düşürmek önce devresine bağlı 10'dan az sanal ağlara sahip olacak emin olmanız gerekir. Birden fazla 10 varsa, güncelleştirme isteği başarısız olur ve biz premium oranlarda fatura.
* Tüm sanal ağları diğer coğrafi bölgelerde bağlantısını gerekir. Tüm sanal ağları bağlantısını yok, güncelleştirme isteği başarısız olur ve biz premium oranlarda fatura.
* Yol tablosu özel eşleme için 4. 000'den az yolları olmalıdır. Rota tablosu boyutunuz 4.000 yolları büyükse, BGP oturumu bırakır. Tanıtılan ön ek sayısı 4.000 kadar oturumu yeniden iler hale olmaz.

Aşağıdaki örnek kullanarak var olan bağlantı hattı için ExpressRoute premium eklentisi devre dışı bırakabilirsiniz:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Standard
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>ExpressRoute bağlantı hattı bant genişliğini güncelleştirmek için

Denetimi sağlayıcınız için desteklenen bant genişliği seçenekleri için [ExpressRoute SSS](expressroute-faqs.md). Var olan bağlantı hattınız boyutundan büyük herhangi bir boyutta seçebilirsiniz.

> [!IMPORTANT]
> Varolan bir bağlantı üzerinde yetersiz kapasite ise, expressroute bağlantı hattı yeniden oluşturmanız gerekebilir. Varsa hiçbir ek kapasite kullanılabilir o konumda bağlantı hattı yükseltemezsiniz.
>
> Bir expressroute bağlantı hattı kesintiye uğratmadan bant indiremezsiniz. Bant genişliği eski sürüme düşürmeyi expressroute bağlantı hattı yetkisini kaldırma ve yeni bir expressroute bağlantı hattı yeniden hazırlayana gerektirir.
>

Gereksinim boyutu karar verdikten sonra bağlantı hattınız yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --bandwidth 1000
```

Bağlantı hattınız Microsoft tarafında boyutlandırılır. Ardından, bu değişikliği eşleşecek şekilde kendi tarafında yapılandırmalar güncelleştirileceğini bağlantı sağlayıcınız başvurmanız gerekir. Bu bildirim yaptıktan sonra güncelleştirilmiş bant seçeneği için faturalama başlayın.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>SKU taşımak için sınırsız olarak ölçülen

Aşağıdaki örnek kullanarak ExpressRoute devresi SKU'su değiştirebilirsiniz:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-family UnlimitedData
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Klasik ve Resource Manager ortamları erişimi denetlemek için

' Ndaki yönergeleri gözden [Resource Manager dağıtım modeline taşıma ExpressRoute bağlantı hatları Klasik](expressroute-howto-move-arm.md).

## <a name="delete"></a>Sağlamayı kaldırmayı ve bir expressroute bağlantı hattı silme

Yetkisini kaldırma ve bir expressroute bağlantı hattını silmek için aşağıdaki ölçütleri anladığınızdan emin olun:

* Expressroute bağlantı hattı tüm sanal ağlardan bağlantısını gerekir. Bu işlem başarısız olursa, bağlantı hattı için herhangi bir sanal ağa bağlı olan denetleyin.
* Sağlama durumu ExpressRoute bağlantı hattı hizmet sağlayıcı ise **sağlama** veya **hazırlandı**, kendi tarafında hattı yetkisini kaldırma için hizmet sağlayıcınıza birlikte çalışmalısınız. Kaynakları ayırabilir ve hizmet sağlayıcısı devre sağlama kaldırma işlemi tamamlandıktan ve bize bildiren kadar sizi faturalandırmak devam ediyoruz.
* Hizmet sağlayıcısı hattı sağlaması kaldırılıyor. sağlaması hattı silebilirsiniz. Bir bağlantı hattı sağlaması kaldırılıyor. sağlaması, hizmet sağlayıcısı sağlama durumu kümesine **sağlanmadı**. Bağlantı hattı için fatura durdurur.

Aşağıdaki komutu çalıştırarak, expressroute bağlantı hattı silebilirsiniz:

```azurecli
az network express-route delete  -n MyCircuit -g ExpressRouteResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bağlantı hattınız oluşturduktan sonra aşağıdaki görevleri yaptığınızdan emin olun:

* [Oluşturma ve expressroute bağlantı hattı için yönlendirmeyi değiştirme](howto-routing-cli.md)
* [Sanal ağ, ExpressRoute devresine bağlama](howto-linkvnet-cli.md)