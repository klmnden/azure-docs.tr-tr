---
title: "Azure Disk şifrelemesi ile ilgili SSS | Microsoft Docs"
description: "Bu makale için Microsoft Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler hakkında sık sorulan soruların yanıtlarını sağlar."
services: security
documentationcenter: na
author: deventiwari
manager: avibm
editor: yuridio
ms.assetid: 7188da52-5540-421d-bf45-d124dee74979
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: devtiw
ms.openlocfilehash: 2ccadfdec0e653264671f5a9a38d4541b0fc4e69
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-disk-encryption-faq"></a>Azure Disk şifrelemesi ile ilgili SSS

Bu makale için Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler hakkında sık sorulan sorular (SSS) yanıtlarını sağlar. Bu hizmet hakkında daha fazla bilgi için bkz: [için Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="general-questions"></a>Genel sorular
**S:** Azure Disk şifrelemesi genel kullanılabilirlik (GA) nerede?

**Y:** için Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler olan genel kullanılabilirlik tüm Azure ortak bölgelerde.

**S:** hangi kullanıcı deneyimleri Azure Disk şifrelemesi ile kullanılabilir?

**Y:** Azure Resource Manager şablonları, Azure PowerShell ve Azure CLI Azure Disk şifrelemesi GA destekler. Bu, büyük bir esneklik sağlar. Disk şifrelemesi, Iaas VM'ler için etkinleştirmek için üç farklı seçeneğiniz vardır. Azure Disk şifrelemesi dağıtım senaryoları ve deneyimleri Azure Disk şifrelemesi adım adım kılavuzlar ve kullanıcı deneyimi hakkında daha fazla bilgi için bkz.

**S:** nasıl Azure Disk şifrelemesi maliyeti?

**Y:** VM diskleri Azure Disk şifrelemesi ile şifrelemek için herhangi bir ücret alınmaz.

**S:** hangi sanal makine katmanları Azure Disk şifrelemesi destekliyor mu?

**Y:** dahil olmak üzere standart katmanı Vm'lerinde Azure Disk şifrelemesi kullanılabilir [A, D, DS, G, GS ve F](https://azure.microsoft.com/pricing/details/virtual-machines/) serisi Iaas Vm'leri. Premium depolama ile VM'ler için kullanılabilir. Temel katman VM'ler üzerinde kullanılabilir değil.

**S:** ne Linux dağıtımları Azure Disk şifrelemesi desteği mu?

**Y:** Azure Disk şifrelemesi aşağıdaki Linux sunucu dağıtımları ve sürümleri desteklenir:

| Linux dağıtım | Sürüm | Şifreleme için desteklenen birim türü|
| --- | --- |--- |
| Ubuntu | 16.04 GÜNLÜK LTS | İşletim sistemi ve veri diski |
| Ubuntu | 14.04.5-DAILY-LTS | İşletim sistemi ve veri diski |
| RHEL | 7.3 | İşletim sistemi ve veri diski |
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
| SLES | Öncelik: 12-SP1 | Veri diski |
| SLES | HPC 12 | Veri diski |
| SLES | Öncelik: 11-SP4 | Veri diski |
| SLES | 11 SP4 | Veri diski |

**S:** nasıl ı başlayabileceğini Azure Disk Şifrelemesi'ni kullanarak?

**Y:** başlamak için okuma [için Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) teknik incelemesi.

**S:** Azure Disk şifrelemesi ile hem önyükleme hem de veri birimleri şifreleyebilir mi?

**Y:** Evet, Windows ve Linux Iaas VM'ler için önyükleme ve veri birimlerini şifreleyebilirsiniz. Windows VM'ler için işletim sistemi birimi şifrelemeden veriler şifrelenemedi. Linux VM'ler için işletim sistemi birimi ilk şifrelemek gerek kalmadan veri hacmi şifreleyebilirsiniz. Linux için işletim sistemi birimi şifrelenmiş sonra bir işletim sistemi biriminde Linux Iaas VM'ler için şifrelemeyi devre dışı bırakmak desteklenmiyor.

**S:** mu Azure Disk şifrelemesi kendi anahtarı (BYOK) yeteneği Getir izin verir mi?

**Y:** Evet, kendi anahtar şifreleme anahtarları sağlayabilir. Bu anahtarları Azure anahtar Kasası ' Azure Disk şifrelemesi için anahtar deposu olduğu korunur. Anahtar şifreleme anahtarları hakkında daha fazla bilgi için senaryoları desteklemek, Azure Disk şifrelemesi dağıtım senaryoları ve deneyimleri bakın.

**S:** oluşturulan Azure anahtar şifreleme anahtarı kullanabilir miyim?

**Y:** Evet, Azure disk şifrelemesi kullanmak için bir anahtar şifreleme anahtarı oluşturmak için Azure anahtar kasası kullanabilirsiniz. Bu anahtarları Azure anahtar Kasası ' Azure Disk şifrelemesi için anahtar deposu olduğu korunur. Azure Disk şifrelemesi dağıtım senaryoları ve deneyimleri anahtar şifreleme anahtar destek senaryoları hakkında daha fazla bilgi için bkz.

**S:** bir şirket içi anahtar yönetimi hizmeti veya HSM şifreleme anahtarları korumak için kullanabilir miyim?

**Y:** Azure Disk şifrelemesi ile şifreleme anahtarları korumak için şirket içi anahtar yönetimi hizmeti veya HSM kullanamazsınız. Azure anahtar kasası hizmetindeki yalnızca şifreleme anahtarları korumak için de kullanabilirsiniz. Azure Disk şifrelemesi dağıtım senaryoları ve deneyimleri anahtar şifreleme anahtar destek senaryoları hakkında daha fazla bilgi için bkz.

**S:** Azure Disk şifrelemesi yapılandırmak için gereken önkoşullar nelerdir?

**Y:** önkoşul bir PowerShell komut dosyası yok. Bu komut dosyası ile Azure Active Directory uygulama oluşturmak, yeni bir anahtar kasası oluşturma veya şifreleme ve koruma parolaları ve anahtarlar etkinleştirmek disk şifreleme erişimi olan bir anahtar kasası ayarlayın. Azure Disk şifrelemesi önkoşulları ve dağıtım senaryoları ve deneyimleri anahtar şifreleme anahtar destek senaryoları hakkında daha fazla bilgi için bkz.

**S:** Azure Disk şifrelemesi yapılandırmak için PowerShell kullanma hakkında daha fazla bilgi nereden alabilirim?

**Y:** nasıl daha Gelişmiş senaryolar yanı sıra temel Azure Disk şifrelemesi görevleri gerçekleştirebilir üzerinde bazı harika makaleleri sunuyoruz. Temel görevler için bkz: [keşfedin Azure Disk şifrelemesi Azure PowerShell – bölümü 1 ile](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Daha Gelişmiş senaryolar için bkz: [keşfedin Azure Disk şifrelemesi Azure PowerShell – bölüm 2 ile](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/).

**S:** Azure PowerShell hangi sürümü Azure Disk şifrelemesi destekliyor mu?

**Y:** Azure Disk şifrelemesi yapılandırmak için Azure PowerShell SDK'ın en son sürümünü kullanın. En son sürümünü indirme [Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Azure Disk şifrelemesi *değil* Azure SDK sürüm 1.1.0 tarafından desteklenir.

> [!NOTE]
> Linux Azure disk şifrelemesi Önizleme uzantısını kullanım dışıdır. Ayrıntılar için bkz [onaysız kılınmadan Azure disk şifrelemesi Önizleme uzantısı Linux Iaas VM'ler](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/).

**S:** Azure Disk şifrelemesi my özel Linux görüntüye uygulayabilirsiniz?

**Y:** özel Linux görüntünüzü Azure Disk şifrelemesi uygulanamıyor. Daha önce çağrılan desteklenen dağıtımları için yalnızca galeri Linux görüntüleri destekliyoruz. Şu anda özel Linux görüntüleri desteklemiyoruz.

**S:** Linux Red Hat yum güncelleştirme kullanan VM için güncelleştirmeler uygulayabilir mi?

**Y:** Evet, bir güncelleştirme gerçekleştirmek veya bir Red Hat Linux VM düzeltme eki. Daha fazla bilgi için bkz: [yum Update'i kullanarak güncelleştirmeleri bir şifrelenmiş Azure Iaas Red Hat VM için uygulamaya](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/).

**S:** Linux için önerilen Azure disk şifrelemesi iş akışı nedir?

**Y:** aşağıdaki iş akışı en iyi sonuçlar Linux'ta önerilir:
* İstenen işletim sistemi distro ve sürüm karşılık gelen değiştirilmemiş stok galeri görüntüden Başlat
* Şifrelenir tüm bağlı sürücülerin yedekleyin.  Bu örneğin şifreleme tamamlanmadan önce VM yeniden başlatılıncaya kadar hatası durumunda kurtarma izin verir.
* Şifreleme (birden çok saat veya hatta günlerce vm özelliklerini ve eklenen veri disklerini boyutuna bağlı olarak alabilir)
* Özelleştirme ve gerektiğinde görüntüye yazılım ekleyin.

Bu iş akışının mümkün değilse, güvenmek [depolama hizmeti şifrelemesi](https://docs.microsoft.com/en-us/azure/storage/common/storage-service-encryption) (SSE) adresinde platform depolama hesabı katman alternatif dm-crypt kullanan tam disk şifrelemesi olabilir.

**S:** disk "Bek birimi" veya "/ mnt/azure_bek_disk" nedir?

**Y:** "Bek" Windows ya da "/ mnt/azure_bek_disk" için Linux için birimdir güvenli bir şekilde şifrelenmiş Azure Iaas VM'ler için şifreleme anahtarlarını depolayan bir yerel veri birimi.
> [!NOTE]
> Silmeyin veya bu diskin tüm içeriğini Düzenle. Şifreleme anahtarı varlığı Iaas VM herhangi bir şifreleme işlemi için gerekli olmadığından disk çıkarın değil.

**S:** miyim nereye soru sorun ya da geribildirim sağlamak için?

**Y:** sorular sormak veya geri bildirim sağlayan [Azure Disk şifrelemesi Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureDiskEncryption).

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Disk şifrelemesi için ilgili en sık kullanılan sorular hakkında daha fazla öğrendiniz. Bu hizmet ve özelliklerini hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Güvenlik Merkezi'nde disk şifrelemesi Uygula](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Azure sanal Makine'yi şifreleme](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Bekleyen Azure verileri şifreleme](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
