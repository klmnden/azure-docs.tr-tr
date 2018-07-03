---
title: include dosyası
description: include dosyası
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 06/28/2018
ms.author: raynew
ms.openlocfilehash: f7d6c3f68618fec839ccff06b73ba44d106999d2
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342831"
---
| Ad | Ticari URL'si | Kamu URL'si | Açıklama |
|---|---|---|---|
| Azure AD | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | AAD kullanılarak erişim denetimi ve kimlik yönetimi için kullanılır |
| Backup | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Çoğaltma veri aktarımı ve düzenlemesi için kullanılır |
| Çoğaltma | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Çoğaltma yönetimi işlemleri ve düzenleme için kullanılır |
| Depolama | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Çoğaltılan verileri depolayan depolama hesabına erişim için kullanılır |
| Telemetri (isteğe bağlı) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Telemetri için kullanılan |

``time.nist.gov`` ve ``time.windows.com`` sistem ile tüm dağıtımlarda genel saat arasındaki saat eşitlemesini denetlemek için kullanılır.


