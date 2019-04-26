---
title: PostgreSQL için Azure veritabanı'nda bir sunucuya geri yükleme
description: Bu makalede Azure portalını kullanarak PostgreSQL için Azure veritabanı'nda bir sunucuya geri yükleme.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 04/01/2018
ms.openlocfilehash: 60489e3fc7fc6482a14a241e777066b3805a4d5e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60420554"
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-portal"></a>Nasıl yedeklenir ve Azure portalını kullanarak PostgreSQL için Azure veritabanı'nda bir sunucuya geri yükleme

## <a name="backup-happens-automatically"></a>Yedekleme otomatik olarak gerçekleşir
Sunucuları düzenli aralıklarla geri yükleme özellikleri etkinleştirmek için yedeklenen PostgreSQL için Azure veritabanı. Bu özelliği kullanarak, sunucuyu ve tüm veritabanlarında için bir önceki-belirli bir noktaya, yeni bir sunucuya geri yükleyebilirsiniz.

## <a name="set-backup-configuration"></a>Yedekleme kümesi yapılandırması

Yerel olarak yedekli yedeklemeler veya sunucu oluşturma, saat coğrafi olarak yedekli yedekleme için sunucunuzu yapılandırma arasındaki tercihi yapabilmek **fiyatlandırma katmanı** penceresi.

> [!NOTE]
> Sahip, yedeklilik türünü bir sunucu oluşturulduktan sonra yerel olarak yedekli, coğrafi olarak yedekli vs moduna geçiş yapılamaz.
>

Azure Portalı aracılığıyla bir sunucu oluşturulurken **fiyatlandırma katmanı** penceredir seçeneğini burada **yerel olarak yedekli** veya **coğrafi olarak yedekli** yedeklemeler için sunucunuz yok. Seçtiğiniz Ayrıca bu pencereyi olduğu **yedekleme Bekletme dönemi** - ne kadar süre (gün sayısı içinde) için depolanan sunucu yedeklemelerini istiyor.

   ![Fiyatlandırma katmanı - fazladan yedek seçin](./media/howto-restore-server-portal/pricing-tier.png)

Oluşturma sırasında bu değerleri ayarlama hakkında daha fazla bilgi için bkz. [PostgreSQL server Hızlı Başlangıç için Azure veritabanı](quickstart-create-server-database-portal.md).

