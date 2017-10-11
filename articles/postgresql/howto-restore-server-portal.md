---
title: "Azure veritabanı Server'da PostgreSQL için geri yükleme | Microsoft Docs"
description: "Bu makalede, Azure veritabanındaki bir sunucu için Azure portalını kullanarak PostgreSQL geri açıklar."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 07/20/2017
ms.openlocfilehash: 3fbdb7741481bd3620466c3489d3609f9ea6961f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-portal"></a>Yedekleme ve Azure veritabanındaki bir sunucu için Azure portalını kullanarak PostgreSQL geri yükleme

## <a name="backup-happens-automatically"></a>Yedekleme otomatik olarak gerçekleşir
Azure veritabanı için PostgreSQL kullanırken, veritabanı hizmeti yedekleme hizmetinin her 5 dakikada bir otomatik olarak yapar. 

Temel katman ve 35 gün kullanırken yedeklemeler 7 gün için kullanılabilir olan standart katmanı kullanırken. Daha fazla bilgi için bkz: [PostgreSQL hizmet katmanları için Azure veritabanı](concepts-service-tiers.md)

Bu otomatik yedekleme özelliğini kullanarak, sunucu ve tüm veritabanlarını içine bir önceki noktası zaman içinde yeni bir sunucuya geri yükleyebilirsiniz.

## <a name="restore-in-the-azure-portal"></a>Azure portalında geri yükleme
Azure veritabanı PostgreSQL için sağlar, sunucu saati ve içine bir noktaya geri geri yüklemek sunucu yeni bir kopyasını için. Bu yeni sunucu verilerinizi kurtarmak için kullanabilirsiniz. 

Örneğin, bir tablo yanlışlıkla varsa bugün öğlen bırakılan, zaman öğlen hemen önce geri yükleme ve bu sunucunun yeni kopyadan eksik tablo ve veri almak.

Aşağıdaki adımları örnek sunucunun saat içinde bir noktaya geri:
1. Oturum [Azure portalı](https://portal.azure.com/)
2. Azure veritabanınız PostgreSQL sunucu için bulun. Azure portalında tıklatın **tüm kaynakları** adı yazın ve sol taraftaki menüden gibi **mypgserver 20170401**, varolan sunucunuz için aranacak. Arama sonucunda listelenen sunucu adına tıklayın. Sunucunuzun **Genel bakış** sayfası açılır ve daha fazla yapılandırma seçenekleri sunulur.

   ![Azure portal - sunucunuz bulmak için arama](media/postgresql-howto-restore-server-portal/1-locate.png)

3. Sunucu genel bakış dikey üst kısmında tıklayın **geri** araç çubuğunda. Geri yükleme dikey pencere açılır.

   ![Azure veritabanı için PostgreSQL - genel bakış - Geri Yükle düğmesi](./media/postgresql-howto-restore-server-portal/2_server.png)

4. Gerekli bilgileri geri yükleme formu doldurun:

   ![Azure veritabanı PostgreSQL - geri yüklemesi bilgi için ](./media/postgresql-howto-restore-server-portal/3_restore.png)
  - **Geri yükleme noktası**: bir nokta sunucu değiştirilmeden önce oluşan zaman seçin
  - **Hedef sunucu**: geri yüklemek istediğiniz yeni bir sunucu adı sağlayın
  - **Konum**: bölge seçemezsiniz, varsayılan olarak kaynak sunucuyla aynı.
  - **Fiyatlandırma katmanı**: bir sunucu geri yüklerken bu değer değiştirilemez. Kaynak sunucu ile aynı. 

5. Tıklatın **Tamam** zaman içinde bir noktaya geri yüklemenizi sunucuyu geri yüklemek için. 

6. Geri yükleme tamamlandıktan sonra verilerin beklenen şekilde geri doğrulamak için oluşturulan yeni sunucu bulun.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure veritabanı PostgreSQL için için bağlantı kitaplıkları](concepts-connection-libraries.md)
