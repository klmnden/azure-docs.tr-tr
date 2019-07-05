---
title: Bilgi Azure automation'da birden çok VM için yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümlerin nasıl
description: Ekleme için bir Azure sanal makine nasıl Azure Otomasyonu parçası olan güncelleştirme yönetimi, değişiklik izleme ve stok çözümleriyle öğrenin
services: automation
ms.service: automation
author: bobbytreed
ms.author: robreed
ms.date: 04/11/2019
ms.topic: article
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 11dda62a7d8a92b17eb1d431e61086680f356195
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476614"
---
# <a name="enable-update-management-change-tracking-and-inventory-solutions-on-multiple-vms"></a>Güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri birden çok VM'de etkinleştir

Azure Otomasyonu, stok bilgisayarlarınızda yüklü işletim sistemi güvenlik güncelleştirmelerini yönetmek ve değişiklikleri izlemek için çözümler sağlar. Makine birden çok yolu vardır, bu çözüme ekleyebilir [bir sanal makineden](automation-onboard-solutions-from-vm.md), öğesinden, [Otomasyon hesabı](automation-onboard-solutions-from-automation-account.md), sanal makineler göz atarken veya tarafından [runbook](automation-onboard-solutions.md). Bu makalede onboarding Azure sanal makineler'de göz atarken bu çözümler ele alınmaktadır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure'da oturum açın

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Azure portalında gidin **sanal makineler**.

Onay kutularını kullanarak, değişiklik izleme ve stok veya güncelleştirme yönetimi ile eklemek istediğiniz sanal makineleri seçin. Onboarding aynı anda Üçe kadar farklı kaynak grupları için kullanılabilir. Otomasyon hesabınızın konum ne olursa olsun tüm bölgelerdeki Azure Vm'leri bulunabilir.

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

Log Analytics çalışma alanını ve Otomasyon hesabı seçenekleri gözden geçirin. Bir mevcut bir çalışma alanını ve Otomasyon hesabı, varsayılan olarak seçilir. Farklı bir Log Analytics çalışma alanı ve Otomasyon hesabı kullanmak istiyorsanız, tıklayın **özel** bunları seçilecek **özel yapılandırma** sayfası. Bir Log Analytics çalışma alanı seçtiğinizde, bir Otomasyon hesabıyla bağlantılı olduğunu belirlemek için bir onay yapılır. Bağlantılı bir Otomasyon hesabının bulunursa, aşağıdaki ekranı görürsünüz. İşiniz bittiğinde, tıklayın **Tamam**.

![Çalışma alanı ve hesabı seçin](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

Seçili çalışma alanı bir Otomasyon hesabına bağlı değilse, aşağıdaki ekranı görürsünüz. Bir Otomasyon hesabı seçin ve tıklayın **Tamam** tamamlandığında.

![Çalışma alanı](media/automation-onboard-solutions-from-browse/no-workspace.png)

> [!NOTE]
> Çözümleri etkinleştirirken Log Analytics çalışma alanı ile Otomasyon Hesabı arasında bağlantı kurma seçeneği yalnızca belirli bölgelerde desteklenmektedir.
>
> Desteklenen eşleme çiftlerine bir listesi için bkz. [Otomasyon hesabının ve Log Analytics çalışma alanı için bölge eşleme](how-to/region-mappings.md).

Etkinleştirmek için istemediğiniz herhangi bir sanal makineyi yanındaki onay işaretini kaldırın. Zaten seçili sanal makinelerin etkinleştirilemez.

Tıklayın **etkinleştirme** çözümü etkinleştirmek için. Çözümün etkinleştirilmesi 15 dakika sürer.

## <a name="unlink-workspace"></a>Çalışma alanının bağlantısını Kaldır

Aşağıdaki çözümlerden bir Log Analytics çalışma alanına bağlıdır:

* [Güncelleştirme yönetimi](automation-update-management.md)
* [Değişiklik İzleme](automation-change-tracking.md)
* [Vm'leri çalışma saatleri dışında başlatma/durdurma](automation-solution-vm-management.md)

Bir Log Analytics çalışma alanı ile Otomasyon hesabınızı tümleştirmek istediğiniz karar verirseniz, doğrudan Azure portalından hesabınızı kesebilir. Devam etmeden önce öncelikle daha önce bahsedilen çözümleri kaldırmanız gerekir, bu işlem devam etmeden gelen Aksi takdirde engellenir. Kaldırmak için gerekli adımları anlamak için alınan belirli çözüm makalesini gözden geçirin.

Bu çözümleri kaldırdıktan sonra Otomasyon hesabının bağlantısını kaldırmak için aşağıdaki adımları gerçekleştirebilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri dahil olmak üzere bazı çözümler Otomasyon varlıklarından oluşturmuş olabilir ve ayrıca çalışma alanının bağlantısı kaldırılıyor önce kaldırılması gerekebilir.

1. Azure portalından Otomasyon hesabınızı açın ve sayfa seçin üzerinde Otomasyon hesabı **bağlantılı çalışma** bölümünde **ilgili kaynakları** soldaki.

2. Bağlantıyı kaldır çalışma sayfasında tıklayın **çalışma alanının bağlantısını Kaldır**.

   ![Çalışma sayfası bağlantısını Kaldır](media/automation-onboard-solutions-from-browse/automation-unlink-workspace-blade.png).

   Devam etmek istediğinizi doğrulayan bir ileti alacaksınız.

3. Azure Otomasyonu hesabı Log Analytics çalışma alanınızın bağlantısını dener, ancak altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde.

İsteğe bağlı olarak, güncelleştirme yönetimi çözümünü kullandıysanız, çözümü kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Güncelleştirme zamanlamaları - her oluşturduğunuz güncelleştirme dağıtımlarının eşleşen adlara sahip)

* Çözüm için - oluşturulan karma çalışan grupları her benzer şekilde machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8 için olarak adlandırılır).

