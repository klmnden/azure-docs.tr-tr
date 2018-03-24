---
title: Azure Disk şifrelemesi sorunlarını giderme | Microsoft Docs
description: Bu makalede, Microsoft Azure Disk şifrelemesi for Windows ve Linux Iaas VM'ler için sorun giderme ipuçları sağlar.
services: security
documentationcenter: na
author: DevTiw
manager: avibm
editor: barclayn
ms.assetid: ce0e23bd-07eb-43af-a56c-aa1a73bdb747
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: DevTiw;ejarvi;mayank88mahajan;vermashi;sudhakarareddyevuri;aravindthoram
ms.openlocfilehash: d49da52b3c45970c797150b7ff2bc6b699967977
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Azure Disk şifrelemesi sorun giderme kılavuzu

Bu, BT uzmanları, bilgi güvenlik Çözümleyicileri ve bulut Yöneticiler, kuruluşlarının Azure Disk şifrelemesi kullanın ve disk şifreleme ilgili sorunları gidermek için yardıma gereksinim kılavuzdur.

## <a name="troubleshooting-linux-os-disk-encryption"></a>Linux işletim sistemi disk şifrelemesi sorunlarını giderme

Linux işletim sistemi (OS) disk şifrelemesi, işletim sistemi sürücüsü tam disk şifreleme işlemini çalıştırmadan önce çıkarmanız gerekir. Bir hata iletisi, sürücü çıkarılamıyor ise "sonra çıkarılamadı..." oluşma olasılığı yüksektir.

