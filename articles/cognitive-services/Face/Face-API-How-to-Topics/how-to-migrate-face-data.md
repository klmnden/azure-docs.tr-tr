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
ms.openlocfilehash: 02e9b64c89eda1471d644e0116bbf8c1c061ccc3
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64682527"
---
# <a name="migrate-your-face-data-to-a-different-face-subscription"></a>Yüz tanıma verileriniz farklı bir yüz aboneliğe geçirme

Bu kılavuz, yüz tanıma veri taşıma gösterir (kaydedilmiş gibi **PersonGroup** dikdörtgenlerini) anlık görüntü özelliğini kullanarak farklı bir yüz tanıma API'si aboneliğe. Bu sayede sürekli oluşturma ve eğitme zorunda kalmamak bir **PersonGroup** veya **FaceList** taşırken veya işlemlerinizi genişletme. Örneğin, oluşturmuş olabileceğiniz bir **PersonGroup** bir ücretsiz deneme sürümü aboneliğine kaydolup şimdi istediğiniz kullanarak Ücretli aboneliğinizi geçirmek veya büyük ölçekli işlem için bölgelere face veri eşitlemesine izin gerekebilir.

Bu aynı geçiş stratejisi için de geçerlidir. **LargePersonGroup** ve **LargeFaceList** nesneleri. Bu kılavuzdaki kavramlar hakkında bilgi sahibi değilseniz, bunların tanımlarını görmek [yüz tanıma kavramları](../concepts/face-recognition.md) Kılavuzu. Bu kılavuz, yüz tanıma API'si .NET istemci kitaplığı ile kullanır C#.

## <a name="prerequisites"></a>Önkoşullar

- İki yüz tanıma API'si Abonelik anahtarları (mevcut verilerle ve geçirmek için bir). Bölümündeki yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yüz tanıma API'si hizmete abone ve anahtarınızı alın.
- Hedef aboneliği için karşılık gelen yüz tanıma API'si abonelik kimliği dizesi (bulunan **genel bakış** Azure portalındaki dikey). 
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü.

## <a name="create-the-visual-studio-project"></a>Visual Studio projesini oluşturma

