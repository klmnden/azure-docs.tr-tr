---
title: "Azure Analysis Services öğreticisi 1. ders: Yeni tablosal model projesi oluşturma | Microsoft Docs"
description: "Yeni bir Azure Analysis Services öğretici projesinin nasıl oluşturulacağını açıklar."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 06/01/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 40aac182af22d03c4cff535fd8c87b29ecae376a
ms.contentlocale: tr-tr
ms.lasthandoff: 06/03/2017

---
# <a name="lesson-1-create-a-new-tabular-model-project"></a>1. Ders: Yeni bir tablosal model projesi oluşturma

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Bu derste, SQL Server Veri Araçları’nı (SSDT) kullanarak 1400 uyumluluk düzeyinde yeni bir tablosal model projesi oluşturursunuz. Yeni projeniz oluşturulduktan sonra, veri eklemeye ve modelinizi yazmaya başlayabilirsiniz. Bu ders, SSDT’de tablosal model yazma ortamı hakkında temel bilgiler de sağlamaktadır.  
  
Bu dersin tahmini tamamlanma süresi: **10 dakika**  
  
## <a name="prerequisites"></a>Ön koşullar  
Bu konu, tablosal model yazma öğreticisindeki ilk derstir. Bu dersi tamamlamak için karşılamanız gereken çeşitli ön koşullar vardır. Daha fazla bilgi edinmek için bkz. [Azure Analysis Services - Adventure Works Öğreticisi](../tutorials/aas-adventure-works-tutorial.md).  
  
## <a name="create-a-new-tabular-model-project"></a>Yeni tablosal model projesi oluşturma  
  
#### <a name="to-create-a-new-tabular-model-project"></a>Yeni bir tablosal model projesi oluşturmak için  
  
1.  SSDT’deki **Dosya** menüsünde **Yeni** > **Proje**’ye tıklayın.  
  
2.  **Yeni Proje** iletişim kutusunda, **Yüklü** > **İş Zekası** > **Analysis Services** seçeneğini genişletin ve **Analysis Services Tablosal Proje**’ye tıklayın.  
  
3.  **Ad** alanına **AW İnternet Satışları** yazın ve proje dosyaları için bir konum belirtin.  
  
    Varsayılan olarak **Çözüm Adı** proje adıyla aynıdır, ancak farklı bir çözüm adı yazabilirsiniz.  
  
4.  **Tamam** düğmesine tıklayın.  
  
5.  **Tablosal model tasarımcısı** iletişim kutusunda **Tümleşik çalışma alanı**’nı seçin.  
  
    Model yazıldığı sırada, çalışma alanı projeyle aynı ada sahip bir tablosal model veritabanı barındırır. Çalışma alanının tümleşik olması sayesinde SSDT yerleşik bir örneği kullandığından, yalnızca model yazmak için ayrı bir Analysis Services sunucu örneği yükleme gereksinimi ortadan kalkar.
      
