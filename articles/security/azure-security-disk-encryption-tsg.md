---
title: Sorun giderme - Iaas Vm'leri için Azure Disk şifrelemesi | Microsoft Docs
description: Bu makale için Microsoft Azure Disk şifrelemesi Windows ve Linux Iaas Vm'leri için sorun giderme ipuçları sağlar.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/12/2019
ms.custom: seodec18
ms.openlocfilehash: 3c6c552a6605278d8ab31264f5d180206e0badac
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59470704"
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Azure Disk şifrelemesi sorun giderme kılavuzu

Bu, BT uzmanları, bilgi güvenlik Çözümleyicileri ve bulut yöneticileri, kuruluşları Azure Disk şifrelemesi kullanın kılavuzudur. Bu makalede, disk şifreleme ilgili sorunları gidermeye yardımcı olmaktır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="troubleshooting-linux-os-disk-encryption"></a>Linux işletim sistemi disk şifrelemesi sorunlarını giderme

Linux işletim sistemi (OS) disk şifrelemesi, işletim sistemi sürücüsünü tam disk şifreleme işlemini çalıştırmadan önce çıkarmanız gerekir. Bir hata iletisi, sürücü çıkarılamıyor, "sonra çıkarılamadı..." oluşma olasılığı yüksektir.

İşletim sistemi disk şifreleme desteklenen stok galeri görüntüden değiştirildi bir hedef VM ortamında çalıştığında bu hata oluşabilir. Desteklenen görüntü sapmalar, işletim sistemi sürücüsünü çıkaramadı uzantının özelliği sayesinde engelleyebilir. Sapmalar örnekleri aşağıdaki öğeleri içerebilir:
- Artık eşleşen bir desteklenen dosya sistemi veya bölümleme düzeni özelleştirilmiş görüntüler.
- Yüklü ve çalışır işletim sisteminde şifreleme önce olduklarında, SAP, MongoDB, Apache Cassandra ve Docker gibi büyük uygulamalar desteklenmez. Azure Disk şifrelemesi, güvenli bir şekilde gerektiği gibi disk şifreleme için hazırlık işletim sistemi sürücüsünün bu işlemlerin kapatılması silemiyor. Dosya tanıtıcılarını Aç için işletim sistemi sürücüsünü bulunduran hala etkin işlemler varsa, işletim sistemi sürücüsünü, işletim sistemi sürücüsünü şifrelemek için bir neden, kaldırılan olamaz. 
- Özel Kapat zaman yakınlık etkinleştiriliyor şifreleme için çalışan komut dosyaları veya diğer durumunda değişiklikler VM'de şifreleme işlemi sırasında yapılmıştır. Birden çok uzantı aynı anda yürütmek için bir Azure Resource Manager şablonu tanımlar veya bir özel betik uzantısı ya da diğer eylem aynı anda disk şifrelemesi için çalıştığında, bu çakışma oluşabilir. Serileştirme ve yalıtma gibi adımlar sorunu çözebilir.
- Güvenliği artırılmış Linux (SELinux) çıkarma adım başarısız olduğu için şifreleme, etkinleştirmeden önce devre dışı edilmemiş. Şifreleme işlemi tamamlandıktan sonra SELinux yeniden iler hale.
- İşletim sistemi diski, bir mantıksal birim Yöneticisi (LVM) şeması kullanır. Sınırlı LVM veri disk desteği kullanılabilir olsa da, bir LVM işletim sistemi diski yok.
- Olmayan en düşük bellek gereksinimleri karşılanıyor (7 GB, işletim sistemi disk şifreleme için önerilir).
- Veri sürücüleri /mnt/ dizini veya birbiriyle (örneğin, /mnt/data1, /mnt/data2, /data3 + /data3/data4) altında yinelemeli olarak sağlar.
- Diğer Azure Disk şifrelemesi [önkoşulları](azure-security-disk-encryption-prerequisites.md) Linux için karşılanmadığı.

## <a name="bkmk_Ubuntu14"></a> Ubuntu 14.04 LTS varsayılan çekirdek güncelleştirmesi

