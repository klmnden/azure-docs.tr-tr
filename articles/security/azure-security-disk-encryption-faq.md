---
title: SSS - Iaas Vm'leri için Azure Disk şifrelemesi | Microsoft Docs
description: Bu makale için Microsoft Azure Disk şifrelemesi Windows ve Linux Iaas sanal makineleri hakkında sık sorulan soruların yanıtlarını sağlar.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 06/05/2019
ms.custom: seodec18
ms.openlocfilehash: e1583ccf04b68f81a71bd2f63779680427ce3362
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068777"
---
# <a name="azure-disk-encryption-for-iaas-vms-faq"></a>Azure Disk şifrelemesi için Iaas Vm'leri SSS

Bu makale için Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler hakkında sık sorulan sorular (SSS) yanıtlarını sağlar. Bu hizmet hakkında daha fazla bilgi için bkz. [için Azure Disk şifrelemesi Windows ve Linux Iaas sanal makineleri](azure-security-disk-encryption-overview.md).

## <a name="where-is-azure-disk-encryption-in-general-availability-ga"></a>Azure Disk şifrelemesi genel kullanıma (GA) nerede?

Azure için Disk şifreleme Windows ve Linux Iaas sanal makineleri, genel tüm genel Azure bölgelerinde kullanılabilir.

## <a name="what-user-experiences-are-available-with-azure-disk-encryption"></a>Hangi kullanıcı deneyimleri Azure Disk şifrelemesi ile kullanılabilir?

Azure Resource Manager şablonları, Azure PowerShell ve Azure CLI, Azure Disk şifrelemesi GA destekler. Farklı kullanıcı deneyimleri esneklik sağlar. Iaas VM'ler için disk şifrelemesi'ni etkinleştirmek için üç farklı seçeneğiniz vardır. Azure Disk şifrelemesini adım adım kılavuzlar ve kullanıcı deneyimi hakkında daha fazla bilgi için bkz. [etkinleştirme Azure Disk şifrelemesi için Windows](azure-security-disk-encryption-windows.md) ve [Linux için Azure Disk şifrelemesini etkinleştirme](azure-security-disk-encryption-linux.md).

## <a name="how-much-does-azure-disk-encryption-cost"></a>Azure Disk şifrelemesi nin ücreti ne kadardır?

Azure Disk şifrelemesi ile VM disklerini şifrelemek için bir ücret yoktur, ancak Azure anahtar kasası kullanımıyla ilişkili ücretler vardır. Azure Key Vault maliyetleri hakkında daha fazla bilgi için bkz. [anahtar kasası fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/) sayfası.


## <a name="which-virtual-machine-tiers-does-azure-disk-encryption-support"></a>Hangi sanal makine katmanları Azure Disk şifrelemesi destekliyor mu?

