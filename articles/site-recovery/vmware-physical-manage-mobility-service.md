---
title: Mobility Aracısı VMware vm'lerinin olağanüstü durum kurtarma için sunucular ve Azure Site Recovery ile fiziksel sunucuları yönetme | Microsoft Docs
description: VMware Vm'lerini ve fiziksel sunucuları Azure Site Recovery hizmetini kullanarak azure'a olağanüstü durum kurtarma için Mobility Hizmeti Aracısı yönetin.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: ramamill
ms.openlocfilehash: 69b8e1c533747d1bade69949911ea43f299f49e9
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59794244"
---
# <a name="manage-mobility-agent-on-protected-machines"></a>Mobility Aracısı korunan makinelere yönetme

VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma için Azure Site Recovery kullandığınızda sunucunuzda mobility aracınızı ayarlayın. Mobility Aracısı, korunan makinenin, yapılandırma sunucusu/genişleme işlem sunucusu arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir. Bu makalede dağıtıldıktan sonra mobility aracısını yönetmek için ortak görevler özetlenir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="update-mobility-service-from-azure-portal"></a>Azure portalından mobility hizmetini güncelleştirme

1. Başlamadan önce Mobility hizmetinin korunan makinelere güncelleştirmeden önce yapılandırma sunucusunu, ölçeği genişletilmiş işlem sunucularını ve dağıtımınızın bir parçası olan herhangi bir ana hedef sunucuları güncelleştirildiğinden emin olun.
2. Kasa portalda açın > **çoğaltılan öğeler**.
3. Yapılandırma sunucusunu en son sürüm ise "Yeni Site recovery çoğaltma aracısı güncelleştirmesi kullanılabilir. okuyan bir bildirim görür Yüklemek için tıklayın."

     ![Çoğaltılan öğeler penceresi](./media/vmware-azure-install-mobility-service/replicated-item-notif.png)

4. Bildirime tıklayın ve **Aracısı güncelleştirmesi**, Mobility hizmetini yükseltmek istediğiniz makineleri seçin. Daha sonra, **Tamam**'a tıklayın.

     ![Çoğaltılan öğeler VM listesi](./media/vmware-azure-install-mobility-service/update-okpng.png)

5. Her seçili makineler için Mobility hizmetini güncelleştirme işlemi başlatır.

## <a name="update-mobility-service-through-powershell-script-on-windows-server"></a>Windows server powershell betiği ile Mobility hizmetini güncelleştirme

Power shell cmdlet'i aracılığıyla bir sunucu üzerinde Mobility hizmetini yükseltme betiği aşağıdaki kullanın

```azurepowershell
Update-AzRecoveryServicesAsrMobilityService -ReplicationProtectedItem $rpi -Account $fabric.fabricSpecificDetails.RunAsAccounts[0]
```

## <a name="update-account-used-for-push-installation-of-mobility-service"></a>Mobility hizmeti gönderme yüklemesi için kullanılan hesabı güncelleştirme

Mobility hizmetinin göndererek yüklenmesine ilişkin etkinleştirmek için Site Recovery dağıttığınızda makinelerine erişebilir ve makine için çoğaltma etkinleştirildiğinde, hizmeti yüklemek için Site Recovery işlem sunucusu kullanan bir hesap belirtilene. Bu hesabın kimlik bilgilerini güncelleştirmek istiyorsanız, izleyin [bu yönergeleri](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="uninstall-mobility-service"></a>Mobility hizmetini kaldırın

### <a name="on-a-windows-machine"></a>Bir Windows makinede

Kullanıcı Arabirimi veya komut istemi'ni kaldırın.

- **Kullanıcı arabiriminden**: Denetim Masası'nda makinenin seçin **programlar**. Seçin **Microsoft Azure Site Recovery Mobility hizmeti/ana hedef sunucusu** > **kaldırma**.
- **Bir komut isteminden**: Makinede bir yönetici olarak bir komut istemi penceresi açın. Şu komutu çalıştırın: 
    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

### <a name="on-a-linux-machine"></a>Bir Linux makinesinde
1. Linux makinesinde oturum olarak bir **kök** kullanıcı.
2. Bir terminal penceresinde /user/local/ASR için gidin.
3. Şu komutu çalıştırın:
    ```
    uninstall.sh -Y

## Install Site Recovery VSS provider on source machine

Azure Site Recovery VSS provider is required on the source machine to generate application consistency points. If the installation of the provider didn't succeed through push installation, follow the below given guidelines to install it manually.

1. Open admin cmd window.
2. Navigate to the mobility service installation location. (Eg - C:\Program Files (x86)\Microsoft Azure Site Recovery\agent)
3. Run the script InMageVSSProvider_Uninstall.cmd . This will uninstall the service if it already exists.
4. Run the script InMageVSSProvider_Install.cmd to install the VSS provider manually.

## Next steps

- [Set up disaster recovery for VMware VMs](vmware-azure-tutorial.md)
- [Set up disaster recovery for physical servers](physical-azure-disaster-recovery.md)