Bir sunucunun yedekleme saklama dönemi, aşağıdaki adımları izleyerek değiştirilebilir:
1. [Azure portal](https://portal.azure.com/) oturum açın.
2. PostgreSQL için Azure Veritabanı sunucunuzu seçin. Bu eylem açar **genel bakış** sayfası.
3. Seçin **fiyatlandırma katmanı** menüsünden altında **ayarları**. Değiştirebileceğiniz kaydırıcıyı kullanarak **yedekleme Bekletme dönemi** tercihinize 7 ila 35 gün arasında.
Aşağıdaki ekran görüntüsünde 34 gün olarak artırıldı.
![Yedekleme bekletme süresi artar](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. Tıklayın **Tamam** Değişikliği onaylamak için.

Yedekleme bekletme süresi ne kadar geri mevcut yedekleme dayalı olduğundan zaman içinde nokta geri alınabilir zaman yönetir. Belirli bir noktaya geri yükleme aşağıdaki bölümde daha ayrıntılı açıklanmıştır. 

## <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme
PostgreSQL için Azure veritabanı bir-belirli bir noktaya geri dönün ve halinde sunucuya geri yüklemenize olanak sağlayan yeni bir kopyasını sunucu için. Verilerinizi kurtarmak için bu yeni sunucu kullanın veya bu yeni sunucuya işaret edecek, istemci uygulamaları vardır.

Örneğin, bir tabloyu yanlışlıkla olduysa bugün öğlen bırakılan, yalnızca öğleden önce geçmesi gereken süreyi geri eksik tablo ve ve verileri bu yeni sunucu kopyasından alır. Belirli bir noktaya geri yükleme sunucuda veritabanı düzeyinde düzeyini değil.

Aşağıdaki adımlar bir-belirli bir noktaya için örnek sunucuyu geri yükle:
1. Azure portalında PostgreSQL için Azure veritabanı sunucunuza seçin. 

2. Sunucunun araç **genel bakış** sayfasında **geri**.

   ![PostgreSQL - genel bakış - geri düğmesi için Azure veritabanı](./media/howto-restore-server-portal/2-server.png)

3. Geri yükleme formunu gerekli bilgiler ile doldurun:

   ![PostgreSQL - geri yükleme bilgileri için Azure veritabanı](./media/howto-restore-server-portal/3-restore.png)
   - **Geri yükleme noktası**: -Geri yüklemek istediğiniz belirli bir noktaya seçin.
   - **Hedef sunucu**: Yeni sunucu için bir ad sağlayın.
   - **Konum**: Bölgeyi seçemezsiniz. Varsayılan olarak, kaynak sunucuyla aynıdır.
   - **Fiyatlandırma katmanı**: Belirli bir noktaya geri yükleme yaparken bu parametrelerin değiştiremezsiniz. Kaynak sunucuyla aynıdır. 

4. Tıklayın **Tamam** bir-belirli bir noktaya için geri yüklemek için sunucuyu geri yükleyin. 

5. Geri yükleme işlemi tamamlandıktan sonra verilerin beklenen şekilde geri yüklendiğini doğrulamak için oluşturulan yeni sunucuyu bulun.

>[!Note]
>Belirli bir noktaya geri yükleme ile oluşturulan yeni sunucusunun aynı Sunucu Yöneticisi oturum açma adını ve zaman içinde nokta var olan sunucu için geçerli bir parola Seç. Yeni Sunucu parolayı değiştirebilirsiniz **genel bakış** sayfası.

## <a name="geo-restore"></a>Coğrafi geri yükleme
Coğrafi olarak yedekli yedekleme için sunucunuzu yapılandırdıysanız, bu var olan bir sunucuyu yedekten yeni bir sunucu oluşturulabilir. Bu yeni sunucu, PostgreSQL için Azure veritabanı kullanılabilir herhangi bir bölgede oluşturulabilir.  

1. Portalın sol üst köşesinde bulunan **Kaynak oluştur** düğmesini (+) seçin. **Veritabanları** > **PostgreSQL için Azure Veritabanı**'nı seçin.

   !["PostgreSQL için Azure Veritabanı" seçeneği](./media/howto-restore-server-portal/1-navigate-to-postgres.png)

2. Formun **Kaynağı Seç** açılır listesinde, seçin **yedekleme**. Bu eylem, etkin coğrafi olarak yedekli yedeklemelere sahip sunucularının bir listesini yükler. Yeni sunucunuzun bir kaynak olarak bu yedeklemeler birini seçin.
   ![Kaynağı seçin: Yedekleme ve coğrafi olarak yedekli yedeklemeleri listesi](./media/howto-restore-server-portal/2-georestore.png)

   > [!NOTE]
   > Bir sunucu ilk oluşturulduğunda coğrafi geri yükleme için hemen kullanılabilir olmayabilir. Bu doldurulması gerekli meta veriler için birkaç saat sürebilir.
   >

3. Rest tercihlerinizi formun doldurun. Seçebilirsiniz **konumu**. Seçebileceğiniz konumu seçtikten sonra **fiyatlandırma katmanı**. Varsayılan olarak sunucusundansa mevcut sunucu parametrelerini görüntülenir. Tıklayabilirsiniz **Tamam** bu ayarları devralmak için herhangi bir değişiklik yapmadan. Veya değiştirebileceğiniz **işlem nesli** (bölgede kullanılabilir seçtiğiniz varsa) sayısı **sanal çekirdekler**, **yedekleme Bekletme dönemi**, ve **yedekleme Yedeklilik seçeneğine**. Değiştirme **fiyatlandırma katmanı** (temel, genel amaçlı ve bellek için iyileştirilmiş) veya **depolama** geri yükleme işlemi sırasında boyutu desteklenmiyor.

>[!Note]
>Coğrafi geri yükleme ile oluşturulan yeni sunucunun aynı Sunucu Yöneticisi oturum açma adı ve geri yükleme başlatıldığı anda var olan sunucu için geçerli bir parola vardır. Yeni Sunucu parola değiştirilebilir **genel bakış** sayfası.


## <a name="next-steps"></a>Sonraki adımlar
- Hizmet hakkında daha fazla bilgi [yedeklemeleri](concepts-backup.md).
- Daha fazla bilgi edinin [iş sürekliliği](concepts-business-continuity.md) seçenekleri.
