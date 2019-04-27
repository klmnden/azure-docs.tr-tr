---
title: Desteklenen kaynak türleri Azure kaynak durumu aracılığıyla | Microsoft Docs
description: Azure kaynak durumu aracılığıyla desteklenen kaynak türleri
author: stephbaron
ms.author: stbaron
ms.topic: conceptual
ms.service: service-health
ms.date: 01/29/2019
ms.openlocfilehash: 0f79a1eed044814d6c2e27f4eadb5ba68a47303f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622292"
---
# <a name="resource-types-and-health-checks-in-azure-resource-health"></a>Kaynak türleri ve sistem durumu Azure kaynak durumu denetler
Kaynak durumu kaynak türleri tarafından yürütülen tüm denetimleri tam bir listesi aşağıdadır.

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers
|Yürütülen denetimleri|
|---|
|<ul><li>Sunucunun çalışır mı?</li><li>Sunucunun belleği yetersiz çalıştırıldı?</li><li>Sunucu başlatılıyor?</li><li>Sunucu kurtarılıyor?</li></ul>|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service
|Yürütülen denetimleri|
|---|
|<ul><li>API Management hizmeti açık ve çalışıyor?</li></ul>|

## <a name="microsoftcacheredisredis"></a>Microsoft.CacheRedis/Redis
|Yürütülen denetimleri|
|---|
|<ul><li>Tüm önbellek düğümleri çalışmaya misiniz?</li><li>Önbellek veri merkezi içinde erişilebilir?</li><li>Önbellek bağlantıları sayısı üst sınırına?</li><li> Önbellek, kullanılabilir bellek tüketmiş? </li><li>Önbellek çok sayıda sayfa hataları yaşıyor?</li><li>Önbellek ağır yük altında mı?</li></ul>|

## <a name="microsoftcdnprofile"></a>Microsoft.CDN/profile
|Yürütülen denetimleri|
|---|
|<ul> <li>Ek portalı CDN yapılandırma işlemleri için erişilebilir?</li><li>CDN uç noktası ile sürekli teslim sorunları var mıdır?</li><li>Kullanıcılar, CDN kaynaklarını yapılandırmasını değiştirebilir miyim?</li><li>Yapılandırma değişiklikleri beklenen oranı yayma misiniz?</li><li>Kullanıcılar, Azure portalı, PowerShell veya API kullanarak CDN yapılandırmayı yönetebilir miyim?</li> </ul>|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.classiccompute/virtualmachines
|Yürütülen denetimleri|
|---|
|<ul><li>Konak sunucunun çalışır mı?</li><li>Konak işletim sistemi önyükleme tamamladı?</li><li>Sanal makine kapsayıcısını sağlandığında ve güç?</li><li>Konak ve depolama hesabı arasında ağ bağlantısı var mı?</li><li>Konuk işletim sistemi önyükleme tamamladı?</li><li>Devam eden planlı bakım var mı?</li></ul>|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.cognitiveservices/Accounts
|Yürütülen denetimleri|
|---|
|<ul><li>Hesap veri merkezi içinde erişilebilir?</li><li>Bilişsel Hizmetleri kaynak sağlayıcı kullanılabilir mi?</li><li>Bilişsel hizmet uygun bölgede kullanılabilir mi?</li><li>Bilgi işlem depolama hesabı kaynak meta verileri tutan gerçekleştirilebilir?</li><li>API çağrısı kotasını ulaşıldı?</li><li>API çağrısı okuma sınırına ulaşıldı?</li></ul>|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.COMPUTE/virtualmachines
|Yürütülen denetimleri|
|---|
|<ul><li>Sunucu yukarı bu sanal makineyi barındıran ve çalışır?</li><li>Konak işletim sistemi önyükleme tamamladı?</li><li>Sanal makine kapsayıcısını sağlandığında ve güç?</li><li>Konak ve depolama hesabı arasında ağ bağlantısı var mı?</li><li>Konuk işletim sistemi önyükleme tamamladı?</li><li>Devam eden planlı bakım var mı?</li></ul>|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories
|Yürütülen denetimleri|
|---|
|<ul><li>İşlem hattı çalıştırması hataları var. neydi?</li><li>Kümenin iyi durumda Data Factory barındırıyor?</li></ul>|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.datalakeanalytics/Accounts
|Yürütülen denetimleri|
|---|
|<ul><li>Gönderme veya Data Lake Analytics işlerini listeleme kullanıcılar deneyimli sorunlar mı var?</li><li>Data Lake Analytics işlerini sistem hataları nedeniyle tamamlayamıyor misiniz?</li></ul>|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.datalakestore/Accounts
|Yürütülen denetimleri|
|---|
|<ul><li>Kullanıcılar, Data Lake Store için karşıya veri yükleme sorunlarını gördünüz mü?</li><li>Kullanıcılar, Data Lake Store ' veri indirme sorunları gördünüz mü?</li></ul>|

