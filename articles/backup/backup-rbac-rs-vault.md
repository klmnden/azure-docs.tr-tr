---
title: Azure rol tabanlı erişim denetimi ile yedeklemeleri Yönetme '
description: Kurtarma Hizmetleri kasasına Yedekleme Yönetimi işlemlerinde erişimi yönetmek için rol tabanlı erişim denetimi kullanın.
services: backup
author: utraghuv
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: utraghuv
ms.openlocfilehash: 3b4585422a36992241fb4839238b1f6aa46c659f
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565649"
---
# <a name="use-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Azure Backup kurtarma noktaları yönetmek için rol tabanlı erişim denetimi kullanma
Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak ekibiniz içinde görevleri ayırabilir, bu işlere gerek duyan kişilere sadece erişim miktarını verebilirsiniz.

> [!IMPORTANT]
> Azure Backup tarafından sağlanan rolleri Azure portalında veya REST API aracılığıyla gerçekleştirilen eylemlere sınırlı veya PowerShell veya CLI cmdlet'leri kurtarma Hizmetleri kasası. Data Protection Manager kullanıcı arabirimini veya Azure Backup sunucusu kullanıcı Arabirimi denetimi bu rollerden dışında olan Azure Yedekleme aracısı istemcisi kullanıcı Arabirimi veya System center gerçekleştirilen eylemler.

Azure Backup, yedekleme yönetim işlemlerini denetlemek için üç yerleşik rol sağlar. [Azure RBAC yerleşik rolleri](../role-based-access-control/built-in-roles.md) hakkında daha fazla bilgi edinin

