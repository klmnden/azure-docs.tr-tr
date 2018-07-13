---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/03/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 6572adb0d8d629910492603a17988b89acce2f17
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38756034"
---
| Kaynak | Varsayılan Sınır |
| --- | --- |
| Her Abonelikteki bölge başına depolama hesabı sayısı | 200<sup>1</sup> |
| En fazla depolama hesabı kapasitesi | 500 TiB<sup>2</sup> |
| Blob kapsayıcıları, blobları, dosya paylaşımları, tablolar, kuyruklar, varlıklar veya iletileri depolama hesabı başına en fazla sayısı | Sınırsız |
| Depolama hesabı başına en fazla istek hızı | saniyede 20.000 istek<sup>2</sup> |
| En büyük giriş<sup>3</sup> (ABD bölgeleri) depolama hesabı başına | RA-GRS/GRS etkinleştirilirse, LRS/ZRS için 20 GB/sn, 10 Gbps<sup>4</sup> |
| En büyük çıkış<sup>3</sup> (ABD bölgeleri) depolama hesabı başına | RA-GRS/GRS etkinleştirilirse, 30 GB/sn LRS/ZRS için 20 GB/sn<sup>4</sup> |
| En büyük giriş<sup>3</sup> (ABD dışı bölgeler) depolama hesabı başına | RA-GRS/GRS etkinleştirilirse, LRS/ZRS için 10 GB/sn 5 GB/sn<sup>4</sup> |
| En büyük çıkış<sup>3</sup> (ABD dışı bölgeler) depolama hesabı başına | RA-GRS/GRS etkinleştirilirse, 15 GB/sn LRS/ZRS için 10 GB/sn<sup>4</sup> |

<sup>1</sup>hem standart hem de Premium depolama hesapları dahildir. Belirli bir bölgede 200'den fazla depolama hesabı gerekiyorsa, bir istekte aracılığıyla [Azure Destek](https://azure.microsoft.com/support/faq/). Azure depolama ekibi, işinizin durumunu inceler ve 250 depolama hesapları için belirli bir bölgeye kadar onaylayabilir. 

<sup>2</sup> depolama hesabınız için genişletilmiş limitlere ihtiyacınız varsa lütfen şuraya başvurun [Azure Destek](https://azure.microsoft.com/support/faq/). Azure depolama ekibi isteği inceler ve olay temelinde sınırları daha yüksek onaylayabilir. Hem genel amaçlı ve Blob Depolama hesapları, istek tarafından artan kapasite, giriş/çıkış ve istek hızı destekler. Yeni üst sınırlar için Blob Depolama hesapları için bkz: [daha büyük, daha yüksek ölçek depolama hesapları ile tanışın](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).

<sup>3</sup> yalnızca hesabın giriş/çıkış sınırları göre ücret alınır. *Giriş* bir depolama hesabına gönderilen tüm verileri (istekler) ifade eder. *Çıkış*, bir depolama hesabından alınan tüm verileri (yanıtlar) ifade eder.  

<sup>4</sup>azure depolama yedekliliği seçenekleri şunlardır:
* **RA-GRS**: Okuma erişimli coğrafi olarak yedekli depolama. RA-GRS etkinleştirilirse, ikincil konumdaki çıkış hedeflerini birincil konumu olarak aynı olan.
* **GRS**: coğrafi olarak yedekli depolama. 
* **ZRS**: bölgesel olarak yedekli depolama.
* **LRS**: yerel olarak yedekli depolama. 
