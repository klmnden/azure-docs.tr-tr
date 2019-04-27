---
title: Uyarı yönetimi çözümü Azure Log analytics'te | Microsoft Docs
description: Log analytics'teki uyarı yönetimi çözümü tüm uyarıları ortamınız analiz etmenize yardımcı olur.  Log Analytics içinde oluşturulan sağlamlaştırmak uyarıların yanı sıra, bu uyarılar bağlı System Center Operations Manager yönetim gruplarından Log Analytics'e içeri aktarır.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: fe5d534e-0418-4e2f-9073-8025e13271a8
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/19/2018
ms.author: bwren
ms.openlocfilehash: 06532369efb802606eb13a4b38a8579a3528f999
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60777036"
---
# <a name="alert-management-solution-in-azure-log-analytics"></a>Azure Log analytics'teki uyarı yönetimi çözümü

![Uyarı Yönetimi simgesi](media/alert-management-solution/icon.png)

> [!NOTE]
>  Destekleyen gelişmiş yetenekleri için artık azure İzleyici [uygun ölçekte uyarılarınızı yönetme](https://aka.ms/azure-alerts-overview), tarafından oluşturulan dahil olmak üzere [izleme araçları SCOM, Zabbix veya Nagios gibi](https://aka.ms/managing-alerts-other-monitoring-services).
>  


Uyarı yönetimi çözümü tüm uyarıları Log Analytics deponuzdaki analiz etmenize yardımcı olur.  Bu uyarılar bir çeşitli kaynaklardan kaynaklar dahil olmak üzere gelmiş olabilir [Log Analytics tarafından oluşturulan](../../azure-monitor/platform/alerts-overview.md) veya [Nagios veya Zabbix içeri aktarılan](../../azure-monitor/learn/quick-collect-linux-computer.md). Çözüm ayrıca uyarılar herhangi aktarır [bağlı System Center Operations Manager Yönetim grupları](../../azure-monitor/platform/om-agents.md).

## <a name="prerequisites"></a>Önkoşullar
Çözüm herhangi bir kayıt türü ile Log Analytics deposunda çalışır **uyarı**, ne olursa olsun bu kayıtları toplamak için gerekli bir yapılandırmadır gerçekleştirmeniz gerekir.

- Log Analytics uyarılarını için [uyarı kuralları oluşturma](../../azure-monitor/platform/alerts-overview.md) doğrudan depoda uyarı kayıtları oluşturmak için.
- Nagios ve Zabbix uyarıları için [bu sunucuları yapılandırmak](../../azure-monitor/learn/quick-collect-linux-computer.md) uyarıları Log Analytics'e göndermek için.
- System Center Operations Manager uyarıları için [Operations Manager yönetim grubunuzu Log Analytics çalışma alanınıza bağlanmak](../../azure-monitor/platform/om-agents.md).  System Center Operations Manager'da oluşturulan tüm uyarılar, Log Analytics'e aktarılır.  

## <a name="configuration"></a>Yapılandırma
Uyarı yönetimi çözümü açıklanan işlemi kullanarak Log Analytics çalışma alanınıza eklemek [çözüm ekleme](../../azure-monitor/insights/solutions.md). Başka bir yapılandırma işlemi gerekmez.

## <a name="management-packs"></a>Yönetim paketleri
Ardından System Center Operations Manager yönetim grubunuzun Log Analytics çalışma alanınıza bağlıysa, bu çözümü eklediğinizde, aşağıdaki yönetim paketlerini System Center Operations Manager'da yüklenir.  Yapılandırma veya bakım gerekli yönetim paketleri yoktur.

* Microsoft System Center Advisor uyarı Yönetimi (Microsoft.IntelligencePacks.AlertManagement)

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](../../azure-monitor/platform/om-agents.md).

