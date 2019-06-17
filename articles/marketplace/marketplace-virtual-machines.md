---
title: Sanal makine teklifi Azure Marketi için Kılavuzu yayımlama
description: Bu makalede, bir sanal makine ve marketten dağıtılacak yazılım bir ücretsiz deneme sürümü yayımlamak için gereksinimleri anlatılmaktadır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: ellacroi
ms.service: marketplace
ms.topic: article
ms.date: 07/09/2018
ms.author: ellacroi
ms.openlocfilehash: ccb6fc9c522e8d05d0184fc5e248d070efb9921d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64937734"
---
# <a name="virtual-machine-offer-publishing-guide"></a>Sanal makine teklifi yayımlama Kılavuzu

Sanal makine görüntüleri Azure Market'te çözüm yayımlama için temel yollarından biridir. Bu teklif gereksinimlerini anlamak için bu kılavuzu kullanın. 

Hangi dağıtılır ve faturalandırılır Market aracılığıyla işlem teklifler şunlardır. Bir kullanıcının gördüğü eylem çağrısı "Şimdi edinin." olan

## <a name="free-trial"></a>Ücretsiz Deneme 

Kullanıcıların kendi lisansını getir (KLG) faturalandırma modeli kullanılırken sınırlı terimi yazılım lisanslarını erişerek teklifinizi sınamak düzenleyebilirsiniz. Bu teklif dağıtmak için gereksinimleri aşağıdadır. 

|Gereksinimler  |Ayrıntılar  |
|---------|---------|
|Ücretsiz deneme süresi ve deneme sürümü deneyimi     |   Müşterilerinizin uygulamanızı sınırlı bir süre için ücretsiz deneyebilirsiniz. Müşterilere, teklifiniz için herhangi bir lisans ya da Abonelik ücretleri ödemeniz gerekecektir Not aynıdır. Müşterileriniz temel alınan Microsoft birinci taraf ürün veya hizmet için ödeme yapmak gerekmez. Tüm deneme sürümü seçeneği, Azure aboneliğinize dağıtılır. Yönetim ve maliyet iyileştirmesi tek kontrol sizde. Ücretsiz deneme sürümü ya da etkileşimli tanıtım tercih edebilirsiniz. Neyin seçin ne olursa olsun, ücretsiz deneme sürümünüzü müşteriler ek ücret ödemeden, bir teklif denemek için önceden ayarlanmış bir süre sağlamanız gerekir.|
|Kolayca yapılandırılabilir, kullanıma hazır bir çözüm    |  Uygulamanızı, kolay ve hızlı yapılandırmak ve ayarlamak olmalıdır.       |
|Kullanılabilirlik / çalışma süresi    |    En az % 99,9 çalışma süresi, SaaS uygulaması veya platformu olmalıdır.     |
|Azure Active Directory     |    Teklifinizi Azure Active Directory (Azure AD) ile etkin onay Federasyon çoklu oturum açma (SSO) (Azure AD Federasyon SSO) izin vermeniz gerekir.     |

## <a name="test-drive"></a>Test Sürüşü

Bir veya daha fazla sanal makine olarak-a-hizmet altyapı (Iaas) veya hizmet olarak yazılım-a-(SaaS) uygulamaları aracılığıyla dağıtın. Bir sanal makine ya da bir iş ortağı-tarafından barındırılan yönetilen çözümün tamamında otomatik sağlama yayımlama seçeneği olan test sürüşü yararı Kılavuzlu Tur. Bir test sürüşüne müşteriniz için ek ücret ödemeden bir değerlendirme sağlar. Müşterinizin deneme sürümü deneyimi ile etkileşim kurmak amacıyla, mevcut bir Azure müşterisi olmasına gerek yok. 

Şu adresten bizimle [amp testdrive](mailto:amp-testdrive@microsoft.com) kullanmaya başlamak için. 

|Gereksinimler  |Ayrıntılar |
|---------|---------|
| Market uygulaması vardır   |    Iaas veya SaaS aracılığıyla bir veya daha fazla sanal makineler.      |

