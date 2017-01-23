---
title: "Azure Otomasyonu Grafik Runbook’larında Hata İşleme | Microsoft Belgeleri"
description: "Bu makalede, Azure Otomasyonu grafik runbook’larında hata işleme mantığının nasıl uygulanacağı açıklanmıştır."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/26/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: f9b359691122da9e5d93e51f3085cad51e55d8f2
ms.openlocfilehash: 13c3e1693badcf4148738cb63666f34546d1696c

---

# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Azure Otomasyonu grafik runbook’larında hata işleme

Göz önünde bulundurmanız gereken temel bir runbook tasarımı prensibi, bir runbook’un yaşayabileceği çeşitli sorunları (başarı, beklenen hata durumları ve beklenmeyen hata koşulları gibi) belirlemektir.  Runbook’ların sorunları buna uygun olarak algılayabilmesi için hata işleme içermesi gerekir.  Grafik runbook’ları ile bir etkinliğin çıkışını doğrulamak ya da bir hatayı uygun bir şekilde işlemek istediğinizde gerçekleştireceğiniz olası işlemler bir PowerShell kod etkinliğini kullanmak, etkinliğin çıkış bağlantısında koşullu mantığı tanımlamak veya başka bir yöntem uygulamak olur.          

Çoğu zaman, runbook’larınız yürütüldüğünde bir etkinlikte gerçekleşen ve işlemin sonlandırılmasına neden olmayan bir hata gerçekleşirse hatadan sonraki tüm etkinlikler bundan bağımsız olarak işlenir.  Bir özel durumun oluşturulması doğal olarak yüksek bir ihtimal olsa da asıl önemli olan nokta bir sonraki etkinliğin yürütülmesine izin verilmesidir. Bu davranışın altında yatan neden, PowerShell dilinin tasarım itibarıyla hataları işleme biçimidir.    

Yürütme sırasında oluşabilecek PowerShell hataları sonlandıran ve sonlandırmayan hatalar şeklinde iki türe ayrılır.  Bunların arasındaki fark aşağıda açıklanmıştır:

* Sonlandıran hata: Yürütme sırasında gerçekleşen ve komutu (veya betiğin yürütülmesini) tamamen durduran ciddi bir hata. Örnek olarak mevcut olmayan cmdlet’ler, bir cmdlet’in çalışmasını önleyecek söz dizimi hataları veya diğer önemli hatalar verilebilir.

* Sonlandırmayan hata: Hataya rağmen yürütme işleminin devam etmesini engellenmeyen ve ciddi olmayan bir hata. Örnek olarak bir dosyanın bulunamaması, izin sorunları yaşanması gibi işlemsel hatalar verilebilir.

Azure Otomasyonu grafik runbook’ları hata işleme özelliğini içerecek şekilde geliştirildiğinden, artık özel durumları sonlandırmayan hatalara dönüştürebilir ve etkinlikler arasında hata bağlantıları oluşturabilirsiniz. Bu sayede, bir runbook yazarı hataları yakalayabilir ve gerçekleşen ya da beklenmeyen koşulun yönetilmesi için bir yola sahip olur.  

## <a name="when-to-use"></a>Kullanılması gereken durumlar

İş akışının yürütülmesini denetleme olanağı, bir hata veya özel durum oluşturan kritik bir etkinlik olduğunda runbook’unuzdaki bir sonraki etkinliğe geçilmesini engelleyebilmeniz ve hatayı uygun bir şekilde işleyebilmeniz açısından önemlidir.  Bu, özellikle de runbook’larınızın bir işletmeyi ya da hizmet işlemleri sürecini desteklediği durumlarda gereklidir.

Runbook yazarı, bir hata oluşturabilecek her bir etkinlik için başka herhangi bir etkinliği işaret eden bir hata bağlantısı ekleyebilir.  Hedef etkinlik herhangi bir türde olabilir: Kod etkinliği, bir cmdlet’in çağrılması, başka bir runbook’un çağrılması, vs. 

Üstelik, hedef etkinlik giden bağlantılara da sahip olabileceğinden ve bunlar normal bağlantılar veya hata bağlantıları olabileceğinden, runbook yazarı bir kod etkinliği eklemek zorunda kalmadan karmaşık hata işleme mantığı uygulayabilir.  Önerilen uygulama genel işlevlere sahip adanmış bir hata işleme runbook’u oluşturmak olsa da bu zorunlu değildir ve bir PowerShell kod etkinliğinde hata işleme mantığı tek alternatif değildir.  