Ubuntu 14.04 LTS görüntüsünü 4.4 varsayılan çekirdek sürümü ile birlikte gelir. Bu çekirdek sürümü, yetersiz bellek kaldırıcı yanlış gg komutu işletim sistemi şifreleme işlemi sırasında sonlandırır bilinen bir sorun var. Bu hatanın en son düzeltildiğini Azure Linux çekirdeğinin ayarlanmış. Görüntü şifreleme etkinleştirilmeden önce bu hatayı önlemek için güncelleştirme [Azure çekirdek 4.15 ayarlanmış](https://packages.ubuntu.com/trusty/linux-azure) veya daha sonra aşağıdaki komutları kullanarak:

```
sudo apt-get update
sudo apt-get install linux-azure
sudo reboot
```

VM yeni çekirdeğe yeniden başlatıldıktan sonra yeni çekirdek sürümü kullanarak onaylanabilir:

```
uname -a
```

## <a name="update-the-azure-virtual-machine-agent-and-extension-versions"></a>Uzantı sürümleri ve Azure sanal makine aracısını güncelleştir

Azure Disk şifrelemesi işlemleri, Azure sanal makine Aracısı'nın desteklenmeyen sürümleri kullanılarak sanal makine görüntülerinde başarısız olabilir. Daha fazla bilgi için bkz [azure'da sanal makine aracıları için Minimum sürüm desteği](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).  

Microsoft.Azure.Security.AzureDiskEncryption veya Microsoft.Azure.Security.AzureDiskEncryptionForLinux Konuk Aracısı uzantısı doğru sürümünü de gereklidir. Uzantı sürümleri tutulur ve Azure sanal makine Aracısı önkoşullara uyduğunuzdan ve desteklenen bir sanal makine Aracısı sürümü kullanıldığında platform tarafından otomatik olarak güncelleştirilir.

Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux uzantı kullanım dışı bırakıldı ve artık desteklenmiyor.  

## <a name="unable-to-encrypt-linux-disks"></a>Linux diskleri şifrelenemiyor

Bazı durumlarda, disk şifreleme "İşletim sistemi şifreleme kullanmaya disk" durmuş görünüyor Linux ve SSH devre dışı bırakıldı. Şifreleme işlemi, bir stok galeri görüntüsü üzerinde tamamlamak için 3-16 saat arasında sürebilir. Çoklu terabayt boyutlu veri diski eklenirse, işlem gün sürebilir.

Linux işletim sistemi disk şifreleme dizisi, işletim sistemi sürücüsünü geçici olarak çıkarır. Ardından, şifrelenmiş durumunda remounts önce tüm işletim sistemi diskinin blok blok şifreleme gerçekleştirir. Şifreleme işlemi devam ederken Windows üzerinde Azure Disk şifrelemesi, eş zamanlı kullanımını VM için Linux Disk şifrelemesi izin vermez. VM performans özellikleri, şifrelemeyi tamamlamak için gereken süreyi önemli bir fark yapabilirsiniz. Bu özellikleri, disk ve depolama hesabı olup standart veya premium (SSD) depolama boyutunu içerir.

Şifreleme durumu denetlemek için yoklama **ProgressMessage** döndürüldüğü alan [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) komutu. İşletim sistemi sürücüsünü şifrelenir, ancak VM hizmet durumu girer ve herhangi bir kesinti devam eden işlemi için SSH devre dışı bırakır. **EncryptionInProgress** şifreleme işlemi devam ederken çoğu zaman raporları iletisi. Birkaç saat sonra bir **VMRestartPending** ileti sanal Makineyi yeniden başlatmanızı ister. Örneğin:


```azurepowershell
PS > Get-AzVMDiskEncryptionStatus -ResourceGroupName "MyVirtualMachineResourceGroup" -VMName "VirtualMachineName"
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : EncryptionInProgress
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started

PS > Get-AzVMDiskEncryptionStatus -ResourceGroupName "MyVirtualMachineResourceGroup" -VMName "VirtualMachineName"
OsVolumeEncrypted          : VMRestartPending
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk successfully encrypted, please reboot the VM
```

