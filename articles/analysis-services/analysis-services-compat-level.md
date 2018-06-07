---
title: Veri modeli uyumluluk düzeyi Azure Analysis Services | Microsoft Docs
description: Tablo veri modeline uyumluluk düzeyi anlama.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: d68d544a66448fbbf193ff53fa43e179b1edb706
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34602076"
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a>Analysis Services tablolu modeller için uyumluluk düzeyi

*Uyumluluk düzeyi* Analysis Services altyapısı sürüm özgü davranışlarının başvuruyor. Uyumluluk düzeyini değişiklikler genellikle ana SQL Server sürümleri ile çakıştığı. Bu değişiklikler, Azure Analysis Services'ı, her iki platform arasında eşliği sağlamak da uygulanır. Uyumluluk düzeyi, ayrıca, tablolu modeller etkisi özellikler değiştirir. Örneğin, DirectQuery ve tablo nesne meta verilerini uyumluluk düzeyine bağlı olarak farklı uygulamaları vardır. Uyumluluk düzeyi tablolu modeli projesi, Visual Studio (SSDT) belirtildi. İçinde oluşturulan ve Power BI masaüstünden alınan tablolu modeller 1400 uyumluluk yalnızca düzeyindedir.

Azure Analysis Services tablolu modelleri 1200 ve 1400 uyumluluk düzeylerinde destekler. 

Son uyumluluk düzeyini 1400 ' dir. Bu düzey, SQL Server 2017 Analysis Services ile örtüşür. 1400 uyumluluk düzeyi önemli özellikleri şunlardır:

*  Veri bağlantısını ve içeri aktarma için yeni özellikler ZEL API'ları ve TMSL komut dosyası çalıştırma desteği. 
*  Veri dönüştürme ve Veri Al ve M ifadeler kullanarak veri karma özellikleri.
*  Ölçüler bir DAX ifadesi ayrıntı satırları özelliğiyle destekler. Bu özellik toplanan raporundan ayrıntılı veri aşağıya doğru incelemek için Microsoft Excel gibi istemci araçlar sağlar. Örneğin, kullanıcıların bir bölge ve ay için toplam satış görüntülediğinizde ilişkili sipariş ayrıntılarını görüntüleyebilirsiniz. 
*  Nesne düzeyinde güvenlik içerdikleri verileri yanı sıra tablo ve sütun adları için.
*  Düzensiz hiyerarşileri için gelişmiş destek.
*  Performans ve izleme geliştirmeleri.
  
## <a name="set-compatibility-level"></a>Set uyumluluk düzeyi 
 SSDT içinde yeni bir tablo modeli projesi oluştururken, üzerinde uyumluluk düzeyini belirtebilirsiniz **Tabular modeli Tasarımcısı** iletişim. 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 Seçerseniz **bu iletiyi tekrar gösterme** seçeneği, tüm sonraki projeleri varsayılan olarak belirttiğiniz uyumluluk düzeyi kullanın. SSDT içinde varsayılan uyumluluk düzeyi değiştirebileceğiniz **Araçları** > **seçenekleri**.  
  
 SSDT var olan tablo modeli projesinde yükseltmek için ayarlanmış **uyumluluk düzeyini** modeldeki özellik **özellikleri** penceresi. İçinde-unutmayın, uyumluluk düzeyini yükseltme işlemi geri alınamaz.
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a>SQL Server Management Studio'da tablo modelli bir veritabanı uyumluluk düzeyi denetimi 
 SSMS, veritabanı adına sağ tıklayın > **özellikleri** > **uyumluluk düzeyi**.  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a>SSMS Server'da desteklenen uyumluluk düzeyini denetleyin  
 SSMS, sunucu adına sağ tıklayın > **özellikleri** > **desteklenen uyumluluk düzeyi**.  
  
 Bu özellik yüksek (Önizleme hariç) sunucuda çalışacak bir veritabanı uyumluluk düzeyini belirtir. Desteklenen uyumluluk düzeyi değiştirilemez.  

## <a name="next-steps"></a>Sonraki adımlar
  [Azure portalında bir model oluşturma](analysis-services-create-model-portal.md)   
  [Çözümleme Hizmetleri yönetme](analysis-services-manage.md)  
