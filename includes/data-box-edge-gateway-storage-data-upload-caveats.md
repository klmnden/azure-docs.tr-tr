---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/14/2019
ms.author: alkohli
ms.openlocfilehash: 8bc4b78d262cf5f30076e90b40b92c4f3237924a
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56333799"
---
Azure'a taşınırken aşağıdaki uyarılar için veri uygulayın.

- Birden fazla cihaz olanla aynı kapsayıcıya yazamaz öneririz.
- Kopyalanan nesne olarak aynı ada sahip bir bulutta (örneğin, bir bloba veya bir dosya) var olan bir Azure nesne varsa, cihaz bulut dosyanın üzerine yazar.
- Paylaşım klasör altında oluşturulan bir boş dizin hiyerarşisi (olmadan tüm dosyaları) için blob kapsayıcılar karşıya yüklenmemiş.
- Büyük dosyaları kopyalama işlemi aralıklı hatalar için yeniden denediğinden robocopy kullanmanızı öneririz.
- Paylaşım için oluşturma sırasında tanımlanan BLOB türü ile eşleşmiyor BLOB'ları ile Azure depolama kapsayıcısı ilişkili paylaşımı yükler, blobları gibi güncelleştirilmez. Örneğin, cihazda bir blok blobu paylaşımı oluşturun. Paylaşım, sayfa BLOB'ları olan bir var olan bulut kapsayıcısı ile ilişkilendirin. Bu paylaşım dosyaları indirmek için yenileyin. Bazı bulut sayfa blobları olarak depolanır yenilenmiş dosyaları değiştirin. Göreceğiniz karşıya yükleme hataları.
