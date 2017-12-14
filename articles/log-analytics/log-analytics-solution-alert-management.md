---
title: "Yönetim çözümü Operations Management Suite (OMS) uyarı | Microsoft Docs"
description: "Günlük analizi uyarı Yönetimi çözümünde tüm uyarılar, ortamınızdaki analiz etmenize yardımcı olur.  OMS içinde oluşturulan uyarıların birleştirilmesi ek olarak, bu uyarılar bağlı System Center Operations Manager yönetim gruplarından günlük analizi alır."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: fe5d534e-0418-4e2f-9073-8025e13271a8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2017
ms.author: bwren
ms.openlocfilehash: 4ec80fccdf4521792ff6be115ec66227f0fe1ed2
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Uyarı yönetimi çözümü Operations Management Suite (OMS)

![Uyarı Yönetimi simgesi](media/log-analytics-solution-alert-management/icon.png)

Uyarı yönetimi çözümü tüm uyarılar günlük analizi deponuzun analiz etmenize yardımcı olur.  Bu uyarıların bir çeşitli kaynaklardan bu kaynakları da dahil olmak üzere ortaya çıkabilir [günlük analizi tarafından oluşturulan](log-analytics-alerts.md) veya [Nagios veya Zabbix içeri](log-analytics-linux-agents.md).  Çözüm ayrıca uyarıları birinden alır [bağlı System Center Operations Manager Yönetim grupları](log-analytics-om-agents.md).

## <a name="prerequisites"></a>Ön koşullar
Günlük analizi deposundaki türüne sahip herhangi bir kayıt ile çözüm çalışır **uyarı**, ne olursa olsun bu kayıtları toplamak için gerekli yapılandırmadır gerçekleştirmeniz gerekir.

- Günlük analizi uyarılar için [uyarı kuralları oluşturmak](log-analytics-alerts.md) doğrudan deposunda uyarı kayıtları oluşturmak için.
- Nagios ve Zabbix uyarılar için [bu sunucuları yapılandırmak](log-analytics-linux-agents.md) günlük analizi için uyarıları göndermek için.
- System Center Operations Manager uyarılar için [Operations Manager yönetim grubunuzu günlük analizi çalışma alanına bağlayın](log-analytics-om-agents.md).  System Center Operations Manager'da oluşturulan herhangi bir uyarı günlük analizi alınır.  

## <a name="configuration"></a>Yapılandırma
Uyarı yönetimi çözümü açıklanan işlemi kullanarak OMS çalışma alanınıza ekleyin [çözümleri Ekle](log-analytics-add-solutions.md).  Başka bir yapılandırma işlemi gerekmez.

## <a name="management-packs"></a>Yönetim paketleri
Ardından System Center Operations Manager yönetim grubunuzu, OMS çalışma alanınızla bağlıysa, bu çözüm eklediğinizde, aşağıdaki yönetim paketlerini System Center Operations Manager'da yüklenir.  Yapılandırma veya gerekli yönetim paketlerinin bakım yoktur.  

* Microsoft System Center Advisor uyarı Yönetimi (Microsoft.IntelligencePacks.AlertManagement)

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](log-analytics-om-agents.md).

## <a name="data-collection"></a>Veri toplama
### <a name="agents"></a>Aracılar
Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Destek | Açıklama |
|:--- |:--- |:--- |
| [Windows aracıları](log-analytics-windows-agent.md) | Hayır |Doğrudan Windows aracıları uyarılar oluşturmaz.  Günlük analizi uyarılar olaylarından oluşturulabilir ve aracıları Windows'dan toplanan performans verileri. |
| [Linux aracıları](log-analytics-linux-agents.md) | Hayır |Doğrudan Linux aracılarını uyarılar oluşturmaz.  Günlük analizi uyarılar olayları ve performans verilerini Linux aracılarını toplanan oluşturulabilir.  Nagios ve Zabbix uyarıları Linux Aracısı gerektiren bu sunuculardan toplanır. |
| [System Center Operations Manager yönetim grubu](log-analytics-om-agents.md) |Evet |Operations Manager aracıları oluşturulan uyarıların yönetim grubuna teslim ve günlük analizi iletilir.<br><br>Günlük analizi Operations Manager aracıları arasında doğrudan bağlantı gerekli değildir. Uyarı verileri yönetim grubundan günlük analizi depoya iletilir. |


