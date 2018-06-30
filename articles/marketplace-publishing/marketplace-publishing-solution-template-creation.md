---
title: Market bir çözüm şablonu oluşturmak için kılavuz | Microsoft Docs
description: Oluşturma, onaylamak ve dağıtmak için bir çoklu VM görüntü çözüm şablonu konusunda ayrıntılı yönergeler Azure Market satın alın.
services: marketplace-publishing
documentationcenter: ''
author: msmbaldwin
manager: mbaldwin
editor: ''
ms.assetid: e14e05f2-2385-4ce0-b351-0747cb74ba19
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: mbaldwin
ms.openlocfilehash: 147b0b9b4a3fe789544457d17fed3d29badbe12c
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113961"
---
# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Azure Market'ten bir çözüm şablonu oluşturmak için kılavuz
Adım 1 ' tamamladıktan sonra [hesap oluşturma ve kayıt][link-acct-creation], bir Azure uyumlu çözüm şablon oluşturma işleminde size kılavuzluk [oluşturmak için teknik Önkoşullar bir Çözüm şablonu](marketplace-publishing-solution-template-creation-prerequisites.md). Biz, üzerinde birden çok VM için bir çözüm şablonu oluşturmak için adımlarda size yol gösterecek artık [yayımlama portalında] [ link-pubportal] Azure Marketi için.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Çözüm şablonu teklifiniz yayımlama portalında oluşturma
Git [ https://publish.windowsazure.com ](http://publish.windowsazure.com). İlk kez oturum açarken [yayımlama portalında](https://publish.windowsazure.com/), aynı hesabı, şirketinizin satıcı profili kaydedildiği ile kullanın. Daha sonra şirketinizin herhangi bir personel yayımlama portalında ortak yönetici olarak ekleyebilirsiniz.

### <a name="1-select-solution-templates"></a>1. "Çözüm şablonları" seçin
  ![Çizim][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Yeni bir çözüm şablonu oluşturun
  ![Çizim][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. Topolojileri ile Başlat
Bir çözüm şablonu tüm topolojilerinin "üst öğesidir". Bir teklif/çözüm şablonunda birden fazla topoloji tanımlayabilirsiniz. Bir teklif hazırlama için gönderildiğinde tüm topolojileri ile birlikte iletilir. Teklifiniz tanımlamak için aşağıdaki adımları izleyin:     

* Bir topoloji oluşturun: "topoloji" tanımlayıcısıdır genellikle çözüm şablonu için topoloji adı. Topoloji tanımlayıcı URL'de aşağıda gösterildiği gibi kullanılır:

  Azure Market: http://azure.microsoft.com/marketplace/partners/{PublisherNamespace}/{OfferIdentifier}{TopologyIdentifier}

  Azure portalı: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{TopologyIdentifier}
* Yeni bir sürüm ekleyin.

### <a name="4-get-your-topology-versions-certified"></a>4. Sertifikalı topoloji sürümlerinizi Al
Bu belirli sürümü topolojisinin sağlamak için tüm gerekli dosyaları içeren bir zip dosyasını karşıya yükleyin. Bu zip dosyası şunları içermelidir:

* *mainTemplate.json* ve *createUiDefinition.json* dosyasını kendi kök dizininde.
* Tüm bağlı şablonları ve gerekli tüm betikler.

  > [!TIP]
  > Geliştiricilere çözüm şablonu topolojileri oluşturmak ve bunları sertifikalı, iş alma üzerinde çalışırken pazarlama ve/veya yasal Departmanlar şirketinizin pazarlama ve yasal içerik üzerinde çalışabilir.
  >
  >

## <a name="next-steps"></a>Sonraki adımlar
Çözüm şablonu oluşturduğunuz ve zip dosyası karşıya göre Lütfen'ndaki yönergeleri izleyin [Market pazarlama içerik Kılavuzu](marketplace-publishing-push-to-staging.md) teklif hazırlama için itme önce. Market makaleleri yayımlama, tamamını görmek için ziyaret [Başlarken: bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md).

Bu ilgili makalelerde ilginizi çekebilir:

* VM görüntüleri: [azure'da sanal makine görüntülerini hakkında](https://msdn.microsoft.com/library/azure/dn790290.aspx)
* VM uzantıları: [VM aracısı ve VM uzantılarına genel bakış](https://msdn.microsoft.com/library/azure/dn832621.aspx) ve [Azure VM uzantıları ve özellikleri](https://docs.microsoft.com/azure/virtual-machines/extensions/features-windows)
* Azure Kaynak Yöneticisi: [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md) ve [basit bir şablon örnekleri](https://github.com/rjmax/ArmExamples)
* Depolama hesabı kısıtlar: [izleme depolama hesabı daraltması](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) ve [Premium depolama](../virtual-machines/windows/premium-storage.md#scalability-and-performance-targets)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
