---
title: Azure Otomasyonu grafik runbook’larında hata işleme
description: Bu makalede, Azure Otomasyonu grafik runbook’larında hata işleme mantığının nasıl uygulanacağı açıklanmıştır.
services: automation
documentationcenter: ''
author: yunan2016
manager: digimobile
editor: tysonn
ms.assetid: ''
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/16/2018
ms.date: 05/14/2018
ms.author: v-nany
ms.openlocfilehash: d7fe38334b71334d4dae9235643117efdf5fbd5d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61233129"
---
# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Azure Otomasyonu grafik runbook’larında hata işleme

Bir runbook’un karşılaşabileceği farklı sorunların tanımlanması, göz önünde bulundurulması gereken temel bir runbook tasarım ilkesidir. Bu sorunlar arasında başarı, beklenen hata durumları ve beklenmeyen hata koşulları sayılabilir.

Runbook’lar hata işleme özelliğini içermelidir. Grafiksel runbook’lar ile ilgili bir etkinliğin çıktısını doğrulamak veya hatayı işlemek için bir Windows PowerShell kod etkinliği kullanabilir, etkinliğin çıktı bağlantısında koşullu mantığı tanımlayabilir veya başka bir yöntem uygulayabilirsiniz.          

Genellikle, bir runbook etkinliğiyle ilgili sonlandırıcı olmayan bir hata oluşursa, sonrasındaki herhangi bir etkinlik hataya bakılmaksızın işlenir. Hata büyük olasılıkla bir özel durum oluşturur, ancak sonraki etkinliğin çalışmasına yine de izin verilir. PowerShell, hataları işlemek için bu şekilde tasarlanmıştır.    

Yürütme sırasında oluşabilecek PowerShell hataları sonlandıran ve sonlandırmayan hatalar şeklinde iki türe ayrılır. Sonlandıran ve sonlandırıcı olmayan hatalar arasındaki farklar aşağıdaki gibidir:

* **Sonlandıran hata**: Yürütme sırasında gerçekleşen ve komutu (veya betiğin yürütülmesini) tamamen durduran ciddi bir hata. Örnek olarak mevcut olmayan cmdlet’ler, bir cmdlet’in çalışmasını önleyecek söz dizimi hataları veya diğer önemli hatalar verilebilir.

* **Sonlandırmayan hata**: Yürütme başarısız olmasına rağmen devam etmesini engellenmeyen ve ciddi olmayan bir hata. Örnek olarak bir dosyanın bulunamaması, hatalar ve izin sorunları yaşanması gibi işlemsel hatalar verilebilir.

Azure Otomasyonu grafik runbook'ları hata işleme özelliğiyle geliştirilmiştir. Bundan böyle özel durumları sonlandırıcı olmayan hatalara dönüştürebilir ve etkinlikler arasında hata bağlantıları oluşturabilirsiniz. Bu işlem bir runbook yazarının hataları yakalamasına ve gerçekleşen ya da beklenmeyen koşulları yönetmesine olanak tanır.  

## <a name="when-to-use-error-handling"></a>Hata işleme ne zaman kullanılır

Hata veya özel durum oluşturan kritik bir etkinlik olduğunda runbook’unuzdaki bir sonraki etkinliğin işlenmesini önlemek ve hatayı uygun bir şekilde işlemek önemlidir. Bu, özellikle de runbook’larınızın bir işletmeyi ya da hizmet işlemleri sürecini desteklediği durumlarda gereklidir.

Runbook yazarı, bir hata oluşturabilecek her bir etkinlik için başka herhangi bir etkinliği işaret eden bir hata bağlantısı ekleyebilir. Hedef etkinlik kod etkinliği, bir cmdlet’in çağrılması, başka bir runbook’un çağrılması gibi herhangi bir türde olabilir.

