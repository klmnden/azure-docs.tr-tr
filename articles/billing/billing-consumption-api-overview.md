---
title: Azure tüketim API genel bakış | Microsoft Docs
description: Azure kaynaklarınız için kullanım verileri nasıl Azure tüketim API'leri, maliyet için programlı erişim sağlar ve öğrenin.
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
ms.openlocfilehash: 16e0bdfa0fc70d5239cb4127e61891a013bf54a3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60615890"
---
# <a name="azure-consumption-api-overview"></a>Azure tüketim API genel bakış 

Azure Tüketim API'leri, Azure kaynaklarınızla ilgili maliyet ve kullanım verilerinize program aracılığıyla erişmenizi sağlar. Bu API'ler şu anda yalnızca Kurumsal kayıtları ve Web Direct Aboneliklerindeki (birkaç istisna dışında) destekler. API'leri, Azure abonelikleri diğer türlerini desteklemek için sürekli olarak güncelleştirilir.

Azure Tüketim API'leri şu verilere erişim sunar:
- Kurumsal ve Doğrudan Web Müşterileri 
    - Kullanım Ayrıntıları 
    - Market Ücretleri 
    - Rezervasyon Önerileri 
    - Rezervasyon Ayrıntıları 
    - Rezervasyon Özetleri 
- Yalnızca Kurumsal Müşteriler 
    - Fiyat listesi 
    - Bütçeler 
    - Bakiyeler 

## <a name="usage-details-api"></a>API kullanım ayrıntıları

Tüm Azure için ücretsiz olarak ve kullanım verileri almak için kullanım ayrıntılarını API'yi kullanın. 1. taraf kaynaklar. Şu anda bir kez her kaynağı günde ölçüm başına gönderilir, kullanım ayrıntı kaydı biçiminde bilgilerdir. Tüm kaynaklar arasında maliyetleri ekleyip maliyetlerini incelemenize bilgi kullanılabilir / belirli kaynakların kullanımı. 

API içerir:

-   **Ölçüm düzeyi tüketim verilerini** - maliyet, ücret yayma ölçüm kullanımı dahil olmak üzere verileri görüntüle ve hangi Azure kaynak ücreti için ilgilidir. Tüm kullanım ayrıntı kaydı günlük bir demete eşle.
-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portalında](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Filtreleme** -aşağıdaki filtreleri kullanarak kullanım ayrıntı kaydı daha küçük bir kümesi için API Sonuç kümenizi Trim:
    - Kullanım bitiş / kullanım Başlat
    - Kaynak Grubu
    - Kaynak Adı
-   **Veri toplama** -kullan ifadeleri etiketlere göre kullanım ayrıntıları için geçerli veya filtre özellikleri için OData
-   **Farklı bir teklif türleri için kullanım** -kullanım ayrıntısı bilgileri şu anda Kurumsal ve Web Direct müşterileri için kullanılabilir.

Daha fazla bilgi için bkz: teknik belirtimi [kullanım ayrıntılarını API'si](https://docs.microsoft.com/rest/api/consumption/usagedetails).

## <a name="marketplace-charges-api"></a>Market ücretlerini API

Market ücretleri API gider ve kullanım verileri tüm Market kaynakları almak için kullanın (Azure 3. taraf teklifleri). Bu veriler, tüm Market kaynaklar arasında maliyetleri ekleyip maliyetlerini incelemenize kullanılabilir / belirli kaynakların kullanımı. 

API içerir:

-   **Ölçüm düzeyi tüketim verilerini** - maliyet, ücret yayma ölçüm Market kullanım dahil olmak üzere verileri görüntüle ve ücretsiz olarak hangi kaynak için ile ilgilidir. Tüm kullanım ayrıntı kaydı günlük bir demete eşle.
-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portalında](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Filtreleme** -Market kayıtları aşağıdaki filtreleri kullanarak daha küçük bir kümesi için API Sonuç kümenizi Trim:
    - Kullanım başlangıç / bitiş kullanımı
    - Kaynak Grubu
    - Kaynak Adı
-   **Farklı bir teklif türleri için kullanım** -Market'teki bilgileri şu anda Enterprise ve Web Direct müşterileri için kullanılabilir.

Daha fazla bilgi için bkz: teknik belirtimi [Market ücretleri API](https://docs.microsoft.com/rest/api/consumption/marketplaces).

## <a name="balances-api"></a>Faturanın bakiyeler API

Kurumsal müşteriler, aylık özeti bakiyelerini, yeni satın alma işlemleri, Azure Market hizmet ücretlerini, ayarlamalar ve fazla kullanım ücretleri hakkında bilgi almak için dengeleyen API'yi kullanabilirsiniz. Geçerli fatura dönemi veya geçmişte herhangi bir nokta için bu bilgileri elde edebilirsiniz. Kuruluşlar, bu verileri el ile hesaplanan Özet Ücretlerle bir karşılaştırma gerçekleştirmek için kullanabilirsiniz. Bu API, kaynağa özgü bilgileri ve maliyetleri birleşik bir görünümünü sağlamaz. 

API içerir:

-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portalında](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Yalnızca Kurumsal müşteriler** bu API'si, yalnızca mevcut bir EA müşterileri. 
    - Müşterilerin bu API'yi çağırmak için Kurumsal yönetici izinlerine sahip olması gerekir 

Daha fazla bilgi için bkz: teknik belirtimi [bakiyeleri API](https://docs.microsoft.com/rest/api/consumption/balances).

## <a name="budgets-api"></a>Bütçe API

Kurumsal müşteriler, bu API, kaynaklar, kaynak grupları veya faturalandırma ölçümlerinde maliyet ya da kullanım bütçeleri oluşturmak için kullanabilirsiniz. Bu bilgiler belirlendikten sonra uyarı kullanıcı tarafından tanımlanan bütçe eşikleri aşıldığında bildirmek için yapılandırılabilir. 

API içerir:

-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portalında](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Yalnızca Kurumsal müşteriler** -bu API'si, yalnızca mevcut bir EA müşterileri.
-   **Yapılandırılabilir bildirimler** -bütçe dönüş olduğunda bilgilendirilmeniz için kullanıcıları belirtin.
-   **Kullanım veya maliyet tabanlı bütçelerini** -senaryonuza göre gerektiği gibi kullanım veya maliyet tabanlı bütçenizi oluşturun.
-   **Filtreleme** -bütçenizi aşağıdaki yapılandırılabilir filtreleri kullanarak kaynakları daha küçük bir alt kümesine Filtrele
    - Kaynak Grubu
    - Kaynak Adı
    - Ölçüm
-   **Yapılandırılabilir bütçe zaman dilimi boyunca** -bütçeye sıklıkla sıfırlamalısınız ve bütçe ne kadar süre için geçerli olan belirtin.

Daha fazla bilgi için bkz: teknik belirtimi [bütçelerini API](https://docs.microsoft.com/rest/api/consumption/budgets).

## <a name="reservation-recommendations-api"></a>Ayırma öneriler API'si

Ayrılmış VM örnekleri satın alma önerileri almak için bu API'yi kullanın. Öneriler değeri, müşterilerin beklenen maliyet tasarrufu analiz edin ve tutarları satın tanır şekilde tasarlanmıştır. 

API içerir:

-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portalında](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Filtreleme** -öneri sonuçlarınızı aşağıdaki filtreleri kullanarak uygun hale getirin:
    - Kapsam
    - Lookback süresi
-   **Ayırma bilgilerini farklı teklif türlerinin** -ayırma bilgileri şu anda Enterprise ve Web Direct müşterileri için kullanılabilir.

Daha fazla bilgi için bkz: teknik belirtimi [ayırma öneriler API'si](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations).

## <a name="reservation-details-api"></a>Rezervasyon ayrıntıları API

Kullanım bilgileri önceden görmek için ayırma ayrıntıları API ne kadar tüketimi karşı ne kadar ayrılmış gibi VM rezervasyon satın gerçekten kullanılıyor. Veri gördüğünüz bir VM başına ayrıntı düzeyi. 

API içerir:

-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portalında](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Filtreleme** -ayırmaları filtresini kullanarak daha küçük bir kümesi için API Sonuç kümenizi Trim:
    - Tarih aralığı
-   **Ayırma bilgilerini farklı teklif türlerinin** -ayırma bilgileri şu anda Enterprise ve Web Direct müşterileri için kullanılabilir.

Daha fazla bilgi için bkz: teknik belirtimi [rezervasyon ayrıntıları API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails).

## <a name="reservation-summaries-api"></a>Ayırma özetleri API

Toplam bilgileri önceden görmek için bu API ne kadar tüketimi ne kadar gerçekten toplama kullanılıyor karşı ayrılmış gibi VM rezervasyon satın kullanın. 

API içerir:

-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portalında](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Filtreleme** -günlük dilimde gösterimi ile aşağıdaki filtre kullanırken sonuçlarınızı uyarlayın:
    - Kullanım Tarihi
-   **Ayırma bilgilerini farklı teklif türlerinin** -ayırma bilgileri şu anda Enterprise ve Web Direct müşterileri için kullanılabilir.
-   **Günlük veya aylık toplamalar** – çağıranlar, günlük veya aylık dilimi ayırma özet verilerini isteyip istemediğinizi belirtebilirsiniz.

Daha fazla bilgi için teknik belirtimine bakın [ayırma özetleri API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries).

## <a name="price-sheet-api"></a>Fiyat listesi API'si
Kurumsal müşteri, kendi özel fiyatlandırma için tüm ölçümleri almak için bu API'yi kullanabilirsiniz. Kuruluşlar bu kullanım ayrıntılarını ve marketlerden kullanım bilgileri ile birlikte kullanım ve Market verileri kullanarak maliyet hesaplamaları gerçekleştirmek için kullanabilirsiniz. 

API içerir:

-   **Azure rol tabanlı erişim denetimi** -yapılandırma erişim ilkeleri [Azure portalında](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview) belirtmek için kullanıcılar veya uygulamalar aboneliğin kullanım verilerine erişim elde edebilirsiniz. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Yalnızca Kurumsal müşteriler** -bu API'si, yalnızca mevcut bir EA müşterileri. Doğrudan Web müşterileri, ilgili fiyatlandırma Al RateCard API'si kullanmanız gerekir. 

Daha fazla bilgi için bkz: teknik belirtimi [fiyat listesi API'si](https://docs.microsoft.com/rest/api/consumption/pricesheet).

## <a name="scenarios"></a>Senaryolar

Tüketim API'leri yapılabilir senaryolardan bazıları şunlardır:

-   **Fatura uzlaştırma** -Microsoft mı ücret bana doğru miktarı?  Faturamı ve kendim ben bunu hesaplar nedir?
-   **Ücretleri çapraz** - göre ne kadar gereksinim duyan Kuruluşumdaki ödemek için ücretlendirilirim bilebilirim?
-   **En iyi duruma getirme maliyeti** -oluşturursam ne kadar ücret biliyorum... nasıl Azure'da geçiriyorum para dışında daha fazla bilgi edinebilirim?
-   **Maliyet İzlemesi** -ne kadar ı harcama ve zaman içinde Azure kullanarak görmek istiyorum. Eğilimleri nelerdir? Nasıl daha iyi yapıyor?
-   **Azure harcamalarınızı ay boyunca** -ne kadar my geçerli ay için tarih harcama kullanıcının? My harcama ve/veya Azure kullanımını içinde ayarlamaları yapın gerekiyor mu? Ay içinde ne zaman miyim en çok kullanan Azure miyim?
-   **Uyarıları Ayarlama** - kaynak tabanlı tüketim Kurulumu istiyorsunuz veya tabanlı bir bütçe parasal tabanlı uyarı.

## <a name="next-steps"></a>Sonraki Adımlar

- Azure faturalandırma API'lerini program aracılığıyla Azure kullanımınızı öngörü almak için kullanma hakkında daha fazla bilgi için bkz: [Azure faturalama API'sine genel bakış](billing-usage-rate-card-overview.md).



