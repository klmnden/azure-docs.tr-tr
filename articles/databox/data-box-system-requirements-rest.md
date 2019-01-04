---
title: Microsoft Azure veri kutusu Blob Depolama alanı gereksinimleri | Microsoft Docs
description: API, SDK'ları ve istemci kitaplıklarının Azure veri kutusu Blob Depolama için desteklenen sürümleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 12/11/2018
ms.author: alkohli
ms.openlocfilehash: e7c2cc0c0ffaae11bd7bf5113c942cdb98397201
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53551560"
---
# <a name="azure-data-box-blob-storage-requirements"></a>Azure veri kutusu Blob Depolama alanı gereksinimleri

Bu makalede, Azure API'leri, SDK'lar ve Araçlar Veri kutusu Blob Depolama ile desteklenen sürümleri listelenmiştir. Veri kutusu Blob Depolama, Azure ile tutarlı semantiğine sahip blob Yönetimi işlevselliği sağlar. Bu makalede, ayrıca Azure depolama hizmetlerinde bilinen Azure veri kutusu Blob Depolama farklar özetlenmektedir.

Veri kutusu Blob depolamaya bağlanma ve daha sonra geri gerektiği şekilde başvurduğu önce bilgileri dikkatlice gözden öneririz.


## <a name="storage-differences"></a>Depolama farkları

|     Özellik                                             |     Azure Storage                                     |     Veri kutusu Blob Depolama |
|---------------------------------------------------------|-------------------------------------------------------|---------------------------|
|    Azure dosya depolama                                   |    Desteklenen bulut tabanlı SMB dosya paylaşımları              |    Desteklenmiyor      |
|    Bekleyen veriler için şifreleme hizmeti                  |    256 bit AES şifrelemesi                             |    256 bit AES şifrelemesi |
|    Depolama hesabı türü                                 |    Genel amaçlı ve Azure blob depolama hesapları    |    Yalnızca genel amaçlı v1|
|    Blob adı                                            |    1024 karakter (2.048 bayt)                     |    880 karakterleri (1,760 bayt)|
|    Blok blobu en büyük boyutu                              |    4,75 TB (100 MB X 50.000 blok)                   |    Azure Data Box v 1.7 ve sonraki sürümler için 4,75 TB (100 MB x 50.000 blok).|
|    Sayfa blob en büyük boyutu                               |    8 TB                                               |    1 TB                   |
|    Sayfa blob sayfa boyutu                                  |    512 bayt                                          |    4 KB                   |

## <a name="supported-api-versions"></a>Desteklenen API sürümleri

Veri kutusu Blob Depolama ile Azure depolama hizmeti API'ın şu sürümleri desteklenir:

Genel Önizleme sürümü (Azure Data Box 1.7 ve sonraki sürümler)

- [2017-04-17](/rest/api/storageservices/version-2017-04-17)
- [2016-05-31](/rest/api/storageservices/version-2016-05-31)
- [2015-12-11](/rest/api/storageservices/version-2015-12-11)
- [2015-07-08](/rest/api/storageservices/version-2015-07-08)
- [2015-04-05](/rest/api/storageservices/version-2015-04-05)

## <a name="supported-sdk-versions"></a>Desteklenen SDK sürümleri

|     İstemci kitaplığı     |     Veri kutusu Blob Depolama desteklenen sürüm     |     Bağlantı             |     Uç nokta belirtimi         |
|------------------------|-------------------------------------------------|---------------------------------------------|------------------------------------|
|    .NET                |    8.7.0 için 6.2.0.                         |    Nuget paketi:   https://www.nuget.org/packages/WindowsAzure.Storage/ <br>GitHub sürüm:   https://github.com/Azure/azure-storage-net/releases                                                                      |    app.config dosyası                 |
|    Java                |    4.1.0 6.1.0 için                          |    Maven paketi:   http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage   <br>GitHub sürüm:   https://github.com/Azure/azure-storage-java/releases                                                      |    Bağlantı dizesi kurulumu         |
|    Node.js             |    1.1.0 2.7.0 için                          |    NPM bağlantısı:   https://www.npmjs.com/package/azure-storage   (Örneğin: Çalıştır "npm yükleme azure-storage@2.7.0")   <br>GitHub sürüm:   https://github.com/Azure/azure-storage-node/releases                            |    Hizmet örneği bildirimi    |
|    C++                 |    2.4.0 3.1.0 için                          |    Nuget paketi:   https://www.nuget.org/packages/wastorage.v140/   <br>GitHub sürüm:   https://github.com/Azure/azure-storage-cpp/releases                                                                            |    Bağlantı dizesi kurulumu         |
|    PHP                 |    0.15.0 1.0.0 için                         |    GitHub sürüm:   https://github.com/Azure/azure-storage-php/releases   <br>Oluşturucusu yükleyin (aşağıdaki ayrıntılara bakın)                                                                                                   |    Bağlantı dizesi kurulumu         |
|    Python              |    0.30.0 1.0.0 için                         |    GitHub sürüm:   https://github.com/Azure/azure-storage-python/releases                                                                                                                                              |    Hizmet örneği bildirimi    |
|    Ruby                |    0.12.1 1.0.1 için                         |    RubyGems paketi:<br>Ortak:   https://rubygems.org/gems/azure-storage-common/   <br>BLOB: https://rubygems.org/gems/azure-storage-blob/      <br>GitHub sürüm:   https://github.com/Azure/azure-storage-ruby/releases    |                                   |

## <a name="supported-azure-client-libraries"></a>Azure istemci kitaplıkları desteklenir

Veri kutusu Blob Depolama için belirli istemci kitaplıkları ve belirli bir uç nokta son ek gereksinimler vardır.