Ayrıca, hedef etkinliğin giden bağlantıları da olabilir. Bu bağlantılar normal bağlantılar veya hata bağlantıları olabilir. Başka bir deyişle, runbook yazarı bir kod etkinliğine başvurmadan karmaşık hata işleme mantığı uygulayabilir. Ortak işlevselliğe sahip bir özel hata işleme runbook’u oluşturulması önerilir, ancak zorunlu değildir. Bir PowerShell kod etkinliğinde hata işleme mantığı tek seçenek değildir.  

Örneğin, VM başlatmayı ve üzerine uygulama yüklemeyi deneyen bir runbook düşünün. VM doğru başlatılmazsa iki eylem gerçekleştirir:

1. Bu sorun hakkında bir bildirim gönderir.
2. Bunun yerine otomatik olarak yeni bir VM sağlayan başka bir runbook başlatır.

Bir çözümü ise birinci adımı işleyen etkinliği işaret eden bir hata bağlantısı oluşturulmasıdır. Örneğin, **Write-Warning** cmdlet’ini ikinci adımın **Start-AzureRmAutomationRunbook** cmdlet’i gibi bir etkinliğine bağlayabilirsiniz.

Ayrıca, bu iki etkinliği farklı bir hata işleme runbook’una ekleyerek ve daha önce önerilen kılavuzu izleyerek bu davranışı çok sayıda runbook’ta kullanılmak üzere genelleştirebilirsiniz. Bu hata işleme runbook’una bir çağrı yapmadan önce özgün runbook’taki verilerden özel bir ileti oluşturabilir ve bu iletiyi bir parametre olarak hata işleme runbook’una geçirebilirsiniz.

## <a name="how-to-use-error-handling"></a>Hata işleme nasıl kullanılır

Her etkinliğin özel durumları sonlandırıcı olmayan hatalara dönüştürmeye yönelik bir yapılandırma ayarı vardır. Bu ayar varsayılan olarak devre dışıdır. Hataları işlemek istediğiniz tüm etkinliklerde bu ayarı etkinleştirmeniz önerilir.  

Bu yapılandırmayı etkinleştirerek hem sonlandıran hem de sonlandırıcı olmayan etkinlik hatalarının sonlandırıcı olmayan hatalar olarak işlenmesini ve bir hata bağlantısıyla işlenebilmesini mümkün kılarsınız.  

Bu ayarı yapılandırdıktan sonra hatayı işleyen bir etkinlik oluşturursunuz. Bir etkinlik tarafından herhangi bir hata oluşturulursa giden hata bağlantıları izlenir ve etkinlik ek olarak normal bir çıkış oluşturmuş olsa bile normal bağlantılar izlenmez.<br><br> ![Otomasyon runbook’u hata bağlantısı örneği](media/automation-runbook-graphical-error-handling/error-link-example.png)

Aşağıdaki örnekte runbook, bir sanal makinenin bilgisayar adını içeren bir değişken alır. Ardından, sanal makineyi sonraki etkinlikle başlatmayı dener.<br><br> ![Otomasyon runbook’u hata işleme örneği](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

**Get-AutomationVariable** etkinliği ve **Start-AzureRmVm**, özel durumları hatalara dönüştürecek şekilde yapılandırılmıştır. Değişkeni alma veya VM’yi başlatma ile ilgili sorunlarla karşılaşılırsa hata oluşturulur.<br><br> ![Otomasyon runbook’u hata işleme etkinlik ayarları](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png)

Hata bağlantıları bu etkinliklerden tek bir **hata yönetimi** etkinliğine (kod etkinliği) geçer. Bu etkinlik, geçerli özel durumu açıklayan *$Error.Exception.Message* ile birlikte *Throw* anahtar sözcüğünü kullanan bir PowerShell ifadesiyle birlikte yapılandırılır.<br><br> ![Otomasyon runbook’u hata işleme kod örneği](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)


## <a name="next-steps"></a>Sonraki adımlar

* Bağlantılar ve grafik runbook’larındaki bağlantı türleri hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu’nda grafik yazma](automation-graphical-authoring-intro.md#links-and-workflow).

* Runbook yürütme, runbook işlerini izleme ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [Runbook işi izleme](automation-runbook-execution.md).
