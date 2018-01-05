---
title: "URL yönlendirme kuralları - Azure CLI 2.0 kullanarak bir uygulama ağ geçidi oluşturma | Microsoft Docs"
description: "Bu sayfayı oluşturmak ve URL yönlendirme kurallarını kullanarak bir uygulama ağ geçidi yapılandırma hakkında yönergeler sağlar."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.openlocfilehash: 10d01d5d80e2d111d6b39598eed3612f80162b23
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-application-gateway-by-using-path-based-routing-with-azure-cli-20"></a>Azure CLI 2.0 ile yol tabanlı yönlendirme kullanarak bir uygulama ağ geçidi oluşturma

> [!div class="op_single_selector"]
> * [Azure portalı](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

URL yolu tabanlı yönlendirme, HTTP isteklerini URL yola göre yollar ilişkilendirin. Uygulama ağ geçidi listelenen URL için yapılandırılmış bir arka uç sunucu havuzuna bir yolu yoktur ve ardından ağ trafiğini tanımlı havuzuna gönderir olup olmadığını denetler. URL yolu tabanlı yönlendirme için bir ortak Yük Dengeleme isteklerini farklı arka uç sunucu havuzu için farklı içerik türleri için kullanılır.

Azure uygulama ağ geçidini kullanan iki kural türü: basic ve URL yolunu tabanlı kurallar. Temel kural türünü, arka uç havuzları hepsini hizmetidir. Hepsini dağıtım yanı sıra yol tabanlı kurallar, istenen URL yolu desenini de uygun arka uç havuzu seçmek için kullanın.

## <a name="scenario"></a>Senaryo

Aşağıdaki örnekte, bir uygulama ağ geçidi trafiği contoso.com için iki arka uç sunucu havuzu ile hizmet eder: varsayılan sunucu havuzu ve bir görüntü sunucu havuzu.

İstekleri için http://contoso.com/image * görüntü sunucu havuzuna yönlendirilen (**imagesBackendPool**). Yol deseni eşleşmezse, uygulama ağ geçidi varsayılan sunucu havuzuna seçer (**appGatewayBackendPool**).

![URL rota](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Açık **Microsoft Azure komut istemi** ve oturum açın:

```azurecli
az login -u "username"
```

> [!NOTE]
> Aynı zamanda `az login` aka.ms/devicelogin adresindeki kodunu girerek gerektirir cihaz oturum açma anahtarı olmadan.

Önceki komutu girdikten sonra bir kodu alırsınız. Oturum açma işlemine devam etmek için bir tarayıcıda https://aka.ms/devicelogin gidin.

![cmd gösteren cihaz oturum açma][1]

Tarayıcıda, aldığınız kodu girin. Bir oturum açma sayfasına yönlendirir.

![Tarayıcı kodu girin][2]

Oturum açmak için kodu girin ve ardından devam etmek için tarayıcıyı kapatın.

![başarıyla oturum açıldı][3]

## <a name="add-a-path-based-rule-to-an-existing-application-gateway"></a>Var olan bir uygulama ağ geçidi için bir yol tabanlı Kuralı Ekle

Aşağıdaki adımlar var olan bir uygulama ağ geçidi için bir yol tabanlı kuralı eklemeyi gösterir.
### <a name="create-a-new-back-end-pool"></a>Yeni bir arka uç havuzu oluştur

Uygulama ağ geçidi ayarlarını yapılandırmanız **imagesBackendPool** arka uç havuzundaki yük dengeli ağ trafiği için. Bu örnekte, yeni arka uç havuzu farklı arka uç havuzu ayarlarını yapılandırın. Her bir arka uç havuzu kendi ayarı olabilir. Yol tabanlı kurallar arka uç HTTP ayarları doğru arka uç havuzu üyelerine trafiğini yönlendirmek için kullanın. Bu trafik arka uç havuzu üyelerine gönderirken kullanılan bağlantı noktası ve protokolü belirler. Arka uç HTTP ayarları da tanımlama bilgisi tabanlı oturum belirler.  Etkinleştirilirse, tanımlama bilgisi tabanlı oturum benzeşimi trafiği için aynı arka uç her paket için olarak önceki istekleri gönderir.

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port-for-an-application-gateway"></a>Bir uygulama ağ geçidi için yeni ön uç bağlantı noktası oluşturma

Ön uç bağlantı noktası yapılandırma nesnesi, uygulama ağ geçidi dinleyicisi trafiğini dinleyen hangi bağlantı noktasını tanımlamak için bir dinleyici tarafından kullanılır.

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a>Yeni bir dinleyici oluşturun

Bu adım, gelen ağ trafiğini almak için kullanılan genel IP adresi ve bağlantı noktası için dinleyiciyi yapılandırır. Aşağıdaki örnek, önceden yapılandırılmış ön uç IP yapılandırmasını, ön uç bağlantı noktası yapılandırması ve bir protokol (http veya https büyük küçük harfe duyarlı) alır ve dinleyiciyi yapılandırır. Bu örnekte, dinleyiciyi Bu senaryoda, oluşturulan HTTP trafiği ortak IP adresi üzerinde 82 numaralı bağlantı noktasında dinler.

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-the-url-path-map"></a>URL yolu eşlemesi oluşturma

Bu adım, yol ve gelen trafiği işlemek için atanmış arka uç havuzuna arasındaki eşlemeyi tanımlamak için uygulama ağ geçidi tarafından kullanılan göreli URL yolu yapılandırır.

> [!IMPORTANT]
> Her bir yol ile başlamalıdır "/", ve bir yıldız işareti izin verilen tek sonunda yerdir. Geçerli örnekler /xyz, /xyz* veya/xyz / *. Yol Eşleştirici sat dize ilk sonra herhangi bir metin içermiyor "?" veya "#" ve bu karakterleri izin verilmez. 

Aşağıdaki örnek, bir kural için/görüntüleri/oluşturur * için arka uç trafik yönlendirme, yol **imagesBackendPool**. Bu kural her kümesi URL'leri trafiği arka ucuna yönlendirilir sağlar. Http://adatum.com/images/figure1.jpg Örneğin, gider **imagesBackendPool**. Yolun önceden tanımlanmış bir yol kurallardan herhangi birinin eşleşmiyorsa, kural yol haritası yapılandırmasını varsayılan arka uç adres havuzu da yapılandırır. Http://adatum.com/shoppingcart/test.html Örneğin, gider **pool1** eşleşmeyen trafiği için varsayılan havuzu olarak tanımlanmadığından.

```azurecli-interactive
az network application-gateway url-path-map create \
--gateway-name AdatumAppGateway \
--name imagespathmap \
--paths /images/* \
--resource-group myresourcegroup2 \
--address-pool imagesBackendPool \
--default-address-pool appGatewayBackendPool \
--default-http-settings appGatewayBackendHttpSettings \
--http-settings appGatewayBackendHttpSettings \
--rule-name images
```

## <a name="next-steps"></a>Sonraki adımlar

Güvenli Yuva Katmanı (SSL) yük boşaltma hakkında bilgi edinmek istiyorsanız, bkz: [SSL yük boşaltımı için bir uygulama ağ geçidi](application-gateway-ssl-cli.md).


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
