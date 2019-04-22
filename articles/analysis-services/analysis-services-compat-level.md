---
title: Veri modelini Azure Analysis Services uyumluluk düzeyinde | Microsoft Docs
description: Tablo verisi modeli uyumluluk düzeyi anlama.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 67a6c99253c549f0b8d3b55809b35b81756843eb
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58803502"
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a>Analysis Services tablolu modellerine yönelik uyumluluk düzeyi

*Uyumluluk düzeyi* Analysis Services altyapısı yayın özel davranışları gösterir. Uyumluluk düzeyini değişiklikler genellikle SQL Server'ın ana sürümler ile çakışacak. Bu değişiklikleri, Azure Analysis Services'ı, her iki platform arasında eşlik korumak için de uygulanır. Uyumluluk düzeyi, tablosal Modellerinizi etkin özellikler de değişir. Örneğin, DirectQuery ve tablosal nesne meta verilerini uyumluluk düzeyine bağlı olarak farklı uygulamalara sahip. Uyumluluk düzeyine sahip tablosal model projesi içinde Visual Studio (SSDT) belirtilir.

Azure Analysis Services 1200 ve 1400 uyumluluk düzeylerinde tablosal modelleri destekler. Son uyumluluk düzeyini 1400 ' dir. Bu düzey, SQL Server 2017 Analysis Services ile örtüşür. 1400 uyumluluk düzeyinde önemli özellikleri şunlardır:

*  TOM API'leri ve TMSL betik oluşturma desteği ile veri bağlantısı ve içeri aktarma için yeni özellikler. 
*  Veri dönüştürme ve Veri Al ve M ifadeleri kullanarak veri mashup özellikleri.
*  Ölçüler bir DAX ifadesi bir ayrıntı satırları özelliği destekler. Bu özellik, ayrıntılı verileri toplu bir rapordan aşağı inmek için Microsoft Excel gibi istemci araçlarını etkinleştirir. Örneğin, kullanıcıların bir bölge ve ay için toplam satış görüntülediğinizde ilişkili sipariş ayrıntılarını görüntüleyebilirsiniz. 
*  Nesne düzeyinde güvenlik içerdikleri verilerin yanı sıra tablo ve sütun adları.
*  Düzensiz Hiyerarşiler için gelişmiş destek.
*  Performans ve izleme geliştirmeleri.

> [!NOTE]
> Azure Analysis Services 1465 uyumluluk düzeyinde içeri aktarılan Power BI Desktop dosyalarını destekler. Ancak, her zaman bir önizleme özelliği olsaydı, Power BI Desktop işlevselliği İçeri Aktar kullanımdan kaldırıldı ve Mart 2019 içinde hizmetinden kaldırılır. Mevcut modellerde 1465 uyumluluk düzeyinde desteklenen kalır.  


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

> [!NOTE]
> Ssms'de, bir Azure Analysis Services sunucusuna bağlandığında **uyumluluk düzeyi desteklenen** özelliği gösterilir **1200**. Bu bilinen bir sorundur ve gelecek bir SSMS çözümlenir güncelleştirme. Çözümlendi, bu özellik en yüksek desteklenen uyumluluk düzeyini gösterir.

## <a name="next-steps"></a>Sonraki adımlar

  [Analysis Services'ı yönetme](analysis-services-manage.md)  
