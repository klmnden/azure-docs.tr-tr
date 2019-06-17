---
title: Bir Azure VM'den ekleme güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri
description: Bilgi nasıl için yerleşik bir Azure sanal makinesi ile güncelleştirme yönetimi, değişiklik izleme ve sayım çözümlerini parçası olan Azure Otomasyonu.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/20/2019
ms.topic: conceptual
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 38b5b233d21c0c5d79d7bcb6a145e6232b238b0e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66133113"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Bir Azure sanal makinesinden yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri

Azure Otomasyonu, işletim sistemi güvenlik güncelleştirmelerini yönetme, değişiklikleri izleme ve stok bilgisayarlarınızda yüklü yardımcı olmaya yönelik çözümler sunar. Makine birden çok yolu vardır. Bir sanal makineden çözüm ekleyebilir [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md), [birden çok makine gözatma gelen](automation-onboard-solutions-from-browse.md), kullanarak veya bir [runbook](automation-onboard-solutions.md). Bu makale bir Azure sanal makinesinden bu çözümleri ekleme kapsar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.

## <a name="enable-the-solutions"></a>Çözümleri etkinleştirme

Varolan bir sanal makineye gidin. Altında **işlemleri**seçin **güncelleştirme yönetimi**, **Envanter**, veya **değişiklik izleme**. Sanal makine konumu Otomasyon hesabınızın ne olursa olsun tüm bölgelerdeki mevcut olabilir. Bir VM'den bir çözüm ekleme gerektiğinde sahip `Microsoft.OperationalInsights/workspaces/read` VM için bir çalışma alanına eklenen olup olmadığını belirlemek için izni. Genel olarak gerekli olan ek izinler hakkında bilgi edinmek için [makine için gereken izinleri](automation-role-based-access-control.md#onboarding).

Yalnızca VM için çözümü etkinleştirmek için emin olun **bu VM için etkinleştirme** seçilir. İçin yerleşik bir çözüm, birden çok makineye seçin **VM'ler için bu abonelikte etkinleştirmek**ve ardından **etkinleştirmek için makineleri seçmek için tıklatın**. Nasıl için yerleşik birden çok makine aynı anda öğrenmek için [yerleşik güncelleştirme yönetimi, değişiklik izleme ve sayım çözümlerini](automation-onboard-solutions-from-automation-account.md).

Azure Log Analytics çalışma alanını ve Otomasyon hesabı seçin ve ardından **etkinleştirme** çözümü etkinleştirmek için. Çözümün etkinleştirilmesi 15 dakika sürer.

![Yerleşik güncelleştirme yönetimi çözümü](media/automation-onboard-solutions-from-vm/onboard-solution.png)

Diğer çözümleri gidin ve ardından **etkinleştirme**. Bu çözümler daha önce etkin bir çözüm olarak aynı çalışma alanını ve Otomasyon hesabı kullandığından Log Analytics çalışma alanını ve Otomasyon hesabı açılan listeler devre dışı bırakıldı.

> [!NOTE]
> **Değişiklik izleme** ve **Envanter** aynı çözümü kullanın. Bu çözümlerden birini etkinleştirildiğinde, diğeri de etkinleştirilir.

## <a name="scope-configuration"></a>Kapsam yapılandırması

Her bir çözümün kapsam yapılandırması çözümünü edinme bilgisayarları hedeflemek için çalışma alanında kullanır. Kapsam yapılandırması belirli bilgisayarlara çözüm kapsamını sınırlamak için kullanılan bir veya daha fazla kayıtlı aramalar grubudur. Kapsam yapılandırmaları, Otomasyon hesabınızda altında erişmeye **ilgili kaynakları**seçin **çalışma**. Çalışma alanında, altında **çalışma alanı veri kaynakları**seçin **kapsam yapılandırmaları**.

Seçilen çalışma alanı, güncelleştirme yönetimi veya değişiklik izleme çözümlerini zaten yoksa, aşağıdaki kapsam yapılandırmaları oluşturulur:

* **MicrosoftDefaultScopeConfig değişiklik izleme**

* **MicrosoftDefaultScopeConfig güncelleştirmeleri**

Seçilen çalışma alanında Çözüm zaten varsa, çözümü yeniden değildir ve kapsam yapılandırması eklenmez.

Üç nokta simgesini ( **...** ) yapılandırmaları tıklayın ve ardından hiçbirinde **Düzenle**. İçinde **düzenleme kapsam yapılandırması** bölmesinde **bilgisayar grupları Seç**. **Bilgisayar grupları** kapsam yapılandırması oluşturmak için kullanılan kayıtlı aramalar bölmesi gösterir.

## <a name="saved-searches"></a>Kayıtlı aramalar

Güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri için bir bilgisayar eklendiğinde, bilgisayar bir çalışma alanınızdaki iki kayıtlı aramalar eklenir. Kayıtlı aramalar, bu çözümleri için hedeflenen bilgisayarları içeren sorgular kullanılmaktadır.

Çalışma alanınıza gidin. Altında **genel**seçin **kayıtlı aramalar**. Bu çözümler tarafından kullanılan iki kayıtlı aramalar aşağıdaki tabloda gösterilmiştir:

|Ad     |Kategori  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  Değişiklik izleme       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

Grup doldurmak için kullanılan sorguyu görüntülemek için kaydedilmiş aramalara birini seçin. Aşağıdaki resimde, sorgu ve sonuçları gösterilmektedir:

![Kayıtlı aramalar](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="unlink-workspace"></a>Çalışma alanının bağlantısını Kaldır

Aşağıdaki çözümlerden bir Log Analytics çalışma alanına bağlıdır:

* [Güncelleştirme yönetimi](automation-update-management.md)
* [Değişiklik İzleme](automation-change-tracking.md)
* [Vm'leri çalışma saatleri dışında başlatma/durdurma](automation-solution-vm-management.md)

Bir Log Analytics çalışma alanı ile Otomasyon hesabınızı tümleştirmek istediğiniz karar verirseniz, doğrudan Azure portalından hesabınızı kesebilir.  Devam etmeden önce öncelikle daha önce bahsedilen çözümleri kaldırmanız gerekir, bu işlem devam etmeden gelen Aksi takdirde engellenir. Kaldırmak için gerekli adımları anlamak için alınan belirli çözüm makalesini gözden geçirin.

Bu çözümleri kaldırdıktan sonra Otomasyon hesabının bağlantısını kaldırmak için aşağıdaki adımları gerçekleştirebilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri dahil olmak üzere bazı çözümler Otomasyon varlıklarından oluşturmuş olabilir ve ayrıca çalışma alanının bağlantısı kaldırılıyor önce kaldırılması gerekebilir.

1. Azure portalından Otomasyon hesabınızı açın ve sayfa seçin üzerinde Otomasyon hesabı **bağlantılı çalışma** bölümünde **ilgili kaynakları** soldaki.

2. Bağlantıyı kaldır çalışma sayfasında tıklayın **çalışma alanının bağlantısını Kaldır**.

   ![Çalışma sayfası bağlantısını Kaldır](media/automation-onboard-solutions-from-vm/automation-unlink-workspace-blade.png).

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

## <a name="next-steps"></a>Sonraki adımlar

Çözümler, bunları nasıl kullanacağınızı öğrenmek için öğreticilere geçin:

* [Öğretici - sanal Makineniz için güncelleştirmeleri yönetme](automation-tutorial-update-management.md)
* [Öğretici - VM üzerinde yazılım tanımlama](automation-tutorial-installed-software.md)
* [Öğretici - bir VM üzerindeki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)
