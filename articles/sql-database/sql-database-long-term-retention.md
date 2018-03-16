---
title: "10 yılı aşkın Azure SQL veritabanı yedeklemelerini depolamak | Microsoft Docs"
description: "Azure SQL veritabanı depolanmasını yedeklemeleri 10 yılı aşkın nasıl desteklediğini öğrenin."
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.date: 12/22/2016
ms.author: sashan
ms.openlocfilehash: 2f31e89fce2746e57d6a670aef949d0d534af4c1
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Azure SQL veritabanı yedeklemelerini depolamak için en fazla 10 yıl
Birçok uygulama yasal varsa, uyumluluk ya da diğer iş amaçlı veritabanı yedeklemeleri Azure SQL veritabanı tarafından sağlanan 7-35 gün dışında tutulacak gerektiren [otomatik yedeklemeler](sql-database-automated-backups.md). Uzun vadeli yedekleme bekletme özelliğini kullanarak, 10 yılı aşkın bir Azure kurtarma Hizmetleri kasasına SQL veritabanı yedeklerinizi depolayabilirsiniz. Kasa başına en çok 1.000 veritabanları depolayabilirsiniz. Ardından, yeni bir veritabanı olarak geri yüklemek için kasa herhangi bir yedekleme de seçebilirsiniz.

> [!IMPORTANT]
> Uzun vadeli yedekleme bekletme olduğundan şu anda önizlemede ve aşağıdaki bölgelerde kullanılabilir: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Orta ABD, Doğu Asya, Doğu ABD, Doğu ABD 2, Hindistan Orta, Hindistan Güney, Japonya Doğu, Japonya Batı, Kuzey Orta ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Batı Avrupa ve Batı ABD.
>

> [!NOTE]
> Kasa başına en fazla 200 veritabanları 24 saatlik dönem boyunca etkinleştirebilirsiniz. Bu sınır etkisini en aza indirmek için her sunucu için ayrı bir kasa kullanmanızı öneririz. 
> 

## <a name="how-sql-database-long-term-backup-retention-works"></a>SQL veritabanı uzun vadeli yedekleme bekletme nasıl çalışır?

Uzun vadeli yedekleme bekletme ile bir SQL veritabanı sunucusuna Azure kurtarma Hizmetleri kasası ile ilişkilendirebilirsiniz. 

* SQL server oluşturulan aynı Azure abonelikte ve kaynak grubu ve aynı coğrafi bölgede kasası oluşturmanız gerekir. 
* Ardından, herhangi bir veritabanı için bir bekletme ilkesi de yapılandırın. İlke, Kurtarma Hizmetleri Kasası'na kopyalanır ve belirtilen Bekletme dönemi (en fazla 10 yıl) korunur haftalık tam veritabanı yedeklemeleri neden olur. 
* Ardından veritabanı bu yedeklemeler hiçbirini abonelik herhangi bir sunucuya yeni bir veritabanına geri yükleyebilirsiniz. Azure depolama mevcut yedeklerden bir kopyasını oluşturur ve kopyalama performansını mevcut veritabanında bir etkisi yoktur.

> [!TIP]
> Nasıl yapılır kılavuzu için bkz: [yapılandırma ve Azure SQL veritabanı uzun vadeli yedekleme bekletme geri](sql-database-long-term-backup-retention-configure.md).

## <a name="enable-long-term-backup-retention"></a>Uzun vadeli yedekleme bekletme etkinleştir

Bir veritabanı için uzun vadeli yedekleme bekletme yapılandırmak için:

1. Bir Azure kurtarma Hizmetleri kasası, SQL veritabanı sunucusu olarak aynı bölge, abonelik ve kaynak grubu oluşturun. 
2. Sunucuyu kasaya kaydedin.
3. Bir Azure kurtarma Hizmetleri koruma ilkesi oluşturun.
4. Koruma İlkesi uzun vadeli yedekleme bekletme gerektiren veritabanları için geçerlidir.

