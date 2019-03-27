---
title: Gelen/giden IP adresleri - Azure App Service | Microsoft Docs
description: Açıklayan nasıl gelen ve giden IP adreslerini, App Service ve bunları uygulamanız için bilgi bulmak kullanılır.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 96f580532d9ea45dd767e32c2451243e83af66ea
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58480813"
---
# <a name="inbound-and-outbound-ip-addresses-in-azure-app-service"></a>Azure App Service'te gelen ve giden IP adresleri

[Azure App Service](overview.md) dışında çok kiracılı bir hizmet olan [App Service ortamları](environment/intro.md). Bir App Service Ortamı'nda olmayan uygulamaları (değil [yalıtılmış katmanı](https://azure.microsoft.com/pricing/details/app-service/)) diğer uygulamalarla paylaşım ağ altyapısı. Sonuç olarak, gelen ve giden IP adresleri, bir uygulamanın farklı olabilir ve belirli durumlarda bile değiştirebilirsiniz. 

[App Service ortamları](environment/intro.md) Adanmış ağ altyapıları kullanan, bir App Service ortamında çalışan uygulamalar, statik alabilmeniz gelen ve giden bağlantılar için hem de ayrılmış IP adresleri.

## <a name="when-inbound-ip-changes"></a>Gelen IP değiştiğinde

Ölçeği genişletilen örnekleri sayısı ne olursa olsun, her uygulamanın tek bir gelen IP adresi vardır. Aşağıdaki eylemlerden birini gerçekleştirdiğinizde gelen IP adresi değişebilir:

- Bir uygulamayı silin ve farklı bir kaynak grubunda yeniden oluşturun.
- Bir kaynak grubundaki son uygulamayı silmek _ve_ bölge birleşimi ve yeniden oluşturun.
- Sertifika yenileme sırasında gibi mevcut bir SSL bağlaması Sil (bkz [sertifikaları yenileme](app-service-web-tutorial-custom-ssl.md#renew-certificates)).

## <a name="get-static-inbound-ip"></a>Statik bir gelen IP alma

Bazen, uygulamanız için adanmış ve statik bir IP adresi isteyebilirsiniz. Statik bir gelen IP adresini almak için yapılandırmanız gereken bir [IP tabanlı SSL bağlaması](app-service-web-tutorial-custom-ssl.md#bind-your-ssl-certificate). Uygulamanızı güvenli hale getirmek için SSL işlevi gerçekten ihtiyacınız yoksa, bu bağlama için otomatik olarak imzalanan bir sertifika da karşıya yükleyebilirsiniz. Bir IP tabanlı SSL bağlaması içinde bir statik IP adresi gerçekleşecek şekilde bunu App Service hükümlerine IP adresini, kendi sertifika bağlıdır. 

## <a name="when-outbound-ips-change"></a>Giden IP'ler değiştiğinde

Ölçeği genişletilen örnekleri sayısı ne olursa olsun, her uygulamanın belirli bir zamanda sayıda giden IP adresleri vardır. App Service uygulamasında, bir arka uç veritabanı gibi herhangi bir giden bağlantı giden IP adreslerinden birini kaynak IP adresini kullanır. Hangi IP adresi, arka uç hizmetinize uygulamanızın tüm giden IP adresleri için güvenlik duvarı açmanız gerekir böylece giden bağlantı için belirli bir uygulamanın örneği önceden kullanacağı bilemezsiniz.

Kümesi giden IP adresleri uygulama değişiklikleriniz uygulamanızı daha düşük Katmanlar arasındaki ölçeklediğinizde (**temel**, **standart**, ve **Premium**) ve  **Premium V2** katmanı.

Uygulamanızı kullanabilir, bakarak fiyatlandırma katmanları bağımsız olarak tüm olası giden IP adresleri kümesini bulabilirsiniz `possibleOutboundIPAddresses` özelliği veya **ek giden IP adresleri** alanındaki **özellikleri**  Azure portalındaki dikey penceresinde. Bkz: [Bul giden IP'ler](#find-outbound-ips).

## <a name="find-outbound-ips"></a>Giden IP'ler bulun

Uygulamanızı Azure portalında şu anda kullandığı giden IP adresleri bulmak için tıklatın **özellikleri** uygulamanızın sol gezinti bölmesinde. İçinde listelenen **giden IP adresleri** alan.

Aşağıdaki komutu çalıştırarak aynı bilgileri bulabilirsiniz [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).OutboundIpAddresses
```

Bulunacak _tüm_ fiyatlandırma katmanları, bağımsız olarak uygulamanız için olası giden IP adresleri tıklayın **özellikleri** uygulamanızın sol gezinti bölmesinde. İçinde listelenen **ek giden IP adresleri** alan.

Aşağıdaki komutu çalıştırarak aynı bilgileri bulabilirsiniz [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).PossibleOutboundIpAddresses
```

## <a name="next-steps"></a>Sonraki adımlar

Kaynak IP adreslerinden gelen trafiği kısıtlamak öğrenin.

> [!div class="nextstepaction"]
> [Statik IP kısıtlamaları](app-service-ip-restrictions.md)
