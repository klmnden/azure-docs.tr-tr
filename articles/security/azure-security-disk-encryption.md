---
title: "Windows ve Linux Iaas VM'ler için Azure Disk şifrelemesi | Microsoft Docs"
description: "Bu makale için Microsoft Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler genel bakış sağlar."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: d3fac8bb-4829-405e-8701-fa7229fb1725
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: kakhan
ms.openlocfilehash: 0ed575283807137f60eca005262cff27388c140f
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a>Windows ve Linux Iaas VM'ler için Azure Disk şifrelemesi
Microsoft Azure veri gizliliği, veri egemenliği ve etkinleştirir, Azure veri aralığı boyunca barındırılan denetime Gelişmiş Şifreleme teknolojileri denetlemek ve şifreleme anahtarlarını yönetmek sağlamak için kesinlikle kaydedilmiş veri denetim & Denetim erişimi. Bu Azure müşterilerin kendi iş gereksinimlerine en uygun çözümü seçim yapma esnekliği sağlar. Bu yazıda, biz, yeni bir teknoloji çözümüne "Azure Disk şifrelemesi Windows ve Linux Iaas VM'ın" korumak ve Kuruluş güvenliği ve uyumluluk taahhüt karşılamak için verilerinizi korumaya yardımcı olmak için tanıtılacaktır. Kağıt desteklenen senaryolar ve kullanıcı da dahil olmak üzere Azure disk şifrelemesi özelliklerinin nasıl kullanılacağı hakkında ayrıntılı yönergeler deneyimleri sağlar.

> [!NOTE]
> Bazı öneriler verileri, ağ veya ek lisans ya da abonelik maliyetlerinizi kaynaklanan hesaplama kaynak kullanımını artırabilir.

