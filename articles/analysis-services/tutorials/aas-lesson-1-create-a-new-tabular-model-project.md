---
title: 'Azure Analysis Services Öğreticisi Ders 1: Yeni bir tablosal model projesi oluşturma | Microsoft Docs'
description: Yeni bir Azure Analysis Services öğretici projesinin nasıl oluşturulacağını açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 3291721847d34b0fa9a6259bfeb6ec6fa06ed2b5
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188020"
---
# <a name="create-a-tabular-model-project"></a>Tablosal model projesi oluşturma

Bu derste, Visual Studio Analiz Hizmetleri projeleri veya SQL Server veri Araçları (SSDT) kullanarak 1400 uyumluluk düzeyinde yeni bir tablosal model projesi oluşturmak için kullanın. Yeni projeniz oluşturulduktan sonra, veri eklemeye ve modelinizi yazmaya başlayabilirsiniz. Bu ders, Visual Studio’da tablosal model yazma ortamı hakkında temel bilgiler de sağlamaktadır.  
  
Bu dersi tamamlamak için tahmini süre: **10 dakika**  
  
## <a name="prerequisites"></a>Önkoşullar  
Bu konu, tablosal model yazma öğreticisindeki ilk derstir. Bu dersi tamamlamak için karşılamanız gereken çeşitli ön koşullar vardır. Daha fazla bilgi edinmek için bkz. [Azure Analysis Services - Adventure Works Öğreticisi](../tutorials/aas-adventure-works-tutorial.md).  
  
## <a name="create-a-new-tabular-model-project"></a>Yeni tablosal model projesi oluşturma  
  
#### <a name="to-create-a-new-tabular-model-project"></a>Yeni bir tablosal model projesi oluşturmak için  
  
1.  Visual Studio’daki **Dosya** menüsünde **Yeni** > **Proje**’ye tıklayın.  
  
2.  **Yeni Proje** iletişim kutusunda, **Yüklü** > **İş Zekası** > **Analysis Services** seçeneğini genişletin ve **Analysis Services Tablosal Proje**’ye tıklayın.  
  
3.  **Ad** alanına **AW İnternet Satışları** yazın ve proje dosyaları için bir konum belirtin.  
  
    Varsayılan olarak **Çözüm Adı** proje adıyla aynıdır, ancak farklı bir çözüm adı yazabilirsiniz.  
  
4.  **Tamam** düğmesine tıklayın.  
  
5.  **Tablosal model tasarımcısı** iletişim kutusunda **Tümleşik çalışma alanı**’nı seçin.  
  
    Model yazıldığı sırada, çalışma alanı projeyle aynı ada sahip bir tablosal model veritabanı barındırır. Çalışma alanının tümleşik olması sayesinde Visual Studio yerleşik bir örneği kullandığından, yalnızca model yazmak için ayrı bir Analysis Services sunucu örneği yükleme gereksinimi ortadan kalkar.
      
