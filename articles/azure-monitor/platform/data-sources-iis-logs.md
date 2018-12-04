---
title: IIS günlüklerini Azure Log Analytics'te | Microsoft Docs
description: Internet Information Services (IIS) kullanıcı etkinliği Log Analytics tarafından toplanan günlük dosyalarını depolar.  Bu makalede, IIS günlükler koleksiyonunu ve ayrıntıları Log Analytics çalışma alanında oluşturdukları kayıtlarının nasıl yapılandırılacağı açıklanır.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: bwren
ms.comopnent: ''
ms.openlocfilehash: 38f21a3d8f2bf1adcd224480223a37113b347e18
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52838506"
---
# <a name="iis-logs-in-log-analytics"></a>Log Analytics'te IIS günlükleri
Internet Information Services (IIS) kullanıcı etkinliği Log Analytics tarafından toplanan günlük dosyalarını depolar.  

![IIS günlükleri](media/data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>IIS yapılandırma günlükleri
Log Analytics, girdileri gerekir, böylece IIS tarafından oluşturulan günlük dosyalarını toplar [IIS için günlüğe kaydetmeyi yapılandırma](https://technet.microsoft.com/library/hh831775.aspx).

Log Analytics, yalnızca depolanan W3C biçiminde IIS günlük dosyalarını destekler ve özel alanlar veya Gelişmiş IIS günlüğü desteklemez.  
Log Analytics, Günlükleri NCSA veya IIS yerel biçiminde toplamaz.

IIS günlükleri Log Analytics'ten yapılandırmak [günlük analizi ayarları veri menüde](agent-data-sources.md#configuring-data-sources).  Yapılandırma gerekli seçerek dışındaki yoktur **toplamak W3C biçiminde IIS dosya günlükleri**.


## <a name="data-collection"></a>Veri toplama
Log Analytics günlük her kapatıldığında ve yeni bir tane oluşturulur her Aracıdan IIS günlük girişi toplar. Bu sıklığı tarafından denetlenir **günlük dosyası aktarma zamanlaması** varsayılan olarak günde bir kez olan IIS sitesi ayarlama. Örneğin, ayarları ise **saatlik**, sonra Log Analytics günlük her saat toplar.  Ayar **günlük**, ardından Log Analytics günlük 24 saatte bir toplar.


## <a name="iis-log-record-properties"></a>IIS günlük kaydı özellikleri
IIS günlük kayıtları sahip bir tür **w3cııslog** ve aşağıdaki tabloda gösterilen özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olay toplandığı bilgisayarın adı. |
| CIP |İstemci IP adresi. |
| csMethod |GET veya POST gibi istek yöntemi. |
| csReferer |Kullanıcının geçerli siteye bağlantıdan takip sitesi. |
| csUserAgent |İstemci tarayıcısı türü. |
| csUserName |Sunucuya erişen kimliği doğrulanmış kullanıcının adı. Anonim kullanıcılar bir tire işaretiyle gösterilir. |
| csUriStem |İsteği gibi bir web sayfası hedefi. |
| csUriQuery |Sorgu, varsa istemcinin gerçekleştirmeye çalıştığı. |
| ManagementGroupName |Operations Manager aracıları için yönetim grubunun adı.  Diğer aracılar için AOI - budur\<çalışma alanı kimliği\> |
| RemoteIPCountry |İstemci IP adresini ülke. |
| RemoteIPLatitude |İstemci IP adresi bulunduğu enlem. |
| RemoteIPLongitude |İstemci IP adresi bulunduğu boylam. |
| scStatus |HTTP durum kodu. |
| scSubStatus |Alt hata kodu. |
| scWin32Status |Windows durum kodu. |
| SIP |Web sunucusunun IP adresi. |
| SourceSystem |OpsMgr |
| Spor |' % S'sunucusuna bağlı istemci bağlantı noktası. |
| sSiteName |IIS site adı. |
| TimeGenerated |Tarih ve saat giriş günlüğe kaydedildi. |
| timeTaken |Süreyi milisaniye cinsinden İstek işlenemedi. |

## <a name="log-searches-with-iis-logs"></a>IIS günlükleri ile günlük aramaları
Aşağıdaki tabloda IIS günlük kayıtları almak günlük sorguları farklı örnekler sağlar.

| Sorgu | Açıklama |
|:--- |:--- |
| W3cııslog |Tüm IIS günlük kaydı. |
| W3cııslog &#124; burada scStatus 500 == |Tüm IIS günlük kayıtları 500 dönüş durumu. |
| W3cııslog &#124; Count() işlevi tarafından CIP özetleme |İstemci IP adresine göre sayısı, IIS günlük girdilerinin. |
| W3cııslog &#124; burada csHost "www.contoso.com" == &#124; Count() işlevi tarafından csUriStem özetleme |URL'ye göre girişlerinde konak www.contoso.com oturum sayısı, IIS. |
| W3cııslog &#124; sum(csBytes) bilgisayara göre özetlemek &#124; 500000 Al |Her IIS bilgisayarına göre alınan toplam bayt sayısı. |

## <a name="next-steps"></a>Sonraki adımlar
* Diğer toplamak için log Analytics'i yapılandırma [veri kaynakları](agent-data-sources.md) analiz.
* Hakkında bilgi edinin [günlük aramaları](../../azure-monitor/log-query/log-query-overview.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için.
* Log analytics'te, IIS günlükleri bulunan önemli koşulları proaktif olarak bildirmek için uyarıları yapılandırın.
