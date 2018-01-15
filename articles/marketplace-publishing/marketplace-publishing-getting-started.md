---
title: "Oluşturma ve bir teklif Market'te dağıtma hakkında genel bakış | Microsoft Docs"
description: "Onaylanan bir Microsoft Geliştirici hale oluşturmak ve sanal makine görüntüsünün, şablon, veri hizmeti veya Azure Marketi Geliştirici hizmetinde dağıtmak için gereken adımları anlayın"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5343bd26-c6e4-4589-85b7-4a2c00bba8ab
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio
ms.openlocfilehash: 82580fbab68eab28a2027cd277213f1fb2a76e07
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
> [!NOTE]
> Bu belge, artık geçerli değil ve doğru değil. Lütfen Azure Marketi'nde gitmeyi [Seller Kılavuzu](https://docs.microsoft.com/azure/marketplace/seller-guide/cloud-partner-portal-seller-guide) bir teklifi Azure Marketinde yayımlama konusunda yönergeler için.

# <a name="publish-and-manage-an-offer-in-the-azure-marketplace"></a>Yayımlama ve Azure markette bir teklif yönetme
Bu makalede, geliştiricilerin oluşturmak, dağıtmak ve diğer Azure müşterileri ve ortakları satın almak ve kullanmak Azure Market listelenen çözümleri yönetmek amacıyla sağlanır.

## <a name="marketplace-publishing"></a>Market yayımlama
Azure bir yayımcı olarak dağıtın ve yenilikçi çözüm ya da hizmet diğer geliştiriciler, ISV, satış ve BT uzmanları Market'te. Market üzerinden hızlı bir şekilde bulut tabanlı uygulamalarını ve mobil çözümleri geliştirmek için isteyen müşteriler ulaşabilirsiniz. Çözümünüzü iş kullanıcıları hedefliyorsa, düşünmek isteyebilirsiniz [AppSource](http://appsource.microsoft.com) Market.


## <a name="supported-types-of-solutions"></a>Desteklenen tür çözümler
Ne tür bir çözüm tanımlamak için bir yayımcı olarak yapmanız gereken ilk şey, şirketinizin öneriyor. Market teklifleri aşağıdaki türlerini destekler:

|Çözüm türü|Sanal makine|Çözüm şablonu|
|---|---|---|
|**Tanımı**|Tam olarak yüklenmiş bir işletim sistemi ve bir veya daha fazla uygulama önceden yapılandırılmış görüntülerle. Bir sanal makine görüntüsü oluşturma ve Azure sanal makineler hizmet sanal makineleri dağıtmak için gereken bilgileri sağlar.|Bir veya daha fazla farklı Azure Hizmetleri başvurabilir veri yapısı, Hizmetleri dahil olmak üzere diğer satıcılar tarafından yayımladı. Azure aboneleri, bir veya daha fazla teklifleri tek ve eşgüdümlü bir şekilde dağıtmak için kullanabilirsiniz.|
|**Örnek**|Azure bir yayımcı olarak oluşturulabilir ve doğrulanabilir bir yenilikçi veritabanı hizmeti ile bir VM. Diğer Azure aboneleri tedarik etmek ve bu VM bulut hizmeti ortamlarını dağıtmak istediğiniz.|Azure bir yayımcı olarak arasında Yük Dengeleme, Gelişmiş Güvenlik ve yüksek kullanılabilirlik ile birlikte bulut hizmetlerini dağıtmak hızlı olun Azure hizmetlerinden kümesi paketlenmiş. Diğer Azure aboneleri kendi hedefi karşılayan çözüm şablonu temin etme zamandan tasarruf edebilirsiniz. Bunlar el ile bulun, tedarik etmek, dağıtmak ve aynı veya benzer Azure hizmetleri yapılandırmak zorunda değilsiniz.|

> [!NOTE]
> Çözümleri farklı türleri arasında paylaşılan bazı adımlar ve başkalarının çözümü ilgili türüne farklıdır. Bu makale, çözümü her tür için tamamlanması gereken adımları kısa bir genel bakış sağlar.

## <a name="publish-a-solution"></a>Bir çözüm yayımlama
![Belirler, kaydetme, yayımlama](media/marketplace-publishing-getting-started/img01.png)

