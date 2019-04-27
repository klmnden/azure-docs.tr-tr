---
title: Genel Bakış - Iaas Vm'leri için Azure Disk şifrelemesi | Microsoft Docs
description: Bu makale, Iaas sanal makineleri için Microsoft Azure Disk Şifrelemesi'ne genel bakış sağlar.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/16/2019
ms.custom: seodec18
ms.openlocfilehash: 66d788aec83e3e57a49b063f2ca80484360f639d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60612080"
---
# <a name="azure-disk-encryption-for-iaas-vms"></a>Iaas VM'ler için Azure Disk şifrelemesi

Microsoft Azure, veri gizliliği ve veri egemenliği sağlamaya yönelik taahhüt eder. Azure, Azure'da barındırılan verilerinizi şifrelemek için Denetim ve şifreleme anahtarlarını ve veri denetimi ve Denetim erişimi yönetmek için Gelişmiş teknolojilerden bir dizi aracılığıyla denetlemenize olanak tanıyor. Bu denetim, Azure müşterilerine iş gereksinimlerini en iyi karşılayan çözümü seçme esnekliği sağlar. Bu makale için bir teknoloji çözümü sunar: "Azure için Disk şifreleme Windows ve Linux Iaas sanal makineleri (VM'ler)." Bu teknoloji, Kurumsal güvenlik ve uyumluluk taahhütlerinizi yerine verilerinizi koruyarak yardımcı olur. 

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]


## <a name="overview"></a>Genel Bakış

