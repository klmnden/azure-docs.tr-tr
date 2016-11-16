---
title: Azure Automation Nedir? | Microsoft Belgeleri
description: "Azure Automation’ın sağladığı değerleri öğrenin ve runbook’lar oluşturmaya, kullanmaya ve Azure Automation DSC kullanmaya başlayabilmeniz için sık sorulan soruların yanıtlarını alın."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "otomasyon nedir, azure otomasyonu, azure otomasyonu örnekleri"
ms.assetid: 0cf1f3e8-dd30-4f33-b52a-e148e97802a9
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2016
ms.author: magoedte;bwren
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: b00cf526bfed1e8d8962127439f1e41225b8023d


---
# <a name="azure-automation-overview"></a>Azure Automation’a genel bakış
Microsoft Azure Automation, kullanıcılara bir bulutta ve kurumsal ortamda sık olarak gerçekleştirilen el ile, uzun süreli, hatasız ve sık tekrarlanan görevleri otomatik hale getirmek için bir yol sunar. Bu zamandan tasarruf sağlar ve normal yönetim görevlerinin güvenilirliğini artırır ve hatta bunları düzenli aralıklarla otomatik olarak gerçekleştirilecek şekilde zamanlar. Runbook’ları kullanarak işlemleri otomatik hale getirebilir ya da İstenen Durum Yapılandırması’nı kullanarak yapılandırmayı otomatik hale getirebilirsiniz. Bu makale, Azure Automation’a ilişkin kısa bir genel bakış sağlar ve bazı sık sorulan soruları yanıtlar. Farklı konulara ilişkin daha ayrıntılı bilgiler için bu kitaplıktaki diğer makalelere başvurabilirsiniz.

## <a name="automating-processes-with-runbooks"></a>Runbook'larla işlemleri otomatik hale getirme
Bir runbook, Azure Automation’da bazı otomatik işlemleri gerçekleştiren görevler grubudur. Bu, bir sanal makineyi başlatma ve günlük girişi oluşturma gibi basit bir işlem olabilir ya da birden fazla kaynakta ve hatta birden fazla bulutta veya şirket içi ortamda karmaşık bir işlemi gerçekleştirmek için diğer küçük runbook’ları birleştiren karmaşık bir runbook’unuz olabilir.  

Örneğin, sunucuya bağlanma, veritabanına bağlanma, veritabanının mevcut boyutunu alma, eşiğin aşılıp aşılmadığını kontrol etmek ve ardından kesme ve kullanıcıyı bilgilendirme gibi birden fazla adımı içeren maksimum boyuta yaklaşıyorsa, SQL veritabanını kesmek üzere el ile yapılan bir işleminiz olabilir.  Bu adımların her birini el ile gerçekleştirmek yerine, tüm bu adımları tek bir işlem olarak gerçekleştirecek bir runbook oluşturabilirsiniz. Runbook’u çalıştırır, SQL sunucusu adı, veritabanı adı ve alıcı e-posta adresi gibi gerekli bilgileri girer ve işlem tamamlanırken arkanıza yaslanırsınız. 

## <a name="what-can-runbooks-automate"></a>Runbook’lar neleri otomatik hale getirir?
Azure Automation’daki runbook’lar Windows PowerShell ya da Windows PowerShell İş Akışı tabanlıdır, bu nedenle bunlar PowerShell’in yapabileceği her şeyi yapar. Bir uygulama veya hizmetin API’si varsa, bir runbook bununla çalışabilir. Uygulama için bir PowerShell modülünüz varsa, bu modülü Azure Automation’a yükleyebilir ve bu cmdlet’leri runbook’unuza ekleyebilirsiniz. Azure Automation runbook’ları Azure bulutunda çalışır ve bulut kaynaklarına veya herhangi bir buluttan erişilebilen dış kaynaklara erişebilir. [Karma Runbook Çalışanı](automation-hybrid-runbook-worker.md) kullanarak, runbook'lar yerel kaynakları yönetmek için yerel veri merkezinizde çalışabilir. 

