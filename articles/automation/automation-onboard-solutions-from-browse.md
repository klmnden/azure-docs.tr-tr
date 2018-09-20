---
title: Bilgi Azure automation'da birden çok VM için yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümlerin nasıl
description: Ekleme için bir Azure sanal makine nasıl Azure Otomasyonu parçası olan güncelleştirme yönetimi, değişiklik izleme ve stok çözümleriyle öğrenin
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: c326232e0fc8f5f878241186eac8ae5ed23f0958
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46366769"
---
# <a name="enable-update-management-change-tracking-and-inventory-solutions-on-multiple-vms"></a>Güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri birden çok VM'de etkinleştir

Azure Otomasyonu, stok bilgisayarlarınızda yüklü işletim sistemi güvenlik güncelleştirmelerini yönetmek ve değişiklikleri izlemek için çözümler sağlar. Makine birden çok yolu vardır, bu çözüme ekleyebilir [bir sanal makineden](automation-onboard-solutions-from-vm.md), öğesinden, [Otomasyon hesabı](automation-onboard-solutions-from-automation-account.md), sanal makineler göz atarken veya tarafından [runbook](automation-onboard-solutions.md). Bu makalede onboarding Azure sanal makineler'de göz atarken bu çözümler ele alınmaktadır.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure’da oturum açın

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Azure portalında gidin **sanal makineler**.

Onay kutularını kullanarak, değişiklik izleme ve stok veya güncelleştirme yönetimi ile eklemek istediğiniz sanal makineleri seçin. Onboarding aynı anda Üçe kadar farklı kaynak grupları için kullanılabilir.