İsteğe bağlı olarak, yoğun olmayan saatlerde çözüm sırasında Vm'leri başlatma/durdurma kullandıysanız, çözümü kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Başlatma ve durdurma VM runbook zamanlama
* VM runbook'ları durdurun ve başlatın
* Değişkenler

Alternatif olarak, ayrıca çalışma alanınızı Otomasyon hesabınızdan Log Analytics çalışma alanınızdan kesebilir. Çalışma alanınızda seçin **Otomasyon hesabı** altında **ilgili kaynakları**. Otomasyon hesabı sayfasında **hesabı bağlantısını**.

## <a name="troubleshooting"></a>Sorun giderme

Zaman ekleme birden çok makine gösterme makineler olabilir **etkinleştirilemiyor**. Neden bazı makineler değil etkinleştirilebilir farklı neden vardır. Aşağıdaki bölümlerde göstermek için olası nedenler **etkinleştirilemiyor** ekleme çalışırken, bir VM'de durum.

### <a name="vm-reports-to-a-different-workspace-workspacename--change-configuration-to-use-it-for-enabling"></a>Sanal makine farklı bir çalışma alanına Rapor: '\<workspaceName\>'.  Etkinleştirmek için kullanılacak yapılandırmasını değiştirme

**Neden**: Bu hata, başka bir çalışma alanı için yerleşik raporları çalıştığınız VM gösterir.

**Çözüm**: Tıklayın **yapılandırma olarak** hedeflenen Otomasyon hesabının ve Log Analytics çalışma alanını değiştirmek için.

### <a name="vm-reports-to-a-workspace-that-is-not-available-in-this-subscription"></a>Bu abonelikte kullanılabilir olmayan bir çalışma alanına VM raporları

**Neden**: Sanal makine için raporlar çalışma alanı:

* Farklı bir abonelikte veya
* Artık yok, veya
* İçin erişim izinlerine sahip değilseniz bir kaynak grubunda bulunuyor

**Çözüm**: Otomasyon hesabı için VM raporları çalışma alanıyla ilişkili ve yerleşik sanal makine kapsam yapılandırması değiştirerek bulun.

### <a name="vm-operating-system-version-or-distribution-is-not-supported"></a>VM işletim sistemi sürümü veya dağıtım desteklenmiyor

**Neden:** Çözüm, tüm Linux dağıtımları veya tüm Windows sürümleri için desteklenmez.

**Çözüm:** Başvurmak [desteklenen istemciler listesi](automation-update-management.md#clients) çözümü.

### <a name="classic-vms-cannot-be-enabled"></a>Klasik Vm'leri etkinleştirilemez

**Neden**: Klasik dağıtım modelini kullanan sanal makineler desteklenmez.

**Çözüm**: Sanal makine, Resource Manager dağıtım modeline geçirilir. Bunu yapma hakkında bilgi için bkz: [Klasik dağıtım modeli kaynakları geçirme](../virtual-machines/windows/migration-classic-resource-manager-overview.md).

### <a name="vm-is-stopped-deallocated"></a>VM durduruldu. (serbest bırakıldı)

**Neden**: Sanal makinede değil, bir **çalıştıran** durumu.

**Çözüm**: İçin yerleşik bir çözüm bu VM için bir VM çalıştırılması gerekir. Tıklayın **VM Başlat** sayfadan ayrılmak gitmeden VM'yi başlatmak için satır içi bağlantı.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makineleriniz için çözüm etkinleştirildikten sonra makineleriniz için güncelleştirme değerlendirmesini görüntüleme hakkında bilgi edinmek için güncelleştirme yönetimi Genel Bakış makalesini ziyaret edin.

> [!div class="nextstepaction"]
> [Güncelleştirme yönetimi - güncelleştirme değerlendirmesini görüntüleme](./automation-update-management.md#viewing-update-assessments)

Çözümler ve bunların nasıl kullanıldığı eğitimler toplama:

* [Öğretici - sanal Makineniz için güncelleştirmeleri yönetme](automation-tutorial-update-management.md)

* [Öğretici - VM üzerinde yazılım tanımlama](automation-tutorial-installed-software.md)

* [Öğretici - bir VM üzerindeki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)
