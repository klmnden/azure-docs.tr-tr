---
title: "Karşıya yüklenen görüntülerin yeniden boyutlandırılmasını otomatikleştirmek için Azure Event Grid kullanma | Microsoft Docs"
description: "Azure Event Grid, Azure Depolama’da blob yüklemelerini tetikleyebilir. Bu hizmeti kullanarak, Azure Depolama’ya yüklenmiş görüntü dosyalarını, yeniden boyutlandırma ve diğer iyileştirmeler için Azure İşlevleri gibi diğer hizmetlere gönderebilirsiniz."
services: event-grid
author: ggailey777
manager: cfowler
editor: 
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: b0fccd058620537f6dcfaf37ee14c1ff0cb8857a
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="automate-resizing-uploaded-images-using-event-grid"></a>Karşıya yüklenen görüntüleri yeniden boyutlandırmayı Event Grid kullanarak otomatikleştirme

[Azure Event Grid](overview.md), bulut için bir olay oluşturma hizmetidir. Event Grid, Azure hizmetleri veya üçüncü taraf kaynaklar tarafından başlatılan olaylara abonelikler oluşturmanızı sağlar.  

Bu öğretici, Depolama öğreticileri serisinin ikinci bölümüdür. [Önceki Depolama öğreticisine][previous-tutorial], Azure Event Grid ve Azure İşlevleri’ni kullanarak sunucusuz otomatik küçük resim oluşturma işlemini ekler. Event Grid, [Azure İşlevleri](..\azure-functions\functions-overview.md)’nin [Azure Blob depolama](..\storage\blobs\storage-blobs-introduction.md) olaylarına yanıt vermesini ve karşıya yüklenen görüntülerin küçük resimlerini oluşturmasını sağlar. Blob depolama oluşturma olayına karşı bir olay aboneliği oluşturulur. Belirli bir Blob depolama kapsayıcısına blob eklendiğinde bir işlev uç noktası çağrılır. Event Grid’den işlev bağlamaya geçirilen veriler, bloba erişmek ve küçük resim görüntüsünü oluşturmak için kullanılır. 

Var olan bir görüntü yükleme uygulamasına yeniden boyutlandırma işlevini eklemek için Azure CLI ve Azure portalını kullanabilirsiniz.

