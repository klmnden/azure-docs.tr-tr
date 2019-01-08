---
title: Market çözüm şablonu oluşturmak için kılavuz | Microsoft Docs
description: Ayrıntılı yönergeler oluşturma, onaylamak ve dağıtmak için bir çoklu VM görüntüsü çözüm şablonu, Azure Marketi'nde satın.
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: e14e05f2-2385-4ce0-b351-0747cb74ba19
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: hascipio; v-divte
ROBOTS: NOINDEX
ms.openlocfilehash: 914dece4ba064af00c5b2c34d43dedbb1d62e2e2
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073750"
---
# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Azure Marketi için bir çözüm şablonu oluşturmak için kılavuz
1. adım tamamlandıktan sonra [hesap oluşturma ve kayıt][link-acct-creation], biz Azure ile uyumlu çözüm şablonunun oluşturma destekli [oluşturmaya yönelik teknik Önkoşullar bir Çözüm şablonu](marketplace-publishing-solution-template-creation-prerequisites.md). Birden çok VM için bir çözüm şablonu oluşturma adımlarında size yol gösterir artık [yayımlama portalı] [ link-pubportal] Azure Marketi için.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Yayımlama Portalı'nda çözüm şablonu teklifinizi oluşturun
Git [ https://publish.windowsazure.com ](http://publish.windowsazure.com). İçin ilk kez oturum açarken [yayımlama portalı](https://publish.windowsazure.com/), aynı hesabı, şirketinizin satıcı profili kaydedildiği ile kullanın. Daha sonra Yayımlama Portalı'nda bir coadmin olarak herhangi bir çalışanın şirketinize ekleyebilirsiniz.

### <a name="1-select-solution-templates"></a>1. "Çözüm şablonları" seçin
  ![Çizim][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Yeni bir çözüm şablonu oluşturma
  ![Çizim][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. Topolojileri ile Başlat
Bir çözüm şablonu tüm topolojilerinin "üst öğesidir". Bir teklif/çözüm şablonunda birden fazla topoloji tanımlayabilirsiniz. Bir teklif hazırlama için gönderildiğinde tüm topolojileri ile birlikte iletilir. Teklifinizi tanımlamak için aşağıdaki adımları izleyin:     

* Bir topoloji oluşturun: "Topolojisi tanımlayıcı" genellikle topolojisi için çözüm şablonu adıdır. Topoloji tanımlayıcı, aşağıda gösterildiği gibi URL kullanılır:

  Azure Market: http://azure.microsoft.com/marketplace/partners/{PublisherNamespace}/{OfferIdentifier}{TopologyIdentifier}

  Azure portalı: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{TopologyIdentifier}
* Yeni bir sürüm ekleyin.

### <a name="4-get-your-topology-versions-certified"></a>4. Sertifikalı topolojisi sürümü Al
Bu belirli sürümü topolojinin sağlamak için tüm gerekli dosyaları içeren bir zip dosyası karşıya yükleyin. Bu zip dosyası, aşağıdaki dosya içermesi gerekir:

* *mainTemplate.json* ve *createUiDefinition.json* kök dizinde dosya.
* Tüm bağlı şablonların ve gerekli tüm betikler.

  > [!TIP]
  > Geliştiricilerinizin çözüm şablonu topolojileri oluşturmaya ve bunları sertifikası, işletmenin alma üzerinde çalışırken pazarlama ve/veya yasal bölümleri, şirketinizin pazarlama ve yasal içerik üzerinde çalışabilir.
  >
  >

## <a name="next-steps"></a>Sonraki adımlar
Yönergelerini, çözüm şablonu oluşturduğunuz ve zip dosyası karşıya göre [Market pazarlama içerik Kılavuzu](marketplace-publishing-push-to-staging.md) teklif, hazırlamaya göndermeden önce. Eksiksiz bir Market makaleleri yayımlama listesi görmek için ziyaret [kullanmaya başlama: Bir teklifi Azure Marketinde yayımlama nasıl](marketplace-publishing-getting-started.md).

Ayrıca bu ilgili makaleler ilginizi çekebilir:

* VM görüntüleri: [Azure'da sanal makine görüntüleri hakkında](https://msdn.microsoft.com/library/azure/dn790290.aspx)
* VM uzantıları: [Azure VM uzantıları ve özellikleri](../virtual-machines/extensions/features-windows.md)
* Azure Resource Manager: [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md) ve [basit şablon örnekleri](https://github.com/rjmax/ArmExamples)
* Depolama hesabı kısıtlar: [Depolama hesabı azaltma için izleme](https://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) ve [Premium depolama](../virtual-machines/windows/premium-storage.md#scalability-and-performance-targets)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
