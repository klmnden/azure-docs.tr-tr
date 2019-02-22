---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: a8979edf94c0dd0271293feb28c18530faeba09c
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56660515"
---
## <a name="key-transactions-max-transactions-allowed-in-10-seconds-per-vault-per-regionsup1sup"></a>Anahtar işlemleri (10 saniye içinde başına izin verilen en fazla işlem kasası bölge başına<sup>1</sup>):

|Anahtar türü|HSM anahtarı<br>Anahtar oluşturma|HSM anahtarı<br>Diğer tüm işlemler|Yazılım anahtarı<br>Anahtar oluşturma|Yazılım anahtarı<br>Diğer tüm işlemler|
|:---|---:|---:|---:|---:|
|RSA 2048 bit|5|1000|10|2000|
|RSA 3072 bit|5|250|10|500|
|4096 bit RSA|5|125|10|250|
|ECC P-256|5|1000|10|2000|
|ECC P-384|5|1000|10|2000|
|ECC P-521|5|1000|10|2000|
|ECC SECP256K1|5|1000|10|2000|

> [!NOTE]
> Yukarıdaki tabloda RSA 2048 bit yazılım-keys için 10 saniye başına 2000 GET işlem an olanağı ve RSA 2048 bit HSM-anahtarları için biz 10 saniye başına 1000 GET işlemlere izin görüyoruz.
>
> Kısıtlama eşikleri ağırlıklı ve zorlama toplamları üzerinde olduğunu unutmayın. Örneğin, yukarıdaki tabloda RSA HSM anahtarları GET işlemleri gerçekleştirirken, 4096 bit anahtarlar 2048 bit anahtara göre kullanılacak 8 katı daha pahalı olduğunu görebiliriz (1000/125 beri = 8). Bu nedenle, belirli bir 10 saniyelik aralık AKV istemci tam olarak karşılaşıldığında önce aşağıdakilerden birini yapın bir `429` azaltma HTTP durum kodu:
> - 2000 RSA 2048 bit anahtarı yazılım GET işlemleri **veya**
> - 1000 RSA 2048 bit anahtar HSM GET işlemleri **veya**
> - 125 RSA 4096 bit anahtar HSM GET işlemleri **veya**
> - 124 RSA 4096 bit anahtar HSM GET işlemleri ve 8 RSA 2048 bit anahtar HSM GET işlemleri.

## <a name="secrets-managed-storage-account-keys-and-vault-transactions"></a>Gizli diziler, yönetilen depolama hesabı anahtarları ve kasa işlemleri:
| İşlem türü | Bölge başına 10 saniye içinde izin verilen en fazla işlem kasası<sup>1</sup> |
| --- | --- |
| Tüm işlemler |2000 |

Bkz: [Azure Key Vault azaltma Kılavuzu](../articles/key-vault/key-vault-ovw-throttling.md) bu sınırlar aşıldığında azaltmayı hakkında bilgi için.

<sup>1</sup> tüm işlem türleri için anahtar kasası başına 5 x bir aboneliği genelinde sınırı yoktur. Abonelik başına 10 saniye içinde diğer işlemleri abonelik başına 5000 hareketleri sınırlı Örneğin, HSM -.
