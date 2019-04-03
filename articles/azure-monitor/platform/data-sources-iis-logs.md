---
title: Azure İzleyici'de IIS günlüklerini | Microsoft Docs
description: Internet Information Services (IIS) kullanıcı etkinliği Azure İzleyici tarafından toplanan günlük dosyalarını depolar.  Bu makalede, IIS günlükler koleksiyonunu ve ayrıntıları Azure İzleyicisi'nde oluşturdukları kayıtlarının nasıl yapılandırılacağı açıklanır.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2018
ms.author: bwren
ms.openlocfilehash: 402cd4723791c0bc33db22c8857d1b785862f596
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58850617"
---
# <a name="collect-iis-logs-in-azure-monitor"></a>Azure İzleyici'de toplama IIS Günlükler
Internet Information Services (IIS), Azure İzleyici tarafından toplanan ve depolanan günlük dosyalarında kullanıcı etkinliğini depolar [günlük verilerini](data-platform.md).

![IIS günlükleri](media/data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>IIS yapılandırma günlükleri
Azure İzleyici girdileri gerekir, böylece IIS tarafından oluşturulan günlük dosyalarını toplar [IIS için günlüğe kaydetmeyi yapılandırma](https://technet.microsoft.com/library/hh831775.aspx).

Azure İzleyici, yalnızca depolanan W3C biçiminde IIS günlük dosyalarını destekler ve özel alanlar veya Gelişmiş IIS günlüğü desteklemez. NCSA veya IIS yerel biçiminde günlükleri toplamaz.

Azure İzleyici'den IIS günlükler yapılandırmak [Gelişmiş Ayarlar menüsünden](agent-data-sources.md#configuring-data-sources).  Yapılandırma gerekli seçerek dışındaki yoktur **toplamak W3C biçiminde IIS dosya günlükleri**.


## <a name="data-collection"></a>Veri toplama
Azure İzleyici, IIS günlük girişi günlüğü her kapatıldığında ve yeni bir tane oluşturulur. her Aracıdan toplar. Bu sıklığı tarafından denetlenir **günlük dosyası aktarma zamanlaması** varsayılan olarak günde bir kez olan IIS sitesi ayarlama. Örneğin, ayarları ise **saatlik**, sonra Azure İzleyici her saat bir günlük toplar.  Ayar **günlük**, ardından Azure izleyici günlüğü 24 saatte bir toplar.


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

## <a name="log-queries-with-iis-logs"></a>IIS günlükleri ile günlük sorguları
Aşağıdaki tabloda IIS günlük kayıtları almak günlük sorguları farklı örnekler sağlar.

| Sorgu | Açıklama |
|:--- |:--- |
| W3CIISLog |Tüm IIS günlük kaydı. |
| W3cııslog &#124; burada scStatus 500 == |Tüm IIS günlük kayıtları 500 dönüş durumu. |
| W3cııslog &#124; Count() işlevi tarafından CIP özetleme |İstemci IP adresine göre sayısı, IIS günlük girdilerinin. |
| W3cııslog &#124; burada csHost == "www\.contoso.com" &#124; Count() işlevi tarafından csUriStem özetleme |Konak www URL'sine göre sayısı, IIS günlük girişi\.contoso.com. |
| W3cııslog &#124; sum(csBytes) bilgisayara göre özetlemek &#124; 500000 Al |Her IIS bilgisayarına göre alınan toplam bayt sayısı. |

## <a name="next-steps"></a>Sonraki adımlar
* Azure İzleyici'nın diğer toplamak için yapılandırma [veri kaynakları](agent-data-sources.md) analiz.
* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için.