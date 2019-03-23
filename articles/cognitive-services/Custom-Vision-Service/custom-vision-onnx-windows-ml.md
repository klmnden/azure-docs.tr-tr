---
title: "Öğretici: Windows ML - Custom Vision Service'e ONNX model kullanın"
titlesuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler’den dışarı aktarılan bir ONNX modelini kullanan Windows UWP uygulamasının nasıl oluşturulacağını öğrenin.
services: cognitive-services
author: larryfr
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: larryfr
ms.openlocfilehash: af1b96b4ab47053a6737893832b484372ed37e99
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351873"
---
# <a name="tutorial-use-an-onnx-model-from-custom-vision-with-windows-ml-preview"></a>Öğretici: Bir özel görüntü işleme ONNX modelden Windows ML (Önizleme) ile kullanma

Windows ML (önizleme) ile Özel Görüntü İşleme hizmetinden dışarı aktarılan bir ONNX modelinin nasıl kullanılacağını öğrenin.

Bu belgedeki bilgiler, Windows ML ile Özel Görüntü İşleme Hizmeti’nden dışarı aktarılan bir ONNX dosyasının nasıl kullanılacağını göstermektedir. Örnek bir Windows UWP uygulaması sağlanmıştır. Örneğe, tanıyabilen bir eğitimli model eklenmiştir. Bu örnek ile kendi modelinizi nasıl kullanabileceğinize ilişkin adımlar da sağlanmıştır.

> [!div class="checklist"]
> * Örnek uygulama hakkında
> * Örnek kodunu alma
> * Örneği çalıştırma
> * Kendi modelinizi kullanma

## <a name="prerequisites"></a>Önkoşullar

* Windows 10 derleme 17738 veya üzeri

* Derleme 17738 veya üzeri için Windows SDK

* __Evrensel Windows Platformu geliştirmesi__ iş yükü etkinleştirilmiş Visual Studio 2017 15.7 veya sonraki bir sürüm.

* Etkinleştirilmiş geliştirici modu. Daha fazla bilgi için [Geliştirme için cihazınızı etkinleştirme](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) belgesine bakın.

## <a name="about-the-example-app"></a>Örnek uygulama hakkında

Uygulama, genel bir Windows UWP uygulamasıdır. Bilgisayarınızdan bir görüntü seçerek modele göndermenizi sağlar. Model tarafından döndürülen etiketler ve puanlar görüntünün yanında görüntülenir.

## <a name="get-the-example-code"></a>Örnek kodunu alma

Örnek uygulamaya [https://github.com/Azure-Samples/cognitive-services-onnx12-customvision-sample/](https://github.com/Azure-Samples/cognitive-services-onnx12-customvision-sample/) konumundan erişilebilir.

## <a name="run-the-example"></a>Örneği çalıştırma

1. Visual Studio’dan uygulamayı başlatmak için `F5` tuşunu kullanın. Geliştirici modunu etkinleştirmeniz istenebilir. Daha fazla bilgi için [Geliştirme için cihazınızı etkinleştirme](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) belgesine bakın.

1. Uygulama başlatıldığında düğmeyi kullanarak puanlanacak bir görüntü seçin.

## <a name="use-your-own-model"></a>Kendi modelinizi kullanma

Kendi modelinizi kullanmak için aşağıdaki adımları uygulayın:

1. Özel Görüntü İşleme Hizmeti ile bir sınıflandırıcı [oluşturun ve eğitin](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier). Modeli dışarı aktarmak için **Genel (sıkıştırılmış)** gibi bir __sıkıştırılmış__ etki alanı seçin. Var olan bir sınıflandırıcıyı dışarı aktarmak için sağ üstteki dişli simgesini seçerek etki alanını sıkıştırın. __Ayarlar__ sayfasında sıkıştırılmış bir model seçin, projenizi kaydedin ve eğitin.  

1. Performans sekmesine giderek [modelinizi dışarı aktarın](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model). Sıkıştırılmış etki alanı ile eğitilmiş bir yineleme seçtiğinizde "Dışarı aktar" düğmesi görünür. *Dışarı Aktar*, *ONNX*, *ONNX1.2* ve ardından *Dışarı Aktar*'ı seçin. Dosya hazır duruma geldikten sonra *İndir* düğmesini seçin.

1. ONNX dosyasını projenizin __Varlıklar__ klasörüne bırakın. 

1. Çözüm Gezgini'nde Varlıklar klasörüne sağ tıklayın ve __Var Olan Öğe Ekle__'yi seçin. ONNX dosyasını seçin.

1. Çözüm Gezgini'nde Varlıklar klasöründen ONNX dosyasını seçin. Dosya için aşağıdaki özellikleri değiştirin:

    * __Derleme Eylemi__ -> __İçerik__
    * __Çıkış Dizinine Kopyala__ -> __Daha yeniyse kopyala__

1. `_onnxFileNames` değişkenini ONNX dosyasının adıyla değiştirin. Ayrıca `ClassLabel` değerini de modelde bulunan etiket sayısına göre değiştirin.

1. Derleyin ve çalıştırın.

1. Değerlendirilecek görüntüyü seçmek için düğmeye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Başka dışarı aktarma yollarını keşfetmek ve Özel Görüntü İşleme modelini kullanmak için aşağıdaki belgelere bakın:

* [Verilerinizi dışarı aktarma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model)
* [Android uygulamasında dışarı aktarılan Tensorflow modelini kullanma](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample)
* [Swift iOS uygulamasında dışarı aktarılan CoreML modelini kullanma](https://go.microsoft.com/fwlink/?linkid=857726)
* [Xamarin ile iOS uygulamasında dışarı aktarılan CoreML modelini kullanma](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel)

Windows ML ile ONNX modellerini kullanma hakkında daha fazla bilgi için [Windows ML ile uygulamanızda bir modeli tümleştirme](https://docs.microsoft.com/windows/uwp/machine-learning/integrate-model) belgesine bakın.
