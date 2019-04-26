---
title: Karşıya yüklenen görüntülerin yeniden boyutlandırılmasını otomatikleştirmek için Azure Event Grid kullanma | Microsoft Docs
description: Azure Event Grid, Azure Depolama’da blob yüklemelerini tetikleyebilir. Bu hizmeti kullanarak, Azure Depolama’ya yüklenmiş görüntü dosyalarını, yeniden boyutlandırma ve diğer iyileştirmeler için Azure İşlevleri gibi diğer hizmetlere gönderebilirsiniz.
services: event-grid, functions
author: spelluru
manager: jpconnoc
editor: ''
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/29/2019
ms.author: spelluru
ms.custom: mvc
ms.openlocfilehash: 652fe182663d37c389658c8fe3b172826168e51f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60462835"
---
# <a name="tutorial-automate-resizing-uploaded-images-using-event-grid"></a>Öğretici: Karşıya yüklenen görüntüleri yeniden boyutlandırmayı Event Grid kullanarak otomatikleştirme

[Azure Event Grid](overview.md), bulut için bir olay oluşturma hizmetidir. Event Grid, Azure hizmetleri veya üçüncü taraf kaynaklar tarafından başlatılan olaylara abonelikler oluşturmanızı sağlar.  

Bu öğretici, Depolama öğreticileri serisinin ikinci bölümüdür. [Önceki Depolama öğreticisine][previous-tutorial], Azure Event Grid ve Azure İşlevleri’ni kullanarak sunucusuz otomatik küçük resim oluşturma işlemini ekler. Event Grid, [Azure İşlevleri](../azure-functions/functions-overview.md)’nin [Azure Blob depolama](../storage/blobs/storage-blobs-introduction.md) olaylarına yanıt vermesini ve karşıya yüklenen görüntülerin küçük resimlerini oluşturmasını sağlar. Blob depolama oluşturma olayına karşı bir olay aboneliği oluşturulur. Belirli bir Blob depolama kapsayıcısına blob eklendiğinde bir işlev uç noktası çağrılır. Event Grid’den işlev bağlamaya geçirilen veriler, bloba erişmek ve küçük resim görüntüsünü oluşturmak için kullanılır.

Var olan bir görüntü yükleme uygulamasına yeniden boyutlandırma işlevini eklemek için Azure CLI ve Azure portalını kullanabilirsiniz.

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

![Tarayıcıda yayımlanan web uygulaması](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

# <a name="nodejs-v2-sdktabnodejs"></a>[node.js V2 SDK'sı](#tab/nodejs)

![Tarayıcıda yayımlanan web uygulaması](./media/resize-images-on-storage-blob-upload-event/upload-app-nodejs-thumb.png)

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)

![Tarayıcıda yayımlanan web uygulaması](./media/resize-images-on-storage-blob-upload-event/upload-app-nodejs-thumb.png)

---

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Genel bir Azure Depolama hesabı oluşturma
> * Azure İşlevleri’ni kullanarak sunucusuz kod dağıtma
> * Event Grid’de bir Blob depolama olayı aboneliği oluşturma

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiyi tamamlamak için:

Önceki Blob Depolama öğreticisini tamamlamış olmanız gerekir: [Azure depolama ile buluta görüntü verileri yükleme][previous-tutorial].

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Event Grid kaynak sağlayıcısını önceden aboneliğinize kaydetmediyseniz burada kaydettiğinizden emin olun.

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.14 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 

Cloud Shell kullanmıyorsanız önce `az login` kullanarak oturum açmanız gerekir.

## <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

Azure İşlevleri, genel bir depolama hesabı gerektirir. Önceki öğreticide oluşturduğunuz Blob Depolama hesabına ek olarak ayrı bir genel depolama hesabı kullanarak kaynak grubunu oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account) komutu. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. 

1. Önceki öğreticide oluşturduğunuz kaynak grubunun adını tutacak bir değişken ayarlayın. 

    ```azurecli-interactive
    resourceGroupName=myResourceGroup
    ```
2. Azure işlevleri gerektiren yeni bir depolama hesabı adı için bir değişken ayarlayın. 

    ```azurecli-interactive
    functionstorage=<name of the storage account to be used by the function>
    ```
3. Depolama hesabı için Azure işlevi oluşturun. 

    ```azurecli-interactive
    az storage account create --name $functionstorage --location southeastasia \
    --resource-group $resourceGroupName --sku Standard_LRS --kind storage
    ```

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma  

İşlevinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun sunucusuz yürütülmesine yönelik bir ortam sağlar. [az functionapp create](/cli/azure/functionapp) komutunu kullanarak bir işlev uygulaması oluşturun. 

