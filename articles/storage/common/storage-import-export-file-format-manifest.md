---
title: "Azure içeri/dışarı aktarma bildirim dosyasının biçimi | Microsoft Docs"
description: "BLOB'ları Azure Blob Depolama ve alma bir sürücüde dosyaları arasındaki eşlemeyi açıklar sürücü bildirim dosyasının biçimi hakkında bilgi edinin veya içeri/dışarı aktarma hizmeti işinde dışa aktarın."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: f3119e1c-2c25-48ad-8752-a6ed4adadbb0
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: c1857eb94fba13c30e7f07669616f5d0ab9953f4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="azure-importexport-service-manifest-file-format"></a>Azure içeri/dışarı aktarma hizmeti bildirim dosyası biçimi
Sürücü bildirim dosyası Azure Blob storage blobları ve içe veya dışa aktarma işi kapsayan sürücüsündeki dosyaları arasındaki eşlemeyi açıklar. Bir alma işlemi için bildirim dosyası sürücü hazırlama işleminin bir parçası olarak oluşturulur ve sürücü Azure veri merkezine gönderilmeden önce sürücüde depolanır. Bir dışa aktarma işlemi sırasında bildirim oluşturulur ve Azure içeri/dışarı aktarma hizmeti tarafından sürücüde depolanan.  
  
Her ikisi de almak ve işleri dışarı aktarmak için sürücü bildirim dosyasını içeri veya dışarı sürücüsünde depolanır; herhangi bir API işlemi aracılığıyla hizmete aktarılan değil.  
  
Aşağıdaki sürücü bildirim dosyası genel biçimi açıklar:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveManifest Version="2014-11-01">  
  <Drive>  
    <DriveId>drive-id</DriveId>  
    import-export-credential  
  
    <!-- First Blob List -->  
    <BlobList>  
      <!-- Global properties and metadata that applies to all blobs -->  
      [<MetadataPath Hash="md5-hash">global-metadata-file-path</MetadataPath>]  
      [<PropertiesPath   
        Hash="md5-hash">global-properties-file-path</PropertiesPath>]  
  
      <!-- First Blob -->  
      <Blob>  
        <BlobPath>blob-path-relative-to-account</BlobPath>  
        <FilePath>file-path-relative-to-transfer-disk</FilePath>  
        [<ClientData>client-data</ClientData>]  
        [<Snapshot>snapshot</Snapshot>]  
        <Length>content-length</Length>  
        [<ImportDisposition>import-disposition</ImportDisposition>]  
        page-range-list-or-block-list          
        [<MetadataPath Hash="md5-hash">metadata-file-path</MetadataPath>]  
        [<PropertiesPath Hash="md5-hash">properties-file-path</PropertiesPath>]  
      </Blob>  
  
      <!-- Second Blob -->  
      <Blob>  
      . . .  
      </Blob>  
    </BlobList>  
  
    <!-- Second Blob List -->  
    <BlobList>  
    . . .  
    </BlobList>  
  </Drive>  
</DriveManifest>  
  
import-export-credential ::=   
  <StorageAccountKey>storage-account-key</StorageAccountKey> | <ContainerSas>container-sas</ContainerSas>  
  
page-range-list-or-block-list ::=   
  page-range-list | block-list  
  
page-range-list ::=   
    <PageRangeList>  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       Hash="md5-hash"/>]  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       Hash="md5-hash"/>]  
    </PageRangeList>  
  
block-list ::=  
    <BlockList>  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]  
       Hash="md5-hash"/>]  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]   
       Hash="md5-hash"/>]  
    </BlockList>  

