---
title: 'Öğretici: Azure görüntüleri için meta verileri oluşturun'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, Azure görüntü işleme hizmeti görüntüleri için meta verileri oluşturmak için bir web uygulaması tümleştirin öğreneceksiniz.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: a755a0bada0dbf6797465ea40ddbb30a84e3f289
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60006010"
---
# <a name="tutorial-use-computer-vision-to-generate-image-metadata-in-azure-storage"></a>Öğretici: Görüntü işleme, Azure Depolama'da görüntü meta verilerini oluşturmak için kullanın

Bu öğreticide, Azure görüntü işleme hizmeti meta verilerini karşıya yüklenen görüntüleri oluşturmak için bir web uygulamasına tümleştirmeyi öğreneceksiniz. Tam Uygulama Kılavuzu bulunabilir [Azure depolama ve Bilişsel hizmetler Laboratuvar](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md) GitHub ve bu öğreticinin 5 laboratuvarı aslında arka planda çalışma. Her adım izleyerek uçtan uca uygulama oluşturmak istiyor ancak yalnızca görüntü işleme, mevcut bir web uygulamasıyla nasıl tümleştirilebilir görmek istiyorsanız, buradan okuyun.

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Azure'da bir görüntü işleme kaynağı oluşturma
> * Azure depolama görüntülerinde görüntü analizi gerçekleştirin
> * Meta verileri Azure depolama görüntüleri ekleme
> * Görüntü meta verilerini Azure Depolama Gezgini'ni kullanarak denetleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2017 Community edition](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) veya üzeri "ASP.NET ve web geliştirme" ve "Azure geliştirme" iş yükleri yüklü.
- Görüntüler için ayrılmış bir blob kapsayıcısı ile bir Azure depolama hesabı (izleyin [Azure depolama Laboratuvar Egzersizleri 1](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise1) bu adımla ilgili yardıma ihtiyacınız olursa).
- Azure Depolama Gezgini aracını (izleyin [Azure depolama laboratuvarı alıştırması 2](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise2) bu adımla ilgili yardıma ihtiyacınız olursa).
- Azure depolama erişimi olan ASP.NET web uygulaması (izleyin [Azure depolama laboratuvarı alıştırması 3](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise3) böyle bir uygulamayı hızla oluşturmak için).

## <a name="create-a-computer-vision-resource"></a>Görüntü işleme kaynak oluştur

Azure hesabınız için bir görüntü işleme kaynağı oluşturmanız gerekir; Bu kaynak, Azure'un görüntü işleme hizmeti, erişimini yönetir.