Aşağıdaki komutta kendi benzersiz işlev uygulamanızın adını sağlayın. İşlev uygulaması adı, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. 

1. Oluşturulması gereken işlev uygulaması için bir ad belirtin. 

    ```azurecli-interactive
    functionapp=<name of the function app>
    ```
2. Azure işlevi oluşturun. 

    ```azurecli-interactive
    az functionapp create --name $functionapp --storage-account $functionstorage \
    --resource-group $resourceGroupName --consumption-plan-location southeastasia
    ```

Şimdi, işlev uygulamasını [önceki öğreticide][previous-tutorial] oluşturduğunuz Blob depolama hesabına bağlanacak şekilde yapılandırmanız gerekir.

## <a name="configure-the-function-app"></a>İşlev uygulamasını yapılandırma

İşlevi kullanarak işlev uygulaması, uygulama ayarları eklenen Blob Depolama hesabı için kimlik bilgileri gerekiyor. [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) komutu.

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

```azurecli-interactive
blobStorageAccount=<name of the Blob storage account you created in the previous tutorial>
storageConnectionString=$(az storage account show-connection-string --resource-group $resourceGroupName \
--name $blobStorageAccount --query connectionString --output tsv)

az functionapp config appsettings set --name $functionapp --resource-group $resourceGroupName \
--settings AzureWebJobsStorage=$storageConnectionString THUMBNAIL_CONTAINER_NAME=thumbnails \
THUMBNAIL_WIDTH=100 FUNCTIONS_EXTENSION_VERSION=~2
```

# <a name="nodejs-v2-sdktabnodejs"></a>[node.js V2 SDK'sı](#tab/nodejs)

```azurecli-interactive
blobStorageAccount=<name of the Blob storage account you created in the previous tutorial>

storageConnectionString=$(az storage account show-connection-string --resource-group $resourceGroupName \
--name $blobStorageAccount --query connectionString --output tsv)

az functionapp config appsettings set --name $functionapp --resource-group $resourceGroupName \
--settings AZURE_STORAGE_CONNECTION_STRING=$storageConnectionString \
THUMBNAIL_WIDTH=100 FUNCTIONS_EXTENSION_VERSION=~2
```

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)

```azurecli-interactive
blobStorageAccount=<name of the Blob storage account you created in the previous tutorial>

blobStorageAccountKey=$(az storage account keys list -g $resourceGroupName \
-n $blobStorageAccount --query [0].value --output tsv)

storageConnectionString=$(az storage account show-connection-string --resource-group $resourceGroupName \
--name $blobStorageAccount --query connectionString --output tsv)

az functionapp config appsettings set --name $functionapp --resource-group $resourceGroupName \
--settings FUNCTIONS_EXTENSION_VERSION=~2 BLOB_CONTAINER_NAME=thumbnails \
AZURE_STORAGE_ACCOUNT_NAME=$blobStorageAccount \
AZURE_STORAGE_ACCOUNT_ACCESS_KEY=$blobStorageAccountKey \
AZURE_STORAGE_CONNECTION_STRING=$storageConnectionString
```

---

`FUNCTIONS_EXTENSION_VERSION=~2` ayarı işlev uygulamasının Azure İşlevleri çalışma zamanının 2.x sürümünde çalışmasını sağlar.

Bu işlev uygulamasına bir işlev kodu projesi dağıtabilirsiniz.

## <a name="deploy-the-function-code"></a>İşlev kodunu dağıtma 

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

Örnek C# yeniden boyutlandırma işlevi, üzerinde kullanılabilir [GitHub](https://github.com/Azure-Samples/function-image-upload-resize). Bu kod projesini kullanarak işlev uygulamasına dağıtın [az functionapp deployment kaynak yapılandırma](/cli/azure/functionapp/deployment/source) komutu. 

