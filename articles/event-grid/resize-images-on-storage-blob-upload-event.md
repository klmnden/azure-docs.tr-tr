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
ms.openlocfilehash: 6015d226bce578661816bd0f934f7818746b4c3b
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55507765"
---
# <a name="tutorial-automate-resizing-uploaded-images-using-event-grid"></a>Öğretici: Karşıya yüklenen görüntüleri yeniden boyutlandırmayı Event Grid kullanarak otomatikleştirme

[Azure Event Grid](overview.md), bulut için bir olay oluşturma hizmetidir. Event Grid, Azure hizmetleri veya üçüncü taraf kaynaklar tarafından başlatılan olaylara abonelikler oluşturmanızı sağlar.  

Bu öğretici, Depolama öğreticileri serisinin ikinci bölümüdür. [Önceki Depolama öğreticisine][previous-tutorial], Azure Event Grid ve Azure İşlevleri’ni kullanarak sunucusuz otomatik küçük resim oluşturma işlemini ekler. Event Grid, [Azure İşlevleri](../azure-functions/functions-overview.md)’nin [Azure Blob depolama](../storage/blobs/storage-blobs-introduction.md) olaylarına yanıt vermesini ve karşıya yüklenen görüntülerin küçük resimlerini oluşturmasını sağlar. Blob depolama oluşturma olayına karşı bir olay aboneliği oluşturulur. Belirli bir Blob depolama kapsayıcısına blob eklendiğinde bir işlev uç noktası çağrılır. Event Grid’den işlev bağlamaya geçirilen veriler, bloba erişmek ve küçük resim görüntüsünü oluşturmak için kullanılır.

Var olan bir görüntü yükleme uygulamasına yeniden boyutlandırma işlevini eklemek için Azure CLI ve Azure portalını kullanabilirsiniz.

