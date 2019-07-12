---
title: Azure Backup sunucusu ve DPM ile ilgili SSS
description: 'Hakkında sık sorulan sorulara yanıtlar: Azure Backup sunucusu ve DPM.'
services: backup
author: srinathvasireddy
manager: sivan
ms.service: backup
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: srinathv
ms.openlocfilehash: 7a598038ee435b67b9ad8f06bdec2490bc1c53c3
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705096"
---
# <a name="azure-backup-server-and-dpm---faq"></a>Azure Backup sunucusu ve DPM - SSS
Bu makalede Azure Backup sunucusu ve DPM hakkında sık sorulan sorular yanıtlanmaktadır.

### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Bir fiziksel sunucu için Tam Kurtarma (BMR) yedeklemesi oluşturmak üzere Azure Backup Sunucusu'nu kullanabilir miyim? <br/>
Evet.

### <a name="can-i-register-the-server-to-multiple-vaults"></a>Sunucunun birden fazla kasaya kaydedebilir miyim?
Hayır. Bir DPM veya Azure Backup sunucusu yalnızca bir kasaya kaydedilebilir.


### <a name="can-i-use-dpm-to-back-up-apps-in-azure-stack"></a>Uygulamaları Azure Stack'te yedeklemek için DPM'yi kullanabilir miyim?
Hayır. Uygulamaları Azure Stack'te yedeklemek için DPM'yi kullanarak Azure Backup desteklemiyor, Azure Stack korumak için Azure Backup'ı kullanabilirsiniz.

### <a name="if-ive-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-install-system-center-dpm-to-back-up-on-premises-workloads-to-azure"></a>Dosya ve klasörlerimi korumak için Azure Backup aracısını yükledim, şirket içi iş yüklerini Azure'a yedeklemek için System Center DPM'yi yükleyebilir miyim?
Evet. Ancak önce DPM'yi ayarlayın ve ardından Azure Backup aracısını yüklemeniz gerekir.  Bileşenlerinin bu sırada yüklenmesi, Azure Backup aracısının DPM ile çalışmasını sağlar. Aracıyı DPM yüklenmeden önce yüklenmesi önerilmez veya değil.

### <a name="why-cant-i-add-an-external-dpm-server-after-installing-ur7-and-latest-azure-backup-agent"></a>Harici bir DPM sunucusu UR7 ve en son Azure Backup aracısını yükledikten sonra neden ekleyemiyorum?
UR7 ve en son Azure Backup aracısını başlatmak için yükledikten sonra en az bir gün beklemeniz gerekir (güncelleştirme paketi 7'den önceki, bir güncelleştirme paketini kullanarak) bulut için korunan veri kaynakları ile DPM sunucuları için **dış DPM Ekle sunucu**. Bir günlük dönem için DPM koruma gruplarının meta verileri Azure'a karşıya yüklemek için gereklidir. Koruma grubu meta verileri ilk kez gecelik işi aracılığıyla yüklenir.

## <a name="vmware-and-hyper-v-backup"></a>VMware ve Hyper-V yedekleme

### <a name="can-i-back-up-vmware-vcenter-servers-to-azure"></a>VMware vCenter sunucularını Azure'a yedekleyebilir miyim?
Evet. VMware vCenter Server ve ESXi konaklarının azure'a yedeklemek için Azure Backup Sunucusu'nu kullanabilirsiniz.

- [Daha fazla bilgi edinin](backup-mabs-protection-matrix.md) desteklenen sürümleri hakkında.
- [Bu adımları](backup-azure-backup-server-vmware.md) bir VMware sunucusunu yedeklemek için.

### <a name="do-i-need-a-separate-license-to-recover-a-full-on-premises-vmwarehyper-v-cluster"></a>Tam şirket içi VMware/Hyper-V kümeyi kurtarmak için ayrı bir lisans gerekiyor mu?
Vmware'den/Hyper-V koruması için lisanslama ayrı yoktur.

- System Center müşterisiyseniz, VMware Vm'leri korumak için System Center Data Protection Manager (DPM) kullanın.
- System Center müşteri değilseniz, VMware Vm'leri korumak için Azure Backup sunucusu (Kullandıkça Öde) kullanabilirsiniz.


## <a name="sharepoint"></a>SharePoint

### <a name="can-i-recover-a-sharepoint-item-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson-with-protection-on-disk"></a>SharePoint, SQL AlwaysOn (disk koruması) kullanılarak yapılandırılmışsa, bir SharePoint öğesi için özgün konuma kurtarma gerçekleştirebilir miyim?
Evet, öğe özgün SharePoint sitesine kurtarılabilir.

### <a name="can-i-recover-a-sharepoint-database-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson"></a>SharePoint, SQL Alwayson'u kullanarak yapılandırılmışsa, bir SharePoint veritabanını özgün konumuna kurtarma gerçekleştirebilir miyim?
SharePoint veritabanlarını SQL AlwaysOn yapılandırıldığından, bunlar kullanılabilirlik grubu kaldırılmadığı sürece değiştirilemez. Sonuç olarak, DPM veritabanını özgün konumuna geri yükleyemezsiniz. Bir SQL Server veritabanını başka bir SQL Server örneğine kurtarabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Diğer SSS'leri okuyun:

- [Daha fazla bilgi edinin](backup-support-matrix-mabs-dpm.md) hakkında Azure Backup sunucusu ve DPM destek matrisi.
- [Daha fazla bilgi edinin](backup-azure-mabs-troubleshoot.md) Azure Backup sunucusu ve DPM hakkında sorun giderme yönergeleri.
