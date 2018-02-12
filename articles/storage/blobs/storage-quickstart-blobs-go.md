---
title: "Azure Hızlı Başlangıç - Go kullanarak nesneleri Azure Blob depolama içine/dışına aktarma | Microsoft Docs"
description: "Go lang kullanarak nesneleri Azure Blob depolama içine/dışına aktarmayı kısa sürede öğrenin"
services: storage
author: seguler
manager: jahogg
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: go
ms.topic: quickstart
ms.date: 01/29/2018
ms.author: seguler
ms.openlocfilehash: 4ba9721dc12bc50b20ad85019b1df51a56b52ebc
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
#  <a name="transfer-objects-tofrom-azure-blob-storage-using-go"></a>Go kullanarak nesneleri Azure Blob depolama içine/dışına aktarma
Bu hızlı başlangıçta, Azure Blob depolamadaki bir kapsayıcıda blok bloblarını karşıya yüklemek, indirmek ve listelemek için Go programlama dilini nasıl kullanabileceğinizi öğreneceksiniz. 

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için: 
* [Go 1.8 veya üstünü ](https://golang.org/dl/) yükleyin.
* `go get -u github.com/azure/azure-storage-blob-go/2016-05-31/azblob` kullanarak [Go için Azure Depolama Blobu SDK'sını](https://github.com/azure/azure-storage-blob-go/) indirin ve yükleyin. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:
Bu [hızlı başlangıçta](https://github.com/Azure-Samples/storage-blobs-go-quickstart.git) kullanılan örnek uygulama, temel bir Go uygulamasıdır.  

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek için [Git](https://git-scm.com/)'i kullanın. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-go-quickstart 
```

Bu komut, depoyu yerel Git klasörünüze kopyalar. Blob depolama alanının Go örneğini açmak için, storage-quickstart.go dosyasını bulun.  

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma
Bu çözüm, depolama hesabı adınızın ve anahtarınızın çözümü çalıştıran makinede yerel olarak bulunan ortam değişkenlerinde güvenli bir şekilde depolanmasını gerektirir. Ortam değişkenlerini oluşturmak için işletim sisteminize bağlı olarak aşağıdaki örneklerden birini izleyin.

# <a name="linuxtablinux"></a>[Linux](#tab/Linux)

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

# <a name="windowstabwindows"></a>[Windows](#tab/Windows)

```
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

---

## <a name="run-the-sample"></a>Örneği çalıştırma
Bu örnek geçerli klasörde bir test dosyası oluşturur, test dosyasını Blob depolamaya yükler, kapsayıcıdaki blobları listeler ve dosyayı bir arabelleğe indirir. 

Örneği çalıştırmak için aşağıdaki komutu yürütün: 

```go run storage-quickstart.go```

Aşağıdaki çıktı, uygulama çalıştırılırken döndürülen çıktının bir örneğidir:
  
```
Azure Blob storage quick start sample
Creating a container named quickstart-5568059279520899415
Creating a dummy file to test the upload and download
Uploading the file with blob name: 630910657703031215
Blob name: 630910657703031215
Downloaded the blob: hello world
this is a blob
Press the enter key to delete the sample files, example container, and exit the application.
```
Devam etmek için tuşa bastığınızda, örnek program depolama kapsayıcısını ve dosyaları siler. 

> [!TIP]
> Ayrıca, Blob depolamadaki dosyaları görüntülemek için, [Azure Depolama Gezgini](http://storageexplorer.com) gibi bir araç da kullanabilirsiniz. Azure Depolama Gezgini, depolama hesabı bilgilerinize erişmenize olanak tanıyan ücretsiz ve platformlar arası bir araçtır. 
>

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Sonraki aşamada, nasıl çalıştığını anlayabilmeniz için örnek kodu inceleyeceğiz.

### <a name="create-containerurl-and-bloburl-objects"></a>ContainerURL ve BlobURL nesnelerini oluşturma
İlk yapılacak olan, Blob depolamaya erişmek ve Blob depolamayı yönetmek için kullanılan ContainerURL ve BlobURL nesnelerine başvuru oluşturmaktır. Bu nesneler, REST API'lerini göndermek için Create, PutBlob ve GetBlob gibi alt düzey API'ler sunar.

* Kimlik bilgilerinizi depolamak için **SharedKeyCredential** struct'ını kullanın. 

* Kimlik bilgilerini ve seçenekleri kullanarak bir **işlem hattı** oluşturun. İşlem hattı yeniden deneme ilkeleri, günlük ve HTTP yanıtı iş yüklerini seri durumdan çıkarma gibi daha birçok öğeyi belirtir.  

* Kapsayıcı (Create) ve bloblar (PutBlob ve GetBlob) üzerinde işlemleri çalıştırmak için yeni ContainerURL'yi ve yeni BlobURL nesnesini başlatın.


ContainerURL'niz olduğunda, bir bloba işaret eden **BlobURL** nesnesini başlatabilir ve karşıya yükleme, indirme ve kopyalama gibi işlemler yapabilirsiniz.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcılar ve blob adları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

Bu bölümde, yeni bir kapsayıcı oluşturursunuz. Bu, **quickstartblobs-[random string]** adlı kapsayıcıdır. 

```go 
// From the Azure portal, get your storage account name and key and set environment variables.
accountName, accountKey := os.Getenv("AZURE_STORAGE_ACCOUNT"), os.Getenv("AZURE_STORAGE_ACCESS_KEY")
if len(accountName) == 0 || len(accountKey) == 0 {
    log.Fatal("Either the AZURE_STORAGE_ACCOUNT or AZURE_STORAGE_ACCESS_KEY environment variable is not set")
}

// Create a default request pipeline using your storage account name and account key.
credential := azblob.NewSharedKeyCredential(accountName, accountKey)
p := azblob.NewPipeline(credential, azblob.PipelineOptions{})

// Create a random string for the quick start container
containerName := fmt.Sprintf("quickstart-%s", randomString())

// From the Azure portal, get your storage account blob service URL endpoint.
URL, _ := url.Parse(
    fmt.Sprintf("https://%s.blob.core.windows.net/%s", accountName, containerName))

// Create a ContainerURL object that wraps the container URL and a request
// pipeline to make requests.
containerURL := azblob.NewContainerURL(*URL, p)

// Create the container
fmt.Printf("Creating a container named %s\n", containerName)
ctx := context.Background() // This example uses a never-expiring context
_, err := containerURL.Create(ctx, azblob.Metadata{}, azblob.PublicAccessNone)
handleErrors(err)
```
### <a name="upload-blobs-to-the-container"></a>Blobları kapsayıcıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blok blobları en sık kullanılan bloblardır ve bu hızlı başlangıçta bu bloblar kullanılmıştır.  

Bloba dosya yüklemek için, **os.Open** kullanarak dosyayı açın. Ardından, dosyayı belirtilen yola yüklemek için REST API'lerden birini kullanabilirsiniz: PutBlob, PutBlock/PutBlockList. 

Alternatif olarak, SDK alt düzey REST API'lerinin üstüne yapılandırılmış [üst düzey API'ler](https://github.com/Azure/azure-storage-blob-go/blob/master/2016-05-31/azblob/highlevel.go) sağlar. Örnek vermek gerekirse, ***UploadFileToBlockBlob*** işlevi, aktarım hızını iyileştirmek için PutBlock işlemlerini kullanarak bir dosyayı öbekler halinde eşzamanlı olarak karşıya yükler. Dosya 256 MB'den küçükse, aktarımı tek işlemde tamamlamak için onun yerine PutBlob kullanır.

Aşağıdaki örnek, dosyayı **quickstartblobs-[randomstring]** adlı kapsayıcınıza yükler.

```go
// Here's how to upload a blob.
blobURL := containerURL.NewBlockBlobURL(fileName)
file, err := os.Open(fileName)
handleErrors(err)

// You can use the low-level PutBlob API to upload files. Low-level APIs are simple wrappers for the Azure Storage REST APIs.
// Note that PutBlob can upload up to 256MB data in one shot. Details: https://docs.microsoft.com/en-us/rest/api/storageservices/put-blob
// Following is commented out intentionally because we will instead use UploadFileToBlockBlob API to upload the blob
// _, err = blobURL.PutBlob(ctx, file, azblob.BlobHTTPHeaders{}, azblob.Metadata{}, azblob.BlobAccessConditions{})
// handleErrors(err)

// The high-level API UploadFileToBlockBlob function uploads blocks in parallel for optimal performance, and can handle large files as well.
// This function calls PutBlock/PutBlockList for files larger 256 MBs, and calls PutBlob for any file smaller
fmt.Printf("Uploading the file with blob name: %s\n", fileName)
_, err = azblob.UploadFileToBlockBlob(ctx, file, blobURL, azblob.UploadToBlockBlobOptions{
    BlockSize:   4 * 1024 * 1024,
    Parallelism: 16})
handleErrors(err)
```

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

**ContainerURL** üzerinde **ListBlobs** yöntemini kullanarak kapsayıcıdaki dosyaların listesini alın. ListBlobs, belirtilen **Marker**'dan başlayarak tek bir blob segmenti (en çok 5000 blob) döndürür. Sabit listenin en baştan başlatılması için boş bir Marker kullanın. Blob adları, sözlük sıralamasına göre döndürülür. Segmenti aldıktan sonra işleyin ve ardından daha önce döndürülen Marker'ı geçirerek ListBlobs yöntemini yeniden çağırın.  

```go
// List the blobs in the container
for marker := (azblob.Marker{}); marker.NotDone(); {
    // Get a result segment starting with the blob indicated by the current Marker.
    listBlob, err := containerURL.ListBlobs(ctx, marker, azblob.ListBlobsOptions{})
    handleErrors(err)

    // ListBlobs returns the start of the next segment; you MUST use this to get
    // the next segment (after processing the current result segment).
    marker = listBlob.NextMarker

    // Process the blobs returned in this result segment (if the segment is empty, the loop body won't execute)
    for _, blobInfo := range listBlob.Blobs.Blob {
        fmt.Print("Blob name: " + blobInfo.Name + "\n")
    }
}
```

### <a name="download-the-blob"></a>Blobu indirme

BlobURL'de alt düzey **GetBlob** yöntemini kullanarak blobları indirin. Alternatif olarak, bir Stream oluşturabilir ve [highlevel.go](https://github.com/Azure/azure-storage-blob-go/blob/master/2016-05-31/azblob/highlevel.go)'da sağlanan üst düzey **NewDownloadStream** API'sini kullanarak aralıkları ondan okuyabilirsiniz. NewDownloadStream işlevi bir bağlantı hatası olduğunda yeniden denenirken, Get Blob API'si yalnızca 503 (Sunucu Meşgul) gibi HTTP durum kodlarında yeniden denenir. Aşağıdaki kod, **NewDownloadStream** işlevini kullanarak blobu indirir. Blobun içeriği arabelleğe yazılır ve konsolda gösterilir.

```go
// Here's how to download the blob. NOTE: This method automatically retries if the connection fails
// during download (the low-level GetBlob function does NOT retry errors when reading from its stream).
stream := azblob.NewDownloadStream(ctx, blobURL.GetBlob, azblob.DownloadStreamOptions{})
downloadedData := &bytes.Buffer{}
_, err = downloadedData.ReadFrom(stream)
handleErrors(err)
```

### <a name="clean-up-resources"></a>Kaynakları temizleme
Bu hızlı başlangıçta karşıya yüklenen bloblara artık ihtiyacınız kalmadığında, **Delete** yöntemini kullanarak kapsayıcının tamamını silebilirsiniz. 

```go
// Cleaning up the quick start by deleting the container and the file created locally
fmt.Printf("Press the enter key to delete the sample files, example container, and exit the application.\n")
bufio.NewReader(os.Stdin).ReadBytes('\n')
fmt.Printf("Cleaning up.\n")
containerURL.Delete(ctx, azblob.ContainerAccessConditions{})
file.Close()
os.Remove(fileName)
```

## <a name="next-steps"></a>Sonraki adımlar
 
Bu hızlı başlangıçta, dosyaları Go kullanarak yerel bir disk ile Azure blob depolama arasında aktarmayı öğrendiniz. Azure Depolama Blobu SDK'sı hakkında daha fazla bilgi için, [Kaynak Kodu](https://github.com/Azure/azure-storage-blob-go/) ve [API Başvurusu](https://godoc.org/github.com/Azure/azure-storage-blob-go/2016-05-31/azblob) konularına bakın.
