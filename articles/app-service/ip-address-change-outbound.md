---
title: Giden IP adresi değişikliği için - Azure hazırlamayı öğrenin
description: Giden IP adresiniz, değiştirilecek olacaksa, böylece uygulamanız için değişiklik sonrasında çalışmaya devam yapmanız gerekenler öğrenin.
services: app-service\web
author: tdykstra
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 06/28/2018
ms.author: tdykstra
ms.openlocfilehash: a68085ee571654bc1bff2ea23303a02bfb2a9b0d
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39343493"
---
# <a name="how-to-prepare-for-an-outbound-ip-address-change"></a>Giden IP adresi değişikliği için hazırlama

Azure App Service uygulamanızı giden IP adreslerini değiştirerek bir bildirim aldıysanız, bu makaledeki yönergeleri izleyin.

## <a name="determine-if-you-have-to-do-anything"></a>Herhangi bir şey gerekip gerekmediğini belirleme

* 1. seçenek: App Service uygulamanızı IP filtreleme, açık ekleme listesi ya da özel işleme giden trafik yönlendirme veya güvenlik duvarı gibi kullanmıyorsa, hiçbir işlem gerekli değildir.

* 2. seçenek: uygulamanızı (aşağıdaki örneklere bakın) giden IP adresleri için özel işlem varsa, mevcut görünen yeni giden IP adresleri ekleyin. Mevcut IP adreslerini değiştirin yok. Sonraki bölümde yönergeleri takip ederek yeni giden IP adresleri bulabilirsiniz.

  Örneğin, bir giden IP adresi açıkça uygulamanızı dışında bir güvenlik duvarı eklenebilir veya bir dış ödeme hizmet uygulamanız için giden IP adresi içeren bir izin verilenler olabilir. Giden adresinizi uygulamanızı dışında herhangi bir yeri listesindeki yapılandırılmışsa, değiştirmesi gerekir.

## <a name="find-the-outbound-ip-addresses-in-the-azure-portal"></a>Giden IP adresleri, Azure portalında bulun

Etkili olmadan önce portalda yeni giden IP adresleri gösterilir. Eskileri, artık Azure yenilerini kullanılarak başlatıldığında kullanılacaktır. Aynı anda yalnızca bir küme kullanılır, bu nedenle ekleme listeleri girişleri anahtarı meydana geldiğinde kesinti önlemek için eski ve yeni IP adresleri olması gerekir. 

1.  [Azure portalı](https://portal.azure.com) açın.

2.  Sol taraftaki gezinti menüsünde seçin **uygulama hizmetleri**.

3.  App Service uygulamanızı listeden seçin.

4.  Uygulamayı bir işlev uygulaması, bakın [işlev uygulaması giden IP adresleri](../azure-functions/ip-addresses.md#function-app-outbound-ip-addresses).

4.  Altında **ayarları** başlık tıklayın **özellikleri** sol gezinti ve Bul etiketlenmiş bölümü **giden IP adresleri**.

5. IP adreslerini kopyalayıp giden trafik bir filtre gibi özel işlem eklemek veya izin verilen listesi. Listenin mevcut IP adreslerini silmeyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure tarafından başlatılan bir IP adresi değişikliği hazırlama açıklanmıştır. Azure App Service'te IP adresleri hakkında daha fazla bilgi için bkz. [giden ve giden IP adresleri Azure App Service'te](app-service-ip-addresses.md).