![Microsoft Edge tarayıcısında yayımlanan web uygulaması](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Genel bir Azure Depolama hesabı oluşturma
> * Azure İşlevleri’ni kullanarak sunucusuz kod dağıtma
> * Event Grid’de bir Blob depolama olayı aboneliği oluşturma

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

Önceki Blob Depolama öğreticisini tamamlamış olmanız gerekir: [Azure depolama ile buluta görüntü verileri yükleme][previous-tutorial].

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Event Grid kaynak sağlayıcısını önceden aboneliğinize kaydetmediyseniz burada kaydettiğinizden emin olun.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.EventGrid
```

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.14 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 

Cloud Shell kullanmıyorsanız önce `az login` kullanarak oturum açmanız gerekir.

## <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

Azure İşlevleri, genel bir depolama hesabı gerektirir. Kaynak grubunda [az storage account create](/cli/azure/storage/account#az-storage-account-create) komutunu kullanarak ayrı bir genel depolama hesabı oluşturun.

Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. 

Aşağıdaki komutta, genel depolama hesabına ilişkin kendi genel benzersiz adınızı `<general_storage_account>` yer tutucusunu gördüğünüz yere yerleştirin. 

1. Önceki öğreticide oluşturduğunuz kaynak grubunun adını tutacak bir değişken ayarlayın. 

    ```azurecli-interactive
    resourceGroupName=<Name of the resource group that you created in the previous tutorial>
    ```
2. Azure işlevi gerektirir depolama hesabının adı için bir değişken ayarlayın. 

    ```azurecli-interactive
    functionstorage=<name of the storage account to be used by function>
    ```
3. Depolama hesabı için Azure işlevi oluşturun. Bu görüntüleri içeren depolama alanından farklıdır. 

    ```azurecli-interactive
    az storage account create --name $functionstorage --location eastus --resource-group $resourceGroupName --sku Standard_LRS --kind storage
    ```

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma  

İşlevinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun sunucusuz yürütülmesine yönelik bir ortam sağlar. [az functionapp create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak bir işlev uygulaması oluşturun. 

Aşağıdaki komutta kendi benzersiz işlev uygulamanızın adını `<function_app>` yer tutucusunun yerine ekleyin. İşlev uygulaması adı, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. `<general_storage_account>` için, oluşturduğunuz genel depolama hesabının adını kullanın.

1. Oluşturulması gereken işlev uygulaması için bir ad belirtin. 

    ```azurecli-interactive
    functionapp=<name of the function app>
    ```
2. Azure işlevi oluşturun. 

    ```azurecli-interactive
    az functionapp create --name $functionapp --storage-account  $functionstorage --resource-group $resourceGroupName --consumption-plan-location eastus
    ```

Şimdi, işlev uygulamasını [önceki öğreticide][previous-tutorial] oluşturduğunuz Blob depolama hesabına bağlanacak şekilde yapılandırmanız gerekir.

## <a name="configure-the-function-app"></a>İşlev uygulamasını yapılandırma

İşlevin Blob depolama hesabına bağlanması için bağlantı dizesi gerekir. Aşağıdaki adımda Azure'a dağıttığınız işlev kodu, myblobstorage_STORAGE uygulama ayarında bağlantı dizesini arar ve myContainerName uygulama ayarında da küçük resim görüntüsü kapsayıcı adını arar. [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string) komutu ile bağlantı dizesini alın. [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) komutuyla uygulama ayarlarını belirleyin.

Aşağıdaki CLI komutlarında `<blob_storage_account>`, önceki öğreticide oluşturduğunuz Blob depolama hesabının adıdır.

1. Görüntüleri içeren depolama hesabı için bağlantı dizesini alın. 

    ```azurecli-interactive
    storageConnectionString=$(az storage account show-connection-string --resource-group $resourceGroupName --name $blobStorageAccount --query connectionString --output tsv)
    ```
2. İşlev uygulamasını yapılandırın. 

    ```azurecli-interactive
    az functionapp config appsettings set --name $functionapp --resource-group $resourceGroupName --settings AzureWebJobsStorage=$storageConnectionString THUMBNAIL_CONTAINER_NAME=thumbnails THUMBNAIL_WIDTH=100 FUNCTIONS_EXTENSION_VERSION=~2
    ```

    `FUNCTIONS_EXTENSION_VERSION=~2` ayarı işlev uygulamasının Azure İşlevleri çalışma zamanının 2.x sürümünde çalışmasını sağlar.

Bu işlev uygulamasına bir işlev kodu projesi dağıtabilirsiniz.

## <a name="deploy-the-function-code"></a>İşlev kodunu dağıtma 

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

Örnek C# (.csx) yeniden boyutlandırma betiği [GitHub](https://github.com/Azure-Samples/function-image-upload-resize) üzerinde mevcuttur. [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config) komutunu kullanarak bu İşlevler kod projesini işlev uygulamasına dağıtın. 

Aşağıdaki komutta `<function_app>`, daha önce oluşturduğunuz işlev uygulamasının adıdır.

```azurecli-interactive
az functionapp deployment source config --name $functionapp --resource-group $resourceGroupName --branch master --manual-integration --repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
Örnek Node.js yeniden boyutlandırma işlevi [GitHub](https://github.com/Azure-Samples/storage-blob-resize-function-node) üzerinde mevcuttur. [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config) komutunu kullanarak bu İşlevler kod projesini işlev uygulamasına dağıtın.

Aşağıdaki komutta `<function_app>`, daha önce oluşturduğunuz işlev uygulamasının adıdır.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-resize-function-node
```
---

Görüntüyü yeniden boyutlandırma işlevi, Event Grid hizmetinden gönderilen HTTP istekleriyle tetiklenir. Bir olay aboneliği oluşturarak Event Grid'e işlevinizin URL'sinde bu bildirimleri almak istediğinizi bildirirsiniz. Bu öğreticide, blob tarafından oluşturulan olaylara abone oluyorsunuz.

Event Grid bildiriminden işleve geçirilen veriler, blobun URL'sini içerir. Ardından karşıya yüklenen görüntüyü Blob depolamadan almak için bu URL giriş bağlamasına geçirilir. İşlev bir küçük resim görüntüsü oluşturur ve elde edilen akışı Blob depolama içinde ayrı bir kapsayıcıya yazar. 

Bu proje, tetikleyici türü olarak `EventGridTrigger` kullanır. Genel HTTP tetikleyicileri yerine Event Grid tetikleyicisinin kullanılması önerilir. Event Grid, Event Grid İşlevi tetikleyicilerini otomatik olarak doğrular. Genel HTTP tetikleyicileri ile [doğrulama yanıtını](security-authentication.md#webhook-event-delivery) uygulamanız gerekir.

Bu işlev hakkında daha fazla bilgi edinmek için bkz. [function.json ve run.csx dosyaları](https://github.com/Azure-Samples/function-image-upload-resize/tree/master/imageresizerfunc).
 
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

**Karşıya fotoğraf yükle** bölgesine tıklayarak bir dosyayı seçip karşıya yükleyin. Ayrıca bu bölgeye fotoğraf sürükleyebilirsiniz. 

Karşıya yüklenen görüntü kaybolduktan sonra **Oluşturulan küçük resimler** döngüsünde karşıya yüklenen görüntünün bir kopyasının gösterildiğine dikkat edin. Bu görüntü, işlev tarafından yeniden boyutlandırılmış, *thumbnails* kapsayıcısına eklenmiş ve web istemcisi tarafından indirilmiştir.

![Microsoft Edge tarayıcısında yayımlanan web uygulaması](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

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