Bu işletim sistemi disk şifrelemesi, değiştirilen veya kendi desteklenen stok galeri görüntüden değiştirilen hedef VM ortam denendi ortaya olasılıkla hatasıdır. İşletim sistemi sürücüsü çıkarmak için uzantının yeteneği etkileyebilir desteklenen resim sapmaları örnekleri şunlardır:
- Artık eşleşen bir desteklenen dosya sistemi veya bölümleme düzeni özelleştirilmiş görüntüler.
- Yüklü ve çalışan işletim sisteminde şifreleme önce olduklarında SAP, MongoDB, Apache Cassandra ve Docker gibi büyük uygulamaları desteklenmez.  Azure Disk şifrelemesi disk şifrelemesi işletim sistemi sürücüsünün hazırlığı için gereken şekilde bu işlemleri güvenle kapatmak üzere alamıyor.  İşletim sistemi sürücüsü dosya işleyici açık tutarak hala etkin işlemler varsa, işletim sistemi sürücüsü, işletim sistemi sürücüsünü şifrelemek için karşılanamamasına, kaldırılan olamaz. 
- Özel Kapat zaman yakınlık etkinleştiriliyor şifreleme çalışan komut dosyaları veya diğer durumunda değişiklikler VM şifreleme işlemi sırasında gerçekleştirilen. Bu çakışma bir Azure Resource Manager şablonunu aynı anda çalıştırmak için birden çok uzantı tanımladığında veya bir özel betik uzantısı veya diğer eylem aynı anda disk şifrelemesi çalışırken ortaya çıkabilir. Seri hale getirme ve yalıtma gibi adımlar sorunu çözebilir.
- Gelişmiş Güvenlik Linux (SELinux) çıkarma adım başarısız olduğu için şifreleme, etkinleştirmeden önce devre dışı değil. Şifreleme işlemi tamamlandıktan sonra SELinux yeniden iler hale.
- İşletim sistemi diski, bir mantıksal birim Yöneticisi (LVM) düzenini kullanır. Sınırlı LVM veri disk desteği kullanılabilir olsa da, LVM işletim sistemi diski değil.
- En düşük bellek gereksinimleri karşılanmadı (işletim sistemi disk şifrelemesi için 7 GB önerilen).
- Veri sürücüleri /mnt/ dizin veya birbiriyle (örneğin, /mnt/data1, /mnt/data2, /data3 + /data3/data4) altında takılı yinelemeli olarak sağlar.
- Diğer Azure Disk şifrelemesi [Önkoşullar](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Linux için karşılanmadı.

## <a name="unable-to-encrypt"></a>Şifrelenmesi başarısız oldu

Bazı durumlarda, disk şifrelemesi "OS başlatılan şifreleme disk" durmuş görünüyor Linux ve SSH devre dışıdır. Şifreleme işlemi, bir stok galeri görüntüsü tamamlanması 3-16 saat arasında sürebilir. Çoklu terabayt boyutlu veri diski eklenirse, işlem gün sürebilir.

Linux işletim sistemi disk şifreleme dizisi, işletim sistemi sürücüsü geçici olarak çıkarır. Şifrelenmiş durumundayken remounts önce sonra tüm işletim sistemi diski blok blok şifreleme gerçekleştirir. Şifreleme işlemi devam ederken Windows Azure Disk şifrelemesi VM eş zamanlı kullanım için Linux Disk şifrelemesi izin vermiyor. VM performans özelliklerini tam şifreleme için gereken zamanı önemli bir fark yapabilirsiniz. Bu özellikleri disk ve depolama hesabı standart olup veya premium (SSD) depolama boyutunu içerir.

Şifreleme durumunu denetlemek için yoklama **ProgressMessage** döndürülen alan [Get-AzureRmVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) komutu. İşletim sistemi sürücüsü şifrelenirken, VM bakım durumuna girer ve devam eden işlem herhangi kesintiyi önlemek için SSH devre dışı bırakır. **EncryptionInProgress** şifreleme işlemi devam ederken sürenin büyük bölümünden için raporları iletisi. Birkaç saat sonra bir **VMRestartPending** iletisi VM yeniden başlatmanızı ister. Örneğin:


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

Sonra VM yeniden istenir ve VM yeniden başlatıldıktan sonra hedef sunucuda gerçekleştirilecek son adımlar ve yeniden başlatma için 2-3 dakika beklemeniz gerekir. Şifreleme son olduğunda durum iletisi değişiklikleri tamamlayın. Bu ileti kullanılabilir olduktan sonra şifrelenmiş işletim sistemi sürücü kullanıma hazır olması bekleniyor ve VM yeniden kullanılmak üzere hazırdır.

Aşağıdaki durumlarda anlık görüntü veya hemen önce şifreleme alınan yedeklemeyi dön VM geri öneririz:
   - Yeniden başlatma sırası açıklanan, daha önce gerçekleşmez.
   - Önyükleme bilgisi, ilerleme iletisi ya da diğer hata Göstergeleri raporu, işletim sistemi şifreleme ortasında bu işlemi başarısız oldu Bu kılavuzda açıklanan "kaldırılması başarısız oldu" hata ileti örnektir.

Sonraki denemeden önce VM özelliklerini yeniden ve tüm önkoşullara uyduğunuzdan emin olun.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Azure Disk şifrelemesi güvenlik duvarının arkasında sorunlarını giderme
Bağlantı bir güvenlik duvarı, proxy gereksinim ya da ağ güvenlik grubu (NSG) ayarlarını sınırlı olduğunda, gerekli görevleri gerçekleştirmek için uzantı yeteneklerini kesilmiş olabilir. Durum iletileri "Uzantı durumu VM üzerinde mevcut değil." gibi bu kesintiye neden olabilir Beklenen senaryolarda tamamlamak şifreleme başarısız olur. Aşağıdaki bölümlerde, araştırabilirsiniz bazı genel güvenlik duvarı sorunları vardır.

### <a name="network-security-groups"></a>Ağ güvenlik grupları
Uygulanan ağ güvenlik grubu ayarlarını belgelenen ağ yapılandırması karşılamak uç nokta izin vermelidir [Önkoşullar](https://docs.microsoft.com/azure/security/azure-security-disk-encryption#prerequisites) disk şifrelemesi için.

### <a name="azure-key-vault-behind-a-firewall"></a>Bir güvenlik duvarının arkasında Azure anahtar kasası
VM bir anahtar kasası erişebilmeleri gerekir. Bir güvenlik duvarı ardından erişiyorsa anahtar kasasına erişim hakkında yönergeler için bkz, [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall) takım korur.

### <a name="linux-package-management-behind-a-firewall"></a>Bir güvenlik duvarının arkasında Linux paket Yönetimi

Çalışma zamanında şifreleme etkinleştirilmeden önce gerekli Önkoşul bileşenlerini yüklemek için hedef dağıtım ait paket yönetim sistemi Linux için Azure Disk şifrelemesi kullanır. Güvenlik Duvarı ayarları, bu bileşenlerin karşıdan yükleyip becerisinden VM engelliyorsa, sonraki hataları beklenir. Bu paket yönetim sistemi yapılandırma adımlarını dağıtım göre farklılık gösterebilir. Bir proxy gerektiğinde Red Hat üzerinde yum ve Abonelik Yöneticisi düzgün ayarlandığından emin olmanız gerekir. Daha fazla bilgi için bkz: [Abonelik Yöneticisi ve yum sorunlarının nasıl giderileceği](https://access.redhat.com/solutions/189533).  

## <a name="troubleshooting-windows-server-2016-server-core"></a>Windows Server 2016 Sunucu Çekirdeği sorunlarını giderme

Windows Server 2016 Server Core üzerinde bdehdcfg bileşeni varsayılan olarak kullanılamıyor. Bu bileşen tarafından Azure Disk şifrelemesi gereklidir. Sistem birimi VM yaşam süresi için yalnızca bir kez gerçekleştirilir OS birimden bölmek için kullanılır. Bu ikili dosyaları sonraki şifreleme işlemleri sırasında gerekli değildir.

Geçici çözüm için bu sorun, aşağıdaki 4 dosyaların kopyalanacağı bir Windows Server 2016 veri merkezi VM sunucu çekirdeği üzerinde aynı konuma:

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

   3. Bu komut 550 MB sistem bölümü oluşturur. Sistemi yeniden başlatın.

   4. Birimleri denetleyin ve sonra devam etmek için DiskPart komutunu kullanın.  

Örneğin:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```
## <a name="troubleshooting-encryption-status"></a>Şifreleme durumu sorunlarını giderme

Beklenen şifreleme durumu portalda bildirilmekte olduğu eşleşmiyor, lütfen aşağıdaki Destek makalesine bakın: [şifreleme durumu yanlış Azure Yönetim Portalı'ndaki görüntülenir](https://support.microsoft.com/en-us/help/4058377/encryption-status-is-displayed-incorrectly-on-the-azure-management-por)

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Disk şifrelemesi ve bu sorunları gidermeye yönelik bazı yaygın sorunlar hakkında daha fazla öğrendiniz. Bu hizmet ve özelliklerini hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Güvenlik Merkezi'nde disk şifrelemesi Uygula](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Azure sanal Makine'yi şifreleme](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Bekleyen Azure verileri şifreleme](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
