---
title: Önkoşullar - Iaas Vm'leri için Azure Disk şifrelemesi | Microsoft Docs
description: Bu makale, Iaas sanal makineleri için Microsoft Azure Disk şifrelemesi kullanılarak önkoşulları sağlar.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/25/2019
ms.custom: seodec18
ms.openlocfilehash: 1da35b55a458ad73689f51c49e73855fd33ee45f
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59496259"
---
# <a name="azure-disk-encryption-prerequisites"></a>Azure Disk şifrelemesi önkoşulları

Bu makalede, Azure Disk şifrelemesi önkoşulları, Azure Disk şifrelemesi kullanabilmeniz için önce karşılanması gereken öğeleri açıklar. Azure Disk şifrelemesi ile tümleşiktir [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/) şifreleme anahtarlarını yönetmeye yardımcı olmak için. Kullanabileceğiniz [Azure PowerShell](/powershell/azure/overview), [Azure CLI](/cli/azure/), veya [Azure portalında](https://portal.azure.com) Azure Disk şifrelemesini yapılandırmak için.

Ele alınan desteklenen senaryolar için Azure Iaas sanal makinelerinde Azure Disk Şifrelemesi'ı etkinleştirmeden önce [Azure Disk Şifrelemesi'ne genel bakış](azure-security-disk-encryption-overview.md) makalesi, önkoşulların sağlandığından emin olun. 

> [!WARNING]
> - Daha önce kullandıysanız [Azure AD uygulaması ile Azure Disk şifrelemesi](azure-security-disk-encryption-prerequisites-aad.md) bu sanal Makineyi şifrelemek için devam gerekecektir VM'nizi şifrelemek için bu seçeneği kullanın. Kullanamazsınız [Azure Disk şifrelemesi](azure-security-disk-encryption-prerequisites.md) bu desteklenen bir senaryo değildir gibi şifrelenmiş bu VM üzerinde bu VM şifreli için AAD uygulaması uzağa anlamı geçiş henüz desteklenmiyor.
> - Bazı öneriler, veri, ağ veya ek lisans ya da abonelik maliyetlerinizi kaynaklanan işlem kaynak kullanımını artırabilir. Desteklenen bölgelerdeki Azure kaynakları oluşturmak için geçerli bir etkin Azure aboneliğinizin olması gerekir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="bkmk_OSs"></a> Desteklenen işletim sistemleri
Azure Disk şifrelemesi, aşağıdaki işletim sistemlerinde desteklenir:

- Windows Server sürümleri: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2012 R2 Sunucu Çekirdeği ve Windows Server 2016 Server core.
Windows Server 2008 R2 için .NET Framework 4.5, azure'daki şifreleme etkinleştirmeden önce yüklü olması gerekir. Windows Update'ten isteğe bağlı bir güncelleştirme Windows Server 2008 R2 x64 tabanlı sistemleri (KB2901983) için Microsoft .NET Framework 4.5.2 ile yükleyin.
- VM'de bdehdcfg bileşeni yüklendikten sonra Windows Server 2012 R2 Core ve Windows Server 2016 Core, Azure Disk Şifrelemesi tarafından desteklenir.
- Windows istemci sürümleri: Windows 8 istemcisi ve Windows 10 istemcisi.
- Azure Disk şifrelemesi yalnızca Windows'da desteklenmektedir belirli Azure Galerisi'nde Linux sunucusu dağıtımları ve sürümleri dayalıdır. Şu anda desteklenen sürümlerin listesi için başvurmak [Azure Disk şifrelemesi hakkında SSS](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport). Başvurmak [Azure'da desteklenen Linux dağıtımı](../virtual-machines/linux/endorsed-distros.md) için desteklenen Microsoft tarafından ve çok görüntülerin listesini [ne Linux dağıtımı, Azure Disk şifrelemesi desteği mu?](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) içinde [Azure Disk şifrelemesi hakkında SSS](azure-security-disk-encryption-faq.md) desteklenen görüntü dağıtımlarda şu anda desteklenen sürümlerin listesi için.
- Azure Disk şifrelemesi, anahtar kasası ve VM'lerin aynı Azure bölgesindeki ve abonelikte bulunmasını gerektirir. Kaynaklarını ayrı bölge içinde yapılandırma Azure Disk şifreleme özelliği etkinleştirilirken bir hata neden olur.

## <a name="bkmk_LinuxPrereq"></a> Linux Iaas sanal makineleri için ek Önkoşullar 

- Linux için Azure Disk şifrelemesi, 7 GB işletim sistemi disk şifrelemeyi etkinleştirmek için VM üzerindeki RAM gerektirir [desteklenen resimler](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport). İşletim sistemi disk şifreleme işlemi tamamlandıktan sonra VM ile daha az bellek çalıştırmak için yapılandırılabilir.
- Azure Disk şifrelemesi vfat modülü sistemde mevcut olması gerekir.  Bu modül varsayılan görüntüden devre dışı bırakma veya kaldırma sistemi anahtar birimin okuma ve sonraki yeniden başlatmalar disklerde kilidini açmak için gerekli anahtarı almak için engeller. Sistemden vfat modülü kaldırmak sistem sağlamlaştırma adımlarının Azure Disk şifrelemesi ile uyumlu değildir. 
- Şifreleme etkinleştirilmeden önce şifrelenmiş veri diskleri düzgün /etc/fstab içinde listelenmesi gerekir. Kalıcı blok cihaz adı bu giriş için "/ dev/sdX" biçimindeki adlarını sırasında özellikle şifreleme uygulandıktan sonra yeniden başlatmaları arasında aynı disk ile ilişkilendirilmesi dayanan olamaz cihazı olarak kullanın. Bu davranışı hakkında daha fazla ayrıntı için bkz: [Linux VM cihaz adı değişikliklerle ilgili sorunları giderme](../virtual-machines/linux/troubleshoot-device-names-problems.md)
- /Etc/fstab ayarlarını, bağlama için düzgün şekilde yapılandırıldığından emin olun. Bu ayarları yapılandırmak için bağlama - bir komut çalıştırın veya VM'yi yeniden başlatın ve bu şekilde onarılmasının tetikleyin. Bu işlem tamamlandıktan sonra sürücüyü yine de bağlandığını doğrulamak için lsblk komutunun çıkışını kontrol edin. 
  - Azure Disk şifrelemesi, /etc/fstab dosya sürücünün doğru şifreleme etkinleştirilmeden önce değil bağlarsanız, düzgün bir şekilde bağlamak mümkün olmayacaktır.
  - Azure Disk şifreleme işlemi /etc/fstab dışında ve kendi yapılandırma dosyasına bağlama bilgilerini şifreleme işleminin bir parçası olarak taşınır. Tamamlandıktan sonra veri Sürücü Şifrelemesi /etc/fstab eksik giriş görmek için alarmed çekinmeyin.
  -  Yeniden başlatıldıktan sonra yeni şifrelenmiş diskler bağlamak Azure Disk şifrelemesi işlemi için saat sürer. Bunlar yeniden başlatmanın ardından hemen kullanılabilir olmaz. İşlemi başlatmak, kilidini açmak ve ardından erişmek diğer işlemler için kullanılabilir olan önce şifrelenmiş sürücüleri bağlamak için zaman gerekir. Bu işlem, sistem özelliklerine bağlı olarak yeniden başlatma işleminden sonra birden fazla işlem birkaç dakika sürebilir.

Veri diskleri bağlayın ve gerekli/etc/fstab girişleri oluşturmak için kullanılan komutlar örneği bulunabilir [244 248 bu betik dosyasının satırları](https://github.com/ejarvi/ade-cli-getting-started/blob/master/validate.sh#L244-L248). 


## <a name="bkmk_GPO"></a> Ağ ve Grup İlkesi

**Özelliği, Iaas Vm'leri Azure Disk Şifrelemesi'ni etkinleştirmek için aşağıdaki ağ uç noktası yapılandırması gereksinimleri karşılaması gerekir:**
  - Anahtar kasanızı bağlanmak için bir belirteç almak üzere Iaas VM bir Azure Active Directory uç noktasına bağlanabilir olmalıdır \[login.microsoftonline.com\].
  - Şifreleme anahtarları için anahtar kasanıza yazmak için Iaas VM anahtar kasası uç noktasına bağlanabilir olmalıdır.
  - Iaas VM Azure uzantısı depoyu ve VHD dosyalarını barındıran Azure depolama hesabı'nı barındıran bir Azure depolama uç noktasına bağlanabiliyor olmanız gerekir.
  -  Güvenlik ilkeniz Azure vm'lerinden Internet erişimi sınırlayan, önceki URI çözmek ve IP'ler giden bağlantıya izin verecek bir kuralı yapılandırın. Daha fazla bilgi için [bir güvenlik duvarının ardındayken Azure anahtar kasası](../key-vault/key-vault-access-behind-firewall.md).    


**Grup İlkesi:**
 - Azure Disk şifrelemesi çözümü, BitLocker dış anahtar koruyucusu Windows Iaas Vm'leri için kullanır. Etki alanına katılmış sanal makineleri, TPM koruyucusu zorlamak için tüm grup ilkeleri anında iletme yok. "Uyumlu TPM'siz BitLocker izin ver" için Grup İlkesi hakkında bilgi için bkz: [BitLocker Grup İlkesi başvurusu](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#a-href-idbkmk-unlockpol1arequire-additional-authentication-at-startup).

-  Özel Grup İlkesi ile etki alanına katılmış sanal makinelerde BitLocker'ı İlkesi şu ayar eklemeniz gerekir: [Kullanıcı depolama alanını yapılandırmak bitlocker kurtarma bilgilerinin -> izin 256 bitlik kurtarma anahtarı](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). Azure Disk şifrelemesi, BitLocker için özel Grup İlkesi ayarları uyumsuz olduğunda başarısız olur. Doğru ilkeyi gerekmedi makinelerde yeni ilkeyi uygulamak, (gpupdate.exe/Force) güncelleştirmek için yeni ilke zorlayın ve daha sonra yeniden başlatmak gerekli olabilir.

- Azure Disk şifrelemesi, Bitlocker tarafından kullanılan AES-CBC algoritması etki alanı düzeyi Grup İlkesi engelliyorsa, başarısız olur.


## <a name="bkmk_PSH"></a> Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) kullanan cmdlet'leri takımına [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) Azure kaynaklarınızı yönetmek için model. İle tarayıcınızda kullanmak [Azure Cloud Shell](../cloud-shell/overview.md), veya her PowerShell oturumunda kullanmak için aşağıdaki yönergeleri kullanarak yerel makinenize yükleyin. Yerel olarak yüklü zaten varsa, Azure Disk şifrelemesini yapılandırmak için en son Azure PowerShell SDK'sı sürümü kullandığınızdan emin olun. En son sürümünü indirin [Azure PowerShell sürümü](https://github.com/Azure/azure-powershell/releases).

### <a name="install-azure-powershell-for-use-on-your-local-machine-optional"></a>Yerel makinenizde (isteğe bağlı) kullanmak için Azure PowerShell'i yükleyin: 
1. İşletim sisteminiz için bağlantıları'ndaki yönergeleri izleyin sonra devam edebilirsiniz ancak geri kalan adımları.      
   - [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/install-az-ps). 
     - PowerShellGet, Azure PowerShell'i yükleme ve Az modül yükle. 

2. Az modülünün yüklü sürümlerini doğrulayın. Gerekirse, [Azure PowerShell modülü güncelleştirme](/powershell/azure/install-az-ps#update-the-azure-powershell-module).
    En son Az Modül sürümü kullanılması önerilir.

     ```powershell
     Get-Module Az -ListAvailable | Select-Object -Property Name,Version,Path
     ```

3. Oturum açmak için Azure kullanarak [Connect AzAccount](/powershell/module/az.accounts/connect-azaccount) cmdlet'i.
     
     ```azurepowershell-interactive
     Connect-AzAccount
     # For specific instances of Azure, use the -Environment parameter.
     Connect-AzAccount –Environment (Get-AzEnvironment –Name AzureUSGovernment)
    
     <# If you have multiple subscriptions and want to specify a specific one, 
     get your subscription list with Get-AzSubscription and 
     specify it with Set-AzContext.  #>
     Get-AzSubscription
     Set-AzContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
     ```

4.  Gerekirse, gözden [Azure PowerShell'i kullanmaya başlama](/powershell/azure/get-started-azureps).

## <a name="bkmk_CLI"></a> (İsteğe bağlı) yerel makinenizde kullanmak için Azure CLI yükleme

[Azure CLI 2.0](/cli/azure) Azure kaynaklarını yönetmek için bir komut satırı aracıdır. CLI, esnek bir şekilde verileri sorgulamak, engelleyici olmayan süreçler olarak uzun süreli işlemleri desteklemek ve kolay bir komut dosyası yapmak için tasarlanmıştır. [Azure Cloud Shell](../cloud-shell/overview.md) ile tarayıcınızda kullanabilir veya yerel makinenize yükleyip herhangi bir PowerShell oturumunda kullanabilirsiniz.

1. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli) kullanılmak üzere yerel makinenize (isteğe bağlı):

2. Yüklü sürümünü doğrulayın.
     
     ```azurecli-interactive
     az --version
     ``` 

3. Oturum açmak için Azure kullanarak [az login](/cli/azure/authenticate-azure-cli).
     
     ```azurecli
     az login
     
     # If you would like to select a tenant, use: 
     az login --tenant "<tenant>"

     # If you have multiple subscriptions, get your subscription list with az account list and specify with az account set.
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. Gözden geçirme [Azure CLI 2.0 ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli) gerekirse. 


## <a name="prerequisite-workflow-for-key-vault"></a>Key Vault için önkoşul iş akışı
Zaten Azure Disk şifrelemesi için Key Vault ve Azure AD önkoşulları alışık olduğunuz, kullanabileceğiniz [Azure Disk şifrelemesi önkoşulları PowerShell Betiği](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 ). Önkoşulları betiği kullanma hakkında daha fazla bilgi için bkz. [VM hızlı başlangıç şifrelemek](quick-encrypt-vm-powershell.md) ve [Azure Disk şifrelemesi ek](azure-security-disk-encryption-appendix.md#bkmk_prereq-script). 

1. Gerekirse, bir kaynak grubu oluşturun.
2. Bir anahtar kasası oluşturma. 
3. Set anahtar kasası erişim ilkeleri Gelişmiş.

>[!WARNING]
>Bir anahtar kasası silmeden önce var olan tüm Vm'lerle şifrelenmez olduğunu emin olun. Bir kasayı yanlışlıkla silinmeye karşı korumak için [geçici silmeyi etkinleştir](../key-vault/key-vault-soft-delete-powershell.md#enabling-soft-delete) ve [kaynak kilidi](../azure-resource-manager/resource-group-lock-resources.md) kasasındaki. 
 
## <a name="bkmk_KeyVault"></a> Anahtar kasası oluşturma 
Azure Disk şifrelemesi ile tümleşiktir [Azure anahtar kasası](https://azure.microsoft.com/documentation/services/key-vault/) denetlemenize ve disk şifreleme anahtarlarını ve gizli anahtar kasası aboneliğinizi yönetmenize yardımcı olmak için. Anahtar kasası oluşturma veya mevcut bir Azure Disk şifrelemesi kullanın. Anahtar kasası hakkında daha fazla bilgi için bkz: [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md) ve [anahtar kasanızın güvenliğini sağlama](../key-vault/key-vault-secure-your-key-vault.md). Bir anahtar kasası oluşturmak için Resource Manager şablonu, Azure PowerShell veya Azure CLI'yı kullanabilirsiniz. 


>[!WARNING]
>Şifreleme parolaları bölgesel sınırlar arası yoksa emin olmak için Azure Disk şifrelemesi anahtar kasası ve VM'lerin aynı bölgede bulunması gerekir. Oluşturun ve şifrelenmiş VM ile aynı bölgede olan bir anahtar Kasası'nı kullanın. 


### <a name="bkmk_KVPSH"></a> PowerShell ile key vault oluşturma

Azure PowerShell kullanarak bir anahtar kasası oluşturabilirsiniz [yeni AzKeyVault](/powershell/module/az.keyvault/New-azKeyVault) cmdlet'i. Key Vault için ek cmdlet'ler için bkz [Az.KeyVault](/powershell/module/az.keyvault/). 

1. Gerekirse, [Azure aboneliğinize bağlanma](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH). 
2. Gerekirse, yeni bir kaynak grubu oluşturma ile [yeni AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup).  Veri Merkezi konumlarını listesinde, kullanmak için [Get-AzLocation](/powershell/module/az.resources/get-azlocation). 
     
     ```azurepowershell-interactive
     # Get-AzLocation 
     New-AzResourceGroup –Name 'MyKeyVaultResourceGroup' –Location 'East US'
     ```

3. Kullanarak yeni bir anahtar kasası oluşturma [yeni AzKeyVault](/powershell/module/az.keyvault/New-azKeyVault)
    
      ```azurepowershell-interactive
     New-AzKeyVault -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -Location 'East US'
     ```

4. Not **kasa adı**, **kaynak grubu adı**, **kaynak kimliği**, **kasa URI'si**ve **nesne kimliği** diskleri şifreleme, daha sonra kullanmak üzere getirilir. 


### <a name="bkmk_KVCLI"></a> Azure CLI ile anahtar kasası oluşturma
Azure CLI kullanarak anahtar kasanıza yönetebileceğiniz [az keyvault](/cli/azure/keyvault#commands) komutları. Bir anahtar kasası oluşturmak için kullanın [az keyvault oluşturma](/cli/azure/keyvault#az-keyvault-create).

1. Gerekirse, [Azure aboneliğinize bağlanma](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Gerekirse, yeni bir kaynak grubu oluşturma ile [az grubu oluşturma](/cli/azure/group#az-group-create). Konumlar listesinde, kullanmak için [az hesabı konumları-Listele](/cli/azure/account#az-account-list) 
     
     ```azurecli-interactive
     # To list locations: az account list-locations --output table
     az group create -n "MyKeyVaultResourceGroup" -l "East US"
     ```

3. Kullanarak yeni bir anahtar kasası oluşturma [az keyvault oluşturma](/cli/azure/keyvault#az-keyvault-create).
    
     ```azurecli-interactive
     az keyvault create --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --location "East US"
     ```

4. Not **kasa adı** (ad), **kaynak grubu adı**, **kaynak kimliği** (ID) **Vault URI'si**ve **nesnekimliği** , döndürülür kullanılmak üzere daha sonra. 

### <a name="bkmk_KVRM"></a> Resource Manager şablonu ile anahtar kasası oluşturma

Bir anahtar kasası kullanarak oluşturabileceğiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create).

1. Azure Hızlı Başlangıç şablonu üzerinde **azure'a Dağıt**.
2. Abonelik, kaynak grubu, kaynak grubu konumu, anahtar kasası adını, nesne kimliği, yasal koşulları ve Sözleşmesi'ni seçin ve ardından **satın alma**. 


## <a name="bkmk_KVper"></a> Set anahtar kasası erişim ilkeleri Gelişmiş
Azure platform şifreleme anahtarları veya gizli anahtar kasanızı önyükleme ve birimler şifresini çözmek için VM ayıklanarak erişmesi gerekir. Disk şifrelemeyi etkinleştirme anahtar kasası veya dağıtımları başarısız olur.  

### <a name="bkmk_KVperPSH"></a> Gelişmiş erişim ilkeleri Azure PowerShell ile anahtar kasası ayarlama
 Anahtar kasası PowerShell cmdlet'ini kullanın [kümesi AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) anahtar kasası disk şifrelemeyi etkinleştirmek için.

  - **Anahtar kasası disk şifrelemesi için etkinleştir:** Azure Disk şifrelemesi için EnabledForDiskEncryption gereklidir.
      
     ```azurepowershell-interactive 
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDiskEncryption
     ```

  - **Key Vault, gerekirse dağıtım için etkinleştir:** Bu anahtar kasası kaynak oluşturma, örneğin bir sanal makine oluşturulurken başvurulduğundan olduğunda bu anahtar kasasından gizli dizilerini alma Microsoft.Compute kaynak sağlayıcısına sağlar.

     ```azurepowershell-interactive
      Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDeployment
     ```

  - **Key Vault şablon dağıtımı için gerekirse etkinleştir:** Bu anahtar kasasına bir şablon dağıtımında başvuru olduğunda bu anahtar kasasından gizli dizileri almak üzere Azure Resource Manager sağlar.

     ```azurepowershell-interactive             
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForTemplateDeployment
     ```

### <a name="bkmk_KVperCLI"></a> Gelişmiş erişim ilkeleri Azure CLI kullanarak anahtar kasası ayarlama
Kullanım [az keyvault update](/cli/azure/keyvault#az-keyvault-update) anahtar kasası disk şifrelemeyi etkinleştirmek için. 

 - **Anahtar kasası disk şifrelemesi için etkinleştir:** Etkin-için-disk şifreleme gerekli değildir. 

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-disk-encryption "true"
     ```  

 - **Key Vault, gerekirse dağıtım için etkinleştir:** Bu anahtar kasası kaynak oluşturma, örneğin bir sanal makine oluşturulurken başvurulduğundan olduğunda bu anahtar kasasından gizli dizilerini alma Microsoft.Compute kaynak sağlayıcısına sağlar.

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-deployment "true"
     ``` 

 - **Key Vault şablon dağıtımı için gerekirse etkinleştir:** Resource Manager'ın gizli dizileri kasadan almak için izin verin.
     ```azurecli-interactive  
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-template-deployment "true"
     ```


### <a name="bkmk_KVperrm"></a> Gelişmiş erişim ilkeleri Azure portalı üzerinden anahtar kasası ayarlama

1. Anahtar Kasası'nı seçin, Git **erişim ilkeleri**, ve **Gelişmiş erişim ilkelerini görüntülemek için tıklayın**.
2. Etiketli kutunun seçili olduğunu seçin **Azure Disk şifrelemesi birim şifreleme için erişimi etkinleştirme**.
3. Seçin **dağıtımı için Azure sanal Makineler'e erişimi etkinleştir** ve/veya **erişimi için Azure Resource Manager şablon dağıtımı için Etkinleştir**, gerekirse. 
4. **Kaydet**’e tıklayın.

![Gelişmiş erişim ilkeleri azure anahtar kasası](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)


## <a name="bkmk_KEK"></a> Bir anahtar şifreleme anahtarı (isteğe bağlı) ayarlama
Bir ek şifreleme anahtarları için güvenlik katmanı için bir anahtar şifreleme anahtarı (KEK) kullanmak istiyorsanız bir KEK anahtar kasanızı ekleyin. Kullanım [Ekle AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey) anahtar şifreleme anahtarı anahtar kasasını oluşturmak için cmdlet'i. Ayrıca, şirket içi Anahtar Yönetimi'nden HSM bir KEK içeri aktarabilirsiniz. Daha fazla bilgi için [Key Vault belgeleri](../key-vault/key-vault-hsm-protected-keys.md). Anahtar şifreleme anahtarı belirtildiğinde, Azure Disk şifrelemesi anahtar Kasası'na yazmadan önce şifreleme parolaları sarmalamak için bu anahtarı kullanır.

* Anahtarları oluşturulurken bir RSA anahtar türü kullanın. Azure Disk şifrelemesi, Eliptik Eğri anahtarlar kullanılarak henüz desteklemiyor.

* Anahtar kasası gizli dizi ve KEK URL'leri tutulan olmalıdır. Azure, sürüm oluşturma bu kısıtlamayı zorlar. Geçerli bir gizli dizi ve KEK URL'ler için aşağıdaki örneklere bakın:

  * Geçerli bir gizli URL örneği:   *https://contosovault.vault.azure.net/secrets/EncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Geçerli bir KEK URL örneği:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure Disk şifrelemesi, anahtar kasası gizli dizileri ve KEK URL'leri bir parçası olarak belirten bir bağlantı noktası numaralarını desteklemez. Desteklenen ve desteklenmeyen key vault URL'leri örnekleri için aşağıdaki örneklere bakın:

  * Kabul edilebilir bir key vault URL'si  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Kabul edilebilir bir key vault URL'si:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*


### <a name="bkmk_KEKPSH"></a> Azure PowerShell ile bir anahtar şifreleme anahtarı ayarlama 
PowerShell betiğini kullanmadan önce betiği adımları anlamak için Azure Disk şifrelemesi önkoşulları hakkında bilgi sahibi olmalıdır. Örnek betik, ortamınız için değişiklikleri gerekebilir. Bu betik, tüm Azure Disk şifrelemesi önkoşulları oluşturur ve disk şifreleme anahtarı bir anahtar şifreleme anahtarı kullanılarak sarmalama var olan bir Iaas VM, şifreler. 

 ```powershell
 # Step 1: Create a new resource group and key vault in the same location.
     # Fill in 'MyLocation', 'MyKeyVaultResourceGroup', and 'MySecureVault' with your values.
     # Use Get-AzLocation to get available locations and use the DisplayName.
     # To use an existing resource group, comment out the line for New-AzResourceGroup
     
     $Loc = 'MyLocation';
     $KVRGname = 'MyKeyVaultResourceGroup';
     $KeyVaultName = 'MySecureVault'; 
     New-AzResourceGroup –Name $KVRGname –Location $Loc;
     New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname -Location $Loc;
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $KeyVaultResourceId = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname).ResourceId;
     $diskEncryptionKeyVaultUrl = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname).VaultUri;
     
 #Step 2: Enable the vault for disk encryption.
     Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDiskEncryption;
      
 #Step 3: Create a new key in the key vault with the Add-AzKeyVaultKey cmdlet.
     # Fill in 'MyKeyEncryptionKey' with your value.
     
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     Add-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName -Destination 'Software';
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     
 #Step 4: Encrypt the disks of an existing IaaS VM
     # Fill in 'MySecureVM' and 'MyVirtualMachineResourceGroup' with your values. 
     
     $VMName = 'MySecureVM';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
```


 
## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Windows için Azure Disk şifrelemesini etkinleştirme](azure-security-disk-encryption-windows.md)

> [!div class="nextstepaction"]
> [Linux için Azure Disk şifrelemesini etkinleştirme](azure-security-disk-encryption-linux.md)

