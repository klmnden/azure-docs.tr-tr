---
title: "Java kullanarak azure depolama örnekleri | Microsoft Docs"
description: "Görüntülemek, indirin ve örnek kod ve uygulamaları için Azure Storage çalıştırın. BLOB, kuyruklar, tablolar ve dosyaları için Java depolama istemcisi kitaplıklarını kullanarak örnek Başlarken bulur."
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.openlocfilehash: fd27e1ac9a773e7b0f5245aa74acdb0521cd098c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-storage-samples-using-java"></a>Java kullanarak azure depolama örnekleri

## <a name="java-sample-index"></a>Java örnek dizini

Aşağıdaki tabloda bizim örnek depo ve her örnek kapsamdaki senaryolar genel bakış sağlar. Github'da karşılık gelen örnek kod görüntülemek için bağlantıları tıklatın.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Uç Nokta</th><th style="font-size:110%">Senaryo</th><th style="font-size:110%">Örnek kod</th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><b>BLOB</b></td>
<td>BLOB ekleme</td> 
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Blok blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmeti ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>İstemci Tarafında Şifreleme</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Java'da Azure istemci tarafı şifreleme ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>BLOB kopyalama</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmeti ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Kapsayıcı oluşturma</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmeti ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>BLOB Sil</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmeti ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Kapsayıcısını silmek</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmeti ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>BLOB meta verileri/özellikler/Stats</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Java'da Azure Blob hizmeti ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Kapsayıcı ACL/meta veri/özellikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Java'da Azure Blob hizmeti ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Sayfa aralıklarını alma</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Örnek sayfa blobu testleri</a></td>
</tr> 
<tr> 
<td>Kira Blob/kapsayıcısı</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmeti ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Liste Blob/kapsayıcısı</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmeti ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Sayfa blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmeti ile çalışmaya başlama</a></td>
</tr>
<tr> 
<td>SAS</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">SAS testleri örnek</a></td>
</tr>   
<tr> 
<td>Hizmet Özellikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Java'da Azure Blob hizmeti ile çalışmaya başlama</a></td>
</tr>           
<tr> 
<td>Anlık görüntü Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Java'da Azure Blob hizmeti ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td rowspan="9"><b>Dosya</b></td>
<td>Dizinleri/paylaşımları/dosyaları oluşturma</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr>
<tr> 
<td>Dizinleri/paylaşımları/dosyaları silin</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Dizin özellikleri/meta veri</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Dosyaları indirme</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Dosya özellikleri/meta veri/ölçümleri</td> 
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
<td>Meta veri/özellikler/Stats paylaşma</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Java'da Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr>
<tr> 
<td rowspan="8"><b>Sırası</b></td>
<td>Mesaj ekleyin.</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Depolama Java istemci kitaplığı örnekleri</a></td> 
</tr> 
<tr> 
<td>İstemci Tarafında Şifreleme</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Depolama Java istemci kitaplığı örnekleri</a></td> 
</tr> 
<tr> 
<td>Sıra oluşturma</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Java'da Azure kuyruk hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>İleti/kuyruk silme</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Java'da Azure kuyruk hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>İletiye Gözat</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Java'da Azure kuyruk hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Sıra ACL/meta veri/Stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Java'da Azure kuyruk hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Kuyruk hizmeti özellikleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Java'da Azure kuyruk hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Güncelleştirme iletisi</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Java'da Azure kuyruk hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td rowspan="7"><b>Tablo</b></td>
<td>Tablo oluşturma</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Java'da Azure tablo hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Varlık/tablo silme</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Java'da Azure tablo hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Birleştirme/Ekle/Değiştir varlık</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Depolama Java istemci kitaplığı örnekleri</a></td> 
</tr> 
<tr> 
<td>Sorgu varlıklar</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Java'da Azure tablo hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Sorgu tabloları</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Java'da Azure tablo hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Tablo ACL/özellikleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Java'da Azure tablo hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Varlık güncelleştir</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Depolama Java istemci kitaplığı örnekleri</a></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a>Azure Kod Örnekleri Kitaplığı

Tam örnek kitaplığını görüntülemek için Git [Azure Kod örnekleri](https://azure.microsoft.com/resources/samples/?service=storage) Azure Storage için karşıdan yükle ve yerel olarak çalıştırılan örnekleri içeren kitaplık. Kod örneği kitaplığı .zip biçimi örnek kodda sağlar. Alternatif olarak, bulun ve her bir örnek için GitHub deposunu kopyalayın.

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>Başlarken kılavuzları alma

Yükleyin ve Azure Storage istemcisi kitaplıklarını ile çalışmaya başlama hakkında yönergeler arıyorsanız aşağıdaki kılavuzlara denetleyin.

* [Java'da Azure Blob hizmeti ile çalışmaya başlama](../blobs/storage-java-how-to-use-blob-storage.md)
* [Java'da Azure kuyruk hizmeti ile çalışmaya başlama](../storage-java-how-to-use-queue-storage.md)
* [Java'da Azure tablo hizmeti ile çalışmaya başlama](../../cosmos-db/table-storage-how-to-use-java.md)
* [Java'da Azure dosya hizmeti ile çalışmaya başlama](../storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a>Sonraki adımlar

Diğer diller için örnekleri hakkında daha fazla bilgi için:

* .NET: [.NET kullanarak azure Storage örnekleri](../storage-samples-dotnet.md)
* Tüm diğer dillere: [Azure Storage örnekleri](../storage-samples.md)
