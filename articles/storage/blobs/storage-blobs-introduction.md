---
title: Blob depolamaya giriş - Azure’da nesne depolama | Microsoft Docs
description: Azure Blob depolama, metin veya ikili veri gibi çok miktarda yapılandırılmamış nesne verilerini depolamak için tasarlanmıştır. Uygulamalarınız, REST üzerinden veya Azure Storage istemci kitaplıkları aracılığıyla koddan, Azure CLI’dan ya da PowerShell’den Blob depolamadaki nesnelere erişebilir.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: overview
ms.date: 03/27/2018
ms.author: tamram
ms.openlocfilehash: 0fff0032ec2452413bcd1df3175634b14a64208f
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="introduction-to-blob-storage"></a>Blob depolamaya giriş

Azure Blob depolama, Microsoft’un veri nesnelerine yönelik bulut depolama çözümüdür. Blob depolama, metin veya ikili veri gibi çok miktarda yapılandırılmamış nesne verilerini depolayabilir. Blob depolamadaki verilere, dünyanın her yerinden HTTP veya HTTPS üzerinden erişilebilir. Verileri genel olarak herkese açık kullanıma sunmak veya uygulama verilerini özel olarak depolamak için Blob Storage’ı kullanabilirsiniz.

Blob Storage’ın yaygın kullanımları şunlardır:

* Görüntülerin veya belgelerin doğrudan bir tarayıcıya sunulması
* Dağıtılan erişim için dosyaların depolanması
* Video ve ses akışları
* Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için depolama
* Şirket içi veya Azure barındırılan hizmetle analiz için verilerin depolanması
* Azure Sanal Makineleri ile kullanmak için VHD’leri depolama

## <a name="blob-service-concepts"></a>Blob hizmeti kavramları

Blob hizmetinde şu bileşenler bulunur:

![Blob mimarisi](./media/storage-blobs-introduction/blob1.png)

* **Depolama Hesabı:** Tüm Azure Storage erişimi bir depolama hesabıyla yapılır. Bu depolama hesabı, **Genel amaçlı depolama hesabı (v1 veya v2)** ya da **Blob depolama hesapları** olabilir. Daha fazla bilgi için bkz. [Azure depolama hesapları hakkında](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

* **Kapsayıcı:** Kapsayıcı, bir dizi blobun gruplandırılmasını sağlar. Tüm bloblar bir kapsayıcıda olmalıdır. Bir hesapta sınırsız sayıda kapsayıcı olabilir. Kapsayıcıda sınırsız sayıda blob depolanabilir. Kapsayıcı adındaki harflerin küçük harf olması gerektiğini unutmayın.

* **Blob:** Herhangi bir türde ve boyutta bir dosya. Azure Depolama üç tür blob sunar: blok blobları, [sayfa blobları](storage-blob-pageblob-overview.md) ve ekleme blobları.
  
    *Blok blobları*, belgeler ve medya dosyaları gibi metin veya ikili dosyaların depolanması için idealdir. *Ekleme blobları* blok bloblarına benzer; bloklardan oluşturulmuş olsalar da ekleme işlemleri için iyileştirilmişlerdir; bu nedenle, günlük kaydı senaryoları için kullanışlıdırlar. Tek bir blok blobu, her birinin büyüklüğü 100 MB’a kadar olabilen 50.000 blok içerebilir; toplam boyut 4,75 TB'tan biraz fazladır (100 MB X 50.000). Tek bir ekleme blobu, her birinin büyüklüğü 4 MB’a kadar olabilen 50.000 blok içerebilir; toplam boyut 195 GB'tan biraz fazladır (4 MB X 50.000).
  
    *Sayfa blobları* boyut olarak 8 TB’ye kadar olabilir; sık gerçekleştirilen okuma/yazma işlemleri için daha verimlidir. Azure Virtual Machines sayfa bloblarını işletim sistemi ve veri diskleri olarak kullanır.
  
    Kapsayıcıları ve blobları adlandırma hakkında ayrıntılı bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama hesabı oluşturma](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [.NET ile Blob depolamayı kullanmaya başlama](storage-dotnet-how-to-use-blobs.md)
* [.NET kullanan Azure Depolama örnekleri](../common/storage-samples-dotnet.md)
* [Java kullanan Azure Depolama örnekleri](../common/storage-samples-java.md)