1. Oturum [Azure portalı](https://ms.portal.azure.com) tıklatıp **kaynak Oluştur**çizgidir **yapay ZEKA + Machine Learning** ve **görüntü işleme**.

    ![Yeni bir görüntü işleme API'si aboneliği oluşturuluyor](../Images/new-vision-api.png)

1. "İşleme-api-key" iletişim kutusu penceresinde girin **adı** seçin ve alan **F0** olarak **fiyatlandırma katmanı**. Aynı seçin **konumu** Azure depolama hesabınızı ayarladığınızda, seçtiğiniz. Altında **kaynak grubu**seçin **var olanı kullan** ve de aynı kaynak grubunu seçin. Denetleme **onaylıyorum** kutusuna ve ardından **Oluştur**.

    ![Görüntü işleme API'si abone olma](../Images/create-vision-api.png)

1. Kaynak grubunuz için menüsüne geri dönün ve yeni oluşturduğunuz görüntü işleme API'si aboneliğe tıklayın. URL'nin altına kopyalayın **uç nokta** için bir yere, kolayca birazdan alabileceğiniz. Ardından **erişim anahtarlarını gösterme**.

    ![Azure portal sayfasındaki ana hatlarıyla belirtilen uç nokta URL'si ve erişim anahtarları bağlantı](../Images/copy-vision-endpoint.png)

1. Sonraki penceresinde değerini kopyalayın **anahtar 1** panoya.

    ![Anahtarları iletişim özetlenen Kopyala düğmesini ile yönetme](../Images/copy-vision-key.png)

## <a name="add-computer-vision-credentials"></a>Görüntü işleme kimlik bilgilerini ekleyin

Görüntü işleme kaynaklarına erişebilmesi için bundan, gerekli kimlik bilgileri uygulamanıza ekleyeceksiniz

ASP.NET web uygulamanızı Visual Studio'da açın ve gidin **Web.config** projenin köküne dosya. İçin aşağıdaki deyimleri ekleyin `<appSettings>` bölümü dosyasının değiştirerek `VISION_KEY` önceki adımda kopyaladığınız anahtar ile ve `VISION_ENDPOINT` , önce adımda kaydettiğiniz URL'si ile.

```xml
<add key="SubscriptionKey" value="VISION_KEY" />
<add key="VisionEndpoint" value="VISION_ENDPOINT" />
```

Ardından Çözüm Gezgini'nde projeye sağ tıklayın ve kullanmak **NuGet paketlerini Yönet** paketini yüklemek için komutu **Microsoft.Azure.CognitiveServices.Vision.ComputerVision**. Bu paket, görüntü işleme API'sini çağırmak için gereken türleri içerir.

## <a name="add-metadata-generation-code"></a>Meta veri oluşturma kodunu ekleyin

Ardından, görüntüleri için meta verileri oluşturmak için görüntü işleme hizmeti yararlanan kod ekleyeceksiniz. Bu adımları Laboratuvardaki ASP.NET uygulaması için geçerli olacaktır, ancak kendi uygulamanıza uyarlayabilirsiniz. Önemli olan bu noktada, görüntüleri bir Azure depolama kapsayıcısına karşıya yüklemek, görüntüleri, okumak ve bunları görüntülemek bir ASP.NET web uygulamasını olmasıdır. Bu konuda emin değilseniz, izlemek en iyi [Azure depolama laboratuvarı alıştırması 3](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise3). 

1. Açık *HomeController.cs* proje dosyasında **denetleyicileri** klasörü ve aşağıdaki `using` deyimini dosyanın üst:

    ```csharp
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;
    ```

1. Ardından, Git **karşıya** yöntemi; bu yöntem dönüştürür ve blob depolama için karşıya yükleme görüntüleri. İle başlayan blok hemen sonra aşağıdaki kodu ekleyin `// Generate a thumbnail` (veya görüntü blob oluşturma işleminin sonunda). Bu kod, görüntü içeren blob alır (`photo`) ve o yansıma için bir açıklama oluşturmak için görüntü işleme kullanır. Görüntü işleme API'si, aynı zamanda görüntüye uygulamaya anahtar sözcüklerin listesini oluşturur. Böylece daha sonra alınabilir oluşturulan açıklaması ve anahtar sözcükleri blobun meta verilerinde depolanır.

    ```csharp
    // Submit the image to Azure's Computer Vision API
    ComputerVisionClient vision = new ComputerVisionClient(
        new ApiKeyServiceClientCredentials(ConfigurationManager.AppSettings["SubscriptionKey"]),
        new System.Net.Http.DelegatingHandler[] { });
    vision.Endpoint = ConfigurationManager.AppSettings["VisionEndpoint"];

    VisualFeatureTypes[] features = new VisualFeatureTypes[] { VisualFeatureTypes.Description };
    var result = await vision.AnalyzeImageAsync(photo.Uri.ToString(), features);

    // Record the image description and tags in blob metadata
    photo.Metadata.Add("Caption", result.Description.Captions[0].Text);

    for (int i = 0; i < result.Description.Tags.Count; i++)
    {
        string key = String.Format("Tag{0}", i);
        photo.Metadata.Add(key, result.Description.Tags[i]);
    }

    await photo.SetMetadataAsync();
    ```

1. Ardından, Git **dizin** yöntemi aynı dosyada; bu yöntem hedef blob kapsayıcısında depolanan bir resmi blobları numaralandırır (olarak **Ilistblobıtem** örnekleri) ve bunların uygulama görünümüne geçirir. Değiştirin `foreach` aşağıdaki kodla bu yöntemdeki engelleyin. Bu kod **CloudBlockBlob.FetchAttributes** meta verilerini bağlı her blob almak için. Bilgisayar tarafından oluşturulan açıklama ayıklar (`caption`) meta verilerden ve ekler **BlobInfo** görünüme iletilir nesnesidir.
    
    ```csharp
    foreach (IListBlobItem item in container.ListBlobs())
    {
        var blob = item as CloudBlockBlob;
    
        if (blob != null)
        {
            blob.FetchAttributes(); // Get blob metadata
            var caption = blob.Metadata.ContainsKey("Caption") ? blob.Metadata["Caption"] : blob.Name;
    
            blobs.Add(new BlobInfo()
            {
                ImageUri = blob.Uri.ToString(),
                ThumbnailUri = blob.Uri.ToString().Replace("/photos/", "/thumbnails/"),
                Caption = caption
            });
        }
    }
    ```

## <a name="test-the-app"></a>Uygulamayı test edin

Visual Studio ve ENTER tuşuna yaptığınız değişiklikleri kaydetmek **Ctrl + F5** tarayıcınızda uygulamayı başlatmak için. Laboratuvar kaynakları "fotoğrafları" klasöründen veya kendi klasöründen birkaç görüntüleri karşıya yüklemek için uygulama kullanın. İmleç görünümünde görüntülerden birini üzerine geldiğinizde, araç ipucu penceresi görünür ve bilgisayar tarafından oluşturulan açıklamalı alt yazı görüntü için görüntüleme.

![Bilgisayar tarafından oluşturulan açıklamalı alt yazı](../Images/thumbnail-with-tooltip.png)

Tüm bağlı meta verileri görüntülemek için görüntüler için kullandığınız depolama kapsayıcısı görüntülemek için Azure Depolama Gezgini'ni kullanın. BLOB kapsayıcı seçip sağ **özellikleri**. İletişim kutusunda, anahtar-değer çiftlerinin listesini görürsünüz. Bilgisayar tarafından oluşturulan görüntü açıklaması "Başlık" öğesinde depolanır ve arama anahtar sözcükleri "Tag0 içinde" "Etiket1," depolanır ve benzeri. İşlemi tamamladığınızda, tıklayın **iptal** iletişim kutusunu kapatmak için.

![Listelenen meta veri etiketleri ile Görüntü Özellikleri iletişim kutusu penceresi](../Images/blob-metadata.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Web uygulamanız üzerinde çalışmaya devam etmek istiyorsanız, bkz. [sonraki adımlar](#next-steps) bölümü. Bu uygulamayı kullanmaya devam etmeyi planlamıyorsanız, uygulamaya özgü tüm kaynakları silmeniz gerekir. Bunu yapmak için Azure depolama aboneliğiniz ve görüntü işleme kaynak içeren kaynak grubunu silmeniz yeterlidir. Bu depolama hesabı için karşıya yüklenen bloblara ve ASP.NET web uygulaması ile bağlanmak için gereken bir App Service kaynak kaldırır. 

Kaynak grubunu silmek için açın **kaynak grupları** portalındaki dikey penceresinde, bu proje için kullanılan kaynak grubunu gelin ve tıklayın **kaynak grubunu Sil** üstünde görüntüle. Bir kez silinmiş olduğundan, bunu silmek istediğinizde, bir kaynak grubu kurtarılamaz onaylamak için kaynak grubunun adını yazın istenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure'nın görüntü işleme hizmeti karşıya olarak açıklamalı alt yazılar ve anahtar sözcükler için blob görüntüleri otomatik olarak oluşturmak için bir varolan web uygulamasıyla tümleşik. Ardından, Azure depolama, web uygulamanıza arama işlevi eklemeyi öğrenin Laboratuvar, alıştırma 6 bakın. Bu görüntü işleme hizmeti oluşturur arama anahtar sözcükleri yararlanır.

> [!div class="nextstepaction"]
> [Arama uygulamanıza ekleme](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise6)
