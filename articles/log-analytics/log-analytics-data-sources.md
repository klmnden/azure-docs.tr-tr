---
title: "OMS günlük analizi veri kaynakları yapılandırma | Microsoft Docs"
description: "Veri kaynakları günlük analizi toplar aracıları ve diğer kaynakları bağlı verileri tanımlayın.  Bu makalede kavramı nasıl günlük analizi veri kaynakları kullanır, bunların nasıl yapılandırılacağı ayrıntılarını açıklar ve kullanılabilen değişik veri kaynakları özetini sağlar."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2017
ms.author: bwren
ms.openlocfilehash: 00d030a502cf70ea9a5dea767f560cdf2919573e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="data-sources-in-log-analytics"></a>Günlük analizi veri kaynaklarında
Günlük analizi OMS çalışma alanınızdaki bağlı kaynaklardan veri toplar ve bunları OMS deposunda saklar.  Her birinden toplanan verileri yapılandırdığınız veri kaynakları tarafından tanımlanır.  OMS deposundaki verileri bir kayıt kümesi depolanır.  Her veri kaynağı kendi özellikler kümesini sahip her türüyle belirli bir türdeki kayıtları oluşturur.

![Günlük analizi veri toplama](./media/log-analytics-data-sources/overview.png)

Veri kaynakları, aynı zamanda bağlı kaynaklardan veri toplamak ve OMS depoya kayıtları oluşturmak OMS çözümleri farklıdır.  Çözümleri alanınıza çözümleri Galeriden eklenebilir ve genellikle OMS portalında ek çözümleme araçları sağlar.  

## <a name="summary-of-data-sources"></a>Veri kaynakları özeti
Günlük analizi şu anda kullanılabilir veri kaynakları aşağıdaki tabloda listelenmiştir.  Her ayrıntı için bu veri kaynağı sağlayarak ayrı bir makale için bir bağlantı vardır.

| Veri kaynağı | Olay türü | Açıklama |
|:--- |:--- |:--- |
| [Özel günlükler](log-analytics-data-sources-custom-logs.md) |\<Günlükadı\>_CL |Metin dosyaları günlüğü bilgilerini içeren Windows veya Linux aracıları hakkında. |
| [Windows olay günlükleri](log-analytics-data-sources-windows-events.md) |Olay |Olayları olay günlüğünden Windows bilgisayarlarda toplanır. |
| [Windows performans sayaçları](log-analytics-data-sources-performance-counters.md) |Perf |Performans sayaçları Windows bilgisayarlardan toplanan. |
| [Linux performans sayaçları](log-analytics-data-sources-performance-counters.md) |Perf |Linux bilgisayarlardan toplanan performans sayaçları. |
| [IIS günlükleri](log-analytics-data-sources-iis-logs.md) |W3CIISLog |Internet Information Services W3C biçiminde kaydeder. |
| [Syslog](log-analytics-data-sources-syslog.md) |Syslog |Syslog olayları Windows veya Linux bilgisayarlardaki. |

## <a name="configuring-data-sources"></a>Veri kaynaklarını yapılandırma
Veri kaynaklarından yapılandırma **veri** günlük analizi menüde **ayarları**.  Herhangi bir yapılandırma OMS çalışma alanınızdaki tüm bağlı kaynakları teslim edilir.  Şu anda bu yapılandırmasından tüm aracıları dışarıda bırakılamaz.

![Windows olayları yapılandırın](./media/log-analytics-data-sources/configure-events.png)

1. OMS konsolunda **ayarları** döşeme veya **ayarları** ekranın üstündeki düğmesi.
2. Seçin **veri**.
3. Yapılandırmak için veri kaynağını tıklatın.
4. Yapılandırmalarını hakkında ayrıntılar için yukarıdaki tabloda her bir veri kaynağı için belgelerine bağlantıyı izleyin.

> [!NOTE]
> Bu gibi durumlarda, günlük analizi veri kaynakları şu anda Azure Portalı'nda yapılandıramazsınız.

## <a name="data-collection"></a>Veri toplama
Veri kaynağı yapılandırmaları günlük analizi için birkaç dakika içinde doğrudan bağlı olan aracılara teslim edilir.  Belirtilen veri Aracıdan toplanır ve her bir veri kaynağı için belirli aralıklarla doğrudan günlük analizi için teslim.  Bu özellikleri için her bir veri kaynağı belgelerine bakın.

Bağlı yönetim grubu için aracıları System Center Operations Manager (SCOM), veri kaynağı yapılandırmaları yönetim paketleri çevrilen ve yönetim grubuna varsayılan olarak 5 dakikada bir teslim.  Aracı gibi diğer yönetim paketi indirir ve belirtilen verileri toplar. Veri kaynağı bağlı olarak veri ya da günlük analizi veri ileten bir yönetim sunucusuna gönderilen olur veya aracı veri günlük analizi için yönetim sunucusu üzerinden geçmeden gönderir. Başvurmak [OMS özelliklerin ve çözümlerin için veri toplama ayrıntılarına](log-analytics-add-solutions.md#data-collection-details) Ayrıntılar için.  Bu yapılandırma SCOM ve OMS bağlanma ve sıklığını değiştirme hakkında ayrıntılar okuyabilir, teslim [System Center Operations Manager tümleştirmesini yapılandırma](log-analytics-om-agents.md).

Aracı günlük analizi veya Operations Manager bağlanamıyor ise, bağlantı kurduğunda teslim eder veri toplamaya devam eder.  Veriler veri miktarı istemci için en büyük önbellek boyutu ulaşırsa veya aracı 24 saat içinde bir bağlantı kurup kuramadığını değilse kaybolmuş olabilir.

## <a name="log-analytics-records"></a>Log Analytics kayıtları
Günlük analizi tarafından toplanan tüm veriler, OMS depoya kayıt olarak depolanır.  Farklı veri kaynakları tarafından toplanan kayıtları kendi özellikler kümesini sahip olur ve tarafından tanımlanan kendi **türü** özelliği.  Her veri kaynağı için belgelere ve çözüm ayrıntıları için her bir kayıt türü bakın.

## <a name="next-steps"></a>Sonraki Adımlar
* Hakkında bilgi edinin [çözümleri](log-analytics-add-solutions.md) işlevselliği için günlük analizi eklemek ve de OMS depoya veri toplama.
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümleri toplanan verileri çözümlemek için.  
* Yapılandırma [uyarıları](log-analytics-alerts.md) proaktif olarak, toplanan veri kaynakları ve çözümlerinin kritik verilerin bildirmek için.
