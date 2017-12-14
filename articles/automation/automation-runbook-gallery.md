---
title: "Azure Otomasyonu Runbook ve modül galerileri | Microsoft Docs"
description: "Runbook'lardan ve modüllerden Microsoft ve topluluk yüklemenizi ve Azure Otomasyonu ortamınızda kullanmak için kullanılabilir.  Bu makalede, bu kaynakları nasıl erişebileceğiniz açıklanır ve runbook'larınızın Galeriye katkıda bulunma."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: d3fee7b4-630a-4c10-8425-9bf51d7c9e58
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 70bbc131f153efd88816450c239920c79665fdff
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Azure Otomasyonu Runbook ve modül galerileri
Azure Automation'da kendi runbook'lardan ve modüllerden oluşturmak yerine, zaten Microsoft ve topluluk tarafından oluşturulmuş senaryoları çeşitli erişebilir.  Bu senaryolar değişiklik yapmadan ya da kullanabilir veya bir başlangıç noktası olarak kullanın ve bunları belirli gereksinimleriniz için düzenleyin.

Runbook'lardan alabilirsiniz [Runbook Galerisi](#runbooks-in-runbook-gallery) ve modüllerden [PowerShell Galerisi](#modules-in-powerShell-gallery).  Ayrıca, geliştirdiğiniz senaryoları paylaşarak topluluğa katkıda bulunabilir.

