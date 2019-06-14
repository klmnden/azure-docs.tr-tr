---
title: Azure faturalandırma API'leri ile Azure kullanım alın | Microsoft Docs
description: Azure kaynak tüketimine ve eğilimleri hakkında Öngörüler sağlamak için kullanılan Azure faturalama kullanım ve RateCard API'leri hakkında bilgi edinin.
services: ''
documentationcenter: ''
author: tonguyen
manager: mumami
editor: ''
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 5/10/2018
ms.author: erikre
ms.custom: seodec18
ms.openlocfilehash: 99772579fd28b0880d497abb038fb25fd2a6139e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60369276"
---
# <a name="use-azure-billing-apis-to-programmatically-get-insight-into-your-azure-usage"></a>Azure faturalandırma API'lerini program aracılığıyla Azure kullanımınızı öngörü almak için kullanın
Azure faturalandırma API'lerini kullanımı ve kaynak veri çekmek üzere, tercih edilen veri analizi araçları kullanın. Azure Kaynak Kullanımı ve RateCard API'leri maliyetlerinizi doğru tahmin etmenize ve yönetmenize yardımcı olabilir. API'ler, bir kaynak sağlayıcısı ve Azure Resource Manager tarafından kullanıma sunulan API'ler ailesinin bir parçası olarak uygulanır.  

