---
title: Azure faturalama API'leri | Microsoft Docs
description: Azure kaynak tüketimini ve eğilimleri sağlamak için kullanılan Azure faturalama kullanım ve RateCard API'ları hakkında bilgi edinin.
services: ''
documentationcenter: ''
author: tonguyen
manager: tonguyen
editor: ''
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 10/9/2017
ms.author: mobandyo;bryanla
ms.openlocfilehash: f0e546095ca1079ccc59c51b9b5230be04415eb5
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="use-azure-billing-apis-to-programmatically-get-insight-into-your-azure-usage"></a>Program aracılığıyla Azure kullanımınızı bir anlayış almak için Azure faturalama API'lerini kullanın
Azure faturalama API'leri kullanımı ve kaynak veri çekmek için tercih edilen veri analizi araçlarınızı kullanın. Azure kaynak kullanımı ve RateCard API'leri doğru şekilde tahmin etmek ve maliyetlerinizi yönetmenize yardımcı olabilir. API'ler bir kaynak sağlayıcısı ve Azure Resource Manager tarafından kullanıma sunulan API ailesinin bir parçası olarak uygulanır.  

## <a name="azure-invoice-download-api-preview"></a>Azure fatura indirme API (Önizleme)
Bir kez [katılımı tam](billing-manage-access.md#opt-in), önizleme sürümünü kullanarak indirme faturaları [fatura API](/rest/api/billing). Özellikleri içerir:

* **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portal](https://portal.azure.com) aracılığıyla veya [Azure PowerShell cmdlet'lerini](/powershell/azure/overview) hangi kullanıcılar veya uygulamalar için erişim sağlayabilmek için belirtmek için Aboneliğin kullanım verileri. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için faturalama Okuyucu, okuyucu, sahibi veya katkıda bulunan rolü ekleyin.
* **Tarih filtreleme** -kullanım `$filter` tüm faturalar ters sırasına göre fatura dönem bitiş tarihi almak için parametre. 

> [!NOTE]
> Bu özellik ilk önizleme sürümünde olduğu ve uyumsuz geriye dönük değişiklikler tabi olabilir. Şu anda kullanılabilir belirli abonelik teklifler (Kurumsal Sözleşme, CSP, desteklenmeyen AIO) ve Azure Almanya değil.

## <a name="azure-resource-usage-api-preview"></a>Azure kaynak kullanım API'si (Önizleme)
Azure kullanmak [kaynak kullanım API'si](https://msdn.microsoft.com/library/azure/mt219003) tahmini Azure tüketim verilerinizi almak için. API içerir:

* **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portal](https://portal.azure.com) aracılığıyla veya [Azure PowerShell cmdlet'lerini](/powershell/azure/overview) hangi kullanıcılar veya uygulamalar için erişim sağlayabilmek için belirtmek için Aboneliğin kullanım verileri. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için faturalama Okuyucu, okuyucu, sahibi veya katkıda bulunan rolü ekleyin.
* **Saatlik veya günlük toplamalar** - arayanlar olup Azure kullanım verilerini saatlik istedikleri aralıkları belirtebilirsiniz veya günlük aralıkları. Varsayılan günlük.
* **(Kaynak etiketleri içerir) örneği meta veri** – alma tam Kaynak URI gibi örnek düzeyi ayrıntısı (/subscriptions/ {subscrıptıon-ID} /..), kaynak grubu bilgileri ve kaynak etiketleri. Çapraz ücretlendirme kullanım örnekleri ister için bu meta veriler, belirleyici biçimde ve program aracılığıyla kullanım etiketlere göre ayırmak yardımcı olur.
* **Kaynak meta verilerini** -kaynak ayrıntılarını ölçüm adı, ölçer kategori, ölçüm alt kategorisi, birim ve bölge gibi daha iyi ne tüketilen anlamak çağıran verin. Ayrıca Azure portalı kaynak meta verileri terminolojisi hizalamak için çalışıyoruz Azure kullanım CSV, CSV ve diğer genel kullanıma yönelik deneyimleri deneyimleri arasında verilerin bağıntısını olanak faturalama EA.
* **Farklı bir teklif türleri için kullanım** – kullanım verileri, Kullandıkça Öde, MSDN, parasal taahhüt, kredi ve EA, gibi teklif türleri için kullanılabilir dışında [CSP](https://docs.microsoft.com/azure/cloud-solution-provider/billing/azure-csp-invoice#retrieve-usage-data-for-a-specific-subscription).

## <a name="azure-resource-ratecard-api-preview"></a>Azure kaynak RateCard API (Önizleme)
Kullanım [Azure kaynak RateCard API](https://msdn.microsoft.com/library/azure/mt219005) mevcut Azure kaynakları ve her biri için tahmini fiyatlandırma bilgileri listesini almak için. API içerir:

* **Azure rol tabanlı erişim denetimi** -erişim ilkelerinizi yapılandırmasına [Azure portal](https://portal.azure.com) aracılığıyla veya [Azure PowerShell cmdlet'lerini](/powershell/azure/overview) hangi kullanıcılar veya uygulamalar için erişim sağlayabilmek için belirtmek için RateCard verileri. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için okuyucu, sahibi veya katkıda bulunan rolü ekleyin.
* **Kullandıkça Öde, MSDN, parasal taahhüt ve kredi teklifleri için destek (EA ve [CSP](https://docs.microsoft.com/azure/cloud-solution-provider/billing/azure-csp-pricelist#get-prices-by-using-the-azure-rate-card) desteklenmiyor)** -bu API Azure teklifi düzeyi oranı bilgi sağlar.  Bu API'yi çağıran kaynak ayrıntılarını ve ücretlerin almak için teklif bilgileri geçmesi gerekir. EA teklifleri kayıt göre oranları özelleştirdikten çünkü EA oranları sağlamak şu anda kaydedemiyoruz. 

## <a name="scenarios"></a>Senaryolar
Kullanım ve RateCard API'leri birleşimiyle olası hale getirilen senaryolardan bazıları şunlardır:

* **Azure ay sırasında harcadığı** - kullanım birleşimini kullanın ve RateCard API'ları, bulut daha iyi fikir almak için bir ay sırasında harcadığı. Kullanım ve Ücret tahminleri saatlik ve günlük demet analiz edebilirsiniz.
* **Uyarıları ayarlamak** – tahmini bulut kullanımı ve ücretleri almak ve kaynak veya parasal tabanlı uyarıları ayarlamak için kullanım ve RateCard API'lerini kullanın.
* **Fatura tahmin** – tahmini tüketim ve bulut harcamanız ve fatura fatura döneminin sonunda ne olacağını tahmin etmek için makine öğrenimi algoritmalarını uygulamak alın.
* **Analiz maliyetiyle öncesi tüketim** –, iş yükleri için Azure taşıdığınızda ne kadar faturanızı beklenen kullanımınız için olacaktır tahmin etmek için RateCard API'sini kullanın. Ayrıca var olan iş yükleri diğer Bulut veya özel bulutlara varsa, Azure ile kullanımınızı eşleştirebilirsiniz oranları Azure daha iyi kestirmek için harcadıkları. Bu tahmin teklif ve karşılaştırma ve parasal taahhüt ve kredi gibi Kullandıkça Öde ötesinde farklı teklif türleri arasında karşıtlığı Özet olanağı sağlar. API da bölgeye göre maliyet farklılıkları görme olanağı verir ve dağıtım kararları vermenize yardımcı olmak için durum maliyet çözümlemesi yapmanıza izin verir.
* **Çözümlemeleri** -
  
  * Başka bir bölgede ya da başka bir Azure kaynak yapılandırması iş yüklerini çalıştırmak için daha uygun maliyetli olup olmadığını belirleyebilirsiniz. Azure kaynak maliyetleri kullanmakta olduğunuz Azure bölgesinde değişebilir.
  * Başka bir Azure Teklif türü bir Azure kaynağı üzerinde daha iyi oranı sağlar, ayrıca belirleyebilirsiniz.
  
## <a name="partner-solutions"></a>İş ortağı çözümleri
[Cruiser ve Microsoft Azure fatura API tümleştirme bulut](billing-usage-rate-card-partner-solution-cloudcruiser.md) açıklar nasıl [Azure Pack için bulut Cruiser'ın Express](http://www.cloudcruiser.com/partners/microsoft/) doğrudan Microsoft Azure Pack (WAP) portalından çalışır. Bu gibi durumlarda, Microsoft Azure özel veya barındırılan genel bulut işletimsel ve finansal yönleri sorunsuz bir şekilde bir tek kullanıcı arabiriminden yönetebilirsiniz.   

## <a name="next-steps"></a>Sonraki adımlar
* Github'daki kod örnekleri gözden geçirin:
  * [Fatura API kod örneği](https://go.microsoft.com/fwlink/?linkid=845124)

  * [Kullanım API'si kod örneği](https://github.com/Azure-Samples/billing-dotnet-usage-api)

  * [RateCard API kod örneği](https://github.com/Azure-Samples/billing-dotnet-ratecard-api)

* Azure Kaynak Yöneticisi hakkında daha fazla bilgi edinmek için [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md). 

* Bulut bir anlayış yardımcı olmak gerekli araçları paketi hakkında daha fazla bilgi için harcadıkları, Gartner makalesine bakın [BT Finansal Yönetimi (ITFM) araçları için Pazar Kılavuzu](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