## <a name="data-collection"></a>Veri toplama
### <a name="agents"></a>Aracılar
Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Destek | Açıklama |
|:--- |:--- |:--- |
| [Windows aracıları](agent-windows.md) | Hayır |Doğrudan Windows aracıları uyarılar oluşturmaz.  Log Analytics uyarılarını, olaylarından oluşturulabilir ve Windows aracıları toplanan performans verileri. |
| [Linux aracıları](../../azure-monitor/learn/quick-collect-linux-computer.md) | Hayır |Doğrudan Linux aracıları uyarılar oluşturmaz.  Log Analytics uyarılarını olayları ve performans verilerinden Linux aracılarından toplanan oluşturulabilir.  Nagios ve Zabbix uyarıları Linux Aracısı gerektiren bu sunuculardan toplanır. |
| [System Center Operations Manager yönetim grubu](../../azure-monitor/platform/om-agents.md) |Evet |Operations Manager aracıları üzerinde oluşturulan uyarıların yönetim grubu için teslim ve sonra Log Analytics'e iletilir.<br><br>Operations Manager aracılarının doğrudan Log analytics'e bağlantı gerekli değildir. Uyarı verileri yönetim grubundan Log Analytics deposuna iletilir. |


### <a name="collection-frequency"></a>Toplama sıklığı
- Depoda depolanan hemen sonra çözüme var olan uyarı kayıt.
- Uyarı verileri Operations Manager yönetim grubundan üç dakikada Log Analytics'e gönderilir.  

## <a name="using-the-solution"></a>Çözümü kullanma
Uyarı yönetimi çözümü, Log Analytics çalışma alanınıza eklediğinizde **uyarı Yönetimi** kutucuk, panonuza eklenir.  Bu kutucuk, sayı ve grafik temsilini son 24 saat içinde oluşturulan etkin uyarıların sayısını görüntüler.  Bu zaman aralığını değiştiremezsiniz.

![Uyarı Yönetimi kutucuğu](media/alert-management-solution/tile.png)

Tıklayarak **uyarı Yönetimi** açmak için kutucuğa **uyarı Yönetimi** Pano.  Pano aşağıdaki tabloda gösterilen sütunları içerir.  Her bir sütunun Belirtilen kapsam ve zaman aralığı için söz konusu sütunun ölçütlerle eşleşen sayısına göre en çok kullanılan 10 uyarılar listeler.  Tıklayarak listenin tamamını sağlayan bir günlük araması çalıştırabilirsiniz **tümünü gör** alt sütun veya sütun başlığına tıklayarak.

| Sütun | Açıklama |
|:--- |:--- |
| Kritik Uyarılar |Tüm uyarılar ile uyarı adına göre gruplandırılmış kritik bir önem düzeyi.  Bu uyarı için tüm kayıtları döndüren bir günlük araması çalıştırmak için bir uyarı adına tıklayın. |
| Uyarı Bildirimleri |Tüm uyarı uyarı uyarı adına göre gruplandırılmış bir önem derecesi.  Bu uyarı için tüm kayıtları döndüren bir günlük araması çalıştırmak için bir uyarı adına tıklayın. |
| Etkin SCOM Uyarıları |Tüm uyarıları Operations Manager'dan dışında herhangi bir durumu ile toplanan *kapalı* uyarının kaynağına göre gruplandırılmış. |
| Tüm etkin uyarıları |Tüm uyarılar ile uyarı adına göre gruplandırılmış tüm önem derecesi. Yalnızca Operations Manager uyarıları ile herhangi bir durumu dışında içeren *kapalı*. |

Sağa kaydırdığınızda Pano gerçekleştirmek için tıklayabilirsiniz birkaç yaygın sorguları listeler bir [günlük araması](../../azure-monitor/log-query/log-query-overview.md) uyarı verileri için.

![Uyarı Yönetim Panosu](media/alert-management-solution/dashboard.png)


## <a name="log-analytics-records"></a>Log Analytics kayıtları
Uyarı yönetimi çözümü türünde herhangi bir kayıt çözümler **uyarı**.  Log Analytics tarafından oluşturulan veya Nagios veya Zabbix toplanan uyarılar çözüm tarafından doğrudan toplanmadı.

Çözüm, uyarıları System Center Operations Manager'dan içe ve her bir türü ile ilgili bir kayıt oluşturur **uyarı** ve Analytics'teki **OpsManager**.  Bu kayıtlar aşağıdaki tabloda özelliklere sahiptir:  

