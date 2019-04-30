---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: d579e7a4fd83c1a0ce335e0b2357dcbafb217398
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097718"
---
Bu adımda, aynı ağ üzerinde çalışan bir istemci uygulaması kullanarak kullanılabilirlik grubu dinleyicisi sınayın.

İstemci bağlantısı aşağıdaki gereksinimlere sahiptir:

* İstemci bağlantıları dinleyicisi olandan farklı bir bulut hizmeti barındıran her zaman açık kullanılabilirlik çoğaltmalarının bulunan makineleri gelmelidir.
* İstemciler her zaman açık çoğaltma farklı alt ağlarda olup olmadığını belirtmeniz *MultisubnetFailover = True* bağlantı dizesindeki. Bu durum, çoğaltma çeşitli alt ağlar için paralel bağlantı denemelerinde de sonuçlanır. Bu senaryo, bir bölgeler arası Always On kullanılabilirlik grubu dağıtımının içerir.

Dinleyici için aynı Azure sanal ağı (ancak bir çoğaltmasını barındıran bir değil) sanal makinelerin birinden bağlanmak için bir örnek verilebilir. Kullanılabilirlik grubu dinleyicisi için SQL Server Management Studio bağlanmak bu testin tamamlanması için kolay bir yolu denemektir. Başka bir basit yöntemi çalıştırmaktır [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx)gibi:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [!NOTE]
> EndpointPort değer ise *1433*, çağrıyı belirtmeniz gerekmez. Önceki çağrı da istemci makine aynı etki alanına katılmış olan ve Windows kimlik doğrulaması kullanarak arayan veritabanı izinleri verildi varsayar.
> 
> 

Dinleyiciyi test ettiğinizde istemciler arasında yük devretme işlemleri için dinleyici bağlanabildiğinden emin olmak için kullanılabilirlik grubu yük devretme emin olun.

<!-- Update_Description: update meta properties -->