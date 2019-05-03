---
title: 'Dönüştürme geçerlidir: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Girdi veri kümesi üzerinde önceden hesaplanan bir dönüştürme göre değiştirmek için Azure Machine Learning hizmetinde uygulama dönüştürme modülü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 810f375642af49814049589cb83ad17fea578b13
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028718"
---
# <a name="apply-transformation-module"></a>Dönüştürme modülü Uygula

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Girdi veri kümesi üzerinde önceden hesaplanan bir dönüştürme göre değiştirmek için bu modülü kullanın.  
  
Örneğin, z puanları kullanarak eğitim verilerinizi normalleştirmek için kullanılan **Normalleştir veri** modülü Hesaplandı z-puanı değeri eğitim Puanlama aşamasında de için kullanmak istediğiniz. Azure Machine Learning'de normalleştirme yöntemi bir dönüştürme ve ardından kullanarak kaydedebilirsiniz **uygulamak dönüştürme** Puanlama önce giriş verilerini z puanı uygulamak için.
  
Azure Machine Learning oluşturmak ve ardından özel dönüştürmeler birçok farklı türde uygulamak için destek sağlar. Örneğin, kaydedin ve dönüştürmeler için daha sonra yeniden isteyebilirsiniz:  
  
- Kaldırın veya değiştirin kullanarak eksik değerleri **eksik verileri temizleme**
- Kullanarak verileri Normalleştir **veri normalleştirin**
  

## <a name="how-to-use-apply-transformation"></a>Geçerli dönüştürme kullanma  
  
1. Ekleme **uygulamak dönüştürme** denemenizi modülü. Bu modül altında bulabilirsiniz **Machine Learning**, **puanı** kategorisi. 
  
2. Girdi olarak kullanmak için mevcut bir dönüştürme bulun.  Önceden kaydedilen dönüşümleri bulunabilir **dönüştüren** sol gezinti bölmesinde grubu.  
  
   
  
3. Dönüştürmek istediğiniz veri kümesine bağlanın. Veri kümesi, tam olarak aynı şemaya (sütunları, sütun adları, veri türleri sayısı) dönüştürme ilk tasarlanmış veri kümesi olması gerekir.  
  
4. Başka bir parametre dönüştürme tanımlarken, tüm özelleştirme bitti beri ayarlamanız gerekir.  
  
5. Yeni veri kümesine bir dönüştürme uygulamak için denemeyi çalıştırın.  

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 