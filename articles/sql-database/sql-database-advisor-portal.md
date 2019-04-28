---
title: Performans önerilerini - Azure SQL veritabanı uygulama | Microsoft Docs
description: Azure SQL veritabanınızın performansını en iyi duruma getirebilirsiniz performans önerileri bulmak için Azure portalını kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 12/19/2018
ms.openlocfilehash: d80581aae56fc9d65d6f24d21f2c582cb74b3f2d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61420375"
---
# <a name="find-and-apply-performance-recommendations"></a>Bulma ve performans önerilerini uygulama

Azure portalı, Azure SQL veritabanınızın performansını en iyi duruma getirebilirsiniz performans önerilerini bulun veya iş yükünüzü tanımlanan bazı sorunu gidermek için kullanabilirsiniz. **Performans önerisi** sayfası Azure portalında, üst önerileri, olası etkilerini göre Bul olanak tanır. 

## <a name="viewing-recommendations"></a>Önerileri görüntüleme

Performans önerilerini uygulama ve görüntülemek için doğru ihtiyacınız [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) azure'da izinleri. **Okuyucu**, **SQL DB Katılımcısı** öneriler, görüntülemek için gereken izinler ve **sahibi**, **SQL DB Katılımcısı** için izinleri gereklidir herhangi bir eylem yürütün; oluşturma, dizinleri bırakın veya dizin oluşturmayı iptal et.

Performans önerilerini Azure portalında bulmak için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Git **tüm hizmetleri** > **SQL veritabanları**ve veritabanınızı seçin.
3. Gidin **performans önerisi** seçili veritabanı için kullanılabilir önerileri görüntüleyebilirsiniz.

Performans önerileri üzerinde aşağıdaki şekilde gösterilene benzer tabloda gösterilmiştir:

![Öneriler](./media/sql-database-advisor-portal/recommendations.png)

Öneriler, şu kategorilere performans üzerindeki olası etkilerini göre sıralanır:

| Etki | Açıklama |
|:--- |:--- |
| Yüksek |En önemli performans etkisi yüksek etkili öneriler sağlamanız gerekir. |
| Orta |Orta etki önerileri, performansı artırmak ancak önemli ölçüde değil. |
| Düşük |Düşük etki önerileri gerek kalmadan daha iyi performans sağlamak, ancak iyileştirmeleri, önemli olmayabilir. |


> [!NOTE]
> Etkinlikler, bir gün için bazı öneriler belirlemek amacıyla en az izlemek Azure SQL veritabanı gerekir. Azure SQL veritabanı, daha kolay rastgele yetersiz etkinlik patlamalarına alabilir ve daha tutarlı sorgu desenleri için iyileştirebilirsiniz. Öneri şu anda mevcut değilse **performans önerisi** sayfası nedenini açıklayan bir ileti sağlar.
> 

Ayrıca, geçmiş işlemlerin durumunu da görüntüleyebilirsiniz. Daha fazla bilgi için bir öneri veya durumu seçin.

Azure portalında "dizin oluşturma" öneri örneği aşağıda verilmiştir.