Sonra sanal Makineyi yeniden başlatmanız istenir ve VM yeniden başlatıldıktan sonra hedef üzerinde gerçekleştirilecek ilişkin son adımlar ve yeniden başlatma için 2-3 dakika beklemeniz gerekir. Şifreleme son olduğunda durum iletisi değişiklikleri tamamlayın. Bu ileti kullanılabilir olduktan sonra şifrelenmiş işletim sistemi sürücüsünü kullanıma hazır olması beklenir ve VM yeniden kullanılmak üzere hazırdır.

Aşağıdaki durumlarda anlık görüntü veya hemen önce şifreleme alınan yedeklemeyi dön VM geri yükleme öneririz:
   - Daha önce açıklanan yeniden başlatma sırası gerçekleşmezse.
   - Önyükleme bilgileri, ilerleme iletisi ya da diğer hata göstergeleri, işletim sistemi şifreleme bildirirse ortasında bu işlem başarısız oldu. Bu kılavuzda açıklanan "çıkarma başarısız oldu" hata iletisi bir örneğidir.

Ve sonraki girişiminden önce VM özelliklerini yeniden ve tüm önkoşulların karşılandığından emin olun.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Güvenlik duvarının arkasında Azure Disk şifrelemesi sorunlarını giderme
Bağlantı bir güvenlik duvarı, proxy gereksinim veya ağ güvenlik grubu (NSG) ayarları tarafından kısıtlanmış, görevleri gerçekleştirmek için uzantının özelliği kesilmiş olabilir. Durum iletileri "Uzantı durumu sanal makinede kullanılabilir değil." gibi bu kesintiye neden olabilir Beklenen senaryolarda şifrelemeyi tamamlamak başarısız olur. Aşağıdaki bölümlerde, araştırabilirsiniz bazı yaygın bir güvenlik duvarı sorunlar var.

