---
title: IP adreslerini Azure App Service'te | Microsoft Docs
description: Açıklar nasıl gelen ve giden IP adresleri, uygulama hizmeti ve bunları uygulamanız için bilgi bulmak nasıl kullanılır.
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
ms.openlocfilehash: 906a5d511615c57b6ff807ac240a838c63917e66
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31789948"
---
# <a name="inbound-and-outbound-ip-addresses-in-azure-app-service"></a>Azure App Service'te gelen ve giden IP adresleri

[Azure uygulama hizmeti](app-service-web-overview.md) dışında için çok kiracılı bir hizmet olan [App Service ortamları](environment/intro.md). Bir uygulama hizmeti ortamı'nda olmayan uygulamaları (değil, [yalıtılmış katmanı](https://azure.microsoft.com/pricing/details/app-service/)) diğer uygulamalarla paylaşımı ağ altyapısı. Sonuç olarak, gelen ve giden IP adresleri, bir uygulamanın farklı olabilir ve belirli durumlarda bile değiştirebilirsiniz. 

[Uygulama hizmeti ortamları](environment/intro.md) Adanmış ağ altyapıları kullanan, hem gelen ve giden bağlantılar için bir uygulama hizmeti ortamı'nda çalışan uygulamalar statik almak için ayrılmış IP adresleri.

## <a name="when-inbound-ip-changes"></a>Gelen IP değiştiğinde

Ölçeklendirilmiş örneklerinin sayısına bakılmaksızın, her uygulamanın tek bir gelen IP adresi vardır. Aşağıdaki eylemlerden birini gerçekleştirdiğinizde gelen IP adresi değişebilir:

- Bir uygulamayı silin ve farklı bir kaynak grubu içinde oluşturun.
- Bir kaynak grubunda son uygulama Sil _ve_ bölge birleşim ve yeniden oluşturun.
- Sertifika yenileme sırasında gibi mevcut bir SSL bağlaması Sil (bkz [sertifikalarını yeniler](app-service-web-tutorial-custom-ssl.md#renew-certificates)).

## <a name="get-static-inbound-ip"></a>Statik gelen IP Al

Bazı durumlarda, uygulamanız için ayrılmış, statik IP adresi isteyebilirsiniz. Gelen bir statik IP adresi almak için yapılandırmanız gereken bir [IP temelli SSL bağlama](app-service-web-tutorial-custom-ssl.md#bind-your-ssl-certificate). Uygulamanızın güvenliğini sağlamak için SSL işlevselliği gerçekten gerek duymuyorsanız, bile bu bağlama için otomatik olarak imzalanan bir sertifika karşıya yükleyebilirsiniz. Bir IP tabanlı SSL bağlaması sertifika IP adresi kendisi, statik IP adresi durum yapmak için bu uygulama hizmeti hükümleri bağlıdır. 

## <a name="when-outbound-ips-change"></a>Giden IP'leri değiştirdiğinizde

Ölçeklendirilmiş örneklerinin sayısına bakılmaksızın, her uygulamanın belirli bir zamanda giden IP adresi kümesi sayısı vardır. Giden herhangi bir arka uç veritabanı gibi App Service uygulaması bağlantısından giden IP adreslerinden biri kaynak IP adresini kullanır. Hangi IP adresini kendi güvenlik duvarı tüm giden IP adreslerine, uygulamanızın arka uç hizmetinizin açmanız gerekir böylece giden bağlantıyı kurmak için belirli bir uygulamanın örneği önceden kullanacak bilemezsiniz.

Giden IP kümesi adresleri için uygulama değişikliklerinizi uygulamanızı alt katmanları arasında ölçeklendirdiğinizde (**temel**, **standart**, ve **Premium**) ve  **Premium V2** katmanı.

Uygulamanızı kullanabilir, bakarak fiyatlandırma katmanlarına bağımsız olarak tüm olası giden IP adresleri kümesini bulabilirsiniz `possibleOutboundIPAddresses` özelliği. Bkz: [Bul giden IP'leri](#find-outbound-ips).

## <a name="find-outbound-ips"></a>Giden IP Bul

Uygulamanızı Azure portalında şu anda kullandığı giden IP adresleri bulmak için tıklatın **özellikleri** uygulamanızın sol gezinti içinde. 

Aşağıdaki komutu çalıştırarak aynı bilgiyi bulabilirsiniz [bulut Kabuk](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
```

Olası tüm bulmak için giden IP adreslerini fiyatlandırma katmanlarına, bağımsız olarak uygulamanız için aşağıdaki komutu çalıştırın [bulut Kabuk](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```

## <a name="next-steps"></a>Sonraki adımlar

Kaynak IP adresleri ile gelen trafiği kısıtlamayı öğrenin.

> [!div class="nextstepaction"]
> [Statik IP kısıtlamaları](app-service-ip-addresses.md)
