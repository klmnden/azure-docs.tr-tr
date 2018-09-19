---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/13/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: fb349c9ea2178d84e367e763797d5d93d3ae4c53
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46299131"
---
Aşağıdaki tabloda, Azure depolama için varsayılan sınırlara açıklanmaktadır. *Giriş* sınırı bir depolama hesabına gönderilen tüm verileri (istekler) ifade eder. *Çıkış* sınırı bir depolama hesabından alınan tüm verileri (yanıtlar) ifade eder.

| Kaynak | Varsayılan Sınır |
| --- | --- |
| Standart ve premium hesapları dahil olmak üzere, abonelik başına bölgeye göre depolama hesabı sayısı | 200 |
| En fazla depolama hesabı kapasitesi | 500 TiB |
| Blob kapsayıcıları, blobları, dosya paylaşımları, tablolar, kuyruklar, varlıklar veya iletileri depolama hesabı başına en fazla sayısı | Sınırsız |
| Depolama hesabı başına en fazla istek hızı | saniyede 20.000 istekleri |
| Depolama hesabı (ABD bölgeleri) başına en fazla giriş | RA-GRS/GRS etkinleştirilirse, LRS/ZRS için 20 GB/sn, 10 Gbps<sup>1</sup> |
| Depolama hesabı (ABD bölgeleri) başına en fazla çıkışı | 50 GB/sn |
| Depolama hesabı (ABD dışı bölgeler) başına en fazla giriş | RA-GRS/GRS etkinleştirilirse, LRS/ZRS için 10 GB/sn 5 GB/sn<sup>1</sup> |
| Depolama hesabı (ABD dışı bölgeler) başına en fazla çıkışı | 50 GB/sn |

<sup>1</sup>[azure depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy) Seçenekler şunlardır:
* **RA-GRS**: Okuma erişimli coğrafi olarak yedekli depolama. RA-GRS etkinleştirilirse, ikincil konumdaki çıkış hedeflerini birincil konumu olarak aynı olan.
* **GRS**: coğrafi olarak yedekli depolama. 
* **ZRS**: bölgesel olarak yedekli depolama.
* **LRS**: yerel olarak yedekli depolama. 