### <a name="network-security-groups"></a>Ağ güvenlik grupları
Uygulanan ağ güvenlik grubu ayarlarını belgelenmiş ağ yapılandırmasını karşılamak uç nokta hala izin vermelidir [önkoşulları](azure-security-disk-encryption-prerequisites.md#bkmk_GPO) disk şifrelemesi için.

### <a name="azure-key-vault-behind-a-firewall"></a>Bir güvenlik duvarının arkasındaki Azure Key Vault

Şifreleme etkinleştirildiğinde ile [Azure AD kimlik](azure-security-disk-encryption-prerequisites-aad.md), hedef sanal Makineyi hem Azure Active Directory uç noktalarına hem de anahtar kasası uç noktaları bağlanmaya izin vermelidir. Geçerli Azure Active Directory kimlik doğrulama uç noktaları 56 ve 59, bölümlerde tutulan [Office 365 URL'leri ve IP adresi aralıkları](https://docs.microsoft.com/office365/enterprise/urls-and-ip-address-ranges) belgeleri. Key Vault yönergeler, ilgili belgelerde verilmiştir [bir güvenlik duvarının arkasındaki Azure anahtar kasası](../key-vault/key-vault-access-behind-firewall.md).

### <a name="azure-instance-metadata-service"></a>Azure örnek meta veri hizmeti 
VM erişebilir olmalıdır [Azure örnek meta veri hizmetine](../virtual-machines/windows/instance-metadata-service.md) iyi bilinen yönlendirilemeyen bir IP adresi kullanan uç noktası (`169.254.169.254`), erişilebilir yalnızca VM içinden.  Bu adrese (örneğin, X-iletilen-için üst bilgi ekleme) yerel HTTP trafiğini alter proxy yapılandırmaları desteklenmez.

### <a name="linux-package-management-behind-a-firewall"></a>Bir güvenlik duvarının arkasında Linux paket Yönetimi

Çalışma zamanında, Linux için Azure Disk şifrelemesi şifreleme etkinleştirilmeden önce gerekli Önkoşul bileşenlerini yüklemek için hedef dağıtım ait paket yönetim sistemini kullanır. Güvenlik Duvarı ayarlarını VM indirebilir ve bu bileşenleri yüklemek engellemek, ardından sonraki hataları beklenmektedir. Bu paket yönetim sistemini yapılandırma adımları dağıtım göre farklılık gösterebilir. Bir proxy gerektiğinde Red Hat üzerinde size Abonelik Yöneticisi ve yum düzgün ayarlandığından emin olmanız gerekir. Daha fazla bilgi için [Abonelik Yöneticisi ve yum sorunları nasıl giderilir](https://access.redhat.com/solutions/189533).  


## <a name="troubleshooting-windows-server-2016-server-core"></a>Windows Server 2016 Sunucu Çekirdeği sorunlarını giderme

Windows Server 2016 Server Core üzerinde bdehdcfg bileşeni varsayılan olarak sağlanmaz. Bu bileşen tarafından Azure Disk şifrelemesi gereklidir. Sistem biriminin sanal makinenin yaşam süresi için yalnızca bir kez gerçekleştirilir işletim sistemi birimden bölmek için kullanılır. Bu ikili dosyalar daha sonra şifreleme işlemleri sırasında gerekli değildir.

Bu sorunu çözmek için sunucu çekirdeği üzerinde aynı konuma bir Windows Server 2016 veri merkezi sanal makinesinden aşağıdaki dört dosyaları kopyalayın:

   ```
   \windows\system32\bdehdcfg.exe
   \windows\system32\bdehdcfglib.dll
   \windows\system32\en-US\bdehdcfglib.dll.mui
   \windows\system32\en-US\bdehdcfg.exe.mui
   ```

1. Aşağıdaki komutu girin:

   ```
   bdehdcfg.exe -target default
   ```

1. Bu komut, 550 MB sistem bölümü oluşturur. Sistemi yeniden başlatın.

1. Birimleri denetleyin ve ardından devam etmek için DiskPart kullanın.  

Örneğin:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```
<!-- ## Troubleshooting encryption status

If the expected encryption state does not match what is being reported in the portal, see the following support article:
[Encryption status is displayed incorrectly on the Azure Management Portal](https://support.microsoft.com/en-us/help/4058377/encryption-status-is-displayed-incorrectly-on-the-azure-management-por) --> 

## <a name="troubleshooting-encryption-status"></a>Şifreleme durumu sorunlarını giderme 

Portal bile VM içinden şifrelenmemiş edildikten sonra bir disk olarak şifrelenmiş görüntüleyebilir.  Alt düzey komutları doğrudan diskten daha yüksek düzey Azure Disk şifrelemesi yönetimi komutları kullanmak yerine VM'de sıfırlamaktır kullanıldığında ortaya çıkabilir.  Daha yüksek düzeyde, yalnızca diskten VM içinde sıfırlamaktır, ancak VM dışında Ayrıca önemli platform düzeyinde şifreleme ayarları ve VM ile ilişkili uzantı ayarları güncelleştirme komutları.  Bu hizalama tutulmazsa, platform şifreleme durumu bildirmek veya VM'yi düzgün şekilde sağlamak mümkün olmayacaktır.   

Düzgün bir şekilde Azure Disk şifrelemesi devre dışı bırakmak için şifreleme ile etkin bilinen bir iyi durumda başlatın ve ardından [devre dışı bırak AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) ve [Remove-AzVMDiskEncryptionExtension](/powershell/module/az.compute/remove-azvmdiskencryptionextension) Powershell komutlar, veya [az vm şifreleme devre dışı](/cli/azure/vm/encryption) CLI komutu. 

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Disk şifrelemesi ve bu sorunları gidermeye yönelik bazı yaygın sorunlar hakkında daha fazla öğrendiniz. Bu hizmet ve özellikleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Güvenlik Merkezi'nde Azure disk şifrelemesi Uygula](../security-center/security-center-apply-disk-encryption.md)
- [Azure veri bekleme sırasında şifreleme](azure-security-encryption-atrest.md)
