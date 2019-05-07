---
title: Sunucu günlükleri, PostgreSQL - Azure Portalı'nda tek bir sunucu için Azure veritabanına erişmek ve yapılandırma
description: Bu makalede, PostgreSQL - Azure Portalı'ndan tek bir sunucu için Azure veritabanı'nda sunucu günlüklerini erişmek ve yapılandırma açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 13a8c1ee4b7ca114211f93245d74866e5aa5d52c
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067443"
---
# <a name="configure-and-access-azure-database-for-postgresql---single-server-logs-in-the-azure-portal"></a>PostgreSQL için Azure veritabanına erişmek ve yapılandırma - Azure portalında tek bir sunucu günlüklerini

Yapılandırma, liste indirin ve [günlükleri PostgreSQL için Azure veritabanı](concepts-server-logs.md) Azure portalından.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- [PostgreSQL sunucusu için Azure veritabanı](quickstart-create-server-database-portal.md)

## <a name="configure-logging"></a>Günlük tutmayı yapılandırma
Sorgu ve hata günlükleri erişimi yapılandırın. 

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

3. Altında **izleme** kenar seçme bölümünde **sunucu günlükleri**. 

   ![Sunucu günlükleri'ni seçin ve ' etkinleştirmek için... buraya' seçin](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Başlığı seçin **günlüklerini etkinleştirme ve günlük parametrelerini yapılandırmak için burayı tıklatın** sunucu parametreleri görmek için.

5. Ayarlamak için gereken parametreler değiştirin. Bu oturumda yaptığınız tüm değişiklikler mor renkle vurgulanır.

   Parametreleri değiştirildi. bir kez tıklayabilirsiniz **Kaydet**. Veya **at** yaptığınız değişiklikleri. 

   ![Değişiklikleri kaydetmeli veya atmalısınız parametrelerle uzun listesi](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

6. Dönüş günlükleri listesine tıklayarak **Kapat düğmesi** (X simgesi) üzerinde **sunucu parametreleri** sayfası.

## <a name="view-list-and-download-logs"></a>Listesini görüntüleyebilir ve günlükleri indir
Günlük başladıktan sonra mevcut bir listesini görüntülemek ve sunucu günlüklerini bölmesinde ayrı günlük dosyalarına indirin. 

1. Azure portalı açın.

2. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

3. Altında **izleme** kenar seçme bölümünde **sunucu günlükleri**. Sayfa gösterildiği gibi günlük dosyalarının listesini gösterir:

   ![Sunucu günlükleri listesi](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > Adlandırma kuralı günlüğü **postgresql-yyyy-aa-dd_hh0000.log**. Dosya adında kullanılan saat ve tarihi olan günlük yayımlandığında zamandır. Günlük dosyaları, her bir saat veya 100 MB'lık boyut döndürme, hangisinin önce geldiğine.

4. Gerekirse, kullanın **arama kutusuna** tarihi/saatini temel alan belirli bir günlük için hızlı bir şekilde daraltmak için. Arama günlüğü üzerinde adıdır.

   ![Örnek arama günlük adları](./media/howto-configure-server-logs-in-portal/5-search.png)

5. Kullanarak tek bir günlük dosyalarını indirin **indirme** düğmesine (ok simgesi) yanında her tablo satırı günlük dosyasında gösterildiği gibi:

   ![İndirme simgesine tıklayın](./media/howto-configure-server-logs-in-portal/6-download.png)

## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [CLI sunucu günlüklerine erişme](howto-configure-server-logs-using-cli.md) günlüklerini programlı olarak indirme hakkında bilgi edinmek için.
- Daha fazla bilgi edinin [sunucu günlükleri](concepts-server-logs.md) PostgreSQL için Azure DB'de. 
- PostgreSQL günlüğe kaydetme ve parametre tanımları hakkında daha fazla bilgi için PostgreSQL belgeleri bakın [hata bildirimi ve günlüğe kaydetme](https://www.postgresql.org/docs/current/static/runtime-config-logging.html).

