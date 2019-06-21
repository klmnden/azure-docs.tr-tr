---
title: Sunucuları kaldırma ve korumayı devre dışı bırakma | Microsoft Docs
description: Bu makalede Site Recovery kasasından sunucularının kaydını sil ve sanal makineler ve fiziksel sunucuları için korumayı devre dışı bırakın.
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/18/2019
ms.author: rajani-janaki-ram
ms.openlocfilehash: 400ffaa9e6fed14ceabf34283cd5fa7c7a0336b8
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203407"
---
# <a name="remove-servers-and-disable-protection"></a>Sunucuları kaldırma ve korumayı devre dışı bırakma

Bu makalede, bir kurtarma Hizmetleri kasası sunuculardan kaydı yapmayı ve Site Recovery tarafından korunan makineler için korumayı devre dışı bırakma açıklanır.


## <a name="unregister-a--configuration-server"></a>Yapılandırma sunucusunun kaydını kaldırma

VMware Vm'leri veya Windows/Linux fiziksel sunucuları Azure'a çoğaltmak, şu şekilde bir kasa bir bağlantısız yapılandırma sunucusundan kaydını kaldırabilirsiniz:

1. [Sanal makinelerin korumasını devre dışı bırakmak](#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure).
2. [İlişkisini kaldır veya Sil](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) çoğaltma ilkeleri.
3. [Yapılandırma sunucusunu silme](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server)

## <a name="unregister-a-vmm-server"></a>VMM sunucusunun kaydını kaldırma

1. Kaldırmak istediğiniz VMM sunucusundaki Bulutlar içinde sanal makineleri çoğaltma durdurun.
2. Silmek istediğiniz VMM sunucusundaki Bulutlar tarafından kullanılan tüm ağ eşlemeleri silin. İçinde **Site Recovery altyapısı** > **için System Center VMM** > **ağ eşlemesini**, ağ eşlemesini sağ tıklayın > **Sil**.
3. VMM sunucusu Kimliğini not alın.
4. Çoğaltma İlkeleri'nden kaldırmak istediğiniz VMM sunucusundaki Bulutlar arasındaki ilişki kaldırılamıyor.  İçinde **Site Recovery altyapısı** > **için System Center VMM** >  **çoğaltma ilkeleri**, ilişkili İlkesi'ne çift tıklayın. Buluta sağ tıklayın > **ilişkisini**.
5. VMM sunucusunun veya etkin düğüm silin. İçinde **Site Recovery altyapısı** > **için System Center VMM** > **VMM sunucuları**, sunucuya sağ tıklayın > **Sil** .
6. Bağlantısı kesilmiş durumda VMM sunucunuz varsa, sonra da indirin ve çalıştırın [temizleme betiği](https://aka.ms/asr-cleanup-script-vmm) VMM sunucusunda. PowerShell ile açın **yönetici olarak çalıştır** seçeneği, varsayılan (LocalMachine) kapsam için yürütme ilkesini değiştirmek için. Betikte, kaldırmak istediğiniz VMM sunucusunun Kimliğini belirtin. Betik, kayıt ve bulut eşleştirme bilgilerini sunucusundan kaldırır.
5. Temizleme betiği, tüm ikincil VMM sunucusunda çalıştırın.
6. Temizleme betiği sağlayıcısı yüklü olan tüm diğer edilgen VMM küme düğümleri üzerinde çalıştırın.
7. Sağlayıcı VMM sunucusunda el ile kaldırın. Bir kümeniz varsa, tüm düğümlerden kaldırın.
8. Sanal makinelerinizi Azure'a çoğaltılması, silinen bulutlarındaki Hyper-V konaklarından Microsoft Kurtarma Hizmetleri Aracısı'nı kaldırmanız gerekir.

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a>Hyper-V sitesindeki bir Hyper-V konağının kaydını kaldırma

VMM tarafından yönetilmeyen Hyper-V konaklarını bir Hyper-V sitesine toplanır. Bir konağı bir Hyper-V sitesine şu şekilde kaldırabilirsiniz:

1. Hyper-V konakta bulunan sanal makineler için çoğaltma devre dışı bırakın.
2. Hyper-V sitesi için ilkeler arasındaki ilişki kaldırılamıyor. İçinde **Site Recovery altyapısı** > **Hyper-V siteleri için** >  **çoğaltma ilkeleri**, ilişkili İlkesi'ne çift tıklayın. Siteye sağ tıklayın > **ilişkisini**.
3. Hyper-V konakları silin. İçinde **Site Recovery altyapısı** > **Hyper-V siteleri için** > **Hyper-V konakları**, sunucuya sağ tıklayın > **Sil** .
4. Tüm konaklar ondan temizlendikten sonra Hyper-V sitesini silin. İçinde **Site Recovery altyapısı** > **Hyper-V siteleri için** > **Hyper-V siteleri**, siteye sağ tıklayın > **Sil** .
5. İçinde Hyper-V konağınız varsa bir **bağlantısı kesilmiş** belirtin ve ardından kaldırdığınız her Hyper-V konağı üzerinde aşağıdaki betiği çalıştırın. Betik, sunucudaki ayarları temizler ve kasadan kaydını iptal eder.


```powershell
        pushd .
        try
        {
            $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
            $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
            $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
            $isAdmin=$principal.IsInRole($administrators)
            if (!$isAdmin)
            {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;
            }

            $error.Clear()
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host

            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }

            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }

            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'
            $idMgmtCloudContainerId='IdMgmtCloudContainerId'


            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."
                    Remove-Item -Recurse -Path $registrationPath
                }

                if (Test-Path $proxySettingsPath)
                {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }

                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                if($regNode.IdMgmtCloudContainerId -ne $null)
                {
                    "Removing IdMgmtCloudContainerId"
                    Remove-ItemProperty -Path $asrHivePath -Name $idMgmtCloudContainerId
                }
                "Registry keys removed."
            }

            # First retrieve all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {
            [system.exception]
            Write-Host "Error occurred" -ForegroundColor "Red"
            $error[0]
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd
```


## <a name="disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure"></a>Bir VMware VM veya fiziksel sunucu (Vmware'den azure'a) için korumayı devre dışı bırak

1. İçinde **korunan öğeler** > **çoğaltılan öğeler**, makineye sağ tıklayın > **çoğaltma devre dışı bırakma**.
2. İçinde **çoğaltma devre dışı bırakma** sayfasında, aşağıdaki seçeneklerden birini seçin:
    - **Çoğaltma ve Kaldır (önerilir) devre dışı** - bu seçenek çoğaltılan öğeyi Azure Site Recovery'den kaldırın ve makine için çoğaltma durdurulur. Yapılandırma sunucusunu çoğaltma yapılandırması temizlenir ve bu korumalı sunucu için Site Recovery Faturalaması durdurulur. Yapılandırma sunucusu bağlı durumda olduğunda bu seçeneği yalnızca kullanılabileceğini unutmayın.
    - **Kaldırma** -bu seçeneği yalnızca kaynak ortamı (bağlı değil) silindi veya erişilebilir değil ise, kullanılması gereken. Bu işlem çoğaltılan öğeyi Azure Site Kurtarma (faturalandırma sona erdirilir) kaldırır. Yapılandırma sunucusunu çoğaltma yapılandırması **yapmamayı** temizlenir. 

> [!NOTE]
> Mobility hizmetinin korunan sunucularda kaldırılmayacak hem seçenekler, el ile kaldırmanız gerekir. Aynı yapılandırma sunucusuna yeniden kullanarak sunucuyu korumayı planlıyorsanız, mobility hizmetini kaldırma atlayabilirsiniz.

> [!NOTE]
> Bir VM üzerinde zaten başarısız olan ve Azure'da çalışan, korumayı devre dışı Not değil kaldırın / başarısız VM üzerinde etkiler.
## <a name="disable-protection-for-a-azure-vm-azure-to-azure"></a>Bir Azure sanal makine (Azure'dan Azure'a) için korumayı devre dışı bırakın

-  İçinde **korunan öğeler** > **çoğaltılan öğeler**, makineye sağ tıklayın > **çoğaltma devre dışı bırakma**.
> [!NOTE]
> Mobility hizmetinin korunan sunucularda kaldırılmayacak, el ile kaldırmanız gerekir. Sunucu yeniden korumayı planlıyorsanız, mobility hizmetini kaldırma atlayabilirsiniz.

## <a name="disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure"></a>(Hyper-V'den azure'a) Hyper-V sanal makine için korumayı devre dışı bırakın

> [!NOTE]
> Bir VMM sunucusu olmadan Azure'a Hyper-V sanal makinelerini çoğaltıyorsanız, bu yordamı kullanın. Kullanarak sanal makinelerinizde çoğaltıyorsanız **System Center VMM'den azure'a** senaryo, ardından izleme yönergeleri korumayı devre dışı bırakma için Hyper-V sanal makine Azure'a senaryosu için System Center VMM kullanarak çoğaltma

1. İçinde **korunan öğeler** > **çoğaltılan öğeler**, makineye sağ tıklayın > **çoğaltma devre dışı bırakma**.
2. İçinde **çoğaltma devre dışı bırakma**, aşağıdaki seçenekleri belirleyebilirsiniz:
   - **Çoğaltma ve Kaldır (önerilir) devre dışı** - bu seçenek çoğaltılan öğeyi Azure Site Recovery'den kaldırın ve makine için çoğaltma durdurulur. Şirket içi sanal makine çoğaltma yapılandırması temizlenir ve bu korumalı sunucu için Site Recovery Faturalaması durdurulur.
   - **Kaldırma** -bu seçeneği yalnızca kaynak ortamı (bağlı değil) silindi veya erişilebilir değil ise, kullanılması gereken. Bu işlem çoğaltılan öğeyi Azure Site Kurtarma (faturalandırma sona erdirilir) kaldırır. Şirket içi sanal makine çoğaltma yapılandırması **yapmamayı** temizlenir. 

 > [!NOTE]
     > Seçerseniz, **Kaldır** sonra aşağıdaki komut kümesini çalıştırmak seçeneği çoğaltma ayarları temizlemek için şirket içi Hyper-V sunucusu.

> [!NOTE]
> Bir VM üzerinde zaten başarısız olan ve Azure'da çalışan, korumayı devre dışı Not değil kaldırın / başarısız VM üzerinde etkiler.

1. Sanal makine için çoğaltmayı kaldırmak için kaynak Hyper-V konak sunucusu üzerinde. SQLVM1 sanal makinenizin adıyla değiştirin ve bir yönetici bir PowerShell üzerinden betiği çalıştırın

```powershell
    $vmName = "SQLVM1"
    $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
    $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
    $replicationService.RemoveReplicationRelationship($vm.__PATH)
```

## <a name="disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario"></a>System Center VMM Azure'a senaryosu kullanarak azure'da çoğaltma bir Hyper-V sanal makine için korumayı devre dışı bırak

1. İçinde **korunan öğeler** > **çoğaltılan öğeler**, makineye sağ tıklayın > **çoğaltma devre dışı bırakma**.
2. İçinde **çoğaltma devre dışı bırakma**, aşağıdaki seçeneklerden birini seçin:

   - **Çoğaltma ve Kaldır (önerilir) devre dışı** - bu seçenek çoğaltılan öğeyi Azure Site Recovery'den kaldırın ve makine için çoğaltma durdurulur. Şirket içi sanal makine çoğaltma yapılandırması temizlenir ve bu korumalı sunucu için Site Recovery Faturalaması durdurulur.
   - **Kaldırma** -bu seçeneği yalnızca kaynak ortamı (bağlı değil) silindi veya erişilebilir değil ise, kullanılması gereken. Bu işlem çoğaltılan öğeyi Azure Site Kurtarma (faturalandırma sona erdirilir) kaldırır. Şirket içi sanal makine çoğaltma yapılandırması **yapmamayı** temizlenir. 

     > [!NOTE]
     > Seçerseniz, **Kaldır** çoğaltma ayarları temizlemek için aşağıdaki komutlar tun şirket VMM sunucusu seçeneğini.
3. VMM konsolundan (yönetici ayrıcalıkları gereklidir) PowerShell kullanarak kaynak VMM sunucusunda bu betiği çalıştırın. Yer tutucusunu değiştirin **SQLVM1** sanal makinenizin adıyla.

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection
4. Yukarıdaki adımları, VMM sunucusundaki çoğaltma ayarlarını temizleyin. Hyper-V konak sunucusunda çalışan sanal makine için çoğaltmayı durdurmak için bu betiği çalıştırın. SQLVM1 Hyper-V konak sunucusu adı sanal makine ve host01.contoso.com adıyla değiştirin.

```powershell
    $vmName = "SQLVM1"
    $hostName  = "host01.contoso.com"
    $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
    $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
    $replicationService.RemoveReplicationRelationship($vm.__PATH)
```

## <a name="disable-protection-for-a-hyper-v-virtual-machine-replicating-to-secondary-vmm-server-using-the-system-center-vmm-to-vmm-scenario"></a>System Center VMM'vmm senaryosu kullanarak VMM sunucusuna ikincil çoğaltma bir Hyper-V sanal makine için korumayı devre dışı bırak

1. İçinde **korunan öğeler** > **çoğaltılan öğeler**, makineye sağ tıklayın > **çoğaltma devre dışı bırakma**.
2. İçinde **çoğaltma devre dışı bırakma**, aşağıdaki seçeneklerden birini seçin:

   - **Çoğaltma ve Kaldır (önerilir) devre dışı** - bu seçenek çoğaltılan öğeyi Azure Site Recovery'den kaldırın ve makine için çoğaltma durdurulur. Şirket içi sanal makine çoğaltma yapılandırması temizlenir ve bu korumalı sunucu için Site Recovery Faturalaması durdurulur.
   - **Kaldırma** -bu seçeneği yalnızca kaynak ortamı (bağlı değil) silindi veya erişilebilir değil ise, kullanılması gereken. Bu işlem çoğaltılan öğeyi Azure Site Kurtarma (faturalandırma sona erdirilir) kaldırır. Şirket içi sanal makine çoğaltma yapılandırması **yapmamayı** temizlenir. Çoğaltma ayarları şirket içi sanal makineleri temizleme betikleri aşağıdaki kümesini çalıştırın.
     > [!NOTE]
     > Seçerseniz, **Kaldır** çoğaltma ayarları temizlemek için aşağıdaki komutlar tun şirket VMM sunucusu seçeneğini.

3. VMM konsolundan (yönetici ayrıcalıkları gereklidir) PowerShell kullanarak kaynak VMM sunucusunda bu betiği çalıştırın. Yer tutucusunu değiştirin **SQLVM1** sanal makinenizin adıyla.

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection
4. İkincil VMM sunucusuna, ikincil sanal makine ayarlarını temizlemek için bu betiği çalıştırın:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force
5. İkincil VM'yi yeniden VMM konsolunda algılanan, böylece ikincil VMM sunucusunda Hyper-V ana bilgisayar sunucusunda sanal makineleri yenileyin.
6. Yukarıdaki adımları, VMM sunucusundaki çoğaltma ayarlarını temizleyin. Sanal makine için çoğaltmayı durdurmak istiyorsanız, aşağıdaki betiği oh birincil ve ikincil VM'ler çalıştırın. SQLVM1 sanal makinenizin adıyla değiştirin.

        Remove-VMReplication –VMName “SQLVM1”




