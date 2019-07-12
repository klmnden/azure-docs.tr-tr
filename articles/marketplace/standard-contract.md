---
title: Standart sözleşme | Azure
description: Azure Market ve AppSource standart Sözleşmesi
services: Azure, Marketplace, Compute, Storage, Networking
author: qianw211
ms.service: marketplace
ms.topic: article
ms.date: 07/05/2019
ms.author: ellacroi
ms.openlocfilehash: 80c157423572d356026f257e81d52650ce01d3e8
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620358"
---
# <a name="standard-contract"></a>Standart Sözleşme

Müşteriler için satın alma işlemini basitleştirmek ve yazılım satıcıları için yasal karmaşıklığını azaltmak için Microsoft Market bir işlemde kolaylaştırmaya yardımcı olması için standart Sözleşme şablonunu sunar. Özel hüküm ve koşullar oluşturmak yerine, Azure Marketi yayımcılarının yazılımlarını sözleşmesindeki standart Kıdemli ve bir kez kabul etmek için müşterilerin yalnızca ihtiyaç duyan, teklif seçebilirsiniz. Standart sözleşme burada bulunabilir: [ https://go.microsoft.com/fwlink/?linkid=2041178 ](https://go.microsoft.com/fwlink/?linkid=2041178). 

Hüküm ve koşulları bir teklif için bir teklif bulut iş ortağı Portalı'nda oluştururken Market sekmesinde tanımlanır. Standart sözleşme seçeneği Evet olarak ayarı değiştirerek etkinleştirilir.

![Standart sözleşme seçeneğini etkinleştirme](media/marketplace-publishers-guide/standard-contract.png)

>[!Note] 
>Ayrı hüküm ve koşullar standart sözleşme kullanmayı seçerseniz, hala gereklidir [bulut çözümü sağlayıcısı](./cloud-solution-providers.md) kanal.

## <a name="standard-contract-amendments"></a>Standart sözleşme tarihli Amendments

Standart sözleşme tarihli Amendments standart sözleşme kolaylık ve ürün ya da işletmeniz özelleştirilmiş koşullarıyla seçmek yayımcılar izin verir.  Müşteriler, yalnızca bunlar zaten gözden geçirdi ve Microsoft Standart sözleşmeyi kabul tarihli amendments sözleşme gözden geçirmeniz gerekir.

Azure Market yayımcıları için tarihli amendments iki tür vardır:

* Evrensel tarihli Amendments: Şu tarihli amendments Evrensel standart sözleşme tüm müşteriler için uygulanır. Ürün satın alma akıştaki her müşteri için evrensel tarihli amendments gösterilmektedir.

![Evrensel tarihli Amendments](media/marketplace-publishers-guide/universal-amendaments.png)

* Özel tarihli Amendments: Azure Market, kiracılar için hedeflenen özel tarihli amendments için bir sağlama da vardır. Yalnızca belirli müşteriler için hedeflenen özel tarihli amendments standart sözleşme değildirler. Yayımcılar Kiracı hedeflemek istediğinizi seçebilirsiniz. Kiracınıza ait müşteriler standart sözleşme ve hedeflenen tarihli amendments bölümünde ürün satın.

![Özel tarihli Amendments](media/marketplace-publishers-guide/custom-amendaments.png)

>[!Note] 
>Özel tarihli amendments ile hedeflenen müşteriler satın alma sırasında standart koşullarını Evrensel değişiklik de alırsınız.

>[!Note]
>Standart sözleşme tarihli Amendments aşağıdaki teklif türlerini destekler: Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar), sanal makineler, kapsayıcılar, kapsayıcı uygulamaları.

### <a name="customer-experience"></a>Müşteri Deneyimi

Müşteriler Azure portalında satın alma işlemi sırasında Microsoft Standart sözleşme ve tarihli amendments ürünle ilgili koşulları görmeye olacaktır.

![Azure portal müşteri deneyimi.](media/marketplace-publishers-guide/ibiza-customer-experience.png)

### <a name="api"></a>API

Müşterileri kullanabilir `Get-AzureRmMarketplaceTerms` teklif koşullarını almak ve kabul etmek için. Standart sözleşme ve ilişkili tarihli amendments cmdlet çıktısında döndürülür.

---
