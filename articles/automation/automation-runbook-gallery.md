---
title: Azure Otomasyonu Runbook ve modül galerileri
description: Runbook'ları ve Microsoft ve topluluk modülleri yükleme ve Azure Otomasyonu ortamınızda kullanmak için kullanılabilir.  Bu makalede, bu kaynaklara nasıl erişebileceğinizi açıklanır ve runbook'larınızı Galeriye katkıda bulunma.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/20/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 20aafc117ad8b6bd625894180fdfe79bd86192bd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60737413"
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Azure Otomasyonu Runbook ve modül galerileri

Azure Automation'da kendi runbook'ları ve modüller oluşturmak yerine, Microsoft ve topluluk tarafından zaten oluşturulduğundan senaryoları erişebilirsiniz.

PowerShell runbook'ları alabilirsiniz ve [modülleri](#modules-in-powershell-gallery) PowerShell Galerisi'ndeki ve [Python runbook'ları](#python-runbooks) Komut Merkezi Galerisi. Topluluğa bir runbook Galerisine ekleme bakın, sizin geliştirdiğiniz senaryoları paylaşarak da katkıda bulunabilir

## <a name="runbooks-in-powershell-gallery"></a>PowerShell Galerisi runbook'ları

[PowerShell Galerisi](https://www.powershellgallery.com/packages) runbook'ları çeşitli Microsoft ve Azure Automation'a aktarabilirsiniz topluluk sağlar. Kullanmak için bir runbook'u Galeriden indirin veya runbook'ları galerisinden ya da Azure portalında Otomasyon hesabınızdan doğrudan alabilirsiniz.

Azure portalını kullanarak yalnızca PowerShell Galerisi'nden doğrudan içeri aktarabilirsiniz. PowerShell kullanarak bu işlevi yerine getiremez.

> [!NOTE]
> PowerShell Galerisi'nden alma ve yükleme ve bir üretim ortamında çalıştırırken dikkatli runbook'ları içeriğini doğrulamalıdır.

### <a name="to-import-a-powershell-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Azure portalı ile Runbook Galerisi PowerShell runbook'u içeri aktarmak için

1. Azure portalında, Otomasyon hesabınızı açın.
2. Altında **süreç otomasyonu**, tıklayarak **Runbook'lar Galerisi**
3. Seçin **kaynağı: PowerShell Galerisi**.
4. Ayrıntılarını görüntülemek için seçin ve istediğiniz galeri öğesini bulun. Sol tarafta, yayımcı ve türü için ek arama parametrelerini girebilirsiniz.

   ![Galerisi'ne göz atın](media/automation-runbook-gallery/browse-gallery.png)

5. Tıklayarak **kaynak projeyi görüntüle** öğeyi görüntülemek için [TechNet Komut Merkezi](https://gallery.technet.microsoft.com/).
6. Bir öğe almak için ayrıntılarını görüntülemek ve ardından ona tıklayın **alma** düğmesi.

   ![İçeri Aktar düğmesi](media/automation-runbook-gallery/gallery-item-detail.png)

7. İsteğe bağlı olarak, runbook adını değiştirin ve ardından **Tamam** runbook'u içeri aktarmak için.
8. Runbook görünür **runbook'ları** Otomasyon hesabı için sekmesinde.

### <a name="adding-a-powershell-runbook-to-the-gallery"></a>Bir PowerShell runbook Galerisine ekleme

Microsoft, runbook'ları diğer müşteriler için yararlı olabilecek düşündüğünüz PowerShell Galerisi eklemenizi önerir. PowerShell Galerisi, PowerShell modülleri ve PowerShell betiklerini kabul eder. Bir runbook tarafından ekleyebilirsiniz [PowerShell Galerisi'nden yükleme](/powershell/gallery/how-to/publishing-packages/publishing-a-package).

> [!NOTE]
> Grafik runbook'ları, PowerShell Galerisi'nde desteklenmez.

## <a name="modules-in-powershell-gallery"></a>PowerShell Galerisi modülleri

PowerShell modülleri larınızda kullanabileceğiniz cmdlet'leri içeren ve Azure Automation'da yükleyebilmek için var olan modüller kullanılabilir [PowerShell Galerisi](https://www.powershellgallery.com). Azure portalında bu Galeriden başlatın ve bunları doğrudan Azure Automation'a yükleyin. Bunları indirmek ve bunları el ile yükleyin.  

### <a name="to-import-a-module-from-the-automation-module-gallery-with-the-azure-portal"></a>Otomasyon modülü Galerisi'nden Azure portalıyla bir modülü içeri aktarmak için

1. Azure portalında, Otomasyon hesabınızı açın.
2. Seçin **modülleri** altında **paylaşılan kaynakları** modüllerin listesini açın.
3. Tıklayın **Galeriye Gözat** sayfanın üst.

   ![Modül Galerisi](media/automation-runbook-gallery/modules-blade.png)

4. Üzerinde **Galeriye Gözat** sayfasında, aşağıdaki alanlara göre arama:

   * Modül adı
   * Tags
   * Yazar
   * Cmdlet/DSC kaynak adı

5. İlginizi çeken bir modül bulup ayrıntılarını görüntülemek için seçin.  

   Belirli bir modül olarak ayrıntıya daha fazla bilgi görüntüleyebilirsiniz. Bu bilgiler gerekli PowerShell Galerisi yönelik bir bağlantı içerir. bağımlılıklar ve tüm cmdlet'leri veya modülünde DSC kaynakları.

   ![PowerShell modül ayrıntıları](media/automation-runbook-gallery/gallery-item-details-blade.png)

6. Doğrudan Azure Automation'a modülü yüklemek için tıklayın **alma** düğmesi.
7. Üzerinde İçe Aktar düğmesini tıklattığınızda **alma** bölmesinde almak üzere olduğunuz modül adı görürsünüz. Tüm bağımlılıkların yüklü değilse, **Tamam** düğmesi etkinleştirilir. Bağımlılıkları kayıpsa, bu modül içeri aktarmadan önce bu bağımlılıkların almanız gerekir.
8. Üzerinde **alma** sayfasında **Tamam** modülü içeri aktarın. Azure Automation hesabınız için bir modül alırken, modül ve cmdlet'ler hakkındaki meta verileri ayıklar. Her etkinlik ayıklanacak sonun bu işlem birkaç dakika sürebilir.
9. Tamamlanıp tamamlanmadığını modülü dağıtılmakta bir ilk bildirimi ve başka bir bildirim alırsınız.
10. Modül içeri aktardıktan sonra kullanılabilir etkinlikleri görebilirsiniz. Runbook'ları ve Desired State Configuration kaynaklarını kullanabilirsiniz.

> [!NOTE]
> Yalnızca PowerShell core desteği modülleri, Azure Automation'da desteklenmez ve Azure portalında içeri aktarılan veya PowerShell Galerisi'nden doğrudan dağıtılabilir.

## <a name="python-runbooks"></a>Python runbook'ları

Python runbook'ları kullanılabilir [Komut Merkezi Galerisi](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B1%5D.Type=ProgrammingLanguage&f%5B1%5D.Value=Python&f%5B1%5D.Text=Python&sortBy=Date&username=). Komut Merkezi Galerisi Python runbook'larına tıklayarak katkıda bulunabilir **bir katkı karşıya**. Ne zaman olun etiket ekleme **Python** katkılarınız karşıya yüklenirken.

> [!NOTE]
> İçeriği karşıya yükleme için [betik Merkezi](https://gallery.technet.microsoft.com/scriptcenter) en az 100 noktaları gereklidir. 

## <a name="requesting-a-runbook-or-module"></a>Bir runbook veya modül isteme

İstekleri gönderebilirsiniz [User Voice](https://feedback.azure.com/forums/246290-azure-automation/).  İhtiyaç duyarsanız bir runbook yazma ile veya PowerShell ile ilgili sorularınız varsa soru gönderin bizim [Forumu](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc).

## <a name="next-steps"></a>Sonraki adımlar

* Runbook'larını kullanmaya başlamak için bkz: [Azure Otomasyonu'nda runbook yönetme](manage-runbooks.md)
* PowerShell ve PowerShell iş akışı runbook'ları ile farklarını anlamak için bkz: [Learning PowerShell iş akışı](automation-powershell-workflow.md)
