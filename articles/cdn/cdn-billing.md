---
title: Azure CDN faturalama anlama | Microsoft Docs
description: Bu SSS, Azure CDN faturalama nasıl çalıştığı açıklanmaktadır.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: v-deasim
ms.openlocfilehash: 218c493c772dc8fd212efaf60a0599fa2e896411
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32163521"
---
# <a name="understanding-azure-cdn-billing"></a>Azure CDN faturalama anlama

Bu SSS, Azure içerik teslim ağı (CDN) tarafından barındırılan içerik için fatura yapısını açıklar.

## <a name="what-is-a-billing-region"></a>Bir fatura bölge nedir?
Hangi oranı nesneleri teslimat için Azure CDN doludur belirlemek için kullanılan bir coğrafi bölge bir fatura bölgedir. Geçerli fatura bölgeleri ve bunların bölgeleri aşağıdaki gibidir:

- Bölge 1: Kuzey Amerika, Avrupa, Orta Doğu ve Afrika

- Bölge 2: Asya Pasifik (Japonya dahil)

- Bölge 3: Güney Amerika

- Bölge 4: Avustralya ve Yeni Zelanda

- Bölge 5: Hindistan

Bulunma noktası (POP) bölgeler hakkında daha fazla bilgi için bkz: [bölgeye göre Azure CDN POP konumları](https://docs.microsoft.com/azure/cdn/cdn-pop-locations). Örneğin, Meksika içinde bulunan POP Kuzey Amerika bölgede olması ve bu nedenle 1 bölgesinde eklenir. 

Azure CDN fiyatlandırma hakkında daha fazla bilgi için bkz: [içerik teslim ağı fiyatlandırma](https://azure.microsoft.com/is-is/pricing/details/cdn/).

## <a name="how-are-delivery-charges-calculated-by-region"></a>Teslimat giderleri bölgeye göre nasıl hesaplanır?
Azure CDN fatura bölge için son kullanıcı içerik teslim kaynak sunucusunun konumunu temel alır. İstemcinin (fiziksel konumunu) hedef fatura bölge dikkate alınmaz.

Örneğin, Meksika içinde bulunan bir kullanıcı isteği yayınlar ve bu isteği bir Amerika Birleşik Devletleri POP eşliği veya trafiği koşul nedeniyle bulunan bir sunucu tarafından hizmet verilen fatura bölge Amerika Birleşik Devletleri olacaktır.

## <a name="what-is-a-billable-azure-cdn-transaction"></a>Faturalanabilir Azure CDN işlem nedir?
Tüm yanıt türleri içerir Faturalanabilir bir olay CDN sonlandırır herhangi bir HTTP (S) istek olduğu: başarı, hata veya diğer. Ancak, farklı yanıtlar farklı trafik tutarlar oluşturabilir. Örneğin, *304 değişiklik* ve küçük olduklarından az trafik diğer yalnızca üstbilgi yanıtları oluşturmak üstbilgisi yanıt; benzer şekilde, hata yanıtları (örneğin, *404 Bulunamadı*) Faturalanabilir olan ancak küçük yanıt yükü nedeniyle küçük bir maliyet doğurur.

## <a name="what-other-azure-costs-are-associated-with-azure-cdn-use"></a>Diğer Azure maliyetler Azure CDN kullanım ile ilişkilidir?
Azure CDN kullanarak, bazı nesneler için kaynak kullanılan hizmetleri kullanım ücretleri doğurur. Bu maliyetleri genellikle CDN kullanım masraflar küçük bir bölümünü yayımlanır.

İçeriğiniz için kaynak Azure Blob Depolama Birimi kullanıyorsanız, siz de aşağıdaki depolama önbellek dolgular için ücretlendirme:

- Kullanılan gerçek GB: kaynağı nesnelerinizi depolamanın.

- GB cinsinden aktarımları: aktarılan veri miktarını CDN önbellekleri doldurmak için.

- Önbellek doldurmak için gerektiği gibi işlemler:.

Azure Storage faturalama hakkında daha fazla bilgi için bkz: [anlama Azure depolama faturalama – bant genişliği, işlemleri ve kapasite](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/).

Kullanıyorsanız *hizmet teslim barındırılan*, aşağıdaki gibi ücret uygulanabilir:

- Azure işlem saati: kaynağı hareket işlem örnekleri.

- Azure işlem aktarımı: verileri Azure CDN önbellekleri doldurmak için işlem örneklerden aktarır.

İstemci (bağımsız olarak kaynak hizmeti) bayt aralığı isteklerini kullanıyorsa, aşağıdaki maddeler geçerlidir:

- A *bayt aralığı isteği* CDN adresindeki Faturalanabilir bir işlemdir. Bir istemci bir bayt aralığı isteği gönderdiğinde, bu istek bir alt nesne için (aralık) kümesidir. CDN istenen içeriğin yalnızca kısmi bir bölümü ile yanıt verir. Bu kısmi yanıt Faturalanabilir bir işlemdir ve aktarımı miktarı boyutu aralık yanıt (artı üstbilgileri) sınırlıdır.

- Bir istek (bayt aralığı üstbilgi belirterek) yalnızca bir nesnenin parçası ulaştığında, CDN kendi önbelleğine tüm nesne getirin. Sonuç olarak, CDN Faturalanabilir işlem için bir kısmi yanıt olsa bile kaynaktan Faturalanabilir işlem nesnesinin tam boyutunu gerektirebilir.

## <a name="how-much-transfer-activity-occurs-to-support-the-cache"></a>Önbellek desteklemek için ne kadar aktarımı etkinlik oluşur?
CDN POP önbelleğini doldurmak için gereken her zaman bu önbelleğe alınması nesnesi için kaynağı isteğinde bulunur. Sonuç olarak, kaynak her önbellek isabetsizliği Faturalanabilir bir işlemde doğurur. Önbellek İsabetsizlik Sayısı bir dizi etkene bağlıdır:

- Nasıl alınabilir içeriktir: (time-to-live) yüksek TTL içeriğe sahip / geçerlilik süresi değerleri ve olduğundan, bu kalması sık erişilen önbellek sonra çoğunluğu yükü de popüler CDN tarafından işlenir. Bir tipik iyi önbellek isabet üzerinde % 90'ını istemci isteklerini % 10'değerinden ' nin kaynak için döndürmeniz anlamına gelir, bir önbellek isabetsizliği veya nesne için yenileme iyi orandır.

- Kaç tane düğümleri nesne yüklemeniz gerekir: isteğe bağlı olarak bir düğümün bir nesne kaynaktan her yüklenişinde Faturalanabilir hareket doğurur. Sonuç olarak, daha fazla genel içerik (daha fazla düğümü erişilir) daha Faturalanabilir işlemlerinde sonuçlanır.

- TTL etkisi: bir nesne için daha yüksek bir TTL kaynaktan daha az sıklıkta getirilmesi gerekir anlamına gelir. Ayrıca, istemciler, tarayıcıları gibi daha uzun CDN hareketlerle azaltabilir nesne önbelleğe alabilir anlamına gelir.

## <a name="how-do-i-manage-my-costs-most-effectively"></a>My maliyetleri en verimli şekilde nasıl yönetebilirim?
İçeriğinizi üzerinde olası en uzun TTL ayarlayın. 
