---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/11/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ee9b70827c19236287f81f66f4b9c6e1004a54fd
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59532750"
---
Aşağıdaki tabloda, Azure genel amaçlı v1, v2 ve Blob Depolama hesapları için varsayılan sınırlara açıklanmaktadır. *Giriş* sınırı bir depolama hesabına gönderilen istekleri gelen tüm verilere başvurur. *Çıkış* sınırı tüm verileri bir depolama hesabından alınan yanıtlardan başvuruyor.

| Kaynak | Varsayılan limit |
| --- | --- |
| Standart ve premium hesapları dahil olmak üzere, abonelik başına bölgeye göre depolama hesabı sayısı | 250 |
| En fazla depolama hesabı kapasitesi | 2 PB Amerika ve Avrupa'da, 500 UK içeren TB için diğer tüm bölgeler için |
| Blob kapsayıcıları, blobları, dosya paylaşımları, tablolar, kuyruklar, varlıklar veya depolama hesabı başına ileti sayısı | Sınırsız |
| İstek hızı üst sınırı<sup>1</sup> depolama hesabı başına | saniyede 20.000 istekleri |
| En büyük giriş<sup>1</sup> (ABD bölgelerinde) depolama hesabı başına | RA-GRS/GRS etkinse, 10 GB/sn, LRS/ZRS için 20 GB/sn<sup>2</sup> |
| En büyük giriş<sup>1</sup> (ABD dışı bölgeler) depolama hesabı başına | RA-GRS/GRS etkinse, 5 GB/sn, LRS/ZRS için 10 GB/sn<sup>2</sup> |
| En fazla çıkışı için genel amaçlı v2 ve Blob Depolama hesapları (tüm bölge) | 50 Gbps |
| En fazla çıkışı için genel amaçlı v1 depolama hesaplarında (ABD bölgelerinde) | RA-GRS/GRS etkinse, 20 GB/sn, LRS/ZRS için 30 GB/sn<sup>2</sup> |
| En fazla çıkışı için genel amaçlı v1 depolama hesaplarında (ABD dışı bölgeler) | RA-GRS/GRS etkinse, 10 GB/sn, LRS/ZRS için 15 GB/sn<sup>2</sup> |

<sup>1</sup>azure standart depolama hesapları için giriş sınırları daha yüksek isteğiyle destekler. Hesabı sınırları girişi için artış istemek için başvurun [Azure Destek](https://azure.microsoft.com/support/faq/).

<sup>2</sup> [azure depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy) Seçenekler şunlardır:
* **RA-GRS**: Okuma erişimli coğrafi olarak yedekli depolama. RA-GRS etkinleştirilirse, ikincil konumdaki çıkış hedeflerini birincil konumu olarak aynı olan.
* **GRS**: Coğrafi olarak yedekli depolama. 
* **ZRS**: Bölgesel olarak yedekli depolama.
* **LRS**: Yerel olarak yedekli depolama. 

> [!NOTE]
> Çoğu senaryo için bir genel amaçlı v2 depolama hesabı kullanmanızı öneririz. Genel amaçlı v2 hesabına kapalı kalma süresi olmadan ve verileri kopyalamak zorunda kalmadan kolayca bir genel amaçlı v1 veya bir Azure Blob Depolama hesabına yükseltebilirsiniz.
>
> Azure depolama hesapları hakkında daha fazla bilgi için bkz. [depolama hesabına genel bakışın](../articles/storage/common/storage-account-overview.md). 

Uygulamanızın ihtiyaçlarını tek bir depolama hesabı ölçeklenebilirlik hedefleri aşarsanız, birden çok depolama hesaplarını kullanmak için uygulamanızı oluşturabilirsiniz. Ardından, bu depolama hesabı arasında veri nesnelerinizi bölümleyebilirsiniz. Birim fiyatlandırma hakkında daha fazla bilgi için bkz: [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

Tüm depolama hesapları, bir düz ağ topolojisi üzerine çalıştırın ve oluşturuldukları zaman bağımsız olarak, bu makalede açıklanan ölçeklenebilirlik ve performans hedefleri destekler. Ölçeklenebilirlik ve Azure depolama düz ağ mimarisi üzerinde daha fazla bilgi için bkz: [Microsoft Azure Depolama: Güçlü tutarlılık ile yüksek oranda kullanılabilir bulut depolama hizmeti](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).

