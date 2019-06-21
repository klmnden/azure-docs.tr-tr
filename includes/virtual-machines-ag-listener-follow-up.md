---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 8ae22ec7f75b9cd0d7958977dfd97169706c9389
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188318"
---
Kullanılabilirlik grubu dinleyicisi oluşturduktan sonra dinleyici kaynak RegisterAllProvidersIP ve HostRecordTTL küme parametrelerini ayarlamak gerekli olabilir. Bu parametreler, yeniden bağlanma zaman aşımları engelleyebilecek bir yük devretme sonrasında azaltabilir. Bu parametreleri yanı sıra örnek kod hakkında daha fazla bilgi için bkz: [oluşturun veya bir kullanılabilirlik grubu dinleyicisi yapılandırma](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).

