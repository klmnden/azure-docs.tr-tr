---
title: Desteklenen kaynak türleri aracılığıyla Azure kaynak durumu | Microsoft Docs
description: Desteklenen kaynak türleri aracılığıyla Azure kaynak durumu
services: Resource health
documentationcenter: ''
author: BernardoAMunoz
manager: ''
editor: ''
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 10/09/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: e37266f2438f9c6bc9de2d01624bda77f9d6ee8a
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36945882"
---
# <a name="resource-types-and-health-checks-in-azure-resource-health"></a>Kaynak türleri ve sistem durumu denetler Azure kaynak durumu
Kaynak durumu kaynak türleri tarafından yürütülen tüm denetimler tam bir listesi aşağıdadır.

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers
|Yürütülen denetimleri|
|---|
|<ul><li>Sunucunun hazır ve çalışır mı?</li><li>Sunucunun bellek yetersiz çalıştırıldı?</li><li>Sunucu başlatılıyor?</li><li>Sunucunun kurtarma?</li></ul>|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service
|Yürütülen denetimleri|
|---|
|<ul><li>API Management hizmeti açık ve çalışıyor?</li></ul>|

## <a name="microsoftcacheredisredis"></a>Microsoft.CacheRedis/Redis
|Yürütülen denetimleri|
|---|
|<ul><li>Tüm ön bellek düğümleri hazır ve çalışır misiniz?</li><li>Önbellek veri merkezi içinde erişilebilir?</li><li>Önbellek en fazla bağlantı sayısı üst sınırına?</li><li> Önbellek kullanılabilir bellek tükendi? </li><li>Önbellek çok sayıda sayfa hataları yaşıyor?</li><li>Önbellek ağır yük altında mı?</li></ul>|

## <a name="microsoftcdnprofile"></a>Microsoft.CDN/profile
|Yürütülen denetimleri|
|---|
|<ul> <li>Uç noktaları hiçbirini, kaldırılan veya yanlış yapılandırılmış durduruldu mu?</li><li>Ek portal CDN yapılandırma işlemleri için erişilebilir mi?</li><li>CDN uç noktası ile devam eden teslim sorunlar var mı?</li><li>Kullanıcılar kendi CDN kaynakların yapılandırmasını değiştirebilir miyim?</li><li>Yapılandırma değişiklikleri beklenen oranı yayılıyor misiniz?</li><li>Kullanıcılar, Azure portalı, PowerShell veya API kullanarak CDN yapılandırma yönetebilir miyim?</li> </ul>|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.classiccompute/virtualmachines
|Yürütülen denetimleri|
|---|
|<ul><li>Ana bilgisayar sunucusu hazır ve çalışır mı?</li><li>Ana bilgisayar işletim sistemi önyükleme tamamlandı?</li><li>Sanal makine kapsayıcısını sağlandığında ve güç?</li><li>Ana bilgisayar ve depolama hesabı arasında ağ bağlantısı var mı?</li><li>Konuk işletim sistemi önyükleme tamamlandı?</li><li>Devam eden planlı bakım var mı?</li></ul>|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.cognitiveservices/Accounts
|Yürütülen denetimleri|
|---|
|<ul><li>Hesap veri merkezi içinde erişilebilir?</li><li>Bilişsel hizmetler kaynak sağlayıcısı var mı?</li><li>Bilişsel hizmet uygun bölgede var mı?</li><li>Okuyabilir operations kaynak meta verilerini tutan depolama hesabında yapılmalıdır?</li><li>API çağrısı kotasına ulaşıldı?</li><li>API çağrısı okuma-üst sınırına ulaşılmış olabilir?</li></ul>|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.COMPUTE/virtualmachines
|Yürütülen denetimleri|
|---|
|<ul><li>Sunucu yukarı bu sanal makine barındırma ve çalıştırma?</li><li>Ana bilgisayar işletim sistemi önyükleme tamamlandı?</li><li>Sanal makine kapsayıcısını sağlandığında ve güç?</li><li>Ana bilgisayar ve depolama hesabı arasında ağ bağlantısı var mı?</li><li>Konuk işletim sistemi önyükleme tamamlandı?</li><li>Devam eden planlı bakım var mı?</li></ul>|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.datalakeanalytics/Accounts
|Yürütülen denetimleri|
|---|
|<ul><li>Gönderme veya Data Lake Analytics işlerini listeleme kullanıcılar deneyimli sorunları var mı?</li><li>Data Lake Analytics işlerini sistem hatalarını tamamlayamıyor misiniz?</li></ul>|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.datalakestore/Accounts
|Yürütülen denetimleri|
|---|
|<ul><li>Kullanıcılar verileri Data Lake Store için karşıya yükleme sorunları karşılaşmış?</li><li>Kullanıcılar verileri Lake Deposu'ndan veri indirme sorunları karşılaşmış?</li></ul>|

## <a name="microsoftdevicesiothubs"></a>Microsoft.devices/iothubs

