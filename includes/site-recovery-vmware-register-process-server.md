---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: e18d0a6a01a86f844edc213fc95003cf4f4b46c9
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164580"
---
* Uzak Masaüstü Bağlantısı’nı kullanarak İşlem Sunucusu sanal makinesine bağlanın.
* Masaüstündeki kısayola tıklayarak cspsconfigtool.exe aracını başlatabilirsiniz. (Bu ilk kez, işlem sunucusu oturum açıyorsanız araç otomatik olarak başlatılır).
  - Yapılandırma Sunucusunun tam adı (FQDN) veya IP Adresi
  - Yapılandırma sunucusunun dinleme yaptığı bağlantı noktası. Değer 443 olmalıdır
  - Yapılandırma sunucusuna bağlanmak için bağlantı parolası.
  - Bu İşlem Sunucusu için yapılandırılacak Veri Aktarımı bağlantı noktası. Varsayılan değeri ortamınızdaki farklı bir bağlantı noktası numarasıyla değiştirmediyseniz olduğu gibi bırakın.

    ![İşlem Sunucusunu Kaydetme](./media/site-recovery-vmware-register-process-server/register-ps.png)
* Yapılandırmayı ve İşlem Sunucusunu kaydetmek için kaydet düğmesine tıklayın.