## <a name="getting-runbooks-from-the-community"></a>Topluluktan runbook'ları alma
[Runbook Galerisi](automation-runbook-gallery.md#runbooks-in-runbook-gallery) Microsoft’tan ve topluluktan ortamınızda değiştirmeden kullanabileceğiniz ya da kendi amaçlarınız doğrultusunda özelleştirebileceğiniz runbook’ları içerir. Bunlar, ayrıca kendi runbook’larınızı oluşturmada başvurular olarak da faydalıdır. Hatta diğer kullanıcıların faydalı bulabileceğini düşündüğünüz kendi runbook’arınızla galeriye katkıda bulanabilirsiniz. 

## <a name="creating-runbooks-with-azure-automation"></a>Azure Automation ile Runbook’ları oluşturma
En baştan [kendi runbook'larınızı oluşturabilir](automation-creating-importing-runbook.md) ya da kendi gereksinimleriniz doğrultusunda [Runbook Galerisi](http://msdn.microsoft.com/library/azure/dn781422.aspx)’nden aldığınız runbook’ları değiştirebilirsiniz. Gereksinimlerinize ve PowerShell deneyiminize göre seçebileceğiniz üç farklı [runbook türü](automation-runbook-types.md) vardır. Doğrudan PowerShell kodu ile çalışmayı tercih ediyorsanız, çevrimdışı veya Azure portaldaki [metin düzenleyicisi](http://msdn.microsoft.com/library/azure/dn879137.aspx)nde düzenleyebileceğiniz bir [PowerShell runbook’u](automation-runbook-types.md#powershell-runbooks) ya da [PowerShell İş Akışı runbook’u](automation-runbook-types.md#powershell-workflow-runbooks) kullanabilirsiniz. Temeldeki kodu görmeden bir runbook'u düzenlemek isterseniz, Azure portaldaki [grafik düzenleyicisi](automation-graphical-authoring-intro.md)ni kullanarak [Grafik runbook’u](automation-runbook-types.md#graphical-runbooks) oluşturabilirsiniz. 

Okumak yerine izlemeyi mi tercih ediyorsunuz? Mayıs 2015 tarihli Microsoft Ignite oturumuna ait aşağıdaki videoya gözatın. Not: Bu videoda ele alınan kavramlar ve özellikler doğru olsa da, Azure Automation bu videonun kaydedilmesinden sonra değişmiştir; Azure Portal’da daha geniş bir kullanıcı arabirimine sahiptir ve ek özellikleri desteklemektedir.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3451/player]
> 
> 

## <a name="automating-configuration-management-with-desired-state-configuration"></a>İstenen Durum Yapılandırması ile yapılandırma yönetimini otomatik hale getirme
[PowerShell DSC](https://technet.microsoft.com/library/dn249912.aspx) tanımlayıcı bir PowerShell söz dizimi kullanan fiziksel ana bilgisayarlar ve sanal makineler için yapılandırmayı yönetmenizi, dağıtmanızı ve uygulamanızı sağlayan bir yönetim platformudur. Merkezi bir DSC Çekme Sunucusu’nda hedef makinelerin otomatik olarak alabileceği ve uygulayabileceği yapılandırmaları tanımlayabilirsiniz. DSC, yapılandırmaları ve kaynakları yönetmek için kullanabileceğiniz bir PowerShell cmdlet'leri grubu sağlar.  

[Azure Automation DSC](automation-dsc-overview.md), PowerShell DSC’ye yönelik kurumsal ortamlar için hizmetler için gerekli hizmetleri sağlayan bulut tabanlı bir çözümdür.  Azure Automation DSC kaynaklarınızı yönetebilir ve bunları Azure buluttaki DSC Çekme Sunucusu’ndan alana sanal veya fiziksel makinelere uygulayabilirsiniz.  Bu ayrıca, düğümlerin kendilerine atanan yapılandırmalardan sapması ve yeni bir yapılandırma uygulanması gibi önemli olayları size bildiren raporlama hizmetleri de sağlar. 

## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Azure Automation’da kendi DSC yapılandırmalarınızı oluşturma
[DSC yapılandırmaları](automation-dsc-overview.md#azure-automation-dsc-terms) istenen düğüm durumunu belirtir.  Birden çok düğüm, tümünün aynı durumu yapılandırmayı sağlamak üzere aynı yapılandırmayı uygulayabilir.  Yerel makinenizde bir metin düzenleyicisi kullanarak yapılandırma oluşturabilir ve ardından bunu derleyebileceğiniz ve düğümlerini uygulayabileceğiniz Azure Automation’a aktarabilirsiniz.

## <a name="getting-modules-and-configurations"></a>Modülleri ve yapılandırmaları alma
Runbook’larınızda ve DSC yapılandırmalarınızda kullanabileceğiniz cmdlet’leri içeren [PowerShell modüllerini](automation-runbook-gallery.md#modules-in-powershell-gallery) [PowerShell Galerisi](http://www.powershellgallery.com/)’nden alabilirsiniz. Bu galeriyi Azure portaldan başlatabilir ve modülleri doğrudan Azure Automation’a aktarabilir ya da bunları el ile indirebilir ve içeri aktarabilirsiniz. Modülleri doğrudan Azure portalına yükleyemezsiniz, ancak bunları herhangi başka bir modül gibi indirebilir ve yükleyebilirsiniz. 

## <a name="example-practical-applications-of-azure-automation"></a>Azure Automation’ın örnek pratik uygulamalar
Aşağıda Azure Automation otomasyon senaryosu türlerine ilişkin yalnızca birkaç örnek verilmiştir. 

* Farklı Azure aboneliklerinde sanal makineler oluşturun ve kopyalayın. 
* Yerel bir makinedeki dosya kopyalarını Azure Blob Storage kapsayıcısına zamanlayın. 
* Bir hizmet reddi saldırısı algılandığında,istekleri reddetme gibi güvenlik işlevlerini otomatik hale getirin. 
* Makinelerin yapılandırılmış güvenlik ilkesiyle sürekli hizalanmasını sağlayın.
* Uygulana kodunun buluta ve şirket içi altyapıya sürekli dağıtımını yönetin. 
* Azure’da laboratuvar ortamınız için bir Active Directory ormanı oluşturun. 
* DB en büyük boyuta yaklaşıyorsa, SQL veritabanındaki bir tabloyu kesin. 
* Bir Azure Web sitesi için ortam ayarlarını uzaktan güncelleştirin. 

## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Azure Automation’ın diğer otomasyon araçları ile nasıl ilişkilidir?
[Service Management Automation (SMA)](http://technet.microsoft.com/library/dn469260.aspx) özel bulutta yönetim görevlerini otomatik hale getirmek için tasarlanmıştır. [Microsoft Azure Pack](https://www.microsoft.com/en-us/server-cloud/) bileşeni olarak veri merkezinize yerel olarak yüklenir. SMA ve Azure Automation Windows PowerShell ve Windows PowerShell İş Akışı’nı temel alan aynı runbook biçimini kullanır, ancak SMA desteklemiyor [grafik runbook'lar](automation-graphical-authoring-intro.md)ı desteklemez.  

[System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) şirket içi kaynakların otomasyonuna yöneliktir. Azure Automation ve Service Management Automation’dan farklı bir runbook biçimini kullanır ve betiklere gerek kalmadan runbook oluşturmak için grafik arabirimine sahiptir. Runbook'ları, Tümleştirme Paketleri’nde bulunan özellikle Orchestrator için yazılmış olan etkinliklerden oluşur. 

## <a name="where-can-i-get-more-information"></a>Daha fazla bilgiyi nereden bulabilirim?
Azure Automation ve kendi runbook'larınızı oluşturma hakkında daha fazla bilgi almak için kullanabileceğiniz çeşitli kaynaklar vardır. 

* **Azure Automation Kitaplığı** şu anda bulunduğunuz yerdir. Bu kitaplıktaki makaleler Azure Automation yapılandırılması ve yönetimi ve kendi runbook'larınızı yazma konusunda kapsamlı belgeler sağlar. 
* [Azure PowerShell cmdlet'leri](http://msdn.microsoft.com/library/jj156055.aspx) Windows PowerShell kullanarak Azure işlemlerini otomatik hale getirme hakkında bilgi sağlar. Runbook'ları Azure kaynakları ile çalışmak için bu cmdlet'leri kullanır. 
* [Yönetim Web günlüğü](https://azure.microsoft.com/blog/tag/azure-automation/) Microsoft’tan Azure Automation ve diğer yönetim teknolojilerine ilişkin en son bilgileri sağlar. Azure Automation ekibinin en son verdiği bilgilerle güncel kalma için bu web günlüğüne abone olmalısınız. 
* [Automation Forumu](http://go.microsoft.com/fwlink/p/?LinkId=390561) Microsoft ve Automation topluluk tarafından ele alınması için Azure Automation hakkında sorularınızı yayınlamanıza olanak sağlar. 
* [Azure Automation Cmdlet'leri](https://msdn.microsoft.com/library/mt244122.aspx) yönetim görevlerini otomatik hale getirmek için bilgiler sağlar. Bu, Automation hesaplarını, varlıkları, runbook’ları, DSC’yi, yönetmek için cmdlet’leri içerir.

## <a name="can-i-provide-feedback"></a>Geribildirim sağlayabilir miyim?
**Lütfen bize geri bildirimde bulunun.** Bir Azure Automation runbook çözümü veya bir tümleştirme modülü arıyorsanız, Betik Merkezi'ne bir Betik İsteği gönderin. Azure Automation ile ilgili geribildirim ya da özellik isteğiniz varsa, [User Voice](http://feedback.windowsazure.com/forums/34192--general-feedback)’da yayınlayın. Teşekkür ederiz! 




<!--HONumber=Nov16_HO2-->


