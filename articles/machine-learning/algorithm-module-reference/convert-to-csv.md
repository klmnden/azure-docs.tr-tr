---
title: "CSV'ye Dönüştür: Modül başvurusu"
titleSuffix: Azure Machine Learning service
description: CSV modülüne Dönüştür Azure Machine Learning hizmetinde bir veri kümesi indirilen, dışarı aktarılan veya R veya Python betik modülleri ile paylaşılan bir CSV biçime dönüştürmek için nasıl kullanılacağını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 8b8b6758cc2df7a092ce36e9507f84ac534d0e3d
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028733"
---
# <a name="convert-to-csv-module"></a>CSV modülüne Dönüştür

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

İndirilen, dışarı aktarılan veya R veya Python betik modülleri ile paylaşılan bir CSV biçiminde bir veri kümesini dönüştürmek için bu modülü kullanın.

### <a name="more-about-the-csv-format"></a>CSV biçimi hakkında daha fazla bilgi 

"Virgülle ayrılmış değerler için" anlamına gelir, CSV biçimi, machine learning araçları çok sayıda dış tarafından kullanılan bir dosya biçimidir. CSV, R veya Python gibi açık kaynaklı dilleriyle çalışırken yaygın bir değişim biçimi.

Azure Machine learning'de iş çoğunu olsa bile, olduğunda, Veri kümenizi dış araçları kullanmak için CSV dönüştürmek için kullanışlı bulabilirsiniz zamanlar vardır. Örneğin:

+ Excel ile açmak için CSV dosyası indirin veya ilişkisel veritabanına aktarmak.  
+ Bulut depolama ve görselleştirme oluşturmak için Power BI'dan bağlanmak için CSV dosyasını kaydedin.  
+ CSV biçimi, R ve Python kullanmak için veri hazırlamak için kullanın. Yalnızca doğrudan Python ya da bir Jupyter not defteri verilere erişmek için gereken kodu oluşturmak için bir modülün çıkışına sağ tıklayın. 

Bir veri kümesi CSV'ye Dönüştür dosyayı Azure ML çalışma alanınızda kaydedilir. Açıp dosyayı doğrudan kullanmak için bir Azure depolama yardımcı programını kullanabilirsiniz veya modülün çıkış sağ tıklayın ve CSV dosyasını bilgisayarınıza indirin veya R veya Python kodu kullanın.  

## <a name="how-to-configure-convert-to-csv"></a>CSV Dönüştür yapılandırma

1.  Ekleme [CSV'ye Dönüştür](./convert-to-csv.md) denemenizi modülü. Bu modülde bulabilirsiniz **veri biçim dönüştürmelerini** arabirimi grubu. 

2. Bu, bir veri kümesi veren tüm modülüne bağlayın.   
  
3.  Denemeyi çalıştırın.

### <a name="results"></a>Sonuçlar
  

Çıkışı çift [CSV'ye Dönüştür](./convert-to-csv.md), aşağıdaki seçeneklerden birini seçin.  

 + **Sonuç veri kümesi -> indirme**: Hemen bir yerel klasöre kaydedebilirsiniz CSV biçiminde verilerin bir kopyasını açılır. Bir klasör belirtmezseniz varsayılan dosya adı uygulanır ve CSV dosyası yerel kaydedilir **indirir** kitaplığı.


 + **Sonuç veri kümesini veri kümesi olarak Kaydet ->**: CSV dosyası ayrı bir veri kümesi Azure ML çalışma alanına kaydeder.

 + **Veri erişim kodu**: Azure ML iki Python kullanarak ya da r kullanarak verilere erişmek kod oluşturur Verilere erişmek için uygulamanıza kod parçacığını kopyalayın. (*Veri erişim kodu oluşturmak gelen yakında.* )

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 