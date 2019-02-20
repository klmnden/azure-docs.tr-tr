---
title: Azure Otomasyonu Runbook ve modül galerileri
description: Runbook'ları ve Microsoft ve topluluk modülleri yükleme ve Azure Otomasyonu ortamınızda kullanmak için kullanılabilir.  Bu makalede, bu kaynaklara nasıl erişebileceğinizi açıklanır ve runbook'larınızı Galeriye katkıda bulunma.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 68eb3f3e5c568bb518251aca2a4e76932ce2eee5
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56416217"
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Azure Otomasyonu Runbook ve modül galerileri

Azure Automation'da kendi runbook'ları ve modüller oluşturmak yerine, Microsoft ve topluluk tarafından zaten oluşturulduğundan senaryoları erişebilirsiniz.

Runbook'ları alabilirsiniz [Runbook Galerisi](#runbooks-in-runbook-gallery) ve modüllerden [PowerShell Galerisi](#modules-in-powerShell-gallery).  Ayrıca katkıda bulunabilir bakın topluluğa geliştirme senaryoları paylaşabilir, [runbook Galerisine ekleme](automation-runbook-gallery.md#adding-a-runbook-to-the-runbook-gallery)

## <a name="runbooks-in-runbook-gallery"></a>Runbook'ları Runbook Galerisi

