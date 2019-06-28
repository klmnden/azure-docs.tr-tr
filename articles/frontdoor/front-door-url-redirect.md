---
title: Azure ön kapı hizmeti - URL yeniden yönlendirme | Microsoft Docs
description: Bu makalede Azure ön kapısı Hizmeti'nin nasıl URL yeniden yönlendirmesi kendi yollar için yapılandırılmışsa desteklediği anlamanıza yardımcı olur.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: sharadag
ms.openlocfilehash: 3d77a16d24a1a843b39d97904a675518c43a525a
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332471"
---
# <a name="url-redirect"></a>URL yeniden yönlendirme
Azure ön kapısı hizmet trafiği yönlendirmek için kullanabilirsiniz. Birden çok düzeyde (protokol, ana bilgisayar adı, yol, sorgu dizesi) trafiği yönlendirebilir ve yol tabanlı yeniden yönlendirme olduğu gibi tüm işlevleri için her bir mikro hizmetin yapılandırılabilir. Bu uygulama yapılandırmasını basitleştiren, kaynak kullanımını en iyi duruma getirir ve genel ve yol tabanlı yeniden yönlendirme de dahil olmak üzere yeni yeniden yönlendirme senaryoları destekler.
</br>

![Azure ön kapısı URL yeniden yönlendirme][1]

## <a name="redirection-types"></a>Yeniden yönlendirme türü
Bir yeniden yönlendirme türü, yeniden yönlendirme amacı anlamak istemcilerin yanıt durum kodunu ayarlar. Yeniden yönlendirme aşağıdaki türleri desteklenir:

- **301 (kalıcı olarak taşındı)** : Hedef kaynak, yeni bir kalıcı URI atanmış ve kapalı bir URI'leri birini kullanmak için bu kaynak için gelecekteki tüm başvuruları ifadesi gösterir. Durum kodu 301 HTTP'yi HTTPS'ye yönlendirmeyi kullanın. 
- **(Bulunamadı) 302**: Hedef kaynak geçici olarak başka bir URI'ya altında bulunan gösterir. Yeniden yönlendirme durum değiştirmiş olmadığından, istemcinin sonraki istekleri için geçerli isteğin URI kullanmaya devam etmek için nerede depolandığının.
- **307 (geçici yeniden yönlendirme)** : Hedef kaynağın altında farklı bir URI geçici olarak bulunur ve kullanıcı aracısı gerekir, bu URI'ye otomatik bir yeniden yönlendirme gerçekleştirir, istek yöntemi değiştirin gösterir. Yeniden yönlendirme zamanla değişebilir olduğundan, istemcinin sonraki istekleri için özgün geçerli isteğin URI kullanmaya devam etmek için nerede depolandığının.
- **308 (kalıcı yeniden yönlendirme)** : Hedef kaynak, yeni bir kalıcı URI atanmış ve kapalı bir URI'leri birini kullanmak için bu kaynak için gelecekteki tüm başvuruları ifadesi gösterir. Bağlantıyı düzenleme özellikleri, istemcilerle nerede depolandığının otomatik olarak yeniden Bağla başvurular için etkin istek URI bir veya daha fazla mümkün olduğunda, sunucu tarafından gönderilen yeni başvuru.

## <a name="redirection-protocol"></a>Yeniden yönlendirme protokolü
Yeniden yönlendirme için kullanılacak protokolü ayarlayabilirsiniz. Bu HTTP HTTPS'ye yönlendirmeyi ayarlamak için yeniden yönlendirme özelliği, en yaygın kullanım durumlarından biri için sağlar.

- **Yalnızca HTTPS**: HTTP trafiği HTTPS'ye yönlendirmek istiyorsanız Protokolü HTTPS için yalnızca ayarlayın. Azure ön kapısı hizmeti her zaman yeniden yönlendirme için HTTPS yalnızca ayarlamanız önerir.
- **Yalnızca HTTP**: Bu, gelen istek için HTTP yönlendirir. Bu değeri yalnızca, trafiğiniz HTTP tutmak istiyorsanız, şifrelenmemiş kullanın.
- **Eşleşen istek**: Bu seçenek gelen istek tarafından kullanılan protokol korur. Bu nedenle, bir HTTP isteği HTTP kalır ve bir HTTPS isteği HTTPS sonrası yeniden yönlendirme kalır.

## <a name="destination-host"></a>Hedef konak
Bir yeniden yönlendirme yönlendirmeyi yapılandırma bir parçası olarak ana bilgisayar adı veya etki alanı yeniden yönlendirme isteği için de değiştirebilirsiniz. URL yeniden yönlendirmesi için ana bilgisayar adını değiştirin veya aksi halde gelen istek ana bilgisayar adı korumak için bu alanı ayarlayabilirsiniz. Bu nedenle, bu alanı kullanarak gönderilen tüm istekleri yönlendirebilirsiniz https://www.contoso.com/ * için https://www.fabrikam.com/ *.

## <a name="destination-path"></a>Hedef yolu
Yeniden yönlendirme bir parçası olarak bir URL'nin yol kesimi değiştirmek istediğiniz durumlarda bu alanı yeni yolu değerle ayarlayabilirsiniz. Aksi takdirde, yol değerini yeniden yönlendirme bir parçası olarak korumak seçebilirsiniz. Bu nedenle, bu alanı kullanarak, gönderilen tüm istekleri yönlendirebilirsiniz https://www.contoso.com/ * için https://www.contoso.com/redirected-site.

## <a name="query-string-parameters"></a>Sorgu dizesi parametreleri
Ayrıca, yeniden yönlendirilen URL'de bulunan sorgu dizesi parametrelerini değiştirebilirsiniz. Gelen istek URL'si var olan tüm Sorgu dizesinden değiştirmek için bu alan için 'Replace' ayarlayın ve sonra uygun değere ayarlayın. Aksi takdirde 'Preserve' alanına ayarlayarak sorgu dizelerini özgün kümesini tutabilirsiniz. Örneğin, bu alanı kullanarak, gönderilen tüm trafik yönlendirebilirsiniz https://www.contoso.com/foo/bar için https://www.contoso.com/foo/bar?&utm_referrer=https%3A%2F%2Fwww.bing.com%2F. 

## <a name="destination-fragment"></a>Hedef parçası
Hedef parça URL bölümüdür. '#', normalde bir sayfadaki belirli bir bölümünde kavuşmak için tarayıcılar tarafından kullanılan sonra. Yeniden yönlendirme URL'sine bir parça eklemek için bu alanı ayarlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.

<!--Image references-->
[1]: ./media/front-door-url-redirect/front-door-url-redirect.png