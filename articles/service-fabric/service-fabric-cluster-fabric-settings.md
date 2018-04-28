---
title: Azure Service Fabric kümesi ayarları değiştirme | Microsoft Docs
description: Bu makalede, yapı ayarları ve özelleştirebileceğiniz doku yükseltme ilkeler açıklanır.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 7ced36bf-bd3f-474f-a03a-6ebdbc9677e2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/09/2018
ms.author: aljo
ms.openlocfilehash: d2454b09e69c26317499d17ea89b6829f52dedd2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="customize-service-fabric-cluster-settings-and-fabric-upgrade-policy"></a>Service Fabric kümesi ayarlarını ve yapı yükseltme İlkesi özelleştirme
Bu belge çeşitli doku ayarlarını özelleştirmek anlatır ve yapı Service Fabric kümesi için ilke yükseltin. Aralarında özelleştirebilirsiniz [Azure portal](https://portal.azure.com) veya bir Azure Resource Manager şablonu kullanarak.

> [!NOTE]
> Tüm ayarlar Portalı'nda kullanılabilir. Aşağıda listelenen bir ayar portalı yoluyla kullanılamaz durumda bir Azure Resource Manager şablonu kullanarak özelleştirin.
> 

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Resource Manager şablonları kullanarak küme ayarlarını özelleştirme
Aşağıdaki adımlar yeni bir ayar eklemek nasıl çalışılacağını *MaxDiskQuotaInMB* için *tanılama* bölümü.

1. Şuraya gidin: https://resources.azure.com
2. Aboneliğinize genişleterek gidin **abonelikleri** -> **\<bilgisayarınızı abonelik >** -> **resourceGroups**  ->   **\<Bilgisayarınızı kaynak grubu >** -> **sağlayıcıları** -> **Microsoft.ServiceFabric**  ->  **kümeleri** -> **\<küme adınız >**
3. Sağ alt köşesinde üst seçin **okuma/yazma.**
4. Seçin **Düzenle** ve güncelleştirme `fabricSettings` JSON öğesi ve yeni bir öğe ekleyin:

```
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

Bir liste verilmiştir dokusu özelleştirebileceğiniz, ayarları bölümü tarafından düzenlenir.

## <a name="applicationgatewayhttp"></a>ApplicationGateway/Http
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|ApplicationCertificateValidationPolicy|Varsayılan dizesidir L "Hiçbiri"|Statik| ApplicationCertificateValidationPolicy: None: sunucu sertifikası; doğrulamaz İstek başarılı. ServiceCertificateThumbprints: ters proxy güvenebileceği uzak sertifikaları parmak izlerini virgülle ayrılmış listesi için yapılandırma ServiceCertificateThumbprints bakın. ServiceCommonNameAndIssuer: ters proxy güvenebileceği uzak sertifikaları konu adı ve verenin parmak izi için yapılandırma ServiceCommonNameAndIssuer bakın. |
|BodyChunkSize |Uint, 16384 varsayılandır |Dinamik| Gövde okumak için kullanılan bayt öbek boyutunu sağlar. |
|CrlCheckingFlag|uint, varsayılan 0x40000000 olduğu |Dinamik| Uygulama/hizmet sertifika zinciri doğrulaması bayrakları; Örneğin CRL 0x10000000 denetleme CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN 0x40000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT 0x80000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ONLY ayarını 0 devre dışı bırakır CRL denetimi tam desteklenen değerler listesi CertGetCertificateChain dwFlags tarafından belgelenmiştir: http://msdn.microsoft.com/library/windows/desktop/aa376078(v=vs.85).aspx  |
|DefaultHttpRequestTimeout |Saniye cinsinden süre. Varsayılan 120'dir |Dinamik|TimeSpan saniye cinsinden belirtin.  Http uygulama ağ geçidi işlenmekte olan http isteği için varsayılan istek zaman aşımını sağlar. |
|ForwardClientCertificate|bool, varsayılan FALSE olur|Dinamik| |
|GatewayAuthCredentialType |dize, varsayılan "Hiçbiri": |Statik| Http uygulama ağ geçidi uç nokta geçerli değerlere kullanılacak güvenlik kimlik bilgileri türünü gösterir "hiçbiri / X 509. |
|GatewayX509CertificateFindType |Varsayılan "FindByThumbprint" dizesidir |Dinamik| GatewayX509CertificateStoreName desteklenen değeri tarafından belirtilen deposundaki sertifikayı aramak nasıl gösterir: FindByThumbprint; FindBySubjectName. |
|GatewayX509CertificateFindValue | dize, varsayılan değer "" |Dinamik| Arama filtresi değeri http uygulama ağ geçidi sertifikası bulmak için kullanılır. Bu sertifika, https uç noktada yapılandırılır ve hizmetler tarafından gerekirse uygulama kimliğini doğrulamak için de kullanılabilir. FindValue ilk Aranan; ve yoksa; FindValueSecondary aranır. |
|GatewayX509CertificateFindValueSecondary | dize, varsayılan değer "" |Dinamik|Arama filtresi değeri http uygulama ağ geçidi sertifikası bulmak için kullanılır. Bu sertifika, https uç noktada yapılandırılır ve hizmetler tarafından gerekirse uygulama kimliğini doğrulamak için de kullanılabilir. FindValue ilk Aranan; ve yoksa; FindValueSecondary aranır.|
|GatewayX509CertificateStoreName |dize, varsayılan değer "Benim" |Dinamik| Http uygulama ağ geçidi sertifikasını içeren X.509 Sertifika deposu adı. |
|HttpRequestConnectTimeout|TimeSpan, Common::TimeSpan::FromSeconds(5) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin.  Http uygulama ağ geçidi'nden gönderilen http isteklerini bağlantı zaman aşımı sağlar.  |
|IgnoreCrlOfflineError|bool, varsayılan true'dur.|Dinamik|Uygulama/hizmet sertifika doğrulama için CRL çevrimdışı hatayı yok sayıp görüntülenmeyeceğini belirtir. |
|IsEnabled |Bool, varsayılan false |Statik| HttpApplicationGateway etkinleştirir/devre dışı bırakır. HttpApplicationGateway varsayılan olarak devre dışıdır ve bu yapılandırma etkinleştirmek üzere ayarlanması gerekir. |
|NumberOfParallelOperations | Uint, varsayılan 5000'dir |Statik|Http sunucusu sıraya göndermek için okuma sayısı. Bu HttpGateway tarafından karşılanan eşzamanlı istek sayısını denetler. |
|RemoveServiceResponseHeaders|Varsayılan L "tarih; dizesidir Sunucu"|Statik|Noktalı virgül / virgülle ayrılmış hizmet yanıttan; kaldırılacak yanıt üstbilgilerinin listesi istemciye iletmeden önce. Bu boş dize olarak ayarlanırsa; Hizmet olarak tarafından döndürülen tüm üstbilgileri geçirmek-değil. yani Tarih ve sunucu üzerine yazma |
|ResolveServiceBackoffInterval |Zamanı saniye cinsinden varsayılan 5'tir |Dinamik|TimeSpan saniye cinsinden belirtin.  Başarısız yeniden denemeden önce varsayılan dışı aralığı çözmek verir işlemi hizmet. |
|SecureOnlyMode|bool, varsayılan FALSE olur|Dinamik| SecureOnlyMode: true: Ters Proxy yalnızca güvenli uç noktalarını yayımlama hizmetleri iletin. false: Ters Proxy, güvenli/güvenli olmayan Uç noktalara isteklerini iletebileceği.  |
|ServiceCertificateThumbprints|Varsayılan L dizesidir""|Dinamik| |

## <a name="applicationgatewayhttpservicecommonnameandissuer"></a>Http/ApplicationGateway/ServiceCommonNameAndIssuer
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, varsayılan, Yok'tur|Dinamik|  |

## <a name="clustermanager"></a>ClusterManager
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|EnableDefaultServicesUpgrade | Bool, varsayılan false |Dinamik|Uygulama yükseltme sırasında yükseltme varsayılan hizmetleri sağlar. Varsayılan hizmet açıklamaları yükseltme sonrasında üzerine. |
|FabricUpgradeHealthCheckInterval |Zaman içinde varsayılan 60 saniyedir |Dinamik|Sistem durumu sıklığını izlenen bir Fabric yükseltmesi sırasında denetleyin |
|FabricUpgradeStatusPollInterval |Zaman içinde varsayılan 60 saniyedir |Dinamik|Fabric yükseltme durumu için yoklama sıklığı. Bu değer, herhangi bir GetFabricUpgradeProgress çağrısına güncelleştirmesi oranını belirler. |
|ImageBuilderTimeoutBuffer |Zamanı saniye cinsinden varsayılan 3. |Dinamik|TimeSpan saniye cinsinden belirtin. İstemciye döndürülecek Image Builder belirli zaman aşımı hataları için izin vermek için süre miktarı. Bu arabellek çok küçük ise; ardından istemci sunucunun önce zaman aşımına uğradı ve genel zaman aşımı hatası alır. |
|InfrastructureTaskHealthCheckRetryTimeout | Zaman içinde varsayılan 60 saniyedir |Dinamik|TimeSpan saniye cinsinden belirtin. Yeniden deneniyor harcadıkları süre bir altyapı görevi sonrası işlerken sistem durumu denetimleri başarısız oldu. Geçirilen sistem durumu denetimi Gözlemleme bu süreölçer sıfırlanır. |
|InfrastructureTaskHealthCheckStableDuration | Zamanı saniye cinsinden varsayılan 0'dır|Dinamik| TimeSpan saniye cinsinden belirtin. Bir altyapı görevinin sonrası işlemeden önce ardışık geçirilen sistem durumu denetimini gözlemlemek süreyi başarıyla tamamlanır. Başarısız sistem durumu denetimi Gözlemleme bu süreölçer sıfırlanır. |
|InfrastructureTaskHealthCheckWaitDuration |Zamanı saniye cinsinden varsayılan 0'dır|Dinamik| TimeSpan saniye cinsinden belirtin. Bir altyapı görevi sonrası işledikten sonra sistem durumu başlatmadan önce beklenecek süre miktarını denetler. |
|InfrastructureTaskProcessingInterval | Zamanı saniye cinsinden varsayılan 10'dur |Dinamik|TimeSpan saniye cinsinden belirtin. Durum makinesinin işleme altyapısı görev tarafından kullanılan işleme aralığı. |
|MaxCommunicationTimeout |Zamanı saniye cinsinden 600 varsayılandır |Dinamik|TimeSpan saniye cinsinden belirtin. ClusterManager ve diğer sistem arasındaki iç iletişim için en uzun zaman aşımı (yani; Hizmetleri Adlandırma Hizmeti; "Yük Devretme Yöneticisi ve VS.). Bu zaman aşımı (olabilir gibi her bir istemci işlemin sistem bileşenleri arasında birden çok iletişim) genel MaxOperationTimeout küçük olmalıdır. |
|MaxDataMigrationTimeout |Zamanı saniye cinsinden 600 varsayılandır |Dinamik|TimeSpan saniye cinsinden belirtin. Fabric yükseltmesi gerçekleştikten sonra veri geçiş kurtarma işlemleri için en büyük zaman aşımı. |
|MaxOperationRetryDelay |Zamanı saniye cinsinden varsayılan 5'tir|Dinamik| TimeSpan saniye cinsinden belirtin. Hataları karşılaştığında iç deneme en büyük gecikme. |
|MaxOperationTimeout |Zamanı saniye cinsinden MaxValue varsayılandır |Dinamik| TimeSpan saniye cinsinden belirtin. Dahili olarak ClusterManager işlemlerini işlemek için maksimum genel zaman aşımı. |
|MaxTimeoutRetryBuffer | Zamanı saniye cinsinden 600 varsayılandır |Dinamik|TimeSpan saniye cinsinden belirtin. Dahili zaman aşımı nedeniyle denenirken maksimum işlemi zaman aşımı <Original Time out>  +  <MaxTimeoutRetryBuffer>. Ek zaman aşımı MinOperationTimeout artışlarla eklenir. |
|MinOperationTimeout | Zaman içinde varsayılan 60 saniyedir |Dinamik|TimeSpan saniye cinsinden belirtin. Dahili olarak ClusterManager işlemlerini işlemek için en düşük genel zaman aşımı. |
|MinReplicaSetSize |Int, varsayılan 3. |İzin Verilmiyor|MinReplicaSetSize ClusterManager için. |
|PlacementConstraints | dize, varsayılan değer "" |İzin Verilmiyor|ClusterManager için PlacementConstraints. |
|QuorumLossWaitDuration |Zamanı saniye cinsinden MaxValue varsayılandır |İzin Verilmiyor| TimeSpan saniye cinsinden belirtin. ClusterManager için QuorumLossWaitDuration. |
|ReplicaRestartWaitDuration |Saat (60,0 * 30) saniye cinsinden varsayılandır|İzin Verilmiyor|TimeSpan saniye cinsinden belirtin. ClusterManager için ReplicaRestartWaitDuration. |
|ReplicaSetCheckTimeoutRollbackOverride |Zamanı saniye cinsinden varsayılan 1200'dür |Dinamik| TimeSpan saniye cinsinden belirtin. ReplicaSetCheckTimeout DWORD en büyük değere ayarlarsanız; geri alma amaçları doğrultusunda bu yapılandırma değeri ile geçersiz kılındı sonra. İleri alma için kullanılan değer hiçbir zaman geçersiz kılındı. |
|SkipRollbackUpdateDefaultService | Bool, varsayılan false |Dinamik|CM uygulaması yükseltme geri alma sırasında dönüştürülüyor güncelleştirilmiş varsayılan Hizmetleri atlar. |
|StandByReplicaKeepDuration | Zamanı saniye cinsinden varsayılan (3600.0 * 2).|İzin Verilmiyor|TimeSpan saniye cinsinden belirtin. ClusterManager için StandByReplicaKeepDuration. |
|TargetReplicaSetSize |Int, 7 varsayılandır |İzin Verilmiyor|ClusterManager için TargetReplicaSetSize. |
|UpgradeHealthCheckInterval |Zaman içinde varsayılan 60 saniyedir |Dinamik|İzlenen uygulama yükseltmeler sırasında sıklığı sistem durumunu denetler |
|UpgradeStatusPollInterval |Zaman içinde varsayılan 60 saniyedir |Dinamik|Uygulama yükseltme durumu için yoklama sıklığı. Bu değer, herhangi bir GetApplicationUpgradeProgress çağrısına güncelleştirmesi oranını belirler. |

## <a name="common"></a>Common
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PerfMonitorInterval |Zamanı saniye olarak varsayılan 1. |Dinamik|TimeSpan saniye cinsinden belirtin. Performans izleme aralığı. 0 veya negatif değer ayarına izlemeyi devre dışı bırakır. |

## <a name="defragmentationemptynodedistributionpolicy"></a>DefragmentationEmptyNodeDistributionPolicy
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|KeyIntegerValueMap, varsayılan, Yok'tur|Dinamik|İlke birleştirme düğüm boşaltma izlediği belirtir. Belirli bir metrik için 0, BT düğümleri UDs ve FDs arasında eşit olarak birleştirmek denemelisiniz gösterir; 1 yalnızca düğümlerin birleştirilmesi gösterir |

## <a name="defragmentationmetrics"></a>DefragmentationMetrics
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|KeyBoolValueMap, varsayılan, Yok'tur|Dinamik|Birleştirme ve Yük Dengeleme için kullanılması gereken ölçümleri belirler. |

## <a name="defragmentationmetricspercentornumberofemptynodestriggeringthreshold"></a>DefragmentationMetricsPercentOrNumberOfEmptyNodesTriggeringThreshold
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, varsayılan, Yok'tur|Dinamik|Aralık içinde her iki yüzde belirterek birleştirilmiş küme göz önünde bulundurmanız gereken boş düğüm sayısını belirler [0.0-1.0) ya da boş düğüm sayısını sayı > = 1.0 |

## <a name="diagnostics"></a>Tanılama
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|AppDiagnosticStoreAccessRequiresImpersonation |Bool, varsayılan değer true şeklindedir | Dinamik |Desteklemediğini tanılama erişme uygulama adına depoladığında kimliğe bürünme gereklidir. |
|AppEtwTraceDeletionAgeInDays |Int, varsayılan 3. | Dinamik |Daha sonra şu uygulama ETW izlemelerini içeren eski ETL dosyaları silmek gün sayısı. |
|ApplicationLogsFormatVersion |Int, varsayılan 0'dır | Dinamik |Uygulama için sürüm biçimi günlüğe kaydeder. 0 ve 1 değerleri desteklenir. Sürüm 1, sürüm 0 ETW olay kaydından daha fazla alan içerir. |
|ClusterId |Dize | Dinamik |Küme benzersiz kimliği. Bu, küme oluşturulduğunda oluşturulur. |
|ConsumerInstances |Dize | Dinamik |DCA tüketici örneklerinin listesi. |
|DiskFullSafetySpaceInMB |Int, varsayılan 1024'tür. | Dinamik |Kalan disk alanı DCA tarafından kullanımdan korumak için MB. |
|EnableCircularTraceSession |Bool, varsayılan false | Statik |Bayrak döngüsel izleme oturumlarını kullanılması gerekip gerekmediğini belirtir. |
|EnableTelemetry |Bool, varsayılan değer true şeklindedir | Dinamik |Bu etkinleştirme veya devre dışı telemetri geçiyor. |
|MaxDiskQuotaInMB |Int, 65536 varsayılandır | Dinamik |Disk kotası Windows Fabric MB günlük dosyalarında. |
|ProducerInstances |Dize | Dinamik |DCA üretici örneklerinin listesi. |

## <a name="dnsservice"></a>DnsService
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|IsEnabled|bool, varsayılan FALSE olur|Statik| |
|Instancecount|int, varsayılan -1'dir|Statik|  |

## <a name="fabricclient"></a>FabricClient
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|ConnectionInitializationTimeout |Zamanı saniye cinsinden varsayılan 2'dir |Dinamik|TimeSpan saniye cinsinden belirtin. Bağlantı zaman aşımı aralığı her zaman istemci için ağ geçidi için bir bağlantı açmak çalışır.|
|HealthOperationTimeout |Zamanı saniye cinsinden varsayılan 120'dir |Dinamik|TimeSpan saniye cinsinden belirtin. Sistem Yöneticisi için gönderilen bir raporu iletisi için zaman aşımı. |
|HealthReportRetrySendInterval |Zamanı saniye cinsinden varsayılan 30'dur |Dinamik|TimeSpan saniye cinsinden belirtin. Sağlık Yöneticisi birikmiş sistem durumu raporlarının bileşeni yeniden gönderir aralığı raporlama. |
|HealthReportSendInterval |Zamanı saniye cinsinden varsayılan 30'dur |Dinamik|TimeSpan saniye cinsinden belirtin. Bileşen Durumu Yöneticisi birikmiş sistem durumu raporlarının gönderir aralığı raporlama. |
|KeepAliveIntervalInSeconds |Int, varsayılan 20'dir |Statik|Seçtiğiniz aralık FabricClient taşıma için ağ geçidi etkin tutma iletileri gönderir. İçin 0; keepAlive devre dışı bırakılır. Pozitif bir değer olmalıdır. |
|MaxFileSenderThreads |Uint, varsayılan 10'dur |Statik|Paralel olarak aktarılan dosyaların maksimum sayı. |
|NodeAddresses |dize, varsayılan değer "" |Statik|Adresleri (bağlantı dizeleri) ile iletişim kurmak için kullanılan farklı düğümlerde koleksiyonu adlandırma hizmeti. Başlangıçta istemci bir adres rastgele seçerek bağlanır. Birden fazla bağlantı dizesi tarafından sağlanan ve bir bağlantı iletişimi veya zaman aşımı hatası nedeniyle başarısız olursa; sonraki adresi sıralı olarak kullanmak için istemci geçiş yapar. Adlandırma Hizmeti bölümü yeniden deneme semantiği hakkında ayrıntılı bilgi için yeniden deneme adresini bakın. |
|PartitionLocationCacheLimit |Int, 100000 varsayılandır |Statik|(Sınırsız için 0 olarak ayarlayın) hizmeti çözümleme için önbelleğe alınan bölüm sayısı. |
|RetryBackoffInterval |Zamanı saniye cinsinden varsayılan 3. |Dinamik|TimeSpan saniye cinsinden belirtin. İşlemi yeniden denemeden önce geri alma aralığı. |
|ServiceChangePollInterval |Zamanı saniye cinsinden varsayılan 120'dir |Dinamik|TimeSpan saniye cinsinden belirtin. Hizmet için ardışık yoklamalar arasındaki aralığı ağ geçidi kayıtlı hizmet değişiklik bildirimleri geri aramalar için istemciden değiştirir. |

## <a name="fabrichost"></a>FabricHost
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|ActivationMaxFailureCount |Int, varsayılan 10'dur |Dinamik|Bu sistem vazgeçmeden önce başarısız etkinleştirme deneyecek en büyük sayı değil. |
|ActivationMaxRetryInterval |Zaman içinde varsayılan 300 saniyedir |Dinamik|TimeSpan saniye cinsinden belirtin. Etkinleştirme için en fazla yeniden deneme aralığı. Her sürekli hatada yeniden deneme aralığını (ActivationMaxRetryInterval; Min hesaplanır Sürekli hata sayısı * ActivationRetryBackoffInterval). |
|ActivationRetryBackoffInterval |Zamanı saniye cinsinden varsayılan 5'tir |Dinamik|TimeSpan saniye cinsinden belirtin. Geri Çekilme aralığı her etkinleştirme hatası; sistem etkinleştirme için MaxActivationFailureCount kadar deneyecek her sürekli etkinleştirme hatası. Yeniden deneme aralığı her deneyin sürekli etkinleştirme hatası, bir ürün ve geri alma etkinleştirme aralığı ' dir. |
|EnableRestartManagement |Bool, varsayılan false |Dinamik|Bu sunucunun yeniden başlatılmasını etkinleştirmek için yapılır. |
|EnableServiceFabricAutomaticUpdates |Bool, varsayılan false |Dinamik|Bu yapı otomatik güncelleştirme Windows Update aracılığıyla etkinleştirmek için yapılır. |
|EnableServiceFabricBaseUpgrade |Bool, varsayılan false |Dinamik|Bu sunucu için temel güncelleştirmesi sağlamaktır. |
|StartTimeout |Zaman içinde varsayılan 300 saniyedir |Dinamik|TimeSpan saniye cinsinden belirtin. Fabricactivationmanager başlangıç zaman aşımına uğradı. |
|StopTimeout |Zaman içinde varsayılan 300 saniyedir |Dinamik|TimeSpan saniye cinsinden belirtin. Barındırılan hizmet etkinleştirme için zaman aşımı; devre dışı bırakma ve yükseltme. |

## <a name="fabricnode"></a>FabricNode
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|ClientAuthX509FindType |Varsayılan "FindByThumbprint" dizesidir |Dinamik|ClientAuthX509StoreName desteklenen değeri tarafından belirtilen deposundaki sertifikayı aramak nasıl gösterir: FindByThumbprint; FindBySubjectName. |
|ClientAuthX509FindValue |dize, varsayılan değer "" | Dinamik|Arama filtresi değeri varsayılan yönetici rolünde FabricClient sertifikası bulmak için kullanılır. |
|ClientAuthX509FindValueSecondary |dize, varsayılan değer "" |Dinamik|Arama filtresi değeri varsayılan yönetici rolünde FabricClient sertifikası bulmak için kullanılır. |
|ClientAuthX509StoreName |dize, varsayılan değer "Benim" |Dinamik|Varsayılan Yönetici rolü FabricClient sertifikasını içeren X.509 Sertifika deposu adı. |
|ClusterX509FindType |Varsayılan "FindByThumbprint" dizesidir |Dinamik|Küme sertifika ClusterX509StoreName desteklenen değerler tarafından belirtilen deposunda aramak nasıl belirtir: "FindByThumbprint"; "FindBySubjectName" ile "FindBySubjectName"; birden çok eşleşme; olduğunda en son kullanma tarihi olan bir kullanılır. |
|ClusterX509FindValue |dize, varsayılan değer "" |Dinamik|Küme sertifikasını bulmak için kullanılan arama filtresi değeri. |
|ClusterX509FindValueSecondary |dize, varsayılan değer "" |Dinamik|Küme sertifikasını bulmak için kullanılan arama filtresi değeri. |
|ClusterX509StoreName |dize, varsayılan değer "Benim" |Dinamik|Küme içi iletişimi korumak için küme sertifikasını içeren X.509 Sertifika deposu adı. |
|EndApplicationPortRange |Int, varsayılan 0'dır |Statik|Alt sistemi barındırma tarafından yönetilen uygulama noktalarının son (Hayır dahil). EndpointFilteringEnabled içinde barındırma true ise gereklidir. |
|ServerAuthX509FindType |Varsayılan "FindByThumbprint" dizesidir |Dinamik|Sunucu sertifikası ServerAuthX509StoreName desteklenen değeri tarafından belirtilen deposunda aramak nasıl gösterir: FindByThumbprint; FindBySubjectName. |
|ServerAuthX509FindValue |dize, varsayılan değer "" |Dinamik|Arama filtresi değeri sunucu sertifikasını bulmak için kullanılır. |
|ServerAuthX509FindValueSecondary |dize, varsayılan değer "" |Dinamik|Arama filtresi değeri sunucu sertifikasını bulmak için kullanılır. |
|ServerAuthX509StoreName |dize, varsayılan değer "Benim" |Dinamik|Başlangıç hizmeti için sunucu sertifikası içeriyor X.509 Sertifika deposu adı. |
|StartApplicationPortRange |Int, varsayılan 0'dır |Statik|Alt sistemi barındırma tarafından yönetilen uygulama bağlantı noktalarını başlangıcı. EndpointFilteringEnabled içinde barındırma true ise gereklidir. |
|StateTraceInterval |Zaman içinde varsayılan 300 saniyedir |Statik|TimeSpan saniye cinsinden belirtin. İzleme düğümü durumu her düğümde ve FM/FMM düğümlerinde yedeklemek için aralığı. |
|UserRoleClientX509FindType |Varsayılan "FindByThumbprint" dizesidir |Dinamik|UserRoleClientX509StoreName desteklenen değeri tarafından belirtilen deposundaki sertifikayı aramak nasıl gösterir: FindByThumbprint; FindBySubjectName. |
|UserRoleClientX509FindValue |dize, varsayılan değer "" |Dinamik|Arama filtresi değeri varsayılan kullanıcı rolünde FabricClient sertifikası bulmak için kullanılır. |
|UserRoleClientX509FindValueSecondary |dize, varsayılan değer "" |Dinamik|Arama filtresi değeri varsayılan kullanıcı rolünde FabricClient sertifikası bulmak için kullanılır. |
|UserRoleClientX509StoreName |dize, varsayılan değer "Benim" |Dinamik|Varsayılan kullanıcı rolü FabricClient sertifikasını içeren X.509 Sertifika deposu adı. |

## <a name="failovermanager"></a>FailoverManager
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|BuildReplicaTimeLimit|TimeSpan, Common::TimeSpan::FromSeconds(3600) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. Durum bilgisi olan bir çoğaltma oluşturmaya yönelik zaman sınırı; Daha sonra bir uyarı sistem durumu raporu başlatılmayacak |
|ClusterPauseThreshold|Int, varsayılan 1.|Dinamik|Sistem düğümlerin sayısı bu değeri sonra yerleştirme giderseniz; Yük Dengeleme; ve yük devretme durdurulur. |
|CreateInstanceTimeLimit|TimeSpan, Common::TimeSpan::FromSeconds(300) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. Durum bilgisiz örneği oluşturmak için zaman sınırı; Daha sonra bir uyarı sistem durumu raporu başlatılmayacak |
|ExpectedClusterSize|Int, varsayılan 1.|Dinamik|Kümenin ilk kez başlatıldığında; FM bu diğer hizmetler yerleştirme başlamadan önce Yukarı kendilerini bildirmek için çok sayıda düğüm bekleyin; adlandırma gibi sistem hizmetleri dahil olmak üzere. Bu değer artırıldığında başlamak için bir küme süresinin artmasına neden olur; Aşırı yüklenmiş haline gelen erken düğümleri ve ayrıca daha fazla düğüm çevrimiçine gibi gerekli ek taşır, ancak önler. Bu değer genellikle ilk küme boyutu için bazı küçük kesir ayarlamanız gerekir. |
|ExpectedNodeDeactivationDuration|TimeSpan, Common::TimeSpan::FromSeconds(60.0 * 30) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. Beklenen süre içinde devre dışı bırakma işlemini tamamlamak bir düğüm için budur. |
|ExpectedNodeFabricUpgradeDuration|TimeSpan, Common::TimeSpan::FromSeconds(60.0 * 30) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. Yükseltilecek bir düğüm için beklenen süre budur Windows Fabric yükseltmesi sırasında. |
|ExpectedReplicaUpgradeDuration|TimeSpan, Common::TimeSpan::FromSeconds(60.0 * 30) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. Uygulama yükseltme sırasında bir düğümde yükseltilecek tüm çoğaltmalar için beklenen süre budur. |
|IsSingletonReplicaMoveAllowedDuringUpgrade|bool, varsayılan true'dur.|Dinamik|İse true; ayarlanmış yükseltme sırasında taşımak için bir hedef çoğaltma kümesi boyutu 1 ile çoğaltmalar izin verilir. |
|MinReplicaSetSize|Int, varsayılan 3.|İzin Verilmiyor|FM için en küçük çoğaltma kümesi boyutu budur. Etkin FM çoğaltmaların sayısı bu değerin altına düşerse; FM değişiklikleri en az kadar kümeye reddeder çoğaltmaların min sayısı kurtarıldı |
|PlacementConstraints|Varsayılan L dizesidir""|İzin Verilmiyor|Yük Devretme Yöneticisi çoğaltmalar için yerleştirme kısıtlamalar |
|PlacementTimeLimit|TimeSpan, Common::TimeSpan::FromSeconds(600) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. Hedef çoğaltma sayısı ulaşmak için süre sınırı; Daha sonra bir uyarı sistem durumu raporu başlatılmayacak |
|QuorumLossWaitDuration |Zamanı saniye cinsinden MaxValue varsayılandır |Dinamik|TimeSpan saniye cinsinden belirtin. Çekirdek kayıp durumda olması bir bölüm izin veriyoruz en fazla süre budur. Bölüm bu süreden sonra hala çekirdek kaybında ise; Bölüm çekirdek kaybına karşı aşağı çoğaltmaları kayıp olarak dikkate alarak kurtarılır. Bu olası veri kaybına neden olduğunu unutmayın. |
|ReconfigurationTimeLimit|TimeSpan, Common::TimeSpan::FromSeconds(300) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. Yeniden yapılandırma için süre sınırı; Daha sonra bir uyarı sistem durumu raporu başlatılmayacak |
|ReplicaRestartWaitDuration|TimeSpan, Common::TimeSpan::FromSeconds(60.0 * 30) varsayılandır|İzin Verilmiyor|TimeSpan saniye cinsinden belirtin. FMService ReplicaRestartWaitDuration budur |
|StandByReplicaKeepDuration|TimeSpan, Common::TimeSpan::FromSeconds(3600.0 * 24 * 7) varsayılandır|İzin Verilmiyor|TimeSpan saniye cinsinden belirtin. FMService StandByReplicaKeepDuration budur |
|TargetReplicaSetSize|Int, 7 varsayılandır|İzin Verilmiyor|Windows Fabric tutacağı FM çoğaltmaları hedef sayısıdır. Daha yüksek bir sayı FM veri yüksek güvenilirliğini olur; küçük performans kolaylığını ile. |
|UserMaxStandByReplicaCount |Int, varsayılan 1. |Dinamik|Varsayılan en fazla kullanıcı Hizmetleri için sistem tutan StandBy çoğaltmalarının sayısı. |
|UserReplicaRestartWaitDuration |Zamanı saniye cinsinden varsayılan 60,0 * 30'dur |Dinamik|TimeSpan saniye cinsinden belirtin. Kalıcı çoğaltma gittiğinde; Windows Fabric çoğaltma (Bu durum bir kopyasını gerektirir) yeni değiştirme çoğaltmaları oluşturmadan önce geri gelmesi için bu süre bekler. |
|UserStandByReplicaKeepDuration |Zamanı saniye cinsinden varsayılan 3600.0 * 24 * 7'dir |Dinamik|TimeSpan saniye cinsinden belirtin. Kalıcı çoğaltma olduğunuzda geri aşağı durumundan; zaten değiştirilmiş. Bu zamanlayıcı FM bekleme çoğaltma atılmadan önce ne kadar tutacağını belirler. |

## <a name="faultanalysisservice"></a>FaultAnalysisService
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|CompletedActionKeepDurationInSeconds | Int, 604800 varsayılandır |Statik| Yaklaşık budur, bir terminal durumda olan eylemleri tutmak için ne kadar süre. Bu aynı zamanda üzerinde StoredActionCleanupIntervalInSeconds bağlıdır; temizleme işi yalnızca bu aralıkta gerçekleştirilen bu yana. 604800 7 gündür. |
|DataLossCheckPollIntervalInSeconds|int, varsayılan 5.|Statik|Veri kaybı gerçekleşecek şekilde beklenirken sistem gerçekleştirir denetimleri arasındaki süre budur. Veri kaybı numarası iç yineleme denetlenecek DataLossCheckWaitDurationInSeconds/bu sayısıdır. |
|DataLossCheckWaitDurationInSeconds|Int, varsayılan 25'tir|Statik|Toplam süreyi; saniye cinsinden; Sistem veri kaybı gerçekleşecek şekilde olmasını bekler. StartPartitionDataLossAsync() API çağrıldığında dahili olarak kullanılır. |
|MinReplicaSetSize |Int, varsayılan 0'dır |Statik|MinReplicaSetSize FaultAnalysisService için. |
|PlacementConstraints | dize, varsayılan değer ""|Statik| FaultAnalysisService için PlacementConstraints. |
|QuorumLossWaitDuration | Zamanı saniye cinsinden MaxValue varsayılandır |Statik|TimeSpan saniye cinsinden belirtin. FaultAnalysisService için QuorumLossWaitDuration. |
|ReplicaDropWaitDurationInSeconds|int, varsayılan 600'dür|Statik|Bu parametre, veri kaybı API çağrıldığında kullanılır. Sistem Kaldır sonra bırakılan bir çoğaltma ne kadar bekleyeceğini denetimleri çoğaltma dahili olarak çağrılır üzerinde. |
|ReplicaRestartWaitDuration |Zamanı saniye cinsinden, varsayılan değer 60 dakikadır|Statik|TimeSpan saniye cinsinden belirtin. FaultAnalysisService için ReplicaRestartWaitDuration. |
|StandByReplicaKeepDuration| Zamanı saniye cinsinden varsayılandır (60*24*7) dakika |Statik|TimeSpan saniye cinsinden belirtin. FaultAnalysisService için StandByReplicaKeepDuration. |
|StoredActionCleanupIntervalInSeconds | Int, 3600 varsayılandır |Statik|Mağaza ne sıklıkta temizlenecek budur. Yalnızca eylemleri bir terminal durumda; ve en az tamamlandı CompletedActionKeepDurationInSeconds önce olacaktır kaldırıldı. |
|StoredChaosEventCleanupIntervalInSeconds | Int, 3600 varsayılandır |Statik|Depo için temizleme ne sıklıkta denetlenmez budur; olay sayısı birden fazla 30000 Temizleme tetiklersiniz olarak. |
|TargetReplicaSetSize |Int, varsayılan 0'dır |Statik|NOT_PLATFORM_UNIX_START FaultAnalysisService TargetReplicaSetSize. |

## <a name="federation"></a>Federasyon
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|Kirasüresi |Zamanı saniye cinsinden varsayılan 30'dur |Dinamik|Bir düğüm ile komşularına arasında bir kira süresi süresi. |
|LeaseDurationAcrossFaultDomain |Zamanı saniye cinsinden varsayılan 30'dur |Dinamik|Hata etki alanlarında bir düğüm ile komşularına arasında bir kira süresi süresi. |

## <a name="filestoreservice"></a>FileStoreService
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|AnonymousAccessEnabled | Bool, varsayılan değer true şeklindedir |Statik|FileStoreService paylaşımlarına anonim erişimi etkinleştir/devre dışı bırak. |
|CommonName1Ntlmx509CommonName|Varsayılan L dizesidir""|Statik| X509'ın ortak adı CommonName1NtlmPasswordSecret üzerinde HMAC NTLM kimlik doğrulaması kullanılırken oluşturmak için kullanılan sertifika |
|CommonName1Ntlmx509StoreLocation|Varsayılan L "LocalMachine" dizesidir|Statik|X509 depo konumunu CommonName1NtlmPasswordSecret üzerinde HMAC NTLM kimlik doğrulaması kullanılırken oluşturmak için kullanılan sertifika |
|CommonName1Ntlmx509StoreName|Varsayılan L "MY" dizesidir| Statik|X509 deposu adını CommonName1NtlmPasswordSecret üzerinde HMAC NTLM kimlik doğrulaması kullanılırken oluşturmak için kullanılan sertifika |
|CommonName2Ntlmx509CommonName|Varsayılan L dizesidir""|Statik|X509'ın ortak adı CommonName2NtlmPasswordSecret üzerinde HMAC NTLM kimlik doğrulaması kullanılırken oluşturmak için kullanılan sertifika |
|CommonName2Ntlmx509StoreLocation|Varsayılan L "LocalMachine" dizesidir| Statik|X509 depo konumunu CommonName2NtlmPasswordSecret üzerinde HMAC NTLM kimlik doğrulaması kullanılırken oluşturmak için kullanılan sertifika |
|CommonName2Ntlmx509StoreName|Varsayılan L "MY" dizesidir|Statik| X509 deposu adını CommonName2NtlmPasswordSecret üzerinde HMAC NTLM kimlik doğrulaması kullanılırken oluşturmak için kullanılan sertifika |
|CommonNameNtlmPasswordSecret|SecureString, Common::SecureString(L"") varsayılandır| Statik|Oluşturulan aynı parola çekirdek olarak NTLM kimlik doğrulaması kullanılırken kullanılan parola gizli anahtar |
|GenerateV1CommonNameAccount| bool, varsayılan true'dur.|Statik|Kullanıcı adı V1 üretme algoritması olan bir hesap oluşturulup oluşturulmayacağını belirtir. Service Fabric sürüm 6.1 başlatılıyor; v2 oluşturma ile bir hesabı her zaman oluşturulur. V1 hesap başlangıç/bitiş V2 oluşturma (önce 6.1) desteklemeyen sürümleri yükseltmeleri için gereklidir.|
|MaxCopyOperationThreads | Uint, varsayılan 0'dır |Dinamik| Bu ikincil paralel dosyaları sayısı, birincil sunucudan kopyalayabilirsiniz. '0' çekirdek sayısı ==. |
|MaxFileOperationThreads | Uint, varsayılan 100'dür |Statik| Paralel iş parçacığı birincil FileOperations (Copy/Move) gerçekleştirmek için izin verilen maksimum sayısı. '0' çekirdek sayısı ==. |
|MaxRequestProcessingThreads | Uint, 200 varsayılandır |Statik|Paralel iş parçacığı birincil isteklerini işlemek için izin verilen maksimum sayısı. '0' çekirdek sayısı ==. |
|MaxSecondaryFileCopyFailureThreshold | Uint, varsayılan 25'tir|Dinamik|Vazgeçmeden önce ikincil dosya kopyalama yeniden deneme sayısı. |
|MaxStoreOperations | Uint, varsayılan 4096'dır |Statik|Paralel deposu işlem işlemlerinin birincil üzerinde izin verilen maksimum sayısı. '0' çekirdek sayısı ==. |
|NamingOperationTimeout |Zaman içinde varsayılan 60 saniyedir |Dinamik|TimeSpan saniye cinsinden belirtin. Adlandırma işlemi gerçekleştirmek için zaman aşımı. |
|PrimaryAccountNTLMPasswordSecret | SecureString, varsayılan boştur |Statik| NTLM kimlik doğrulaması kullanılırken oluşturulan aynı parola için çekirdek olarak kullanılan parola gizli anahtar. |
|PrimaryAccountNTLMX509StoreLocation | Varsayılan "LocalMachine" dizesidir|Statik| X509 depo konumunu PrimaryAccountNTLMPasswordSecret üzerinde HMAC NTLM kimlik doğrulaması kullanılırken oluşturmak için kullanılan sertifika. |
|PrimaryAccountNTLMX509StoreName | Varsayılan "MY" dizesidir|Statik| X509 deposu adını PrimaryAccountNTLMPasswordSecret üzerinde HMAC NTLM kimlik doğrulaması kullanılırken oluşturmak için kullanılan sertifika. |
|PrimaryAccountNTLMX509Thumbprint | dize, varsayılan değer ""|Statik|Parmak izi X509 PrimaryAccountNTLMPasswordSecret üzerinde HMAC NTLM kimlik doğrulaması kullanılırken oluşturmak için kullanılan sertifika. |
|PrimaryAccountType | dize, varsayılan değer "" |Statik|Birincil ACL FileStoreService için asıl AccountType paylaşır. |
|PrimaryAccountUserName | dize, varsayılan değer "" |Statik|Birincil hesap kullanıcı adı için ACL asıl FileStoreService paylaşır. |
|PrimaryAccountUserPassword | SecureString, varsayılan boştur |Statik|ACL asıl birincil hesap parolasını FileStoreService paylaşır. |
|QueryOperationTimeout | Zaman içinde varsayılan 60 saniyedir |Dinamik|TimeSpan saniye cinsinden belirtin. Sorgu işlemi gerçekleştirmek için zaman aşımı. |
|SecondaryAccountNTLMPasswordSecret | SecureString, varsayılan boştur |Statik| NTLM kimlik doğrulaması kullanılırken oluşturulan aynı parola için çekirdek olarak kullanılan parola gizli anahtar. |
|SecondaryAccountNTLMX509StoreLocation | Varsayılan "LocalMachine" dizesidir |Statik|X509 depo konumunu SecondaryAccountNTLMPasswordSecret üzerinde HMAC NTLM kimlik doğrulaması kullanılırken oluşturmak için kullanılan sertifika. |
|SecondaryAccountNTLMX509StoreName | Varsayılan "MY" dizesidir |Statik|X509 deposu adını SecondaryAccountNTLMPasswordSecret üzerinde HMAC NTLM kimlik doğrulaması kullanılırken oluşturmak için kullanılan sertifika. |
|SecondaryAccountNTLMX509Thumbprint | dize, varsayılan değer ""| Statik|Parmak izi X509 SecondaryAccountNTLMPasswordSecret üzerinde HMAC NTLM kimlik doğrulaması kullanılırken oluşturmak için kullanılan sertifika. |
|SecondaryAccountType | dize, varsayılan değer ""|Statik| İkincil asıl ACL FileStoreService AccountType paylaşır. |
|SecondaryAccountUserName | dize, varsayılan değer ""| Statik|İkincil hesap kullanıcı adı için ACL asıl FileStoreService paylaşır. |
|SecondaryAccountUserPassword | SecureString, varsayılan boştur |Statik|ACL asıl ikincil hesap parolasını FileStoreService paylaşır. |

## <a name="healthmanager"></a>HealthManager
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
| EnableApplicationTypeHealthEvaluation |Bool, varsayılan false |Statik|Küme sistem durumu değerlendirme İlkesi: uygulama türünü sistem durumu değerlendirmesi etkinleştirin. |

## <a name="healthmanagerclusterhealthpolicy"></a>HealthManager/ClusterHealthPolicy
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|ConsiderWarningAsError |Bool, varsayılan false |Statik|Küme sistem durumu değerlendirme İlkesi: uyarıları hata olarak kabul edilir. |
|MaxPercentUnhealthyApplications | Int, varsayılan 0'dır |Statik|Küme sistem durumu değerlendirme İlkesi: en yüksek yüzde sağlıksız uygulamaların izin sağlam olması küme için. |
|MaxPercentUnhealthyNodes | Int, varsayılan 0'dır |Statik|Küme sistem durumu değerlendirme İlkesi: en yüksek yüzde sağlıksız düğümleri izin sağlam olması küme için. |

## <a name="healthmanagerclusterupgradehealthpolicy"></a>HealthManager/ClusterUpgradeHealthPolicy
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|MaxPercentDeltaUnhealthyNodes|Int, varsayılan 10'dur|Statik|Küme yükseltme sistem durumu değerlendirme İlkesi: delta sağlıksız düğümleri maksimum yüzdesi kümenin sağlıklı olması için izin verilen |
|MaxPercentUpgradeDomainDeltaUnhealthyNodes|Int, varsayılan değer 15 dakikadır.|Statik|Küme yükseltme sistem durumu değerlendirme İlkesi: en fazla bir yükseltme etki alanındaki sağlıksız düğümleri delta yüzdesi izin sağlam olması küme için |

## <a name="hosting"></a>Barındırma
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|ActivationMaxFailureCount |Tam sayı olarak varsayılan 10'dur |Dinamik|Sistem yeniden deneme sayısı vazgeçmeden önce etkinleştirme başarısız oldu |
|ActivationMaxRetryInterval |Zaman içinde varsayılan 300 saniyedir |Dinamik|Her sürekli etkinleştirme hatasında etkinleştirme için ActivationMaxFailureCount kadar sistem yeniden dener. ActivationMaxRetryInterval her etkinleştirme hatası sonrasında yeniden deneme bekleme zaman aralığını belirtir |
|ActivationRetryBackoffInterval |Zamanı saniye cinsinden varsayılan 5'tir |Dinamik|Her etkinleştirme hatasında geri Çekilme aralığı; Her sürekli etkinleştirme hatasında etkinleştirme için MaxActivationFailureCount kadar sistem yeniden dener. Yeniden deneme aralığı her deneyin sürekli etkinleştirme hatası, bir ürün ve geri alma etkinleştirme aralığı ' dir. |
|ActivationTimeout| TimeSpan, Common::TimeSpan::FromSeconds(180) varsayılandır|Dinamik| TimeSpan saniye cinsinden belirtin. Uygulama etkinleştirme için zaman aşımı; devre dışı bırakma ve yükseltme. |
|ApplicationHostCloseTimeout| TimeSpan, Common::TimeSpan::FromSeconds(120) varsayılandır|Dinamik| TimeSpan saniye cinsinden belirtin. Doku çıkış kendini etkinleştirilmiş işlemlerde algılandığında; FabricRuntime tüm kullanıcının ana bilgisayar (applicationhost) işlemi yinelemede kapatır. Kapatma işlemi için zaman aşımı budur. |
|ApplicationUpgradeTimeout| TimeSpan, Common::TimeSpan::FromSeconds(360) varsayılandır|Dinamik| TimeSpan saniye cinsinden belirtin. Uygulama yükseltme için zaman aşımı. Zaman aşımı "ActivationTimeout" dağıtıcı başarısız olur daha az ise. |
|CreateFabricRuntimeTimeout|TimeSpan, Common::TimeSpan::FromSeconds(120) varsayılandır|Dinamik| TimeSpan saniye cinsinden belirtin. FabricCreateRuntime eşitleme için zaman aşımı değeri çağırın |
|DeploymentMaxFailureCount|Int, varsayılan 20'dir| Dinamik|Uygulama dağıtımı, düğümde bu uygulama dağıtımının başarısız önce DeploymentMaxFailureCount zamanları denenecek.| 
|DeploymentMaxRetryInterval| TimeSpan, Common::TimeSpan::FromSeconds(3600) varsayılandır|Dinamik| TimeSpan saniye cinsinden belirtin. Dağıtım için en fazla yeniden deneme aralığı. Her sürekli hatada yeniden deneme aralığını (DeploymentMaxRetryInterval; Min hesaplanır Sürekli hata sayısı * DeploymentRetryBackoffInterval) |
|DeploymentRetryBackoffInterval| TimeSpan, Common::TimeSpan::FromSeconds(10) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. Geri alma dağıtım hatası aralığı. Her sürekli dağıtım hatası sistem MaxDeploymentFailureCount kadar dağıtım için yeniden deneyecek. Yeniden deneme aralığını sürekli dağıtım hatası, bir ürün ve dağıtım geri Çekilme aralığı ' dir. |
|EnableActivateNoWindow| bool, varsayılan FALSE olur|Dinamik| Etkinleştirilen işlemi arka planda herhangi bir konsol olmadan oluşturulur. |
|EnableDockerHealthCheckIntegration|bool, varsayılan true'dur.|Statik|Docker durum denetimi olayları Service Fabric sistem durumu raporu ile entegrasyonunu sağlar |
|EnableProcessDebugging|bool, varsayılan FALSE olur|Dinamik| Hata ayıklayıcı altında uygulama konakları başlatma etkinleştirir |
|EndpointProviderEnabled| bool, varsayılan FALSE olur|Statik| Uç nokta kaynaklar doku tarafından yönetilmesini sağlar. FabricNode başlangıç ve bitiş uygulama bağlantı noktası aralığının belirtilmesini gerektiriyor. |
|FabricContainerAppsEnabled| bool, varsayılan FALSE olur|Statik| |
|FirewallPolicyEnabled|bool, varsayılan FALSE olur|Statik| Uç nokta kaynaklar için güvenlik duvarı bağlantı noktalarının açık bağlantı noktalarını ServiceManifest içinde belirtilen açılarak etkinleştirir |
|GetCodePackageActivationContextTimeout|TimeSpan, Common::TimeSpan::FromSeconds(120) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. CodePackageActivationContext çağrıları için zaman aşımı değeri. Bu geçici hizmetler için geçerli değildir. |
|IPProviderEnabled|bool, varsayılan FALSE olur|Statik|IP adresi yönetimi sağlar. |
|NTLMAuthenticationEnabled|bool, varsayılan FALSE olur|Statik| NTLM göre çalışan diğer kullanıcılar ve böylece süreçler makineler arasında güvenli iletişim kurabilmek kod paketi kullanmak için desteği etkinleştirir. |
|NTLMAuthenticationPasswordSecret|SecureString, Common::SecureString(L"") varsayılandır|Statik|NTLM kullanıcılar için parola oluşturmak için kullanılan bir şifrelenmiş sahip olur. NTLMAuthenticationEnabled true ise ayarlanması gerekir. Dağıtıcı tarafından doğrulandı. |
|NTLMSecurityUsersByX509CommonNamesRefreshInterval|TimeSpan, Common::TimeSpan::FromMinutes(3) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. Ortama özgü ayarlar FileStoreService NTLM yapılandırması için kullanılacak yeni sertifikalar için hangi barındırma düzenli aralıklarla tarar. |
|NTLMSecurityUsersByX509CommonNamesRefreshTimeout|TimeSpan, Common::TimeSpan::FromMinutes(4) varsayılandır|Dinamik| TimeSpan saniye cinsinden belirtin. Sertifika ortak adları kullanarak NTLM kullanıcıları yapılandırmak için zaman aşımı. NTLM kullanıcılar FileStoreService paylaşımlar için gereklidir. |
|RegisterCodePackageHostTimeout|TimeSpan, Common::TimeSpan::FromSeconds(120) varsayılandır|Dinamik| TimeSpan saniye cinsinden belirtin. FabricRegisterCodePackageHost eşitleme çağrısı için zaman aşımı değeri. Bu yalnızca çoklu kod paketi uygulama ana FWP gibi için geçerlidir |
|RequestTimeout|TimeSpan, Common::TimeSpan::FromSeconds(30) varsayılandır|Dinamik| TimeSpan saniye cinsinden belirtin. Bu kullanıcının uygulama ana bilgisayarı ve Fabrika kayıt gibi çeşitli barındırma ilgili işlemleri için doku işlemi arasındaki iletişim için zaman aşımını gösterir; çalışma zamanı kaydı. |
|RunAsPolicyEnabled| bool, varsayılan FALSE olur|Statik| Kullanıcı dışındaki yerel kullanıcı kodu paketlerini çalıştırma etkinleştirir hangi yapıda işlemi çalışmıyor. Bu ilkeyi etkinleştirmek için doku sistem veya SeAssignPrimaryTokenPrivilege sahip kullanıcı olarak çalışmalıdır. |
|ServiceFactoryRegistrationTimeout| TimeSpan, Common::TimeSpan::FromSeconds(120) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. Eşitleme kaydı (Stateless/Stateful) ServiceFactory çağrısı için zaman aşımı değeri |
|ServiceTypeDisableFailureThreshold |Tam sayı olarak varsayılan 1. |Dinamik|Daha sonra bu düğümde hizmet türü devre dışı bırakabilir ve yerleştirme için farklı bir düğüme deneyin FailoverManager (FM) bildirilir hata sayısı için eşik budur. |
|ServiceTypeDisableGraceInterval|TimeSpan, Common::TimeSpan::FromSeconds(30) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. Daha sonra hizmet türü devre dışı bırakılabilir zaman aralığı |
|ServiceTypeRegistrationTimeout |Zaman içinde varsayılan 300 saniyedir |Dinamik|ServiceType doku ile kayıtlı olması için izin verilen en uzun süre |

## <a name="httpgateway"></a>HttpGateway
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|ActiveListeners |Uint, varsayılan 50'dir |Statik| Http sunucusu sıraya göndermek için okuma sayısı. Bu HttpGateway tarafından karşılanan eşzamanlı istek sayısını denetler. |
|HttpGatewayHealthReportSendInterval |Zamanı saniye cinsinden varsayılan 30'dur |Statik|TimeSpan saniye cinsinden belirtin. Hangi Http ağ geçidi birikmiş sistem durumu gönderir aralığı sistem yöneticisine bildirir. |
|IsEnabled|Bool, varsayılan false |Statik| HttpGateway etkinleştirir/devre dışı bırakır. HttpGateway varsayılan olarak devre dışıdır. |
|MaxEntityBodySize |Uint 4194304 varsayılandır |Dinamik|Http isteğinden beklenebilir gövde en büyük boyutunu sağlar. Varsayılan değer 4 MB'tır. Httpgateway boyutu gövdesi varsa istek başarısız olur > Bu değer. En az okuma öbek boyutu 4096 bayt'tır. Bu olması gerekir böylece > 4096 =. |

## <a name="imagestoreclient"></a>ImageStoreClient
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|ClientCopyTimeout | Zamanı saniye cinsinden 1800 varsayılandır |Dinamik| TimeSpan saniye cinsinden belirtin. Görüntü deposu hizmetine en üst düzey kopya isteği zaman aşımı değeri. |
|ClientDefaultTimeout | Zamanı saniye cinsinden 180 varsayılandır |Dinamik| TimeSpan saniye cinsinden belirtin. Tüm olmayan-karşıya yükleme/olmayan-indirme isteği zaman aşımı değerini (örneğin var; Sil) görüntü depolama hizmeti için. |
|ClientDownloadTimeout | Zamanı saniye cinsinden 1800 varsayılandır |Dinamik| TimeSpan saniye cinsinden belirtin. Görüntü deposu hizmetine en üst düzey indirme isteği zaman aşımı değeri. |
|ClientListTimeout | Zamanı saniye cinsinden 600 varsayılandır |Dinamik|TimeSpan saniye cinsinden belirtin. Görüntü deposu hizmetine üst düzey liste isteği zaman aşımı değeri. |
|ClientUploadTimeout |Zamanı saniye cinsinden 1800 varsayılandır |Dinamik|TimeSpan saniye cinsinden belirtin. Görüntü deposu hizmetine en üst düzey karşıya yükleme isteği zaman aşımı değeri. |

## <a name="imagestoreservice"></a>ImageStoreService
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|Etkin |Bool, varsayılan false |Statik|ImageStoreService için etkin bayrağı. Varsayılan: false |
|MinReplicaSetSize | Int, varsayılan 3. |Statik|MinReplicaSetSize ImageStoreService için. |
|PlacementConstraints | dize, varsayılan değer "" |Statik| ImageStoreService için PlacementConstraints. |
|QuorumLossWaitDuration | Zamanı saniye cinsinden MaxValue varsayılandır |Statik| TimeSpan saniye cinsinden belirtin. ImageStoreService için QuorumLossWaitDuration. |
|ReplicaRestartWaitDuration | Zamanı saniye cinsinden varsayılan 60,0 * 30'dur |Statik|TimeSpan saniye cinsinden belirtin. ImageStoreService için ReplicaRestartWaitDuration. |
|StandByReplicaKeepDuration | Zamanı saniye cinsinden varsayılan 3600.0 * 2'dir |Statik| TimeSpan saniye cinsinden belirtin. ImageStoreService için StandByReplicaKeepDuration. |
|TargetReplicaSetSize | Int, 7 varsayılandır |Statik|ImageStoreService için TargetReplicaSetSize. |

## <a name="ktllogger"></a>KtlLogger
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|AutomaticMemoryConfiguration |Int, varsayılan 1. |Dinamik|Bellek ayarları otomatik olarak ve dinamik olarak yapılandırılması gerekip gerekmediğini belirten bayrak. Sıfır sonra bellek yapılandırma ayarları doğrudan kullanılır ve değiştirmeyin sistem koşullara göre. Bir sonra bellek ayarları otomatik olarak yapılandırılır ve değişebilir sistem koşullara göre. |
|MaximumDestagingWriteOutstandingInKB | Int, varsayılan 0'dır |Dinamik|Ayrılmış bir günlük öncesinde ilerletmek paylaşılan günlük izin vermek için KB sayısı. 0 sınır belirtmek için kullanın.
|SharedLogId |dize, varsayılan değer "" |Statik|Paylaşılan günlük kapsayıcısı için benzersiz GUID. Kullan "" fabric veri kökü altındaki varsayılan yolu kullanıyorsanız. |
|SharedLogPath |dize, varsayılan değer "" |Statik|Paylaşılan günlük kapsayıcı yerleştirileceği konum için yolu ve dosya adı. Kullan "" fabric veri kökü altındaki varsayılan yolu kullanma. |
|SharedLogSizeInMB |Int, 8192 varsayılandır |Statik|Paylaşılan günlük kapsayıcısında ayrılacak MB sayısı. |
|WriteBufferMemoryPoolMaximumInKB | Int, varsayılan 0'dır |Dinamik|En fazla büyümeye yazma arabelleği bellek havuzunda izin vermek için KB sayısı. 0 sınır belirtmek için kullanın. |
|WriteBufferMemoryPoolMinimumInKB |Int, 8388608 varsayılandır |Dinamik|Başlangıçta yazma arabelleği bellek havuzu için ayrılacak KB sayısı. Varsayılan sınır göstermek için 0 kullanın SharedLogSizeInMB aşağıdaki ile tutarlı olmalıdır. |

## <a name="management"></a>Yönetim
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|AzureStorageMaxConnections | Int, varsayılan 5000'dir |Dinamik|Azure depolama alanına eşzamanlı bağlantı sayısı. |
|AzureStorageMaxWorkerThreads | Int, varsayılan 25'tir |Dinamik|Paralel çalışan iş parçacığı sayısı. |
|AzureStorageOperationTimeout | Zamanı saniye cinsinden 6000 varsayılandır |Dinamik|TimeSpan saniye cinsinden belirtin. Xstore işlemin tamamlanmasını zaman aşımına uğradı. |
|DisableChecksumValidation | Bool, varsayılan false |Statik| Bu yapılandırma etkinleştirmek veya devre dışı uygulama sağlama sırasında sağlama toplamı doğrulaması olanak tanır. |
|DisableServerSideCopy | Bool, varsayılan false |Statik|Bu yapılandırma etkinleştirir veya sunucu tarafı kopyası görüntü uygulama paketinin uygulama sağlama sırasında devre dışı bırakır. |
|ImageCachingEnabled | Bool, varsayılan değer true şeklindedir |Statik|Bu yapılandırma etkinleştirmek veya önbelleğe almayı devre dışı olanak tanır. |
|ImageStoreConnectionString |SecureString |Statik|Görüntü için kök bağlantı dizesi. |
|ImageStoreMinimumTransferBPS | Int, varsayılan 1024'tür. |Dinamik|Görüntü ve küme arasındaki en düşük aktarım hızı. Bu değer dış görüntü erişirken zaman aşımını belirlemek için kullanılır. Yalnızca görüntü ve küme arasındaki gecikme süresi dış görüntü indirmek küme için daha fazla süre tanıyın yüksek olduğunda bu değeri değiştirin. |

## <a name="metricactivitythresholds"></a>MetricActivityThresholds
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|KeyIntegerValueMap, varsayılan, Yok'tur|Dinamik|MetricActivityThresholds kümedeki ölçümünün belirler. MaxNodeLoad MetricActivityThresholds büyükse, Dengeleme çalışır. Birleştirme ölçümleri veya hangi Service Fabric düğümü boş değerlendirir altında yük eşit miktarda tanımlar |

## <a name="metricbalancingthresholds"></a>MetricBalancingThresholds
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, varsayılan, Yok'tur|Dinamik|MetricBalancingThresholds kümedeki ölçümünün belirler. MaxNodeLoad/minNodeLoad MetricBalancingThresholds büyükse, Dengeleme çalışır. MaxNodeLoad/minNodeLoad en az bir FD veya UD MetricBalancingThresholds küçükse, birleştirme çalışır. |

## <a name="namingservice"></a>NamingService
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|GatewayServiceDescriptionCacheLimit |Int, varsayılan 0'dır |Statik|Giriş sayısı üst sınırı ağ geçidinde adlandırma (sınırsız için 0 olarak ayarlayın) LRU hizmet açıklaması önbellekte saklanır. |
|MaxClientConnections |Int, varsayılan 1000'dir |Dinamik|Ağ geçidi başına istemci bağlantılarının sayısı izin verilen en fazla. |
|MaxFileOperationTimeout |Zamanı saniye cinsinden varsayılan 30'dur |Dinamik|TimeSpan saniye cinsinden belirtin. Dosya deposu hizmet işlemi için izin verilen en uzun zaman aşımı. Daha büyük bir zaman aşımı belirtme istekleri kabul edilmeyecek. |
|MaxIndexedEmptyPartitions |Int, varsayılan 1000'dir |Dinamik|Yeniden bağlanan istemciler eşitlemek için bildirim önbelleğinde, dizine alınan kalacak boş bölümler maksimum sayısı. Bu sayı yukarıda boş bölümler, arama sürüm artan dizinden kaldırılır. İstemcileri yeniden bağlanmayı hala eşitlemek ve kaçırılan boş bölüm güncelleştirmeleri almak; ancak eşitleme protokolü daha pahalı hale gelir. |
|MaxMessageSize |Int, varsayılan değer 4\*1024\*1024 |Statik|Adlandırma kullanırken istemci düğümü iletişimi için maksimum ileti boyutu. DOS saldırısı alleviation; Varsayılan değer 4 MB'tır. |
|MaxNamingServiceHealthReports | Int, varsayılan 10'dur |Dinamik|Adlandırma depolamak yavaş işlemlerinin maksimum sayısı raporları sağlıksız bir kerede hizmet. 0; Tüm yavaş işlemleri gönderilir. |
|MaxOperationTimeout |Zamanı saniye cinsinden 600 varsayılandır |Dinamik|TimeSpan saniye cinsinden belirtin. İstemci işlemleri için izin verilen en uzun zaman aşımı. Daha büyük bir zaman aşımı belirtme istekleri kabul edilmeyecek. |
|MaxOutstandingNotificationsPerClient |Int, varsayılan 1000'dir |Dinamik|Bir istemci kaydı yapmadan önce bekleyen bildirimleri sayısı ağ geçidi tarafından zorla kapatıldı. |
|MinReplicaSetSize | Int, varsayılan 3. |İzin Verilmiyor| Adlandırma Hizmeti çoğaltmaları güncelleştirmesini tamamlamak içine yazmak için gerekli minimum sayısı. Bu daha az çoğaltmaları varsa çoğaltmaları geri yüklenene kadar sisteminde active güvenilirlik sistem güncelleştirmeleri adlandırma hizmeti deposuna reddeder. Bu değer hiçbir zaman birden çok TargetReplicaSetSize olmalıdır. |
|bölüm sayısı |Int, varsayılan 3. |İzin Verilmiyor|Oluşturulacak bölüm adlandırma hizmetinin depolar. Her bölüm için dizinini karşılık gelen bir tek bölüm anahtarı sahibi; Bu bölüm anahtarlarını [0; Bölüm sayısı) yok. Adlandırma Hizmeti, herhangi bir yedekleme çoğaltma tarafından tutulan verileri ortalama miktarını azaltarak gerçekleştirebilirsiniz ölçek adlandırma hizmeti bölümleri artar sayısını artırmayı ayarlayın; kaynakların artan kullanımı maliyetle (PartitionCount itibaren * ReplicaSetSize hizmet çoğaltmaları saklanabilir).|
|PlacementConstraints | dize, varsayılan değer "" |İzin Verilmiyor| Adlandırma Hizmeti yerleştirme kısıtlaması. |
|QuorumLossWaitDuration | Zamanı saniye cinsinden MaxValue varsayılandır |İzin Verilmiyor| TimeSpan saniye cinsinden belirtin. Ne zaman bir adlandırma hizmeti çekirdek kayıp alır; Bu süreölçer başlatır. Süresi dolduğunda, FM aşağı çoğaltmaları kayıp olarak kabul eder; ve çekirdek kurtarmayı deneyin. Bu veri kaybına neden olabilir değil. |
|RepairInterval | Zamanı saniye cinsinden varsayılan 5'tir |Statik| TimeSpan saniye cinsinden belirtin. Adlandırma tutarsızlık onarım yetkilisi sahibi ve ad sahibi arasında başlayacağı aralığı. |
|ReplicaRestartWaitDuration | Saat (60,0 * 30) saniye cinsinden varsayılandır|İzin Verilmiyor| TimeSpan saniye cinsinden belirtin. Adlandırma Hizmeti çoğaltma gittiğinde; Bu süreölçer başlatır. Süresi dolduğunda, FM aşağı olan çoğaltmaları değiştirmek başlar (Bu henüz bunları kayıp dikkate almaz). |
|ServiceDescriptionCacheLimit | Int, varsayılan 0'dır |Statik| Giriş sayısı üst sınırı adlandırma deposu (sınırsız için 0 olarak ayarlayın) hizmetine LRU hizmet açıklaması önbellekte saklanır. |
|ServiceNotificationTimeout |Zamanı saniye cinsinden varsayılan 30'dur |Dinamik|TimeSpan saniye cinsinden belirtin. İstemciye hizmet bildirimleri teslim edilirken kullanılan zaman aşımı. |
|StandByReplicaKeepDuration | Zamanı saniye cinsinden varsayılan 3600.0 * 2'dir |İzin Verilmiyor| TimeSpan saniye cinsinden belirtin. Ne zaman bir adlandırma hizmeti çoğaltma gelen geri aşağı durumundan; zaten değiştirilmiş. Bu zamanlayıcı FM bekleme çoğaltma atılmadan önce ne kadar tutacağını belirler. |
|TargetReplicaSetSize |Int, 7 varsayılandır |İzin Verilmiyor|Yineleme sayısını adlandırma hizmeti deposunun her bölüm için ayarlar. Çoğaltma kümeleri sayısını artırmayı adlandırma hizmeti deposundaki bilgiler güvenilirliği düzeyini artırır; düğüm hatası sonucunda bilgiler kaybolur değişiklik azalan; Windows Fabric ve süreyi artan iş yükünün maliyetle adlandırma veri güncelleştirmeleri gerçekleştirmek için gereken.|

## <a name="nodebufferpercentage"></a>NodeBufferPercentage
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, varsayılan, Yok'tur|Dinamik|Düğüm kapasitesi yüzdesini ölçüm adı başına; Yük devretme çalışması için bir düğüm üzerinde boş bir yere tutmak için arabellek olarak kullanılır. |

## <a name="nodecapacities"></a>NodeCapacities
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup |NodeCapacityCollectionMap |Statik|Farklı ölçümleri için düğüm kapasiteler koleksiyonu. |

## <a name="nodedomainids"></a>NodeDomainIds
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup |NodeFaultDomainIdCollection |Statik|Bir düğüme ait hata etki alanlarını açıklar. Hata etki alanı düğümü veri merkezindeki konumunu tanımlayan bir URI ile tanımlanır.  Hata etki alanı URI biçimi olan fd: / fd/ardından tarafından URI yol kesimi.|
|UpgradeDomainId |dize, varsayılan değer "" |Statik|Yükseltme etki alanına bir düğüme ait açıklar. |

## <a name="nodeproperties"></a>NodeProperties
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup |NodePropertyCollectionMap |Statik|Düğüm özellikleri anahtar-değer çiftleri koleksiyonu. |

## <a name="paas"></a>Paas
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|ClusterId |dize, varsayılan değer "" |İzin Verilmiyor|X509 sertifika deposunu yapılandırma koruması fabric tarafından kullanılır. |

## <a name="performancecounterlocalstore"></a>PerformanceCounterLocalStore
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|Sayaçları |Dize | Dinamik |Toplanacak performans sayaçlarını virgülle ayrılmış listesi. |
|IsEnabled |Bool, varsayılan değer true şeklindedir | Dinamik |Bayrağı, performans sayacı toplama yerel düğümünde etkinleştirilip etkinleştirilmeyeceğini gösterir. |
|MaxCounterBinaryFileSizeInMB |Int, varsayılan 1. | Dinamik |Her performans sayacı ikili dosya için maksimum boyut (MB cinsinden). |
|NewCounterBinaryFileCreationIntervalInMinutes |Int, varsayılan 10'dur | Dinamik |Daha sonra yeni bir performans sayacı ikili dosyası oluşturulur üst aralığı (saniye cinsinden). |
|SamplingIntervalInSeconds |Int, 60 varsayılandır | Dinamik |Toplanmakta olan performans sayaçları için örnekleme aralığı. |

## <a name="placementandloadbalancing"></a>PlacementAndLoadBalancing
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|AffinityConstraintPriority | Int, varsayılan 0'dır | Dinamik|Benzeşim kısıtlaması önceliğini belirler: 0: sabit; 1: yazılım; Negatif: yoksay. |
|ApplicationCapacityConstraintPriority | Int, varsayılan 0'dır | Dinamik|Kapasite kısıtlaması önceliğini belirler: 0: sabit; 1: yazılım; Negatif: yoksay. |
|AutoDetectAvailableResources|bool, varsayılan true'dur.|Statik|Bu yapılandırma, kullanılabilir kaynakları (CPU ve bellek) düğümünde otomatik algılanmasını tetikleyecek bu yapılandırma ayarlandığında true - biz gerçek kapasiteleri okuyun ve kullanıcı hatalı düğüm kapasiteleri belirtilen ya da bu yapılandırma - false olarak ayarlanmışsa, bunları hiç tanımlamadığınız düzeltmenize şunları yapacağız  hatalı düğüm kapasiteleri belirtilen kullanıcının bir uyarı izleme; ancak biz bunları düzeltmez; Bu kullanıcı olarak belirtilen kapasiteye sahip isteyen anlamı > düğümü gerçekten varolandan veya kapasiteleri tanımsız; Sınırsız kapasite varsayacak |
|BalancingDelayAfterNewNode | Zamanı saniye cinsinden varsayılan 120'dir |Dinamik|TimeSpan saniye cinsinden belirtin. Yeni bir düğüm ekledikten sonra bu süre içinde etkinlikleri Dengeleme başlatmayın. |
|BalancingDelayAfterNodeDown | Zamanı saniye cinsinden varsayılan 120'dir |Dinamik|TimeSpan saniye cinsinden belirtin. Bir düğüm aşağı olayını bu süre içinde etkinlikleri Dengeleme başlatmayın. |
|CapacityConstraintPriority | Int, varsayılan 0'dır | Dinamik|Kapasite kısıtlaması önceliğini belirler: 0: sabit; 1: yazılım; Negatif: yoksay. |
|ConsecutiveDroppedMovementsHealthReportLimit | Int, varsayılan 20'dir | Dinamik|ResourceBalancer verilen hareketleri tanılama yürütülür ve sistem durumu uyarıları yayılan önce bırakılan arda kaç kez tanımlar. Negatif: Hiçbir uyarı yayılan bu koşul altında. |
|ConstraintFixPartialDelayAfterNewNode | Zamanı saniye cinsinden varsayılan 120'dir |Dinamik| TimeSpan saniye cinsinden belirtin. DDo FaultDomain değil düzeltin ve yeni bir düğüm ekledikten sonra bu süre içinde UpgradeDomain kısıtlaması ihlali. |
|ConstraintFixPartialDelayAfterNodeDown | Zamanı saniye cinsinden varsayılan 120'dir |Dinamik| TimeSpan saniye cinsinden belirtin. Bir düğüm aşağı olayını bu süre içinde değil FaultDomain düzeltin ve UpgradeDomain sabiti ihlallerini yapın. |
|ConstraintViolationHealthReportLimit | Int, varsayılan 50'dir |Dinamik| Tanılama yürütülür ve gösterilen durumu raporları önce kalıcı olarak Düzeltilmeyen olacak çoğaltma ihlal kısıtlamasına sahip sayısını tanımlar. |
|DetailedConstraintViolationHealthReportLimit | Int, 200 varsayılandır |Dinamik| Tanılama yürütülür ve sistem durumu raporlarını yayınlaması ayrıntılı önce kalıcı olarak Düzeltilmeyen olacak çoğaltma ihlal kısıtlamasına sahip sayısını tanımlar. |
|DetailedDiagnosticsInfoListLimit | Int, varsayılan değer 15 dakikadır. |Dinamik| Tanılama kesilmesi önce eklenecek kısıtlaması başına (ayrıntılı bilgilerle) tanı girdisi sayısını tanımlar.|
|DetailedNodeListLimit | Int, varsayılan değer 15 dakikadır. |Dinamik| Kesme yerleştirilmemiş çoğaltma raporlarında önce eklenecek kısıtlaması başına en fazla düğüm sayısını tanımlar. |
|DetailedPartitionListLimit | Int, varsayılan değer 15 dakikadır. |Dinamik| Tanılama kesilmesi önce dahil etmek bir kısıtlama için tanı girdisi başına bölüm sayısını tanımlar. |
|DetailedVerboseHealthReportLimit | Int, 200 varsayılandır | Dinamik|Ayrıntılı sistem durumu raporlarını yayınlaması önce kalıcı olarak yerleştirilmemiş olacak bir yerleştirilmemiş çoğaltmaya sahip sayısını tanımlar. |
|FaultDomainConstraintPriority | Int, varsayılan 0'dır |Dinamik| Hata etki alanı kısıtlaması önceliğini belirler: 0: sabit; 1: yazılım; Negatif: yoksay. |
|GlobalMovementThrottleCountingInterval | Zamanı saniye cinsinden 600 varsayılandır |Statik| TimeSpan saniye cinsinden belirtin. Etki alanı çoğaltma hareketleri (GlobalMovementThrottleThreshold birlikte kullanılan) başına izlemek üzere son aralık uzunluğu belirtin. 0 olarak genel tamamen azaltma yoksayacak şekilde ayarlayabilirsiniz. |
|GlobalMovementThrottleThreshold | Uint, varsayılan 1000'dir |Dinamik| Hareketleri Dengeleme aşamasında GlobalMovementThrottleCountingInterval tarafından belirtilen son aralığı izin verilen maksimum sayısı. |
|GlobalMovementThrottleThresholdForBalancing | Uint, varsayılan 0'dır | Dinamik|Hareketleri Dengeleme aşamasında GlobalMovementThrottleCountingInterval tarafından belirtilen son aralığı izin verilen maksimum sayısı. 0 sınır gösterir. |
|GlobalMovementThrottleThresholdForPlacement | Uint, varsayılan 0'dır |Dinamik| Sınır hareketleri yerleştirme aşamasında GlobalMovementThrottleCountingInterval.0 tarafından belirtilen son aralığı izin verilen en fazla sayısını gösterir.|
|GlobalMovementThrottleThresholdPercentage|Double, varsayılan 0'dır|Dinamik|Toplam hareketleri GlobalMovementThrottleCountingInterval tarafından belirtilen son aralığında (çoğaltma kümedeki toplam sayısı yüzdesi olarak ifade edilir) Yük Dengeleme ve yerleştirme aşamaları izin verilen maksimum sayısı. 0 sınır gösterir. Hem de bu ve GlobalMovementThrottleThreshold belirtilir; Daha sonra daha pasif sınırı kullanılır.|
|GlobalMovementThrottleThresholdPercentageForBalancing|Double, varsayılan 0'dır|Dinamik|Hareketleri (PLB yinelemede toplam sayısı yüzdesi olarak ifade edilir) aşamasında Dengeleme GlobalMovementThrottleCountingInterval tarafından belirtilen son aralığı izin verilen maksimum sayısı. 0 sınır gösterir. Hem de bu ve GlobalMovementThrottleThresholdForBalancing belirtilir; Daha sonra daha pasif sınırı kullanılır.|
|InBuildThrottlingAssociatedMetric | dize, varsayılan değer "" |Statik| Bu azaltma için ilişkili ölçüm adı. |
|InBuildThrottlingEnabled | Bool, varsayılan false |Dinamik| Yapı azaltma etkin olup olmadığını belirler. |
|InBuildThrottlingGlobalMaxValue | Int, varsayılan 0'dır |Dinamik|Genel izin yapı çoğaltmaların sayısı üst sınırından. |
|InterruptBalancingForAllFailoverUnitUpdates | Bool, varsayılan false | Dinamik|Yük devretme birimi güncelleştirme herhangi bir türde veya hızlı kesme çalıştırmak Dengeleme yavaş belirler. "False" Çalıştır Dengeleme kesilmesi durumunda ile belirtilen FailoverUnit: oluşturulan/silinir; çoğaltmalar eksik; Birincil çoğaltma konumu veya değiştirilen çoğaltmaların sayısı değiştirildi. Çalıştırma Dengeleme kesintiye diğer durumlarda - varsa FailoverUnit: ek çoğaltmalarına; sahip tüm çoğaltma bayrağı değiştirildi; yalnızca bölüm sürümü veya diğer herhangi bir durumu değişti. |
|MinConstraintCheckInterval | Zamanı saniye olarak varsayılan 1. |Dinamik| TimeSpan saniye cinsinden belirtin. Denetim iki ardışık kısıtlaması önce yuvarlar geçmesi gereken en düşük süreyi tanımlar. |
|MinLoadBalancingInterval | Zamanı saniye cinsinden varsayılan 5'tir |Dinamik| TimeSpan saniye cinsinden belirtin. İki ardışık karşı yuvarlar önce geçmesi gereken en düşük süreyi tanımlar. |
|Minplacementınterval | Zamanı saniye olarak varsayılan 1. |Dinamik| TimeSpan saniye cinsinden belirtin. İki ardışık yerleştirme yuvarlar önce geçmesi gereken en düşük süreyi tanımlar. |
|MoveExistingReplicaForPlacement | Bool, varsayılan değer true şeklindedir |Dinamik|Yerleştirme sırasında mevcut çoğaltma taşımak IF belirleyen ayarlama. |
|MovementPerPartitionThrottleCountingInterval | Zamanı saniye cinsinden 600 varsayılandır |Statik| TimeSpan saniye cinsinden belirtin. Geçen süre (MovementPerPartitionThrottleThreshold birlikte kullanılan) her bölüm için çoğaltma hareketleri izlemek üzere gösterir. |
|MovementPerPartitionThrottleThreshold | Uint, varsayılan 50'dir |Dinamik| Çoğaltmalar için ilgili hareketleri, bu bölümün Dengeleme sayısı üst sınırına veya belirttiği geçmiş aralığında MovementPerFailoverUnitThrottleThreshold aşıldı hiçbir Dengeleme ile ilgili taşıma için bir bölüm ortaya çıkar MovementPerPartitionThrottleCountingInterval. |
|MoveParentToFixAffinityViolation | Bool, varsayılan false |Dinamik| Benzeşim kısıtlamaları düzeltmek için üst çoğaltmaları taşınıp taşınamayacağını belirleyen ayarı.|
|PartiallyPlaceServices | Bool, varsayılan değer true şeklindedir |Dinamik| Kümedeki tüm hizmet çoğaltmaları "tümü veya hiçbiri" yerleştirilip yerleştirilmeyeceğini sınırlı uygun düğümler için bunları belirler.|
|PlaceChildWithoutParent | Bool, varsayılan değer true şeklindedir | Dinamik|Hiçbir üst çoğaltma yukarı ise alt çoğaltma hizmeti belirleyen ayarı yerleştirilebilir. |
|PlacementConstraintPriority | Int, varsayılan 0'dır | Dinamik|Yerleştirme kısıtlaması önceliğini belirler: 0: sabit; 1: yazılım; Negatif: yoksay. |
|PlacementConstraintValidationCacheSize | Int, varsayılan 10000 arasıdır |Dinamik| Tabloyu hızlı doğrulama için kullanılan ve yerleştirme kısıtlaması ifadeler, önbellek boyutunu sınırlar. |
|PlacementSearchTimeout | Zamanı saniye cinsinden 0,5 varsayılandır |Dinamik| TimeSpan saniye cinsinden belirtin. Hizmetleri yerleştirirken; Bu süre için en çok bir sonuç döndürmeden önce arayın. |
|PLBRefreshGap | Zamanı saniye olarak varsayılan 1. |Dinamik| TimeSpan saniye cinsinden belirtin. PLB durumu yeniden yeniler önce geçmesi gereken en düşük süreyi tanımlar. |
|PreferredLocationConstraintPriority | Int, varsayılan 2'dir| Dinamik|Tercih edilen konum kısıtlaması önceliğini belirler: 0: sabit; 1: yazılım; 2: en iyi duruma getirme; Negatif: yoksay |
|PreventTransientOvercommit | Bool, varsayılan false | Dinamik|PLB tarafından başlatılan taşıma önbellekten kaynaklar hemen saymak belirler. Varsayılan olarak; PLB taşıma çıkışı başlatabilir ve hangi geçici oluşturabilirsiniz aynı düğümde taşıma bilgisayar. Bu parametre ayarı true olarak bu tür engeller, overcommits ve devre dışı isteğe bağlı birleştirme (diğer adıyla placementWithMove). |
|ScaleoutCountConstraintPriority | Int, varsayılan 0'dır |Dinamik| Genişletme sayısı kısıtlaması önceliğini belirler: 0: sabit; 1: yazılım; Negatif: yoksay. |
|SwapPrimaryThrottlingAssociatedMetric | dize, varsayılan değer ""|Statik| Bu azaltma için ilişkili ölçüm adı. |
|SwapPrimaryThrottlingEnabled | Bool, varsayılan false|Dinamik| Takas birincil azaltma etkin olup olmadığını belirler. |
|SwapPrimaryThrottlingGlobalMaxValue | Int, varsayılan 0'dır |Dinamik| Genel izin takas birincil çoğaltmaların sayısı üst sınırından. |
|TraceCRMReasons |Bool, varsayılan değer true şeklindedir |Dinamik|Çalışma olaylarını kanala hareketleri verilen CRM nedenlerle izleme belirtir. |
|UpgradeDomainConstraintPriority | Int, varsayılan 1.| Dinamik|Yükseltme etki alanı kısıtlaması önceliğini belirler: 0: sabit; 1: yazılım; Negatif: yoksay. |
|UseMoveCostReports | Bool, varsayılan false | Dinamik|Puanlama işlevi Maliyet öğesinin yoksaymayı LB söyler; Sonuçta elde edilen çok sayıda olasılığı daha iyi dengelenmiş yerleştirme için taşır. |
|UseSeparateSecondaryLoad | Bool, varsayılan değer true şeklindedir | Dinamik|Varsa belirleyen ayarı farklı ikincil yük kullanın. |
|ValidatePlacementConstraint | Bool, varsayılan değer true şeklindedir |Dinamik| Bir hizmetin ServiceDescription güncelleştirildiğinde PlacementConstraint ifade bir hizmet için doğrulanmış olup olmadığını belirtir. |
|VerboseHealthReportLimit | Int, varsayılan 20'dir | Dinamik|(Ayrıntılı sistem durumu raporlama etkinse) sistem durumu uyarısı için bildirilen önce yerleştirilmemiş gitmek için bir çoğaltma sahip sayısını tanımlar. |

## <a name="reconfigurationagent"></a>ReconfigurationAgent
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|ApplicationUpgradeMaxReplicaCloseDuration | Zamanı saniye cinsinden 900 varsayılandır |Dinamik|TimeSpan saniye cinsinden belirtin. Uygulama yükseltme sırasında içinde takılmış çoğaltmaları olan hizmet ana bilgisayarlar sonlandırmadan önce sistemin bekleyeceği süre kapatın.|
|FabricUpgradeMaxReplicaCloseDuration | Zamanı saniye cinsinden 900 varsayılandır |Dinamik| TimeSpan saniye cinsinden belirtin. İçinde takılmış çoğaltmaları olan hizmet ana bilgisayarlar sonlandırmadan önce sistemin bekleyeceği süre fabric yükseltmesi sırasında kapatın. |
|GracefulReplicaShutdownMaxDuration|TimeSpan, Common::TimeSpan::FromSeconds(120) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. İçinde takılmış çoğaltmaları olan hizmet ana bilgisayarlar sonlandırmadan önce sistemin bekleyeceği süre kapatın. Bu değer 0 olarak ayarlarsanız, çoğaltmaları kapatmak için istenecek değil.|
|NodeDeactivationMaxReplicaCloseDuration | Zamanı saniye cinsinden 900 varsayılandır |Dinamik|TimeSpan saniye cinsinden belirtin. İçinde takılmış çoğaltmaları olan hizmet ana bilgisayarlar sonlandırmadan önce sistemin bekleyeceği süre düğümü devre dışı bırakma sırasında kapatın. |
|PeriodicApiSlowTraceInterval | Zamanı saniye cinsinden, varsayılan değer 5 dakikadır |Dinamik| TimeSpan saniye cinsinden belirtin. PeriodicApiSlowTraceInterval üzerinden yavaş API çağrıları API İzleyici tarafından yeniden taranma aralığı tanımlar. |
|ReplicaChangeRoleFailureRestartThreshold|Int, varsayılan 10'dur|Dinamik| Tam sayı. Otomatik azaltma eylemi (çoğaltma yeniden) sonra uygulanacak olan birincil yükseltmesi sırasında API hatalarının sayısını belirtin. |
|ReplicaChangeRoleFailureWarningReportThreshold|int, varsayılan değer 2147483647'dir|Dinamik| Tam sayı. Daha sonra sistem durumu raporu uyarı gerçekleştirilecektir birincil yükseltmesi sırasında API hatalarının sayısını belirtin.|
|ServiceApiHealthDuration | Zamanı saniye cinsinden, varsayılan değer 30 dakikadır |Dinamik| TimeSpan saniye cinsinden belirtin. Biz biz sağlıksız raporu önce çalıştırmak hizmeti API'si için ne kadar süreyle bekleme ServiceApiHealthDuration tanımlar. |
|ServiceReconfigurationApiHealthDuration | Zamanı saniye cinsinden varsayılan 30'dur |Dinamik| TimeSpan saniye cinsinden belirtin. Biz biz sağlıksız rapor önce çalıştırmak hizmeti API'si için ne kadar süreyle bekleme ServiceReconfigurationApiHealthDuration tanımlar. Bu kullanılabilirlik etkisi API çağrıları için geçerlidir.|

## <a name="replication"></a>Çoğaltma
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi**| **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|BatchAcknowledgementInterval|TimeSpan, Common::TimeSpan::FromMilliseconds(15) varsayılandır|Statik|TimeSpan saniye cinsinden belirtin. Onay yeniden göndermeden önce bir işlem aldıktan sonra çoğaltıcı bekleyeceği süreyi belirler. Bu süre içinde alınan diğer işlemleri çoğaltıcı verimini büyük olasılıkla azaltma ancak azalan ağ trafiğini -> tek bir iletide gönderilen kendi onayları sahip olur.|
|MaxCopyQueueSize|Uint, varsayılan 1024'tür.|Statik|Bu en yüksek değer çoğaltma işlemleri tutar sıranın ilk boyutu değeri tanımlar. 2'in üssü olması gerektiğini unutmayın. Çalışma zamanı sırasında bu boyutu işlem sırası büyürse birincil ve ikincil çoğaltıcılar kısıtlanacak.|
|MaxPrimaryReplicationQueueMemorySize|Uint, varsayılan 0'dır|Statik|Birincil çoğaltma sırası bayt cinsinden en büyük değerini budur.|
|MaxPrimaryReplicationQueueSize|Uint, varsayılan 1024'tür.|Statik|Birincil çoğaltma sırası mevcut işlemlerinin maksimum sayısı budur. 2'in üssü olması gerektiğini unutmayın.|
|MaxReplicationMessageSize|Uint 52428800 varsayılandır|Statik|Çoğaltma işlemleri maksimum ileti boyutu. Varsayılan 50 MB'dir.|
|MaxSecondaryReplicationQueueMemorySize|Uint, varsayılan 0'dır|Statik|İkincil çoğaltma sırası bayt cinsinden en büyük değerini budur.|
|MaxSecondaryReplicationQueueSize|uint, varsayılan 2048'dir|Statik|Bu ikincil çoğaltma sırası mevcut işlemlerinin maksimum sayıdır. 2'in üssü olması gerektiğini unutmayın.|
|QueueHealthMonitoringInterval|TimeSpan, Common::TimeSpan::FromSeconds(30) varsayılandır|Statik|TimeSpan saniye cinsinden belirtin. Bu değer, çoğaltma işlemi sıralardaki uyarı/hata sistem durumu olaylarını izlemek için çoğaltıcı tarafından kullanılan süreyi belirler. '0' değeri, sistem durumu izleme devre dışı bırakır. |
|QueueHealthWarningAtUsagePercent|uint, varsayılan 80'dir|Statik|Bu değer daha sonra size yüksek sıra kullanımı hakkında uyarı rapor çoğaltma kuyruğu kullanımı (yüzde) belirler. Biz bunu QueueHealthMonitoringInterval yetkisiz aralığından sonra yapın. Sıranın kullanım yetkisiz aralığa bu yüzdenin altında kalırsa|
|ReplicatorAddress|Varsayılan L "localhost:0" dizesidir|Statik|Bir dizenin-'Windows Fabric çoğaltması tarafından gönderme/alma işlemleri için diğer çoğaltmaları ile bağlantı kurmak için kullanılan IP: BağlantıNoktası' formunda uç noktası.|
|ReplicatorListenAddress|Varsayılan L "localhost:0" dizesidir|Statik|Bir dizenin-'Windows Fabric çoğaltması tarafından diğer çoğaltmalardan alma işlemleri için kullanılan IP: BağlantıNoktası' formunda uç noktası.|
|ReplicatorPublishAddress|Varsayılan L "localhost:0" dizesidir|Statik|Bir dizenin-'Windows Fabric çoğaltması tarafından diğer çoğaltmalar için gönderme işlemleri için kullanılan IP: BağlantıNoktası' formunda uç noktası.|
|Retryınterval|TimeSpan, Common::TimeSpan::FromSeconds(5) varsayılandır|Statik|TimeSpan saniye cinsinden belirtin. Ne zaman bir işlem kaybolur veya bu Zamanlayıcı reddedilen işlemi gönderme çoğaltıcı ne sıklıkta deneyecek belirler.|

## <a name="runas"></a>Farklı Çalıştır
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|RunAsAccountName |dize, varsayılan değer "" |Dinamik|RunAs hesabı adını gösterir. Bu yalnızca "DomainUser" veya "Olarak" hesabı için gerekli türü. Geçerli değerler: "etki alanı\kullanıcı" veya "user@domain". |
|RunAsAccountType|dize, varsayılan değer "" |Dinamik|RunAs hesap türünü gösterir. Tüm RunAs bölümü geçerli değerler "DomainUser/NetworkService/olarak/LocalSystem" için bu gereklidir.|
|Frk.Çal.parolası|dize, varsayılan değer "" |Dinamik|RunAs hesabı parolasını belirtir. Bu, yalnızca "DomainUser" hesap türü için gereklidir. |

## <a name="runasdca"></a>RunAs_DCA
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|RunAsAccountName |dize, varsayılan değer "" |Dinamik|RunAs hesabı adını gösterir. Bu yalnızca "DomainUser" veya "Olarak" hesabı için gerekli türü. Geçerli değerler: "etki alanı\kullanıcı" veya "user@domain". |
|RunAsAccountType|dize, varsayılan değer "" |Dinamik|RunAs hesap türünü gösterir. Tüm RunAs bölümü geçerli değerler "YerelKullanıcı/DomainUser/NetworkService/olarak/LocalSystem" için bu gereklidir. |
|Frk.Çal.parolası|dize, varsayılan değer "" |Dinamik|RunAs hesabı parolasını belirtir. Bu, yalnızca "DomainUser" hesap türü için gereklidir. |

## <a name="runasfabric"></a>RunAs_Fabric
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|RunAsAccountName |dize, varsayılan değer "" |Dinamik|RunAs hesabı adını gösterir. Bu yalnızca "DomainUser" veya "Olarak" hesabı için gerekli türü. Geçerli değerler: "etki alanı\kullanıcı" veya "user@domain". |
|RunAsAccountType|dize, varsayılan değer "" |Dinamik|RunAs hesap türünü gösterir. Tüm RunAs bölümü geçerli değerler "YerelKullanıcı/DomainUser/NetworkService/olarak/LocalSystem" için bu gereklidir. |
|Frk.Çal.parolası|dize, varsayılan değer "" |Dinamik|RunAs hesabı parolasını belirtir. Bu, yalnızca "DomainUser" hesap türü için gereklidir. |

## <a name="runashttpgateway"></a>RunAs_HttpGateway
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|RunAsAccountName |dize, varsayılan değer "" |Dinamik|RunAs hesabı adını gösterir. Bu yalnızca "DomainUser" veya "Olarak" hesabı için gerekli türü. Geçerli değerler: "etki alanı\kullanıcı" veya "user@domain". |
|RunAsAccountType|dize, varsayılan değer "" |Dinamik|RunAs hesap türünü gösterir. Tüm RunAs bölümü geçerli değerler "YerelKullanıcı/DomainUser/NetworkService/olarak/LocalSystem" için bu gereklidir. |
|Frk.Çal.parolası|dize, varsayılan değer "" |Dinamik|RunAs hesabı parolasını belirtir. Bu, yalnızca "DomainUser" hesap türü için gereklidir. |

## <a name="security"></a>Güvenlik
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|AADClientApplication|Varsayılan L dizesidir""|Statik|Yerel istemci uygulaması adı veya kimliği doku istemcileri temsil eden |
|AADClusterApplication|Varsayılan L dizesidir""|Statik|Web API uygulaması adı veya kümenin temsil eden kimliği |
|AADTenantId|Varsayılan L dizesidir""|Statik|Kiracı kimlik (GUID) |
|AdminClientCertThumbprints|Varsayılan L dizesidir""|Dinamik|Yönetici rolü istemciler tarafından kullanılan sertifikalarının parmak izleri. Virgülle ayrılmış ad listesi verilmiştir. |
|AdminClientClaims|Varsayılan L dizesidir""|Dinamik|Tüm olası talepler yönetici istemcilerden beklenen; ClientClaims ile aynı biçimi; Bu liste ClientClaims için dahili olarak eklenen; Ayrıca aynı girdiler için ClientClaims eklemek için bu nedenle gerekmez. |
|AdminClientIdentities|Varsayılan L dizesidir""|Dinamik|Yönetici rolü doku istemcilerin Windows kimliklerini; Ayrıcalıklı doku işlemleri yetkilendirmek için kullanılır. Virgülle ayrılmış bir liste olduğu; Her bir etki alanı hesabı veya grup adı girişidir. Kolaylık olması için; fabric.exe çalıştıran hesabı otomatik olarak yönetici rolü atanır; Bu nedenle olduğu ServiceFabricAdministrators grup. |
|CertificateExpirySafetyMargin|TimeSpan, Common::TimeSpan::FromMinutes(43200) varsayılandır|Statik|TimeSpan saniye cinsinden belirtin. Sertifika süre sonu için güvenlik marjı; sona erme bu değerden daha yakın olduğunda sertifika durumu rapor Tamam uyarı olarak değiştirir. Varsayılan değer 30 gündür. |
|CertificateHealthReportingInterval|TimeSpan, Common::TimeSpan::FromSeconds(3600 * 8) varsayılandır|Statik|TimeSpan saniye cinsinden belirtin. Sistem durumu sertifikası raporlama aralığı belirtin; Varsayılan olarak 8 saat; Sistem durumu sertifikası raporlama için 0 ayarı devre dışı bırakır |
|ClientCertThumbprints|Varsayılan L dizesidir""|Dinamik|Kümeye anlaşmak için istemcileri tarafından kullanılan sertifika parmak izlerini; Küme kullanan bu gelen bağlantıyı yetkilendirir. Virgülle ayrılmış ad listesi verilmiştir. |
|ClientClaimAuthEnabled|bool, varsayılan FALSE olur|Statik|Talep tabanlı kimlik doğrulaması istemcilerde etkin olup olmadığını gösterir; Bu örtülü olarak doğru ayarı ClientRoleEnabled ayarlar. |
|ClientClaims|Varsayılan L dizesidir""|Dinamik|Tüm olası talepler istemcilerinden gelen ağ geçidine bağlanmak için bekleniyor. Bu 'Veya' listesidir: ClaimsEntry \| \| ClaimsEntry \| \| ... ClaimsEntry her ClaimsEntry "Ve" listesidir: ClaimType ClaimValue = & & ClaimType ClaimValue = & & ClaimType ClaimValue =... |
|ClientIdentities|Varsayılan L dizesidir""|Dinamik|FabricClient kimliklerini Windows; adlandırma ağ geçidi bu gelen bağlantıları yetkilendirmek için kullanır. Virgülle ayrılmış bir liste olduğu; Her bir etki alanı hesabı veya grup adı girişidir. Kolaylık olması için; fabric.exe çalıştıran hesabı otomatik olarak izin verilir; Bu nedenle Grup ServiceFabricAllowedUsers ve ServiceFabricAdministrators'dır. |
|ClientRoleEnabled|bool, varsayılan FALSE olur|Statik|İstemci rolü etkin olup olmadığını gösterir; ayarlandığında true; istemciler kendi kimliği temel rol atanır. V2 için; Etkinleştirme bu istemci AdminClientCommonNames/AdminClientIdentities değil, yalnızca salt okunur işlemler yürütme anlamına gelir. |
|ClusterCertThumbprints|Varsayılan L dizesidir""|Dinamik|Kümeye katılmak için izin verilen sertifikaların parmak izleri; bir adı virgülle ayrılmış listesi. |
|ClusterCredentialType|Varsayılan dizesidir L "Hiçbiri"|İzin Verilmiyor|Küme güvenli hale getirmek için kullanılacak güvenlik kimlik bilgileri türünü belirtir. Geçerli değerler "Hiçbiri/X509/Windows" |
|ClusterIdentities|Varsayılan L dizesidir""|Dinamik|Küme düğümlerinin Windows kimliklerini; Küme üyeliği yetkilendirme için kullanılır. Virgülle ayrılmış bir liste olduğu; Her bir etki alanı hesabı veya grup adı bir giriştir |
|ClusterSpn|Varsayılan L dizesidir""|İzin Verilmiyor|Hizmet asıl adı küme; ne zaman doku tek etki alanı kullanıcısı (gMSA/etki alanı kullanıcı hesabı) olarak çalışır. Kira dinleyicileri ve fabric.exe dinleyicileri SPN'sini olduğu: Federasyon dinleyicileri; İç çoğaltma dinleyicileri; çalışma zamanı hizmet dinleyici ve adlandırma ağ geçidi dinleyicisi. Doku makine hesabı olarak çalıştığında bu boş bırakılmalıdır; yan bağlanma ve bu durumda dinleyicisi SPN dinleyicisi aktarım adresinden işlem. |
|CrlCheckingFlag|uint, varsayılan 0x40000000 olduğu|Dinamik|Varsayılan Sertifika zinciri doğrulama bayrağı; Bileşen özgü bayrağı tarafından geçersiz kılınmış olabilir; Örneğin Federasyon/X509CertChainFlags 0x10000000 CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN 0x40000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT 0x80000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ 0 devre dışı bırakır CRL denetimi tam desteklenen değerler listesi yalnızca ayarına CertGetCertificateChain dwFlags tarafından belgelenmiştir: http://msdn.microsoft.com/library/windows/desktop/aa376078(v=vs.85).aspx |
|CrlDisablePeriod|TimeSpan, Common::TimeSpan::FromMinutes(15) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. Ne kadar süreyle CRL denetimi için belirli bir sertifika çevrimdışı hatayla karşılaşıyor sonra devre dışı bırakılır; CRL çevrimdışı hata göz ardı edilebilir değilse. |
|CrlOfflineHealthReportTtl|TimeSpan, Common::TimeSpan::FromMinutes(1440) varsayılandır|Dinamik|TimeSpan saniye cinsinden belirtin. |
|DisableFirewallRuleForDomainProfile| bool, varsayılan true'dur. |Statik| Güvenlik duvarı kuralı etki alanı profili için etkinleştirilmemelidir olmadığını gösterir |
|DisableFirewallRuleForPrivateProfile| bool, varsayılan true'dur. |Statik| Güvenlik duvarı kuralı için özel profil etkinleştirilmemelidir olmadığını gösterir | 
|DisableFirewallRuleForPublicProfile| bool, varsayılan true'dur. | Statik|Güvenlik duvarı kuralı için genel profil etkinleştirilmemelidir olmadığını gösterir |
|FabricHostSpn| Varsayılan L dizesidir"" |Statik| Hizmet asıl adı FabricHost; ne zaman doku tek etki alanı kullanıcısı (gMSA/etki alanı kullanıcı hesabı) olarak çalışır ve FabricHost makine hesabı altında çalışır. FabricHost, SPN IPC dinleyiciyi olur; FabricHost makine hesabı altında çalışır olduğundan varsayılan olarak boş bırakılmalıdır |
|IgnoreCrlOfflineError|bool, varsayılan FALSE olur|Dinamik|Sunucu tarafı gelen istemci sertifikalarını doğrularken CRL çevrimdışı hatayı yok sayıp verilip |
|IgnoreSvrCrlOfflineError|bool, varsayılan true'dur.|Dinamik|İstemci tarafı gelen sunucu sertifikalarını doğrularken CRL çevrimdışı hatayı yok sayıp verilip verilmeyeceğini; Varsayılan true. İptal edilen sertifikaları saldırılarına DNS tehlikeye gerektirir; ile daha zor istemci sertifikalarını iptal etti. |
|ServerAuthCredentialType|Varsayılan dizesidir L "Hiçbiri"|Statik|FabricClient ve küme arasındaki iletişimin güvenliğini sağlamak için kullanılacak güvenlik kimlik bilgileri türünü belirtir. Geçerli değerler "Hiçbiri/X509/Windows" |
|ServerCertThumbprints|Varsayılan L dizesidir""|Dinamik|İstemciler için iletişim kurabilecek şekilde küme tarafından kullanılan sunucu sertifikaları parmak izlerini; istemciler bu küme kimlik doğrulaması yapmak için kullanın. Virgülle ayrılmış ad listesi verilmiştir. |
|SettingsX509StoreName| Varsayılan L "MY" dizesidir| Dinamik|X509 sertifika deposunu yapılandırma koruması fabric tarafından kullanılan |
|X509Folder|Varsayılan /var/lib/waagent dizesidir|Statik|Klasör nerede X509 sertifikaları ve özel anahtarları konumlandırıldığını |

## <a name="securityadminclientx509names"></a>Güvenlik/AdminClientX509Names
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, varsayılan, Yok'tur|Dinamik| |

## <a name="securityclientaccess"></a>Güvenlik/ClientAccess
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|ActivateNode |Varsayılan "Yönetici" dizesidir |Dinamik| Bir düğüm etkinleştirme için güvenlik yapılandırma. |
|CancelTestCommand |Varsayılan "Yönetici" dizesidir |Dinamik| Uçuş modunda olması durumunda belirli bir TestCommand - iptal eder. |
|CodePackageControl |Varsayılan "Yönetici" dizesidir |Dinamik| Yeniden başlatma kodu paketleri Güvenlik Yapılandırması'nı tıklatın. |
|CreateApplication |Varsayılan "Yönetici" dizesidir | Dinamik|Uygulama oluşturma Güvenlik Yapılandırması. |
|CreateComposeDeployment|Varsayılan L "Yönetici" dizesidir| Dinamik|Oluşturma dosyaları tarafından açıklanan Oluştur dağıtımı oluşturur |
|CreateName |Varsayılan "Yönetici" dizesidir |Dinamik|Güvenlik yapılandırma adlandırma URI'si oluşturma. |
|CreateService |Varsayılan "Yönetici" dizesidir |Dinamik| Hizmet oluşturma için güvenlik yapılandırma. |
|CreateServiceFromTemplate |Varsayılan "Yönetici" dizesidir |Dinamik|Şablondan hizmet oluşturma için güvenlik yapılandırma. |
|DeactivateNode |Varsayılan "Yönetici" dizesidir |Dinamik| Bir düğüm devre dışı bırakma için Güvenlik Yapılandırması'nı tıklatın. |
|DeactivateNodesBatch |Varsayılan "Yönetici" dizesidir |Dinamik| Birden çok düğüm devre dışı bırakma için Güvenlik Yapılandırması'nı tıklatın. |
|Sil |Varsayılan "Yönetici" dizesidir |Dinamik| Görüntü için güvenlik yapılandırmalarını istemci silme işlemi depolar. |
|DeleteApplication |Varsayılan "Yönetici" dizesidir |Dinamik| Uygulama silme işlemi için güvenlik yapılandırması. |
|DeleteComposeDeployment|Varsayılan L "Yönetici" dizesidir| Dinamik|Oluşturma dağıtım siler |
|DeleteName |Varsayılan "Yönetici" dizesidir |Dinamik|Güvenlik yapılandırma adlandırma URI'si silme işlemi için. |
|DeleteService |Varsayılan "Yönetici" dizesidir |Dinamik|Hizmet silinmek güvenlik yapılandırması. |
|EnumerateProperties |dize, varsayılan değer "yönetici\|\|kullanıcı" | Dinamik|Özellik numaralandırma Adlandırma Güvenlik Yapılandırması'nı tıklatın. |
|EnumerateSubnames |dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Adlandırma URI'si numaralandırması için güvenlik yapılandırma. |
|FileContent |Varsayılan "Yönetici" dizesidir |Dinamik| Görüntü için Güvenlik Yapılandırması istemci dosya aktarımı (kümeye dış) depolar. |
|FileDownload |Varsayılan "Yönetici" dizesidir |Dinamik| Görüntü deposu istemci dosya indirme başlatma (kümeye dış) için güvenlik yapılandırma. |
|FinishInfrastructureTask |Varsayılan "Yönetici" dizesidir |Dinamik| Sonlandırma altyapı görevleri için güvenlik yapılandırma. |
|GetChaosReport | dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Belirli bir zaman aralığı içinde karmaşık dünyada durum getirir. |
|GetClusterConfiguration | dize, varsayılan değer "yönetici\|\|kullanıcı" | Dinamik|GetClusterConfiguration bir bölüme uygulanmasını. |
|GetClusterConfigurationUpgradeStatus | dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| GetClusterConfigurationUpgradeStatus bir bölüme uygulanmasını. |
|GetFabricUpgradeStatus |dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Küme yükseltme durumunu yoklama için Güvenlik Yapılandırması'nı tıklatın. |
|GetNodeDeactivationStatus |Varsayılan "Yönetici" dizesidir |Dinamik| Devre dışı bırakma durumunu denetlemek için Güvenlik Yapılandırması'nı tıklatın. |
|GetNodeTransitionProgress | dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Bir düğüm geçiş komutunda ilerleme durumunu almak için Güvenlik Yapılandırması'nı tıklatın. |
|GetPartitionDataLossProgress | dize, varsayılan değer "yönetici\|\|kullanıcı" | Dinamik|Bir Invoke veri kaybı API çağrısı için ilerleme getirir. |
|GetPartitionQuorumLossProgress | dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Bir Invoke çekirdek kayıp API çağrısı için ilerleme getirir. |
|GetPartitionRestartProgress | dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Bir yeniden başlatma bölüm API çağrısı için ilerleme getirir. |
|GetServiceDescription |dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Uzun yoklama hizmet bildirimleri ve hizmeti açıklamaları okumak için güvenlik yapılandırma. |
|GetStagingLocation |Varsayılan "Yönetici" dizesidir |Dinamik| Güvenlik Yapılandırma görüntüsü için konum alma hazırlama istemci depolar. |
|GetStoreLocation |Varsayılan "Yönetici" dizesidir |Dinamik| Görüntü için Güvenlik Yapılandırması istemci deposu konum alma depolar. |
|GetUpgradeOrchestrationServiceState|Varsayılan L "Yönetici" dizesidir| Dinamik|Bir bölüme GetUpgradeOrchestrationServiceState uygulanmasını |
|GetUpgradesPendingApproval |Varsayılan "Yönetici" dizesidir |Dinamik| GetUpgradesPendingApproval bir bölüme uygulanmasını. |
|GetUpgradeStatus |dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Uygulama yükseltme durumunu yoklama için Güvenlik Yapılandırması'nı tıklatın. |
|InternalList |Varsayılan "Yönetici" dizesidir | Dinamik|Görüntü için Güvenlik Yapılandırması istemci dosya listesi işlemi (iç) depolar. |
|InvokeInfrastructureCommand |Varsayılan "Yönetici" dizesidir |Dinamik| Altyapı görev yönetimi komutları için güvenlik yapılandırma. |
|InvokeInfrastructureQuery |dize, varsayılan değer "yönetici\|\|kullanıcı" | Dinamik|Altyapı görevleri sorgulamak için Güvenlik Yapılandırması'nı tıklatın. |
|Liste |dize, varsayılan değer "yönetici\|\|kullanıcı" | Dinamik|Görüntü için Güvenlik Yapılandırması istemci dosya listesi işlemi depolar. |
|MoveNextFabricUpgradeDomain |Varsayılan "Yönetici" dizesidir |Dinamik| Küme yükseltme açıkça yükseltme etki alanı ile sürdürülüyor için Güvenlik Yapılandırması'nı tıklatın. |
|MoveNextUpgradeDomain |Varsayılan "Yönetici" dizesidir |Dinamik| Açık bir yükseltme etki alanı ile uygulama yükseltmelerini sürdürme için Güvenlik Yapılandırması'nı tıklatın. |
|MoveReplicaControl |Varsayılan "Yönetici" dizesidir | Dinamik|Çoğaltma taşıyın. |
|NameExists |dize, varsayılan değer "yönetici\|\|kullanıcı" | Dinamik|Güvenlik yapılandırma adlandırma URI'si varlığı denetler. |
|NodeControl |Varsayılan "Yönetici" dizesidir |Dinamik| Başlangıç için Güvenlik Yapılandırması'nı tıklatın; durdurma; ve düğüm yeniden başlatılıyor. |
|NodeStateRemoved |Varsayılan "Yönetici" dizesidir |Dinamik| Kaldırılan düğüm durumu raporlama için Güvenlik Yapılandırması'nı tıklatın. |
|Ping |dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| İstemci ping için güvenlik yapılandırma. |
|PredeployPackageToNode |Varsayılan "Yönetici" dizesidir |Dinamik| Dağıtım öncesi API. |
|PrefixResolveService |dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Hizmet uyumlu tabanlı ön ek çözümleme için güvenlik yapılandırma. |
|PropertyReadBatch |dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Güvenlik yapılandırma adlandırma özelliği okuma işlemlerini. |
|PropertyWriteBatch |Varsayılan "Yönetici" dizesidir |Dinamik|Güvenlik yapılandırmalarını adlandırma özelliği için yazma işlemleri. |
|ProvisionApplicationType |Varsayılan "Yönetici" dizesidir |Dinamik| Uygulama sağlama türünü için Güvenlik Yapılandırması'nı tıklatın. |
|ProvisionFabric |Varsayılan "Yönetici" dizesidir |Dinamik| MSI ve/veya küme bildirimi sağlama Güvenlik Yapılandırması'nı tıklatın. |
|Sorgu |dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Sorgular için güvenlik yapılandırma. |
|RecoverPartition |Varsayılan "Yönetici" dizesidir | Dinamik|Bir bölüm kurtarmak için Güvenlik Yapılandırması'nı tıklatın. |
|RecoverPartitions |Varsayılan "Yönetici" dizesidir | Dinamik|Bölümler kurtarmak için Güvenlik Yapılandırması'nı tıklatın. |
|RecoverServicePartitions |Varsayılan "Yönetici" dizesidir |Dinamik| Hizmet bölümlerini kurtarmak için Güvenlik Yapılandırması'nı tıklatın. |
|RecoverSystemPartitions |Varsayılan "Yönetici" dizesidir |Dinamik| Sistem hizmeti bölümleri kurtarmak için Güvenlik Yapılandırması'nı tıklatın. |
|RemoveNodeDeactivations |Varsayılan "Yönetici" dizesidir |Dinamik| Birden çok düğümde geri alma devre dışı bırakma için güvenlik yapılandırma. |
|ReportFabricUpgradeHealth |Varsayılan "Yönetici" dizesidir |Dinamik| Küme yükseltme geçerli yükseltme ilerleme durumu ile sürdürülüyor için Güvenlik Yapılandırması'nı tıklatın. |
|ReportFault |Varsayılan "Yönetici" dizesidir |Dinamik| Hata raporlama için Güvenlik Yapılandırması'nı tıklatın. |
|ReportHealth |Varsayılan "Yönetici" dizesidir |Dinamik| Sistem durumu raporlama için Güvenlik Yapılandırması'nı tıklatın. |
|ReportUpgradeHealth |Varsayılan "Yönetici" dizesidir |Dinamik| Geçerli yükseltme ilerleme durumu ile uygulama yükseltmelerini sürdürme için Güvenlik Yapılandırması'nı tıklatın. |
|ResetPartitionLoad |dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Bir failoverUnit sıfırlama yük için güvenlik yapılandırma. |
|ResolveNameOwner |dize, varsayılan değer "yönetici\|\|kullanıcı" | Dinamik|Adlandırma URI'si sahibi çözmek için Güvenlik Yapılandırması'nı tıklatın. |
|ResolvePartition |dize, varsayılan değer "yönetici\|\|kullanıcı" | Dinamik|Sistem Hizmetleri çözmek için Güvenlik Yapılandırması'nı tıklatın. |
|ResolveService |dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Hizmet uyumlu tabanlı çözümlemesi için güvenlik yapılandırması. |
|ResolveSystemService|Varsayılan dizesidir L "yönetici\|\|kullanıcı"|Dinamik| Sistem Hizmetleri çözmek için güvenlik yapılandırması |
|RollbackApplicationUpgrade |Varsayılan "Yönetici" dizesidir |Dinamik| Uygulama yükseltme geri için Güvenlik Yapılandırması'nı tıklatın. |
|RollbackFabricUpgrade |Varsayılan "Yönetici" dizesidir |Dinamik| Küme yükseltme geri için Güvenlik Yapılandırması'nı tıklatın. |
|ServiceNotifications |dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Olay tabanlı hizmet bildirimleri için güvenlik yapılandırma. |
|SetUpgradeOrchestrationServiceState|Varsayılan L "Yönetici" dizesidir| Dinamik|Bir bölüme SetUpgradeOrchestrationServiceState uygulanmasını |
|StartApprovedUpgrades |Varsayılan "Yönetici" dizesidir |Dinamik| StartApprovedUpgrades bir bölüme uygulanmasını. |
|StartChaos |Varsayılan "Yönetici" dizesidir |Dinamik| Zaten başlatılmadığında Chaos - başlatır. |
|StartClusterConfigurationUpgrade |Varsayılan "Yönetici" dizesidir |Dinamik| StartClusterConfigurationUpgrade bir bölüme uygulanmasını. |
|StartInfrastructureTask |Varsayılan "Yönetici" dizesidir | Dinamik|Altyapısı görevlerini başlatmak için Güvenlik Yapılandırması'nı tıklatın. |
|StartNodeTransition |Varsayılan "Yönetici" dizesidir |Dinamik| Bir düğüm geçişi başlatmak için Güvenlik Yapılandırması'nı tıklatın. |
|StartPartitionDataLoss |Varsayılan "Yönetici" dizesidir |Dinamik| Veri kaybı bir bölüme uygulanmasını. |
|StartPartitionQuorumLoss |Varsayılan "Yönetici" dizesidir |Dinamik| Bir bölüm çekirdek kaybı uygulanmasını. |
|StartPartitionRestart |Varsayılan "Yönetici" dizesidir |Dinamik| Aynı anda bir bölüm kopyalarını bazılarını veya tümünü yeniden başlatır. |
|StopChaos |Varsayılan "Yönetici" dizesidir |Dinamik| Başlatılmış olup olmadığını Chaos - durdurur. |
|ToggleVerboseServicePlacementHealthReporting | dize, varsayılan değer "yönetici\|\|kullanıcı" |Dinamik| Ayrıntılı ServicePlacement HealthReporting geçiş için güvenlik yapılandırma. |
|UnprovisionApplicationType |Varsayılan "Yönetici" dizesidir |Dinamik| Uygulama türü sağlamanın kaldırılması için Güvenlik Yapılandırması'nı tıklatın. |
|UnprovisionFabric |Varsayılan "Yönetici" dizesidir |Dinamik| MSI ve/veya küme bildirimi sağlamanın kaldırılması için Güvenlik Yapılandırması'nı tıklatın. |
|UnreliableTransportControl |Varsayılan "Yönetici" dizesidir |Dinamik| Ekleme ve kaldırma davranışları için güvenilir olmayan aktarım. |
|UpdateService |Varsayılan "Yönetici" dizesidir |Dinamik|Hizmet güncelleştirmeleri için güvenlik yapılandırma. |
|UpgradeApplication |Varsayılan "Yönetici" dizesidir |Dinamik| Başlatılması veya uygulama yükseltmelerini kesintiye Güvenlik Yapılandırması'nı tıklatın. |
|UpgradeComposeDeployment|Varsayılan L "Yönetici" dizesidir| Dinamik|Oluşturma dağıtım yükseltir |
|UpgradeFabric |Varsayılan "Yönetici" dizesidir |Dinamik| Küme yükseltme başlatmak için Güvenlik Yapılandırması'nı tıklatın. |
|Karşıya Yükle |Varsayılan "Yönetici" dizesidir | Dinamik|Görüntü için güvenlik yapılandırması istemcisi karşıya yükleme işlemi depolar. |

## <a name="securityclientcertificateissuerstores"></a>Güvenlik/ClientCertificateIssuerStores
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|IssuerStoreKeyValueMap, varsayılan, Yok'tur |Dinamik|X509 veren sertifika depoları için istemci sertifikaları; Adı = clientIssuerCN; Değer = depoları virgülle ayrılmış listesi |

## <a name="securityclientx509names"></a>Güvenlik/ClientX509Names
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, varsayılan, Yok'tur|Dinamik| |

## <a name="securityclustercertificateissuerstores"></a>Güvenlik/ClusterCertificateIssuerStores
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|IssuerStoreKeyValueMap, varsayılan, Yok'tur |Dinamik|X509 veren sertifika depoları için küme sertifikaları; Adı = clusterIssuerCN; Değer = depoları virgülle ayrılmış listesi |

## <a name="securityclusterx509names"></a>Güvenlik/ClusterX509Names
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, varsayılan, Yok'tur|Dinamik| |

## <a name="securityservercertificateissuerstores"></a>Güvenlik/ServerCertificateIssuerStores
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|IssuerStoreKeyValueMap, varsayılan, Yok'tur |Dinamik|X509 veren sertifika depoları için sunucu sertifikaları; Adı = serverIssuerCN; Değer = depoları virgülle ayrılmış listesi |

## <a name="securityserverx509names"></a>Güvenlik/ServerX509Names
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, varsayılan, Yok'tur|Dinamik| |

## <a name="setup"></a>Kurulum
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|ContainerNetworkName|Varsayılan L dizesidir""| Statik |Bir kapsayıcı ağı kurma sırasında kullanmak için ağ adı.|
|ContainerNetworkSetup|bool, varsayılan FALSE olur| Statik |Bir kapsayıcı ağ ayarlanıp ayarlanmayacağını belirtir.|
|FabricDataRoot |Dize | İzin Verilmiyor |Service Fabric veri kök dizini. Varsayılan Azure d:\svcfab için |
|FabricLogRoot |Dize | İzin Verilmiyor |Service fabric günlük kök dizini. BT günlüklerine ve izlemelere yerleştirildiği budur. |
|NodesToBeRemoved|dize, varsayılan değer ""| Dinamik |Yapılandırma yükseltmesinin bir parçası kaldırılmalıdır düğümleri. (Yalnızca için tek başına dağıtımlarında)|
|ServiceRunAsAccountName |Dize | İzin Verilmiyor |Hesap adı altında doku ana bilgisayar hizmeti çalıştırmak için. |
|SkipFirewallConfiguration |Bool, varsayılan false | İzin Verilmiyor |Güvenlik Duvarı ayarlarını veya sistem tarafından ayarlanması gerekip gerekmediğini belirtir. Bu, yalnızca windows güvenlik duvarı kullanıyorsanız geçerlidir. Ardından üçüncü taraf güvenlik duvarları kullanıyorsanız, sistem ve uygulama kullanmak için bağlantı noktalarını açmanız gerekir |

## <a name="tokenvalidationservice"></a>TokenValidationService
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|Sağlayıcılar |Varsayılan "DSTS" dizesidir |Statik|Etkinleştirmek için belirteci doğrulama sağlayıcılarının virgülle ayrılmış listesi (geçerli sağlayıcıları şunlardır: DSTS; AAD). Şu anda herhangi bir anda yalnızca tek bir sağlayıcı etkinleştirilebilir. |

## <a name="traceetw"></a>İzleme/Etw
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|Düzey |Int, varsayılan 4'tür | Dinamik |Değerler 1, izleme etw düzey alabilir 2, 3, 4. Desteklenmesi için izleme düzeyi konumundaki 4 tutmanız gerekir |

## <a name="transactionalreplicator"></a>TransactionalReplicator
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|BatchAcknowledgementInterval | Zamanı saniye cinsinden 0,015 varsayılandır | Statik | TimeSpan saniye cinsinden belirtin. Onay yeniden göndermeden önce bir işlem aldıktan sonra çoğaltıcı bekleyeceği süreyi belirler. Bu süre içinde alınan diğer işlemleri çoğaltıcı verimini büyük olasılıkla azaltma ancak azalan ağ trafiğini -> tek bir iletide gönderilen kendi onayları sahip olur. |
|CheckpointThresholdInMB |Int, varsayılan 50'dir |Statik|Günlük kullanımı bu değeri aştığında bir denetim noktası başlatılmayacak. |
|InitialPrimaryReplicationQueueSize |Uint, varsayılan 64'tür. | Statik |Bu değer, birincil çoğaltma işlemleri tutar sıra için başlangıç boyutu tanımlar. 2'in üssü olması gerektiğini unutmayın.|
|InitialSecondaryReplicationQueueSize |Uint, varsayılan 64'tür. | Statik |Bu değer, ikincil çoğaltma işlemleri tutar sıra için başlangıç boyutu tanımlar. 2'in üssü olması gerektiğini unutmayın. |
|MaxAccumulatedBackupLogSizeInMB |Int, 800 varsayılandır |Statik|En büyük boyutunu (MB) verilen yedek günlüğü zinciri yedekleme günlüklerinde toplanan. Artımlı yedekleme birikmiş yedekleme günlüklerini bu boyuttan büyük olacak şekilde ilgili tam yedekleme sonrasındaki neden olacak bir yedekleme günlüğü oluşturur bir artımlı yedekleme istekleri başarısız olur. Böyle durumlarda, kullanıcı, tam yedek almak için gereklidir. |
|MaxCopyQueueSize |Uint, 16384 varsayılandır | Statik |Bu en yüksek değer çoğaltma işlemleri tutar sıranın ilk boyutu değeri tanımlar. 2'in üssü olması gerektiğini unutmayın. Çalışma zamanı sırasında bu boyutu işlem sırası büyürse birincil ve ikincil çoğaltıcılar kısıtlanacak. |
|MaxMetadataSizeInKB |Int, varsayılan 4'tür |İzin Verilmiyor|Günlük akışı meta verisi en büyük boyutu. |
|MaxPrimaryReplicationQueueMemorySize |Uint, varsayılan 0'dır | Statik |Birincil çoğaltma sırası bayt cinsinden en büyük değerini budur. |
|MaxPrimaryReplicationQueueSize |Uint, 8192 varsayılandır | Statik |Birincil çoğaltma sırası mevcut işlemlerinin maksimum sayısı budur. 2'in üssü olması gerektiğini unutmayın. |
|MaxRecordSizeInKB |Uint, varsayılan 1024'tür. |İzin Verilmiyor| Bir günlük akışı kaydı en büyük boyutu. |
|MaxReplicationMessageSize |Uint 52428800 varsayılandır | Statik | Çoğaltma işlemleri maksimum ileti boyutu. Varsayılan 50 MB'dir. |
|MaxSecondaryReplicationQueueMemorySize |Uint, varsayılan 0'dır | Statik |İkincil çoğaltma sırası bayt cinsinden en büyük değerini budur. |
|MaxSecondaryReplicationQueueSize |Uint, 16384 varsayılandır | Statik |Bu ikincil çoğaltma sırası mevcut işlemlerinin maksimum sayıdır. 2'in üssü olması gerektiğini unutmayın. |
|MaxWriteQueueDepthInKB |Int, varsayılan 0'dır |İzin Verilmiyor| Int maksimum çekirdek Günlükçü kilobayt cinsinden belirtildiği için bu çoğaltma ile ilişkili günlük kullanabilmeniz için sıra derinliği yazma. Bu değer en fazla çekirdek Günlükçü güncelleştirmeleri sırasında bekleyen bayt sayısı olur. Uygun bir değeri hesaplamak çekirdek Günlükçü için 0 veya 4 birden fazla olabilir. |
|MinLogSizeInMB |Int, varsayılan 0'dır |Statik|İşlem günlüğü minimum boyutu. Günlük bu ayarı aşağıda boyuta bir kesecek şekilde izin verilmiyor. Çoğaltıcı diğer ayarlara göre en küçük günlük boyutu belirleyecek 0 gösterir. Bu değer artırıldığında kısmi kopyalar ve artımlı yedeklemeler kesilmesine azaltıldığı ilgili günlük kayıtları olasılığını itibaren yapmanın olasılığı artar. |
|ReplicatorAddress |Varsayılan "localhost:0" dizesidir | Statik | Bir dizenin-'Windows Fabric çoğaltması tarafından gönderme/alma işlemleri için diğer çoğaltmaları ile bağlantı kurmak için kullanılan IP: BağlantıNoktası' formunda uç noktası. |
|SecondaryClearAcknowledgedOperations |Bool, varsayılan false | Statik |İkincil çoğaltma işlemleri (diske) birincil için onaylanan sonra kaldırdıysanız, denetimleri Bool. Ayarları için TRUE, yük devretme sonrasında çoğaltmaları yedeklemek yakalama sırasında ek disk okuma yeni birincil sonuçlanabilir. |
|SharedLogId |Dize |İzin Verilmiyor|Paylaşılan günlük tanımlayıcısı. Bu bir GUID ve paylaşılan her oturum için benzersiz olmalıdır. |
|SharedLogPath |Dize |İzin Verilmiyor|Paylaşılan günlük yolu. Bu değer boş ise varsayılan paylaşılan günlüğü kullanılır. |
|SlowApiMonitoringDuration |Zaman içinde varsayılan 300 saniyedir |Statik| Uyarı sistem durumu olayı tetiklenir önce API için süreyi belirtin.|

## <a name="transport"></a>Aktarım
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi** |**Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|ConnectionOpenTimeout|TimeSpan, Common::TimeSpan::FromSeconds(60) varsayılandır|Statik|TimeSpan saniye cinsinden belirtin. Bağlantı Kur hem gelen hem de kabul tarafında (güvenlik görüşmeleri güvenli modda dahil) için zaman aşımı |
|ResolveOption|dize, L "belirtilmeyen" varsayılandır|Statik|FQDN nasıl çözümleneceğini belirler.  Geçerli değerler "belirtilmeyen/IPv4/IPv6". |

## <a name="upgradeorchestrationservice"></a>UpgradeOrchestrationService
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|AutoupgradeEnabled | Bool, varsayılan değer true şeklindedir |Statik| Otomatik yoklama ve hedef durumu dosyasını temel alarak yükseltme eylemi. |
|MinReplicaSetSize |Int, varsayılan 0'dır |Statik |MinReplicaSetSize UpgradeOrchestrationService için.
|PlacementConstraints | dize, varsayılan değer "" |Statik| UpgradeOrchestrationService için PlacementConstraints. |
|QuorumLossWaitDuration | Zamanı saniye cinsinden MaxValue varsayılandır |Statik| TimeSpan saniye cinsinden belirtin. UpgradeOrchestrationService için QuorumLossWaitDuration. |
|ReplicaRestartWaitDuration | Zamanı saniye cinsinden, varsayılan değer 60 dakikadır|Statik| TimeSpan saniye cinsinden belirtin. UpgradeOrchestrationService için ReplicaRestartWaitDuration. |
|StandByReplicaKeepDuration | Zaman içinde varsayılan 60 saniyedir*24*7 dakika |Statik| TimeSpan saniye cinsinden belirtin. UpgradeOrchestrationService için StandByReplicaKeepDuration. |
|TargetReplicaSetSize |Int, varsayılan 0'dır |Statik |UpgradeOrchestrationService için TargetReplicaSetSize. |
|UpgradeApprovalRequired | Bool, varsayılan false | Statik|Devam etmeden önce yönetici onayı iste kod yükseltme yapmak için ayarlama. |

## <a name="upgradeservice"></a>UpgradeService
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa bir açıklama** |
| --- | --- | --- | --- |
|BaseUrl | dize, varsayılan değer "" |Statik|UpgradeService BaseUrl. |
|ClusterId | dize, varsayılan değer "" |Statik|UpgradeService ClusterId. |
|CoordinatorType | Varsayılan "WUTest" dizesidir|İzin Verilmiyor|UpgradeService için CoordinatorType. |
|MinReplicaSetSize | Int, varsayılan 2'dir |İzin Verilmiyor| MinReplicaSetSize UpgradeService için. |
|OnlyBaseUpgrade | Bool, varsayılan false |Dinamik|UpgradeService OnlyBaseUpgrade. |
|PlacementConstraints |dize, varsayılan değer "" |İzin Verilmiyor|Yükseltme hizmeti için PlacementConstraints. |
|TargetReplicaSetSize | Int, varsayılan 3. |İzin Verilmiyor| UpgradeService için TargetReplicaSetSize. |
|TestCabFolder | dize, varsayılan değer "" |Statik| UpgradeService TestCabFolder. |
|X509FindType | dize, varsayılan değer ""|Dinamik| UpgradeService X509FindType. |
|X509FindValue | dize, varsayılan değer "" |Dinamik| UpgradeService X509FindValue. |
|X509SecondaryFindValue | dize, varsayılan değer "" |Dinamik| UpgradeService X509SecondaryFindValue. |
|X509StoreLocation | dize, varsayılan değer "" |Dinamik| UpgradeService X509StoreLocation. |
|X509StoreName | dize, varsayılan değer "Benim"|Dinamik|UpgradeService X509StoreName. |

## <a name="next-steps"></a>Sonraki adımlar
Küme yönetimi hakkında daha fazla bilgi için bu makaleler okuyun:

[Eklemek için değil, UTC'ye, Azure kümenizden sertifikaları kaldırın ](service-fabric-cluster-security-update-certs-azure.md) 