```azurecli-interactive
az functionapp deployment source config --name $functionapp --resource-group $resourceGroupName --branch master --manual-integration --repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

# <a name="nodejs-v2-sdktabnodejs"></a>[node.js V2 SDK'sı](#tab/nodejs)

Örnek Node.js yeniden boyutlandırma işlevi [GitHub](https://github.com/Azure-Samples/storage-blob-resize-function-node) üzerinde mevcuttur. [az functionapp deployment source config](/cli/azure/functionapp/deployment/source) komutunu kullanarak bu İşlevler kod projesini işlev uygulamasına dağıtın.

```azurecli-interactive
az functionapp deployment source config --name $functionapp \
--resource-group $resourceGroupName --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-resize-function-node
```

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)

Örnek Node.js yeniden boyutlandırma işlevi [GitHub](https://github.com/Azure-Samples/storage-blob-resize-function-node-v10) üzerinde mevcuttur. [az functionapp deployment source config](/cli/azure/functionapp/deployment/source) komutunu kullanarak bu İşlevler kod projesini işlev uygulamasına dağıtın.

```azurecli-interactive
az functionapp deployment source config --name $functionapp \
--resource-group $resourceGroupName --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-resize-function-node-v10
```
---

Görüntüyü yeniden boyutlandırma işlevi, Event Grid hizmetinden gönderilen HTTP istekleriyle tetiklenir. Bir olay aboneliği oluşturarak Event Grid'e işlevinizin URL'sinde bu bildirimleri almak istediğinizi bildirirsiniz. Bu öğreticide, blob tarafından oluşturulan olaylara abone oluyorsunuz.

Event Grid bildiriminden işleve geçirilen veriler, blobun URL'sini içerir. Ardından karşıya yüklenen görüntüyü Blob depolamadan almak için bu URL giriş bağlamasına geçirilir. İşlev bir küçük resim görüntüsü oluşturur ve elde edilen akışı Blob depolama içinde ayrı bir kapsayıcıya yazar. 

Bu proje, tetikleyici türü olarak `EventGridTrigger` kullanır. Genel HTTP tetikleyicileri yerine Event Grid tetikleyicisinin kullanılması önerilir. Event Grid, Event Grid İşlevi tetikleyicilerini otomatik olarak doğrular. Genel HTTP tetikleyicileri ile [doğrulama yanıtını](security-authentication.md) uygulamanız gerekir.

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

Bu işlev hakkında daha fazla bilgi edinmek için bkz. [function.json ve run.csx dosyaları](https://github.com/Azure-Samples/function-image-upload-resize/tree/master/ImageFunctions).

# <a name="nodejs-v2-sdktabnodejs"></a>[node.js V2 SDK'sı](#tab/nodejs)

Bu işlev hakkında daha fazla bilgi için bkz. [function.json ve index.js dosyaları](https://github.com/Azure-Samples/storage-blob-resize-function-node/tree/master/Thumbnail).

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)

Bu işlev hakkında daha fazla bilgi için bkz. [function.json ve index.js dosyaları](https://github.com/Azure-Samples/storage-blob-resize-function-node-v10/tree/master/Thumbnail).

---

İşlev proje kodu, doğrudan ortak örnek depodan dağıtılır. Azure İşlevleri’ne ilişkin dağıtım seçenekleri hakkında daha fazla bilgi için bkz. [Azure İşlevleri için sürekli dağıtım](../azure-functions/functions-continuous-deployment.md).

## <a name="create-an-event-subscription"></a>Olay aboneliği oluşturma

Olay aboneliği, belirli bir uç noktaya gönderilmesini istediğiniz, sağlayıcı tarafından oluşturulmuş olayları gösterir. Bu örnekte uç nokta, işleviniz tarafından kullanıma sunulur. Azure Portal'da işlevinize bildirimler gönderen bir olay aboneliği oluşturmak için aşağıdaki adımları kullanın: 

1. İçinde [Azure portalında](https://portal.azure.com)seçin **tüm hizmetleri** sol menüsünü ve ardından **işlev uygulamaları**. 

    ![Azure portalında İşlev Uygulamalarına göz atma](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. İşlev uygulamanızı genişletin, seçin **küçük resim** işlevi ve ardından **Event Grid aboneliği Ekle**.

    ![Azure portalında İşlev Uygulamalarına göz atma](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Tabloda belirtilen olay aboneliği ayarlarını kullanın.
    
    ![Azure portalında işlevden olay aboneliği oluşturma](./media/resize-images-on-storage-blob-upload-event/event-subscription-create.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Ad** | imageresizersub | Yeni olay aboneliğinizi tanımlayan ad. | 
    | **Konu türü** |  Depolama hesapları | Depolama hesabı olay sağlayıcısını seçin. | 
    | **Abonelik** | Azure aboneliğiniz | Varsayılan olarak, geçerli Azure aboneliğiniz seçili durumdadır.   |
    | **Kaynak grubu** | myResourceGroup | **Var olanı kullan**’ı seçin ve bu öğreticide kullandığınız kaynak grubunu belirleyin.  |
    | **Kaynak** |  Blob depolama hesabınız |  Oluşturduğunuz Blob depolama hesabını seçin. |
    | **Olay türleri** | Oluşturulan blob | **Oluşturulan blob** dışındaki tüm türlerin işaretini kaldırın. Yalnızca `Microsoft.Storage.BlobCreated` türündeki olaylar işleve geçirilir.| 
    | **Abone türü** |  otomatik oluşturulmuş |  Web Kancası olarak önceden tanımlanmış. |
    | **Abone uç noktası** | otomatik oluşturulmuş | Sizin için oluşturulan uç nokta URL'sini kullanın. | 
4. Geçiş **filtre** sekmesini tıklatın ve aşağıdaki eylemleri gerçekleştirin:     
    1. Seçin **filtreleme etkinleştir konu** seçeneği.
    2. İçin **konu ile başlayan**, şu değeri girin: **/blobServices/varsayılan/kapsayıcılar/resimler/blobları/**.

        ![Olay aboneliği için filtre belirtin.](./media/resize-images-on-storage-blob-upload-event/event-subscription-filter.png) 
2. Seçin **Oluştur** olay aboneliği eklemek için. Bu tetikleyen bir olay aboneliği oluşturur `Thumbnail` işlev bir blob eklendiğinde `images` kapsayıcı. İşlev görüntüleri yeniden boyutlandırır ve eklenmeye `thumbnails` kapsayıcı.

Arka uç hizmetleri yapılandırıldıktan sonra, görüntü yeniden boyutlandırma işlevini örnek web uygulamasında test edin. 

## <a name="test-the-sample-app"></a>Örnek uygulamayı test etme

Web uygulamasında görüntü yeniden boyutlandırmayı test etmek için, yayımlanan uygulamanızın URL'sine gidin. Web uygulamasının varsayılan URL'si `https://<web_app>.azurewebsites.net` şeklindedir.

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