## <a name="runbooks-in-runbook-gallery"></a>Runbook Galerisi runbook'ları
[Runbook Galerisi](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=RootCategory&f\[0\].Value=WindowsAzure&f\[1\].Type=SubCategory&f\[1\].Value=WindowsAzure_automation&f\[1\].Text=Automation) runbook'lar çeşitli Microsoft ve Microsoft Azure Automation'a aktarabilirsiniz topluluk sağlar. İçinde barındırılan Galeriden bir runbook indirebilirsiniz [TechNet Komut Merkezi](https://gallery.technet.microsoft.com/scriptcenter/site/upload), veya Klasik Azure portalı ya da Azure portal galerisinden runbook'ları doğrudan aktarabilirsiniz.

Yalnızca Azure Klasik portalında veya Azure portal kullanarak doğrudan Runbook'u Galeriden içeri aktarabilirsiniz. Windows PowerShell kullanarak bu işlev gerçekleştiremiyor.

> [!NOTE]
> Runbook'u Galeriden alın ve yükleme ve bir üretim ortamında çalışan çok dikkatli tüm runbook içeriğini doğrulamalıdır. |
> 
> 

### <a name="to-import-a-runbook-from-the-runbook-gallery-with-the-azure-classic-portal"></a>Klasik Azure portalı ile Runbook Galeriden bir runbook'u içeri aktarma
1. Azure Portal'ı tıklatın, **yeni**, **uygulama hizmetleri**, **Otomasyon**, **Runbook**, **Galeri'den**.
2. İlgili runbook'ları görüntülemek için bir kategori seçin ve bir runbook'un ayrıntılarını görüntülemek için seçin. İstediğiniz runbook'u seçin, sağ ok düğmesine tıklayın.
   
    ![Runbook galerisi](media/automation-runbook-gallery/runbook-gallery.png)
3. Runbook'un içeriğini gözden geçirin ve gereksinimlere açıklamasında unutmayın. İşiniz bittiğinde sağ ok düğmesine tıklayın.
4. Runbook ayrıntılarını girin ve ardından onay işareti düğmesine tıklayın. Runbook adı zaten doldurulur.
5. Runbook kasasındaki **Runbook'lar** Otomasyon hesabının sekmesi.

### <a name="to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Azure portalı ile Runbook Galeriden bir runbook'u içeri aktarma
1. Azure portalında, Otomasyon hesabınızı açın.
2. Runbook'ların listesini açmak için **Runbook'lar** kutucuğuna tıklayın.
3. Tıklatın **Gözat galeri** düğmesi.
   
    ![Gözat galeri düğmesi](media/automation-runbook-gallery/browse-gallery-button.png)
4. İstediğiniz ve ayrıntılarını görüntülemek için seçin galeri öğesini bulun.
   
    ![Galerisine gözatma](media/automation-runbook-gallery/browse-gallery.png)
5. Tıklayın **görünüm kaynak proje** öğesinde görüntülemek için [TechNet Komut Merkezi](http://gallery.technet.microsoft.com/).
6. Bir öğeyi almak için ayrıntılarını görüntülemek ve ardından tıklayın **alma** düğmesi.
   
    ![İçeri Aktar düğmesi](media/automation-runbook-gallery/gallery-item-detail.png)
7. İsteğe bağlı olarak, runbook adını değiştirin ve ardından **Tamam** runbook'u içeri aktarma.
8. Runbook kasasındaki **Runbook'lar** Otomasyon hesabının sekmesi.

### <a name="adding-a-runbook-to-the-runbook-gallery"></a>Bir runbook için runbook Galerisine ekleme
Microsoft, diğer müşteriler için yararlı olabilecek düşündüğünüz Runbook Galerisi runbook'lar eklemek için önerir.  Bir runbook tarafından ekleyebilirsiniz [betik Merkezi'ne karşıya](http://gallery.technet.microsoft.com/site/upload) dikkate alarak aşağıdaki ayrıntıları.

* Belirtmeniz gerekir *Windows Azure* için **kategori** ve *Otomasyon* için **alt kategori** Sihirbazı'nda görüntülenecek runbook.  
* Karşıya yükleme, tek bir .ps1 veya .graphrunbook dosyası olmalıdır.  Runbook tüm modülleri, alt runbook'ları veya varlıklar gerektiriyorsa, bu gönderimi açıklaması ve runbook'un Açıklamalar bölümüne listelenmelidir.  Birden çok runbook gerektiren bir senaryo varsa, her ayrı olarak karşıya yükleme ve her açıklamalarının ilgili runbook'larda adlarını listeler. Böylece aynı kategoride Göster aynı etiketleri kullandığınızdan emin olun. Bir kullanıcı başka runbook'lar gerekli olduğunu bilmek açıklamayı okuma gerekecek çalışmaya senaryo.
* Yayın yapıyorsanız "GraphicalPS" etiketi eklemek bir **grafik runbook** (olmayan grafik iş akışı). 
* Açıklama kullanarak bir PowerShell veya PowerShell iş akışı kod parçacığını eklemek **Ekle kod bölümünde** simgesi.
* Bir kullanıcı runbook'u işlevselliğini tanımlamak yardımcı olacak ayrıntılı bilgileri sağlaması gerekir böylece karşıya yükleme özeti Runbook Galerisi sonuçları görüntülenir.
* Yüklemek için aşağıdaki etiketlerin bir ile üç atamanız gerekir.  Runbook Sihirbazı'ndaki kendi etiketlerle eşleşecek kategorisi altında listelenir.  Bu listedeki herhangi bir etiket sihirbaz tarafından göz ardı edilir. Eşleşen herhangi bir etiket belirlemezseniz, runbook bir kategorisi altında listelenir.
  
  * Backup
  * Kapasite Yönetimi
  * Değişiklik denetimi
  * Uyumluluk
  * Geliştirme / Test ortamları
  * Olağanüstü Durum Kurtarma
  * İzleme
  * Düzeltme eki uygulama
  * Sağlama
  * Düzeltme
  * VM yaşam döngüsü yönetimi
* Otomasyon galeri saatte bir kez güncelleştirir, böylece katkılarınız hemen göremezsiniz.

## <a name="modules-in-powershell-gallery"></a>PowerShell galerisinde modülleri
PowerShell modülleri larınızda kullanabileceğiniz cmdlet'leri içeren ve Azure Otomasyonu'nda yükleyebilmek için var olan modülleri kullanılabilir [PowerShell Galerisi](http://www.powershellgallery.com).  Bu galeri Azure portalından başlatın ve bunları doğrudan Azure Automation'a yükleyin veya bunları indirebilir ve bunları el ile yükleyin.  Modülleri doğrudan Azure Klasik portalından yükleyemezsiniz, ancak bunları yükleyebilirsiniz, başka bir modül gibi bunları yükleyin.

### <a name="to-import-a-module-from-the-automation-module-gallery-with-the-azure-portal"></a>Azure portal ile otomasyon modülü Galeriden bir modülü içeri aktarmak için
1. Azure portalında, Otomasyon hesabınızı açın.
2. Seçin **modülleri** altında **paylaşılan kaynakları** modülleri listesini açın.
4. Tıklatın **Gözat galeri** sayfasının üstten.
   
    ![Modül Galerisi](media/automation-runbook-gallery/modules-blade.png) <br>
5. Üzerinde **Gözat galeri** sayfasında, aşağıdaki alanlara göre arayabilirsiniz:
   
   * Modül adı
   * Etiketler
   * Yazar
   * Cmdlet/DSC kaynağı adı
6. İlgilendiğiniz bir modül bulup ayrıntılarını görüntülemek için seçin.  
   Belirli bir modüle ayrıntıya, daha fazla bilgi görüntüleyebilirsiniz bağlantı geri PowerShell Galerisi modülü hakkında gerekli bağımlılıkları ve tüm cmdlet'ler ve/veya modülü içeren DSC kaynakları.
   
    ![PowerShell modülü ayrıntıları](media/automation-runbook-gallery/gallery-item-details-blade.png) <br>
7. Doğrudan Azure Automation'a modülünü yüklemek için tıklayın **alma** düğmesi.
   
    ![İçeri aktarma modülü düğmesi](media/automation-runbook-gallery/module-import-button.png)
8. Üzerinde İçe Aktar düğmesini tıklattığınızda **alma** bölmesinde gördüğünüz almak üzere olduğunuz modül adı. Tüm bağımlılıkları yüklediyseniz, **Tamam** düğmesi etkinleştirilir. Bağımlılıklar yoksa, bu modül içeri aktarmadan önce bu içeri aktarmanız gerekir.
9. Tıklatın **Tamam** modülü içeri aktarmak için. Azure Automation hesabınız için bir modül alırken, modül ve cmdlet'leri hakkındaki meta verileri ayıklar.
   
    ![İçeri aktarma modül sayfası](media/automation-runbook-gallery/module-import-blade.png)
   
    Her etkinlik ayıklanacak gerektiğinden bu birkaç dakika sürebilir.
10. Bu tamamlandığında, modül dağıtıldığı bir ilk bildirim ve başka bir bildirim alırsınız.
11. Modül içeri aktarıldıktan sonra kullanılabilir etkinlikler görebilirsiniz ve runbook'larınızda ve istenen durum yapılandırması kaynaklarını kullanabilirsiniz.

## <a name="requesting-a-runbook-or-module"></a>Bir runbook veya modülü isteme
İstekleri gönderebilirsiniz [kullanıcı sesi](https://feedback.azure.com/forums/246290-azure-automation/).  Bir runbook yazma Yardım veya PowerShell hakkında bir sorunuz varsa, bir soru gönderin bizim [Forumu](http://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc).

## <a name="next-steps"></a>Sonraki Adımlar
* Runbook'ları kullanmaya başlamak için bkz: [oluşturma veya bir Azure Otomasyonu runbook'u içeri aktarma](automation-creating-importing-runbook.md)
* Runbook'larla PowerShell ve PowerShell iş akışı arasındaki farkları anlamak için bkz: [öğrenme PowerShell iş akışı](automation-powershell-workflow.md)

