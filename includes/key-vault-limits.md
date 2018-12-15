---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: efa367157a8fd896cdc9680bf2ab6ba6a9e3dbb0
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53430058"
---
Anahtar işlemleri (10 saniye içinde başına izin verilen en fazla işlem kasası bölge başına<sup>1</sup>):

|Anahtar türü|HSM anahtarı<br>Anahtar oluşturma|HSM anahtarı<br>Diğer tüm işlemler|Yazılım anahtarı<br>Anahtar oluşturma|Yazılım anahtarı<br>Diğer tüm işlemler|
|:---|---:|---:|---:|---:|
|RSA 2048 bit|5|1000|10|2000|
|RSA 3072 bit|5|250|10|500|
|4096 bit RSA|5|125|10|250|
|ECC P-256|5|1000|10|2000|
|ECC P-384|5|1000|10|2000|
|ECC P-521|5|1000|10|2000|
|ECC SECP256K1|5|1000|10|2000|
|

> [!NOTE]
> Aşağıdaki tabloda bakarsanız, yazılım tarafından desteklenen anahtarlar için HSM yanı sıra, 10 saniye başına 2000 işlemleri desteklenen 10 saniye başına 1000 işlem izin veriyoruz izin veriyoruz olduğunu görürsünüz. Yazılım tarafından desteklenen işlemler 2048 anahtarları 3072 anahtarları için 500/2000 veya 0.4 oranıdır. Bu bir müşteri mu 500 3072 anahtar işlemleri, 10 saniye bunlar kendi maksimum sınırına ulaşmadığınız ve herhangi bir anahtar işlemleri yapamazsınız anlamına gelir. 
   
|Anahtar türü  | Yazılım anahtarı |HSM anahtarı  |
|---------|---------|---------|
|RSA 2048 bit     |    2000     |   1000    |
|RSA 3072 bit     |     500    |    250     |
|4096 bit RSA     |    125     |    250     |
|ECC P-256     |    2000     |  1000     |
|ECC P-384     |    2000     |  1000     |
|ECC P-521     |    2000     |  1000     |
|ECC SECP256K1     |    2000     |  1000     |


Gizli diziler, yönetilen depolama hesabı anahtarları ve kasa işlemleri:
| İşlem türü | Bölge başına 10 saniye içinde izin verilen en fazla işlem kasası<sup>1</sup> |
| --- | --- |
| Tüm işlemler |2000 |
|

Bkz: [Azure Key Vault azaltma Kılavuzu](../articles/key-vault/key-vault-ovw-throttling.md) bu sınırlar aşıldığında azaltmayı hakkında bilgi için.

<sup>1</sup> tüm işlem türleri için anahtar kasası başına 5 x bir aboneliği genelinde sınırı yoktur. Abonelik başına 10 saniye içinde diğer işlemleri abonelik başına 5000 hareketleri sınırlı Örneğin, HSM -.
