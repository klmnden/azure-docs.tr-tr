---
title: "Azure rol tabanlı erişim denetimi ile yedek yönetme | Microsoft Docs"
description: "Kurtarma Hizmetleri kasasına yedekleme yönetim işlemlerini erişimi yönetmek için rol tabanlı erişim denetimini kullanın."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 3bd46b97-4b29-47a5-b5ac-ac174dd36760
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/22/2017
ms.author: trinadhk;markgal
ms.openlocfilehash: b6e4c6761e1bd5c17c9c3428491113042d3b1d31
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Azure yedekleme kurtarma noktaları yönetmek için rol tabanlı erişim denetimi kullanma
Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, ekibiniz içinde görevleri kurabilmeleri ve işlerini yapmak için gereksinim duydukları kullanıcılara sadece erişim miktarını verebilirsiniz.

> [!IMPORTANT]
> Azure yedekleme tarafından sağlanan rolleri Azure portalında gerçekleştirilen eylemler için sınırlı veya PowerShell cmdlet'leri kurtarma Hizmetleri kasası. Data Protection Manager kullanıcı Arabirimi veya Azure yedekleme sunucusu kullanıcı Arabirimi olan bu rolleri denetimi dışında Azure Yedekleme aracısı istemcisi kullanıcı arabirimini veya System center gerçekleştirilen eylemler.

Azure Backup, Yedekleme yönetimi işlemlerini denetlemek için 3 yerleşik roller sağlar. Daha fazla bilgi almak [Azure RBAC yerleşik rolleri](../active-directory/role-based-access-built-in-roles.md)

* [Yedekleme katkıda bulunan](../active-directory/role-based-access-built-in-roles.md#backup-contributor) -bu rol oluşturmak ve kurtarma Hizmetleri kasası oluşturma ve erişim başkalarına verip dışında yedekleme yönetmek için tüm izinlere sahiptir. Bu rolü her yedekleme Yönetimi işlemi yapmak için Yedekleme yönetimi yönetici olarak düşünün.
* [Yedekleme işletmeni](../active-directory/role-based-access-built-in-roles.md#backup-operator) -bu rol her şeyi katılımcı dışında yedekleme ve yönetme yedekleme ilkeleri kaldırma için izinlere sahip. Stop yedekleme ile verileri silmek veya şirket içi kaynakları kaydının kaldırılması gibi bozucu işlemlerini gerçekleştiremezsiniz dışında bu katkıda bulunan eşdeğer bir roldür.
* [Yedekleme okuyucu](../active-directory/role-based-access-built-in-roles.md#backup-reader) -bu rol tüm yedekleme yönetim işlemlerini görüntülemek için gerekli izinlere sahip. Bu rol bir izleme olmayı düşünün.

Daha fazla denetim için kendi rolleri tanımlamak için arıyorsanız, bkz: nasıl yapılır [özel roller yapı](../active-directory/role-based-access-control-custom-roles.md) Azure rbac'de.



## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Yedekleme yönetimi eylemleri yedekleme yerleşik roller eşleme
Aşağıdaki tabloda, yedekleme yönetim eylemleri ve bu işlemi gerçekleştirmek için gerekli olan ilgili minimum RBAC rolü yakalar.

| Yönetim işlemi | Gereken en düşük RBAC rolü |
| --- | --- |
| Kurtarma Hizmetleri kasası oluşturma | Kasasının kaynak grubu üzerinde katılımcı |
| Azure sanal makinelerini yedeklemeyi etkinleştirme | Kasa, sanal makine Katılımcısı vm'lerde yedekleme işletmeni |
| Talep üzerine yedekleme VM | Yedekleme işletmeni |
| VM geri yükleme | Yedekleme işletmeni, kaynak grubu katkıda bulunan, VM ve sanal ağlar dağıtılan zorunda kalacaklarını |
| Diskler, tek tek dosyaların VM yedekten geri yükleme | Yedekleme işletmeni, sanal makine Katılımcısı vm'lerde |
| Azure VM yedeklemesi için yedekleme ilkesi oluşturma | Yedekleme katkıda bulunan |
| Azure VM yedekleme, yedekleme ilkesini değiştirme | Yedekleme katkıda bulunan |
| Azure VM yedekleme, yedekleme ilkesi silme | Yedekleme katkıda bulunan |
| Stop yedekleme (verileri tut ile veya verileri Sil) VM yedekleme hakkında | Yedekleme katkıda bulunan |
| Şirket içi Windows Server/istemci/SCDPM veya Azure yedekleme sunucusu kaydetme | Yedekleme işletmeni |
| Kayıtlı şirket içi Windows Server/istemci/SCDPM veya Azure yedekleme sunucusu silme | Yedekleme katkıda bulunan |

## <a name="next-steps"></a>Sonraki adımlar
* [Rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md): Azure portalında RBAC ile çalışmaya başlama.
* Erişimle yönetmeyi öğrenin:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Rol tabanlı erişim denetimi sorun giderme](../active-directory/role-based-access-control-troubleshooting.md): sık karşılaşılan sorunları düzeltmek için öneriler alın.