## <a name="interactive-demo"></a>Etkileşimli Tanıtım

Etkileşimli bir Demo kullanarak müşterilerinize çözümünüzün Kılavuzlu bir deneyim sağlayın. Etkileşimli tanıtım seçeneği yayımlama avantajı, karmaşık çözümünüzü karmaşık sağlamanıza gerek kalmadan bir deneme sürümü deneyimi sağlamak içindir. 

## <a name="virtual-machine-offer"></a>Sanal makine teklifi

Müşteriniz ile ilişkili bir abonelik için bir sanal gereç dağıttığınızda sanal makine teklif türünü kullanın. Tam olarak Kullandıkça Öde veya-kendi-lisansını getir (KLG) lisanslama modelleri kullanarak etkin ticaret vm'leridir. Microsoft commerce işlemi'ni barındırır ve müşterinizin sizin adınıza düzenler. Müşteri ile Microsoft, tüm kurumsal sözleşmeler de dahil olmak üzere, tercih edilen ödeme ilişkisi kullanmanın avantajı sahip olursunuz.

> [!NOTE]
> Şu anda bir kurumsal anlaşma ile ilişkili parasal taahhütler VM'niz Azure kullanımınızın karşı ancak lisans ücretleri, yazılımlara karşı olanağına sahip olursunuz.  
> 
> [!NOTE]
> Bulma ve dağıtım vm'nizin görüntüsü yayımlama ve özel bir teklif olarak fiyatlandırma belirli bir müşteri kümesine kısıtlayabilirsiniz. Özel teklifler kilidini açma özelliği için size en yakın müşterileriniz için özel teklifler oluşturun ve özelleştirilmiş yazılım ve koşulları sunar. Özelleştirilmiş koşulları senaryolarında, özel fiyatlandırma ve koşullar hem de sınırlı sürüm yazılımı erken erişim alanı öncülüğünde fırsatlarla dahil olmak üzere çeşitli vurgulamak etkinleştirin. Özel teklifler ile ayrıntılarını yeni bir SKU'ya oluşturarak, özel fiyatlandırma vermek veya ürünler için sınırlı sayıda müşteri etkinleştirin.  
> *   Özel teklifler hakkında daha fazla bilgi için özel teklifleri Azure Marketi sayfasında bulunan ziyaret [azure.microsoft.com/blog/private-offers-on-azure-marketplace](https://azure.microsoft.com/blog/private-offers-on-azure-marketplace).  

| Gereksinim | Ayrıntılar |  
|:--- |:--- | 
| Faturalama ve ölçüm | Sanal makinenizin KLG ya da Kullandıkça Öde aylık faturalandırma desteklemesi gerekir. |  
| Azure ile uyumlu sanal sabit disk (VHD) | Windows veya Linux Vm'leri oluşturulmalıdır. <ul> <li>Linux VHD'si oluşturma hakkında daha fazla bilgi için bkz. [Azure'da desteklenen Linux dağıtımı](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).</li> <li>Bir Windows VHD oluşturma hakkında daha fazla bilgi için bkz. [Azure ile uyumlu bir VHD oluşturma](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md).</li> </ul> |  

>[!Note]
>Bulut çözümü sağlayıcıları (CSP) iş ortağı kanalı katılımı kullanıma sunuldu.  Lütfen [bulut çözüm sağlayıcıları](./cloud-solution-providers.md) teklifinizi Microsoft CSP aracılığıyla pazarlama hakkında daha fazla bilgi için iş ortağı kanalı.

## <a name="next-steps"></a>Sonraki adımlar

Zaten yapmadıysanız, 

- [Kayıt](https://azuremarketplace.microsoft.com/sell) Market'te.

Kayıtlı ve yeni bir teklif oluşturur veya mevcut bir proje üzerinde çalışmaya,

- [Bulut iş ortağı portalında oturum açın](https://cloudpartner.azure.com) oluşturmak veya teklifiniz tamamlayın.
- Bkz: [sanal makine teklifi](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-virtual-machine-offer) daha fazla bilgi için.
