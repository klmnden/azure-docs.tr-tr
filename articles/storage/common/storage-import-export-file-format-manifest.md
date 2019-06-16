---
title: Azure içeri/dışarı aktarma bildirimi dosyası biçimi | Microsoft Docs
description: Azure Blob Depolama'daki blobları ve dosyaları bir içeri aktarma sürücüde arasındaki eşlemeyi tanımlar sürücü bildirim dosyasının biçimi hakkında bilgi edinin veya içeri/dışarı aktarma hizmeti işinde aktarın.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: ee53cc3a639a79e1b29ac6cd537bfb04e05b1bca
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61478632"
---
# <a name="azure-importexport-service-manifest-file-format"></a>Azure içeri/dışarı aktarma hizmet bildirimi dosyası biçimi
Sürücü bildirim dosyası, Azure Blob Depolama'daki blobları ve dosyaları bir içeri veya dışarı aktarma işi kapsayan sürücüde arasındaki eşlemeyi açıklar. Bildirim dosyası, bir içeri aktarma işlemi için sürücü hazırlama işleminin bir parçası olarak oluşturulur ve Azure veri merkezine bir sürücü gönderilmeden önce bir sürücüde depolanır. Bir dışarı aktarma işlemi sırasında bildirim oluşturulur ve bu sürücüde Azure içeri/dışarı aktarma hizmeti tarafından depolanan.  
  
Her ikisi de içeri ve dışarı aktarma işleri için sürücü bildirim dosyasını içeri veya dışarı aktarmayı sürücüsünde depolanır; Her API işlemi hizmete gönderilen değil.  
  
Aşağıdaki sürücü bildirim dosyasının genel biçimini tanımlar:  
  
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

Aşağıdaki tabloda, veri öğeleri ve öznitelikleri sürücü bildirim XML biçiminde belirtilir.  
  
