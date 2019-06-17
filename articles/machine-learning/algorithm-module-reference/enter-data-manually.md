---
title: 'Verileri el ile girin: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Verileri el ile girin modül değerlerini yazarak küçük bir veri kümesini oluşturmak için Azure Machine Learning hizmetinde kullanmayı öğrenin. Veri kümesi, birden fazla sütuna sahip olabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ee15b6fb7160ece907d55e790b0ae38ee458ab96
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028613"
---
# <a name="enter-data-manually-module"></a>Modül verileri el ile girin

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Değerlerini yazarak küçük bir veri kümesini oluşturmak için bu modülü kullanın. Veri kümesi, birden fazla sütuna sahip olabilir.
  
Bu modül, bu gibi senaryolarda yararlı olabilir:  
  
- Test etmek için küçük bir dizi oluşturma  
  
- Etiketlerin kısa listesini oluşturma
  
- Bir veri kümesi eklemek için sütun adlarının bir listesini yazın

## <a name="enter-data-manually"></a>Verileri El ile Girme 
  
1.  Ekleme [verileri el ile girin](./enter-data-manually.md) denemenizi modülü. Bu modülde bulabilirsiniz **veri giriş ve çıkış** Azure Machine learning'de kategorisi. 
  
2.  İçin **DataFormat**, aşağıdaki seçeneklerden birini belirleyin. Bu seçenekler, sağladığınız verileri nasıl ayrıştırılması gerekip belirler. Her biçim gereksinimlerini önemli ölçüde farklılık gösterir, bu nedenle ilgili konular okuduğunuzdan emin olun.  
  
    -   **ARFF'YE**. Weka tarafından kullanılan öznitelik-ilişki dosyası biçimi.   
  
    -   **CSV**. Virgülle ayrılmış değerler biçiminde. Daha fazla bilgi için [CSV'ye Dönüştür](./convert-to-csv.md).  
  
    -   **SVMLight**. Vowpal Wabbit ve diğer makine öğrenmesi çerçeveleri tarafından kullanılan biçimi.  
  
    -   **TSV**. Sekmeyle ayrılmış değerler biçiminde.

     Bir biçim seçin ve biçim belirtimleri karşılayan verileri sağlamazsanız, bir çalışma zamanı hatası oluşur.
  
3.  İçine tıklayın **veri** metin kutusu verileri girmeye başlayın. Şu biçimlerden özel dikkat gerektiren:  
  
    - **CSV**:  Birden çok sütun oluşturmak için virgülle ayrılmış metni yapıştırın veya birden çok sütun alanları arasında virgül kullanarak yazın.
  
        Seçerseniz **HasHeader** seçeneği değerleri ilk satırı sütun başlığı olarak kullanabilirsiniz.  
  
        Bu seçenek, sütun adları, Sütun1, Sütun2 ve benzeri işaretini kaldırırsanız, kullanılır. Ekleyebilir veya sütunları daha sonra kullanarak adları [meta verileri Düzenle](./edit-metadata.md).  
  
    - **TSV**: Birden çok sütun oluşturmak için sekmeyle ayrılmış metnini yapıştırın veya alanlar arasında sekmeleri kullanarak birden çok sütun yazın.  
  
        Seçerseniz **HasHeader** seçeneği değerleri ilk satırı sütun başlığı olarak kullanabilirsiniz.  
  
        Bu seçenek, sütun adları, Sütun1, Sütun2 ve benzeri işaretini kaldırırsanız, kullanılır. Ekleyebilir veya sütunları daha sonra kullanarak adları [meta verileri Düzenle](./edit-metadata.md).  
  
    -   **ARFF'YE**:  Var olan bir ARFF'ye biçimi dosyaya yapıştırın. Değerleri doğrudan yazıyorsanız, isteğe bağlı üst bilgi ve gerekli öznitelik alanları veri başında eklemeyi unutmayın. 
    
        Örneğin, aşağıdaki üst bilgi ve öznitelik satırları basit bir listesine eklenemedi. Sütun başlığı olacaktır `SampleText`.
    
        ```text
        % Title: SampleText.ARFF  
        % Source: Enter Data module  
        @ATTRIBUTE SampleText STRING  
        @DATA  
        \<type first data row here>  
        ```

    -   **SVMLight**: Yazın veya SVMLight biçimini kullanarak değerleri yapıştırın.  
  
        Örneğin, aşağıdaki örnek, ilk birkaç satırı Tansiyon Bağış kümesinin SVMight biçimde temsil eder:  
  
        ```text  
        # features are [Recency], [Frequency], [Monetary], [Time]  
        1 1:2 2:50 3:12500 4:98   
        1 1:0 2:13 3:3250 4:28   
        ```  
  
        Çalıştırdığınızda [verileri el ile girin](./enter-data-manually.md) modülü, bu satırlar sütun dataset nesnesine dönüştürülür ve dizin aşağıdaki gibi değerleri:  
  
        |Sütun1|Sütun2|Col3|Sütun4|Etiketler|  
        |-|-|-|-|-|  
        |0.00016|0.004|0.999961|0.00784|1|  
        |0|0.004|0.999955|0.008615|1|  
  
4.  Yeni bir satıra başlatmak için her satır sonra ENTER'a basın.  
  
     **Son satır sonra ENTER tuşuna basın emin olun.** 
     
     ENTER tuşuna basın, birden çok eklemek için birden çok kez satırları sondaki boş, son boş satır kırpılmış kaldırılır, ancak diğer boş satır eksik değer olarak kabul edilir.  
  
     Eksik değerler satırları oluşturursanız, her zaman bunları daha sonra filtreleyebilirsiniz.  
  
5.  Modül sağ tıklayıp **Seçileni Çalıştır** kullanarak verileri ayrıştırır ve çalışma alanınıza bir veri kümesi olarak yüklemek için.  
  
     Veri kümesini görüntülemek için çıkış bağlantı noktasına tıklayın ve seçin **Görselleştir**.  
## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 