---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/21/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ad4b244b58d741ad45463297df5bd358f3ae9918
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58450148"
---
Bir RBAC rolü için bir güvenlik sorumlusu atamadan önce güvenlik sorumlusu olması gereken erişim kapsamını belirleyin. En iyi uygulamalar, her zaman yalnızca olası en dar kapsamdan vermek en iyi olduğunu gerektirir.

Aşağıdaki listede, Azure blob ve kuyruk kaynaklarına erişimi dar kapsamlı başlatma kapsamını sınırlandırabilirsiniz düzeyleri açıklanmaktadır:

- **Tek bir kapsayıcı.** Bu kapsamda bir güvenlik sorumlusu tüm blobların kapsayıcı, hem de kapsayıcı özellikleri ve meta veri erişimi vardır.
- **Tek bir kuyruk.** Bu kapsamda bir güvenlik sorumlusu iletileri kuyruğa yanı sıra özellikleri ve meta verileri erişebilir.
- **Depolama hesabı.** Bu kapsamda bir güvenlik sorumlusu tüm kapsayıcılar ve bunların bloblar veya tüm sıralarının ve iletilerinin erişimi vardır.
- **Kaynak grubu.** Bu kapsamda bir güvenlik sorumlusu kapsayıcıları veya kaynak grubundaki tüm depolama hesaplarını kuyruklarda hepsine erişebilir.
- **Abonelik.** Bu kapsamda bir güvenlik sorumlusu kapsayıcıları veya tüm depolama hesaplarını Abonelikteki kaynak gruplarının tüm kuyruklarda tüm erişebilir.
