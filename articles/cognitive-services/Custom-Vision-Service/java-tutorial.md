---
title: Azure Bilişsel hizmetler Custom Vision Service Java öğreticisi için - Oluştur | Microsoft Docs
description: Microsoft Bilişsel hizmetler özel görüntü işleme API'sini kullanan basit bir Java uygulaması keşfedin. Bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yüklemek, projenizi eğitmek ve varsayılan uç nokta kullanarak bir tahminde bulunmak.
services: cognitive-services
author: areddish
manager: chbuehle
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: a83a2f5cac9281a4cd79c1a0cead0f2af82d73df
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44305712"
---
# <a name="use-custom-vision-api-to-build-an-image-classification-project-with-java"></a>Java ile bir görüntü sınıflandırma projesi oluşturmak için özel görüntü işleme API'sini kullanın

Özel görüntü işleme hizmeti Java kullanarak bir görüntü sınıflandırma projesi oluşturmayı öğrenin. Oluşturulduktan sonra etiketler ekleyin, görüntüleri karşıya yüklemek, proje Eğitimi, projenin varsayılan tahmin uç nokta URL'sini alma ve program aracılığıyla resim test etmek için kullanın. Bu açık kaynaklı örneği, özel görüntü işleme API'sini kullanarak kendi uygulamanızı oluşturmaya yönelik şablon olarak kullanın.

## <a name="prerequisites"></a>Önkoşullar

- JDK 7 veya 8 yüklü.
- Maven yüklü.

## <a name="get-the-training-and-prediction-keys"></a>Eğitim ve tahmin anahtarları alma

Bu örnekte kullanılan anahtarlarını almak için şurayı ziyaret edin [Custom Vision web sayfası](https://customvision.ai) seçip __dişli simgesini__ sağ üst köşedeki. İçinde __hesapları__ bölümünde, değerleri kopyalayın __eğitim anahtarı__ ve __tahmin anahtar__ alanları.

![UI anahtarları görüntüsü](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Özel görüntü işleme hizmeti SDK'sını yükleme

Maven central depodan Custom Vision SDK'sını yükleyebilirsiniz:
* [Eğitim SDK'sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
* [Tahmin SDK'sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

## <a name="understand-the-code"></a>Kodu anlama

Görüntüleri dahil olmak üzere tam proje kullanılabilir [Java depo için özel görüntü işleme Azure örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). 

En sevdiğiniz Java IDE kullanarak `Vision/CustomVision` proje. 

Bu uygulamayı aldığınız önceki adlı yeni bir proje oluşturmak için eğitim anahtar kullanan __örnek Java projesi__. Ardından, eğitmek ve bir sınıflandırıcı test etmek için görüntüleri yükler. Sınıflandırıcı bir ağaç olup olmadığını tanımlayan bir __köknar__ veya __Japonca tek tek seçme işlemi__.

Aşağıdaki kod parçacıkları, bu örnekte birincil işlevselliğini uygular:

## <a name="create-a-custom-vision-service-project"></a>Custom Vision Service projesi oluşturma

> [!IMPORTANT]
> Ayarlama `trainingApiKey` daha önce aldığınız eğitim anahtar değeri.

```java
final String trainingApiKey = "insert your training key here";
TrainingApi trainClient = CustomVisionTrainingManager.authenticate(trainingApiKey);

Trainings trainer = trainClient.trainings();

System.out.println("Creating project...");
Project project = trainer.createProject()
            .withName("Sample Java Project")
            .execute();
```

## <a name="add-tags-to-your-project"></a>Etiket projenize ekleme

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

Örnek, görüntüleri son pakette dahil etmek üzere kurulur. Görüntüleri jar kaynakları bölümünden okumak ve hizmetine yüklenir.

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

Önceki kod parçacığı, görüntü kaynağı akışları olarak almak ve bunları hizmetine yüklemek iki yardımcı işlev kullanır.

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

## <a name="train-the-project"></a>Proje eğitimi

Bu projedeki ilk yineleme oluşturur ve bu yineleme varsayılan yineleme olarak işaretler. 

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

## <a name="get-and-use-the-default-prediction-endpoint"></a>Alma ve varsayılan tahmin uç noktası kullanma

> [!IMPORTANT]
> Ayarlama `predictionApiKey` daha önce aldığınız tahmin anahtar değeri.

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

## <a name="run-the-example"></a>Örneği çalıştırın

Derleme ve maven kullanarak çözümü çalıştırmak için:

```
mvn compile exec:java
```

Uygulama çıktısı aşağıdaki metne benzer:

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