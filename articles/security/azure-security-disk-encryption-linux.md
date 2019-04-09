---
title: Linux Iaas sanal makineleri için Azure Disk şifrelemesini etkinleştirme
description: Bu makalede, Linux Iaas sanal makineleri için Microsoft Azure Disk şifrelemesi etkinleştirme hakkında yönergeler sağlar.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 04/05/2019
ms.custom: seodec18
ms.openlocfilehash: 33011a419c8c966fc59b769106aaff428b2a0709
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59057685"
---
# <a name="enable-azure-disk-encryption-for-linux-iaas-vms"></a>Linux Iaas sanal makineleri için Azure Disk şifrelemesini etkinleştirme 

Çok sayıda disk şifreleme senaryoları etkinleştirebilirsiniz ve adımları senaryoya göre değişiklik gösterebilir. Aşağıdaki bölümlerde, Linux Iaas sanal makineleri için daha ayrıntılı senaryoları kapsar. Disk şifreleme kullanmadan önce [Azure Disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md) tamamlanması gerekir ve [Linux Iaas sanal makineleri için ek Önkoşullar](azure-security-disk-encryption-prerequisites.md#bkmk_LinuxPrereq) bölümü gözden.

Ele bir [anlık görüntü](../virtual-machines/windows/snapshot-copy-managed-disk.md) ve/veya diskler şifrelenir önce yedekleyin. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata oluşması durumunda olası olduğundan emin olun. Şifreleme gerçekleşmeden önce yönetilen disklere sahip VM'ler yedeklemesini gerektirir. Bir yedekleme yapıldıktan sonra - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için Set-AzVMDiskEncryptionExtension cmdlet'ini kullanabilirsiniz. Yedekleme ve şifrelenmiş Vm'leri geri yükleme hakkında daha fazla bilgi için bkz. [Azure Backup](../backup/backup-azure-vms-encryption.md) makalesi. 

>[!WARNING]
> - Daha önce kullandıysanız [Azure AD uygulaması ile Azure Disk şifrelemesi](azure-security-disk-encryption-prerequisites-aad.md) bu sanal Makineyi şifrelemek için devam gerekecektir VM'nizi şifrelemek için bu seçeneği kullanın. Kullanamazsınız [Azure Disk şifrelemesi](azure-security-disk-encryption-prerequisites.md) bu desteklenen bir senaryo değildir gibi şifrelenmiş bu VM üzerinde bu VM şifreli için AAD uygulaması uzağa anlamı geçiş henüz desteklenmiyor.
 > - Azure Disk şifrelemesi, anahtar kasası ve VM'lerin aynı bölgede bulunması gerekir. Oluşturun ve şifrelenmiş VM ile aynı bölgede olan bir anahtar Kasası'nı kullanın.
> - Linux işletim sistemi birimlerinin şifrelerken sanal Makineyi kullanılamaz kabul edilmelidir. Şifreleme ilerlerken şifreleme işlemi sırasında erişilmesi gereken açık dosyaları engelleme sorunlarını önlemek için SSH oturumu açma önlemek için önerilir. İlerleme durumunu denetlemek için [Get-AzVMDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) veya [vm şifreleme show](/cli/azure/vm/encryption#az-vm-encryption-show) komutları kullanılabilir. Bu işlem birkaç saat 30 GB işletim sistemi birimi yanı sıra, veri hacimleri şifrelemek için ek süre olması için beklenebilir. Verileri toplu şifreleme süresi için boyutuyla orantılı olur ve veri birimlerini miktarını şifrele biçimlendirme sürece tüm seçeneği kullanılır. 
> - Linux vm'lerinde şifreleme devre dışı bırakıldığında, yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifreli değilse veri veya işletim sistemi birimleri üzerinde desteklenmiyor.  

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="bkmk_RunningLinux"> </a> Var olan veya çalışan bir Iaas Linux VM üzerinde şifrelemeyi etkinleştir
Bu senaryoda, Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleyebilirsiniz. Sanal makine uzantısı için şema bilgileri gerekiyorsa bkz [Linux uzantısı için Azure Disk şifrelemesi](../virtual-machines/extensions/azure-disk-enc-linux.md) makalesi.

>[!IMPORTANT]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra Set-AzVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, karşı temel alan bir yönetilen disk Vm'leri yedekleme yapılan ve bu parametre belirtilen kadar başarısız olacak. 
>
>Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 
>

### <a name="bkmk_RunningLinuxCLI"> </a>Bir var olan veya çalışan Azure CLI kullanarak Linux VM üzerinde şifrelemeyi etkinleştir 
Disk şifrelemesi yüklenerek ve kullanılarak şifrelenmiş VHD'nizi etkinleştirebilirsiniz [Azure CLI 2.0](/cli/azure) komut satırı aracı. [Azure Cloud Shell](../cloud-shell/overview.md) ile tarayıcınızda kullanabilir veya yerel makinenize yükleyip herhangi bir PowerShell oturumunda kullanabilirsiniz. Var olan veya Iaas sanal makineleri Azure'da çalışan şifrelemeyi etkinleştirmek için aşağıdaki CLI komutları kullanın:

Kullanım [az vm şifrelemeyi etkinleştirme](/cli/azure/vm/encryption#az-vm-encryption-enable) azure'da çalıştırılan Iaas sanal makine üzerinde şifrelemeyi etkinleştirmek için komutu.

- **Çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **KEK kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br>
Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Diskler şifrelenir doğrulayın:** Iaas VM şifreleme durumunu denetlemek için kullanmak [az vm şifreleme show](/cli/azure/vm/encryption#az-vm-encryption-show) komutu. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
     ```

- **Şifreleme devre dışı bırakın:** Şifreleme devre dışı bırakmak için [az vm şifreleme devre dışı](/cli/azure/vm/encryption#az-vm-encryption-disable) komutu. Şifreleme devre dışı bırakıldığında yalnızca veri birimlerinde Linux VM'ler için izin verilir.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type DATA
     ```

### <a name="bkmk_RunningLinuxPSH"> </a> Bir var olan veya çalışan PowerShell kullanarak Linux VM üzerinde şifrelemeyi etkinleştir
Kullanım [kümesi AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) azure'da çalıştırılan Iaas sanal makine şifrelemesini etkinleştirmek için cmdlet'i. Ele bir [anlık görüntü](../virtual-machines/windows/snapshot-copy-managed-disk.md) ve/veya sanal makine ile yedeklemek [Azure Backup](../backup/backup-azure-vms-encryption.md) önce diskler şifrelenir. Çalışan bir Linux VM şifrelemek için PowerShell betiklerini - skipVmBackup parametresi zaten belirtilmiş.

-  **Çalışan bir VM şifrele:** Aşağıdaki komut dosyası değişkenleri başlatır ve Set-AzVMDiskEncryptionExtension cmdlet'ini çalıştırır. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup MySecureVM ve MySecureVault değerleriniz ile değiştirin. Şifreleme hangi disklerin belirtmek için - VolumeType parametresini değiştirin.

     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $sequenceVersion = [Guid]::NewGuid();  

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType '[All|OS|Data]' -SequenceVersion $sequenceVersion -skipVmBackup;
     ```
- **KEK kullanarak çalışan bir VM şifrele:** Veri diskleri ve işletim sistemi disk şifreleme, - VolumeType parametresi eklemeniz gerekebilir. 

     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MyExtraSecureVM';
      $KeyVaultName = 'MySecureVault';
      $keyEncryptionKeyName = 'MyKeyEncryptionKey';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
      $sequenceVersion = [Guid]::NewGuid();  

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType '[All|OS|Data]' -SequenceVersion $sequenceVersion -skipVmBackup;
     ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 
    
- **Diskler şifrelenir doğrulayın:** Iaas VM şifreleme durumunu denetlemek için kullanmak [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet'i. 
    
     ```azurepowershell-interactive 
     Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```
    
- **Disk şifreleme devre dışı bırakın:** Şifreleme devre dışı bırakmak için [devre dışı bırak AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'i. Şifreleme devre dışı bırakıldığında yalnızca veri birimlerinde Linux VM'ler için izin verilir.
     
     ```azurepowershell-interactive 
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```


### <a name="bkmk_RunningLinux"> </a> Var olan veya çalışan Iaas Linux VM'de bir şablonla şifrelemeyi etkinleştir

Var olan veya çalışan Iaas Linux VM'de Azure disk şifrelemesi kullanarak etkinleştirebilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad).

1. Tıklayın **azure'a Dağıt** Azure Hızlı Başlangıç şablonu.

2. Abonelik, kaynak grubu, kaynak grubu konumu, parametreleri, yasal koşulları ve Sözleşmesi'ni seçin. Tıklayın **Oluştur** var olan veya çalışan Iaas VM üzerinde şifrelemeyi etkinleştirmek için.

Resource Manager şablonu parametreleri, mevcut veya sanal makineleri çalıştırmak için aşağıdaki tabloda listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| vmName | Şifreleme işlemi çalıştırmak için sanal makinenin adı. |
| keyVaultName | BitLocker anahtarı karşıya yüklenmelidir anahtar kasasının adı. Cmdlet'ini kullanarak elde edeceğinizi `(Get-AzKeyVault -ResourceGroupName <MyKeyVaultResourceGroupName>). Vaultname` veya Azure CLI komutu `az keyvault list --resource-group "MyKeyVaultResourceGroupName"`|
| keyVaultResourceGroup | Anahtar kasasını içeren kaynak grubunun adı|
|  KeyEncryptionKeyURL | Oluşturulan BitLocker anahtarı şifrelemek için kullanılan anahtar şifreleme anahtarı URL'si. Bu seçerseniz isteğe bağlı bir parametredir **nokek** UseExistingKek aşağı açılan listesinde. Seçerseniz **kek** UseExistingKek aşağı açılan listeden girmelisiniz _keyEncryptionKeyURL_ değeri. |
| VolumeType | Şifreleme işlemi gerçekleştirilir birim türü. Geçerli değerler _işletim sistemi_, _veri_, ve _tüm_. 
| forceUpdateTag | İşlemi çalıştırmak olması zorlamak gereken her zaman bir GUID gibi benzersiz bir değer geçirin. |
| resizeOSDisk | Sistem birimi bölme önce tam işletim sistemi VHD'si kaplayacak şekilde işletim sistemi bölümü yeniden boyutlandırılması. |
| location | Tüm kaynaklar için konum. |



## <a name="encrypt-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri şifrele
[Azure sanal makine ölçek kümeleri](../virtual-machine-scale-sets/overview.md) oluşturma ve yönetme bir grup özdeş, yük dengeli sanal makineler sağlar. Tanımlı bir zamanlamaya veya talebe yanıt olarak sanal makine örneği sayısı otomatik olarak artabilir ya da azalabilir. Sanal makine ölçek kümeleri şifrelemek için CLI veya Azure PowerShell kullanın. Yalnızca şifreleme veri disklerinin Linux ölçek kümesi sanal makinelerde desteklenir.

Linux ölçek kümesi veri disk şifreleme için bir toplu iş dosyası örneği bulunabilir [burada](https://github.com/Azure-Samples/azure-cli-samples/tree/master/disk-encryption/vmss). Bu örnek, bir kaynak grubu, Linux ölçek kümesi oluşturur, 5 GB'lık veri diskini bağlar ve sanal makine ölçek kümesi şifreler.

### <a name="encrypt-virtual-machine-scale-sets-with-azure-cli"></a>Azure CLI ile sanal makine ölçek kümeleri şifrele

Kullanım [az vmss şifrelemeyi etkinleştirme](/cli/azure/vmss/encryption#az-vmss-encryption-enable) bir Windows sanal makine ölçek kümesi şifrelemesini etkinleştirmek için. Yükseltme İlkesi el ile ölçek kümesi, şifreleme ile Başlat [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances). Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır. 

-  **Çalışan bir sanal makine ölçek kümesi şifrele**
    ```azurecli-interactive
    az vmss encryption enable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss" --volume-type DATA --disk-encryption-keyvault "MySecureVault"
    ```

-  **Şifreleme anahtarı sarmalama için KEK kullanarak çalışan bir sanal makine ölçek**
     ```azurecli-interactive
     az vmss encryption enable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss" --volume-type DATA --disk-encryption-keyvault "MySecureVault" --key-encryption-key "MyKEK" --key-encryption-keyvault "MySecureVault"
     ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Bir sanal makine ölçek kümesi için şifreleme durumunu alın:** Kullanım [az vmss şifreleme Göster](/cli/azure/vmss/encryption#az-vmss-encryption-show)

    ```azurecli-interactive
    az vmss encryption show --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss"
    ```

- **Bir sanal makine ölçek kümesi üzerinde şifrelemeyi devre dışı**: Kullanım [az vmss şifrelemeyi devre dışı bırakma](/cli/azure/vmss/encryption#az-vmss-encryption-disable)
    ```azurecli-interactive
    az vmss encryption disable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss"
    ```

### <a name="encrypt-virtual-machine-scale-sets-with-azure-powershell"></a>Azure PowerShell ile sanal makine ölçek kümeleri şifrele

Kullanım [kümesi AzVmssDiskEncryptionExtension](/powershell/module/az.compute/set-azvmssdiskencryptionextension) bir Windows sanal makine ölçek kümesi şifrelemesini etkinleştirmek için cmdlet'i. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır.

-  **Çalışan bir sanal makine ölçek kümesi şifrelemek**:
      ```powershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMSSRGname = 'MyVMScaleSetResourceGroup';
      $VmssName = "MySecureVmss";
      $KeyVaultName= "MySecureVault";
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      Set-AzVmssDiskEncryptionExtension -ResourceGroupName $VMSSRGname -VMScaleSetName $VmssName -VolumeType Data -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
      ```

-  **Şifreleme anahtarı sarmalama için KEK kullanarak çalışan bir sanal makine ölçek**:
      ```powershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMSSRGname = 'MyVMScaleSetResourceGroup';
      $VmssName = "MySecureVmss";
      $KeyVaultName= "MySecureVault";
      $keyEncryptionKeyName = "MyKeyEncryptionKey";
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $KeyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
      Set-AzVmssDiskEncryptionExtension -ResourceGroupName $VMSSRGname -VMScaleSetName $VmssName -VolumeType Data -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl  -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $KeyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
      ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Bir sanal makine kümesi için şifreleme durumunu Al**: Kullanım [Get-AzVmssVMDiskEncryption](/powershell/module/az.compute/get-azvmssvmdiskencryption) cmdlet'i.
    
    ```powershell
    Get-AzVmssVMDiskEncryption -ResourceGroupName "MyVMScaleSetResourceGroup" -VMScaleSetName "MySecureVmss"
    ```

- **Bir sanal makine ölçek kümesi üzerinde şifrelemeyi devre dışı**: Kullanım [devre dışı bırak AzVmssDiskEncryption](/powershell/module/az.compute/disable-azvmssdiskencryption) cmdlet'i. 

    ```powershell
    Disable-AzVmssDiskEncryption -ResourceGroupName "MyVMScaleSetResourceGroup" -VMScaleSetName "MySecureVmss"
    ```

### <a name="azure-resource-manager-templates-for-linux-virtual-machine-scale-sets"></a>Linux sanal makine ölçek kümesi için Azure Resource Manager şablonları ayarlar

Şifrelemek veya Linux sanal makine ölçek şifresini çözmek için ayarlar, Azure Resource Manager şablonları ve aşağıdaki yönergeleri kullanın:

- [Bir Linux sanal makine ölçek kümesi şifrelemeyi etkinleştirin](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)
- [Linux sanal makineleri bir Sıçrama kutusu ile bir VM ölçek kümesini dağıtmak ve Linux VM ölçek kümesi şifrelemeyi etkinleştirin](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox)
- [Bir Linux VM ölçek kümesinde şifrelemeyi devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

     1. **Azure’a dağıt**’a tıklayın.
     2. Gerekli alanları doldurun, sonra hüküm ve koşulları kabul etmiş olursunuz.
     3. Tıklayın **satın alma** kullanarak şablonu dağıtabilirsiniz.

## <a name="bkmk_EFA"> </a>Linux Iaas sanal makinesi, veri diskleri için EncryptFormatAll özelliğini kullanın

**EncryptFormatAll** parametresi şifrelenmesini Linux veri diskleri için zaman azaltır. Belirli bir ölçüte uyan bölümleri (kendi geçerli dosya sistemi ile) biçimlendirilir. Ardından bunlar geri bu komutu yürütmeden önce olduğu için yeniden bağlanması. Ölçütleri karşılayan bir veri diski dışlamak istiyorsanız, komutu çalıştırmadan önce çıkarma.

 Bu komutu çalıştırdıktan sonra bağlı herhangi bir sürücü önceden biçimlendirilir. Ardından üzerine artık boş sürücü şifreleme katmanı başlatılacak. Bu seçenek belirlendiğinde VM'ye bağlı kısa ömürlü kaynak diski de şifrelenir. Kısa ömürlü sürücü sıfırlarsanız, yeniden biçimlendirildi ve sanal makine için Azure Disk şifrelemesi çözümü sonraki fırsatta yeniden şifrelenir.

>[!WARNING]
> Bir sanal makinenin veri birimlerinde gerekli verileri olduğunda EncryptFormatAll kullanılmamalıdır. Çıkarma tarafından şifrelemeden diskler dışarıda bırakılabilir. İlk önce bir test sanal makinesi üzerinde EncryptFormatAll denemek, özellik parametre ve VM üretimde denemeden önce itiraz hakkının düşmesi anlamak. EncryptFormatAll seçeneği veri diski biçimlendirir ve tüm veriler kaybolacak. Devam etmeden önce hariç tutmak istediğiniz disklerin düzgün şekilde takılmamış olduğunu doğrulayın. </br></br>
 >Bu parametre şifreleme ayarları güncelleştirilirken tutunun, gerçek şifrelemeyi önce yeniden başlatma için neden olabilir. Bu durumda, biçimlendirilmiş fstab dosyasını istemediğiniz diski kaldırmak ayrıca isteyeceksiniz. Benzer şekilde, şifreleme işlemi başlatmadan önce şifrelemek için fstab dosyasını biçimli istediğiniz bölümü eklemeniz gerekir. 

### <a name="bkmk_EFACriteria"> </a> EncryptFormatAll ölçütleri
Parametresi, ancak tüm bölümleri geçerlidir ve karşıladıkları sürece şifreler **tüm** aşağıdaki ölçütlerden biri: 
- Bir işletim sistemi/kök/önyükleme bölümü değildir
- Zaten şifreli değil
- BEK birimi değil
- Bir RAID birimine değil
- LVM'yi birimi değil
- Bağlanmıştır

RAID veya LVM birimin yerine RAID veya LVM birimin oluşturan diskleri şifreleme.

### <a name="bkmk_EFAPSH"> </a> Azure CLI ile EncryptFormatAll parametresini kullanın
Kullanım [az vm şifrelemeyi etkinleştirme](/cli/azure/vm/encryption#az-vm-encryption-enable) azure'da çalıştırılan Iaas sanal makine üzerinde şifrelemeyi etkinleştirmek için komutu.

-  **EncryptFormatAll kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --encrypt-format-all
     ```

### <a name="bkmk_EFAPSH"> </a> Bir PowerShell cmdlet'i ile EncryptFormatAll parametresini kullanın
Kullanım [kümesi AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet'i EncryptFormatAll parametresine sahip. 

**EncryptFormatAll kullanarak çalışan bir VM şifrele:** Örneğin, aşağıdaki betiği değişkenlerinizi başlatır ve Set-AzVMDiskEncryptionExtension cmdlet'i EncryptFormatAll parametresi ile çalıştırır. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup MySecureVM ve MySecureVault değerleriniz ile değiştirin.
  
```azurepowershell
$KVRGname = 'MyKeyVaultResourceGroup';
$VMRGName = 'MyVirtualMachineResourceGroup';
$vmName = 'MySecureVM';
$KeyVaultName = 'MySecureVault';
$KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;

Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -EncryptFormatAll
```


### <a name="bkmk_EFALVM"> </a> Mantıksal birim Yöneticisi (LVM) ile EncryptFormatAll parametresini kullanın 
LVM'yi üzerinde crypt Kurulum öneririz. Aşağıdaki örnekler tüm için bağlama ve cihaz yolunu ne olursa olsun, kullanım örneği uygun ile değiştirin. Bu Kurulum şu şekilde yapabilirsiniz:

- Sanal Makineyi oluşturan veri diski ekleyin.
- Biçim, bağlamak ve bu disklerin fstab dosyaya ekleyin.

    1. Yeni eklenen diski biçimlendirin. Burada Azure tarafından oluşturulan çözümlemeyin kullanırız. Çözümlemeyin kullanarak cihaz adları değiştirme ilgili sorunları önler. Daha fazla bilgi için [cihaz adları sorun giderme sorunları](../virtual-machines/linux/troubleshoot-device-names-problems.md) makalesi.
    
         `mkfs -t ext4 /dev/disk/azure/scsi1/lun0`
    
    2. Diskleri bağlayın.
         
         `mount /dev/disk/azure/scsi1/lun0 /mnt/mountpoint`
    
    3. Fstab için ekleyin.
         
        `echo "/dev/disk/azure/scsi1/lun0 /mnt/mountpoint ext4 defaults,nofail 1 2" >> /etc/fstab`
    
    4. -Bu disklerini şifrelemek için EncryptFormatAll ile Set-AzVMDiskEncryptionExtension PowerShell cmdlet'ini çalıştırın.
         ```azurepowershell-interactive
         Set-AzVMDiskEncryptionExtension -ResourceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl "https://mykeyvault.vault.azure.net/" -EncryptFormatAll
         ```
    5. LVM'yi, bu yeni diskler üzerinde ayarlayın. VM önyükleme tamamlandıktan sonra şifrelenmiş sürücülerin kilidi olduğuna dikkat edin. Bu nedenle, LVM bağlama sonradan geciktirileceği da gerekir.


## <a name="bkmk_VHDpre"> </a> VHD ve şifreleme anahtarları müşteri şifrelenmiş oluşturulan yeni Iaas VM'ler
Bu senaryoda, PowerShell cmdlet'lerini veya CLI komutları kullanarak şifreleme etkinleştirebilirsiniz. 

Yönergeleri ekte, Azure'da kullanılabilen önceden şifrelenmiş görüntüleri hazırlama için kullanın. Görüntü oluşturulduktan sonra şifrelenmiş bir Azure VM oluşturmak için sonraki bölümde adımları kullanabilirsiniz.

* [Önceden şifrelenmiş bir Windows VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Önceden şifrelenmiş bir Linux VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra Set-AzVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, karşı temel alan bir yönetilen disk Vm'leri yedekleme yapılan ve bu parametre belirtilen kadar başarısız olacak. 
>
>Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 



### <a name="bkmk_VHDprePSH"> </a> Azure Iaas Vm'leri önceden şifrelenmiş VHD'ler ile şifrelemek için PowerShell kullanma 
Disk şifreli VHD'nizi PowerShell cmdlet'ini kullanarak şifreleyebilirsiniz [kümesi AzVMOSDisk](/powershell/module/Az.Compute/Set-AzVMOSDisk#examples). Aşağıdaki örnek, bazı ortak parametreler sunar. 

```azurepowershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen verileri disk şifrelemeyi etkinleştirin

Kullanarak yeni bir veri diski ekleyebilirsiniz [az vm disk ekleme](../virtual-machines/linux/add-disk.md), veya [Azure portalı üzerinden](../virtual-machines/linux/attach-disk-portal.md). Ayrıca şifreleme önce yeni eklenen veri diski ilk bağlama gerekir. Sürücü Şifreleme işlemi devam ederken kullanılamaz olduğundan veri sürücüsü şifrelenmesi istemeniz gerekir. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Azure CLI ile yeni eklenen bir disk şifrelemeyi etkinleştirin

 VM ile önceden şifrelenmişse sonra "Tüm" birim türü parametresi tüm kalmalıdır. Hem işletim sistemi ve veri diskleri içerir. VM "OS" ile bir birim türü önceden şifrelenmişse sonra birim türü parametre değiştirilmesi gereken tüm böylece hem işletim sistemi hem de yeni bir veri diski eklenecek. VM yalnızca "Data" birim türüyle şifrelenmişse, ardından bunu "Veri" aşağıda gösterildiği gibi kalır. Ekleme ve bir VM için yeni bir veri diski ekleme şifreleme için yeterli hazırlık değil. Yeni eklenen diski ayrıca olmalıdır biçimlendirilmiş ve şifreleme etkinleştirilmeden önce VM içinde düzgün bir şekilde bağlanır. Linux üzerinde disk ile/etc/fstab dosyasında takılmalıdır bir [kalıcı blok cihaz adı](https://docs.microsoft.com/azure/virtual-machines/linux/troubleshoot-device-names-problems).  

PowerShell sözdizimi aksine, şifreleme etkinleştirilirken benzersiz dizisi sürümü sağlamak kullanıcı CLI gerektirmez. CLI'yı otomatik olarak oluşturur ve kendi benzersiz dizisi sürümü değeri kullanır.

-  **Çalışan bir VM'ye veri birimlerini şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **Veri birimleri KEK kullanarak çalışan bir sanal makinenin şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell ile yeni eklenen bir disk şifrelemeyi etkinleştirin
 Linux için yeni bir disk şifrelemek için PowerShell kullanırken, yeni bir sıra sürüm belirtilmesi gerekiyor. Dizisi sürümü benzersiz olması gerekir. Aşağıdaki komut dizisi sürümü için bir GUID oluşturur. Ele bir [anlık görüntü](../virtual-machines/windows/snapshot-copy-managed-disk.md) ve/veya sanal makine ile yedeklemek [Azure Backup](../backup/backup-azure-vms-encryption.md) önce diskler şifrelenir. Yeni eklenen veri diski şifrelemek için PowerShell betiklerini - skipVmBackup parametresi zaten belirtilmiş.
 

-  **Çalışan bir VM'ye veri birimlerini şifrele:** Aşağıdaki komut dosyası değişkenleri başlatır ve Set-AzVMDiskEncryptionExtension cmdlet'ini çalıştırır. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup MySecureVM ve MySecureVault değerleriniz ile değiştirin. -VolumeType parametresi için kabul edilebilir değerler, işletim sistemi ve veri tümü. VM, "OS" veya "All" birim türü ile önceden şifrelenmişse, böylece hem işletim sistemi hem de yeni bir veri diski eklenecek ardından - VolumeType parametresi tüm değiştirilmelidir.

      ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $sequenceVersion = [Guid]::NewGuid();

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion -skipVmBackup;
      ```
- **Veri birimleri KEK kullanarak çalışan bir sanal makinenin şifrele:** -VolumeType parametresi için kabul edilebilir değerler, işletim sistemi ve veri tümü. VM, "OS" veya "All" birim türü ile önceden şifrelenmişse, böylece hem işletim sistemi hem de yeni bir veri diski eklenecek ardından - VolumeType parametresi tüm değiştirilmelidir.

     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MyExtraSecureVM';
      $KeyVaultName = 'MySecureVault';
      $keyEncryptionKeyName = 'MyKeyEncryptionKey';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
      $sequenceVersion = [Guid]::NewGuid();

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion -skipVmBackup;
     ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[KVresource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 


## <a name="disable-encryption-for-linux-vms"></a>Linux VM'ler için şifrelemeyi devre dışı bırakma
Azure PowerShell, Azure CLI kullanarak şifreleme devre dışı bırakabilir veya bir Resource Manager şablonu ile. 

>[!IMPORTANT]
>Linux vm'lerinde Azure Disk şifreleme ile şifreleme devre dışı bırakıldığında, yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifreli değilse veri veya işletim sistemi birimleri üzerinde desteklenmiyor.  

- **Azure PowerShell ile disk şifrelemesini devre dışı bırakın:** Şifreleme devre dışı bırakmak için [devre dışı bırak AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'i. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' [--volume-type {ALL, DATA, OS}]
     ```

- **Azure CLI ile şifreleme devre dışı bırakın:** Şifreleme devre dışı bırakmak için [az vm şifreleme devre dışı](/cli/azure/vm/encryption#az-vm-encryption-disable) komutu. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
- **Resource Manager şablonu ile şifreleme devre dışı bırakın:** Kullanım [çalışan bir Linux VM üzerinde şifrelemeyi devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) şifreleme devre dışı bırakmak için şablon.
     1. **Azure’a dağıt**’a tıklayın.
     2. Abonelik, kaynak grubu, konum, VM, yasal koşulları ve Sözleşmesi'ni seçin.
     3.  Tıklayın **satın alma** çalışan bir Windows VM'de disk şifreleme devre dışı bırakmak için. 


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Windows için Azure Disk şifrelemesini etkinleştirme](azure-security-disk-encryption-windows.md)
