---
title: 'Öğretici: Görüntü sınıflandırma projesi derleme - Özel Görüntü İşleme Hizmeti, Java'
titlesuffix: Azure Cognitive Services
description: Bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yükleyin, projenizi eğitin ve varsayılan uç noktayı kullanarak bir tahminde bulunun.
services: cognitive-services
author: areddish
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: 9a7f50e0eb33016d6a2d8f28be047b327135c51f
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367364"
---
# <a name="tutorial-build-an-image-classification-project-with-java"></a>Öğretici: Java ile görüntü sınıflandırma projesi derleme

Java kullanarak Özel Görüntü İşleme Hizmeti ile nasıl görüntü sınıflandırma projesi oluşturulacağını öğrenin. Oluşturulduktan sonra etiketler ekleyebilir, görüntüleri karşıya yükleyebilir, projeyi eğitebilir, projenin varsayılan tahmin uç nokta URL’sini alabilir ve bir görüntüyü programlama yoluyla test etmek için bunu kullanabilirsiniz. Özel Görüntü İşleme API’sini kullanarak kendi uygulamanızı derlemek için şablon olarak bu açık kaynak örneği kullanın.

## <a name="prerequisites"></a>Ön koşullar

- JDK 7 veya 8 yüklenmiş olmalıdır.
- Maven yüklenmiş olmalıdır.

## <a name="get-the-training-and-prediction-keys"></a>Eğitim ve tahmin anahtarlarını alma

