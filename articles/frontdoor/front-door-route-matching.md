---
title: Azure ön kapısı hizmeti - Yönlendirme kural eşleşen izleme | Microsoft Docs
description: Bu makale Azure ön kapısı hizmet gelen bir istek için kullanmak üzere hangi yönlendirme kuralını nasıl eşleştiğini anlamanıza yardımcı olur.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: eec99bde0ea73a99a9dc1345f938b821a95a7c05
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60736301"
---
# <a name="how-front-door-matches-requests-to-a-routing-rule"></a>Ön kapısı istek yönlendirme kuralı için nasıl eşleşir?

Bir istek üzerinde gölünüzdeki olduğunda bağlantı kurduktan sonra bir SSL el sıkışması yapılması bir ön kapısı ortamı ön kapısı yapan ilk şeylerden biri tüm yapılandırmalardan, hangi isteği eşleştirmek için belirli yönlendirme kuralı ve sonra alma belirliyor tanımlı eylem. Belgesinde, ön kapısı bir HTTP isteği işlerken kullanılacak rota yapılandırmayı nasıl belirlediğini açıklar.

## <a name="structure-of-a-front-door-route-configuration"></a>Bir ön kapısı yol yapılandırması yapısı
Bir ön kapısı yönlendirme kuralı yapılandırması, iki ana bölümden oluşur: "sol tarafındaki" ve "sağ tarafında". Biz istek işleminin nasıl sağ tarafı tanımlar sol tarafına rotanın gelen istek eşleştirme.

### <a name="incoming-match-left-hand-side"></a>Gelen eşleşme (sol tarafı)
Aşağıdaki özellikler, gelen istek yönlendirme kuralı (veya sol tarafı) ile eşleşip eşleşmediğini belirler:

* **HTTP protokolleri** (HTTP/HTTPS)
* **Konaklar** (örneğin, www\.foo.com, \*. bar.com)
* **Yolları** (örneğin, /\*, /users/\*, /file.gif)

Bu özellikler böylece her bir birleşimi Protokolü/konak/yol olası eşleşme kümesi kullanıma dahili olarak genişletilir.

### <a name="route-data-right-hand-side"></a>Rota verilerini (sağ taraf)
Önbelleğe almanın etkin olup veya belirli bir yol için bir isteği işlemek nasıl karar bağlıdır. Bu nedenle, istek için önbelleğe alınan yanıt de yoksa, biz isteği yapılandırılmış arka uç havuzundaki uygun arka uca iletir.

## <a name="route-matching"></a>Eşleşen yol
Bu bölümde, belirli bir ön kapısı yönlendirme kuralını nasıl eşleştirme üzerinde odaklanır. Temel kavramlar sizi her zaman için eşleşmesidir **en belirgin eşleşen ilk** görünümlü yalnızca "sol tarafındaki" konumunda.  İlk HTTP protokolü, ardından Frontend ana bilgisayar ardından yolu göre eşleştirme.

### <a name="frontend-host-matching"></a>Frontend ana bilgisayar eşleştirme
Frontend ana eşleştirirken mantığı aşağıdaki gibi kullanırız:

1. Bir konaktaki bir tam eşleşme ile yönlendirme bakın.
2. Tam ön uç konak eşleşiyorsa isteği onaylayabileceğini ve 400 Hatalı istek hatası gönderin.

Bu işlem daha da açıklamak için örnek bir yapılandırma (yalnızca sol tarafı) ön kapısı yolların göz atalım:

| Yönlendirme kuralı | Frontend ana bilgisayar | `Path` |
|-------|--------------------|-------|
| A | foo.contoso.com | /\* |
| B | foo.contoso.com | /Users/\* |
| C | www\.fabrikam.com, foo.adventure works.com'u  | /\*, /images/\* |

Aşağıdaki gelen istekler için ön kapı göndermediyse aşağıdaki Yönlendirme kuralları yukarıdaki karşı eşleşir:

| Gelen frontend ana bilgisayar | Yönlendirme kuralları eşleşmesi |
|---------------------|---------------|
| foo.contoso.com | A, B |
| www\.fabrikam.com | C |
| images.fabrikam.com | 400\. hata: Bozuk İstek |
| foo.Adventure Works.com'u | C |
| contoso.com | 400\. hata: Bozuk İstek |
| www\.adventure works.com'u | 400\. hata: Bozuk İstek |
| www\.adının | 400\. hata: Bozuk İstek |

