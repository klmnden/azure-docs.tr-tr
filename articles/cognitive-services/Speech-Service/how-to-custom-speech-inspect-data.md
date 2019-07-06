---
title: Özel konuşma tanıma - konuşma Hizmetleri veri kalitesini denetleme
titlesuffix: Azure Cognitive Services
description: Özel konuşma tanıma işleminin sonucu ilgili ses verilerle karşılaştırarak modelin tanıma kalite görsel olarak inceleyin olanak tanıyan araçlar sağlar. Özel konuşma tanıma Portalı'ndan yüklenen ses kayıttan yürütme ve sağlanan tanıma işleminin sonucu doğru olup olmadığını belirler.  Bu aracı, herhangi bir ses veri konuşmaların gerek kalmadan Microsoft'un temel konuşma metin modeli veya özel bir eğitilen model kalitesini hızlı bir şekilde incelemek sağlar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 900d9b032b4ed121589d904a8ad18059b3283661
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603119"
---
# <a name="inspect-custom-speech-data"></a>Özel konuşma verileri İnceleme

> [!NOTE]
> Bu sayfayı okuduğunuz varsayılır [hazırlama test verileri için özel konuşma](how-to-custom-speech-test-data.md) ve inceleme için bir veri kümesi.

Özel konuşma tanıma işleminin sonucu ilgili ses verilerle karşılaştırarak modelin tanıma kalite görsel olarak inceleyin olanak tanıyan araçlar sağlar. Özel konuşma tanıma Portalı'ndan yüklenen ses kayıttan yürütme ve sağlanan tanıma işleminin sonucu doğru olup olmadığını belirler. Bu aracı, herhangi bir ses veri konuşmaların gerek kalmadan Microsoft'un temel konuşma metin modeli veya özel bir eğitilen model kalitesini hızlı bir şekilde incelemek sağlar.

Bu belgede daha önce yüklenmiş eğitim verilerini kullanarak bir model kalitesini görsel olarak inceleyin öğreneceksiniz.

Bu sayfada, Microsoft'un temel konuşma metin model ve/veya size eğittiğimize özel bir model kalitesini görsel olarak inceleyin öğreneceksiniz. İçin yüklediğiniz verileri kullanacağınız **veri** test etmek için sekmesinde.

## <a name="create-a-test"></a>Test oluşturma

Bir test oluşturmak için aşağıdaki yönergeleri izleyin:

1. Gidin **konuşma metin > özel konuşma > test**.
2. Tıklayın **Test Ekle**.
3. Seçin **kalite (yalnızca ses veriler) incelemek**. Test açıklaması, bir ad verin ve ses kümenizi seçin.
4. Test etmek istediğiniz en fazla iki modeli seçin.
5. **Oluştur**’a tıklayın.

Bir test başarıyla oluşturulduktan sonra modeli yan yana karşılaştırabilirsiniz.

## <a name="side-by-side-model-comparisons"></a>Yan yana modeli karşılaştırmaları

Test durumu olduğunda *başarılı*, testin ayrıntılarını görmek için test öğesi adına tıklayın. Bu Ayrıntıları sayfası, tüm konuşma transkripsiyonu gönderilen veri kümesinden yanı sıra iki modeli tanıma sonuçları gösteren veri kümenizde, listeler.

Yan yana karşılaştırmayı denetlemek amacıyla, ekleme, silme ve değiştirme gibi çeşitli hata türleri geçiş yapabilirsiniz. Ses için dinleme ve karşılaştırma tanıma sonuçları (insan etiketli transkripsiyonu ve iki konuşma metin modeli sonuçları gösteriliyor) her sütunda hangi modelle ihtiyaçlarınıza uygun ve geliştirmeleri gerekmesi karar verebilirsiniz.

Kalite test inceleyerek bir konuşma tanıma uç kalitesini bir uygulama için yeterli olup olmadığını doğrulamak kullanışlıdır.  Doğruluk için bir hedef ölçü bulunan yönergeleri izleyerek transcribed ses gerektiren [değerlendirmek doğruluğu](how-to-custom-speech-evaluate-data.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
* [Modelinizi eğitin](how-to-custom-speech-train-model.md)
* [Modelinizi dağıtma](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Özel konuşma için test verilerini hazırladınız](how-to-custom-speech-test-data.md)
