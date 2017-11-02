---
title: "Azure Analysis Services Adventure Works Öğreticisi | Microsoft Docs"
description: "Azure Analysis Services için Adventure Works öğreticisini sunar"
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 0e223222c482d6d3aeaed85388f3a1ce1b53a78d
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="azure-analysis-services---adventure-works-tutorial"></a>Azure Analysis Services - Adventure Works Öğreticisi

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Bu öğreticide, [SQL Server Veri Araçları’nı (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) kullanarak 1400 uyumluluk düzeyinde bir tablosal modelin nasıl oluşturulduğu ve dağıtıldığı ile ilgili dersler sağlanmaktadır.  

Analysis Services ve tablosal modelleme konusunda yeniyseniz, basit bir tablosal model oluşturup dağıtmayı öğrenmenin en hızlı yolu bu öğreticiyi tamamlamaktır. Ön koşullar karşılandıktan sonra, tamamlanması iki ila üç saat arası sürmelidir.  
  
## <a name="what-you-learn"></a>Öğrenecekleriniz   
  
-   SSDT’de **1400 uyumluluk düzeyinde** yeni bir tablosal model projesi oluşturma.
  
-   İlişkisel veritabanındaki verileri bir tablosal model projesinde içeri aktarma.  
  
-   Modelde tablolar arasındaki ilişkileri oluşturma ve yönetme.  
  
-   Kullanıcıların kritik iş ölçümlerini analiz etmesine yardımcı olan hesaplanan sütunlar, ölçüler ve Temel Performans Göstergeleri oluşturma.  
  
-   Kullanıcıların işletme ve uygulamaya özgü bakış açıları sağlayarak model verilerine daha kolay göz atmasına yardımcı olan perspektifleri ve hiyerarşileri oluşturma ve yönetme.  
  
-   Tablo verilerini diğer bölümlerden bağımsız olarak işlenebilecek daha küçük mantıksal kısımlara ayıran bölümler oluşturma.  
  
-   Kullanıcı üyeleriyle roller oluşturarak model nesnelerinin ve verilerinin güvenliğini sağlama.  
  
-   Bir **Azure Analysis Services** sunucusuna ya da şirket içi SQL Server 2017 Analysis Services sunucusuna tablosal model dağıtma.  
  
## <a name="prerequisites"></a>Ön koşullar  
Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:  
  
-   Modelinizin dağıtılacağı bir Azure Analysis Services veya SQL Server 2017 Analysis Services örneği. Ücretsiz [Azure Analysis Services denemesi](https://azure.microsoft.com/services/analysis-services/) için kaydolun ve [bir sunucu oluşturun](../analysis-services-create-server.md). Veya kaydolun ve [SQL Server 2017 Community Technology Preview](https://www.microsoft.com/evalcenter/evaluate-sql-server-vnext-ctp)’ı indirin. 

-   [AdventureWorksDW2014 örnek veritabanını](http://go.microsoft.com/fwlink/?LinkID=335807) ile bir SQL Server Veri Ambarı veya Azure SQL Veri Ambarı. Bu örnek veritabanı, bu öğreticinin tamamlanması için gereken verileri içerir. [Ücretsiz SQL Server sürümlerini](https://www.microsoft.com/sql-server/sql-server-downloads) indirin. Veya ücretsiz bir [Azure SQL Veritabanı denemesi](https://azure.microsoft.com/services/sql-database/) için kaydolun. 

    **Önemli:** Örnek veritabanını şirket içi bir SQL Server’a, tablolu modelinizi ise bir Azure Analysis Services sunucusuna yüklerseniz [Şirket içi veri ağ geçidi](../analysis-services-gateway.md) gerekir.

-   [SQL Server Veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx)’nın (SSDT) en son sürümü.

-   [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)’nun (SSMS) en son sürümü.    

-   [Power BI Desktop](https://powerbi.microsoft.com/desktop/) veya Excel gibi bir istemci uygulaması. 

## <a name="scenario"></a>Senaryo  
Bu öğretici, kurgusal bir şirket olan Adventure Works Cycles’ı temel alır. Adventure Works; Kuzey Amerika, Avrupa ve Asya pazarları için bisiklet, parça ve aksesuar üretip pazarlayan büyük, çok uluslu bir üretim şirketidir. Şirketin 500 çalışanı vardır. Ayrıca, Adventure Works pazar ağının tamamında çeşitli bölgesel satış ekipleri görevlendirir. Proje hedefiniz, satış ve pazarlama kullanıcılarının AdventureWorksDW veritabanında İnternet satış verilerini analiz etmesi için tablosal bir model oluşturmaktır.  
  
Öğreticiyi tamamlamak için çeşitli dersleri tamamlamanız gerekir. Her derste çeşitli görevler vardır. Dersin tamamlanması için her görevin sırayla tamamlanması gerekir. Belirli bir dersteyken benzer bir sonucu elde etmek için farklı görevlerle karşılaşabilirsiniz, ancak her görevi tamamlama biçiminiz biraz farklıdır. Bu yöntem, bir görevi gerçekleştirmek için genellikle birden çok yol olduğunu gösterir ve önceki bölümlerde ve görevlerde öğrendiğiniz beceriler konusunda sizi sınar.  
  
Bu kılavuzdaki derslerin amacı, SSDT’deki birçok özelliği kullanarak temel bir tablosal model yazma konusunda size rehberlik etmektir. Her ders bir önceki dersin devamı niteliğinde olduğundan, dersleri sırasıyla tamamlamanız gerekir.
  
Bu öğretici, Azure portalında bir sunucuyu yönetme, SSMS’yi kullanarak bir sunucuyu veya veritabanını yönetme ya da istemci uygulamasını kullanarak model verilerine göz atama konusunda bilgi sağlamaz. 


## <a name="lessons"></a>Dersler  
Bu öğretici aşağıdaki dersleri içerir:  
  
|Ders|Tahmini tamamlanma süresi|  
|----------|------------------------------|  
|[1 - Yeni tablosal model projesi oluşturma](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md)|10 dakika|  
|[2 - Veri alma](../tutorials/aas-lesson-2-get-data.md)|10 dakika|  
|[3 - Tarih Tablosu olarak işaretleme](../tutorials/aas-lesson-3-mark-as-date-table.md)|3 dakika|  
|[4 - İlişki oluşturma](../tutorials/aas-lesson-4-create-relationships.md)|10 dakika|  
|[5 - Hesaplanan sütunlar oluşturma](../tutorials/aas-lesson-5-create-calculated-columns.md)|15 dakika|
|[6 - Ölçü oluşturma](../tutorials/aas-lesson-6-create-measures.md)|30 dakika|  
|[7 - Ana Performans Göstergeleri (KPI) oluşturma](../tutorials/aas-lesson-7-create-key-performance-indicators.md)|15 dakika|  
|[8 - Perspektif oluşturma](../tutorials/aas-lesson-8-create-perspectives.md)|5 dakika|  
|[9 - Hiyerarşi oluşturma](../tutorials/aas-lesson-9-create-hierarchies.md)|20 dakika|  
|[10 - Bölüm oluşturma](../tutorials/aas-lesson-10-create-partitions.md)|15 dakika|  
|[11 - Rol oluşturma](../tutorials/aas-lesson-11-create-roles.md)|15 dakika|  
|[12 - Excel’de çözümleme](../tutorials/aas-lesson-12-analyze-in-excel.md)|5 dakika| 
|[13 - Dağıtma](../tutorials/aas-lesson-13-deploy.md)|5 dakika|  
  
## <a name="supplemental-lessons"></a>Ek dersler  
Bu dersler öğreticinin tamamlanması için gerekli değildir, ancak gelişmiş tablosal model yazma özelliklerinin daha iyi anlaşılmasına yardımcı olabilir.  
  
|Ders|Tahmini tamamlanma süresi|  
|----------|------------------------------|  
|[Ayrıntı Satırları](../tutorials/aas-supplemental-lesson-detail-rows.md)|10 dakika|
|[Dinamik güvenlik](../tutorials/aas-supplemental-lesson-dynamic-security.md)|30 dakika|
|[Düzensiz hiyerarşiler](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)|20 dakika| 

  
## <a name="next-steps"></a>Sonraki adımlar  
Başlamak için bkz. [1. Ders: Yeni Tablosal Model Projesi Oluşturma](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).  
  
  
  

