---
title: Sunucuları kaldırın ve koruma devre dışı bırakma | Microsoft Docs
description: Bu makalede Site Recovery kasası sunucularından kaydı ve sanal makineleri ve fiziksel sunucuları için korumayı devre dışı bırakmak için açıklar.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: raynew
ms.openlocfilehash: 16a5eaac1138d328f81cfa7d50f8705da867e352
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
ms.locfileid: "29806424"
---
# <a name="remove-servers-and-disable-protection"></a>Sunucuları kaldırma ve korumayı devre dışı bırakma

Bu makalede, bir kurtarma Hizmetleri kasası sunucularından kaydı etme ve Site Recovery tarafından korunan makineler için korumayı devre dışı bırakma açıklanmaktadır.


## <a name="unregister-a--configuration-server"></a>Yapılandırma sunucusu kaydı

VMware Vm'leri veya Windows/Linux fiziksel sunucuları Azure'a çoğaltma durumunda bir kasa bağlantısız yapılandırma sunucusundan gibi kaydını kaldırabilirsiniz:

1. [Sanal makinelerin korumasını devre dışı](#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure).
2. [İlişkisini kaldırın veya silme](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) çoğaltma ilkeleri.
3. [Yapılandırma sunucusu Sil](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server)

## <a name="unregister-a-vmm-server"></a>Bir VMM sunucusunun kaydı silinemedi

1. Sanal makineler, kaldırmak istediğiniz VMM sunucusundaki çoğaltma durdurun.
2. Silmek istediğiniz VMM sunucusundaki Bulutlar tarafından kullanılan tüm ağ eşlemeleri silin. İçinde **Site Recovery altyapısı** > **için System Center VMM** > **ağ eşlemesi**, ağ eşlemesi sağ tıklayın > **silmek**.
3. VMM sunucusu Kimliğini not alın.
4. Kaldırmak istediğiniz VMM sunucusundaki Bulutlar ilkelerden çoğaltma ilişkisini kaldırın.  İçinde **Site Recovery altyapısı** > **için System Center VMM** >  **çoğaltma ilkeleri**, ilişkili ilkeye çift tıklayın. Buluta sağ tıklayın > **ilişkisini**.
5. VMM sunucusu veya etkin düğüm silin. İçinde **Site Recovery altyapısı** > **için System Center VMM** > **VMM sunucuları**, sunucuya sağ tıklayın > **silmek**.
6. VMM sunucunuz bağlantısı kesik bir durumda ise, ardından indirme ve çalıştırma [temizleme betiğini](http://aka.ms/asr-cleanup-script-vmm) VMM sunucusunda. PowerShell ile açmak **yönetici olarak çalıştır** seçeneği, varsayılan (LocalMachine) kapsamı yürütme ilkesini değiştirmek için. Komut dosyasında, kaldırmak istediğiniz VMM sunucusunu Kimliğini belirtin. Betik kaydı ve bulut eşleştirme bilgilerini sunucusundan kaldırır.
5. Temizleme betiğini ikincil hiçbir VMM sunucusunda çalıştırın.
6. Yüklü sağlayıcı olan tüm diğer pasif VMM küme düğümlerine temizleme betiğini çalıştırın.
7. Sağlayıcı VMM sunucusunda el ile kaldırın. Bir kümeniz varsa, tüm düğümlerden kaldırın.
8. Sanal makinelerin Azure'a çoğaltılması, silinen bulutlarındaki Hyper-V ana bilgisayarından Microsoft Kurtarma Hizmetleri Aracısı'nı kaldırmanız gerekir.

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a>Bir Hyper-V sitesi bir Hyper-V ana bilgisayar kaydı

VMM tarafından yönetilmeyen Hyper-V konaklarının, Hyper-V sitesi toplanır. Bir konak bir Hyper-V sitede aşağıdaki gibi kaldırın:

1. Hyper-V ana bilgisayarda yer alan VM'ler için çoğaltma devre dışı bırakın.
2. Hyper-V sitesi için ilkeler ilişkisini kaldırın. İçinde **Site Recovery altyapısı** > **için Hyper-V sitelerini** >  **çoğaltma ilkeleri**, ilişkili ilkeye çift tıklayın. Sitesi > **ilişkisini**.
3. Hyper-V konakları silin. İçinde **Site Recovery altyapısı** > **için Hyper-V sitelerini** > **Hyper-V konakları**, sunucuya sağ tıklayın > **Sil** .
4. Tüm konaklar ondan temizlendikten sonra Hyper-V sitesi silin. İçinde **Site Recovery altyapısı** > **için Hyper-V sitelerini** > **Hyper-V sitelerini**, siteye sağ tıklayın > **Sil** .
5. İçinde Hyper-V ana bilgisayarınız varsa bir **bağlantı kesildi** belirtin, sonra kaldırdığınız her Hyper-V ana bilgisayarda aşağıdaki betiği çalıştırın. Betik sunucu ayarları temizler ve kasadan kaydını siler.



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
                "Registry keys removed."
            }

            # First retrive all the certificates to be deleted
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
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0]
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd



