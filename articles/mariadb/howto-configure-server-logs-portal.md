---
title: Yapılandırma ve erişimi sunucu için MariaDB için Azure veritabanı, Azure portalında kaydeder.
description: Bu makalede, Azure veritabanı'nda sunucu günlüklerini MariaDB için Azure portalından erişmek ve yapılandırma açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 3dbf7064e409230916668e62ef861c0ce149fdbb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065631"
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Yapılandırma ve Azure portalında erişim sunucusu günlükleri

Yapılandırma, liste indirin ve [yavaş sorgu günlüklerini MariaDB için Azure veritabanı](concepts-server-logs.md) Azure portalından.

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
Günlüğe kaydetme başladıktan sonra kullanılabilir yavaş sorgu günlüklerini listesini görüntüleyebilir ve sunucu günlüklerini bölmesinde ayrı günlük dosyalarına indirin. 

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
- Daha fazla bilgi edinin [yavaş sorgu günlüklerini](concepts-server-logs.md) MariaDB için Azure veritabanı'nda.
- Parametre tanımlarıyla ve günlüğe kaydetme hakkında daha fazla bilgi için üzerinde MariaDB belgelerine bakın. [günlükleri](https://mariadb.com/kb/en/library/slow-query-log-overview/).

<!--- See [Access Server Logs in CLI](howto-configure-server-logs-in-cli.md) to learn how to download logs programmatically. -->
