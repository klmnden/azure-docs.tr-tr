---
title: include dosyası
description: include dosyası
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 08/30/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: fe1227d91c0d039a94e5b9a3046b879f4f17355a
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56247204"
---
| Kaynak | Temel | Standart | Premium |
|---|---|---|---|---|
| Depolama<sup>1</sup> | 10 GiB | 100 GiB| 500 giB |
| En büyük görüntü katman boyutu | 20 GiB | 20 GiB | 50 giB |
| Dakika başına okuma işlemleri:<sup>2, 3</sup> | 1000 | 3.000 | 10,000 |
| Yazma işlemleri: dakika başına<sup>2, 4</sup> | 100 | 500 | 2,000 |
| MB/sn bant genişliği indirme<sup>2</sup> | 30 | 60 | 100 |
| MB/sn bant genişliği karşıya<sup>2</sup> | 10 | 20 | 50 |
| Web Kancaları | 2 | 10 | 100 |
| Coğrafi çoğaltma | Yok | Yok | [Desteklenen][geo-replication] |
| İçerik güveni (önizleme) | Yok | Yok | [Desteklenen][content-trust] |

<sup>1</sup> miktarı belirtilen depolama sınırları olan *dahil* her katman için depolama. Bir ek bir günlük fiyat GiB başına bu sınırların üzerinde resim depolama için ücret ödersiniz. Hızı için bilgi [kapsayıcı kayıt defteri fiyatlandırması][pricing].

<sup>2</sup> *okuma işlemleri:*, *yazma işlemleri:*, ve *bant genişliği* minimum biriminizdeki tahmini fiyatlardır. ACR kullanımı gerektirdiğinden performansı üstlenmeye çalışır.

<sup>3</sup> [docker isteği](https://docs.docker.com/registry/spec/api/#pulling-an-image) görüntünün yanı sıra, bildirim alma katmanlarında sayısına bağlı olarak birden çok okuma işlemleri için çevirir.

<sup>4</sup> [docker itme](https://docs.docker.com/registry/spec/api/#pushing-an-image) itilecek gerekir katmanları sayısına göre birden fazla yazma işlemleri için çevirir. A `docker push` içerir *okuma işlemleri:* varolan bir görüntü için bir bildirim almak için.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
