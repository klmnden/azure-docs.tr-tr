---
title: Azure Automation Runbook türleri
description: "Azure Otomasyonu ve kullanmak için tür belirlerken dikkate almanız önemli noktalar kullanabileceğiniz runbook'ları farklı türleri açıklanmaktadır. "
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 03/05/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8d90f1c03a9ed892a9dc8a4fecbeb9815c2dd376
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476894"
---
# <a name="azure-automation-runbook-types"></a>Azure Automation runbook türleri

Azure Otomasyonu aşağıdaki tabloda kısaca açıklanmış runbook'ları çeşitli türlerini destekler.  Aşağıdaki bölümlerde, her zaman hakkında dikkat edilecek noktalar dahil olmak üzere her tür hakkında daha fazla bilgi verilmektedir.

| Tür | Açıklama |
|:--- |:--- |
| [Grafik](#graphical-runbooks)|Windows PowerShell ve Azure portalında oluşturulan ve tamamen düzenlenen grafik düzenleyicisini temel. |
| [Grafik PowerShell iş akışı](#graphical-runbooks)|Windows PowerShell iş akışına dayalı, oluşturulan ve Azure portalında grafik düzenleyicisinde düzenlenir. |
| [PowerShell](#powershell-runbooks) |Windows PowerShell komut dosyasına dayalı metin runbook. |
| [PowerShell İş Akışı](#powershell-workflow-runbooks)|Windows PowerShell iş akışına dayalı metin runbook. |
| [Python](#python-runbooks) |Python tabanlı metin runbook. |

## <a name="graphical-runbooks"></a>Grafik runbook'ları

[Grafik](automation-runbook-types.md#graphical-runbooks) ve grafik PowerShell iş akışı runbook'ları oluşturulur ve Azure portalında Grafik düzenleyicisiyle düzenlenir.  Bunları bir dosyaya dışarı aktarmak ve ardından bunları başka bir Otomasyon hesabına aktarın. Ancak oluşturamaz veya bunları başka bir aracı ile düzenleyin. PowerShell kodu grafik runbook'ları oluşturmak, ancak doğrudan görüntüleyemez veya kodu değiştirin. Grafik runbook'ları bir sürüme dönüştürülemez [metin biçimleri](automation-runbook-types.md), ya da grafik biçiminde bir metin runbook dönüştürülebilir. Grafik runbook'ları içeri aktarma ve aksi yönde sırasında grafik PowerShell iş akışı runbook'larına dönüştürülebilir.

### <a name="advantages"></a>Yararları

* Ekle bağlantısını yapılandırma Visual geliştirme modeli  
* Verilerin sürecinde nasıl aktığını odaklanın  
* Yönetim süreçlerini görsel olarak temsil eder  
* Üst düzey iş akışları oluşturmak için alt runbook'lar olarak diğer runbook'ları Ekle  
* Modüler programlama teşvik eder  

### <a name="limitations"></a>Sınırlamalar

* Azure portal dışında runbook'u düzenleyemez.
* Karmaşık mantık yürütmek amacıyla PowerShell kodu içeren bir kod etkinliği gerektirebilir.
* Görüntüleyemez veya doğrudan grafik iş akışı tarafından oluşturulan PowerShell kodu düzenleyin. Herhangi bir kod etkinlik oluşturma kodu görüntüleyebilirsiniz.
* Olamaz Linux karma Runbook çalışanı üzerinde çalışan olabilir

## <a name="powershell-runbooks"></a>PowerShell runbook'ları

PowerShell runbook'ları Windows PowerShell üzerinde temel alır.  Doğrudan Azure portalında metin düzenleyiciyi kullanarak runbook'u kodunu düzenleme.  Herhangi bir çevrimdışı metin düzenleyicisi kullanabilirsiniz ve [runbook'u içeri aktar](manage-runbooks.md) Azure Otomasyonu ile.

### <a name="advantages"></a>Yararları

* PowerShell iş akışı ek karmaşıklığını olmadan PowerShell kod ile tüm karmaşık mantığı uygulayın.
* Runbook hızlı bir şekilde PowerShell iş akışı runbook'ları çalıştırmadan önce derlenmesi gerekli olmayan bu yana başlatır.
* Olması çalıştırdığınız azure'da ya da hem Linux hem de Windows karma Runbook çalışanları

### <a name="limitations"></a>Sınırlamalar

* PowerShell komut dosyalarıyla ilgili bilgi sahibi olmanız gerekir.
* Kullanamazsınız [paralel işleme](automation-powershell-workflow.md#parallel-processing) paralel olarak birden fazla eylem yürütülecek.
* Kullanamazsınız [kontrol noktaları](automation-powershell-workflow.md#checkpoints) bir hata varsa, runbook devam etmek için.
* PowerShell iş akışı runbook'ları ve grafik runbook'ları yalnızca yeni bir iş oluşturur başlangıç AzureAutomationRunbook cmdlet'ini kullanarak alt runbook'lar olarak dahil edilebilir.

### <a name="known-issues"></a>Bilinen Sorunlar

PowerShell runbook'ları bilinen geçerli sorunlar aşağıda verilmiştir.

* PowerShell runbook'ları bir şifrelenmemiş alamıyor [değişken varlığı](automation-variables.md) null değerine sahip.
* PowerShell runbook'ları alamıyor bir [değişken varlığı](automation-variables.md) ile *~* adı.
* GET-işleminde, bir döngüde bir PowerShell runbook yaklaşık 80 yinelemeden sonra kilitlenebilir.
* Tek seferde büyük miktarda veri çıkış akışına yazmak çalışırsa bir PowerShell runbook başarısız olabilir.   Genellikle, yalnızca büyük nesneler ile çalışırken, ihtiyacınız olan bilgileri çıktısı tarafından bu sorunu geçici olarak çalışabilir.  Örneğin, aşağıdaki gibi çıktısı yerine *Get-Process*, yalnızca gerekli alanları ile çıktı *Get-Process | ProcessName, CPU seçin*.

## <a name="powershell-workflow-runbooks"></a>PowerShell iş akışı runbook'ları

PowerShell iş akışı runbook'ları, dayalı metin runbook'larıdır [Windows PowerShell iş akışı](automation-powershell-workflow.md).  Doğrudan Azure portalında metin düzenleyiciyi kullanarak runbook'u kodunu düzenleme.  Herhangi bir çevrimdışı metin düzenleyicisi kullanabilirsiniz ve [runbook'u içeri aktar](manage-runbooks.md) Azure Otomasyonu ile.

### <a name="advantages"></a>Yararları

* PowerShell iş akışı kod ile tüm karmaşık mantığı uygulayın.
* Kullanım [kontrol noktaları](automation-powershell-workflow.md#checkpoints) bir hata varsa, runbook devam etmek için.
* Kullanım [paralel işleme](automation-powershell-workflow.md#parallel-processing) paralel olarak birden fazla eylem gerçekleştirmek için.
* Üst düzey iş akışları oluşturmak için alt runbook'lar olarak diğer grafik runbook'ları ve PowerShell iş akışı runbook'ları içerebilir.

### <a name="limitations"></a>Sınırlamalar

* Yazar, PowerShell iş akışı ile ilgili bilgi sahibi olmanız gerekir.
* Runbook gerekir işlem PowerShell iş akışı ile ek karmaşıklığı gibi [nesneleri seri durumdan](automation-powershell-workflow.md#code-changes).
* Runbook çalıştırmadan önce derlenecek sonun PowerShell runbook'ları başlatmak için daha uzun sürer.
* PowerShell runbook'ları yalnızca yeni bir iş oluşturur başlangıç AzureAutomationRunbook cmdlet'ini kullanarak alt runbook'lar olarak dahil edilebilir.
* Olamaz Linux karma Runbook çalışanı üzerinde çalışan olabilir

## <a name="python-runbooks"></a>Python runbook'ları

Python runbook'ları Python 2 kapsamında derleyin.  Azure portalında veya çevrimdışı bir metin düzenleyicisi metin düzenleyiciyi kullanarak runbook'un kodu doğrudan düzenleyebilirsiniz ve [runbook'u içeri aktar](manage-runbooks.md) Azure Otomasyonu ile.

### <a name="advantages"></a>Yararları

* Güçlü bir Python kitaplıkları kullanın.
* Olması çalıştı azure'da ya da hem Linux karma Runbook çalışanları. Windows karma Runbook çalışanları ile desteklenen [python2.7](https://www.python.org/downloads/release/latest/python2) yüklü.

### <a name="limitations"></a>Sınırlamalar

* Python betik oluşturma ile ilgili bilgi sahibi olmanız gerekir.
* Python 2 şu anda Python 3 belirli işlevleri başarısız olur, yani desteklenir.
* Üçüncü taraf kitaplıklar kullanan için [paketini içeri aktarma](python-packages.md) Otomasyon hesabına onun için kullanılacak.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Belirli bir runbook için kullanılacak türün belirlerken aşağıdaki ek konuları göz önünde bulundurur.

* Runbook'ları grafik metin türü veya tersine dönüştüremezsiniz.
* Runbook'ları farklı türde bir alt runbook kullanarak sınırlamalar vardır. Daha fazla bilgi için [Azure automation'da alt runbook'lar](automation-child-runbooks.md).

## <a name="next-steps"></a>Sonraki adımlar

* Grafik runbook yazma hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda grafik yazma](automation-graphical-authoring-intro.md)
* PowerShell ve PowerShell arasındaki farkları görmek için runbook'ları, iş akışlarını [Windows PowerShell iş akışını öğrenme](automation-powershell-workflow.md)
* Oluşturma veya bir Runbook'u içeri aktarma hakkında daha fazla bilgi için bkz. [oluşturma veya bir Runbook'u içeri aktarma](manage-runbooks.md)

