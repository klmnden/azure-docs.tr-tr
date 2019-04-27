---
title: Sunucu günlükleri, Azure portalında MySQL için Azure veritabanına erişmek ve yapılandırma
description: Bu makalede, Azure portalından MySQL için Azure veritabanı'nda sunucu günlüklerini erişmek ve yapılandırma açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: e0701d2e10b366a6bf849512484fb216c42823bc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60525999"
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Yapılandırma ve Azure portalında erişim sunucusu günlükleri

Yapılandırma, liste indirin ve [sunucu günlükleri MySQL için Azure veritabanı](concepts-server-logs.md) Azure portalından.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- [MySQL sunucusu için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>Günlük tutmayı yapılandırma
MySQL yavaş sorgu günlüğü erişimi yapılandırın. 

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. MySQL sunucusu için Azure veritabanı'nı seçin.

3. Altında **izleme** kenar seçme bölümünde **sunucu günlükleri**. 
   ![Select sunucusu günlüklerini yapılandırmak için tıklayın](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Başlığı seçin **günlüklerini etkinleştirme ve günlük parametrelerini yapılandırmak için burayı tıklatın** sunucu parametreleri görmek için.

5. Ayarlamak için gereken parametreler değiştirin. Bu oturumda yaptığınız tüm değişiklikler mor renkle vurgulanır. 

   Parametreleri değiştirildi. bir kez tıklayabilirsiniz **Kaydet**. Veya **at** yaptığınız değişiklikleri.

   ![Kaydet'e tıklayın veya atın](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

6. Dönüş günlükleri listesine tıklayarak **Kapat düğmesi** (X simgesi) üzerinde **sunucu parametreleri** sayfası.

## <a name="view-list-and-download-logs"></a>Listesini görüntüleyebilir ve günlükleri indir
Günlük başladıktan sonra mevcut bir listesini görüntülemek ve sunucu günlüklerini bölmesinde ayrı günlük dosyalarına indirin. 

1. Azure portalı açın.

2. MySQL sunucusu için Azure veritabanı'nı seçin.

3. Altında **izleme** kenar seçme bölümünde **sunucu günlükleri**. Sayfa gösterildiği gibi günlük dosyalarının listesini gösterir:

   ![Günlükleri listesi](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > Adlandırma kuralı günlüğü **mysql - yavaş - < sunucu adı >-yyyymmddhh.log**. Dosya adında kullanılan saat ve tarihi olan günlük yayımlandığında zamandır. Günlük dosyaları Döndürülmüş her 24 saat veya 7.5 GB, hangisi gelir önce.

4. Gerekirse, kullanın **arama kutusuna** tarihi/saatini temel alan belirli bir günlük için hızlı bir şekilde daraltmak için. Arama günlüğü üzerinde adıdır.

5. Kullanarak tek bir günlük dosyalarını indirin **indirme** düğmesine (ok simgesi) yanında her tablo satırı günlük dosyasında gösterildiği gibi:

   ![İndirme simgesine tıklayın](./media/howto-configure-server-logs-in-portal/5-download.png)


## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [CLI sunucu günlüklerine erişme](howto-configure-server-logs-in-cli.md) günlüklerini programlı olarak indirme hakkında bilgi edinmek için.
- Daha fazla bilgi edinin [sunucu günlükleri](concepts-server-logs.md) MySQL için Azure veritabanı'nda. 
- MySQL günlük kaydı ve parametre tanımları hakkında daha fazla bilgi için MySQL belgeleri bakın [günlükleri](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

