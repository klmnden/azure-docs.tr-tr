---
title: Bilgi Azure Automation birden çok VM için yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri hakkında
description: Nasıl için yerleşik bir Azure sanal makine Azure Otomasyonu parçası olan güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri ile bilgi edinin
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 6d0109b9043b48fbbbeeaccea6c798eaa547a056
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839885"
---
# <a name="enable-update-management-change-tracking-and-inventory-solutions-on-multiple-vms"></a>Güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri birden çok VM üzerinde etkinleştirmek

Azure Otomasyonu bilgisayarlarınızda yüklü stok işletim sistemi güvenlik güncelleştirmeleri yönetmek ve değişiklikleri izlemek için çözüm sağlar. Yerleşik makinelere birden çok yolu vardır, çözüm gerçekleştirebilir [bir sanal makineden](automation-onboard-solutions-from-vm.md), gelen, [Otomasyon hesabı](automation-onboard-solutions-from-automation-account.md), sanal makinelerin göz atarken veya tarafından [runbook](automation-onboard-solutions.md). Bu makalede onboarding bu çözümleri Azure sanal makinelerde göz atarken kapsar.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure’da oturum açın

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Azure portalında gidin **sanal makineleri**.

Onay kutularını kullanarak, değişiklik izleme ve envanter veya güncelleştirme yönetimi yerleşik istediğiniz sanal makineleri seçin. Onboarding aynı anda en çok üç farklı kaynak grupları için kullanılabilir.

![Sanal makineleri listesi](media/automation-onboard-solutions-from-browse/vmlist.png)
> [!TIP]
> Tüm sanal makineler listesinden seçmek için üst onay kutusuna tıklayın ve sanal makinelerin listesini değiştirmek için filtre denetimleri kullanın.

Komut çubuğundaki **Hizmetleri** seçin **değişiklik izleme**, **stok**, veya **güncelleştirme yönetimi**.

> [!NOTE]
> **Değişiklik izleme** ve **stok** bir etkinleştirildiğinde aynı çözümü kullanan diğer de etkinleştirilir.

Güncelleştirme yönetimi için görüntüdür. Değişiklik izleme ve stok aynı düzeni ve davranışı sahiptir.

Sanal makineler listesi yalnızca aynı abonelik ve konumda olan sanal makineler göstermek için filtrelenir. Sanal makinelerinizi üçten fazla kaynak gruplarında varsa, ilk üç kaynak grupları seçilidir.

Sanal makineler farklı Aboneliklerde, konumları ve kaynak grupları seçmek için filtre denetimleri kullanın.

![Yerleşik güncelleştirme yönetimi çözümü](media/automation-onboard-solutions-from-browse/onboardsolutions.png)

Otomasyon hesabı ve günlük analizi çalışma alanı için seçimleri gözden geçirin. Yeni çalışma ve Automation hesabı varsayılan olarak seçilidir. Mevcut günlük analizi çalışma ve Automation hesabı varsa istediğiniz'nı tıklatın **değiştirme** onlardan seçmek için **yapılandırma** sayfası. İşiniz bittiğinde **Kaydet**’e tıklayın.

![Çalışma alanı ve hesabı seçin](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

Etkinleştirmek istediğiniz olmayan herhangi bir sanal makine yanındaki onay kutusunun seçimini kaldırın. Etkinleştirilemez sanal makineler zaten seçili değilse.

Tıklatın **etkinleştirmek** çözümü etkinleştirilemiyor. Çözümün etkinleştirilmesi 15 dakika sürer.

## <a name="troubleshooting"></a>Sorun giderme

Zaman ekleme birden fazla makine gösterme makineler olabilir **etkinleştiremiyor**. Neden bazı makineler etkinleştirilmemiş farklı nedenleri vardır. Aşağıdaki bölümlerde olası nedenleri Göster **etkinleştiremiyor** için yerleşik çalışırken bir VM'de durum.

### <a name="vm-reports-to-a-different-workspace-workspacename--change-configuration-to-use-it-for-enabling"></a>VM raporları için farklı bir çalışma alanı: '\<workspaceName\>'.  Etkinleştirme için kullanmak üzere yapılandırma değiştirme

**Neden**: Bu hata başka bir çalışma alanına yerleşik raporlar çalıştığınız VM gösterir.

**Çözüm**: satır içi bağlantı veya tıklatın tıklayarak hedeflediğiniz çalışma değiştirmek **değiştirmek**. Her makine için amaçlanan bir çalışma alanı değiştirmek için aşağıdaki komut dosyalarını da kullanabilirsiniz.

### <a name="vm-reports-to-a-workspace-that-is-not-available-in-this-subscription"></a>Bu abonelikte kullanılabilir olmayan bir çalışma alanına VM raporları

**Neden**: sanal makine için raporlar çalışma alanı:

* Farklı bir abonelikte veya
* Artık yok veya
* Erişim izinlerine sahip olmayan bir kaynak grubunda olduğundan

**Çözüm**: Otomasyon hesabı için VM raporlar çalışma alanı ile ilişkili ve yerleşik sanal makine kapsam yapılandırması değiştirerek bulun.

### <a name="vm-operating-system-version-or-distribution-is-not-supported"></a>VM işletim sistemi sürümü veya dağıtım desteklenmiyor

**Neden:** çözümü tüm Linux dağıtımları veya tüm Windows sürümleri için desteklenmiyor.

**Çözüm:** başvurmak [desteklenen istemciler listesi](automation-update-management.md#clients) çözüm için.

### <a name="classic-vms-cannot-be-enabled"></a>Klasik sanal makineleri etkinleştirilemiyor

**Neden**: Klasik dağıtım modeli kullanan sanal makinelerde desteklenmiyor.

**Çözüm**: resource manager dağıtım modeli için sanal makineyi geçirin. Bunun nasıl yapılacağını öğrenmek için bkz: [Klasik dağıtım modeli kaynakları geçirme](../virtual-machines/windows/migration-classic-resource-manager-overview.md).

### <a name="vm-is-stopped-deallocated"></a>VM durduruldu. (serbest bırakıldı)

**Neden**: sanal makineyi değil, bir **çalıştıran** durumu.

**Çözüm**: aşağıdakileri yapmak için yerleşik bir çözüm VM için bir VM çalıştırması gerekir. Tıklatın **VM Başlat** sayfasından ayrılmayın gitmeden VM başlatmak için satır içi bağlantı.

## <a name="next-steps"></a>Sonraki adımlar

Çözüm, sanal makineleriniz için etkinleştirilir, makinelerinizi için güncelleştirme değerlendirmesi görüntüleme hakkında bilgi edinmek için güncelleştirme yönetimi genel bakış makalesine bakın.

> [!div class="nextstepaction"]
> [Güncelleştirme yönetimi - görünüm güncelleştirme değerlendirmesi](./automation-update-management.md#viewing-update-assessments)

Çözümler ve bunları nasıl kullanacağınızı Toplama öğreticiler:

* [Öğretici - VM için güncelleştirmeleri yönetebilirsiniz.](automation-tutorial-update-management.md)

* [Öğretici - VM yazılımı tanımlayın](automation-tutorial-installed-software.md)

* [Öğretici - VM üzerindeki değişiklikler sorun giderme](automation-tutorial-troubleshoot-changes.md)