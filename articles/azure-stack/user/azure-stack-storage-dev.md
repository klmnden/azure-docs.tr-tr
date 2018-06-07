---
title: Azure yığın depolama geliştirme araçları ile çalışmaya başlama | Microsoft Docs
description: Azure yığın depolama geliştirme araçlarını kullanmaya başlamak için kılavuz
services: azure-stack
author: mabriggs
ms.author: mabrigg
ms.date: 05/21/2018
ms.topic: get-started-article
ms.service: azure-stack
manager: femila
ms.reviewer: xiaofmao
ms.openlocfilehash: 0ceda393412f8217a893a347ec5f3a9ac03efa3d
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604486"
---
# <a name="get-started-with-azure-stack-storage-development-tools"></a>Azure yığın depolama geliştirme araçları ile çalışmaya başlama

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Microsoft Azure yığın blob, tablo ve kuyruk depolama içeren bir dizi depolama hizmetleri sağlar.

Bu makalede, Azure yığın depolama geliştirme araçlarını kullanmaya başlamak için bir kılavuz olarak kullanın. Daha ayrıntılı bilgi ve örnek kod karşılık gelen Azure depolama eğitimlerine bulabilirsiniz.

> [!NOTE]  
> Azure yığın depolama ve her platform için belirli gereksinimleri dahil olmak üzere Azure storage arasındaki farklar bilinen vardır. Örneğin, belirli istemci kitaplıkları ve Azure yığını için belirli bir uç soneki gereksinimleri vardır. Daha fazla bilgi için bkz: [Azure yığın depolama: farklar ve konuları](azure-stack-acs-differences.md).

## <a name="azure-client-libraries"></a>Azure istemci kitaplıkları

Desteklenen REST API Azure yığın depolama 2017-04-17, 2016-05-31, 2015-12-11, 2015-07-08, 2015-04-05 1802 güncelleştirmesi veya daha yeni sürümleri için ve önceki sürümler için 2015-04-05 sürümleridir. Azure yığın uç noktaları Azure storage REST API'si en son sürümü ile tam eşlik yok. Depolama istemcisi kitaplıklarını için REST API ile uyumlu olan sürümle farkında olmanız gerekir.

### <a name="1802-update-or-newer-versions"></a>1802 güncelleştirmesi veya daha yeni sürümleri

| İstemci kitaplığı | Azure desteklenen yığın sürümü | Bağlantı | Uç nokta belirtimi |
|----------------|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET | 8.7.0 | Nuget paketi:<br>https://www.nuget.org/packages/WindowsAzure.Storage/8.7.0<br> <br>GitHub sürüm:<br>https://github.com/Azure/azure-storage-net/releases/tag/v8.7.0 | app.config dosyası |
| Java | 6.1.0 | Maven paketi:<br>http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/6.1.0<br> <br>GitHub sürüm:<br>https://github.com/Azure/azure-storage-java/releases/tag/v6.1.0 | Bağlantı dizesi kurulumu |
| Node.js | 2.7.0 | NPM bağlantı:<br>https://www.npmjs.com/package/azure-storage<br>(Çalıştırın: `npm install azure-storage@2.7.0`)<br> <br>Github sürüm:<br>https://github.com/Azure/azure-storage-node/releases/tag/v2.7.0 | Hizmet örneği bildirimi |
| C++ | 3.1.0 | Nuget paketi:<br>https://www.nuget.org/packages/wastorage.v140/3.1.0<br> <br>GitHub sürüm:<br>https://github.com/Azure/azure-storage-cpp/releases/tag/v3.1.0 | Bağlantı dizesi kurulumu |
| PHP | 1.0.0 | GitHub sürüm:<br>Ortak: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-common<br>BLOB: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-blob<br>Sıra:<br>https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-queue<br>Tablo: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-table<br> <br>Oluşturucu yükleyin (daha fazla bilgi edinmek için [ayrıntıları görmek](#install-php-client-via-composer---current).) | Bağlantı dizesi kurulumu |
| Python | 1.0.0 | GitHub sürüm:<br>Ortak:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-common<br>BLOB:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-blob<br>Sıra:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-queue | Hizmet örneği bildirimi |
| Ruby | 1.0.1 | RubyGems paketi:<br>Ortak:<br>https://rubygems.org/gems/azure-storage-common/versions/1.0.1<br>BLOB: https://rubygems.org/gems/azure-storage-blob/versions/1.0.1<br>Sıra: https://rubygems.org/gems/azure-storage-queue/versions/1.0.1<br>Tablo: https://rubygems.org/gems/azure-storage-table/versions/1.0.1<br> <br>GitHub sürüm:<br>Ortak: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common<br>BLOB: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob<br>Sıra: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-queue<br>Tablo: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-table | Bağlantı dizesi kurulumu |

#### <a name="install-php-client-via-composer---current"></a>Oluşturucu - geçerli PHP istemcisi yükleme

Oluşturucu yüklemek için: (örnek olarak alın blob).

1. Adlı bir dosya oluşturun **composer.json** aşağıdaki kod ile proje kökündeki:

  ```php
    {
      "require": {
      "Microsoft/azure-storage-blob":"1.0.0"
      }
    }
  ```

2. Karşıdan [composer.phar](http://getcomposer.org/composer.phar) proje kök dizini.
3. Çalıştır: `php composer.phar install`.

### <a name="previous-versions"></a>Önceki sürümler

|İstemci kitaplığı|Azure desteklenen yığın sürümü|Bağlantı|Uç nokta belirtimi|
|---------|---------|---------|---------|
|.NET     |6.2.0|Nuget paketi:<br>[https://www.nuget.org/packages/WindowsAzure.Storage/6.2.0](https://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)<br><br>GitHub sürüm:<br>[https://github.com/Azure/azure-storage-net/releases/tag/v6.2.1](https://github.com/Azure/azure-storage-net/releases/tag/v6.2.1)|app.config dosyası|
|Java|4.1.0|Maven paketi:<br>[http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/4.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/4.1.0)<br><br>GitHub sürüm:<br> [https://github.com/Azure/azure-storage-java/releases/tag/v4.1.0](https://github.com/Azure/azure-storage-java/releases/tag/v4.1.0)|Bağlantı dizesi kurulumu|
|Node.js     |1.1.0|NPM bağlantı:<br>[https://www.npmjs.com/package/azure-storage](https://www.npmjs.com/package/azure-storage)<br>(çalıştırın: `npm install azure-storage@1.1.0)`<br><br>Github sürüm:<br>[https://github.com/Azure/azure-storage-node/releases/tag/1.1.0](https://github.com/Azure/azure-storage-node/releases/tag/1.1.0)|Hizmet örneği bildirimi||C++|2.4.0|Nuget paketi:<br>[https://www.nuget.org/packages/wastorage.v140/2.4.0](https://www.nuget.org/packages/wastorage.v140/2.4.0)<br><br>GitHub sürüm:<br>[https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0](https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0)|Bağlantı dizesi kurulumu|
|C++|2.4.0|Nuget paketi:<br>[https://www.nuget.org/packages/wastorage.v140/2.4.0](https://www.nuget.org/packages/wastorage.v140/2.4.0)<br><br>GitHub sürüm:<br>[https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0](https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0)|Bağlantı dizesi kurulumu|
|PHP|0.15.0|GitHub sürüm:<br>[https://github.com/Azure/azure-storage-php/releases/tag/v0.15.0](https://github.com/Azure/azure-storage-php/releases/tag/v0.15.0)<br><br>Oluşturucu yükleyin (aşağıda yer alan ayrıntılara bakın)|Bağlantı dizesi kurulumu|
|Python     |0.30.0|PIP paketi:<br> [https://pypi.python.org/pypi/azure-storage/0.30.0](https://pypi.python.org/pypi/azure-storage/0.30.0)<br>(Çalıştırın: `pip install -v azure-storage==0.30.0)`<br><br>GitHub sürüm:<br> [https://github.com/Azure/azure-storage-python/releases/tag/v0.30.0](https://github.com/Azure/azure-storage-python/releases/tag/v0.30.0)|Hizmet örneği bildirimi|
|Ruby|0.12.1<br>Önizleme|RubyGems paketi:<br> [https://rubygems.org/gems/azure-storage/versions/0.12.1.preview](https://rubygems.org/gems/azure-storage/versions/0.12.1.preview)<br><br>GitHub sürüm:<br> [https://github.com/Azure/azure-storage-ruby/releases/tag/v0.12.1](https://github.com/Azure/azure-storage-ruby/releases/tag/v0.12.1)|Bağlantı dizesi kurulumu|

#### <a name="install-php-client-via-composer---previous"></a>Oluşturucu - önceki aracılığıyla PHP istemcisini yükleme

Oluşturucu yüklemek için:

1. Adlı bir dosya oluşturun **composer.json** aşağıdaki kod ile proje kökündeki:

  ```php
    {
          "require":{
          "Microsoft/azure-storage":"0.15.0"
          }
    }
  ```

2. Karşıdan [composer.phar](http://getcomposer.org/composer.phar) proje kök içine.
3. Çalıştır: `php composer.phar install`.

## <a name="endpoint-declaration"></a>Uç nokta bildirimini

Bir Azure yığın uç nokta iki bölümleri içerir: bir bölge ve Azure yığın etki alanı adı.
Azure yığın Development Kit'te varsayılan uç noktadır **local.azurestack.external**.
Uç noktanız hakkında emin değilseniz, bulut yöneticinize başvurun.

## <a name="examples"></a>Örnekler

### <a name="net"></a>.NET

Azure yığını için uç nokta soneki app.config dosyasında belirtilir:

```
<add key="StorageConnectionString"
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=local.azurestack.external;" />
```

### <a name="java"></a>Java

Azure yığını için uç nokta soneki bağlantı dizesi ayarında belirtilen:

```
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=local.azurestack.external";
```

### <a name="nodejs"></a>Node.js

Azure yığını için uç nokta soneki bildirimi örneğinde belirtilmiştir:

```
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob.local.azurestack.external');
```

### <a name="c"></a>C++

Azure yığını için uç nokta soneki bağlantı dizesi ayarında belirtilen:

```
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=local.azurestack.external"));
```

### <a name="php"></a>PHP

Azure yığını için uç nokta soneki bağlantı dizesi ayarında belirtilen:

```
$connectionString = 'BlobEndpoint=http://<storage account name>.blob.local.azurestack.external/;
QueueEndpoint=http:// <storage account name>.queue.local.azurestack.external/;
TableEndpoint=http:// <storage account name>.table.local.azurestack.external/;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Azure yığını için uç nokta soneki bildirimi örneğinde belirtilmiştir:

```
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix='local.azurestack.external')
```

### <a name="ruby"></a>Ruby

Azure yığını için uç nokta soneki bağlantı dizesi ayarında belirtilen:

```
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=local.azurestack.external
```

## <a name="blob-storage"></a>Blob depolama

Aşağıdaki Azure Blob Depolama öğreticileri Azure yığını için geçerlidir. Azure önceki açıklanan yığını belirli uç nokta soneki gereksinimini Not [örnekler](#examples) bölümü.

* [.NET kullanarak Azure Blob Depolama ile çalışmaya başlama](../../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Java'da Blob Depolama'yı kullanma](../../storage/blobs/storage-java-how-to-use-blob-storage.md)
* [Node.js'de Blob Depolama'yı kullanma](../../storage/blobs/storage-nodejs-how-to-use-blob-storage.md)
* [C++ içinden BLOB storage kullanma](../../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [PHP'de Blob Depolama'yı kullanma](../../storage/blobs/storage-php-how-to-use-blobs.md)
* [Python'dan Azure Blob storage kullanma](../../storage/blobs/storage-python-how-to-use-blob-storage.md)
* [Ruby'de Blob Depolama'yı kullanma](../../storage/blobs/storage-ruby-how-to-use-blob-storage.md)

## <a name="queue-storage"></a>Kuyruk depolama

Aşağıdaki Azure kuyruk depolama öğreticileri Azure yığını için geçerlidir. Azure önceki açıklanan yığını belirli uç nokta soneki gereksinimini Not [örnekler](#examples) bölümü.

* [.NET kullanarak Azure Kuyruk Depolama ile çalışmaya başlama](../../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Java'da Kuyruk Depolama'yı kullanma](../../storage/queues/storage-java-how-to-use-queue-storage.md)
* [Node.js'de Kuyruk Depolama'yı kullanma](../../storage/queues/storage-nodejs-how-to-use-queues.md)
* [C++ içinden kuyruk depolama kullanma](../../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [PHP'de Kuyruk Depolama'yı kullanma](../../storage/queues/storage-php-how-to-use-queues.md)
* [Python'da Kuyruk Depolama'yı kullanma](../../storage/queues/storage-python-how-to-use-queue-storage.md)
* [Ruby'de Kuyruk Depolama'yı kullanma](../../storage/queues/storage-ruby-how-to-use-queue-storage.md)

## <a name="table-storage"></a>Table Storage

Aşağıdaki Azure Table depolama öğreticileri Azure yığını için geçerlidir. Azure önceki açıklanan yığını belirli uç nokta soneki gereksinimini Not [örnekler](#examples) bölümü.

* [.NET kullanarak Azure Tablo Depolama ile çalışmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Java'da Tablo Depolama'yı kullanma](../../cosmos-db/table-storage-how-to-use-java.md)
* [Node.js'den Azure Tablo depolamayı kullanma](../../cosmos-db/table-storage-how-to-use-nodejs.md)
* [Tablo depolama C++ içinden kullanma](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [PHP'de Tablo Depolama'yı kullanma](../../cosmos-db/table-storage-how-to-use-php.md)
* [Tablo depolama Python içinde kullanma](../../cosmos-db/table-storage-how-to-use-python.md)
* [Ruby'de Tablo Depolama'yı kullanma](../../cosmos-db/table-storage-how-to-use-ruby.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Storage'a giriş](../../storage/common/storage-introduction.md)