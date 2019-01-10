---
title: Veri modelini Azure Analysis Services uyumluluk düzeyinde | Microsoft Docs
description: Tablo verisi modeli uyumluluk düzeyi anlama.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 31ca6deef6d81ca7beb08f6df1a15d52ef381a46
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54190400"
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a>Analysis Services tablolu modellerine yönelik uyumluluk düzeyi

*Uyumluluk düzeyi* Analysis Services altyapısı yayın özel davranışları gösterir. Uyumluluk düzeyini değişiklikler genellikle SQL Server'ın ana sürümler ile çakışacak. Bu değişiklikleri, Azure Analysis Services'ı, her iki platform arasında eşlik korumak için de uygulanır. Uyumluluk düzeyi, tablosal Modellerinizi etkin özellikler de değişir. Örneğin, DirectQuery ve tablosal nesne meta verilerini uyumluluk düzeyine bağlı olarak farklı uygulamalara sahip. Uyumluluk düzeyine sahip tablosal model projesi içinde Visual Studio (SSDT) belirtilir. İçinde oluşturulan ve Power BI Desktop'tan alınan tablosal modelleri yalnızca kullanarak 1400 uyumluluk düzeyinde ' dir.

Azure Analysis Services 1200 ve 1400 uyumluluk düzeylerinde tablosal modelleri destekler. 

> [!NOTE]
> Power BI Desktop Eylül 2018'e ve sonraki sürümleri 1465 bir .pbix uyumluluk düzeyine sahip. Bu uyumluluk düzeyi, Azure Analysis Services'de desteklenir. Ancak, bir Power BI Desktop dosyasını içeri üretim ortamları için önerilmez. Daha fazla bilgi için bkz. [bir Power BI Desktop dosyasını içeri aktarma](analysis-services-import-pbix.md).

Son uyumluluk düzeyini 1400 ' dir. Bu düzey, SQL Server 2017 Analysis Services ile örtüşür. 1400 uyumluluk düzeyinde önemli özellikleri şunlardır:

*  TOM API'leri ve TMSL betik oluşturma desteği ile veri bağlantısı ve içeri aktarma için yeni özellikler. 
*  Veri dönüştürme ve Veri Al ve M ifadeleri kullanarak veri mashup özellikleri.
*  Ölçüler bir DAX ifadesi bir ayrıntı satırları özelliği destekler. Bu özellik, ayrıntılı verileri toplu bir rapordan aşağı inmek için Microsoft Excel gibi istemci araçlarını etkinleştirir. Örneğin, kullanıcıların bir bölge ve ay için toplam satış görüntülediğinizde ilişkili sipariş ayrıntılarını görüntüleyebilirsiniz. 
*  Nesne düzeyinde güvenlik içerdikleri verilerin yanı sıra tablo ve sütun adları.
*  Düzensiz Hiyerarşiler için gelişmiş destek.
*  Performans ve izleme geliştirmeleri.
 
## <a name="set-compatibility-level"></a>Uyumluluk düzeyini ayarlama

 Yeni bir tablosal model projesi SSDT'de oluştururken, şirket uyumluluk düzeyini belirtebilirsiniz **Tabular modeli Tasarımcısı** iletişim. 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 Seçerseniz **bu iletiyi tekrar gösterme** seçeneği, tüm sonraki proje, varsayılan olarak belirttiğiniz uyumluluk düzeyini kullanır. SSDT'de varsayılan uyumluluk düzeyinde değiştirebilirsiniz **Araçları** > **seçenekleri**.  
  
 Mevcut bir tablosal model projesi ssdt'de yükseltmek için ayarlanmış **uyumluluk düzeyi** modeldeki özellik **özellikleri** penceresi. Unutmayın, uyumluluk düzeyini yükseltme geri alınamaz.
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a>SQL Server Management Studio'da tablo modelli bir veritabanı uyumluluk düzeyini denetleyin 

 SSMS'de, veritabanı adına sağ tıklayın > **özellikleri** > **uyumluluk düzeyi**.  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a>Ssms'de sunucu için desteklenen uyumluluk düzeyini denetleyin  

 SSMS'de sunucu adına sağ tıklayın > **özellikleri** > **uyumluluk düzeyi desteklenen**.  
  
 Bu özellik, en yüksek (Önizleme hariç) sunucusunda çalışacak bir veritabanı uyumluluk düzeyini belirtir. Desteklenen uyumluluk düzeyi değiştirilemez.  

## <a name="next-steps"></a>Sonraki adımlar

  [Azure portalında bir model oluşturma](analysis-services-create-model-portal.md)   
  [Analysis Services'ı yönetme](analysis-services-manage.md)  