### <a name="path-matching"></a>Yol ile eşleşen
Belirli bir ön uç konak belirleme ve filtreleme için yalnızca ön uç barındıran rotalarla olası yönlendirme kuralları sonra ön kapısı sonra isteği yola göre yönlendirme kurallarını filtreler. Ön uç ana bilgisayarları olarak benzer bir mantık kullanırız:

1. Yol üzerindeki bir tam eşleşme ile herhangi bir yönlendirme kural arayın
2. Tam eşleşme yol, sözcük joker karakterle eşleşen yolu yönlendirme kurallarını arayın
3. Yönlendirme kuralı yok ile eşleşen bir yol bulunmazsa, isteği reddetmek ve bir 400 döndürür: Hatalı istek Hatası HTTP yanıtı.

>[!NOTE]
> Yollar joker karakteri olmadan tam eşleşme yollar kabul edilir. Yol eğik çizgiyle sona ererse bile, yine de tam eşleşme değerlendirilir.

Daha fazla açıklamak için başka bir örnekler kümesini göz atalım:

| Yönlendirme kuralı | Frontend ana bilgisayar    | `Path`     |
|-------|---------|----------|
| A     | www\.contoso.com | /        |
| B     | www\.contoso.com | /\*      |
| C     | www\.contoso.com | /AB      |
| D     | www\.contoso.com | /abc     |
| E     | www\.contoso.com | /abc/    |
| F     | www\.contoso.com | /ABC/\*  |
| G     | www\.contoso.com | / abc/def |
| H     | www\.contoso.com | /Path/   |

Bu yapılandırmayı göz önünde bulundurulduğunda, aşağıdaki örnek eşleşen tablo neden olur:

| Gelen istek    | Eşleşen bir rota |
|---------------------|---------------|
| www\.contoso.com/            | A             |
| www\.contoso.com/a           | B             |
| www\.contoso.com/ab          | C             |
| www\.contoso.com/abc         | D             |
| www\.contoso.com/abzzz       | B             |
| www\.contoso.com/abc/        | E             |
| www\.contoso.com/abc/d       | F             |
| www\.contoso.com/abc/def     | G             |
| www\.contoso.com/abc/defzzz  | F             |
| www\.contoso.com/abc/def/ghi | F             |
| www\.contoso.com/path        | B             |
| www\.contoso.com/path/       | H             |
| www\.contoso.com/path/zzz    | B             |

>[!WARNING]
> </br> Catch tüm bir tam eşleşme frontend ana bilgisayar için yönlendirme kuralı yok ise yolu rota (`/*`), daha sonra herhangi bir yönlendirme kuralı bir eşleşme olmayacaktır.
>
> Örnek yapılandırma:
>
> | Yol | Ana bilgisayar             | `Path`    |
> |-------|------------------|---------|
> | A     | Profile.contoso.com | /api/\* |
>
> Eşleşen Tablo:
>
> | Gelen istek       | Eşleşen bir rota |
> |------------------------|---------------|
> | Profile.domain.com/Other | Yok. 400\. hata: Bozuk İstek |

### <a name="routing-decision"></a>Yönlendirme karar
Biz tek bir ön kapısı yönlendirme kural eşleşen sonra biz sonra isteği işlemek nasıl seçmeniz gerekir. Ardından, eşleşen yönlendirme kuralı için bir önbelleğe alınan yanıt kullanılabilir ön kapısı varsa aynı istemciye hizmet. Aksi takdirde, değerlendirilen bir sonraki şey mi yapılandırdığınıza olan [URL yeniden yazma (özel yönlendirme yolunu)](front-door-url-rewrite.md) eşleşen yönlendirme için kural ya da değil. Ardından tanımlanan özel iletme yol değilse, istek uygun arka uca olarak yapılandırılmış bir arka uç havuzundaki iletilen. Aksi takdirde istek yolu olarak başına güncelleştirilir [özel iletme yolu](front-door-url-rewrite.md) tanımlı ve ardından arka uç için iletme.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