## <a name="microsoftdatamigrationservices"></a>Microsoft.datamigration/Services
|Yürütülen denetimleri|
|---|
|<ul><li>Veritabanı geçiş hizmeti sağlamak başarısız oldu?</li><li>Veritabanı geçiş hizmetini eylemsizlik veya Kullanıcı isteği nedeniyle durdu?</li></ul>|

## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers
|Yürütülen denetimleri|
|---|
|<ul><li>Bakım nedeniyle sunucu kullanılamıyor?</li><li>Sunucu yeniden yapılandırma nedeniyle kullanılamıyor?</li></ul>|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers
|Yürütülen denetimleri|
|---|
|<ul><li>Bakım nedeniyle sunucu kullanılamıyor?</li><li>Sunucu yeniden yapılandırma nedeniyle kullanılamıyor?</li></ul>|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers
|Yürütülen denetimleri|
|---|
|<ul><li>Bakım nedeniyle sunucu kullanılamıyor?</li><li>Sunucu yeniden yapılandırma nedeniyle kullanılamıyor?</li></ul>|

## <a name="microsoftdevicesiothubs"></a>Microsoft.devices/iothubs
|Yürütülen denetimleri|
|---|
|<ul><li>IOT hub'ı çalışır mı?</li></ul>|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.documentdb/databaseAccounts
|Yürütülen denetimleri|
|---|
|<ul><li>Bulunmamış bir Azure Cosmos DB hizmet kullanılabilir olmaması nedeniyle hizmet olmayan veritabanı veya koleksiyon istekleri?</li><li>Bulunmamış bir Azure Cosmos DB hizmet kullanılabilir olmaması nedeniyle hizmet değil herhangi bir belge isteğinin?</li></ul>|

## <a name="microsofteventhubnamespaces"></a>Microsoft.eventhub/namespaces
|Yürütülen denetimleri|
|---|
|<ul><li>Event Hubs ad alanı, kullanıcı tarafından oluşturulan hatalar yaşıyor?</li><li>Event Hubs ad alanı şu anda yükseltiliyor?</li></ul>|

## <a name="microsofthdinsightclusters"></a>Microsoft.hdinsight/Clusters
|Yürütülen denetimleri|
|---|
|<ul><li>Çekirdek Hizmetleri HDInsight kümesinde kullanılabilir mi?</li><li>HDInsight küme BYOK bekleme sırasında şifreleme anahtarı erişebilir miyim?</li></ul>|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults
|Yürütülen denetimleri|
|---|
|<ul><li>İstekleri anahtar kasasına Azure KeyVault platform sorunları nedeniyle başarısız oluyor?</li><li>İstekleri anahtar kasası için müşteri tarafından atılan çok fazla istek nedeniyle kısıtlanan?</li></ul>|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationgateways
|Yürütülen denetimleri|
|---|
|<ul><li>Performans düzeyi düşürülmüş uygulama ağ geçidinin var mı?</li><li>Uygulama ağ geçidi kullanılabilir mi?</li></ul>|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/Connections
|Yürütülen denetimleri|
|---|
|<ul><li>VPN tüneli bağlı mı?</li><li>Yapılandırma çakışmaları bağlantı vardır?</li><li>Önceden paylaşılan anahtarlar düzgün şekilde yapılandırıldığından?</li><li>Şirket içi VPN cihazının ulaşılabildiğinden?</li><li>Uyuşmazlıkların IPSec/IKE ilkesi var mı?</li><li>S2S VPN bağlantısının düzgün olarak sağlanan veya başarısız durumda mı?</li><li>VNET-VNET bağlantısı düzgün bir şekilde sağlanmış veya başarısız durumda mı?</li></ul>|

## <a name="microsoftnetworkexpressreoutecircuits"></a>Microsoft.Network/expressreoutecircuits
|Yürütülen denetimleri|
|---|
|<ul><li>ExpressRoute bağlantı hattı iyi durumda?</li></ul>|

## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors
|Yürütülen denetimleri|
|---|
|<ul><li>Sistem durumu araştırmaları hatalarla ön kapısı arka uçlar yanıt?</li><li>Gecikmeli yapılandırma değişiklikleri?</li></ul>|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.network/virtualNetworkGateways
|Yürütülen denetimleri|
|---|
|<ul><li>VPN ağ geçidi, internet'ten erişilebilen mi?</li><li>VPN ağ geçidinin bekleme modunda mi?</li><li>VPN hizmeti ağ geçidi üzerinde çalışıyor mu?</li></ul>|

