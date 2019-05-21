---
title: Yüz tanıma verileriniz abonelikler - yüz tanıma API'si arasında geçirme
titleSuffix: Azure Cognitive Services
description: Bu kılavuz, depolanan yüz tanıma verileriniz bir yüz tanıma API'si abonelikten diğerine geçirmek nasıl gösterir.
services: cognitive-services
author: lewlu
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: lewlu
ms.openlocfilehash: 702aed12860c090e83b997e6b56d56e06b416568
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65913797"
---
# <a name="migrate-your-face-data-to-a-different-face-subscription"></a>Yüz tanıma verileriniz farklı bir yüz aboneliğe geçirme

Bu kılavuz, farklı Azure Bilişsel hizmetler yüz tanıma API'si abonelik için kaydedilmiş PersonGroup bir yüz, nesneyle gibi yüz veri taşıma gösterilir. Verileri taşımak için anlık görüntü özelliğini kullanın. Bu şekilde, sürekli derleme ve işlemlerinizi genişletin veya taşıdığınızda PersonGroup veya FaceList nesne eğitme olmamasına özen gösterin. Örneğin, belki de PersonGroup nesne ücretsiz bir deneme aboneliği kullanarak oluşturduğunuz ve artık Ücretli aboneliğinizi geçiş yapmak istiyorsanız. Veya, büyük ölçekli işlem için bölgelere face veri eşitlemesine izin gerekebilir.

Bu aynı geçiş stratejisi LargePersonGroup ve LargeFaceList nesneleri için de geçerlidir. Bu kılavuzdaki kavramlar hakkında bilgi sahibi değilseniz, bunların tanımlarını görmek [yüz tanıma kavramları](../concepts/face-recognition.md) Kılavuzu. Bu kılavuz, yüz tanıma API'si .NET istemci kitaplığı ile kullanır C#.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki öğeler gerekir:

