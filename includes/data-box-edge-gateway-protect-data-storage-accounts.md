---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 653c175a559f5c0b7dc551b396e91276332df20a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60754320"
---
Cihazınızı verilerinizi azure'da için hedef olarak kullanılan bir depolama hesabı ile ilişkilidir. Depolama hesabına erişim için abonelik ve iki adet 512 bit Depolama tarafından denetlenen erişim anahtarları, depolama hesabıyla ilişkilendirilmiş.

Veri kutusu sınır cihazı, depolama hesabına eriştiğinde anahtarlarından birini kimlik doğrulaması için kullanılır. Düzenli aralıklarla anahtarlarını döndürmek için diğer anahtarı ayrılmış tutulur.

Güvenlik nedenleriyle, çok sayıda veri merkezleri anahtar döndürme gerektirir. Anahtar döndürme için bu en iyi uygulamaları izlemenizi öneririz:

- Depolama hesabı anahtarınız depolama hesabınızın kök parolasına benzer. Hesap anahtarınızın dikkatli bir şekilde koruyun. Yok parola diğer kullanıcılara dağıtmak, sabit kod veya başkalarının erişebileceği düz metin, herhangi bir yere kaydedin.
- [Hesap anahtarınızın yeniden](../articles/storage/common/storage-account-manage.md#regenerate-access-keys) düşünüyorsanız portal Azure tehlikeye girebilir.
- Azure yöneticiniz düzenli olarak değiştirin veya doğrudan depolama hesabına erişmek için Azure portalında depolama bölümünü kullanarak birincil veya ikincil anahtarını yeniden gerekir.