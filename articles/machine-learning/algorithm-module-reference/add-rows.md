---
title: 'Satırları ekleyin: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: İki veri kümeleri birleştirmek için Azure Machine Learning hizmetinde Add Rows modülünü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ffd693ea3452ef48dc3e05e7bc4a6d3988a487b0
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028763"
---
# <a name="add-rows-module"></a>Satırları Modül Ekle

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

İki veri kümeleri birleştirmek için bu modülü kullanın. Birleştirme ikinci veri kümesine satırlar ilk veri kümesini sonuna eklenir.  
  
Satır bitiştirme, bu gibi senaryolarda kullanışlıdır:  
  
+ Değerlendirme istatistikleri bir dizi oluşturulmasını ve daha kolay raporlama için bir tabloda birleştirin istiyorsunuz.  
  
+ Farklı veri kümeleriyle çalışmaktadır ve son bir veri kümesini oluşturmak için veri kümeleri birleştirmek istiyorsanız.  

## <a name="how-to-use-add-rows"></a>Add Rows kullanma  

İki veri kümelerindeki satırları birleştirmek için satırları tam olarak aynı şemaya sahip olmalıdır. Diğer bir deyişle, aynı sayıda sütun yanı sıra, aynı sütunlarda veri türü.

1.  Sürükleme **Add Rows** modülün denemenizde içine bulabilirsiniz altında **veri dönüştürme**, **Değiştir** kategorisi.

2. Veri kümeleri, iki giriş bağlantı noktalarına bağlayın. Eklemek istediğiniz veri kümesini ikinci (sağdaki) bağlantı noktasına bağlı olmanız gerekir. 
  
3.  Denemeyi çalıştırın. Çıkış veri kümesinde satır sayısını hem giriş veri kümeleri satırlarını toplamına eşit olmalıdır.

    Her iki girişleri için aynı veri eklerseniz **Add Rows** modülü, veri kümesini yineleniyor. 

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 