## <a name="disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure"></a>VMware VM veya fiziksel sunucu (VMware azure'a) korumayı devre dışı bırakın

1. İçinde **korunan öğeler** > **çoğaltılan öğeler**, makine adını sağ tıklayın > **çoğaltma devre dışı bırakma**.
2. İçinde **çoğaltma devre dışı bırakma** sayfasında, aşağıdaki seçeneklerden birini seçin:
    - **Çoğaltma ve Kaldır (önerilen) devre dışı** - bu seçenek, Azure Site Recovery'den çoğaltılmış öğeyi kaldırın ve makine için çoğaltma durduruldu. Yapılandırma sunucusundaki çoğaltma yapılandırma Temizlenen ve bu korumalı sunucunun Site Recovery Faturalaması durdurulur.
    - **Kaldırma** -bu seçenek yalnızca kaynak ortamını (bağlı değil) silinmiş veya erişilebilir değil ise, kullanılan bekleniyor. Bu, Azure Site Kurtarma'dan (Faturalaması durdurulur) çoğaltılmış öğeyi kaldırır. Yapılandırma sunucusundaki çoğaltma yapılandırma **almayacak** temizlendi. 

> [!NOTE]
> Mobility hizmetinin korunan sunucularda kaldırılmayacak hem seçenekler, el ile kaldırmanız gerekir. Aynı yapılandırma sunucusuna kullanarak yeniden sunucu korumayı planlıyorsanız, mobility hizmetini kaldırma atlayabilirsiniz.

## <a name="disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure"></a>(Hyper-V Azure) Hyper-V sanal makine için korumayı devre dışı bırakın