|Yürütülen denetimleri|
|---|
|<ul><li>IOT hub'ı hazır ve çalışır mı?</li></ul>|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.documentdb/databaseAccounts
|Yürütülen denetimleri|
|---|
|<ul><li>Bulunmamış bir Azure Cosmos DB hizmet kullanılamazlık nedeniyle hizmet olmayan veritabanı veya koleksiyon istekleri?</li><li>Bulunmamış bir Azure Cosmos DB hizmet kullanılamazlık nedeniyle hizmet olmayan herhangi bir belge isteğini?</li></ul>|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/Connections
|Yürütülen denetimleri|
|---|
|<ul><li>VPN tüneli bağlı mı?</li><li>Yapılandırma çakışmaları bağlantı var mı?</li><li>Önceden paylaşılan anahtarlar düzgün yapılandırılmış?</li><li>VPN şirket içi cihaz ulaşılabilir mu?</li><li>Eşleşmeler IPSec/IKE Güvenlik İlkesi'nde bulunur?</li><li>S2S VPN bağlantısı düzgün sağlanan veya başarısız durumda mı?</li><li>VNET-VNET bağlantısı düzgün sağlanan veya başarısız durumda mı?</li></ul>|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.network/virtualNetworkGateways
|Yürütülen denetimleri|
|---|
|<ul><li>VPN ağ geçidi, internet'ten erişilebilen mi?</li><li>VPN ağ geçidi bekleme modunda mı?</li><li>VPN hizmeti üzerinde ağ geçidi çalışıyor mu?</li></ul>|

## <a name="microsoftnotificationhubsnamespace"></a>Microsoft.NotificationHubs/namespace
|Yürütülen denetimleri|
|---|
|<ul><li> Çalışma zamanı işlemleri kayıt, yükleme veya gönderme gibi ad alanında gerçekleştirilebilir?</li></ul>|

## <a name="microsoftpowerbiworkspacecollections"></a>Microsoft.PowerBI/workspaceCollections
|Yürütülen denetimleri|
|---|
|<ul><li>Konak işletim sistemi hazır ve çalışır mı?</li><li>WorkspaceCollection veri merkezi dışında erişilebildiğinden?</li><li>Powerbı kaynak sağlayıcısı var mı?</li><li>Powerbı hizmeti uygun bölgede var mı?</li></ul>|

## <a name="microsoftsearchsearchservices"></a>Microsoft.search/searchServices
|Yürütülen denetimleri|
|---|
|<ul><li>Kümede tanılama işlemleri gerçekleştirilebilir?</li></ul>|

## <a name="microsoftsqlserverdatabase"></a>Microsoft.SQL/Server/database
|Yürütülen denetimleri|
|---|
|<ul><li> Oturum açma bilgileri veritabanına bulunmamış mi?</li></ul>|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts
|Yürütülen denetimleri|
|---|
|<ul><li>Depolama hesabından veri okuma isteklerini Azure depolama platform sorunları nedeniyle başarısız oluyor?</li><li>Veri depolama hesabına yazma isteklerini Azure depolama platform sorunları nedeniyle başarısız oluyor?</li><li>Depolama hesabının bulunduğu depolama kümesi kullanılamıyor?</li></ul>|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs
|Yürütülen denetimleri|
|---|
|<ul><li>Burada iş yukarı yürütme ve çalışan tüm konaklar misiniz?</li><li>İş başlatılamadı?</li><li>Devam eden çalışma zamanı yükseltmeler var mı?</li><li>İş beklenen durumda (Örneğin çalışan veya müşteri tarafından durduruldu)?</li><li>İş, bellek özel durumları çıkışı karşılaştı?</li><li>Devam eden zamanlanmış işlem güncelleştirmeler var mı?</li><li>Yürütme Yöneticisi'ni (Denetim plan) var mı?</li></ul>|

## <a name="microsoftwebserverfarms"></a>Microsoft.web/serverFarms
|Yürütülen denetimleri|
|---|
|<ul><li>Ana bilgisayar sunucusu hazır ve çalışır mı?</li><li>Internet Information Services çalışıyor mu?</li><li>Yük Dengeleyici çalışıyor mu?</li><li>Uygulama hizmeti planı gelen veri merkezi içinde ulaşılabilen?</li><li>Depolama hesabı kullanılabilir serverFarm siteleri içeriğini barındıran??</li></ul>|

## <a name="microsoftwebsites"></a>Microsoft.Web/Sites
|Yürütülen denetimleri|
|---|
|<ul><li>Ana bilgisayar sunucusu hazır ve çalışır mı?</li><li>Internet Information server çalışıyor mu?</li><li>Yük Dengeleyici çalışıyor mu?</li><li>Web uygulaması veri merkezi içinde erişilebilir?</li><li>Depolama hesabı kullanılabilir site içeriğini barındıran?</li></ul>|

# <a name="next-steps"></a>Sonraki Adımlar
-  Bkz: [Azure hizmet sağlığı panosunu giriş](service-health-overview.md) ve [Azure kaynak durumu giriş](resource-health-overview.md) bunları daha iyi anlamak için. 
-  [Azure kaynak durumu hakkında sık sorulan sorular](resource-health-faq.md)
- Sistem durumu sorunları, bildirim almak için uyarıları ayarlayın. Daha fazla bilgi için bkz: [hizmet sistem durumu olayları için uyarıları Yapılandır](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 
