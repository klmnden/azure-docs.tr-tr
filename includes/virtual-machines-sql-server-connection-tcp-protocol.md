---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: 8b919608dfc562d8db77619d5215a6828a53a4aa
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188313"
---
1. Uzak Masaüstü ile sanal makineye bağlıyken **Yapılandırma Yöneticisi**‘ni arayın:

    ![SSCM’yi Açma](./media/virtual-machines-sql-server-connection-tcp-protocol/sql-server-configuration-manager.png)

1. SQL Server Yapılandırma Yöneticisi’ndeki konsol bölmesinde **SQL Server Ağ Yapılandırması**’nı genişletin.

1. Konsol bölmesinde **MSSQLSERVER Protokolleri**’ne tıklayın (varsayılan örnek adı). Ayrıntılar bölmesinde **TCP**’ye sağ tıklayın ve henüz etkinleştirilmemişse **Etkinleştir**’e tıklayın.

    ![TCP’yi Etkinleştirme](./media/virtual-machines-sql-server-connection-tcp-protocol/enable-tcp.png)

1. Konsol bölmesinde **SQL Server Hizmetleri**’ne tıklayın. Ayrıntılar bölmesinde, **SQL Server (*örnek adı*)** (varsayılan örnek **SQL Server (MSSQLSERVER)** ) ve ardından **yenidenbaşlatın**, durdurmak ve SQL Server örneğini yeniden başlatın.

    ![Veritabanı Altyapısını Yeniden Başlatma](./media/virtual-machines-sql-server-connection-tcp-protocol/restart-sql-server.png)

1. SQL Server Yapılandırma Yöneticisi’ni kapatın.

SQL Server Veritabanı Altyapısı’nda protokolleri etkinleştirme hakkında daha fazla bilgi için bkz. [Sunucu Ağ Protokolünü Etkinleştirme veya Devre Dışı Bırakma](https://msdn.microsoft.com/library/ms191294.aspx).
