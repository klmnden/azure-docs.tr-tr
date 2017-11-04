---
title: "Görüntüleri yeniden boyutlandırma otomatikleştirmek için kullanım Azure olay kılavuz karşıya | Microsoft Docs"
description: "Azure olay kılavuz, Azure Storage blobu yüklemeler üzerinde tetikleyebilir. Yeniden boyutlandırma gibi başka hizmetleri, Azure işlevleri ve diğer geliştirmeler Azure depolama birimine yüklenen görüntü dosyaları göndermek için bunu kullanabilirsiniz."
services: event-grid
author: ggailey777
manager: cfowler
editor: 
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 709d23ab590c06d5da9b03e2767bc0be5905355b
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="automate-resizing-uploaded-images-using-event-grid"></a>Olay kılavuzu kullanarak karşıya yüklenen görüntüleri yeniden boyutlandırma otomatikleştirme

[Azure olay kılavuz](overview.md) bulut için bir olay hizmetidir. Olay kılavuz, Azure Hizmetleri veya üçüncü taraf kaynakları tarafından başlatılan olayları abonelikleri oluşturmanıza olanak sağlar.  

Bu öğretici iki depolama eğitim serileri parçasıdır. Bunu genişletir [önceki depolama öğretici] [ previous-tutorial] sunucusuz otomatik küçük resim oluşturma Azure olay kılavuz ve Azure işlevleri kullanarak eklemek için. Olay kılavuz sağlar [Azure işlevleri](..\azure-functions\functions-overview.md) yanıt verecek şekilde [Azure Blob Depolama](..\storage\blobs\storage-blobs-introduction.md) olayları ve karşıya yüklenen görüntülerin küçük resimleri oluşturur. Bir olay aboneliği karşı Blob oluşturulan depolama olayı oluşturun. Belirli bir Blob Depolama kapsayıcıya bir blob eklendiğinde işlevi uç nokta adı verilir. İşlev bağlama olay kılavuzdan geçirilen verileri blob erişmek ve küçük resim oluşturmak için kullanılır. 

Var olan bir görüntüyü karşıya yükleme uygulamaya yeniden boyutlandırma işlevini eklemek için Azure CLI ve Azure Portalı'nı kullanın.

