---
title: "Hızlı Başlangıç: Java için özel görüntü işleme SDK'sı ile bir nesne algılama projesi oluşturma"
titlesuffix: Azure Cognitive Services
description: Java SDK'sını kullanarak bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yükleyin, projenizi eğitin ve nesneleri algılayın.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: areddish
ms.openlocfilehash: 00684df614771437f33655538a808468ee778d29
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60995974"
---
# <a name="quickstart-create-an-object-detection-project-with-the-custom-vision-sdk-for-java"></a>Hızlı Başlangıç: Java için özel görüntü işleme SDK'sı ile bir nesne algılama projesi oluşturma

Bu makalede, Özel Görüntü İşleme SDK'sını Java ile kullanarak nesne algılama modeli oluşturmaya başlarken size yardımcı olacak bilgiler ve örnek kod sağlanır. Oluşturulduktan sonra etiketlenmiş bölgeler ekleyebilir, görüntüleri karşıya yükleyebilir, projeyi eğitebilir, projenin varsayılan tahmin uç nokta URL’sini alabilir ve bir görüntüyü programlama yoluyla test etmek için uç noktayı kullanabilirsiniz. Kendi Java uygulamanızı oluştururken bu örneği şablon olarak kullanın.

## <a name="prerequisites"></a>Önkoşullar

- Kendi seçtiğiniz bir Java IDE
- [JDK 7 veya 8](https://aka.ms/azure-jdks) yüklenmiş olmalıdır.
- Maven yüklenmiş olmalıdır

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Özel Görüntü İşleme SDK’sını ve örnek kodu alma

Özel Görüntü İşleme kullanan bir Java uygulaması yazmak için Özel Görüntü İşleme maven paketlerine ihtiyacınız olacaktır. Bunlar indireceğiniz örnek projeye dahil edilmiştir, ama burada bunlara tek tek erişebilirsiniz.

Maven merkezi deposundan Özel Görüntü İşleme SDK’sını yükleyebilirsiniz:
- [Eğitim SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [Tahmin SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

[Bilişsel Hizmetler Java SDK'sı Örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master) projesini kopyalayın veya indirin. **Vision/CustomVision/** klasörüne gidin.

Bu Java projesi, [Özel Görüntü İşleme web sitesi](https://customvision.ai/) üzerinden erişilebilen __Sample Java OD Project__ adlı yeni bir Özel Görüntü İşleme nesne algılama projesi oluşturur. Daha sonra bir sınıflandırıcıyı eğitip test etmek için görüntüleri karşıya yükler. Bu projede sınıflandırıcının, bir ağacın __Köknar__ mı yoksa __Japon Kirazı__ mı olduğunu belirlemesi hedeflenmiştir.

[!INCLUDE [get-keys](includes/get-keys.md)]

Program, önemli verilerinizi ortam değişkeni olarak depolayacak şekilde yapılandırılır. PowerShell'de **Vision/CustomVision** klasörüne giderek bu ortam değişkenlerini ayarlayın. Ardından komutları girin:

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="understand-the-code"></a>Kodu anlama

Java IDE'nize `Vision/CustomVision` projesini yükleyin ve _CustomVisionSamples.java_ dosyasını açın. **runSample** yöntemini bulun ve **ImageClassification_Sample** yöntem çağrısını açıklama haline getirin; bu yöntem, bu kılavuzun kapsamına girmeyen görüntü sınıflandırma senaryosunu çalıştırır. **ObjectDetection_Sample** yöntemi bu hızlı başlangıcın birincil işlevini gerçekleştirir; yöntemin tanımına gidin ve kodu inceleyin. 

### <a name="create-a-new-custom-vision-service-project"></a>Yeni bir Özel Görüntü İşleme Hizmeti projesi oluşturma

Eğitim istemcisi ve nesne algılama projesini oluşturan kod bloğuna gidin. Oluşturulan proje, daha önce ziyaret ettiğiniz [Özel Görüntü İşleme web sitesinde](https://customvision.ai/) gösterilir. 

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=181-206)]

### <a name="add-tags-to-your-project"></a>Projenize etiketler ekleme

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=208-218)]

### <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Nesne algılama projelerinde görüntüleri etiketlediğinizde etiketli her nesnenin bölgesini normalleştirilmiş koordinatları kullanarak belirtmeniz gerekir. `regionMap` Haritasının tanımına gidin. Bu kod, örnek görüntülerin her birini etiketlendikleri bölgeyle ilişkilendirir.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=130-179)]

Ardından projeye görüntüleri ekleyen kod bloğuna atlayın. Görüntüler projenin **src/main/resources** klasöründen okunur ve uygun etiketleri ve bölge koordinatlarıyla hizmete yüklenir.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=220-231)]

Önceki kod parçacığı, kaynak akışları olarak görüntüleri alan ve bunları hizmete yükleyen iki yardımcı işlevinden yararlanır.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=277-314)]

### <a name="train-the-project-and-publish"></a>Proje eğitin ve yayımlayın

Bu kod, ilk yineleme projede oluşturur ve ardından bu yineleme tahmin uç noktaya yayımlar. Yayımlanmış bir yineleme için verilen ad, tahmin istekleri göndermek için kullanılabilir. Yineleme yayımlanmadan tahmin uç noktasında kullanılabilir değil.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=233-242)]

### <a name="use-the-prediction-endpoint"></a>Tahmin uç noktasını kullanma

Burada `predictor` nesnesiyle gösterilen tahmin uç noktası, bir görüntüyü geçerli modele göndermek ve sınıflandırma tahmini almak için kullandığınız başvurudur. Bu örnekte `predictor`, tahmin anahtarı ortam değişkeni kullanılarak başka bir yerde tanımlanır.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=244-270)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Maven kullanarak çözümü derlemek ve çalıştırmak için, PowerShell'de proje dizininde aşağıdaki komutu çalıştırın:

```powershell
mvn compile exec:java
```

Günlüğe kaydetme ve tahmin sonuçları için konsol çıkışını görüntüleyin. Ardından test görüntüsünün etiketlendiğini ve algılama bölgesinin doğru olduğunu onaylayabilirsiniz.

[!INCLUDE [clean-od-project](includes/clean-od-project.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık kodda nesne algılama işleminin her adımının nasıl uygulanabileceğini gördünüz. Bu örnek tek bir eğitim yinelemesi yürütür ama modelinizin daha doğru olmasını sağlamak için çoğunlukla birden çok kez eğitmeniz ve test etmeniz gerekecektir. Sonraki kılavuzda görüntü sınıflandırma konusu üstünde durulur ancak temel ilkeleri nesne algılamaya benzer.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](test-your-model.md)