![Edge tarayıcısında web uygulaması yayımlama](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Genel bir Azure Depolama hesabı oluşturma
> * Azure İşlevleri’ni kullanarak sunucusuz kod dağıtma
> * Event Grid’de bir Blob depolama olayı aboneliği oluşturma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

+ Önceki Blob depolama öğreticisini tamamlamış olmanız gerekir: [Azure Storage ile görüntü verilerini buluta yükleme][previous-tutorial]. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.14 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

Cloud Shell kullanmıyorsanız önce `az login` kullanarak oturum açmanız gerekir.

## <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

Azure İşlevleri, genel bir depolama hesabı gerektirir. Kaynak grubunda [az storage account create](/cli/azure/storage/account#az_storage_account_create) komutunu kullanarak ayrı bir genel depolama hesabı oluşturun.

Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. 

Aşağıdaki komutta, genel depolama hesabına ilişkin kendi genel benzersiz adınızı `<general_storage_account>` yer tutucusunu gördüğünüz yere yerleştirin. 

```azurecli-interactive
az storage account create --name <general_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind storage
```

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma  

İşlevinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun sunucusuz yürütülmesine yönelik bir ortam sağlar. [az functionapp create](/cli/azure/functionapp#az_functionapp_create) komutunu kullanarak bir işlev uygulaması oluşturun. 

Aşağıdaki komutta kendi benzersiz işlev uygulamanızın adını `<function_app>` yer tutucusunun yerine ekleyin. İşlev uygulaması adı, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. `<general_storage_account>` için, oluşturduğunuz genel depolama hesabının adını kullanın.

```azurecli-interactive
az functionapp create --name <function_app> --storage-account  <general_storage_account>  \
--resource-group myResourceGroup --consumption-plan-location westcentralus
```

Şimdi, işlev uygulamasını [önceki öğreticide][previous-tutorial] oluşturduğunuz Blob depolama hesabına bağlanacak şekilde yapılandırmanız gerekir.

## <a name="configure-the-function-app"></a>İşlev uygulamasını yapılandırma

İşlevin Blob depolama hesabına bağlanması için bağlantı dizesi gerekir. Aşağıdaki adımda Azure'a dağıttığınız işlev kodu, myblobstorage_STORAGE uygulama ayarında bağlantı dizesini arar ve myContainerName uygulama ayarında da küçük resim görüntüsü kapsayıcı adını arar. [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string) komutu ile bağlantı dizesini alın. [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) komutuyla uygulama ayarlarını belirleyin.

Aşağıdaki CLI komutlarında `<blob_storage_account>`, önceki öğreticide oluşturduğunuz Blob depolama hesabının adıdır.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <blob_storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings myblobstorage_STORAGE=$storageConnectionString \
myContainerName=thumbs
```

Bu işlev uygulamasına bir işlev kodu projesi dağıtabilirsiniz.

## <a name="deploy-the-function-code"></a>İşlev kodunu dağıtma 

Görüntüyü yeniden boyutlandıran C# işlevi, [bu GitHub deposunda](https://github.com/Azure-Samples/function-image-upload-resize) mevcuttur. [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config) komutunu kullanarak bu İşlevler kod projesini işlev uygulamasına dağıtın. 

Aşağıdaki komutta `<function_app>`, daha önce oluşturduğunuz işlev uygulamasının adıdır.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

Görüntüyü yeniden boyutlandırma işlevi, Event Grid hizmetinden gönderilen HTTP istekleriyle tetiklenir. Bir olay aboneliği oluşturarak Event Grid'e işlevinizin URL'sinde bu bildirimleri almak istediğinizi bildirirsiniz. Bu öğreticide, blob tarafından oluşturulan olaylara abone oluyorsunuz.

Event Grid bildiriminden işleve geçirilen veriler, blobun URL'sini içerir. Ardından karşıya yüklenen görüntüyü Blob depolamadan almak için bu URL giriş bağlamasına geçirilir. İşlev bir küçük resim görüntüsü oluşturur ve elde edilen akışı Blob depolama içinde ayrı bir kapsayıcıya yazar. 

Bu proje, tetikleyici türü olarak `EventGridTrigger` kullanır. Genel HTTP tetikleyicileri yerine Event Grid tetikleyicisinin kullanılması önerilir. Event Grid, Event Grid İşlevi tetikleyicilerini otomatik olarak doğrular. Genel HTTP tetikleyicileri ile [doğrulama yanıtını](security-authentication.md#webhook-event-delivery) uygulamanız gerekir.

Bu işlev hakkında daha fazla bilgi edinmek için bkz. [function.json ve run.csx dosyaları](https://github.com/Azure-Samples/function-image-upload-resize/tree/master/imageresizerfunc).
 
İşlev proje kodu, doğrudan ortak örnek depodan dağıtılır. Azure İşlevleri’ne ilişkin dağıtım seçenekleri hakkında daha fazla bilgi için bkz. [Azure İşlevleri için sürekli dağıtım](../azure-functions/functions-continuous-deployment.md).

## <a name="create-an-event-subscription"></a>Olay aboneliği oluşturma

Olay aboneliği, belirli bir uç noktaya gönderilmesini istediğiniz, sağlayıcı tarafından oluşturulmuş olayları gösterir. Bu örnekte uç nokta, işleviniz tarafından kullanıma sunulur. Azure Portal'da işlevinize bildirimler gönderen bir olay aboneliği oluşturmak için aşağıdaki adımları kullanın: 

1. [Azure Portal](https://portal.azure.com)'da, sol altta bulunan oka tıklayarak tüm hizmetleri genişletin, **Filtre** alanına *işlevler* yazın ve **İşlev Uygulamaları**'nı seçin. 

    ![Azure portalında İşlev Uygulamalarına göz atma](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. İşlev uygulamanızı genişletin, **imageresizerfunc** işlevini ve ardından **Event Grid aboneliği ekle**’yi seçin.

    ![Azure portalında İşlev Uygulamalarına göz atma](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Tabloda belirtilen olay aboneliği ayarlarını kullanın.
    
    ![Azure portalında işlevden olay aboneliği oluşturma](./media/resize-images-on-storage-blob-upload-event/event-subscription-create-flow.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Ad** | imageresizersub | Yeni olay aboneliğinizi tanımlayan ad. | 
    | **Konu türü** |  Depolama hesapları | Depolama hesabı olay sağlayıcısını seçin. | 
    | **Abonelik** | Azure aboneliğiniz | Varsayılan olarak, geçerli Azure aboneliğiniz seçili durumdadır.   |
    | **Kaynak grubu** | myResourceGroup | **Var olanı kullan**’ı seçin ve bu öğreticide kullandığınız kaynak grubunu belirleyin.  |
    | **Örnek** |  Blob depolama hesabınız |  Oluşturduğunuz Blob depolama hesabını seçin. |
    | **Olay türleri** | Oluşturulan blob | **Oluşturulan blob** dışındaki tüm türlerin işaretini kaldırın. Yalnızca `Microsoft.Storage.BlobCreated` türündeki olaylar işleve geçirilir.| 
    | **Abone türü** |  Web Kancası |  Seçenekler: Web Kancası ve Event Hubs. |
    | **Abone uç noktası** | otomatik oluşturulmuş | Sizin için oluşturulan uç nokta URL'sini kullanın. | 
    | **Önek filtresi** | /blobServices/default/containers/images/blobs/ | Depolama olaylarını yalnızca **images** kapsayıcısı üzerindeki olaylarla filtreler.| 

4. Olay aboneliği eklemek için **Oluştur**’a tıklayın. Bu işlem, *images* kapsayıcısına bir blob eklendiğinde `imageresizerfunc` olayını tetikleyen bir olay aboneliği oluşturur. İşlev görüntüleri yeniden boyutlandırır ve *thumbs* kapsayıcısına ekler.

Arka uç hizmetleri yapılandırıldıktan sonra, görüntü yeniden boyutlandırma işlevini örnek web uygulamasında test edin. 

## <a name="test-the-sample-app"></a>Örnek uygulamayı test etme

Web uygulamasında görüntü yeniden boyutlandırmayı test etmek için, yayımlanan uygulamanızın URL'sine gidin. Web uygulamasının varsayılan URL'si `https://<web_app>.azurewebsites.net` şeklindedir.

**Karşıya fotoğraf yükle** bölgesine tıklayarak bir dosyayı seçip karşıya yükleyin. Ayrıca bu bölgeye fotoğraf sürükleyebilirsiniz. 

Karşıya yüklenen görüntü kaybolduktan sonra **Oluşturulan küçük resimler** döngüsünde karşıya yüklenen görüntünün bir kopyasının gösterildiğine dikkat edin. Bu görüntü, işlev tarafından yeniden boyutlandırılmış, *thumbs* kapsayıcısına eklenmiş ve web istemcisi tarafından indirilmiştir.

![Edge tarayıcısında web uygulaması yayımlama](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

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
+ Azure İşlevleri’ni ön plana çıkaran başka bir öğretici denemek için bkz. [Azure Logic Apps ile tümleşen işlev oluşturma](..\azure-functions\functions-twitter-email.md). 

[previous-tutorial]: ../storage/blobs/storage-upload-process-images.md
