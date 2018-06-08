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
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34852067"
---
| Kaynak | Varsayılan Sınır |
| --- | --- |
| Depolama hesabı her Abonelikteki bölge başına sayısı | 200<sup>1</sup> |
| En fazla depolama hesabı kapasitesi | 500 Tıb<sup>2</sup> |
| En fazla blob kapsayıcıları, BLOB'lar, dosya paylaşımları, tablolar, kuyruklar, varlık veya depolama hesabı başına ileti sayısı | Sınırsız |
| Depolama hesabı başına en fazla istek oranı | saniye başına 20.000 istek<sup>2</sup> |
| En fazla giriş<sup>3</sup> her depolama hesabı (BİZE bölgelerde) | RA-GRS/GRS etkinleştirilirse, 20 GB/sn LRS/ZRS için 10 GB/sn<sup>4</sup> |
| En büyük çıkış<sup>3</sup> her depolama hesabı (BİZE bölgelerde) | RA-GRS/GRS etkinleştirilirse, LRS/ZRS için 30 GB/sn 20 GB/sn<sup>4</sup> |
| En fazla giriş<sup>3</sup> her depolama hesabı (ABD olmayan bölgeleri için) | RA-GRS/GRS etkinleştirilirse, 10 GB/sn LRS/ZRS için 5 GB/sn<sup>4</sup> |
| En büyük çıkış<sup>3</sup> her depolama hesabı (ABD olmayan bölgeleri için) | RA-GRS/GRS etkinleştirilirse, 15 GB/sn LRS/ZRS için 10 GB/sn<sup>4</sup> |

<sup>1</sup>standart ve Premium depolama hesaplarını içerir. Belirli bir bölgedeki 200'den fazla depolama hesaplarında gerekiyorsa, bir istekte aracılığıyla [Azure Destek](https://azure.microsoft.com/support/faq/). Azure depolama ekibi iş durumunuz incelenecek ve belirli bir bölgedeki 250 depolama hesapları kadar onaylama. 

<sup>2</sup> depolama hesabınız için genişletilmiş sınırları gerekiyorsa, lütfen başvurun [Azure Destek](https://azure.microsoft.com/support/faq/). Azure depolama ekibi istekleri incelemeli ve olay temelinde daha yüksek sınırları onaylama. Her ikisi de genel amaçlı ve Blob storage hesapları isteğiyle desteği artan kapasite, giriş/çıkış ve istek oranı. Yeni üst sınırlar için Blob storage hesapları için bkz: [daha büyük ve daha yüksek ölçek depolama hesapları Duyurusu](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).

<sup>3</sup> yalnızca hesabın giriş/çıkış sınırları tarafından tutulabilir. *Giriş* bir depolama hesabına gönderilen tüm veriler (istek) başvuruyor. *Çıkış*, bir depolama hesabından alınan tüm verileri (yanıtlar) ifade eder.  

<sup>4</sup>azure Depolama artıklık seçenekleri:
* **RA-GRS**: coğrafi olarak yedekli depolamaya okuma erişimi. RA-GRS etkinleştirilirse, çıkış hedefler ve ikincil konum için birincil konum olanlarla aynıdır.
* **GRS**: coğrafi olarak yedekli depolama. 
* **ZRS**: bölge olarak yedekli depolama.
* **LRS**: yerel olarak yedekli depolama. 
