---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/23/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 84333b26ac70db4b400f7236d4255f4b57a6ca7d
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572862"
---
Aşağıdaki tabloda, Azure depolama için varsayılan sınırlara açıklanmaktadır. *Giriş* sınırı bir depolama hesabına gönderilen tüm verileri (istekler) ifade eder. *Çıkış* sınırı bir depolama hesabından alınan tüm verileri (yanıtlar) ifade eder.

| Kaynak | Varsayılan Sınır |
| --- | --- |
| Standart ve premium hesapları dahil olmak üzere, abonelik başına bölgeye göre depolama hesabı sayısı | 250 |
| En fazla depolama hesabı kapasitesi | 2 PB ABD ve Avrupa, UK dahil olmak üzere diğer tüm bölgeler için 500 TB için |
| Blob kapsayıcıları, blobları, dosya paylaşımları, tablolar, kuyruklar, varlıklar veya iletileri depolama hesabı başına en fazla sayısı | Sınırsız |
| İstek hızı üst sınırı<sup>1</sup> depolama hesabı başına | saniyede 20.000 istekleri |
| En büyük giriş<sup>1</sup> (ABD bölgeleri) depolama hesabı başına | RA-GRS/GRS etkinleştirilirse, LRS/ZRS için 20 GB/sn, 10 Gbps<sup>2</sup> |
| En büyük giriş<sup>1</sup> (ABD dışı bölgeler) depolama hesabı başına | RA-GRS/GRS etkinleştirilirse, LRS/ZRS için 10 GB/sn 5 GB/sn<sup>2</sup> |
| En fazla çıkışı için genel amaçlı v2 ve Blob Depolama hesapları (tüm bölge) | 50 GB/sn |
| Genel amaçlı v1 depolama hesapları (ABD bölgeleri) için en fazla çıkışı | RA-GRS/GRS etkinleştirilirse, 30 GB/sn LRS/ZRS için 20 GB/sn <sup>2</sup> |
| Genel amaçlı v1 depolama hesapları (ABD dışı bölgeler) için en fazla çıkışı | RA-GRS/GRS etkinleştirilirse, 15 GB/sn LRS/ZRS için 10 GB/sn <sup>2</sup> |

<sup>1</sup> azure depolama hesapları için giriş sınırları daha yüksek isteğiyle destekler. Hesabı sınırları girişi için artış istemek için başvurun [Azure Destek](https://azure.microsoft.com/support/faq/).

<sup>2</sup>[azure depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy) Seçenekler şunlardır:
* **RA-GRS**: Okuma erişimli coğrafi olarak yedekli depolama. RA-GRS etkinleştirilirse, ikincil konumdaki çıkış hedeflerini birincil konumu olarak aynı olan.
* **GRS**: coğrafi olarak yedekli depolama. 
* **ZRS**: bölgesel olarak yedekli depolama.
* **LRS**: yerel olarak yedekli depolama. 

Uygulamanızın ihtiyaçlarını tek bir depolama hesabı ölçeklenebilirlik hedefleri aşarsanız, birden çok depolama hesaplarını kullanmak için uygulamanızı oluşturabilirsiniz. Ardından, bu depolama hesabı arasında veri nesnelerinizi bölümleyebilirsiniz. Bkz: [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) birim fiyatlandırma hakkında bilgi için.

Tüm depolama hesapları, bir düz ağ topolojisi üzerine çalıştırın ve oluşturuldukları zaman bağımsız olarak, bu makalede açıklanan ölçeklenebilirlik ve performans hedefleri destekler. Ölçeklenebilirlik ve Azure depolama düz ağ mimarisi üzerinde daha fazla bilgi için bkz: [Microsoft Azure Depolama: yüksek oranda kullanılabilir bulut depolama hizmet güçlü tutarlılıkla](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).

