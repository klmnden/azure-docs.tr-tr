---
title: "Performans önerileri - Azure SQL veritabanı uygulamak | Microsoft Docs"
description: "Azure SQL veritabanınızın performansını en iyi duruma getirebilirsiniz performans önerileri bulmak için Azure Portalı'nı kullanın."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: cda8a646-0584-4368-b28a-85cdd9b54fcd
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 3c621fc557ed466ddf2b514136a32d98be454325
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="find-and-apply-performance-recommendations"></a>Bul ve performans önerileri uygulayın

Azure portalı, Azure SQL veritabanı performansını en iyi duruma getirebilirsiniz performans önerileri bulmak için veya yükünüzü içinde tanımlanan bazı sorunu gidermek için kullanabilirsiniz. **Performans öneri** sayfası Azure portalında olası etkilerini dayalı üst önerileri bulmanızı sağlar. 

## <a name="viewing-recommendations"></a>Öneriler görüntüleme

Görüntülemek ve performans önerileri uygulamak için doğru ihtiyacınız [rol tabanlı erişim denetimi](../active-directory/role-based-access-control-what-is.md) Azure izinleri. **Okuyucu**, **SQL DB Katılımcısı** önerileri, görüntülemek için gereken izinler ve **sahibi**, **SQL DB Katılımcısı** herhangi bir eylem yürütme; oluşturun veya dizinleri bırakın ve dizin oluşturma işlemini iptal etmek için gereken izinler.

Azure Portal'da performans önerileri bulmak için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Git **daha fazla hizmet** > **SQL veritabanları**, veritabanınızı seçin.
3. Gidin **performans öneri** seçili veritabanı için kullanılabilir önerilerini görüntülemek için.

Performans önerileri üzerinde aşağıdaki şekilde gösterilene benzer tablo gösterilmektedir:

![Öneriler](./media/sql-database-advisor-portal/recommendations.png)

Öneriler olası etkilerini performansı şu kategorilere göre sıralanır:

| Etkisi | Açıklama |
|:--- |:--- |
| Yüksek |Yüksek etkisi önerileri en önemli performans etkisi sağlamalıdır. |
| Orta |Orta etkisi önerileri performansını artırmak, ancak önemli ölçüde değil. |
| Düşük |Düşük etkisi önerileri olmadan daha iyi performans sağlaması gerekir, ancak geliştirmeleri önemli ölçüde olmayabilir. |


> [!NOTE]
> Azure SQL veritabanı etkinlikleri en az bir gün için bazı öneriler belirlemek amacıyla izleneceği gerekir. Bu etkinlik için rastgele yetersiz WINS'e daha Azure SQL veritabanı daha kolay tutarlı sorgu desenlerine için en iyi duruma getirebilirsiniz. Önerileri şu anda kullanılabilir değilse **performans öneri** sayfası, nedenini açıklayan bir ileti sağlar.
> 

Ayrıca, geçmiş işlemleri durumunu görüntüleyebilirsiniz. Daha fazla ayrıntı görmek için bir öneri veya durumu seçin.

"Dizin oluşturma" öneri Azure portalında bir örneği burada verilmiştir.