Bu kılavuz, yüz tanıma veri geçişi yürütmek için bir basit bir konsol uygulaması kullanır. Tam bir uygulama için bkz: [yüz tanıma API'si anlık görüntü örnek](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) GitHub üzerinde.

1. Visual Studio'da yeni bir oluşturma **konsol uygulaması (.NET Framework)** adlandırın ve proje **FaceApiSnapshotSample**.
1. Gereken NuGet paketlerini alın. Çözüm Gezgini'nde projenize sağ tıklayıp **NuGet paketlerini Yönet**. Tıklayın **Gözat** sekmenize **ön sürümü dahil et**; ardından bulun ve aşağıdaki paketi yükleyin:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.3.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)

## <a name="create-face-clients"></a>Yüz tanıma istemcileri oluşturma

İçinde **ana** yönteminde *Program.cs*, iki **[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)** örnekleri için kaynak ve hedef abonelikler. Bu örnekte, Doğu Asya bölgesi kaynağı olarak bir yüz tanıma aboneliğinin ve Batı ABD abonelik hedef olarak kullanacağız. Bu, verileri bir Azure bölgesinden diğerine geçirmek nasıl sürdürebileceğiniz gösterilecek. Aboneliklerinizi farklı bölgelerde bulunuyorsa, değiştirmem gerekecek mi `Endpoint` dizeleri.

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

Abonelik anahtarı değerleri ve kaynak ve hedef abonelikler uç nokta URL'lerinin doldurmanız gerekir.


## <a name="prepare-a-persongroup-for-migration"></a>Bir PersonGroup geçiş için hazırlama

Kimliği ihtiyacınız **PersonGroup** kaynak aboneliğinize geçirmek için hedef aboneliği. Kullanım **[PersonGroupOperationsExtensions.ListAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperationsextensions.listasync?view=azure-dotnet)** yönteminin listesini almak için **PersonGroup** nesnelerini; daha sonra **[ PersonGroup.PersonGroupId](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.persongroup.persongroupid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_Face_Models_PersonGroup_PersonGroupId)** özelliği. Bu işlemin ne bağlı olarak farklı görünecektir **PersonGroup** sahip nesneleri. Bu kılavuz, kaynak **PersonGroup** kimliği depolanan `personGroupId`.

> [!NOTE]
> [Örnek kod](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) oluşturur ve yeni bir eğitir **PersonGroup** ancak, geçiş için çoğu zaman zaten olmalıdır bir **PersonGroup** kullanılacak.

## <a name="take-snapshot-of-persongroup"></a>PersonGroup anlık görüntüsünü alın

Anlık görüntü belirli yüz veri türleri için geçici bir uzak depolamadır. Pano verileri bir abonelikten diğerine kopyalamak için bir tür olarak işlev görür. İlk kullanıcı "verilerin bir anlık görüntüsünü kaynak aboneliği alır" ve ardından ", hedef abonelikte yeni bir veri nesnesi uygulandıkları".

Kaynak abonelik kullanmak **FaceClient** bir anlık görüntüsünü almak için örnek **PersonGroup**kullanarak **[TakeAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperationsextensions.takeasync?view=azure-dotnet)** ile **PersonGroup** kimliği ve hedef abonelik kimliği Birden çok hedef abonelik varsa, dizi girişleri üçüncü parametre olarak ekleyebilirsiniz.

```csharp
var takeSnapshotResult = await FaceClientEastAsia.Snapshot.TakeAsync(
    SnapshotObjectType.PersonGroup,
    personGroupId,
    new[] { "<Azure West US Subscription ID>" /* Put other IDs here, if multiple target subscriptions wanted */ });
```

> [!NOTE]
> Alma ve anlık görüntü uygulama işlemini kaynak veya hedef normal çağrıları kesintiye **PersonGroup**s (veya **FaceList**s). Kaynak nesne değiştirme eş zamanlı çağrı yaparak ancak önermiyoruz ([FaceList yönetim çağrılarını](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.facelistoperations?view=azure-dotnet) veya [PersonGroup eğitme](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperations?view=azure-dotnet) , örneğin arama), anlık görüntü işlemi olabilir önce veya sonra bu işlemleri yürütmek ya da hatalarla karşılaşabilirsiniz.

## <a name="retrieve-the-snapshot-id"></a>Anlık görüntü kimliği alınamıyor

Anlık görüntü alma yöntemi zaman uyumsuz olduğundan, (anlık görüntü işlemleri iptal edilemez) tamamlanmasını bekle gerekecektir. Bu kodda, `WaitForOperation` yöntemi zaman uyumsuz çağrının durumu denetleniyor izleyen her 100ms. İşlem tamamlandığında, bir işlem kimliğini almanız mümkün olmayacak Ayrıştırarak edinebilirsiniz `OperationLocation` alan. 

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

Olarak işlem durumunu işaretlendiğinde `Succeeded`, anlık görüntü kimliği ayrıştırarak ardından alabilirsiniz `ResourceLocation` alan döndürülen **OperationStatus** örneği.

```csharp
var snapshotId = Guid.Parse(operationStatus.ResourceLocation.Split('/')[2]);
```

Tipik bir `resourceLocation` değeri şu şekilde görünür:

```csharp
"/snapshots/e58b3f08-1e8b-4165-81df-aa9858f233dc"
```

## <a name="apply-snapshot-to-target-subscription"></a>Anlık görüntü için hedef aboneliği Uygula

Ardından, yeni bir tane oluşturmanız **PersonGroup** hedef abonelikte rastgele oluşturulmuş bir kimliği kullanarak Sonra hedef aboneliğin kullanın **FaceClient** anlık görüntü bu PersonGroup için geçirme anlık görüntü kimliği ve yeni uygulamak için örnek **PersonGroup** kimliği 

```csharp
var newPersonGroupId = Guid.NewGuid().ToString();
var applySnapshotResult = await FaceClientWestUS.Snapshot.ApplyAsync(snapshotId, newPersonGroupId);
```


> [!NOTE]
> Bir anlık görüntü nesnesi yalnızca 48 saat boyunca geçerlidir. Veri geçişi için kullanmak istiyorsanız, yalnızca bir anlık görüntü uygulamanız gereken hemen sonra.

Bir anlık görüntü Uygula isteği başka bir işlem kimliğini döndürür Bu kimliği ayrıştırarak alabileceğiniz `OperationLocation` alan döndürülen **applySnapshotResult** örneği. 

```csharp
var applyOperationId = Guid.Parse(applySnapshotResult.OperationLocation.Split('/')[2]);
```

Anlık görüntü uygulama işlemini de zaman uyumsuz, bu nedenle yeniden kullanmak `WaitForOperation` tamamlanmasını beklemek için.

```csharp
operationStatus = await WaitForOperation(FaceClientWestUS, applyOperationId);
```

## <a name="test-the-data-migration"></a>Test veri geçişi

Anlık görüntü uyguladığınız sonra yeni **PersonGroup** hedef abonelik özgün yüz verilerle doldurulur. Varsayılan olarak, eğitim sonuçları da kopyalanır, böylece yeni **PersonGroup** yeniden eğitme gerek kalmadan yüz kimliği çağrıları için hazır olacaktır.

Veri taşıma işlemini test etmek için aşağıdaki işlemleri çalıştırmak ve konsola yazdırma sonuçlarını karşılaştırın.

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

Yeni kullanmaya başlamak şimdi **PersonGroup** hedef aboneliği. 

Hedef güncelleştirmek isterseniz **PersonGroup** yeniden gelecekte yeni oluşturmak ihtiyacınız olacak **PersonGroup** (Bu kılavuzun adımları izleyerek) anlık görüntü almak için. Tek bir **PersonGroup** nesne yalnızca bir kez uygulanmış bir anlık görüntü olabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Yüz tanıma verileri geçirmeyi tamamladıktan sonra anlık görüntü nesneyi el ile silmeniz önerilir.

```csharp
await FaceClientEastAsia.Snapshot.DeleteAsync(snapshotId);
```

## <a name="next-steps"></a>Sonraki adımlar

Ardından, anlık görüntü özelliği kullanan bir örnek uygulamayı inceleme veya belirtilen API işlemleri burada kullanmaya başlamak için bir nasıl yapılır Kılavuzu izleyin ilgili API başvuru belgelerine bakın.

- [Anlık görüntü başvuru belgeleri (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)
- [Yüz tanıma API'si anlık görüntü örneği](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample)
- [Yüzleri ekleme](how-to-add-faces.md)
- [Görüntüdeki Yüzleri Algılama](HowtoDetectFacesinImage.md)
- [Görüntüdeki yüzleri belirlemek için nasıl](HowtoIdentifyFacesinImage.md)
