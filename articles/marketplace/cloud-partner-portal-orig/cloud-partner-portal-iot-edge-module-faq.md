---
title: IOT Edge modülü Market SSS | Microsoft Docs
description: IOT Edge modülü Market SSS.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 2c2e7729eb70a5dd37dc4df10605eec9429e1043
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811392"
---
# <a name="iot-edge-module-marketplace-frequently-asked-questions"></a>IOT Edge modülü Market hakkında sık sorulan sorular


## <a name="what-is-the-edge-module-marketplace"></a>Edge modül Market nedir?


IOT Edge modülü markette listesidir *sertifikalı* olan Edge modüllerini önceden oluşturulmuş *bulunabilir* Azure Marketi aracılığıyla.

![IoT Edge Modülleri](./media/cloud-partner-portal-iot-edge-module-faq/iot-edge-modules.png)

## <a name="where-will-edge-modules-be-visible"></a>Burada Edge modüllerini görünür olacak? 


İçinde [Azure portal Market](https://ms.portal.azure.com/) (kimlik doğrulaması deneyimi), nesnelerin interneti kategorisi altında "IOT Edge modülü" etiketlenmiş.

Ve [Azure Marketi Web sitesinde](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/internet-of-things?page=1) (anonim deneyimi) nesnelerin interneti kategorisi altında "IOT Edge modülü" etiketlenir.

## <a name="is-it-open-to-partners"></a>İş ortakları için açık mı?


Henüz değil. Şu anda yalnızca Microsoft tarafından yazılmış modüller IOT Edge Marketi bölümünde yayımlanır. 

## <a name="why-should-partners-publish-their-iot-edge-modules"></a>Neden iş ortakları kendi IOT Edge modüllerinin yayımlamalısınız?


-   Bu kanal pazara ekleme ve çözümleri vitrini, ürün görünürlüğüne artırmak için.

-   Daha fazla müşteri adayları alma Azure Marketi'nde bir parçası olarak, kendi iş çözümdeki ilgilenen kim hakkında ayrıntılı bilgi elde edebilirsiniz.

-   Daha fazla kazanç elde etme özelliklerinden yararlanmak için ilk deneyenlerden olacak.

## <a name="what-is-the-onboarding-process"></a>Onboarding işlemi nedir?


Henüz açık durumdayken, ekleme işlemi Azure Marketi aracılığıyla yapılır. Ayrıntılı yönergeler bulunduğunuz [Azure Market'te ve Appsource'ta yayımlama Kılavuzu](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide). Kapsayıcılar için liste türünü transact bakın. 

İş ortakları, ilk Azure Market yayımcı olmak gerekir. Bunlar yerleşik önlemler sonra kendi Edge modüllerinin aracılığıyla [bulut iş ortağı portalı](./cloud-partner-portal-getting-started-with-the-cloud-partner-portal.md).

## <a name="are-there-any-monetization-capabilities"></a>Herhangi bir kazanç özellik var mı?


Değil yepyeni şimdilik. Market ile açmak için ilk Hedefimiz olan *ücretsiz* veya *kendi lisansını Getir* kenar modüllerini. Kullanım faturalandırma modeli gibi daha fazla kazanç elde etme özellikleri ekleyeceğiz.

## <a name="what-is-bring-your-own-license-byol"></a>-Kendi-lisansını getir (KLG) nedir?


Resmi tanımında [Microsoft Azure Marketi katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) olan:

- *Müşteriler Azure Marketi dışında teklife erişme veya teklifi kullanma hakkını elde eder ve Azure Marketi'nde teklifin Azure Marketi'nde teklifin kullanımı için ücret alınmaz.*

İş ortaklarının yazılım lisans ve gelirlerini topladığınıza aittir.

## <a name="can-partners-publish-a-freemium-module"></a>İş ortakları "freemium" modülü yayımlayabilirsiniz?


Evet, freemium modüller, kullanılabilir modül ücretsiz olan ancak bazı sınırlamalarla birlikte herhangi bir yayın olarak kabul edilir.

## <a name="is-intellectual-property-protected"></a>Fikri mülkiyet korunuyor mu?


Bir Edge modülü Docker uyumlu kapsayıcıdır. Dağıtılmış kapsayıcıları paketlenmiş, fikri mülkiyet (IP) korumak için iş ortakları aittir.

## <a name="where-will-the-modules-be-hosted"></a>Burada modülleri barındırılacak mı?


IOT Edge modülleri, anonim olarak-Docker Hub gibi sorgulayabilir, Microsoft'a ait bir kapsayıcı kayıt defteri barındırılacak.

## <a name="what-are-the-integration-plans-between-the-azure-marketplace-and-the-azure-iot-tools"></a>Azure Marketi ile Azure IOT araçları arasında tümleştirme planları nelerdir?

Azure Marketi ile Azure IOT araçları arasında sıkı tümleştirme oluşturacağız. Biz göz önünde sahip bazı IOT Edge modülü IOT hub'ı Portalı'ndan doğrudan veya doğrudan Visual Studio Code Marketi gözatma örnekleridir.

## <a name="which-operating-system-or-architecture-should-my-container-support"></a>Hangi işletim sistemi veya mimari my kapsayıcı desteklemesi gerekir?

IOT Edge modülleri, aynı işletim sistemi desteklemelidir / üretimde olarak IOT Edge mimarisini matrisi. Bu liste tutulur [Azure IOT Edge desteği](https://docs.microsoft.com/azure/iot-edge/support). Örneğin, bir modül, şu anda Linux x64 ve Linux ARM32 desteklemelidir.

## <a name="are-there-any-other-certification-constraints-to-be-aware-of"></a>Dikkat edilmesi gereken diğer sertifika kısıtlamalardan var mı?

Yine de tam olarak sertifika kısıtlamaları kümesini üzerinde çalışıyoruz. Bizim yol gösterici ilkeler şunlardır:

-   Kalite miktarın üzerindeki yükseltiliyor.

-   IOT Edge özel kapsayıcılar (değil rastgele olanlar için).

-   Üretim sınıf.

-   1-tıklatma dağıtım (en az kümesi varsayılan yapılandırması sağlanan değerler).

## <a name="any-other-questions"></a>Diğer herhangi bir sorunuz mu var?


İlgili kişi [Azure IOT Edge kolaylaşmasına](mailto:azureiotedgeonboarding@service.microsoft.com).
