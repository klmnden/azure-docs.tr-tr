---
title: Yapılandırma ve Azure Portalı'nda MariaDB için için Azure veritabanı sunucu günlüklerine erişme
description: Bu makalede Azure Portalı'ndan MariaDB için Azure veritabanı'nda sunucu günlüklerini erişmek ve yapılandırma.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 4ff2fbd5976a8e203bbc43a87b31ddb1bed63402
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61040694"
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Yapılandırma ve Azure portalında erişim sunucusu günlükleri

Yapılandırma, liste indirin ve [MariaDB sunucu günlükleri için Azure veritabanı](concepts-server-logs.md) Azure portalından.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- [MariaDB için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>Günlük tutmayı yapılandırma
Yavaş sorgu günlüğü erişimi yapılandırın. 

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. MariaDB için Azure veritabanı sunucunuza seçin.

3. Altında **izleme** kenar seçme bölümünde **sunucu günlükleri**. 
   ![Select sunucusu günlüklerini yapılandırmak için tıklayın](./media/howto-configure-server-logs-portal/1-select-server-logs-configure.png)

4. Başlığı seçin **günlüklerini etkinleştirme ve günlük parametrelerini yapılandırmak için burayı tıklatın** sunucu parametreleri görmek için.

5. "On" "slow_query_log" açma dahil olmak üzere ayarlamak için gereken parametreler değiştirin. Bu oturumda yaptığınız tüm değişiklikler mor renkle vurgulanır. 

   Parametreleri değiştirildi. bir kez tıklayabilirsiniz **Kaydet**. Veya **at** yaptığınız değişiklikleri.

   ![Kaydet'e tıklayın veya atın](./media/howto-configure-server-logs-portal/3-save-discard.png)

6. Dönüş günlükleri listesine tıklayarak **Kapat düğmesi** (X simgesi) üzerinde **sunucu parametreleri** sayfası.

## <a name="view-list-and-download-logs"></a>Listesini görüntüleyebilir ve günlükleri indir
Günlük başladıktan sonra mevcut bir listesini görüntülemek ve sunucu günlüklerini bölmesinde ayrı günlük dosyalarına indirin. 

1. Azure portalı açın.

2. MariaDB için Azure veritabanı sunucunuza seçin.

3. Altında **izleme** kenar seçme bölümünde **sunucu günlükleri**. Sayfa gösterildiği gibi günlük dosyalarının listesini gösterir:

   ![Günlükleri listesi](./media/howto-configure-server-logs-portal/4-server-logs-list.png)

   > [!TIP]
   > Adlandırma kuralı günlüğü **mysql - yavaş - < sunucu adı >-yyyymmddhh.log**. Dosya adında kullanılan saat ve tarihi olan günlük yayımlandığında zamandır. Günlük dosyaları Döndürülmüş her 24 saat veya 7.5 GB, hangisi gelir önce.

4. Gerekirse, kullanın **arama kutusuna** tarihi/saatini temel alan belirli bir günlük için hızlı bir şekilde daraltmak için. Arama günlüğü üzerinde adıdır.

5. Kullanarak tek bir günlük dosyalarını indirin **indirme** düğmesine (ok simgesi) yanında her tablo satırı günlük dosyasında gösterildiği gibi:

   ![İndirme simgesine tıklayın](./media/howto-configure-server-logs-portal/5-download.png)

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [sunucu günlükleri](concepts-server-logs.md) MariaDB için Azure veritabanı'nda.
- Parametre tanımlarıyla ve günlüğe kaydetme hakkında daha fazla bilgi için üzerinde MariaDB belgelerine bakın. [günlükleri](https://mariadb.com/kb/en/library/slow-query-log-overview/).

<!-- - See [Access Server Logs in CLI](howto-configure-server-logs-in-cli.md) to learn how to download logs programmatically. -->
