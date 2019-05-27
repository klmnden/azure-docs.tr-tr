---
title: Azure AD uygulama Linux Iaas sanal makineleri (önceki sürüm) ile Azure Disk şifrelemesi
description: Bu makalede, Linux Iaas sanal makineleri için Microsoft Azure Disk şifrelemesi etkinleştirme hakkında yönergeler sağlar.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: 1e535ed92305d124499fd0ce9933b7edd19df32e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66118088"
---
# <a name="enable-azure-disk-encryption-for-linux-iaas-vms-previous-release"></a>Linux Iaas VM'ler (önceki sürüm) için Azure Disk şifrelemesini etkinleştirme

**Azure Disk Şifrelemesi'nın yeni sürümüne VM disk şifrelemeyi etkinleştirmek için bir Azure AD uygulama parametresi sağlama gereksinimini ortadan kaldırır. Yeni sürümle birlikte, artık Azure AD kimlik etkinleştir şifreleme adımı sırasında sağlayın isteniyor. Tüm yeni Vm'lere yeni sürümünü kullanarak Azure AD uygulama parametresiz şifrelenmelidir. Yeni sürümünü kullanarak VM disk şifrelemeyi etkinleştirmek için yönergeleri görüntülemek için bkz: [Linux VM'ler için Azure Disk şifrelemesi](azure-security-disk-encryption-linux.md). Zaten Azure AD uygulama parametreleri ile şifrelenmiş VM'ler hala desteklenmektedir ve AAD sözdizimiyle korunmasını devam etmelidir.**

Çok sayıda disk şifreleme senaryoları etkinleştirebilirsiniz ve adımları senaryoya göre değişiklik gösterebilir. Aşağıdaki bölümlerde, Linux Iaas sanal makineleri için daha ayrıntılı senaryoları kapsar. Disk şifreleme kullanmadan önce [Azure Disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites-aad.md) tamamlanması gerekir ve [Linux Iaas sanal makineleri için ek Önkoşullar](azure-security-disk-encryption-prerequisites-aad.md#bkmk_LinuxPrereq) bölümü gözden.

Ele bir [anlık görüntü](../virtual-machines/windows/snapshot-copy-managed-disk.md) ve/veya diskler şifrelenir önce yedekleyin. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata oluşması durumunda olası olduğundan emin olun. Şifreleme gerçekleşmeden önce yönetilen disklere sahip VM'ler yedeklemesini gerektirir. Bir yedekleme yapıldıktan sonra - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için Set-AzVMDiskEncryptionExtension cmdlet'ini kullanabilirsiniz. Yedekleme ve şifrelenmiş Vm'leri geri yükleme hakkında daha fazla bilgi için bkz. [Azure Backup](../backup/backup-azure-vms-encryption.md) makalesi. 

>[!WARNING]
 > - Daha önce kullandıysanız [Azure AD uygulaması ile Azure Disk şifrelemesi](azure-security-disk-encryption-prerequisites-aad.md) bu sanal Makineyi şifrelemek için devam gerekecektir VM'nizi şifrelemek için bu seçeneği kullanın. Kullanamazsınız [Azure Disk şifrelemesi](azure-security-disk-encryption-prerequisites.md) bu desteklenen bir senaryo değildir gibi şifrelenmiş bu VM üzerinde bu VM şifreli için AAD uygulaması uzağa anlamı geçiş henüz desteklenmiyor.
 > - Şifreleme parolaları bölgesel sınırlar arası yoksa emin olmak için Azure Disk şifrelemesi anahtar kasası ve VM'lerin aynı bölgede bulunması gerekir. Oluşturun ve şifrelenmiş VM ile aynı bölgede olan bir anahtar Kasası'nı kullanın.
 > - Linux işletim sistemi birimlerinin şifrelerken işlemi birkaç saat sürebilir. Linux işletim sistemi birimleri şifrelemek için veri birimleri uzun sürmesine normaldir.
> - Linux işletim sistemi birimlerinin şifrelerken sanal Makineyi kullanılamaz kabul edilmelidir. Şifreleme ilerlerken şifreleme işlemi sırasında erişilmesi gereken açık dosyaları engelleme sorunlarını önlemek için SSH oturumu açma önlemek için önerilir. İlerleme durumunu denetlemek için [Get-AzVMDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) veya [vm şifreleme show](/cli/azure/vm/encryption#az-vm-encryption-show) komutları kullanılabilir. Bu işlem birkaç saat 30 GB işletim sistemi birimi yanı sıra, veri hacimleri şifrelemek için ek süre olması için beklenebilir. Verileri toplu şifreleme süresi için boyutuyla orantılı olur ve veri birimlerini miktarını şifrele biçimlendirme sürece tüm seçeneği kullanılır. 
 > - Linux vm'lerinde şifreleme devre dışı bırakıldığında, yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifreli değilse veri veya işletim sistemi birimleri üzerinde desteklenmiyor.  

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="bkmk_RunningLinux"> </a> Var olan veya çalışan bir Iaas Linux VM üzerinde şifrelemeyi etkinleştir

Bu senaryoda, Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleyebilirsiniz. 

>[!IMPORTANT]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra Set-AzVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, karşı temel alan bir yönetilen disk Vm'leri yedekleme yapılan ve bu parametre belirtilen kadar başarısız olacak. 
>
>Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 
>

### <a name="bkmk_RunningLinuxCLI"> </a>Bir var olan veya çalışan Azure CLI kullanarak Linux VM üzerinde şifrelemeyi etkinleştir 
Disk şifrelemesi yüklenerek ve kullanılarak şifrelenmiş VHD'nizi etkinleştirebilirsiniz [Azure CLI 2.0](/cli/azure) komut satırı aracı. [Azure Cloud Shell](../cloud-shell/overview.md) ile tarayıcınızda kullanabilir veya yerel makinenize yükleyip herhangi bir PowerShell oturumunda kullanabilirsiniz. Var olan veya Iaas sanal makineleri Azure'da çalışan şifrelemeyi etkinleştirmek için aşağıdaki CLI komutları kullanın:

Kullanım [az vm şifrelemeyi etkinleştirme](/cli/azure/vm/encryption#az-vm-encryption-enable) azure'da çalıştırılan Iaas sanal makine üzerinde şifrelemeyi etkinleştirmek için komutu.

-  **İstemci gizli anahtarı kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **İstemci gizli anahtarı sarmalama KEK kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> </br>
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

- **İstemci gizli anahtarı kullanarak çalışan bir VM şifrele:** Aşağıdaki komut dosyası değişkenleri başlatır ve Set-AzVMDiskEncryptionExtension cmdlet'ini çalıştırır. Kaynak grubu, VM, anahtar kasası, AAD uygulaması ve istemci gizli anahtarı zaten önkoşul olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup, MyKeyVaultResourceGroup, MySecureVM, My-AAD-client-ID ve My-AAD-CLIENT-secret MySecureVault değerleriniz ile değiştirin. Şifreleme hangi disklerin belirtmek için - VolumeType parametresini değiştirin.

    ```azurepowershell
     $VMRGName = 'MyVirtualMachineResourceGroup';
     $KVRGname = 'MyKeyVaultResourceGroup';
     $vmName = 'MySecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $sequenceVersion = [Guid]::NewGuid();

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType '[All|OS|Data]' -SequenceVersion $sequenceVersion -skipVmBackup;
    ```
- **İstemci gizli anahtarı sarmalama KEK kullanarak çalışan bir VM şifrele:** Azure Disk şifrelemesi var olan bir anahtar şifreleme etkinleştirirken oluşturulan disk şifreleme gizli dizileri sarmalamak için anahtar kasanızdaki belirtmenize olanak sağlar. Anahtar şifreleme anahtarı belirtildiğinde, Azure Disk şifrelemesi anahtar Kasası'na yazmadan önce şifreleme parolaları sarmalamak için bu anahtarı kullanır. Şifreleme hangi disklerin belirtmek için - VolumeType parametresini değiştirin. 

     ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     $sequenceVersion = [Guid]::NewGuid();

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType '[All|OS|Data]' -SequenceVersion $sequenceVersion -skipVmBackup;
     ```

  >[!NOTE]
  > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[KVresource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> </br>
  Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 
    
- **Diskler şifrelenir doğrulayın:** Iaas VM şifreleme durumunu denetlemek için kullanmak [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet'i. 
    
     ```azurepowershell-interactive 
     Get-AzVmDiskEncryptionStatus -ResourceGroupName MyVirtualMachineResourceGroup -VMName MySecureVM
     ```
    
- **Disk şifreleme devre dışı bırakın:** Şifreleme devre dışı bırakmak için [Disable-AzureRmVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'i. Şifreleme devre dışı bırakıldığında yalnızca veri birimlerinde Linux VM'ler için izin verilir.
     
     ```azurepowershell-interactive 
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```


### <a name="bkmk_RunningLinux"> </a> Var olan veya çalışan Iaas Linux VM'de bir şablonla şifrelemeyi etkinleştir

Var olan veya çalışan Iaas Linux VM'de Azure disk şifrelemesi kullanarak etkinleştirebilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

1. Tıklayın **azure'a Dağıt** Azure Hızlı Başlangıç şablonu.

2. Abonelik, kaynak grubu, kaynak grubu konumu, parametreleri, yasal koşulları ve Sözleşmesi'ni seçin. Tıklayın **Oluştur** var olan veya çalışan Iaas VM üzerinde şifrelemeyi etkinleştirmek için.

Var olan veya bir Azure AD İstemci Kimliğini kullanan sanal makineleri çalıştırmak için Resource Manager şablon parametreleri aşağıdaki tabloda listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| Aadclientıd | Bir anahtar kasasına gizli anahtarları yazmak için izne sahip olan Azure AD uygulamasının istemci kimliği. |
| AADClientSecret | Gizli anahtar kasanız için yazma iznine sahip Azure AD uygulamasının istemci gizli anahtarı. |
| keyVaultName | Anahtarı karşıya yüklenmelidir anahtar kasasının adı. Azure CLI komutunu kullanarak bunu alabilirsiniz `az keyvault show --name "MySecureVault" --query KVresourceGroup`. |
|  KeyEncryptionKeyURL | Oluşturulan anahtarı şifrelemek için kullanılan anahtar şifreleme anahtarı URL'si. Bu seçerseniz isteğe bağlı bir parametredir **nokek** UseExistingKek aşağı açılan listesinde. Seçerseniz **kek** UseExistingKek aşağı açılan listeden girmelisiniz _keyEncryptionKeyURL_ değeri. |
| VolumeType | Şifreleme işlemi gerçekleştirilir birim türü. Geçerli desteklenen değerler şunlardır: _işletim sistemi_ veya _tüm_ (bkz: desteklenen Linux dağıtımları ve bunların sürümlerini önkoşul bölümünde daha önce işletim sistemi ve veri diskleri için). |
| SequenceVersion | BitLocker işlemi dizisi sürümü. Bu sürüm numarası, aynı sanal makinede her bir disk şifreleme işlemi gerçekleştirildiğinde artırın. |
| vmName | Şifreleme işlemi gerçekleştirilecek olan bir VM adı. |
| passphrase | Veri şifreleme anahtarı güçlü bir parola yazın. |



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

### <a name="bkmk_EFATemplate"> </a> Bir şablonla EncryptFormatAll parametresini kullanın
EncryptFormatAll seçeneğini kullanmak için bir Linux VM şifreler önceden mevcut olan tüm Azure Resource Manager şablonu kullanma ve değiştirme **EncryptionOperation** AzureDiskEncryption kaynak için alan.

1. Bir örnek [çalışan Linux Iaas VM şifreleme için Resource Manager şablonunu](https://github.com/vermashi/azure-quickstart-templates/tree/encrypt-format-running-linux-vm/201-encrypt-running-linux-vm). 
2. Tıklayın **azure'a Dağıt** Azure Hızlı Başlangıç şablonu.
3. Değişiklik **EncryptionOperation** gelen **EnableEncryption** için **EnableEncryptionFormatAl**
4. Abonelik, kaynak grubu, kaynak grubu konumu, diğer parametreler, yasal koşulları ve Sözleşmesi'ni seçin. Tıklayın **Oluştur** var olan veya çalışan Iaas VM üzerinde şifrelemeyi etkinleştirmek için.


### <a name="bkmk_EFAPSH"> </a> Bir PowerShell cmdlet'i ile EncryptFormatAll parametresini kullanın
Kullanım [kümesi AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet'iyle `EncryptFormatAll` parametresi.

**Bir istemci gizli anahtarı ve EncryptFormatAll kullanarak çalışan bir VM şifrele:** Örneğin, aşağıdaki betiği değişkenlerinizi başlatır ve Set-AzVMDiskEncryptionExtension cmdlet'i EncryptFormatAll parametresi ile çalıştırır. Kaynak grubu, VM, anahtar kasası, AAD uygulaması ve istemci gizli anahtarı zaten önkoşul olarak oluşturulmuş olmalıdır. MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM, My-AAD-client-ID ve My-AAD-CLIENT-secret MySecureVault değerleriniz ile değiştirin.
  
   ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup'; 
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
      
     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -EncryptFormatAll
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
Bu senaryoda, Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleme etkinleştirebilirsiniz. Aşağıdaki bölümlerde Resource Manager şablonu ve CLI komutları daha ayrıntılı olarak açıklanmaktadır. 

Yönergeleri ekte, Azure'da kullanılabilen önceden şifrelenmiş görüntüleri hazırlama için kullanın. Görüntü oluşturulduktan sonra şifrelenmiş bir Azure VM oluşturmak için sonraki bölümde adımları kullanabilirsiniz.

* [Önceden şifrelenmiş bir Windows VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Önceden şifrelenmiş bir Linux VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra Set-AzVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, karşı temel alan bir yönetilen disk Vm'leri yedekleme yapılan ve bu parametre belirtilen kadar başarısız olacak. 
>
>Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 



### <a name="bkmk_VHDprePSH"> </a> Azure Iaas Vm'leri önceden şifrelenmiş VHD'ler ile şifrelemek için PowerShell kullanma 
Disk şifreli VHD'nizi PowerShell cmdlet'ini kullanarak şifreleyebilirsiniz [kümesi AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk#examples). Aşağıdaki örnek, bazı ortak parametreler sunar. 

```powershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen verileri disk şifrelemeyi etkinleştirin
Kullanarak yeni bir veri diski ekleyebilirsiniz [az vm disk ekleme](../virtual-machines/linux/add-disk.md), veya [Azure portalı üzerinden](../virtual-machines/linux/attach-disk-portal.md). Ayrıca şifreleme önce yeni eklenen veri diski ilk bağlama gerekir. Sürücü Şifreleme işlemi devam ederken kullanılamaz olduğundan veri sürücüsü şifrelenmesi istemeniz gerekir. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Azure CLI ile yeni eklenen bir disk şifrelemeyi etkinleştirin
 VM ile önceden şifrelenmişse sonra "Tüm" birim türü parametresi tüm kalmalıdır. Hem işletim sistemi ve veri diskleri içerir. VM "OS" ile bir birim türü önceden şifrelenmişse sonra birim türü parametre değiştirilmesi gereken tüm böylece hem işletim sistemi hem de yeni bir veri diski eklenecek. VM yalnızca "Data" birim türüyle şifrelenmişse, ardından bunu "Veri" aşağıda gösterildiği gibi kalır. Ekleme ve bir VM için yeni bir veri diski ekleme şifreleme için yeterli hazırlık değil. Yeni eklenen diski ayrıca olmalıdır biçimlendirilmiş ve şifreleme etkinleştirilmeden önce VM içinde düzgün bir şekilde bağlanır. Linux üzerinde disk ile/etc/fstab dosyasında takılmalıdır bir [kalıcı blok cihaz adı](https://docs.microsoft.com/azure/virtual-machines/linux/troubleshoot-device-names-problems).  

PowerShell sözdizimi aksine, şifreleme etkinleştirilirken benzersiz dizisi sürümü sağlamak kullanıcı CLI gerektirmez. CLI'yı otomatik olarak oluşturur ve kendi benzersiz dizisi sürümü değeri kullanır.

-  **İstemci gizli anahtarı kullanarak çalışan bir VM şifrele:** 

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **İstemci gizli anahtarı sarmalama KEK kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell ile yeni eklenen bir disk şifrelemeyi etkinleştirin
 Linux için yeni bir disk şifrelemek için PowerShell kullanırken, yeni bir sıra sürüm belirtilmesi gerekiyor. Dizisi sürümü benzersiz olması gerekir. Aşağıdaki komut dizisi sürümü için bir GUID oluşturur. 
 

-  **İstemci gizli anahtarı kullanarak çalışan bir VM şifrele:** Aşağıdaki komut dosyası değişkenleri başlatır ve Set-AzVMDiskEncryptionExtension cmdlet'ini çalıştırır. Kaynak grubu, VM, anahtar kasası, AAD uygulaması ve istemci gizli anahtarı zaten önkoşul olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup, MyKeyVaultResourceGroup, MySecureVM, My-AAD-client-ID ve My-AAD-CLIENT-secret MySecureVault değerleriniz ile değiştirin. -VolumeType parametre veri diskleri ve işletim sistemi diski için ayarlanır. VM, "OS" veya "All" birim türü ile önceden şifrelenmişse, böylece hem işletim sistemi hem de yeni bir veri diski eklenecek ardından - VolumeType parametresi tüm değiştirilmelidir.

     ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup'; 
     $vmName = 'MySecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $sequenceVersion = [Guid]::NewGuid();

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion;
     ```
- **İstemci gizli anahtarı sarmalama KEK kullanarak çalışan bir VM şifrele:** Azure Disk şifrelemesi var olan bir anahtar şifreleme etkinleştirirken oluşturulan disk şifreleme gizli dizileri sarmalamak için anahtar kasanızdaki belirtmenize olanak sağlar. Anahtar şifreleme anahtarı belirtildiğinde, Azure Disk şifrelemesi anahtar Kasası'na yazmadan önce şifreleme parolaları sarmalamak için bu anahtarı kullanır. -VolumeType parametre veri diskleri ve işletim sistemi diski için ayarlanır. VM, "OS" veya "All" birim türü ile önceden şifrelenmişse, böylece hem işletim sistemi hem de yeni bir veri diski eklenecek ardından - VolumeType parametresi tüm değiştirilmelidir.

     ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     $vmName = 'MyExtraSecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     $sequenceVersion = [Guid]::NewGuid();

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion;
     ```


>[!NOTE]
> Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] </br> </br>
Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

## <a name="disable-encryption-for-linux-vms"></a>Linux VM'ler için şifrelemeyi devre dışı bırakma
Azure PowerShell, Azure CLI kullanarak şifreleme devre dışı bırakabilir veya bir Resource Manager şablonu ile. 

>[!IMPORTANT]
>Linux vm'lerinde Azure Disk şifreleme ile şifreleme devre dışı bırakıldığında, yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifreli değilse veri veya işletim sistemi birimleri üzerinde desteklenmiyor.  

- **Azure PowerShell ile disk şifrelemesini devre dışı bırakın:** Şifreleme devre dışı bırakmak için [Disable-AzureRmVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'i. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' [--volume-type {ALL, DATA, OS}]
     ```

- **Azure CLI ile şifreleme devre dışı bırakın:** Şifreleme devre dışı bırakmak için [az vm şifreleme devre dışı](/cli/azure/vm/encryption#az-vm-encryption-disable) komutu. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
- **Resource Manager şablonu ile şifreleme devre dışı bırakın:** Kullanım [çalışan bir Linux VM üzerinde şifrelemeyi devre dışı bırak](https://aka.ms/decrypt-linuxvm) şifreleme devre dışı bırakmak için şablon.
     1. **Azure’a dağıt**’a tıklayın.
     2. Abonelik, kaynak grubu, konum, VM, yasal koşulları ve Sözleşmesi'ni seçin.
     3.  Tıklayın **satın alma** çalışan bir Windows VM'de disk şifreleme devre dışı bırakmak için. 


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Windows için Azure Disk şifrelemesini etkinleştirme](azure-security-disk-encryption-windows-aad.md)
