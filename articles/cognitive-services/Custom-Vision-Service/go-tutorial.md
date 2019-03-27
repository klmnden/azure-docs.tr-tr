---
title: "Hızlı Başlangıç: Go için özel görüntü işleme SDK'sı ile bir görüntü sınıflandırma projesi oluşturma"
titlesuffix: Azure Cognitive Services
description: Bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yüklemek, projenizi eğitmek ve Go SDK'sını kullanarak bir tahminde bulunmak.
services: cognitive-services
author: areddish
manager: daauld
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: areddish
ms.openlocfilehash: f740974d17ad5f95bca6530a61619ee0283f819a
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58479997"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-go-sdk"></a>Hızlı Başlangıç: Özel görüntü işleme Go SDK'sı ile bir görüntü sınıflandırma projesi oluşturma

Bu makalede, bilgi ve yardımcı olması için örnek kod, bir görüntü sınıflandırma modeli oluşturmak için Git ile özel görüntü işleme SDK'sı ile çalışmaya başlamak sağlar. Oluşturulduktan sonra etiketler ekleyin, görüntüleri karşıya yüklemek, proje eğitmek, projenin yayımlanan tahmin uç nokta URL'si almak ve program aracılığıyla resim test etmek için uç noktayı kullanın. Bu örnek, kendi Go uygulaması oluşturmak için şablon olarak kullanın. Kod _içermeyen_ bir sınıflandırma modeli oluşturma ve kullama işlemi yapmak istiyorsanız, [tarayıcı tabanlı kılavuz](getting-started-build-a-classifier.md) konusuna bakın.

## <a name="prerequisites"></a>Önkoşullar

