---
title: Oluşturma ve bir teklifi Market'te dağıtma hakkında genel bakış | Microsoft Docs
description: Onaylanan bir Microsoft geliştiricisi olun, oluşturmak ve bir sanal makine görüntüsünü, şablon, veri hizmeti veya Geliştirici hizmeti Azure Marketi'nde dağıtmak için gereken adımları anlayın
services: marketplace-publishing
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: 5343bd26-c6e4-4589-85b7-4a2c00bba8ab
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio
ms.openlocfilehash: 2c8c97d8f5477e7640df87030ed6ef27c4c7b979
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310088"
---
# <a name="publish-and-manage-an-offer-in-the-azure-marketplace"></a>Yayımlama ve Azure Marketi'nde Teklif yönetme

> [!NOTE]
> Bu belgeler artık geçerli değil ve doğru değil. Lütfen Azure Marketi'nde gitmeyi [satıcı Kılavuzu](https://docs.microsoft.com/azure/marketplace/seller-guide/cloud-partner-portal-seller-guide) bir teklifi Azure Marketi'nde yayımlama konusunda yönergeler için.

Bu makalede, geliştiricilerin oluşturmanıza, dağıtmanıza ve diğer Azure müşterileri ve iş ortakları satın alıp kullanmak için Azure Market'te listelenen çözümleri yönetmenize yardımcı olmak için sağlanır.

