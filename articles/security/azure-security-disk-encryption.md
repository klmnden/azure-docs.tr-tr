---
title: Windows ve Linux Iaas sanal makineleri için Azure Disk şifrelemesi | Microsoft Docs
description: Bu makale için Microsoft Azure Disk şifrelemesi Windows ve Linux Iaas sanal makineleri hakkında genel bir bakış sağlar.
services: security
documentationcenter: na
author: DevTiw
manager: avibm
editor: barclayn
ms.assetid: d3fac8bb-4829-405e-8701-fa7229fb1725
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/13/2018
ms.author: devtiw
ms.openlocfilehash: f350716d0ca906376f3eadce9e117694ff14515c
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2018
ms.locfileid: "35756405"
---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a>Windows ve Linux Iaas sanal makineleri için Azure Disk şifrelemesi
Microsoft Azure, veri gizliliği, veri egemenliği ve şifreleme teknolojileri, Azure'da barındırılan bir dizi aracılığıyla verileri denetlemek için Gelişmiş etkinleştirir, denetlemek ve şifreleme anahtarlarını yönetmek sağlamak için kesin kaydedilmiş veri denetimi ve Denetim erişim. Bu, Azure müşterilerine iş gereksinimlerini en iyi karşılayan çözümü seçme esnekliği sağlar. Bu yazıda, biz, yeni bir teknoloji çözümü "Azure Disk şifrelemesi Windows ve Linux Iaas VM'ın" Kurumsal güvenlik ve uyumluluk taahhütlerinizi yerine verilerinizi koruyarak yardımcı olmak için neden. İnceleme deneyimlerini desteklenen senaryolar ve kullanıcı dahil olmak üzere Azure disk şifreleme özelliklerini kullanma hakkında ayrıntılı kılavuz sağlar.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]


