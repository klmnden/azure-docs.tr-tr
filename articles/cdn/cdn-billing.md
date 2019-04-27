---
title: Azure CDN faturalamasını anlama | Microsoft Docs
description: Bu SSS, Azure CDN faturalamasını nasıl çalıştığını açıklar.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: magattus
ms.openlocfilehash: af8e57f39b5b83b1d1be09c29d8b6eb5d49c7b6c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60782069"
---
# <a name="understanding-azure-cdn-billing"></a>Azure CDN faturalamasını anlama

Bu SSS, Azure içerik teslim ağı (CDN) tarafından barındırılan içerik için faturalandırma yapısını açıklar.

## <a name="what-is-a-billing-region"></a>Fatura bölgesi nedir?
Hangi nesnelerin teslim edilmek üzere Azure CDN from ücretlendirilir belirlemek için kullanılan bir coğrafi bölge bir fatura bölgedir. Geçerli fatura alanları ve bunların bölgeleri aşağıdaki gibidir:

- Bölge 1: Kuzey Amerika, Avrupa, Orta Doğu ve Afrika

- Bölge 2: Asya Pasifik (Japonya dahil)

- Bölge 3: Güney Amerika

- Bölge 4: Avustralya ve Yeni Zelanda

- Bölge 5: Hindistan

Varlık noktası (POP) bölgeleri hakkında daha fazla bilgi için bkz: [bölgeye göre Azure CDN POP konumları](https://docs.microsoft.com/azure/cdn/cdn-pop-locations). Örneğin, Meksika'da bulunan POP Kuzey Amerika bölgesi içinde olduğundan ve bu nedenle bölge 1 dahil edilir. 

Azure CDN fiyatlandırması hakkında daha fazla bilgi için bkz: [Content Delivery Network fiyatlandırması](https://azure.microsoft.com/pricing/details/cdn/).

## <a name="how-are-delivery-charges-calculated-by-region"></a>Nasıl teslim ücretler bölgeye göre hesaplanır?
Azure CDN fatura bölgesi içeriği son kullanıcıya teslim kaynak sunucusunun konumunu temel alır. ' % S'hedef (fiziksel konumunu) istemcinin bulunduğu faturalama bölgesinde olarak kabul edilmez.

Örneğin, Meksika'da bulunan bir kullanıcı isteği yayınlar ve bu isteği bir Amerika Birleşik Devletleri POP eşlemesi ya da trafiği koşullar nedeniyle bulunan bir sunucu tarafından hizmet verilen, Amerika Birleşik Devletleri bulunduğu faturalama bölgesinde olacaktır.

## <a name="what-is-a-billable-azure-cdn-transaction"></a>Faturalanabilir Azure CDN işlem nedir?
CDN sonlandıran tüm HTTP (S) tüm yanıt türleri içeren Faturalanabilir bir olay isteğidir: başarı, başarısızlık veya diğer. Ancak, farklı yanıtlar miktarları farklı trafik oluşturabilir. Örneğin, *304 değiştirilmedi* ve küçük oldukları için biraz trafik diğer yalnızca üstbilgi yanıtları oluşturmak üst bilgi yanıtı; benzer şekilde, hata yanıtları (örneğin, *404 Bulunamadı*) Faturalanabilir niteliktedir ancak küçük yanıt yükünde nedeniyle küçük bir ücret.

## <a name="what-other-azure-costs-are-associated-with-azure-cdn-use"></a>Azure CDN kullanım ile ilişkili diğer Azure maliyetlerini misiniz?
Azure CDN kullanarak, ayrıca, nesneler için kaynak kullanılan hizmetler bazı kullanım ücreti alınmaz. Bu genellikle genel CDN kullanım maliyeti küçük bir bölümünü ücretlerdir.

İçeriğiniz için kaynağı Azure Blob Depolama kullanıyorsanız, önbellek dolguları için aşağıdaki depolama ücretleri de ücretler:

- Gerçek GB kullanıldı: Depolamanın kaynak nesnelerinizin.

- GB cinsinden aktarımları: CDN önbellekleri doldurmak için aktarılan veri miktarı.

- İşlemler: Önbelleği doldurma gerektiğinde.

Azure depolama faturalamasını hakkında daha fazla bilgi için bkz: [anlama Azure depolama Faturalaması – bant genişliği, işlemler ve kapasite](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/).

Kullanıyorsanız *barındırılan hizmet sunumu*, şu şekilde ücretlendirilirsiniz:

- Azure işlem süresi: Kaynak davranan işlem örnekleri.

- Azure işlem aktarımı: Azure CDN önbellekleri doldurmak için işlem örneklerden veri aktarır.

İstemci (bağımsız olarak kaynak hizmeti) bayt aralığı isteklerini kullanıyorsa, aşağıdaki maddeler geçerlidir:

- A *bayt aralığı istek* CDN, Faturalanabilir bir işlemdir. Bir istemci bir bayt aralığı isteği gönderdiğinde, bu isteği bir alt nesne için (aralık) kümesidir. CDN'nin istenen içeriği yalnızca kısmi bir bölümü ile yanıt verir. Bu kısmi yanıt Faturalanabilir bir işlemdir ve aktarımı miktarı aralığı yanıt (ek üst bilgiler) boyutuyla sınırlıdır.

- Bir istek yalnızca bir nesnenin bir parçası için (bir byte-range üst bilgisini belirterek) ulaştığında, CDN kendi önbelleğine tüm nesne getirin. Sonuç olarak, CDN Faturalanabilir işlem kısmi yanıt olsa bile, Faturalanabilir işlem kaynaktan tam nesnenin boyutunu içerebilir.

## <a name="how-much-transfer-activity-occurs-to-support-the-cache"></a>Önbellek desteklemek için ne kadar aktarım etkinlik gerçekleşir?
CDN POP önbelleğini doldurmak için her durumda önbelleğe alınmasını nesne başlangıcı için bir talep gönderir. Sonuç olarak, her önbellek isabetsizliği bir Faturalanabilir işlem kaynağı doğurur. Önbellek kaçaklarının sayısı, bir dizi faktöre bağlıdır:

- İçeriği nasıl önbelleğe olur: İçeriği yüksek TTL (zaman yaşam) sahip / zaman aşımı değerleri ve ise, uyumlu şekilde ilerlemesi için sık erişilen önbellek, ardından yükünü büyük çoğunluğu, popüler CDN tarafından işlenir. Tipik bir iyi isabetli önbellek okuması oranı iyi istemci isteklerine % 10'küçüktür ' nin başlangıç noktasına döndürmeniz anlamına gelir % 90, bir önbellek isabetsizliği veya nesne için yenileyin.

- Kaç tane düğümleri nesne yüklenmeye gerekir: Bir düğüm bir nesne ve kaynaktan her yüklenişinde bir Faturalanabilir işlem doğurur. Sonuç olarak, daha fazla küresel içerik (daha fazla düğümünden erişilebilir) içinde daha Faturalandırılabilir sonuçlanır.

- TTL etkiler: Bir nesne için daha yüksek bir TTL, kaynaktan daha az sıklıkta getirilmesi gereken anlamına gelir. Ayrıca, istemciler, tarayıcılar gibi uzun, CDN işlemleri azaltabilirsiniz nesne önbelleğe alabilir anlamına gelir.

## <a name="how-do-i-manage-my-costs-most-effectively"></a>My maliyetlerini en etkili bir şekilde nasıl yönetebilirim?
İçeriğinizi olası en uzun TTL ayarlayın. 
