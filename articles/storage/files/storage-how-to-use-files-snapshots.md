---
title: "Paylaşım anlık görüntüler (Önizleme) ile çalışma | Microsoft Docs"
description: "Paylaşım anlık bir noktada paylaşımını yedekleme için bir yöntem olarak, zaman içinde alınmış bir Azure dosya paylaşımının salt okunur bir sürümüdür."
services: storage
documentationcenter: .net
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: renash
ms.openlocfilehash: c4a5f7d28601867c383b8b348568e4bb580a81eb
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="work-with-share-snapshots-preview"></a>Paylaşım anlık görüntüler (Önizleme) ile çalışma
Bir paylaşım anlık görüntü (Önizleme), bir noktada geçen süre içinde bir Azure dosya paylaşımının salt okunur bir sürümüdür. Paylaşım anlık görüntü oluşturulduktan sonra okumak, kopyalanan, veya silinmiş, ancak değişiklik. Paylaşım anlık bir anda göründüğü gibi paylaşım yedeklemek için bir yol sağlar. 

Bu makalede, oluşturmak, yönetmek ve Paylaşım anlık görüntüleri silmek öğreneceksiniz. Daha fazla bilgi için bkz: [anlık görüntü genel bakış paylaşmak](storage-snapshots-files.md) veya [SSS anlık görüntü](storage-files-faq.md).

## <a name="create-a-share-snapshot"></a>Paylaşım anlık görüntü oluşturma

Azure portalı, PowerShell, Azure CLI, REST API veya bir Azure depolama SDK'sını kullanarak bir paylaşım anlık görüntü oluşturabilirsiniz. Aşağıdaki bölümlerde portal, CLI ve PowerShell kullanarak bir paylaşım anlık görüntü oluşturma açıklanmaktadır. 

Kullanımda olduğu sırada bir dosya paylaşımı paylaşımı görüntüsünü alabilir. Ancak, paylaşım anlık görüntü komutu verilen aynı anda yalnızca bir dosyaya yazılır veriler paylaşmak anlık görüntüleri yakalama paylaşır. Bu, herhangi bir uygulama veya işletim sistemi tarafından önbelleğe herhangi bir veri hariç.

### <a name="create-a-share-snapshot-by-using-the-portal"></a>Portalı kullanarak paylaşım anlık görüntü oluşturma  
Zaman içinde nokta paylaşımı anlık görüntü oluşturmak için dosyanıza paylaşma Portalı'nda seçin gidin ve **anlık görüntü**.

>   ![Portal anlık görüntü menüsü](./media/storage-snapshots-create/portal-create-snapshot.png)


### <a name="create-a-share-snapshot-by-using-azure-cli-20"></a>Azure CLI 2.0 kullanarak paylaşım anlık görüntü oluşturma
Kullanarak bir paylaşım anlık görüntü oluşturabilirsiniz `az storage share snapshot` komutu:

```azurecli-interactive
az storage share snapshot -n <share name>
```

Örnek çıktı:
```json
{
  "metadata": {},
  "name": "<share name>",
  "properties": {
    "etag": "\"0x8D50B7F9A8D7F30\"",
    "lastModified": "2017-10-04T23:28:22+00:00",
    "quota": null
  },
  "snapshot": "2017-10-04T23:28:35.0000000Z"
}
```

### <a name="create-a-share-snapshot-by-using-powershell"></a>PowerShell kullanarak bir paylaşım anlık görüntü oluşturma
Kullanarak bir paylaşım anlık görüntü oluşturabilirsiniz `$share.Snapshot()` komutu:

```powershell
$connectionstring="DefaultEndpointsProtocol=http;FileEndpoint=http:<Storage Account Name>.file.core.windows.net /;AccountName=:<Storage Account Name>;AccountKey=:<Storage Account Key>"
$sharename=":<file share name>"

$ctx = New-AzureStorageContext -ConnectionString $connectionstring

##create snapshot
$share=Get-AzureStorageShare -Context $ctx -Name <file share name>
$share.Properties.LastModified
$share.IsSnapshot
$snapshot=$share.Snapshot()

```

## <a name="perform-common-share-snapshot-operations"></a>Ortak paylaşımı anlık görüntü işlemleri

Kullanarak, dosya paylaşımı ile ilişkili paylaşımı anlık görüntüleri listeleme **önceki sürümler** Windows, REST, istemci kitaplığı, PowerShell ve portal aracılığıyla sekmesindedir. Dosya Paylaşımı takılı sonra kullanarak önceki tüm sürümlerini dosyasını görüntüleyebilirsiniz **önceki sürümler** Windows sekmesindedir. 

