---
title: Sorun giderme - Iaas Vm'leri için Azure Disk şifrelemesi | Microsoft Docs
description: Bu makale için Microsoft Azure Disk şifrelemesi Windows ve Linux Iaas Vm'leri için sorun giderme ipuçları sağlar.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 01/25/2019
ms.custom: seodec18
ms.openlocfilehash: 70cf6c65592eef94ce657c9aaef7dc78de4ffa11
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55468402"
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Azure Disk şifrelemesi sorun giderme kılavuzu

Bu, BT uzmanları, bilgi güvenlik Çözümleyicileri ve bulut yöneticileri, kuruluşları Azure Disk şifrelemesi kullanın kılavuzudur. Bu makalede, disk şifreleme ilgili sorunları gidermeye yardımcı olmaktır.

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

## <a name="unable-to-encrypt-linux-disks"></a>Linux diskleri şifrelenemiyor

Bazı durumlarda, disk şifreleme "İşletim sistemi şifreleme kullanmaya disk" durmuş görünüyor Linux ve SSH devre dışı bırakıldı. Şifreleme işlemi, bir stok galeri görüntüsü üzerinde tamamlamak için 3-16 saat arasında sürebilir. Çoklu terabayt boyutlu veri diski eklenirse, işlem gün sürebilir.

Linux işletim sistemi disk şifreleme dizisi, işletim sistemi sürücüsünü geçici olarak çıkarır. Ardından, şifrelenmiş durumunda remounts önce tüm işletim sistemi diskinin blok blok şifreleme gerçekleştirir. Şifreleme işlemi devam ederken Windows üzerinde Azure Disk şifrelemesi, eş zamanlı kullanımını VM için Linux Disk şifrelemesi izin vermez. VM performans özellikleri, şifrelemeyi tamamlamak için gereken süreyi önemli bir fark yapabilirsiniz. Bu özellikleri, disk ve depolama hesabı olup standart veya premium (SSD) depolama boyutunu içerir.

Şifreleme durumu denetlemek için yoklama **ProgressMessage** döndürüldüğü alan [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) komutu. İşletim sistemi sürücüsünü şifrelenir, ancak VM hizmet durumu girer ve herhangi bir kesinti devam eden işlemi için SSH devre dışı bırakır. **EncryptionInProgress** şifreleme işlemi devam ederken çoğu zaman raporları iletisi. Birkaç saat sonra bir **VMRestartPending** ileti sanal Makineyi yeniden başlatmanızı ister. Örneğin:


```
PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : EncryptionInProgress
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started

PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
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
Şifreleme etkinleştirildiğinde ile [Azure AD kimlik](azure-security-disk-encryption-prerequisites-aad.md), hedef sanal Makineyi Azure AD kimlik doğrulama uç noktaları ve bunun yanı sıra anahtar kasası uç noktalar için erişim verilmesi gerekir.  Bu işlem hakkında daha fazla bilgi için bir güvenlik duvarının korumasında anahtar kasasına erişme konusunda yönergeler için lütfen bakın, [Azure anahtar kasası](../key-vault/key-vault-access-behind-firewall.md) takım tutar. 

### <a name="azure-instance-metadata-service"></a>Azure örnek meta veri hizmeti 
VM erişebilir olmalıdır [Azure örnek meta veri hizmetine](../virtual-machines/windows/instance-metadata-service.md) iyi bilinen yönlendirilemeyen bir IP adresi kullanan uç noktası (`169.254.169.254`), erişilebilir yalnızca VM içinden.

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

   2. Aşağıdaki komutu girin:

   ```
   bdehdcfg.exe -target default
   ```

   3. Bu komut, 550 MB sistem bölümü oluşturur. Sistemi yeniden başlatın.

   4. Birimleri denetleyin ve ardından devam etmek için DiskPart kullanın.  

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

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Disk şifrelemesi ve bu sorunları gidermeye yönelik bazı yaygın sorunlar hakkında daha fazla öğrendiniz. Bu hizmet ve özellikleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Güvenlik Merkezi'nde Azure disk şifrelemesi Uygula](../security-center/security-center-apply-disk-encryption.md)
- [Azure veri bekleme sırasında şifreleme](azure-security-encryption-atrest.md)
