---
title: "Yapılandırma ve Azure Portalı'nda PostgreSQL için sunucu günlüklerine erişim | Microsoft Docs"
description: "Bu makalede, yapılandırmak ve sunucu günlüklerini Azure veritabanındaki PostgreSQL Azure portalından erişmek açıklar."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/19/2017
ms.openlocfilehash: a2f67b21293a1a0456b27cad9043be01fdd5274a
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Yapılandırma ve erişim sunucusu Azure portalında oturum

Yapılandırma, liste indirin ve [Azure veritabanı PostgreSQL sunucu günlükleri için](concepts-server-logs.md) Azure portalından.

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu adım için gerekir:
- [Azure veritabanı PostgreSQL sunucu için](quickstart-create-server-database-portal.md)

## <a name="configure-logging"></a>Günlük tutmayı yapılandırma
Sorgu ve hata günlükleri erişimi yapılandırın. 

1. [Azure Portal](http://portal.azure.com/)’da oturum açın.

2. Azure veritabanınızı PostgreSQL sunucusu seçin.

3. Altında **izleme** kenar, select bölümünde **sunucu günlükleri**. 

   ![Sunucu günlükleri ve ' burada etkinleştirmek için tıklatın...' seçin](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Başlığı seçin **günlüklerini etkinleştirmek ve günlük parametreleri yapılandırmak için burayı tıklatın** sunucu parametreleri görmek için.

5. Seçin **daha fazla Göster** genişletilmiş bir kullanılabilir parametrelerin listesini görmek için genişletici. 

   Parametreleri tanımları hakkında daha fazla bilgi için üzerinde PostgreSQL belgelerine bakın. [hata bildirimi ve günlüğü](https://www.postgresql.org/docs/current/static/runtime-config-logging.html).

   ![Günlük parametrelerin listesi kısa. Daha uzun süre için Göster'i tıklatın](./media/howto-configure-server-logs-in-portal/2-show-more.png)

6. Ayarlamanız gereken parametrelerini değiştirin. Bu oturumda yaptığınız tüm değişiklikleri mor ile vurgulanır.

   Parametreleri değişti sonra tıklayabilirsiniz **kaydetmek**. Veya **atmak** değişikliklerinizi. 

   ![Değişiklikleri kaydedin veya atın parametrelerle uzun listesi](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

7. Tıklayarak günlükleri listesine dönmek **Kapat düğmesini** (X simgesi) üzerinde **sunucu parametreleri** sayfası.

## <a name="view-list-and-download-logs"></a>Listesini görüntülemek ve günlükleri indirmek
Günlüğe kaydetme başladıktan sonra kullanılabilir günlüklerini listesini görüntülemek ve sunucu günlüklerini bölmesinde ayrı günlük dosyalarını yükleyin. 

1. Azure portalı açın.

2. Azure veritabanınızı PostgreSQL sunucusu seçin.

3. Altında **izleme** kenar, select bölümünde **sunucu günlükleri**. Sayfa gösterildiği gibi günlük dosyalarının listesini gösterir:

   ![Sunucu günlükleri listesi](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > Günlük adlandırma kuralı **postgresql-yyyy-aa-dd_hh0000.log**. Tarih ve saat kullanılan dosya adı, günlük yayımlandığında saattir. Günlük dosyaları her bir saat ya da 100 MB boyutuna döndür, hangisi önce gelirse.

4. Gerekirse, kullanın **arama kutusu** tarih/saat üzerine göre belirli bir günlük için hızlı bir şekilde daraltın. Arama günlüğü üzerinde adıdır.

   ![Örnek arama günlüğü adları](./media/howto-configure-server-logs-in-portal/5-search.png)

5. Kullanarak tek tek günlük dosyaları karşıdan **karşıdan** düğmesini (ok simgesi) her tablo satırı günlük dosyasında yanındaki gösterildiği gibi:

   ![İndirme simgesine tıklayın](./media/howto-configure-server-logs-in-portal/6-download.png)

## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [CLI erişim Server günlükleri](howto-configure-server-logs-using-cli.md) günlükleri programlı olarak indirmek hakkında bilgi edinmek için.
- Daha fazla bilgi edinmek [sunucu günlükleri](concepts-server-logs.md) PostgreSQL için Azure DB'de. 
- Parametre tanımları ve PostgreSQL günlüğe kaydetme hakkında daha fazla bilgi için PostgreSQL belgelerine bakın [hata bildirimi ve günlüğe kaydetme](https://www.postgresql.org/docs/current/static/runtime-config-logging.html).

