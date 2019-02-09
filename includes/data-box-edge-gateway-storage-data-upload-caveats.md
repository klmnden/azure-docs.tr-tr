---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/07/2019
ms.author: alkohli
ms.openlocfilehash: ad7ceffed61665a729d4391b5f0ff2935375c253
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967681"
---
Azure'a taşınırken aşağıdaki uyarılar için veri uygulayın.

- Birden fazla cihaz olanla aynı kapsayıcıya yazamaz öneririz.
- Kopyalanan nesne olarak aynı ada sahip bir bulutta (örneğin, bir bloba veya bir dosya) var olan bir Azure nesne varsa, cihaz bulut dosyanın üzerine yazar.
- Paylaşım klasör altında oluşturulan bir boş dizin hiyerarşisi (olmadan tüm dosyaları) için blob kapsayıcılar karşıya yüklenmemiş.
- Büyük dosyaları kopyalama işlemi aralıklı hatalar için yeniden denediğinden robocopy kullanmanızı öneririz.
