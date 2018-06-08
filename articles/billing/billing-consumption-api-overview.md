---
title: Azure tüketim API genel bakış | Microsoft Docs
description: Nasıl Azure tüketim API'leri, maliyet için programlı erişim vermek ve Azure kaynaklarınızı için kullanım verilerini öğrenin.
services: billing
documentationcenter: ''
author: Erikre
manager: dougeby
editor: ''
tags: billing
ms.assetid: 68825e85-de64-466d-b11a-8baffde836b5
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 6/07/2018
ms.author: erikre
ms.openlocfilehash: becc6d6e86fd292cfbc919f1eee36a4f5edb711c
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34851361"
---
# <a name="azure-consumption-api-overview"></a>Azure tüketim API genel bakış 

Azure tüketim API'leri, Azure kaynaklarınız için maliyet programlı erişim ve kullanım verileri verin. Bu API'leri şu anda yalnızca Kurumsal kayıtları ve Web doğrudan abonelikleri (birkaç istisna dışında) destekler. API'leri, Azure abonelikleri diğer türlerini desteklemek için sürekli olarak güncelleştirilir.

Azure tüketim API'leri erişim sağlar:
- Kuruluş ve Web doğrudan müşterileri 
    - Kullanım Ayrıntıları 
    - Market ücretleri 
    - Ayırma önerileri 
    - Ayırma ayrıntıları 
    - Ayırma özetleri 
- Yalnızca Kurumsal müşteriler 
    - Fiyat listesi 
    - Bütçeler 
    - Bakiyelerini 

## <a name="usage-details-api"></a>Kullanım ayrıntılarını API

Tüm Azure için ücret ve kullanım verileri almak için kullanım ayrıntılarını API kullanın 1 taraf kaynakları. Şu anda bir kez günde kaynak başına ölçüm başına gösterilen kullanım ayrıntı kaydı biçiminde bilgilerdir. Bilgiler, tüm kaynaklar arasında maliyetleri eklemek veya maliyetleri araştırmak için kullanılabilir / belirli kaynakların kullanımı. 

API içerir:

-   **Ölçüm düzeyi Tüketim verileri** - dahil olmak üzere maliyet, ücret yayma ölçer kullanım verileri ve hangi Azure kaynak ücret ilgilidir için. Tüm kullanım ayrıntı kaydı için günlük kova eşleyin.
-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için faturalama Okuyucu, okuyucu, sahibi veya katkıda bulunan rolü ekleyin. 
-   **Filtreleme** -kullanım ayrıntı kaydı aşağıdaki filtreleri kullanarak daha küçük bir dizi API Sonuç kümenizi kırpma:
    - Kullanım bitiş / kullanım Başlat
    - Kaynak Grubu
    - Kaynak Adı
-   **Veri toplama** -OData ifadeleri etiketlere göre kullanım ayrıntıları için geçerli veya filtre özellikleri için kullanın
-   **Farklı bir teklif türleri için kullanım** -kullanım ayrıntı bilgileri şu anda Kurumsal ve Web doğrudan müşterileri için kullanılabilir.