```

## <a name="manifest-xml-elements-and-attributes"></a>Bildirim XML öğeleri ve öznitelikleri

Aşağıdaki tabloda, veri öğeleri ve sürücü bildirim XML biçimi özniteliklerini belirtilir.  
  
|XML öğesi|Tür|Açıklama|  
|-----------------|----------|-----------------|  
|`DriveManifest`|Kök öğesi|Bildirim dosyasının kök öğesinin. Diğer tüm öğeleri dosyasındaki altında bu öğesidir.|  
|`Version`|Öznitelik, dize|Bildirim dosyasının sürümü.|  
|`Drive`|İç içe geçmiş XML öğesi|Her sürücü için bildirimi içerir.|  
|`DriveId`|Dize|Sürücü benzersiz sürücü tanımlayıcısı. Sürücü tanımlayıcı sürücü seri numarası için sorgulanarak bulunur. Sürücü seri numarası genellikle sürücüsünün de dışarıda yazdırılır. `DriveID` Öğesi önce görünmelidir `BlobList` bildirim dosyasındaki öğesi.|  
|`StorageAccountKey`|Dize|İçeri aktarma bulunuyorsa ve yalnızca iş için gereken `ContainerSas` belirtilmedi. Azure depolama hesabı için hesap anahtarı işle ilişkili.<br /><br /> Bu öğe bir verme işlemi için bildirimindeki atlanır.|  
|`ContainerSas`|Dize|İçeri aktarma bulunuyorsa ve yalnızca iş için gereken `StorageAccountKey` belirtilmedi. İşlemle ilişkili BLOB'ları erişmek için kapsayıcı SAS. Bkz: [Put işlemini](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) , biçimi için. Bu öğe bir verme işlemi için bildirimindeki atlanır.|  
|`ClientCreator`|Dize|XML dosyasını oluşturulan istemciyi belirtir. Bu değer içeri/dışarı aktarma hizmeti tarafından yorumlanmaz.|  
|`BlobList`|İç içe geçmiş XML öğesi|İçeri aktarma parçası olan veya iş verme BLOB listesini içerir. Bir blob listedeki her bir blob aynı meta verileri ve özellikleri paylaşır.|  
|`BlobList/MetadataPath`|Dize|İsteğe bağlı. BLOB alma işlemi için blob listesinde üzerinde ayarlanmış varsayılan meta veriler içeren diskteki bir dosyanın göreli yolunu belirtir. Bu meta veriler isteğe bağlı olarak bir blob tarafından blob temelinde geçersiz kılınabilir.<br /><br /> Bu öğe bir verme işlemi için bildirimindeki atlanır.|  
|`BlobList/MetadataPath/@Hash`|Öznitelik, dize|Meta veri dosyası için Base16 kodlu MD5 karma değeri belirtir.|  
|`BlobList/PropertiesPath`|Dize|İsteğe bağlı. BLOB alma işlemi için blob listesinde üzerinde ayarlanmış varsayılan özellikleri içeren diskteki bir dosyanın göreli yolunu belirtir. Bu özellikleri isteğe bağlı olarak bir blob tarafından blob temelinde geçersiz kılınabilir.<br /><br /> Bu öğe bir verme işlemi için bildirimindeki atlanır.|  
|`BlobList/PropertiesPath/@Hash`|Öznitelik, dize|Özellikler dosya için Base16 kodlu MD5 karma değeri belirtir.|  
|`Blob`|İç içe geçmiş XML öğesi|Her bir blob listedeki her bir blob hakkında bilgiler içerir.|  
|`Blob/BlobPath`|Dize|Kapsayıcı adı ile başlayan göreli URI blob. Kök kapsayıcıda blob ise, ile başlamalıdır `$root`.|  
|`Blob/FilePath`|Dize|Sürücüsündeki dosyanın göreli yolunu belirtir. Dışarı aktarma işleri için blob yolu Mümkünse dosya yolu için kullanılır; *örneğin*, `pictures/bob/wild/desert.jpg` aktarılacak `\pictures\bob\wild\desert.jpg`. Ancak, NTFS adları sınırlamaları nedeniyle, bir blob blob yolu benzer olmayan bir yola sahip bir dosyaya dışarı.|  
|`Blob/ClientData`|Dize|İsteğe bağlı. Müşteriden açıklamaları içerir. Bu değer içeri/dışarı aktarma hizmeti tarafından yorumlanmaz.|  
|`Blob/Snapshot`|Tarih saat|Dışarı aktarma işleri için isteğe bağlıdır. Dışarı aktarılan blob anlık görüntü için anlık görüntü tanımlayıcısını belirtir.|  
|`Blob/Length`|Tamsayı|Blob toplam uzunluğu bayt cinsinden belirtir. Değer bir blok blobu en fazla 200 GB olabilir ve bir sayfa blob'u için 1 TB'a kadar. Bir sayfa blob'u için bu değer 512 birden fazla olmalıdır.|  
|`Blob/ImportDisposition`|Dize|Dışarı aktarma işleri için atlanmış içeri aktarma işi için isteğe bağlıdır. Bu, burada aynı ada sahip bir blob zaten içeri/dışarı aktarma hizmeti bir alma işi söz konusu nasıl yöneteceğini belirtir. Bu değer alma bildirimden atlanırsa, varsayılan değer olan `rename`.<br /><br /> Bu öğe için değerleri şunlardır:<br /><br /> -   `no-overwrite`: Aynı ada sahip bir hedef blob zaten varsa, içeri aktarma işlemi bu dosyayı içeri aktarırken atlar.<br />-   `overwrite`: Tüm var olan hedef blob tamamen yeni içeri aktarılan dosya tarafından üzerine yazılır.<br />-   `rename`: Yeni blob değiştirilmiş bir adla karşıya.<br /><br /> Yeniden adlandırma kuralı aşağıdaki gibidir:<br /><br /> -Blob adı noktayla içermiyorsa, yeni bir ad eklenerek oluşturulur `(2)` bu yeni ad Ayrıca varolan bir blob adla sonra çakışırsa özgün blob adına; `(3)` yerine eklenen `(2)`; ve benzeri.<br />Blob adı noktayla içeriyorsa, son nokta aşağıdaki bölümü uzantı adı olarak kabul edilir. Yukarıdaki yordama benzer `(2)` adı sonra hizmet varolan yeni ad hala çakışmaları blob; yeni bir ad oluşturmak üzere son nokta denemeden önce eklenen `(3)`, `(4)`ve benzeri çakışmayan bir adı bulunana kadar.<br /><br /> Bazı örnekler:<br /><br /> Blob `BlobNameWithoutDot` adlandırılacak:<br /><br /> `BlobNameWithoutDot (2)  // if BlobNameWithoutDot exists`<br /><br /> `BlobNameWithoutDot (3)  // if both BlobNameWithoutDot and BlobNameWithoutDot (2) exist`<br /><br /> Blob `Seattle.jpg` adlandırılacak:<br /><br /> `Seattle (2).jpg  // if Seattle.jpg exists`<br /><br /> `Seattle (3).jpg  // if both Seattle.jpg and Seattle (2).jpg exist`|  
|`PageRangeList`|İç içe geçmiş XML öğesi|Bir sayfa blob'u için gereklidir.<br /><br /> İçin alma işlemi, bir bayt aralıkları listesi, içeri aktarılacak bir dosyanın belirtir. Her sayfası aralık bir uzaklık ve uzunluk kaynak dosyasında bir MD5 karma değeri bölgenin birlikte sayfa aralığı açıklar tarafından tanımlanır. `Hash` Bir sayfa aralığının özniteliği gereklidir. Hizmet blob verileri karmasını hesaplanan MD5 karma değeri sayfa aralığından eşleştiğini doğrular. Herhangi bir sayıda sayfa aralıklarını alma işlemi için bir dosya tanımlamak, 1 TB'ye kadar toplam boyutu için kullanılabilir. Tüm sayfa aralıklarını tarafından uzaklığı sıralanmalıdır ve hiçbir üst üste geliyor verilir.<br /><br /> İşlemi, bir dışarı aktarma için bir dizi sürücüye dışarı bayt aralığı bir BLOB belirtir.<br /><br /> Sayfa aralıklarını birlikte bir blob veya dosya yalnızca alt aralığını kapsayan.  Herhangi bir sayfa aralığı tarafından kapsanmayan dosyanın kalan kısmını beklenen ve içeriği tanımlanmamış olabilir.|  
|`PageRange`|XML öğesi|Sayfa aralığını temsil eder.|  
|`PageRange/@Offset`|Öznitelik, tamsayı|Belirtilen sayfa aralığı başladığı blob ve aktarım dosyası de uzaklığını belirtir. Bu değer 512'ın katları olmalıdır.|  
|`PageRange/@Length`|Öznitelik, tamsayı|Sayfa aralığı uzunluğunu belirtir. Bu değer birden fazla 512 ve en fazla 4 MB olmalıdır.|  
|`PageRange/@Hash`|Öznitelik, dize|Sayfa aralığı Base16 kodlu MD5 karma değeri belirtir.|  
|`BlockList`|İç içe geçmiş XML öğesi|Adlandırılmış bloklara sahip bir blok blobu için gereklidir.<br /><br /> Bir alma işlemi için bir Azure depolama alanına içeri aktarılacak blokları kümesi engelleme listesi belirtir. Bir dışarı aktarma işlemi için burada her bloğu verme diskteki dosyasındaki zamandır saklanan engelleme listesi belirtir. Her blok uzaklığı dosya ve blok uzunluğu tarafından açıklanan; Her blok blok Kimliği özniteliği tarafından ayrıca adlandırılır ve blok için MD5 karma değeri içerir. En fazla 50.000 blok blob tanımlamak için kullanılabilir.  Tüm bloklar gerekir sıralı uzaklık ve birlikte dosyanın tam aralığını kapsayan *yani*, blokları boşluksuz olmalıdır. Blob en fazla 64 MB ise, her blok blok kimliği tüm yok ya da tüm mevcut olması gerekir. Blok kimlikleri Base64 ile kodlanmış dize olması gerekir. Bkz: [yerleştirme blok](/rest/api/storageservices/put-block) daha fazla blok kimliği için gereksinimleri için.|  
|`Block`|XML öğesi|Bloğunu temsil eder.|  
|`Block/@Offset`|Öznitelik, tamsayı|Belirtilen blok başladığı uzaklığını belirtir.|  
|`Block/@Length`|Öznitelik, tamsayı|Bayt sayısını bloğunda belirtir; Bu değer, en fazla 4 MB olmalıdır.|  
|`Block/@Id`|Öznitelik, dize|Bloğun blok Kimliğini temsil eden bir dize belirtir.|  
|`Block/@Hash`|Öznitelik, dize|Bloğun Base16 kodlu MD5 karma değeri belirtir.|  
|`Blob/MetadataPath`|Dize|İsteğe bağlı. Meta veri dosyasının göreli yolunu belirtir. Alma işlemi sırasında meta veriler üzerinde hedef blob ayarlanır. Dışa aktarma işlemi sırasında blob'un meta veri sürücüsünde meta veri dosyasında depolanır.|  
|`Blob/MetadataPath/@Hash`|Öznitelik, dize|Blob'un meta veri dosyasının Base16 kodlu MD5 karma değeri belirtir.|  
|`Blob/PropertiesPath`|Dize|İsteğe bağlı. Özellikler dosyanın göreli yolunu belirtir. Alma işlemi sırasında hedef blob üzerindeki özellikleri ayarlanır. Dışa aktarma işlemi sırasında blob özellikleri sürücüde özellikleri dosyasında depolanır.|  
|`Blob/PropertiesPath/@Hash`|Öznitelik, dize|Blob'un özellikleri dosyasının Base16 kodlu MD5 karma değeri belirtir.|  
  
## <a name="next-steps"></a>Sonraki adımlar
 
* [Depolama içeri/dışarı aktarma REST API'si](/rest/api/storageimportexport/)
