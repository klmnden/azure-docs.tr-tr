---
title: Özel bir model - Form tanıyıcı için bir eğitim veri kümesi oluşturma
titleSuffix: Azure Cognitive Services
description: Bir Form tanıyıcı modeli eğitmek için eğitim veri kümeniz iyileştirilmesini sağlamak öğrenin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: pafarley
ms.openlocfilehash: 611d5f7983c61fab12c55a46fedf35a3c420c4c8
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454823"
---
# <a name="build-a-training-data-set-for-a-custom-model"></a>Özel bir model için bir eğitim veri kümesi oluşturma

Form tanıyıcı özel modeli kullandığınız zaman için sektöre özel formlarınızı modeli eğitmek kendi eğitim verilerini sağlar. Bir model ile doldurulmuş beş forms ya da boş bir form eğitebilirsiniz ("dosya adı boş" sözcüğünü eklemediğinizden) artı doldurulmuş iki tür. Eğitim veri kümeniz için boş bir form ekleme ile eğitmek için yeterli doldurulmuş forms olsa bile, modelin doğruluğunu geliştirebilir.

## <a name="training-data-tips"></a>Eğitim veri ipuçları

Eğitim için optimize edilmiş bir veri kümesini kullanmak önemlidir. Kullanım emin olmak için aşağıdaki ipuçlarını en iyi sonuçları almak [modeli eğitme](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/TrainCustomModel) işlemi:

* Mümkünse, metin tabanlı PDF belgeleri görüntü tabanlı belge yerine kullanın. PDF taranmış görüntü olarak işlenir.
* Kullanılabilir bunları varsa boş bir form ve iki doldurulmuş biçimini kullanın.
* Formlarda doldurulmuş doldurulmuş alanlarının tüm örnekleri kullanın.
* Her alandaki farklı değerleri olan form kullanın.
* Form görüntülerinizi daha düşük kaliteli, daha büyük bir veri kümesi (örneğin, 10-15 görüntüleri) kullanın.

## <a name="general-input-requirements"></a>Genel bir giriş gereksinimleri

Eğitim veri kümesi, ayrıca tüm Form tanıyıcı içerik için giriş gereksinimleri uyduğundan emin olun.
[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bir eğitim veri kümesi oluşturmak öğrendiniz, özel bir formu tanıyıcı modeli eğitmek ve formlarınızı kullanmaya başlamak için bir Hızlı Başlangıç'ı izleyin.

* [Hızlı Başlangıç: Bir modeli eğitmek ve cURL kullanarak form verileri ayıklayın](./quickstarts/curl-train-extract.md)
* [Hızlı Başlangıç: Bir modeli eğitmek ve Python ile REST API kullanarak form verilerini ayıklama](./quickstarts/python-train-extract.md)

