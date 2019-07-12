---
title: 'Hızlı Başlangıç: .NET için soru-cevap Oluşturucu istemci kitaplığı'
titlesuffix: Azure Cognitive Services
description: .NET için soru-cevap Oluşturucu istemci kitaplığını kullanmaya başlayın. Temel görevleri için örnek kod deneyin ve paketi yüklemek için aşağıdaki adımları izleyin.  Soru-Cevap Oluşturma, SSS belgeleri, URL'ler ve ürün kılavuzları gibi yarı yapılandırılmış içeriklerinizden bir soru cevap hizmeti oluşturmanızı sağlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 07/10/2019
ms.author: diberry
ms.openlocfilehash: f38cbfa0efd3b9be9ca091942dca7ffcd91b6019
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828123"
---
# <a name="quickstart-qna-maker-client-library-for-net"></a>Hızlı Başlangıç: .NET için soru-cevap Oluşturucu istemci kitaplığı

.NET için soru-cevap Oluşturucu istemci kitaplığını kullanmaya başlayın. Temel görevleri için örnek kod deneyin ve paketi yüklemek için aşağıdaki adımları izleyin.  Soru-Cevap Oluşturma, SSS belgeleri, URL'ler ve ürün kılavuzları gibi yarı yapılandırılmış içeriklerinizden bir soru cevap hizmeti oluşturmanızı sağlar. 

Soru-cevap Oluşturucu istemci kitaplığı için .NET için kullanın:

* Bilgi bankası oluşturma 
* Bilgi Bankası'nı yönetme
* Bilgi bankası yayımlama