Bu örnekte kullanılan anahtarları almak için [Özel Görüntü İşleme web sayfasını](https://customvision.ai) ziyaret edin ve sağ üst kısımdaki __dişli simgesini__ seçin. __Hesaplar__ bölümünde, __Eğitim Anahtarı__ ve __Tahmin Anahtarı__ alanlarından değerleri kopyalayın.

![Anahtarlar kullanıcı arabiriminin görüntüsü](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Özel Görüntü İşleme Hizmeti SDK’sını yükleme

Maven merkezi deposundan Özel Görüntü İşleme SDK’sını yükleyebilirsiniz:
* [Eğitim SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
* [Tahmin SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

## <a name="understand-the-code"></a>Kodu anlama

Görüntüler de dahil olmak üzere tam projeye [Java deposu için Özel Görüntü İşleme Azure örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master) bölümünden erişilebilir. 

`Vision/CustomVision` projesini açmak için sık kullandığınız Java IDE’nizi kullanın. 

Bu uygulama, __Örnek Java Projesi__ adlı yeni bir proje oluşturmak için daha önce aldığınız eğitim anahtarını kullanır. Daha sonra bir sınıflandırıcıyı eğitip test etmek için görüntüleri karşıya yükler. Sınıflandırıcı, bir ağacın __Kanada Çamı__ mı yoksa __Japon Vişnesi__ mi olduğunu belirler.

Aşağıdaki kod parçacıkları, bu örneğin birincil işlevlerini uygular:

## <a name="create-a-custom-vision-service-project"></a>Özel Görüntü İşleme Hizmeti projesi oluşturma

> [!IMPORTANT]
> `trainingApiKey` öğesini, daha önce aldığınız eğitim anahtarı değerine ayarlayın.

```java
final String trainingApiKey = "insert your training key here";
TrainingApi trainClient = CustomVisionTrainingManager.authenticate(trainingApiKey);

Trainings trainer = trainClient.trainings();

System.out.println("Creating project...");
Project project = trainer.createProject()
            .withName("Sample Java Project")
            .execute();
```

## <a name="add-tags-to-your-project"></a>Projenize etiketler ekleme

```java
// create hemlock tag
Tag hemlockTag = trainer.createTag()
    .withProjectId(project.id())
    .withName("Hemlock")
    .execute();

// create cherry tag
Tag cherryTag = trainer.createTag()
    .withProjectId(project.id())
    .withName("Japanese Cherry")
    .execute();
```

## <a name="upload-images-to-the-project"></a>Projeye görüntüleri karşıya yükleme

Örnek, görüntüler nihai pakete dahil edilecek şekilde ayarlanır. Görüntüler, jar’ın kaynaklar bölümünden okunur ve hizmete yüklenir.

```java
System.out.println("Adding images...");
for (int i = 1; i <= 10; i++) {
    String fileName = "hemlock_" + i + ".jpg";
    byte[] contents = GetImage("/Hemlock", fileName);
    AddImageToProject(trainer, project, fileName, contents, hemlockTag.id(), null);
}

for (int i = 1; i <= 10; i++) {
    String fileName = "japanese_cherry_" + i + ".jpg";
    byte[] contents = GetImage("/Japanese Cherry", fileName);
    AddImageToProject(trainer, project, fileName, contents, cherryTag.id(), null);
}
```

Önceki kod parçacığı, kaynak akışları olarak görüntüleri alan ve bunları hizmete yükleyen iki yardımcı işlevinden yararlanır.

```java
private static void AddImageToProject(Trainings trainer, Project project, String fileName, byte[] contents, UUID tag)
{
    System.out.println("Adding image: " + fileName);

    ImageFileCreateEntry file = new ImageFileCreateEntry()
        .withName(fileName)
        .withContents(contents);

    ImageFileCreateBatch batch = new ImageFileCreateBatch()
        .withImages(Collections.singletonList(file));

    batch = batch.withTagIds(Collections.singletonList(tag));

    trainer.createImagesFromFiles(project.id(), batch);
}

private static byte[] GetImage(String folder, String fileName)
{
    try {
        return ByteStreams.toByteArray(CustomVisionSamples.class.getResourceAsStream(folder + "/" + fileName));
    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }

    return null;
}
```

## <a name="train-the-project"></a>Projeyi eğitme

Bu, projedeki ilk yinelemeyi oluşturur ve bu yinelemeyi varsayılan yineleme olarak işaretler. 

```java
System.out.println("Training...");
Iteration iteration = trainer.trainProject(project.id());

while (iteration.status().equals("Training"))
{
    System.out.println("Training Status: "+ iteration.status());
    Thread.sleep(1000);
    iteration = trainer.getIteration(project.id(), iteration.id());
}

System.out.println("Training Status: "+ iteration.status());
trainer.updateIteration(project.id(), iteration.id(), iteration.withIsDefault(true));
```

## <a name="get-and-use-the-default-prediction-endpoint"></a>Varsayılan tahmin uç noktasını alma ve kullanma

> [!IMPORTANT]
> `predictionApiKey` öğesini, daha önce aldığınız tahmin anahtarı değerine ayarlayın.

```java
final String predictionApiKey = "insert your prediction key here";
PredictionEndpoint predictClient = CustomVisionPredictionManager.authenticate(predictionApiKey);

// Use below for predictions from a url
// String url = "some url";
// ImagePrediction results = predictor.predictions().predictImage()
//                         .withProjectId(project.id())
//                         .withUrl(url)
//                         .execute();

// load test image
byte[] testImage = GetImage("/Test", "test_image.jpg");

// predict
ImagePrediction results = predictor.predictions().predictImage()
    .withProjectId(project.id())
    .withImageData(testImage)
    .execute();

for (Prediction prediction: results.predictions())
{
    System.out.println(String.format("\t%s: %.2f%%", prediction.tagName(), prediction.probability() * 100.0f));
}
```

## <a name="run-the-example"></a>Örneği çalıştırma

Maven kullanarak çözümü derlemek ve çalıştırmak için:

```
mvn compile exec:java
```

Uygulamanın çıktısı aşağıdaki metne benzer:

```
Creating project...
Adding images...
Adding image: hemlock_1.jpg
Adding image: hemlock_2.jpg
Adding image: hemlock_3.jpg
Adding image: hemlock_4.jpg
Adding image: hemlock_5.jpg
Adding image: hemlock_6.jpg
Adding image: hemlock_7.jpg
Adding image: hemlock_8.jpg
Adding image: hemlock_9.jpg
Adding image: hemlock_10.jpg
Adding image: japanese_cherry_1.jpg
Adding image: japanese_cherry_2.jpg
Adding image: japanese_cherry_3.jpg
Adding image: japanese_cherry_4.jpg
Adding image: japanese_cherry_5.jpg
Adding image: japanese_cherry_6.jpg
Adding image: japanese_cherry_7.jpg
Adding image: japanese_cherry_8.jpg
Adding image: japanese_cherry_9.jpg
Adding image: japanese_cherry_10.jpg
Training...
Training status: Training
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```