## <a name="marketplace-publishing"></a>Market'te yayımlama
Bir Azure yayımcı dağıtın ve yenilikçi bir çözüm ya da hizmet diğer geliştiriciler, ISV'ler, satış ve Market'te BT uzmanları. Market aracılığıyla, hızla bulut tabanlı uygulamalarını ve mobil çözümler geliştirmek isteyen müşterilere ulaşabilir. İş kullanıcıları, çözümünüzün olmasını istiyorsanız, düşünmek isteyebilirsiniz [AppSource](https://appsource.microsoft.com) Market.


## <a name="supported-types-of-solutions"></a>Desteklenen tür çözümler
Ne tür bir çözüm tanımlamak için bir yayımcı olarak yapmanız gereken ilk şey, şirketinizin teklifidir. Market, tekliflerin aşağıdaki türlerini destekler:

|Çözüm türü|Sanal makine|Çözüm şablonu|
|---|---|---|
|**Tanım**|Tamamen yüklenmiş işletim sistemi ve bir veya daha fazla uygulamaların bulunduğu önceden yapılandırılmış görüntüler. Bir sanal makine görüntüsü oluşturma ve Azure sanal makineler hizmetinde sanal makineler dağıtmak gereken bilgileri sağlar.|Bir veya daha fazla farklı Azure hizmetlerini başvuran bir veri yapısı Hizmetleri dahil olmak üzere diğer satıcılar tarafından yayımlanmış. Azure aboneleri, tek ve eşgüdümlü bir şekilde bir veya daha fazla tekliflerini dağıtmak için kullanabilirsiniz.|
|**Örnek**|Bir Azure yayımcı oluşturduğunuz ve yenilikçi bir veritabanı hizmeti ile bir VM doğrulandı. Diğer Azure aboneleri, tedarik ve bu VM bulut hizmeti ortamlarını dağıtma istiyorsunuz.|Bir Azure yayımcı Yük Dengeleme, geliştirilmiş güvenlik ve yüksek kullanılabilirlik ile birlikte bulut hizmetlerini dağıtmak hızlı olun Azure hizmetlerinden birtakım paketlenmiş. Diğer Azure aboneleri, bunların amaca uygun çözüm şablonu temin etme ile zamandan tasarruf edebilirsiniz. El ile bulun, tedarik edin, dağıtın ve aynı veya benzer Azure hizmetlerini yapılandırmak yok.|

> [!NOTE]
> Bazı adımlar farklı türde çözümleri arasında paylaşılır ve diğer ilgili çözüm türü için farklı. Bu makalede, herhangi bir çözüm türü için tamamlanması gereken adımları kısa bir genel bakış sağlar.

## <a name="publish-a-solution"></a>Bir çözümü yayımlama
![Aday gösterin, kaydetme, yayımlama](media/marketplace-publishing-getting-started/img01.png)

### <a name="nominate-your-solution-for-pre-approval"></a>Çözümünüzü Ön onay için aday gösterin
Bir sanal makine yayımlamak için [çözüm](https://createopportunity.azurewebsites.net) ve Market, Microsoft Azure sertifikalı tamamlamak **çözüm ADAYLIK formu**.

>[!NOTE]
> Bir iş ortağı Hesap Yöneticisi veya DX ortak yönetici ile çalışıyorsanız, çözümünüzü Azure sertifikası programı için aday gösterin isteyin. Ayrıca gidebilirsiniz [Microsoft Azure sertifikası](http://createopportunity.azurewebsites.net) Web ve uygulama formu doldurun. İş ortağı Hesap Yöneticisi veya DX ortak Yöneticisi'nde e-postası girin **Microsoft Sponsor kişi** kutusu.

Uygunluk ölçütlerini karşılıyorsanız [Azure Marketi katılım ilkeleri](https://go.microsoft.com/fwlink/?LinkID=526833) ve uygulamanızın onaylanan, biz size ekleme çözümünüzü Marketi çalışmaya başlayın.

### <a name="register-your-account-as-a-microsoft-seller"></a>Hesabınızı bir Microsoft satıcı kaydedin.
Microsoft hesabınız olarak kaydetmek bir [Microsoft Developer hesabı](marketplace-publishing-accounts-creation-registration.md).

### <a name="publish-your-solution"></a>Çözümünüzü yayımlayın
Market'te çözüm yayımlama için şu adımları izleyin:
1. Yedeğin gerekliliklerini.

    a. Karşılama [yedeğin önkoşulları](marketplace-publishing-pre-requisites.md).

    b. Karşılama [VM teknik Önkoşullar](marketplace-publishing-vm-image-creation-prerequisites.md).

    c. Karşılama [çözüm şablonu teknik Önkoşullar](marketplace-publishing-solution-template-creation-prerequisites.md).

2. Teklifinizi oluşturun.

    a. Oluşturma bir [sanal makine](marketplace-publishing-vm-image-creation.md) sunar.

    b. Oluşturma bir [çözüm şablonu](marketplace-publishing-solution-template-creation.md) sunar.

3. Teklifinizi oluşturun [içeriği pazarlama](marketplace-publishing-push-to-staging.md).

4. Teklifiniz hazırlama, test edin.

    a. VM teklifinizi sınamak [hazırlama](marketplace-publishing-vm-image-test-in-staging.md).

    b. Çözüm şablonu teklifinizi sınamak [hazırlama](marketplace-publishing-solution-template-test-in-staging.md).

5. Teklifinizin dağıtma [Market](marketplace-publishing-push-to-production.md).


### <a name="create-and-manage-a-virtual-machine-image"></a>Oluşturma ve bir sanal makine görüntüsü yönetme
Oluşturup bu kaynakları kullanarak bir VM görüntüsü yönetebilirsiniz:
* Bir VM görüntüsü oluşturma [şirket içi](marketplace-publishing-vm-image-creation-on-premise.md).
* Çalıştıran bir sanal makine oluşturma [Windows Azure portalında](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Çalıştıran bir sanal makine oluşturma [Azure portalında Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Sırasında karşılaşılan yaygın sorunları giderme [VHD oluşturma](marketplace-publishing-vm-image-creation-troubleshooting.md).

## <a name="manage-your-solution"></a>Çözümünüzü yönetme
Aşağıdaki kaynaklardan Yardım ile çözümünüzü yönetin:
* [Sanal makine teklifler için üretim sonrası kılavuzunu okuyun](marketplace-publishing-vm-image-post-publishing.md)
* [Bir teklif ve SKU yedeğin ayrıntılarını güncelleştirme](marketplace-publishing-vm-image-post-publishing.md#update-the-nontechnical-details-of-an-offer-or-a-sku)
* [Bir teklif SKU ve teknik ayrıntılarını güncelleştirme](marketplace-publishing-vm-image-post-publishing.md#update-the-technical-details-of-a-sku)
* [Listelenen bir teklif altında yeni bir SKU ekleyin](marketplace-publishing-vm-image-post-publishing.md#add-a-new-sku-under-a-listed-offer)
* [Veri diski sayısı listelenen SKU için değiştirin](marketplace-publishing-vm-image-post-publishing.md#change-the-data-disk-count-for-a-listed-sku)
* [Marketten listelenen teklifi Sil](marketplace-publishing-vm-image-post-publishing.md)
* [Marketten bir listelenen SKU Sil](marketplace-publishing-vm-image-post-publishing.md#delete-a-listed-sku-from-the-marketplace)
* [Marketten bir listelenen SKU'ın geçerli sürümü silin](marketplace-publishing-vm-image-post-publishing.md#delete-the-current-version-of-a-listed-sku-from-the-marketplace)
* [Üretim değerleri liste fiyatı geri döndür](marketplace-publishing-vm-image-post-publishing.md#revert-the-listing-price-to-production-values)
* [Faturalandırma modeli üretim değerlere geri döndür](marketplace-publishing-vm-image-post-publishing.md#revert-the-billing-model-to-production-values)
* [Listelenen bir SKU üretim değerine görünürlük ayarlarını geri döndür](marketplace-publishing-vm-image-post-publishing.md#revert-the-visibility-setting-of-a-listed-sku-to-the-production-value)

## <a name="additional-resources"></a>Ek kaynaklar
[Azure PowerShell ayarlama ayarlayın](marketplace-publishing-powershell-setup.md)
