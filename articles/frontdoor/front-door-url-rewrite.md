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
ms.openlocfilehash: 00fe3aa7a641b9d07aad90a9d008a99efc6e9d97
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993485"
---
# <a name="url-rewrite-custom-forwarding-path"></a>URL yeniden yazma (özel Yönlendirme yolu)
Azure ön kapısı hizmeti, isteğe bağlı yapılandırmanıza olanak sağlayarak URL yeniden yazma destekler **özel yönlendirme yolunu** arka ucuna iletmek için istek oluşturulurken kullanılacak. Hiçbir özel iletme yolu sağlanmazsa, varsayılan olarak, ardından ön kapısı gelen URL yolu iletilen istekte kullanılan URL'nin kopyalar. Seçilen arka uç için yapılandırılmış iletilen istekte kullanılan ana bilgisayar üst bilgisini aynıdır. Okuma [arka uç ana bilgisayar üstbilgisi](front-door-backend-pool.md#hostheader) ne işe yarar ve nasıl yapılandıracağınızı öğrenin.

Eşleşen bir joker karakter yolu iletilen yolu için gelen yolunun herhangi bir bölümü kopyalayın URL yeniden yazma özel yönlendirme yolunu kullanarak güçlü bir parçasıdır (Bu yol segmentler **yeşil** aşağıdaki örnekte parçaları):
</br>
![Azure ön kapısı URL yeniden yazma][1]

## <a name="url-rewrite-example"></a>URL yeniden yazma örneği
Yönlendirme kuralı aşağıdaki ön uç ana bilgisayarları ve yapılandırılmış yolları ile göz önünde bulundurun:

| Ana bilgisayarlar      | Yollar       |
|------------|-------------|
| www.contoso.com | /\*         |
|            | /foo        |
|            | /foo/\*     |
|            | /foo/çubuğu /\* |

Aşağıdaki tabloda, ilk sütun gelen istekleri örneklerini gösterir ve ikinci sütun, "en-specific" eşleşen yol 'Path' ne olacağını gösterir.  Tablonun ilk satırını üçüncü ve sonraki sütunlarının örnekler yapılandırılmış **özel ileten yollarını**, iletilen istek yolunun eşleşmesi durumunda olacaktır örnekleri temsil eden bu sütunların satır rest ile söz konusu satırdaki isteği.

Biz ikinci satırda okuma, örneğin, gelen istek için diyor `www.contoso.com/sub`, özel yönlendirme yolunu olduysa `/`, yönlendirilmiş yol sonra `/sub`. Özel iletme yolu ise `/fwd/`, yönlendirilmiş yol sonra `/fwd/sub`. Ve kalan sütunların benzeri. **Vurgulanmış** aşağıdaki yollardan bölümlerini joker karakter eşleştirmeyi parçası olan bölümleri temsil eder.


| Gelen istek       | Çoğu özel eşleşme yolu | /          | /FWD/          | /foo/          | /foo/çubuğu /          |
|------------------------|--------------------------|------------|----------------|----------------|--------------------|
| www.contoso.com/            | /\*                      | /          | /FWD/          | /foo/          | /foo/çubuğu /          |
| www.contoso.com/**alt**     | /\*                      | /**alt**   | /FWD/**alt**   | /foo/**alt**   | /foo/çubuğu/**alt**   |
| www.contoso.com/**a/b/c**   | /\*                      | /**a/b/c** | /FWD/**a/b/c** | /foo/**a/b/c** | /foo/çubuğu/**a/b/c** |
| www.contoso.com/foo         | /foo                     | /          | /FWD/          | /foo/          | /foo/çubuğu /          |
| www.contoso.com/foo/        | /foo/\*                  | /          | /FWD/          | /foo/          | /foo/çubuğu /          |
| www.contoso.com/foo/**çubuğu** | /foo/\*                  | /**Çubuk**   | /FWD/**çubuğu**   | /foo/**çubuğu**   | /foo/çubuğu/**çubuğu**   |


## <a name="optional-settings"></a>İsteğe bağlı ayarlar
Verilen yönlendirme kuralı ayarlarını belirtebilirsiniz ek isteğe bağlı ayar vardır:

* **Yapılandırma önbelleğe** - devre dışı ya da bu yönlendirme kuralı ile eşleşen istekleri önbelleğe alınmış içeriği kullanmak çalışmaz ve bunun yerine her zaman arka ucundan getirir, belirtilmemiş. Daha fazla bilgi edinin [ön kapısı ile önbelleğe alma](front-door-caching.md).



## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [ön kapı oluşturmak](quickstart-create-front-door.md).
- Bilgi [ön kapısı işleyişi](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-url-rewrite/front-door-url-rewrite-example.jpg