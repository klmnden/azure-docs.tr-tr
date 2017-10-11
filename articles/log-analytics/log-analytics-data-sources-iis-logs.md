---
title: "IIS günlüklerini günlük analizi | Microsoft Docs"
description: "Internet Information Services (IIS), günlük dosyalarında günlük analizi tarafından toplanan kullanıcı etkinliği depolar.  Bu makalede, IIS günlüklerini koleksiyonunu ve OMS depoya oluşturdukları kayıtları ayrıntılarını nasıl yapılandırılacağı açıklanmaktadır."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2017
ms.author: bwren
ms.openlocfilehash: 2114bdafb3b9fe2eb0632271840b8b70a76d10f1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="iis-logs-in-log-analytics"></a>IIS günlük analizi günlüğe kaydeder
Internet Information Services (IIS), günlük dosyalarında günlük analizi tarafından toplanan kullanıcı etkinliği depolar.  

![IIS günlükleri](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>IIS yapılandırma günlükleri
Günlük analizi girişleri gerekir böylece IIS tarafından oluşturulan günlük dosyalarını toplar [IIS için günlüğe kaydetmeyi yapılandırmak](https://technet.microsoft.com/library/hh831775.aspx).

Günlük analizi, yalnızca IIS günlük dosyalarına W3C biçiminde depolanan destekler ve özel alanlar veya Gelişmiş IIS günlüğü desteklemez.  
Günlük analizi Günlükleri NCSA veya IIS yerel biçiminde toplamaz.

Günlük analizi IIS günlüklerini yapılandırma [günlük analizi ayarları veri menüde](log-analytics-data-sources.md#configuring-data-sources).  Olduğundan herhangi bir yapılandırma gerekli seçerek dışında **toplamak W3C biçimi IIS günlük dosyaları**.

IIS günlük toplama etkinleştirdiğinizde, her sunucuda IIS günlük aktarma ayarı yapılandırmanız gerekir öneririz.

## <a name="data-collection"></a>Veri toplama
Günlük analizi IIS günlüğü girişlerini bağlı her kaynaktan yaklaşık her 15 dakikada toplar.  Aracı onun yerine üzerinden topladığı her olay günlüğüne kaydeder.  Aracı çevrimdışı olursa, aracıyı çevrimdışıyken olayları oluşturulmuş olsalar bile sonra günlük analizi olayları son devre dışı kaldığı toplar.

## <a name="iis-log-record-properties"></a>IIS günlük kaydı Özellikler
IIS günlük kayıtlarını sahip bir tür **W3CIISLog** ve aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olay toplandığı bilgisayarın adı. |
| CIP |İstemci IP adresi. |
| csMethod |GET veya POST gibi istek yöntemi. |
| csReferer |Kullanıcının geçerli siteye bir bağlantı takip sitesi. |
| csUserAgent |İstemci tarayıcısı türü. |
| csUserName |Sunucuya erişen kimliği doğrulanmış kullanıcının adı. Anonim kullanıcılar bir tire işaretiyle gösterilir. |
| csUriStem |Hedef bir web sayfası gibi isteği. |
| csUriQuery |Sorgu, varsa istemcinin gerçekleştirmeye çalıştığı olduğunu. |
| ManagementGroupName |Operations Manager aracıları için yönetim grubu adı.  Diğer aracılar için AOI - budur\<çalışma alanı kimliği\> |
| RemoteIPCountry |İstemcinin IP adresi ülke. |
| RemoteIPLatitude |İstemci IP adresi enlem. |
| RemoteIPLongitude |İstemci IP adresi boylam. |
| scStatus |HTTP durum kodu. |
| scSubStatus |Alt durum hata kodu. |
| scWin32Status |Windows durum kodu. |
| SIP |Web sunucusunun IP adresi. |
| SourceSystem |OpsMgr |
| Spor |İstemci sunucusundaki bağlantı noktasına bağlı. |
| sSiteName |IIS site adı. |
| TimeGenerated |Tarih ve saat girişi günlüğe kaydedildi. |
| TimeTaken |Süreyi milisaniye cinsinden isteği işleyemiyor. |

## <a name="log-searches-with-iis-logs"></a>IIS günlükleri ile günlük aramalar
Aşağıdaki tabloda IIS günlük kayıtlarını almak günlük sorguları farklı örnekler sağlar.

| Sorgu | Açıklama |
|:--- |:--- |
| Tür W3CIISLog = |Tüm IIS günlük kaydı. |
| Tür W3CIISLog scStatus = 500 = |Tüm IIS günlük kayıtları dönüş durumu 500 ile. |
| Tür = W3CIISLog &#124; Ölçü count() CIP tarafından |Count, IIS girişleri istemci IP adresi ile oturum açın. |
| Tür W3CIISLog csHost = = "www.contoso.com" &#124; Ölçü count() csUriStem tarafından |Count, IIS URL girdilerinin konak www.contoso.com için oturum açın. |
| Tür = W3CIISLog &#124; Bilgisayar & #124 tarafından ölçü Sum(csBytes); üst 500000 |Her IIS bilgisayar tarafından alınan toplam bayt sayısı. |

>[!NOTE]
> Çalışma alanınız [yeni Log Analytics sorgu diline](log-analytics-log-search-upgrade.md) yükseltilmişse, yukarıdaki sorguların aşağıdaki gibi değiştirilmesi gerekir.

> | Sorgu | Açıklama |
|:--- |:--- |
| W3CIISLog |Tüm IIS günlük kaydı. |
| W3CIISLog &#124; Burada scStatus 500 == |Tüm IIS günlük kayıtları dönüş durumu 500 ile. |
| W3CIISLog &#124; tarafından CIP Count() özetler |Count, IIS girişleri istemci IP adresi ile oturum açın. |
| W3CIISLog &#124; Burada csHost "www.contoso.com" &#124; == tarafından csUriStem Count() özetler |Count, IIS URL girdilerinin konak www.contoso.com için oturum açın. |
| W3CIISLog &#124; Bilgisayar & #124 tarafından SUM(csBytes) özetlemek; 500000 alın |Her IIS bilgisayar tarafından alınan toplam bayt sayısı. |

## <a name="next-steps"></a>Sonraki adımlar
* Diğer toplamak için günlük analizi yapılandırma [veri kaynakları](log-analytics-data-sources.md) çözümleme için.
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümleri toplanan verileri çözümlemek için.
* Günlük analizi proaktif olarak, IIS günlüklerine bulunan önemli koşullar konusunda sizi bilgilendirmek için uyarıları yapılandırın.