![VM'lerin listesini](media/automation-onboard-solutions-from-browse/vmlist.png)
> [!TIP]
> Ardından listeden tüm sanal makineler seçmek için üstteki onay kutusuna tıklayın ve sanal makinelerin listesini değiştirmek için filtre denetimleri kullanın.

Komut çubuğundaki **Hizmetleri** seçin ya da **değişiklik izleme**, **Envanter**, veya **güncelleştirme yönetimi**.

> [!NOTE]
> **Değişiklik izleme** ve **Envanter** biri etkinleştirildiğinde, aynı çözümü kullanın diğeri de etkinleştirilir.

Güncelleştirme yönetimi için görüntüsüdür. Değişiklik izleme ve stok aynı düzeni ve davranışı vardır.

Sanal makinelerin listesini, yalnızca aynı abonelik ve konumda bulunan sanal makineler gösterecek şekilde filtrelenir. Üçten fazla kaynak gruplarındaki sanal makineleriniz varsa, ilk üç kaynak grupları seçilidir.

### <a name="resource-group-limit"></a> Onboarding sınırlamaları

Onboarding için kullanabileceğiniz kaynak grubu sayısı tarafından sınırlı [Resource Manager dağıtım sınırları](../azure-resource-manager/resource-manager-cross-resource-group-deployment.md). Güncelleştirme dağıtımları ile karıştırılmamalıdır resource Manager dağıtımları, dağıtım başına 5 kaynak grupları sınırlıdır. Onboarding bütünlüğünü sağlamak için Log Analytics çalışma alanı, Otomasyon hesabı ve ilişkili kaynakları yapılandırmak için bu kaynak grubunun 2 ayrılmıştır. Bu, dağıtım için seçilecek 3 kaynak gruplarıyla bırakır.

Sanal makine, farklı Aboneliklerde, konum ve kaynak grupları seçmek için filtre denetimleri kullanın.

![Güncelleştirme yönetimi çözümü ekleme](media/automation-onboard-solutions-from-browse/onboardsolutions.png)

Log analytics çalışma alanı ve Otomasyon hesabı seçimleri gözden geçirin. Yeni çalışma alanı ve Otomasyon hesabı, varsayılan olarak seçilir. Bir mevcut Log Analytics çalışma alanını ve Otomasyon hesabı varsa, istediğiniz'a tıklayın **değiştirme** bunları seçilecek **yapılandırma** sayfası. İşiniz bittiğinde **Kaydet**’e tıklayın.

![Çalışma alanı ve hesabı seçin](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

Etkinleştirmek için istemediğiniz herhangi bir sanal makineyi yanındaki onay işaretini kaldırın. Zaten seçili sanal makinelerin etkinleştirilemez.

Tıklayın **etkinleştirme** çözümü etkinleştirmek için. Çözümün etkinleştirilmesi 15 dakika sürer.

## <a name="troubleshooting"></a>Sorun giderme

Zaman ekleme birden çok makine gösterme makineler olabilir **etkinleştirilemiyor**. Neden bazı makineler değil etkinleştirilebilir farklı neden vardır. Aşağıdaki bölümlerde göstermek için olası nedenler **etkinleştirilemiyor** ekleme çalışırken, bir VM'de durum.

### <a name="vm-reports-to-a-different-workspace-workspacename--change-configuration-to-use-it-for-enabling"></a>Sanal makine farklı bir çalışma alanına Rapor: '\<workspaceName\>'.  Etkinleştirmek için kullanılacak yapılandırmasını değiştirme

**Neden**: Bu hata, başka bir çalışma alanı için yerleşik raporları çalıştığınız VM gösterir.

**Çözüm**: tıklayın **yapılandırma olarak** hedeflenen Otomasyon hesabının ve Log Analytics çalışma alanını değiştirmek için.

### <a name="vm-reports-to-a-workspace-that-is-not-available-in-this-subscription"></a>Bu abonelikte kullanılabilir olmayan bir çalışma alanına VM raporları

**Neden**: sanal makine için raporlar çalışma alanı:

* Farklı bir abonelikte veya
* Artık yok, veya
* İçin erişim izinlerine sahip değilseniz bir kaynak grubunda bulunuyor

**Çözüm**: Otomasyon hesabı için VM raporları çalışma alanıyla ilişkili ve yerleşik sanal makine kapsam yapılandırması değiştirerek bulun.

### <a name="vm-operating-system-version-or-distribution-is-not-supported"></a>VM işletim sistemi sürümü veya dağıtım desteklenmiyor

**Neden:** çözümü tüm Linux dağıtımları veya tüm Windows sürümleri için desteklenmiyor.

**Çözüm:** başvurmak [desteklenen istemciler listesi](automation-update-management.md#clients) çözümü.

### <a name="classic-vms-cannot-be-enabled"></a>Klasik Vm'leri etkinleştirilemez

**Neden**: Klasik dağıtım modelini kullanan sanal makineler desteklenmez.

**Çözüm**: resource manager dağıtım modeli için sanal makineyi geçirin. Bunu yapma hakkında bilgi için bkz: [Klasik dağıtım modeli kaynakları geçirme](../virtual-machines/windows/migration-classic-resource-manager-overview.md).

### <a name="vm-is-stopped-deallocated"></a>VM durduruldu. (serbest bırakıldı)

**Neden**: sanal makinenin içinde bir **çalıştıran** durumu.

**Çözüm**: bir çözüm bu VM için bir VM ekleme için çalıştırmalıdır. Tıklayın **VM Başlat** sayfadan ayrılmak gitmeden VM'yi başlatmak için satır içi bağlantı.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makineleriniz için çözüm etkinleştirildikten sonra makineleriniz için güncelleştirme değerlendirmesini görüntüleme hakkında bilgi edinmek için güncelleştirme yönetimi Genel Bakış makalesini ziyaret edin.

> [!div class="nextstepaction"]
> [Güncelleştirme yönetimi - güncelleştirme değerlendirmesini görüntüleme](./automation-update-management.md#viewing-update-assessments)

Çözümler ve bunların nasıl kullanıldığı eğitimler toplama:

* [Öğretici - sanal Makineniz için güncelleştirmeleri yönetme](automation-tutorial-update-management.md)

* [Öğretici - VM üzerinde yazılım tanımlama](automation-tutorial-installed-software.md)

* [Öğretici - bir VM üzerindeki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)