## <a name="microsoftnotificationhubsnamespace"></a>Microsoft.NotificationHubs/namespace
|Yürütülen denetimleri|
|---|
|<ul><li>Çalışma zamanı işlemleri kayıt, yükleme veya gönderme gibi ad alanı üzerinde gerçekleştirilebilir?</li></ul>|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.operationalinsights/Workspaces
|Yürütülen denetimleri|
|---|
|<ul><li>Gecikmeler çalışma alanı için dizin var mı?</li></ul>|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/Capacities
|Yürütülen denetimleri|
|---|
|<ul><li>Kapasite kaynak çalışır mı?</li><li>Tüm iş yüklerini çalışır mı?</li></ul>|

## <a name="microsoftpowerbiworkspacecollections"></a>Microsoft.PowerBI/workspaceCollections
|Yürütülen denetimleri|
|---|
|<ul><li>Konak işletim sistemi hazır ve çalışır durumda?</li><li>Veri Merkezi dışından erişilebilir workspaceCollection mi?</li><li>Power BI kaynak sağlayıcısı kullanılabilir mi?</li><li>Power BI hizmetinde uygun bölgede kullanılabilir mi?</li></ul>|

## <a name="microsoftsearchsearchservices"></a>Microsoft.search/searchServices
|Yürütülen denetimleri|
|---|
|<ul><li>Kümede tanılama işlemleri gerçekleştirilebilir?</li></ul>|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces
|Yürütülen denetimleri|
|---|
|<ul><li>Müşteriler, kullanıcı tarafından oluşturulan Service Bus hatalar sorunu yaşıyor musunuz?</li><li>Kullanıcılar, bir Service Bus ad alanı yükseltmesi nedeniyle geçici hatalar arasında bir artış sorunu yaşıyor musunuz?</li></ul>|

## <a name="microsoftsqlserverdatabase"></a>Microsoft.SQL/Server/database
|Yürütülen denetimleri|
|---|
|<ul><li> Oturum açma bilgileri veritabanına var. neydi?</li></ul>|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts
|Yürütülen denetimleri|
|---|
|<ul><li>Depolama hesabından veri okuma isteklerinin Azure depolama platformu sorunları nedeniyle başarısız oluyor?</li><li>Veri depolama hesabına yazma isteklerinin Azure depolama platformu sorunları nedeniyle başarısız oluyor?</li><li>Depolama hesabının bulunduğu depolama kümesi kullanılamıyor mu?</li></ul>|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs
|Yürütülen denetimleri|
|---|
|<ul><li>Burada iş yukarı yürütme ile çalışan tüm Konaklara misiniz?</li><li>İşi başlatamadı?</li><li>Devam eden çalışma zamanını yükseltme var mı?</li><li>İş, beklenen bir durumda (Örneğin çalışan veya müşteri tarafından durduruldu)?</li><li>İşi dışarı bellek özel durumları karşılaştı?</li><li>Devam eden zamanlanmış işlem güncelleştirmeleri var mı?</li><li>Yürütme Yöneticisi (denetim planı) kullanılabilir mi?</li></ul>|

## <a name="microsoftwebserverfarms"></a>Microsoft.web/serverFarms
|Yürütülen denetimleri|
|---|
|<ul><li>Konak sunucunun çalışır mı?</li><li>Internet Information Services çalışıyor mu?</li><li>Yük Dengeleyici çalışıyor mu?</li><li>App Service planı'den veri merkezi içinde ulaşılabilir?</li><li>Depolama hesabı kullanılabilir serverFarm siteleri içeriğini barındıran??</li></ul>|

## <a name="microsoftwebsites"></a>Microsoft.web/sites
|Yürütülen denetimleri|
|---|
|<ul><li>Konak sunucunun çalışır mı?</li><li>Internet Information server çalışıyor mu?</li><li>Yük Dengeleyici çalışıyor mu?</li><li>Web uygulaması veri merkezi içinde erişilebilir?</li><li>Depolama hesabı kullanılabilir site içeriği barındırıyor?</li></ul>|

# <a name="next-steps"></a>Sonraki Adımlar
-  Bkz: [Azure hizmet durumu Panosu giriş](service-health-overview.md) ve [Azure kaynak durumu giriş](resource-health-overview.md) bunları daha iyi anlamak için. 
-  [Azure kaynak durumu hakkında sık sorulan sorular](resource-health-faq.md)
- Sistem durumu sorunları bildirim almak için uyarılar ayarlayın. Daha fazla bilgi için [hizmet durumu olayları için uyarıları yapılandırın](../azure-monitor/platform/alerts-activity-log-service-notifications.md). 
