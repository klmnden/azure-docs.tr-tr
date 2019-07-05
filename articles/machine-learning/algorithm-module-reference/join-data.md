---
title: 'Veri Katıl: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmetindeki verileri birleştirme modülü birleştirme, veri kümeleri birleştirmek için kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: peterclu
ms.date: 06/01/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 7e814f5ea4bd47ceb0697e860c946039ce39ae1f
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67518020"
---
# <a name="join-data"></a>Verileri birleştirme

Bu makalede nasıl kullanılacağını **verileri birleştirme** modülünde bir veritabanı stili birleştirme işlemi kullanarak iki veri kümeleri birleştirmek için Azure Machine Learning hizmeti görsel arabirim.  

## <a name="how-to-configure-join-data"></a>Veri birleştirme yapılandırma

İki veri kümesi üzerinde bir katılma işlemi için bunlar bir anahtar sütunu ile ilişkili olmalıdır. Bileşik anahtarlar kullanarak birden çok sütun da desteklenir. 

1. Eklemek istediğiniz birleştirebilir ve ardından veri kümeleri **veri birleştirme** denemenizi modüle. 

    Modülünde bulabilirsiniz **veri dönüştürme** kategori altında **işleme**.

1. Veri kümelerine bağlanma **veri birleştirme** modülü. 
 
1. Seçin **Sütun seçiciyi Başlat** anahtar sütunları seçmek için. Sol ve sağ girişleri için sütun seçmeyi unutmayın.

    Tek bir anahtarı:

    Her iki girişleri için tek bir anahtar sütunu seçin.
    
    Bir bileşik anahtarı için:

    Tüm anahtar sütunlarını sol giriş ve aynı sırada sağ giriş seçin. **Verileri birleştirme** tüm anahtar sütunlarını eşleştiğinde, modül tabloları birleştirme. Bu seçeneği işaretleyin **izin yinelemeleri ve sütun sırasını seçimdeki korumak** sütun sırasını özgün tablo aynı değilse. 

    ![Sütun Seçici](media/module/join-data-column-selector.png)


1. Seçin **eşleşen servis talebi** , bir metin sütunu birleşimde büyük/küçük harfe duyarlılık korumak istiyorsanız seçeneği. 
   
1. Kullanım **birleştirme türü** veri kümelerinin nasıl birleştirileceğini belirtmek için açılır liste.  
  
    * **İç birleşim**: Bir *INNER JOIN* en yaygın birleştirme işlemi. Yalnızca anahtar sütunların değerlerinin eşleştiğinde birleşik satırları döndürür.  
  
    * **Sol dış birleştirme**: A *left outer JOIN* katılmış sol tablodaki satırları döndürür. Sol tablosunda bir satıra sağ tablodaki eşleşen hiçbir satır olduğunda, döndürülen satır sağ tablodan gelen tüm sütunlar için eksik değerler içeriyor. Bir değiştirme değeri eksik değerleri için de belirtebilirsiniz.  
  
    * **Tam dış birleşim**: A *tam dış birleşim* sol tablodan tüm satırları döndürür (**table1**) ve sağ tablodan (**table2**).  
  
         Her eşleşen hiçbir satır diğer olan ya da tablo satır için sonuç eksik değerleri içeren bir satır içerir.  
  
    * **Noktalı birleştirme sol**: A *yarı birleştirme sol* anahtar sütunların değerlerinin eşleştiğinde sol tablodan yalnızca değerlerini döndürür.  

1. Seçeneği için **birleştirilmiş tabloda sağa anahtar sütunları tutun**:

    * Giriş her iki tablonun anahtarlarını görüntülemek için bu seçeneği belirleyin.
    * Yalnızca dönün, sol girdiden anahtar sütunları seçimini kaldırın.

1. Denemeyi çalıştırın veya verileri birleştirme modülü seçin ve seçili **çalıştırma seçili** katılma işlemi için.

1. Sonuçları görüntülemek için sağ **veri birleştirme** > **sonuçları veri kümesi** > **Görselleştir**.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 