6.  **Uyumluluk düzeyi**’nde **SQL Server 2017 / Azure Analysis Services (1400)** seçeneğini belirleyin.   
 
    ![aas-lesson1-tmd](../tutorials/media/aas-lesson1-tmd.png)
      
    Uyumluluk düzeyi liste kutusunda SQL Server 2017 / Azure Analysis Services (1400) seçeneğini görmüyorsanız SQL Server Veri Araçları’nın son sürümünü kullanmıyorsunuz demektir. Son sürümü edinmek için bkz. [SQL Server Veri Araçları’nı yükleme](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  
      
  
## <a name="understanding-the-visual-studio-tabular-model-authoring-environment"></a>Visual Studio tablosal model yazma ortamını anlama  
Artık yeni bir tablosal model projesi oluşturduğunuza göre, Visual Studio’daki tablosal model yazma ortamını keşfetmeye biraz zaman ayırabiliriz.  
  
Projeniz oluşturulduktan sonra Visual Studio’da açılır. Sağ taraftaki **Tablosal Model Gezgini**’nde, modelinizdeki nesnelerin ağaç görünümünü görürsünüz. Henüz içeri veri aktarmadığınızdan klasörler boştur. Bir nesne klasörüne sağ tıklayarak menü çubuğuna benzer şekilde çeşitli eylemler gerçekleştirebilirsiniz. Bu öğreticide ilerledikçe, model projenizdeki farklı nesnelere gitmek için Tablosal Model Gezgini’ni kullanırsınız.

![aas-lesson1-tme](../tutorials/media/aas-lesson1-tme.png)

**Çözüm Gezgini** sekmesine tıklayın. Burada **Model.bim** dosyanızı görürsünüz. Solda tasarımcı penceresini (Model.bim sekmesini içeren boş pencere) görmüyorsanız, **Çözüm Gezgini**’ndeki **AW İnternet Satışları Projesi** bölümünde **Model.bim** dosyasına çift tıklayın. Model.bim dosyası, model projenizin meta verilerini içerir. 

![aas-lesson1-se](../tutorials/media/aas-lesson1-se.png)
  
**Model.bim** öğesine tıklayın. **Özellikler** penceresinde model özelliklerini görürsünüz ve bunların en önemlisi **DirectQuery Modu** özelliğidir. Bu özellik, modelin Bellek İçi modda (Kapalı) mı yoksa DirectQuery modunda (Açık) mı dağıtıldığını belirtir. Bu öğretici için modelinizi Bellek İçi modunda yazar ve dağıtırsınız.

![aas-lesson1-properties](../tutorials/media/aas-lesson1-properties.png)
  
Model projesi oluşturduğunuz sırada, bazı model özellikleri, **Araçlar** menüsü > **Seçenekler** iletişim kutusundan belirtilebilen Veri Modelleme ayarlarına göre otomatik olarak ayarlanır. Veri Yedekleme, Çalışma Alanı Tutma ve Çalışma Alanı Sunucusu özellikleri, çalışma alanı veritabanının (model yazma veritabanınız) nasıl ve nerede yedekleneceğini, bellek içinde korunacağını ve derleneceğini belirtir. Gerekirse bu ayarları daha sonra değiştirebilirsiniz, ancak şimdilik bu özellikleri olduğu gibi bırakın.  

**Çözüm Gezgini**’nde **AW İnternet Satışları**’na (proje) sağ tıklayıp **Özellikler**’e tıklayın. **AW İnternet Satışları Özellik Sayfaları** iletişim kutusu açılır. Bu özelliklerden bazılarını, daha sonra modelinizi dağıtırken ayarlarsınız.  
  
Analysis Services projeleri veya SSDT'yi yüklediğinizde Visual Studio ortamına birkaç yeni menü öğesi eklendi. **Model** menüsüne tıklayın. Buradan içeri veri aktarabilir, çalışma alanı verilerini yenileyebilir, Excel’de modelinize göz atabilir, perspektifler ve roller oluşturabilir, model görünümünü seçebilir ve hesaplama seçeneklerini ayarlayabilirsiniz. **Tablo** menüsüne tıklayın. Buradan ilişkiler oluşturup bunları yönetebilir, tarih tablosu ayarlarını belirtebilir, bölümler oluşturabilir ve tablo özelliklerini düzenleyebilirsiniz. **Sütun** menüsüne tıklarsanız bir tabloya sütun ekleyebilir ve tablodaki sütunları silebilir, sütunları dondurabilir ve sıralama düzenini belirtebilirsiniz. Visual Studio bazı düğmeler çubuğunu da ekler. Bu düğmelerin en kullanışlısı, seçilen bir sütuna yönelik standart bir toplama ölçüsü oluşturmak için kullanılabilen Otomatik Toplam özelliğidir. Diğer araç çubuğu düğmeleri, sık kullanılan özelliklere ve komutlara hızlı erişim sağlar.  
  
Tablosal modeller yazmaya özgü bu çeşitli özelliklerin iletişim kutularını ve konumlarını keşfedin. Bazı öğeler henüz etkin olmasa da tablosal model yazma ortamı hakkında fikir edinebilirsiniz.  
  

## <a name="whats-next"></a>Sırada ne var?
[2. Ders: Veri alma](../tutorials/aas-lesson-2-get-data.md).

  
  
  