Örneğin, bir VM’yi çalıştırmaya ve VM üzerinde bir uygulamayı yüklemeye çalışan bir runbook olduğunu düşünün. VM doğru başlatılamazsa runbook iki eylem gerçekleştirir: 

1. Bu sorun hakkında bir bildirim gönderme.
2. Bunun yerine otomatik olarak yeni bir VM sağlayan başka bir runbook başlatma. 

Uygulanabilecek çözümlerden birisi, 1. adımın işlenmesine yönelik bir etkinliği işaret eden bir hata bağlantısı oluşturmaktır. Örneğin, 2. adım için bir etkinliğe (**Start-AzureRmAutomationRunbook** cmdlet’i gibi) bağlı **Write-Warning** cmdlet’i gibi. 

Ayrıca, bu davranışı çok sayıda runbook’ta kullanılmak üzere genelleştirebilir ve daha önce önerilen kılavuzu izleyerek bu iki etkinliği farklı bir hata işleme runbook’una ekleyebilirsiniz.  Bu hata işleme runbook’una bir çağrı yapmadan önce özgün runbook’taki verilerden özel bir ileti oluşturabilir ve bu iletiyi bir parametre olarak hata işleme runbook’una geçirebilirsiniz. 

## <a name="how-to-use"></a>Nasıl kullanılır

Her etkinliğin özel durumları sonlandırmayan hatalara dönüştürmeye yönelik bir yapılandırması vardır. Bu yapılandırma varsayılan olarak devre dışıdır.  Hataları işlemek istediğiniz tüm etkinliklerde bunu etkinleştirmelisiniz.  Bu yapılandırmayı etkinleştirerek hem sonlandıran hem de sonlandırmayan etkinlik hatalarının sonlandırmayan hatalar olarak işlenmesini ve bir hata bağlantısıyla işlenebilmesini mümkün kılarsınız.  Bu ayarı yapılandırdıktan sonra hatayı işleyecek bir etkinlik oluşturursunuz.  Bir etkinlik tarafından herhangi bir hata oluşturulursa giden hata bağlantıları izlenir ve etkinlik ek olarak normal bir çıkış oluşturmuş olsa bile normal bağlantılar izlenmez.<br><br> ![Otomasyon Runbook’u Hata Bağlantısı Örneği](media/automation-runbook-graphical-error-handling/error-link-example.png)

Aşağıdaki basit örnekte, bir sanal makinenin bilgisayar adını içeren bir değişkeni alan ve bir sonraki etkinlikle sanal makineyi başlatmaya çalışan bir runbook var.<br><br> ![Otomasyon Runbook’u Hata İşleme Örneği](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

**Get-AutomationVariable** etkinliği ve **Start-AzureRmVm**, özel durumları hatalara dönüştürecek şekilde yapılandırılmıştır.  Değişkeni alma ya da VM’yi başlatma konusunda bir sorun yaşanırsa hatalar oluşturulur.<br><br> ![Otomasyon Runbook’u Hata İşleme Etkinlik Ayarları](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png)

Hata bağlantıları, bu etkinliklerden işlemeyi durdurmaya yönelik *Throw* anahtar kelimesinin yanı sıra geçerli özel durumu açıklayan iletinin alınması için *$Error.Exception.Message* kullanılarak basit bir PowerShell ifadesiyle yapılandırılmış tek bir **Hata Yönetimi** etkinliğine (bir Kod etkinliği) gönderilir.<br><br> ![Otomasyon Runbook’u Hata İşleme Kod Örneği](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)


## <a name="next-steps"></a>Sonraki adımlar

* Bağlantılar hakkında daha fazla bilgi edinmek ve grafik runbook’larındaki bağlantı türlerini anlamak için bkz. [Azure Otomasyonu’nda Grafiksel Yazma](automation-graphical-authoring-intro.md#links-and-workflow).

* Runbook yürütme, runbook işlerini izleme ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [Runbook işi izleme](automation-runbook-execution.md) 


<!--HONumber=Jan17_HO1-->