|XML öğesi|Tür|Açıklama|  
|-----------------|----------|-----------------|  
|`DriveManifest`|Kök öğe|Bildirim dosyasının kök öğe. Bu öğenin dosyasındaki tüm diğer öğeleri şunlardır:|  
|`Version`|Öznitelik, dize|Bildirim dosyası sürümü.|  
|`Drive`|İç içe geçmiş XML öğesi|Her sürücü için bildirimi içerir.|  
|`DriveId`|String|Sürücünün sürücü benzersiz tanımlayıcısı. Sürücü tanımlayıcısını sürücüsü için seri numarasını sorgulanarak bulunur. Sürücü seri numarası genellikle sürücünün de dış yazdırılır. `DriveID` Öğesi önce görünmelidir `BlobList` bildirimi dosyasındaki öğesi.|  
|`StorageAccountKey`|String|İçeri aktarma ve yalnızca iş için gereken `ContainerSas` belirtilmedi. Azure depolama hesabı için hesap anahtarı işle ilişkili.<br /><br /> Bu öğe bildirimden dışarı aktarma işlemi için atlandı.|  
|`ContainerSas`|String|İçeri aktarma ve yalnızca iş için gereken `StorageAccountKey` belirtilmedi. İşle ilişkili bloblara erişmek için kapsayıcı SAS. Bkz: [Put işlemini](/rest/api/storageimportexport/jobs) biçimi için. Bu öğe bildirimden dışarı aktarma işlemi için atlandı.|  
|`ClientCreator`|String|XML dosyasını oluşturulan istemci belirtir. Bu değer içeri/dışarı aktarma hizmeti tarafından yorumlanmaz.|  
|`BlobList`|İç içe geçmiş XML öğesi|İçeri aktarma parçası veya dışarı aktarma işi BLOB listesini içerir. Her blob bir blob listesindeki aynı meta veriler ve Özellikler paylaşır.|  
|`BlobList/MetadataPath`|String|İsteğe bağlı. BLOB'ları içeri aktarma işlemi için blob listesinde ayarlanır varsayılan meta veriler içeren diskteki dosyasının göreli yolunu belirtir. Bu meta veriler isteğe bağlı olarak bir blob tarafından blob temelinde geçersiz kılınabilir.<br /><br /> Bu öğe bildirimden dışarı aktarma işlemi için atlandı.|  
|`BlobList/MetadataPath/@Hash`|Öznitelik, dize|Meta veri dosyası için MD5 Base16 kodlanmış karma değer belirtir.|  
|`BlobList/PropertiesPath`|String|İsteğe bağlı. BLOB'ları içeri aktarma işlemi için blob listesinde ayarlanır varsayılan özellikleri içeren diskteki dosyasının göreli yolunu belirtir. Bu özellikler isteğe bağlı olarak bir blob tarafından blob temelinde geçersiz kılınabilir.<br /><br /> Bu öğe bildirimden dışarı aktarma işlemi için atlandı.|  
|`BlobList/PropertiesPath/@Hash`|Öznitelik, dize|Özellikler dosyası Base16 kodlu MD5 karma değeri belirtir.|  
|`Blob`|İç içe geçmiş XML öğesi|Her bir blob listedeki her blob hakkında bilgi içerir.|  
|`Blob/BlobPath`|String|Kapsayıcı adı ile başlayan göreli URI blob. Kök kapsayıcı içinde blob ise ile başlamalıdır `$root`.|  
|`Blob/FilePath`|String|Sürücüdeki dosyaya göreli yolunu belirtir. Dışarı aktarma işleri için blob yolu Mümkünse dosya yolu için kullanılır. *örn*, `pictures/bob/wild/desert.jpg` aktarılacağı `\pictures\bob\wild\desert.jpg`. Ancak, blob yolu benzer olmayan bir yola sahip bir dosya için bir blob NTSF adları sınırlamaları nedeniyle, verilebilir.|  
|`Blob/ClientData`|String|İsteğe bağlı. Müşteri açıklamalarını içerir. Bu değer içeri/dışarı aktarma hizmeti tarafından yorumlanmaz.|  
|`Blob/Snapshot`|DateTime|Dışarı aktarma işleri için isteğe bağlı. Bir dışarı aktarılan blob anlık görüntüsü için anlık görüntü tanımlayıcısını belirtir.|  
|`Blob/Length`|Integer|Blob toplam uzunluğu, bayt cinsinden belirtir. Değer bir blok blobu için en fazla 200 GB olabilir ve bir sayfa blobu 1 TB'ye kadar büyütün. Bir sayfa blobu için bu değer 512 katı olmalıdır.|  
|`Blob/ImportDisposition`|String|İçeri aktarma işlerinde, dışarı aktarma işleri için atlanmış için isteğe bağlı. Bu, aynı ada sahip bir blob zaten mevcut olduğu içeri/dışarı aktarma hizmeti çalışması için içeri aktarma işi nasıl işleyeceğini belirtir. İçeri aktarma bildiriminden bu değer belirtilmezse, varsayılan değer: `rename`.<br /><br /> Bu öğe için değerleri şunlardır:<br /><br /> -   `no-overwrite`: Aynı ada sahip bir hedef blob zaten varsa, içeri aktarma işlemi bu dosyayı içeri atlar.<br />-   `overwrite`: Tüm mevcut hedef blob tamamen yeni içeri aktarılan dosyası tarafından üzerine yazılır.<br />-   `rename`: Yeni blob değiştirilmiş bir adla karşıya.<br /><br /> Yeniden adlandırma kuralı aşağıdaki gibidir:<br /><br /> -Blob adı bir nokta içermiyorsa, yeni adı eklenerek oluşturulur `(2)` bu yeni adı da mevcut bir blob adıyla ardından çakışıyorsa, özgün blob adını; `(3)` yerine eklenmiş `(2)`; ve benzeri.<br />-Son nokta aşağıdaki bölümü blob adı bir nokta içeriyorsa, uzantı adı olarak kabul edilir. Yukarıdaki yordama benzer `(2)` adı ve ardından hizmeti ile varolan yeni adı hala çakışmaları blob varsa yeni bir ad; oluşturmak için son nokta denemeden önce eklenen `(3)`, `(4)`ve benzeri çakışmayan bir adı bulunana kadar.<br /><br /> Bazı örnekler:<br /><br /> Blob `BlobNameWithoutDot` adlandırılacak:<br /><br /> `BlobNameWithoutDot (2)  // if BlobNameWithoutDot exists`<br /><br /> `BlobNameWithoutDot (3)  // if both BlobNameWithoutDot and BlobNameWithoutDot (2) exist`<br /><br /> Blob `Seattle.jpg` adlandırılacak:<br /><br /> `Seattle (2).jpg  // if Seattle.jpg exists`<br /><br /> `Seattle (3).jpg  // if both Seattle.jpg and Seattle (2).jpg exist`|  
|`PageRangeList`|İç içe geçmiş XML öğesi|Bir sayfa blobu için gereklidir.<br /><br /> Yönelik içeri aktarma işlemi, içeri aktarılacak bir dosya bayt aralıkları listesi belirtir. Her sayfa aralık bir uzaklık ve uzunluk bölgesinin bir MD5 karma değeri ile birlikte sayfa aralığını açıklayan kaynak dosyadaki tarafından açıklanmıştır. `Hash` Sayfası aralığının özniteliği gereklidir. Hizmeti BLOB veri karması hesaplanan MD5 karma değeri sayfa aralıktan eşleştiğini doğrular. Herhangi bir sayıda sayfa aralıklarını toplam boyutu 1 TB'ye kadar bir içeri aktarma için bir dosya açıklamak için kullanılabilir. Tüm sayfa aralıklarını uzaklığı göre sıralanmalıdır ve hiçbir çakışma izin verilir.<br /><br /> Bir dışarı aktarma için işlemi, bir dizi sürücüye dışarı aktardığınız bir blobu bayt aralığı belirtir.<br /><br /> Sayfa aralıklarını birlikte bir blob veya dosya yalnızca alt aralığı kapsayabilir.  Beklenen bir dosyanın herhangi bir sayfa aralığı tarafından kapsanmayan kalan bölümü ve içeriğini tanımsız olabilir.|  
|`PageRange`|XML öğesi|Sayfa aralığını temsil eder.|  
|`PageRange/@Offset`|Öznitelik, bir tamsayı|Dosya aktarımı ve belirtilen sayfa aralığı başladığı blob uzaklığını belirtir. Bu değer 512 olmalıdır.|  
|`PageRange/@Length`|Öznitelik, bir tamsayı|Sayfa aralığı belirtir. Bu değer 512 ve en fazla 4 MB olmalıdır.|  
|`PageRange/@Hash`|Öznitelik, dize|Sayfa aralığı Base16 kodlu MD5 karma değeri belirtir.|  
|`BlockList`|İç içe geçmiş XML öğesi|Adlandırılmış blokları blok blob'u için gereklidir.<br /><br /> Bir içeri aktarma işlemi için bir Azure depolama alanına aktarılır blokları kümesi Engellenenler listesine belirtir. Bir dışarı aktarma işlemi için burada her blok dosyasına dışarı aktarma diskte daha fazladır saklanan Engellenenler listesine belirtir. Her blok bir uzaklık ve blok uzunluğu ile tanımlanır; Her blok ayrıca bir blok kimliği özniteliğiyle olarak da adlandırılır ve bir MD5 karma değeri bloğu içerir. En fazla 50.000 blok blob açıklamak için kullanılabilir.  Tüm bloklar gerekir sıralı uzaklık ve birlikte dosya tam aralığını kapsamalıdır *yani*, blokları boşluksuz olmalıdır. En fazla 64 MB blob ise her blok blok kimliği tüm yok veya tüm mevcut olması gerekir. Base64 ile kodlanmış dize olarak blok kimliği gerekir. Bkz: [blok yerleştirme](/rest/api/storageservices/put-block) blok kimliği için ek gereksinimler için.|  
|`Block`|XML öğesi|Bir blok temsil eder.|  
|`Block/@Offset`|Öznitelik, bir tamsayı|Belirtilen bloğu başladığı uzaklığını belirtir.|  
|`Block/@Length`|Öznitelik, bir tamsayı|Bloğunda bulunan bayt sayısını belirtir. Bu değer, en fazla 4 MB olmalıdır.|  
|`Block/@Id`|Öznitelik, dize|Bloğun blok Kimliğini temsil eden bir dize belirtir.|  
|`Block/@Hash`|Öznitelik, dize|Bloğun Base16 kodlu MD5 karma değeri belirtir.|  
|`Blob/MetadataPath`|String|İsteğe bağlı. Meta veri dosyasının göreli yolunu belirtir. Bir içeri aktarma sırasında meta veriler, hedef blob üzerinde ayarlanır. Bir dışarı aktarma işlemi sırasında blob meta verileri meta veri dosyası sürücüsünde depolanır.|  
|`Blob/MetadataPath/@Hash`|Öznitelik, dize|Blobun meta veri dosyası Base16 kodlu MD5 karması belirtir.|  
|`Blob/PropertiesPath`|String|İsteğe bağlı. Özellikleri dosyasının göreli yolunu belirtir. Bir içeri aktarma sırasında özelliklerini, hedef blob üzerinde ayarlanır. Bir dışarı aktarma işlemi sırasında blob özelliklerini özellikler dosyası sürücüsünde depolanır.|  
|`Blob/PropertiesPath/@Hash`|Öznitelik, dize|Blobun özellikler dosyası Base16 kodlu MD5 karması belirtir.|  
  
## <a name="next-steps"></a>Sonraki adımlar
 
* [Depolama içeri/dışarı aktarma REST API'si](/rest/api/storageimportexport/)
