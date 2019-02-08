---
title: Çok sayıda Vm'lerini azure'a geçişini otomatikleştirmek | Microsoft Docs
description: Çok sayıda Azure Site Recovery kullanarak sanal makineleri geçirmek için komut dosyaları kullanmayı açıklar
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: article
ms.date: 02/07/2019
ms.author: snehaa
ms.openlocfilehash: c0fc4fa0bdd58b8ecdf4f26051d60324118c4b21
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55896720"
---
# <a name="scale-migration-of-vms-using-azure-site-recovery"></a>Azure Site Recovery kullanarak bir VM ölçek geçiş

Bu makalede Azure Site RECOVERY'yi kullanarak VM'lerin çok sayıda geçirmek için betikleri kullanma işlemi anlamanıza yardımcı olur. Bu komut dosyalarını, yükleme için kullanılabilir [Azure PowerShell örnekleri](https://github.com/Azure/azure-docs-powershell-samples) github deposu. Betikler, VMware, AWS, GCP Vm'leri ve fiziksel sunucuları Azure'a geçirmek için kullanılabilir. Bu betikler, fiziksel sunucuları olarak sanal makineleri geçiriyorsanız Hyper-V sanal makineleri geçirmek için de kullanabilirsiniz. Azure Site Recovery belgelenen PowerShell betikleri yararlanarak [burada](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell).

## <a name="current-limitations"></a>Geçerli sınırlamalar:
- Komut şu anda yalnızca yönetilmeyen diskler'e geçiş desteği
- Standart diskler yalnızca geçişi destekler
- ' % S'hedef sanal makine, yalnızca birincil NIC için statik IP adresi belirtme desteği
- Azure hibrit avantajı ilgili betikleri almayan girişlerinin portalında çoğaltılan sanal makinenin özelliklerini el ile güncelleştirmeniz gerekir

## <a name="how-does-it-work"></a>Nasıl çalışır?

### <a name="prerequisites"></a>Önkoşullar
Başlamadan önce aşağıdakileri yapmanız gerekir:
- Site Recovery kasası, Azure aboneliğinizde oluşturduğunuzdan emin olun
- Yapılandırma sunucusu ve işlem sunucusu kaynak ortamda yüklü olan ve kasa ortamı bulabilir olduğundan emin olun
- Bir çoğaltma ilkesi oluşturuldu ve yapılandırma sunucusu ile ilişkili emin olun
- (Bu, şirket içi sanal makineleri çoğaltmak için kullanılacak) yapılandırma sunucusu sanal makine yönetici hesabı eklediğinizden emin olun
- Azure'da hedef sorumluluk oluşturduğunuzdan emin olun
    - Hedef Kaynak Grubu
    - Hedef depolama hesabı (ve kaynak grubu)
    - Hedef yük devretme için sanal ağ (ve kaynak grubu)
    - Hedef alt ağ
    - Hedef yük devretme testi için sanal ağ (ve kaynak grubu)
    - Kullanılabilirlik kümesi (gerekirse)
    - Hedef ağ güvenlik grubu ve kaynak grubu
- ' % S'hedef sanal makine özellikleri üzerinde karar verdiğiniz emin olun.
    - Hedef VM adı
    - Hedef VM boyutu azure'da (Azure geçişi değerlendirmesi'ni kullanarak karar)
    - Birincil NIC VM'deki özel IP adresi
- Betiklerin indirme [Azure PowerShell örnekleri](https://github.com/Azure/azure-docs-powershell-samples) github deposu

### <a name="csv-input-file"></a>CSV giriş dosyası
Tüm ön koşullar tamamlandı aldıktan sonra geçirmek istediğiniz her bir kaynak makine için veri içeren bir CSV dosyası oluşturmak gerekir. ' % S'giriş CSV giriş ayrıntılarını içeren bir üst bilgi satırı ve geçirilmesi gereken her makineye ilişkin ayrıntılı bir satır olması gerekir. Tüm betikler, aynı CSV dosyası üzerinde çalışacak şekilde tasarlanmıştır. Örnek bir CSV şablonu scripts klasörü başvuru amacıyla kullanılabilir.

### <a name="script-execution"></a>Betik yürütme
CSV hazır hale geldikten sonra şirket içi sanal makineleri geçirmek için aşağıdaki adımları yürütebilirsiniz:

**Adım #** | **Betik adı** | **Açıklama**
--- | --- | ---
1 | asr_startmigration.ps1 | Tüm sanal makineler için çoğaltmayı etkinleştirme csv dosyasında listelenen, betik iş ayrıntılarını CSV çıkışında, her VM için oluşturur.
2 | asr_replicationstatus.ps1 | Çoğaltma durumunu, betik durumu ile bir csv, her VM için oluşturur.
3 | asr_updateproperties.ps1 | Çoğaltılan ve korumalı Vm'leri olduktan sonra VM (işlem ve ağ özellikleri) hedef özelliklerini güncelleştirmek için bu betiği kullanın
4 | asr_propertiescheck.ps1 | Özellikleri uygun şekilde güncelleştirilir olmadığını doğrulayın
5 | asr_testmigration.ps1 |  Csv dosyasında listelenen sanal makinelerinin yük devretme testi başlatmak için betik iş ayrıntılarını CSV çıkışında, her VM için oluşturur.
6 | asr_cleanuptestmigration.ps1 | El ile doğrulama sonra başarısız olan Vm'leri test, test yük devretmesi Vm'leri temizlemek için bu betiği kullanın
7 | asr_migration.ps1 | Csv dosyasında listelenen VM'ler planlanmamış yük devretme gerçekleştirmek, betiği bir CSV çıkışında iş ayrıntılarını ile her VM için oluşturur. Betik mu kapatma şirket içi Vm'leri uygulama tutarlılığı için yük devretmeyi tetiklemeden önce tavsiye edilir betiği çalıştırmadan önce Vm'lerini el ile kapatın.
8 | asr_completemigration.ps1 | Vm'lerde yürütme işlemini gerçekleştirmek ve ASR varlıklarını silme
9 | asr_postmigration.ps1 | NIC yük devretmesi için ağ güvenlik grupları atamayı düşünüyorsanız, bunu yapmak için bu betiği kullanabilirsiniz. ' % S'hedef sanal makine olarak herhangi bir NIC bir NSG atar.

## <a name="next-steps"></a>Sonraki Adımlar

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) Azure Site Recovery kullanarak azure'a geçirme hakkında sunucuları