## <a name="overview"></a>Genel Bakış
Azure Disk şifrelemesi, Windows ve Linux Iaas sanal makine disklerinizi şifreleyin yardımcı olan yeni bir özelliktir. Azure Disk şifrelemesi, endüstri standardı yararlanır [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Windows özelliğidir ve [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için bir özelliğidir. İle tümleşik bir çözüm [Azure anahtar kasası](https://azure.microsoft.com/documentation/services/key-vault/) denetlemenize ve disk şifreleme anahtarlarını ve gizli anahtar kasası aboneliğinizi yönetmenize yardımcı olmak için. Çözüm ayrıca sanal makine disklerindeki tüm veriler Azure depolama alanınızda bekleyen şifrelenmesini sağlar.

Windows ve Linux Iaas sanal makineleri için Azure disk şifrelemesi artık olan **genel kullanılabilirlik** tüm genel Azure bölgeleri ve premium depolama ile standart VM'ler ve VM'ler için AzureGov bölgelerde.

> [!NOTE]
> Bazı öneriler, veri, ağ veya ek lisans ya da abonelik maliyetlerinizi kaynaklanan işlem kaynak kullanımını artırabilir.


### <a name="encryption-scenarios"></a>Şifreleme senaryoları
Azure Disk şifrelemesi çözümü, aşağıdaki müşteri senaryoları destekler:

* Önceden şifrelenmiş VHD ve şifreleme anahtarlarını oluşturulan yeni Iaas Vm'leri şifrelemeyi etkinleştirin
* Desteklenen Azure galeri görüntüleri kullanılarak oluşturulan yeni Iaas Vm'leri şifrelemeyi etkinleştirin
* Azure'da çalıştırılan Iaas Vm'leri mevcut şifrelemeyi etkinleştirin
* Windows Iaas Vm'leri üzerinde şifrelemeyi devre dışı bırak
* Linux Iaas sanal makineler için veri sürücülerinde şifrelemeyi devre dışı bırakma
* Yönetilen disk Vm'lerinin şifrelenmesini etkinleştirmek
* Mevcut şifrelenmiş premium ve premium olmayan depolama VM'nin şifreleme ayarlarını güncelleştirme
* Yedekleme ve şifrelenmiş Vm'leri geri yükleme

Microsoft Azure'da etkinleştirildiğinde çözüm Iaas Vm'leri için aşağıdaki senaryoları destekler:

* Azure Key Vault ile tümleştirme
* Standart katmanı Vm'lerini: [A, D, DS, G, GS, F ve benzeri serisi Iaas Vm'leri](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Windows ve Linux Iaas Vm'leri ve yönetilen disk sanal makinelerini desteklenen Azure Galerisi görüntülerinden şifrelemeyi etkinleştirin
* Windows Iaas Vm'leri ve yönetilen disk sanal makinelerini için işletim sistemi ve veri sürücülerini şifreleme devre dışı bırak
* Linux Iaas Vm'leri ve yönetilen disk sanal makinelerini için veri sürücülerinde şifrelemeyi devre dışı bırakma
* Windows istemci işletim sistemi çalıştıran Iaas Vm'leri şifrelemeyi etkinleştirin
* Bağlama yolu birimlerle şifrelemeyi etkinleştirin
* Disk ile yapılandırılmış Linux vm'lerinde şifrelemeyi etkinleştirme mdadm kullanarak şeritleme (RAID)
* Linux vm'lerinde LVM kullanarak veri diskleri için şifrelemeyi etkinleştirme
* LVM'yi 7.3 Linux işletim sistemi ve veri diskleri için şifrelemeyi etkinleştirin 
* Depolama alanları ile yapılandırılmış Windows vm'lerinde şifrelemeyi etkinleştir
* Mevcut şifrelenmiş premium ve premium olmayan depolama VM'nin şifreleme ayarlarını güncelleştirme
* Yedekleme ve no-KEK hem KEK senaryoları (KEK - anahtar şifreleme anahtarı) şifrelenmiş Vm'leri geri yükleme
* Tüm Azure genel ve AzureGov bölgeler desteklenir

Çözüm, aşağıdaki senaryolarda, özellikleri ve teknoloji desteklemez:

* Temel katman Iaas Vm'leri
* Linux Iaas Vm'leri için şifreleme bir işletim sistemi sürücüsünde devre dışı bırakma
* Linux Iaas sanal makineleri için işletim sistemi sürücüsünü şifrelendiyse veri sürücüsü üzerinde şifrelemeyi devre dışı bırakma
* Iaas VM'ler, Klasik VM oluşturma yöntemini kullanarak oluşturulur
* Windows ve Linux Iaas Vm'leri müşteri özel görüntüleri desteklenmiyor şifrelemeyi etkinleştirin.
* Şirket içi anahtar yönetimi hizmeti ile tümleştirme
* Azure dosyaları (paylaşılan dosya sistemi), ağ dosya sistemi (NFS), dinamik birimler ve yazılım tabanlı RAID sistemler ile yapılandırılan Windows Vm'leri

### <a name="encryption-features"></a>Şifreleme özellikleri
Etkinleştirme ve Azure Disk şifrelemesi için Azure Iaas Vm'lerini dağıtmak, sağlanan yapılandırmasına bağlı olarak aşağıdaki özellikleri etkinleştirilir:

* Önyükleme birimi depolama alanınızda bekleyen korumak için işletim sistemi birimi şifrelemesi
* Veri birimleri, depolama alanınızı bölgesinde, bekleyen veri birimlerini korumak için şifreleme
* Windows Iaas Vm'leri için işletim sistemi ve veri sürücülerini şifreleme devre dışı bırakma
* (Yalnızca işletim sistemi şifreli değil sürücü varsa) Linux Iaas sanal makineleri için veri şifrelemeyi devre dışı sürücüler
* Şifreleme anahtarlarını ve gizli anahtar kasası aboneliğinizdeki koruma
* Şifrelenmiş Iaas VM şifreleme durumunu raporlama
* Iaas sanal makine disk şifreleme yapılandırma ayarlarını, temizleme
* Backup ve Azure Backup hizmetini kullanarak şifrelenmiş Vm'leri geri yükleme

Azure Disk şifrelemesi çözümü, Windows ve Linux Iaas VM'leri için şunları içerir:

* Windows için disk şifrelemesi uzantısı.
* Linux için disk şifrelemesi uzantısı.
* Disk şifrelemesi PowerShell cmdlet'leri.
* Disk şifreleme Azure komut satırı arabirimi (CLI) cmdlet'leri.
* Disk şifrelemesi Azure Resource Manager şablonları.

Azure Disk şifrelemesi çözümü, Windows veya Linux işletim sistemi çalıştırılan Iaas Vm'lerine desteklenir. Desteklenen işletim sistemleri hakkında daha fazla bilgi için "Önkoşullar" bölümüne bakın.

> [!NOTE]
> Azure Disk şifrelemesi ile VM disklerini şifrelemek için ek ücret yoktur.

### <a name="value-proposition"></a>Değer önerisi
Azure Disk şifrelemesi yönetim çözümü uyguladığınızda, aşağıdaki iş gereksinimlerini karşılamak:

* Kurumsal güvenlik ve uyumluluk gereksinimlerini karşılamak için endüstri standardı şifreleme teknolojisini kullanabileceğinizden Iaas Vm'leri bekleme durumundayken güvenlidir.
* Müşteri tarafından denetlenen anahtarları ve ilkeleri ve altında Iaas Vm'leri önyükleme kullanımlarını anahtar kasanızdaki denetleyebilirsiniz.

### <a name="encryption-workflow"></a>Şifreleme iş akışı
Windows ve Linux Vm'leri için disk şifrelemeyi etkinleştirmek için aşağıdakileri yapın:

1. Bir şifreleme senaryosu önceki şifreleme senaryolarının arasından seçin.
2. Etkinleştirme için iyileştirilmiş disk şifrelemesi Azure Disk şifrelemesi Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutu aracılığıyla ve şifreleme yapılandırması belirtin.

   * Müşteri şifrelenmiş VHD senaryosu için depolama hesabınızı ve şifreleme anahtar malzemesi anahtar kasanıza şifrelenmiş VHD yükleyin. Ardından, yeni bir Iaas VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırmasını sağlar.
   * Marketten oluşturulan yeni VM'ler ve Azure'da çalışmakta olan var olan VM'ler için Iaas VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırması sağlar.

3. Şifreleme anahtarı malzeme (BitLocker şifreleme anahtarlarını Windows sistemleri için) ve parolayı Linux Iaas VM üzerinde şifrelemeyi etkinleştirmek için anahtar kasanıza okuma için Azure platformunda erişim izni verin.

4. Anahtar kasanızı malzeme şifreleme anahtarını yazmak için Azure Active Directory (Azure AD) uygulama kimliği sağlayın. Bunun yapılması, 2. adımda bahsedilen senaryolar için Iaas VM üzerinde şifrelemeyi etkinleştirir.

5. Azure VM hizmet modeli şifrelemesi ile key vault yapılandırması güncelleştirir ve, şifrelenmiş sanal Makineyi ayarlar.

 ![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a>Şifre çözme iş akışı
Iaas Vm'leri için disk şifreleme devre dışı bırakmak için aşağıdaki üst düzey adımları tamamlayın:

1. Şifreleme (şifre çözme) devre dışı bırakmak azure'da çalıştırılan Iaas VM'de PowerShell cmdlet'leri ve Azure Disk şifrelemesi Resource Manager şablonu seçin ve şifre çözme yapılandırmayı belirtin.

 Bu adım işletim sistemi veya veri hacmi ya da hem de çalışan Windows Iaas VM şifreleme devre dışı bırakır. Ancak, önceki bölümde belirtildiği gibi Linux işletim sistemi disk şifreleme devre dışı bırakma desteklenmiyor. İşletim sistemi diski şifreli sürece şifre çözme adım yalnızca Linux vm'lerinde veri sürücülerine izin verilir.
2. Azure VM hizmet modelini güncelleştirir ve Iaas VM şifresi çözülmüş olarak işaretlenir. VM içeriğini artık bekleme durumundayken şifrelenir.

> [!NOTE]
> Şifreleme devre dışı bırakma işlemi, anahtar kasanıza ve şifreleme anahtar malzemesi (BitLocker şifreleme anahtarlarını Windows sistemleri için) veya Linux için parola silmez.
 > Linux için işletim sistemi disk şifreleme devre dışı bırakma desteklenmiyor. Şifre çözme adım, yalnızca Linux vm'lerinde veri sürücüleri için izin verilir.
İşletim sistemi sürücüsünü şifrelendiyse Linux için disk şifrelemeyi devre dışı bırakma desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar
"Genel bakış" bölümünde ele alınan desteklenen senaryolar için Azure Iaas sanal makinelerinde Azure Disk Şifrelemesi'ı etkinleştirmeden önce aşağıdaki önkoşullara bakın:

* Desteklenen bölgelerdeki Azure kaynakları oluşturmak için geçerli bir etkin Azure aboneliğinizin olması gerekir.
* Azure Disk şifrelemesi, aşağıdaki Windows Server sürümlerinde desteklenir: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016.
* Azure Disk şifrelemesi, aşağıdaki Windows istemci sürümlerinde desteklenir: Windows 8 istemcisi ve Windows 10 istemcisi.

> [!NOTE]
> Windows Server 2008 R2 için .NET Framework 4.5, azure'daki şifreleme etkinleştirmeden önce yüklü olması gerekir. Windows Update'ten isteğe bağlı bir güncelleştirme Windows Server 2008 R2 x64 tabanlı sistemler için Microsoft .NET Framework 4.5.2 yükleyerek yükleyebilirsiniz ([KB2901983](https://support.microsoft.com/kb/2901983)).

* Azure Disk şifrelemesi yalnızca Windows'da desteklenmektedir belirli Azure Galerisi'nde Linux sunucusu dağıtımları ve sürümleri dayalıdır.  Şu anda desteklenen sürümlerin listesi için bkz [Azure Disk şifrelemesi hakkında SSS](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-faq).

* Azure Disk şifrelemesi, anahtar kasası ve VM'lerin aynı Azure bölgesindeki ve abonelikte bulunmasını gerektirir.

> [!NOTE]
> Kaynaklarını ayrı bölge içinde yapılandırma Azure Disk şifreleme özelliği etkinleştirilirken bir hata neden olur.

* Ayarlama ve anahtar kasanız için Azure Disk şifrelemesini yapılandırmak için bkz **kümesi ayarlama ve anahtar kasanızın Azure Disk şifrelemesi için yapılandırma** içinde *önkoşulları* bu makalenin.
* Ve Azure AD uygulaması, Azure Disk şifrelemesi için Azure Active Directory'de yapılandırma bölümüne bakın. **Azure Active Directory'de Azure AD uygulaması ayarlama** içinde *önkoşulları* bölümü Bu makalede.
* Ve Azure AD uygulaması için anahtar kasası erişim ilkesini yapılandırma bölümüne bakın. **Azure AD uygulaması için anahtar kasası erişim ilkesini ayarlama** içinde *önkoşulları* bu makalenin.
* Önceden şifrelenmiş bir Windows VHD hazırlamak için konudaki **önceden şifrelenmiş bir Windows VHD hazırlama** içinde *ek*.
* Önceden şifrelenmiş bir Linux VHD hazırlamak için konudaki **önceden şifrelenmiş bir Linux VHD hazırlama** içinde *ek*.
* Azure platformu şifreleme anahtarları veya gizli anahtar kasanızı önyüklenir ve sanal makine işletim sistemi birimi şifresini çözer, sanal makine için kullanılabilir hale getirmek için erişim verilmesi gerekir. Azure platformu izinler vermek için ayarlanmış **EnabledForDiskEncryption** key vault'ta özelliği. Daha fazla bilgi için bkz. **kümesi ayarlama ve anahtar kasanızın Azure Disk şifrelemesi için yapılandırma** ekte.
* Anahtar kasası gizli dizi ve KEK URL'leri tutulan olmalıdır. Azure, sürüm oluşturma bu kısıtlamayı zorlar. Geçerli bir gizli dizi ve KEK URL'ler için aşağıdaki örneklere bakın:

  * Geçerli bir gizli URL örneği:   *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Geçerli bir KEK URL örneği:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure Disk şifrelemesi, anahtar kasası gizli dizileri ve KEK URL'leri bir parçası olarak belirten bir bağlantı noktası numaralarını desteklemez. Desteklenen ve desteklenmeyen key vault URL'leri örnekleri için aşağıdakilere bakın:

  * Kabul edilebilir bir key vault URL'si  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Kabul edilebilir bir key vault URL'si:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Özelliği, Iaas Vm'leri Azure Disk Şifrelemesi'ni etkinleştirmek için aşağıdaki ağ uç noktası yapılandırması gereksinimleri karşılaması gerekir:
  * Anahtar kasanızı bağlanmak için bir belirteç almak üzere Iaas VM bir Azure Active Directory uç noktasına bağlanabilir olmalıdır \[login.microsoftonline.com\].
  * Şifreleme anahtarları için anahtar kasanıza yazmak için Iaas VM anahtar kasası uç noktasına bağlanabilir olmalıdır.
  * Iaas VM Azure uzantısı depoyu ve VHD dosyalarını barındıran Azure depolama hesabı'nı barındıran bir Azure depolama uç noktasına bağlanabiliyor olmanız gerekir.

  > [!NOTE]
  > Güvenlik ilkeniz Azure vm'lerinden Internet erişimi sınırlayan, önceki URI çözmek ve IP'ler giden bağlantıya izin verecek bir kuralı yapılandırın.
  >
  >Yapılandırma ve Azure anahtar kasası bir Güvenlik Duvarı'nı erişmek için (https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall)

* Azure Disk şifrelemesini yapılandırmak için Azure PowerShell SDK'sı sürüm en son sürümünü kullanın. En son sürümünü indirin [Azure PowerShell sürümü](https://github.com/Azure/azure-powershell/releases)

 > [!NOTE]
  > Azure Disk şifrelemesi desteklenmez [Azure PowerShell SDK'sı sürüm 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016). Aldığınız hata ilgili 1.1.0 Azure PowerShell kullanarak, bakın [Azure Disk şifreleme hatası ilgili Azure PowerShell'e 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).

* Tüm Azure CLI komutunu çalıştırın ve Azure aboneliğinizle ilişkilendirmek için Azure CLI'yı yüklemeniz gerekir:
  * Azure CLI'yı yükleme ve Azure aboneliğinizle ilişkilendirmek için bkz: [Azure CLI'yi yükleme ve yapılandırma işlemini](../cli-install-nodejs.md).
  * Mac, Linux ve Windows Azure Resource Manager ile Azure CLI kullanmak için bkz: [Resource Manager modunda Azure CLI komutları](../virtual-machines/azure-cli-arm-commands.md).

* Yönetilen disk şifreleme, şifreleme etkinleştirilmeden önce yönetilen diskin anlık görüntüsünü ya da Azure Disk şifrelemesi dışında disk yedeğini alırken zorunlu bir önkoşuldur.  Yerinde bir yedek olmadan şifreleme sırasında beklenmeyen bir hata disk ve VM kurtarma seçeneği olmaksızın erişilemez duruma getirebilir.  Set-AzureRmVMDiskEncryptionExtension şu anda yönetilen diskleri yedekleme ve hata - skipVmBackup parametresi belirtilmediyse, yönetilen disk karşı kullandıysanız olur.  Bu parametre bir yedekleme dışında Azure Disk şifrelemesi kılmadığınız sürece kullanmak için güvenli değildir.   -SkipVmBackup parametresi belirtildiğinde, cmdlet şifreleme önce yönetilen disk yedeğini yapmaz.  Bu nedenle, bu durumda kurtarma daha sonra gerekli olan Azure Disk Şifrelemesi'ni etkinleştirdikten önce yerinde vm'dir yönetilen disk yedeğini emin olmak için zorunlu önkoşul olarak kabul edilir.  
> [!NOTE]
 > Azure Disk şifrelemesi dışında bir anlık görüntü veya yedekleme önceden yapılmış sürece - skipVmBackup parametresi hiçbir zaman kullanılmamalıdır. 

* Azure Disk şifrelemesi çözümü, BitLocker dış anahtar koruyucusu Windows Iaas Vm'leri için kullanır. Etki alanına katılmış sanal makineleri, yok TPM koruyucusu zorlamak için tüm grup ilkeleri gönderin. "Uyumlu TPM'siz BitLocker izin ver" için Grup İlkesi hakkında bilgi için bkz: [BitLocker Grup İlkesi başvurusu](https://technet.microsoft.com/library/ee706521).
* Özel Grup İlkesi ile etki alanına katılmış sanal makinelerde BitLocker'ı İlkesi şu ayar içermelidir: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key` Bitlocker için özel Grup İlkesi ayarları uyumsuz olduğunda Azure Disk şifrelemesi başarısız olur. Doğru ilkeyi yoktu makinelerde ayarı, yeni ilke uygulama, (gpupdate.exe/Force) güncelleştirmek için yeni ilke zorlama ve daha sonra yeniden başlatma gerekebilir.  
* Azure AD uygulaması oluşturmak için bir anahtar kasası oluşturma veya mevcut bir anahtar kasası ayarlama ve şifrelemeyi etkinleştirmek için bkz [Azure Disk şifrelemesi önkoşul PowerShell Betiği](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).
* Azure CLI kullanarak disk şifrelemesi önkoşulları yapılandırma için bkz: [bu Bash betiğini](https://github.com/ejarvi/ade-cli-getting-started).
* Yedekleme ve şifreleme ile Azure Disk şifrelemesi etkin olduğunda şifrelenmiş Vm'leri geri yükleme için Azure Backup hizmeti kullanmak için Azure Disk şifrelemesi anahtar yapılandırmasını kullanarak sanal makinelerinizi şifreleyin. Hayır-KEK veya KEK yapılandırmaları kullanılarak şifrelenmiş VM'ler için Yedekleme hizmetini destekler. Bkz: [şifrelenmiş sanal makineleri Azure Backup şifrelemesi ile yedeklemek ve geri yükleme](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).

* Bir Linux işletim sistemi birimi şifrelerken VM yeniden başlatma işleminin sonunda şu anda gerekli olduğunu unutmayın. Bu, portal, powershell veya CLI yapılabilir.   Şifreleme ilerlemesini izlemek için düzenli aralıklarla Get-AzureRmVMDiskEncryptionStatus tarafından döndürülen durum iletisi yoklama https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus.  Şifreleme işlemi tamamlandıktan sonra bu komutu tarafından döndürülen durum iletisi bu gösterir. Örneğin, "ProgressMessage: Lütfen VM'yi yeniden başlatın, işletim sistemi diski ile başarıyla şifrelendi" Bu noktada VM yeniden ve kullanılabilecek.  

* Linux için Azure Disk şifrelemesi bağlı dosya sistemi Linux önce şifreleme sağlamak için veri diskleri gerektirir

* Veri diskleri için Linux tarafından Azure Disk şifrelemesi desteklenmez yinelemeli olarak bağlanır. Örneğin, hedef sistem/foo/çubuğunda bir disk bağlı ve ardından başka /foo/bar/baz, /foo/bar/baz şifrelenmesi başarılı olur, ancak şifreleme/foo/çubuğunun başarısız olur. 

* Azure Disk şifrelemesi yalnızca yukarıda sözü edilen önkoşulları karşılayan desteklenen Azure galeri görüntüleri desteklenir. Müşteri özel görüntüleri, özel bölümleme şeması ve bu görüntülerde bulunabilir işlem davranışları nedeniyle desteklenmez. Ayrıca, hatta galeri görüntüsü başlangıçta önkoşulların ancak oluşturma uyumsuz sonra değiştirilmiş VM'nin temel.  Bunun için bir Linux VM şifreleme için önerilen yordamı bir temiz galeri görüntüsünden Başlat, VM şifreleme ve ardından özel bir yazılım veya veri sanal Makineye gerektiği gibi ekleyin nedenidir.  

* Azure Disk şifrelemesi ve yerel veri hacmi - Bek birim şifreleme anahtarı güvenli bir şekilde tutmak Linux Iaas Vm'leri için Windows ve/mnt/azure_bek_disk için. Silmeyin ve bu diskteki tüm içeriği düzenleyebilir. Iaas VM üzerindeki tüm şifreleme işlemleri için şifreleme anahtar bulunması gerekli olmadığından, diski çıkarın değil. Ek ayrıntılar biriminde bulunan Benioku dosyası içerir.

#### <a name="set-up-the-azure-ad-application-in-azure-active-directory"></a>Azure Active Directory'de Azure AD uygulaması ayarlayın
Azure'da çalışan bir VM'de etkinleştirilmesi için şifreleme, ihtiyacınız olduğunda, Azure Disk şifrelemesi oluşturur ve şifreleme anahtarları için anahtar kasanıza yazar. Anahtar kasanızı şifreleme anahtarları yönetme, Azure AD kimlik doğrulaması gerektirir.

Bu amaç için bir Azure AD uygulaması oluşturun. Bir uygulama blog gönderisinin "Alma bir kimlik için uygulama" bölümünde kaydetmek için ayrıntılı adımları bulabilirsiniz [Azure Key Vault - adım adım](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Bu gönderinin ayrıca ayarlamak ve anahtar kasanıza yapılandırmak için yararlı örnekler içerir. Kimlik doğrulama amacıyla istemci gizli anahtarı tabanlı kimlik doğrulaması veya istemci sertifikası tabanlı Azure AD kimlik doğrulaması kullanabilirsiniz.

#### <a name="client-secret-based-authentication-for-azure-ad"></a>Azure AD için istemci gizli anahtarı tabanlı kimlik doğrulaması
Aşağıdaki bölümlerde, bir istemci gizli anahtarı tabanlı kimlik doğrulaması için Azure AD yapılandırmanıza yardımcı olabilir.

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a>Azure PowerShell kullanarak bir Azure AD uygulaması oluşturma
Azure AD uygulaması oluşturmak için aşağıdaki PowerShell cmdlet'ini kullanın:

    $aadClientSecret = "yourSecret"
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> Azure AD ClientID $azureAdApplication.ApplicationId bağlıdır ve $aadClientSecret istemci parolası, Azure Disk Şifrelemesi'ni etkinleştirmek için daha sonra kullanmanız gerekir. Azure AD gizli uygun şekilde koruyun.

##### <a name="setting-up-the-azure-ad-client-id-and-secret-from-the-azure-portal"></a>Azure AD İstemci Kimliğini ve parolasını Azure portalından ayarlama
Ayrıca, Azure AD İstemci Kimliğini ve parolasını Azure portalını kullanarak ayarlayabilirsiniz. Bu görevi gerçekleştirmek için aşağıdakileri yapın:

1. Seçin **tüm hizmetler > Azure Active Directory**

 ![Azure Disk Şifrelemesi](./media/azure-security-disk-encryption/aad-service.png)

2. Seçin **uygulama kayıtları > Yeni uygulama kaydı**

 ![Azure Disk Şifrelemesi](./media/azure-security-disk-encryption/aad-app-registration.png)

3. İstenen bilgileri sağlayın ve uygulama oluşturun:

 ![Azure Disk Şifrelemesi](./media/azure-security-disk-encryption/aad-create-app.png)

4. Uygulama kimliği de dahil olmak üzere, özelliklerini görüntülemek için yeni oluşturduğunuz uygulamayı seçin  Uygulama için bir anahtar oluşturmak için Seç **Ayarları > anahtarlar**, açıklama ve süre sonu için bir anahtar ekleyin ve tıklayın **Kaydet**

 ![Azure Disk Şifrelemesi](./media/azure-security-disk-encryption/aad-create-pw.png)

5. Oluşturulan gizli değeri kopyalayın ve uygun şekilde korunması.

 ![Azure Disk Şifrelemesi](./media/azure-security-disk-encryption/aad-save-pw.png)


##### <a name="use-an-existing-application"></a>Var olan bir uygulama kullanma
Aşağıdaki komutları yürütmek için elde edilir ve [Azure AD PowerShell modülünün](https://technet.microsoft.com/library/jj151815.aspx).

> [!NOTE]
> Yeni bir PowerShell penceresinden aşağıdaki komutları yürütülmelidir. Azure PowerShell veya Azure Resource Manager penceresini komutları çalıştırmak için kullanmayın. Bu cmdlet'ler MSOnline modülü ya da Azure AD PowerShell olduğundan bu yaklaşım öneririz.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a>Azure AD için sertifika tabanlı kimlik doğrulaması
> [!NOTE]
> Linux Vm'lerinde Azure AD sertifika tabanlı kimlik doğrulaması şu anda desteklenmiyor.

Aşağıdaki bölümlerde, sertifika tabanlı kimlik doğrulamasını Azure AD için yapılandırma gösterilmektedir.

##### <a name="create-an-azure-ad-application"></a>Azure AD uygulaması oluşturun
Azure AD uygulaması oluşturmak için aşağıdaki PowerShell cmdlet'lerini çalıştırın:

> [!NOTE]
> Aşağıdaki değiştirin `yourpassword` güvenli parolanızı, dize ve parola koruyun.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

Bu adımı tamamladıktan sonra anahtar kasanızın PFX dosyasını karşıya yükleyin ve o sertifikayı bir VM dağıtmak için gerekli erişim ilkesini etkinleştir.

##### <a name="use-an-existing-azure-ad-application"></a>Mevcut bir Azure AD uygulamasını kullanın
Mevcut bir uygulama için sertifika tabanlı kimlik doğrulaması yapılandırıyorsanız, burada gösterilen PowerShell cmdlet'lerini kullanın. Yeni bir PowerShell penceresinden yürütülecek emin olun.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

Bu adımı tamamladıktan sonra anahtar kasanızın PFX dosyasını karşıya yükleme ve sertifikayı bir VM dağıtmak için gerekli olan erişim ilkesini etkinleştir.

##### <a name="upload-a-pfx-file-to-your-key-vault"></a>Anahtar kasanızı PFX dosyasını karşıya yükle
Bu işlem ayrıntılı bir açıklaması için bkz. [resmi Azure anahtar kasası ekibi blogunu](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx). Ancak, aşağıdaki PowerShell cmdlet'lerini, tüm görev için ihtiyacınız olan. Azure PowerShell konsolundan yürütülecek emin olun.

> [!NOTE]
> Aşağıdaki değiştirin `yourpassword` güvenli parolanızı, dize ve parola koruyun.

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

##### <a name="deploy-a-certificate-in-your-key-vault-to-an-existing-vm"></a>Mevcut bir VM'ye sertifika anahtar kasanızdaki dağıtma
PFX karşıya yükleme işlemini tamamladıktan sonra bir sertifikayı key vault'ta aşağıdaki var olan bir sanal makineye dağıtın:
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

#### <a name="set-up-the-key-vault-access-policy-for-the-azure-ad-application"></a>Azure AD uygulaması için anahtar kasası erişim ilkesini ayarlama
Azure AD uygulamanızın ihtiyaç duyduğu kasadaki gizli dizileri ve anahtarları erişim hakları. Kullanım [ `Set-AzureKeyVaultAccessPolicy` ](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) (uygulama kaydedildiği zaman oluşturuldu) istemci kimliği olarak kullanarak, uygulamaya izinleri vermek için cmdlet _– ServicePrincipalName_ parametre değeri. Daha fazla bilgi için blog gönderisine bakın [Azure Key Vault - adım adım](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). PowerShell aracılığıyla bu görevi gerçekleştirmeye ilişkin bir örnek aşağıda verilmiştir:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> Azure Disk şifrelemesi aşağıdaki erişim ilkeleri, Azure AD İstemci uygulamanıza yapılandırmanızı gerektirir: _WrapKey_ ve _ayarlamak_ izinleri.

## <a name="terminology"></a>Terminoloji
Bu teknoloji tarafından kullanılan ortak terimlerden bazılarının anlamak için aşağıdaki terimleri tabloyu kullanın:

| Terminoloji | Tanım |
| --- | --- |
| Azure AD | Azure AD, [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Bir Azure AD hesabı kimlik doğrulaması, depolama ve gizli dizileri anahtar kasasından almak için bir önkoşuldur. |
| Azure Key Vault | Key Vault, şifreleme anahtarlarını ve gizli gizli bilgilerinizi korumak Federal Bilgi işleme standartları FIPS doğrulanmış donanım güvenliği modüllerini temel, alan bir şifreleme ve anahtar yönetim hizmetidir. Daha fazla bilgi için [Key Vault](https://azure.microsoft.com/services/key-vault/) belgeleri. |
| ARM | Azure Resource Manager |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) Windows Iaas Vm'leri disk şifrelemesini etkinleştirmek için kullanılan bir sektörde tanınmış Windows birim şifreleme teknolojisidir. |
| BEK | BitLocker şifreleme anahtarları, veri hacimleri ve işletim sistemi önyükleme birimi şifrelemek için kullanılır. BitLocker anahtarları bir anahtar kasasında gizli dizi olarak korunur. |
| CLI | Bkz: [Azure komut satırı arabirimi](../cli-install-nodejs.md). |
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux Iaas sanal makineleri disk şifrelemesini etkinleştirmek için kullanılan Linux tabanlı, saydam disk şifreleme alt sistemi. |
| KEK | Dosya korumak veya gizli dizi sarmalamak için kullanabileceğiniz asimetrik anahtardır (RSA 2048) anahtar şifreleme anahtarıdır. Bir donanım güvenlik modülleri (HSM) sağlayabilir-korumalı bir anahtar veya yazılım korumalı anahtarı. Daha fazla ayrıntı için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) belgeleri. |
| PS cmdlet'leri | Bkz: [Azure PowerShell cmdlet'lerini](/powershell/azure/overview). |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a>Ayarlama ve anahtar kasanızın Azure Disk şifrelemesi için yapılandırma
Azure Disk şifrelemesi koruyun disk şifreleme anahtarlarını ve gizli anahtar kasanızdaki yardımcı olur. Anahtar kasanız için Azure Disk şifrelemesi ayarlamak için aşağıdaki bölümlerde adımları tamamlayın.

#### <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma
Bir anahtar kasası oluşturmak için aşağıdaki seçeneklerden birini kullanın:

* ["101-Key-Vault-oluşturma" Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
* [Azure PowerShell anahtar kasası cmdlet'leri](/powershell/module/azurerm.keyvault/#key_vault)
* Azure Resource Manager
* Nasıl yapılır [anahtar kasanızın güvenliğini sağlama](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault)

> [!NOTE]
> Aboneliğiniz için bir anahtar kasası zaten ayarladıysanız, sonraki bölüme atlayın.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a>Bir anahtar şifreleme anahtarı (isteğe bağlı) ayarlama
Ek bir BitLocker şifreleme anahtarları için güvenlik katmanı için bir KEK kullanmak istiyorsanız, bir KEK anahtar kasanızı ekleyin. Kullanım [ `Add-AzureKeyVaultKey` ](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) anahtar şifreleme anahtarı anahtar kasasını oluşturmak için cmdlet'i. Ayrıca, şirket içi Anahtar Yönetimi'nden HSM bir KEK içeri aktarabilirsiniz. Daha fazla ayrıntı için [Key Vault belgeleri](https://azure.microsoft.com/documentation/services/key-vault/).

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

Azure Resource Manager'a giderek veya, anahtar kasası arabirimini kullanarak KEK ekleyebilirsiniz.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a>Anahtar kasası izinlerini ayarla
Azure platform şifreleme anahtarları veya gizli anahtar kasanızı önyükleme ve birimler şifresini çözmek için VM ayıklanarak erişmesi gerekir. Azure platformu izinler vermek için ayarlanmış **EnabledForDiskEncryption** özelliğinde anahtar kasası anahtar kasası PowerShell cmdlet'ini kullanarak:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

Ayrıca **EnabledForDiskEncryption** ederek özelliği [Azure kaynak Gezgini](https://resources.azure.com).

Daha önce bahsedildiği gibi ayarlamalısınız **EnabledForDiskEncryption** anahtar kasanıza özelliği. Aksi takdirde, dağıtım başarısız olur.

Burada gösterildiği gibi anahtar kasası arabiriminden Azure AD uygulamanız için erişim ilkeleri ayarlayabilirsiniz:

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

Üzerinde **Gelişmiş erişim ilkeleri** sekmesinde, anahtar kasanız için Azure Disk şifrelemesi etkin olduğundan emin olun:

![Azure anahtar kasası](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a>Disk şifrelemesi dağıtım senaryolarını ve kullanıcı deneyimleri
Çok sayıda disk şifreleme senaryoları etkinleştirebilirsiniz ve adımları senaryoya göre değişiklik gösterebilir. Aşağıdaki bölümlerde daha ayrıntılı senaryoları kapsar.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-the-marketplace"></a>Marketten oluşturulan yeni Iaas VM'ler şifrelemeyi etkinleştirin
Azure Market'teki yeni Iaas Windows VM'de disk şifrelemesi kullanarak etkinleştirebilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).

1. Azure Hızlı Başlangıç şablonu **azure'a Dağıt**, şifreleme yapılandırma girin **parametreleri** dikey penceresinde ve ardından **Tamam**.

2. Abonelik, kaynak grubu, kaynak grubu konumu, yasal koşulları ve Sözleşmesi'ni seçin ve ardından **Oluştur** yeni bir Iaas VM üzerinde şifrelemeyi etkinleştirmek için.

> [!NOTE]
> Bu şablon, Windows Server 2012 galeri görüntüsü kullanan bir yeni şifrelenmiş Windows VM oluşturur.

Bunu kullanarak yeni Iaas Red Hat Linux 7.2 VM 200 GB RAID-0 dizisi olan disk şifrelemeyi etkinleştirebilirsiniz [Resource Manager şablonu](https://aka.ms/fde-rhel). Şablonu dağıttıktan sonra kullanarak VM şifreleme durumu doğrulayın `Get-AzureRmVmDiskEncryptionStatus` açıklandığı cmdlet'i [şifreleme işletim sistemi sürücü üzerinde çalışan bir Linux VM](#encrypting-os-drive-on-a-running-linux-vm). Makine durumu döndürdüğünde _VMRestartPending_, VM'yi yeniden başlatın.

Aşağıdaki tabloda, Azure AD İstemci Kimliğini kullanarak Market senaryosundaki yeni VM'ler için Resource Manager şablon parametreleri listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| adminUserName | Sanal makine için yönetici kullanıcı adı. |
| adminPassword | Sanal makine için yönetici kullanıcı parolası. |
| newStorageAccountName | İşletim sistemi ve veri VHD'leri depolamak için depolama hesabının adıdır. |
| vmSize | VM boyutu. Şu anda yalnızca standart A, D ve G serisi desteklenmektedir. |
| virtualNetworkName | VM NIC ait olması gereken Vnet'in adı. |
| subnetName | VM NIC ait olmalıdır bir sanal ağ içindeki alt ağ adı. |
| Aadclientıd | Gizli anahtar kasanız için yazma iznine sahip Azure AD uygulamasının istemci kimliği. |
| AADClientSecret | Gizli anahtar kasanız için yazma iznine sahip Azure AD uygulamasının istemci gizli anahtarı. |
| keyVaultURL | BitLocker anahtarı karşıya yüklenmelidir anahtar kasasının URL'si. Cmdlet'ini kullanarak elde edeceğinizi `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`. |
| keyEncryptionKeyURL | (İsteğe bağlı) oluşturulan BitLocker anahtarı şifrelemek için kullanılan anahtar şifreleme anahtarı URL'si. |
| keyVaultResourceGroup | Anahtar kasasının kaynak grubu. |
| vmName | Şifreleme işlemi gerçekleştirilecek olan bir VM adı. |

> [!NOTE]
> _KeyEncryptionKeyURL_ isteğe bağlı bir parametredir. Anahtar kasanızda veri şifreleme anahtarının (parola) daha fazla koruma için kendi KEK getirebilirsiniz.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a>VHD müşteri şifrelenir ve şifreleme anahtarlarını oluşturulan yeni Iaas VM'ler şifrelemeyi etkinleştirin
Bu senaryoda, Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleme etkinleştirebilirsiniz. Aşağıdaki bölümlerde Resource Manager şablonu ve CLI komutları daha ayrıntılı olarak açıklanmaktadır.

Azure'da kullanılabilen önceden şifrelenmiş görüntüleri hazırlama için bu bölümlerin birinden yönergeleri izleyin. Görüntü oluşturulduktan sonra şifrelenmiş bir Azure VM oluşturmak için sonraki bölümde adımları kullanabilirsiniz.

* [Önceden şifrelenmiş bir Windows VHD hazırlama](#preparing-a-pre-encrypted-windows-vhd)
* [Önceden şifrelenmiş bir Linux VHD hazırlama](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-the-resource-manager-template"></a>Resource Manager şablonu kullanma
Disk şifrelemesi kullanarak şifrelenmiş VHD'nizi etkinleştirebilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).

1. Azure Hızlı Başlangıç şablonu **azure'a Dağıt**, şifreleme yapılandırma girin **parametreleri** dikey penceresinde ve ardından **Tamam**.

2. Abonelik, kaynak grubu, kaynak grubu konumu, yasal koşulları ve Sözleşmesi'ni seçin ve ardından **Oluştur** yeni Iaas VM üzerinde şifrelemeyi etkinleştirmek için.

Aşağıdaki tabloda, şifrelenmiş bir VHD için Resource Manager şablon parametreleri listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| newStorageAccountName | Şifrelenmiş işletim sistemi VHD'SİNİN depolanacağı depolama hesabı adı. Bu depolama hesabı zaten aynı kaynak grubunda ve aynı konumdaki VM oluşturulmuş olmalıdır. |
| osVhdUri | Depolama hesabı işletim sistemi VHD URI'si. |
| osType | İşletim sistemi ürün türü (Windows/Linux). |
| virtualNetworkName | VM NIC ait olması gereken Vnet'in adı. Ad zaten aynı kaynak grubunda ve aynı konumdaki VM oluşturulmuş olmalıdır. |
| subnetName | VM NIC ait olması gereken bir VNet üzerinde alt ağın adı. |
| vmSize | VM boyutu. Şu anda yalnızca standart A, D ve G serisi desteklenmektedir. |
| keyVaultResourceID | Azure Resource Manager'daki anahtar kasası kaynağı tanımlayan ResourceId. PowerShell cmdlet'ini kullanarak elde edeceğinizi `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`. |
| keyVaultSecretUrl | Anahtar Kasası'nda ayarladığınız disk şifreleme anahtarı URL'si. |
| keyVaultKekUrl | Oluşturulan disk şifreleme anahtarını şifrelemek için anahtar şifreleme anahtarı URL'si. |
| vmName | Iaas VM adı. |

#### <a name="using-powershell-cmdlets"></a>PowerShell cmdlet'lerini kullanarak
Disk şifreli VHD'nizi PowerShell cmdlet'ini kullanarak şifreleyebilirsiniz [ `Set-AzureRmVMOSDisk` ](/powershell/module/azurerm.compute/set-azurermvmosdisk).  

#### <a name="using-cli-commands"></a>CLI komutlarını kullanma
CLI komutlarını kullanarak bu senaryo için disk şifrelemeyi etkinleştirmek için aşağıdakileri yapın:

1. Anahtar kasanıza erişim ilkeleri ayarlayın:

   * Ayarlama **EnabledForDiskEncryption** bayrağı:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Azure AD uygulaması gizli anahtar kasanız için yazma izinleri ayarla:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. Var olan veya çalışan bir VM üzerinde şifrelemeyi etkinleştirmek için şunu yazın:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Şifreleme durumunu alın:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. Şifrelenmiş VHD'nizi yeni VM'den şifrelemesini etkinleştirmek için aşağıdaki parametreleri kullanın `azure vm create` komutu:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a>Var olan veya Azure'da Iaas Windows VM çalıştırmaya şifrelemeyi etkinleştir
Bu senaryoda, Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleme etkinleştirebilirsiniz. Aşağıdaki bölümlerde, CLI komutları ve Resource Manager şablonu kullanarak etkinleştirmek nasıl daha ayrıntılı olarak açıklanmaktadır.

#### <a name="using-the-resource-manager-template"></a>Resource Manager şablonu kullanma
Disk şifrelemesi kullanarak Azure'da Iaas Windows Vm'leri çalıştırma ya da varolan etkinleştirebilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).

1. Azure Hızlı Başlangıç şablonu **azure'a Dağıt**, şifreleme yapılandırma girin **parametreleri** dikey penceresinde ve ardından **Tamam**.

2. Abonelik, kaynak grubu, kaynak grubu konumu, yasal koşulları ve Sözleşmesi'ni seçin ve ardından **Oluştur** var olan veya çalışan Iaas VM üzerinde şifrelemeyi etkinleştirmek için.

Var olan veya bir Azure AD İstemci Kimliğini kullanan sanal makineleri çalıştırmak için Resource Manager şablon parametreleri aşağıdaki tabloda listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| Aadclientıd | Bir anahtar kasasına gizli anahtarları yazmak için izne sahip olan Azure AD uygulamasının istemci kimliği. |
| AADClientSecret | Bir anahtar kasasına gizli anahtarları yazmak için izne sahip olan Azure AD uygulamasının istemci gizli anahtarı. |
| keyVaultName | BitLocker anahtarı karşıya yüklenmelidir anahtar kasasının adı. Cmdlet'ini kullanarak elde edeceğinizi `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | Oluşturulan BitLocker anahtarı şifrelemek için kullanılan anahtar şifreleme anahtarı URL'si. Bu seçerseniz isteğe bağlı bir parametredir **nokek** UseExistingKek aşağı açılan listesinde. Seçerseniz **kek** UseExistingKek aşağı açılan listeden girmelisiniz _keyEncryptionKeyURL_ değeri. |
| volumeType | Şifreleme işlemi gerçekleştirilir birim türü. Geçerli değerler _işletim sistemi_, _veri_, ve _tüm_. |
| sequenceVersion | BitLocker işlemi dizisi sürümü. Bu sürüm numarası, aynı sanal makinede her bir disk şifreleme işlemi gerçekleştirildiğinde artırın. |
| vmName | Şifreleme işlemi gerçekleştirilecek olan bir VM adı. |

> [!NOTE]
> _KeyEncryptionKeyURL_ isteğe bağlı bir parametredir. Daha fazla koruma için anahtar Kasası'nda veri şifreleme anahtarı (BitLocker şifreleme gizli bilgisi) kendi KEK getirebilirsiniz.

#### <a name="using-powershell-cmdlets"></a>PowerShell cmdlet'lerini kullanarak
PowerShell cmdlet'lerini kullanarak Azure Disk şifrelemesi ile şifrelemeyi etkinleştirme hakkında daha fazla bilgi için bkz. blog gönderileri [Azure PowerShell - bölüm 1 ile Azure Disk şifrelemesi keşfedin](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) ve [Azure Disk şifrelemesi keşfedin Azure PowerShell ile - bölüm 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

#### <a name="using-cli-commands"></a>CLI komutlarını kullanma
Var olan veya Iaas Windows VM CLI komutlarını kullanarak Azure'da çalışan şifrelemeyi etkinleştirmek için aşağıdakileri yapın:

1. Anahtar kasası erişim ilkeleri ayarlamak için:
   * Ayarlama **EnabledForDiskEncryption** bayrağı:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Azure AD uygulaması gizli anahtar kasanız için yazma izinleri ayarla:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. Var olan veya çalışan bir VM üzerinde şifrelemeyi etkinleştirmek için:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. Şifreleme durumu almak için:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. Şifrelenmiş VHD'nizi yeni VM'den şifrelemesini etkinleştirmek için aşağıdaki parametreleri kullanın `azure vm create` komutu:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a>Azure'da bir var olan veya çalışan Iaas Linux VM üzerinde şifrelemeyi etkinleştir
Var olan veya çalışan Iaas Linux VM'de Azure disk şifrelemesi kullanarak etkinleştirebilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

1. Tıklayın **azure'a Dağıt** şifreleme yapılandırması Azure Hızlı Başlangıç şablonu girin **parametreleri** dikey penceresinde ve ardından **Tamam**.

2. Abonelik, kaynak grubu, kaynak grubu konumu, yasal koşulları ve Sözleşmesi'ni seçin ve ardından **Oluştur** var olan veya çalışan Iaas VM üzerinde şifrelemeyi etkinleştirmek için.

Var olan veya bir Azure AD İstemci Kimliğini kullanan sanal makineleri çalıştırmak için Resource Manager şablon parametreleri aşağıdaki tabloda listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| Aadclientıd | Bir anahtar kasasına gizli anahtarları yazmak için izne sahip olan Azure AD uygulamasının istemci kimliği. |
| AADClientSecret | Gizli anahtar kasanız için yazma iznine sahip Azure AD uygulamasının istemci gizli anahtarı. |
| keyVaultName | BitLocker anahtarı karşıya yüklenmelidir anahtar kasasının adı. Cmdlet'ini kullanarak elde edeceğinizi `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | Oluşturulan BitLocker anahtarı şifrelemek için kullanılan anahtar şifreleme anahtarı URL'si. Bu seçerseniz isteğe bağlı bir parametredir **nokek** UseExistingKek aşağı açılan listesinde. Seçerseniz **kek** UseExistingKek aşağı açılan listeden girmelisiniz _keyEncryptionKeyURL_ değeri. |
| volumeType | Şifreleme işlemi gerçekleştirilir birim türü. Geçerli desteklenen değerler şunlardır: _işletim sistemi_ veya _tüm_ (bkz: desteklenen Linux dağıtımları ve bunların sürümlerini prerequisiteis bölümünde daha önce işletim sistemi ve veri diskleri için). |
| sequenceVersion | BitLocker işlemi dizisi sürümü. Bu sürüm numarası, aynı sanal makinede her bir disk şifreleme işlemi gerçekleştirildiğinde artırın. |
| vmName | Şifreleme işlemi gerçekleştirilecek olan bir VM adı. |
| Parola | Veri şifreleme anahtarı güçlü bir parola yazın. |

> [!NOTE]
> _KeyEncryptionKeyURL_ isteğe bağlı bir parametredir. Anahtar kasanızda veri şifreleme anahtarının (parola) daha fazla koruma için kendi KEK getirebilirsiniz.

#### <a name="cli-commands"></a>CLI komutları
Disk şifrelemesi yüklenerek ve kullanılarak şifrelenmiş VHD'nizi etkinleştirebilirsiniz [CLI komutunu](../cli-install-nodejs.md). Var olan veya CLI komutlarını kullanarak Iaas sanal makineleri Azure'da çalışan şifrelemeyi etkinleştirmek için aşağıdakileri yapın:

1. Anahtar kasası erişim ilkeleri ayarlayın:

 * Ayarlama **EnabledForDiskEncryption** bayrağı:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * Azure AD uygulaması gizli anahtar kasanız için yazma izinleri ayarla:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. Var olan veya çalışan bir VM üzerinde şifrelemeyi etkinleştirmek için:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Şifreleme durumunu alın:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. Şifrelenmiş VHD'nizi yeni VM'den şifrelemesini etkinleştirmek için aşağıdaki parametreleri kullanın `azure vm create` komutu:
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-the-encryption-status-of-an-encrypted-iaas-vm"></a>Şifrelenmiş bir Iaas VM şifreleme durumunu alın
Azure Resource Manager kullanarak şifreleme durumu alabilirsiniz [PowerShell cmdlet'leri](/powershell/azure/overview), veya CLI komutları. Aşağıdaki bölümlerde, Azure portalı ve CLI komutları şifreleme durumu almak için nasıl kullanılacağını açıklanmaktadır.

#### <a name="get-the-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a>Azure Resource Manager kullanılarak şifrelenmiş bir Windows VM şifreleme durumunu alın
Iaas VM şifreleme durumunu, aşağıdakileri yaparak, bir Azure Kaynak Yöneticisi'nden alabilirsiniz:

1. Oturum [Azure portalı](https://portal.azure.com/)ve ardından **sanal makineler** aboneliğinizdeki sanal makinelerinizi bir Özet görünümünü görmek için sol bölmedeki. Sanal makineler görünümü abonelik adını seçerek filtreleyebilirsiniz **abonelik** aşağı açılan listesi.

2. Üst kısmındaki **sanal makineler** sayfasında **sütunları**.

3. Üzerinde **Seç sütun** dikey penceresinde **Disk şifrelemesi**ve ardından **güncelleştirme**. Disk şifrelemesi sütun şifreleme durumunu gösteren görmelisiniz _etkin_ veya _etkin_ aşağıdaki şekilde gösterildiği gibi her VM için:

 ![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-the-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-the-disk-encryption-powershell-cmdlet"></a>Disk şifrelemesi PowerShell cmdlet'ini kullanarak bir şifrelenmiş (Windows/Linux) Iaas VM şifreleme durumunu alın
Disk şifrelemesi PowerShell cmdlet'inden Iaas VM şifreleme durumunu alabilirsiniz `Get-AzureRmVMDiskEncryptionStatus`. VM'niz için şifreleme ayarları almak için şunu girin:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

Çıkışı inceleyebilirsiniz _Get-AzureRmVMDiskEncryptionStatus_ URL'ler için şifreleme anahtarı.

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

OSVolumeEncrypted ve DataVolumesEncrypted ayarları değerlerini ayarlamak _şifreli_, her iki birimin Azure Disk şifrelemesi kullanılarak şifrelenmiş olduğunu gösterir. PowerShell cmdlet'lerini kullanarak Azure Disk şifrelemesi ile şifrelemeyi etkinleştirme hakkında daha fazla bilgi için bkz. blog gönderileri [Azure PowerShell - bölüm 1 ile Azure Disk şifrelemesi keşfedin](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) ve [Azure Disk şifrelemesi keşfedin Azure PowerShell ile - bölüm 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

> [!NOTE]
> Linux Vm'lerinde üç ila dört dakika sürer `Get-AzureRmVMDiskEncryptionStatus` şifreleme durumu bildirmek için cmdlet'i.

#### <a name="get-the-encryption-status-of-the-iaas-vm-from-the-disk-encryption-cli-command"></a>Disk şifrelemesi CLI komutundan Iaas VM şifreleme durumunu alın
Disk şifrelemesi CLI komutunu kullanarak Iaas VM şifreleme durumunu alabilirsiniz `azure vm show-disk-encryption-status`. VM'niz için şifreleme ayarları almak için Azure CLI oturumunuzu girin:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a>Windows Iaas VM çalıştırma şifrelemeyi devre dışı bırakma
Azure Disk şifrelemesi Resource Manager şablonu veya PowerShell cmdlet'leri aracılığıyla bir çalışan Windows veya Linux Iaas VM üzerinde şifrelemeyi devre dışı bırakın ve şifre çözme yapılandırmayı belirtin.

##### <a name="windows-vm"></a>Windows VM
Şifreleme devre dışı bırakma adımı bir işletim sistemi, veri hacmi veya hem de çalışan Windows Iaas VM şifreleme devre dışı bırakır. İşletim sistemi birimi devre dışı bırakın ve şifrelenmiş veri hacmi bırakın. Disable-şifreleme adım gerçekleştirildiğinde, VM hizmet modeli Azure Klasik dağıtım modelini güncelleştirir ve Windows Iaas VM şifresi çözülmüş olarak işaretlenir. VM içeriğini artık bekleme durumundayken şifrelenir. Şifre çözme, anahtar kasanıza ve şifreleme anahtar malzemesi (BitLocker şifreleme anahtarları için Windows ve Linux için parola) silmez.

##### <a name="linux-vm"></a>Linux VM
Şifreleme devre dışı bırakma adımı şifreleme, veri hacminin çalışan Linux Iaas sanal makinesinde devre dışı bırakır. Bu adım, yalnızca işletim sistemi diski şifreli değilse çalışır.

> [!NOTE]
> İşletim sistemi disk şifreleme devre dışı bırakma Linux VM üzerinde izin verilmez.

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Var olan veya çalışan bir Iaas VM üzerinde şifrelemeyi devre dışı bırakma
Disk şifrelemesi kullanarak Windows Iaas Vm'leri çalıştırma devre dışı bırakabilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).

1. Azure Hızlı Başlangıç şablonu **azure'a Dağıt**, şifre çözme yapılandırma girin **parametreleri** dikey penceresinde ve ardından **Tamam**.

2. Abonelik, kaynak grubu, kaynak grubu konumu, yasal koşulları ve Sözleşmesi'ni seçin ve ardından **Oluştur** yeni bir Iaas VM üzerinde şifrelemeyi etkinleştirmek için.

Linux VM'ler için şifreleme kullanarak devre dışı bırakabilirsiniz [çalışan bir Linux VM üzerinde şifrelemeyi devre dışı bırak](https://aka.ms/decrypt-linuxvm) şablonu.

Çalışan bir Iaas VM üzerinde şifrelemeyi devre dışı bırakmak için Resource Manager şablon parametreleri aşağıdaki tabloda listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| vmName | Şifreleme işlemi gerçekleştirilecek olan bir VM adı.
| volumeType | Bir şifre çözme işlemi gerçekleştirilir birim türü. Geçerli değerler _işletim sistemi_, _veri_, ve _tüm_. Windows Iaas VM işletim sistemi/önyükleme birimi şifreleme üzerinde devre dışı bırakmadan çalıştırma şifrelemesini devreden çıkaramazsınız _veri_ birim. Ayrıca, işletim sistemi disk şifreleme devre dışı bırakma Linux Vm'lerinde izin verilmeyen unutmayın. |
| sequenceVersion | BitLocker işlemi dizisi sürümü. Bu sürüm numarası, aynı sanal makinede bir diski şifre çözme işlemi gerçekleştirildiğinde artırın. |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Var olan veya çalışan bir Iaas VM üzerinde şifrelemeyi devre dışı bırakma
PowerShell cmdlet'ini kullanarak var olan veya çalışan bir Iaas VM üzerinde şifrelemeyi devre dışı bırakmak için bkz: [ `Disable-AzureRmVMDiskEncryption` ](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption). Bu cmdlet, hem Windows hem de Linux Vm'leri destekler. Şifreleme devre dışı bırakmak için bir uzantı sanal makineye yükler. Varsa _adı_ parametresi belirtilmezse, varsayılan ada sahip bir uzantı _Windows Vm'leri için AzureDiskEncryption_ oluşturulur.

Linux Vm'lerinde AzureDiskEncryptionForLinux uzantısı kullanılır.

> [!NOTE]
> Bu cmdlet, sanal makine yeniden başlatılır.

### <a name="enable-encryption-on-pre-encrypted-iaas-vm-with-azure-managed-disk"></a>Azure yönetilen Disk ile önceden şifrelenmiş Iaas VM üzerinde şifrelemeyi etkinleştir
Konumunda bulunan bir ARM şablonu kullanarak önceden şifrelenmiş bir VHD'den şifrelenmiş bir VM oluşturmak için Azure yönetilen Disk ARM şablonu kullanın   
[Önceden şifrelenmiş bir VHD/storage blobundan yeni şifrelenmiş yönetilen disk oluşturma] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)

### <a name="enable-encryption-on-a-new-linux-iaas-vm-with-azure-managed-disk"></a>Azure yönetilen Disk ile yeni bir Linux Iaas sanal makinesi üzerinde şifrelemeyi etkinleştir
Yeni şifrelenmiş Iaas konumunda bulunan bir ARM şablonu kullanarak Linux VM oluşturmak için Azure yönetilen Disk ARM şablonu kullanın   
[Tam disk şifrelemesi ile RHEL 7.2 dağıtımı] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)

### <a name="enable-encryption-on-a-new-windows-iaas-vm-with-azure-managed-disk"></a>Azure yönetilen Disk ile yeni bir Windows Iaas VM üzerinde şifrelemeyi etkinleştir
 Yeni şifrelenmiş Iaas konumunda bulunan bir ARM şablonu kullanarak Linux VM oluşturmak için Azure yönetilen Disk ARM şablonu kullanın   
 [Bir yeni şifrelenmiş Windows Iaas yönetilen Disk galeri görüntüsünden VM oluşturma] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)

  > [!NOTE]
  >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel.  Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya Azure Backup kullanılabilir.  Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun.  Bir yedekleme yapıldıktan sonra Set-AzureRmVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir.  Bu komut, yönetilen disk tabanlı VM'nin karşı yapılan bir yedekleme ve bu parametre belirtilen kadar başarısız olacak.    
 
### <a name="update-encryption-settings-of-an-existing-encrypted-non-premium-vm"></a>Mevcut bir şifrelenmiş premium olmayan VM'nin şifreleme ayarlarını güncelleştirme
  AAD istemci kimliği/parola, anahtar şifreleme anahtarı [KEK] BitLocker şifreleme anahtarı Windows VM veya Linux VM parolası gibi VM [PS cmdlet'leri, CLI veya ARM şablonları] şifreleme ayarlarını güncelleştirmeye çalıştırmak için mevcut Azure disk şifreleme destekli arabirimleri kullanın VS. Güncelleştirme şifreleme ayarı hem premium hem de premium olmayan depolama Vm'leri için desteklenir.

## <a name="appendix"></a>Ek
### <a name="connect-to-your-subscription"></a>Aboneliğinize bağlanma
Devam etmeden önce gözden *önkoşulları* bu makaledeki bir bölüm. Tüm önkoşulların karşılandığından emin olduktan sonra aşağıdakileri yaparak aboneliğinize bağlanın:

1. Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:

    `Connect-AzureRmAccount`

2. Birden çok aboneliğiniz varsa ve kullanmak üzere belirtmek istiyorsanız hesabınızın aboneliklerini görmek için aşağıdaki komutu yazın:

    `Get-AzureRmSubscription`

3. Kullanmak istediğiniz aboneliği belirtmek için şunu yazın:

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. Yapılandırılmış aboneliğin doğru olduğunu doğrulamak için şunu yazın:

    `Get-AzureRmSubscription`

5. Azure Disk şifrelemesi cmdlet'lerinin yüklü doğrulamak için şunu yazın:

    `Get-command *diskencryption*`

6. Aşağıdaki çıktı, Azure Disk şifrelemesi PowerShell yükleme onaylar:

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a>Önceden şifrelenmiş bir Windows VHD hazırlama
Aşağıdaki bölümlerde, önceden şifrelenmiş bir Windows VHD Azure Iaas içinde şifrelenmiş bir VHD olarak dağıtımına hazırlamak gereklidir. Hazırlama ve Azure Site Recovery ya da Azure üzerinde bir yeni Windows VM (VHD) önyükleme bilgileri kullanın.

#### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>TPM olmayan işletim sistemi koruma için izin vermek için Grup İlkesi güncelleştirme
BitLocker Grup İlkesi ayarını yapılandırma **BitLocker Sürücü Şifrelemesi**, hangi altında bulabilirsiniz **yerel bilgisayar ilkesi** > **Bilgisayar Yapılandırması**  >  **Yönetim Şablonları** > **Windows bileşenleri**. Bu ayarı değiştirmek **işletim sistemi sürücüleri** > **başlangıçta ek kimlik doğrulaması gerektiren** > **uyumluTPM'sizBitLockerizinver**aşağıdaki şekilde gösterildiği gibi:

![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a>BitLocker özellik bileşenlerini yükleme
Windows Server 2012 ve daha sonra aşağıdaki komutu kullanın:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

Windows Server 2008 R2 için aşağıdaki komutu kullanın:

    ServerManagerCmd -install BitLockers

#### <a name="prepare-the-os-volume-for-bitlocker-by-using-bdehdcfg"></a>İşletim sistemi birimi için BitLocker'ı kullanarak hazırlama `bdehdcfg`
İşletim sistemi bölümü sıkıştırma ve makine BitLocker için hazırlama için aşağıdaki komutu yürütün:

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-the-os-volume-by-using-bitlocker"></a>BitLocker'ı kullanarak işletim sistemi birimi koruma
Kullanım [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) dış bir anahtar koruyucusu kullanılarak önyükleme birimi şifrelemesini etkinleştirmek için komutu. Ayrıca dış sürücü veya birim üzerinde dış anahtarı (.bek dosyası) getirin. Şifreleme sistem/önyükleme biriminde bir sonraki yeniden başlatmada etkinleştirilir.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> BitLocker'ı kullanarak dış anahtarı almak için ayrı bir veri/kaynak VHD ile VM hazırlayın.

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a>Bir işletim sistemi sürücüsünde çalışan bir Linux VM şifreleme

##### <a name="prerequisites-for-os-disk-encryption"></a>İşletim sistemi disk şifrelemesi önkoşulları

* VM işletim sistemi disk şifreleme ile uyumlu bir dağıtım bağlantısında listelendiği gibi kullanılmalıdır [Azure Disk şifrelemesi hakkında SSS](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-faq#what-linux-distributions-does-azure-disk-encryption-support) 
* Azure Resource Manager'daki Market görüntüsünden VM yeniden oluşturulması gerekir.
* En az 4 GB RAM ile Azure VM (boyutudur, 7 GB önerilir).
* (RHEL ve CentOS) SELinux devre dışı bırakın. SELinux devre dışı bırakmak için "4.4.2. bkz. SELinux devre dışı bırakma" [SELinux kullanıcı ve Yönetici Kılavuzu](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) VM üzerinde.
* SELinux devre dışı bıraktıktan sonra VM'yi en az bir kez yeniden başlatın.

##### <a name="steps"></a>Adımlar
1. Daha önce belirtilen dağıtımları birini kullanarak bir VM oluşturun.

 CentOS 7.2 için işletim sistemi disk şifreleme özel bir görüntü desteklenir. Bu görüntü kullanmak için VM'yi oluştururken "7.2n" SKU olarak belirtin:
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. Sanal makine gereksinimlerinize göre yapılandırın. Oluşturacağınız, tüm (işletim sistemi ve veri) şifrelemek için sürücüleri, veri sürücüleri belirtilen ve gelen bağlanabilir /etc/fstab olmanız gerekir.

 > [!NOTE]
 > UUID kullanmak =... veri sürücülerinde blok cihaz adı (örneğin, / dev/sdb1) belirtmek yerine/etc/fstab dosyasında belirtmek için. Şifreleme sırasında sanal makinede sürücüleri sırasını değiştirir. Sanal makinenizin blok cihazları belirli bir sırada dayanıyorsa, şifreleme sonrasında bağlamak başarısız olur.

3. SSH oturumları dışında oturum açın.

4. İşletim sistemi şifrelemek için volumeType olarak belirtin. **tüm** veya **işletim sistemi** olduğunda, [şifrelemeyi etkinleştirme](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

 > [!NOTE]
 > Olarak çalışan tüm kullanıcı alanı işlemleri `systemd` Hizmetleri sonlandırılan ile bir `SIGKILL`. VM'yi yeniden başlatın. VM kapalı kalma süresi üzerinde çalışan bir VM'deki işletim sistemi disk şifreleme etkinleştirdiğinizde planlayın.

5. Düzenli aralıklarla içindeki yönergeleri kullanarak şifreleme ilerleyişini izlemek [sonraki bölümde](#monitoring-os-encryption-progress).

6. Get-AzureRmVmDiskEncryptionStatus "VMRestartPending" gösterdikten sonra oturum açma veya portal, PowerShell veya CLI kullanarak VM'nizi yeniden başlatın.
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot the VM
    ```
Yeniden önce kaydetmeniz önerilir [önyükleme tanılaması](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) VM.

#### <a name="monitoring-os-encryption-progress"></a>İşletim sistemi şifreleme ilerlemesini izleme
İşletim sistemi şifreleme ilerleme üç yolla izleyebilirsiniz:

* Kullanım `Get-AzureRmVmDiskEncryptionStatus` cmdlet'i ve ProgressMessage alanın inceleyin:
    ```
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

 İşletim sistemi şifreleme işlemi devam ederken, sanal Makineye oturum açın, öneririz. Diğer iki yöntemden yalnızca başarısız olan günlükler kopyalayın.

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a>Önceden şifrelenmiş bir Linux VHD hazırlama
##### <a name="ubuntu-16"></a>Ubuntu 16
Aşağıdakileri yaparak dağıtım yüklenmesi sırasında şifreleme yapılandırın:

1. Seçin **yapılandırma şifrelenmiş birimleri** bölümleme zaman diskleri.

 ![Ubuntu 16.04 Kurulumu](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Şifrelenmemiş olması bir ayrı önyükleme sürücüsü oluşturun. Kök drive'ınızdaki şifreleyin.

 ![Ubuntu 16.04 Kurulumu](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Bir parola girin. Anahtar Kasası'na yüklediğiniz parola budur.

 ![Ubuntu 16.04 Kurulumu](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Bölümleme tamamlayın.

 ![Ubuntu 16.04 Kurulumu](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. VM'yi önyüklemek için bir parola istendiğinde, 3. adımda belirttiğiniz parolayı kullanın.

 ![Ubuntu 16.04 Kurulumu](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. VM, Azure kullanarak yüklemek için hazırlama [bu yönergeleri](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-ubuntu/). (VM sağlamayı kaldırma) işleminin son adımında çalıştırmayın henüz.

Şifreleme aşağıdakileri yaparak Azure ile çalışacak şekilde yapılandırın:

1. Aşağıdaki betiği içerikle /usr/local/sbin/azure_crypt_key.sh altında bir dosya oluşturun. Azure tarafından kullanılan parola dosya adı olduğundan KeyFileName dikkat edin.

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

2. Şifreli yapılandırmada değişiklik */etc/crypttab*. Şu şekilde görünmelidir:
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. Düzenlediğinizde *azure_crypt_key.sh* Linux çalıştırmak için Windows ve, kopyalarsınız `dos2unix /usr/local/sbin/azure_crypt_key.sh`.

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

 ![Ubuntu 16.04 Kurulumu](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Sonraki adıma devam edin ve [VHD'nizi karşıya](#upload-encrypted-vhd-to-an-azure-storage-account) azure'a.

##### <a name="opensuse-132"></a>openSUSE 13.2
Dağıtım yüklenmesi sırasında şifreleme yapılandırmak için aşağıdakileri yapın:
1. Diskleri bölümlemek bittiğinde **şifrelemek birim grubu**ve ardından bir parola girin. Bu anahtar kasanızı yükleyeceği paroladır.

 ![openSUSE 13.2 Kurulumu](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Parolanızı kullanarak VM'yi önyükleyin.

 ![openSUSE 13.2 Kurulumu](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. VM içindeki yönergeleri izleyerek Azure'a yüklemek için hazırlama [Azure için SLES veya openSUSE sanal makine hazırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131). (VM sağlamayı kaldırma) işleminin son adımında çalıştırmayın henüz.

Azure ile çalışacak şekilde şifrelemesini yapılandırmak için aşağıdakileri yapın:
1. /Etc/dracut.conf düzenleyin ve aşağıdaki satırı ekleyin:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Bu satırları açıklama satırı yapar dosya /usr/lib/dracut/modules.d/90crypt/module-setup.sh sonunda:
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

3. Dosya /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh başındaki aşağıdaki satırı ekleyin:
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
4. /Usr/lib/dracut/Modules.d/90crypt/cryptroot-ASK.sh düzenleyin ve "# açık LUKS cihaza" ekleyin:

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
5. Çalıştırma `/usr/sbin/dracut -f -v` initrd güncelleştirilecek.

6. VM'nin sağlamasını kaldırmak artık ve [VHD'nizi karşıya](#upload-encrypted-vhd-to-an-azure-storage-account) azure'a.

##### <a name="centos-7"></a>CentOS 7
Dağıtım yüklenmesi sırasında şifreleme yapılandırmak için aşağıdakileri yapın:
1. Seçin **verilerimi şifrelemek** bölümleme zaman diskleri.

 ![CentOS 7 Kurulumu](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Emin **şifrele** kök bölümü için seçilir.

 ![CentOS 7 Kurulumu](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Bir parola girin. Anahtar kasanızı yükleyeceği parolayı budur.

 ![CentOS 7 Kurulumu](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. VM'yi önyüklemek için bir parola istendiğinde, 3. adımda belirttiğiniz parolayı kullanın.

 ![CentOS 7 Kurulumu](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. VM içindeki "CentOS 7.0 +" yönergeleri kullanarak Azure'a yüklemek için hazırlama [Azure'da CentOS tabanlı bir sanal makine hazırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70). (VM sağlamayı kaldırma) işleminin son adımında çalıştırmayın henüz.

6. VM'nin sağlamasını kaldırmak artık ve [VHD'nizi karşıya](#upload-encrypted-vhd-to-an-azure-storage-account) azure'a.

Azure ile çalışacak şekilde şifrelemesini yapılandırmak için aşağıdakileri yapın:

1. /Etc/dracut.conf düzenleyin ve aşağıdaki satırı ekleyin:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Bu satırları açıklama satırı yapar dosya /usr/lib/dracut/modules.d/90crypt/module-setup.sh sonunda:
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

3. Dosya /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh başındaki aşağıdaki satırı ekleyin:
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
4. /Usr/lib/dracut/Modules.d/90crypt/cryptroot-ASK.sh düzenleyin ve bu "# açık LUKS cihaz sonra" ekleyin:
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
5. Çalıştır "/ usr/sbin/dracut - f - v" initrd güncelleştirilecek.

![CentOS 7 Kurulumu](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a>Azure depolama hesabınız için şifrelenmiş VHD yükleme
BitLocker şifrelemesi veya DM-Crypt şifrelemesi etkinleştirildikten sonra depolama hesabınıza yüklenmek üzere yerel şifrelenmiş VHD gerekir.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-the-disk-encryption-secret-for-the-pre-encrypted-vm-to-your-key-vault"></a>Disk şifreleme gizli anahtar kasanıza önceden şifrelenmiş VM için karşıya yükleme
Elde ettiğiniz disk şifreleme gizli anahtar kasanızdaki gizli dizi olarak daha önce karşıya yüklenmelidir. Anahtar kasası disk şifrelemesi ve Azure AD istemciniz için etkinleştirilmiş olmalıdır.

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Disk şifreleme gizli bilgisi bir KEK ile şifrelenmiş değil
Gizli anahtar kasanızdaki ayarlamak için kullanın [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret). Bir Windows sanal makine varsa, bek dosya kodlanmış bir base64 dizesi ve ardından kullanarak anahtar kasası karşıya `Set-AzureKeyVaultSecret` cmdlet'i. Linux için parola base64 dizesi olarak kodlanmış ve daha sonra anahtar kasasına yüklenmiş. Ayrıca, anahtar kasasında gizli dizi oluşturduğunuzda, aşağıdaki etiketleri ayarlandığından emin olun.

    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

Kullanım `$secretUrl` için sonraki adımda [KEK kullanmadan işletim sistemi diskindeki](#without-using-a-kek).

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>Bir KEK ile şifrelenmiş disk şifreleme gizli bilgisi
Gizli anahtar Kasası'na yüklemeden önce bir anahtar şifreleme anahtarı kullanarak isteğe bağlı olarak şifreleyebilirsiniz. Kaydırma kullanın [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) ilk anahtar şifreleme anahtarı kullanarak şifrelemek için. Bir gizli dizi kullanarak daha sonra karşıya yükleyebilirsiniz base64 URL olarak kodlanmış dize çıktıdır bu kaydırma işleminin [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet'i.

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

Kullanım `$KeyEncryptionKey` ve `$secretUrl` için sonraki adımda [KEK kullanarak işletim sistemi diski](#using-a-kek).

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a>Bir işletim sistemi diski, gizli bir URL belirtin
#### <a name="without-using-a-kek"></a>Bir KEK kullanmadan
İşletim sistemi diski iliştirmekte olduğunuz olsa da geçirmeniz gerekir `$secretUrl`. URL "Disk şifreleme gizli bir KEK ile şifrelenmiş değil" bölümünde oluşturuldu.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a>Kullanarak bir KEK
İşletim sistemi diski, pass `$KeyEncryptionKey` ve `$secretUrl`. URL "Disk şifreleme gizli bir KEK ile şifrelenmiş değil" bölümünde oluşturuldu.

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

## <a name="for-more-information"></a>Daha fazla bilgi edinmek için
[Azure PowerShell - bölüm 1 ile Azure Disk şifrelemesini keşfetme](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)  
[Azure PowerShell - bölüm 2 ile Azure Disk şifrelemesini keşfetme](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
