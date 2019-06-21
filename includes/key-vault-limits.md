---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 0b9d87fd7929607da8407ae5bbfb2f6dd6d69dab
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188776"
---
#### <a name="key-transactions-maximum-transactions-allowed-in-10-seconds-per-vault-per-regionsup1sup"></a>Anahtar işlemleri (10 saniye içinde başına izin verilen en fazla işlem sayısı, bölge başına kasası<sup>1</sup>):

|Anahtar türü|HSM anahtarı<br>Anahtar oluşturma|HSM anahtarı<br>Diğer tüm işlemler|Yazılım anahtarı<br>Anahtar oluşturma|Yazılım anahtarı<br>Diğer tüm işlemler|
|:---|---:|---:|---:|---:|
|2\.048 bit RSA|5|1000|10|2,000|
|3\.072 bit RSA|5|250|10|500|
|4\.096 bit RSA|5|125|10|250|
|ECC P-256|5|1000|10|2,000|
|ECC P-384|5|1000|10|2,000|
|ECC P-521|5|1000|10|2,000|
|ECC SECP256K1|5|1000|10|2,000|

> [!NOTE]
> Önceki tabloda, 10 saniye başına 2.000 GET işlemleri için RSA 2.048 bit yazılım anahtarları, izin verildiğini bakın. RSA 2.048 bit HSM-anahtarları için 10 saniye başına 1000 GET işlem izin verilir.
>
> Kısıtlama eşikleri ağırlıklı ve zorlama üzerinde kendi toplamıdır. RSA HSM anahtarları, GET işlemleri gerçekleştirirken Örneğin, önceki tabloda olarak gösterilen, bunu 2.048 bit anahtara göre 4.096 bit anahtarları kullanmak sekiz katı daha pahalıdır. Çünkü 1000/125 = 8.
>
> Belirtilen bir 10 saniyelik aralık içinde bir Azure anahtar kasası istemci yapabilirsiniz *tek* bulduğu önce aşağıdaki işlemlerden birini bir `429` azaltma HTTP durum kodu:
> - 2\.000 bit RSA 2.048 yazılım anahtarı GET işlemleri
> - 1\.000 bit RSA 2.048 HSM anahtarı GET işlemleri
> - 125 RSA 4.096 bit HSM anahtarı GET işlemleri
> - 124 RSA 4.096 bit HSM anahtar GET işlemleri ve 8 bit RSA 2.048 HSM anahtarı GET işlemleri

#### <a name="secrets-managed-storage-account-keys-and-vault-transactions"></a>Gizli diziler, yönetilen depolama hesabı anahtarlarını ve kasa işlemleri:
| İşlem türü | 10 saniye içinde başına izin verilen en fazla işlem sayısı, bölge başına kasası<sup>1</sup> |
| --- | --- |
| Tüm işlemler |2,000 |

Bu sınırlar aşıldığında azaltmayı nasıl hakkında daha fazla bilgi için bkz: [Azure Key Vault azaltma Kılavuzu](../articles/key-vault/key-vault-ovw-throttling.md).

<sup>1</sup> aboneliği genelinde sınırı tüm işlem türleri için beş anahtar kasası sınırıdır. Örneğin, abonelik başına HSM diğer işlem abonelik başına 10 saniye içinde 5.000 işlem için sınırlıdır.