## <a name="azure-invoice-download-api-preview"></a>Azure fatura indirme API'si (Önizleme)
Bir kez [katılımı tam](billing-manage-access.md#opt-in), önizleme sürümünü kullanarak indirme faturalar [fatura API](/rest/api/billing). Özellikler şunlardır:

* **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portalında](https://portal.azure.com) aracılığıyla veya [Azure PowerShell cmdlet'lerini](/powershell/azure/overview) hangi kullanıcılar ve uygulamalar erişebilir belirtmek için Aboneliğin kullanım verileri. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin.
* **Tarih filtreleme** -kullanım `$filter` tüm faturalara ters sırasına göre fatura dönemi bitiş tarihi almak için parametre.

> [!NOTE]
> Bu özellik, ilk önizleme sürümünde olan ve geriye dönük uyumsuz değişiklikler tabi olabilir. Şu anda, kullanılabilir belirli abonelik teklif (Kurumsal Anlaşma, CSP, desteklenmeyen AIO) ve Azure Almanya değildir.

## <a name="azure-resource-usage-api-preview"></a>Azure kaynak kullanım API'si (Önizleme)
Azure kullanan [kaynak kullanım API'si](/previous-versions/azure/reference/mt219003(v=azure.100)) tahmini Azure kullanım verilerinizi almak için. API içerir:

* **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portalında](https://portal.azure.com) aracılığıyla veya [Azure PowerShell cmdlet'lerini](/powershell/azure/overview) hangi kullanıcılar ve uygulamalar erişebilir belirtmek için Aboneliğin kullanım verileri. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin.
* **Saatlik veya günlük toplamalar** - çağıranlar olup Azure kullanım verilerini saatlik istedikleri demetlerine belirtebilirsiniz veya günlük demetlerine. Varsayılan günlük.
* **Örnek meta veri (kaynak etiketleri içerir)** – alma tam Kaynak URI gibi örnek düzeyi ayrıntısı (/subscriptions/ {abonelik-kimliği} /..), kaynak grubu bilgileri ve kaynak etiketleri. Çapraz ücretlendirme, kullanım örnekleri ister için bu meta veriler belirleyici ve program aracılığıyla etiketlere göre kullanım ayırmanıza yardımcı olur.
* **Kaynak meta verileri** -kaynak ayrıntıları ölçüm adı, ölçüm kategorisi, ölçüm alt kategorisi, birim ve bölge gibi daha iyi bir ne tüketildiğinin'ın anlayış çağıran verin. Ayrıca Azure portalı kaynak meta verileri terminolojisi hizalama çalışıyoruz Azure kullanım CSV, CSV ve diğer genel kullanıma yönelik deneyimler deneyimler verilerin bağıntısını izin vermek için faturalama EA.
* **Farklı bir teklif türleri için kullanım** – kullanım verilerini, teklif türleri gibi Kullandıkça Öde, MSDN, parasal taahhüt, parasal kredi ve EA, kullanılabilir dışında [CSP](https://docs.microsoft.com/azure/cloud-solution-provider/billing/azure-csp-invoice#retrieve-usage-data-for-a-specific-subscription).

## <a name="azure-resource-ratecard-api-preview"></a>Azure kaynak RateCard API'si (Önizleme)
Kullanım [Azure kaynak RateCard API'si](/previous-versions/azure/reference/mt219005(v=azure.100)) kullanılabilir Azure kaynaklarını ve tahmini fiyatlandırma bilgileri için her bir listesini almak için. API içerir:

* **Azure rol tabanlı erişim denetimi** -erişim ilkelerinizi yapılandırın [Azure portalında](https://portal.azure.com) aracılığıyla veya [Azure PowerShell cmdlet'lerini](/powershell/azure/overview) hangi kullanıcılar ve uygulamalar erişebilir belirtmek için RateCard verileri. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için okuyucu, sahibi veya katkıda bulunan rolüne ekleyin.
* **Kullandıkça Öde, MSDN, parasal taahhüt ve parasal kredi teklifleri için destek (EA ve [CSP](https://docs.microsoft.com/azure/cloud-solution-provider/billing/azure-csp-pricelist#get-prices-by-using-the-azure-rate-card) desteklenmiyor)** -bu API, Azure teklifi düzeyinde fiyat bilgileri sağlar.  Bu API'yi çağıran kaynak ayrıntıları ve fiyatları almak için teklif bilgilerini geçmesi gerekir. EA teklifler kayıt tarifelerine özelleştirdiğiniz çünkü EA fiyatlandırması sağlamak şu anda kaydedemiyoruz.

## <a name="scenarios"></a>Senaryolar
Kullanım ve RateCard API'leri ile birlikte olası yapılan senaryolardan bazıları şunlardır:

* **Azure harcamalarınızı ay boyunca** - kullanım bileşimini kullanın ve bulutunuzu daha iyi fikir almak için RateCard API'leri ay boyunca ayırın. Kullanım ve Ücret tahminleri saatlik ve günlük demetler çözümleyebilirsiniz.
* **Uyarıları Ayarlama** – kullanım ve RateCard API'leri kullanın tahmini bulut tüketimi ve ücretleri alın ve kaynak veya parasal tabanlı uyarılar ayarlayın.
* **Fatura tahmin** – Get tahmini tüketim ve bulut harcamalarını ve fatura bir faturalama döneminin sonunda ne olacağını tahmin etmek için makine öğrenimi algoritmaları uygulayın.
* **Maliyet analizi ön tüketim** – RateCard API'si iş yüklerinizi Azure'a taşıyarak ne kadar faturanız için beklenen kullanım olacaktır tahmin etmek için kullanın. Ayrıca, diğer bulut ya da özel Bulutlar mevcut iş yüklerini varsa, Azure ile kullanım eşleyebilirsiniz ücretler Azure daha iyi bir tahminini almak için harcadığınız. Bu tahmin, teklif, karşılaştırma ve parasal taahhüt ve para kredisi gibi Kullandıkça Öde, ötesinde farklı bir teklif türleri arasındaki kontrastı Özet olanağı sağlar. API, bölgeye göre maliyet farklılıkları görme olanağı sunar ve dağıtım kararları vermenize yardımcı olmak için bir durum maliyet analizi yapmanıza izin verir.
* **Benzetim analizi** -

  * Başka bir bölgede veya başka bir Azure kaynak yapılandırması iş yüklerini çalıştırmak için daha uygun maliyetli olup olmadığını belirleyebilirsiniz. Azure kaynak maliyetleri kullanmakta olduğunuz Azure bölgesine göre değişebilir.
  * Başka bir Azure Teklif türü bir Azure kaynağında daha iyi bir fiyat sağlar, ayrıca belirleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
* Github'daki kod örneklerine bakın:
  * [Fatura API’si kod örneği](https://go.microsoft.com/fwlink/?linkid=845124)

  * [Kullanım API’si kod örneği](https://github.com/Azure-Samples/billing-dotnet-usage-api)

  * [RateCard API’si kod örneği](https://github.com/Azure-Samples/billing-dotnet-ratecard-api)

* Azure Resource Manager hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md).