[Başvuru belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker?view=azure-dotnet) | [kitaplığı kaynak kodunu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Knowledge.QnAMaker) | [paketini (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker/)  |  [ C# örnekleri](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği - [ücretsiz oluşturun](https://azure.microsoft.com/free/)
* Geçerli sürümü [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-qna-maker-azure-resource"></a>Soru-cevap Oluşturucu Azure kaynağı oluşturma

Azure Bilişsel hizmetler, abone olduğunuz Azure kaynaklarını tarafından temsil edilir. Soru-cevap Oluşturucu kullanmak için bir kaynak oluşturmak [Azure portalında](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) yerel makinenizde. 

Sonra kaynak, bir anahtar alınırken [bir ortam değişkenini oluşturmak](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) anahtarı için adlı `QNAMAKER_SUBSCRIPTION_KEY`.

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulama

Tercih edilen Düzenleyicisi veya IDE içinde yeni bir .NET Core uygulaması oluşturun. 

Dotnet (örneğin, cmd, PowerShell veya Bash) bir konsol penceresinde kullanın `new` adıyla yeni bir konsol uygulaması oluşturmak için komut `qna-maker-quickstart`. Bu komut, bir basit "Hello World" oluşturur C# tek bir dosya ile proje: `Program.cs`. 

```console
dotnet new console -n qna-maker-quickstart
```

Yeni oluşturulan uygulama klasörüne dizin değiştirin. İle uygulama oluşturabilirsiniz:

```console
dotnet build
```

Yapı çıkış, uyarı veya hata içermelidir. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```


### <a name="install-the-sdk"></a>SDK yükle

Uygulama dizininde, soru-cevap Oluşturucu istemci kitaplığı için .NET şu komutla yükleyin:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker --version 1.0.0
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığının indirilebilir bir NuGet paketi olarak kullanılabilir.


## <a name="object-model"></a>Nesne modeli

Soru-cevap Oluşturucu istemci bir [QnAMakerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet) anahtarınızı içeren Microsoft.Rest.ServiceClientCredentials kullanarak Azure'da kimlik doğrulayan bir nesne.

İstemci oluşturulduktan sonra kullanmak [Bilgi Bankası](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_QnAMakerClient_Knowledgebase) özelliği oluşturmak, yönetmek ve, Bilgi Bankası yayımlama. 

Bilgi bankanızı bir JSON nesnesi göndererek yönetin. Hemen işlemlerinde, bir yöntem genellikle durumunu gösteren bir JSON nesnesi döndürür. Uzun süre çalışan işlemler için işlem kimliğini yanıt. Çağrı [istemci. Operations.GetDetailsAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.operationsextensions.getdetailsasync?view=azure-dotnet) belirlemek için işlem kimliği yöntemiyle [isteğinin durumunu](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operationstatetype?view=azure-dotnet). 

 
## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, soru-cevap Oluşturucu istemci kitaplığı ile .NET için aşağıdakileri yapın gösterilmektedir:

* [Bilgi bankası oluşturma](#create-a-knowledge-base)
* [Bilgi bankası yükleme](#update-a-knowledge-base)
* [Bilgi Bankası'nı indirin](#download-a-knowledge-base)
* [Bilgi bankası yayımlama](#publish-a-knowledge-base)
* [Bilgi Bankası Sil](#delete-a-knowledge-base)
* [Bir işlemin durumunu alın](#get-status-of-an-operation)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleyin

Proje dizininden açın **Program.cs** , tercih edilen düzenleyicimizi veya IDE'mizi dosyasında. Varolan `using` kodu şu kodla `using` yönergeleri:

[!code-csharp[Using statements](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=Dependencies)]

## <a name="authenticate-the-client"></a>İstemci kimlik doğrulaması

İçinde **ana** yöntemi, adlandırılmış bir ortam değişkenine çekilen kaynağınızın Azure anahtarı için bir değişken oluşturun `QNAMAKER_SUBSCRIPTION_KEY`. Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, düzenleyici, IDE ya da çalışan kabuğunu kapatılmasını ve değişken erişmek için yeniden gerekir. Yöntemi daha sonra oluşturulur.

Ardından, oluşturun bir [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.apikeyserviceclientcredentials?view=azure-dotnet) nesne anahtarınızla ve uç noktanız ile oluşturmak için kullanmak bir [QnAMakerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet) nesne.

Anahtarınızı içinde değilse `westus` konumunu değiştirmek, bölge, bu örnek kodun da gösterdiği **uç nokta** değişkeni. Bu konumda bulunur **genel bakış** soru-cevap Oluşturucu kaynağınızın Azure portalında sayfa.

[!code-csharp[Authorization to resource key](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=Authorization)]

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

Bilgi Bankası için soru ve yanıt çiftleri depolar [CreateKbDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.createkbdto?view=azure-dotnet) üç kaynaklardan nesnesi:

* İçin **editör içerikleri**, kullanın [QnADTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadto?view=azure-dotnet) nesne.
* İçin **dosyaları**, kullanın [FileDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.filedto?view=azure-dotnet) nesne. 
* İçin **URL'leri**, dizeleri listesini kullanın.

Çağrı [CreateAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.createasync?view=azure-dotnet) yöntemi ardından döndürülen işlem Kimliğine geçirin [MonitorOperation](#get-status-of-an-operation) durumunu yoklamak için yöntemi. 

Son satırı aşağıdaki kod, Bilgi Bankası kimliği MonitorOoperation yanıtı döndürür. 

[!code-csharp[Create a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=CreateKB&highlight=29,30)]

## <a name="update-a-knowledge-base"></a>Bilgi bankası güncelleştirme

Bilgi Bankası kimliği girerek bir Bilgi Bankası güncelleştirebilir ve bir [UpdatekbOperationDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdto?view=azure-dotnet) içeren [Ekle](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoadd?view=azure-dotnet), [güncelleştirme](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoupdate?view=azure-dotnet), ve [Sil](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtodelete?view=azure-dotnet)DTO nesnelerini [UpdateAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.updateasync?view=azure-dotnet) yöntemi. Kullanım [MonitorOperation](#get-status-of-an-operation) Güncelleştirme başarılı olup olmadığını belirlemek için yöntemi.

[!code-csharp[Update a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=UpdateKB&highlight=4,13)]

## <a name="download-a-knowledge-base"></a>Bilgi Bankası'nı indirin

Kullanım [DownloadAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.downloadasync?view=azure-dotnet) veritabanı listesi olarak indirmek için yöntemi [QnADocumentsDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadocumentsdto?view=azure-dotnet). Bu _değil_ soru-cevap Oluşturucu portal'ın dışarı aktarma eşdeğer **ayarları** bu yöntemin sonucu TSV dosyasını olmadığı için sayfa.

[!code-csharp[Download a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=DownloadKB&highlight=2)]

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

Kullanarak Bilgi Bankası yayımlama [PublishAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.publishasync?view=azure-dotnet) yöntemi. Bu Bilgi Bankası kimliği tarafından başvurulan geçerli kaydedilmiş ve eğitilen modeli alır ve bir uç noktada, yayımlar. 

[!code-csharp[Publish a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=PublishKB&highlight=2)]

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi Bankası kullanarak silme [DeleteAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.deleteasync?view=azure-dotnet) ile Bilgi Bankası kimliği parametresi yöntemi 

[!code-csharp[Delete a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=DeleteKB&highlight=2)]

## <a name="get-status-of-an-operation"></a>Bir işlemin durumunu alın

Bazı yöntemler gibi oluşturur ve güncelleştirme yeterli, işlemin tamamlanması beklenirken yerine sürebilir bir [işlemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation?view=azure-dotnet) döndürülür. Kullanım [işlem kimliği](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation.operationid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_Operation_OperationId) (yeniden deneme mantığı ile) özgün yöntemi durumunu belirlemek için yoklama işlemi. 

_Döngü_ ve _Task.Delay_ aşağıdaki kod bloğunda yeniden deneme mantığı benzetimini yapmak için kullanılır. Bu, kendi yeniden deneme mantığı ile değiştirilmelidir. 

[!code-csharp[Monitor an operation](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=MonitorOperation&highlight=10)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Dotnet ile uygulamayı çalıştırmak `run` uygulama dizininize komutu.

```dotnet
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Temizleme ve Bilişsel hizmetler abonelik kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili kaynakları siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Öğretici: Oluşturma ve bir KB yanıt](../tutorials/create-publish-query-in-portal.md)

* [Soru-cevap Oluşturucu API'si nedir?](../Overview/overview.md)
* [Bilgi Bankası Düzenle](../how-to/edit-knowledge-base.md)
* [Kullanım analizi alın](../how-to/get-analytics-knowledge-base.md)
* Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/blob/master/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs).