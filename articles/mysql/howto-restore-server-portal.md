---
title: "MySQL için Azure veritabanı bir sunucuya geri yükleme | Microsoft Docs"
description: "Bu makalede, Azure portalını kullanarak MySQL için Azure veritabanındaki bir sunucuyu geri yüklemek açıklar."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 6c1c0f8a0c0e59661b70b787b551b8cfdb024cda
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Yedekleme ve Azure portalını kullanarak için MySQL server Azure veritabanında geri yükleme

## <a name="backup-happens-automatically"></a>Yedekleme otomatik olarak gerçekleşir
Azure veritabanı için MySQL kullanırken, veritabanı hizmeti her beş dakikada bir yedekleme hizmetinin otomatik olarak yapar. 

Temel katman ve 35 gün kullanırken yedeklemeler 7 gün için kullanılabilir olan standart katmanı kullanırken. Daha fazla bilgi için bkz: [Azure veritabanı için MySQL hizmet katmanları](concepts-service-tiers.md)

Bu otomatik yedekleme özelliğini kullanarak, sunucu ve tüm veritabanlarını içine bir önceki noktası zaman içinde yeni bir sunucuya geri yükleyebilirsiniz.

## <a name="restore-in-the-azure-portal"></a>Azure portalında geri yükleme
Azure veritabanı MySQL için saat ve içine sunucunun belirli bir noktaya geri olanak tanır sunucunun yeni bir kopyasını için. Bu yeni sunucu verilerinizi kurtarmak için kullanabilirsiniz. 

Örneğin, bir tablo yanlışlıkla bugün öğlen bırakıldı, birer öğlen hemen önce geri yükleme ve sunucu yeni kopyadan eksik tablo ve veri almak.

Aşağıdaki adımları örnek sunucunun saat içinde belirli bir noktaya geri:

1. Oturum [Azure portalı](https://portal.azure.com/)

2. MySQL sunucusu için Azure veritabanınızı bulun. Sol bölmede seçin **tüm kaynakları**ve ardından listeden sunucunuzu seçin.

3.  Sunucu genel bakış dikey üst kısmında tıklayın **geri** araç çubuğunda. Geri yükleme dikey pencere açılır.
![Geri düğmesini tıklatın](./media/howto-restore-server-portal/click-restore-button.png)

4. Gerekli bilgileri geri yükleme formu doldurun:

- **Geri yükleme noktası (UTC)**: Tarih Seçici ve Saat Seçici kullanarak seçin bir noktaya geri yüklemek için zaman. Belirtilen süre UTC biçiminde kitabıdır olasılıkla yerel saati UTC dönüştürmeniz gerekir.
- **Yeni bir sunucuya geri**: var olan sunucuya geri yüklemek için yeni bir sunucu adı sağlayın.
- **Konum**: Bölge Seçimi kaynak sunucu bölge ile otomatik olarak doldurur ve değiştirilemez.
- **Fiyatlandırma katmanı**: fiyatlandırma katmanı seçimi kaynak sunucuyla aynı fiyatlandırma katmanı otomatik olarak doldurur ve burada değiştirilemez. 
![PITR geri yükleme](./media/howto-restore-server-portal/pitr-restore.png)

5. Tıklatın **Tamam** sunucu zamanında belirtilen noktasına geri yüklemek için. 

6. Geri yükleme tamamlandıktan sonra oluşturulan ve veritabanlarını beklendiği gibi geri yüklendiğini doğrulayın yeni sunucu bulun.

## <a name="next-steps"></a>Sonraki adımlar
- [MySQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).