- İki yüz tanıma API'si Abonelik anahtarları, var olan verilere sahip ve geçirmek için bir tane. Yüz tanıma API'si hizmete abone ve anahtarınızı almak için yönergeleri izleyin. [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
- Hedef aboneliği için karşılık gelen yüz tanıma API'si abonelik kimliği dizesi. Bunu bulmak için seçin **genel bakış** Azure portalında. 
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü.

## <a name="create-the-visual-studio-project"></a>Visual Studio projesini oluşturma

Bu kılavuz, yüz tanıma veri geçişi çalıştırmak için basit bir konsol uygulaması kullanır. Tam bir uygulama için bkz: [yüz tanıma API'si anlık görüntü örnek](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) GitHub üzerinde.

1. Visual Studio'da yeni bir konsol uygulama .NET Framework projesi oluşturun. Adlandırın **FaceApiSnapshotSample**.
1. Gereken NuGet paketlerini alın. Çözüm Gezgini'nde projenize sağ tıklayıp seçin **NuGet paketlerini Yönet**. Seçin **Gözat** sekmesine tıklayın ve **ön sürümü dahil et**. Bulun ve şu paket yükleyin:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.3.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)

## <a name="create-face-clients"></a>Yüz tanıma istemcileri oluşturma

İçinde **ana** yönteminde *Program.cs*, iki [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) örnekleri için kaynak ve hedef abonelikler. Bu örnek, kaynak ve hedef olarak Batı ABD abonelik olarak Doğu Asya bölgesinde bir yüz abonelik kullanır. Bu örnek, verileri bir Azure bölgesinden diğerine geçirmek nasıl gösterir. Aboneliklerinizi farklı bölgelerde bulunuyorsa, değiştirme `Endpoint` dizeleri.

```csharp
var FaceClientEastAsia = new FaceClient(new ApiKeyServiceClientCredentials("<East Asia Subscription Key>"))
    {
        Endpoint = "https://southeastasia.api.cognitive.microsoft.com/>"
    };

var FaceClientWestUS = new FaceClient(new ApiKeyServiceClientCredentials("<West US Subscription Key>"))
    {
        Endpoint = "https://westus.api.cognitive.microsoft.com/"
    };
```

Kaynak ve hedef abonelikler için uç nokta URL'leri ve abonelik anahtar değerlerini doldurun.


## <a name="prepare-a-persongroup-for-migration"></a>Bir PersonGroup geçiş için hazırlama

Hedef aboneliğine geçirmek için kaynak aboneliğinizdeki PersonGroup kimliği gerekir. Kullanım [PersonGroupOperationsExtensions.ListAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperationsextensions.listasync?view=azure-dotnet) PersonGroup nesnelerinin bir listesini almak için yöntemi. Daha sonra [PersonGroup.PersonGroupId](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.persongroup.persongroupid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_Face_Models_PersonGroup_PersonGroupId) özelliği. Bu işlem üzerinde hangi PersonGroup göre farklı görünüyor sahip nesneleri. Bu kılavuzda, kaynak PersonGroup kimliği depolanan `personGroupId`.

> [!NOTE]
> [Örnek kod](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) oluşturur ve geçirmek için yeni bir PersonGroup eğitir. Çoğu durumda, kullanılacak bir PersonGroup olmanız gerekir.

## <a name="take-a-snapshot-of-a-persongroup"></a>Bir PersonGroup anlık görüntüsünü alma

Anlık görüntü belirli yüz veri türleri için geçici bir uzak depolama alanıdır. Pano verileri bir abonelikten diğerine kopyalamak için bir tür olarak işlev görür. İlk olarak, kaynak abonelikte veri anlık görüntüsünü alın. Sonra hedef abonelikte yeni bir veri nesnesine uygulayın.

Kaynak abonelik FaceClient örneği PersonGroup bir anlık görüntüsünü almak için kullanın. Kullanım [TakeAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperationsextensions.takeasync?view=azure-dotnet) PersonGroup kimliği ve hedef abonelik kimliği ile Birden çok hedef abonelik varsa, dizi girişleri üçüncü parametre olarak ekleyin.

```csharp
var takeSnapshotResult = await FaceClientEastAsia.Snapshot.TakeAsync(
    SnapshotObjectType.PersonGroup,
    personGroupId,
    new[] { "<Azure West US Subscription ID>" /* Put other IDs here, if multiple target subscriptions wanted */ });
```

> [!NOTE]
> Alma ve anlık görüntü uygulama işlemini kaynak normal çağrıları kesintiye değil veya kişi veya belirlenmiştir. Kaynak nesne gibi değiştirmek eş zamanlı çağrı yapmayın [FaceList yönetim çağrılarını](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.facelistoperations?view=azure-dotnet) veya [PersonGroup eğitme](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperations?view=azure-dotnet) , örneğin çağırın. Anlık görüntü işlemi öncesinde veya sonrasında bu işlemlerin çalışabilir veya hatalarla karşılaşabilirsiniz.

## <a name="retrieve-the-snapshot-id"></a>Anlık görüntü kimliği alınamıyor

Anlık görüntülerini almak için kullanılan yöntemin zaman uyumsuz olduğundan bunun tamamlanmasını beklemeniz gerekir. Anlık görüntü işlemleri iptal edilemez. Bu kodda, `WaitForOperation` yöntemi zaman uyumsuz çağrı izler. Her 100 ms durumunu denetler. İşlem tamamlandıktan sonra bir işlem kimliği ayrıştırarak almak `OperationLocation` alan. 

```csharp
var takeOperationId = Guid.Parse(takeSnapshotResult.OperationLocation.Split('/')[2]);
var operationStatus = await WaitForOperation(FaceClientEastAsia, takeOperationId);
```

Tipik bir `OperationLocation` değeri şu şekilde görünür:

```csharp
"/operations/a63a3bdd-a1db-4d05-87b8-dbad6850062a"
```

`WaitForOperation` Yardımcı yöntemdir burada:

```csharp
/// <summary>
/// Waits for the take/apply operation to complete and returns the final operation status.
/// </summary>
/// <returns>The final operation status.</returns>
private static async Task<OperationStatus> WaitForOperation(IFaceClient client, Guid operationId)
{
    OperationStatus operationStatus = null;
    do
    {
        if (operationStatus != null)
        {
            Thread.Sleep(TimeSpan.FromMilliseconds(100));
        }

        // Get the status of the operation.
        operationStatus = await client.Snapshot.GetOperationStatusAsync(operationId);

        Console.WriteLine($"Operation Status: {operationStatus.Status}");
    }
    while (operationStatus.Status != OperationStatusType.Succeeded
            && operationStatus.Status != OperationStatusType.Failed);

    return operationStatus;
}
```

Sonra işlem durumunu gösterir `Succeeded`, ayrıştırarak anlık görüntü Kimliğini alın `ResourceLocation` döndürülen OperationStatus örnek alan.

```csharp
var snapshotId = Guid.Parse(operationStatus.ResourceLocation.Split('/')[2]);
```

Tipik bir `resourceLocation` değeri şu şekilde görünür:

```csharp
"/snapshots/e58b3f08-1e8b-4165-81df-aa9858f233dc"
```

## <a name="apply-a-snapshot-to-a-target-subscription"></a>Anlık görüntü bir hedef aboneliğine Uygula

Ardından, yeni PersonGroup rastgele oluşturulmuş bir kimliğini kullanarak hedef abonelikte oluşturun Ardından bu PersonGroup için anlık görüntü uygulamak için hedef aboneliğin FaceClient örneği kullanın. Anlık görüntüde kimliği ve yeni PersonGroup kimliği geçirin.

```csharp
var newPersonGroupId = Guid.NewGuid().ToString();
var applySnapshotResult = await FaceClientWestUS.Snapshot.ApplyAsync(snapshotId, newPersonGroupId);
```


> [!NOTE]
> Bir anlık görüntü nesnesi yalnızca 48 saat için geçerlidir. Veri geçişi için kullanmak istiyorsanız yalnızca anlık hemen sonra.

Bir anlık görüntü Uygula isteği başka bir işlem kimliğini döndürür. Bu kimliği almak için ayrıştırma `OperationLocation` döndürülen applySnapshotResult örnek alan. 

```csharp
var applyOperationId = Guid.Parse(applySnapshotResult.OperationLocation.Split('/')[2]);
```

Anlık görüntü uygulama işlemini de zaman uyumsuz, bu nedenle yeniden kullanmak `WaitForOperation` bitmesini beklemek için.

```csharp
operationStatus = await WaitForOperation(FaceClientWestUS, applyOperationId);
```

## <a name="test-the-data-migration"></a>Test veri geçişi

Anlık görüntü uygulandıktan sonra hedef abonelikte yeni PersonGroup özgün yüz verilerle doldurur. Varsayılan olarak, eğitim sonuçları da kopyalanır. Yeni PersonGroup yeniden eğitme gerek kalmadan yüz kimliği çağrıları için hazırdır.

Veri taşıma işlemini test etmek için aşağıdaki işlemleri çalıştırın ve konsola yazdırma sonuçlarını karşılaştırın:

```csharp
await DisplayPersonGroup(FaceClientEastAsia, personGroupId);
await IdentifyInPersonGroup(FaceClientEastAsia, personGroupId);

await DisplayPersonGroup(FaceClientWestUS, newPersonGroupId);
// No need to retrain the person group before identification,
// training results are copied by snapshot as well.
await IdentifyInPersonGroup(FaceClientWestUS, newPersonGroupId);
```

Aşağıdaki yardımcı yöntemler kullanın:

```csharp
private static async Task DisplayPersonGroup(IFaceClient client, string personGroupId)
{
    var personGroup = await client.PersonGroup.GetAsync(personGroupId);
    Console.WriteLine("Person Group:");
    Console.WriteLine(JsonConvert.SerializeObject(personGroup));

    // List persons.
    var persons = await client.PersonGroupPerson.ListAsync(personGroupId);

    foreach (var person in persons)
    {
        Console.WriteLine(JsonConvert.SerializeObject(person));
    }

    Console.WriteLine();
}
```

```csharp
private static async Task IdentifyInPersonGroup(IFaceClient client, string personGroupId)
{
    using (var fileStream = new FileStream("data\\PersonGroup\\Daughter\\Daughter1.jpg", FileMode.Open, FileAccess.Read))
    {
        var detectedFaces = await client.Face.DetectWithStreamAsync(fileStream);

        var result = await client.Face.IdentifyAsync(detectedFaces.Select(face => face.FaceId.Value).ToList(), personGroupId);
        Console.WriteLine("Test identify against PersonGroup");
        Console.WriteLine(JsonConvert.SerializeObject(result));
        Console.WriteLine();
    }
}
```

Şimdi, hedef abonelikte yeni PersonGroup kullanabilirsiniz. 

' % S'hedef PersonGroup ileride güncelleştirmek için anlık görüntü almak için yeni bir PersonGroup oluşturun. Bunu yapmak için bu kılavuzdaki adımları izleyin. Tek bir PersonGroup nesne yalnızca bir kez uygulanmış bir anlık görüntü olabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Geçiş işlemini tamamladıktan sonra veri yüz tanıma, anlık görüntü nesneyi el ile silin.

```csharp
await FaceClientEastAsia.Snapshot.DeleteAsync(snapshotId);
```

## <a name="next-steps"></a>Sonraki adımlar

Ardından, anlık görüntü özelliği kullanan bir örnek uygulamayı inceleme veya belirtilen API işlemleri burada kullanmaya başlamak için bir nasıl yapılır Kılavuzu izleyin ilgili API başvuru belgelerine bakın:

- [Anlık görüntü başvuru belgeleri (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)
- [Yüz tanıma API'si anlık görüntü örneği](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample)
- [Yüzleri Ekle](how-to-add-faces.md)
- [Görüntüdeki yüzleri algılayın](HowtoDetectFacesinImage.md)
- [Bir görüntüdeki yüzleri belirleme](HowtoIdentifyFacesinImage.md)
