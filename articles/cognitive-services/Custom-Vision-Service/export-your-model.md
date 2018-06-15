---
title: Mobil - özel görme Service - Azure Bilişsel hizmetler için özel görme hizmet modelinizi verme | Microsoft Docs
description: Modelinizi mobil uygulamalar oluştururken kullanılacak verme öğrenin.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: ce8f42d6239867dd217cddfc61a27d7835dc9c9b
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355918"
---
# <a name="export-your-model-for-use-with-mobile-devices"></a>Modelinizi kullanmak için mobil aygıtlarla dışarı aktarma

Özel görme hizmet çevrimdışı çalışmasını verilecek sınıflandırıcı sağlar. Bir uygulamaya verilen sınıflandırıcı katıştırmak ve yerel olarak çalıştırma gerçek zamanlı sınıflandırması için bir aygıtta. 

Özel görme hizmet aşağıdaki dışarı destekler:

* __Tensorflow__ için __Android__.
* __CoreML__ için __iOS11__.
* __ONNX__ için __Windows ML__.
* Bir Windows veya Linux __kapsayıcı__. Kapsayıcı bir Tensorflow içeren model ve hizmet özel görme hizmeti API'si kullanmak için kodu. 

> [!IMPORTANT]
> Yalnızca özel görme hizmet verir __compact__ etki alanları. Compact etki alanı tarafından oluşturulan modelleri mobil cihazlarda gerçek zamanlı sınıflandırma kısıtlamalar için en iyi duruma getirilir. Sıkıştırılmış bir etki alanı ile oluşturulmuş sınıflandırıcı standart bir etki alanı ile aynı miktarda eğitim verileri daha az doğru olmayabilir.
>
> Sınıflandırıcı artırma hakkında daha fazla bilgi için bkz: [, sınıflandırıcı geliştirme](getting-started-improving-your-classifier.md) belge.

## <a name="convert-to-a-compact-domain"></a>Sıkıştırılmış bir etki alanına Dönüştür

> [!NOTE]
> Etki alanı sıkıştırmak için ayarlanmamış bir varolan sınıflandırıcı varsa, yalnızca bu bölümdeki adımları uygulayın.
 
Varolan bir sınıflandırıcı etki alanına dönüştürmek için aşağıdaki adımları kullanın:

1. Gelen [özel görme sayfasını](https://customvision.ai)seçin __giriş__ projelerinizi listesini görüntülemek için simge. Aynı zamanda [ https://customvision.ai/projects ](https://customvision.ai/projects) projelerinizi görmek için.

    ![Ev simgesini ve projeleri listesi görüntüsü](./media/export-your-model/projects-list.png)

2. Bir proje seçin ve ardından __dişli__ sayfasının sağ üst simgesi.

    ![Dişli simgesinin görüntüsü](./media/export-your-model/gear-icon.png)

3. İçinde __etki alanları__ bölümünde, select bir __compact__ etki alanı. Seçin __Değişiklikleri Kaydet__ değişiklikleri kaydedin.

    ![Etki alanları seçimi görüntüsü](./media/export-your-model/domains.png)

4. Sayfanın üst kısmından seçin __Train__ yeni etki alanını kullanan yeniden eğitme için.

## <a name="export-your-model"></a>Modelinizi dışarı aktarma

Yeniden eğitme sonra modeli dışa aktarmak için aşağıdaki adımları kullanın:

1. Git **performans** sekmesinde ve seçin __verme__. 

    ![Dışarı aktarma simgesinin görüntüsü](./media/export-your-model/export.png)

    > [!TIP]
    > Varsa __verme__ girişi kullanılabilir değil ve ardından seçili yineleme compact bir etki alanı kullanmaz. Kullanım __yineleme__ compact bir etki alanı kullanan yineleme seçin ve ardından seçmek için bu sayfayı bölümünü __verme__.

2. Verme biçimini seçin ve ardından __verme__ model karşıdan yüklemek için.

## <a name="next-steps"></a>Sonraki adımlar

Dışarı aktarılan modelinizi bir uygulamayı tümleştirin. Birkaç örnek uygulamalar mevcuttur:

* Bir örnek için [dışarı aktarılan CoreML model bir iOS uygulaması kullanarak](https://go.microsoft.com/fwlink/?linkid=857726) Swift ile gerçek zamanlı görüntü sınıflandırma
* Örnek iOS uygulaması için [Xamarin ile verilen CoreML model kullanarak](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) gerçek zamanlı görüntü sınıflandırma 
* İçin örnek [dışarı aktarılan Tensorflow model bir Android uygulamasını kullanarak](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) gerçek zamanlı görüntü sınıflandırma 
* [Tensorflow modelinizi Windows ile kullanma](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/export-model-python)
* İçin örnek [dışarı aktarılan ONNX modelinizi Windows Machine Learning ile kullanma](https://azure.microsoft.com/en-us/resources/samples/cognitive-services-onnx-customvision-sample/)
