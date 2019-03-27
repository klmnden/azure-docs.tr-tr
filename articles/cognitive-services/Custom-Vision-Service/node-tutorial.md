---
title: "Hızlı Başlangıç: Bir görüntü sınıflandırma proje Node.js için özel görüntü işleme SDK'sı ile oluşturma"
titlesuffix: Azure Cognitive Services
description: Bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yüklemek, projenizi eğitmek ve Node.js SDK'sını kullanarak bir tahminde bulunmak.
services: cognitive-services
author: areddish
manager: daauld
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: areddish
ms.openlocfilehash: 9d9021cd3acaebe689c583281e0316b30d5892c0
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58482462"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-nodejs-sdk"></a>Hızlı Başlangıç: Özel görüntü işleme Node.js SDK'sı ile bir görüntü sınıflandırma projesi oluşturma

Bu makalede, bilgi ve yardımcı olması için örnek kod, bir görüntü sınıflandırma modeli oluşturmak için Node.js ile özel görüntü işleme SDK'sı ile çalışmaya başlamak sağlar. Oluşturulduktan sonra etiketler ekleyin, görüntüleri karşıya yüklemek, proje eğitmek, projenin yayımlanan tahmin uç nokta URL'si almak ve program aracılığıyla resim test etmek için uç noktayı kullanın. Bu örnek, kendi Node.js uygulaması oluşturmak için şablon olarak kullanın. Kod _içermeyen_ bir sınıflandırma modeli oluşturma ve kullama işlemi yapmak istiyorsanız, [tarayıcı tabanlı kılavuz](getting-started-build-a-classifier.md) konusuna bakın.

## <a name="prerequisites"></a>Önkoşullar

