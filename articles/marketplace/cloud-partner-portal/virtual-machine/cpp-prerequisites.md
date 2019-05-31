---
title: Microsoft Azure sanal makine önkoşulları | Azure Market
description: VM teklifi Azure Marketinde yayımlama için gereken önkoşulları listesi.
services: Azure, Marketplace, Cloud Partner Portal
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 03/13/2019
ms.author: pabutler
ms.openlocfilehash: 258d21eae5af50b5dc0bed6887618e2999cae45a
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257395"
---
# <a name="virtual-machine-prerequisites"></a>Sanal makine önkoşulları

Bu makalede her iki teknik listeler ve önce karşılamanız gereken iş gereksinimleri için bir VM teklifi yayımlayabilirsiniz [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/).  Zaten yapmadıysanız, gözden [sanal makine teklifi yayımlama Kılavuzu](../../marketplace-virtual-machines.md).


## <a name="technical-requirements"></a>Teknik gereksinimler

Bir sanal makine (VM) çözümü yayımlama teknik Önkoşullar basittir:

- Etkin bir Azure hesabınızın olması gerekir. Biri yoksa, kaydolabilirsiniz [Microsoft Azure sitesine](https://azure.microsoft.com).  
- Windows veya Linux VM geliştirme destekleyecek şekilde yapılandırılmış bir ortam olmalıdır.  Daha fazla bilgi için ilişkili VM belge sitesine bakın:
    - [Linux Vm'leri ile ilgili belgeler](https://docs.microsoft.com/azure/virtual-machines/linux/)
    - [Windows Vm'leri ile ilgili belgeler](https://docs.microsoft.com/azure/virtual-machines/windows/)


## <a name="business-requirements"></a>İş gereksinimleri

Yordam, sözleşmeye dayalı ve yasal yükümlülüklerin yerine iş gereksinimleri şunlardır: 

<!-- TD: Aren't most of these business requirements common to all AMP offerings?  If yes, then move to higher level, perhaps to the AMP section "Become a Cloud Marketplace Publisher" -->
<!-- TD: Need references for remaining docs/business reqs!-->

- Kayıtlı bir bulut Market yayımcı olmalıdır.  Henüz kaydolmadıysanız makaledeki adımları [bulut Market yayımcısı haline](https://docs.microsoft.com/azure/marketplace/become-publisher).

    > [!NOTE]
    > Oturum açmanız aynı Microsoft Developer Center kayıt hesabı kullanmalısınız [bulut iş ortağı portalı](https://cloudpartner.azure.com).
    > Azure Marketi Teklifleriniz için yalnızca bir Microsoft hesabı olması gerekir. Bireysel hizmetlerin veya teklifler için belirli olmamalıdır.
    
- Şirketinizin (veya yan kuruluşunun), bir satış gelen-ülke/Azure Marketi tarafından desteklenen bölgeler içinde bulunmalıdır.  Bu ülkeler/bölgeler güncel bir listesi için bkz. [Microsoft Azure Marketi katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).
- Ürününüzün Azure Marketi tarafından desteklenen faturalandırma modelleri ile uyumlu bir şekilde lisanslanmalıdır.  Daha fazla bilgi için [faturalandırma seçenekleri Azure Marketi'nde](https://docs.microsoft.com/azure/marketplace/billing-options-azure-marketplace). 
- Teknik Destek kullanılabilir müşterilere ticari açıdan makul bir şekilde yapmaktan sorumlu olursunuz. Bu destek, ücretsiz, ücretli veya topluluk yaklaşım olabilir.
- Yazılımınızı ve üçüncü taraf yazılım bağımlılıkları lisansı sağlamaktan sorumlu olursunuz.
- Teklifinizin Azure Market'te ve Azure Portalı'nda listelenmesi ölçütlerini karşılayan içeriği sağlamanız gerekir. <!-- TD: Meaning/links? -->
- Koşullarını kabul etmelisiniz [Microsoft Azure Marketi katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) ve yayımcı anlaşması.
- Uygun davranmalısınız [Microsoft Azure Web sitesi kullanım koşulları](https://azure.microsoft.com/support/legal/website-terms-of-use/), [Microsoft gizlilik bildirimi](https://privacy.microsoft.com/privacystatement), ve [Microsoft Azure sertifikası Program sözleşmesi](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).


## <a name="next-steps"></a>Sonraki adımlar

Bu önkoşulları karşıladığınızdan sonra [, VM teklifi oluşturma](./cpp-create-offer.md).
