---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 11/06/2018
ms.author: alkohli
ms.openlocfilehash: 67de9042af11a2b17c4b65f8225ecd0580b95466
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263606"
---
| Bağlantı noktası yok.| Daraltma veya genişletme | Bağlantı noktası kapsamı| Gerekli|   Notlar |   |
|--------|-----|-----|-----------|----------|-----------|
| TCP 80 (HTTP)|Çıkan|WAN |Hayır|Giden bağlantı noktası, güncelleştirmeleri almak için internet erişimi için kullanılır. <br>Kullanıcı tarafından yapılandırılabilir bir giden web Ara sunucudur. |
| TCP 443 (HTTPS)|Çıkan|WAN|Evet|Giden bağlantı noktası, bulut veri erişimi için kullanılır.<br>Kullanıcı tarafından yapılandırılabilir bir giden web Ara sunucudur.|   
| UDP 53 (DNS)|Çıkan|WAN|Bazı durumlarda<br>Notlara bakın|Yalnızca Internet tabanlı bir DNS sunucusu kullanıyorsanız, bu bağlantı noktası gereklidir.<br>Yerel bir DNS sunucusunu kullanmanızı öneririz. |
| UDP 123 (NTP)|Çıkan|WAN|Bazı durumlarda<br>Notlara bakın|Yalnızca Internet tabanlı bir NTP sunucusu kullanıyorsanız, bu bağlantı noktası gereklidir.  |
| UDP 67 (DHCP)|Çıkan|WAN|Bazı durumlarda<br>Notlara bakın|Yalnızca bir DHCP sunucusu kullanıyorsanız bu bağlantı noktası gereklidir.  |
| TCP 80 (HTTP)|İçinde|LAN|Evet|Bu bağlantı noktasını, yerel yönetim için cihazda yerel kullanıcı Arabirimi için gelen bağlantı noktasıdır. <br>HTTP üzerinden yerel UI erişme, HTTPS için otomatik olarak yönlendirir.  |
| TCP 443 (HTTPS)|İçinde|LAN|Evet|Bu bağlantı noktasını, yerel yönetim için cihazda yerel kullanıcı Arabirimi için gelen bağlantı noktasıdır. |