- [Node.js 8](https://www.nodejs.org/en/download/) veya sonraki bir sürümü yüklü.
- [npm](https://www.npmjs.com/) yüklü.

## <a name="install-the-custom-vision-sdk"></a>Özel Görüntü İşleme SDK’sını yükleme

Node.js için özel görüntü işleme hizmeti SDK'sını yüklemek için PowerShell'de aşağıdaki komutu çalıştırın:

```powershell
npm install azure-cognitiveservices-customvision-training
npm install azure-cognitiveservices-customvision-prediction
```

[!INCLUDE [get-keys](includes/get-keys.md)]

[!INCLUDE [node-get-images](includes/node-get-images.md)]

## <a name="add-the-code"></a>Kod ekleme

Adlı yeni bir dosya oluşturun *sample.js* tercih edilen proje dizininizde.

### <a name="create-the-custom-vision-service-project"></a>Özel Görüntü İşleme hizmeti projesi oluşturma

Yeni bir Özel Görüntü İşleme hizmeti projesi oluşturmak için betiğinize aşağıdaki kodu ekleyin. Abonelik anahtarlarınızı uygun tanımlara ekleyin.

```javascript
const util = require('util');
const TrainingApi = require("azure-cognitiveservices-customvision-training");
const PredictionApi = require("azure-cognitiveservices-customvision-prediction");

const setTimeoutPromise = util.promisify(setTimeout);

const trainingKey = "<your training key>";
const predictionKey = "<your prediction key>";
const predictionResourceId = "<your prediction resource id>";
const sampleDataRoot = "<path to image files>";

const endPoint = "https://southcentralus.api.cognitive.microsoft.com"

const publishIterationName = "classifyModel";

const trainer = new TrainingApiClient(trainingKey, endPoint);

(async () => {
    console.log("Creating project...");
    const sampleProject = await trainer.createProject("Sample Project")
```

### <a name="create-tags-in-the-project"></a>Projede etiketler oluşturma

Projenize sınıflandırma etiketleri oluşturmak için sonuna aşağıdaki kodu ekleyin *sample.js*:

```javascript
    const hemlockTag = await trainer.createTag(sampleProject.id, "Hemlock");
    const cherryTag = await trainer.createTag(sampleProject.id, "Japanese Cherry");
```

### <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Projeye örnek görüntüleri eklemek için etiket oluşturduktan sonra aşağıdaki kodu ekleyin. Bu kod, her görüntüyü ilgili etiketiyle birlikte karşıya yükler. Bilişsel hizmetler Node.js SDK'sı örnekleri proje indirdiğiniz üzerinde temel temel görüntü dosya yolunu girmeniz gerekir.

> [!NOTE]
> Değiştirmeniz gerekir *sampleDataRoot* Bilişsel Hizmetleri Node.js SDK'sı örnekleri indirdiğiniz üzerinde temel görüntü için daha önce proje.

```javascript
    console.log("Adding images...");
    let fileUploadPromises = [];

    const hemlockDir = `${sampleDataRoot}/Hemlock`;
    const hemlockFiles = fs.readdirSync(hemlockDir);
    hemlockFiles.forEach(file => {
        fileUploadPromises.push(trainer.createImagesFromData(sampleProject.id, fs.readFileSync(`${hemlockDir}/${file}`), { tagIds: [hemlockTag.id] }));
    });

    const cherryDir = `${sampleDataRoot}/Japanese Cherry`;
    const japaneseCherryFiles = fs.readdirSync(cherryDir);
    japaneseCherryFiles.forEach(file => {
        fileUploadPromises.push(trainer.createImagesFromData(sampleProject.id, fs.readFileSync(`${cherryDir}/${file}`), { tagIds: [cherryTag.id] }));
    });

    await Promise.all(fileUploadPromises);
```

### <a name="train-the-classifier-and-publish"></a>Sınıflandırıcı eğitin ve yayımlayın

Bu kod, ilk yineleme projede oluşturur ve ardından bu yineleme tahmin uç noktaya yayımlar. Yayımlanmış bir yineleme için verilen ad, tahmin istekleri göndermek için kullanılabilir. Yineleme yayımlanmadan tahmin uç noktasında kullanılabilir değil.

```javascript
    console.log("Training...");
    let trainingIteration = await trainer.trainProject(sampleProject.id);

    // Wait for training to complete
    console.log("Training started...");
    while (trainingIteration.status == "Training") {
        console.log("Training status: " + trainingIteration.status);
        await setTimeoutPromise(1000, null);
        trainingIteration = await trainer.getIteration(sampleProject.id, trainingIteration.id)
    }
    console.log("Training status: " + trainingIteration.status);
    
    // Publish the iteration to the end point
    await trainer.publishIteration(sampleProject.id, trainingIteration.id, publishIterationName, predictionResourceId);
```

### <a name="get-and-use-the-published-iteration-on-the-prediction-endpoint"></a>Edinin ve öngörü uç noktasında yayımlanan yineleme kullanın

Tahmin uç noktasına bir görüntü göndermek ve tahmini almak için dosyanın sonuna aşağıdaki kodu ekleyin:

```javascript
    const predictor = new PredictionApiClient(predictionKey, endPoint);
    const testFile = fs.readFileSync(`${sampleDataRoot}/Test/test_image.jpg`);

    const results = await predictor.classifyImage(sampleProject.id, publishIterationName, testFile);

    // Step 6. Show results
    console.log("Results:");
    results.predictions.forEach(predictedResult => {
        console.log(`\t ${predictedResult.tagName}: ${(predictedResult.probability * 100.0).toFixed(2)}%`);
    });
})()
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Çalıştırma *sample.js*.

```powershell
node sample.js
```

Uygulamanın çıkışı aşağıdaki metne benzer olmalıdır:

```
Creating project...
Adding images...
Training...
Training started...
Training status: Training
Training status: Training
Training status: Training
Training status: Completed
Results:
         Hemlock: 94.97%
         Japanese Cherry: 0.01%
```

Ardından test görüntüsünün (**<base_image_url>/Images/Test/** yolunda bulunur) düzgün etiketlendiğini doğrulayabilirsiniz. Ayrıca [Özel Görüntü İşleme web sitesine](https://customvision.ai) geri dönebilir ve yeni oluşturulan projenizin geçerli durumunu görebilirsiniz.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık kodda görüntü sınıflandırma işleminin her adımının nasıl uygulanabileceğini gördünüz. Bu örnek tek bir eğitim yinelemesi yürütür ama modelinizin daha doğru olmasını sağlamak için çoğunlukla birden çok kez eğitmeniz ve test etmeniz gerekecektir.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](test-your-model.md)