Azure Disk şifrelemesi, Windows ve Linux Iaas sanal makine disklerinizi şifreleyin yardımcı olan bir özelliktir. Disk şifrelemesi, endüstri standardı yararlanır [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) Windows özelliğidir ve [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için bir özelliğidir. İle tümleşik bir çözüm [Azure anahtar kasası](https://azure.microsoft.com/documentation/services/key-vault/) denetlemek ve disk şifreleme anahtarlarını ve gizli anahtarları yönetme yardımcı olacak. Çözüm Ayrıca, sanal makine disklerindeki tüm veriler Azure depolama alanınızda bekleme sırasında şifrelenir sağlar.

İçin disk şifreleme Windows ve Linux Iaas sanal makineleri, genel kullanıma sunulmuştur tüm Azure genel bölgeler ve standart VM'ler ve Azure Premium depolama ile sanal makineleri için Azure kamu bölgeleri. Disk şifrelemesi yönetim çözümü uyguladığınızda, aşağıdaki iş gereksinimlerini karşılamak:

* Iaas Vm'leri, Kurumsal güvenlik ve uyumluluk gereksinimlerini karşılamak için endüstri standardı şifreleme teknolojisini kullanarak bekleyen güvenlidir.
* Müşteri tarafından denetlenen anahtarları ve ilkeleri altında Iaas Vm'leri önyükleme. Anahtar kasanızda kullanımlarını denetleyebilirsiniz.

Azure Güvenlik Merkezi kullanırsanız, şifreli olmayan VM'ler varsa uyarı. Uyarılar yüksek önem derecesine sahip olarak gösterir ve bu VM'lerin şifrelemek için önerilir.

![Azure Güvenlik Merkezi disk Şifreleme Uyarısı](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Bazı öneriler, veri, ağ veya hesaplama kaynak kullanımı ve sonucunda ek lisans ya da abonelik maliyetlerinizi artırabilir.


## <a name="encryption-scenarios"></a>Şifreleme senaryoları

Disk şifrelemesi çözümü, aşağıdaki müşteri senaryoları destekler:

* Önceden şifrelenmiş VHD ve şifreleme anahtarlarından oluşturulan yeni Windows Iaas Vm'leri şifrelemeyi etkinleştirin.
* Desteklenen Azure galeri görüntüleri kullanılarak oluşturulan yeni Iaas Vm'leri şifrelemeyi etkinleştirin.
* Azure'da çalışan mevcut Iaas Vm'leri şifrelemeyi etkinleştirin.
* Windows sanal makine ölçek kümeleri şifrelemeyi etkinleştirin.
* Veri sürücüleri Linux sanal makine ölçek kümeleri için şifrelemeyi etkinleştirin.
* Windows Iaas Vm'leri üzerinde şifrelemeyi devre dışı bırakın.
* Linux Iaas sanal makineler için veri sürücülerini şifreleme devre dışı bırakın.
* Windows sanal makine ölçek kümeleri üzerinde şifrelemeyi devre dışı bırakın.
* Linux sanal makine ölçek kümeleri için veri sürücülerini şifreleme devre dışı bırakın.
* Yönetilen disk Vm'lerinin şifrelenmesini etkinleştirmek.
* Mevcut şifrelenmiş Premium ve Premium Storage VM'si şifreleme ayarlarını güncelleştirin.
* Yedekleme ve şifrelenmiş Vm'leri geri yükleme.

Microsoft Azure'da etkin çözüm Iaas Vm'leri için aşağıdaki senaryoları destekler:

* Azure anahtar kasası ile tümleştirme.
* Standart katmanı Vm'lerini: [A, D, DS, G, GS, F ve benzeri serisi Iaas Vm'leri](https://azure.microsoft.com/pricing/details/virtual-machines/). [Linux Vm'leri](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) içinde bu katman 7 GB en düşük bellek gereksinimleri karşılaması gerekir.
* Windows ve Linux Iaas Vm'leri ve yönetilen disk ölçek üzerinde şifrelemeyi etkinleştirme, VM'lerin desteklenen Azure galeri görüntüleri kullanılarak ayarlayın.
* İşletim sistemi ve veri sürücüleri devre dışı bırakma şifreleme Windows Iaas VM'ler, Ölçek, vm'leri ve yönetilen disk sanal makinelerini.
* Linux Iaas sanal makineler, Ölçek için devre dışı bırakma şifreleme veri sürücülerinde vm'leri ve yönetilen disk sanal makinelerini.
* Windows istemci işletim sistemi çalıştıran bir Iaas Vm'leri şifrelemeyi etkinleştirin.
* Bağlama yolu birimlerle şifrelemeyi etkinleştirin.
* Linux vm'lerinde mdadm kullanarak (RAID) bölümlenerek diskle yapılandırılmış şifrelemeyi etkinleştirir.
* Linux vm'lerinde LVM kullanan veri diskleri için şifrelemeyi etkinleştirin.
* Linux VM işletim sistemi ve veri diskleri şifrelemeyi etkinleştirin.

   > [!NOTE]
   > Bazı Linux dağıtımlarında için işletim sistemi Sürücü Şifrelemesi desteklenmez. Daha fazla bilgi için [Azure Disk şifrelemesi hakkında SSS](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) makalesi.
   
* Windows Server 2016'dan itibaren Windows depolama alanları ile yapılandırılmış VM üzerinde şifrelemeyi etkinleştirir.
* Bir mevcut şifrelenmiş Premium ve Premium Storage VM'si için şifreleme ayarlarını güncelleştirin.
* Yedekleme ve anahtar şifreleme anahtarı (KEK) hem de KEK olmayan senaryolar için şifrelenmiş vm'leri geri yükleme.
* Tüm Azure genel ve Azure kamu bölgeleri desteklenir.

Çözüm, aşağıdaki senaryolarda, özellikleri ve teknoloji desteklemez:

* Temel katman Iaas Vm'leri.
* Linux Iaas Vm'leri için şifreleme bir işletim sistemi sürücüsünde devre dışı bırakın.
* Linux Iaas sanal makineleri için işletim sistemi sürücüsünü şifreli olduğunda bir veri sürücüsü üzerinde şifrelemeyi devre dışı bırakın.
* Linux sanal makine ölçek kümesi için işletim sistemi Sürücü Şifrelemesi ayarlar.
* Klasik sanal makine oluşturma yöntemini kullanarak oluşturulan Iaas VM'ler.
* Müşteri özel görüntüleri Linux Iaas sanal makineleri üzerinde şifrelemeyi etkinleştirir.
* Şirket içi anahtar yönetimi sistemi ile tümleştirme.
* Azure dosyaları (paylaşılan dosya sistemi).
* Ağ dosya sistemi (NFS).
* Dinamik birimler.
* Windows yazılım tabanlı RAID sistemler ile yapılandırılmış VM'ler.

## <a name="encryption-features"></a>Şifreleme özellikleri

Etkinleştirme ve Azure Iaas Vm'leri için Disk şifrelemesi dağıtma sağlanan yapılandırmasına bağlı olarak aşağıdaki özellikleri etkinleştirilir:

* İşletim sistemi birimi ve önyükleme birimi depolama alanınızda bekleyen korumak için şifreleme.
* Veri birimleri, depolama alanınızı bölgesinde, bekleyen veri birimlerini korumak için şifreleme.
* Windows Iaas Vm'leri için işletim sistemi ve veri sürücülerini şifreleme devre dışı bırakın.
* (Yalnızca işletim sistemi sürücüsünü şifreli değil) Linux Iaas sanal makineler için veri sürücülerini şifreleme devre dışı.
* Şifreleme anahtarlarını ve gizli anahtarları Azure Key Vault aboneliğinizdeki koruyun.
* Şifrelenmiş Iaas VM şifreleme durumunu bildirir.
* Disk şifreleme yapılandırma ayarlarını Iaas VM'den kaldırın.
* Yedekleme ve Azure Backup hizmetini kullanarak şifrelenmiş Vm'leri geri yükleme.

Azure Disk şifrelemesi çözümü, Windows ve Linux Iaas VM'leri için şunları içerir:

* Windows için disk şifrelemesi uzantısı.
* Linux için disk şifrelemesi uzantısı.
* PowerShell disk şifreleme cmdlet'leri.
* Azure CLI disk şifreleme cmdlet'leri.
* Azure Resource Manager disk şifreleme şablonları.

Azure Disk şifrelemesi çözümü, Windows veya Linux işletim sistemi çalıştırılan Iaas Vm'lerine desteklenir. Desteklenen işletim sistemleri hakkında daha fazla bilgi için bkz. [önkoşulları](azure-security-disk-encryption-prerequisites.md) makalesi.

> [!NOTE]
> Azure Disk şifrelemesi ile VM disklerini şifrelemek için ek ücret yoktur. Standart [anahtar kasası fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/) şifreleme anahtarları depolamak için kullanılan anahtar kasası için geçerlidir. 


## <a name="encryption-workflow"></a>Şifreleme iş akışı

Windows ve Linux Vm'leri için disk şifrelemeyi etkinleştirmek için aşağıdaki adımları uygulayın:

1. Bir şifreleme senaryo listelenen senaryolar arasından seçim [şifreleme senaryolarının](#encryption-scenarios) bölümü.

1. Azure Disk şifrelemesi Resource Manager şablonu, PowerShell cmdlet'leri veya Azure CLI üzerinden disk şifrelemeyi etkinleştirmek için katılım ve şifreleme yapılandırması belirtin.

   * Müşteri şifrelenmiş VHD senaryosu için depolama hesabınızı ve şifreleme anahtar malzemesi anahtar kasanıza şifrelenmiş VHD yükleyin. Ardından, yeni bir Iaas VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırmasını sağlar.
   * Marketten oluşturulan yeni VM'ler ve Azure'da çalışması zaten var olan VM'ler için Iaas VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırması sağlar.

1. Iaas VM üzerinde şifrelemeyi etkinleştirmek için anahtar kasanıza (BitLocker şifreleme anahtarlarını Windows sistemleri için) ve Linux için parola şifreleme anahtar malzemesi okumak üzere Azure platformunun erişim izni verin.

1. Azure VM hizmet modeli şifrelemesi ile key vault yapılandırması güncelleştirir ve, şifrelenmiş sanal Makineyi ayarlar.

   ![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Şifre çözme iş akışı
Iaas Vm'leri için disk şifreleme devre dışı bırakmak için aşağıdaki üst düzey adımları tamamlayın:

1. Azure'da çalışan bir Iaas sanal makinesi üzerinde şifreleme (şifre çözme) devre dışı bırakmak seçin ve şifre çözme yapılandırmayı belirtin. Azure Disk şifrelemesi Resource Manager şablonu, PowerShell cmdlet'leri ve Azure CLI devre dışı bırakabilirsiniz.

   Bu adım işletim sistemi veya veri hacmi ya da hem de çalışan Windows Iaas VM şifreleme devre dışı bırakır. Önceki bölümde belirtildiği gibi Linux işletim sistemi disk şifreleme devre dışı bırakma özelliği desteklenmiyor. İşletim sistemi diski şifreli değil sürece şifre çözme adım yalnızca Linux vm'lerinde veri sürücülerine izin verilir.

1. Azure güncelleştirmeleri VM hizmet modeli ve Iaas VM olarak işaretlenmiş şifresi. VM içeriğini artık bekleme durumundayken şifrelenir.

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

   * Müşteri şifrelenmiş VHD senaryosu için depolama hesabınızı ve şifreleme anahtar malzemesi anahtar kasanıza şifrelenmiş VHD yükleyin. Ardından, yeni bir Iaas VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırmasını sağlar.
   * Marketten oluşturulan yeni VM'ler ve Azure'da çalışması zaten var olan VM'ler için Iaas VM üzerinde şifrelemeyi etkinleştirmek için şifreleme yapılandırması sağlar.

1. Iaas VM üzerinde şifrelemeyi etkinleştirmek için anahtar kasanıza (BitLocker şifreleme anahtarlarını Windows sistemleri için) ve Linux için parola şifreleme anahtar malzemesi okumak üzere Azure platformunun erişim izni verin.

1. Anahtar kasanızı malzeme şifreleme anahtarını yazmak için Azure AD uygulama kimliği sağlayın. Bu adım 2. adımda bahsedilen senaryolar için Iaas VM üzerinde şifrelemeyi etkinleştirir.

1. Azure VM hizmet modeli şifrelemesi ile key vault yapılandırması güncelleştirir ve, şifrelenmiş sanal Makineyi ayarlar.


## <a name="terminology"></a>Terminoloji
Aşağıdaki tabloda bazı kullanılan yaygın terimlerin bu teknoloji tanımlar:

| Terminoloji | Tanım |
| --- | --- |
| Azure AD | Bir [Azure AD'ye](https://azure.microsoft.com/documentation/services/active-directory/) hesap kimlik doğrulaması, depolama ve gizli dizileri anahtar kasasından almak için kullanılır. |
| Azure Key Vault | Key Vault, Federal Bilgi işleme standartları (FIPS) üzerinde doğrulanmış donanım güvenlik modülleri dayanıyor bir şifreleme ve anahtar yönetim hizmetidir. Bu standartlar, şifreleme anahtarlarını ve gizli gizli anahtarlarınızı yardımcı olur. Daha fazla bilgi için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) belgeleri. |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) Windows Iaas Vm'leri disk şifrelemesini etkinleştirmek için kullanılan bir sektörde tanınmış Windows birim şifreleme teknolojisidir. |
| BEK | BitLocker şifreleme anahtarları (BEK), veri hacimleri ve işletim sistemi önyükleme birimi şifrelemek için kullanılır. BEKs anahtar kasasında gizli dizi olarak korunur. |
| Azure CLI | [Azure CLI'yı](/cli/azure/install-azure-cli) Azure kaynaklarını komut satırından yönetmek ve için optimize edilmiştir.|
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux Iaas sanal makineleri disk şifrelemesini etkinleştirmek için kullanılan Linux tabanlı, saydam disk şifreleme alt sistemi. |
| KEK | Anahtar şifreleme anahtarı (KEK) koruyabilir veya gizli dizi sarmalamak için kullanabileceğiniz asimetrik anahtardır (RSA 2048) var. Bir donanım güvenlik modülü (HSM) sağladığınız-korumalı bir anahtar veya yazılım korumalı anahtarı. Daha fazla bilgi için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) belgeleri. |
| PowerShell cmdlet'leri | Daha fazla bilgi için [Azure PowerShell cmdlet'lerini](/powershell/azure/overview). |

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure Disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md)
