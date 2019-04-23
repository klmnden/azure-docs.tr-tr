---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 4/20/2019
ms.author: tamram
ms.openlocfilehash: d96f400332b7953b34a157b3b52cf00bb20db76e
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60012511"
---
| Kaynak | Hedef        |
|----------|---------------|
| Tek bir blob kapsayıcısı en büyük boyutu | En fazla depolama hesabı kapasitesi aynı |
| En fazla bir blok içinde blok blob veya ekleme blobu | 50.000 blok |
| Bir bloğu içinde bir blok blobunun en büyük boyutu | 100 MiB |
| Blok blobunun en büyük boyutu | 50.000 x 100 MIB (yaklaşık 4,75 tib'a kadar) |
| Bir bloğu içinde bir ek blobu en büyük boyutu | 4 MiB |
| Bir ek blobunun en büyük boyutu | 50.000 x 4 MIB (yaklaşık 195 GiB) |
| Bir sayfa blobu en büyük boyutu | 8 TiB |
| Saklı erişim ilkeleri blob kapsayıcı başına en fazla sayısı | 5 |
|Tek bir blob için hedef performans düzeyleri |Depolama hesabı giriş/çıkış limitlerde<sup>1</sup> |

<sup>1</sup> tek nesne aktarım hızı, ancak bunlarla sınırlı olmamak gibi çeşitli etkenlere bağlıdır: eşzamanlılık, istek boyutu, performans katmanı, yüklemeleri için kaynak ve hedef yüklemeleri için hızı. Yararlanmak için [yüksek performanslı blok blobu](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/) performans geliştirmeleri, Put blobu veya blok yerleştirme istek boyutu > 4 MIB (> 256 MiB Data Lake depolama Gen2'ye veya premium performans blok blobu depolama için) kullanın.