Desteklenen REST API'si veri kutusu Blob Depolama için 2017-04-17 2016-05-31, 2015-12-11, 2015-07-08 ve Azure Data Box sürüm 1.7 ve sonraki sürümler için 2015-04-05 sürümleridir. Veri kutusu Blob Depolama uç noktaları, en son sürümü Azure Blob Depolama REST API'si ile tam eşlik yoktur. İçin depolama istemci kitaplıkları, REST API ile uyumlu sürümü farkında olmanız gerekir.

### <a name="azure-data-box-17-onwards"></a>Azure Data Box 1.7 ve sonraki sürümler

| İstemci kitaplığı     |Veri kutusu Blob Depolama desteklenen sürüm     | Bağlantı   |     Uç nokta belirtimi      |
|--------------------|--------------------------------------------|--------|---------------------------------|
|    .NET                |    8.7.0                                           |    Nuget paketi:   https://www.nuget.org/packages/WindowsAzure.Storage/8.7.0    <br>GitHub sürüm:   https://github.com/Azure/azure-storage-net/releases/tag/v8.7.0                                                                                                                                                                                               |    app.config dosyası                 |
|    Java                |    6.1.0                                           |    Maven paketi:   http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/6.1.0   <br>GitHub sürüm:   https://github.com/Azure/azure-storage-java/releases/tag/v6.1.0                                                                                                                                                                              |    Bağlantı dizesi kurulumu         |
|    Node.js             |    2.7.0                                           |    NPM bağlantısı:   https://www.npmjs.com/package/azure-storage   (Çalıştırın: npm yükleme azure-storage@2.7.0)   <br>GitHub sürüm:   https://github.com/Azure/azure-storage-node/releases/tag/v2.7.0                                                                                                                                                                        |    Hizmet örneği bildirimi    |
|    C++                 |    3.1.0                                           |    Nuget paketi:   https://www.nuget.org/packages/wastorage.v140/3.1.0   <br>GitHub sürüm:   https://github.com/Azure/azure-storage-cpp/releases/tag/v3.1.0                                                                                                                                                                                                     |    Bağlantı dizesi kurulumu         |
|    PHP                 |    1.0.0                                           |    GitHub sürüm:<br>Ortak: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-common   <br>BLOB: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-blob      <br>(Daha fazla bilgi edinmek için aşağıdaki ayrıntıları görmek için.), oluşturucu yükleme                                                                                                             |    Bağlantı dizesi kurulumu         |
|    Python              |    1.0.0                                           |    GitHub sürüm:<br>Ortak:   https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-common <br>BLOB:   https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-blob                                                                                                                                                                          |    Hizmet örneği bildirimi    |
|    Ruby                |    1.0.1                                           |    RubyGems paketi:<br>Ortak:   https://rubygems.org/gems/azure-storage-common/versions/1.0.1   <br>BLOB: https://rubygems.org/gems/azure-storage-blob/versions/1.0.1         <br>GitHub sürüm:<br>Ortak: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common   <br>BLOB: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob          |    Bağlantı dizesi kurulumu         |



### <a name="install-php-client-via-composer---current"></a>Composer - geçerli PHP istemcisi yükleme

Oluşturucusu yüklemek için: (örnek olarak blob Al).
Aşağıdaki kodla proje kökündeki Composer.JSON adlı bir dosya oluşturun:

```
 {
   "require": {
   "Microsoft/azure-storage-blob":"1.0.0"
   }
```

İndirme `composer.phar` proje kök dizini.

Çalıştır: php composer.phar yükleyin.

### <a name="endpoint-declaration"></a>Uç nokta bildirimini

Bir Azure veri kutusu Blob Depolama uç noktasının iki bölümleri içerir: bir bölge ve Data Box etki alanı adı. Veri kutusu Blob Depolama SDK'da, varsayılan uç nokta, <serial no. of the device>. microsoftdatabox.com.  Blob Hizmeti uç noktası hakkında daha fazla bilgi için Git [veri kutusu Blob Depolama alanı üzerinden Bağlan](data-box-deploy-copy-data-via-rest.md).
 
## <a name="examples"></a>Örnekler

### <a name="net"></a>.NET

Veri kutusu Blob Depolama uç noktası son eki içinde belirtilen `app.config` dosyası:

```
<add key="StorageConnectionString"
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=<<serial no. of the device>.microsoftdatabox.com  />
```

### <a name="java"></a>Java

Veri kutusu Blob Depolama uç noktası son eki bağlantı dizesinin ayarında belirtilen:

```
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=<serial no. of the device>.microsoftdatabox.com ";
```

### <a name="nodejs"></a>Node.js

Veri kutusu Blob Depolama uç noktası son eki bildirimi örneğinde belirtilir:

```
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob. <serial no. of the device>.microsoftdatabox.com ');
```

### <a name="c"></a>C++

Veri kutusu Blob Depolama uç noktası son eki bağlantı dizesinin ayarında belirtilen:

```
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=<serial no. of the device>.microsoftdatabox.com "));
```

### <a name="php"></a>PHP

Veri kutusu Blob Depolama uç noktası son eki bağlantı dizesinin ayarında belirtilen:

```
$connectionString = 'BlobEndpoint=http://<storage account name>.blob.<serial no. of the device>.microsoftdatabox.com /;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Veri kutusu Blob Depolama uç noktası son eki bildirimi örneğinde belirtilir:

```
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix=’<serial no. of the device>.microsoftdatabox.com’)
```

### <a name="ruby"></a>Ruby

Veri kutusu Blob Depolama uç noktası son eki bağlantı dizesinin ayarında belirtilen:

```
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=<serial no. of the device>.microsoftdatabox.com
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Box'ınızı dağıtın](data-box-deploy-ordered.md)