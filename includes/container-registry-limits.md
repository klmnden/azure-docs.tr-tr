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
ms.openlocfilehash: 92a5d162e7a0b2c752a2f8e9c5941edf43e539e3
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991092"
---
| Kaynak | Temel | Standart | Premium |
|---|---|---|---|---|
| Depolama | 10 giB | 100 giB| 500 giB |
| En büyük görüntü katman boyutu | 20 giB | 20 giB | 50 giB |
| Dakika başına okuma işlemleri:<sup>1, 2</sup> | 1000 | 3.000 | 10,000 |
| Yazma işlemleri: dakika başına<sup>1, 3</sup> | 100 | 500 | 2,000 |
| MB/sn bant genişliği indirme<sup>1</sup> | 30 | 60 | 100 |
| MB/sn bant genişliği karşıya<sup>1</sup> | 10 | 20 | 50 |
| Web Kancaları | 2 | 10 | 100 |
| Coğrafi çoğaltma | Yok | Yok | [Destekleniyor](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *okuma işlemleri:*, *yazma işlemleri:*, ve *bant genişliği* minimum biriminizdeki tahmini fiyatlardır. ACR kullanımı gerektirdiğinden performansı üstlenmeye çalışır.

<sup>2</sup> [docker isteği](https://docs.docker.com/registry/spec/api/#pulling-an-image) görüntünün yanı sıra, bildirim alma katmanlarında sayısına bağlı olarak birden çok okuma işlemleri için çevirir.

<sup>3</sup> [docker itme](https://docs.docker.com/registry/spec/api/#pushing-an-image) itilecek gerekir katmanları sayısına göre birden fazla yazma işlemleri için çevirir. A `docker push` içerir *okuma işlemleri:* varolan bir görüntü için bir bildirim almak için.
