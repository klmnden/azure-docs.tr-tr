---
title: Azure işlevleri'nde IP adresleri
description: İşlev uygulamaları için gelen ve giden IP adresleri bulmayı öğrenin ve hangi değiştirmek neden olur.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: glenga
ms.openlocfilehash: 83e5a15d8a7f9c01f6a180ebceb715600b8a39db
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61035873"
---
# <a name="ip-addresses-in-azure-functions"></a>Azure işlevleri'nde IP adresleri

Bu makalede, işlev uygulamaları için IP adreslerini ilgili aşağıdaki konular açıklanmaktadır:

* IP adreslerini şu anda kullanımda bir işlev uygulaması için nasıl bulacağınızı.
* Hangi uygulamanın IP adreslerinin değiştirilmesi bir işlev neden olur.
* Bir işlev uygulaması erişebilmeniz için IP adreslerini kısıtlamak nasıl.
* Ayrılmış IP adresleri için bir işlev uygulaması edinme.

İşlev uygulamaları, bireysel işlevleri ile değil IP adresleri ilişkilendirilir. Gelen HTTP isteklerinin tek tek işlevleri çağırmak için gelen bir IP adresi kullanamazsınız; Bunlar, varsayılan etki alanı adı (functionappname.azurewebsites.net) veya özel etki alanı adı kullanmanız gerekir.

## <a name="function-app-inbound-ip-address"></a>İşlev uygulaması gelen IP adresi

Her işlev uygulaması, tek bir gelen IP adresi vardır. Bu IP adresini bulmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. İşlev uygulamasına gidin.
3. Seçin **Platform özellikleri**.
4. Seçin **özellikleri**, gelen IP adresi altında görünür **sanal IP adresi**.

## <a name="find-outbound-ip-addresses"></a>İşlev uygulaması giden IP adresleri

Her işlev uygulaması, kullanılabilir giden IP adresleri kümesi vardır. Herhangi bir giden bağlantı bir işlevden gibi bir arka uç veritabanı kaynak IP adresi kullanılabilir giden IP adreslerinden birini kullanır. Hangi IP adresini belirli bir bağlantı önceden kullanacak bilemezsiniz. Bu nedenle, arka uç hizmetinize tüm işlevi uygulamanın giden IP adresleri için güvenlik duvarı açmanız gerekir.

Bir işlev uygulaması için kullanılabilen giden IP adresleri bulmak için:

1. Oturum [Azure kaynak Gezgini](https://resources.azure.com).
2. Seçin **abonelikler > {subscription} > sağlayıcılar > Microsoft.Web > siteleri**.
3. JSON panelinde site Bul bir `id` bitiren işlev uygulamanızın adını özelliği.
4. Bkz: `outboundIpAddresses` ve `possibleOutboundIpAddresses`. 

Dizi `outboundIpAddresses` işlev uygulaması için şu anda kullanılabilir. Dizi `possibleOutboundIpAddresses` kullanılabilir IP adreslerini içeren yalnızca işlev uygulaması [ölçekler için diğer fiyatlandırma katmanlarından](#outbound-ip-address-changes).

Kullanılabilir giden IP adreslerini bulmak için alternatif bir yolu kullanmaktır [Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```
> [!NOTE]
> Üzerinde çalıştığı bir işlev uygulaması, [tüketim planı](functions-scale.md#consumption-plan) olan ölçeği, yeni giden IP adresleri aralığı atanabilir. Tüketim planı üzerinde çalışırken, tüm veri merkezinin beyaz listeye gerekebilir.

## <a name="data-center-outbound-ip-addresses"></a>Giden IP adreslerini veri merkezi

Gerekirse beyaz liste giden IP adresleri, işlev uygulamaları tarafından kullanılan, başka bir seçenek beyaz liste ile işlev uygulamaları veri merkezi (Azure bölgesi). Yapabilecekleriniz [tüm Azure veri merkezleri için IP adreslerini listeler bir JSON dosyası indirmeniz](https://www.microsoft.com/en-us/download/details.aspx?id=56519). Ardından işlev uygulamanızı çalışan bölge için geçerli JSON parça bulun.

Örneğin, bu Batı Avrupa'da JSON parçası aşağıdaki gibi görünmelidir.

```
{
  "name": "AzureCloud.westeurope",
  "id": "AzureCloud.westeurope",
  "properties": {
    "changeNumber": 9,
    "region": "westeurope",
    "platform": "Azure",
    "systemService": "",
    "addressPrefixes": [
      "13.69.0.0/17",
      "13.73.128.0/18",
      ... Some IP addresses not shown here
     "213.199.180.192/27",
     "213.199.183.0/24"
    ]
  }
}
```

 Bu dosya güncelleştirildiğinde ve ne zaman IP adresleri hakkında daha fazla bilgi için genişletme **ayrıntıları** bölümünü [Download Center sayfası](https://www.microsoft.com/en-us/download/details.aspx?id=56519).

## <a name="inbound-ip-address-changes"></a>Gelen IP adresi değişiklikleri

Gelen IP adresi **olabilir** ne zaman değiştirmek:

- Bir işlev uygulaması silin ve farklı bir kaynak grubunda yeniden oluşturun.
- Bir kaynak grubunda ve bölgede birlikte son işlev uygulamasını silin ve yeniden oluşturun.
- Sırasında olduğu gibi bir SSL bağlaması Sil [sertifika yenileme](../app-service/app-service-web-tutorial-custom-ssl.md#renew-certificates)).

İşlev uygulamanız çalıştığında bir [tüketim planı](functions-scale.md#consumption-plan), olanları listelenen gibi tüm eylemler kazanmadı olduğunda gelen IP adresi değişebilir.

## <a name="outbound-ip-address-changes"></a>Giden IP adresi değişiklikleri

Bir işlev uygulaması olduğunda değişebilir kümesi kullanılabilir giden IP adresleri:

* Gelen IP adresi değişebilir tüm eylemleri.
* App Service planınızın fiyatlandırma katmanını değiştirin. Uygulamanızı kullanabilir, tüm fiyatlandırma katmanları için tüm olası giden IP adresleri listesi yer `possibleOutboundIPAddresses` özelliği. Bkz: [Bul giden IP'ler](#find-outbound-ip-addresses).

İşlev uygulamanız çalıştığında bir [tüketim planı](functions-scale.md#consumption-plan), olanları listelenen gibi tüm eylemler kazanmadı olduğunda giden IP adresi değişebilir.

Giden IP adresi değişikliği kasıtlı olarak zorlamak için:

1. App Service planınızın standart ve Premium v2 fiyatlandırma katmanları arasında yukarı veya aşağı ölçeklendirin.
2. 10 dakika bekleyin.
3. Geri başladığınız ölçeklendirin.

## <a name="ip-address-restrictions"></a>IP adresi sınırlamaları

İzin vermek veya bir işlev uygulaması için erişimi reddetmek istediğiniz IP adreslerinden oluşan bir liste yapılandırabilirsiniz. Daha fazla bilgi için [Azure uygulama hizmeti statik IP kısıtlamaları](../app-service/app-service-ip-restrictions.md).

## <a name="dedicated-ip-addresses"></a>Ayrılmış IP adresleri

Statik ve ayrılmış IP adresleri ihtiyacınız varsa bunu öneririz [App Service ortamları](../app-service/environment/intro.md) ( [yalıtılmış katmanı](https://azure.microsoft.com/pricing/details/app-service/) App Service planı). Daha fazla bilgi için [App Service ortamı IP adresleri](../app-service/environment/network-info.md#ase-ip-addresses) ve [bir App Service ortamına gelen trafiği denetleme](../app-service/environment/app-service-app-service-environment-control-inbound-traffic.md).

İşlev uygulamanızı bir App Service Ortamı'nda çalışıp çalışmadığını öğrenmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. İşlev uygulamasına gidin.
3. Seçin **genel bakış** sekmesi.
4. App Service planı katmanı altında görünür **App Service planı/fiyatlandırma katmanı**. Fiyatlandırma katmanı App Service ortamı olan **yalıtılmış**.
 
Alternatif olarak, kullandığınız [Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query sku --output tsv
```

App Service ortamı `sku` olduğu `Isolated`.

## <a name="next-steps"></a>Sonraki adımlar

Yaygın bir nedeni, IP değişiklikleri işlevi uygulama ölçeklendirme değişiklikleri var. [İşlev uygulamasını ölçeklendirme hakkında daha fazla bilgi](functions-scale.md).