![Edge tarayıcısında yayımlanan web uygulaması](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Genel Azure depolama hesabı oluşturma
> * Azure işlevleri kullanarak sunucusuz kod dağıtma
> * Olay kılavuzunda bir Blob Depolama olay aboneliği oluşturma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

+ Önceki Blob Depolama öğretici tamamlamış olmanız gerekir: [görüntü verileri Azure Storage ile bulutta yükleme][previous-tutorial]. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu konuda, Azure CLI Sürüm 2.0.14 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

Cloud Shell kullanmıyorsanız önce `az login` kullanarak oturum açmanız gerekir.

## <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

Azure işlevleri, genel depolama hesabı gerektirir. Kullanarak kaynak grubunda ayrı genel depolama hesabı oluşturmak [az depolama hesabı oluşturma](/cli/azure/storage/account#create) komutu.

Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. 

Aşağıdaki komutta, kendi gördüğünüz genel depolama hesabı için genel olarak benzersiz bir ad yerine `<general_storage_account>` yer tutucu. 

```azurecli-interactive
az storage account create --name <general_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind storage
```

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma  

İşlevinizi yürütülmesi barındırmak için bir işlev uygulaması olması gerekir. İşlev uygulaması, işlev kodunuzun sunucusuz yürütülmesine yönelik bir ortam sağlar. [az functionapp create](/cli/azure/functionapp#create) komutunu kullanarak bir işlev uygulaması oluşturun. 

Aşağıdaki komutta, gördüğünüz kendi benzersiz işlevi uygulama adı yerine `<function_app>` yer tutucu. `<function_app>`, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. Bu durumda, `<general_storage_account>` oluşturduğunuz genel depolama hesabının adıdır.  

```azurecli-interactive
az functionapp create --name <function_app> --storage-account  <general_storage_account>  \
--resource-group myResourceGroup --consumption-plan-location westcentralus
```

Şimdi, blob depolama alanına bağlanmak için işlev uygulaması yapılandırmanız gerekir. 

## <a name="configure-the-function-app"></a>İşlev uygulamayı yapılandırma

İşlevi için blob depolama hesabına bağlanmak için bağlantı dizesi gerekiyor. Bu durumda, `<blob_storage_account>` önceki öğreticide oluşturduğunuz Blob Depolama hesabının adıdır. Bağlantı dizesi alma [az depolama hesabı Göster bağlantı dizesi](/cli/azure/storage/account#show-connection-string) komutu. Küçük resim kapsayıcı adı da ayarlanmalıdır `thumbs`. Bu uygulama ayarları işlevi uygulamayla eklemek [az functionapp config appsettings kümesi](/cli/azure/functionapp/config/appsettings#set) komutu.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <blob_storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings myblobstorage_STORAGE=$storageConnectionString \
myContainerName=thumbs
```

Bu gibi durumlarda, işlev kodunu projeyi şimdi bu işlev uygulaması dağıtabilirsiniz.

## <a name="deploy-the-function-code"></a>İşlev kodu dağıtma 

Görüntüyü yeniden boyutlandırma gerçekleştirir C# işlevi bu kullanılabilir [örnek GitHub deposunu](https://github.com/Azure-Samples/function-image-upload-resize). Bu işlevler kodunu projeyi kullanarak işlev uygulaması dağıtma [az functionapp dağıtım kaynağı config](/cli/azure/functionapp/deployment/source#config) komutu. 

Aşağıdaki komutta `<function_app>` önceki komut dosyasında oluşturulan aynı işlevi uygulamadır.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

Görüntüyü yeniden boyutlandırma işlevi, bir Blob oluşturulan olay için olay abonelik tarafından tetiklenir. Tetikleyici geçirilen verileri karşıya yüklenen resim Blob depolama alanından elde etmek için giriş bağlaması sırayla geçirilir blob URL'sini içerir. İşlevi bir küçük resim oluşturur ve elde edilen akış ayrı bir Blob Depolama kapsayıcısını yazar. Bu işlev hakkında daha fazla bilgi için bkz: [örnek depo readme dosyasında](https://github.com/Azure-Samples/function-image-upload-resize/blob/master/README.md).
 
İşlev proje kodunu doğrudan ortak örnek depodan dağıtılır. Azure işlevleri için dağıtım seçenekleri hakkında daha fazla bilgi için bkz: [Azure işlevleri için sürekli dağıtım](../azure-functions/functions-continuous-deployment.md).

## <a name="create-your-event-subscription"></a>Olay aboneliği oluşturma

Sağlayıcı tarafından oluşturulan olayları istediğiniz belirli bir uç noktası için gönderilen bir olay aboneliği gösterir. Bu durumda, uç nokta işlevi tarafından sunulur. Bir olay abonelik Azure portalında işlevinizi oluşturmak için aşağıdaki adımları kullanın: 

1. İçinde [Azure portal](https://portal.azure.com), tüm hizmetler genişletmek için sol alt oku yazın tıklatın `functions` içinde **filtre** alan ve ardından **işlev uygulamalarının**. 

    ![Azure portalında işlevi uygulamalara göz atın](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. İşlev uygulaması'nı genişletin, seçin **imageresizerfunc** işlev ve ardından **ekleme olay kılavuz abonelik**.

    ![Azure portalında işlevi uygulamalara göz atın](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Tabloda belirtildiği gibi olay abonelik ayarlarını kullanın.

    ![Azure portalında işlevden olay aboneliği oluşturma](./media/resize-images-on-storage-blob-upload-event/event-subscription-create-flow.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Ad** | imageresizersub | Yeni olay abonelik tanımlayan ad. | 
    | **Konu Türü** |  Depolama hesapları | Depolama hesabı Olay sağlayıcısı seçin. | 
    | **Abonelik** | Aboneliğiniz | Varsayılan olarak, geçerli aboneliğiniz seçilmelidir.   |
    | **Kaynak grubu** | myResourceGroup | Seçin **var olanı kullan** ve bu konudaki şimdiye kaynak grubunu seçin.  |
    | **Örneği** |  `<blob_storage_account>` |  Oluşturduğunuz Blob Depolama hesabı seçin. |
    | **Olay türleri** | Oluşturulan blob | Tüm türleri dışında işaretini **oluşturulan Blob**. Yalnızca olay türlerini `Microsoft.Storage.BlobCreated` işlevine geçirilen.| 
    | **Abone uç noktası** | otomatik olarak oluşturulur | Sizin için oluşturulan uç nokta URL'sini kullanın. | 
    | **Önek filtresi** | / blobServices/varsayılan/kapsayıcıları/görüntüleri/BLOB'lar / | Üzerinde yalnızca depolama olayları filtreler **görüntüleri** kapsayıcı.| 

4. Tıklatın **oluşturma** olay aboneliği eklemek için. Bu tetikleyen bir olay aboneliği oluşturur **imageresizerfunc** için blob eklendiğinde **görüntüleri** kapsayıcı. Yeniden boyutlandırılan görüntüleri eklenir **başparmak** kapsayıcı.

Arka uç hizmetlerini yapılandırılır, örnek web uygulaması görüntüyü yeniden boyutlandırma işlevselliğini test etmek. 

## <a name="test-the-sample-app"></a>Örnek uygulamayı test etme

Web uygulaması yeniden boyutlandırma görüntü test etmek için yayımlanan uygulamanızın URL'ye gidin. Web uygulamasının varsayılan URL'si `https://<web_app>.azurewebsites.net` şeklindedir.

Tıklatın **karşıya fotoğraf** bölgesini seçin ve bir dosyayı karşıya yüklemek için. Fotoğraf bu bölgeye sürükleyebilirsiniz. 

Karşıya yüklenen görüntü kaybolur sonra karşıya yüklenen görüntü kopyası görüntülendiğini fark **küçük resimleri oluşturulan** Karusel. Bu görüntü, işlev tarafından yeniden boyutlandırılabilir, başparmak kapsayıcıya eklendi ve web istemcisi tarafından indirilir. 

![Edge tarayıcısında yayımlanan web uygulaması](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Genel Azure depolama hesabı oluşturma
> * Azure işlevleri kullanarak sunucusuz kod dağıtma
> * Olay kılavuzunda bir Blob Depolama olay aboneliği oluşturma

Güvenli Depolama hesabı erişim öğrenmek için depolama öğretici seri üç kısmına ilerleyin.

> [!div class="nextstepaction"]
> [Bulut uygulamaları verilere güvenli erişim](../storage/blobs/storage-secure-access-application.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)


+ Olay kılavuz hakkında daha fazla bilgi için bkz: [Azure olay kılavuzuna giriş](overview.md). 
+ Azure işlevleri özellikleri başka bir öğretici denemek için bkz: [Azure Logic Apps ile tümleşen bir işlev oluşturun](..\azure-functions\functions-twitter-email.md). 

[previous-tutorial]: ../storage/blobs/storage-upload-process-images.md