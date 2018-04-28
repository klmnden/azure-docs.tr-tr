---
title: Azure Disk şifrelemesi ile ilgili SSS | Microsoft Docs
description: Bu makale için Microsoft Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler hakkında sık sorulan soruların yanıtlarını sağlar.
services: security
documentationcenter: na
author: DevTiw
manager: avibm
editor: barclayn
ms.assetid: 7188da52-5540-421d-bf45-d124dee74979
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2018
ms.author: barclayn
ms.openlocfilehash: 47ccf91a64653c928cc4da01bc98535c97440d37
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-disk-encryption-faq"></a>Azure Disk şifrelemesi ile ilgili SSS

Bu makale için Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler hakkında sık sorulan sorular (SSS) yanıtlarını sağlar. Bu hizmet hakkında daha fazla bilgi için bkz: [için Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="where-is-azure-disk-encryption-in-general-availability-ga"></a>Azure Disk şifrelemesi genel kullanılabilirlik (GA) nerede?

Azure Disk şifrelemesi for Windows ve Linux Iaas VM'ler olan genel kullanılabilirlik tüm Azure ortak bölgelerde.

## <a name="what-user-experiences-are-available-with-azure-disk-encryption"></a>Hangi kullanıcı deneyimleri Azure Disk şifrelemesi ile kullanılabilir?

Azure Disk şifrelemesi GA Azure Resource Manager şablonları, Azure PowerShell ve Azure CLI destekler. Bu, büyük bir esneklik sağlar. Disk şifrelemesi, Iaas VM'ler için etkinleştirmek için üç farklı seçeneğiniz vardır. Azure Disk şifrelemesi adım adım kılavuzlar ve kullanıcı deneyimi hakkında daha fazla bilgi için bkz: [Azure Disk şifrelemesi dağıtım senaryoları ve deneyimleri](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="how-much-does-azure-disk-encryption-cost"></a>Nasıl Azure Disk şifrelemesi maliyeti nedir?

VM diskleri Azure Disk şifrelemesi ile şifrelemek için herhangi bir ücret alınmaz ancak Azure anahtar kasası kullanımı ile ilişkilendirilen ücretler vardır. Azure anahtar kasası hakkında daha fazla bilgi için maliyetleri başvurmak [anahtar kasası fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/) sayfası.

## <a name="which-virtual-machine-tiers-does-azure-disk-encryption-support"></a>Hangi sanal makine katmanları Azure Disk şifrelemesi destekliyor mu?

Azure Disk şifrelemesi dahil olmak üzere standart katmanı Vm'lerinde kullanılabilir [A, D, DS, G, GS ve F](https://azure.microsoft.com/pricing/details/virtual-machines/) serisi Iaas Vm'leri. Premium depolama ile VM'ler için kullanılabilir. Temel katman VM'ler üzerinde kullanılabilir değil.

## <a name="what-linux-distributions-does-azure-disk-encryption-support"></a>Hangi Linux dağıtımları Azure Disk şifrelemesi destekliyor mu?

Azure Disk şifrelemesi aşağıdaki Linux sunucu dağıtımları ve sürümleri desteklenir:

| Linux dağıtım | Sürüm | Şifreleme için desteklenen birim türü|
| --- | --- |--- |
| Ubuntu | 16.04 GÜNLÜK LTS | İşletim sistemi ve veri diski |
| Ubuntu | 14.04.5-DAILY-LTS | İşletim sistemi ve veri diski |
| RHEL | 7.4 | Veri diski * |
| RHEL | 7.3 | Veri diski * |
| RHEL | 7.2 | Veri diski * |
| RHEL | 6.8 | Veri diski * |
| RHEL | 6.7 | Veri diski * |
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
| SLES | Öncelik: 12-SP1 | Veri diski |
| SLES | HPC 12 | Veri diski |
| SLES | Öncelik: 11-SP4 | Veri diski |
| SLES | 11 SP4 | Veri diski |

*__ADE RHEL için veri diski için desteklenir. Geçerli ADE uygulaması için işletim sistemi diski çalışır ancak şu anda ortaklaşa desteklenmiyor. Hem Microsoft hem de Red Hat ortaklaşa desteklenen bir çözüm üzerinde çalışmaktadır. Bu arada, Linux işletim sistemi disk şifrelemesi ADE Teknik Başvurusu yapabilir [burada](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).__

## <a name="how-can-i-start-using-azure-disk-encryption"></a>Azure Disk Şifrelemesi'ni kullanarak nasıl başlayabilir miyim?

Başlamak için okuma [için Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) teknik incelemesi.

## <a name="can-i-encrypt-both-boot-and-data-volumes-with-azure-disk-encryption"></a>Azure Disk şifrelemesi ile hem önyükleme hem de veri birimleri şifreleyebilir mi?

Evet, Windows ve Linux Iaas VM'ler için önyükleme ve veri birimlerini şifreleyebilirsiniz. Windows VM'ler için işletim sistemi birimi şifrelemeden veriler şifrelenemedi. Linux VM'ler için işletim sistemi birimi ilk şifrelemek gerek kalmadan veri hacmi şifreleyebilirsiniz. Linux için işletim sistemi birimi şifrelenmiş sonra bir işletim sistemi biriminde Linux Iaas VM'ler için şifrelemeyi devre dışı bırakmak desteklenmiyor.

## <a name="does-azure-disk-encryption-allow-you-to-bring-your-own-key-byok-capability"></a>Azure Disk şifrelemesi kendi anahtarı (BYOK) yeteneği Getir izin veriyor mu?

Evet, kendi anahtar şifreleme anahtarlarını da sağlayabilir. Bu anahtarları Azure anahtar Kasası ' Azure Disk şifrelemesi için anahtar deposu olduğu korunur. Anahtar şifreleme anahtarları hakkında daha fazla bilgi için destek senaryoları, bkz: [Azure Disk şifrelemesi dağıtım senaryoları ve deneyimleri](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="can-i-use-an-azure-created-key-encryption-key"></a>Oluşturulan Azure anahtar şifreleme anahtarı kullanabilir miyim?

Evet, Azure disk şifrelemesi kullanmak için bir anahtar şifreleme anahtarı oluşturmak için Azure anahtar kasası kullanabilirsiniz. Bu anahtarları Azure anahtar Kasası ' Azure Disk şifrelemesi için anahtar deposu olduğu korunur. Anahtar şifreleme anahtar destek senaryoları hakkında daha fazla bilgi için bkz: [Azure Disk şifrelemesi dağıtım senaryoları ve deneyimleri](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="can-i-use-an-on-premises-key-management-service-or-hsm-to-safeguard-the-encryption-keys"></a>Şifreleme anahtarları korumak için bir şirket içi anahtar yönetimi hizmeti veya HSM kullanabilir miyim?

Azure Disk şifrelemesi ile şifreleme anahtarları korumak için şirket içi anahtar yönetimi hizmeti veya HSM kullanamazsınız. Azure anahtar kasası hizmetindeki yalnızca şifreleme anahtarları korumak için de kullanabilirsiniz. Anahtar şifreleme anahtar destek senaryoları hakkında daha fazla bilgi için bkz: [Azure Disk şifrelemesi dağıtım senaryoları ve deneyimleri](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="what-are-the-prerequisites-to-configure-azure-disk-encryption"></a>Azure Disk şifrelemesi yapılandırmak için gereken önkoşullar nelerdir?

Önkoşul bir PowerShell komut dosyası yok. Bu komut dosyası ile Azure Active Directory uygulama oluşturmak, yeni bir anahtar kasası oluşturma veya şifreleme ve koruma parolaları ve anahtarlar etkinleştirmek disk şifreleme erişimi olan bir anahtar kasası ayarlayın. Anahtar şifreleme anahtar destek senaryoları hakkında daha fazla bilgi için bkz: [Azure Disk şifrelemesi önkoşulları ve dağıtım senaryoları ve deneyimleri](azure-security-disk-encryption.md#prerequisites).

## <a name="where-can-i-get-more-information-on-how-to-use-powershell-for-configuring-azure-disk-encryption"></a>Azure Disk şifrelemesi yapılandırmak için PowerShell kullanma hakkında daha fazla bilgi nereden alabilirim?

Nasıl daha Gelişmiş senaryolar yanı sıra temel Azure Disk şifrelemesi görevleri gerçekleştirebilir üzerinde bazı harika makaleleri sahibiz. Temel görevler için bkz: [keşfedin Azure Disk şifrelemesi Azure PowerShell – bölümü 1 ile](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Daha Gelişmiş senaryolar için bkz: [keşfedin Azure Disk şifrelemesi Azure PowerShell – bölüm 2 ile](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/).

## <a name="what-version-of-azure-powershell-does-azure-disk-encryption-support"></a>Azure PowerShell hangi sürümünün Azure Disk şifrelemesi destekliyor mu?

Azure Disk şifrelemesi yapılandırmak için Azure PowerShell SDK'ın en son sürümünü kullanın. En son sürümünü indirme [Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Azure Disk şifrelemesi *değil* Azure SDK sürüm 1.1.0 tarafından desteklenir.

> [!NOTE]
> Linux Azure disk şifrelemesi Önizleme uzantısını kullanım dışıdır. Ayrıntılar için bkz [onaysız kılınmadan Azure disk şifrelemesi Önizleme uzantısı Linux Iaas VM'ler](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/).

## <a name="can-i-apply-azure-disk-encryption-on-my-custom-linux-image"></a>Azure Disk şifrelemesi my özel Linux görüntüde uygulayabilir mi?

Azure Disk şifrelemesi özel Linux görüntünüzü uygulanamıyor. Daha önce çağrılan desteklenen dağıtımları için yalnızca galeri Linux görüntüleri destekliyoruz. Şu anda özel Linux görüntüleri desteklemiyoruz.

## <a name="can-i-apply-updates-to-a-linux-red-hat-vm-that-uses-the-yum-update"></a>Bir Linux Red Hat yum güncelleştirme kullanan VM güncelleştirmelerini uygulayabilir mi?

Evet, bir güncelleştirme gerçekleştirmek ya da bir Red Hat Linux VM düzeltme eki. Daha fazla bilgi için bkz: [yum Update'i kullanarak güncelleştirmeleri bir şifrelenmiş Azure Iaas Red Hat VM için uygulamaya](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/).

## <a name="what-is-the-recommended-azure-disk-encryption-workflow-for-linux"></a>Linux için önerilen Azure disk şifrelemesi iş akışı nedir?

Aşağıdaki iş akışı en iyi sonuçlar Linux'ta önerilir:
* İstenen işletim sistemi distro ve sürüm karşılık gelen değiştirilmemiş stok galeri görüntüden Başlat
* Şifrelenir tüm bağlı sürücülerin yedekleyin.  Bu örneğin şifreleme tamamlanmadan önce VM yeniden başlatılıncaya kadar hatası durumunda kurtarma izin verir.
* Şifreleme (birden çok saat veya hatta günlerce vm özelliklerini ve eklenen veri disklerini boyutuna bağlı olarak alabilir)
* Özelleştirme ve gerektiğinde görüntüye yazılım ekleyin.

Bu iş akışının mümkün değilse, güvenmek [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) (SSE) adresinde platform depolama hesabı katman alternatif dm-crypt kullanan tam disk şifrelemesi olabilir.

## <a name="what-is-the-disk-bek-volume-or-mntazurebekdisk"></a>Disk "Bek birimi" veya "/ mnt/azure_bek_disk" nedir?

Windows için "Bek birimi" veya "/ mnt/azure_bek_disk" Linux için güvenli bir şekilde şifrelenmiş Azure Iaas VM'ler için şifreleme anahtarlarını depolayan bir yerel veri biriminde değil.
> [!NOTE]
> Silmeyin veya bu diskin tüm içeriğini Düzenle. Şifreleme anahtarı varlığı Iaas VM herhangi bir şifreleme işlemi için gerekli olmadığından disk çıkarın değil.

## <a name="where-can-i-go-to-ask-questions-or-provide-feedback"></a>Soru sorun ya da geribildirim sağlamak için nereye miyim?

Soru sormak veya geri bildirim sağlayan [Azure Disk şifrelemesi Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDiskEncryption).

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Disk şifrelemesi için ilgili en sık kullanılan sorular hakkında daha fazla öğrendiniz. Bu hizmet ve özelliklerini hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Güvenlik Merkezi'nde disk şifrelemesi Uygula](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Azure sanal Makine'yi şifreleme](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Bekleyen Azure verileri şifreleme](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