Aşağıdaki bölümlerde, Azure portalı, Windows ve Azure CLI 2.0 listelemek için Gözat için nasıl kullanılacağı açıklanmıştır ve Paylaşım anlık görüntü geri yükleme.

### <a name="share-snapshot-operations-in-the-portal"></a>Anlık görüntü işlemlerine portalında paylaşma

Tüm, paylaşım anlık görüntüler bir dosya için portalda paylaşma ve içeriğini görüntülemek için bir paylaşım anlık Gözat bakabilirsiniz.

#### <a name="view-a-share-snapshot"></a>Paylaşım anlık görüntü görüntüleme
Dosya paylaşımında altında **anlık görüntü**seçin **görüntülemek anlık görüntüleri**.

![Portalı'nda "anlık görüntüleri görüntüle" komutu](./media/storage-snapshots-list-browse/snapshot-view-portal.png)

#### <a name="list-and-browse-to-share-snapshot-content"></a>Liste ve anlık görüntü içeriği paylaşmak için Gözat
Paylaşım anlık görüntüleri listesini görüntüleyin ve ardından istediğiniz zaman damgasını doğrudan seçerek bir anlık görüntü içeriğini göz atın.

![Zaman damgaları listesinde seçilen anlık görüntü](./media/storage-snapshots-list-browse/snapshot-browsefiles-portal.png)

Öğesini de seçebilirsiniz **Bağlan** listenize düğmesinde anlık görüntüsünü almak için görünümü `net use` komut ve belirli paylaşımına anlık görüntü dizin yolu. Bu anlık görüntüyü doğrudan göz atabilirsiniz.


![Komut kutusuyla bölmesinde Bağlan](./media/storage-snapshots-list-browse/snapshot-connect-portal.png)

#### <a name="download-or-restore-from-a-share-snapshot"></a>İndirme veya bir paylaşım anlık görüntüden geri yükleme
Portalı'ndan indirin veya kullanarak bir dosyayı bir anlık görüntüden geri **karşıdan** veya **geri** düğmesi, sırasıyla.

![Karşıdan yükleme ve geri yükleme düğmeleri](./media/storage-snapshots-list-browse/snapshot-download-restore-portal.png)

### <a name="share-snapshot-operations-in-windows"></a>Anlık görüntü işlemlerine Windows paylaşma
Dosya paylaşımının paylaşım anlık görüntü zaten gerçekleştirmişsiniz olduğunda, bağlı dosya paylaşımından Windows bir paylaşımı, bir dizin veya belirli bir dosya önceki sürümlerini görüntüleyebilirsiniz. Örnek olarak, işte görüntülemek ve Windows dizininde önceki sürümünü geri yüklemek için önceki sürümler özelliğinden nasıl kullanabilirsiniz.

> [!Note]  
> Paylaşım düzeyi ve dosya düzeyinde aynı işlemleri gerçekleştirebilir. Bu dizin veya dosya değişiklikleri içeren sürüm listesinde gösterilir. Belirli bir dizin veya dosya arasında iki paylaşımı anlık görüntüleri değişmemişse paylaşımı anlık görüntü paylaşım düzeyi önceki sürüm listesi ancak dizinin veya dosyanın önceki sürüm listesi görüntülenir.

#### <a name="mount-a-file-share"></a>Bir dosya paylaşımını bağlama
İlk olarak, kullanarak dosya paylaşımını bağlama `net use` komutu.

#### <a name="open-a-mounted-share-in-file-explorer"></a>Dosya Gezgini'nde bağlanılan paylaşımı açın
Dosya Gezgini'ne gidin ve bağlanılan paylaşımı bulun.

![Dosya Gezgini'nde bağlanılan paylaşımı](./media/storage-snapshots-list-browse/snapshot-windows-mount.png)

#### <a name="list-previous-versions"></a>Liste önceki sürümleri
Öğe veya geri yüklenmesi gereken üst öğeye göz atın. İstenen dizine gitmek için çift tıklayın. Sağ tıklatıp **özellikleri** menüsünde.

![Seçili dizin menüsüne sağ tıklayın](./media/storage-snapshots-list-browse/snapshot-windows-previous-versions.png)

Seçin **önceki sürümler** bu dizin için paylaşım anlık görüntü listesini görmek için. Liste, ağ hızını ve dizindeki paylaşımı anlık görüntü sayısını bağlı olarak yüklemek için birkaç saniye sürebilir.

![Önceki sürümler sekmesi](./media/storage-snapshots-list-browse/snapshot-windows-list.png)

Seçebileceğiniz **açmak** belirli bir anlık görüntü açın. 

![Anlık görüntü açılmış](./media/storage-snapshots-list-browse/snapshot-browse-windows.png)

#### <a name="restore-from-a-previous-version"></a>Önceki bir sürümden geri yükleme
Seçin **geri** tüm dizin yinelemeli olarak içeriğini paylaşımı anlık görüntü oluşturma zamanında özgün konumuna kopyalamak için.
 ![Geri Yükle düğmesi uyarı iletisi](./media/storage-snapshots-list-browse/snapshot-windows-restore.png)

### <a name="share-snapshot-operations-in-azure-cli-20"></a>Azure CLI 2.0 anlık görüntü işlemlerinde paylaşma
Anlık görüntü içeriği paylaşmak için gözatma, geri yükleme listeleme paylaşımı anlık görüntüleri gibi işlemleri gerçekleştirmek için Azure CLI 2.0 kullanabilirsiniz veya anlık görüntü paylaşımı anlık görüntüler veya silmesini karşıdan dosya paylaşımı.

#### <a name="list-share-snapshots"></a>Liste paylaşımı anlık görüntüler

Belirli bir paylaşımına anlık görüntülerini kullanarak listeleyebilirsiniz [ `az storage share list` ](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list) ile `--include-snapshots`:

```azurecli-interactive 
az storage share list --include-snapshots
```

Komut tüm ilişkili özellikleri birlikte paylaşımı anlık görüntüler bir listesi verilmektedir. Aşağıdaki çıkış örneğidir:

```json
[
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:44:13.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:45:18.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": null
  }
]
```

#### <a name="browse-to-a-share-snapshot"></a>Paylaşım anlık görüntüye Gözat
Belirli paylaşımına anlık görüntüye göz atın ve içeriğini kullanarak görüntüleme [ `az storage file list` ](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list). Paylaşım adı ve için göz atın aşağıdaki örnekte gösterildiği gibi istediğiniz zaman damgası belirtin:

```azurecli-interactive 
az storage file list --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z' -otable
```

Çıktıda paylaşımı anlık görüntü içeriğini içeriği aynı olup olmadığını, bir noktada paylaşımının bu paylaşım anlık görüntü oluşturuldu:

```
Name            Content Length    Type    Last Modified
--------------  ----------------  ------  ---------------
HelloWorldDir/                    dir
IMG_0966.JPG    533568            file
IMG_1105.JPG    717711            file
IMG_1341.JPG    608459            file
IMG_1405.JPG    652156            file
IMG_1611.JPG    442671            file
IMG_1634.JPG    1495999           file
IMG_1635.JPG    974058            file

```
#### <a name="restore-from-a-share-snapshot"></a>Bir paylaşım anlık görüntüden geri yükleme

Bir dosya kopyalama veya paylaşım anlık görüntüden yükleme geri yükleyebilirsiniz. Kullanım `az storage file download` aşağıdaki örnekte gösterildiği gibi komut:

```azurecli-interactive 
az storage file download --path IMG_0966.JPG --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z'
```

Çıktıda indirilen dosya ve özelliklerini içeriğini içeriği aynıdır ve anlık görüntü paylaşan özellikleri, bir noktada oluşturulduğu bakın:

```json
{
  "content": null,
  "metadata": {},
  "name": "IMG_0966.JPG",
  "properties": {
    "contentLength": 533568,
    "contentRange": "bytes 0-533567/533568",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentType": "application/octet-stream"
    },
    "copy": {
      "completionTime": null,
      "id": null,
      "progress": null,
      "source": null,
      "status": null,
      "statusDescription": null
    },
    "etag": "\"0x8D50B5F49F7ACDF\"",
    "lastModified": "2017-10-04T19:37:03+00:00",
    "serverEncrypted": true
  }
}
```

<<<<<<< BAŞ
### <a name="file-share-snapshot-operations-in-azure-powershell"></a>Azure PowerShell'de dosya paylaşımı anlık görüntü işlemleri
Anlık görüntü içeriğini paylaşın, geri yükleme veya dosya paylaşımı anlık görüntüden yükleme veya paylaşım anlık görüntülerin silinmesi gözatma listesi paylaşımı anlık görüntüler, örneğin aynı işlemleri gerçekleştirmek için Azure PowerShell'i kullanabilirsiniz.

#### <a name="list-share-snapshots"></a>Liste paylaşımı anlık görüntüler

Belirli paylaşımı kullanarak bir paylaşım anlık görüntüleri listeleyebilir`Get-AzureStorageShare`

```powershell
Get-AzureStorageShare -Name "ContosoShare06" -SnapshotTime "6/16/2017 9:48:41 AM +00:00"
```

#### <a name="browse-share-snapshots"></a>Paylaşım anlık görüntüleri Gözat
İçerik kullanarak görüntülemek için anlık görüntü belirli bir paylaşım içine göz atabilirsiniz `Get-AzureStorageFile` değeriyle `-Share` belirli bir anlık görüntüye işaret eden

```powershell
$snapshot = Get-AzureStorageShare -Name "ContosoShare06" -SnapshotTime "6/16/2017 9:48:41 AM +00:00"
Get-AzureStorageFile -Share $snapshot
```

#### <a name="restore-from-share-snapshots"></a>Paylaşım anlık görüntülerden geri yükleme

Kopyalayarak veya kullanarak anlık görüntü paylaşımından dosya indirme bir dosyayı geri `Get-AzureStorageFileContent` komutu

```powershell
$download='C:\Temp\Download'
Get-AzureStorageFileContent -Share $snapshot -Path $file -Destination $download
```

```powershell
$snapshot = Get-AzureStorageShare -Name "ContosoShare06" -SnapshotTime "6/16/2017 9:48:41 AM +00:00"
$directory = Get-AzureStorageFile -ShareName "ContosoShare06" -Path "ContosoWorkingFolder" | Get-AzureStorageFile
Get-AzureStorageFileContent -Share $snapshot -Path $file -Destination $directory
```


## <a name="delete-azure-files-share-snapshot"></a>Azure dosya paylaşımı anlık görüntüyü Sil
=======
## <a name="delete-a-share-snapshot"></a>Paylaşım anlık görüntüyü Sil
>>>>>>> 6a1833e10031fbf1ab204bb1f30cb54cf5fbcada

Azure portalı, PowerShell, CLI, REST API veya depolama SDK kullanarak paylaşım anlık görüntüleri silebilirsiniz. Aşağıdaki bölümlerde Azure portal, CLI ve PowerShell kullanarak paylaşım anlık görüntüleri silmek nasıl açıklanmaktadır.

Anlık görüntüleri paylaşır ve herhangi bir karşılaştırma aracını kullanarak iki anlık görüntüleri arasındaki farklar görüntülemek için göz atabilirsiniz. Ardından, silmek istediğiniz paylaşımı anlık görüntüyü belirleyebilirsiniz. 

Bir paylaşıma sahip bir paylaşımı silemezsiniz anlık görüntü. Paylaşımı silin yapabilmek için önce tüm paylaşım anlık silmeniz gerekir.

### <a name="delete-a-share-snapshot-by-using-the-portal"></a>Portalı kullanarak bir paylaşım anlık görüntüyü Sil  
Portalda, dosya paylaşımı dikey penceresine gidin ve kullanmak **silmek** düğmesine bir veya daha fazla paylaşımı anlık görüntüleri silin.

>   ![Düğme silme](./media/storage-snapshots-delete/portal-snapshots-delete.png)


### <a name="delete-a-share-snapshot-by-using-azure-cli-20"></a>Azure CLI 2.0 kullanarak paylaşım anlık görüntü silme
Paylaşım anlık görüntü kullanarak silebilirsiniz `[az storage share delete]` komutu. İçin paylaşımı anlık görüntü zaman damgasını kullanmak `--snapshot '2017-10-04T23:28:35.0000000Z' ` aşağıdaki örnekte parametre:

```azurecli-interactive
az storage share delete -n <share name> --snapshot '2017-10-04T23:28:35.0000000Z' 
```

Örnek çıktı:
```json
{
  "deleted": true
}
```

### <a name="delete-a-share-snapshot-by-using-powershell"></a>PowerShell kullanarak bir paylaşım anlık görüntüyü Sil
Paylaşım anlık görüntü kullanarak silebilirsiniz `Remove-AzureStorageShare -Share` komutu:

```powershell
$connectionstring="DefaultEndpointsProtocol=http;FileEndpoint=http:<Storage Account Name>.file.core.windows.net /;AccountName=:<Storage Account Name>;AccountKey=:<Storage Account Key>"
$sharename=":<file share name>"

$ctx = New-AzureStorageContext -ConnectionString $connectionstring

##Create snapshot
$share=Get-AzureStorageShare -Context $ctx -Name <file share name>
$share.Properties.LastModified
$share.IsSnapshot
$snapshot=$share.Snapshot()

##Delete snapshot
Remove-AzureStorageShare -Share $snapshot

```

## <a name="next-steps"></a>Sonraki adımlar
* [Anlık görüntü genel bakış](storage-snapshots-files.md)
* [Anlık görüntü ile ilgili SSS](storage-files-faq.md)
