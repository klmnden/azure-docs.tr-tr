---
title: Hızlı Başlangıç - tarayıcıda JavaScript ve HTML kullanarak Azure depolamada blob oluşturma
description: Bir HTML sayfasında JavaScript kullanarak blobları karşıya yüklemek, listelemek ve silmek için bir BlobService örneğini kullanma hakkında bilgi edinin.
services: storage
keywords: depolama, javascript, html
author: KarlErickson
ms.custom: mvc
ms.service: storage
ms.author: karler
ms.reviewer: seguler
ms.date: 05/20/2019
ms.topic: quickstart
ms.subservice: blobs
ms.openlocfilehash: b16bbeee299f4879c14856a8041e6517968efebf
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481386"
---
<!-- Customer intent: As a web application developer I want to interface with Azure Blob storage entirely on the client so that I can build a SPA application that is able to upload and delete files on blob storage. -->

# <a name="quickstart-upload-list-and-delete-blobs-using-azure-storage-v10-sdk-for-javascripthtml-in-the-browser"></a>Hızlı Başlangıç: Karşıya yükleme, listeleme ve Azure depolama v10 SDK'sı, tarayıcıda JavaScript/HTML kullanarak blobları silme

Bu hızlı başlangıçta kullanmak bilgi edineceksiniz [JavaScript - Blob için Azure depolama SDK'sı V10](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-blob#readme) kitaplığı tamamen tarayıcıdan çalıştırılan JavaScript koddan blobları yönetme. Burada kullanılan yaklaşım, blob depolama hesabınıza korumalı erişimi güvence altına almak için gerekli güvenlik önlemlerinin nasıl kullanılacağını göstermektedir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Azure depolama JavaScript istemci kitaplıkları doğrudan dosya sisteminde çalışmaz ve bir web sunucusundan sunulmalıdır. Bu konuda kullanan [Node.js](https://nodejs.org) temel sunucusu başlatılamıyor. Düğüm yüklememeyi tercih ederseniz, bir yerel web sunucusu çalıştırmanın başka bir yolla kullanabilirsiniz.

Hata ayıklamayı adımları uygulamaya ihtiyacınız olacak [Visual Studio Code](https://code.visualstudio.com) ve her iki [Chrome için hata ayıklayıcı](vscode:extension/msjsdiag.debugger-for-chrome) veya [Microsoft Edge için hata ayıklayıcı](vscode:extension/msjsdiag.debugger-for-edge) uzantısı.

## <a name="setting-up-storage-account-cors-rules"></a>Depolama hesabı CORS kurallarını ayarlama

Web uygulamanızı bir blob depolama istemciden erişebilmeniz için önce hesabınızı etkinleştirmek için yapılandırmalısınız [çıkış noktaları arası kaynak paylaşımı](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services), veya CORS.

Azure portalına geri dönün depolama hesabınızı seçin. Yeni bir CORS kuralı için gidin **ayarları** tıklayın ve bölüm **CORS** bağlantı. Ardından, **Ekle** düğmesine tıklayarak **CORS kuralı ekle** penceresini açın. Bu hızlı başlangıçta bir açık CORS kuralı oluşturacaksınız:

![Azure Blob Depolama Hesabı CORS ayarları](media/storage-quickstart-blobs-javascript-client-libraries/azure-blob-storage-cors-settings.png)

Aşağıdaki tabloda her bir CORS ayarı açıklanmakta ve kuralı tanımlamak için kullanılan değerler anlatılmaktadır.

|Ayar  |Değer  | Açıklama |
|---------|---------|---------|
| İzin verilen çıkış noktaları | * | Kabul edilebilir çıkış noktaları olarak etki alanları kümesinin virgülle ayrılmış bir listesini kabul eder. Değerin `*` olarak ayarlanması, depolama hesabına tüm etki alanlarının erişmesine izin verir. |
| İzin verilen fiiller     | silme, alma, başlık, birleştirme, yayınlama, seçenekler ve yerleştirme | Depolama hesabına göre yürütülmesine izin verilen HTTP fiillerini listeler. Bu hızlı başlangıçta tüm kullanılabilir seçenekleri işaretleyin. |
| İzin verilen üst bilgiler | * | Depolama hesabı tarafından izin verilen istek üst bilgilerinin (ön ekli üst bilgiler dahil) listesini tanımlar. Değerin `*` olarak ayarlanması tüm üst bilgilere erişim izni verir. |
| Kullanıma sunulan üst bilgiler | * | Hesaba göre izin verilen yanıt üst bilgilerini listeler. Değerin `*` olarak ayarlanması hesabın herhangi bir üst bilgiyi göndermesine izin verir.  |
| En uzun geçerlilik süresi (saniye) | 86400 | Tarayıcının denetim öncesi SEÇENEKLER isteğini önbelleğe aldığı en uzun süre. *86400* değeri, önbelleğin bir tam gün boyunca kalmasına izin verir. |

> [!IMPORTANT]
> Üretimde kullandığınız tüm ayarların en az bir depolama hesabınıza güvenli erişimi sürdürmek için gerekli erişim miktarını kullanıma sunduğundan emin olun. Burada açıklanan CORS ayarları esnek bir güvenlik ilkesini tanımladığı için hızlı başlangıç için uygundur. Ancak bu ayarlar gerçek bir bağlam için önerilmez.

Bundan sonra, Azure Cloud Shell hizmetini kullanarak bir güvenlik belirteci oluşturacaksınız.

[!INCLUDE [Open the Azure cloud shell](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-shared-access-signature"></a>Paylaşılan erişim imzası oluşturma

Paylaşılan erişim imzası (SAS), Blob depolama alanına gönderilen isteklerini yetkilendirmek için tarayıcıda çalışan kod tarafından kullanılır. İstemci, SAS kullanarak hesap erişim anahtarı veya bağlantı dizesi kullanmadan depolama kaynaklarına erişimi yetkilendirebilir. SAS hakkında daha fazla bilgi edinmek için bkz. [Paylaşılan erişim imzaları (SAS) kullanma](../common/storage-dotnet-shared-access-signature-part-1.md).

Azure cloud shell aracılığıyla veya Azure portalı ile Azure CLI veya Azure Depolama Gezgini'ni kullanarak bir SAS oluşturabilirsiniz. Aşağıdaki tabloda, CLI ile bir SAS oluşturmak için değer sağlamanız gereken parametreler açıklanmıştır.

| Parametre      |Açıklama  | Yer tutucu |
|----------------|-------------|-------------|
| *expiry*       | Erişim belirtecinin YYYY-AA-GG biçimindeki sona erme tarihi. Bu hızlı başlangıçta kullanmak için yarının tarihini girin. | *FUTURE_DATE* |
| *account-name* | Depolama hesabı adı. Daha önceki bir adımda ayrılan adı kullanın. | *YOUR_STORAGE_ACCOUNT_NAME* |
| *account-key*  | Depolama hesabı anahtarı. Daha önceki bir adımda ayrılan anahtarı kullanın. | *YOUR_STORAGE_ACCOUNT_KEY* |

JavaScript kodunuzda kullanabileceğiniz bir SAS oluşturmak için her yer tutucusu için gerçek değerlerle aşağıdaki CLI komutunu kullanın.

```azurecli-interactive
az storage account generate-sas \
  --permissions racwdl \
  --resource-types sco \
  --services b \
  --expiry FUTURE_DATE \
  --account-name YOUR_STORAGE_ACCOUNT_NAME \
  --account-key YOUR_STORAGE_ACCOUNT_KEY
```

Her parametreden sonraki değer serisini biraz şifreli bulabilirsiniz. Bu parametre değerleri, kendi ilgili izninin ilk harfinden alınır. Aşağıdaki tabloda değerlerin nereden geldiği açıklanmıştır:

| Parametre        | Değer   | Açıklama  |
|------------------|---------|---------|
| *izinleri*    | racwdl  | Bu SAS *read*, *append*, *create*, *write*, *delete* ve *list* özelliklerine izin verir. |
| *resource-types* | sco     | SAS’den etkilenen kaynaklar *service*, *container* ve *object* kaynaklarıdır. |
| *services*       | b       | SAS’den etkilenen hizmet *blob* hizmetidir. |

SAS oluşturulduktan sonra dönüş değeri kopyalayın ve bunu bir yere bir sonraki adımda kullanmak üzere kaydedin. Azure CLI dışında bir yöntem kullanılarak, SAS oluşturursa, ilk kaldırmanız gerekecek `?` varsa. Bu URL şablonu bu konunun ilerleyen bölümlerinde sağlanan URL ayırıcı SAS kullanıldığı karakterdir.

> [!IMPORTANT]
> Üretimde her zaman SSL kullanarak SAS belirteçlerini geçirin. Ayrıca, SAS belirteçleri sunucu üzerinde oluşturulmalı ve Azure Blob Depolama’ya geri geçirmek için HTML sayfasına gönderilmelidir. Düşünebileceğiniz bir yaklaşım, SAS belirteçleri oluşturmak için bir sunucusuz bir işlev kullanmaktır. Azure Portal, bir JavaScript işlevi ile SAS oluşturma özelliğine sahip işlev şablonları içerir.

## <a name="implement-the-html-page"></a>HTML sayfasını uygulama

Bu bölümde, temel bir web sayfası oluşturmak ve başlatmak ve hata ayıklama sayfası için VS Code'u yapılandırma. Ancak, başlatabilirsiniz önce bir yerel web sunucusunu başlatmak ve tarayıcınızı istediğinde sayfaya hizmet etmek için Node.js kullanma gerekir. Ardından, çeşitli blob depolama API'leri çağırmak ve sonuçları sayfasında görüntülemek için JavaScript kodu ekleyeceksiniz. Bu çağrılarında sonuçlarını atabilirsiniz [Azure portalında](https://portal.azure.com), [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer)ve [Azure depolama uzantısı](vscode:extension/ms-azuretools.vscode-azurestorage) VS Code için.

### <a name="set-up-the-web-application"></a>Web uygulamasını ayarlama

İlk olarak, adlı yeni bir klasör oluşturun *azure-blobs-javascript* ve VS Code'da açın. Ardından VS Code'da yeni bir dosya oluşturun, aşağıdaki HTML'yi ekleyin ve kaydedileceği *index.html* içinde *azure-blobs-javascript* klasör.

```html
<!DOCTYPE html>
<html>

<body>
    <button id="create-container-button">Create container</button>
    <button id="delete-container-button">Delete container</button>
    <button id="select-button">Select and upload files</button>
    <input type="file" id="file-input" multiple style="display: none;" />
    <button id="list-button">List files</button>
    <button id="delete-button">Delete selected files</button>
    <p><b>Status:</b></p>
    <p id="status" style="height:160px; width: 593px; overflow: scroll;" />
    <p><b>Files:</b></p>
    <select id="file-list" multiple style="height:222px; width: 593px; overflow: scroll;" />
</body>

<!-- You'll add code here later in this quickstart. -->

</html>
```

### <a name="configure-the-debugger"></a>Hata Ayıklayıcı Yapılandırma

VS code'da hata ayıklama uzantısı ayarlamak için seçin **hata ayıklama > Yapılandırması Ekle...** , ardından **Chrome** veya **Edge**uzantısı, yüklü önkoşullar bölümünde daha önce bağlı olarak. Bu eylem, oluşturur bir *launch.json* dosya ve düzenleyicide açılır.

Ardından, değişiklik *launch.json* dosya böylece `url` değeri içeren `/index.html` gösterildiği gibi:

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://localhost:8080/index.html",
            "webRoot": "${workspaceFolder}"
        }
    ]
}
```

Bu yapılandırma, VS Code başlatmak için tarayıcıyı ve yüklemek için hangi URL söyler.

### <a name="launch-the-web-server"></a>Web sunucusu başlatılamıyor

Yerel Node.js web sunucusunu başlatmak için **Görüntüle > Terminal** VS Code içindeki bir konsol penceresi açmak için ardından aşağıdaki komutu girin.

```console
npx http-server
```

Bu komut yükleyecek *http-server* paketini ve geçerli klasörde önceki adımda belirtilen de dahil olmak üzere varsayılan URL aracılığıyla kullanılabilir hale getirme sunucusu başlatılamıyor.

### <a name="start-debugging"></a>Hata Ayıklamayı Başlat

Başlatmak için *index.html* ile VS Code hata ayıklayıcı iliştirilmiş, select tarayıcıda **hata ayıklama > hata ayıklamayı Başlat** veya VS Code'da F5 tuşuna basın.

Görüntülenen kullanıcı Arabirimi henüz bir işe yaramaz, ancak gösterilen her bir işlevin uygulamak için aşağıdaki bölümde, JavaScript kodu ekleyeceksiniz. Ardından, kesme noktaları ayarlayın ve kodunuz üzerinde durakladığında hata ayıklayıcısı ile etkileşim.

Değişiklikler yaptığınızda *index.html*, tarayıcı değişiklikleri görmek için sayfayı yeniden emin olun. VS Code'da de seçebilirsiniz **hata ayıklama > hata ayıklama yeniden** ya da CTRL + SHIFT + F5 tuşuna basın.

### <a name="add-the-blob-storage-client-library"></a>Blob depolama istemci kitaplığı Ekle

Blob depolama, API çağrıları etkinleştirmeyi [JavaScript - Blob istemci kitaplığı Azure depolama SDK'sını indirin](https://aka.ms/downloadazurestoragejsblob), zip dosyasının içeriğini ayıklayın ve yerleştirin *azure storage.blob.js* dosyasında*azure-blobs-javascript* klasör.

Ardından, aşağıdaki HTML'e yapıştırın *index.html* sonra `</body>` kapanış etiketi, yer tutucu açıklamayı değiştirin.

```html
<script src="azure-storage.blob.js" charset="utf-8"></script>

<script>
// You'll add code here in the following sections.
</script>
```

Bu kod, komut dosyasına bir başvuru ekler ve kendi JavaScript kodu için bir yer sağlar. Bu hızlı başlangıç amaçları doğrultusunda, kullandığımız *azure storage.blob.js* betiği VS Code'da açın, içeriğini okumak ve kesme noktaları ayarlayın. Üretim ortamında, daha fazla compact kullanması gereken *azure storage.blob.min.js* zip dosyasında da sağlanan.

Her blob depolama işlevinde hakkında daha fazla bilgi bulabilirsiniz [başvuru belgeleri](https://docs.microsoft.com/javascript/api/%40azure/storage-blob/index). Bazı SDK'sındaki işlevleri yalnızca kullanılabilir node.js'de veya tarayıcıda yalnızca kullanılabilir olduğunu unutmayın.

Kodda *azure storage.blob.js* adlı bir genel değişken aktarır `azblob`, API'leri blob depolamaya erişmek için JavaScript kodunuzda kullanacağınız.

### <a name="add-the-initial-javascript-code"></a>İlk JavaScript kodunu ekleyin

Ardından, aşağıdaki kodu yapıştırın `<script>` yer tutucu açıklamayı değiştirerek önceki kod bloğunda gösterildiği gibi.

```javascript
const createContainerButton = document.getElementById("create-container-button");
const deleteContainerButton = document.getElementById("delete-container-button");
const selectButton = document.getElementById("select-button");
const fileInput = document.getElementById("file-input");
const listButton = document.getElementById("list-button");
const deleteButton = document.getElementById("delete-button");
const status = document.getElementById("status");
const fileList = document.getElementById("file-list");

const reportStatus = message => {
    status.innerHTML += `${message}<br/>`;
    status.scrollTop = status.scrollHeight;
}
```

Aşağıdaki kodu kullanan her bir HTML öğesi ve uyguladığı için alanları bu kod oluşturur bir `reportStatus` çıkışını görüntülemek için işlev.

Aşağıdaki bölümlerde, her yeni JavaScript kod bloğunun önceki blok sonra ekleyin.

### <a name="add-your-storage-account-info"></a>Depolama hesabı bilgilerinizi ekleyin

Ardından, hesabınızın adını ve bir önceki adımda oluşturulan SAS yer tutucular yerine depolama hesabınızın erişmek için kod ekleyin.

```javascript
const accountName = "<Add your storage account name>";
const sasString = "<Add the SAS you generated earlier>";
const containerName = "testcontainer";
const containerURL = new azblob.ContainerURL(
    `https://${accountName}.blob.core.windows.net/${containerName}?${sasString}`,
    azblob.StorageURL.newPipeline(new azblob.AnonymousCredential)));
```

Bu kod oluşturmak için SAS ve hesap bilgilerini kullanan bir [ContainerURL](https://docs.microsoft.com/javascript/api/@azure/storage-blob/ContainerURL) oluşturmak ve bir depolama kapsayıcısı işlemek için yararlı olan örnek.

### <a name="create-and-delete-a-storage-container"></a>Oluşturma ve depolama kapsayıcısını silme

Ardından, oluşturmak ve karşılık gelen düğmesine bastığınızda, depolama kapsayıcısını silmek için kod ekleyin.

```javascript
const createContainer = async () => {
    try {
        reportStatus(`Creating container "${containerName}"...`);
        await containerURL.create(azblob.Aborter.none);
        reportStatus(`Done.`);
    } catch (error) {
        reportStatus(error.body.message);
    }
};

const deleteContainer = async () => {
    try {
        reportStatus(`Deleting container "${containerName}"...`);
        await containerURL.delete(azblob.Aborter.none);
        reportStatus(`Done.`);
    } catch (error) {
        reportStatus(error.body.message);
    }
};

createContainerButton.addEventListener("click", createContainer);
deleteContainerButton.addEventListener("click", deleteContainer);
```

Bu kod ContainerURL çağırır [oluşturma](https://docs.microsoft.com/javascript/api/@azure/storage-blob/ContainerURL#create-aborter--icontainercreateoptions-) ve [Sil](https://docs.microsoft.com/javascript/api/@azure/storage-blob/ContainerURL#delete-aborter--icontainerdeletemethodoptions-) kullanmadan işlevleri bir [Aborter](https://docs.microsoft.com/javascript/api/@azure/storage-blob/aborter) örneği. Bu hızlı başlangıçta basit bir anlatım gözetildiği için bu kod, depolama hesabınız oluşturulduktan ve etkin olduğunu varsayar. Üretim kodunda Aborter örneği zaman aşımı işlevselliği eklemek için kullanın.

### <a name="list-blobs"></a>Blobları listeleme

Ardından, tuşuna bastığınızda, depolama kapsayıcısını içeriğini listelemek için kod ekleyin **listesi dosyalar** düğmesi.

```javascript
const listFiles = async () => {
    fileList.size = 0;
    fileList.innerHTML = "";
    try {
        reportStatus("Retrieving file list...");
        let marker = undefined;
        do {
            const listBlobsResponse = await containerURL.listBlobFlatSegment(
                azblob.Aborter.none, marker);
            marker = listBlobsResponse.nextMarker;
            const items = listBlobsResponse.segment.blobItems;
            for (const blob of items) {
                fileList.size += 1;
                fileList.innerHTML += `<option>${blob.name}</option>`;
            }
        } while (marker);
        if (fileList.size > 0) {
            reportStatus("Done.");
        } else {
            reportStatus("The container does not contain any files.");
        }
    } catch (error) {
        reportStatus(error.body.message);
    }
};

listButton.addEventListener("click", listFiles);
```

Bu kod [ContainerURL.listBlobFlatSegment](https://docs.microsoft.com/javascript/api/@azure/storage-blob/ContainerURL#listblobflatsegment-aborter--string--icontainerlistblobssegmentoptions-) tüm parçaları alındığından emin olmak için bir döngü işlevi. İsteğe bağlı olarak her bir kesim içeriyor ve güncelleştirmeleri blob öğeleri listesi döngü **dosyaları** listesi.

### <a name="upload-blobs"></a>Blobları karşıya yüklemek

Ardından, dosyaları, tuşuna bastığınızda, depolama kapsayıcısına karşıya yüklemek için kod ekleyin **seçme ve dosyaları karşıya yükleme** düğmesi.

```javascript
const uploadFiles = async () => {
    try {
        reportStatus("Uploading files...");
        const promises = [];
        for (const file of fileInput.files) {
            const blockBlobURL = azblob.BlockBlobURL.fromContainerURL(containerURL, file.name);
            promises.push(azblob.uploadBrowserDataToBlockBlob(
                azblob.Aborter.none, file, blockBlobURL));
        }
        await Promise.all(promises);
        reportStatus("Done.");
        listFiles();
    } catch (error) {
        reportStatus(error.body.message);
    }
}

selectButton.addEventListener("click", () => fileInput.click());
fileInput.addEventListener("input", uploadFiles);
```

Bu kod bağlanır **seçme ve dosyaları karşıya yükleme** düğmeye gizli `file-input` öğesi. Bu şekilde, düğmeyi `click` olayını harekete dosyası girişini `click` olay ve dosya seçici görüntülenir. Dosyaları seçin ve iletişim kutusunu kapatmak sonra `input` olayı oluşur ve `uploadFiles` işlevi çağrılır. Tarayıcı yalnızca bu işlevi çağıran [uploadBrowserDataToBlockBlob](https://docs.microsoft.com/javascript/api/@azure/storage-blob/#uploadbrowserdatatoblockblob-aborter--blob---arraybuffer---arraybufferview--blockbloburl--iuploadtoblockbloboptions-) seçtiğiniz her dosya için işlevi. Her çağrı bir listesine eklenir ve böylece bunların tümü aynı anda paralel olarak karşıya yüklenecek dosyaları neden bekleniyor olabilir bir Promise döndürür.

### <a name="delete-blobs"></a>Blob’ları silme

Ardından, tuşuna bastığınızda, depolama kapsayıcısından dosyaları silmek için kod ekleyin **seçili dosyaları silmek** düğmesi.

```javascript
const deleteFiles = async () => {
    try {
        if (fileList.selectedOptions.length > 0) {
            reportStatus("Deleting files...");
            for (const option of fileList.selectedOptions) {
                const blobURL = azblob.BlobURL.fromContainerURL(containerURL, option.text);
                await blobURL.delete(azblob.Aborter.none);
            }
            reportStatus("Done.");
            listFiles();
        } else {
            reportStatus("No files selected.");
        }
    } catch (error) {
        reportStatus(error.body.message);
    }
};

deleteButton.addEventListener("click", deleteFiles);
```

Bu kod [BlobURL.delete](https://docs.microsoft.com/javascript/api/@azure/storage-blob/BlobURL#delete-aborter--iblobdeleteoptions-) liste kutusunda seçili her dosyayı kaldırmak için işlevi. Ardından `listFiles` içeriğini yenilemek için daha önce gösterilen işlev **dosyaları** listesi.

### <a name="run-and-test-the-web-application"></a>Çalıştırma ve web uygulamasını test etme

Bu noktada, başlatın sayfa ve bir genel görünüm hakkında bilgi almak için deneme blob depolama çalışır. (Kapsayıcı oluşturduğunuz önce gibi dosyaları listelemek için çalıştığınızda) herhangi bir hata oluşursa, **durumu** bölmesinde alınan hata iletisi görüntülenir. Depolama API'leri tarafından döndürülen değerleri incelemek için JavaScript kodunda kesme noktaları da ayarlayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıç sırasında oluşturulan kaynakları temizlemek için şuraya gidin: [Azure portalında](https://portal.azure.com) ve önkoşullar bölümünde oluşturduğunuz kaynak grubunu silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir Web sitesi erişimi JavaScript tarayıcı tabanlı depolamadan blob oluşturdunuz. Kendi blob depolama, nasıl bir Web sitesi barındırabilirsiniz. bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Blob Depolama üzerinde statik Web sitesi barındırma](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website-host)
