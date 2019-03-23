---
title: Modelinizi mobil - Custom Vision Service'e dışarı aktarma
titlesuffix: Azure Cognitive Services
description: Modelinizi mobil uygulamalar oluştururken kullanmak için dışarı aktarma hakkında bilgi edinin.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: anroth
ms.openlocfilehash: 405b6ebd06091536749751a94362d8c4a6495dbc
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351312"
---
# <a name="export-your-model-for-use-with-mobile-devices"></a>Modelinizi kullanılmak ile mobil cihazları dışarı aktarma

Özel görüntü işleme hizmeti sınıflandırıcılar çevrimdışı çalışmasını dışarı aktarılmasına izin verir. Dışarı aktarılan sınıflandırıcınızı bir uygulamaya ekleme ve yerel olarak çalıştırma gerçek zamanlı sınıflandırma için bir cihaz.

Özel görüntü işleme hizmeti, aşağıdaki dışarı aktarmaları destekler:

* __Tensorflow__ için __Android__.
* __CoreML__ için __iOS11__.
* __ONNX__ için __Windows ML__.
* Bir Windows veya Linux __kapsayıcı__. Kapsayıcı içeren bir Tensorflow model ve hizmet özel görüntü işleme hizmeti API'sini kullanmak için kodu. 

> [!IMPORTANT]
> Özel görüntü işleme hizmeti yalnızca dışa aktarır __compact__ etki alanları. Compact etki alanları tarafından oluşturulan modelleri, mobil cihazlarda gerçek zamanlı sınıflandırma kısıtlamaları için iyileştirilmiştir. Sıkıştırılmış bir etki alanı ile oluşturulan sınıflandırıcılar eğitim verilerini aynı miktarda sahip standart bir etki alanı biraz daha az kesin olabilir.
>
> Sınıflandırıcılar artırma hakkında daha fazla bilgi için bkz: [sınıflandırıcınızı sürekli olarak geliştirmek](getting-started-improving-your-classifier.md) belge.

## <a name="convert-to-a-compact-domain"></a>Sıkıştırılmış bir etki alanına dönüştürün

> [!NOTE]
> Bu bölümdeki adımlar, yalnızca etki alanı sıkıştırmak için ayarlanmadı var olan bir sınıflandırıcı varsa geçerlidir.

Etki alanı mevcut bir sınıflandırıcı dönüştürmek için aşağıdaki adımları kullanın:

1. Gelen [özel görüntü işleme sayfasını](https://customvision.ai)seçin __giriş__ projelerinizi listesini görüntülemek için simge. Ayrıca [ https://customvision.ai/projects ](https://customvision.ai/projects) projelerinizi görmek için.

    ![Giriş simgesini ve projeleri listesinin görüntüsü](./media/export-your-model/projects-list.png)

2. Bir proje seçin ve ardından __dişli__ sayfanın sağ üst kısımdaki simgesi.

    ![Dişli simgesinin görüntüsü](./media/export-your-model/gear-icon.png)

3. İçinde __etki alanları__ bölümünden bir __compact__ etki alanı. Seçin __Değişiklikleri Kaydet__ değişiklikleri kaydedin.

    ![Etki alanı seçimi görüntüsü](./media/export-your-model/domains.png)

4. Sayfanın üst kısmından seçin __eğitme__ yeniden ayarlamak yeni etki kullanarak.

## <a name="export-your-model"></a>Modelinizi dışarı aktarma

Modeli yeniden eğitme sonra dışa aktarmak için aşağıdaki adımları kullanın:

1. Git **performans** sekmenize __dışarı__. 

    ![Dışarı aktarma simgesinin görüntüsü](./media/export-your-model/export.png)

    > [!TIP]
    > Varsa __dışarı__ girişi mevcut değil ve ardından seçili yineleme kompakt bir etki alanı kullanmaz. Kullanım __yinelemeler__ kompakt bir etki alanı kullanan bir yineleme seçin ve ardından seçmek için bu sayfanın bölümüne __dışarı__.

2. Dışarı aktarma biçimini seçin ve ardından __dışarı__ modeli indirilemedi.

## <a name="next-steps"></a>Sonraki adımlar

Dışarı aktarılan modelinizi bir uygulamayı tümleştirin. Birkaç örnek uygulamalar mevcuttur:

* Bir örnek için [dışarı aktarılan CoreML modelinizi kullanarak bir iOS uygulaması](https://go.microsoft.com/fwlink/?linkid=857726) Swift ile gerçek zamanlı görüntü sınıflandırma
* İOS uygulaması için örnek [Xamarin ile verilen CoreML modelinizi kullanarak](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) gerçek zamanlı görüntü sınıflandırma 
* İçin örnek [bir Android uygulaması dışarı aktarılan Tensorflow modelinizi kullanarak](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) gerçek zamanlı görüntü sınıflandırma 
* [Tensorflow modelinizi Windows ile kullanma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-model-python)
* İçin örnek [dışarı aktarılan ONNX model Windows Machine Learning ile kullanarak](https://azure.microsoft.com/resources/samples/cognitive-services-onnx-customvision-sample/)