[Runbook Galerisi](https://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=RootCategory&f\[0\].Value=WindowsAzure&f\[1\].Type=SubCategory&f\[1\].Value=WindowsAzure_automation&f\[1\].Text=Automation) runbook'ları çeşitli Microsoft ve Azure Automation'a aktarabilirsiniz topluluk sağlar. Kullanmak için barındırılan Galeriden runbook indirme [TechNet Komut Merkezi](https://gallery.technet.microsoft.com/scriptcenter/site/upload), ya da Azure portalında Galeriden runbook'ları doğrudan aktarabilirsiniz.

Yalnızca Azure portalını kullanarak doğrudan Runbook'u Galeriden içeri aktarabilirsiniz. Windows PowerShell kullanarak bu işlevi yerine getiremez.

> [!NOTE]
> Galeriden Runbook alma ve yükleme ve bir üretim ortamında çalıştırırken dikkatli runbook'ları içeriğini doğrulamalıdır.

### <a name="to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Runbook Galerisi Azure portalıyla bir runbook'u içeri aktarmak için

1. Azure portalında, Otomasyon hesabınızı açın.
2. Altında **süreç otomasyonu**, tıklayarak **Runbook'lar Galerisi**
3. Ayrıntılarını görüntülemek için seçin ve istediğiniz galeri öğesini bulun. Sol tarafta, yayımcı ve türü için ek arama parametrelerini girebilirsiniz.

   ![Galerisi'ne göz atın](media/automation-runbook-gallery/browse-gallery.png)

4. Tıklayarak **kaynak projeyi görüntüle** öğeyi görüntülemek için [TechNet Komut Merkezi](https://gallery.technet.microsoft.com/).
5. Bir öğe almak için ayrıntılarını görüntülemek ve ardından ona tıklayın **alma** düğmesi.

   ![İçeri Aktar düğmesi](media/automation-runbook-gallery/gallery-item-detail.png)

6. İsteğe bağlı olarak, runbook adını değiştirin ve ardından **Tamam** runbook'u içeri aktarmak için.
7. Runbook görünür **runbook'ları** Otomasyon hesabı için sekmesinde.

### <a name="adding-a-runbook-to-the-runbook-gallery"></a>Bir runbook için runbook Galerisi ekleme

Microsoft, runbook'ları diğer müşteriler için yararlı olabilecek düşündüğünüz Runbook Galerisi eklemenizi önerir.  Bir runbook tarafından ekleyebilirsiniz [betik Merkezi'ne karşıya](https://gallery.technet.microsoft.com/site/upload) dikkate alarak aşağıdaki ayrıntıları.

* Belirtmelisiniz *Windows Azure* için **kategori** ve *Otomasyon* için **Subcategory** görüntülenecek runbook Sihirbaz.  
* Karşıya yükleme, tek bir olmalıdır `.ps1` veya `.graphrunbook` dosya.  Runbook herhangi modülleri, alt runbook'ları ve varlıkları gerektiriyorsa, gönderim açıklaması ve runbook'un yorumlar bölümünde öğeleri listesinde.  Birden çok runbook gerektiren bir senaryo varsa, her birini ayrı olarak karşıya yüklemek ve ilgili runbook'ların her birindeki açıklamalarının adlarını listeler. Bunlar aynı kategoride gösterilir böylece aynı etiketleri kullandığınızdan emin olun. Bir kullanıcı diğer runbook'ların gerekli olduğunu bildiren bir açıklama okuyun gerekecektir çalışmak için bir senaryo.
* Yayımlıyorsanız, "GraphicalPS" etiket ekleme bir **grafik runbook** (değil bir grafik iş akışı).
* Açıklama kullanarak PowerShell ya da PowerShell iş akışı kod parçacığı eklemek **Ekle kod bölümünde** simgesi.
* Bir kullanıcı runbook'u işlevselliğini tanımlamak yardımcı olacak ayrıntılı bilgileri sağlamalıdır. Bu nedenle karşıya yükleme özeti Runbook Galerisi sonuçları görüntülenir.
* Yüklemek için aşağıdaki etiketlerin bir ila üç atamanız gerekir.  Runbook sihirbazında kendi etiketlerle eşleşecek kategorisi altında listelenir.  Bu listedeki herhangi bir etiket, sihirbaz tarafından göz ardı edilir. Eşleşen herhangi bir etiket belirtmezseniz, runbook bir kategorisi altında listelenir.
  
  * Backup
  * Kapasite Yönetimi
  * Denetimi değiştirme
  * Uyumluluk
  * Geliştirme / Test ortamları
  * Olağanüstü Durum Kurtarma
  * İzleme
  * Düzeltme eki uygulanıyor
  * Sağlama
  * Düzeltme
  * VM yaşam döngüsü yönetimi

* Otomasyon, Galeri saatte bir kez güncelleştirir, böylece Katkılarınızı hemen göremezsiniz.

## <a name="modules-in-powershell-gallery"></a>PowerShell Galerisi modülleri

PowerShell modülleri larınızda kullanabileceğiniz cmdlet'leri içeren ve Azure Automation'da yükleyebilmek için var olan modüller kullanılabilir [PowerShell Galerisi](https://www.powershellgallery.com). Azure portalında bu Galeriden başlatın ve bunları doğrudan Azure Automation'a yükleyin. Bunları indirmek ve bunları el ile yükleyin.  

### <a name="to-import-a-module-from-the-automation-module-gallery-with-the-azure-portal"></a>Otomasyon modülü Galerisi'nden Azure portalıyla bir modülü içeri aktarmak için

1. Azure portalında, Otomasyon hesabınızı açın.
2. Seçin **modülleri** altında **paylaşılan kaynakları** modüllerin listesini açın.
3. Tıklayın **Galeriye Gözat** sayfanın üst.

   ![Modül Galerisi](media/automation-runbook-gallery/modules-blade.png)

4. Üzerinde **Galeriye Gözat** sayfasında, aşağıdaki alanlara göre arama:

   * Modül Adı
   * Etiketler
   * Yazma
   * Cmdlet/DSC kaynak adı

5. İlginizi çeken bir modül bulup ayrıntılarını görüntülemek için seçin.  

   Belirli bir modül olarak ayrıntıya daha fazla bilgi görüntüleyebilirsiniz. Bu bilgiler gerekli PowerShell Galerisi yönelik bir bağlantı içerir. bağımlılıklar ve tüm cmdlet'leri veya modülünde DSC kaynakları.

   ![PowerShell modül ayrıntıları](media/automation-runbook-gallery/gallery-item-details-blade.png)

6. Doğrudan Azure Automation'a modülü yüklemek için tıklayın **alma** düğmesi.
7. Üzerinde İçe Aktar düğmesini tıklattığınızda **alma** bölmesinde almak üzere olduğunuz modül adı görürsünüz. Tüm bağımlılıkların yüklü değilse, **Tamam** düğmesi etkinleştirilir. Bağımlılıkları kayıpsa, bu modül içeri aktarmadan önce bu bağımlılıkların almanız gerekir.
8. Üzerinde **alma** sayfasında **Tamam** modülü içeri aktarın. Azure Automation hesabınız için bir modül alırken, modül ve cmdlet'ler hakkındaki meta verileri ayıklar. Her etkinlik ayıklanacak sonun bu işlem birkaç dakika sürebilir.
9.  Tamamlanıp tamamlanmadığını modülü dağıtılmakta bir ilk bildirimi ve başka bir bildirim alırsınız.
10. Modül içeri aktardıktan sonra kullanılabilir etkinlikleri görebilirsiniz. Runbook'ları ve Desired State Configuration kaynaklarını kullanabilirsiniz.

> [!NOTE]
> Yalnızca PowerShell core desteği modülleri, Azure Automation'da desteklenmez ve Azure portalında içeri aktarılan veya PowerShell Galerisi'nden doğrudan dağıtılabilir.

## <a name="python-runbooks"></a>Python runbook'ları

Python runbook'ları kullanılabilir [Komut Merkezi Galerisi](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B1%5D.Type=ProgrammingLanguage&f%5B1%5D.Value=Python&f%5B1%5D.Text=Python&sortBy=Date&username=). Python runbook'ları betik merkezi Galeriye katkıda bulunabilir. Ne zaman olun etiket ekleme **Python** katkılarınız karşıya yüklenirken.

## <a name="requesting-a-runbook-or-module"></a>Bir runbook veya modül isteme

İstekleri gönderebilirsiniz [User Voice](https://feedback.azure.com/forums/246290-azure-automation/).  İhtiyaç duyarsanız bir runbook yazma ile veya PowerShell ile ilgili sorularınız varsa soru gönderin bizim [Forumu](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc).

## <a name="next-steps"></a>Sonraki adımlar

* Runbook'larını kullanmaya başlamak için bkz: [Azure Otomasyonu'nda runbook yönetme](manage-runbooks.md)
* PowerShell ve PowerShell iş akışı runbook'ları ile farklarını anlamak için bkz: [Learning PowerShell iş akışı](automation-powershell-workflow.md)