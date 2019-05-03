---
title: 'Sütunları ekleyin: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Sütun Ekle modülü, iki veri kümeleri birleştirmek için Azure Machine Learning hizmetinde kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f1e087e97007c6ba271651a9791c7c3b38a9b9b7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029363"
---
# <a name="add-columns-module"></a>Sütunları Modül Ekle

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

İki veri kümeleri birleştirmek için bu modülü kullanın. Tek bir veri kümesi oluşturmak için giriş olarak belirttiğiniz iki veri kümelerindeki tüm sütunları birleştirin. İkiden fazla veri kümeleri birleştirmek gerekiyorsa, çeşitli örneklerini kullanın **Sütun Ekle**.



## <a name="how-to-configure-add-columns"></a>Sütun Ekle yapılandırma
1. Ekle **Sütun Ekle** denemenizi modülü.

2. Birleştirmek istediğiniz iki veri kümesi bağlanın. İkiden fazla veri kümeleri birleştirmek istiyorsanız, çeşitli birleşimlerini araya zincirleyebilirsiniz **Sütun Ekle**.

    - Farklı sayıda satıra sahip iki sütun birleştirmek mümkündür. Çıktı veri kümesi, daha küçük kaynak sütunundaki her satır için eksik değerleri ile doldurulur.

    - Sütunları eklemek için tek tek seçemezsiniz. Kullandığınızda, her veri kümesinden alınan tüm sütunları bitiştirilir **Sütun Ekle**. Yalnızca bir sütun alt kümesi eklemek istiyorsanız, bu nedenle, istediğiniz sütunları içeren bir veri kümesini oluşturmak için veri kümesindeki sütunları seçme kullanın.

3. Denemeyi çalıştırın.

### <a name="results"></a>Sonuçlar
Denemeyi çalıştırdıktan sonra:

- Yeni veri kümesini ilk satırlarını görmek için çıkışı sağ **Sütun Ekle** ve görsel öğe seçin.

Yeni veri kümesinde sütun sayısı iki giriş veri kümeleri sütunlarının toplamına eşittir.

Giriş veri kümelerinde aynı ada sahip iki sütun varsa sütunun adı için sayısal bir sonek eklenir. Örneğin, TargetOutcome adlı bir sütunun iki örneği varsa, sol sütunda yeniden adlandırılması TargetOutcome_1 ve sağ sütunda TargetOutcome_2 yeniden adlandırıldı.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 