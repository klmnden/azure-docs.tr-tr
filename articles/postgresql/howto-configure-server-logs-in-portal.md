---
title: Yapılandırma ve Azure Portalı'nda PostgreSQL için sunucu günlüklerine erişim
description: Bu makalede, yapılandırmak ve sunucu günlüklerini Azure veritabanındaki PostgreSQL Azure portalından erişmek açıklar.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: aa9823c65b342f922ca78a51ecd3055dfac62869
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29692173"
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

5. Ayarlamanız gereken parametrelerini değiştirin. Bu oturumda yaptığınız tüm değişiklikleri mor ile vurgulanır.

   Parametreleri değişti sonra tıklayabilirsiniz **kaydetmek**. Veya **atmak** değişikliklerinizi. 

   ![Değişiklikleri kaydedin veya atın parametrelerle uzun listesi](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

6. Tıklayarak günlükleri listesine dönmek **Kapat düğmesini** (X simgesi) üzerinde **sunucu parametreleri** sayfası.

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

