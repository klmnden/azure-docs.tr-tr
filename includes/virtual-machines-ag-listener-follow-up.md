---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 8ae22ec7f75b9cd0d7958977dfd97169706c9389
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097703"
---
Kullanılabilirlik grubu dinleyicisi oluşturduktan sonra dinleyici kaynak RegisterAllProvidersIP ve HostRecordTTL küme parametrelerini ayarlamak gerekli olabilir. Bu parametreler, yeniden bağlanma zaman aşımları engelleyebilecek bir yük devretme sonrasında azaltabilir. Bu parametreleri yanı sıra örnek kod hakkında daha fazla bilgi için bkz: [oluşturun veya bir kullanılabilirlik grubu dinleyicisi yapılandırma](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).

<!-- Update_Description: update meta properties -->