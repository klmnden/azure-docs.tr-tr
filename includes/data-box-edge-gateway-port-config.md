---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 98f9e0377e560fa0bba2fd470ff01431b2ed21d9
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58431504"
---
| Bağlantı noktası yok.| Daraltma veya genişletme | Bağlantı noktası kapsamı| Gerekli|   Notlar |   |
|--------|-----|-----|-----------|----------|-----------|
| TCP 80 (HTTP)|Çıkış|WAN |Hayır|Giden bağlantı noktası, güncelleştirmeleri almak için internet erişimi için kullanılır. <br>Kullanıcı tarafından yapılandırılabilir bir giden web Ara sunucudur. |
| TCP 443 (HTTPS)|Çıkış|WAN|Evet|Giden bağlantı noktası, bulut veri erişimi için kullanılır.<br>Kullanıcı tarafından yapılandırılabilir bir giden web Ara sunucudur.|
| UDP 123 (NTP)|Çıkış|WAN|Bazı durumlarda<br>Notlara bakın|Yalnızca Internet tabanlı bir NTP sunucusu kullanıyorsanız, bu bağlantı noktası gereklidir.  |   
| UDP 53 (DNS)|Çıkış|WAN|Bazı durumlarda<br>Notlara bakın|Yalnızca Internet tabanlı bir DNS sunucusu kullanıyorsanız, bu bağlantı noktası gereklidir.<br>Yerel bir DNS sunucusunu kullanmanızı öneririz. |
| TCP 5985 (WinRM)|Giden/içinde|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası, HTTP üzerinden cihaz uzak PowerShell aracılığıyla bağlanmak için gereklidir.  |
| UDP 67 (DHCP)|Çıkış|LAN|Bazı durumlarda<br>Notlara bakın|Yalnızca yerel bir DHCP sunucusu kullanıyorsanız bu bağlantı noktası gereklidir.  |
| TCP 80 (HTTP)|Giden/içinde|LAN|Evet|Bu bağlantı noktasını, yerel yönetim için cihazda yerel kullanıcı Arabirimi için gelen bağlantı noktasıdır. <br>HTTP üzerinden yerel UI erişme, HTTPS için otomatik olarak yönlendirir.  |
| TCP 443 (HTTPS)|Giden/içinde|LAN|Evet|Bu bağlantı noktasını, yerel yönetim için cihazda yerel kullanıcı Arabirimi için gelen bağlantı noktasıdır. |
| TCP 445 (SMB)|İçinde|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası, yalnızca SMB bağlanılıyorsa gereklidir. |
| TCP 2049 (NFS)|İçinde|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası, yalnızca NFS bağlanılıyorsa gereklidir. |
