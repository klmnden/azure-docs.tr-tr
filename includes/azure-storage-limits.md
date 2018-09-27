---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/25/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a0a777151216cb70b696da088b8a0d34779ebc39
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47396144"
---
Aşağıdaki tabloda, Azure depolama için varsayılan sınırlara açıklanmaktadır. *Giriş* sınırı bir depolama hesabına gönderilen tüm verileri (istekler) ifade eder. *Çıkış* sınırı bir depolama hesabından alınan tüm verileri (yanıtlar) ifade eder.

| Kaynak | Varsayılan Sınır |
| --- | --- |
| Standart ve premium hesapları dahil olmak üzere, abonelik başına bölgeye göre depolama hesabı sayısı | 200 |
| En fazla depolama hesabı kapasitesi<sup>1</sup> | 500 TiB |
| Blob kapsayıcıları, blobları, dosya paylaşımları, tablolar, kuyruklar, varlıklar veya iletileri depolama hesabı başına en fazla sayısı | Sınırsız |
| İstek hızı üst sınırı<sup>1</sup> depolama hesabı başına | saniyede 20.000 istekleri |
| En büyük giriş<sup>1</sup> (ABD bölgeleri) depolama hesabı başına | RA-GRS/GRS etkinleştirilirse, LRS/ZRS için 20 GB/sn, 10 Gbps<sup>2</sup> |
| En büyük çıkış<sup>1</sup> (ABD bölgeleri) depolama hesabı başına | RA-GRS/GRS etkinleştirilirse, 30 GB/sn LRS/ZRS için 20 GB/sn |
| En büyük giriş<sup>1</sup> (ABD dışı bölgeler) depolama hesabı başına | RA-GRS/GRS etkinleştirilirse, LRS/ZRS için 10 GB/sn 5 GB/sn<sup>2</sup> |
| En büyük çıkış<sup>1</sup> (ABD dışı bölgeler) depolama hesabı başına | RA-GRS/GRS etkinleştirilirse, 15 GB/sn LRS/ZRS için 10 GB/sn |

<sup>1</sup> azure depolama hesaplarının desteklediği sınırları daha yüksek kapasite, istek hızı, giriş ve çıkış için istek tarafından. Artırılmış sınırlar hakkında daha fazla bilgi için bkz: [daha büyük, daha yüksek ölçek depolama hesapları ile tanışın](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/). Hesabı sınırları artış istemek için başvurun [Azure Destek](https://azure.microsoft.com/support/faq/).

<sup>2</sup>[azure depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy) Seçenekler şunlardır:
* **RA-GRS**: Okuma erişimli coğrafi olarak yedekli depolama. RA-GRS etkinleştirilirse, ikincil konumdaki çıkış hedeflerini birincil konumu olarak aynı olan.
* **GRS**: coğrafi olarak yedekli depolama. 
* **ZRS**: bölgesel olarak yedekli depolama.
* **LRS**: yerel olarak yedekli depolama. 