> [!NOTE]
> Azure için Hyper-V sanal makinelerini çoğaltıyorsanız olmadan bir VMM sunucusu, bu yordamı kullanın. Sanal makinelerinizi kullanarak çoğaltıyorsanız **System Center VMM Azure** senaryosu, yönergeleri izleyerek [System Center VMM kullanarak çoğaltan bir Hyper-V sanal makine için korumayı devre dışı bırakın Azure senaryosu](#disable-protection-for-a-hyper-v-virtual-machine-replicating-using-the-system-centet-vmm-to-azure-scenario)

1. İçinde **korunan öğeler** > **çoğaltılan öğeler**, makine adını sağ tıklayın > **çoğaltma devre dışı bırakma**.
2. İçinde **çoğaltma devre dışı bırakma**, aşağıdaki seçenekleri seçebilirsiniz:
     - **Çoğaltma ve Kaldır (önerilen) devre dışı** - bu seçenek, Azure Site Recovery'den çoğaltılmış öğeyi kaldırın ve makine için çoğaltma durduruldu. Şirket içi sanal makinesinde çoğaltma yapılandırması temizlendi ve bu korumalı sunucunun Site Recovery Faturalaması durdurulur.
    - **Kaldırma** -bu seçenek yalnızca kaynak ortamını (bağlı değil) silinmiş veya erişilebilir değil ise, kullanılan bekleniyor. Bu, Azure Site Kurtarma'dan (Faturalaması durdurulur) çoğaltılmış öğeyi kaldırır. Şirket içi sanal makinesinde çoğaltma yapılandırması **almayacak** temizlendi. 

    > [!NOTE]
    > Seçerseniz **kaldırmak** çoğaltma ayarlarını temizlemek için aşağıdaki komut kümesini çalıştırmak seçeneği şirket içi Hyper-V Server.
1. Sanal makine için çoğaltmayı kaldırmak için kaynak Hyper-V ana bilgisayar sunucusunda. SQLVM1, sanal makine adı ile değiştirin ve yönetici bir PowerShell betiğini çalıştırın


    
    $vmName = "SQLVM1"  $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"  $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  $replicationService.RemoveReplicationRelationship($vm.__PATH)
    

## <a name="disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario"></a>System Center VMM Azure senaryosuna kullanarak çoğaltma bir Hyper-V sanal makine için korumayı devre dışı bırak

1. İçinde **korunan öğeler** > **çoğaltılan öğeler**, makine adını sağ tıklayın > **çoğaltma devre dışı bırakma**.
2. İçinde **çoğaltma devre dışı bırakma**, aşağıdaki seçeneklerden birini seçin:

    - **Çoğaltma ve Kaldır (önerilen) devre dışı** - bu seçenek, Azure Site Recovery'den çoğaltılmış öğeyi kaldırın ve makine için çoğaltma durduruldu. Şirket içi sanal makinesinde çoğaltma yapılandırması Temizlenen ve bu korumalı sunucunun Site Recovery Faturalaması durdurulur.
    - **Kaldırma** -bu seçenek yalnızca kaynak ortamını (bağlı değil) silinmiş veya erişilebilir değil ise, kullanılan bekleniyor. Bu, Azure Site Kurtarma'dan (Faturalaması durdurulur) çoğaltılmış öğeyi kaldırır. Şirket içi sanal makinesinde çoğaltma yapılandırması **almayacak** temizlendi. 

    > [!NOTE]
    > Seçerseniz **kaldırmak** aşağıdaki komutlar çoğaltma ayarlarını temizlemek için tun içi VMM sunucusu sonra seçeneği.
3. VMM konsolundan PowerShell (yönetici ayrıcalıklarına gerekli) kullanarak kaynak VMM sunucusunda bu komut dosyasını çalıştırın. Yer tutucu Değiştir **SQLVM1** , sanal makine adı.

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection
4. Yukarıdaki adımları VMM sunucusundaki çoğaltma ayarları temizleyin. Hyper-V ana bilgisayar sunucusunda çalışan sanal makine için çoğaltma durdurmak için bu komut dosyasını çalıştırın. SQLVM1 adıyla bir sanal makine ve host01.contoso.com Hyper-V konak sunucusu adını değiştirin.

    
    $vmName = "SQLVM1"  $hostName  = "host01.contoso.com"  $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName  $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName  $replicationService.RemoveReplicationRelationship($vm.__PATH)
    
       
 
## <a name="disable-protection-for-a-hyper-v-virtual-machine-replicating-to-secondary-vmm-server-using-the-system-center-vmm-to-vmm-scenario"></a>System Center VMM VMM senaryosuna kullanarak ikincil VMM sunucusuna çoğaltma bir Hyper-V sanal makine için korumayı devre dışı bırak

1. İçinde **korunan öğeler** > **çoğaltılan öğeler**, makine adını sağ tıklayın > **çoğaltma devre dışı bırakma**.
2. İçinde **çoğaltma devre dışı bırakma**, aşağıdaki seçeneklerden birini seçin:

    - **Çoğaltma ve Kaldır (önerilen) devre dışı** - bu seçenek, Azure Site Recovery'den çoğaltılmış öğeyi kaldırın ve makine için çoğaltma durduruldu. Şirket içi sanal makinesinde çoğaltma yapılandırması Temizlenen ve bu korumalı sunucunun Site Recovery Faturalaması durdurulur.
    - **Kaldırma** -bu seçenek yalnızca kaynak ortamını (bağlı değil) silinmiş veya erişilebilir değil ise, kullanılan bekleniyor. Bu, Azure Site Kurtarma'dan (Faturalaması durdurulur) çoğaltılmış öğeyi kaldırır. Şirket içi sanal makinesinde çoğaltma yapılandırması **almayacak** temizlendi. Çoğaltma ayarları şirket içi sanal makinelerini Temizle komut aşağıdaki kümesini çalıştırın.
> [!NOTE]
> Seçerseniz **kaldırmak** aşağıdaki komutlar çoğaltma ayarlarını temizlemek için tun içi VMM sunucusu sonra seçeneği.

3. VMM konsolundan PowerShell (yönetici ayrıcalıklarına gerekli) kullanarak kaynak VMM sunucusunda bu komut dosyasını çalıştırın. Yer tutucu Değiştir **SQLVM1** , sanal makine adı.

         $vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection
4. İkincil VMM sunucusunda ikincil sanal makinenin ayarlarını temizlemek için bu komut dosyasını çalıştırın:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force
5. Böylece ikincil VM yeniden VMM konsolunda algılanan ikincil VMM sunucusunda Hyper-V ana bilgisayar sunucusunda sanal makineleri yenileyin.
6. VMM sunucusundaki çoğaltma ayarlarını yukarıdaki adımları temizleyin. Sanal makine için çoğaltma durdurmak istiyorsanız, aşağıdaki komut dosyasını birincil ve ikincil VM'ler çalıştırın. SQLVM1, sanal makine adı ile değiştirin.

        Remove-VMReplication –VMName “SQLVM1”