### <a name="collection-frequency"></a>Toplama sıklığı
- Depo içinde depolanan hemen uyarı kayıtları çözüme kullanılabilir.
- Uyarı verileri Operations Manager yönetim grubundan üç dakikada günlük analizi için gönderilir.  

## <a name="using-the-solution"></a>Çözümü kullanma
Uyarı yönetimi çözümü, OMS çalışma alanına eklediğinizde **uyarı Yönetimi** döşeme OMS panonuz eklenir.  Bu kutucuğu sayısı ve grafik gösterimi son 24 saat içinde oluşturulan etkin uyarıların sayısını görüntüler.  Bu zaman aralığı değiştiremezsiniz.

![Uyarı Yönetimi döşeme](media/log-analytics-solution-alert-management/tile.png)

Tıklayın **uyarı Yönetimi** açmak için kutucuğa **uyarı Yönetimi** Pano.  Pano aşağıdaki tabloda gösterilen sütunları içerir.  Her bir sütunun üst 10 uyarıları Belirtilen kapsam ve zaman aralığı için bu sütunun ölçütlerle eşleşen sayısına göre listeler.  Listenin tamamını tıklayarak sağlayan bir günlük arama çalıştırabilirsiniz **tümünü görmek** alt sütun veya sütun başlığını tıklatarak.

| Sütun | Açıklama |
|:--- |:--- |
| Kritik uyarılar |Tüm Uyarıları önem derecesi Kritik Uyarı adına göre gruplandırılır.  Bu uyarı için tüm kayıtları döndürme günlük arama çalıştırmak için bir uyarı adı tıklayın. |
| Uyarı bildirimleri |Tüm uyarıları uyarı adına göre gruplandırılmış uyarı önem derecesi.  Bu uyarı için tüm kayıtları döndürme günlük arama çalıştırmak için bir uyarı adı tıklayın. |
| Etkin SCOM uyarıları |Tüm uyarıları Operations Manager'dan dışındaki herhangi bir durum ile toplanan *kapalı* uyarıyı oluşturan kaynağa göre gruplandırılan. |
| Tüm etkin uyarıları |Tüm uyarıları uyarı adına göre gruplandırılmış herhangi bir önem derecesi. Yalnızca Operations Manager uyarıları ile herhangi bir durum dışında içeren *kapalı*. |

Sağa kaydırırsanız, Pano gerçekleştirmek için tıklatabilirsiniz birkaç genel sorgular listeler bir [günlük arama](log-analytics-log-searches.md) uyarı veriler için.

![Uyarı Yönetim Panosu](media/log-analytics-solution-alert-management/dashboard.png)


## <a name="log-analytics-records"></a>Log Analytics kayıtları
Uyarı yönetimi çözümü türüne sahip herhangi bir kaydının çözümler **uyarı**.  Günlük analizi tarafından oluşturulan veya Nagios veya Zabbix toplanan uyarılar çözümü tarafından doğrudan toplanmadı.

Çözüm uyarıları System Center Operations Manager'dan içe ve her bir türü ile ilgili bir kayıt oluşturur **uyarı** ve SourceSystem, **OpsManager**.  Bu kayıtları aşağıdaki tabloda özelliklere sahiptir:  

| Özellik | Açıklama |
|:--- |:--- |
| Tür |*Uyarı* |
| SourceSystem |*OpsManager* |
| AlertContext |XML biçiminde oluşturulacak uyarıya neden veri öğesi ayrıntıları. |
| AlertDescription |Uyarı ayrıntılı bir açıklaması. |
| Alertıd |Uyarı GUID. |
| AlertName |Uyarı adı. |
| AlertPriority |Uyarı öncelik düzeyi. |
| AlertSeverity |Uyarı önem derecesi. |
| AlertState |Uyarının son çözümleme durumu. |
| LastModifiedBy |Uyarıyı son değiştiren kullanıcının adı. |
| ManagementGroupName |Burada uyarının oluşturulmasının yönetim grubunun adı. |
| RepeatCount |Aynı uyarı için aynı üretildi sayısı, çözümlenen bu yana nesne izlenen. |
| Çözüm bulan |Uyarı çözümleyen kullanıcı adı. Uyarı henüz çözümlenmediyse, boş. |
| SourceDisplayName |Uyarı izleme nesnesinin görünen adı. |
| SourceFullName |Uyarı izleme nesnesi tam adı. |
| Ticketıd |System Center Operations Manager ortamı uyarıları biletlerini atamak için bir işlem ile tümleşik uyarı için bilet kimliği.  Bir anahtarı yok, boş bir kimlik atanır. |
| TimeGenerated |Tarih ve uyarının oluşturulduğu saat. |
| TimeLastModified |Tarihi ve uyarının son değiştirilme zamanı. |
| TimeRaised |Tarih ve uyarının oluşturulduğu saat. |
| TimeResolved |Tarih ve saat uyarı çözümlendi. Uyarı henüz çözümlenmediyse, boş. |

