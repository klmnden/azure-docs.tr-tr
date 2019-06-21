---
title: Ek - Iaas Vm'leri için Azure Disk şifrelemesi | Microsoft Docs
description: Bu makale için Microsoft Azure Disk şifrelemesi Windows ve Linux Iaas Vm'leri için ek niteliğindedir.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: 2872d106eea56a37c362195e7a3250058336768b
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295051"
---
# <a name="appendix-for-azure-disk-encryption"></a>Ek Azure Disk şifrelemesi 

Bu makale bir ek niteliğindedir [Iaas Vm'leri için Azure Disk şifrelemesi](azure-security-disk-encryption-overview.md). Iaas Vm'leri makaleleri ilk bağlamı anlamak için Azure Disk şifrelemesi okuduğunuzdan emin olun. Bu makalede önceden şifrelenmiş VHD'ler ve diğer görevleri hazırlamayı öğrenin.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="connect-to-your-subscription"></a>Aboneliğinize bağlanma
Başlamadan önce gözden [önkoşulları](azure-security-disk-encryption-prerequisites.md) makalesi. Tüm önkoşulları karşıladığınızdan sonra aşağıdaki cmdlet'leri çalıştırarak aboneliğinize bağlanın:

### <a name="bkmk_ConnectPSH"></a> PowerShell ile aboneliğinize bağlanma

1. Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:

     ```powershell
     Connect-AzAccount 
     ```
2. Birden çok aboneliğiniz varsa ve kullanmak üzere belirtmek istiyorsanız hesabınızın aboneliklerini görmek için aşağıdaki komutu yazın:
     
     ```powershell
     Get-AzSubscription
     ```
3. Kullanmak istediğiniz aboneliği belirtmek için şunu yazın:
 
     ```powershell
      Select-AzSubscription -SubscriptionName <Yoursubscriptionname>
     ```
4. Yapılandırılmış aboneliğin doğru olduğunu doğrulamak için şunu yazın:
     
     ```powershell
     Get-AzSubscription
     ```
5. Gerekirse, Azure AD'ye bağlanma [Connect-AzureAD](/powershell/module/azuread/connect-azuread).
     
     ```powershell
     Connect-AzureAD
     ```
6. Azure Disk şifrelemesi cmdlet'lerinin yüklü doğrulamak için şunu yazın:
     
     ```powershell
     Get-command *diskencryption*
     ```
                       
7. Gözden geçirme [Azure PowerShell'i kullanmaya başlama](/powershell/azure/get-started-azureps) ve [AzureAD](/powershell/module/azuread), gerekirse.

### <a name="bkmk_ConnectCLI"></a> Azure CLI ile aboneliğinize bağlanma

1. Azure ile oturum [az login](/cli/azure/authenticate-azure-cli#sign-in-interactively). 
     
     ```azurecli
     az login
     ```

2. Altında oturum açmak için bir kiracı seçmek istiyorsanız bu seçeneği kullanın:
    
     ```azurecli
     az login --tenant <tenant>
     ```

3. Birden çok aboneliğe ve belirli bir tanesini belirtmek isterseniz varsa, abonelik listeniz alma [az hesabı listesi](/cli/azure/account#az-account-list) ve belirlediğiniz [az hesabı kümesi](/cli/azure/account#az-account-set).
     
     ```azurecli
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. Yüklü sürümünü doğrulayın.
     
     ```azurecli
        az --version
     ``` 

5. Gözden geçirme [Azure CLI 2.0 ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli) gerekirse. 

## <a name="sample-powershell-scripts-for-azure-disk-encryption"></a>Azure Disk şifrelemesi için örnek PowerShell betikleri 

- **Aboneliğinizdeki tüm şifrelenmiş VM'lerin listesi**

     ```azurepowershell-interactive
     $osVolEncrypted = {(Get-AzVMDiskEncryptionStatus -ResourceGroupName $_.ResourceGroupName -VMName $_.Name).OsVolumeEncrypted}
     $dataVolEncrypted= {(Get-AzVMDiskEncryptionStatus -ResourceGroupName $_.ResourceGroupName -VMName $_.Name).DataVolumesEncrypted}
     Get-AzVm | Format-Table @{Label="MachineName"; Expression={$_.Name}}, @{Label="OsVolumeEncrypted"; Expression=$osVolEncrypted}, @{Label="DataVolumesEncrypted"; Expression=$dataVolEncrypted}
     ```

- **Bir anahtar Kasası'nda VM'ler şifrelemek için kullanılan tüm disk şifreleme gizli dizilerini listeleme** 

     ```azurepowershell-interactive
     Get-AzKeyVaultSecret -VaultName $KeyVaultName | where {$_.Tags.ContainsKey('DiskEncryptionKeyFileName')} | format-table @{Label="MachineName"; Expression={$_.Tags['MachineName']}}, @{Label="VolumeLetter"; Expression={$_.Tags['VolumeLetter']}}, @{Label="EncryptionKeyURL"; Expression={$_.Id}}
     ```

### <a name="bkmk_prereq-script"></a> Azure Disk şifrelemesi önkoşulları PowerShell Betiği kullanma
Zaten Azure Disk şifrelemesi önkoşulları alışık olduğunuz, kullanabileceğiniz [Azure Disk şifrelemesi önkoşulları PowerShell Betiği](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 ). Bu PowerShell Betiği kullanma örneği için bkz: [VM hızlı başlangıç şifrelemek](quick-encrypt-vm-powershell.md). Satırında 211, mevcut bir kaynak grubunda var olan VM'ler için tüm diskler şifrelemek için başlatma komut dosyasının bir bölümünden açıklamaları kaldırabilirsiniz. 

Aşağıdaki tabloda, PowerShell betik parametreleri kullanılabileceğini gösterir: 


|Parametre|Açıklama|Zorunludur|
|------|------|------|
|$resourceGroupName| Anahtar kasası ait olduğu kaynak grubunun adı.  Yoksa, bu ada sahip yeni bir kaynak grubu oluşturulur.| True|
|$keyVaultName|Şifreleme anahtarları, yerleştirilecek olan anahtar kasası adı. Yoksa, bu ada sahip yeni bir kasa oluşturulur.| True|
|$location|Anahtar kasası konumu. Anahtar kasası ve VM'lerin şifrelenmesini aynı konumda olduğundan emin olun. `Get-AzLocation` komutu ile bir konum listesi alın.|True|
|$subscriptionId|Kullanılacak Azure aboneliğini tanımlayıcısı.  Abonelik Kimliğinizi `Get-AzSubscription` komutu ile alabilirsiniz.|True|
|$aadAppName|Anahtar kasası için gizli anahtarları yazmak için kullanılacak Azure AD uygulamasının adı. Bu ada sahip bir uygulama yoksa yeni bir uygulama oluşturulur. Bu uygulama zaten varsa, aadClientSecret parametre betiğine geçirin.|False|
|$aadClientSecret|Daha önce oluşturulan Azure AD uygulamasının istemci gizli anahtarı.|False|
|$keyEncryptionKeyName|KeyVault, isteğe bağlı anahtar şifreleme anahtarı adı. Bu ada sahip yeni bir anahtar yoksa oluşturulur.|False|


## <a name="resource-manager-templates"></a>Resource Manager şablonları

<!--   - [Create a key vault](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create) -->

### <a name="encrypt-or-decrypt-vms-without-an-azure-ad-app"></a>Şifrelemek veya VM'lerin şifresini çözmek bir Azure AD uygulaması


- [Iaas Windows Vm'leri çalıştırma ya da mevcut disk şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad)
- [Iaas Windows Vm'leri çalıştırma ya da mevcut disk şifrelemeyi devre dışı bırakma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm-without-aad)
- [Var olan veya çalışan Iaas Linux VM'de disk şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad)  
  - [Çalışan bir Linux VM üzerinde şifrelemeyi devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) 
    - Şifreleme devre dışı bırakıldığında yalnızca veri birimlerinde Linux VM'ler için izin verilir.  

### <a name="encrypt-or-decrypt-virtual-machine-scale-sets"></a>Şifreleme veya şifrelerini çözme sanal makine ölçek kümeleri

- [Bir çalışan Linux sanal makine ölçek kümesinde disk şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)

- [Bir çalışan Windows sanal makine ölçek kümesinde disk şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-windows)

  - [Linux Vm'leri Linux VMSS bir Sıçrama kutusu ve etkinleştirir şifreleme ile bir sanal makine ölçek kümesini dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox)

  - [Windows Vm'leri Windows VMSS bir Sıçrama kutusu ve etkinleştirir şifreleme ile bir sanal makine ölçek kümesini dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox)

- [Bir çalışan Linux sanal makine ölçek kümesinde disk şifrelemeyi devre dışı bırakma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

- [Bir çalışan Windows sanal makine ölçek kümesinde disk şifrelemeyi devre dışı bırakma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows)

### <a name="encrypt-or-decrypt-vms-with-an-azure-ad-app-previous-release"></a>Şifrelemek veya Vm'leri bir Azure AD uygulamasını (önceki sürüm) şifresini çözme 
 
- [Iaas Windows Vm'leri çalıştırma ya da mevcut disk şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm)

- [Var olan veya çalışan Iaas Linux VM'de disk şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm)    

- [Windows Iaas çalıştırma disk şifrelemeyi devre dışı bırakma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm) 

-  [Çalışan bir Linux VM üzerinde şifrelemeyi devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm) 
    - Şifreleme devre dışı bırakıldığında yalnızca veri birimlerinde Linux VM'ler için izin verilir. 

- [Marketten yeni Iaas Windows VM'de disk şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image)
    - Bu şablon, Windows Server 2012 galeri görüntüsü kullanan bir yeni şifrelenmiş Windows VM oluşturur.

- [Bir yeni şifrelenmiş Windows Iaas yönetilen Disk galeri görüntüsünden VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)
    - Bu şablon, Windows Server 2012 galeri görüntüsü kullanarak yönetilen disklerle şifrelenmiş yeni bir Windows VM oluşturur.

- [Önceden şifrelenmiş bir VHD/storage blobundan yeni şifrelenmiş yönetilen disk oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)
    - Önceden şifrelenmiş bir VHD ve karşılık gelen şifreleme ayarları ile sağlanan yeni şifrelenmiş yönetilen disk oluşturur

- [Bir Azure AD İstemci sertifikası parmak izi kullanarak çalışan bir Windows VM'de disk şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-aad-client-cert)
    


## <a name="bkmk_preWin"></a> Önceden şifrelenmiş bir Windows VHD hazırlama
Aşağıdaki bölümlerde, önceden şifrelenmiş bir Windows VHD Azure Iaas içinde şifrelenmiş bir VHD olarak dağıtımına hazırlamak gereklidir. Hazırlama ve Azure Site Recovery ya da Azure üzerinde bir yeni Windows VM (VHD) önyükleme bilgileri kullanın. Hazırlama ve bir VHD'yi karşıya yükleme hakkında daha fazla bilgi için bkz. [genelleştirilmiş VHD yükleme ve Azure'da yeni VM'ler oluşturmak için bunu kullanın](../virtual-machines/windows/upload-generalized-managed.md).

### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>TPM olmayan işletim sistemi koruma için izin vermek için Grup İlkesi güncelleştirme
BitLocker Grup İlkesi ayarını yapılandırma **BitLocker Sürücü Şifrelemesi**, hangi altında bulabilirsiniz **yerel bilgisayar ilkesi** > **Bilgisayar Yapılandırması**  >  **Yönetim Şablonları** > **Windows bileşenleri**. Bu ayarı değiştirmek **işletim sistemi sürücüleri** > **başlangıçta ek kimlik doğrulaması gerektiren** > **uyumluTPM'sizBitLockerizinver**aşağıdaki şekilde gösterildiği gibi:

![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

### <a name="install-bitlocker-feature-components"></a>BitLocker özellik bileşenlerini yükleme
Windows Server 2012 ve daha sonra aşağıdaki komutu kullanın:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

Windows Server 2008 R2 için aşağıdaki komutu kullanın:

    ServerManagerCmd -install BitLockers
### <a name="prepare-the-os-volume-for-bitlocker-by-using-bdehdcfg"></a>İşletim sistemi birimi için BitLocker'ı kullanarak hazırlama `bdehdcfg`
İşletim sistemi bölümü sıkıştırma ve makine BitLocker için hazırlama için yürütme [bdehdcfg](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-basic-deployment) gerekirse:

    bdehdcfg -target c: shrink -quiet 


### <a name="protect-the-os-volume-by-using-bitlocker"></a>BitLocker'ı kullanarak işletim sistemi birimi koruma
Kullanım [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) dış bir anahtar koruyucusu kullanılarak önyükleme birimi şifrelemesini etkinleştirmek için komutu. Ayrıca dış sürücü veya birim üzerinde dış anahtarı (.bek dosyası) getirin. Şifreleme sistem/önyükleme biriminde bir sonraki yeniden başlatmada etkinleştirilir.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> BitLocker'ı kullanarak dış anahtarı almak için ayrı bir veri/kaynak VHD ile VM hazırlayın.

## <a name="bkmk_LinuxRunning"></a> Bir işletim sistemi sürücüsünde çalışan bir Linux VM şifreleme

### <a name="prerequisites-for-os-disk-encryption"></a>İşletim sistemi disk şifrelemesi önkoşulları

* VM işletim sistemi disk şifreleme ile uyumlu bir dağıtım bağlantısında listelendiği gibi kullanılmalıdır [Azure Disk şifrelemesi desteklenen işletim sistemleri: Linux](azure-security-disk-encryption-prerequisites.md#linux) 
* Azure Resource Manager'daki Market görüntüsünden VM yeniden oluşturulması gerekir.
* En az 4 GB RAM ile Azure VM (boyutudur, 7 GB önerilir).
* (RHEL ve CentOS) SELinux devre dışı bırakın. SELinux devre dışı bırakmak için "4.4.2. bkz. SELinux devre dışı bırakma" [SELinux kullanıcı ve Yönetici Kılavuzu](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) VM üzerinde.
* SELinux devre dışı bıraktıktan sonra VM'yi en az bir kez yeniden başlatın.

### <a name="steps"></a>Adımlar
1. Daha önce belirtilen dağıtımları birini kullanarak bir VM oluşturun.

   CentOS 7.2 için işletim sistemi disk şifreleme özel bir görüntü desteklenir. Bu görüntü kullanmak için VM'yi oluştururken "7.2n" SKU olarak belirtin:

   ```powershell
    Set-AzVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
   ```
2. Sanal makine gereksinimlerinize göre yapılandırın. Gideceğinizi varsa tüm (işletim sistemi ve veri) şifrelemek için sürücüleri, veri sürücüleri belirtilen ve gelen bağlanabilir /etc/fstab olmanız gerekir.

   > [!NOTE]
   > UUID kullanmak =... veri sürücülerinde blok cihaz adı (örneğin, / dev/sdb1) belirtmek yerine/etc/fstab dosyasında belirtmek için. Şifreleme sırasında sanal makinede sürücüleri sırasını değiştirir. Sanal makinenizin blok cihazları belirli bir sırada dayanıyorsa, şifreleme sonrasında bağlamak başarısız olur.

3. SSH oturumları dışında oturum açın.

4. İşletim sistemi şifrelemek için volumeType olarak belirtin. **tüm** veya **işletim sistemi** şifreleme etkinleştirdiğinizde.

   > [!NOTE]
   > Olarak çalışan tüm kullanıcı alanı işlemleri `systemd` Hizmetleri sonlandırılan ile bir `SIGKILL`. VM'yi yeniden başlatın. VM kapalı kalma süresi üzerinde çalışan bir VM'deki işletim sistemi disk şifreleme etkinleştirdiğinizde planlayın.

5. Düzenli aralıklarla içindeki yönergeleri kullanarak şifreleme ilerleyişini izlemek [sonraki bölümde](#monitoring-os-encryption-progress).

6. Get-AzVmDiskEncryptionStatus "VMRestartPending" gösterdikten sonra oturum açma veya portal, PowerShell veya CLI kullanarak VM'nizi yeniden başlatın.
    ```powershell
    C:\> Get-AzVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot the VM
    ```
   Yeniden önce kaydetmeniz önerilir [önyükleme tanılaması](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) VM.

## <a name="monitoring-os-encryption-progress"></a>İşletim sistemi şifreleme ilerlemesini izleme
İşletim sistemi şifreleme ilerleme üç yolla izleyebilirsiniz:

* Kullanım `Get-AzVmDiskEncryptionStatus` cmdlet'i ve ProgressMessage alanın inceleyin:
    ```powershell
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
  "Şifreleme başlatılan işletim sistemi diski" VM ulaştıktan sonra yaklaşık 40 ila 50 dakika sürer. VM üzerinde bir Premium depolama desteklenir.

  Nedeniyle [sorun #388](https://github.com/Azure/WALinuxAgent/issues/388) WALinuxAgent içinde `OsVolumeEncrypted` ve `DataVolumesEncrypted` olarak görünmesini `Unknown` bazı dağıtımları içinde. WALinuxAgent 2.1.5 sürümü ile ve daha sonra bu sorun otomatik olarak düzeltildi. Görürseniz `Unknown` çıktıda Azure kaynak Gezgini kullanarak disk şifreleme durumunu doğrulayabilirsiniz.

  Git [Azure kaynak Gezgini](https://resources.azure.com/)ve ardından sol taraftaki seçim paneli bu hiyerarşide genişletin:

  ~~~~
  |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
  ~~~~                

  Instanceview içinde sürücülerinizin şifreleme durumunu görmek için aşağı kaydırın.

  ![Sanal makine örnek görünümü](./media/azure-security-disk-encryption/vm-instanceview.png)

* Bakmak [önyükleme tanılaması](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/). İletileri ADE uzantıdan önekiyle `[AzureDiskEncryption]`.

* Sanal makineye SSH aracılığıyla oturum açın ve uzantı günlüğünden alın:

    /var/log/Azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

  VM için işletim sistemi şifreleme işlemi devam ederken oturum emin değilseniz öneririz. Diğer iki yöntemden yalnızca başarısız olan günlükler kopyalayın.

## <a name="bkmk_preLinux"></a> Önceden şifrelenmiş bir Linux VHD hazırlama
Önceden şifrelenmiş VHD'ler için hazırlık, dağıtım bağlı olarak farklılık gösterebilir. Hazırlama örnekler [Ubuntu 16](#bkmk_Ubuntu), [openSUSE 13.2](#bkmk_openSUSE), ve [CentOS 7](#bkmk_CentOS) kullanılabilir. 

### <a name="bkmk_Ubuntu"></a> Ubuntu 16
Şifreleme dağıtım yüklemesi sırasında aşağıdaki adımları uygulayarak yapılandırın:

1. Seçin **yapılandırma şifrelenmiş birimleri** bölümleme zaman diskleri.

   ![Ubuntu 16.04 Kurulumu - şifrelenmiş hacimlerini yapılandırma](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Şifrelenmemiş olması bir ayrı önyükleme sürücüsü oluşturun. Kök drive'ınızdaki şifreleyin.

   ![Ubuntu 16.04 Kurulumu - şifrelemek için cihazları seçin](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Bir parola girin. Anahtar kasasına yüklenmiş parolayı budur.

   ![Ubuntu 16.04 Kurulumu - parola girin](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Bölümleme tamamlayın.

   ![Ubuntu 16.04 Kurulumu - son bölümleme](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. VM'yi önyüklemek için bir parola istendiğinde, 3. adımda belirttiğiniz parolayı kullanın.

   ![Ubuntu 16.04 Kurulumu - önyüklemede parolayı girin](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. VM, Azure kullanarak yüklemek için hazırlama [bu yönergeleri](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-ubuntu/). (VM sağlamayı kaldırma) işleminin son adımında çalıştırma henüz.

Şifreleme aşağıdaki adımları uygulayarak Azure ile çalışacak şekilde yapılandırın:

1. Aşağıdaki betiği içerikle /usr/local/sbin/azure_crypt_key.sh altında bir dosya oluşturun. Azure tarafından kullanılan parola dosya adı olduğundan KeyFileName dikkat edin.

    ```bash
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do

        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
   ```

2. Şifreli yapılandırmada değişiklik */etc/crypttab*. Şu şekilde görünmelidir:
   ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. Düzenleme yapıyorsanız *azure_crypt_key.sh* Linux çalıştırmak için Windows ve, kopyalarsınız `dos2unix /usr/local/sbin/azure_crypt_key.sh`.

4. Yürütülebilir izinleri komut dosyasına ekleyin:
   ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
   ```
5. Düzen */etc/initramfs-tools/modules* satırları ekleyerek:
   ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
   ```
6. Çalıştırma `update-initramfs -u -k all` yapmak initramfs güncelleştirilecek `keyscript` etkili.

7. Artık VM yetkisini kaldırabilirsiniz.

   ![Ubuntu 16.04 kurulumu - güncelleştirme initramfs](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Sonraki adıma devam edin ve VHD'nizi Azure'a yükleyin.

### <a name="bkmk_openSUSE"></a>  openSUSE 13.2
Dağıtım yüklenmesi sırasında şifreleme yapılandırmak için aşağıdaki adımları uygulayın:
1. Diskleri bölümlemek bittiğinde **şifrelemek birim grubu**ve ardından bir parola girin. Bu anahtar kasanız için karşıya yükleyelim paroladır.

   ![openSUSE 13.2 Kurulumu - şifreleme birim grubu](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Parolanızı kullanarak VM'yi önyükleyin.

   ![openSUSE 13.2 Kurulumu - önyüklemede parolayı girin](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. VM içindeki yönergeleri izleyerek Azure'a yüklemek için hazırlama [Azure için SLES veya openSUSE sanal makine hazırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131). (VM sağlamayı kaldırma) işleminin son adımında çalıştırma henüz.

Azure ile çalışacak şekilde şifrelemesini yapılandırmak için aşağıdaki adımları uygulayın:
1. /Etc/dracut.conf düzenleyin ve aşağıdaki satırı ekleyin:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Bu satırları açıklama satırı yapar dosya /usr/lib/dracut/modules.d/90crypt/module-setup.sh sonunda:
   ```bash
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
   ```

3. Dosya /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh başındaki aşağıdaki satırı ekleyin:
   ```bash
    DRACUT_SYSTEMD=0
   ```
   Ve tüm oluşumlarını değiştirin:
   ```bash
    if [ -z "$DRACUT_SYSTEMD" ]; then
   ```
   yerine şunu yazın:
   ```bash
    if [ 1 ]; then
   ```
4. /Usr/lib/dracut/Modules.d/90crypt/cryptroot-ASK.sh düzenleyin ve "# açık LUKS cihaza" ekleyin:

    ```bash
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```
5. Çalıştırma `/usr/sbin/dracut -f -v` initrd güncelleştirilecek.

6. Artık VM'nin sağlamasını kaldırmak ve Azure'a VHD'nizi karşıya yükleyin.

### <a name="bkmk_CentOS"></a> CentOS 7
Dağıtım yüklenmesi sırasında şifreleme yapılandırmak için aşağıdaki adımları uygulayın:
1. Seçin **verilerimi şifrelemek** bölümleme zaman diskleri.

   ![CentOS 7 Kurulumu - yükleme hedefi](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Emin **şifrele** kök bölümü için seçilir.

   ![CentOS 7 Kurulumu - şifrelemek için kök bölümü seçin](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Bir parola girin. Anahtar kasanız için karşıya yükleyelim parolayı budur.

   ![CentOS 7 Kurulumu - parola girin](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. VM'yi önyüklemek için bir parola istendiğinde, 3. adımda belirttiğiniz parolayı kullanın.

   ![CentOS 7 Kurulumu - önyükleme parola girin](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. VM içindeki "CentOS 7.0 +" yönergeleri kullanarak Azure'a yüklemek için hazırlama [Azure'da CentOS tabanlı bir sanal makine hazırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70). (VM sağlamayı kaldırma) işleminin son adımında çalıştırma henüz.

6. Artık VM'nin sağlamasını kaldırmak ve Azure'a VHD'nizi karşıya yükleyin.

Azure ile çalışacak şekilde şifrelemesini yapılandırmak için aşağıdaki adımları uygulayın:

1. /Etc/dracut.conf düzenleyin ve aşağıdaki satırı ekleyin:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Bu satırları açıklama satırı yapar dosya /usr/lib/dracut/modules.d/90crypt/module-setup.sh sonunda:
   ```bash
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
   ```

3. Dosya /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh başındaki aşağıdaki satırı ekleyin:
   ```bash
    DRACUT_SYSTEMD=0
   ```
   Ve tüm oluşumlarını değiştirin:
   ```bash
    if [ -z "$DRACUT_SYSTEMD" ]; then
   ```
   -
   ```bash
    if [ 1 ]; then
   ```
4. /Usr/lib/dracut/Modules.d/90crypt/cryptroot-ASK.sh düzenleyin ve sonra "# açık LUKS cihaz" aşağıdakini ekleyin:
    ```bash
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```    
5. Çalıştır "/ usr/sbin/dracut - f - v" initrd güncelleştirilecek.

![CentOS 7 Kurulumu - /usr/sbin/dracut -f - v çalıştıran](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

## <a name="bkmk_UploadVHD"></a> Azure depolama hesabınız için şifrelenmiş VHD yükleme
BitLocker şifrelemesi veya DM-Crypt şifrelemesi etkinleştirildikten sonra depolama hesabınıza yüklenmek üzere yerel şifrelenmiş VHD gerekir.
```powershell
    Add-AzVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]
```
## <a name="bkmk_UploadSecret"></a> Gizli anahtar kasanıza önceden şifrelenmiş VM için karşıya yükleme
Bir Azure AD uygulamasını (önceki sürüm) kullanarak şifreleme, daha önce edindiğiniz disk şifreleme gizli anahtar kasanızdaki gizli dizi olarak yüklenmelidir. Anahtar kasası disk şifrelemesi ve Azure AD istemciniz için etkinleştirilmiş olmalıdır.

```powershell 
 $AadClientId = "My-AAD-Client-Id"
 $AadClientSecret = "My-AAD-Client-Secret"

 $key vault = New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

 Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
 Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption
``` 

### <a name="bkmk_SecretnoKEK"></a> Disk şifreleme gizli bilgisi bir KEK ile şifrelenmiş değil
Gizli anahtar kasanızdaki ayarlamak için kullanın [kümesi AzKeyVaultSecret](/powershell/module/az.keyvault/set-azkeyvaultsecret). Bir Windows sanal makine varsa, bek dosya kodlanmış bir base64 dizesi ve ardından kullanarak anahtar kasası karşıya `Set-AzKeyVaultSecret` cmdlet'i. Linux için parola base64 dizesi olarak kodlanmış ve daha sonra anahtar kasasına yüklenmiş. Ayrıca, anahtar kasasında gizli dizi oluşturduğunuzda, aşağıdaki etiketleri ayarlandığından emin olun.

#### <a name="windows-bek-file"></a>Windows BEK dosyası
```powershell
# Change the VM Name, key vault name, and specify the path to the BEK file.
$VMName ="MySecureVM"
$BEKFilepath = "C:\test\BEK\12345678-90AB-CDEF-A1B2-C3D4E5F67890A.BEK"
$VeyVaultName ="MySecureVault"

# Get the name of the BEK file from the BEK file path. This will be a tag for the key vault secret.
$BEKFileName =  Split-Path $BEKFilepath -Leaf

# These tags will be added to the key vault secret so you can easily see which BEK file belongs to which VM.
$tags = @{“MachineName” = “$VMName”;"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "$BEKFileName"}

# Convert the BEK file to a Base64 string.
$FileContentEncoded = [System.convert]::ToBase64String((Get-Content -Path $BEKFilepath -Encoding Byte))

# Create a new secret in the vault from the converted BEK file. 
# The file is converted to a secure string before import into the key vault

$SecretName = [guid]::NewGuid().ToString()
$SecureSecretValue = ConvertTo-SecureString $FileContentEncoded -AsPlainText -Force
$Secret = Set-AzKeyVaultSecret -VaultName $VeyVaultName -Name $SecretName -SecretValue $SecureSecretValue -tags $tags

# Show the secret's URL and store it as a variable. This is used as -DiskEncryptionKeyUrl in Set-AzVMOSDisk when you attach your OS disk. 
$SecretUrl=$secret.Id
$SecretUrl
```

#### <a name="linux"></a>Linux
```powershell

 # This is the passphrase that was provided for encryption during the distribution installation
 $passphrase = "contoso-password"

 $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
 $secretName = [guid]::NewGuid().ToString()
 $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
 $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

 $secret = Set-AzKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
 $secretUrl = $secret.Id
```


Kullanım `$secretUrl` için sonraki adımda [KEK kullanmadan işletim sistemi diskindeki](#bkmk_URLnoKEK).

### <a name="bkmk_SecretKEK"></a> Bir KEK ile şifrelenmiş disk şifreleme gizli bilgisi
Gizli anahtar Kasası'na yüklemeden önce bir anahtar şifreleme anahtarı kullanarak isteğe bağlı olarak şifreleyebilirsiniz. Kaydırma kullanın [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) ilk anahtar şifreleme anahtarı kullanarak şifrelemek için. Bir gizli dizi kullanarak daha sonra karşıya yükleyebilirsiniz base64 URL olarak kodlanmış dize çıktıdır bu kaydırma işleminin [ `Set-AzKeyVaultSecret` ](/powershell/module/az.keyvault/set-azkeyvaultsecret) cmdlet'i.

```powershell
    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    Add-AzKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id
```

Kullanım `$KeyEncryptionKey` ve `$secretUrl` için sonraki adımda [KEK kullanarak işletim sistemi diski](#bkmk_URLKEK).

##  <a name="bkmk_SecretURL"></a> Bir işletim sistemi diski, gizli bir URL belirtin

###  <a name="bkmk_URLnoKEK"></a>Bir KEK kullanmadan
İşletim sistemi diski iliştirmekte olsa da geçirmeniz gerekir `$secretUrl`. URL "Disk şifreleme gizli bir KEK ile şifrelenmiş değil" bölümünde oluşturuldu.
```powershell
    Set-AzVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl
```
### <a name="bkmk_URLKEK"></a>Kullanarak bir KEK
İşletim sistemi diski, pass `$KeyEncryptionKey` ve `$secretUrl`. URL "Disk şifreleme gizli bir KEK ile şifrelenmiş" bölümünde oluşturuldu.
```powershell
    Set-AzVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id
```