- [Go 1.8 +](https://golang.org/doc/install)

## <a name="install-the-custom-vision-sdk"></a>Özel Görüntü İşleme SDK’sını yükleme

Go için özel görüntü işleme hizmeti SDK'sını yüklemek için PowerShell'de aşağıdaki komutu çalıştırın:

```
go get -u github.com/Azure/azure-sdk-for-go/...
```

veya deponuzu içinde çalıştırmak, dep kullanıyorsanız:
```
dep ensure -add github.com/Azure/azure-sdk-for-go
```

[!INCLUDE [get-keys](includes/get-keys.md)]

[!INCLUDE [python-get-images](includes/python-get-images.md)]

## <a name="add-the-code"></a>Kod ekleme

Adlı yeni bir dosya oluşturun *sqlserversample* tercih edilen proje dizininizde.

### <a name="create-the-custom-vision-service-project"></a>Özel Görüntü İşleme hizmeti projesi oluşturma

Yeni bir Özel Görüntü İşleme hizmeti projesi oluşturmak için betiğinize aşağıdaki kodu ekleyin. Abonelik anahtarlarınızı uygun tanımlara ekleyin.

```go
import(
    "context"
    "bytes"
    "fmt"
    "io/ioutil"
    "path"
    "log"
    "time"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v3.0/customvision/training"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v3.0/customvision/prediction"
)

var (
    training_key string = "<your training key>"
    prediction_key string = "<your prediction key>"
    prediction_resource_id = "<your prediction resource id>"
    endpoint string = "https://southcentralus.api.cognitive.microsoft.com"
    project_name string = "Go Sample Project"
    iteration_publish_name = "classifyModel"
    sampleDataDirectory = "<path to sample images>"
)

func main() {
    fmt.Println("Creating project...")

    ctx = context.Background()

    trainer := training.New(training_key, endpoint)

    project, err := trainer.CreateProject(ctx, project_name, "sample project", nil, string(training.Multilabel))
    if (err != nil) {
        log.Fatal(err)
    }
```

### <a name="create-tags-in-the-project"></a>Projede etiketler oluşturma

Projenize sınıflandırma etiketleri oluşturmak için sonuna aşağıdaki kodu ekleyin *sqlserversample*:

```go
    // Make two tags in the new project
    hemlockTag, _ := trainer.CreateTag(ctx, *project.ID, "Hemlock", "Hemlock tree tag", string(training.Regular))
    cherryTag, _ := trainer.CreateTag(ctx, *project.ID, "Japanese Cherry", "Japanese cherry tree tag", string(training.Regular))
```

### <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Projeye örnek görüntüleri eklemek için etiket oluşturduktan sonra aşağıdaki kodu ekleyin. Bu kod, her görüntüyü ilgili etiketiyle birlikte karşıya yükler. Bilişsel hizmetler Go SDK örnekleri proje indirdiğiniz üzerinde temel temel görüntü URL yolunu girmeniz gerekir.

> [!NOTE]
> Bilişsel hizmetler Go SDK örnekleri proje daha önce indirdiğiniz üzerinde temel görüntülerin yolunu değiştirmeniz gerekir.

```go
    fmt.Println("Adding images...")
    japaneseCherryImages, err := ioutil.ReadDir(path.Join(sampleDataDirectory, "Japanese Cherry"))
    if err != nil {
        fmt.Println("Error finding Sample images")
    }

    hemLockImages, err := ioutil.ReadDir(path.Join(sampleDataDirectory, "Hemlock"))
    if err != nil {
        fmt.Println("Error finding Sample images")
    }

    for _, file := range hemLockImages {
        imageFile, _ := ioutil.ReadFile(path.Join(sampleDataDirectory, "Hemlock", file.Name()))
        imageData := ioutil.NopCloser(bytes.NewReader(imageFile))

        trainer.CreateImagesFromData(ctx, *project.ID, imageData, []string{ hemlockTag.ID.String() })
    }

    for _, file := range japaneseCherryImages {
        imageFile, _ := ioutil.ReadFile(path.Join(sampleDataDirectory, "Japanese Cherry", file.Name()))
        imageData := ioutil.NopCloser(bytes.NewReader(imageFile))
        trainer.CreateImagesFromData(ctx, *project.ID, imageData, []string{ cherryTag.ID.String() })
    }
```

### <a name="train-the-classifier-and-publish"></a>Sınıflandırıcı eğitin ve yayımlayın

Bu kod, ilk yineleme projede oluşturur ve ardından bu yineleme tahmin uç noktaya yayımlar. Yayımlanmış bir yineleme için verilen ad, tahmin istekleri göndermek için kullanılabilir. Yineleme yayımlanmadan tahmin uç noktasında kullanılabilir değil.

```go
    fmt.Println("Training...")
    iteration, _ := trainer.TrainProject(ctx, *project.ID)
    for {
        if *iteration.Status != "Training" {
            break
        }
        fmt.Println("Training status: " + *iteration.Status)
        time.Sleep(1 * time.Second)
        iteration, _ = trainer.GetIteration(ctx, *project.ID, *iteration.ID)
    }
    fmt.Println("Training status: " + *iteration.Status)

    trainer.PublishIteration(ctx, *project.ID, *iteration.ID, iteration_publish_name, prediction_resource_id))
```

### <a name="get-and-use-the-published-iteration-on-the-prediction-endpoint"></a>Edinin ve öngörü uç noktasında yayımlanan yineleme kullanın

Tahmin uç noktasına bir görüntü göndermek ve tahmini almak için dosyanın sonuna aşağıdaki kodu ekleyin:

```go
    fmt.Println("Predicting...")
    predictor := prediction.New(prediction_key, endpoint)

    testImageData, _ := ioutil.ReadFile(path.Join(sampleDataDirectory, "Test", "test_image.jpg"))
    results, _ := predictor.ClassifyImage(ctx, *project.ID, iteration_publish_name, ioutil.NopCloser(bytes.NewReader(testImageData)), "")

    for _, prediction := range *results.Predictions {
        fmt.Printf("\t%s: %.2f%%", *prediction.TagName, *prediction.Probability * 100)
        fmt.Println("")
    }
}
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Çalıştırma *sqlserversample*.

```powershell
go run sample.go
```

Uygulamanın çıkışı aşağıdaki metne benzer olmalıdır:

```
Creating project...
Adding images...
Training...
Training status: Training
Training status: Training
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```

Ardından test görüntüsünün (**<base_image_url>/Images/Test/** yolunda bulunur) düzgün etiketlendiğini doğrulayabilirsiniz. Ayrıca [Özel Görüntü İşleme web sitesine](https://customvision.ai) geri dönebilir ve yeni oluşturulan projenizin geçerli durumunu görebilirsiniz.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık kodda görüntü sınıflandırma işleminin her adımının nasıl uygulanabileceğini gördünüz. Bu örnek tek bir eğitim yinelemesi yürütür ama modelinizin daha doğru olmasını sağlamak için çoğunlukla birden çok kez eğitmeniz ve test etmeniz gerekecektir.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](test-your-model.md)