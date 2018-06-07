---
title: include dosyası
description: include dosyası
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 05/29/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 942b9bdf0201acaefe3333bcf928772899b9bdc2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34665097"
---
| Kaynak | Temel | Standart | Premium |
|---|---|---|---|---|
| Depolama | 10 Gib'den | 100 Gib'den| 500 Gib'den |
| En büyük görüntü katman boyutu | 20 Gib'den | 20 Gib'den | 50 Gib'den |
| Okuma işlemleri: dakika başına<sup>1, 2</sup> | 1000 | 3000 | 10000 |
| Yazma işlemleri: dakika başına<sup>1, 3</sup> | 100 | 500 | 2000 |
| MB/sn bant genişliği karşıdan<sup>1</sup> | 30 | 60 | 100 |
| MB/sn bant genişliği karşıya<sup>1</sup> | 10 | 20 | 50 |
| Web Kancaları | 2 | 10 | 100 |
| Coğrafi çoğaltma | Yok | Yok | [Destekleniyor](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *okuma işlemleri:*, *yazma işlemleri:*, ve *bant genişliği* minimum tahminleri. Kullanım gerektirdiğinden performansı artırmak için ACR çalışır.

<sup>2</sup> [docker çekme](https://docs.docker.com/registry/spec/api/#pulling-an-image) katmanları görüntü yanı sıra bildirim alma sayısına dayalı olarak birden çok okuma işlemleri için çevirir.

<sup>3</sup> [docker itme](https://docs.docker.com/registry/spec/api/#pushing-an-image) gönderilen gerekir Katmanlar sayısına göre birden çok yazma işlemleri için çevirir. A `docker push` içeren *okuma işlemleri:* varolan bir görüntü yönelik bir bildirim almak için.