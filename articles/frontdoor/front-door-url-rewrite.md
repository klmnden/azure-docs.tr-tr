---
title: Azure ön kapı hizmeti - URL yeniden yazma | Microsoft Docs
description: Bu makale, nasıl Azure ön kapısı Service URL yeniden yazma yollarınızı için yapılandırılmışsa yaptığını anlamanıza yardımcı olur.
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
ms.openlocfilehash: dc2126276e3e8e0d35ce8ed1f835544386659eff
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60736199"
---
# <a name="url-rewrite-custom-forwarding-path"></a>URL yeniden yazma (özel Yönlendirme yolu)
Azure ön kapısı hizmeti, isteğe bağlı yapılandırmanıza olanak sağlayarak URL yeniden yazma destekler **özel yönlendirme yolunu** arka ucuna iletmek için istek oluşturulurken kullanılacak. Özel yönlendirme yolu sağlanmazsa Front Door varsayılan olarak yönlendirilen istekte kullanılan URL alanına gelen URL yolunu kopyalar. Yönlendirilen istekte kullanılan Konak üst bilgisi seçilen arka uç için yapılandırılmış olan değerdir. Okuma [arka uç ana bilgisayar üstbilgisi](front-door-backend-pool.md#hostheader) ne işe yarar ve nasıl yapılandıracağınızı öğrenin.

Eşleşen bir joker karakter yolu iletilen yolu için gelen yolunun herhangi bir bölümü kopyalayın URL yeniden yazma özel yönlendirme yolunu kullanarak güçlü bir parçasıdır (Bu yol segmentler **yeşil** aşağıdaki örnekte parçaları):
</br>
![Azure ön kapısı URL yeniden yazma][1]

## <a name="url-rewrite-example"></a>URL yeniden yazma örneği
Yönlendirme kuralı aşağıdaki ön uç ana bilgisayarları ve yapılandırılmış yolları ile göz önünde bulundurun:

| Ana bilgisayarlar      | Yolları       |
|------------|-------------|
| www\.contoso.com | /\*         |
|            | /foo        |
|            | /foo/\*     |
|            | /foo/çubuğu /\* |

Aşağıdaki tabloda, ilk sütun gelen istekleri örneklerini gösterir ve ikinci sütun, "en-specific" eşleşen yol 'Path' ne olacağını gösterir.  Tablonun ilk satırını üçüncü ve sonraki sütunlarının örnekler yapılandırılmış **özel ileten yollarını**, iletilen istek yolunun eşleşmesi durumunda olacaktır örnekleri temsil eden bu sütunların satır rest ile söz konusu satırdaki isteği.

Biz ikinci satırda okuma, örneğin, gelen istek için diyor `www.contoso.com/sub`, özel yönlendirme yolunu olduysa `/`, yönlendirilmiş yol sonra `/sub`. Özel iletme yolu ise `/fwd/`, yönlendirilmiş yol sonra `/fwd/sub`. Ve kalan sütunların benzeri. **Vurgulanmış** aşağıdaki yollardan bölümlerini joker karakter eşleştirmeyi parçası olan bölümleri temsil eder.


| Gelen istek       | Çoğu özel eşleşme yolu | /          | /FWD/          | /foo/          | /foo/çubuğu /          |
|------------------------|--------------------------|------------|----------------|----------------|--------------------|
| www\.contoso.com/            | /\*                      | /          | /FWD/          | /foo/          | /foo/çubuğu /          |
| www\.contoso.com/**alt**     | /\*                      | /**alt**   | /FWD/**alt**   | /foo/**alt**   | /foo/çubuğu/**alt**   |
| www\.contoso.com/**a/b/c**   | /\*                      | /**a/b/c** | /fwd/**a/b/c** | /foo/**a/b/c** | /foo/çubuğu/**a/b/c** |
| www\.contoso.com/foo         | /foo                     | /          | /FWD/          | /foo/          | /foo/çubuğu /          |
| www\.contoso.com/foo/        | /foo/\*                  | /          | /FWD/          | /foo/          | /foo/çubuğu /          |
| www\.contoso.com/foo/**çubuğu** | /foo/\*                  | /**Çubuk**   | /FWD/**çubuğu**   | /foo/**çubuğu**   | /foo/çubuğu/**çubuğu**   |


## <a name="optional-settings"></a>İsteğe bağlı ayarlar
Verilen yönlendirme kuralı ayarlarını belirtebilirsiniz ek isteğe bağlı ayar vardır:

* **Yapılandırma önbelleğe** - devre dışı ya da bu yönlendirme kuralı ile eşleşen istekleri önbelleğe alınmış içeriği kullanmak çalışmaz ve bunun yerine her zaman arka ucundan getirir, belirtilmemiş. Daha fazla bilgi edinin [ön kapısı ile önbelleğe alma](front-door-caching.md).



## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.

<!--Image references-->
[1]: ./media/front-door-url-rewrite/front-door-url-rewrite-example.jpg