![Dizin oluşturma](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a>Önerileri uygulama
Azure SQL veritabanına nasıl önerilerdir üzerinde tam denetim verir aşağıdaki üç seçenekten birini kullanarak etkin: 

* Tek tek önerileri biri aynı anda geçerlidir.
* Otomatik olarak önerileri uygulamak için ayarlama otomatik etkinleştirin.
* Bir öneri el ile uygulamak için veritabanıyla önerilen T-SQL komut dosyasını çalıştırın.

Ayrıntılarını görüntülemek ve ardından herhangi bir önerisi seçin **görüntülemek betik** öneri nasıl oluşturulduğunu tam ayrıntılarını gözden geçirmek için.

Öneri--performans öneri kullanılarak uygulanan ya da otomatik ayarlama hiçbir zaman bir veritabanını çevrimdışı duruma getirir, veritabanı çevrimiçi kalır.

### <a name="apply-an-individual-recommendation"></a>Tek bir öneriyi
Gözden geçirin ve aynı anda önerileri bir kabul edebilir.

1. Üzerinde **önerileri** sayfasında, bir öneri seçin.
2. Üzerinde **ayrıntıları** sayfasında, **Uygula** düğmesi.
   
    ![öneriyi](./media/sql-database-advisor-portal/apply.png)

Seçili öneri veritabanı üzerinde uygulanır.

### <a name="removing-recommendations-from-the-list"></a>Öneriler listesinden kaldırılıyor
Öneriler listesi listeden kaldırmak istediğiniz öğeler içeriyorsa, öneri atabilirsiniz:

1. Bir öneri listesinde seçin **önerileri** ayrıntıları açın.
2. Tıklatın **atmak** üzerinde **ayrıntıları** sayfası.

İsterseniz, atılan öğeler yeniden ekleyebilirsiniz **önerileri** listesi:

1. Üzerinde **önerileri** sayfasında, **atılan Görünüm**.
2. Ayrıntılarını görüntülemek için listeden atılan bir öğe seçin.
3. İsteğe bağlı olarak, tıklayın **geri atmak** dizin, ana listesine eklemek için **önerileri**.


### <a name="enable-automatic-tuning"></a>Otomatik ayarlamayı etkinleştirme
Öneriler otomatik olarak uygulamak için Azure SQL veritabanı ayarlayabilirsiniz. Öneriler kullanılabilir duruma geldiğinde, bunlar otomatik olarak uygulanır. Performans etkisi olumsuz olursa hizmet tarafından yönetilen tüm öneriler gibi öneri döndürüldü.

1. Üzerinde **önerileri** sayfasında, **otomatikleştirme**:
   
    ![Advisor ayarları](./media/sql-database-advisor-portal/settings.png)
2. Otomatik hale getirmek için Eylemler seçin:
   
    ![Dizinleri önerilir](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-the-recommended-t-sql-script"></a>El ile önerilen T-SQL betiği çalıştırma
Herhangi bir önerisi seçin ve ardından **görüntülemek betik**. Bu komut dosyasını el ile öneri uygulamak için veritabanına karşı çalışırlar.

*El ile gerçekleştirilen dizinler değil izlenen ve performans etkisi için hizmet tarafından doğrulanan* önerilen bunlar performans artışı sağlar ve ayarlamak veya gerekirse silme doğrulamak için oluşturulduktan sonra bu dizinler izlemek için. Dizin oluşturma hakkında daha fazla bilgi için bkz: [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).

### <a name="canceling-recommendations"></a>Öneriler iptal etme
Bulunan önerileri bir **bekleyen**, **doğrulama**, veya **başarı** durumu iptal edilemez. Önerileri durumuna sahip bir **Executing** iptal edilemez.

1. Bir öneri seçin **ayarlama geçmişi** açmak için alan **önerileri ayrıntıları** sayfası.
2. Tıklatın **iptal** öneri uygulama işlemi durdurulacak.

## <a name="monitoring-operations"></a>İzleme işlemleri
Bir öneri uygulama eşzamanlı olarak gerçekleşebilir değil. Portal, öneri durumuyla ilgili ayrıntıları sağlar. Bir dizin olabilir olası durumlar şunlardır:

| Durum | Açıklama |
|:--- |:--- |
| Beklemede |Komut alındı ve çalıştırılmak üzere zamanlanmış öneri uygulayın. |
| Yürütme |Öneri uygulanmaktadır. |
| Doğrulama |Öneri başarıyla uygulandı ve hizmet avantajlarını ölçme. |
| Başarılı |Öneri başarıyla uygulandı ve avantajları ölçülür. |
| Hata |Öneriyi uygulama işlemi sırasında bir hata oluştu. Bu geçici bir sorun veya büyük olasılıkla bir şema, tablo olarak değiştirin ve komut dosyası artık geçerli değil. |
| Geri alma |Öneri uygulandı, ancak kullanıcı dışı olarak kabul edildi ve otomatik olarak geri döndürülüyor. |
| Döndürüldü |Öneri geri döndürüldü. |

Daha fazla ayrıntı görmek için bir işlem içi öneri listeden tıklatın:

![Dizinleri önerilir](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a>Bir öneri dönüştürme
Öneriyi için performans önerileri kullandıysanız negatif olamaz performans etkisi bulursa (T-SQL komut dosyasını el ile çalışmadı anlamına gelir), onu otomatik olarak değişikliği geri döner. Herhangi bir nedenle yalnızca bir öneri dönmek istiyorsanız, aşağıdakileri yapabilirsiniz:

1. Başarıyla uygulandı öneride seçin **geçmişi ayarlama** alanı.
2. Tıklatın **Revert** üzerinde **öneri ayrıntılarını** sayfası.

![Dizinleri önerilir](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a>Dizin önerileri performans etkisini izleme
Önerileri başarıyla uygulandıktan sonra (şu anda, dizin işlemleri ve yalnızca sorguları önerileri Parametreleştirme) tıklayabilirsiniz **sorgu öngörüleri** öneriye açmak için Ayrıntılar sayfası [sorgu Performansı öngörüleri](sql-database-query-performance.md) ve üst sorgularınızı performans etkisini bakın.

![İzleyici performansına etkisi](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a>Özet
Azure SQL veritabanı SQL veritabanı performansı artırmak için öneriler sağlar. T-SQL betikleri sağlayarak veritabanınızı en iyi duruma getirme ve sonuçta sorgu performansını iyileştirme yardım alın.

## <a name="next-steps"></a>Sonraki adımlar
Önerilerinizi izleyebilir ve bunları performansı iyileştirmek için uygulanmaya devam eder. Veritabanı iş yüklerini, dinamik ve sürekli olarak değiştirin. Azure SQL veritabanı, izlemek ve veritabanınızın performansı artırmak öneriler sunmak devam eder. 

* Bkz: [otomatik ayarlama](sql-database-automatic-tuning.md) Azure SQL veritabanı'nda otomatik ayarlama hakkında daha fazla bilgi için.
* Bkz: [performans önerileri](sql-database-advisor.md) Azure SQL veritabanı performans önerileri genel bakış.
* Bkz: [sorgu performansı öngörüleri](sql-database-query-performance.md) üst sorgularınızı performans etkisini görüntüleme hakkında bilgi edinmek için.

## <a name="additional-resources"></a>Ek kaynaklar
* [Sorgu deposu](https://msdn.microsoft.com/library/dn817826.aspx)
* [DİZİN OLUŞTURMA](https://msdn.microsoft.com/library/ms188783.aspx)
* [Rol tabanlı erişim denetimi](../active-directory/role-based-access-control-what-is.md)