### <a name="nominate-your-solution-for-pre-approval"></a>Çözümünüz için Ön onay belirler
Bir sanal makine yayımlamak için [çözüm](https://createopportunity.azurewebsites.net) Microsoft Sertifikalı Azure Marketi'nde tamamlamak **çözüm Adaylığı Form**.

>[!NOTE]
> Bir iş ortağı Hesap Yöneticisi'ni veya DX ortak Yöneticisi ile çalışıyorsanız, çözümünüz için Azure Certified program belirler isteyin. Ayrıca gidebilirsiniz [Azure Microsoft Sertifikalı](http://createopportunity.azurewebsites.net) Web sayfası ve uygulama formu doldurun. İş ortağı Hesap Yöneticisi veya DX ortak Yöneticisi'nde e-posta girin **Microsoft sponsoru kişi** kutusu.

Uygunluk ölçütler karşılıyorsa [Azure Marketi katılım ilkeleri](http://go.microsoft.com/fwlink/?LinkID=526833) ve uygulamanızı onaylanır, giriş Market çözümünüze çalışmayı başlatın.

### <a name="register-your-account-as-a-microsoft-seller"></a>Hesabınızı Microsoft satıcı olarak Kaydet
Microsoft hesabınız olarak kaydetmek bir [Microsoft Developer hesabı](marketplace-publishing-accounts-creation-registration.md).

### <a name="publish-your-solution"></a>Çözümünüzü yayımlama
Bir çözüm Marketinde yayımlama için şu adımları izleyin:
1. Yedeğin gereksinimleri karşılamanız.

    a. Karşılama [yedeğin Önkoşullar](marketplace-publishing-pre-requisites.md).

    b. Karşılama [VM teknik Önkoşullar](marketplace-publishing-vm-image-creation-prerequisites.md).

    c. Karşılama [çözüm şablonu teknik önkoşulları](marketplace-publishing-solution-template-creation-prerequisites.md).

2. Teklifiniz oluşturun.

    a. Oluşturma bir [sanal makine](marketplace-publishing-vm-image-creation.md) sunar.

    b. Oluşturma bir [çözüm şablonu](marketplace-publishing-solution-template-creation.md) sunar.

3. Teklifiniz oluşturma [içerik pazarlama](marketplace-publishing-push-to-staging.md).

4. Teklifiniz hazırlamada sınayın.

    a. VM teklifiniz test [hazırlama](marketplace-publishing-vm-image-test-in-staging.md).

    b. Çözüm şablonu teklifiniz test [hazırlama](marketplace-publishing-solution-template-test-in-staging.md).

5. Teklifiniz için dağıtmak [Market](marketplace-publishing-push-to-production.md).


### <a name="create-and-manage-a-virtual-machine-image"></a>Oluşturma ve bir sanal makine görüntüsü yönetme
Oluşturun ve bu kaynakları kullanarak bir VM görüntüsü yönetin:
* Bir VM görüntüsü oluşturma [şirket içi](marketplace-publishing-vm-image-creation-on-premise.md).
* Çalıştıran bir sanal makine oluşturma [Windows Azure portalında](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Çalıştıran bir sanal makine oluşturma [Linux Azure portalında](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Sırasında karşılaşılan yaygın sorunları giderme [VHD oluşturma](marketplace-publishing-vm-image-creation-troubleshooting.md).

## <a name="manage-your-solution"></a>Çözümünüzü yönetme
Yardım çözümünüzle aşağıdaki kaynaklardan yönetin:
* [Sanal makine teklifleri sonrası üretim Kılavuzu'nu okuyun](marketplace-publishing-vm-image-post-publishing.md)
* [Bir teklif veya bir SKU yedeğin ayrıntılarını güncelleştir](marketplace-publishing-vm-image-post-publishing.md#update-the-nontechnical-details-of-an-offer-or-a-sku)
* [Güncelleştirme bir teklif ya da bir SKU teknik ayrıntıları](marketplace-publishing-vm-image-post-publishing.md#update-the-technical-details-of-a-sku)
* [Listelenen bir teklif altında yeni bir SKU ekleme](marketplace-publishing-vm-image-post-publishing.md#add-a-new-sku-under-a-listed-offer)
* [Listelenen SKU veri diski sayısı değiştirme](marketplace-publishing-vm-image-post-publishing.md#change-the-data-disk-count-for-a-listed-sku)
* [Marketten listelenen teklifi Sil](marketplace-publishing-vm-image-post-publishing.md)
* [Marketten listelenen SKU Sil](marketplace-publishing-vm-image-post-publishing.md#delete-a-listed-sku-from-the-marketplace)
* [Marketten listelenen SKU geçerli sürümü Sil](marketplace-publishing-vm-image-post-publishing.md#delete-the-current-version-of-a-listed-sku-from-the-marketplace)
* [Üretim değerlere listeleme fiyat geri](marketplace-publishing-vm-image-post-publishing.md#revert-the-listing-price-to-production-values)
* [Üretim değerleri faturalama modeline geri](marketplace-publishing-vm-image-post-publishing.md#revert-the-billing-model-to-production-values)
* [Listelenen SKU üretim değerine görünürlük ayarını geri](marketplace-publishing-vm-image-post-publishing.md#revert-the-visibility-setting-of-a-listed-sku-to-the-production-value)

## <a name="additional-resources"></a>Ek kaynaklar
[Azure PowerShell ayarlayın](marketplace-publishing-powershell-setup.md)