| Özellik | Açıklama |
|:--- |:--- |
| Tür |*Uyarı* |
| SourceSystem |*OpsManager* |
| AlertContext |XML biçiminde oluşturulacak uyarıya neden veri öğesi ayrıntıları. |
| AlertDescription |Uyarı ayrıntılı açıklaması. |
| Alertıd |Uyarı GUİD'si. |
| AlertName |Uyarının adı. |
| AlertPriority |Uyarı öncelik düzeyi. |
| AlertSeverity |Uyarı önem derecesi. |
| AlertState |Uyarı çözümleme durumu son. |
| LastModifiedBy |Uyarıyı son değiştiren kullanıcı adı. |
| ManagementGroupName |Uyarının verildiği yeri yönetim grubunun adı. |
| RepeatCount |Aynı aynı uyarının verildiği sayısı, çözümlenen bu yana nesne izlediniz. |
| ResolvedBy |Uyarıyı çözümleyen kullanıcı adı. Uyarı henüz çözümlenmedi, boş. |
| SourceDisplayName |Uyarıyı üreten izleme nesnesinin görünen adı. |
| SourceFullName |Uyarıyı üreten izleme nesnesi tam adı. |
| TicketId |System Center Operations Manager ortamı biletleri için uyarı atamak için bir işlem ile tümleşikse uyarı için bilet kimliği.  Boş yok biletin kimliği atanır. |
| TimeGenerated |Tarihi ve uyarının oluşturulduğu saat. |
| TimeLastModified |Tarihi ve uyarının son değiştirilme saati. |
| TimeRaised |Tarihi ve uyarının verildiği saat. |
| TimeResolved |Tarih ve saat, uyarı çözümlendi. Uyarı henüz çözümlenmedi, boş. |

## <a name="sample-log-searches"></a>Örnek günlük aramaları
Aşağıdaki tabloda bu çözüm tarafından toplanan uyarı kayıtlarına ilişkin örnek günlük aramaları sağlar: 

| Sorgu | Açıklama |
|:---|:---|
| Uyarı &#124; burada Analytics'teki "OpsManager" ve AlertSeverity == "error" ve TimeRaised == > ago(24h) |Son 24 saatte oluşturulan kritik uyarılar |
| Uyarı &#124; burada AlertSeverity "uyarı" ve TimeRaised == > ago(24h) |Son 24 saatte oluşturulan uyarı bildirimleri |
| Uyarı &#124; burada Analytics'teki "OpsManager" ve AlertState ==! "Kapalı" ve TimeRaised = > ago(24h) &#124; sayısı özetlemek = count() SourceDisplayName tarafından |Son 24 saatte oluşturulan etkin uyarılara sahip kaynaklar |
| Uyarı &#124; burada Analytics'teki "OpsManager" ve AlertSeverity == "error" ve TimeRaised == > ago(24h) ve AlertState! = "Kapalı" |Son 24 hala etkin olan saatte oluşturulan kritik uyarılar |
| Uyarı &#124; burada Analytics'teki "OpsManager" ve TimeRaised == > ago(24h) ve AlertState "Kapalı" == |Son 24 artık kapatılan saatte oluşturulan uyarılar |
| Uyarı &#124; burada Analytics'teki "OpsManager" ve TimeRaised == > ago(1d) &#124; sayısı özetlemek = count() AlertSeverity tarafından |Önem derecesine göre gruplandırılmış halde, son 1 günde oluşturulan uyarılar |
| Uyarı &#124; burada Analytics'teki "OpsManager" ve TimeRaised == > ago(1d) &#124; RepeatCount desc göre sırala |Yineleme sayısına göre sıralanmış halde, son 1 günde oluşturulan uyarılar |



## <a name="next-steps"></a>Sonraki adımlar
* Log Analytics’ten uyarı oluşturma hakkında daha ayrıntılı bilgi edinmek için bkz. [Log Analytics’teki Uyarılar](../../azure-monitor/platform/alerts-overview.md) .
