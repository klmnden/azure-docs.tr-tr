---
title: Azure Operations Management Suite (OMS) Belgeleri - Öğreticileri | Microsoft Docs
description: Microsoft Operations Management Suite (OMS), şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan, Microsoft'un bulut tabanlı BT yönetim çözümüdür. Bu makalede, OMS'de yer alan farklı hizmetler tanımlanmakta ve bunların ayrıntılı içeriklerinin bağlantıları sağlanmaktadır.
services: operations-management-suite
author: czeumault
manager: carolz
layout: LandingPage
ms.assetid: ''
ms.service: operations-management-suite
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: landing-page
ms.date: 01/23/2017
ms.author: carolz
ms.openlocfilehash: 12f959376d4923e4e2481e37108ade632ac14902
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23071220"
---
# <a name="what-is-operations-management-suite-oms"></a>Operations Management Suite (OMS) nedir?
Microsoft Operations Management Suite (OMS), şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan, Microsoft'un bulut tabanlı BT yönetim çözümüdür.  OMS, bulut tabanlı bir hizmet olarak uygulandığından altyapı hizmetlerine en az yatırımla onu kısa süre içinde çalışır duruma getirebilirsiniz.  Yeni özellikler otomatik olarak sunulduğu için, devam eden bakım ve yükseltme maliyetlerinden tasarruf sağlarsınız.

OMS, değerli hizmetleri tek başına sunabildiği gibi, mevcut yönetim yatırımlarınızın kapsamını buluta kadar genişletmek için System Center Operations Manager gibi System Center bileşenleriyle de tümleştirilebilir.  Tamamen karma bir yönetim deneyiminin elde edilebilmesi için System Center ve OMS birlikte çalışabilir.

Aşağıdaki bölümlerde OMS'nin fayda sağladığı farklı alanların ve bunları uygulayan hizmetlerin ayrıntılı bir açıklaması verilmektedir.  Her birine ilişkin belgeleri ayrıntılı olarak gözden geçirmeden önce farklı OMS bileşenlerine genel bakış için OMS mimarisine bakabilirsiniz.

## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Insight and Analytics](media/operations-management-suite-overview/icon-insight-analytics.png) Insight and Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics), işletim sistemleri ve uygulamalar tarafından oluşturulan günlük ve performans verilerini toplamanıza, bunlar arasında bağıntı kurmanıza, bu veriler üzerinde arama ve işlem yapmanıza yardımcı olur. Fiziksel konumlarından bağımsız olarak tüm iş yükleriniz ve sunucularınız üzerindeki milyonlarca kaydı kolayca çözümleyebilmeniz için tümleşik arama işlevini ve özel panoları kullanarak size gerçek zamanlı operasyonel öngörüler sunar.

Log Analytics'e, toplanacak verileri ve verilerin çözümlemesine yönelik mantığı tanımlayan çözümleri kolayca ekleyebilirsiniz.  Çözümler en az yapılandırmayla veya hiç yapılandırma olmadan aracılara otomatik olarak sunulan ek işlevler içerebilir.  Ayrı çözümler tarafından sağlanan çözümleme araçlarını kullanabilmenin yanı sıra, sistemler ile uygulamalar arasında veri bağıntısı kurmak için veri kümesi genelinde özel aramalar gerçekleştirebilirsiniz.  

## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Otomasyon ve Denetim](media/operations-management-suite-overview/icon-automation-control.png) Otomasyon ve Denetim
Azure Otomasyonu, Azure bulutunda çalışan PowerShell tabanlı [runbook'lar](../automation/automation-runbook-types.md) ile yönetim işlemlerini otomatikleştirir.  Runbook'lar, Amazon Web Services (AWS) gibi diğer bulutlardaki kaynaklar da dahil olmak üzere PowerShell ile yönetilebilen ürünlere veya hizmetlere erişebilir.  Runbook'lar yerel kaynakların yönetilmesi için yerel veri merkezinizdeki bir sunucuda da yürütülebilir.

Azure Otomasyonu, [PowerShell DSC](../automation/automation-dsc-overview.md) ile yapılandırma yönetimi sağlar.  DSC kaynakları oluşturabilir, Azure'da barındırılan DSC kaynaklarını yönetebilir ve bunların yapılandırmasını tanımlamak ve otomatik olarak zorunlu kılmak için bu kaynakları bulut sistemlerine ve şirket içi sistemlere uygulayabilirsiniz.

## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Koruma ve Kurtarma](media/operations-management-suite-overview/icon-protection-recovery.png) Koruma ve Olağanüstü Durum Kurtarma
[Azure Backup](http://azure.microsoft.com/documentation/services/backup), uygulama verilerinizi korur ve herhangi bir sermaye yatırımı olmadan en düşük işletim giderleriyle yıllar boyunca saklar.  SQL Server ve SharePoint gibi uygulama iş yüklerinin yanı sıra fiziksel ve sanal Windows sunucularındaki verileri yedekleyebilir.  Bu ayrıca yedeklilik ve uzun vadeli depolama açısından, korumalı verilerin Azure'a yönelik olarak çoğaltılması için System Center Data Protection Manager (DPM) tarafından da kullanılabilir.

[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery); çoğaltma, yük devretme ve şirket içi Hyper-V sanal makinelerini, VMware sanal makinelerini, fiziksel Windows/Linux sunucularını kurtarma işlemlerini düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı sağlar. Makineleri ikincil bir veri merkezine yönelik olarak çoğaltabilir veya Azure'a yönelik çoğaltarak veri merkezinizi genişletebilirsiniz. Site Recovery aynı zamanda iş yükleri için basit bir yük devretme ve kurtarma işlevi sunar. SQL Server AlwaysOn gibi olağanüstü durum kurtarma mekanizmaları ile tümleştirilir ve birden çok makinede katmanlı halde bulunan iş yüklerine yönelik kolay bir yük devretme işlemi için kurtarma planları sağlar.

## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![OMS Güvenlik ve Uyumluluk](media/operations-management-suite-overview/icon-security-compliance.png) Güvenlik ve Uyumluluk
Güvenlik ve Uyumluluk, altyapınıza ilişkin güvenlik risklerini belirlemenize, değerlendirmenize ve azaltmanıza yardımcı olur.  Bu OMS özellikleri Log Analytics'te, ortamınızda güvenlik sürekliliğinin sağlanması konusunda size yardımcı olmak üzere aracı sistemlerindeki günlük verilerini ve günlük yapılandırmasını çözümleyen birden çok çözüm aracılığıyla uygulanır.

* [Güvenlik ve Denetim çözümü](oms-security-getting-started.md) top, şüpheli etkinlikleri belirlemek için yönetilen sistemlerdeki güvenlik olaylarını toplar ve çözümler.
* [Kötü amaçlı yazılımdan koruma çözümü](../log-analytics/log-analytics-malware.md), yönetilen sistemlerdeki kötü amaçlı yazılımdan koruma işlevinin durumuyla ilgili rapor verir.  
* Sistem Güncelleştirmeleri, düzeltme eki uygulanmasını gerektiren sistemleri kolayca belirleyebilmeniz için güvenlik güncelleştirmelerinde ve yönetilen sistemlerinizdeki diğer güncelleştirmelerde çözümleme gerçekleştirir.

## <a name="next-steps"></a>Sonraki adımlar
* [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) hakkında bilgi edinme.
* [Azure Otomasyonu](../automation/automation-intro.md) hakkında bilgi edinme.
* [Azure Backup](http://azure.microsoft.com/documentation/services/backup) hakkında bilgi edinme.
* [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) hakkında bilgi edinme.

