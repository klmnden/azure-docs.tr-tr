---
title: "Azure Automation Runbook türleri"
description: "Azure Automation ve hangi türün kullanılacağını belirlerken dikkate almanız konuları kullanabilirsiniz runbook'ları farklı türleri açıklanmaktadır. "
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: f3a6b15891a4a1564073d149a198f6789b407342
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="azure-automation-runbook-types"></a>Azure Automation runbook türleri
Azure Otomasyonu aşağıdaki tabloda çeşitli kısaca açıklanmıştır runbook'ları destekler.  Aşağıdaki bölümlerde zaman her kullanılacağı hakkında dikkat edilecek noktalar dahil olmak üzere her tür hakkında daha fazla bilgi verilmektedir.

| Tür | Açıklama |
|:--- |:--- |
| [Grafik](#graphical-runbooks) |Windows PowerShell ve Azure portalında oluşturulan ve tamamen içinde düzenlenen grafik Düzenleyicisi göre. |
| [Grafik PowerShell iş akışı](#graphical-runbooks) |Windows PowerShell iş akışı tabanlı ve oluşturulan ve Azure portalında grafik Düzenleyicisi'nde tamamen düzenlenebilir. |
| [PowerShell](#powershell-runbooks) |Windows PowerShell komut dosyasına dayalı metin runbook. |
| [PowerShell İş Akışı](#powershell-workflow-runbooks) |Windows PowerShell iş akışı tabanlı metin runbook. |
| [Python](#python-runbooks) |Python üzerinde temel metin runbook. |

## <a name="graphical-runbooks"></a>Grafik runbook'lar
[Grafik](automation-runbook-types.md#graphical-runbooks) ve grafik PowerShell iş akışı runbook'ları oluşturulur ve Azure portalında grafik Düzenleyicisi ile düzenlenemez.  Bir dosyaya aktarın ve ardından başka bir Otomasyon hesaba içeri aktarmak, ancak oluşturamaz veya bunları başka bir araçla düzenleyin.  PowerShell kodu grafik runbook'ları oluşturmak, ancak doğrudan görüntüleyemez veya kodu değiştirin. Grafik runbook'lar birine dönüştürülemiyor [metin biçimleri](automation-runbook-types.md), veya bir metin runbook grafik biçimine dönüştürülebilir. Grafik runbook'lar grafik PowerShell iş akışı runbook'larına içeri aktarma ve tam tersini sırasında dönüştürülebilir.

### <a name="advantages"></a>Avantajları
* INSERT bağlantısını yapılandırmak görsel geliştirme modeli  
* Veri sürecinde nasıl akacağını odaklanın  
* Yönetim işlemlerini görsel olarak temsil eder  
* Üst düzey iş akışları oluşturmak için alt runbook'lar olarak diğer runbook'lar içerir  
* Modüler programlama teşvik eder  


### <a name="limitations"></a>Sınırlamalar
* Azure portal dışında runbook'u düzenleyemez.
* Karmaşık mantık gerçekleştirmek için PowerShell kodu içeren bir kod etkinlik gerektirebilir.
* Görüntüleyemez veya doğrudan grafik iş akışı tarafından oluşturulan PowerShell kodu girin. Tüm kod etkinliklerin oluşturma kodu görüntüleyebilirsiniz unutmayın.

## <a name="powershell-runbooks"></a>PowerShell runbook'ları
PowerShell runbook'ları Windows PowerShell üzerinde temel alır.  Doğrudan Azure portalında metin düzenleyicisi kullanarak runbook'un kodu girin.  Herhangi bir çevrimdışı metin düzenleyicisi de kullanabilirsiniz ve [runbook'u içeri](http://msdn.microsoft.com/library/azure/dn643637.aspx) Azure Otomasyonu içine.

### <a name="advantages"></a>Avantajları
* PowerShell iş akışı ek karmaşıklığını olmadan PowerShell kodu ile tüm karmaşık mantığı uygular. 
* Çalıştırmadan önce derlenmesi gerekli olmayan beri Runbook PowerShell iş akışı runbook'ları daha hızlı başlatır.

### <a name="limitations"></a>Sınırlamalar
* PowerShell komut dosyalarıyla bilgi sahibi olmanız gerekir.
* Kullanamazsınız [paralel işleme](automation-powershell-workflow.md#parallel-processing) paralel olarak birden çok eylemleri gerçekleştirmek için.
* Kullanamazsınız [kontrol noktaları](automation-powershell-workflow.md#checkpoints) runbook hata durumunda devam etmek için.
* PowerShell iş akışı runbook'ları ve grafik runbook'lar yalnızca yeni bir iş oluşturur başlangıç AzureAutomationRunbook cmdlet'ini kullanarak alt runbook'lar olarak eklenebilir.

### <a name="known-issues"></a>Bilinen Sorunlar
PowerShell runbook'ları bilinen geçerli sorunlar aşağıda verilmiştir.

* PowerShell runbook'ları bir şifrelenmemiş alamıyor [değişken varlığı](automation-variables.md) null değerine sahip.
* PowerShell runbook'ları alamıyor bir [değişken varlığı](automation-variables.md) ile  *~*  adı.
* Get-Process döngü olarak bir PowerShell runbook yaklaşık 80 yinelemeden sonra kilitlenebilir. 
* Çok büyük miktarda veri yazma çıkış akışına aynı anda kullanmaya çalışırsa, bir PowerShell runbook başarısız olabilir.   Bu sorunu çözmek büyük nesneler ile çalışırken gerekli bilgileri kayıt çıkarma genellikle çalışabilir.  Örneğin, aşağıdakine benzer çıktısı yerine *Get-Process*, yalnızca gerekli alanları ile çıkış *Get-Process | İşlem adı, CPU seçin*.

## <a name="powershell-workflow-runbooks"></a>PowerShell iş akışı runbook'ları
PowerShell iş akışı runbook'ları olan temel metin runbook'lar [Windows PowerShell iş akışı](automation-powershell-workflow.md).  Doğrudan Azure portalında metin düzenleyicisi kullanarak runbook'un kodu girin.  Herhangi bir çevrimdışı metin düzenleyicisi de kullanabilirsiniz ve [runbook'u içeri](http://msdn.microsoft.com/library/azure/dn643637.aspx) Azure Otomasyonu içine.

### <a name="advantages"></a>Avantajları
* PowerShell iş akışı kodu ile tüm karmaşık mantığı uygular.
* Kullanım [kontrol noktaları](automation-powershell-workflow.md#checkpoints) runbook hata durumunda devam etmek için.
* Kullanım [paralel işleme](automation-powershell-workflow.md#parallel-processing) paralel olarak birden çok eylemleri gerçekleştirmek için.
* Üst düzey iş akışları oluşturmak için alt runbook'lar olarak diğer grafik runbook'lar ve PowerShell iş akışı runbook'ları dahil edebilirsiniz.

### <a name="limitations"></a>Sınırlamalar
* Yazar PowerShell iş akışı ile bilgi sahibi olmanız gerekir.
* Runbook gerekir ilgili PowerShell iş akışı ek karmaşıklık ile gibi [nesneleri seri durumdan](automation-powershell-workflow.md#code-changes).
* Runbook çalıştırmadan önce derlenecek gerektiğinden PowerShell runbook'ları başlatmak için daha uzun sürer.
* PowerShell runbook'ları yalnızca yeni bir iş oluşturur başlangıç AzureAutomationRunbook cmdlet'ini kullanarak alt runbook'lar olarak eklenebilir.

## <a name="python-runbooks"></a>Python runbook'ları
Python runbook'ları Python 2 altında derleyin.  Azure portalında metin düzenleyicisi kullanarak runbook kodu doğrudan düzenleyebilirsiniz veya herhangi bir çevrimdışı metin düzenleyicisi kullanabilirsiniz ve [runbook'u içeri](http://msdn.microsoft.com/library/azure/dn643637.aspx) Azure Otomasyonu içine.

### <a name="advantages"></a>Avantajları
* Python sağlam standart kitaplığını kullanın.

### <a name="limitations"></a>Sınırlamalar
* Python komut dosyalarıyla bilgi sahibi olmanız gerekir.
* Yalnızca Python 2 Python 3 belirli işlevleri başarısız olur yani şu anda desteklenmiyor.

### <a name="known-issues"></a>Bilinen Sorunlar
Python runbook'ları bilinen geçerli sorunlar aşağıda verilmiştir.

* Üçüncü taraf kitaplıklar kullanmak için runbook çalıştırılması gereken bir [Windows karma Runbook çalışanı](https://docs.microsoft.com/azure/automation/automation-windows-hrw-install) veya [Linux karma Runbook çalışanı](https://docs.microsoft.com/azure/automation/automation-linux-hrw-install) önce makinede zaten yüklü kitaplıkları ile runbook başlatılır.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Belirli bir runbook için kullanılacak türün belirlerken aşağıdaki ek konuları dikkate almanız gerekir.

* Runbook'ları grafik metinsel türü ya da tam tersini dönüştüremezsiniz.
* Farklı türdeki runbook'lar bir alt runbook'u kullanarak sınırlamalar vardır.  Bkz: [alt runbook'ları Azure Automation](automation-child-runbooks.md) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
* Grafik runbook yazma hakkında daha fazla bilgi için bkz: [Azure Automation'da grafik yazma](automation-graphical-authoring-intro.md)
* PowerShell ve PowerShell arasındaki farkları anlamak için runbook'ları, iş akışları bkz [öğrenme Windows PowerShell iş akışı](automation-powershell-workflow.md)
* Oluşturma veya bir Runbook'u içeri hakkında daha fazla bilgi için bkz: [oluşturma veya bir Runbook'u içeri aktarma](automation-creating-importing-runbook.md)

