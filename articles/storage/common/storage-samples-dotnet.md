---
title: .NET kullanarak azure depolama örnekleri | Microsoft Docs
description: Görüntülemek, indirin ve örnek kod ve uygulamaları için Azure Storage çalıştırın. BLOB, kuyruklar, tablolar ve dosyaları için .NET depolama istemcisi kitaplıklarını kullanarak örnek Başlarken bulur.
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: ''
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.openlocfilehash: 1e6973f0decc448657d869afb8823dd03c62d272
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-storage-samples-using-net"></a>.NET kullanarak azure depolama örnekleri

## <a name="net-sample-index"></a>.NET örnek dizini

Aşağıdaki tabloda bizim örnek depo ve her örnek kapsamdaki senaryolar genel bakış sağlar. Github'da karşılık gelen örnek kod görüntülemek için bağlantıları tıklatın.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Uç Nokta</th><th style="font-size:110%">Senaryo</th><th style="font-size:110%">Örnek Kod</th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><b>BLOB</b></td>
<td>BLOB ekleme</td> 
<td><a href="https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">CloudBlobContainer.GetAppendBlobReference yöntemi örneği</a></td> 
</tr> 
<tr> 
<td>Blok blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Depolama Fotoğraf Galerisi Web uygulaması</a></td>
</tr> 
<tr> 
<td>İstemci Tarafında Şifreleme</td>
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">BLOB şifreleme örnekleri</a></td>
</tr> 
<tr> 
<td>BLOB kopyalama</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'lar ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Kapsayıcı oluşturma</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Depolama Fotoğraf Galerisi Web uygulaması</a></td>
</tr> 
<tr> 
<td>BLOB Sil</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Depolama Fotoğraf Galerisi Web uygulaması</a></td>
</tr> 
<tr> 
<td>Kapsayıcısını silmek</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'lar ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>BLOB meta verileri/özellikler/Stats</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'lar ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Kapsayıcı ACL/meta veri/özellikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Depolama Fotoğraf Galerisi Web uygulaması</a></td>
</tr> 
<tr> 
<td>Sayfa aralıklarını alma</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'lar ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Kira Blob/kapsayıcısı</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'lar ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Liste Blob/kapsayıcısı</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">BLOB'lar ile çalışmaya başlama</a></td>
</tr> 
<tr> 
<td>Sayfa blobu</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">BLOB'lar ile çalışmaya başlama</a></td>
</tr>
<tr> 
<td>SAS</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'lar ile çalışmaya başlama</a></td>
</tr>   
<tr> 
<td>Hizmet Özellikleri</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">BLOB'lar ile çalışmaya başlama</a></td>
</tr>           
<tr> 
<td>Anlık görüntü Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Artımlı anlık görüntüleri yedekleme Azure sanal makine diskleriyle</a></td>
</tr> 
<tr> 
<td rowspan="9"><b>Dosya</b></td>
<td>Dizinleri/paylaşımları/dosyaları oluşturma</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure depolama .NET dosya depolama örnek</a></td> 
</tr>
<tr> 
<td>Dizinleri/paylaşımları/dosyaları silin</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Azure dosya hizmeti .NET kullanmaya Başlarken</a></td> 
</tr> 
<tr> 
<td>Dizin özellikleri/meta veri</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure depolama .NET dosya depolama örnek</a></td> 
</tr> 
<tr> 
<td>Dosyaları indirme</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure depolama .NET dosya depolama örnek</a></td> 
</tr> 
<tr> 
<td>Dosya özellikleri/meta veri/ölçümleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure depolama .NET dosya depolama örnek</a></td> 
</tr> 
<tr> 
<td>Dosya hizmeti özellikleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure depolama .NET dosya depolama örnek</a></td> 
</tr> 
<tr> 
<td>Liste dizinler ve dosyalar</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure depolama .NET dosya depolama örnek</a></td> 
</tr>
<tr> 
<td>Paylaşımları</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure depolama .NET dosya depolama örnek</a></td> 
</tr>
<tr> 
<td>Meta veri/özellikler/Stats paylaşma</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure depolama .NET dosya depolama örnek</a></td> 
</tr>
<tr> 
<td rowspan="8"><b>Sırası</b></td>
<td>Mesaj ekleyin.</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Azure kuyruk hizmetinde .NET kullanmaya Başlarken</a></td> 
</tr> 
<tr> 
<td>İstemci Tarafında Şifreleme</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Azure depolama .NET sıra istemci tarafı şifreleme</a></td> 
</tr> 
<tr> 
<td>Sıra oluşturma</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Azure kuyruk hizmetinde .NET kullanmaya Başlarken</a></td> 
</tr> 
<tr> 
<td>İleti/kuyruk silme</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Azure kuyruk hizmetinde .NET kullanmaya Başlarken</a></td> 
</tr> 
<tr> 
<td>İletiye Gözat</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Azure kuyruk hizmetinde .NET kullanmaya Başlarken</a></td> 
</tr> 
<tr> 
<td>Sıra ACL/meta veri/Stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Azure kuyruk hizmetinde .NET kullanmaya Başlarken</a></td> 
</tr> 
<tr> 
<td>Kuyruk hizmeti özellikleri</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Azure kuyruk hizmetinde .NET kullanmaya Başlarken</a></td> 
</tr> 
<tr> 
<td>Güncelleştirme iletisi</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Azure kuyruk hizmetinde .NET kullanmaya Başlarken</a></td> 
</tr> 
<tr> 
<td rowspan="7"><b>Tablo</b></td>
<td>Tablo Oluştur</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure Storage - örnek uygulaması kullanarak eşzamanlılık yönetme</a></td> 
</tr> 
<tr> 
<td>Varlık/tablo silme</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">.NET’te Azure Tablo Depolama Kullanmaya Başlama</a></td> 
</tr> 
<tr> 
<td>Birleştirme/Ekle/Değiştir varlık</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure Storage - örnek uygulaması kullanarak eşzamanlılık yönetme</a></td> 
</tr> 
<tr> 
<td>Sorgu varlıklar</td> 
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
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure Storage - örnek uygulaması kullanarak eşzamanlılık yönetme</a></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a>Azure Kod Örnekleri Kitaplığı

Tam örnek kitaplığını görüntülemek için Git [Azure Kod örnekleri](https://azure.microsoft.com/resources/samples/?service=storage) Azure Storage için karşıdan yükle ve yerel olarak çalıştırılan örnekleri içeren kitaplık. Kod örneği kitaplığı .zip biçimi örnek kodda sağlar. Alternatif olarak, bulun ve her bir örnek için GitHub deposunu kopyalayın.

[!INCLUDE [storage-dotnet-samples-include](../../../includes/storage-dotnet-samples-include.md)]

## <a name="getting-started-guides"></a>Başlarken kılavuzları alma

Yükleyin ve Azure Storage istemcisi kitaplıklarını ile çalışmaya başlama hakkında yönergeler arıyorsanız aşağıdaki kılavuzlara denetleyin.

* [Azure Blob hizmetine .NET kullanmaya Başlarken](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Azure kuyruk hizmetinde .NET kullanmaya Başlarken](../storage-dotnet-how-to-use-queues.md)
* [Azure tablo hizmetinde .NET kullanmaya Başlarken](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Azure dosya hizmeti .NET kullanmaya Başlarken](../storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a>Sonraki adımlar

Diğer diller için örnekleri hakkında daha fazla bilgi için:

* Java: [Java kullanarak Azure Storage örnekleri](storage-samples-java.md)
* Tüm diğer dillere: [Azure Storage örnekleri](../storage-samples.md)
