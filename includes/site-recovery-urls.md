---
title: include dosyası
description: include dosyası
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/12/2018
ms.author: raynew
ms.openlocfilehash: ae8eebf9667f2bc03fdc1082fb38c19c5c645c10
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060861"
---
**Ad** | **Ticari URL'si**  | **Kamu URL'si** | **Açıklama** |
--- | --- | --- | ---
Azure AD | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | AAD kullanılarak erişim denetimi ve kimlik yönetimi için kullanılır 
Backup | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Çoğaltma veri aktarımı ve düzenlemesi için kullanılır 
Çoğaltma | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Çoğaltma yönetimi işlemleri ve düzenleme için kullanılır 
Depolama | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Çoğaltılan verileri depolayan depolama hesabına erişim için kullanılır 
Telemetri (isteğe bağlı) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Telemetri için kullanılan 
Zaman eşitleme | ``time.windows.com`` | ``time.nist.gov`` | Sistem ile tüm dağıtımlarda genel saat arasındaki saat eşitlemesini denetlemek için Ssed.