## <a name="sample-log-searches"></a>Örnek günlük aramaları
Aşağıdaki tabloda bu çözüm tarafından toplanan uyarı kayıtları için örnek günlük aramaları sağlar: 

| Sorgu | Açıklama |
|:--- |:--- |
| Tür uyarı SourceSystem = OpsManager AlertSeverity = hata TimeRaised = > şimdi - 24 saat |Son 24 saatte oluşturulan kritik uyarılar |
| Tür uyarı AlertSeverity = uyarı TimeRaised = > şimdi - 24 saat |Son 24 saatte oluşturulan uyarı bildirimleri |
| Tür uyarı SourceSystem = OpsManager AlertState =! kapalı TimeRaised = > şimdi 24 saatlik &#124; Ölçü count() SourceDisplayName bazında sayı olarak |Son 24 saatte oluşturulan etkin uyarılara sahip kaynaklar |
| Tür uyarı SourceSystem = OpsManager AlertSeverity = hata TimeRaised = > şimdi 24 saatlik AlertState! kapalı = |Son 24 hala etkin olan saatte oluşturulan kritik uyarılar |
| Tür uyarı SourceSystem = OpsManager TimeRaised = > şimdi 24 saatlik AlertState = kapalı |Son 24 şimdi kapatılan saatte oluşturulan uyarılar |
| Tür uyarı SourceSystem = OpsManager TimeRaised = > şimdi - 1 gün &#124; Ölçü count() AlertSeverity bazında sayı olarak |Önem derecesine göre gruplandırılmış son 1 gün sırasında oluşturulan uyarılar |
| Tür uyarı SourceSystem = OpsManager TimeRaised = > şimdi - 1 gün &#124; RepeatCount desc sıralama |Yineleme sayısına göre sıralanmış son 1 gün sırasında oluşturulan uyarılar |


>[!NOTE]
> Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), önceki sorgular için aşağıdaki değişeceğinden sonra:
>
>| Sorgu | Açıklama |
|:---|:---|
| Hi &#124; Burada SourceSystem "OpsManager" ve AlertSeverity == "error" ve TimeRaised == > ago(24h) |Son 24 saatte oluşturulan kritik uyarılar |
| Hi &#124; Burada AlertSeverity "uyarı" ve TimeRaised == > ago(24h) |Son 24 saatte oluşturulan uyarı bildirimleri |
| Hi &#124; Burada SourceSystem "OpsManager" ve AlertState ==! "Kapalı" = ve TimeRaised > ago(24h) &#124; Count özetlemek SourceDisplayName tarafından count() = |Son 24 saatte oluşturulan etkin uyarılara sahip kaynaklar |
| Hi &#124; Burada SourceSystem "OpsManager" ve AlertSeverity == "error" ve TimeRaised == > ago(24h) ve AlertState! "Kapalı" = |Son 24 hala etkin olan saatte oluşturulan kritik uyarılar |
| Hi &#124; Burada SourceSystem "OpsManager" ve TimeRaised == > ago(24h) ve AlertState "Kapalı" == |Son 24 şimdi kapatılan saatte oluşturulan uyarılar |
| Hi &#124; Burada SourceSystem "OpsManager" ve TimeRaised == > ago(1d) &#124; Count özetlemek AlertSeverity tarafından count() = |Önem derecesine göre gruplandırılmış son 1 gün sırasında oluşturulan uyarılar |
| Hi &#124; Burada SourceSystem "OpsManager" ve TimeRaised == > ago(1d) &#124; RepeatCount desc sıralama |Yineleme sayısına göre sıralanmış son 1 gün sırasında oluşturulan uyarılar |


## <a name="next-steps"></a>Sonraki adımlar
* Log Analytics’ten uyarı oluşturma hakkında daha ayrıntılı bilgi edinmek için bkz. [Log Analytics’teki Uyarılar](log-analytics-alerts.md) .