## <a name="overview"></a>Genel Bakış
Azure Disk şifrelemesi, Windows ve Linux Iaas sanal makine disklerinizi şifrelemek yardımcı olan yeni bir özelliktir. Azure Disk şifrelemesi yararlanır endüstri standardı [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Windows özelliğidir ve [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için özelliğidir. İle tümleşik çözüm [Azure anahtar kasası](https://azure.microsoft.com/documentation/services/key-vault/) denetlemek ve disk şifreleme anahtarları ve gizli anahtar kasası aboneliğinizde yönetmenize yardımcı olacak. Çözüm, aynı zamanda sanal makine disklerdeki tüm veriler Azure depolama alanınızı bekleyen şifrelenmesini sağlar.

Azure disk şifrelemesi Windows ve Linux Iaas VM'ler için şimdi olan **genel kullanılabilirlik** tüm Azure genel bölgeler ve premium depolama ile standart VM'ler ve sanal makineleri için AzureGov bölgeler.

### <a name="encryption-scenarios"></a>Şifreleme senaryosu
Azure Disk şifrelemesi çözümü aşağıdaki müşteri senaryoları destekler:

* Önceden şifrelenmiş VHD ve şifreleme anahtarlarını oluşturulan yeni Iaas VM'ler şifrelemeyi etkinleştirin
* Desteklenen Azure galeri görüntüleri kullanılarak oluşturulan yeni Iaas VM'ler şifrelemeyi etkinleştirin
* Azure'da çalışan Iaas VM'ler şifrelemeyi etkinleştirin
* Windows Iaas Vm'leri üzerinde şifrelemeyi devre dışı bırak
* Veri sürücülerini şifreleme Linux Iaas VM'ler için devre dışı bırak
* Yönetilen diskin VM'ler şifrelemeyi etkinleştir
* Bir mevcut şifrelenmiş premium ve premium olmayan depolama VM şifreleme ayarları
* Yedekleme ve geri yükleme şifrelenmiş VM'ler

Microsoft Azure'da etkinleştirildiğinde çözümü Iaas VM'ler için aşağıdaki senaryoları destekler:

* Azure anahtar kasası ile tümleştirme
* Standart katmanı VMs: [A, D, DS, G, GS, F ve benzeri serisi Iaas VM'ler](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Windows ve Linux Iaas Vm'leri ve yönetilen disk VM'ler desteklenen Azure Galerisi görüntülerden şifrelemeyi etkinleştirin
* Windows Iaas Vm'leri ve yönetilen disk VM'ler için işletim sistemi ve veri sürücülerde şifreleme devre dışı bırak
* Veri sürücülerini şifreleme Linux Iaas Vm'leri ve yönetilen disk VM'ler için devre dışı bırak
* Iaas Windows istemci işletim sistemi çalıştıran VM'ler şifrelemeyi etkinleştirin
* Bağlama yolları birimlerle şifrelemeyi etkinleştirin
* Linux VM diskle yapılandırılmış şifrelemeyi etkinleştirin mdadm kullanarak şeritleme (RAID)
* Veri diskleri için LVM kullanarak Linux VM'ler şifrelemeyi etkinleştirin
* İşletim sistemi ve veri diskleri için Linux LVM 7.3 şifrelemeyi etkinleştirin 
* Windows VM depolama alanları ile yapılandırılmış şifrelemeyi etkinleştirin
* Bir mevcut şifrelenmiş premium ve premium olmayan depolama VM şifreleme ayarları
* Yedekleme ve geri yükleme Hayır KEK ve KEK senaryoları (KEK - anahtar şifreleme anahtarı) şifrelenmiş VM'ler
* Tüm Azure genel ve AzureGov bölgeleri desteklenir

Çözüm, aşağıdaki senaryolarda, özellikler ve teknoloji desteklemez:

* Temel katman Iaas VM'ler
* Bir işletim sistemi sürücüsünde Linux Iaas VM'ler için şifrelemeyi devre dışı bırakma
* İşletim sistemi sürücüsü Linux Iaas VM'ler için şifreliyse, şifreleme veri sürücüsünde devre dışı bırakma
* Klasik VM oluşturma yöntemini kullanarak oluşturduğunuz Iaas VM'ler
* Windows ve Linux Iaas VM'ler müşteri özel resimler desteklenmiyor şifrelemeyi etkinleştirin.
* Şirket içi anahtar yönetimi hizmeti ile tümleştirme
* Azure dosyaları (paylaşılan dosya sistemi), ağ dosya sistemi (NFS), dinamik birimler ve yazılım tabanlı RAID sistemler ile yapılandırılmış Windows VM'ler

### <a name="encryption-features"></a>Şifreleme özellikleri
Etkinleştirme ve Azure Iaas VM'ler için Azure Disk şifrelemesi dağıtma, sağlanan yapılandırmasına bağlı olarak aşağıdaki özellikleri etkinleştirilir:

* Depolama alanınızın bekleyen önyükleme birimi korumak için işletim sistemi birimi şifrelemesi
* Depolama alanınızın kalan veri birimlerini korumak için veri birimlerinin şifrelenmesini
* Windows Iaas VM'ler için işletim sistemi ve veri sürücülerde şifreleme devre dışı bırakma
* (Yalnızca işletim sistemi şifreli değil sürücü varsa) veriler üzerinde şifrelemeyi devre dışı bırakma Linux Iaas VM'ler için sürücüler
* Şifreleme anahtarları ve gizli anahtar kasası aboneliğinizde koruma
* Şifrelenmiş Iaas VM şifreleme durumu raporlama
* Disk şifrelemesi yapılandırma ayarlarının Iaas sanal makineden kaldırma
* Yedekleme ve geri yükleme Azure Yedekleme hizmetini kullanarak şifrelenmiş VM'ler

Windows ve Linux çözüm için Iaas VM'ler için Azure Disk şifrelemesi içerir:

* Windows için disk şifrelemesi uzantısı.
* Linux için disk şifrelemesi uzantısı.
* Disk şifrelemesi PowerShell cmdlet'leri.
* Disk şifrelemesi Azure komut satırı arabirimi (CLI) cmdlet'leri.
* Disk şifrelemesi Azure Resource Manager şablonları.

Azure Disk şifrelemesi çözümü Iaas Vm'leri üzerinde Windows veya Linux işletim sistemi çalıştıran desteklenir. Desteklenen işletim sistemleri hakkında daha fazla bilgi için "Önkoşullar" bölümüne bakın.

> [!NOTE]
> VM diskleri Azure Disk şifrelemesi ile şifrelemek için ek ücret yoktur.

### <a name="value-proposition"></a>Değer önerisi
Azure Disk şifrelemesi yönetim çözümü uyguladığınızda, aşağıdaki iş gereksinimlerini karşılayabilen:

* Kurumsal güvenlik ve uyumluluk gereksinimlerini karşılayacak şekilde endüstri standardı şifreleme teknolojisi kullanabileceğinizden Iaas Vm'leri, bekleyen güvenli hale getirilir.
* Iaas Vm'leri önyükleme müşteri denetlenen anahtarlar ve ilkeleri ve altında kullanımları anahtar kasanızdaki denetleyebilirsiniz.

### <a name="encryption-workflow"></a>Şifreleme iş akışı
Windows ve Linux VM'ler için disk şifrelemeyi etkinleştirmek için aşağıdakileri yapın:

1. Bir şifreleme senaryosu önceki şifreleme senaryosu kümelerini seçin.
2. Etkinleştirme için kabul disk şifrelemesi Azure Disk şifrelemesi Resource Manager şablonu, PowerShell cmdlet'lerini veya CLI komutu aracılığıyla ve şifreleme yapılandırması belirtin.

   * Müşteri şifrelenmiş VHD senaryosu için depolama hesabınız ve anahtar kasanız için şifreleme anahtar malzemesi şifrelenmiş VHD karşıya yükleyin. Ardından, yeni bir Iaas VM şifrelemesini etkinleştirmek için şifreleme yapılandırmasını sağlayın.
   * Marketten oluşturulan yeni VM'ler ve Azure üzerinde çalışmakta olan VM'ler için Iaas VM şifrelemesini etkinleştirmek için şifreleme yapılandırmasını sağlayın.

3. Azure platformu anahtar kasanızı Iaas VM şifrelemesini etkinleştirmek için şifreleme anahtar malzemesi (BitLocker şifreleme anahtarları Windows sistemleri için) ve parola Linux için okuma izni verin.

4. Şifreleme anahtarı anahtar kasanızı malzeme yazmak için Azure Active Directory (Azure AD) uygulama kimliği sağlayın. Bunun yapılması, 2. adımda bahsedilen senaryoları için Iaas VM üzerinde şifrelemeyi etkinleştirir.

5. Azure anahtar kasası yapılandırma ve şifreleme ile VM hizmet modeli güncelleştirir ve şifrelenmiş VM'nizi ayarlar.

 ![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a>Şifre çözme iş akışı
Disk şifrelemesi Iaas VM'ler için devre dışı bırakmak için aşağıdaki üst düzey adımları tamamlayın:

1. Şifreleme (şifre çözme) devre dışı bırakmak azure'da çalışan bir Iaas VM Azure Disk şifrelemesi Resource Manager şablonu veya PowerShell cmdlet'leri seçin ve şifre çözme yapılandırma belirtin.

 Bu adım işletim sistemi veya veri hacmi ya da hem de çalışan Windows Iaas VM şifreleme devre dışı bırakır. Ancak, önceki bölümde belirtildiği gibi Linux işletim sistemi disk şifrelemesi devre dışı bırakma desteklenmiyor. İşletim sistemi diski şifrelenmemiş sürece şifre çözme adım yalnızca Linux VM'ler veri sürücülerinde izin verilir.
2. Azure VM hizmet modeli güncelleştirir ve Iaas VM şifresi çözülmüş işaretlenir. VM içeriğini artık bekleyen şifrelenir.

> [!NOTE]
> Şifreleme devre dışı bırakma işlemi anahtar kasanızı ve şifreleme anahtar malzemesi (BitLocker şifreleme anahtarları Windows sistemleri için) veya Linux için parola silmez.
 > Linux işletim sistemi disk şifrelemesi devre dışı bırakma desteklenmiyor. Şifre çözme adım yalnızca Linux VM'ler veri sürücülerinde izin verilir.
İşletim sistemi sürücüsü şifrelenmiş verileri disk şifrelemesi Linux için devre dışı bırakma desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar
"Genel bakış" bölümünde ele alınan desteklenen senaryolar için Azure Disk şifrelemesi Azure Iaas Vm'leri üzerinde etkinleştirmeden önce aşağıdaki önkoşullara bakın:

* Desteklenen bölgeleri Azure'da kaynak oluşturmak için geçerli bir etkin Azure aboneliğinizin olması gerekir.
* Azure Disk şifrelemesi, aşağıdaki Windows Server sürümlerinde desteklenir: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016.
* Azure Disk şifrelemesi, aşağıdaki Windows istemci sürümlerinde desteklenir: Windows 8 istemci ve Windows 10 istemci.

> [!NOTE]
> Windows Server 2008 R2 için .NET Framework 4.5 Azure şifreleme etkinleştirmeden önce yüklü olması gerekir. Windows Update'ten isteğe bağlı bir güncelleştirme Windows Server 2008 R2 x64 tabanlı sistemler için Microsoft .NET Framework 4.5.2 yükleyerek yükleyebilirsiniz ([KB2901983](https://support.microsoft.com/kb/2901983)).

* Azure Disk şifrelemesi aşağıdaki üzerinde desteklenen Azure Galerisi tabanlı Linux sunucu dağıtımları ve sürümler:

| Linux dağıtım | Sürüm | Şifreleme için desteklenen birim türü|
| --- | --- |--- |
| Ubuntu | 16.04 GÜNLÜK LTS | İşletim sistemi ve veri diski |
| Ubuntu | 14.04.5-DAILY-LTS | İşletim sistemi ve veri diski |
| Ubuntu | 12.10 | Veri diski |
| Ubuntu | 12.04 | Veri diski |
| RHEL | 7.4 | İşletim sistemi ve veri diski |
| RHEL | 7.3 | İşletim sistemi ve veri diski |
| RHEL | LVM 7.3 | İşletim sistemi ve veri diski |
| RHEL | 7.2 | İşletim sistemi ve veri diski |
| RHEL | 6.8 | İşletim sistemi ve veri diski |
| RHEL | 6.7 | Veri diski |
| CentOS | 7.3 | İşletim sistemi ve veri diski |
| CentOS | 7.2n | İşletim sistemi ve veri diski |
| CentOS | 6.8 | İşletim sistemi ve veri diski |
| CentOS | 7.1 | Veri diski |
| CentOS | 7.0 | Veri diski |
| CentOS | 6.7 | Veri diski |
| CentOS | 6.6 | Veri diski |
| CentOS | 6.5 | Veri diski |
| openSUSE | 13.2 | Veri diski |
| SLES | 12 SP1 | Veri diski |
| SLES | 12-SP1 (Premium) | Veri diski |
| SLES | HPC 12 | Veri diski |
| SLES | 11-SP4 (Premium) | Veri diski |
| SLES | 11 SP4 | Veri diski |

* Azure Disk şifrelemesi, anahtar kasası ve VM'lerin aynı Azure bölgesinde ve abonelik bulunmasını gerektirir.

> [!NOTE]
> Kaynaklarını ayrı bölge içinde yapılandırma Azure Disk şifrelemesi özelliği etkinleştirmek bir hata neden olur.

* Ayarlama ve Azure Disk şifrelemesi için anahtar kasanızı yapılandırma için bölümüne bakın **ayarlamak ayarlama ve Azure Disk şifrelemesi için anahtar kasanızı yapılandırma** içinde *Önkoşullar* bu makalenin.
* Ayarlama ve Azure AD uygulaması Azure Active Directory'de Azure Disk şifrelemesi için yapılandırma için bölümüne bakın **Azure Active Directory'de Azure AD uygulaması ayarlama** içinde *Önkoşullar* bu makalenin.
* Kurmak ve Azure AD uygulaması için anahtar kasası erişim ilkesini yapılandırmak için bölümüne bakın **Azure AD uygulaması için anahtar kasası erişim ilkesi ayarlama** içinde *Önkoşullar* bu makalenin.
* Önceden şifrelenmiş Windows VHD hazırlamak için bkz **önceden şifrelenmiş Windows VHD hazırlama** içinde *ek*.
* Önceden şifrelenmiş Linux VHD hazırlamak için bkz **önceden şifrelenmiş Linux VHD hazırlama** içinde *ek*.
* Azure platformu şifreleme anahtarlarını veya önyüklenir ve sanal makine işletim sistemi birimi şifresini çözer, bunları sanal makine için kullanılabilir yapmak için anahtar kasanızdaki gizli anahtarları erişimi olmalıdır. Azure platformu izinleri ayarlayın **EnabledForDiskEncryption** anahtar kasası özelliği. Daha fazla bilgi için bkz: **ayarlamak ayarlama ve Azure Disk şifrelemesi için anahtar kasanızı yapılandırma** ekte.
* Anahtar kasası gizliliği ve KEK URL'leri sürümlü olmalıdır. Azure, bu sürüm kısıtlamasını zorlar. Geçerli gizli ve KEK URL'ler için aşağıdaki örneklere bakın:

  * Geçerli bir gizli URL örneği: *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Geçerli bir KEK URL örneği: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure Disk şifrelemesi anahtar kasasına gizli anahtarları ve KEK URL'leri parçası olarak belirterek bağlantı noktası numaralarını desteklemiyor. Desteklenmeyen ve desteklenen anahtar kasası URL'leri örnekler için aşağıdakilere bakın:

  * Kabul edilebilir anahtar kasası URL'si *https://contosovault.vault.azure.net:443/gizli/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Kabul edilebilir anahtar kasası URL'si: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure Disk şifrelemesi etkinleştirmek için özellik, Iaas Vm'leri aşağıdaki ağ uç noktası yapılandırması gereksinimleri karşılaması gerekir:
  * Anahtar kasanızı bağlanmak için bir belirteç almak üzere Iaas VM bir Azure Active Directory uç noktasına bağlanabilmesi gerekir \[login.microsoftonline.com\].
  * Şifreleme anahtarları için anahtar kasanızı yazmak için Iaas VM anahtar kasası uç noktasını bağlamak mümkün olması gerekir.
  * Iaas VM Azure uzantısı depo ve VHD dosyalarını barındıran bir Azure depolama hesabı barındıran bir Azure depolama uç nokta bağlanabilmesi gerekir.

  > [!NOTE]
  > Güvenlik ilkeniz Azure vm'lerden Internet erişimi sınırlar, önceki URI çözümlemek ve IP'leri giden bağlantı izin vermek için belirli bir kuralın yapılandırın.
  >
  >Yapılandırma ve Azure anahtar kasası (https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall) güvenlik duvarının arkasında erişmek için

* Azure Disk şifrelemesi yapılandırmak için Azure PowerShell SDK sürümü'nın en son sürümünü kullanın. En son sürümünü indirme [Azure PowerShell sürüm](https://github.com/Azure/azure-powershell/releases)

 > [!NOTE]
  > Azure Disk şifrelemesi desteklenmiyor [Azure PowerShell SDK sürüm 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016). Alıyorsanız, bir hata ilgili Azure PowerShell 1.1.0 kullanarak için bkz: [Azure Disk şifrelemesi hata ilgili Azure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).

* Herhangi bir Azure CLI komutunu çalıştırın ve Azure aboneliğinizle ilişkilendirmek için ilk Azure CLI yüklemeniz gerekir:
  * Azure CLI yükleme ve Azure aboneliğinizle ilişkilendirmek için bkz: [Azure CLI'yi yükleme ve yapılandırma nasıl](../cli-install-nodejs.md).
  * Azure CLI Mac, Linux ve Windows Azure Resource Manager ile kullanmak için bkz: [Kaynak Yöneticisi modunda Azure CLI komutları](../virtual-machines/azure-cli-arm-commands.md).

* Yönetilen bir disk şifrelerken şifreleme etkinleştirmeden önce yönetilen diskin anlık görüntüsünü veya bir Azure Disk şifrelemesi dışında disk yedeğini alın zorunlu bir önkoşuldur.  Yerinde bir yedekleme olmadan şifreleme sırasında beklenmeyen bir hata disk ve VM kurtarma seçeneği olmaksızın erişilemez hale getirebilir.  Set-AzureRmVMDiskEncryptionExtension şu anda yönetilen diskleri yedekleme ve hata - skipVmBackup parametresini belirtmediyseniz yönetilen bir diske karşı kullandıysanız olur.  Bu parametre bir yedekleme Azure Disk şifrelemesi dışında kılmadığınız sürece kullanmak için güvenli değildir.   -SkipVmBackup parametresi belirtildiğinde, cmdlet şifreleme önce yönetilen disk yedeğini yapmaz.  Bu nedenle, VM kurtarma daha sonra gerektiğinde Azure Disk şifrelemesi etkinleştirilmeden önce hazır olduktan yönetilen disk yedeğini emin olmak için zorunlu önkoşul olarak değerlendirilir.  
> [!NOTE]
 > Azure Disk şifrelemesi dışında bir anlık görüntü veya yedekleme zaten yapılmış sürece - skipVmBackup parametresi hiçbir zaman kullanılmalıdır. 

* Azure Disk şifrelemesi çözüm BitLocker dış anahtar koruyucusu Windows Iaas VM'ler için kullanır. Etki alanı için TPM koruyucusu zorla tüm grup ilkeleri birleştirilmiş VM'ler, vermeyin iletin. "Uyumlu TPM'siz BitLocker izin ver" için Grup İlkesi hakkında bilgi için bkz: [BitLocker Grup İlkesi başvurusu](https://technet.microsoft.com/library/ee706521).
* Etki alanına katılmış sanal makinelerde BitLocker İlkesi özel Grup İlkesi ile aşağıdaki ayar içermelidir: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key` Bitlocker için özel Grup İlkesi ayarları uyumsuz olduğunda Azure Disk şifrelemesi başarısız olur. Doğru ilkeyi sahip değilse makinelerde ayarı, yeni ilke uygulama, (gpupdate.exe/Force) güncelleştirmek için yeni ilke zorlama ve yeniden başlatarak gerekli olabilir.  
* Azure AD uygulaması oluşturmak için bir anahtar kasası oluşturma veya varolan bir anahtar kasasını oluşturup ve şifrelemeyi etkinleştirmek bkz [Azure Disk şifrelemesi önkoşul PowerShell Betiği](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).
* Azure CLI kullanarak disk şifrelemesi önkoşulları yapılandırma için bkz: [bu Bash betik](https://github.com/ejarvi/ade-cli-getting-started).
* Azure Backup hizmeti ve şifreleme ile Azure Disk şifrelemesi etkin olduğunda şifrelenmiş VM'ler, geri yükleme kullanmak için Azure Disk şifrelemesi anahtar yapılandırmayı kullanarak Vm'leriniz şifreleyin. Yedekleme hizmeti Hayır KEK veya KEK Yapılandırması kullanılarak şifrelenmiş Vm'leri destekler. Bkz: [Azure yedekleme şifreleme ile sanal makineleri yedeklemek ve geri yükleme şifrelenmiş](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).

* Linux işletim sistemi birimi şifrelerken VM yeniden başlatma işleminin sonunda şu anda gerekli olmadığını unutmayın. Bu, portal, powershell veya CLI yapılabilir.   Şifreleme ilerlemesini izlemek için Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus tarafından döndürülen durum iletisi düzenli aralıklarla yoklar.  Şifreleme tamamlandıktan sonra bu komutu tarafından döndürülen durum iletisi bunun gösterir.  Örneğin, "ProgressMessage: işletim sistemi diski başarıyla şifrelendi, lütfen VM yeniden başlatma" Bu noktada VM yeniden ve kullanılabilecek.  

* Linux için Azure Disk şifrelemesi bağlı dosya sistemi Linux önce şifreleme sağlamak için veri diski gerektiriyor

* Veri diskleri için Linux Azure Disk Şifrelemesi tarafından desteklenmez yinelemeli olarak bağlanır. Örneğin, hedef sistem/foo/çubuğunda bir disk bağladı ve ardından başka bir /foo/bar/baz, /foo/bar/baz şifreleme başarılı olur, ancak şifreleme/foo/çubuğunun başarısız olur. 

* Azure Disk şifrelemesi yalnızca daha önce bahsedilen önkoşulları karşılaması desteklenen Azure galerisinde görüntülerinde desteklenir. Müşteri özel resimler özel bölümleme şeması ve bu görüntülerinde bulunabilecek işlemi davranışları nedeniyle desteklenmez. Ayrıca, hatta galeri görüntüsü başlangıçta önkoşulların ancak oluşturma uyumsuz sonra değiştirilmiş VM'in temel.  Bunun için bir Linux VM şifrelemek için önerilen yordamı temiz galeri görüntüden başlatmak, VM şifrelemek ve sonra gerektiği gibi VM özel yazılım veya veri eklemek için nedenidir.  

* Azure Disk şifrelemesi ve yerel veri birimi - Windows ve/mnt/azure_bek_disk güvenli bir şekilde şifreleme anahtarı tutmak Linux Iaas VM'ler için Bek birim. Silmeyin veya bu diskin tüm içeriğini Düzenle. Şifreleme anahtarı varlığı Iaas VM herhangi bir şifreleme işlemi için gerekli olmadığından disk çıkarın değil. Birimde bulunan Benioku dosyası ek bilgiler içerir.

#### <a name="set-up-the-azure-ad-application-in-azure-active-directory"></a>Azure Active Directory'de Azure AD uygulaması ayarlayın
Azure'da çalışan bir VM etkinleştirilmesi şifreleme gerektiğinde Azure Disk şifrelemesi oluşturur ve şifreleme anahtarları için anahtar kasanızı yazar. Anahtar kasanızı şifreleme anahtarlarını yönetme, Azure AD kimlik doğrulaması gerektirir.

Bu amaç için Azure AD uygulaması oluşturun. Blog postası "Get bir kimlik için uygulamayı" bölümünde uygulama kaydetme için ayrıntılı adımlar bulabilirsiniz [Azure anahtar kasası - adım adım](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Bu post ayarlama ve anahtar kasanızı yapılandırma için yararlı örnekler sayısı da içerir. Kimlik doğrulama amacıyla istemci gizli anahtarı tabanlı kimlik doğrulaması veya istemci Azure AD sertifika tabanlı kimlik doğrulaması kullanabilirsiniz.

#### <a name="client-secret-based-authentication-for-azure-ad"></a>Azure AD için istemci gizli anahtarı tabanlı kimlik doğrulaması
Aşağıdaki bölümlerde, bir istemci gizli anahtarı tabanlı kimlik doğrulaması için Azure AD yapılandırmanıza yardımcı olabilir.

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a>Azure PowerShell kullanarak Azure AD uygulaması oluşturma
Azure AD uygulaması oluşturmak için aşağıdaki PowerShell cmdlet'ini kullanın:

    $aadClientSecret = "yourSecret"
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> $azureAdApplication.ApplicationId Azure AD ClientID ve $aadClientSecret, daha sonra Azure Disk şifrelemesi sağlamak için kullanmalısınız istemci gizli anahtarı. Azure AD gizli uygun şekilde koruyun.

##### <a name="setting-up-the-azure-ad-client-id-and-secret-from-the-azure-portal"></a>Azure AD İstemci Kimliğini ve parolasını Azure portalından ayarlama
Azure AD İstemci Kimliğini ve parolasını Azure portalını kullanarak da ayarlayabilirsiniz. Bu görevi gerçekleştirmek için aşağıdakileri yapın:

1. Tıklatın **Active Directory** sekmesi.

 ![Azure Disk Şifrelemesi](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. Tıklatın **uygulama Ekle**ve ardından uygulama adı yazın.

 ![Azure Disk Şifrelemesi](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. OK düğmesine tıklayın ve ardından uygulama özelliklerini yapılandırın.

 ![Azure Disk Şifrelemesi](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. Bitirmek için sol alt köşesindeki onay işaretine tıklayın. Uygulama yapılandırma sayfası görünür ve Azure AD istemci kimliği sayfasının en altında görüntülenir.

 ![Azure Disk Şifrelemesi](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. Azure AD gizli tıklayarak Kaydet **kaydetmek** düğmesi. Azure AD gizli anahtarları metin kutusuna dikkat edin. Uygun şekilde koruyun.

 ![Azure Disk Şifrelemesi](./media/azure-security-disk-encryption/disk-encryption-fig7.png)


##### <a name="use-an-existing-application"></a>Varolan bir uygulama kullanın
Aşağıdaki komutları yürütün için elde edilir ve [Azure AD PowerShell Modülü](https://technet.microsoft.com/library/jj151815.aspx).

> [!NOTE]
> Yeni bir PowerShell penceresinden aşağıdaki komutları yürütülmelidir. Azure PowerShell veya Azure Resource Manager penceresinden komutları çalıştırmak için kullanmayın. Bu cmdlet'ler MSOnline modüle ya da Azure AD PowerShell olduğundan bu yaklaşım öneririz.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a>Azure AD için sertifika tabanlı kimlik doğrulaması
> [!NOTE]
> Azure AD sertifika tabanlı kimlik doğrulaması Linux VM'ler üzerinde şu anda desteklenmiyor.

Aşağıdaki bölümlerde Azure AD bir sertifika tabanlı kimlik doğrulamasını yapılandırmak nasıl gösterir.

##### <a name="create-an-azure-ad-application"></a>Azure AD uygulaması oluştur
Azure AD uygulaması oluşturmak için aşağıdaki PowerShell cmdlet'lerini yürütün:

> [!NOTE]
> Aşağıdaki değiştirin `yourpassword` dize güvenli parolanızla ve parola koruyun.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

Bu adımı tamamladıktan sonra anahtar kasanızı bir PFX dosyasını karşıya yükleyin ve bu sertifika için bir VM'yi dağıtmak için gereken erişim ilkesini etkinleştir.

##### <a name="use-an-existing-azure-ad-application"></a>Var olan Azure AD uygulaması kullanın
Sertifika tabanlı kimlik doğrulaması olan bir uygulamanın yapılandırıyorsanız, burada gösterilen PowerShell cmdlet'lerini kullanın. Yeni bir PowerShell penceresinden yürütülecek emin olun.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

Bu adımı tamamladıktan sonra anahtar kasanızı bir PFX dosyası karşıya yükleme ve sertifika için bir VM'yi dağıtmak için gerekli olan erişim ilkesini etkinleştir.

##### <a name="upload-a-pfx-file-to-your-key-vault"></a>Bir PFX dosyası anahtar kasanızı karşıya yükle
Bu işlem ayrıntılı bir açıklaması için bkz: [resmi Azure anahtar kasası ekip blogu](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx). Ancak, aşağıdaki PowerShell cmdlet'lerini görev için gereken her şeyi değildir. Azure PowerShell konsolundan yürütülecek emin olun.

> [!NOTE]
> Aşağıdaki değiştirin `yourpassword` dize güvenli parolanızla ve parola koruyun.

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### <a name="deploy-a-certificate-in-your-key-vault-to-an-existing-vm"></a>Anahtar kasanızı sertifikada mevcut bir VM'yi dağıtmak
PFX yüklemeyi tamamladıktan sonra aşağıdaki var olan bir VM anahtar kasasında bir sertifikası dağıtın:
 ```
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName
 ```

#### <a name="set-up-the-key-vault-access-policy-for-the-azure-ad-application"></a>Azure AD uygulaması için anahtar kasası erişim ilkesi ayarlama
Azure AD uygulaması bir anahtar veya gizli kasadaki erişim hakları gerekir. Kullanım [ `Set-AzureKeyVaultAccessPolicy` ](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) olarak (uygulama kaydolurken oluşturuldu) istemci Kimliğine kullanarak uygulamayı izinleri vermek için cmdlet _– ServicePrincipalName_ parametre değeri. Daha fazla bilgi için blog gönderisine bakın [Azure anahtar kasası - adım adım](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). PowerShell aracılığıyla bu görevi gerçekleştirmek nasıl bir örneği aşağıda verilmiştir:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> Azure Disk şifrelemesi aşağıdaki erişim ilkeleri, Azure AD İstemci uygulamanıza yapılandırmanızı gerektirir: _WrapKey_ ve _ayarlamak_ izinleri.

## <a name="terminology"></a>Terminoloji
Bu teknoloji tarafından kullanılan ortak terimleri bazıları anlamak için aşağıdaki terimleri tabloyu kullanın:

| Terminoloji | Tanım |
| --- | --- |
| Azure AD | Azure ad [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Bir Azure AD hesabının kimlik doğrulaması, depolama ve gizli anahtar Kasası'nı almak için bir önkoşuldur. |
| Azure Key Vault | Anahtar kasası, şifreleme anahtarlarını ve gizli gizli korumaya yardımcı olmak Federal Bilgi işleme standartları FIPS doğrulamalı donanım güvenlik modülleri üzerinde temel bir şifreleme, anahtar yönetim hizmetidir. Daha fazla bilgi için bkz: [anahtar kasası](https://azure.microsoft.com/services/key-vault/) belgeleri. |
| ARM | Azure Resource Manager |
| BitLocker |[BitLocker'ı](https://technet.microsoft.com/library/hh831713.aspx) Windows Iaas Vm'leri disk şifrelemesini etkinleştirmek için kullanılan bir endüstri tanınan Windows birim şifreleme teknolojisidir. |
| BEK | BitLocker şifreleme anahtarları, işletim sistemi önyükleme birimi ve veri birimlerini şifrelemek için kullanılır. BitLocker anahtarları anahtar kasasına gizli korunur. |
| CLI | Bkz: [Azure komut satırı arabirimi](../cli-install-nodejs.md). |
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux Iaas VM'ler disk şifrelemesini etkinleştirmek için kullanılan Linux tabanlı, saydam disk şifreleme alt sistemi. |
| KEK | Anahtar şifreleme anahtarını korumak veya gizli sarmalamak için kullanabileceğiniz asimetrik anahtar (RSA 2048) ' dir. Bir donanım güvenlik modülleri (HSM) sağlayabilir-korumalı anahtar veya yazılım korumalı anahtarı. Daha fazla ayrıntı için bkz: [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) belgeleri. |
| PS cmdlet'leri | Bkz: [Azure PowerShell cmdlet'lerini](/powershell/azure/overview). |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a>Ayarlama ve anahtar kasanızı Azure Disk şifrelemesi için yapılandırma
Azure Disk şifrelemesi koruma disk şifreleme anahtarları ve anahtar kasanızdaki gizli anahtarları yardımcı olur. Azure Disk şifrelemesi için anahtar kasanızı ayarlamak için aşağıdaki bölümlerin her birindeki adımları tamamlayın.

#### <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma
Bir anahtar kasası oluşturmak için aşağıdaki seçeneklerden birini kullanın:

* ["101-anahtar-kasa-" Resource Manager şablonu oluştur](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
* [Azure PowerShell anahtar kasası cmdlet'leri](/powershell/module/azurerm.keyvault/#key_vault)
* Azure Resource Manager
* Nasıl yapılır [anahtar kasanızı güvenli](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault)

> [!NOTE]
> Aboneliğiniz için bir anahtar kasasını zaten ayarladıysanız, sonraki bölüme atlayın.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a>Bir anahtar şifreleme anahtarı (isteğe bağlı) ayarlama
Ek bir BitLocker şifreleme anahtarları için güvenlik katmanı için bir KEK kullanmak istiyorsanız, bir KEK anahtar kasanızı ekleyin. Kullanım [ `Add-AzureKeyVaultKey` ](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) anahtar kasasına bir anahtar şifreleme anahtarı oluşturmak için cmdlet'i. Ayrıca, şirket içi Anahtar Yönetimi'nden HSM bir KEK içeri aktarabilirsiniz. Daha fazla ayrıntı için bkz: [anahtar kasası belgelerine](https://azure.microsoft.com/documentation/services/key-vault/).

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

Anahtar kasası arabirimi kullanarak veya Azure Resource Manager giderek KEK ekleyebilirsiniz.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a>Anahtar kasası izinleri ayarlama
Azure platformu şifreleme anahtarlarını veya bunları kullanılabilir önyükleme ve birimleri şifresini çözmek için VM yapmak için anahtar kasanızdaki gizli anahtarları erişimi olmalıdır. Azure platformu izinleri ayarlayın **EnabledForDiskEncryption** anahtar özelliğinde kasa anahtar kasası PowerShell cmdlet'ini kullanarak:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

Ayrıca ayarlayabilirsiniz **EnabledForDiskEncryption** ziyaret özelliğiyle [Azure kaynak Gezgini](https://resources.azure.com).

Daha önce belirtildiği gibi ayarlamalısınız **EnabledForDiskEncryption** anahtar kasanızı özelliği. Aksi takdirde, dağıtım başarısız olur.

Aşağıda gösterildiği gibi anahtar kasası arabiriminden Azure AD uygulamanız için erişim ilkeleri ayarlayabilirsiniz:

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

Üzerinde **Gelişmiş erişim ilkeleri** sekmesinde, anahtar kasanızı Azure Disk şifrelemesi için etkinleştirildiğinden emin olun:

![Azure anahtar kasası](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a>Disk şifrelemesi dağıtım senaryoları ve kullanıcı deneyimleri
Çok sayıda disk şifrelemesi senaryolarına olanak sağlayabilirsiniz ve adımlar senaryo göre değişebilir. Aşağıdaki bölümlerde daha ayrıntılı senaryoları kapsar.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-the-marketplace"></a>Marketten oluşturulan yeni Iaas VM'ler şifrelemeyi etkinleştirin
Azure Marketi'nden yeni Iaas Windows VM üzerinde disk şifrelemesi kullanarak etkinleştirebilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).

1. Azure Hızlı Başlangıç şablonu **Azure'a Dağıt**, şifreleme yapılandırması girin **parametreleri** dikey ve ardından **Tamam**.

2. Abonelik, kaynak grubu, kaynak grubu konumu, yasal koşulları ve anlaşma seçin ve ardından **oluşturma** yeni bir Iaas VM şifrelemesini etkinleştirmek için.

> [!NOTE]
> Bu şablon, Windows Server 2012 galeri görüntüsünü kullanan bir yeni şifrelenmiş Windows VM oluşturur.

Bu kullanarak yeni bir Iaas RedHat Linux 7.2 VM 200 GB RAID-0 diziye sahip disk şifrelemeyi etkinleştirebilirsiniz [Resource Manager şablonu](https://aka.ms/fde-rhel). Şablon dağıttıktan sonra VM şifreleme durumu kullanarak doğrulayın `Get-AzureRmVmDiskEncryptionStatus` açıklandığı gibi cmdlet [şifreleme işletim sistemi üzerinde çalışan bir Linux VM sürücü](#encrypting-os-drive-on-a-running-linux-vm). Makinenin durumunu döndüğünde _VMRestartPending_, VM'yi yeniden başlatın.

Aşağıdaki tabloda, Azure AD İstemci Kimliğini kullanarak Market senaryodan yeni VM'ler için Resource Manager şablonu parametreleri listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| adminUserName | Sanal makine için yönetici kullanıcı adı. |
| Admınpassword | Sanal makine için yönetici kullanıcı parolası. |
| newStorageAccountName | İşletim sistemi ve veri VHD'ler depolamak için depolama hesabının adı. |
| vmSize | VM boyutu. Şu anda yalnızca standart bir, D ve G serisi desteklenir. |
| virtualNetworkName | VM NIC ait olması gereken Vnet'in adı. |
| subnetName | VM NIC ait olması gereken sanal ağ içindeki alt ağ adı. |
| Aadclientıd | Gizli anahtar kasanızı yazma iznine sahip Azure AD uygulamasının istemci kimliği. |
| AADClientSecret | Gizli anahtar kasanızı yazma iznine sahip Azure AD uygulamasının istemci gizli anahtarı. |
| keyVaultURL | BitLocker anahtarını karşıya yüklenmelidir anahtar kasası URL'si. Cmdlet'ini kullanarak elde edebilirsiniz `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`. |
| keyEncryptionKeyURL | (İsteğe bağlı) oluşturulan BitLocker anahtarını şifrelemek için kullanılan anahtar şifreleme anahtarını URL'si. |
| keyVaultResourceGroup | Anahtar kasasının kaynak grubu. |
| vmName | Şifreleme işlemi üzerinde gerçekleştirilecek olan VM adıdır. |

> [!NOTE]
> _KeyEncryptionKeyURL_ isteğe bağlı bir parametredir. Anahtar kasanızı veri şifreleme anahtarı (parola gizli) daha fazla koruma için kendi KEK getirebilirsiniz.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a>VHD müşteri şifrelenmiş ve şifreleme anahtarlarını oluşturulan yeni Iaas VM'ler şifrelemeyi etkinleştirin
Bu senaryoda, Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleme etkinleştirebilirsiniz. Aşağıdaki bölümlerde, CLI komutları ve Resource Manager şablonu daha ayrıntılı olarak açıklanmaktadır.

Azure'da kullanılabilen önceden şifrelenmiş görüntüleri hazırlama için bu bölümleri birinden yönergeleri izleyin. Görüntü oluşturulduktan sonra şifrelenmiş bir Azure VM oluşturmak için sonraki bölümde adımları kullanabilirsiniz.

* [Önceden şifrelenmiş Windows VHD hazırlama](#preparing-a-pre-encrypted-windows-vhd)
* [Önceden şifrelenmiş Linux VHD hazırlama](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-the-resource-manager-template"></a>Resource Manager şablonu kullanarak
Kullanarak, şifrelenmiş VHD disk şifrelemesi etkinleştirebilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).

1. Azure Hızlı Başlangıç şablonu **Azure'a Dağıt**, şifreleme yapılandırması girin **parametreleri** dikey ve ardından **Tamam**.

2. Abonelik, kaynak grubu, kaynak grubu konumu, yasal koşulları ve anlaşma seçin ve ardından **oluşturma** yeni Iaas VM şifrelemesini etkinleştirmek için.

Aşağıdaki tabloda, şifrelenmiş VHD için Resource Manager şablonu parametreleri listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| newStorageAccountName | Şifrelenmiş işletim sistemi VHD depolamak için depolama hesabının adı. Bu depolama hesabı zaten aynı kaynak grubunu ve VM ile aynı konumda oluşturulmuş olmalıdır. |
| osVhdUri | Depolama hesabı işletim sistemi VHD'den URI'si. |
| osType | İşletim sistemi ürün türü (Windows/Linux). |
| virtualNetworkName | VM NIC ait olması gereken Vnet'in adı. Ad zaten aynı kaynak grubunu ve VM ile aynı konumda oluşturulmuş olmalıdır. |
| subnetName | Alt ağda VM NIC ait olması gereken sanal ağ adı. |
| vmSize | VM boyutu. Şu anda yalnızca standart bir, D ve G serisi desteklenir. |
| Keyvaultresourceıd | Anahtar kasası kaynağı Azure Kaynak Yöneticisi'nde tanımlayan ResourceId. PowerShell cmdlet'ini kullanarak elde edebilirsiniz `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`. |
| keyVaultSecretUrl | URL anahtar kasasını ayarladığınız disk şifreleme anahtarı. |
| keyVaultKekUrl | Oluşturulan disk şifreleme anahtarını şifrelemek için anahtar şifreleme anahtarı URL'si. |
| vmName | Iaas VM adıdır. |

#### <a name="using-powershell-cmdlets"></a>PowerShell cmdlet'lerini kullanarak
PowerShell cmdlet'ini kullanarak, şifrelenmiş VHD disk şifrelemesi etkinleştirebilirsiniz [ `Set-AzureRmVMOSDisk` ](/powershell/module/azurerm.compute/set-azurermvmosdisk).  

#### <a name="using-cli-commands"></a>CLI komutları kullanarak
Bu senaryo için disk şifrelemesi CLI komutları kullanarak etkinleştirmek için aşağıdakileri yapın:

1. Erişim ilkeleri, anahtar kasanızı ayarlayın:

   * Ayarlama **EnabledForDiskEncryption** bayrağı:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Gizli anahtar kasanızı yazmak için Azure AD uygulaması için izinleri ayarlayın:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. Var olan veya çalışan VM şifrelemesini etkinleştirmek için aşağıdakileri yazın:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Şifreleme durumu alın:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. Yeni bir VM, şifrelenmiş VHD'den şifrelemesini etkinleştirmek için aşağıdaki parametrelerle kullanmak `azure vm create` komutu:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a>Var olan veya Iaas Windows VM Azure'da çalışan şifrelemeyi etkinleştir
Bu senaryoda, Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleme etkinleştirebilirsiniz. Aşağıdaki bölümlerde Resource Manager şablonu ve CLI komutları kullanarak etkinleştirmek nasıl daha ayrıntılı olarak açıklanmaktadır.

#### <a name="using-the-resource-manager-template"></a>Resource Manager şablonu kullanarak
Var olan veya kullanarak Iaas Windows Vm'lerini Azure'da çalışan disk şifrelemesi etkinleştirebilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).

1. Azure Hızlı Başlangıç şablonu **Azure'a Dağıt**, şifreleme yapılandırması girin **parametreleri** dikey ve ardından **Tamam**.

2. Abonelik, kaynak grubu, kaynak grubu konumu, yasal koşulları ve anlaşma seçin ve ardından **oluşturma** var olan veya çalışan Iaas VM şifrelemesini etkinleştirmek için.

Aşağıdaki tabloda, var olan veya bir Azure AD İstemci Kimliğini kullanan sanal makineleri çalıştıran için Resource Manager şablonu parametreleri listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| Aadclientıd | Anahtar kasasına gizli anahtarları yazmak için izinlere sahip Azure AD uygulamasının istemci kimliği. |
| AADClientSecret | Anahtar kasasına gizli anahtarları yazmak için izinlere sahip Azure AD uygulamasının istemci gizli anahtarı. |
| keyVaultName | BitLocker anahtarını karşıya yüklenmelidir anahtar kasasının adı. Cmdlet'ini kullanarak elde edebilirsiniz `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | Oluşturulan BitLocker anahtarını şifrelemek için kullanılan anahtar şifreleme anahtarını URL'si. Bu seçerseniz isteğe bağlı bir parametredir **nokek** UseExistingKek aşağı açılan listesinde. Seçerseniz **kek** UseExistingKek aşağı açılan listesinde, girmelisiniz _keyEncryptionKeyURL_ değeri. |
| volumeType | Şifreleme işlemi gerçekleştirilir birim türü. Geçerli değerler _OS_, _veri_, ve _tüm_. |
| sequenceVersion | BitLocker işlemi sürümü dizisi. Bu sürüm numarası aynı VM'de her bir disk şifreleme işlemi gerçekleştirildiğinde artırın. |
| vmName | Şifreleme işlemi üzerinde gerçekleştirilecek olan VM adıdır. |

> [!NOTE]
> _KeyEncryptionKeyURL_ isteğe bağlı bir parametredir. Daha fazla koruma için kendi KEK anahtar Kasası'nda veri şifreleme anahtarı (BitLocker şifreleme gizli) kullanıma sunabilirsiniz.

#### <a name="using-powershell-cmdlets"></a>PowerShell cmdlet'lerini kullanarak
PowerShell cmdlet'lerini kullanarak şifreleme Azure Disk şifrelemesi ile etkinleştirme hakkında daha fazla bilgi için blog gönderilerine bakın [keşfedin Azure Disk şifrelemesi Azure PowerShell - bölüm 1 ile](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) ve [keşfedin Azure Disk şifrelemesi Azure PowerShell - bölüm 2 ile](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

#### <a name="using-cli-commands"></a>CLI komutları kullanarak
Var olan veya Iaas Windows VM CLI komutları kullanarak Azure'da çalışan şifrelemeyi etkinleştirmek için aşağıdakileri yapın:

1. Anahtar kasasına erişim ilkeleri ayarlamak için:
   * Ayarlama **EnabledForDiskEncryption** bayrağı:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Gizli anahtar kasanızı yazmak için Azure AD uygulaması için izinleri ayarlayın:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. Var olan veya çalışan VM şifrelemesini etkinleştirmek için:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. Şifreleme durumu alınamadı:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. Yeni bir VM, şifrelenmiş VHD'den şifrelemesini etkinleştirmek için aşağıdaki parametrelerle kullanmak `azure vm create` komutu:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a>Azure'da bir var olan veya çalışan Iaas Linux VM şifrelemeyi etkinleştirin
Var olan veya çalışan Iaas Linux VM'de Azure disk şifrelemesi kullanarak etkinleştirebilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

1. Tıklatın **Azure'a Dağıt** şifreleme yapılandırma Azure Hızlı Başlangıç şablonu girin **parametreleri** dikey ve ardından **Tamam**.

2. Abonelik, kaynak grubu, kaynak grubu konumu, yasal koşulları ve anlaşma seçin ve ardından **oluşturma** var olan veya çalışan Iaas VM şifrelemesini etkinleştirmek için.

Aşağıdaki tabloda, var olan veya bir Azure AD İstemci Kimliğini kullanan sanal makineleri çalıştıran için Resource Manager şablonu parametreleri listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| Aadclientıd | Anahtar kasasına gizli anahtarları yazmak için izinlere sahip Azure AD uygulamasının istemci kimliği. |
| AADClientSecret | Gizli anahtar kasanızı yazma iznine sahip Azure AD uygulamasının istemci gizli anahtarı. |
| keyVaultName | BitLocker anahtarını karşıya yüklenmelidir anahtar kasasının adı. Cmdlet'ini kullanarak elde edebilirsiniz `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | Oluşturulan BitLocker anahtarını şifrelemek için kullanılan anahtar şifreleme anahtarını URL'si. Bu seçerseniz isteğe bağlı bir parametredir **nokek** UseExistingKek aşağı açılan listesinde. Seçerseniz **kek** UseExistingKek aşağı açılan listesinde, girmelisiniz _keyEncryptionKeyURL_ değeri. |
| volumeType | Şifreleme işlemi gerçekleştirilir birim türü. Geçerli desteklenen değerler _OS_ veya _tüm_ (bkz: desteklenen Linux distro'lar ve kendi sürümleri prerequisiteis bölümünde daha önce işletim sistemi ve veri diskleri için). |
| sequenceVersion | BitLocker işlemi sürümü dizisi. Bu sürüm numarası aynı VM'de her bir disk şifreleme işlemi gerçekleştirildiğinde artırın. |
| vmName | Şifreleme işlemi üzerinde gerçekleştirilecek olan VM adıdır. |
| Parola | Veri şifreleme anahtarı güçlü bir parola yazın. |

> [!NOTE]
> _KeyEncryptionKeyURL_ isteğe bağlı bir parametredir. Anahtar kasanızı veri şifreleme anahtarı (parola gizli) daha fazla koruma için kendi KEK getirebilirsiniz.

#### <a name="cli-commands"></a>CLI komutları
Disk şifrelemesi yükleme ve kullanma, şifreli VHD etkinleştirebilirsiniz [CLI komutu](../cli-install-nodejs.md). Var olan veya CLI komutları kullanarak Azure Iaas Linux VM'ler çalıştıran şifrelemeyi etkinleştirmek için aşağıdakileri yapın:

1. Erişim ilkeleri anahtar kasasını ayarlayın:

 * Ayarlama **EnabledForDiskEncryption** bayrağı:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * Gizli anahtar kasanızı yazmak için Azure AD uygulaması için izinleri ayarlayın:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. Var olan veya çalışan VM şifrelemesini etkinleştirmek için:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Şifreleme durumu alın:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. Yeni bir VM, şifrelenmiş VHD'den şifrelemesini etkinleştirmek için aşağıdaki parametrelerle kullanmak `azure vm create` komutu:
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-the-encryption-status-of-an-encrypted-iaas-vm"></a>Şifrelenmiş bir Iaas VM'nin şifreleme durumunu Al
Azure Resource Manager kullanarak şifreleme durumu alabilirsiniz [PowerShell cmdlet'leri](/powershell/azure/overview), veya CLI komutları. Aşağıdaki bölümlerde Azure Portal ve CLI komutları şifreleme durumunu almak için nasıl kullanılacağı açıklanmaktadır.

#### <a name="get-the-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a>Azure Resource Manager kullanarak şifrelenmiş bir Windows VM şifreleme durumunu alma
Aşağıdakileri yaparak Azure Kaynak Yöneticisi'nden Iaas VM şifreleme durumu elde edebilirsiniz:

1. Oturum [Azure Portal](https://portal.azure.com/)ve ardından **sanal makineleri** aboneliğinizde sanal makineler Özet görünümünü görmek için sol bölmedeki. Abonelik adı seçerek sanal makineleri görünümünü filtreleyebilirsiniz **abonelik** aşağı açılan liste.

2. Üstündeki **sanal makineleri** sayfasında, **sütunları**.

3. Üzerinde **Seç sütun** dikey penceresinde, select **Disk şifrelemesi**ve ardından **güncelleştirme**. Şifreleme durumunu gösteren disk şifrelemesi sütun görmelisiniz _etkin_ veya _etkin_ aşağıdaki resimde gösterildiği gibi her VM için:

 ![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-the-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-the-disk-encryption-powershell-cmdlet"></a>Disk şifrelemesi PowerShell cmdlet'ini kullanarak bir şifrelenmiş (Windows/Linux) Iaas VM şifreleme durumunu Al
Disk şifrelemesi PowerShell cmdlet'inden Iaas VM şifreleme durumunu almak `Get-AzureRmVMDiskEncryptionStatus`. VM için şifreleme ayarları almak için aşağıdakileri girin:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

Çıktısını inceleyebilirsiniz _Get-AzureRmVMDiskEncryptionStatus_ URL'ler için şifreleme anahtarı.

    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey

    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

OSVolumeEncrypted ve DataVolumesEncrypted ayarları değerlerini ayarlamak _şifreli_, her iki birimin Azure Disk şifrelemesi kullanılarak şifrelenmiş olduğunu gösterir. PowerShell cmdlet'lerini kullanarak şifreleme Azure Disk şifrelemesi ile etkinleştirme hakkında daha fazla bilgi için blog gönderilerine bakın [keşfedin Azure Disk şifrelemesi Azure PowerShell - bölüm 1 ile](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) ve [keşfedin Azure Disk şifrelemesi Azure PowerShell - bölüm 2 ile](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

> [!NOTE]
> Linux VM'ler üzerinde üç ila dört dakika sürdüğünü `Get-AzureRmVMDiskEncryptionStatus` şifreleme durumu raporlamak için cmdlet.

#### <a name="get-the-encryption-status-of-the-iaas-vm-from-the-disk-encryption-cli-command"></a>Disk şifrelemesi CLI komuttan Iaas VM şifreleme durumunu Al
Iaas VM şifreleme durumu disk şifrelemesi CLI komutunu kullanarak alabileceğiniz `azure vm show-disk-encryption-status`. VM için şifreleme ayarları almak için Azure CLI oturumunuz girin:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a>Windows Iaas VM çalıştıran üzerinde şifrelemeyi devre dışı
Azure Disk şifrelemesi Resource Manager şablonu veya PowerShell cmdlet'leri üzerinden çalışan bir Windows ya da Linux Iaas VM üzerinde şifrelemeyi devre dışı bırakabilir ve şifre çözme yapılandırması belirtin.

##### <a name="windows-vm"></a>Windows VM
Devre dışı bırak-şifreleme adım işletim sistemi, veri hacmi ya da hem de çalışan Windows Iaas VM şifreleme devre dışı bırakır. İşletim sistemi birimi devre dışı bırakın ve şifrelenmiş veri birimi bırakın. Devre dışı bırak-şifreleme adım gerçekleştirildiğinde, Azure Klasik dağıtım modeli VM hizmet modeli güncelleştirir ve Windows Iaas VM şifresi çözülmüş işaretlenir. VM içeriğini artık bekleyen şifrelenir. Şifre çözme, anahtar kasanızı ve şifreleme anahtar malzemesi (Windows ve Linux için parola için BitLocker şifreleme anahtarları) silmez.

##### <a name="linux-vm"></a>Linux VM
Şifreleme devre dışı bırakma adımı çalışan Linux Iaas VM üzerindeki veri biriminin şifreleme devre dışı bırakır. Bu adım, yalnızca işletim sistemi diski şifreli değilse çalışır.

> [!NOTE]
> İşletim sistemi diski şifreleme devre dışı bırakma Linux VM'ler üzerinde izin verilmiyor.

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Var olan veya çalışan bir Iaas VM üzerinde şifrelemeyi devre dışı bırak
Disk şifrelemesi kullanarak Windows Iaas Vm'leri çalıştırma devre dışı bırakabilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).

1. Azure Hızlı Başlangıç şablonu **Azure'a Dağıt**, şifre çözme yapılandırma girin **parametreleri** dikey ve ardından **Tamam**.

2. Abonelik, kaynak grubu, kaynak grubu konumu, yasal koşulları ve anlaşma seçin ve ardından **oluşturma** yeni bir Iaas VM şifrelemesini etkinleştirmek için.

Linux VM'ler için şifreleme kullanarak devre dışı bırakabilirsiniz [çalışan bir Linux VM üzerinde şifrelemeyi devre dışı bırakma](https://aka.ms/decrypt-linuxvm) şablonu.

Aşağıdaki tabloda, çalışan bir Iaas VM üzerinde şifrelemeyi devre dışı bırakmak için Resource Manager şablonu parametreleri listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| vmName | Şifreleme işlemi üzerinde gerçekleştirilecek olan VM adıdır.
| volumeType | Bir şifre çözme işlemi gerçekleştirilir birim türü. Geçerli değerler _OS_, _veri_, ve _tüm_. Şifreleme üzerinde devre dışı bırakmadan Windows Iaas VM OS/önyükleme birimi çalıştırma şifreleme devre dışı bırakamazsınız _veri_ birim. Ayrıca, işletim sistemi diski şifreleme devre dışı bırakma Linux VM'ler verilmiyor unutmayın. |
| sequenceVersion | BitLocker işlemi sürümü dizisi. Bu sürüm numarası aynı VM'de her bir disk şifre çözme işlemi gerçekleştirildiğinde artırın. |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Var olan veya çalışan bir Iaas VM üzerinde şifrelemeyi devre dışı bırak
PowerShell cmdlet'ini kullanarak var olan veya çalışan bir Iaas VM üzerinde şifrelemeyi devre dışı bırakmak için bkz: [ `Disable-AzureRmVMDiskEncryption` ](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption). Bu cmdlet, Windows ve Linux VM'ler destekler. Şifreleme devre dışı bırakmak için uzantı sanal makineye yükler. Varsa _adı_ parametresi belirtilmezse, varsayılan ad olan uzantı _Windows VM'ler için AzureDiskEncryption_ oluşturulur.

Linux VM'ler üzerinde AzureDiskEncryptionForLinux uzantısı kullanılır.

> [!NOTE]
> Bu cmdlet, sanal makineyi yeniden başlatır.

### <a name="enable-encryption-on-pre-encrypted-iaas-vm-with-azure-managed-disk"></a>Azure yönetilen diski ile önceden şifrelenmiş Iaas VM şifrelemeyi etkinleştirin
Azure yönetilen Disk ARM şablonu şifreli bir VM konumunda bulunan ARM şablonunu kullanarak önceden şifrelenmiş bir VHD oluşturmak için kullanın   
[Yeni bir şifrelenmiş yönetilen disk önceden şifrelenmiş bir VHD/depolama blobundan Oluştur] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)

### <a name="enable-encryption-on-a-new-linux-iaas-vm-with-azure-managed-disk"></a>Azure yönetilen diski olan yeni bir Linux Iaas VM şifrelemeyi etkinleştirin
Azure yönetilen Disk ARM şablonu bir yeni şifrelenmiş Iaas konumunda bulunan ARM şablonu kullanarak Linux VM oluşturmak için kullanın   
[Tam disk şifrelemesi ile RHEL 7.2 dağıtım] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)

### <a name="enable-encryption-on-a-new-windows-iaas-vm-with-azure-managed-disk"></a>Azure yönetilen diski olan yeni bir Windows Iaas VM şifrelemeyi etkinleştirin
 Azure yönetilen Disk ARM şablonu bir yeni şifrelenmiş Iaas konumunda bulunan ARM şablonu kullanarak Linux VM oluşturmak için kullanın   
 [Yeni bir şifrelenmiş Windows Iaas yönetilen Disk VM galeri görüntüden oluşturun] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)

  > [!NOTE]
  >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen bir disk VM örnek dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce tabanlı.  Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya Azure yedekleme kullanılabilir.  Yedeklemeleri kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata oluşması durumunda olası olduğundan emin olun.  Bir yedekleme yapıldıktan sonra Set-AzureRmVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için kullanılabilir.  Bu komut, yönetilen disk tabanlı VM'in karşı bir yedekleme yaptıysanız ve bu parametre belirtilen kadar başarısız olacak.    
 
### <a name="update-encryption-settings-of-an-existing-encrypted-non-premium-vm"></a>Mevcut bir şifrelenmiş premium olmayan VM'yi şifreleme ayarlarını güncelleştir
  Var olan Azure disk desteklenen şifreleme arabirimleri, AAD istemci kimliği/gizli anahtar şifreleme anahtarı [KEK] BitLocker şifreleme anahtarı Windows VM veya Linux VM vb. için parola gibi VM [PS cmdlet'leri, CLI veya ARM şablonlarını] şifreleme ayarları çalıştırmak için kullanın. Güncelleştirme şifreleme ayarını premium ve premium olmayan depolama Vm'leri için desteklenir.

## <a name="appendix"></a>Ek
### <a name="connect-to-your-subscription"></a>Aboneliğinize bağlanma
Devam etmeden önce gözden *Önkoşullar* bu makalenin bölümünde. Tüm önkoşulların karşılandığından emin olduktan sonra aşağıdakileri yaparak aboneliğinize bağlanma:

1. Bir Azure PowerShell oturumu başlatın ve aşağıdaki komut ile Azure hesabınızda oturum açın:

    `Login-AzureRmAccount`

2. Birden çok aboneliğiniz varsa ve kullanmak üzere belirtmek için hesabınız için abonelikleri görmek için aşağıdaki komutu yazın:

    `Get-AzureRmSubscription`

3. Kullanmak istediğiniz aboneliği belirtmek için şunu yazın:

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. Yapılandırılmış aboneliği doğru olduğunu doğrulamak için şunu yazın:

    `Get-AzureRmSubscription`

5. Azure Disk şifrelemesi cmdlet'lerinin yüklü doğrulamak için şunu yazın:

    `Get-command *diskencryption*`

6. Şu çıktı Azure Disk şifrelemesi PowerShell yükleme onaylar:

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a>Önceden şifrelenmiş Windows VHD hazırlama
Aşağıdaki bölümlerde önceden şifrelenmiş Windows VHD Azure Iaas şifrelenmiş bir VHD olarak dağıtımına hazırlamak gereklidir. Hazırlama ve yeni Windows VM (VHD) Azure Site Recovery veya Azure önyükleme bilgileri kullanın.

#### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>TPM olmayan işletim sistemi koruma için izin vermek için Grup İlkesi güncelleştirme
BitLocker Grup İlkesi ayarını yapılandırma **BitLocker Sürücü Şifrelemesi**, hangi altında bulabilirsiniz **yerel bilgisayar ilkesi** > **Bilgisayar Yapılandırması** > **Yönetim Şablonları** > **Windows bileşenleri**. Bu ayarı değiştirmek **işletim sistemi sürücüleri** > **başlangıçta ek kimlik doğrulamasını gerektiren** > **izin olmadan BitLocker'ı uyumlu bir TPM**, aşağıdaki çizimde gösterildiği gibi:

![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a>BitLocker özelliği bileşenlerini yükle
Windows Server 2012 ve daha sonra aşağıdaki komutu kullanın:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

Windows Server 2008 R2 için aşağıdaki komutu kullanın:

    ServerManagerCmd -install BitLockers

#### <a name="prepare-the-os-volume-for-bitlocker-by-using-bdehdcfg"></a>İşletim sistemi birimi için BitLocker'ı kullanarak hazırlama`bdehdcfg`
İşletim sistemi bölümü Sıkıştır ve makine BitLocker için hazırlamak için aşağıdaki komutu yürütün:

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-the-os-volume-by-using-bitlocker"></a>BitLocker'ı kullanarak işletim sistemi birimi koruma
Kullanım [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) dış bir anahtar koruyucusu kullanarak önyükleme birimi şifrelemesini etkinleştirmek için komutu. Ayrıca dış sürücü veya birim üzerinde dış anahtar (.bek dosyası) yerleştirin. Şifreleme sistem/önyükleme birimde bir sonraki yeniden başlatmada etkinleştirilir.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> BitLocker'ı kullanarak dış anahtar almak için ayrı bir veri/kaynak VHD ile VM hazırlayın.

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a>Çalışan bir Linux VM işletim sistemi sürücüsünde şifreleme
Çalışan bir Linux VM işletim sistemi sürücüsünde şifrelenmesi üzerinde aşağıdaki dağıtımlar desteklenir:

* RHEL 7.2
* CentOS 7.2
* Ubuntu 16.04

##### <a name="prerequisites-for-os-disk-encryption"></a>İşletim sistemi disk şifrelemesi önkoşulları

* VM, Azure Kaynak Yöneticisi'nde Market görüntüsünden oluşturulmalıdır.
* Azure VM ile en az 4 GB RAM (boyutu 7 GB önerilir).
* (RHEL ve CentOS) SELinux devre dışı bırakın. SELinux devre dışı bırakmak için "4.4.2. bkz. SELinux devre dışı bırakma" [SELinux kullanıcının ve Administrator's Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) VM üzerinde.
* VM SELinux devre dışı bıraktıktan sonra en az bir kez yeniden başlatın.

##### <a name="steps"></a>Adımlar
1. Daha önce belirtilen dağıtımları birini kullanarak bir VM oluşturun.

 CentOS 7.2 için işletim sistemi disk şifrelemesi özel görüntüsü aracılığıyla desteklenir. Bu görüntü kullanmak için VM oluşturduğunuz sırada SKU olarak "7.2n" belirtin:
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. VM gereksinimlerinize göre yapılandırın. Adımıdır varsa tüm (işletim sistemi + veri) şifrelemek için sürücüleri, veri sürücüleri belirtilen ve gelen edilebilme /etc/fstab olması gerekir.

 > [!NOTE]
 > UUID kullanmak =... veri sürücüleri blok aygıt adı (örneğin, / dev/sdb1) belirtme yerine/etc/fstab belirtmek için. Şifreleme sırasında VM sürücüleri sırasını değiştirir. VM engelleme aygıtları belirli bir sırada dayalıysa, şifreleme sonrasında bağlama başarısız olur.

3. SSH oturumları dışında oturum açın.

4. İşletim sistemi şifrelemek için volumeType olarak belirtmek **tüm** veya **OS** olduğunda, [şifrelemeyi etkinleştirmek](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

 > [!NOTE]
 > Olarak çalışan tüm kullanıcı alanı işlemleri `systemd` Hizmetleri sonlandırıldı ile bir `SIGKILL`. VM yeniden başlatın. İşletim sistemi disk şifrelemesi çalışan bir VM üzerinde etkinleştirdiğinizde, VM kapalı kalma süresi üzerinde planlayın.

5. Düzenli aralıklarla içindeki yönergeleri kullanarak şifreleme ilerlemesini izlemek [sonraki bölümde](#monitoring-os-encryption-progress).

6. Get-AzureRmVmDiskEncryptionStatus "VMRestartPending" gösterir sonra VM için oturum açma veya portal, PowerShell veya CLI kullanarak yeniden başlatın.
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot the VM
    ```
Yeniden önce kaydetmeniz önerilir [önyükleme tanılama](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) VM.

#### <a name="monitoring-os-encryption-progress"></a>İşletim sistemi şifreleme ilerlemesini izleme
Üç yolla işletim sistemi şifreleme ilerleme durumunu izleyebilirsiniz:

* Kullanım `Get-AzureRmVmDiskEncryptionStatus` cmdlet'i ve ProgressMessage alan inceleyin:
    ```
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 "Şifreleme başlatılan işletim sistemi diski" VM ulaştıktan sonra yaklaşık 40 ila 50 dakika sürer. Premium depolama üzerinde VM yedeklenir.

 Nedeniyle [sorun #388](https://github.com/Azure/WALinuxAgent/issues/388) WALinuxAgent içinde `OsVolumeEncrypted` ve `DataVolumesEncrypted` olarak görünmesini `Unknown` bazı dağıtımları içinde. WALinuxAgent sürüm 2.1.5 ile ve daha sonra bu sorunu otomatik olarak sabit. Görürseniz `Unknown` çıktıda Azure kaynak Gezgini kullanarak disk şifrelemesi durumunu doğrulayabilirsiniz.

 Git [Azure kaynak Gezgini](https://resources.azure.com/)ve ardından bu hiyerarşide Seçim Bölmesi Sol tarafta genişletin:

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

 InstanceView içinde sürücülerinizin şifreleme durumunu görmek için aşağı kaydırın.

 ![VM örnek görünümü](./media/azure-security-disk-encryption/vm-instanceview.png)

* Bakmak [önyükleme tanılama](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/). ADE uzantısı iletilerden önekiyle `[AzureDiskEncryption]`.

* VM SSH aracılığıyla oturum açın ve uzantı günlüğü'nden alın:

    /var/log/Azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

 İşletim sistemi şifreleme işlemi devam ederken VM oturumunuzu değil, öneririz. Yalnızca diğer iki yöntemden yanıt vermediğinde günlüklerini kopyalayın.

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a>Önceden şifrelenmiş Linux VHD hazırlama
##### <a name="ubuntu-16"></a>Ubuntu 16
Şifreleme dağıtım yüklemesi sırasında aşağıdakileri yaparak yapılandırın:

1. Seçin **yapılandırma şifrelenmiş birimler** diskleri bölümlemek zaman.

 ![Ubuntu 16.04 Kurulumu](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Şifrelenmemiş olması bir ayrı önyükleme sürücüsü oluşturun. Kök sürücüsünde şifreleyin.

 ![Ubuntu 16.04 Kurulumu](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Bir parola girin. Anahtar Kasası'na karşıya parola budur.

 ![Ubuntu 16.04 Kurulumu](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Bölümleme tamamlayın.

 ![Ubuntu 16.04 Kurulumu](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. VM önyükleme için bir parola istendiğinde, adım 3'te sağlanan parolası kullanın.

 ![Ubuntu 16.04 Kurulumu](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. Azure kullanarak yüklemek için VM hazırlama [bu yönergeleri](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-ubuntu/). (VM sağlama kaldırmayı) son adımı çalıştırmayın henüz.

Aşağıdakileri yaparak Azure ile çalışmak için şifreleme yapılandırın:

1. Aşağıdaki komut dosyası içerikle /usr/local/sbin/azure_crypt_key.sh altında bir dosya oluşturun. Azure tarafından kullanılan parolayı dosya adı olduğundan KeyFileName dikkat edin.

    ```
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

2. Crypt yapılandırmada değişiklik */etc/crypttab*. Şu şekilde görünmelidir:
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. Düzenlemekte olduğunuz varsa *azure_crypt_key.sh* Windows ve, Linux çalıştırmak için kopyalanan `dos2unix /usr/local/sbin/azure_crypt_key.sh`.

4. Yürütülebilir izinleri komut dosyanıza ekleyin:
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
6. Çalıştırma `update-initramfs -u -k all` yapmak için initramfs güncelleştirmek için `keyscript` etkili.

7. Şimdi VM yetkisini kaldırma.

 ![Ubuntu 16.04 Kurulumu](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Sonraki adıma devam et ve [, VHD'yi karşıya](#upload-encrypted-vhd-to-an-azure-storage-account) Azure içine.

##### <a name="opensuse-132"></a>openSUSE 13.2
Dağıtım yüklemesi sırasında şifreleme yapılandırmak için aşağıdakileri yapın:
1. Diskleri ayırırken seçin **şifrelemek birim grubu**ve ardından bir parola girin. Bu anahtar kasanızı karşıya yükleyecek paroladır.

 ![openSUSE 13.2 Kurulumu](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Parolanızı kullanarak VM önyükleyin.

 ![openSUSE 13.2 Kurulumu](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. VM'ndaki yönergeleri izleyerek Azure'a yüklemek için hazırlama [SLES veya openSUSE bir sanal makine için Azure hazırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131). (VM sağlama kaldırmayı) son adımı çalıştırmayın henüz.

Azure ile çalışmak için şifreleme yapılandırmak için aşağıdakileri yapın:
1. /Etc/dracut.conf düzenleyin ve aşağıdaki satırı ekleyin:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Bu satırı dosya /usr/lib/dracut/modules.d/90crypt/module-setup.sh sonu Açıklama:
 ```
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

3. Dosya /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh başında aşağıdaki satırı ekleyin:
 ```
    DRACUT_SYSTEMD=0
 ```
Ve tüm oluşumlarını değiştirin:
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
yerine şunu yazın:
```
    if [ 1 ]; then
```
4. /Usr/lib/dracut/Modules.d/90crypt/cryptroot-ASK.sh düzenleyin ve "# açık LUKS aygıt için" Ekle:

    ```
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
5. Çalıştırma `/usr/sbin/dracut -f -v` initrd güncelleştirmek için.

6. VM yetkisini kaldırma artık ve [, VHD'yi karşıya](#upload-encrypted-vhd-to-an-azure-storage-account) Azure içine.

##### <a name="centos-7"></a>CentOS 7
Dağıtım yüklemesi sırasında şifreleme yapılandırmak için aşağıdakileri yapın:
1. Seçin **verilerimi şifrelemek** diskleri bölümlemek zaman.

 ![CentOS 7 Kurulumu](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Emin olun **şifrele** kök bölüm için seçilir.

 ![CentOS 7 Kurulumu](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Bir parola girin. Anahtar kasanızı karşıya yükleyecek parola budur.

 ![CentOS 7 Kurulumu](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. VM önyükleme için bir parola istendiğinde, adım 3'te sağlanan parolası kullanın.

 ![CentOS 7 Kurulumu](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. VM içindeki "CentOS 7.0 +" yönergeleri kullanarak Azure'da yüklemek için hazırlama [CentOS tabanlı sanal makine için Azure hazırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70). (VM sağlama kaldırmayı) son adımı çalıştırmayın henüz.

6. VM yetkisini kaldırma artık ve [, VHD'yi karşıya](#upload-encrypted-vhd-to-an-azure-storage-account) Azure içine.

Azure ile çalışmak için şifreleme yapılandırmak için aşağıdakileri yapın:

1. /Etc/dracut.conf düzenleyin ve aşağıdaki satırı ekleyin:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Bu satırı dosya /usr/lib/dracut/modules.d/90crypt/module-setup.sh sonu Açıklama:
```
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

3. Dosya /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh başında aşağıdaki satırı ekleyin:
```
    DRACUT_SYSTEMD=0
```
Ve tüm oluşumlarını değiştirin:
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
-
```
    if [ 1 ]; then
```
4. /Usr/lib/dracut/Modules.d/90crypt/cryptroot-ASK.sh düzenleyin ve bu "# açık LUKS sonra aygıtı" ekleyin:
    ```
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
5. Çalıştır "/ sbin/usr/dracut - f - v" initrd güncelleştirmek için.

![CentOS 7 Kurulumu](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a>Şifrelenmiş VHD bir Azure storage hesabına yükleme
BitLocker şifrelemesi veya DM-Crypt şifreleme etkinleştirildikten sonra depolama hesabınıza karşıya yüklenecek yerel şifrelenmiş VHD gerekir.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-the-disk-encryption-secret-for-the-pre-encrypted-vm-to-your-key-vault"></a>Disk şifrelemesi gizli anahtar kasanızı önceden şifrelenmiş VM'ye için karşıya yükleme
Elde ettiğiniz disk şifrelemesi gizli anahtar kasanızdaki gizli olarak daha önce yüklenmelidir. Anahtar kasası disk şifrelemesi ve izinleri için Azure AD istemcinizi etkinleştirilmiş olmalıdır.

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Disk şifreleme parolası ile KEK şifrelenmez
Anahtar kasanızdaki gizli ayarlamak için kullanın [kümesi AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret). Windows sanal makine varsa, bek dosya kodlanmış bir base64 dizesi ve, anahtar kasası kullanmaya karşıya `Set-AzureKeyVaultSecret` cmdlet'i. Linux için parola kodlanmış bir base64 dizesi ve anahtar Kasası'na karşıya yüklendi. Ayrıca, anahtar kasasına gizli oluşturduğunuzda aşağıdaki etiketlerin ayarlandığından emin olun.

    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

Kullanım `$secretUrl` için sonraki adımda [KEK kullanmadan işletim sistemi diski ekleme](#without-using-a-kek).

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>KEK ile şifrelenmiş disk şifreleme parolası
Gizli anahtar Kasası'na Karşıya yüklemeden önce bir anahtar şifreleme anahtarı kullanarak isteğe bağlı olarak şifreleyebilirsiniz. Kaydırma kullanmak [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) ilk anahtar şifreleme anahtarını kullanarak gizli anahtarı şifrelemek için. Bu kaydırma işlemi kullanarak ardından bir gizlilik olarak yükleyebilirsiniz Base64 ile kodlanmış URL dizesi çıkışıdır [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet'i.

    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

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

Kullanım `$KeyEncryptionKey` ve `$secretUrl` için sonraki adımda [KEK kullanarak işletim sistemi diski ekleme](#using-a-kek).

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a>İşletim sistemi diski taktığınızda gizli bir URL belirtin
#### <a name="without-using-a-kek"></a>Bir KEK kullanmadan
İşletim sistemi diski ekleme olsa da, geçirmeniz gereken `$secretUrl`. URL "Disk şifrelemesi gizliliği ile KEK şifrelenmez" bölümünde oluşturuldu.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a>Bir KEK kullanma
İşletim sistemi diski taktığınızda geçirmek `$KeyEncryptionKey` ve `$secretUrl`. URL "Disk şifrelemesi gizliliği ile KEK şifrelenmez" bölümünde oluşturuldu.

    Set-AzureRmVMOSDisk `
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

## <a name="download-this-guide"></a>Bu Kılavuzu'nu indirin
Bu Rehberde indirebilirsiniz [TechNet Galerisi](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).

## <a name="for-more-information"></a>Daha fazla bilgi için
[Azure PowerShell - bölüm 1 ile Azure Disk şifrelemesi keşfedin](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)  
[Azure PowerShell - bölüm 2 ile Azure Disk şifrelemesi keşfedin](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
