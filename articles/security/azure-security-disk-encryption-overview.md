---
title: Azure Disk şifrelemesi nedir?
description: Bu makalede Azure Disk Şifrelemesi'ne genel bakış sağlar.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 06/05/2019
ms.custom: seodec18
ms.openlocfilehash: 9b00a5262b1e144aa47cd7fd640906225ff4fecd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068800"
---
# <a name="azure-disk-encryption-overview"></a>Azure Disk Şifrelemesi'ne genel bakış

Azure Disk şifrelemesi koruyun ve organizasyonel güvenlik ve uyumluluk yükümlülüklerinizin yerine verilerinizin korunmasına yardımcı olur. Kullandığı [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) Windows özelliğidir ve [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux Azure sanal makinelerini (VM) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için bir özelliğidir. İle de tümleştirilen [Azure anahtar kasası](https://azure.microsoft.com/documentation/services/key-vault/) denetlemenize ve disk şifreleme anahtarlarını ve gizli yönetmenize yardımcı olmak için ve sanal makine disklerindeki tüm veriler Azure depolama bölgesinde, bekleyen şifrelenmesini sağlar. Azure için Disk şifreleme Windows ve Linux Vm'leri, genel kullanıma sunulmuştur tüm Azure genel bölgeler ve standart VM'ler ve Azure Premium depolama ile sanal makineleri için Azure kamu bölgeleri. 

Azure Güvenlik Merkezi kullanırsanız, şifreli olmayan VM'ler varsa uyarı. Uyarılar yüksek önem derecesine sahip olarak gösterir ve bu VM'lerin şifrelemek için önerilir.

![Azure Güvenlik Merkezi disk Şifreleme Uyarısı](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Bazı öneriler, veri, ağ veya hesaplama kaynak kullanımı ve sonucunda ek lisans ya da abonelik maliyetlerinizi artırabilir.


## <a name="encryption-scenarios"></a>Şifreleme senaryoları

Azure Disk şifrelemesi ile bekleyen endüstri standardı şifreleme teknolojisini kullanarak Azure sanal makinelerinize güvenli hale getirme Kurumsal güvenlik ve uyumluluk gereksinimlerini karşılayabilirsiniz. Ayrıca, müşteri tarafından denetlenen anahtarları ve ilkeleri (BYOK) altında önyüklemek için Vm'leri yapılandırma ve anahtar kasanıza Bu anahtarları kullanımını denetleme.

Azure Disk şifrelemesi aşağıdaki müşteri senaryoları destekler:

* Etkinleştirme ve desteklenen Azure galeri görüntüleri kullanılarak oluşturulan yeni VM üzerinde şifrelemeyi devre dışı bırakma.
* Etkinleştirmek ve Azure'da çalıştıran var olan sanal makineler üzerinde şifrelemeyi devre dışı bırakılıyor.
* Etkinleştirme ve önceden şifrelenmiş VHD ve şifreleme anahtarlarını oluşturulan yeni Windows vm'lerinde şifreleme devre dışı bırakma.
* Windows sanal makine ölçek üzerinde şifrelemeyi devre dışı bırakma ve etkinleştirme ayarlar.
* Linux sanal makine ölçek kümeleri için veri şifrelemeyi devre dışı bırakma ve etkinleştirme beraberinde getirir.
* Etkinleştirme ve yönetilen disk Vm'lerinin şifrelemesini devre dışı bırakma.
* Mevcut şifrelenmiş Premium ve Premium Storage VM'si şifreleme ayarları güncelleştiriliyor.
* Şifrelenmiş Vm'leri yedekleme ve geri yükleme.
* Kendi şifreleme (BYOE) getirin ve müşterilerin kendi şifreleme anahtarlarını kullanmak ve bir Azure anahtar kasası depolamaya kendi anahtarı (BYOK) senaryolarını getirin.

Microsoft Azure'da etkin olduğunda da VM'ler için aşağıdaki senaryoları destekler:

* Azure anahtar kasası ile tümleştirme.
* [Standart katmanı Vm'lerini](https://azure.microsoft.com/pricing/details/virtual-machines/). [Linux Vm'leri](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) içinde bu katman 7 GB en düşük bellek gereksinimleri karşılaması gerekir. 
* Windows ve Linux Vm'leri, yönetilen disk ve ölçek üzerinde şifrelemeyi etkinleştirme Vm'leri desteklenen Azure galeri görüntüleri kullanılarak ayarlayın.
* İşletim sistemi ve veri şifrelemeyi devre dışı sürücüler için Windows Vm'leri, Ölçek, vm'leri ve yönetilen disk sanal makinelerini.
* Veri sürücülerinde devre dışı bırakma şifreleme için Linux Vm'leri, Ölçek, vm'leri ve yönetilen disk sanal makinelerini.
* Windows istemci işletim sistemi çalıştıran sanal makineler üzerinde şifrelemeyi etkinleştirme.
* Bağlama yolu içeren birimlerde şifreleme etkinleştiriliyor.
* Şifreleme (RAID) mdadm kullanarak bölümlenerek disk ile yapılandırılmış Linux vm'lerinde etkinleştiriliyor.
* Şifrelemeyi etkinleştirme Linux vm'lerinde LVM veri diskleri için kullanın.
* Linux VM işletim sistemi ve veri diskleri üzerinde şifrelemeyi etkinleştirme.

   > [!NOTE]
   > Bazı Linux dağıtımlarında için işletim sistemi Sürücü Şifrelemesi desteklenmez. Daha fazla bilgi için [Azure Disk şifrelemesi hakkında SSS](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) makalesi.
   
* Windows Server 2016'dan itibaren Windows depolama alanları ile yapılandırılmış VM üzerinde şifrelemeyi etkinleştirme.
* Yedekleme ve anahtar şifreleme anahtarı (KEK) ve KEK olmayan senaryolar için şifrelenmiş Vm'leri geri yükleme.

Azure Disk şifrelemesi, aşağıdaki senaryolarda, özellikler ve teknolojisi için çalışmaz:

* Temel katman sanal makine veya sanal makineleri Klasik VM oluşturma yöntemiyle oluşturulan şifreleme.
* İşletim sistemi sürücü şifrelendiğinde, bir işletim sistemi veya veri sürücüsü bir Linux VM üzerinde şifrelemeyi devre dışı bırakılıyor.
* Linux sanal makine ölçek kümesi için işletim sistemi sürücüsünü şifrelemek ayarlar.
* Yazılım tabanlı RAID sistemler ile yapılandırılan Windows VM şifreleme.
* Linux vm'lerinde şifreleme özel görüntüler.
* Bir şirket içi anahtar yönetimi sistemi ile tümleştirme.
* Azure dosyaları (paylaşılan dosya sistemi).
* Ağ dosya sistemi (NFS).
* Dinamik birimler.

## <a name="encryption-features"></a>Şifreleme özellikleri

Etkinleştirme ve Azure Vm'leri için Azure Disk şifrelemesi dağıtma ne zaman etkin için aşağıdaki özellikleri yapılandırabilirsiniz:

* İşletim sistemi birimi ve önyükleme birimi depolama alanınızda bekleyen korumak için şifreleme.
* Veri birimleri, depolama alanınızı bölgesinde, bekleyen veri birimlerini korumak için şifreleme.
* Windows VM'ler için işletim sistemi ve veri sürücülerini şifreleme devre dışı bırakılıyor.
* (Yalnızca işletim sistemi sürücüsünü şifreli olmayan olduğunda) verileri şifrelemeyi devre dışı bırakma Linux Vm'leri için sürücüler.
* Şifreleme anahtarlarını ve gizli anahtarları Azure Key Vault aboneliğinizdeki koruma.
* Şifrelenmiş sanal Makineyi şifreleme durumunu raporlama.
* Disk şifreleme yapılandırma ayarlarını VM'den kaldırılıyor.
* Yedekleme ve Azure Backup hizmetini kullanarak şifrelenmiş Vm'leri geri yükleme.

VM'ler için Windows ve Linux için Azure Disk şifrelemesi içerir:

* [Windows için disk şifrelemesi uzantısını](../virtual-machines/extensions/azure-disk-enc-windows.md).
* [Linux için disk şifrelemesi uzantısını](../virtual-machines/extensions/azure-disk-enc-linux.md).
* [Disk şifreleme PowerShell cmdlet'lerini](/powershell/module/az.compute/set-azvmdiskencryptionextension?view=azps-2.2.0).
* [Azure CLI disk şifreleme cmdlet'leri](/cli/azure/vm/encryption?view=azure-cli-latest).
* [Azure Resource Manager disk şifreleme şablonları](azure-security-disk-encryption-appendix.md#resource-manager-templates).

Azure Disk şifrelemesi, Windows veya Linux işletim sistemi çalıştıran sanal makineler üzerinde desteklenir. Desteklenen işletim sistemleri hakkında daha fazla bilgi için bkz. [sık sorulan sorular](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport).

> [!NOTE]
> Azure Disk şifrelemesi ile VM disklerini şifrelemek için ek ücret yoktur. Standart [anahtar kasası fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/) şifreleme anahtarları depolamak için kullanılan anahtar kasası için geçerlidir. 

## <a name="encryption-workflow"></a>Şifreleme iş akışı

Windows ve Linux Vm'leri için disk şifrelemeyi etkinleştirmek için aşağıdaki adımları uygulayın:

1. Azure Disk şifrelemesi Resource Manager şablonu, PowerShell cmdlet'leri veya Azure CLI üzerinden disk şifrelemeyi etkinleştirmek için katılım ve şifreleme yapılandırması belirtin.

   * Müşteri şifrelenmiş VHD senaryosu için depolama hesabınızı ve şifreleme anahtar malzemesi anahtar kasanıza şifrelenmiş VHD yükleyin. Ardından, yeni bir VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırmasını sağlar.
   * Desteklenen galeri görüntüleri kullanılarak oluşturulan yeni VM'ler ve Azure'da çalışması zaten var olan VM'ler için VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırması sağlar.

1. Şifreleme anahtar malzemesi (BitLocker şifreleme anahtarlarını Windows sistemleri için) ve parolayı Linux için VM üzerinde şifrelemeyi etkinleştirmek için anahtar kasanıza okumak üzere Azure platformunun erişim izni verin.

1. Azure VM hizmet modeli şifrelemesi ile key vault yapılandırması güncelleştirir ve, şifrelenmiş sanal Makineyi ayarlar.

   ![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Şifre çözme iş akışı
Sanal makineler için disk şifreleme devre dışı bırakmak için aşağıdaki üst düzey adımları tamamlayın:

1. Şifreleme (şifre çözme) devre dışı bırakmak azure'da çalışan bir VM seçin ve şifre çözme yapılandırmayı belirtin. Azure Disk şifrelemesi Resource Manager şablonu, PowerShell cmdlet'leri ve Azure CLI devre dışı bırakabilirsiniz.

   Bu adım işletim sistemi veya veri hacmi ya da hem de çalışan bir Windows VM şifreleme devre dışı bırakır. Önceki bölümde belirtildiği gibi Linux işletim sistemi disk şifreleme devre dışı bırakma özelliği desteklenmiyor. İşletim sistemi diski şifreli değil sürece şifre çözme adım yalnızca Linux vm'lerinde veri sürücülerine izin verilir.

1. Azure güncelleştirmeleri VM hizmet modeli ve VM olarak işaretlenmiş şifresi. VM içeriğini artık bekleme durumundayken şifrelenir.

   > [!NOTE]
   > Devre dışı bırakma şifreleme işlemi, anahtar kasanıza ve şifreleme anahtar malzemesi (BitLocker şifreleme anahtarlarını Windows sistemleri için) veya Linux için parola silmez.
   >
   > Linux için işletim sistemi disk şifreleme devre dışı bırakma desteklenmiyor. Şifre çözme adım, yalnızca Linux vm'lerinde veri sürücüleri için izin verilir.
   >
   > Linux için disk şifrelemeyi devre dışı bırakma, işletim sistemi sürücüsünü şifrelendiyse desteklenmez.


## <a name="encryption-workflow-previous-release"></a>Şifreleme iş akışı (önceki sürüm)

Azure Disk Şifrelemesi'nın yeni sürümüne VM disk şifrelemeyi etkinleştirmek için bir Azure Active Directory (Azure AD) uygulama parametresi sağlama gereksinimini ortadan kaldırır. Yeni sürümle birlikte, artık etkinleştirme şifreleme adımı sırasında bir Azure AD kimlik bilgisi sağlayın isteniyor. Yeni yayın kullandığınızda, tüm yeni Vm'lere Azure AD uygulama parametresiz şifrelenmelidir. Zaten Azure AD uygulama parametreleri ile şifrelenmiş VM'ler hala desteklenmektedir ve Azure AD sözdizimi ile yönetilmeye devam. Windows ve Linux Vm'leri (önceki sürüm) için disk şifrelemeyi etkinleştirmek için aşağıdaki adımları uygulayın:

1. Bir şifreleme senaryo listelenen senaryolar arasından seçim [şifreleme senaryolarının](#encryption-scenarios) bölümü.

1. Azure Disk şifrelemesi Resource Manager şablonu, PowerShell cmdlet'leri veya Azure CLI üzerinden disk şifrelemeyi etkinleştirmek için katılım ve şifreleme yapılandırması belirtin.

   * Müşteri şifrelenmiş VHD senaryosu için depolama hesabınızı ve şifreleme anahtar malzemesi anahtar kasanıza şifrelenmiş VHD yükleyin. Ardından, yeni bir VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırmasını sağlar.
   * Marketten oluşturulan yeni VM'ler ve Azure'da çalışması zaten var olan VM'ler için VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırması sağlar.

1. Şifreleme anahtar malzemesi (BitLocker şifreleme anahtarlarını Windows sistemleri için) ve parolayı Linux için VM üzerinde şifrelemeyi etkinleştirmek için anahtar kasanıza okumak üzere Azure platformunun erişim izni verin.

1. Anahtar kasanızı malzeme şifreleme anahtarını yazmak için Azure AD uygulama kimliği sağlayın. Bu adım 2. adımda bahsedilen senaryolar için VM üzerinde şifrelemeyi etkinleştirir.

1. Azure VM hizmet modeli şifrelemesi ile key vault yapılandırması güncelleştirir ve, şifrelenmiş sanal Makineyi ayarlar.


## <a name="terminology"></a>Terminoloji
Aşağıdaki tabloda bazı Azure disk şifrelemesi belgelerde kullanılan yaygın terimlerin tanımlar:

| Terminoloji | Tanım |
| --- | --- |
| Azure AD | Bir [Azure AD'ye](https://azure.microsoft.com/documentation/services/active-directory/) hesap kimlik doğrulaması, depolama ve gizli dizileri anahtar kasasından almak için kullanılır. |
| Azure Key Vault | Key Vault, Federal Bilgi işleme standartları (FIPS) üzerinde doğrulanmış donanım güvenlik modülleri dayanıyor bir şifreleme ve anahtar yönetim hizmetidir. Bu standartlar, şifreleme anahtarlarını ve gizli gizli anahtarlarınızı yardımcı olur. Daha fazla bilgi için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) belgeleri. |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) Windows Vm'leri disk şifrelemesini etkinleştirmek için kullanılan bir sektörde tanınmış Windows birim şifreleme teknolojisidir. |
| BEK | BitLocker şifreleme anahtarları (BEK), veri hacimleri ve işletim sistemi önyükleme birimi şifrelemek için kullanılır. BEKs anahtar kasasında gizli dizi olarak korunur. |
| Azure CLI | [Azure CLI'yı](/cli/azure/install-azure-cli) Azure kaynaklarını komut satırından yönetmek ve için optimize edilmiştir.|
| DM-Crypt |[DM-Crypt](https://gitlab.com/cryptsetup/cryptsetup/wikis/DMCrypt) Linux vm'lerinde disk şifrelemeyi etkinleştirmek için kullanılan Linux tabanlı, saydam disk şifreleme alt sistemi. |
| Anahtar şifreleme anahtarı (KEK) | Dosya korumak veya gizli dizi sarmalamak için kullanabileceğiniz asimetrik anahtardır (RSA 2048). Bir donanım güvenlik modülü (HSM) sağladığınız-korumalı bir anahtar veya yazılım korumalı anahtarı. Daha fazla bilgi için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) belgeleri. |
| PowerShell cmdlet'leri | Daha fazla bilgi için [Azure PowerShell cmdlet'lerini](/powershell/azure/overview). |

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için bkz: [Azure Disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md).

