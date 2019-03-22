---
title: Konak havuzu Yük Dengeleme yöntemleriyle (Önizleme) - Azure
description: Konak havuzu Yük Dengeleme yöntemlerinin bir Windows sanal masaüstü ortamında için.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 0a07406c8bad4ad0bef2949103d1f2c78e7dd8af
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58318517"
---
# <a name="host-pool-load-balancing-methods"></a>Konak havuzu Yük Dengeleme yöntemi

Windows sanal masaüstü (Önizleme), Yük Dengeleme iki yöntemi destekler. Her bir yöntemin hangi oturumu ana bilgisayarı bir konak havuzdaki bir kaynağa bağlanırken bir kullanıcının oturumunu barındıracak belirler.

Windows sanal masaüstü aşağıdaki Yük Dengeleme yöntemleriyle kullanılabilir:

- Avantajlarına ilk Yük Dengeleme, düzgün bir konak havuzdaki oturumu konaklar arasında kullanıcı oturumlarını Dağıt olanak tanır.
- Derinlik ilk Yük Dengeleme, oturumu ana bilgisayarı bir konak havuzda kullanıcı oturumlarıyla saturate olanak tanır. İlk oturum oturum sınırı eşiğini ulaştığında, yük dengeleyici alt sınırı ve benzeri ulaşana kadar sonraki oturumu ana bilgisayarı ana makine havuzundaki yeni kullanıcı bağlantılarına yönlendirir.

Her konak havuzu, yalnızca bir tür Yük Dengeleme yapılandırabilirsiniz, özel. Ancak, her iki Yük Dengeleme yöntemleri olsun, aşağıdaki davranışları havuzu konak paylaşım bulundukları:

- Bir kullanıcı zaten bir oturumu konak havuzunda varsa ve bu oturumuna bağlanıyor, yük dengeleyicinin başarıyla bunları oturumu ana bilgisayarı, var olan oturumu ile yönlendirilecek. Bu oturumu konağın AllowNewConnections özelliği False olarak ayarlanmış olsa bile bu davranış uygulanır.
- Bir kullanıcının ana havuzuna zaten bir oturumu yok, yük dengeleyici oturumu konaklar Yük Dengeleme sırasında AllowNewConnections özelliği False olarak ayarlanır düşünün olmaz.

## <a name="breadth-first-load-balancing-method"></a>Avantajlarına ilk Yük Dengeleme yöntemi

Yük Dengeleme avantajlarına ilk yöntemi, bu senaryo için en iyi duruma getirmek için kullanıcı bağlantılarını dağıtmak sağlar. Bu yöntem, havuza alınmış sanal masaüstü ortama bağlanan kullanıcılar için en iyi deneyimi sağlamak isteyen kuruluşlar için idealdir.

Avantajlarına ilk yöntem, ilk yeni bağlantılara izin veren oturum ana sorgular. Yöntemi ardından en az oturumu ana bilgisayarı seçer oturum sayısı. Bağ ise, yöntem sorguda ilk oturum ana bilgisayarı seçer.

## <a name="depth-first-load-balancing-method"></a>Derinlik ilk Yük Dengeleme yöntemi

Derinlik ilk Yük Dengeleme yöntemi, bu senaryo için en iyi duruma getirmek için bir zaman bir oturumu ana bilgisayarı saturate sağlar. Bu yöntem, bir ana makine havuzu için ayrılan sanal makine sayısı üzerinde daha ayrıntılı denetim istiyorsanız maliyete kuruluşlar için idealdir.

Derinlik ilk yöntem, ilk yeni bağlantılara izin vermek ve maksimum oturum sınırlarını aştığını gitti henüz oturum ana sorgular. Yöntemi daha sonra en yüksek oturum sayısı oturum ana bilgisayarı seçer. Bağ ise, yöntem sorguda ilk oturum ana bilgisayarı seçer.