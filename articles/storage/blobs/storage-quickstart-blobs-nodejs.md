---
title: Azure Hızlı Başlangıç - Node.js kullanarak nesne depolamada blob oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, nesne (Blob) depolamada depolama hesabı ve kapsayıcı oluşturursunuz. Sonra, Azure Depolama’ya blob yüklemek, blob indirmek ve bir kapsayıcıdaki blobları listelemek amacıyla Node.js için depolama istemcisi kitaplığını kullanırsınız.
services: storage
author: craigshoemaker
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 04/09/2018
ms.author: cshoe
ms.openlocfilehash: 30a64ec6fd4df63eba9c35f1774c81c35fa3506f
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="quickstart-upload-download-and-list-blobs-using-nodejs"></a>Hızlı Başlangıç: Node.js’yi kullanarak blobları yükleme, indirme ve listeleme

Bu hızlı başlangıçta, Azure Blob depolamayı kullanarak bir kapsayıcıda blok bloblarını karşıya yüklemek, indirmek ve listelemek için Node.js’yi nasıl kullanabileceğinizi öğreneceksiniz.

Bu hızlı başlangıcı tamamlamak bir [Azure aboneliğinizin](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) olması gerekir.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Bu hızlı başlangıçtaki [örnek uygulama](https://github.com/Azure-Samples/storage-blobs-node-quickstart.git), basit bir Node.js konsol uygulamasıdır. Başlamak için, aşağıdaki komutu kullanarak depoyu makinenize kopyalayın:

```bash
git clone https://github.com/Azure-Samples/storage-blobs-node-quickstart.git
```

Uygulamayı açmak için, *storage-blobs-node-quickstart* klasörünü arayın ve sık kullandığınız kod düzenleme ortamınızda açın.

[!INCLUDE [storage-copy-connection-string-portal](../../../includes/storage-copy-connection-string-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma

Uygulamayı çalıştırmadan önce, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. Örnek depo, *.env.example* adlı bir dosya içerir. *.example* uzantısını kaldırarak bu dosyayı yeniden adlandırabilirsiniz. Böylece *.env* adlı bir dosya elde edilir. *.env* dosyasının içinde, *AZURE_STORAGE_CONNECTION_STRING* anahtarının sonrasına bağlantı dizesi değerinizi ekleyin.

## <a name="install-required-packages"></a>Gerekli paketleri yükleme

Uygulama dizininde *npm install* komutunu çalıştırarak uygulama için gerekli paketleri yükleyin.

```bash
npm install
```

## <a name="run-the-sample"></a>Örneği çalıştırma
Bağımlılıklar yüklendiğine göre şimdi betiğe komutlar göndererek örneği çalıştırabilirsiniz. Örneğin, bir blob kapsayıcısı oluşturmak için aşağıdaki komutu çalıştırın:

```bash
node index.js --command createContainer
```

Kullanılabilir komutlar aşağıdaki gibidir:

| Komut | Açıklama |
|---------|---------|
|*createContainer* | *test-container* adlı bir kapsayıcı oluşturur (kapsayıcı önceden mevcut olsa da başarılı olur) |
|*upload*          | *example.txt* dosyasını *test-container* kapsayıcısına yükler |
|*download*        | *example* blobunun içeriklerini *example.downloaded.txt* hedefine indirir |
|*sil*          | *example* blobunu siler |
|*list*            | *test-container* kapsayıcısının içeriklerini konsolda listeler |


## <a name="understanding-the-sample-code"></a>Örnek kodu anlama
Bu kod, dosya sistemi ve komut satırı ile arabirim oluşturmak için birkaç modül kullanır. 

```javascript
if (process.env.NODE_ENV !== 'production') {
    require('dotenv').load();
}
const path = require('path');
const args = require('yargs').argv;
const storage = require('azure-storage');
```

Modüllerin amacı aşağıdaki gibidir: 

- *dotenv*, *.env* adlı bir dosyada tanımlanan ortam değişkenlerini geçerli yürütme bağlamına yükler
- *path*, blob depolamaya yüklenecek dosyanın mutlak dosya yolunu belirlemek için gereklidir
- *yargs*, komut satırı bağımsız değişkenlerine erişmek için basit bir arabirim sunar
- *azure-storage*, Node.js için [Azure Depolama SDK’sı](https://docs.microsoft.com/javascript/api/azure-storage) modülüdür

Daha sonra bir değişken serisi başlatılır:

```javascript
const blobService = storage.createBlobService();
const containerName = 'test-container';
const sourceFilePath = path.resolve('./example.txt');
const blobName = path.basename(sourceFilePath, path.extname(sourceFilePath));
```

Değişkenler aşağıdaki değerlere ayarlanır:

- *blobService*, Azure Blob hizmetinin yeni bir örneğine ayarlanır
- *containerName*, kapsayıcının adına ayarlanır
- *sourceFilePath*, karşıya yüklenecek dosyanın mutlak yoluna ayarlanır
- *blobName*, dosya adı alınıp dosya uzantısı kaldırılarak oluşturulur

Aşağıdaki uygulamada, *blobService* işlevlerinin her biri bir *Promise* içinde kaydırılır ve bu da [Azure Depolama API](/nodejs/api/azure-storage/blobservice)’sinin geri çağrı yapısını kolaylaştırmak için JavaScript'in *async* işlevine ve *await* işlecine erişilmesini sağlar. Her bir işlev için başarılı bir yanıt döndürüldüğünde Promise, eyleme özgü bir iletiyle birlikte alakalı verilerle çözümleme gerçekleştirir.

### <a name="create-a-blob-container"></a>Blob kapsayıcısı oluşturma

*createContainer* işlevi, [createContainerIfNotExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists) öğesini çağırır ve blob için uygun erişim düzeyini ayarlar.

```javascript
const createContainer = () => {
    return new Promise((resolve, reject) => {
        blobService.createContainerIfNotExists(containerName, { publicAccessLevel: 'blob' }, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Container '${containerName}' created` });
            }
        });
    });
};
```

**createContainerIfNotExists** için ikinci parametre (*options*), [publicAccessLevel](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists) için bir değer kabul eder. *publicAccessLevel* için *blob* değeri, belirli blob verilerinin genel kullanıma sunulduğunu belirtir. Bu ayar, kapsayıcının içeriklerini listeleme yeteneği sağlayan *kapsayıcı* düzeyinde erişimin tersidir.

**createContainerIfNotExists** kullanılması, kapsayıcı önceden mevcut olduğunda uygulamanın hata döndürmeden *createContainer* komutunu birçok defa çalıştırmasına olanak sağlar. Üretim ortamında, uygulama genelinde aynı kapsayıcı kullanıldığında çoğu zaman **createContainerIfNotExists** komutunu yalnızca bir defa çağırırsınız. Bu durumlarda, önceden portal veya Azure CLI aracılığıyla kapsayıcı oluşturabilirsiniz.

### <a name="upload-a-blob-to-the-container"></a>Bir kapsayıcıya blob yükleme

*upload* işlevi, dosya sistemindeki bir dosyayı blob depolamaya yüklemek ve yazmak veya söz konusu dosyanın üzerine yazmak için [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createBlockBlobFromLocalFile) işlevini kullanır. 

```javascript
const upload = () => {
    return new Promise((resolve, reject) => {
        blobService.createBlockBlobFromLocalFile(containerName, blobName, sourceFilePath, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Upload of '${blobName}' complete` });
            }
        });
    });
};
```
Örnek uygulama bağlamında, *test-container* adlı bir kapsayıcının içinde *example* adlı bir bloba *example.txt* adlı dosya yüklenir. Bloblara içerik yüklemenin başka bir yolu da [text](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createBlockBlobFromText) ve [streams](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createBlockBlobFromStream) ile çalışmaktır.

Dosyanın blob depolamanıza yüklendiğini doğrulamak için [Azure Depolama Gezgini](https://azure.microsoft.com/en-us/features/storage-explorer/)’ni kullanarak hesabınızdaki verileri görüntüleyebilirsiniz.

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

*list* işlevi, bir kapsayıcıdaki blob meta verileri listesini döndürmek için [listBlobsSegmented](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createBlockBlobFromText) yöntemini çağırır. 

```javascript
const list = () => {
    return new Promise((resolve, reject) => {
        blobService.listBlobsSegmented(containerName, null, (err, data) => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Items in container '${containerName}':`, data: data });
            }
        });
    });
};
```

*listBlobsSegmented* çağrıldığında, [BlobResult](/nodejs/api/azure-storage/blobresult) örnekleri dizisi olarak blob meta verileri döndürülür. Sonuçlar, 5.000’er olarak artan toplu işler (segmentler) halinde döndürülür. Bir kapsayıcıda 5.000’den fazla blob varsa, sonuçlar **continuationToken** için bir değer içerir. Blob kapsayıcısında yer alan sonraki segmentleri listelemek için, ikinci bağımsız değişken olarak devamlılık belirtecini **listBlobSegmented** öğesine geri gönderebilirsiniz.

### <a name="download-a-blob-from-the-container"></a>Kapsayıcıdan blob indirme

*download* işlevi, belirtilen mutlak dosya yoluna blobun içeriklerini indirmek için [getBlobToLocalFile](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_getBlobToLocalFile) öğesini kullanır.

```javascript
const download = () => {
    const dowloadFilePath = sourceFilePath.replace('.txt', '.downloaded.txt');
    return new Promise((resolve, reject) => {
        blobService.getBlobToLocalFile(containerName, blobName, dowloadFilePath, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Download of '${blobName}' complete` });
            }
        });
    });
};
```
Burada gösterilen uygulama, dosya adının sonuna *.downloaded.txt* eklemek için kaynak dosya yolunu değiştirir. Gerçek dünya bağlamında, bir indirme hedefini seçerken dosya adının yanı sıra konumu da değiştirebilirsiniz.

### <a name="delete-blobs-in-the-container"></a>Kapsayıcıdaki blobları silme

*deleteBlock* işlevi (diğer adı *delete* konsol komutu olan), [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteBlobIfExists) işlevini çağırır. Adından da anlaşılacağı gibi bu işlev, blob önceden silindiyse bir hata döndürmez.

```javascript
const deleteBlock = () => {
    return new Promise((resolve, reject) => {
        blobService.deleteBlobIfExists(containerName, blobName, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Block blob '${blobName}' deleted` });
            }
        });
    });
};
```

### <a name="upload-and-list"></a>Karşıya yükleme ve listeleme

Promise kullanmanın avantajlarından biri, zincir komutların birlikte çağrılabilmesidir. **uploadAndList** işlevi, bir dosya karşıya yüklendikten sonra bir Blobun içeriklerinin listelenmesinin ne kadar kolay olduğunu gösterir.

```javascript
const uploadAndList = () => {
    return _module.upload().then(_module.list);
};
```

### <a name="calling-code"></a>Kodu çağırma

Komut satırına uygulanan işlevleri kullanıma sunmak için işlevlerin her biri bir nesne değişmez değerine eşlenir.

```javascript
const _module = {
    "createContainer": createContainer,
    "upload": upload,
    "download": download,
    "delete": deleteBlock,
    "list": list,
    "uploadAndList": uploadAndList
};
```

Şimdi *_module* öğesinin bulunmasıyla birlikte komutların her birine komut satırından ulaşılabilir.

```javascript
const commandExists = () => exists = !!_module[args.command];
```

Verilen bir komut yoksa, kullanıcıya yardım metni olarak, *_module* özellikleri konsola işlenir. 

*executeCommand* işlevi, *await* işlevini kullanarak verilen komutu çağıran ve verilere yönelik iletileri konsolda günlüğe kaydeden bir *async* işlevidir.

```javascript
const executeCommand = async () => {
    const response = await _module[args.command]();

    console.log(response.message);

    if (response.data) {
        response.data.entries.forEach(entry => {
            console.log('Name:', entry.name, ' Type:', entry.blobType)
        });
    }
};
```

Son olarak, önce kod yürütüldüğünde, bilinen bir komutun betiğe gönderildiğini doğrulamak için *commandExists* çağrılır. Mevcut bir komut seçilirse, komut çalıştırılır ve tüm hatalar konsolda günlüğe kaydedilir.

```javascript
try {
    const cmd = args.command;

    console.log(`Executing '${cmd}'...`);

    if (commandExists()) {
        executeCommand();
    } else {
        console.log(`The '${cmd}' command does not exist. Try one of these:`);
        Object.keys(_module).forEach(key => console.log(` - ${key}`));
    }
} catch (e) {
    console.log(e);
}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu makalede oluşturulan verileri veya hesapları kullanmayı planlamıyorsanız, istenmeyen faturaları önlemek için bunları silmek isteyebilirsiniz. Blobu ve kapsayıcıları silmek için [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice?view=azure-node-latest#deleteBlobIfExists_container__blob__options__callback_) ve [deleteContainerIfExists](/nodejs/api/azure-storage/blobservice?view=azure-node-latest#deleteContainerIfExists_container__options__callback_) yöntemlerini kullanabilirsiniz. Depolama hesabını [portaldan da](../common/storage-create-storage-account.md) silebilirsiniz.

## <a name="resources-for-developing-nodejs-applications-with-blobs"></a>Bloblarla Node.js uygulamaları geliştirme kaynakları

Blob depolama ile Node.js geliştirmeye yönelik şu ek kaynaklara bakın:

### <a name="binaries-and-source-code"></a>İkili dosyalar ve kaynak kodu

- GitHub’da Azure Depolama için [Node.js istemci kitaplığı kaynak kodunu](https://github.com/Azure/azure-storage-node) görüntüleyin ve yükleyin.

### <a name="client-library-reference-and-samples"></a>İstemci kitaplığı başvurusu ve örnekleri

- Node.js istemci kitaplığı hakkında daha fazla bilgi için bkz. [Node.js API başvurusu](https://docs.microsoft.com/en-us/javascript/api/overview/azure/storage).
- Node.js istemci kitaplığı kullanılarak yazılmış [Blob depolama örneklerini](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=nodejs&term=blob) araştırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Node.js kullanarak yerel bir disk ve Azure Blob depolama arasında nasıl dosya karşıya yükleneceği gösterilmektedir. Blob depolamayla çalışma hakkında daha fazla bilgi edinmek için, Blob depolama nasıl yapılır öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Blob Depolama İşlemleri Nasıl Yapılır](storage-nodejs-how-to-use-blob-storage.md)

Azure Depolama’ya yönelik Node.js başvurusu için bkz. [azure-storage package](https://docs.microsoft.com/javascript/api/azure-storage).