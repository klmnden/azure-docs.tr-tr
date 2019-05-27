---
title: Bilgi nasıl Azure Otomasyonu'nda güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri ekleme
description: Ekleme için bir Azure sanal makine nasıl Azure Otomasyonu parçası olan güncelleştirme yönetimi, değişiklik izleme ve stok çözümleriyle öğrenin
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 4/11/2019
ms.topic: conceptual
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: a1bb5534d2f98a4e5143038ab1d5fbbcc76184fe
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66133197"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>Yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri

Azure Otomasyonu, stok bilgisayarlarınızda yüklü işletim sistemi güvenlik güncelleştirmelerini yönetmek ve değişiklikleri izlemek için çözümler sağlar. Makine birçok yolu vardır, bu çözüme ekleyebilir [bir sanal makineden](automation-onboard-solutions-from-vm.md), [birden çok makine gözatma gelen](automation-onboard-solutions-from-browse.md), Otomasyon hesabınızdan veya göre [runbook](automation-onboard-solutions.md). Bu makalede onboarding Bu çözümler Otomasyon hesabınızdan kapsar.

## <a name="sign-in-to-azure"></a>Oturum açın: Azure

https://portal.azure.com adresinden Azure'da oturum açın

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Otomasyon hesabınıza gidin ve şunlardan birini seçin **Envanter** veya **değişiklik izleme** altında **yapılandırma yönetimi**.

Log Analytics çalışma alanını ve Otomasyon hesabı seçin ve tıklayın **etkinleştirme** çözümü etkinleştirmek için. Çözümün etkinleştirilmesi 15 dakika sürer.