Daha fazla bilgi için bkz: teknik belirtimi için [kullanım ayrıntılarını API'si](https://docs.microsoft.com/rest/api/consumption/usagedetails).

## <a name="marketplace-charges-api"></a>Market ücretleri API

Tüm Market kaynaklara ücret ve kullanım verileri almak için Market ücretleri API kullanın (Azure 3 taraf teklifleri). Bu veriler, tüm Market kaynaklar arasında maliyetleri eklemek veya maliyetleri araştırmak için kullanılabilir / belirli kaynakların kullanımı. 

API içerir:

-   **Ölçüm düzeyi Tüketim verileri** - dahil olmak üzere, ücret yayma ölçer Maliyet Market kullanım verileri ve hangi kaynak Ücret için geçerlidir. Tüm kullanım ayrıntı kaydı için günlük kova eşleyin.
-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için faturalama Okuyucu, okuyucu, sahibi veya katkıda bulunan rolü ekleyin. 
-   **Filtreleme** -aşağıdaki filtreleri kullanarak Market kayıtları daha küçük bir dizi API Sonuç kümenizi kırpma:
    - Kullanım başlangıç / bitiş kullanımı
    - Kaynak Grubu
    - Kaynak Adı
-   **Farklı bir teklif türleri için kullanım** -Market bilgisi şu anda Kurumsal ve Web doğrudan müşterileri için kullanılabilir.

Daha fazla bilgi için bkz: teknik belirtimi için [Market ücretleri API](https://docs.microsoft.com/rest/api/consumption/marketplaces).

## <a name="balances-api"></a>Bakiyelerini API

Kurumsal müşteriler dengeleyen API aylık bir özetini bakiyelerini, yeni satın alma işlemleri, Azure Marketi Hizmet ücretleri, ayarlamalar ve fazla kullanım ücretleri hakkında bilgi almak için kullanabilirsiniz. Geçerli fatura dönemi veya geçmişte herhangi bir süre için bu bilgileri elde edebilirsiniz. Kuruluşlar bu verileri el ile hesaplanan Özet ücretleri karşılaştırmaya gerçekleştirmek için kullanabilirsiniz. Bu API, kaynak özgü bilgileri ve maliyetleri birleşik bir görünümünü sağlamaz. 

API içerir:

-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için faturalama Okuyucu, okuyucu, sahibi veya katkıda bulunan rolü ekleyin. 
-   **Kurumsal müşteriler yalnızca** bu API'dir yalnızca kullanılabilir EA müşteriler. 
    - Müşteriler bu API çağrısı için Kurumsal yönetici izinlerinizin olması gerekir 

Daha fazla bilgi için bkz: teknik belirtimi için [bakiyelerini API](https://docs.microsoft.com/rest/api/consumption/getbalancesbybillingaccount).

## <a name="budgets-api"></a>Bütçeleri API

Kurumsal müşteriler, bu API maliyet veya kullanımı bütçelerini kaynaklar, kaynak grupları veya faturalama ölçümler oluşturmak için kullanabilirsiniz. Bu bilgiler belirlendikten sonra uyarı kullanıcı tarafından tanımlanan bütçe eşikler aşıldığında bildirmek için yapılandırılabilir. 

API içerir:

-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için faturalama Okuyucu, okuyucu, sahibi veya katkıda bulunan rolü ekleyin. 
-   **Kurumsal müşteriler yalnızca** -bu API'dir yalnızca kullanılabilir EA müşteriler.
-   **Yapılandırılabilir bildirimleri** -bütçe dönüş geldiğinde bildirim almak için kullanıcıları belirtin.
-   **Kullanım veya maliyet dayalı bütçeleri** -ya tüketimini ya da maliyet senaryonuz tarafından gerektiği şekilde dayalı bütçenizi oluşturun.
-   **Filtreleme** -bütçenizi aşağıdaki yapılandırılabilir filtreleri kullanarak kaynakların daha küçük bir alt için filtre
    - Kaynak Grubu
    - Kaynak Adı
    - Ölçüm
-   **Yapılandırılabilir bütçe dönemler** -bütçe ne sıklıkta sıfırlamalıdır ve bütçe ne kadar süre için geçerli olan belirtin.

Daha fazla bilgi için bkz: teknik belirtimi için [bütçeleri API](https://docs.microsoft.com/rest/api/consumption/budgets).

## <a name="reservation-recommendations-api"></a>Ayırma önerileri API

VM satın ayrılmış örnekleri için önerileri almak için bu API'yi kullanın. Öneriler sağlar müşterilere beklenen maliyet tasarrufu çözümlemek ve tutarlar satın almak tasarlanmıştır. 

API içerir:

-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için faturalama Okuyucu, okuyucu, sahibi veya katkıda bulunan rolü ekleyin. 
-   **Filtreleme** -öneri sonuçlarınızı aşağıdaki filtreleri kullanarak uyarlamak:
    - Kapsam
    - Lookback süresi
-   **Ayırma bilgilerini farklı teklif türleri için** -ayırma bilgileri şu anda Kurumsal ve Web doğrudan müşterileri için kullanılabilir.

Daha fazla bilgi için bkz: teknik belirtimi için [ayırma önerileri API](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations).

## <a name="reservation-details-api"></a>Ayırma ayrıntıları API

Ne kadar tüketim karşı ne kadar ayrılmış gibi VM ayırma bilgileri önceden görmek için ayırma ayrıntıları API satın kullanım gerçekte kullanılıyor. Veri gördüğünüz bir VM başına ayrıntı düzeyi. 

API içerir:

-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için faturalama Okuyucu, okuyucu, sahibi veya katkıda bulunan rolü ekleyin. 
-   **Filtreleme** -filtresini kullanarak ayırmaları daha küçük bir dizi API Sonuç kümenizi kırpma:
    - Tarih aralığı
-   **Ayırma bilgilerini farklı teklif türleri için** -ayırma bilgileri şu anda Kurumsal ve Web doğrudan müşterileri için kullanılabilir.

Daha fazla bilgi için bkz: teknik belirtimi için [ayırması ayrıntıları API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails).

## <a name="reservation-summaries-api"></a>Ayırma özetleri API

Toplama bilgileri önceden görmek için bu API ne kadar tüketim ne kadar gerçekte toplama kullanılıyor karşı ayrılmış gibi VM ayırmaları satın kullanın. 

API içerir:

-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için faturalama Okuyucu, okuyucu, sahibi veya katkıda bulunan rolü ekleyin. 
-   **Filtreleme** -günlük birimi aşağıdaki filtresiyle kullanırken sonuçlarınızı uyarlamak:
    - Kullanım Tarihi
-   **Ayırma bilgilerini farklı teklif türleri için** -ayırma bilgileri şu anda Kurumsal ve Web doğrudan müşterileri için kullanılabilir.
-   **Günlük veya aylık toplamalar** – arayanlar, günlük veya aylık çizgisi ayırma özet verilerini isteyip istemediğinizi belirtebilirsiniz.

Daha fazla bilgi için bkz: teknik belirtimi için [ayırma özetleri API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries).

## <a name="price-sheet-api"></a>Fiyat listesi API'si
Kurumsal müşteri bu API, kendi özel fiyatlandırma için tüm ölçümler almak için kullanabilirsiniz. Kuruluşlar bu kullanım ayrıntılarını ve Pazar kullanım bilgileri ile birlikte kullanım ve marketplace verilerini kullanarak maliyet hesaplamalar gerçekleştirmek için kullanabilirsiniz. 

API içerir:

-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Arayanlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Çağıran, belirli bir Azure aboneliği kullanım verileri erişmek için faturalama Okuyucu, okuyucu, sahibi veya katkıda bulunan rolü ekleyin. 
-   **Kurumsal müşteriler yalnızca** -bu API'dir yalnızca kullanılabilir EA müşteriler. Web doğrudan müşteriler fiyatlandırma RateCard API'sini kullanmanız gerekir. 

Daha fazla bilgi için bkz: teknik belirtimi için [fiyat listesi API](https://docs.microsoft.com/rest/api/consumption/pricesheet).

## <a name="scenarios"></a>Senaryolar

API tüketiminin olası hale getirilen senaryolardan bazıları şunlardır:

-   **Fatura uzlaştırma** -vermedi Microsoft ücret bana sağ tutar?  Faturamı nedir ve ı, kendim hesaplayabilirsiniz?
-   **Ücretler arası** - göre ne kadar t, gereksinim duyan my kurum içinde ödeme yapmak için ücretlendirilmeden bilebilirim?
-   **En iyi duruma getirme maliyet** -ne kadar benden ücret alındı bilebilirim... nasıl daha fazla Azure üzerinde harcama para dışında alabilirim?
-   **Maliyet İzlemesi** -ne kadar ı çıkarım harcama ve zaman içinde Azure kullanarak görmek istiyorum. Eğilimleri nelerdir? Nasıl ı daha iyi yaparak?
-   **Azure ay sırasında harcadığı** -my geçerli ay ne kadar tarihine harcamanız kullanıcının? My harcama ve/veya Azure kullanımını içinde ayarlamaları yapın gerekiyor mu? Ay ne zaman ı Azure en tüketen 'M?
-   **Uyarıları ayarlamak** - kaynak tabanlı tüketim Kurulumu ister misiniz veya tabanlı bir bütçe para tabanlı uyarı.

## <a name="next-steps"></a>Sonraki Adımlar

- Azure faturalama API'leri kullanarak program aracılığıyla Azure kullanımınızı hakkında bilgi edinme hakkında daha fazla bilgi için bkz: [Azure faturalama API genel bakış](billing-usage-rate-card-overview.md).



