---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 4/11/2019
ms.author: tamram
ms.openlocfilehash: b3e2f018a3f1ba2563ba8cf2df6dfd4959be592e
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59737325"
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

<sup>1</sup> tek nesne aktarım hızı, ancak bunlarla sınırlı olmamak gibi çeşitli etkenlere bağlıdır: eşzamanlılık, işlem boyutu, performans katmanı, yüklemeleri için kaynak ve hedef yüklemeleri için hızı.