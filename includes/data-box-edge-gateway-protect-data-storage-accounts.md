---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 23587e617c62cfe4aa24256330c5f7673dd1c674
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684490"
---
Cihazınızı verilerinizi azure'da için hedef olarak kullanılan bir depolama hesabı ile ilişkilidir. Depolama hesabına erişim için abonelik ve iki adet 512 bit Depolama tarafından denetlenen erişim anahtarları, depolama hesabıyla ilişkilendirilmiş.

Veri kutusu sınır cihazı, depolama hesabına eriştiğinde anahtarlarından birini kimlik doğrulaması için kullanılır. Başka bir tuşa anahtarlarını düzenli aralıklarla döndürmenizi sağlayan yedekte tutulur.

Güvenlik nedenleriyle, çok sayıda veri merkezleri anahtar döndürme gerektirir. Anahtar döndürme için bu en iyi uygulamaları izlemenizi öneririz:

- Depolama hesabı anahtarınız depolama hesabınızın kök parolasına benzer. Hesap anahtarınızın dikkatli bir şekilde koruyun. Yok parola diğer kullanıcılara dağıtmak, sabit kod veya başkalarının erişebileceği düz metin, herhangi bir yere kaydedin.
- [Hesap anahtarınızın yeniden](../articles/storage/common/storage-account-manage.md#regenerate-access-keys) düşünüyorsanız, Azure portalını kullanarak tehlikeye girmiş olabilecek.
- Düzenli aralıklarla Azure yöneticiniz değiştirmek veya bu depolama hesabına doğrudan erişmek için Azure portalında depolama bölümünü kullanarak birincil veya ikincil anahtarını yeniden gerekir.