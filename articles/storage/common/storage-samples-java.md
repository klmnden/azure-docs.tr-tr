---
title: Java kullanarak azure depolama örnekleri | Microsoft Docs
description: Görüntüleyin, indirin ve örnek kod ve Azure depolama için uygulamalar çalıştırın. Başlama bloblar, kuyruklar, tablolar ve dosyalar için Java depolama istemci kitaplıkları kullanarak örnekleri keşfedin.
services: storage
author: seguler
ms.service: storage
ms.devlang: java
ms.topic: article
ms.date: 01/12/2017
ms.author: seguler
ms.subservice: common
ms.openlocfilehash: d216313ae88837df395c64a3ba9e707a5bca06a5
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59565343"
---
# <a name="azure-storage-samples-using-java"></a>Java kullanarak azure depolama örnekleri

## <a name="java-sample-index"></a>Java örnek dizini

Aşağıdaki tabloda örnekleri depomuzda ve her örneğinde kapsanan senaryolar hakkında genel bir bakış sağlar. Github'da karşılık gelen örnek kod için bağlantılar'a tıklayın.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Uç Nokta</th><th style="font-size:110%">Senaryo</th><th style="font-size:110%">Örnek Kod</th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><b>Blob</b></td>
<td>Ekleme Blobu</td> 
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmetini kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td>Blok Blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr> 
<tr> 
<td>İstemci Tarafında Şifreleme</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Java'da Azure istemci tarafı şifreleme kullanmaya başlama</a></td>
</tr> 
<tr> 
<td>Kopya blob'u</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr> 
<tr> 
<td>Kapsayıcı oluşturma</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr> 
<tr> 
<td>BLOB silme</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr> 
<tr> 
<td>Kapsayıcıyı Sil</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr> 
<tr> 
<td>BLOB meta verileri/özellik/Stats</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Java'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr> 
<tr> 
<td>Kapsayıcı ACL/meta verileri/özellikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Java'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr> 
<tr> 
<td>Alma sayfası aralıkları</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Sayfa blobu örnek test eder.</a></td>
</tr> 
<tr> 
<td>Kira Blob/kapsayıcı</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr> 
<tr> 
<td>Blob/kapsayıcı listeleme</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr> 
<tr> 
<td>Sayfa Blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>
<tr> 
<td>SAS</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/89540f018f1160ce55619c6fe7b5f5ff57d0ce10/src/test/java/com/microsoft/azure/storage/Samples.java#L513">SAS testleri örnek</a></td>
</tr>   
<tr> 
<td>Hizmet Özellikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Java'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr>           
<tr> 
<td>Blob anlık görüntüsü</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmetini kullanmaya başlama</a></td>
</tr> 
<tr> 
<td rowspan="9"><b>Dosya</b></td>
<td>Paylaşımları/dizin/dosya oluştur</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr>
<tr> 
<td>Dizinler/paylaşımları/dosyaları sil</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Dizin özelliklerini/meta verileri</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Dosyaları indirme</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Dosya özelliklerini/meta verileri/ölçümleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Dosya hizmeti özellikleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Liste dizinler ve dosyalar</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr>
<tr> 
<td>Paylaşımları</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr>
<tr> 
<td>Paylaşımı özelliklerini/meta verileri/Stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr>
<tr> 
<td rowspan="8"><b>Kuyruk</b></td>
<td>İleti Ekle</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Depolama Java istemci kitaplığı örnekleri</a></td> 
</tr> 
<tr> 
<td>İstemci Tarafında Şifreleme</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Depolama Java istemci kitaplığı örnekleri</a></td> 
</tr> 
<tr> 
<td>Kuyruk oluşturma</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Java'da Azure kuyruk hizmeti kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td>İleti/kuyruğu silin</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Java'da Azure kuyruk hizmeti kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td>İletiye Gözat</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Java'da Azure kuyruk hizmeti kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td>Kuyruk ACL/meta verileri/Stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Java'da Azure kuyruk hizmeti kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td>Kuyruk hizmeti özelliklerini</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Java'da Azure kuyruk hizmeti kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td>İletiyi güncelleştirme</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Java'da Azure kuyruk hizmeti kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td rowspan="7"><b>Tablo</b></td>
<td>Tablo Oluştur</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Varlık/tablo silme</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Birleştirme/Ekle/Değiştir varlık</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Varlıkları sorgulayın</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Sorgu tabloları</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Tablo ACL/özellikleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableAdvanced.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Varlık güncelleştir</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Java’da Azure Tablo Hizmetini Kullanmaya Başlama</a></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a>Azure Kod Örnekleri Kitaplığı

Tam örnek kitaplığı görüntülemek için Git [Azure Kod örnekleri](https://azure.microsoft.com/resources/samples/?service=storage) indirip yerel olarak çalıştırmak için Azure depolama örnekleri içeren bir kitaplık. Kod örneği kitaplığı .zip biçimli örnek kodda sağlar. Alternatif olarak, göz atabilir ve her örnek için GitHub deposunu kopyalayın.

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>Başlarken kılavuzları

Yükleme ve Azure depolama istemci kitaplıkları ile çalışmaya başlama konusunda yönergeler arıyorsanız aşağıdaki kılavuzlara denetleyin.

* [Java'da Azure Blob hizmetini kullanmaya başlama](../blobs/storage-quickstart-blobs-java.md)
* [Java'da Azure kuyruk hizmeti kullanmaya başlama](../queues/storage-java-how-to-use-queue-storage.md)
* [Java’da Azure Tablo Hizmetini Kullanmaya Başlama](../../cosmos-db/table-storage-how-to-use-java.md)
* [Java'da Azure dosya hizmeti ile çalışmaya başlama](../files/storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a>Sonraki adımlar

Diğer diller için örnekleri hakkında daha fazla bilgi için:

* .NET: [.NET kullanan Azure Depolama örnekleri](storage-samples-dotnet.md)
* Tüm diğer diller için: [Azure depolama örnekleri](storage-samples.md)
