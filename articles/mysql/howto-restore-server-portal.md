---
title: "MySQL için Azure veritabanı bir sunucuya geri yükleme"
description: "Bu makalede, Azure portalını kullanarak MySQL için Azure veritabanındaki bir sunucuyu geri yüklemek açıklar."
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 5dac5343551bed709486d97d85ae0ff3a49d1851
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-portal"></a>Yedekleme ve MySQL için Azure portalını kullanarak Azure veritabanı bir sunucuya geri yükleme

## <a name="backup-happens-automatically"></a>Yedekleme otomatik olarak gerçekleşir
Azure veritabanı için MySQL sunucuları düzenli aralıklarla geri yükleme özelliklerini etkinleştirmek için yedeklenir. Bu özelliği kullanarak, sunucu ve tüm veritabanları için bir önceki noktası zaman içinde yeni bir sunucu üzerinde geri yükleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu tamamlamak için gerekir:
- Bir [MySQL server ve veritabanı için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-backup-configuration"></a>Yedekleme kümesi yapılandırması

Yerel olarak yedekli yedekleri veya sunucu oluşturulurken, coğrafi olarak yedekli yedekleme için sunucunuzu yapılandırma arasında seçim yapmanıza **fiyatlandırma katmanı** penceresi.

> [!NOTE]
> Bir sunucu oluşturulduğunda, sahip, artıklık tür coğrafi olarak yedekli vs yerel olarak yedekli olamaz değiştirdi.
>

Azure portal aracılığıyla bir sunucu oluşturulurken **fiyatlandırma katmanı** penceresi seçildiği yer ya da **yerel olarak yedekli** veya **coğrafi olarak yedekli** yedeklemeler için sunucunuz. Bu da burada seçtiğiniz penceredir **yedekleme Bekletme dönemi** - ne kadar süreyle saklanan sunucu yedeklemelerini istediğiniz (gün sayısı içinde).

   ![Fiyatlandırma katmanı - yedekleme artıklık seçin](./media/howto-restore-server-portal/pricing-tier.png)

Oluşturma sırasında bu değerleri ayarlama hakkında daha fazla bilgi için bkz: [Azure veritabanı için MySQL server Hızlı Başlangıç](quickstart-create-mysql-server-database-using-azure-portal.md).

Yedekleme saklama dönemi, bir sunucuda aşağıdaki adımları değiştirilebilir:
1. [Azure portal](https://portal.azure.com/) oturum açın.
2. MySQL sunucusu için Azure veritabanınızı seçin. Bu eylem açılır **genel bakış** sayfası.
3. Seçin **fiyatlandırma katmanı** menüsünde altında **ayarları**. Değiştirebileceğiniz kaydırıcıyı kullanarak **yedekleme Bekletme dönemi** tercihinize 7-35 gün arasında.
Aşağıdaki ekran görüntüsünde 34 gün çıkarılmıştır.
![Yedekleme saklama dönemi artar](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. Tıklatın **Tamam** Değişikliği onaylamak için.

Ne kadar geri yedeklemelerin kullanılabilir dayalı olduğundan zaman içinde nokta geri alınabilir zaman Yedekleme bekletme süresini yönetir. Zaman içinde nokta geri yükleme aşağıdaki bölümde daha ayrıntılı açıklanmıştır. 

## <a name="point-in-time-restore-in-the-azure-portal"></a>Azure portalında zaman içinde nokta geri yükleme
Azure veritabanı için MySQL sağlar, sunucunun bir nokta zaman dönün ve içine geri yüklemek sunucu yeni bir kopyasını için. Verilerinizi kurtarmak için bu yeni sunucuyu kullanın ya da bu yeni sunucuya işaret, istemci uygulamaları vardır.

Örneğin, bir tablo yanlışlıkla varsa bugün öğlen bırakılan, zaman öğlen hemen önce geri yükleme ve bu sunucunun yeni kopyadan eksik tablo ve veri almak. Zaman içinde nokta geri yükleme sunucuda veritabanı düzeyinde düzeyi, değil.

Aşağıdaki adımları bir nokta zaman için örnek sunucunun geri yükleyin:
1. Azure portalında Azure veritabanınızı MySQL sunucusu için seçin. 

2. Sunucunun araç çubuğundaki **genel bakış** sayfasında, **geri**.

   ![Azure veritabanı için MySQL - genel bakış - Geri Yükle düğmesi](./media/howto-restore-server-portal/2-server.png)

3. Gerekli bilgileri geri yükleme formu doldurun:

   ![Azure veritabanı için MySQL - geri yükleme bilgileri ](./media/howto-restore-server-portal/3-restore.png)
  - **Geri yükleme noktası**: noktası geri yüklemek istediğiniz zaman seçin.
  - **Hedef sunucu**: yeni sunucu için bir ad sağlayın.
  - **Konum**: bölge seçemezsiniz. Varsayılan olarak kaynak sunucuyla aynı değildir.
  - **Fiyatlandırma katmanı**: bir zaman içinde nokta geri yükleme yaparken bu parametreler değiştiremezsiniz. Kaynak sunucuyla aynıdır. 

4. Tıklatın **Tamam** bir nokta zaman için geri yüklemek için sunucuyu geri yüklemek için. 

5. Geri yükleme tamamlandıktan sonra verilerin beklenen şekilde geri doğrulamak için oluşturulan yeni sunucu bulun.

>[!Note]
>Zaman içinde nokta geri yüklemesi tarafından oluşturulan yeni sunucusunda aynı sunucu yönetici oturum açma adı ve zaman içinde nokta var olan sunucu için geçerli parola seçtiğiniz unutmayın. Yeni sunucudan kullanıcının parolasını değiştirebilirsiniz **genel bakış** sayfası.

## <a name="geo-restore"></a>Coğrafi geri yükleme
Coğrafi olarak yedekli yedekleme için sunucunuzu yapılandırdıysanız, var olan sunucu yedekten yeni bir sunucu oluşturulabilir. Azure veritabanı için MySQL kullanılabilir herhangi bir bölgede bu yeni sunucu oluşturulabilir.  

1. Portalın sol üst köşesinde bulunan **Yeni** düğmesini (+) seçin. **Veritabanları** > **MySQL için Azure Veritabanı**’nı seçin.

   !["MySQL için Azure veritabanı" seçeneği](./media/howto-restore-server-portal/1-create-database.png)

2. Formun içinde **Kaynağı Seç** açılan listesinde, seçin **yedekleme**. Bu eylem, coğrafi olarak yedekli yedeklemeleri etkin olduğu sunucular listesini yükler. Yeni sunucunuzun bir kaynak olarak bu yedeklemeler birini seçin.
   ![Kaynak Seç: Yedekleme ve coğrafi olarak yedekli yedeklemeleri listesi](./media/howto-restore-server-portal/2-georestore.png)

3. Tercihlerinizi formla geri kalanını doldurun. Seçebilirsiniz **konumu**. Seçebileceğiniz konumu seçtikten sonra **fiyatlandırma katmanı**. Varsayılan olarak, geri yüklemekte olduğunuz var olan sunucu için parametreleri gösterilir. Tıklayabilirsiniz **Tamam** bu ayarları devralmak için herhangi bir değişiklik yapmadan olmadan. Veya değiştirebileceğiniz **işlem oluşturma** (bölgede kullanılabilir seçmiş olduğunuz varsa), sayısı **vCores**, **yedekleme Bekletme dönemi**, ve **yedekleme Artıklığı seçeneği**. Değiştirme **fiyatlandırma katmanı** (temel, genel amaçlı veya bellek için iyileştirilmiş) veya **depolama** geri yükleme sırasında boyutu desteklenmiyor.

>[!Note]
>Coğrafi geri yükleme ile oluşturulan yeni sunucu aynı sunucu yönetici oturum açma adı ve geri yükleme başlatıldı aynı anda var olan sunucu için geçerli parola sahiptir. Parola yeni sunucu değiştirilebilir **genel bakış** sayfası.


## <a name="next-steps"></a>Sonraki adımlar
- Hizmetin hakkında daha fazla bilgi [yedeklemeleri](concepts-backup.md).
- Daha fazla bilgi edinmek [iş sürekliliği](concepts-business-continuity.md) seçenekleri.
