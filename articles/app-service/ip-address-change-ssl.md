---
title: IP SSL adres değişikliği - Azure App Service için hazırlama
description: SSL IP adresiniz, değiştirilecek olacaksa, böylece uygulamanız için değişiklik sonrasında çalışmaya devam yapmanız gerekenler öğrenin.
services: app-service\web
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 06/28/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 6c8c86ff6212acc31e961d6ae62836ca2b7b7380
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61268922"
---
# <a name="how-to-prepare-for-an-ssl-ip-address-change"></a>Nasıl bir IP SSL adresi değişikliğe hazırlanmak için

Azure App Service uygulamanızın IP SSL adresi ile değişiyor bir bildirim aldıysanız, var olan bir IP SSL adresi serbest ve yeni bir tane atamak için bu makaledeki yönergeleri izleyin.

## <a name="release-ssl-ip-addresses-and-assign-new-ones"></a>SSL IP adreslerini serbest bırakma ve yeni etiketler

1.  [Azure portalı](https://portal.azure.com) açın.

2.  Sol taraftaki gezinti menüsünde seçin **uygulama hizmetleri**.

3.  App Service uygulamanızı listeden seçin.

4.  Altında **ayarları** başlığını tıklatın **SSL ayarları** sol gezinti bölmesinde.

1. SSL bağlamaları bölümünde, ana bilgisayar adı kaydı seçin. Açılır düzenleyicide seçin **SNI SSL** üzerinde **SSL türü** açılır menüsüne ve ardından **bağlaması Ekle**. İşlem başarılı iletisini gördüğünüzde, mevcut IP adresi piyasaya Sürüldü.

6.  İçinde **SSL bağlamaları** bölümünde, sertifika ile aynı ana bilgisayar adı kaydı yeniden seçin. Bu kez açan düzenleyicide seçin **IP tabanlı SSL** üzerinde **SSL türü** açılır menüsüne ve ardından **bağlaması Ekle**. İşlem başarılı iletisini gördüğünüzde, yeni bir IP adresi edindiğiniz.

7.  Bir A kaydı (doğrudan IP adresine işaret eden DNS kaydı), etki alanı kayıt Portalı'nda (üçüncü taraf DNS sağlayıcısı veya Azure DNS) yapılandırılmışsa, mevcut IP adresini yeni oluşturulan bir adla değiştirin. Sonraki bölümde yönergeleri takip ederek yeni IP adresini bulabilirsiniz.

## <a name="find-the-new-ssl-ip-address-in-the-azure-portal"></a>Azure Portalı'nda yeni SSL IP adresini bulun

1.  Birkaç dakika bekleyin ve ardından açın [Azure portalında](https://portal.azure.com).

2.  Sol taraftaki gezinti menüsünde seçin **uygulama hizmetleri**.

3.  App Service uygulamanızı listeden seçin.

4.  Altında **ayarları** başlık tıklayın **özellikleri** sol gezinti ve Bul etiketlenmiş bölümü **sanal IP adresi**.

5. IP adresini kopyalayın ve etki alanı kaydı veya IP mekanizması yeniden yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure tarafından başlatılan bir IP adresi değişikliği hazırlama açıklanmıştır. Azure App Service'te IP adresleri hakkında daha fazla bilgi için bkz. [SSL ve SSL IP adresleri Azure App Service'te](overview-inbound-outbound-ips.md).
