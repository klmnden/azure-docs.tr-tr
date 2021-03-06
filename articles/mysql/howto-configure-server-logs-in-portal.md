---
title: Yapılandırma ve yavaş sorgu günlüklerini için MySQL için Azure veritabanı, Azure portalında erişim.
description: Bu makalede, yapılandırmak ve yavaş günlüklerini Azure veritabanı'nda MySQL için Azure portalından erişmek açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: mysql
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: b16ac525d41eb2423828a647fdb75fd3f4a80a31
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052725"
---
# <a name="configure-and-access-slow-query-logs-in-the-azure-portal"></a>Yapılandırma ve Azure portalında erişim yavaş sorgu günlükleri

Yapılandırma, liste indirin ve [MySQL yavaş sorgu günlüklerini için Azure veritabanı](concepts-server-logs.md) Azure portalından.

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
Günlüğe kaydetme başladıktan sonra kullanılabilir yavaş sorgu günlüklerini listesini görüntüleyebilir ve sunucu günlüklerini bölmesinde ayrı günlük dosyalarına indirin.

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
- Bkz: [erişim yavaş sorgu günlüklerini CLI'daki](howto-configure-server-logs-in-cli.md) yavaş sorgu günlüklerini programlı olarak indirme hakkında bilgi edinmek için.
- Daha fazla bilgi edinin [yavaş sorgu günlüklerini](concepts-server-logs.md) MySQL için Azure veritabanı'nda.
- MySQL günlük kaydı ve parametre tanımları hakkında daha fazla bilgi için MySQL belgeleri bakın [günlükleri](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).