![Stok çözümünü ekleme](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

> [!NOTE]
> Çözümleri etkinleştirirken Log Analytics çalışma alanı ile Otomasyon Hesabı arasında bağlantı kurma seçeneği yalnızca belirli bölgelerde desteklenmektedir.
>
> Desteklenen eşleme çiftlerine bir listesi için bkz. [Otomasyon hesabının ve Log Analytics çalışma alanı için bölge eşleme](how-to/region-mappings.md).

Değişiklik İzleme ve Sayım çözümü, sanal makinelerinizde [değişiklikleri izleme](automation-vm-change-tracking.md) ve [sayım](automation-vm-inventory.md) olanağı sağlar. Bu adımda çözümü bir sanal makine üzerinde etkinleştirirsiniz.

Değişiklik izleme ve sayım çözümü ekleme bildirimi tamamlandığında **YAPILANDIRMA YÖNETİMİ** bölümünde **Güncelleştirme Yönetimi**’ne tıklayın.

Güncelleştirme Yönetimi çözümü, Azure Windows VM’leriniz için güncelleştirmeleri ve yamaları yönetmenizi sağlar. Kullanılabilir güncelleştirmelerin durumunu değerlendirebilir, gerekli güncelleştirmelerin yüklemesini zamanlayabilir ve güncelleştirmelerin VM’ye başarıyla uygulandığını doğrulamak için dağıtım sonuçlarını gözden geçirebilirsiniz. Bu eylem çözümü VM'niz için etkin.

Seçin **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**. Seçili Log Analytics çalışma alanı, önceki adımda kullanılan çalışma alanıdır. Güncelleştirme yönetimi çözümünü eklemek için **Etkinleştir**’e tıklayın. Çözümün etkinleştirilmesi 15 dakika sürer.

![Güncelleştirme çözümü](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>Kapsam Yapılandırması

Her bir çözümün kapsam yapılandırması çözümünü edinme bilgisayarları hedeflemek için bir çalışma alanı içinde kullanır. Kapsam yapılandırması belirli bilgisayarlara çözüm kapsamını sınırlamak için kullanılan bir veya daha fazla kayıtlı aramalar grubudur. Kapsam yapılandırmaları, Otomasyon hesabınız kapsamında erişmeye **ilgili kaynakları**seçin **çalışma**. Ardından çalışma alanı altındaki **çalışma alanı veri kaynakları**seçin **kapsam yapılandırmaları**.

Seçilen çalışma alanı güncelleştirme yönetimi veya değişiklik izleme çözümlerini henüz yoksa, aşağıdaki kapsam yapılandırmaları oluşturulur:

* **MicrosoftDefaultScopeConfig değişiklik izleme**

* **MicrosoftDefaultScopeConfig güncelleştirmeleri**

Seçilen çalışma alanında Çözüm zaten varsa, çözümü yeniden dağıtılan değildir ve kapsam yapılandırması için eklenmez.

## <a name="saved-searches"></a>Kayıtlı aramalar

Bir bilgisayarı güncelleştirme yönetimi veya değişiklik izleme ve sayım çözümlerini eklendiğinde, çalışma alanınızdaki iki kayıtlı aramalar birine eklenir. Bu kayıtlı aramalar, bu çözümleri için hedeflenen bilgisayarları içeren sorgular kullanılmaktadır.

Otomasyon hesabınıza gidin ve seçin **kayıtlı aramalar** altında **genel**. Aşağıdaki tabloda bu çözüm tarafından kullanılan iki kayıtlı aramalar görülebilir:

|Ad     |Category  |Diğer Ad  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  Değişiklik izleme       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

Grup doldurmak için kullanılan sorguyu görüntülemek için ya da kayıtlı bir aramayı seçin. Aşağıdaki resimde, sorgu ve sonuçları gösterilmektedir:

![Kayıtlı aramalar](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-azure-vms"></a>Azure VM ekleme

Otomasyon hesabı seçin **Envanter** veya **değişiklik izleme** altında **yapılandırma yönetimi**, veya **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.

Tıklayın **+ Azure VM ekleme**, listeden bir veya daha fazla sanal makine seçin. Sanal makinelerin etkinleştirilemez, out ve seçilmesi için gri. Otomasyon hesabınızın konum ne olursa olsun tüm bölgelerdeki Azure Vm'leri bulunabilir. Üzerinde **güncelleştirme yönetimini etkinleştirme** sayfasında **etkinleştirme**. Bu eylem seçili Vm'lerden bilgisayar grubu kayıtlı araması çözümü ekler.

![Azure sanal makineleri etkinleştirme](media/automation-onboard-solutions-from-automation-account/enable-azure-vms.png)

## <a name="onboard-a-non-azure-machine"></a>Azure olmayan bir makine ekleme

Makineleri Azure el ile eklenmesi gerekir. Otomasyon hesabı seçin **Envanter** veya **değişiklik izleme** altında **yapılandırma yönetimi**, veya **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.

Tıklayın **Ekle Azure olmayan makine**. Bu eylem, yeni bir tarayıcı penceresi ile yukarı açar [yüklemek ve makinede Microsoft Monitoring Agent'ı yapılandırmak yönergeler](../azure-monitor/platform/log-analytics-agent.md) makine çözüme raporlama başlayabilmesi için. Ekleme şu anda yönetilen bir makine System Center Operations Manager tarafından kullanıyorsanız, yeni bir Aracısı gerekli değildir, çalışma alanı bilgileri mevcut aracıyı girilen.

## <a name="onboard-machines-in-the-workspace"></a>Çalışma alanındaki yerleşik makineler

El ile yüklenen makineler veya makineler zaten çalışma alanınıza raporlama etkinleştirilmesi, çözüm için Azure Otomasyonu'için eklenmesi gerekir. Otomasyon hesabı seçin **Envanter** veya **değişiklik izleme** altında **yapılandırma yönetimi**, veya **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.

Seçin **yönetme makineler**. Bu eylem açılan **makinelerini yönetme** sayfası. Bu sayfa, bir makine, tüm kullanılabilir makineleri Grup çözümü etkinleştirin veya tüm geçerli makineler için çözümü etkinleştirme ve tüm gelecek makinelerde etkinleştir sağlar. **Yönetme makineler** düğmesini gri seçenek daha önce seçtiyseniz **tüm mevcut ve gelecekteki makinelerde etkinleştir**.

![Kayıtlı aramalar](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="all-available-machines"></a>Tüm kullanılabilir makineler

Tüm kullanılabilir makineleri çözümü etkinleştirmek için işaretleyin **kullanılabilir tüm makinelerde etkinleştir**. Bu eylem, tek tek makineler eklemek için denetimi devre dışı bırakır. Arama sorgusu kayıtlı bilgisayar grubu için çalışma alanına bildirimde bulunan makineler tüm adları bu görev ekler. Bu eylem, seçili olduğunda, devre dışı bırakır **makinelerini yönetme** düğmesi.

### <a name="all-available-and-future-machines"></a>Tüm mevcut ve gelecekteki makinelerin

Tüm kullanılabilir makinelerde ve gelecekteki makinelerde çözümü etkinleştirmek için seçin **tüm mevcut ve gelecekteki makinelerde etkinleştir**. Bu seçenek kayıtlı aramalar ve kapsam yapılandırmaları çalışma alanından siler. Bu eylem, tüm Azure ve Azure olmayan makineler, çalışma alanına raporlama çözümü açar. Bu eylem, seçili olduğunda, devre dışı bırakır **makinelerini yönetme** sol hiçbir kapsam yapılandırması olduğundan kalıcı olarak düğmesi.

### <a name="selected-machines"></a>Seçilen makineler

Bir veya daha fazla makine için çözümü etkinleştirmek için seçin **seçili makinelerde etkinleştir** tıklatıp **ekleme** çözüme eklemek istediğiniz her makineyi yanında. Bu görev seçili makine adları kaydedilen arama sorgu çözümü bilgisayar grubuna ekler.

## <a name="unlink-workspace"></a>Çalışma alanının bağlantısını Kaldır

Aşağıdaki çözümlerden bir Log Analytics çalışma alanına bağlıdır:

* [Güncelleştirme yönetimi](automation-update-management.md)
* [Değişiklik İzleme](automation-change-tracking.md)
* [Vm'leri çalışma saatleri dışında başlatma/durdurma](automation-solution-vm-management.md)

Bir Log Analytics çalışma alanı ile Otomasyon hesabınızı tümleştirmek istediğiniz karar verirseniz, doğrudan Azure portalından hesabınızı kesebilir.  Devam etmeden önce öncelikle daha önce bahsedilen çözümleri kaldırmanız gerekir, bu işlem devam etmeden gelen Aksi takdirde engellenir. Kaldırmak için gerekli adımları anlamak için içeri aktardığımıza belirli çözüm makalesini gözden geçirin.

Bu çözümleri kaldırdıktan sonra Otomasyon hesabının bağlantısını kaldırmak için aşağıdaki adımları tamamlayabilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri dahil olmak üzere bazı çözümler Otomasyon varlıklarından oluşturmuş olabilir ve ayrıca çalışma alanının bağlantısı kaldırılıyor önce kaldırılması gerekebilir.

1. Azure portalından Otomasyon hesabınızı açın ve sayfa seçin üzerinde Otomasyon hesabı **bağlantılı çalışma** etiketli bölümünde **ilgili kaynakları** soldaki.

2. Bağlantıyı kaldır çalışma sayfasında tıklayın **çalışma alanının bağlantısını Kaldır**.

   ![Çalışma sayfası bağlantısını Kaldır](media/automation-onboard-solutions-from-automation-account/automation-unlink-workspace-blade.png):

   Devam etmek istediğinizi doğrulayan bir ileti alırsınız.

3. Azure Otomasyonu, Log Analytics çalışma alanınızın hesabının bağlantısını kaldırmak denediğinde altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde.

İsteğe bağlı olarak, güncelleştirme yönetimi çözümünü kullandıysanız, çözümü kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Güncelleştirme zamanlamaları - her oluşturduğunuz güncelleştirme dağıtımlarının eşleşen adlara sahip)

* Çözüm için - oluşturulan karma çalışan grupları her benzer şekilde machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8 için olarak adlandırılır).

İsteğe bağlı olarak, yoğun olmayan saatlerde çözümünü sırasında sanal makineleri durdurmak ve başlangıç kullandıysanız, çözümü kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Başlatma ve durdurma VM runbook zamanlama
* VM runbook'ları durdurun ve başlatın
* Değişkenler

Alternatif olarak, ayrıca çalışma alanınızı Otomasyon hesabınızdan Log Analytics çalışma alanınızdan kesebilir. Çalışma alanınızda seçin **Otomasyon hesabı** altında **ilgili kaynakları**. Otomasyon hesabı sayfasında **hesabı bağlantısını**.

## <a name="next-steps"></a>Sonraki adımlar

Bunları nasıl kullanacağınızı öğrenmek için çözümler öğreticiler devam edin.

* [Öğretici - sanal Makineniz için güncelleştirmeleri yönetme](automation-tutorial-update-management.md)

* [Öğretici - VM üzerinde yazılım tanımlama](automation-tutorial-installed-software.md)

* [Öğretici - bir VM üzerindeki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)