Azure Disk şifrelemesi dahil olmak üzere standart katman sanal makinelerinde kullanılabilir [A, D, DS, E, G veya GS ve F](https://azure.microsoft.com/pricing/details/virtual-machines/) serisi Iaas VM'ler. Premium depolama ile sanal makineler için kullanılabilir. Temel katmanı Vm'lerini kullanılamaz.

## <a name="bkmk_LinuxOSSupport"></a> Hangi Linux dağıtımı, Azure Disk şifrelemesi destekliyor mu?

Azure Disk şifrelemesi, bir alt kümesi üzerinde desteklenir [Azure destekli Linux dağıtımları](../virtual-machines/linux/endorsed-distros.md), kendisini tüm Linux sunucusu olası dağıtımların bir alt kümesidir.

 ![Azure Disk şifrelemesi desteği Venn diyagramı Linux sunucusu dağıtımları](./media/azure-security-disk-encryption-faq/ade-supported-distros.png)

Azure tarafından onaylanan değil Linux sunucusu dağıtımı, Azure Disk şifrelemesi desteklemez ve içeriğiyle onaylanan, Azure Disk şifrelemesi yalnızca aşağıdaki dağıtımları ve sürümleri destekler:

| Linux dağıtım | Sürüm | Desteklenen şifreleme için birim türü|
| --- | --- |--- |
| Ubuntu | 18.04| İşletim sistemi ve veri diski |
| Ubuntu | 16.04| İşletim sistemi ve veri diski |
| Ubuntu | 14.04.5</br>[Azure ile 4.15 veya üzeri için güncelleştirilmiş çekirdek ayarlanmış](azure-security-disk-encryption-tsg.md#bkmk_Ubuntu14) | İşletim sistemi ve veri diski |
| RHEL | 7.6 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL | 7.5 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL | 7.4 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL | 7.3 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL | 7.2 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL | 6.8 | Veri diski (aşağıdaki nota bakın) |
| RHEL | 6.7 | Veri diski (aşağıdaki nota bakın) |
| CentOS | 7.5 | İşletim sistemi ve veri diski |
| CentOS | 7.4 | İşletim sistemi ve veri diski |
| CentOS | 7.3 | İşletim sistemi ve veri diski |
| CentOS | 7.2n | İşletim sistemi ve veri diski |
| CentOS | 6.8 | Veri diski |
| openSUSE | 42.3 | Veri diski |
| SLES | 12-SP4 | Veri diski |
| SLES | 12-SP3 | Veri diski |

> [!NOTE]
> Yeni ADE uygulamayı RHEL işletim sistemi ve veri diski RHEL7 Kullandıkça Öde görüntüleri için desteklenir. ADE RHEL Getir Your-kendi-abonelik (BYOS) görüntüler için şu anda desteklenmiyor. Bkz: [Linux için Azure Disk şifrelemesi](azure-security-disk-encryption-linux.md) daha fazla bilgi için.

## <a name="how-can-i-start-using-azure-disk-encryption"></a>Azure Disk şifrelemesi kullanılarak nasıl başlayabilirsiniz?

Başlamak için okuma [Azure Disk Şifrelemesi'ne genel bakış](azure-security-disk-encryption-overview.md).

## <a name="can-i-encrypt-both-boot-and-data-volumes-with-azure-disk-encryption"></a>Ben Azure Disk şifrelemesi ile hem önyükleme hem de veri birimleri şifreleyebilir mi?

Evet, Windows ve Linux Iaas Vm'leri için önyükleme ve veri birimlerini şifreleyebilirsiniz. Windows VM'ler için işletim sistemi birimi şifrelemeden verileri şifreleyemez. Linux VM'ler için işletim sistemi birimi ilk şifrelemek gerek kalmadan veri hacmi şifrelemek mümkündür. Linux için işletim sistemi birimi şifreli sonra bir işletim sistemi birimi Linux Iaas Vm'leri için şifreleme devre dışı bırakma desteklenmiyor. Bir ölçek kümesindeki Linux VM'ler için yalnızca veri hacmi şifrelenebilir.

## <a name="can-i-encrypt-an-unmounted-volume-with-azure-disk-encryption"></a>Ben Azure Disk şifrelemesi ile kaldırılan bir birim şifreleyebilir mi?

Hayır, Azure Disk şifrelemesi yalnızca takılan birimler şifreler.

## <a name="how-do-i-rotate-secrets-or-encryption-keys"></a>Nasıl miyim gizli dizileri veya şifreleme anahtarlarını döndürme?

İlk disk şifrelemeyi etkinleştirmek için kullanılan komutla aynı çağrı gizli dizileri döndürmek için farklı bir Key Vault belirtme. Anahtar şifreleme anahtarı döndürmek için ilk olarak disk şifrelemeyi etkinleştirmek için yeni anahtar şifreleme belirtme kullanılan aynı komut çağırın. 

>[!WARNING]
> - Daha önce kullandıysanız [Azure AD uygulaması ile Azure Disk şifrelemesi](azure-security-disk-encryption-prerequisites-aad.md) bu sanal Makineyi şifrelemek için Azure AD kimlik belirterek devam gerekecektir VM'nizi şifrelemek için bu seçeneği kullanın. Kullanamazsınız [Azure Disk şifrelemesi](azure-security-disk-encryption-prerequisites.md) bu desteklenen bir senaryo değildir gibi şifrelenmiş bu VM üzerinde bu VM şifreli için AAD uygulaması uzağa anlamı geçiş henüz desteklenmiyor.

## <a name="how-do-i-add-or-remove-a-key-encryption-key-if-i-didnt-originally-use-one"></a>Nasıl eklerim veya kullanırsanız başlangıçta alamadık, anahtar şifreleme anahtarı Kaldır?

Anahtar şifreleme anahtarı eklemek, yeniden anahtar şifreleme anahtarı parametre geçirerek enable komutunu çağırın. Anahtar şifreleme anahtarı kaldırmak için anahtar şifreleme anahtar parametresi olmadan tekrar enable komutunu çağırın.

## <a name="does-azure-disk-encryption-allow-you-to-bring-your-own-key-byok"></a>Azure Disk şifrelemesi kendi anahtarını getir (BYOK) izin veriyor mu?

Evet, kendi anahtar şifreleme anahtarlarınızı sağlayabilirsiniz. Bu anahtarlar Azure anahtar Kasası'nda, Azure Disk şifrelemesi için anahtar deposu olduğu korunur. Anahtar şifreleme anahtarları hakkında daha fazla bilgi için destek senaryoları, bkz: [Azure Disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md).

## <a name="can-i-use-an-azure-created-key-encryption-key"></a>Azure tarafından oluşturulan anahtar şifreleme anahtarı kullanabilir miyim?

Evet, Azure Key Vault, Azure disk şifrelemesi kullanmak için bir anahtar şifreleme anahtarı oluşturmak için kullanabilirsiniz. Bu anahtarlar Azure anahtar Kasası'nda, Azure Disk şifrelemesi için anahtar deposu olduğu korunur. Anahtar şifreleme anahtarı hakkında daha fazla bilgi için bkz. [Azure Disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md).

## <a name="can-i-use-an-on-premises-key-management-service-or-hsm-to-safeguard-the-encryption-keys"></a>Şifreleme anahtarlarını korumak için bir şirket içi anahtar yönetimi hizmeti veya HSM kullanabilir miyim?

Azure Disk şifreleme ile şifreleme anahtarlarını korumak için şirket içi anahtar yönetimi hizmeti veya HSM kullanamazsınız. Azure Key Vault hizmeti, şifreleme anahtarlarını korumak için yalnızca kullanabilirsiniz. Anahtar şifreleme anahtarı destek senaryoları hakkında daha fazla bilgi için bkz. [Azure Disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md).

## <a name="what-are-the-prerequisites-to-configure-azure-disk-encryption"></a>Azure Disk şifrelemesini yapılandırmak için Önkoşullar nelerdir?

Azure Disk şifrelemesi önkoşulları vardır. Bkz: [Azure Disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md) makale yeni bir anahtar kasası oluşturma veya var olan bir key vault, şifreleme ve koruma gizli dizileri ve anahtarları etkinleştirmek disk şifreleme erişimi için ayarlayın. Anahtar şifreleme anahtarı destek senaryoları hakkında daha fazla bilgi için bkz. [Azure Disk Şifrelemesi'ne genel bakış](azure-security-disk-encryption-overview.md).

## <a name="what-are-the-prerequisites-to-configure-azure-disk-encryption-with-an-azure-ad-app-previous-release"></a>Bir Azure AD uygulamasını (önceki sürüm) ile Azure Disk şifrelemesini yapılandırmak için Önkoşullar nelerdir?

Azure Disk şifrelemesi önkoşulları vardır. Bkz: [Azure Disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites-aad.md) makale bir Azure Active Directory uygulaması oluşturma, yeni bir anahtar kasası oluşturun veya var olan bir anahtar kasası için şifrelemeyi etkinleştirmek disk şifreleme erişimi ayarlama ve gizli anahtarları koruyun ve anahtarlar. Anahtar şifreleme anahtarı destek senaryoları hakkında daha fazla bilgi için bkz. [Azure Disk Şifrelemesi'ne genel bakış](azure-security-disk-encryption-overview.md).

## <a name="is-azure-disk-encryption-using-an-azure-ad-app-previous-release-still-supported"></a>Azure Disk şifrelemesi, yine de desteklenen bir Azure AD uygulamasını (önceki sürüm) kullanıyor mu?
Evet. Disk şifrelemesi kullanarak bir Azure AD uygulamasını hala desteklenmektedir. Ancak, yeni VM'ler şifrelerken, şifreleme ile bir Azure AD uygulama yerine yeni bir yöntem kullanmak önerilir. 

## <a name="can-i-migrate-vms-that-were-encrypted-with-an-azure-ad-app-to-encryption-without-an-azure-ad-app"></a>Şifreleme olmadan bir Azure AD uygulaması için bir Azure AD uygulama ile şifrelenmiş VM'ler geçişini sağlayabilir miyim?
  Şu anda, şifreleme olmadan bir Azure AD uygulaması için bir Azure AD uygulama ile şifrelenmiş olan makineler için bir doğrudan geçiş yolu yoktur. Ayrıca, bir AD uygulamasını şifrelemeyle şifreleme olmadan bir Azure AD uygulaması'ndan doğrudan bir yolu yoktur. 

## <a name="what-version-of-azure-powershell-does-azure-disk-encryption-support"></a>Azure PowerShell'in hangi sürümünü Azure Disk şifrelemesi destekliyor mu?

Azure Disk şifrelemesini yapılandırmak için en son Azure PowerShell SDK'sı sürümünü kullanın. En son sürümünü indirin [Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Azure Disk şifrelemesi *değil* Azure SDK sürüm 1.1.0 tarafından desteklenir.

> [!NOTE]
> Linux Azure disk şifrelemesi Önizleme uzantısı "Microsoft.OSTCExtension.AzureDiskEncryptionForLinux" kullanım dışı bırakılmıştır. Bu uzantı, Azure disk şifrelemesi Önizleme sürümü için yayımlanmıştır. Uzantı önizleme sürümünü test veya üretim dağıtımınızda kullanmamalısınız.

> Dağıtım senaryoları gibi Azure Kaynak Yöneticisi'ni (Linux Iaas VM'nizi şifrelemesini etkinleştirmek Linux VM için Azure disk şifrelemesi uzantısını dağıtmak için ihtiyaç sahip olduğu, ARM), Azure disk şifrelemesi desteklenen üretim uzantısını kullanmanız gerekir" Microsoft.Azure.Security.AzureDiskEncryptionForLinux".

## <a name="can-i-apply-azure-disk-encryption-on-my-custom-linux-image"></a>Azure Disk şifrelemesi my özel bir Linux görüntüsü üzerinde uygulayabilir miyim?

Azure Disk şifrelemesi, özel bir Linux görüntüsü üzerinde uygulayamazsınız. Galeri Linux görüntüleri yalnızca daha önce bahsedilen desteklenen dağıtımları için desteklenir. Özel Linux görüntüleri şu anda desteklenmemektedir.

## <a name="can-i-apply-updates-to-a-linux-red-hat-vm-that-uses-the-yum-update"></a>Bir Linux Red Hat yum güncelleştirme kullanan VM için güncelleştirmeleri uygulayabilir miyim?

Evet, Red Hat Linux sanal makinesine yum güncelleştirme gerçekleştirebilirsiniz.  Daha fazla bilgi için [Linux paket Yönetimi güvenlik duvarı arkasındaki](azure-security-disk-encryption-tsg.md#linux-package-management-behind-a-firewall).

## <a name="what-is-the-recommended-azure-disk-encryption-workflow-for-linux"></a>Linux için önerilen Azure disk şifrelemesi iş akışı nedir?

Aşağıdaki iş akışı, Linux üzerinde en iyi sonuçlar için önerilir:
* Gerekli işletim sistemi distro sürümü ve için karşılık gelen değiştirilmemiş stok galeri görüntüsü başlayın
* Şifrelenir, tüm bağlı sürücülerin yedekleyin.  Bir hata varsa, örneğin geri en fazla kurtarma için şifreleme tamamlanmadan önce VM yeniden başlatılırsa, böylece.
* Şifreleme (birkaç saat veya gün bile VM özelliklerine ve bağlı veri diskleri boyutuna bağlı olarak sürebilir)
* Özelleştirebilir ve yazılım görüntüyü gerektiği gibi ekleyin.

Bu iş akışını mümkün değilse, bağlı [depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md) (SSE) platform depolama hesabı katmanı dm-crypt kullanan tam disk şifrelemesi için bir alternatif olabilir.

## <a name="what-is-the-disk-bek-volume-or-mntazurebekdisk"></a>Disk "Bek birimi" veya "/ mnt/azure_bek_disk" nedir?

"Bek" Windows veya "/ mnt/azure_bek_disk" Linux için güvenli bir şekilde şifrelenmiş Azure Iaas Vm'leri için şifreleme anahtarlarını depolayan bir yerel veri hacmi birimdir.
> [!NOTE]
> Silmeyin ve bu diskteki tüm içeriği düzenleyebilir. Iaas VM üzerindeki tüm şifreleme işlemleri için şifreleme anahtar bulunması gerekli olmadığından, diski çıkarın değil.


## <a name="what-encryption-method-does-azure-disk-encryption-use"></a>Azure Disk şifrelemesi hangi şifreleme yöntemini kullanıyor?

Windows üzerinde ADE BitLocker AES256 şifreleme yöntemini kullanır. (Windows Server 2012'den önceki sürümlerinde AES256WithDiffuser). Linux üzerinde ADE aes xts plain64 şifresini çözme varsayılan 256 bit birim ana anahtarıyla kullanır.

## <a name="if-i-use-encryptformatall-and-specify-all-volume-types-will-it-erase-the-data-on-the-data-drives-that-we-already-encrypted"></a>EncryptFormatAll kullanın ve tüm birim türlerini belirtmek, bu verileri zaten şifrelenmiş veri sürücülerinde silecek?
Hayır, veri zaten Azure Disk şifrelemesi kullanılarak şifrelenmiş veri sürücülerden silinmesi gerekmez. Benzer şekilde nasıl EncryptFormatAll işletim sistemi sürücüsünü yeniden şifrele siz, bunu zaten şifrelenmiş veri sürücüsü yeniden şifrele olmaz. Daha fazla bilgi için [EncryptFormatAll ölçütleri](azure-security-disk-encryption-linux.md#bkmk_EFACriteria).        

## <a name="is-xfs-filesystem-supported"></a>XFS dosya sistemi destekleniyor mu?
Veri disk şifrelemesi yalnızca EncryptFormatAll XFS birimleri desteklenir. Bu, tüm veriler var daha önce silme birimin biçimlendirilmektedir. Daha fazla bilgi için [EncryptFormatAll ölçütleri](azure-security-disk-encryption-linux.md#bkmk_EFACriteria).

## <a name="can-i-backup-and-restore-an-encrypted-vm"></a>Yedekleme ve miyim şifrelenmiş bir sanal Makineyi geri yükleme mi? 

Azure Backup, yedekleme ve geri yükleme şifrelenmiş sanal makinenin içinde aynı abonelik ve bölge için bir mekanizma sağlar.  Yönergeler için lütfen bkz [yedekleme ve Azure Backup ile şifrelenmiş sanal makineleri geri yükleme](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).  Farklı bir bölgeye şifrelenmiş bir sanal Makineyi geri yükleme şu anda desteklenmiyor.  

## <a name="where-can-i-go-to-ask-questions-or-provide-feedback"></a>Soru sormak veya geri bildirim sağlamak için nereye miyim?

Soru sormak veya geri bildirim sağlayın [Azure Disk şifrelemesi Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDiskEncryption).

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Disk şifrelemesi için en sık kullanılan sorular hakkında daha fazla öğrendiniz. Bu hizmet hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Disk Şifrelemesi'ne genel bakış](azure-security-disk-encryption-overview.md)
- [Güvenlik Merkezi'nde Azure disk şifrelemesi Uygula](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Azure veri bekleme sırasında şifreleme](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
