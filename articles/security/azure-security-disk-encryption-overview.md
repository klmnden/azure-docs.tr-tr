---
title: Azure Disk şifrelemesi için Iaas Vm'leri genel bakış | Microsoft Docs
description: Bu makale, Iaas sanal makineleri için Microsoft Azure Disk Şifrelemesi'ne genel bakış sağlar.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 09/14/2018
ms.openlocfilehash: 193aa8f87a90eb7bbf1e2c49132ad480881d41fe
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45633478"
---
# <a name="azure-disk-encryption-for-iaas-vms"></a>Iaas VM'ler için Azure Disk şifrelemesi 
Microsoft Azure sağlamak için veri gizliliği, veri egemenliği taahhüt eder ve denetlemek için etkinleştirme verileri şifrelemek için Denetim ve şifreleme anahtarlarını yönetmek için Gelişmiş teknolojilerden bir dizi aracılığıyla Azure'da barındırılan ve denetim ve denetim veri erişim. Bu denetim, Azure müşterilerine iş gereksinimlerini en iyi karşılayan çözümü seçme esnekliği sağlar. Bu makalede "Azure Disk şifrelemesi için Windows ve Linux Iaas korumak ve kurumsal güvenlik ve uyumluluk taahhütlerinizi yerine verilerinizi korumak için VM'ler", bir teknoloji çözümü tanıtılmaktadır. 

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]


