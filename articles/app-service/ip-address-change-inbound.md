---
title: Bir gelen IP adresi değişikliği için - Azure hazırlamayı öğrenin
description: Gelen IP adresiniz, değiştirilecek olacaksa, böylece uygulamanız için değişiklik sonrasında çalışmaya devam yapmanız gerekenler öğrenin.
services: app-service\web
author: tdykstra
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 06/28/2018
ms.author: tdykstra
ms.openlocfilehash: 5d30357e5308de0224590703bb5e68fbe73b882b
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39343449"
---
# <a name="how-to-prepare-for-an-inbound-ip-address-change"></a>Nasıl bir gelen IP adresi değişikliğe hazırlanmak için

Azure App Service uygulamanızın gelen IP adresi ile değişiyor bir bildirim aldıysanız, bu makaledeki yönergeleri izleyin.

## <a name="determine-if-you-have-to-do-anything"></a>Herhangi bir şey gerekip gerekmediğini belirleme

* 1. seçenek: App Service uygulamanızı özel bir etki alanı yoksa, hiçbir eylem gerekmiyor.

* 2. seçenek: yalnızca bir CNAME kaydı (URI'yı işaret eden DNS kaydı), etki alanı kayıt Portalı'nda (üçüncü taraf DNS sağlayıcısı veya Azure DNS) yapılandırılmışsa, herhangi bir eylemi gerekli değildir.

* Seçenek 3: bir A kaydı (doğrudan IP adresine işaret eden DNS kaydı), etki alanı kayıt Portalı'nda (üçüncü taraf DNS sağlayıcısı veya Azure DNS) yapılandırılmışsa, mevcut IP adresi yenisiyle değiştirin. Sonraki bölümde yönergeleri takip ederek yeni IP adresini bulabilirsiniz.

* Seçenek 4: uygulamanız, bir yük dengeleyici, IP filtresi veya uygulamanızın IP adresi gerektiren herhangi bir IP mekanizma ise, mevcut IP adresi yenisiyle değiştirin. Sonraki bölümde yönergeleri takip ederek yeni IP adresini bulabilirsiniz.

## <a name="find-the-new-inbound-ip-address-in-the-azure-portal"></a>Azure portalında yeni bir gelen IP adresi Bul

Portalda, uygulamaya verilen yeni gelen IP adresi bulunduğu **sanal IP adresi** alan. Uygulamanıza, artık bu yeni bir IP adresi hem de eski bir bağlı ve daha sonra eski kesilir.

1.  [Azure portalı](https://portal.azure.com) açın.

2.  Sol taraftaki gezinti menüsünde seçin **uygulama hizmetleri**.

3.  App Service uygulamanızı listeden seçin.

4.  Uygulamayı bir işlev uygulaması, bakın [işlev uygulaması gelen IP adresi](../azure-functions/ip-addresses.md#function-app-inbound-ip-address).

4.  Altında **ayarları** başlık tıklayın **özellikleri** sol gezinti ve Bul etiketlenmiş bölümü **sanal IP adresi**.

5. IP adresini kopyalayın ve etki alanı kaydı veya IP mekanizması yeniden yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure tarafından başlatılan bir IP adresi değişikliği hazırlama açıklanmıştır. Azure App Service'te IP adresleri hakkında daha fazla bilgi için bkz. [gelen ve giden IP adresleri, Azure App Service'te](app-service-ip-addresses.md).