![Dizin oluştur](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a>Önerileri uygulama
Azure SQL veritabanı size nasıl önerilerdir üzerinde tam denetim aşağıdaki üç seçenekten birini kullanarak etkin: 

* Tek tek önerileri tek bir zaman geçerlidir.
* Otomatik ayarlama önerileri otomatik olarak uygulamak için etkinleştirin.
* El ile bir öneri uygulamak için önerilen T-SQL betiği veritabanınızda çalıştırın.

Ayrıntılarını görüntülemek ve ardından herhangi bir öneri seçin **komut dosyasını Göster** önerinin nasıl oluşturulduğunu tam ayrıntılarını gözden geçirmek için.

Veritabanı otomatik ayarlama hiçbir zaman bir veritabanını çevrimdışı duruma getirir ya da öneri uygulanan--performans önerisi kullanarak çevrimiçi kalır.

### <a name="apply-an-individual-recommendation"></a>Tek bir öneri uygulamak
Gözden geçirin ve önerileri bir anda kabul et.

1. Üzerinde **önerileri** sayfasında, bir öneri seçin.
2. Üzerinde **ayrıntıları** sayfasında **Uygula** düğmesi.
   
    ![Öneri uygulandıktan](./media/sql-database-advisor-portal/apply.png)

Seçilen öneri, veritabanı üzerinde uygulanır.

### <a name="removing-recommendations-from-the-list"></a>Öneriler listeden kaldırma

Öneriler listesini listesinden kaldırmak istediğiniz öğeler içeriyorsa, öneriyi iptal edebilirsiniz:

1. Bir öneri listesinde seçin **önerileri** ayrıntılarını açın.
2. Tıklayın **at** üzerinde **ayrıntıları** sayfası.

İsterseniz, atılan öğeler yeniden ekleyebilirsiniz **önerileri** listesi:

1. Üzerinde **önerileri** sayfasında **atılanı görüntüle**.
2. Atılan öğesi ayrıntılarını görüntülemek için listeden seçin.
3. İsteğe bağlı olarak, tıklayın **geri at** dizini, ana listesine eklemek için **önerileri**.

> [!NOTE]
> Lütfen unutmayın SQL veritabanı [otomatik ayarlama](sql-database-automatic-tuning.md) etkinleştirilmiş ve el ile bir öneri listesinden atılan, bu tür bir öneri hiçbir zaman otomatik olarak uygulanır. Bir öneri atılıyor, belirli bir öneri uygulanan olmamalıdır gerektiren durumlarda etkinleştirilir otomatik ayarlama kullanıcılar için kullanışlı bir yoldur.
> Geri alma iptal seçeneğini belirleyerek atılan öneriler önerileri listesine ekleyerek bu davranışı geri dönebilirsiniz.
> 

### <a name="enable-automatic-tuning"></a>Otomatik ayarlamayı etkinleştirme
Azure SQL veritabanı önerileri otomatik olarak uygulamak için ayarlayabilirsiniz. Önerileri kullanılabilir oldukça, bunlar otomatik olarak uygulanır. Performans etkisi negatif ise hizmet tarafından yönetilen tüm önerileri gibi ile öneri geri döndürüldü.

1. Üzerinde **önerileri** sayfasında **otomatikleştirme**:
   
    ![Advisor ayarları](./media/sql-database-advisor-portal/settings.png)
2. Eylemleri otomatikleştirmek için seçin:
   
    ![Önerilen dizinleri](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> Lütfen unutmayın **DROP_INDEX** seçeneği şu anda bölüm değiştirme ve dizin ipuçlarını kullanarak uygulamaları ile uyumlu değil. 
>

İstediğiniz yapılandırma seçtikten sonra Uygula'yı tıklatın.

### <a name="manually-apply-recommendations-through-t-sql"></a>El ile T-SQL aracılığıyla önerileri uygulayın

Herhangi bir öneri seçin ve ardından **komut dosyasını Göster**. Bu betik, el ile bir öneri uygulamak için veritabanınızda çalıştırın.

*El ile çalıştırılan dizin değil izlenen ve performans etkisi için hizmet tarafından doğrulanmış* önerilen oluşturulduktan sonra bunlar performans artışı sağlar ve ayarlamak veya gerekirse silme doğrulamak için bu dizinleri izlemek için. Dizin oluşturma hakkında daha fazla bilgi için bkz: [dizin oluşturma (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx). Ayrıca, el ile uygulanan önerileri 24-48 saat için etkin ve öneriler listesinde gösterilen kalır. Sistem otomatik olarak bunları testimizde önce. Bir öneri daha çabuk kaldırmak istiyorsanız, el ile iptal edebilirsiniz.

### <a name="canceling-recommendations"></a>Öneriler iptal ediliyor

Bulunan önerileri bir **bekleyen**, **doğrulama**, veya **başarı** durumu iptal edilemez. Durumu ile ilgili öneriler **Executing** iptal edilemez.

1. İçinde bir öneri seçin **ayarlama geçmişi** açmak için alan **öneri ayrıntılarını** sayfası.
2. Tıklayın **iptal** öneriyi uygulama işlemi iptal etmek için.

## <a name="monitoring-operations"></a>İzleme işlemleri

Bir öneriyi uygulama anında olabilir değil. Portal, öneri durumuyla ilgili ayrıntıları sağlar. Bir dizin olabilir olası durumlar şunlardır:

| Durum | Açıklama |
|:--- |:--- |
| Beklemede |Komut alındı ve yürütme için zamanlanan öneri uygulayın. |
| Yürütülüyor |Öneri uygulanıyor. |
| Doğrulanıyor |Öneri başarıyla uygulandı ve hizmet, avantajlar ölçme. |
| Başarılı |Öneri başarıyla uygulandı ve avantajları ölçülür. |
| Hata |Öneriyi uygulama işlemi sırasında bir hata oluştu. Bu geçici bir sorun veya büyük olasılıkla bir şema olabilir tablosuna dönüştürmek ve komut dosyası artık geçerli değil. |
| Geri döndürülüyor |Öneri uygulandı, ancak yüksek performanslı dışı olarak kabul edildi ve otomatik olarak geri döndürülüyor. |
| Geri döndürüldü |Öneri geri döndürüldü. |

Daha fazla bilgi için listeden bir işlemde öneri tıklayın:

![Önerilen dizinleri](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a>Bir öneriyi geri almanızın
Öneri uygulamak için performans önerisi kullandıysanız eksi değer için performans etkisi bulursa (el ile T-SQL betiği çalışmadı anlamına gelir), bunu otomatik olarak değişiklik döner. Herhangi bir nedenle yalnızca bir öneri geri dönmek istiyorsanız, aşağıdakileri yapabilirsiniz:

1. İçinde başarılı bir şekilde uygulanan bir öneri seçin **ayarlama geçmişi** alan.
2. Tıklayın **Revert** üzerinde **öneri ayrıntılarını** sayfası.

![Önerilen dizinleri](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a>Dizin önerileri performans etkisini izleme
Öneriler başarıyla kullanılmaya başladıktan sonra (şu anda dizin işlemleri ve sorguları önerileri yalnızca Parametreleştirme), tıklayabilirsiniz **sorgu öngörüleri** öneriye açmak için Ayrıntılar sayfasını [sorgu Performans öngörüleri](sql-database-query-performance.md) ve sık kullandığınız sorguların performans etkisini bakın.

![İzleyici performansına etkisi](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a>Özet
Azure SQL veritabanı, SQL veritabanı performansını geliştirmeye yönelik öneriler sağlar. T-SQL betikleri sağlayarak veritabanınızı en iyi duruma getirme ve sonuçta sorgu performansını geliştirmeye yardım alın.

## <a name="next-steps"></a>Sonraki adımlar
Önerilerinizi izleyin ve performansı iyileştirmek için bunları uygulanmaya devam eder. Veritabanı iş yüklerini, dinamik ve sürekli olarak değiştirin. Azure SQL veritabanı, büyük olasılıkla veritabanınızın performansını iyileştirebilir önerileri sağlamak ve izlemek devam eder. 

* Bkz: [otomatik ayarlama](sql-database-automatic-tuning.md) Azure SQL veritabanı'nda otomatik ayarlama hakkında daha fazla bilgi edinmek için.
* Bkz: [performans önerileri](sql-database-advisor.md) genel bakış, Azure SQL veritabanı performans için.
* Bkz: [sorgu performansı öngörüleri](sql-database-query-performance.md) performans etkisini en sık kullandığınız sorguların görüntüleme hakkında bilgi edinmek için.

## <a name="additional-resources"></a>Ek kaynaklar
* [Sorgu Deposu](https://msdn.microsoft.com/library/dn817826.aspx)
* [DİZİN OLUŞTURMA](https://msdn.microsoft.com/library/ms188783.aspx)
* [Rol tabanlı erişim denetimi](../role-based-access-control/overview.md)