## <a name="overview"></a>Genel Bakış
Azure Disk şifrelemesi (ADE), Windows ve Linux Iaas sanal makine disklerinizi şifreleyin yardımcı olan bir özelliktir. ADE yararlanır endüstri standardı [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) Windows özelliğidir ve [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için bir özelliğidir. İle tümleşik bir çözüm [Azure anahtar kasası](https://azure.microsoft.com/documentation/services/key-vault/) denetlemek ve disk şifreleme anahtarlarını ve gizli anahtarları yönetme yardımcı olacak. Çözüm ayrıca sanal makine disklerindeki tüm veriler Azure depolama alanınızda bekleyen şifrelenmesini sağlar.

Windows ve Linux Iaas sanal makineleri için Azure disk şifrelemesi olan **genel kullanılabilirlik** tüm genel Azure bölgeleri ve premium depolama ile standart VM'ler ve VM'ler için AzureGov bölgelerde. Azure Disk şifrelemesi yönetim çözümü uyguladığınızda, aşağıdaki iş gereksinimlerini karşılamak:

* Iaas Vm'leri, Kurumsal güvenlik ve uyumluluk gereksinimlerini için endüstri standardı şifreleme teknolojisi kullanılarak, bekleme sırasında korunur.
* Müşteri tarafından denetlenen anahtarları ve ilkeleri ve altında Iaas Vm'leri önyükleme kullanımlarını anahtar kasanızdaki denetleyebilirsiniz.


Azure Güvenlik Merkezi kullanırsanız, şifrelenmemiş sanal makineleriniz varsa, sizi uyarır. Bu uyarılar Yüksek Önem Derecesine Sahip olarak gösterilir ve bu sanal makineleri şifrelemeniz önerilir.
![Azure Güvenlik Merkezi disk Şifreleme Uyarısı](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Bazı öneriler, veri, ağ veya ek lisans ya da abonelik maliyetlerinizi kaynaklanan işlem kaynak kullanımını artırabilir.


## <a name="encryption-scenarios"></a>Şifreleme senaryoları
Azure Disk şifrelemesi çözümü, aşağıdaki müşteri senaryoları destekler:

* Önceden şifrelenmiş VHD ve şifreleme anahtarlarından oluşturulan yeni Windows Iaas Vm'leri şifrelemeyi etkinleştirin 
* Desteklenen Azure galeri görüntüleri kullanılarak oluşturulan yeni Iaas Vm'leri şifrelemeyi etkinleştirin
* Azure'da çalıştırılan Iaas Vm'leri mevcut şifrelemeyi etkinleştirin
* Windows sanal makine ölçek kümeleri şifrelemeyi etkinleştirin
* Veri sürücüleri Linux sanal makine ölçek kümeleri için şifrelemeyi etkinleştirin
* Windows Iaas Vm'leri üzerinde şifrelemeyi devre dışı bırak
* Linux Iaas sanal makineler için veri sürücülerinde şifrelemeyi devre dışı bırakma
* Windows sanal makine ölçek kümeleri üzerinde şifrelemeyi devre dışı bırak
* Linux sanal makine ölçek kümeleri için veri sürücülerini şifreleme devre dışı bırak
* Yönetilen disk Vm'lerinin şifrelenmesini etkinleştirmek
* Mevcut şifrelenmiş premium ve premium olmayan depolama VM'nin şifreleme ayarlarını güncelleştirme
* Yedekleme ve şifrelenmiş Vm'leri geri yükleme

Microsoft Azure'da etkin çözüm Iaas Vm'leri için aşağıdaki senaryoları destekler:

* Azure Key Vault ile tümleştirme
* Standart katmanı Vm'lerini: [A, D, DS, G, GS, F ve benzeri serisi Iaas Vm'leri](https://azure.microsoft.com/pricing/details/virtual-machines/)
    * [Linux Vm'leri](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) içinde bu katman 7 GB bellek alt sınırı gereksinimi karşılamalıdır
* Windows ve Linux Iaas Vm'leri ve yönetilen disk ölçek üzerinde şifrelemeyi etkinleştirme Vm'leri desteklenen Azure Galerisi'ndeki görüntüleri ayarlama
* Windows Iaas Vm'leri, Ölçek için işletim sistemi ve veri sürücülerinde devre dışı bırakma şifreleme vm'leri ve yönetilen disk sanal makinelerini
* Linux Iaas sanal makineler, Ölçek için devre dışı bırakma şifreleme veri sürücülerinde vm'leri ve yönetilen disk sanal makinelerini
* Windows istemci işletim sistemi çalıştıran Iaas Vm'leri şifrelemeyi etkinleştirin
* Bağlama yolu birimlerle şifrelemeyi etkinleştirin
* Disk ile yapılandırılmış Linux vm'lerinde şifrelemeyi etkinleştirme mdadm kullanarak şeritleme (RAID)
* Linux vm'lerinde LVM kullanarak veri diskleri için şifrelemeyi etkinleştirme
* Linux VM işletim sistemi ve veri diskleri şifrelemeyi etkinleştirin 
* Depolama alanları ile yapılandırılmış Windows vm'lerinde şifrelemeyi etkinleştir
* Mevcut şifrelenmiş premium ve premium olmayan depolama VM'nin şifreleme ayarlarını güncelleştirme
* Yedekleme ve no-KEK hem KEK senaryoları (KEK - anahtar şifreleme anahtarı) şifrelenmiş Vm'leri geri yükleme
* Tüm Azure genel ve AzureGov bölgeler desteklenir

Çözüm, aşağıdaki senaryolarda, özellikleri ve teknoloji desteklemez:

* Temel katman Iaas Vm'leri
* Linux Iaas Vm'leri için şifreleme bir işletim sistemi sürücüsünde devre dışı bırakma
* Linux Iaas sanal makineleri için işletim sistemi sürücüsünü şifrelendiyse veri sürücüsü üzerinde şifrelemeyi devre dışı bırakma
* Iaas VM'ler, Klasik VM oluşturma yöntemini kullanarak oluşturulur
* Linux Iaas sanal makineleri müşteri özel görüntüleri şifrelemesini etkinleştirme
* Şirket içi anahtar yönetimi hizmeti ile tümleştirme
* Azure dosyaları (paylaşılan dosya sistemi)
* Ağ dosya sistemi (NFS)
* Dinamik birimler
* Yazılım tabanlı RAID sistemler ile yapılandırılan Windows Vm'leri

## <a name="encryption-features"></a>Şifreleme özellikleri
Etkinleştirme ve Azure Disk şifrelemesi için Azure Iaas Vm'lerini dağıtmak, sağlanan yapılandırmasına bağlı olarak aşağıdaki özellikleri etkinleştirilir:

* Önyükleme birimi depolama alanınızda bekleyen korumak için işletim sistemi birimi şifrelemesi
* Veri birimleri, depolama alanınızı bölgesinde, bekleyen veri birimlerini korumak için şifreleme
* Windows Iaas Vm'leri için işletim sistemi ve veri sürücülerini şifreleme devre dışı bırakma
* Linux Iaas sanal makineler için veri sürücülerini şifreleme (yalnızca işletim sistemi sürücüsünü şifreli değil ise) devre dışı bırakma
* Şifreleme anahtarlarını ve gizli anahtar kasası aboneliğinizdeki koruma
* Şifrelenmiş Iaas VM şifreleme durumunu raporlama
* Iaas sanal makine disk şifreleme yapılandırma ayarlarını, temizleme
* Backup ve Azure Backup hizmetini kullanarak şifrelenmiş Vm'leri geri yükleme

Azure Disk şifrelemesi çözümü, Windows ve Linux Iaas VM'leri için şunları içerir:

* Windows için disk şifrelemesi uzantısı.
* Linux için disk şifrelemesi uzantısı.
* Disk şifrelemesi PowerShell cmdlet'leri.
* Disk şifrelemesi Azure komut satırı arabirimi (CLI) cmdlet'leri.
* Disk şifreleme Azure Resource Manager şablonları.

Azure Disk şifrelemesi çözümü, Windows veya Linux işletim sistemi çalıştırılan Iaas Vm'lerine desteklenir. Desteklenen işletim sistemleri hakkında daha fazla bilgi için bkz. [önkoşulları](azure-security-disk-encryption-prerequisites.md) makalesi.

> [!NOTE]
> Azure Disk şifrelemesi ile VM disklerini şifrelemek için ek ücret yoktur. Standart [anahtar kasası fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/) şifreleme anahtarları depolamak için kullanılan anahtar kasası için geçerlidir. 


## <a name="encryption-workflow"></a>Şifreleme iş akışı

 Windows ve Linux Vm'leri için disk şifrelemeyi etkinleştirmek için aşağıdaki adımları uygulayın:

1. Bir şifreleme senaryosu önceki şifreleme senaryolarının arasından seçin.
2. Etkinleştirme için iyileştirilmiş disk şifrelemesi Azure Disk şifrelemesi Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutu aracılığıyla ve şifreleme yapılandırması belirtin.

   * Müşteri şifrelenmiş VHD senaryosu için depolama hesabınızı ve şifreleme anahtar malzemesi anahtar kasanıza şifrelenmiş VHD yükleyin. Ardından, yeni bir Iaas VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırmasını sağlar.
   * Marketten oluşturulan yeni VM'ler ve Azure'da çalışmakta olan var olan VM'ler için Iaas VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırması sağlar.

3. Şifreleme anahtarı malzeme (BitLocker şifreleme anahtarlarını Windows sistemleri için) ve parolayı Linux Iaas VM üzerinde şifrelemeyi etkinleştirmek için anahtar kasanıza okuma için Azure platformunda erişim izni verin.

4. Azure VM hizmet modeli şifrelemesi ile key vault yapılandırması güncelleştirir ve, şifrelenmiş sanal Makineyi ayarlar.

 ![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Şifre çözme iş akışı
Iaas Vm'leri için disk şifreleme devre dışı bırakmak için aşağıdaki üst düzey adımları tamamlayın:

1. Azure'da çalışan bir Iaas sanal makinesi üzerinde şifreleme (şifre çözme) devre dışı bırakmak seçin ve şifre çözme yapılandırmayı belirtin. Azure Disk şifrelemesi Resource Manager şablonu, PowerShell cmdlet'leri veya Azure CLI devre dışı bırakabilirsiniz.

 Bu adım işletim sistemi veya veri hacmi ya da hem de çalışan Windows Iaas VM şifreleme devre dışı bırakır. Ancak, önceki bölümde belirtildiği gibi Linux işletim sistemi disk şifreleme devre dışı bırakma desteklenmiyor. İşletim sistemi diski şifreli değil sürece şifre çözme adım yalnızca Linux vm'lerinde veri sürücülerine izin verilir.
2. Azure VM hizmet modelini güncelleştirir ve Iaas VM şifresi çözülmüş olarak işaretlenir. VM içeriğini artık bekleme durumundayken şifrelenir.

> [!NOTE]
> Şifreleme devre dışı bırakma işlemi, anahtar kasanıza ve şifreleme anahtar malzemesi (BitLocker şifreleme anahtarlarını Windows sistemleri için) veya Linux için parola silmez.
 > Linux için işletim sistemi disk şifreleme devre dışı bırakma desteklenmiyor. Şifre çözme adım, yalnızca Linux vm'lerinde veri sürücüleri için izin verilir.
İşletim sistemi sürücüsünü şifrelendiyse Linux için disk şifrelemeyi devre dışı bırakma desteklenmiyor.


## <a name="encryption-workflow-previous-release"></a>Şifreleme iş akışı (önceki sürüm)

Azure disk şifrelemesi'nın yeni sürümüne VM disk şifrelemeyi etkinleştirmek için bir Azure AD uygulama parametresi sağlama gereksinimini ortadan kaldırır. Yeni sürümle birlikte, bir Azure AD kimlik bilgisi etkin şifreleme adımı sırasında sağlamak için artık gerekli değildir. Tüm yeni Vm'lere yeni sürümünü kullanarak Azure AD uygulama parametresiz şifrelenmelidir. Zaten Azure AD uygulama parametreleri ile şifrelenmiş VM'ler hala desteklenmektedir ve AAD sözdizimiyle korunmasını devam etmelidir. Windows ve Linux Vm'leri (önceki sürüm) için disk şifrelemeyi etkinleştirmek için aşağıdaki adımları uygulayın:

1. Bir şifreleme senaryosu önceki şifreleme senaryolarının arasından seçin.
2. Etkinleştirme için iyileştirilmiş disk şifrelemesi Azure Disk şifrelemesi Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutu aracılığıyla ve şifreleme yapılandırması belirtin.

   * Müşteri şifrelenmiş VHD senaryosu için depolama hesabınızı ve şifreleme anahtar malzemesi anahtar kasanıza şifrelenmiş VHD yükleyin. Ardından, yeni bir Iaas VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırmasını sağlar.
   * Marketten oluşturulan yeni VM'ler ve Azure'da çalışmakta olan var olan VM'ler için Iaas VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırması sağlar.

3. Şifreleme anahtarı malzeme (BitLocker şifreleme anahtarlarını Windows sistemleri için) ve parolayı Linux Iaas VM üzerinde şifrelemeyi etkinleştirmek için anahtar kasanıza okuma için Azure platformunda erişim izni verin.

4. Anahtar kasanızı malzeme şifreleme anahtarını yazmak için Azure Active Directory (Azure AD) uygulama kimliği sağlayın. Bunun yapılması, 2. adımda bahsedilen senaryolar için Iaas VM üzerinde şifrelemeyi etkinleştirir.

5. Azure VM hizmet modeli şifrelemesi ile key vault yapılandırması güncelleştirir ve, şifrelenmiş sanal Makineyi ayarlar.


## <a name="terminology"></a>Terminoloji
Bu teknoloji tarafından kullanılan ortak terimlerden bazılarının anlamak için aşağıdaki terimleri tabloyu kullanın:

| Terminoloji | Tanım |
| --- | --- |
| Azure AD | Azure AD, [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Bir Azure AD hesabı kimlik doğrulaması, depolama ve gizli dizileri anahtar kasasından almak için kullanılır. |
| Azure Key Vault | Key Vault, şifreleme anahtarlarını ve gizli gizli korunmasına yardımcı olan donanım güvenlik modülleri Federal Bilgi işleme standartları (FIPS) temel alarak bir şifreleme, anahtar yönetim hizmeti doğrulanacağını ' dir. Daha fazla bilgi için [Key Vault](https://azure.microsoft.com/services/key-vault/) belgeleri. |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) Windows Iaas Vm'leri disk şifrelemesini etkinleştirmek için kullanılan bir sektörde tanınmış Windows birim şifreleme teknolojisidir. |
| BEK | BitLocker şifreleme anahtarları, veri hacimleri ve işletim sistemi önyükleme birimi şifrelemek için kullanılır. BitLocker anahtarları bir anahtar kasasında gizli dizi olarak korunur. |
| CLI | Bkz: [Azure komut satırı arabirimi](/cli/azure/install-azure-cli).|
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux Iaas sanal makineleri disk şifrelemesini etkinleştirmek için kullanılan Linux tabanlı, saydam disk şifreleme alt sistemi. |
| KEK | Dosya korumak veya gizli dizi sarmalamak için kullanabileceğiniz asimetrik anahtardır (RSA 2048) anahtar şifreleme anahtarıdır. Bir donanım güvenlik modülü (HSM) sağladığınız-korumalı bir anahtar veya yazılım korumalı anahtarı. Daha fazla bilgi için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) belgeleri. |
| PS cmdlet'leri | Bkz: [Azure PowerShell cmdlet'lerini](/powershell/azure/overview). |

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure Disk Şifrelemesi Önkoşulları](azure-security-disk-encryption-prerequisites.md)