**Karşıya fotoğraf yükle** bölgesine tıklayarak bir dosyayı seçip karşıya yükleyin. Ayrıca bu bölgeye fotoğraf sürükleyebilirsiniz. 

Karşıya yüklenen görüntü kaybolduktan sonra **Oluşturulan küçük resimler** döngüsünde karşıya yüklenen görüntünün bir kopyasının gösterildiğine dikkat edin. Bu görüntü, işlev tarafından yeniden boyutlandırılmış, *thumbnails* kapsayıcısına eklenmiş ve web istemcisi tarafından indirilmiştir.

![Tarayıcıda yayımlanan web uygulaması](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

# <a name="nodejs-v2-sdktabnodejs"></a>[node.js V2 SDK'sı](#tab/nodejs)

Tıklayın **Dosya Seç** bir dosya seçmek için ardından **karşıya resim**. Karşıya yükleme başarılı olduğunda, tarayıcının bir başarı sayfasına götürür. Giriş sayfasına dönmek için bağlantıya tıklayın. Karşıya yüklenen görüntüyü bir kopyasını görüntülenen **oluşturulan küçük resimler** alan. (İlk olarak görüntüyü görünmüyorsa, sayfayı yeniden yüklemeyi deneyin.) Bu görüntü, işlev tarafından yeniden boyutlandırılmış, *thumbnails* kapsayıcısına eklenmiş ve web istemcisi tarafından indirilmiştir.

![Tarayıcıda yayımlanan web uygulaması](./media/resize-images-on-storage-blob-upload-event/upload-app-nodejs-thumb.png)

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)

Tıklayın **Dosya Seç** bir dosya seçmek için ardından **karşıya resim**. Karşıya yükleme başarılı olduğunda, tarayıcının bir başarı sayfasına götürür. Giriş sayfasına dönmek için bağlantıya tıklayın. Karşıya yüklenen görüntüyü bir kopyasını görüntülenen **oluşturulan küçük resimler** alan. (İlk olarak görüntüyü görünmüyorsa, sayfayı yeniden yüklemeyi deneyin.) Bu görüntü, işlev tarafından yeniden boyutlandırılmış, *thumbnails* kapsayıcısına eklenmiş ve web istemcisi tarafından indirilmiştir.

![Tarayıcıda yayımlanan web uygulaması](./media/resize-images-on-storage-blob-upload-event/upload-app-nodejs-thumb.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Genel bir Azure Depolama hesabı oluşturma
> * Azure İşlevleri’ni kullanarak sunucusuz kod dağıtma
> * Event Grid’de bir Blob depolama olayı aboneliği oluşturma

Depolama hesabına erişimin güvenliğini sağlama hakkında bilgi almak için Depolama öğreticisi serisinin üçüncü bölümüne geçin.

> [!div class="nextstepaction"]
> [Bulutta uygulama verilerine erişimin güvenliğini sağlama](../storage/blobs/storage-secure-access-application.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

+ Event Grid hakkında daha fazla bilgi için bkz. [Azure Event Grid’e giriş](overview.md). 
+ Azure İşlevleri’ni ön plana çıkaran başka bir öğretici denemek için bkz. [Azure Logic Apps ile tümleşen işlev oluşturma](../azure-functions/functions-twitter-email.md). 

[previous-tutorial]: ../storage/blobs/storage-upload-process-images.md
