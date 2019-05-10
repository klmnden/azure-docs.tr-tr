---
title: .NET kullanarak azure depolama örnekleri | Microsoft Docs
description: Görüntüleyin, indirin ve örnek kod ve Azure depolama için uygulamalar çalıştırın. Bloblar, kuyruklar, tablolar ve dosyalar için örnekleri .NET depolama istemci kitaplıklarını kullanmaya Başlarken keşfedin.
services: storage
author: mhopkins-msft
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 05/03/2019
ms.author: mhopkins
ms.subservice: common
ms.openlocfilehash: df7c14f1ee83015303657f9a0babde3d60c92292
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65209702"
---
# <a name="azure-storage-samples-using-net"></a>.NET kullanarak azure depolama örnekleri

## <a name="net-sample-index"></a>.NET örnek dizini

Aşağıdaki tabloda örnekleri depomuzda ve her örneğinde kapsanan senaryolar hakkında genel bir bakış sağlar. Github'da karşılık gelen örnek kod için bağlantılar'a tıklayın.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Uç Nokta</th><th style="font-size:110%">Senaryo</th><th style="font-size:110%">Örnek Kod</th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><b>Blob</b></td>
<td>Ekleme Blobu</td> 
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs#L1144">BLOB'ları ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Blok Blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Depolama Fotoğraf Galerisi Web uygulaması</a></td>
</tr> 
<tr> 
<td>İstemci Tarafında Şifreleme</td>
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">BLOB şifreleme örnekleri</a></td>
</tr> 
<tr> 
<td>Kopya blob'u</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'ları ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Kapsayıcı oluşturma</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Depolama Fotoğraf Galerisi Web uygulaması</a></td>
</tr> 
<tr> 
<td>BLOB silme</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Depolama Fotoğraf Galerisi Web uygulaması</a></td>
</tr> 
<tr> 
<td>Kapsayıcıyı Sil</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'ları ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>BLOB meta verileri/özellik/Stats</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'ları ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Kapsayıcı ACL/meta verileri/özellikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Depolama Fotoğraf Galerisi Web uygulaması</a></td>
</tr> 
<tr> 
<td>Alma sayfası aralıkları</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'ları ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Kira Blob/kapsayıcı</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'ları ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Blob/kapsayıcı listeleme</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">BLOB'ları ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Sayfa Blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">BLOB'ları ile çalışmaya başlama</a></td>
</tr>
<tr> 
<td>SAS</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'ları ile çalışmaya başlama</a></td>
</tr>   
<tr> 
<td>Hizmet Özellikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'ları ile çalışmaya başlama</a></td>
</tr>           
<tr> 
<td>Blob anlık görüntüsü</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Artımlı anlık görüntülerle Azure sanal makinesini yedekle diskler</a></td>
</tr> 
<tr> 
<td rowspan="9"><b>Dosya</b></td>
<td>Paylaşımları/dizin/dosya oluştur</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure depolama .NET dosya depolama örneği</a></td> 
</tr>
<tr> 
<td>Dizinler/paylaşımları/dosyaları sil</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">. NET'te Azure dosya hizmeti ile çalışmaya başlama</a></td> 
</tr> 
<tr> 
<td>Dizin özelliklerini/meta verileri</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure depolama .NET dosya depolama örneği</a></td> 
</tr> 
<tr> 
<td>Dosyaları indirme</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure depolama .NET dosya depolama örneği</a></td> 
</tr> 
<tr> 
<td>Dosya özelliklerini/meta verileri/ölçümleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure depolama .NET dosya depolama örneği</a></td> 
</tr> 
<tr> 
<td>Dosya hizmeti özellikleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure depolama .NET dosya depolama örneği</a></td> 
</tr> 
<tr> 
<td>Liste dizinler ve dosyalar</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure depolama .NET dosya depolama örneği</a></td> 
</tr>
<tr> 
<td>Paylaşımları</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure depolama .NET dosya depolama örneği</a></td> 
</tr>
<tr> 
<td>Paylaşımı özelliklerini/meta verileri/Stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure depolama .NET dosya depolama örneği</a></td> 
</tr>
<tr> 
<td rowspan="8"><b>Kuyruk</b></td>
<td>İleti Ekle</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">. NET'te Azure kuyruk hizmeti kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td>İstemci Tarafında Şifreleme</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Azure depolama .NET sıra istemci tarafı şifreleme</a></td> 
</tr> 
<tr> 
<td>Kuyruk oluşturma</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">. NET'te Azure kuyruk hizmeti kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td>İleti/kuyruğu silin</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">. NET'te Azure kuyruk hizmeti kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td>İletiye Gözat</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">. NET'te Azure kuyruk hizmeti kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td>Kuyruk ACL/meta verileri/Stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">. NET'te Azure kuyruk hizmeti kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td>Kuyruk hizmeti özelliklerini</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">. NET'te Azure kuyruk hizmeti kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td>İletiyi güncelleştirme</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">. NET'te Azure kuyruk hizmeti kullanmaya başlama</a></td> 
</tr> 
<tr> 
<td rowspan="7"><b>Tablo</b></td>
<td>Tablo Oluştur</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure depolama - örnek uygulaması kullanarak eşzamanlılığı yönetme</a></td> 
</tr> 
<tr> 
<td>Varlık/tablo silme</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">.NET’te Azure Tablo Depolama Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Birleştirme/Ekle/Değiştir varlık</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure depolama - örnek uygulaması kullanarak eşzamanlılığı yönetme</a></td> 
</tr> 
<tr> 
<td>Varlıkları sorgulayın</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">.NET’te Azure Tablo Depolama Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Sorgu tabloları</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">.NET’te Azure Tablo Depolama Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Tablo ACL/özellikleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">.NET’te Azure Tablo Depolama Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Varlık güncelleştir</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure depolama - örnek uygulaması kullanarak eşzamanlılığı yönetme</a></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a>Azure Kod Örnekleri Kitaplığı

Tam örnek kitaplığı görüntülemek için Git [Azure Kod örnekleri](https://azure.microsoft.com/resources/samples/?service=storage) indirip yerel olarak çalıştırmak için Azure depolama örnekleri içeren bir kitaplık. Kod örneği kitaplığı .zip biçimli örnek kodda sağlar. Alternatif olarak, göz atabilir ve her örnek için GitHub deposunu kopyalayın.

[!INCLUDE [storage-dotnet-samples-include](../../../includes/storage-dotnet-samples-include.md)]

## <a name="getting-started-guides"></a>Başlarken kılavuzları

Yükleme ve Azure depolama istemci kitaplıkları ile çalışmaya başlama konusunda yönergeler arıyorsanız aşağıdaki kılavuzlara denetleyin.

* [. NET'te Azure Blob hizmetini kullanmaya başlama](../blobs/storage-dotnet-how-to-use-blobs.md)
* [. NET'te Azure kuyruk hizmeti kullanmaya başlama](../storage-dotnet-how-to-use-queues.md)
* [. NET'te Azure tablo hizmeti kullanmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [. NET'te Azure dosya hizmeti ile çalışmaya başlama](../storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a>Sonraki adımlar

Diğer diller için örnekleri hakkında daha fazla bilgi için:

* Java: [Java kullanan Azure Depolama örnekleri](storage-samples-java.md)
* Tüm diğer diller için: [Azure depolama örnekleri](../storage-samples.md)
