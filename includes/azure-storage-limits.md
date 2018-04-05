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
ms.openlocfilehash: 6381f8f0e68853183fc3e17e76b4ab93b152b48b
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
| Kaynak | Varsayılan Sınır |
| --- | --- |
| Depolama hesabı her bölge sayısı | 200<sup>1</sup> |
| En fazla depolama hesabı kapasitesi | 500 TiB<sup>2</sup> |
| En fazla blob kapsayıcıları, BLOB'lar, dosya paylaşımları, tablolar, kuyruklar, varlık veya depolama hesabı başına ileti sayısı | Sınırsız |
| Depolama hesabı başına en fazla istek oranı | saniye başına 20.000 istek<sup>2</sup> |
| En fazla giriş<sup>3</sup> her depolama hesabı (BİZE bölgelerde) | RA-GRS/GRS etkinleştirilirse, 20 GB/sn LRS/ZRS için 10 GB/sn<sup>4</sup> |
| En büyük çıkış<sup>3</sup> her depolama hesabı (BİZE bölgelerde) | RA-GRS/GRS etkinleştirilirse, LRS/ZRS için 30 GB/sn 20 GB/sn<sup>4</sup> |
| En fazla giriş<sup>3</sup> her depolama hesabı (ABD olmayan bölgeleri için) | RA-GRS/GRS etkinleştirilirse, 10 GB/sn LRS/ZRS için 5 GB/sn<sup>4</sup> |
| En büyük çıkış<sup>3</sup> her depolama hesabı (ABD olmayan bölgeleri için) | RA-GRS/GRS etkinleştirilirse, 15 GB/sn LRS/ZRS için 10 GB/sn<sup>4</sup> |

<sup>1</sup>standart ve Premium depolama hesaplarını içerir. 200'den fazla depolama hesabı gerekiyorsa, [Azure Destek](https://azure.microsoft.com/support/faq/) üzerinden bir istek oluşturun. Azure Depolama ekibi, işinizin durumunu inceler ve 250’ye kadar depolama hesabı için onay verebilir. 

<sup>2</sup> depolama hesabınız için genişletilmiş sınırları gerekiyorsa, lütfen başvurun [Azure Destek](https://azure.microsoft.com/support/faq/). Azure depolama ekibi istekleri incelemeli ve olay temelinde daha yüksek sınırları onaylama. Her ikisi de genel amaçlı ve Blob storage hesapları isteğiyle desteği artan kapasite, giriş/çıkış ve istek oranı. Yeni üst sınırlar için Blob storage hesapları için bkz: [daha büyük ve daha yüksek ölçek depolama hesapları Duyurusu](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).

<sup>3</sup> yalnızca hesabın giriş/çıkış sınırları tarafından tutulabilir. *Giriş* bir depolama hesabına gönderilen tüm veriler (istek) başvuruyor. *Çıkış*, bir depolama hesabından alınan tüm verileri (yanıtlar) ifade eder.  

<sup>4</sup>azure Depolama artıklık seçenekleri:
* **RA-GRS**: coğrafi olarak yedekli depolamaya okuma erişimi. RA-GRS etkinleştirilirse, çıkış hedefler ve ikincil konum için birincil konum olanlarla aynıdır.
* **GRS**: coğrafi olarak yedekli depolama. 
* **ZRS**: bölge olarak yedekli depolama.
* **LRS**: yerel olarak yedekli depolama. 
