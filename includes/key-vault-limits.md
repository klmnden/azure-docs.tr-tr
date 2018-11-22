---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: ed0c387f9785336fbf18b3fd3c0cd9a7b09df633
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52280006"
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

Gizli diziler, yönetilen depolama hesabı anahtarları ve kasa işlemleri:
| İşlem türü | Bölge başına 10 saniye içinde izin verilen en fazla işlem kasası<sup>1</sup> |
| --- | --- |
| Tüm işlemler |2000 |
|

Bkz: [Azure Key Vault azaltma Kılavuzu](../articles/key-vault/key-vault-ovw-throttling.md) bu sınırlar aşıldığında azaltmayı hakkında bilgi için.

<sup>1</sup> tüm işlem türleri için anahtar kasası başına 5 x bir aboneliği genelinde sınırı yoktur. Abonelik başına 10 saniye içinde diğer işlemleri abonelik başına 5000 hareketleri sınırlı Örneğin, HSM -.
