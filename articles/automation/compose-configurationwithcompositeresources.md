---
title: DSC yapılandırmaları, Azure Otomasyonu durum yapılandırması (bileşik kaynakları kullanarak DSC) oluşturma
description: Bileşik kaynakları Azure Otomasyon durum yapılandırması (DSC) kullanarak yapılandırmaları oluşturma hakkında bilgi edinin
keywords: PowerShell dsc, istenen durum yapılandırması, powershell dsc azure, bileşik kaynaklar
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
ms.date: 08/21/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 64588829cec964e52dcb44465869e0090f36f9f1
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59278631"
---
# <a name="composing-dsc-configurations-in-azure-automation-state-configuration-dsc-using-composite-resources"></a>DSC yapılandırmaları, Azure Otomasyonu durum yapılandırması (bileşik kaynakları kullanarak DSC) oluşturma

Bir kaynağın birden çok tek istenen durum yapılandırması (DSC) yapılandırma ile yönetilmek üzere gerektiğinde, en iyi yolu kullanmaktır [bileşik kaynakları](/powershell/dsc/authoringresourcecomposite). Bileşik kaynak başka bir yapılandırmasındaki bir DSC kaynağı olarak kullanılan bir iç içe geçmiş ve parametreli bir yapılandırmadır. Bu, ayrı ayrı yönetilir ve yerleşik temel bileşik kaynakları (parametreli yapılandırmaları) sağlarken karmaşık yapılandırmanın oluşturulmasını sağlar.

Azure Otomasyonu sağlar [içeri aktarma ve derleme bileşik kaynak](automation-dsc-compile.md#composite-resources). Bileşik kaynaklar Otomasyon hesabınıza alındıktan sonra kullanabilmek için **oluşturma yapılandırma** deneyimini **durum yapılandırması (DSC)** sayfası.

## <a name="composing-a-configuration-from-composite-resources"></a>Bileşik kaynaklar yapılandırmasından oluşturma

Bileşik kaynakları Azure portalında yapılan bir yapılandırma atamadan önce onu oluşturan gerekir. Bu yapılabilir kullanarak **oluşturma yapılandırma** üzerinde **durum yapılandırması (DSC)** durumdayken her iki sayfa **yapılandırmaları** veya **Compiled yapılandırmaları** sekmeler.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** sayfasında **durum yapılandırması (DSC)** altında **yapılandırma yönetimi**.
1. Üzerinde **durum yapılandırması (DSC)** sayfasında, **yapılandırmaları** veya **yapılandırmaları derlenmiş** sekmesine ve ardından tıklayın **oluşturma yapılandırması**  sayfanın üst kısmındaki menüde.
1. Üzerinde **Temelleri** adım, yeni yapılandırma adı (gerekli) ve herhangi bir yeri tıklatın, yeni yapılandırmanızda eklemek ve ardından istediğiniz her bir bileşik kaynak satırda sağlayın **sonraki** veya tıklayın **Kaynak kodu** adım. Aşağıdaki adımlarda, seçtik **PSExecutionPolicy** ve **RenameAndDomainJoin** bileşik kaynaklar.
   ![Temel adımın oluşturma yapılandırma sayfasının ekran görüntüsü](./media/compose-configurationwithcompositeresources/compose-configuration-basics.png)
1. **Kaynak kodu** adım seçili bileşik kaynakları oluşan yapılandırmasını nasıl göründüğünü gösterir. Tüm parametreleri birleştirme görebilirsiniz ve nasıl bileşik kaynağa geçirilir. İşiniz bittiğinde yeni kaynak kodunu inceleyerek tıklayın **sonraki** veya **parametreleri** adım.
   ![Kaynak kodu adımın oluşturma yapılandırma sayfanın ekran görüntüsü](./media/compose-configurationwithcompositeresources/compose-configuration-sourcecode.png)
1. Üzerinde **parametreleri** adım, her bileşik kaynak parametresi, böylece bunlar sağlanabilir gösterilir. Bir parametreye açıklama sahipse parametresi alanın yanında görüntülenir. Bir alan ise bir **PSCredential** tür parametresi, yapılandırmak için açılan bir liste sağlar **kimlik bilgisi** nesneleri geçerli Otomasyon hesabı. A **+ kimlik bilgisi Ekle** seçenektir de kullanılabilir. Tüm gerekli parametreleri sağlanan bitince **Kaydet ve derle**.
   ![Parametreleri adımın oluşturma yapılandırma sayfasının ekran görüntüsü](./media/compose-configurationwithcompositeresources/compose-configuration-parameters.png)

Yeni yapılandırma kaydedildikten sonra bu derleme için gönderilir. Derleme işi durumu gibi herhangi bir içeri aktarılan yapılandırma görüntülenebilir. Daha fazla bilgi için [bir derleme işi görüntüleme](automation-dsc-getting-started.md#viewing-a-compilation-job).

Derleme başarıyla tamamlandığında, yeni yapılandırmayı görünür **yapılandırmaları derlenmiş** sekmesi. Bu sekmede görünür duruma gelince, bu adımları kullanarak bir yönetilen düğüme atanabilir [farklı düğüm yapılandırması düğüme yeniden atama](automation-dsc-getting-started.md#reassigning-a-node-to-a-different-node-configuration).

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için bkz: [Azure Otomasyon durum yapılandırması ile çalışmaya başlama](automation-dsc-getting-started.md)
- Bilgi edinmek için nasıl yerleşik düğümlerine bkz [makineleri Azure Otomasyon durum yapılandırması tarafından Yönetim için hazırlama](automation-dsc-onboarding.md)
- Hedef düğümleri atayabilirsiniz böylece, DSC yapılandırmaları derleme hakkında bilgi edinmek için [yapılandırmaları Azure Automation durumu yapılandırma derleme](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için bkz. [Azure Otomasyonu durumu yapılandırma cmdlet'leri](/powershell/module/azurerm.automation/#automation)
- Fiyatlandırma bilgileri için bkz: [Azure Otomasyon durum yapılandırması için fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/)
- Bir sürekli dağıtım işlem hattı, Azure Otomasyonu durum yapılandırmasını kullanarak bir örnek görmek için bkz: [sürekli dağıtımı kullanarak Azure Otomasyon durum yapılandırması ve Chocolatey](automation-dsc-cd-chocolatey.md)
