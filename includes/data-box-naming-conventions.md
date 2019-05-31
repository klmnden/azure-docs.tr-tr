---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: d965b89659a9e3dc01035221a729f094c336eb5b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244570"
---
| Varlık                                       | Kurallar   |
|----------------------------------------------|-----------------------------------------------------|
| Kapsayıcı adları için blok blobu ve sayfa blobu | 3-63 karakter uzunluğunda olan geçerli bir DNS adı olmalıdır. <br>  Bir harf veya sayı ile başlamalıdır. <br> Yalnızca küçük harf, sayı ve tire (-) içerebilir. <br> Kısa çizgiden (-) hemen önce ve sonra bir harf veya rakam gelmelidir. <br> Art arda kısa çizgi adlarında izin. |
| Azure dosyaları için paylaşım adları                  | Yukarıdakiyle aynı                                          |
| Azure dosyaları için dizin ve dosya adları     |<li> Durum koruma, büyük/küçük harfe ve 255 karakterden uzun olmamalıdır. </li><li> (/) İleri eğik çizgiyle bitemez. </li><li>Sağlanırsa, otomatik olarak kaldırılacak. </li><li> Şu karakterler izin verilmez: <code>" \\ / : \| < > * ?</code></li><li> Ayrılmış URL karakterleri doğru şekilde atlanmalıdır. </li><li> Geçersiz URL yolu karakterler izin verilmez. Kod noktaları gibi \\uE000 Unicode karakterler geçerli değildir. Bazı ASCII veya Unicode karakterleri gibi denetim karakterleri (0x00 için 0x1F \\u0081, vb.), izin verilmeyen de. Unicode yöneten kurallar için HTTP/1.1 dizelerde RFC 2616 ', bölüm 2.2 bakın: Temel kurallar ve RFC 3987. </li><li> Şu dosya adlarına izin verilmez: LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, PRN, AUX, NUL, CON, CLOCK$, nokta karakteri (.) ve iki nokta karakteri (..).</li>|
| Blok blobu ve sayfa blobu için blob adları      | </li><li>Blob adları büyük/küçük harfe duyarlıdır ve karakterler herhangi bir düzende sıralanabilir. </li><li>Blob adı 1 ila 1024 karakter uzunluğunda olmalıdır. </li><li>Ayrılmış URL karakterleri doğru şekilde atlanmalıdır. </li><li>Blob adını oluşturan yolun bölümleri 254 karakterden uzun olamaz. Yol bölümü, arka arkaya gelen sınırlayıcı karakterlerinin (örneğin eğik çizgi "/") arasında yer alan ve bir sanal dizinin adına karşılık gelen dizedir.</li> |