6.  **Uyumluluk düzeyi**’nde **SQL Server 2017 / Azure Analysis Services (1400)** seçeneğini belirleyin.   
 
    ![aas-lesson1-tmd](../tutorials/media/aas-lesson1-tmd.png)
      
    Uyumluluk düzeyi liste kutusunda SQL Server 2017 / Azure Analysis Services (1400) seçeneğini görmüyorsanız SQL Server Veri Araçları’nın son sürümünü kullanmıyorsunuz demektir. Son sürümü edinmek için bkz. [SQL Server Veri Araçları’nı yükleme](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  
      
  
## <a name="understanding-the-ssdt-tabular-model-authoring-environment"></a>SSDT tablosal model yazma ortamını anlama  
Artık yeni bir tablosal model projesi oluşturduğunuza göre, SSDT’deki tablosal model yazma ortamını keşfetmeye biraz zaman ayırabiliriz.  
  
Projeniz oluşturulduktan sonra SSDT’de açılır. Sağ taraftaki **Tablosal Model Gezgini**’nde, modelinizdeki nesnelerin ağaç görünümünü görürsünüz. Henüz içeri veri aktarmadığınızdan klasörler boştur. Bir nesne klasörüne sağ tıklayarak menü çubuğuna benzer şekilde çeşitli eylemler gerçekleştirebilirsiniz. Bu öğreticide ilerledikçe, model projenizdeki farklı nesnelere gitmek için Tablosal Model Gezgini’ni kullanırsınız.

![aas-lesson1-tme](../tutorials/media/aas-lesson1-tme.png)

**Çözüm Gezgini** sekmesine tıklayın. Burada **Model.bim** dosyanızı görürsünüz. Solda tasarımcı penceresini (Model.bim sekmesini içeren boş pencere) görmüyorsanız, **Çözüm Gezgini**’ndeki **AW İnternet Satışları Projesi** bölümünde **Model.bim** dosyasına çift tıklayın. Model.bim dosyası, model projenizin meta verilerini içerir. 

![aas-lesson1-se](../tutorials/media/aas-lesson1-se.png)
  
**Model.bim** öğesine tıklayın. **Özellikler** penceresinde model özelliklerini görürsünüz ve bunların en önemlisi **DirectQuery Modu** özelliğidir. Bu özellik, modelin Bellek İçi modda (Kapalı) mı yoksa DirectQuery modunda (Açık) mı dağıtıldığını belirtir. Bu öğretici için modelinizi Bellek İçi modunda yazar ve dağıtırsınız.

![aas-lesson1-properties](../tutorials/media/aas-lesson1-properties.png)
  
Model projesi oluşturduğunuz sırada, bazı model özellikleri, **Araçlar** menüsü > **Seçenekler** iletişim kutusundan belirtilebilen Veri Modelleme ayarlarına göre otomatik olarak ayarlanır. Veri Yedekleme, Çalışma Alanı Tutma ve Çalışma Alanı Sunucusu özellikleri, çalışma alanı veritabanının (model yazma veritabanınız) nasıl ve nerede yedekleneceğini, bellek içinde korunacağını ve derleneceğini belirtir. Gerekirse bu ayarları daha sonra değiştirebilirsiniz, ancak şimdilik bu özellikleri olduğu gibi bırakın.  

**Çözüm Gezgini**’nde **AW İnternet Satışları**’na (proje) sağ tıklayıp **Özellikler**’e tıklayın. **AW İnternet Satışları Özellik Sayfaları** iletişim kutusu açılır. Bu özelliklerden bazılarını, daha sonra modelinizi dağıtırken ayarlarsınız.  
  
SSDT’yi yüklediğinizde Visual Studio ortamına birkaç yeni menü öğesi eklendi. **Model** menüsüne tıklayın. Buradan içeri veri aktarabilir, çalışma alanı verilerini yenileyebilir, Excel’de modelinize göz atabilir, perspektifler ve roller oluşturabilir, model görünümünü seçebilir ve hesaplama seçeneklerini ayarlayabilirsiniz. **Tablo** menüsüne tıklayın. Buradan ilişkiler oluşturup bunları yönetebilir, tarih tablosu ayarlarını belirtebilir, bölümler oluşturabilir ve tablo özelliklerini düzenleyebilirsiniz. **Sütun** menüsüne tıklarsanız bir tabloya sütun ekleyebilir ve tablodaki sütunları silebilir, sütunları dondurabilir ve sıralama düzenini belirtebilirsiniz. SSDT tarafından çubuğa da birkaç düğme eklenir. Bu düğmelerin en kullanışlısı, seçilen bir sütuna yönelik standart bir toplama ölçüsü oluşturmak için kullanılabilen Otomatik Toplam özelliğidir. Diğer araç çubuğu düğmeleri, sık kullanılan özelliklere ve komutlara hızlı erişim sağlar.  
  
Tablosal modeller yazmaya özgü bu çeşitli özelliklerin iletişim kutularını ve konumlarını keşfedin. Bazı öğeler henüz etkin olmasa da tablosal model yazma ortamı hakkında fikir edinebilirsiniz.  
  

## <a name="whats-next"></a>Sırada ne var?
[2. Ders: Verileri alma](../tutorials/aas-lesson-2-get-data.md).

  
  
  