Yapılandırma, yönetme ve Azure kurtarma Hizmetleri kasası Otomatik yedeklemelerin uzun vadeli yedekleme bekletme bir veritabanını geri için aşağıdakilerden birini yapın:

* Azure portalını kullanarak: tıklatın **uzun vadeli yedekleme bekletme**, bir veritabanı seçin ve ardından **yapılandırma**. 

   ![Uzun vadeli yedekleme bekletme için bir veritabanı seçin](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

* PowerShell kullanarak: Git [yapılandırma ve Azure SQL veritabanı uzun vadeli yedekleme bekletme geri](sql-database-long-term-backup-retention-configure.md).

## <a name="restore-a-database-thats-stored-with-the-long-term-backup-retention-feature"></a>Uzun vadeli yedekleme bekletme özelliğiyle depolanan bir veritabanını geri yükle

Uzun vadeli yedekleme bekletme yedeklemeden kurtarmak için:

1. Yedeğin depolandığı kasası listeleyin.
2. Mantıksal sunucunuza eşlenen kapsayıcı listeleyin.
3. Veri kaynağı veritabanınıza eşlenen kasa içinde listeleyin.
4. Geri yüklemek kullanılabilir kurtarma noktaları listesi.
5. Veritabanı kurtarma noktasından aboneliğinizi içinde hedef sunucuya geri yükleyin.

Yapılandırma, yönetme ve Azure kurtarma Hizmetleri kasası Otomatik yedeklemelerin uzun vadeli yedekleme bekletme bir veritabanını geri için aşağıdakilerden birini yapın:

* Azure portalını kullanarak: Git [Azure portalını kullanarak uzun vadeli yedekleme bekletme yönetmek](sql-database-long-term-backup-retention-configure.md). 

* PowerShell kullanarak: Git [uzun vadeli yedekleme bekletme PowerShell kullanarak yönetme](sql-database-long-term-backup-retention-configure.md).

## <a name="get-pricing-for-long-term-backup-retention"></a>Uzun vadeli yedekleme bekletme için fiyatlandırma Al

SQL veritabanı uzun vadeli yedekleme bekletme ücret göre [oranları fiyatlandırma Azure yedekleme Hizmetleri](https://azure.microsoft.com/pricing/details/backup/).

SQL veritabanı sunucusuna Kasası'na kaydedildikten sonra kasasında depolanan haftalık yedekleri tarafından kullanılan toplam depolama alanı için ücretlendirilirsiniz.

## <a name="view-available-backups-that-are-stored-in-long-term-backup-retention"></a>Uzun vadeli yedekleme bekletme içinde depolanan kullanılabilir yedekleri görüntüle

Yapılandırma, yönetme ve Azure portalını kullanarak bir veritabanını bir Azure kurtarma Hizmetleri kasası Otomatik yedeklemelerin uzun vadeli yedekleme bekletme geri için aşağıdakilerden birini yapın:

* Azure portalını kullanarak: Git [Azure portalını kullanarak uzun vadeli yedekleme bekletme yönetmek](sql-database-long-term-backup-retention-configure.md). 

* PowerShell kullanarak: Git [uzun vadeli yedekleme bekletme PowerShell kullanarak yönetme](sql-database-long-term-backup-retention-configure.md).

## <a name="disable-long-term-retention"></a>Uzun vadeli bekletme devre dışı bırak

Kurtarma hizmeti temizleme yedeklerini sağlanan bekletme ilkesini temel alarak otomatik olarak yönetir. 

Belirli bir veritabanı için yedekleme Kasası'na gönderilmesini durdurmak için bu veritabanı için bekletme ilkesini kaldırın.
  
```
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName 'RG1' -ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
```

> [!NOTE]
> Zaten kasaya olan yedekleri etkilenmez. Kendi saklama süresi sona erdiğinde, bunlar kurtarma hizmeti tarafından otomatik olarak silinir.

## <a name="long-term-backup-retention-faq"></a>Uzun vadeli yedekleme bekletme SSS

**El ile de kasadaki belirli yedeklemelerinize silebilmek için?**

Şu anda değil. Bekletme süresi dolduğunda kasa yedekler otomatik olarak temizlenir.

**Birden fazla kasasına yedeklemelerini depolamak için my server kayıt mi?**

Hayır, aynı anda yalnızca bir kasa yedeklemeler şu anda depolayabilirsiniz.

**Farklı Aboneliklerde kasası ve sunucu olabilir?**

Hayır, şu anda kasası ve sunucu aynı abonelik ve kaynak grubu olması gerekir.

**I my sunucunun bölgesinden farklı bir bölgede oluşturulan bir kasa kullanabilir miyim?**

Hayır, sunucu ve kasa kopyalama süresini en aza indirmek ve trafiği ücretlere önlemek için aynı bölgede olması gerekir.

**Kaç tane veritabanları t bir kasaya depolayabilir miyim?**

Şu anda, kasa başına en çok 1.000 veritabanlarını destekler. 

**Abonelik başına kaç tane kasalarını oluşturabilirim?**

Abonelik başına en fazla 25 kasa oluşturabilirsiniz.

**Kasa başına günde kaç tane veritabanları yapılandırabilir miyim?**

Günde bir kasa başına 200 veritabanları ayarlayabilirsiniz.

**Uzun vadeli yedekleme bekletme esnek havuzları ile çalışır mı?**

Evet. Havuzdaki herhangi bir veritabanı bekletme ilkesi ile yapılandırılabilir.

**Yedeklemenin oluşturulduğu zaman seçebilir miyim?**

Hayır, SQL veritabanı veritabanlarınızı performans etkisini en aza indirmek için yedekleme zamanlamasını denetler.

**Saydam veri şifreleme için veritabanı etkin var. Bu Kasayla birlikte kullanabilir miyim?** 

Evet, saydam veri şifreleme desteklenir. Özgün veritabanı artık mevcut olsa bile kasadan veritabanını geri yükleyebilirsiniz.

**Aboneliğimi askıya alınırsa yedekleme kasasında ile ne olur?** 

Aboneliğiniz askıya alınırsa varolan veritabanları ve yedeklemeleri korur. Yeni yedekleme Kasası'na kopyalanmaz. Abonelik yeniden sonra hizmet yedekleme Kasası'na kopyalama devam eder. Kasanızı abonelik askıya alınmadan önce var. kopyalanan yedeklemeler kullanarak geri yükleme işlemleri için erişilebilir hale gelir. 

**I indirin veya SQL Server'a geri erişim SQL veritabanı yedekleme dosyaları alabilirim?**

Hayır, şu anda.

**Bir SQL bekletme ilkesi içinde birden çok zamanlama (günlük, haftalık, aylık, Yıllık) olması mümkündür.**

Hayır, birden çok zamanlamaları, yalnızca sanal makine yedeklemeler için şu anda kullanılabilir.

**Ne biz uzun vadeli yedekleme bekletme aktif coğrafi çoğaltma ikincil veritabanı bir veritabanı üzerinde ayarlama?**

Biz yedeklemeleri çoğaltmaları yok tuttuğundan, da şu anda ikincil veritabanlarında uzun vadeli yedekleme bekletme için bir seçenek yoktur. Bununla birlikte, bu nedenle uzun vadeli yedekleme bekletme aktif coğrafi çoğaltma ikincil veritabanı üzerinde ayarlamak kullanıcıların önemlidir:
* Bir yük devretme olur ve veritabanının birincil veritabanı haline gelir ki tam bir kasa için karşıya yedekleme, alın.
* Var. ek müşteriye uzun vadeli yedekleme bekletme ikincil bir veritabanı üzerinde ayarlamak için bir maliyeti yoktur.

## <a name="next-steps"></a>Sonraki adımlar
Veritabanı Yedeklemeleri yanlışlıkla Bozulması veya silinmesi verileri korumak için herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejiniz temel parçası oldukları. Diğer SQL veritabanı iş sürekliliği çözümleri hakkında bilgi edinmek için [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