* [Yedekleme Katılımcısı](../role-based-access-control/built-in-roles.md#backup-contributor) -bu rol oluşturmak ve kurtarma Hizmetleri kasası silme ve diğerleri için erişim verme dışındaki yedekleme yönetmek için tüm izinlere sahiptir. Bu rol tüm yedekleme Yönetimi işlemi yapmak için Yedekleme Yönetimi Yöneticisi olarak düşünün.
* [Yedekleme işleci](../role-based-access-control/built-in-roles.md#backup-operator) -bu rolün katkıda bulunan yedekleme ve yönetmeye yedekleme ilkelerini kaldırma dışındaki her şey için izinlere sahip. İçeren yedeklemeyi durdurma verileri silmek ya da şirket içi kaynaklara kaydını kaldırma gibi zararlı işlemler gerçekleştirilemiyor dışında bu rol için katkıda bulunan eşdeğerdir.
* [Yedekleme okuyucusu](../role-based-access-control/built-in-roles.md#backup-reader) -bu rolün tüm yedekleme yönetimi işlemlerini görüntüleme iznine sahiptir. Bu rol, bir izleme kişi olmasını düşünün.

Daha fazla denetim için kendi rolleri tanımlamak için arıyorsanız, bkz. nasıl [özel roller oluşturma](../role-based-access-control/custom-roles.md) Azure rbac'de.



## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Yedekleme yönetim eylemleri için eşleme yedekleme yerleşik roller
Aşağıdaki tabloda, yedekleme yönetim eylemleri ve bu işlemi gerçekleştirmek için gerekli olan karşılık gelen en düşük RBAC rolü yakalar.

| Yönetim işlemi | Gerekli en düşük RBAC rolü | Kapsamı gereklidir |
| --- | --- | --- |
| Kurtarma Hizmetleri kasası oluşturma | Yedekleme Katılımcısı | Kasa içeren kaynak grubu |
| Azure VM yedeklemeyi etkinleştirme | Yedekleme işletmeni | Kasa içeren kaynak grubu |
| | Sanal makine Katılımcısı | VM kaynağı |
| İsteğe bağlı yedekleme VM | Yedekleme işletmeni | Kurtarma kasası kaynağı |
| VM'yi geri yükle | Yedekleme işletmeni | Kurtarma Hizmetleri kasası |
| | Katılımcı | VM'nin dağıtılacağı kaynak grubu |
| | Sanal makine Katılımcısı | Kaynak yedeklenen sanal makine |
| Yönetilmeyen diskler VM yedeklemesini geri yükleme | Yedekleme işletmeni | Kurtarma kasası kaynağı |
| | Sanal makine Katılımcısı | Kaynak yedeklenen sanal makine |
| | Depolama hesabı Katılımcısı | Diskleri geri nerede bulunacağını depolama hesabı kaynağı |
| Yönetilen diskler, VM yedekten geri yükleyin | Yedekleme işletmeni | Kurtarma kasası kaynağı |
| | Sanal makine Katılımcısı | Kaynak yedeklenen sanal makine |
| | Depolama hesabı Katılımcısı | Yönetilen disklere dönüştürmeden önce verileri kasadan tutmak için geri yükleme işleminin bir parçası olarak seçili olan geçici depolama hesabı |
| | Katılımcı | Kaynak grubu için yönetilen diskleri geri yüklenmesi |
| Sanal makine yedeklemesini tek tek dosyaları geri yükleme | Yedekleme işletmeni | Kurtarma kasası kaynağı |
| | Sanal makine Katılımcısı | Kaynak yedeklenen sanal makine |
| Azure VM yedeklemesi için yedekleme ilkesi oluşturma | Yedekleme Katılımcısı | Kurtarma kasası kaynağı |
| Azure VM yedekleme, yedekleme ilkesini değiştirme | Yedekleme Katılımcısı | Kurtarma kasası kaynağı |
| Azure VM yedekleme, yedekleme ilkesini silme | Yedekleme Katılımcısı | Kurtarma kasası kaynağı |
| Yedeklemeyi Durdur (verileri tut ile veya veri silme) VM yedeklemesi hakkında | Yedekleme Katılımcısı | Kurtarma kasası kaynağı |
| Şirket içi Windows Server/istemci/SCDPM ya da Azure Backup sunucusu kaydetme | Yedekleme işletmeni | Kurtarma kasası kaynağı |
| Şirket içi Windows Server/istemci/SCDPM ya da Azure Backup sunucusu silme kayıtlı | Yedekleme Katılımcısı | Kurtarma kasası kaynağı |

> [!IMPORTANT]
> VM katkıda bulunan bir sanal makine kaynak kapsamda belirtin ve yedekleme VM ayarlarının bir parçası tıklayın, sanal makine zaten yalnızca abonelik düzeyinde yedekleme durumu çalıştığını doğrulamak için bir çağrı olarak yedeklenen olsa bile 'Yedeklemeyi Etkinleştir' ekranı açılır. Bunu önlemek için ya da kasa ve sanal makinenin yedekleme öğesi görünümü açma veya abonelik düzeyinde VM katkıda bulunanı rolü belirtin gidin.

## <a name="minimum-role-requirements-for-the-azure-file-share-backup"></a>Azure dosya paylaşımı yedeklemesi için en düşük rolü gereksinimleri
Aşağıdaki tabloda, Azure dosya paylaşımı işlemi gerçekleştirmek için gerekli karşılık gelen rol ve yedekleme yönetim eylemleri yakalar.

| Yönetim işlemi | Gerekli rolü | Kaynaklar |
| --- | --- | --- |
| Azure dosya paylaşımlarının yedeklemesini etkinleştirin | Yedekleme Katılımcısı | Kurtarma Hizmetleri kasası |
| | Depolama Hesabı | Katkıda bulunan depolama hesabı kaynağı |
| İsteğe bağlı yedekleme VM | Yedekleme işletmeni | Kurtarma Hizmetleri kasası |
| Dosya paylaşımını geri yükleme | Yedekleme işletmeni | Kurtarma Hizmetleri kasası |
| | Depolama hesabı Katılımcısı | Depolama hesabı kaynaklarına geri yükleme kaynak ve hedef dosya paylaşımları mevcut olduğu |
| Tek tek dosyaları geri yükleme | Yedekleme işletmeni | Kurtarma Hizmetleri kasası |
| | Depolama hesabı Katılımcısı |   Depolama hesabı kaynaklarına geri yükleme kaynak ve hedef dosya paylaşımları mevcut olduğu |
| Korumayı Durdur | Yedekleme Katılımcısı | Kurtarma Hizmetleri kasası |      
| Depolama hesabından kasa kaydı |   Yedekleme Katılımcısı | Kurtarma Hizmetleri kasası |
| | Depolama hesabı Katılımcısı | Depolama hesabı kaynağı|


## <a name="next-steps"></a>Sonraki adımlar
* [Rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md): Azure portalında RBAC ile çalışmaya başlayın.
* Erişim ile yönetmeyi öğrenin:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Rol tabanlı erişim denetimi sorunlarını giderme](../role-based-access-control/troubleshooting.md): Yaygın sorunları çözme için öneriler alın.
