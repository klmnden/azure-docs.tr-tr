---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/28/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 518c57bc3327511b70deef143826f2a1b9df8639
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66115588"
---
Bir RBAC rolü için bir güvenlik sorumlusu atamadan önce güvenlik sorumlusu olması gereken erişim kapsamını belirleyin. En iyi uygulamalar, her zaman yalnızca olası en dar kapsamdan vermek en iyi olduğunu gerektirir.

Aşağıdaki listede, Azure blob ve kuyruk kaynaklarına erişimi dar kapsamlı başlatma kapsamını sınırlandırabilirsiniz düzeyleri açıklanmaktadır:

- **Tek bir kapsayıcı.** Bu kapsamda bir rol ataması tüm blobların kapsayıcı, hem de kapsayıcı özellikleri ve meta veriler için geçerlidir.
- **Tek bir kuyruk.** Bu kapsamda bir rol ataması iletileri kuyruğa yanı sıra özellikleri ve meta verileri için geçerlidir.
- **Depolama hesabı.** Bu kapsamda bir rol ataması, tüm kapsayıcılar ve bunların bloblar veya tüm sıralarının ve iletilerinin geçerlidir.
- **Kaynak grubu.** Bu kapsamda bir rol ataması tüm kuyruklarda tüm depolama hesaplarını kaynak grubunda ve kapsayıcılar için geçerlidir.
- **Abonelik.** Bu kapsamda bir rol ataması kapsayıcıları veya tüm depolama hesaplarını Abonelikteki kaynak gruplarının tüm kuyruklarda tüm geçerlidir.

> [!IMPORTANT]
> Aboneliğinize bir Azure DataBricks ad alanı varsa, abonelik kapsamında atanan rollerin, blob ve kuyruk veri erişim engellenir.
