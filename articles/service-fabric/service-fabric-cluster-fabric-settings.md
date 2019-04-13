---
title: Azure Service Fabric küme ayarlarını değiştir | Microsoft Docs
description: Bu makalede, yapı ayarları ve özelleştirebileceğiniz fabric yükseltme ilkeleri açıklanır.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 7ced36bf-bd3f-474f-a03a-6ebdbc9677e2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/10/2019
ms.author: aljo
ms.openlocfilehash: 46c9b37e9bb8613b34dea6705320f5689eeb51d8
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59526546"
---
# <a name="customize-service-fabric-cluster-settings"></a>Service Fabric küme ayarlarını özelleştirme
Bu makalede, Service Fabric kümenizin özelleştirebileceğiniz çeşitli yapı ayarları açıklanır. Azure'da barındırılan kümeler için ayarları aracılığıyla özelleştirebilirsiniz [Azure portalında](https://portal.azure.com) veya bir Azure Resource Manager şablonu kullanarak. Daha fazla bilgi için [Azure kümesine yapılandırmasını yükseltme](service-fabric-cluster-config-upgrade-azure.md). Tek başına kümeler için ayarlarını güncelleştirerek özelleştirdiğiniz *ClusterConfig.json* dosyası ve bir yapılandırmasını gerçekleştirmek kümenizde yükseltin. Daha fazla bilgi için [tek başına küme yapılandırmasını yükseltme](service-fabric-cluster-config-upgrade-windows-server.md).

Üç farklı yükseltme ilkesi vardır:

- **Dinamik** – dinamik bir yapılandırmada değişiklik Service Fabric işlemleri veya, hizmet ana bilgisayarı işlemlerinin tüm işlem yeniden başlatmalar neden olmaz. 
- **Statik** – statik bir yapılandırmada değişiklik değişiklik kullanmak yeniden başlatmak Service Fabric düğüm neden olur. Düğümler üzerinde hizmetleri yeniden başlatılır.
- **Noktayla** – bu ayarlar değiştirilemez. Bu ayarları gerektiren küme yok edilecek değiştirme ve yeni bir kümesi. 

Bir liste verilmiştir dokusu özelleştirebileceğiniz, ayarları bölümü tarafından düzenlenir.

## <a name="applicationgatewayhttp"></a>ApplicationGateway/Http

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|ApplicationCertificateValidationPolicy|dize, "None" varsayılan|Statik| Bu, sunucu sertifikasının doğrulamaz; İstek başarılı. Ters proxy güvenebileceği uzak sertifikaları parmak izleriyle virgülle ayrılmış listesi için yapılandırma ServiceCertificateThumbprints bakın. Ters proxy güvenebileceği uzak sertifikalardaki konu adı ve verenin parmak izi için yapılandırma ServiceCommonNameAndIssuer bakın. Daha fazla bilgi için bkz. [ters Ara sunucu güvenli bağlantı](service-fabric-reverseproxy-configure-secure-communication.md#secure-connection-establishment-between-the-reverse-proxy-and-services). |
|BodyChunkSize |Uint 16384 varsayılandır |Dinamik| Öbek gövdesini okumak için kullanılan bayt cinsinden boyutunu vermektedir. |
|CrlCheckingFlag|uint, varsayılan 0x40000000 olduğu |Dinamik| Uygulama/hizmet sertifika zinciri doğrulaması bayrakları; Örneğin CRL 0x10000000 denetleme CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN 0x40000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT 0x80000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ONLY ayarı 0 devre dışı bırakır CRL denetimi tam listesini desteklenen değerler tarafından CertGetCertificateChain, CertOpenStore belgelenmiştir: https://msdn.microsoft.com/library/windows/desktop/aa376078(v=vs.85).aspx  |
|DefaultHttpRequestTimeout |Saniye cinsinden süre. Varsayılan 120'dir |Dinamik|Saniye cinsinden zaman aralığı belirtin.  Http uygulama ağ geçidi işlenmekte olan http isteği için varsayılan isteği zaman aşımı sağlar. |
|ForwardClientCertificate|bool, varsayılan FALSE olur.|Dinamik|Ne zaman yanlış, ters proxy kümesine için istemci sertifikası istemez. Doğru geriye doğru ara sunucu kümesine SSL el sıkışması sırasında için istemci sertifikası isteyin ve base64 olarak kodlanmış iletmek PEM biçimi dize X istemci Certificate.The hizmeti adlı bir üst bilgi hizmeti isteği uygun durum koduyla başarısız olabilir Sertifika verileri inceleyerek sonra. Bu true ise ve istemci bir sertifika sunması değil, ters proxy, boş bir üst bilgi iletmek ve durumu işlemek service gerisini halleder. Ters proxy saydam bir katman olarak görür. Daha fazla bilgi için bkz. [istemci sertifikası kimlik doğrulamasını ayarlama](service-fabric-reverseproxy-configure-secure-communication.md#setting-up-client-certificate-authentication-through-the-reverse-proxy). |
|GatewayAuthCredentialType |dize, "None" varsayılan |Statik| Http uygulama ağ geçidi uç noktası geçerli değerlere kullanılacak güvenlik kimlik bilgileri türünü gösterir "hiçbiri / X 509. |
|GatewayX509CertificateFindType |"FindByThumbprint" varsayılan bir dize ise |Dinamik| GatewayX509CertificateStoreName desteklenen değeri tarafından belirtilen deposundaki sertifikayı aramak nasıl gösterir: FindByThumbprint; FindBySubjectName. |
|GatewayX509CertificateFindValue | Varsayılan bir dize ise "" |Dinamik| Http uygulama ağ geçidi sertifikası bulmak için kullanılan arama filtre değeri. Bu sertifika, https uç noktasında yapılandırılmış ve hizmetler tarafından gerekli olursa uygulamanın kimliğini doğrulamak için de kullanılabilir. FindValue ilk baktığı; ve yoksa; FindValueSecondary aranır. |
|GatewayX509CertificateFindValueSecondary | Varsayılan bir dize ise "" |Dinamik|Http uygulama ağ geçidi sertifikası bulmak için kullanılan arama filtre değeri. Bu sertifika, https uç noktasında yapılandırılmış ve hizmetler tarafından gerekli olursa uygulamanın kimliğini doğrulamak için de kullanılabilir. FindValue ilk baktığı; ve yoksa; FindValueSecondary aranır.|
|GatewayX509CertificateStoreName |Varsayılan bir dize ise "My" |Dinamik| Http uygulama ağ geçidi sertifikasını içeren X.509 Sertifika deposunun adı. |
|HttpRequestConnectTimeout|Zaman aralığı, Common::TimeSpan::FromSeconds(5) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin.  Bağlantı zaman aşımı, http isteklerini http uygulama ağ geçidinden gönderilmesini sağlar.  |
|IgnoreCrlOfflineError|bool, varsayılan true'dur.|Dinamik|Uygulama/hizmet sertifika doğrulama için CRL çevrimdışı hatasını görmezden Gel verilmeyeceğini belirtir. |
|IsEnabled |Bool, varsayılan değer false'tur |Statik| HttpApplicationGateway etkinleştirir/devre dışı bırakır. HttpApplicationGateway varsayılan olarak devre dışıdır ve bu yapılandırma etkinleştirmek için ayarlanması gerekir. |
|NumberOfParallelOperations | Uint, varsayılan 5000'dir |Statik|Http sunucu kuyruğa gönderilecek okuma sayısı. Bu, HttpGateway tarafından karşılanabileceğini eş zamanlı istek sayısını denetler. |
|RemoveServiceResponseHeaders|"tarih; varsayılan bir dize ise Sunucu"|Statik|Noktalı virgül / virgülle ayrılmış hizmeti yanıtı; kaldırılacak yanıt üstbilgilerinin listesi istemciye iletmeden önce. Boş dize olarak ayarlanmışsa; hizmet tarafından döndürülen tüm üstbilgileri geçirin-olduğu. yani Tarih ve sunucu üzerine yazma |
|ResolveServiceBackoffInterval |Zamanı saniye cinsinden varsayılan 5'tir |Dinamik|Saniye cinsinden zaman aralığı belirtin.  Başarısız bir yeniden denemeden önce varsayılan geri alma aralığı çözmek verir işlemi hizmet. |
|SecureOnlyMode|bool, varsayılan FALSE olur.|Dinamik| SecureOnlyMode: true: Ters Proxy, yalnızca güvenli uç noktalarını yayımlama hizmetlere iletir. false: Ters Proxy istekleri güvenli/güvenli olmayan uç noktalarına iletebilir. Daha fazla bilgi için bkz. [ters Ara sunucu uç noktası seçim mantığını](service-fabric-reverseproxy-configure-secure-communication.md#endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints).  |
|ServiceCertificateThumbprints|Varsayılan bir dize ise ""|Dinamik|Ters proxy güvenebileceği uzak sertifikaları parmak izleriyle virgülle ayrılmış listesi. Daha fazla bilgi için bkz. [ters Ara sunucu güvenli bağlantı](service-fabric-reverseproxy-configure-secure-communication.md#secure-connection-establishment-between-the-reverse-proxy-and-services). |

## <a name="applicationgatewayhttpservicecommonnameandissuer"></a>Http/Applicationgateway'inin/ServiceCommonNameAndIssuer

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, varsayılan, Yok'tur|Dinamik| Konu adı ve verenin parmak izi ters proxy güvenebileceği uzak sertifikaları. Daha fazla bilgi için bkz. [ters Ara sunucu güvenli bağlantı](service-fabric-reverseproxy-configure-secure-communication.md#secure-connection-establishment-between-the-reverse-proxy-and-services). |

## <a name="backuprestoreservice"></a>BackupRestoreService

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|MinReplicaSetSize|int, varsayılan 0'dır|Statik|MinReplicaSetSize BackupRestoreService için |
|PlacementConstraints|Varsayılan bir dize ise ""|Statik|  PlacementConstraints BackupRestore hizmeti |
|SecretEncryptionCertThumbprint|Varsayılan bir dize ise ""|Dinamik|Gizli şifreleme X509 sertifikanın parmak izi |
|SecretEncryptionCertX509StoreName|Varsayılan bir dize ise "My"|   Dinamik|    Bu, şifreleme ve şifre çözme yedeklemesini geri yükleme hizmeti tarafından kullanılan şifresini çözmeyi deposu kimlik bilgilerini şifrelemek için kullanılan kimlik bilgileri adı, X.509 Sertifika deposunun için kullanılacak sertifikayı belirtir |
|TargetReplicaSetSize|int, varsayılan 0'dır|Statik| TargetReplicaSetSize BackupRestoreService için |

## <a name="clustermanager"></a>ClusterManager

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|Enabledefaultservicesupgrade özelliğini | Bool, varsayılan değer false'tur |Dinamik|Uygulama yükseltme sırasında yükseltme varsayılan hizmetleri etkinleştirin. Varsayılan hizmet açıklamaları, yükseltme sonrasında üzerine yazılır. |
|FabricUpgradeHealthCheckInterval |Zaman içinde varsayılan 60 saniyedir |Dinamik|İzlenen bir Fabric yükseltmesi sırasında sıklığı sistem durumunu denetleyin |
|FabricUpgradeStatusPollInterval |Zaman içinde varsayılan 60 saniyedir |Dinamik|Fabric yükseltme durumu için yoklama sıklığı. Bu değer herhangi bir GetFabricUpgradeProgress çağrısına güncelleştirmesi oranını belirler. |
|ImageBuilderTimeoutBuffer |Zamanı saniye cinsinden varsayılan 3'tür |Dinamik|Saniye cinsinden zaman aralığı belirtin. İstemciye döndürülecek belirli bir zaman aşımı hataları görüntü oluşturucusu için izin verilecek süre miktarı. Bu arabellek çok küçük ardından istemci önce sunucu zaman aşımına uğrar ve bir genel zaman aşımı hatası alır. |
|InfrastructureTaskHealthCheckRetryTimeout | Zaman içinde varsayılan 60 saniyedir |Dinamik|Saniye cinsinden zaman aralığı belirtin. Yeniden deneniyor harcadığınız süreyi Altyapı görevi sonrası işlerken sistem durumu denetimleri başarısız oldu. Başarılı sistem durumu denetimi gözleme Bu zamanlayıcı sıfırlar. |
|InfrastructureTaskHealthCheckStableDuration | Zamanı saniye cinsinden varsayılan 0'dır|Dinamik| Saniye cinsinden zaman aralığı belirtin. Altyapı bir görevin son işlemeden önce art arda başarılı sistem durumu denetimleri gözlemlemek için süreyi başarıyla tamamlanır. Başarısız sistem durumu denetimi gözleme Bu zamanlayıcı sıfırlar. |
|InfrastructureTaskHealthCheckWaitDuration |Zamanı saniye cinsinden varsayılan 0'dır|Dinamik| Saniye cinsinden zaman aralığı belirtin. Bir altyapı görevi sonrası işledikten sonra sistem durumu başlatmadan önce beklenecek zaman miktarını denetler. |
|InfrastructureTaskProcessingInterval | Zamanı saniye cinsinden varsayılan 10'dur |Dinamik|Saniye cinsinden zaman aralığı belirtin. Durum makinesi işleme Altyapı görevi tarafından kullanılan işleme aralığı. |
|MaxCommunicationTimeout |Zaman 600 saniye cinsinden varsayılandır |Dinamik|Saniye cinsinden zaman aralığı belirtin. ClusterManager ve diğer sistem arasındaki dahili iletişim için en uzun zaman aşımı (örn; Hizmetleri Adlandırma Hizmeti; "Yük Devretme Yöneticisi'ni ve VS.). Bu zaman aşımı (olabilir gibi birden fazla iletinin her istemci işlemi için sistem bileşenleri arasında) genel MaxOperationTimeout küçük olmalıdır. |
|MaxDataMigrationTimeout |Zaman 600 saniye cinsinden varsayılandır |Dinamik|Saniye cinsinden zaman aralığı belirtin. Fabric yükseltmesi gerçekleştikten sonra veriler geçiş kurtarma işlemleri için en uzun zaman aşımı. |
|MaxOperationRetryDelay |Zamanı saniye cinsinden varsayılan 5'tir|Dinamik| Saniye cinsinden zaman aralığı belirtin. Hataları karşılaştığında iç yeniden denemeler için en büyük gecikme. |
|MaxOperationTimeout |Zamanı saniye cinsinden MaxValue varsayılandır |Dinamik| Saniye cinsinden zaman aralığı belirtin. Dahili olarak ClusterManager işlemleri işlemek için maksimum genel zaman aşımı. |
|MaxTimeoutRetryBuffer | Zaman 600 saniye cinsinden varsayılandır |Dinamik|Saniye cinsinden zaman aralığı belirtin. Dahili olarak zaman aşımı nedeniyle denerken en fazla işlem zaman aşımı `<Original Time out> + <MaxTimeoutRetryBuffer>`. Ek zaman aşımı MinOperationTimeout artışlarla eklenir. |
|MinOperationTimeout | Zaman içinde varsayılan 60 saniyedir |Dinamik|Saniye cinsinden zaman aralığı belirtin. Dahili olarak ClusterManager işlemleri işlemek için en düşük genel zaman aşımı. |
|MinReplicaSetSize |Int, varsayılan 3'tür |İzin Verilmiyor|MinReplicaSetSize ClusterManager için. |
|PlacementConstraints | Varsayılan bir dize ise "" |İzin Verilmiyor|PlacementConstraints ClusterManager için. |
|QuorumLossWaitDuration |Zamanı saniye cinsinden MaxValue varsayılandır |İzin Verilmiyor| Saniye cinsinden zaman aralığı belirtin. QuorumLossWaitDuration ClusterManager için. |
|ReplicaRestartWaitDuration |Zamanı saniye cinsinden (60,0 * 30) varsayılandır|İzin Verilmiyor|Saniye cinsinden zaman aralığı belirtin. ReplicaRestartWaitDuration ClusterManager için. |
|ReplicaSetCheckTimeoutRollbackOverride |Zamanı saniye cinsinden varsayılan 1200'dür |Dinamik| Saniye cinsinden zaman aralığı belirtin. ReplicaSetCheckTimeout DWORD maksimum değerine ayarlanırsa, Daha sonra geri alma amacıyla bu yapılandırma değeri ile geçersiz kılındı. Sarma için kullanılan değer hiçbir zaman geçersiz kılınır. |
|SkipRollbackUpdateDefaultService | Bool, varsayılan değer false'tur |Dinamik|CM geri döndürülüyor güncelleştirilmiş varsayılan hizmetler uygulama yükseltme geri alma sırasında atlanacak. |
|StandByReplicaKeepDuration | Zamanı saniye cinsinden varsayılan (3600.0 * 2) gösterilir.|İzin Verilmiyor|Saniye cinsinden zaman aralığı belirtin. StandByReplicaKeepDuration ClusterManager için. |
|TargetReplicaSetSize |Int, varsayılan 7'dir |İzin Verilmiyor|TargetReplicaSetSize ClusterManager için. |
|UpgradeHealthCheckInterval |Zaman içinde varsayılan 60 saniyedir |Dinamik|İzlenen uygulama yükseltmeleri sırasında sıklığı sistem durumunu denetler |
|UpgradeStatusPollInterval |Zaman içinde varsayılan 60 saniyedir |Dinamik|Uygulama yükseltme durumu için yoklama sıklığı. Bu değer herhangi bir GetApplicationUpgradeProgress çağrısına güncelleştirmesi oranını belirler. |

## <a name="common"></a>Common

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PerfMonitorInterval |Zamanı saniye olarak varsayılan 1'dir |Dinamik|Saniye cinsinden zaman aralığı belirtin. Performans izleme aralığı. Ayarı 0 veya negatif bir değer için izlemeyi devre dışı bırakır. |

## <a name="defragmentationemptynodedistributionpolicy"></a>DefragmentationEmptyNodeDistributionPolicy
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|KeyIntegerValueMap, varsayılan, Yok'tur|Dinamik|Düğüm boşaltma, ilke birleştirme izleyen belirtir. Belirli bir ölçüm için 0 SF düğümleri eşit UD ve Fd'ler birleştirme denemesi gerektiğini gösterir; 1, yalnızca düğümlerin birleştirilmesi gösterir |

## <a name="defragmentationmetrics"></a>DefragmentationMetrics
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|KeyBoolValueMap, varsayılan, Yok'tur|Dinamik|Birleştirme ve Yük Dengeleme için kullanılması gereken ölçüm kümesini belirler. |

## <a name="defragmentationmetricspercentornumberofemptynodestriggeringthreshold"></a>DefragmentationMetricsPercentOrNumberOfEmptyNodesTriggeringThreshold
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, varsayılan, Yok'tur|Dinamik|Küme ya da yüzde aralığı belirterek birleştirilmiş göz önünde bulundurmanız gereken boş düğüm sayısını belirler [0.0-1.0) ya da boş düğüm sayısını sayı > = 1,0 |

## <a name="diagnostics"></a>Tanılama

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|AppDiagnosticStoreAccessRequiresImpersonation |Bool, varsayılan değer true şeklindedir | Dinamik |Olup olmadığını tanılama erişme adına uygulamayı depoladığında kimliğe bürünme gereklidir. |
|AppEtwTraceDeletionAgeInDays |Int, varsayılan 3'tür | Dinamik |Eski ETL dosyalarını içeren uygulama ETW izlemelerini sonra silmemiz gün sayısı. |
|ApplicationLogsFormatVersion |int, varsayılan 0'dır | Dinamik |Uygulama için sürüm biçimi günlüğe kaydeder. Desteklenen değerler şunlardır: 0 ve 1. Sürüm 1, sürüm 0 ETW olay kaydının daha fazla alan içerir. |
|Lclusterıd |String | Dinamik |Küme benzersiz kimliği. Bu, küme oluşturulduğunda oluşturulur. |
|ConsumerInstances |String | Dinamik |DCA tüketici örneklerinin listesi. |
|DiskFullSafetySpaceInMB |Int, varsayılan 1024'tür. | Dinamik |Kalan disk alanı DCA tarafından kullanımdan korumak için MB cinsinden. |
|EnableCircularTraceSession |Bool, varsayılan değer false'tur | Statik |Bayrak döngüsel izleme oturumlarını kullanılıp kullanılmayacağını belirtir. |
|EnableTelemetry |Bool, varsayılan değer true şeklindedir | Dinamik |Bu, etkinleştirme veya devre dışı telemetri geçiyor. |
|MaxDiskQuotaInMB |Int, 65536 varsayılandır | Dinamik |Disk kotası'Windows Fabric MB günlük dosyalarında. |
|ProducerInstances |String | Dinamik |DCA üretici örnekleri listesi. |

## <a name="dnsservice"></a>DnsService
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|EnablePartitionedQuery|bool, varsayılan FALSE olur.|Statik|Bölümlenmiş Hizmetleri için DNS sorgularını desteğini etkinleştirmek için bayrak. Bu özellik varsayılan olarak kapalıdır. Daha fazla bilgi için [Service Fabric DNS hizmeti.](service-fabric-dnsservice.md)|
|Instancecount|int, varsayılan -1'dir|Statik|Varsayılan değer DnsService her düğüm üzerinde çalıştığı anlamına gelir. -1'dir. OneBox bu DnsService, iyi bilinen bağlantı noktası 53'ü kullanır, aynı makinede birden fazla örneği olamaz bu nedenle bu yana 1 olarak ayarlanması gerekir.|
|IsEnabled|bool, varsayılan FALSE olur.|Statik|DnsService etkinleştirir/devre dışı bırakır. DnsService, varsayılan olarak devre dışıdır ve bu yapılandırma etkinleştirmek için ayarlanması gerekir. |
|PartitionPrefix|Varsayılan bir dize ise "--"|Statik|Bölümlenmiş Hizmetleri için DNS sorgularını bölüm önek dizesi değeri kontrol eder. Değer: <ul><li>Bir DNS sorgusunun bir parçası olacağı RFC uyumlu olmalıdır.</li><li>Bir nokta içermemelidir '.' gibi nokta DNS soneki davranışı ile uğratır.</li><li>5 karakterden uzun olmamalıdır.</li><li>Boş bir dize olamaz.</li><li>PartitionPrefix ayarı geçersiz kılınan sonra PartitionSuffix kılınmalıdır ve tersi.</li></ul>Daha fazla bilgi için [Service Fabric DNS hizmeti.](service-fabric-dnsservice.md).|
|PartitionSuffix|Varsayılan bir dize ise ""|Statik|Bölümlenmiş Hizmetleri için DNS sorgularını bölüm sonek dizesi değeri kontrol eder. Değer: <ul><li>Bir DNS sorgusunun bir parçası olacağı RFC uyumlu olmalıdır.</li><li>Bir nokta içermemelidir '.' gibi nokta DNS soneki davranışı ile uğratır.</li><li>5 karakterden uzun olmamalıdır.</li><li>PartitionPrefix ayarı geçersiz kılınan sonra PartitionSuffix kılınmalıdır ve tersi.</li></ul>Daha fazla bilgi için [Service Fabric DNS hizmeti.](service-fabric-dnsservice.md). |

## <a name="eventstore"></a>EventStore

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|MinReplicaSetSize|int, varsayılan 0'dır|Statik|Eventstore'a hizmeti MinReplicaSetSize |
|PlacementConstraints|Varsayılan bir dize ise ""|Statik|  Eventstore'a hizmeti PlacementConstraints |
|TargetReplicaSetSize|int, varsayılan 0'dır|Statik| Eventstore'a hizmeti TargetReplicaSetSize |

## <a name="fabricclient"></a>FabricClient

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|ConnectionInitializationTimeout |Zamanı saniye cinsinden varsayılan 2'dir |Dinamik|Saniye cinsinden zaman aralığı belirtin. Bağlantı zaman aşımı aralığı için her zaman istemci, ağ geçidine bir bağlantı açmayı dener.|
|HealthOperationTimeout |Zamanı saniye cinsinden varsayılan 120'dir |Dinamik|Saniye cinsinden zaman aralığı belirtin. Sistem Durumu Yöneticisi gönderilen rapor mesaj zaman aşımı süresi. |
|HealthReportRetrySendInterval |Zamanı saniye cinsinden varsayılan 30, en az 1. |Dinamik|Saniye cinsinden zaman aralığı belirtin. Başlangıçtan birikmiş sistem durumu raporlama bileşenini beşe aralığı için sistem durumu Yöneticisi bildirir. |
|HealthReportSendInterval |Zamanı saniye cinsinden varsayılan 30'dur |Dinamik|Saniye cinsinden zaman aralığı belirtin. Raporlama aralığı, bileşen sistem durumu Yöneticisi birikmiş sistem durumu raporu gönderir. |
|KeepAliveIntervalInSeconds |Int, varsayılan 20'dir |Statik|Aralığı FabricClient taşıma için ağ geçidi etkin tutma iletileri gönderir. 0 için; keepAlive devre dışı bırakıldı. Bir pozitif değer olmalıdır. |
|MaxFileSenderThreads |Uint, varsayılan 10'dur |Statik|Paralel olarak aktarılan dosyaların maksimum sayısı. |
|NodeAddresses |Varsayılan bir dize ise "" |Statik|Adlandırma Hizmeti ile iletişim kurmak için kullanılan, farklı düğümlerde adresleri (bağlantı dizeleri) koleksiyonudur. Başlangıçta istemci, rastgele adreslerden birini seçerek bağlanır. Birden fazla bağlantı dizesi tarafından sağlanan ve iletişim veya zaman aşımı hatası nedeniyle bağlantı başarısız olursa; sonraki adresi sıralı olarak kullanmak için istemci geçiş yapar. Adlandırma Hizmeti bölümü yeniden deneme semantiği hakkında ayrıntılı bilgi için'yeniden dene adresi bakın. |
|PartitionLocationCacheLimit |Int, varsayılan 100000'dir |Statik|(Sınırsız için 0 olarak ayarlayın) hizmeti çözümlemesi için önbelleğe alınmış bir bölüm sayısı. |
|RetryBackoffInterval |Zamanı saniye cinsinden varsayılan 3'tür |Dinamik|Saniye cinsinden zaman aralığı belirtin. İşlemi yeniden denemeden önce geri alma aralığı. |
|ServiceChangePollInterval |Zamanı saniye cinsinden varsayılan 120'dir |Dinamik|Saniye cinsinden zaman aralığı belirtin. Ağ geçidi için kayıtlı hizmet değişiklik bildirimleri geri çağırmalar için istemciden hizmet için ardışık yoklamaları arasındaki zaman aralığını değiştirir. |

## <a name="fabrichost"></a>{Fabrichost kaynağından

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|ActivationMaxFailureCount |Int, varsayılan 10'dur |Dinamik|Bu, sistem vazgeçmeden önce etkinleştirilemedi yeniden denenecek en yüksek sayıdır. |
|ActivationMaxRetryInterval |Süresi 300 saniye cinsinden varsayılandır |Dinamik|Saniye cinsinden zaman aralığı belirtin. Etkinleştirme için en fazla yeniden deneme aralığı. Her sürekli hata durumunda yeniden deneme aralığı (ActivationMaxRetryInterval; Min hesaplanır. Sürekli hata sayısı * ActivationRetryBackoffInterval). |
|ActivationRetryBackoffInterval |Zamanı saniye cinsinden varsayılan 5'tir |Dinamik|Saniye cinsinden zaman aralığı belirtin. Geri alma aralığı her etkinleştirme hatası; her sürekli etkinleştirme hatasında MaxActivationFailureCount kadar etkinleştirme için sistemin yeniden denenecek. Yeniden deneme aralığı her deneyin, sürekli bir etkinleştirme hatası çarpımını ve geri alma etkinleştirme aralığı ' dir. |
|EnableRestartManagement |Bool, varsayılan değer false'tur |Dinamik|Bu sunucunun yeniden başlatılması etkinleştirmektir. |
|EnableServiceFabricAutomaticUpdates |Bool, varsayılan değer false'tur |Dinamik|Bu doku otomatik güncelleştirme Windows Update aracılığıyla etkinleştirmektir. |
|EnableServiceFabricBaseUpgrade |Bool, varsayılan değer false'tur |Dinamik|Bu sunucu için temel güncelleştirme etkinleştirmektir. |
|StartTimeout |Süresi 300 saniye cinsinden varsayılandır |Dinamik|Saniye cinsinden zaman aralığı belirtin. Fabricactivationmanager başlangıç zaman aşımına uğrayabilir. |
|StopTimeout |Süresi 300 saniye cinsinden varsayılandır |Dinamik|Saniye cinsinden zaman aralığı belirtin. Barındırılan hizmet etkinleştirme zaman aşımı; devre dışı bırakma ve yükseltme. |

## <a name="fabricnode"></a>FabricNode

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|ClientAuthX509FindType |"FindByThumbprint" varsayılan bir dize ise |Dinamik|ClientAuthX509StoreName desteklenen değeri tarafından belirtilen deposundaki sertifikayı aramak nasıl gösterir: FindByThumbprint; FindBySubjectName. |
|ClientAuthX509FindValue |Varsayılan bir dize ise "" | Dinamik|Varsayılan Yönetici rolü FabricClient sertifikası bulmak için kullanılan arama filtre değeri. |
|ClientAuthX509FindValueSecondary |Varsayılan bir dize ise "" |Dinamik|Varsayılan Yönetici rolü FabricClient sertifikası bulmak için kullanılan arama filtre değeri. |
|ClientAuthX509StoreName |Varsayılan bir dize ise "My" |Dinamik|Varsayılan Yönetici rolü FabricClient sertifikası içeren X.509 Sertifika deposunun adı. |
|ClusterX509FindType |"FindByThumbprint" varsayılan bir dize ise |Dinamik|Küme sertifikası ClusterX509StoreName desteklenen değerleri tarafından belirtilen deposunda aranacak nasıl gösterir: "FindByThumbprint"; "FindBySubjectName" ile "FindBySubjectName"; birden çok eşleşme; olduğunda en son kullanma tarihi olan bir kullanılır. |
|ClusterX509FindValue |Varsayılan bir dize ise "" |Dinamik|Küme sertifikası bulmak için kullanılan arama filtre değeri. |
|ClusterX509FindValueSecondary |Varsayılan bir dize ise "" |Dinamik|Küme sertifikası bulmak için kullanılan arama filtre değeri. |
|ClusterX509StoreName |Varsayılan bir dize ise "My" |Dinamik|Küme içi iletişimin güvenliğini sağlamak için küme sertifikasını içeren X.509 Sertifika deposunun adı. |
|EndApplicationPortRange |int, varsayılan 0'dır |Statik|Barındırma alt sistemi tarafından yönetilen bir uygulama bağlantı noktası, Bitiş (Hayır dahil). EndpointFilteringEnabled barındırma de true ise gereklidir. |
|ServerAuthX509FindType |"FindByThumbprint" varsayılan bir dize ise |Dinamik|Sunucu sertifikası ServerAuthX509StoreName desteklenen değeri tarafından belirtilen deposunda aranacak nasıl gösterir: FindByThumbprint; FindBySubjectName. |
|ServerAuthX509FindValue |Varsayılan bir dize ise "" |Dinamik|Sertifikanızı bulmak için kullanılan arama filtre değeri. |
|ServerAuthX509FindValueSecondary |Varsayılan bir dize ise "" |Dinamik|Sertifikanızı bulmak için kullanılan arama filtre değeri. |
|ServerAuthX509StoreName |Varsayılan bir dize ise "My" |Dinamik|Başlangıç hizmeti için sunucu sertifikasını içeren X.509 Sertifika deposunun adı. |
|StartApplicationPortRange |int, varsayılan 0'dır |Statik|Barındırma alt sistemi tarafından yönetilen bir uygulama bağlantı noktası başlangıcı. EndpointFilteringEnabled barındırma de true ise gereklidir. |
|StateTraceInterval |Süresi 300 saniye cinsinden varsayılandır |Statik|Saniye cinsinden zaman aralığı belirtin. Her düğümde ve FM/FMM düğümlerinde yukarı düğüm durumu izleme aralığı. |
|UserRoleClientX509FindType |"FindByThumbprint" varsayılan bir dize ise |Dinamik|UserRoleClientX509StoreName desteklenen değeri tarafından belirtilen deposundaki sertifikayı aramak nasıl gösterir: FindByThumbprint; FindBySubjectName. |
|UserRoleClientX509FindValue |Varsayılan bir dize ise "" |Dinamik|Varsayılan kullanıcı rolü FabricClient sertifikası bulmak için kullanılan arama filtre değeri. |
|UserRoleClientX509FindValueSecondary |Varsayılan bir dize ise "" |Dinamik|Varsayılan kullanıcı rolü FabricClient sertifikası bulmak için kullanılan arama filtre değeri. |
|UserRoleClientX509StoreName |Varsayılan bir dize ise "My" |Dinamik|Varsayılan kullanıcı rolü FabricClient sertifikası içeren X.509 Sertifika deposunun adı. |

## <a name="failovermanager"></a>FailoverManager

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|BuildReplicaTimeLimit|Zaman aralığı, Common::TimeSpan::FromSeconds(3600) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Durum bilgisi olan bir çoğaltma oluşturmak için zaman sınırı; bir uyarı sistem durumu raporu daha sonra başlatılacak |
|ClusterPauseThreshold|int, varsayılan 1.|Dinamik|Sistem düğümlerin sayısı bu değeri sonra yerleştirme giderseniz; Yük Dengeleme; ve yük devretme durdurulur. |
|CreateInstanceTimeLimit|Zaman aralığı, Common::TimeSpan::FromSeconds(300) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Durum bilgisi olmayan bir örneğini oluşturmak için zaman sınırı; bir uyarı sistem durumu raporu daha sonra başlatılacak |
|ExpectedClusterSize|int, varsayılan 1.|Dinamik|Kümenin ilk kez başlatıldığında; FM bunu diğer hizmetlere yerleştirme başlamadan önce Yukarı kendilerini bildirmek için birçok düğüm bekleyin; adlandırma gibi sistem hizmetleri dahil olmak üzere. Bu değeri artırmak başlamak için bir küme süresini artırır; aşırı yüklenerek gelen erken düğümleri ve ayrıca daha fazla düğüm çevrimiçi olması gibi gerekli olacak ek taşır, ancak önler. Bu değer genellikle bazı ilk küme boyutu küçük kesir olarak ayarlanması gerekir. |
|ExpectedNodeDeactivationDuration|Zaman aralığı, Common::TimeSpan::FromSeconds(60.0 * 30) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Beklenen süre içinde devre dışı bırakmayı tamamlamak bir düğüm için budur. |
|ExpectedNodeFabricUpgradeDuration|Zaman aralığı, Common::TimeSpan::FromSeconds(60.0 * 30) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Bu yükseltilecek bir düğüm için beklenen süre, Windows Fabric yükseltmesi sırasında. |
|ExpectedReplicaUpgradeDuration|Zaman aralığı, Common::TimeSpan::FromSeconds(60.0 * 30) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Uygulama yükseltme sırasında bir düğümde yükseltilecek tüm çoğaltmaları için beklenen süre budur. |
|IsSingletonReplicaMoveAllowedDuringUpgrade|bool, varsayılan true'dur.|Dinamik|İse true; ayarlanmıştır yükseltme sırasında taşımak için bir hedef çoğaltma kümesi boyutu 1 Çoğaltmalarla izin verilir. |
|MinReplicaSetSize|Int, varsayılan 3'tür|İzin Verilmiyor|FM için en küçük çoğaltma kümesi boyutu budur. Etkin FM çoğaltmaların sayısı bu değerin altına düşerse; kümeye kadar en az değişiklik FM reddeder çoğaltma en küçük sayısı, kurtarılır |
|PlacementConstraints|Varsayılan bir dize ise ""|İzin Verilmiyor|Yük Devretme Yöneticisi çoğaltmalar için herhangi bir yerleştirme kısıtlamaları |
|PlacementTimeLimit|Zaman aralığı, Common::TimeSpan::FromSeconds(600) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Hedef çoğaltma sayısının ulaşmak için zaman sınırı; bir uyarı sistem durumu raporu daha sonra başlatılacak |
|QuorumLossWaitDuration |Zamanı saniye cinsinden MaxValue varsayılandır |Dinamik|Saniye cinsinden zaman aralığı belirtin. Çekirdek kayıp durumda bir bölüme izin veriyoruz en fazla süre budur. Bölümü, bu süreden sonra hala çekirdek kaybına ise; Bölüm çekirdek kaybından aşağı çoğaltmaları kayıp olarak dikkate alarak kurtarılır. Bu olası veri kaybı doğurduğuna dikkat edin. |
|ReconfigurationTimeLimit|Zaman aralığı, Common::TimeSpan::FromSeconds(300) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Yeniden yapılandırma süresi sınırını; bir uyarı sistem durumu raporu daha sonra başlatılacak |
|ReplicaRestartWaitDuration|Zaman aralığı, Common::TimeSpan::FromSeconds(60.0 * 30) varsayılandır|İzin Verilmiyor|Saniye cinsinden zaman aralığı belirtin. FMService için ReplicaRestartWaitDuration budur. |
|StandByReplicaKeepDuration|Zaman aralığı, Common::TimeSpan::FromSeconds(3600.0 * 24 * 7) varsayılandır|İzin Verilmiyor|Saniye cinsinden zaman aralığı belirtin. FMService için StandByReplicaKeepDuration budur. |
|TargetReplicaSetSize|Int, varsayılan 7'dir|İzin Verilmiyor|Windows Fabric tutacaktır FM çoğaltmaları hedef sayısıdır. FM verilerin daha yüksek güvenilirlik daha yüksek bir sayı olur; küçük performansta düşüş ile. |
|UserMaxStandByReplicaCount |int, varsayılan 1. |Dinamik|Varsayılan en büyük kullanıcı Hizmetleri için sistem tutan StandBy çoğaltmalarının sayısı. |
|UserReplicaRestartWaitDuration |Zaman 60,0 * 30 saniye cinsinden varsayılandır |Dinamik|Saniye cinsinden zaman aralığı belirtin. Kalıcı çoğaltma ne zaman arıza; Windows Fabric çoğaltma (durumunun bir kopyasını gerektirecek) yeni değiştirme çoğaltma oluşturmadan önce geri gelmesi için bu süre bekler. |
|UserStandByReplicaKeepDuration |Zamanı saniye cinsinden 3600.0 * 24 * 7 varsayılandır |Dinamik|Saniye cinsinden zaman aralığı belirtin. Kalıcı çoğaltma olduğunuzda geri aşağı durumundan; zaten değiştirilmiş. Bu zamanlayıcı FM bekleme çoğaltma atılmadan önce ne kadar süreyle korur belirler. |

## <a name="faultanalysisservice"></a>FaultAnalysisService

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|CompletedActionKeepDurationInSeconds | Int, 604800 varsayılandır |Statik| Bu, yaklaşık bir terminal durumunda eylemleri tutmak için ne kadar. Bu da StoredActionCleanupIntervalInSeconds bağlıdır; bu yana temizleme işi, yalnızca o aralıkta yapılır. 604800 7 gündür. |
|DataLossCheckPollIntervalInSeconds|int, varsayılan 5'tir|Statik|Bu veri kaybı gerçekleşecek beklenirken sistemde gerçekleştirdiği denetimleri arasındaki süredir. Veri kaybı numarası dahili yineleme başına denetlenecek DataLossCheckWaitDurationInSeconds/bu sayısıdır. |
|DataLossCheckWaitDurationInSeconds|int, varsayılan 25'tir|Statik|Toplam süre miktarını; saniyeler içinde; Sistem veri kaybı gerçekleşecek olmasını bekler. StartPartitionDataLossAsync() API çağrıldığında dahili olarak kullanılır. |
|MinReplicaSetSize |int, varsayılan 0'dır |Statik|MinReplicaSetSize FaultAnalysisService için. |
|PlacementConstraints | Varsayılan bir dize ise ""|Statik| PlacementConstraints FaultAnalysisService için. |
|QuorumLossWaitDuration | Zamanı saniye cinsinden MaxValue varsayılandır |Statik|Saniye cinsinden zaman aralığı belirtin. QuorumLossWaitDuration FaultAnalysisService için. |
|ReplicaDropWaitDurationInSeconds|600 int, varsayılan olan|Statik|Bu parametre, veri kaybı API çağrıldığında kullanılır. Sonra Kaldır bırakılan bir çoğaltma için sistemin ne kadar süreyle bekleyeceği denetimleri çoğaltma dahili olarak çağrılır üzerinde. |
|ReplicaRestartWaitDuration |Zamanı saniye cinsinden varsayılan değer 60 dakikadır|Statik|Saniye cinsinden zaman aralığı belirtin. ReplicaRestartWaitDuration FaultAnalysisService için. |
|StandByReplicaKeepDuration| Zamanı saniye cinsinden varsayılan değer (60*24*7) dakika |Statik|Saniye cinsinden zaman aralığı belirtin. StandByReplicaKeepDuration FaultAnalysisService için. |
|StoredActionCleanupIntervalInSeconds | Int, 3600 varsayılandır |Statik|Deponun ne sıklıkta temizlenecektir budur. Yalnızca eylemleri bir terminal durumunda; Tamamlanan en az önce CompletedActionKeepDurationInSeconds olur ve kaldırıldı. |
|StoredChaosEventCleanupIntervalInSeconds | Int, 3600 varsayılandır |Statik|Bu depo için temizleme ne sıklıkta denetlenecektir, olay sayısı fazla 30000 Temizleme etkili olur. |
|TargetReplicaSetSize |int, varsayılan 0'dır |Statik|NOT_PLATFORM_UNIX_START TargetReplicaSetSize FaultAnalysisService için. |

## <a name="federation"></a>Federasyon

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|Kirasüresi |Zamanı saniye cinsinden varsayılan 30'dur |Dinamik|Bir düğüm ve komşuları kira sürer süresi. |
|LeaseDurationAcrossFaultDomain |Zamanı saniye cinsinden varsayılan 30'dur |Dinamik|Hata etki alanları arasında bir düğüm ve komşuları kira sürer süresi. |

## <a name="filestoreservice"></a>FileStoreService

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|AcceptChunkUpload|bool, varsayılan true'dur.|Dinamik|Dosya depolama hizmeti tabanlı öbek dosya karşıya yükleme kabul edip etmeyeceğini veya uygulama Paket Kopyala sırasında değil belirlemek için yapılandırma. |
|AnonymousAccessEnabled | Bool, varsayılan değer true şeklindedir |Statik|FileStoreService paylaşımlarına anonim erişim etkinleştir/devre dışı bırak. |
|CommonName1Ntlmx509CommonName|Varsayılan bir dize ise ""|Statik| Ortak adı X509 HMAC CommonName1NtlmPasswordSecret üzerinde NTLM kimlik doğrulaması kullanırken oluşturmak için kullanılan sertifika |
|CommonName1Ntlmx509StoreLocation|"LocalMachine" varsayılan bir dize ise|Statik|Depolama konumu X509 HMAC CommonName1NtlmPasswordSecret üzerinde NTLM kimlik doğrulaması kullanırken oluşturmak için kullanılan sertifika |
|CommonName1Ntlmx509StoreName|"MY" varsayılan bir dize ise| Statik|Depo adı X509 HMAC CommonName1NtlmPasswordSecret üzerinde NTLM kimlik doğrulaması kullanırken oluşturmak için kullanılan sertifika |
|CommonName2Ntlmx509CommonName|Varsayılan bir dize ise ""|Statik|Ortak adı X509 HMAC CommonName2NtlmPasswordSecret üzerinde NTLM kimlik doğrulaması kullanırken oluşturmak için kullanılan sertifika |
|CommonName2Ntlmx509StoreLocation|"LocalMachine" varsayılan bir dize ise| Statik|Depolama konumu X509 HMAC CommonName2NtlmPasswordSecret üzerinde NTLM kimlik doğrulaması kullanırken oluşturmak için kullanılan sertifika |
|CommonName2Ntlmx509StoreName|"MY" varsayılan bir dize ise|Statik| Depo adı X509 HMAC CommonName2NtlmPasswordSecret üzerinde NTLM kimlik doğrulaması kullanırken oluşturmak için kullanılan sertifika |
|CommonNameNtlmPasswordSecret|SecureString, Common::SecureString("") varsayılandır| Statik|NTLM kimlik doğrulaması kullanırken oluşturulan aynı parola çekirdek olarak kullanılan parola gizliliği |
|GenerateV1CommonNameAccount| bool, varsayılan true'dur.|Statik|Kullanıcı adı V1 oluşturma algoritması olan bir hesap oluşturulup oluşturulmayacağını belirtir. Service Fabric 6.1 sürümünde başlatılıyor; sürüm 2. nesil bir hesapla her zaman oluşturulur. V1 hesabı V2 nesil (önce 6.1) desteklemeyen sürümleri / için yükseltmeleri için gereklidir.|
|MaxCopyOperationThreads | Uint, varsayılan 0'dır |Dinamik| Bu ikincil paralel dosya sayısı, birincil sunucudan kopyalayabilirsiniz. '0' == çekirdek sayısı. |
|MaxFileOperationThreads | Uint, varsayılan 100'dür |Statik| Birincil FileOperations (kopyalama/taşıma) gerçekleştirmesine izin paralel iş parçacığı sayısı. '0' == çekirdek sayısı. |
|MaxRequestProcessingThreads | Uint, 200 varsayılandır |Statik|Paralel iş parçacığı birincil istekleri işlemek için izin verilen maksimum sayısı. '0' == çekirdek sayısı. |
|MaxSecondaryFileCopyFailureThreshold | Uint, varsayılan 25'tir|Dinamik|Dosya kopyalama yeniden deneme sayısı vazgeçmeden önce ikincil. |
|MaxStoreOperations | Uint, 4096 varsayılandır |Statik|Birincil izin paralel deposu işlem işlemlerinin maksimum sayısı. '0' == çekirdek sayısı. |
|NamingOperationTimeout |Zaman içinde varsayılan 60 saniyedir |Dinamik|Saniye cinsinden zaman aralığı belirtin. Adlandırma işlemi gerçekleştirmek için zaman aşımı. |
|PrimaryAccountNTLMPasswordSecret | Varsayılan SecureString, boştur |Statik| NTLM kimlik doğrulaması kullanırken oluşturulan aynı parola çekirdek olarak kullanılan parola gizli anahtarı. |
|PrimaryAccountNTLMX509StoreLocation | "LocalMachine" varsayılan bir dize ise|Statik| Depolama konumu X509 HMAC PrimaryAccountNTLMPasswordSecret üzerinde NTLM kimlik doğrulaması kullanırken oluşturmak için kullanılan sertifika. |
|PrimaryAccountNTLMX509StoreName | "MY" varsayılan bir dize ise|Statik| Depo adı X509 HMAC PrimaryAccountNTLMPasswordSecret üzerinde NTLM kimlik doğrulaması kullanırken oluşturmak için kullanılan sertifika. |
|PrimaryAccountNTLMX509Thumbprint | Varsayılan bir dize ise ""|Statik|Parmak izini X509 HMAC PrimaryAccountNTLMPasswordSecret üzerinde NTLM kimlik doğrulaması kullanırken oluşturmak için kullanılan sertifika. |
|PrimaryAccountType | Varsayılan bir dize ise "" |Statik|Birincil ACL FileStoreService için asıl AccountType paylaşır. |
|PrimaryAccountUserName | Varsayılan bir dize ise "" |Statik|Birincil hesap ACL uygulamak için sorumlu kullanıcı adı FileStoreService paylaşır. |
|PrimaryAccountUserPassword | Varsayılan SecureString, boştur |Statik|Asıl ACL için birincil hesap parolası FileStoreService paylaşır. |
|QueryOperationTimeout | Zaman içinde varsayılan 60 saniyedir |Dinamik|Saniye cinsinden zaman aralığı belirtin. Sorgu işlemi gerçekleştirmek için zaman aşımı. |
|SecondaryAccountNTLMPasswordSecret | Varsayılan SecureString, boştur |Statik| NTLM kimlik doğrulaması kullanırken oluşturulan aynı parola çekirdek olarak kullanılan parola gizli anahtarı. |
|SecondaryAccountNTLMX509StoreLocation | "LocalMachine" varsayılan bir dize ise |Statik|Depolama konumu X509 HMAC SecondaryAccountNTLMPasswordSecret üzerinde NTLM kimlik doğrulaması kullanırken oluşturmak için kullanılan sertifika. |
|SecondaryAccountNTLMX509StoreName | "MY" varsayılan bir dize ise |Statik|Depo adı X509 HMAC SecondaryAccountNTLMPasswordSecret üzerinde NTLM kimlik doğrulaması kullanırken oluşturmak için kullanılan sertifika. |
|SecondaryAccountNTLMX509Thumbprint | Varsayılan bir dize ise ""| Statik|Parmak izini X509 HMAC SecondaryAccountNTLMPasswordSecret üzerinde NTLM kimlik doğrulaması kullanırken oluşturmak için kullanılan sertifika. |
|SecondaryAccountType | Varsayılan bir dize ise ""|Statik| İkincil asıl ACL FileStoreService için AccountType paylaşır. |
|SecondaryAccountUserName | Varsayılan bir dize ise ""| Statik|İkincil hesap ACL uygulamak için sorumlu kullanıcı adı FileStoreService paylaşır. |
|SecondaryAccountUserPassword | Varsayılan SecureString, boştur |Statik|ACL sorumlusuna ikincil hesap parolasını FileStoreService paylaşır. |
|SecondaryFileCopyRetryDelayMilliseconds|uint, varsayılan 500'dür|Dinamik|Dosya kopyalama yeniden deneme gecikmesi (milisaniye cinsinden).|
|UseChunkContentInTransportMessage|bool, varsayılan true'dur.|Dinamik|Karşıya yükleme protokol v6.4 içinde sunulan yeni sürümü kullanmak için bayrak. Bu protokol sürümü, önceki sürümlerinde kullanılan SMB protokolü daha iyi performans sağlayan görüntü deposu için dosyaları karşıya yüklemek için service fabric aktarım kullanır. |

## <a name="healthmanager"></a>HealthManager

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|EnableApplicationTypeHealthEvaluation |Bool, varsayılan değer false'tur |Statik|Küme sistem durumu değerlendirme İlkesi: uygulama türü sistem durumu değerlendirmesi etkinleştirin. |
|MaxSuggestedNumberOfEntityHealthReports|Varsayılan Int, 500'dür |Dinamik|Sistem durumu sayısı İzleme'nın sistem durumu raporlama mantığını DB'dir tetiklenmeden önce bir varlık olabilir bildirir. Her sistem durumu varlık sistem durumu raporlarının sayısı nispeten küçük bir sayı olması beklenir. Rapor sayısı bu sayıyı aşması durumunda; İzleme'nın uygulama ile ilgili sorunlar olabilir. Varlık değerlendirildiğinde aracılığıyla bir uyarı sistem durumu raporu varlıkta çok fazla sayıda güvenlik raporları ile işaretlenir. |

## <a name="healthmanagerclusterhealthpolicy"></a>HealthManager/ClusterHealthPolicy

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|ConsiderWarningAsError |Bool, varsayılan değer false'tur |Statik|Küme sistem durumu değerlendirme İlkesi: uyarıları hata olarak kabul edilir. |
|MaxPercentUnhealthyApplications | int, varsayılan 0'dır |Statik|Küme sistem durumu değerlendirme İlkesi: iyi durumda olmayan uygulamalar en yüksek yüzdesi verilen kümenin iyi durumda olması için. |
|MaxPercentUnhealthyNodes | int, varsayılan 0'dır |Statik|Küme sistem durumu değerlendirme İlkesi: en yüksek yüzdesi iyi durumda olmayan düğümler, kümenin iyi durumda olması için izin. |

## <a name="healthmanagerclusterupgradehealthpolicy"></a>HealthManager/ClusterUpgradeHealthPolicy

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|MaxPercentDeltaUnhealthyNodes|Int, varsayılan 10'dur|Statik|Küme yükseltme sistem durumu değerlendirme İlkesi: iyi durumda olmayan delta düğümlerinin en yüksek yüzdesi verilen kümenin iyi durumda olması için |
|MaxPercentUpgradeDomainDeltaUnhealthyNodes|int, varsayılan 15'tir|Statik|Küme yükseltme sistem durumu değerlendirme İlkesi: izin kümenin iyi durumda olması için bir yükseltme etki alanındaki iyi durumda olmayan düğümler delta değeri en yüksek yüzdesi |

## <a name="hosting"></a>Barındırma

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|ActivationMaxFailureCount |Tam sayı, varsayılan 10'dur |Dinamik|Sistem yeniden deneme sayısı, vazgeçmeden önce etkinleştirilemedi |
|ActivationMaxRetryInterval |Süresi 300 saniye cinsinden varsayılandır |Dinamik|Her sürekli etkinleştirme hatasında ActivationMaxFailureCount kadar etkinleştirme için sistemin yeniden dener. ActivationMaxRetryInterval her etkinleştirme hatadan sonra yeniden deneme bekleme zaman aralığını belirtir. |
|ActivationRetryBackoffInterval |Zamanı saniye cinsinden varsayılan 5'tir |Dinamik|Geri alma aralığı her etkinleştirme hatasında; Her sürekli etkinleştirme hatasında MaxActivationFailureCount kadar etkinleştirme için sistemin yeniden dener. Yeniden deneme aralığı her deneyin, sürekli bir etkinleştirme hatası çarpımını ve geri alma etkinleştirme aralığı ' dir. |
|ActivationTimeout| Zaman aralığı, Common::TimeSpan::FromSeconds(180) varsayılandır|Dinamik| Saniye cinsinden zaman aralığı belirtin. Uygulama etkinleştirme zaman aşımı; devre dışı bırakma ve yükseltme. |
|ApplicationHostCloseTimeout| Zaman aralığı, Common::TimeSpan::FromSeconds(120) varsayılandır|Dinamik| Saniye cinsinden zaman aralığı belirtin. Yapı çıkış kendini etkinleştirilmiş bir işlemde algılandığında; Tüm çoğaltmalar kullanıcının ana (applicationhost) işleminde FabricRuntime kapatır. Kapatma işlemi zaman aşımı süresi olmasıdır. |
|ApplicationUpgradeTimeout| Zaman aralığı, Common::TimeSpan::FromSeconds(360) varsayılandır|Dinamik| Saniye cinsinden zaman aralığı belirtin. Uygulama yükseltmesi için zaman aşımı. Zaman aşımı "ActivationTimeout" dağıtıcı başarısız olur daha az ise. |
|ContainerServiceArguments|Varsayılan bir dize ise "-H 2375 -H npipe: / /"|Statik|Service Fabric (BT) docker Daemon programını yönetir (Win10 gibi windows istemci makinelerinde hariç). Bu yapılandırma, başlatma sırasında docker cinini geçirilmelidir özel bağımsız değişkenlerini belirtmek kullanıcı sağlar. Özel bağımsız değişkenler belirtildiğinde, Service Fabric geçirmeyin hiçbir bağımsız değişkeni Docker altyapısına dışında '--pidfile' bağımsız değişkeni. Bu nedenle kullanıcıların değil belirtmelisiniz '--pidfile' bağımsız değişkeni müşteri bağımsız bir parçası olarak. Ayrıca, özel bağımsız değişkenler docker daemon dinlediği varsayılan adı kanalda Windows üzerinde emin olmanız gerekir (veya UNIX etki alanı yuva Linux'ta) ile iletişim kurabilmesi Service Fabric için.|
|ContainerServiceLogFileMaxSizeInKb|int, varsayılan 32768 olduğu|Statik|Docker kapsayıcıları tarafından oluşturulan günlük dosyası maksimum dosya boyutu.  Yalnızca Windows.|
|Containerımagedownloadtimeout|int, saniye sayısı varsayılan değer 1200 (20 dakika)|Dinamik|Görüntü indirilmesi zaman aşımına uğramadan önce beklenecek saniye sayısı.|
|ContainerImagesToSkip|dize, dikey çizgi karakteriyle ayırarak görüntü adları varsayılan değer ""|Statik|Silinmemesi gereken bir veya daha fazla kapsayıcı görüntülerini adı.  PruneContainerImages parametresiyle birlikte kullanılır.|
|ContainerServiceLogFileNamePrefix|"sfcontainerlogs" varsayılan bir dize ise|Statik|Docker kapsayıcıları tarafından oluşturulan günlük dosyaları için dosya adı ön eki.  Yalnızca Windows.|
|ContainerServiceLogFileRetentionCount|Int, varsayılan 10'dur|Statik|Docker kapsayıcıları günlük dosyalarını önce tarafından oluşturulan günlük dosyası sayısı üzerine yazılır.  Yalnızca Windows.|
|CreateFabricRuntimeTimeout|Zaman aralığı, Common::TimeSpan::FromSeconds(120) varsayılandır|Dinamik| Saniye cinsinden zaman aralığı belirtin. Zaman aşımı değeri FabricCreateRuntime eşitleme çağrısı |
|DefaultContainerRepositoryAccountName|Varsayılan bir dize ise ""|Statik|ApplicationManifest.xml içinde belirtilen kimlik bilgileri yerine kullanılan varsayılan kimlik bilgileri |
|DefaultContainerRepositoryPassword|Varsayılan bir dize ise ""|Statik|ApplicationManifest.xml içinde belirtilen kimlik bilgileri yerine kullanılan varsayılan parola kimlik bilgileri|
|DefaultContainerRepositoryPasswordType|Varsayılan bir dize ise ""|Statik|Boş olmayan dize, değer "Şifreli" veya "SecretsStoreRef" olabilir.|
|DeploymentMaxFailureCount|Int, varsayılan 20'dir| Dinamik|Uygulama dağıtımı düğümde, uygulama dağıtımını başarısızlığından önce geçen DeploymentMaxFailureCount zamanları yeniden denenecek.| 
|DeploymentMaxRetryInterval| Zaman aralığı, Common::TimeSpan::FromSeconds(3600) varsayılandır|Dinamik| Saniye cinsinden zaman aralığı belirtin. Dağıtım için en fazla yeniden deneme aralığı. Her sürekli hata durumunda yeniden deneme aralığı (DeploymentMaxRetryInterval; Min hesaplanır. Sürekli hata sayısı * DeploymentRetryBackoffInterval) |
|DeploymentRetryBackoffInterval| Zaman aralığı, Common::TimeSpan::FromSeconds(10) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Dağıtım hatası için geri alma aralığı. Her sürekli dağıtım hatası sistem dağıtımının MaxDeploymentFailureCount kadar yeniden deneyecek. Yeniden deneme aralığı, bir ürün sürekli dağıtım hatası ve dağıtımı geri alma aralığı ' dir. |
|DisableContainers|bool, varsayılan FALSE olur.|Statik|Kapsayıcılar olan DisableContainerServiceStartOnContainerActivatorOpen yerine kullanılan - devre dışı bırakma yapılandırma yapılandırma kullanım dışı. |
|DisableDockerRequestRetry|bool, varsayılan FALSE olur. |Dinamik| Varsayılan olarak SF gg (docker dameon) ile 'DockerRequestTimeout' kendisine gönderilen her http isteği için bir zaman aşımı ile iletişim kurar. DD yoksa bu süre içinde yanıt verir; Kalan süre üst düzey işlem hala varsa, BT isteği yeniden gönderir.  Hyperv kapsayıcıyla; DD, bazen kapsayıcınızı getirebilirsiniz veya devre dışı çok daha uzun zaman. Böyle durumlarda gg isteği zaman aşımına SF Perspektif ve SF işlemi yeniden dener. Bazen bu ekler daha fazla baskısı üzerinde gg gibi görünüyor Bu yapılandırma, bu yeniden deneme devre dışı bırakın ve yanıt vermesi için DD bekleyin imkan tanır. |
|EnableActivateNoWindow| bool, varsayılan FALSE olur.|Dinamik| Etkin işlem arka planda herhangi bir konsol olmadan oluşturulur. |
|EnableContainerServiceDebugMode|bool, varsayılan true'dur.|Statik|Docker kapsayıcıları için günlük kaydını etkinleştir/devre dışı bırak.  Yalnızca Windows.|
|EnableDockerHealthCheckIntegration|bool, varsayılan true'dur.|Statik|Service Fabric sistem durumu raporu ile docker HEALTHCHECK olayları entegrasyonunu sağlar |
|EnableProcessDebugging|bool, varsayılan FALSE olur.|Dinamik| Başlatılırken hata ayıklayıcısı altında uygulama ana bilgisayarları etkinleştirir |
|EndpointProviderEnabled| bool, varsayılan FALSE olur.|Statik| Uç nokta kaynak doku tarafından yönetilmesini sağlar. Başlangıç ve bitiş uygulama bağlantı noktası aralığında FabricNode belirtimi gerektirir. |
|FabricContainerAppsEnabled| bool, varsayılan FALSE olur.|Statik| |
|FirewallPolicyEnabled|bool, varsayılan FALSE olur.|Statik| Açık bağlantı noktaları ServiceManifest içinde belirtilen Endpoint kaynaklar için güvenlik duvarı bağlantı noktaları açma etkinleştirir |
|GetCodePackageActivationContextTimeout|Zaman aralığı, Common::TimeSpan::FromSeconds(120) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. CodePackageActivationContext çağrıları için zaman aşımı değeri. Bu geçici Hizmetleri için geçerli değildir. |
|GovernOnlyMainMemoryForProcesses|bool, varsayılan FALSE olur.|Statik|Kaynak İdaresi varsayılan davranışını MemoryInMB işleminin kullandığı toplam bellek (RAM + takas) üzerinde belirtilen sınırı eklemektir. Bu sınır aşılırsa; işlem OutOfMemory özel durumu alır. Bu parametre ayarlanmışsa true; yalnızca bir işlem kullanacağı RAM bellek miktarı için sınır uygulanır. Bu sınır aşılırsa; ve bu ayar true ise; Ardından, işletim sistemi ana bellek birbirleriyle değiştirecektir. |
|IPProviderEnabled|bool, varsayılan FALSE olur.|Statik|IP adresi yönetimi sağlar. |
|IsDefaultContainerRepositoryPasswordEncrypted|bool, varsayılan FALSE olur.|Statik|Olup olmadığını DefaultContainerRepositoryPassword şifrelenir.|
|LinuxExternalExecutablePath|Varsayılan bir dize ise "/ usr/bin /" |Statik|Dış yürütülebilir komut düğümde birincil dizin.|
|NTLMAuthenticationEnabled|bool, varsayılan FALSE olur.|Statik| NTLM süreçler makineler arasında güvenli bir şekilde iletişim kurabilmesi için çalışan diğer kullanıcıların kod paketleri kullanmak için desteği etkinleştirir. |
|NTLMAuthenticationPasswordSecret|SecureString, Common::SecureString("") varsayılandır|Statik|NTLM kullanıcılar için parola oluşturmak için kullanılan bir şifrelenmiş sahip olur. NTLMAuthenticationEnabled doğru olması durumunda ayarlanması gerekir. Dağıtıcı doğrulandı. |
|NTLMSecurityUsersByX509CommonNamesRefreshInterval|Zaman aralığı, Common::TimeSpan::FromMinutes(3) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Ortama özgü ayarlar FileStoreService NTLM yapılandırması için kullanılacak yeni sertifikalar için hangi barındırma düzenli aralıklarla tarar. |
|NTLMSecurityUsersByX509CommonNamesRefreshTimeout|Zaman aralığı, Common::TimeSpan::FromMinutes(4) varsayılandır|Dinamik| Saniye cinsinden zaman aralığı belirtin. Sertifika ortak adları kullanarak NTLM kullanıcıları yapılandırmak için zaman aşımı. NTLM kullanıcılar FileStoreService paylaşımları için gereklidir. |
|PruneContainerImages|bool, varsayılan FALSE olur.|Dinamik| Kullanılmayan uygulama kapsayıcı görüntüleri düğümünden kaldırın. Service Fabric kümesinden bir ApplicationType silindiğinde, bu uygulama tarafından kullanılan kapsayıcı görüntüleri, Service Fabric tarafından yüklendiği düğümlerinde kaldırılacak. Ayıklama, kümeden kaldırılmalıdır görüntüler için her saat çalışır, böylece en çok bir saat (artı görüntüsü ayıklamak üzere zaman) alabilir.<br>Service Fabric, hiçbir zaman indirin veya bir uygulama için ilgili olmayan görüntüleri kaldırın.  El ile veya başka türlü indirildi ilgisiz görüntüleri açıkça kaldırılması gerekir.<br>Silinmemesi gereken görüntüleri ContainerImagesToSkip parametresi belirtilebilir.| 
|RegisterCodePackageHostTimeout|Zaman aralığı, Common::TimeSpan::FromSeconds(120) varsayılandır|Dinamik| Saniye cinsinden zaman aralığı belirtin. FabricRegisterCodePackageHost eşitleme çağrısı zaman aşımı değeri. Bu yalnızca çoklu kod paketi uygulama ana FWP gibi için geçerlidir |
|RequestTimeout|Zaman aralığı, Common::TimeSpan::FromSeconds(30) varsayılandır|Dinamik| Saniye cinsinden zaman aralığı belirtin. Bu, kullanıcının uygulama konağı ile Fabrika kayıt gibi çeşitli barındırma ilgili işlemler için yapı işlemi arasındaki iletişim için zaman aşımı temsil eder; çalışma zamanı kaydı. |
|RunAsPolicyEnabled| bool, varsayılan FALSE olur.|Statik| Kullanıcı dışındaki yerel kullanıcı olarak kod paketleri çalıştıran etkinleştirir altında hangi yapı işlemi çalışmıyor. Bu ilkeyi etkinleştirmek için doku sistem veya SeAssignPrimaryTokenPrivilege sahip kullanıcı olarak çalıştırılması gerekir. |
|ServiceFactoryRegistrationTimeout| Zaman aralığı, Common::TimeSpan::FromSeconds(120) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Kayıt (durum bilgisi olmayan/durum bilgisi olan) ServiceFactory çağrı eşitleme için zaman aşımı değeri |
|ServiceTypeDisableFailureThreshold |Tam sayı olarak varsayılan 1. |Dinamik|Daha sonra bu düğüme hizmet türü devre dışı bırakabilir ve yerleştirme için farklı bir düğüme deneyin FailoverManager (FM) bildirilir hata sayısı eşiği budur. |
|ServiceTypeDisableGraceInterval|Zaman aralığı, Common::TimeSpan::FromSeconds(30) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Daha sonra hizmet türü devre dışı bırakılabilir zaman aralığı |
|ServiceTypeRegistrationTimeout |Süresi 300 saniye cinsinden varsayılandır |Dinamik|ServiceType fabric ile kayıtlı olması için izin verilen maksimum süre |
|UseContainerServiceArguments|bool, varsayılan true'dur.|Statik|Bu yapılandırma, docker cinini bağımsız değişkenleri geçirme (ContainerServiceArguments yapılandırmada belirtilir) atlamak için barındırma söyler.|

## <a name="httpgateway"></a>HttpGateway

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|ActiveListeners |Uint, varsayılan 50'dir |Statik| Http sunucu kuyruğa gönderilecek okuma sayısı. Bu, HttpGateway tarafından karşılanabileceğini eş zamanlı istek sayısını denetler. |
|HttpGatewayHealthReportSendInterval |Zamanı saniye cinsinden varsayılan 30'dur |Statik|Saniye cinsinden zaman aralığı belirtin. Hangi Http ağ geçidi birikmiş sistem durumu gönderir aralığı için sistem durumu Yöneticisi bildirir. |
|HttpStrictTransportSecurityHeader|Varsayılan bir dize ise ""|Dinamik| HttpGateway tarafından gönderilen her yanıt dahil edilecek HTTP katı aktarım güvenliği üst bilgi değeri belirtin. Boş dize olarak ayarlandığında; Bu üst bilgi, ağ geçidi yanıta dahil değildir.|
|IsEnabled|Bool, varsayılan değer false'tur |Statik| HttpGateway etkinleştirir/devre dışı bırakır. HttpGateway varsayılan olarak devre dışıdır. |
|MaxEntityBodySize |Uint 4194304 varsayılandır |Dinamik|Http isteğinden beklenebilir gövdesi boyutu üst sınırı sağlar. Varsayılan değer 4 MB'dir. Gövde boyutu varsa, Httpgateway isteği başarısız olur > Bu değeri. 4096 bayt okuma en düşük öbek boyutudur. Bu olması gerekir böylece > 4096 =. |

## <a name="imagestoreservice"></a>ImageStoreService

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|Etkin |Bool, varsayılan değer false'tur |Statik|ImageStoreService için etkin bayrağı. Varsayılan: false |
|MinReplicaSetSize | Int, varsayılan 3'tür |Statik|MinReplicaSetSize ImageStoreService için. |
|PlacementConstraints | Varsayılan bir dize ise "" |Statik| PlacementConstraints ImageStoreService için. |
|QuorumLossWaitDuration | Zamanı saniye cinsinden MaxValue varsayılandır |Statik| Saniye cinsinden zaman aralığı belirtin. QuorumLossWaitDuration ImageStoreService için. |
|ReplicaRestartWaitDuration | Zaman 60,0 * 30 saniye cinsinden varsayılandır |Statik|Saniye cinsinden zaman aralığı belirtin. ReplicaRestartWaitDuration ImageStoreService için. |
|StandByReplicaKeepDuration | Zamanı saniye cinsinden varsayılan 3600.0 * 2'dir |Statik| Saniye cinsinden zaman aralığı belirtin. StandByReplicaKeepDuration ImageStoreService için. |
|TargetReplicaSetSize | Int, varsayılan 7'dir |Statik|TargetReplicaSetSize ImageStoreService için. |

## <a name="ktllogger"></a>KtlLogger

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|AutomaticMemoryConfiguration |int, varsayılan 1. |Dinamik|Bellek ayarları otomatik olarak ve dinamik olarak yapılandırılması gerekip gerekmediğini gösteren bayrak. Sıfır sonra bellek yapılandırma ayarları doğrudan kullanılır ve değiştirmeyin sistem koşullara göre. Bir sonra bellek ayarları otomatik olarak yapılandırılır ve değişebilir sistem koşullara göre. |
|MaximumDestagingWriteOutstandingInKB | int, varsayılan 0'dır |Dinamik|KB önceden ayrılmış bir günlük ilerlemek Paylaşılan oturum sayısı. 0 sınır belirtmek için kullanın.
|SharedLogId |Varsayılan bir dize ise "" |Statik|Paylaşılan günlük kapsayıcı için benzersiz GUID. Kullanım "" fabric veri kökü altındaki varsayılan yol kullanıyorsanız. |
|SharedLogPath |Varsayılan bir dize ise "" |Statik|Paylaşılan günlük kapsayıcı yerleştirileceği konum için yolu ve dosya adı. Kullanım "" fabric veri kökü altındaki varsayılan yol kullanma. |
|SharedLogSizeInMB |Int, 8192 varsayılandır |Statik|Paylaşılan günlük kapsayıcısında ayrılacak MB sayısı. |
|SharedLogThrottleLimitInPercentUsed|int, varsayılan 0'dır | Statik | Azaltma anlamına paylaşılan günlük kullanım yüzdesi. Değer 0 ile 100 arasında olmalıdır. 0 değeri, varsayılan yüzde değeri kullanarak anlamına gelir. Hiçbir kısıtlama 100 değerini gösterir. 1 ile 99 arasında bir değer yukarıdaki hangi azaltma meydana gelir günlük kullanım yüzdesini belirtir. Paylaşılan günlük 10 GB ve değeri ise 9 GB kullanımda olduğunda, azaltma çıkar örneğin 90 olması nedeniyle. Varsayılan değerin kullanılması önerilir.|
|WriteBufferMemoryPoolMaximumInKB | int, varsayılan 0'dır |Dinamik|Yazma arabellek belleği havuzu kadar büyümesine izin vermek için KB sayısı. 0 sınır belirtmek için kullanın. |
|WriteBufferMemoryPoolMinimumInKB |Int, 8388608 varsayılandır |Dinamik|Başlangıçta yazma arabellek belleği havuzu için ayrılacak KB sayısı. Varsayılan sınır belirtmek için 0 kullanın SharedLogSizeInMB aşağıdaki ile tutarlı olmalıdır. |

## <a name="management"></a>Yönetim

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|AutomaticUnprovisionInterval|Zaman aralığı, Common::TimeSpan::FromMinutes(5) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Temizleme aralığı için izin verilen için otomatik uygulama türü temizlenmesi sırasında uygulama türünün kaydını silin.|
|AzureStorageMaxConnections | Int, varsayılan 5000'dir |Dinamik|Azure depolama alanına eş zamanlı bağlantı sayısı. |
|AzureStorageMaxWorkerThreads | int, varsayılan 25'tir |Dinamik|Paralel çalışan iş parçacığı sayısı. |
|AzureStorageOperationTimeout | Zamanı saniye cinsinden 6000 varsayılandır |Dinamik|Saniye cinsinden zaman aralığı belirtin. Xstore işlemin tamamlanması zaman aşımına uğradı. |
|CleanupApplicationPackageOnProvisionSuccess|bool, varsayılan FALSE olur. |Dinamik|Etkinleştirir veya otomatik temizleme başarılı sağlama üzerinde uygulama paketinin devre dışı bırakır. |
|CleanupUnusedApplicationTypes|bool, varsayılan FALSE olur. |Dinamik|Bu yapılandırma, etkinleştirilirse, böylece görüntü deposu tarafından kullanılan disk alanı kırpma en son üç kullanılmayan sürümleri, atlanıyor kullanılmayan uygulama türü sürümleri otomatik olarak kaydını sağlar. Otomatik temizleme sonunda, bu belirli uygulama türü için başarılı sağlama tetiklenir ve tüm uygulama türleri için günde bir kez düzenli aralıklarla da çalışır. Atlamak için kullanılmayan sürüm sayısı "MaxUnusedAppTypeVersionsToKeep" parametresi kullanılarak yapılandırılabilir. |
|DisableChecksumValidation | Bool, varsayılan değer false'tur |Statik| Bu yapılandırmayı etkinleştirme veya devre dışı uygulama sağlama sırasında sağlama toplamı doğrulaması sağlıyor. |
|DisableServerSideCopy | Bool, varsayılan değer false'tur |Statik|Bu yapılandırmayı etkinleştirir veya uygulama sağlama sırasında uygulama paketini ImageStore üzerinde sunucu tarafı kopyasını devre dışı bırakır. |
|ImageCachingEnabled | Bool, varsayılan değer true şeklindedir |Statik|Bu yapılandırmayı etkinleştirme veya devre dışı önbelleğe alma sağlıyor. |
|Imagestoreconnectionstring |SecureString |Statik|Kök ImageStore için bağlantı dizesi. |
|ImageStoreMinimumTransferBPS | Int, varsayılan 1024'tür. |Dinamik|Küme ImageStore arasındaki en düşük aktarım hızı. Bu değer, dış ImageStore erişirken zaman aşımını belirlemek için kullanılır. Yalnızca ImageStore ve küme arasındaki gecikme dış ImageStore indirmek küme için daha fazla zaman izin vermek için yüksek olduğunda bu değeri değiştirin. |
|MaxUnusedAppTypeVersionsToKeep | Int, varsayılan 3'tür |Dinamik|Bu yapılandırma için temizleme atlanacak kullanılmayan uygulama türü sürümleri sayısını tanımlar. Bu parametre, parametre CleanupUnusedApplicationTypes etkinse geçerlidir. |


## <a name="metricactivitythresholds"></a>MetricActivityThresholds
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|KeyIntegerValueMap, varsayılan, Yok'tur|Dinamik|Kümedeki ölçümler için MetricActivityThresholds kümesini belirler. MaxNodeLoad MetricActivityThresholds büyükse Dengeleme çalışır. Birleştirme ölçümleri veya altında olan Service Fabric düğümü boş dikkate alacaktır yük eşit miktarda tanımlar |

## <a name="metricbalancingthresholds"></a>MetricBalancingThresholds
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, varsayılan, Yok'tur|Dinamik|Kümedeki ölçümler için MetricBalancingThresholds kümesini belirler. MaxNodeLoad/minNodeLoad MetricBalancingThresholds büyükse Dengeleme çalışır. En az bir FD veya UD maxNodeLoad/minNodeLoad MetricBalancingThresholds küçükse, birleştirme çalışır. |

## <a name="namingservice"></a>NamingService

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|GatewayServiceDescriptionCacheLimit |int, varsayılan 0'dır |Statik|(Sınırsız için 0 olarak ayarlayın) adlandırma Gateway LRU hizmet açıklaması önbelleğinde tutulan girişleri maksimum sayısı. |
|MaxClientConnections |Int, varsayılan 1000'dir |Dinamik|Ağ geçidi başına istemci bağlantısı sayısı izin verilen en fazla. |
|MaxFileOperationTimeout |Zamanı saniye cinsinden varsayılan 30'dur |Dinamik|Saniye cinsinden zaman aralığı belirtin. Dosya depolama hizmeti işlem için izin verilen en uzun zaman aşımı. Daha büyük bir zaman aşımı belirterek istekler reddedilir. |
|MaxIndexedEmptyPartitions |Int, varsayılan 1000'dir |Dinamik|Yeniden bağlanan istemciler eşitlemek için bildirim önbelleğinde kalacak boş bölümler sayısı dizini. Tüm boş bölümler yukarıda bu sayı, arama sürüm artan dizin CİHAZDAN kaldırılır. İstemcileri yeniden bağlanmayı hala eşitleyin ve eksik boş bölüm güncelleştirmelerini; ancak eşitleme protokolü daha pahalı olur. |
|MaxMessageSize |Int, varsayılan değer 4\*1024\*1024 |Statik|Adlandırma kullanırken istemci düğüm iletişimi için en büyük ileti boyutu. DOS saldırısı alleviation; Varsayılan değer 4 MB'dir. |
|MaxNamingServiceHealthReports | Int, varsayılan 10'dur |Dinamik|Adlandırma depolayan yavaş işlemlerinin maksimum sayısı aynı anda raporları sağlıksız hizmet. 0 ise; Tüm yavaş işlemleri gönderilir. |
|MaxOperationTimeout |Zaman 600 saniye cinsinden varsayılandır |Dinamik|Saniye cinsinden zaman aralığı belirtin. İstemci işlemleri için izin verilen en uzun zaman aşımı. Daha büyük bir zaman aşımı belirterek istekler reddedilir. |
|MaxOutstandingNotificationsPerClient |Int, varsayılan 1000'dir |Dinamik|Bir istemci kaydı önce bekleyen bildirimler sayısı ağ geçidi tarafından zorla kapatıldı. |
|MinReplicaSetSize | Int, varsayılan 3'tür |İzin Verilmiyor| Adlandırma Hizmeti çoğaltmaları oturum güncelleştirmesini tamamlamak yazmak için gerekli en düşük sayısı. Bundan daha az çoğaltmalar varsa çoğaltmalarını geri yüklenene kadar sistem etkin güvenilirlik sistem güncelleştirmeleri için adlandırma hizmeti Store reddeder. Bu değeri hiçbir zaman birden çok TargetReplicaSetSize olmamalıdır. |
|PartitionCount |Int, varsayılan 3'tür |İzin Verilmiyor|Oluşturulacak bölüm adlandırma hizmeti depolayın. Her bölüm kendi dizine karşılık gelen bir tek bölüm anahtarı sahip; Bu nedenle bölüm anahtarları [0; Bölüm sayısı) yok. Adlandırma Hizmeti herhangi bir yedekleme çoğaltma tarafından tutulan verileri ortalama miktarını azaltarak gerçekleştirmesi ölçek adlandırma hizmeti bölümleri artış sayısını artırmayı ayarlayın. kaynakları artan kullanım maliyeti (bölüm sayısı bu yana * ReplicaSetSize hizmet çoğaltmaları saklanabilir).|
|PlacementConstraints | Varsayılan bir dize ise "" |İzin Verilmiyor| Adlandırma Hizmeti için yerleşim kısıtlaması. |
|QuorumLossWaitDuration | Zamanı saniye cinsinden MaxValue varsayılandır |İzin Verilmiyor| Saniye cinsinden zaman aralığı belirtin. Zaman içinde çekirdek kayıp bir adlandırma hizmeti alır; Bu süreölçer başlatır. Belirtecin süresi dolduğunda, FM aşağı çoğaltmaları kayıp olarak dikkate alacaktır; ve çekirdek kurtarmayı deneyin. Bu veri kaybına neden olabilir değil. |
|RepairInterval | Zamanı saniye cinsinden varsayılan 5'tir |Statik| Saniye cinsinden zaman aralığı belirtin. Ad sahibi ve yetki sahibi arasında adlandırma tutarsızlık onarım başlayacağı aralığı. |
|ReplicaRestartWaitDuration | Zamanı saniye cinsinden (60,0 * 30) varsayılandır|İzin Verilmiyor| Saniye cinsinden zaman aralığı belirtin. Adlandırma hizmeti çoğaltması zaman arıza; Bu süreölçer başlatır. Belirtecin süresi dolduğunda, FM basılı olan yinelemeler Değiştir başlar (Bu henüz bunları kayıp göz önünde bulundurmaz). |
|ServiceDescriptionCacheLimit | int, varsayılan 0'dır |Statik| (Sınırsız için 0 olarak ayarlayın) adlandırma Store hizmetine LRU hizmet açıklaması önbelleğinde tutulan girişleri maksimum sayısı. |
|ServiceNotificationTimeout |Zamanı saniye cinsinden varsayılan 30'dur |Dinamik|Saniye cinsinden zaman aralığı belirtin. Hizmet bildirimleri istemciye teslim edilirken kullanılan zaman aşımı. |
|StandByReplicaKeepDuration | Zamanı saniye cinsinden varsayılan 3600.0 * 2'dir |İzin Verilmiyor| Saniye cinsinden zaman aralığı belirtin. Adlandırma hizmeti çoğaltması olduğunuzda geri aşağı durumundan; zaten değiştirilmiş. Bu zamanlayıcı FM bekleme çoğaltma atılmadan önce ne kadar süreyle korur belirler. |
|TargetReplicaSetSize |Int, varsayılan 7'dir |İzin Verilmiyor|Adlandırma Hizmeti deposunun her bölüm için çoğaltma sayısını ayarlar. Çoğaltma kümeleri sayısının artırılması, adlandırma hizmeti Store bilgileri güvenilirlik düzeyini artırır; düğüm hataları nedeniyle bilgiler kaybolacak değişiklik azalan; Windows Fabric ve süreyi artan yükü maliyetiyle adlandırma veri güncelleştirmeleri gerçekleştirmek için alır.|

## <a name="nodebufferpercentage"></a>NodeBufferPercentage
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|KeyDoubleValueMap, varsayılan, Yok'tur|Dinamik|Ölçüm adı başına düğüm kapasitesi yüzdesini; Yük devretme çalışması için bir düğüm üzerinde ücretsiz bir yere tutmak için bir arabellek olarak kullanılır. |

## <a name="nodecapacities"></a>NodeCapacities

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup |NodeCapacityCollectionMap |Statik|Farklı ölçümler için düğüm kapasiteleri koleksiyonudur. |

## <a name="nodedomainids"></a>NodeDomainIds

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup |NodeFaultDomainIdCollection |Statik|Bir düğüme ait hata etki alanları açıklar. Hata etki alanı, veri merkezinde düğüm konumunu tanımlayan bir URI aracılığıyla tanımlanır.  Hata etki alanı URI biçimi olan fd: / fd/URI yol kesimi tarafından izlenen.|
|UpgradeDomainId |Varsayılan bir dize ise "" |Statik|Yükseltme etki alanına ait bir düğümü tanımlar. |

## <a name="nodeproperties"></a>NodeProperties

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup |NodePropertyCollectionMap |Statik|Düğüm özellikleri anahtar-değer çiftleri koleksiyonu. |

## <a name="paas"></a>Paas

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|Lclusterıd |Varsayılan bir dize ise "" |İzin Verilmiyor|X509 sertifika deposunu yapılandırma koruması için fabric tarafından kullanılır. |

## <a name="performancecounterlocalstore"></a>PerformanceCounterLocalStore

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|Sayaçları |String | Dinamik |Performans sayaçları toplamak için virgülle ayrılmış listesi. |
|IsEnabled |Bool, varsayılan değer true şeklindedir | Dinamik |Bayrağı, performans sayacı koleksiyonu yerel düğümde etkin olup olmadığını gösterir. |
|MaxCounterBinaryFileSizeInMB |int, varsayılan 1. | Dinamik |Her performans sayacı ikili dosya için maksimum boyut (MB cinsinden). |
|NewCounterBinaryFileCreationIntervalInMinutes |Int, varsayılan 10'dur | Dinamik |Sonra yeni bir performans sayacı ikili dosya oluşturulduğunda en büyük aralık (saniye cinsinden). |
|SamplingIntervalInSeconds |Int, varsayılan değer 60'tır | Dinamik |Toplanan performans sayaçları için örnekleme aralığı. |

## <a name="placementandloadbalancing"></a>PlacementAndLoadBalancing

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|AffinityConstraintPriority | int, varsayılan 0'dır | Dinamik|Benzeşim kısıtlama önceliğini belirler: 0: Sabit; 1: Geçici; Negatif: Yoksayın. |
|ApplicationCapacityConstraintPriority | int, varsayılan 0'dır | Dinamik|Kapasite kısıtlama önceliğini belirler: 0: Sabit; 1: Geçici; Negatif: Yoksayın. |
|AutoDetectAvailableResources|bool, varsayılan true'dur.|Statik|Bu yapılandırma, otomatik algılama kullanılabilir kaynakları (CPU ve bellek) düğümünde tetikleyecek bu yapılandırma ayarlandığında true - biz gerçek kapasiteler okuyun ve kullanıcı hatalı düğüm kapasiteleri belirtilen ya da bu yapılandırma - false olarak ayarlanmışsa bunları hiç tanımlamadığınız düzeltmenize yapacağız  Belirtilen kullanıcının hatalı düğüm kapasiteleri uyarı izleme; ancak bunları değil; Kullanıcının istediği olarak belirtilen kapasiteleri anlamı > düğümü gerçekten olandan veya kapasiteleri tanımlanmamış; Sınırsız kapasite varsayar |
|BalancingDelayAfterNewNode | Zamanı saniye cinsinden varsayılan 120'dir |Dinamik|Saniye cinsinden zaman aralığı belirtin. Yeni bir düğüm ekledikten sonra bu süre içinde etkinlikleri Dengeleme başlatmayın. |
|BalancingDelayAfterNodeDown | Zamanı saniye cinsinden varsayılan 120'dir |Dinamik|Saniye cinsinden zaman aralığı belirtin. Bu süreden sonra aşağı olayını bir düğüm içindeki etkinlikleri Dengeleme başlatmayın. |
|CapacityConstraintPriority | int, varsayılan 0'dır | Dinamik|Kapasite kısıtlama önceliğini belirler: 0: Sabit; 1: Geçici; Negatif: Yoksayın. |
|ConsecutiveDroppedMovementsHealthReportLimit | Int, varsayılan 20'dir | Dinamik|ResourceBalancer tarafından verilen hareketler tanılama yürütülür ve sistem durumu uyarıları yayılan önce bırakılan art arda kaç kez sayısını tanımlar. Negatif: Hiçbir uyarı yayılan altında bu koşulu. |
|ConstraintFixPartialDelayAfterNewNode | Zamanı saniye cinsinden varsayılan 120'dir |Dinamik| Saniye cinsinden zaman aralığı belirtin. DDo FaultDomain değil düzeltmek ve yeni bir düğüm ekledikten sonra bu süre içinde UpgradeDomain kısıtlaması ihlali. |
|ConstraintFixPartialDelayAfterNodeDown | Zamanı saniye cinsinden varsayılan 120'dir |Dinamik| Saniye cinsinden zaman aralığı belirtin. Bir düğüm aşağı olayını sonra bu süre içinde değil FaultDomain düzeltin ve UpgradeDomain sabiti ihlallerini yapın. |
|ConstraintViolationHealthReportLimit | Int, varsayılan 50'dir |Dinamik| Tanılama yürütülür ve yayılan durumu raporları önce sınıflandırmanıza sabitlenmemiş olacak çoğaltma ihlal kısıtlamasına sahip sayısını tanımlar. |
|DetailedConstraintViolationHealthReportLimit | Int, 200 varsayılandır |Dinamik| Tanılama yürütülür ve sistem durumu raporlarını yayınlaması ayrıntılı önce sınıflandırmanıza sabitlenmemiş olacak çoğaltma ihlal kısıtlamasına sahip sayısını tanımlar. |
|DetailedDiagnosticsInfoListLimit | int, varsayılan 15'tir |Dinamik| Tanılama kesilmeden önce içerecek şekilde sınırlama başına sayıda tanı girdisi (ayrıntılı bilgilerle) tanımlar.|
|DetailedNodeListLimit | int, varsayılan 15'tir |Dinamik| Yerleştirilmemiş çoğaltmayı raporlarında kesilmeden önce içerecek şekilde sınırlama başına düğüm sayısını tanımlar. |
|DetailedPartitionListLimit | int, varsayılan 15'tir |Dinamik| Tanılama kesilmeden önce dahil etmek bir kısıtlama için tanı girdisi başına bölüm sayısını tanımlar. |
|DetailedVerboseHealthReportLimit | Int, 200 varsayılandır | Dinamik|Ayrıntılı sistem durumu raporlarını yayınlaması önce kalıcı olarak unplaced olarak unplaced bir çoğaltma performansa sayısını tanımlar. |
|FaultDomainConstraintPriority | int, varsayılan 0'dır |Dinamik| Hata etki alanı kısıtlama önceliğini belirler: 0: Sabit; 1: Geçici; Negatif: Yoksayın. |
|GlobalMovementThrottleCountingInterval | Zaman 600 saniye cinsinden varsayılandır |Statik| Saniye cinsinden zaman aralığı belirtin. Geçen süre (GlobalMovementThrottleThreshold birlikte kullanılan) etki alanı çoğaltma hareketleri başına izlemek istediğiniz gösterir. Genel tamamen azaltma yok saymak için 0 olarak ayarlanabilir. |
|GlobalMovementThrottleThreshold | Uint, varsayılan 1000'dir |Dinamik| Hareketleri Dengeleme aşamasında GlobalMovementThrottleCountingInterval tarafından belirtilen son aralığı izin verilen maksimum sayısı. |
|GlobalMovementThrottleThresholdForBalancing | Uint, varsayılan 0'dır | Dinamik|Hareketleri Dengeleme aşamasında GlobalMovementThrottleCountingInterval tarafından belirtilen son aralığı izin verilen maksimum sayısı. 0 sınır olmadığını. |
|GlobalMovementThrottleThresholdForPlacement | Uint, varsayılan 0'dır |Dinamik| Yerleştirme aşamasında GlobalMovementThrottleCountingInterval.0 tarafından belirtilen son aralığı izin verilen hareketleri sayısının hiçbir sınırı göstermez.|
|GlobalMovementThrottleThresholdPercentage|Double, varsayılan 0'dır|Dinamik|Toplam hareketleri (çoğaltmaları kümedeki toplam sayısının yüzdesi olarak ifade edilir), Yük Dengeleme ve yerleştirme aşamalardaki GlobalMovementThrottleCountingInterval tarafından belirtilen son aralığı izin verilen maksimum sayısı. 0 sınır olmadığını. Varsa bu iki ve GlobalMovementThrottleThreshold belirtilir; Daha sonra daha pasif sınırı kullanılır.|
|GlobalMovementThrottleThresholdPercentageForBalancing|Double, varsayılan 0'dır|Dinamik|Hareketleri (PLB yinelemede toplam sayısının yüzdesi olarak ifade edilir) aşamasında Dengeleme GlobalMovementThrottleCountingInterval tarafından belirtilen son aralığı izin verilen maksimum sayısı. 0 sınır olmadığını. Varsa bu iki ve GlobalMovementThrottleThresholdForBalancing belirtilir; Daha sonra daha pasif sınırı kullanılır.|
|InBuildThrottlingAssociatedMetric | Varsayılan bir dize ise "" |Statik| Bu kısıtlama için ilişkili ölçüm adı. |
|InBuildThrottlingEnabled | Bool, varsayılan değer false'tur |Dinamik| Azaltma derleme etkin olup olmadığını belirler. |
|InBuildThrottlingGlobalMaxValue | int, varsayılan 0'dır |Dinamik|Küresel olarak izin verilen derleme çoğaltmaları düzeyde sayısı. |
|InterruptBalancingForAllFailoverUnitUpdates | Bool, varsayılan değer false'tur | Dinamik|Yük devretme birimi güncelleştirme herhangi bir türde veya hızlı kesme çalıştırma Dengeleme yavaş belirler. "False" çalıştırma Dengeleme kesilmesi durumunda olan belirtilen FailoverUnit: oluşturulan/silinir; çoğaltmalar eksik; Birincil çoğaltma konumu veya değiştirilen çoğaltmaların sayısı değişti. Çalıştırma Dengeleme kesintiye diğer durumlarda - varsa FailoverUnit: ek yinelemeler; sahip herhangi bir çoğaltma bayrağı değiştirildi; yalnızca bölüm sürümü veya diğer herhangi bir durumu değiştirildi. |
|MinConstraintCheckInterval | Zamanı saniye olarak varsayılan 1'dir |Dinamik| Saniye cinsinden zaman aralığı belirtin. İki ardışık kısıtlamasında önce yuvarlar geçmesi gereken en düşük süreyi tanımlar. |
|MinLoadBalancingInterval | Zamanı saniye cinsinden varsayılan 5'tir |Dinamik| Saniye cinsinden zaman aralığı belirtin. İki ardışık karşı yuvarlar önce geçmesi gereken en düşük süreyi tanımlar. |
|MinPlacementInterval | Zamanı saniye olarak varsayılan 1'dir |Dinamik| Saniye cinsinden zaman aralığı belirtin. İki ardışık yerleştirme yuvarlar önce geçmesi gereken en düşük süreyi tanımlar. |
|MoveExistingReplicaForPlacement | Bool, varsayılan değer true şeklindedir |Dinamik|Yerleştirme sırasında mevcut çoğaltma taşımak ise belirleyen ayarlama. |
|MovementPerPartitionThrottleCountingInterval | Zaman 600 saniye cinsinden varsayılandır |Statik| Saniye cinsinden zaman aralığı belirtin. Geçen süre (MovementPerPartitionThrottleThreshold birlikte kullanılan) her bölüm için çoğaltma hareketleri izlemek istediğiniz gösterir. |
|MovementPerPartitionThrottleThreshold | Uint, varsayılan 50'dir |Dinamik| Bu bölüm çoğaltmaları için ilgili hareketleri Dengeleme sayısını ulaştınız veya belirttiği son aralığında MovementPerFailoverUnitThrottleThreshold aşıldı Dengeleme ile ilgili hiçbir taşıma için bir bölüm meydana gelir MovementPerPartitionThrottleCountingInterval. |
|MoveParentToFixAffinityViolation | Bool, varsayılan değer false'tur |Dinamik| Benzeşim kısıtlamaları düzeltmek için üst çoğaltmaları taşınıp taşınamayacağını belirleyen bir ayar.|
|PartiallyPlaceServices | Bool, varsayılan değer true şeklindedir |Dinamik| Kümedeki tüm hizmet çoğaltmalar "tümü veya hiçbiri" yerleştirilip yerleştirilmeyeceğini sınırlı uygun düğümleri için bunları belirler.|
|PlaceChildWithoutParent | Bool, varsayılan değer true şeklindedir | Dinamik|Üst kopyası çalışıyorsa alt yineleme hizmeti belirleyen ayarlama yerleştirilebilir. |
|PlacementConstraintPriority | int, varsayılan 0'dır | Dinamik|Yerleştirme kısıtlama önceliğini belirler: 0: Sabit; 1: Geçici; Negatif: Yoksayın. |
|PlacementConstraintValidationCacheSize | Int, varsayılan 10000'dir |Dinamik| Tabloyu hızlı doğrulama için kullanılan ve yerleşim kısıtlaması ifadeleri önbellek boyutunu sınırlar. |
|PlacementSearchTimeout | Zamanı saniye cinsinden 0,5 varsayılandır |Dinamik| Saniye cinsinden zaman aralığı belirtin. Hizmetleri yerleştirirken; Bu süre için en fazla bir sonuç döndürmeden önce arayın. |
|PLBRefreshGap | Zamanı saniye olarak varsayılan 1'dir |Dinamik| Saniye cinsinden zaman aralığı belirtin. PLB durumu yeniden yenilenmeden önce geçmesi gereken en düşük süreyi tanımlar. |
|PreferredLocationConstraintPriority | Varsayılan Int, 2'dir| Dinamik|Tercih edilen konum kısıtlaması önceliğini belirler: 0: Sabit; 1: Soft; 2: İyileştirme; Negatif: Yoksayma |
|PreferUpgradedUDs|bool, varsayılan true'dur.|Dinamik|Açma ve kapatma zaten geçmeyi tercih eden mantığını etkinleştirir, UD yükseltildi.|
|PreventTransientOvercommit | Bool, varsayılan değer false'tur | Dinamik|PLB tarafından başlatılan taşıma yukarı boşaltılacak kaynakları hemen güvenebilirsiniz belirler. Varsayılan olarak; PLB dışarı taşıma başlatabilir ve hangi geçici oluşturabilirsiniz aynı düğümde taşıma fazla kullanma. Bu parametre, doğru olarak ayarlanması bu tür engeller, overcommits ve devre dışı üzerine birleştirme (diğer adıyla placementWithMove) olacaktır. |
|ScaleoutCountConstraintPriority | int, varsayılan 0'dır |Dinamik| Genişletme sayısı kısıtlaması önceliğini belirler: 0: Sabit; 1: Geçici; Negatif: Yoksayın. |
|SwapPrimaryThrottlingAssociatedMetric | Varsayılan bir dize ise ""|Statik| Bu kısıtlama için ilişkili ölçüm adı. |
|SwapPrimaryThrottlingEnabled | Bool, varsayılan değer false'tur|Dinamik| Takas birincil azaltma etkin olup olmadığını belirler. |
|SwapPrimaryThrottlingGlobalMaxValue | int, varsayılan 0'dır |Dinamik| Küresel olarak izin verilen takas birincil çoğaltma düzeyde sayısı. |
|TraceCRMReasons |Bool, varsayılan değer true şeklindedir |Dinamik|İşletimsel olaylar kanala hareketleri verilen CRM nedenlerle izleme belirtir. |
|UpgradeDomainConstraintPriority | int, varsayılan 1.| Dinamik|Yükseltme etki alanı kısıtlama önceliğini belirler: 0: Sabit; 1: Geçici; Negatif: Yoksayın. |
|UseMoveCostReports | Bool, varsayılan değer false'tur | Dinamik|LB Puanlama işlevin maliyet öğesini yok saymasını söyler; taşıma için daha iyi dengelenmiş yerleştirme elde edilen çok sayıda. |
|UseSeparateSecondaryLoad | Bool, varsayılan değer true şeklindedir | Dinamik|Farklı bir ikincil yük durumunda belirleyen bu ayarı kullanın. |
|ValidatePlacementConstraint | Bool, varsayılan değer true şeklindedir |Dinamik| Bir hizmetin ServiceDescription güncelleştirildiğinde PlacementConstraint ifade bir hizmet için doğrulanmış olup olmadığını belirtir. |
|VerboseHealthReportLimit | Int, varsayılan 20'dir | Dinamik|Bir çoğaltma (ayrıntılı sistem durumu raporlama etkinleştirildiyse) sistem durumu uyarısı için bildirilen önce yerleştirilmemiş gitmesi sayısını tanımlar. |

## <a name="reconfigurationagent"></a>ReconfigurationAgent

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|ApplicationUpgradeMaxReplicaCloseDuration | Zaman içinde varsayılan 900 saniyedir |Dinamik|Saniye cinsinden zaman aralığı belirtin. Uygulama yükseltme sırasında çoğaltmaları hizmet konakları sonlandırmadan önce sistemin bekleyeceği süre takılı kalıyor kapatın.|
|FabricUpgradeMaxReplicaCloseDuration | Zaman içinde varsayılan 900 saniyedir |Dinamik| Saniye cinsinden zaman aralığı belirtin. Fabric yükseltmesi sırasında çoğaltmaları hizmet konakları sonlandırmadan önce sistemin bekleyeceği süre takılı kalıyor kapatın. |
|GracefulReplicaShutdownMaxDuration|Zaman aralığı, Common::TimeSpan::FromSeconds(120) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Çoğaltmaları hizmet konakları sonlandırmadan önce sistemin bekleyeceği süre takılı kalıyor kapatın. Bu değer 0 olarak ayarlanırsa, çoğaltmaları kapatmak için istenecek değil.|
|NodeDeactivationMaxReplicaCloseDuration | Zaman içinde varsayılan 900 saniyedir |Dinamik|Saniye cinsinden zaman aralığı belirtin. Çoğaltmaları hizmet konakları sonlandırmadan önce sistemin bekleyeceği süre takılı kalıyor, düğümü devre dışı bırakma sırasında kapatın. |
|PeriodicApiSlowTraceInterval | Zamanı saniye cinsinden varsayılan değer 5 dakikadır |Dinamik| Saniye cinsinden zaman aralığı belirtin. PeriodicApiSlowTraceInterval üzerinden yavaş API çağrıları API İzleyici tarafından yeniden taranma aralığı tanımlar. |
|ReplicaChangeRoleFailureRestartThreshold|Int, varsayılan 10'dur|Dinamik| Tamsayı. Otomatik risk azaltma eylemi (çoğaltma yeniden başlatma) sonra uygulanacak birincil yükseltmesi sırasında API hatalarının sayısını belirtin. |
|ReplicaChangeRoleFailureWarningReportThreshold|int, varsayılan değer 2147483647'dir|Dinamik| Tamsayı. Sonra sistem durumu raporu uyarı oluşturulur ve birincil yükseltmesi sırasında API hatalarının sayısını belirtin.|
|ServiceApiHealthDuration | Zamanı saniye cinsinden varsayılan değer 30 dakikadır |Dinamik| Saniye cinsinden zaman aralığı belirtin. Biz size sağlıksız raporu önce çalıştırılacak bir hizmeti API'si için ne kadar süreyle bekleme ServiceApiHealthDuration tanımlar. |
|ServiceReconfigurationApiHealthDuration | Zamanı saniye cinsinden varsayılan 30'dur |Dinamik| Saniye cinsinden zaman aralığı belirtin. Biz size sağlıksız rapor önce çalıştırılacak bir hizmeti API için ne kadar süreyle bekleme ServiceReconfigurationApiHealthDuration tanımlar. Bu kullanılabilirlik etkileyen API çağrıları için geçerlidir.|

## <a name="replication"></a>Çoğaltma
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi**| **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|BatchAcknowledgementInterval|Zaman aralığı, Common::TimeSpan::FromMilliseconds(15) varsayılandır|Statik|Saniye cinsinden zaman aralığı belirtin. Bir bildirim geri göndermeden önce bir işlem aldıktan sonra çoğaltıcı beklediği süreyi belirler. Bu süre içinde alınan diğer işlemler, potansiyel olarak çoğaltıcı hacmini azaltır ancak azalan ağ trafiği -> tek bir iletide gönderilen, onayları sahip olur.|
|MaxCopyQueueSize|uint, varsayılan değer 1024'tür|Statik|Bu, en yüksek değeri ilk çoğaltma işlemleri tutar kuyruk boyutunu tanımlar. Bu 2'in kuvveti olması gerektiğini unutmayın. Çalışma zamanı sırasında bu boyutu işlem için kuyruk büyürse birincil ve ikincil çoğaltıcılar kısıtlanır.|
|MaxPrimaryReplicationQueueMemorySize|Uint, varsayılan 0'dır|Statik|Birincil çoğaltma kuyruğu bayt cinsinden en büyük değerini budur.|
|MaxPrimaryReplicationQueueSize|uint, varsayılan değer 1024'tür|Statik|Birincil çoğaltma kuyrukta var olabilecek işlemlerinin maksimum sayısı budur. Bu 2'in kuvveti olması gerektiğini unutmayın.|
|(Maxreplicationmessagesize)|uint, varsayılan 52428800 olduğu|Statik|Çoğaltma işlemleri en büyük ileti boyutu. Varsayılan 50 MB'tır.|
|MaxSecondaryReplicationQueueMemorySize|Uint, varsayılan 0'dır|Statik|İkincil çoğaltma kuyruğu bayt cinsinden en büyük değerini budur.|
|MaxSecondaryReplicationQueueSize|uint, varsayılan 2048'dir|Statik|Bu ikincil çoğaltma kuyrukta var olabilecek işlemleri en büyük sayısıdır. Bu 2'in kuvveti olması gerektiğini unutmayın.|
|QueueHealthMonitoringInterval|Zaman aralığı, Common::TimeSpan::FromSeconds(30) varsayılandır|Statik|Saniye cinsinden zaman aralığı belirtin. Bu değer, çoğaltma işlemi kuyrukları bir uyarı/hata sistem durumu olaylarını izlemek için çoğaltıcı tarafından kullanılan süreyi belirler. '0' değeri, sistem durumu izleme devre dışı bırakır. |
|QueueHealthWarningAtUsagePercent|uint, varsayılan 80'dir|Statik|Bu değer daha sonra size yüksek sıra kullanımı hakkında uyarı raporu çoğaltma kuyruğu kullanımı (yüzde cinsinden) belirler. Biz bunu QueueHealthMonitoringInterval bir yetkisiz kullanım aralıktan sonra yapın. Kuyruğu kullanımı bu yüzdenin altında yetkisiz aralığa denk gelirse|
|ReplicatorAddress|"localhost:0" varsayılan bir dize ise|Statik|Uç nokta biçiminde bir dize-'Windows Fabric Çoğaltıcısı tarafından işlemleri gönderme ve alma için diğer yinelemeler ile bağlantı kurmak için kullanılan IP: BağlantıNoktası'.|
|ReplicatorListenAddress|"localhost:0" varsayılan bir dize ise|Statik|Uç nokta biçiminde bir dize-'Windows Fabric Çoğaltıcısı tarafından diğer çoğaltmalardan alma işlemleri için kullanılan IP: BağlantıNoktası'.|
|ReplicatorPublishAddress|"localhost:0" varsayılan bir dize ise|Statik|Uç nokta biçiminde bir dize-'Windows Fabric Çoğaltıcısı tarafından diğer yinelemeler için gönderme işlemleri için kullanılan IP: BağlantıNoktası'.|
|Retryınterval|Zaman aralığı, Common::TimeSpan::FromSeconds(5) varsayılandır|Statik|Saniye cinsinden zaman aralığı belirtin. Ne zaman bir işlem kaybolur veya bu Zamanlayıcı reddedilen çoğaltıcı gönderme işlemi ne sıklıkta yeniden dener belirler.|

## <a name="resourcemonitorservice"></a>ResourceMonitorService
| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi**| **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|IsEnabled|bool, varsayılan FALSE olur. |Statik|Hizmet veya kümede etkinleştirilmişse denetler. |

## <a name="runas"></a>Farklı Çalıştır

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|RunAsAccountName |Varsayılan bir dize ise "" |Dinamik|RunAs hesabı adını belirtir. Bu yalnızca "Etkialanıkullanıcısı" veya "ManagedServiceAccount" hesabı için gerekli tür. Geçerli değerler "etki alanı\kullanıcı" veya "user@domain". |
|RunAsAccountType|Varsayılan bir dize ise "" |Dinamik|Farklı Çalıştır hesap türünü gösterir. Bu, tüm RunAs bölümü geçerli değerler "Etkialanıkullanıcısı/NetworkService/ManagedServiceAccount/LocalSystem" için gereklidir.|
|Frk.Çal.parolası|Varsayılan bir dize ise "" |Dinamik|Farklı Çalıştır hesabı parolasını belirtir. Bu, yalnızca "Etkialanıkullanıcısı" hesap türü için gereklidir. |

## <a name="runasdca"></a>RunAs_DCA

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|RunAsAccountName |Varsayılan bir dize ise "" |Dinamik|RunAs hesabı adını belirtir. Bu yalnızca "Etkialanıkullanıcısı" veya "ManagedServiceAccount" hesabı için gerekli tür. Geçerli değerler "etki alanı\kullanıcı" veya "user@domain". |
|RunAsAccountType|Varsayılan bir dize ise "" |Dinamik|Farklı Çalıştır hesap türünü gösterir. Bu, tüm RunAs bölümü geçerli değerler "LocalUser/Etkialanıkullanıcısı/NetworkService/ManagedServiceAccount/LocalSystem" için gereklidir. |
|Frk.Çal.parolası|Varsayılan bir dize ise "" |Dinamik|Farklı Çalıştır hesabı parolasını belirtir. Bu, yalnızca "Etkialanıkullanıcısı" hesap türü için gereklidir. |

## <a name="runasfabric"></a>RunAs_Fabric

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|RunAsAccountName |Varsayılan bir dize ise "" |Dinamik|RunAs hesabı adını belirtir. Bu yalnızca "Etkialanıkullanıcısı" veya "ManagedServiceAccount" hesabı için gerekli tür. Geçerli değerler "etki alanı\kullanıcı" veya "user@domain". |
|RunAsAccountType|Varsayılan bir dize ise "" |Dinamik|Farklı Çalıştır hesap türünü gösterir. Bu, tüm RunAs bölümü geçerli değerler "LocalUser/Etkialanıkullanıcısı/NetworkService/ManagedServiceAccount/LocalSystem" için gereklidir. |
|Frk.Çal.parolası|Varsayılan bir dize ise "" |Dinamik|Farklı Çalıştır hesabı parolasını belirtir. Bu, yalnızca "Etkialanıkullanıcısı" hesap türü için gereklidir. |

## <a name="runashttpgateway"></a>RunAs_HttpGateway

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|RunAsAccountName |Varsayılan bir dize ise "" |Dinamik|RunAs hesabı adını belirtir. Bu yalnızca "Etkialanıkullanıcısı" veya "ManagedServiceAccount" hesabı için gerekli tür. Geçerli değerler "etki alanı\kullanıcı" veya "user@domain". |
|RunAsAccountType|Varsayılan bir dize ise "" |Dinamik|Farklı Çalıştır hesap türünü gösterir. Bu, tüm RunAs bölümü geçerli değerler "LocalUser/Etkialanıkullanıcısı/NetworkService/ManagedServiceAccount/LocalSystem" için gereklidir. |
|Frk.Çal.parolası|Varsayılan bir dize ise "" |Dinamik|Farklı Çalıştır hesabı parolasını belirtir. Bu, yalnızca "Etkialanıkullanıcısı" hesap türü için gereklidir. |

## <a name="security"></a>Güvenlik
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi**| **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|AADCertEndpointFormat|Varsayılan bir dize ise ""|Statik|AAD sertifikası uç nokta Azure kamu gibi varsayılan olmayan ortamda belirtilen biçimi, varsayılan Azure ticari "https:\//login.microsoftonline.us/{0}/federationmetadata/2007-06/federationmetadata.xml" |
|AADClientApplication|Varsayılan bir dize ise ""|Statik|Yerel istemci uygulama adı veya kimliği Fabric istemcileri temsil eden |
|AADClusterApplication|Varsayılan bir dize ise ""|Statik|Web API uygulaması adı veya küme temsil eden kimliği |
|AADLoginEndpoint|Varsayılan bir dize ise ""|Statik|AAD oturum açma için Azure kamu gibi ortam varsayılan olmayan belirtilen uç noktası, varsayılan Azure ticari "https:\//login.microsoftonline.us" |
|AADTenantId|Varsayılan bir dize ise ""|Statik|Kiracı kimliği (GUID) |
|AdminClientCertThumbprints|Varsayılan bir dize ise ""|Dinamik|Yönetici rolünde istemcileri tarafından kullanılan sertifika parmak izleri. Bu ad virgülle ayrılmış listesidir. |
|AADTokenEndpointFormat|Varsayılan bir dize ise ""|Statik|AAD belirteci Azure kamu gibi varsayılan olmayan ortamı için belirtilen uç noktası, varsayılan Azure ticari "https:\//login.microsoftonline.us/{0}" |
|AdminClientClaims|Varsayılan bir dize ise ""|Dinamik|Tüm olası talepler; yönetici istemcilerden bekleniyor ClientClaims aynı biçimi; Bu liste ClientClaims için dahili olarak eklenen; Ayrıca aynı girişleri için ClientClaims ekleme gerek yoktur. |
|AdminClientIdentities|Varsayılan bir dize ise ""|Dinamik|Windows fabric istemciler, yönetici rolünde kimliklerini; Ayrıcalıklı fabric işlemleri yetkilendirmek için kullanılır. Bir virgülle ayrılmış listesi olduğu; Her bir etki alanı hesap adı veya grup adı girdidir. Kolaylık olması için; fabric.exe çalıştıran hesap yöneticisi rolüne otomatik olarak atanır; Bu nedenle olan ServiceFabricAdministrators gruplandırın. |
|AppRunAsAccountGroupX509Folder|/home/sfuser/sfusercerts varsayılan bir dize ise |Statik|AppRunAsAccountGroup X509 sertifikaları ve özel anahtarları bulunduğu klasörü |
|CertificateExpirySafetyMargin|Zaman aralığı, Common::TimeSpan::FromMinutes(43200) varsayılandır|Statik|Saniye cinsinden zaman aralığı belirtin. Sertifika süre sonu için güvenlik kenar boşluğu; sona erme bu değerden daha yakın olduğunda sertifika durumu rapor Tamam'a uyarı olarak değiştiğini. Varsayılan değer 30 gündür. |
|CertificateHealthReportingInterval|Zaman aralığı, Common::TimeSpan::FromSeconds(3600 * 8) varsayılandır|Statik|Saniye cinsinden zaman aralığı belirtin. Sistem durumu sertifikası raporlama aralığı belirtin; Varsayılan olarak 8 saat; Sertifika sağlık raporlaması için 0 ayarı devre dışı bırakır |
|ClientCertThumbprints|Varsayılan bir dize ise ""|Dinamik|Kümeye konuşmak için istemciler tarafından kullanılan sertifikaların parmak izleriyle; Küme kullanır bu gelen bağlantı yetkilendirme. Bu ad virgülle ayrılmış listesidir. |
|ClientClaimAuthEnabled|bool, varsayılan FALSE olur.|Statik|Talep tabanlı kimlik doğrulaması istemcilerde etkin olup olmadığını gösterir. Bu örtük olarak true ayarı ClientRoleEnabled ayarlar. |
|ClientClaims|Varsayılan bir dize ise ""|Dinamik|Tüm olası talepler ağ geçidine bağlanmak için istemcilerden bekleniyor. Bu, 'OR' listesi verilmiştir: ClaimsEntry \| \| ClaimsEntry \| \| ClaimsEntry... her ClaimsEntry "Ve" bir listesidir: ClaimType ClaimValue = & & ClaimType ClaimValue = & & ClaimType ClaimValue =... |
|ClientIdentities|Varsayılan bir dize ise ""|Dinamik|FabricClient kimliklerini Windows; adlandırma ağ geçidi gelen bağlantıları korunmasına yetki vermek için bunu kullanır. Bir virgülle ayrılmış listesi olduğu; Her bir etki alanı hesap adı veya grup adı girdidir. Kolaylık olması için; fabric.exe çalıştıran hesabı otomatik olarak izin verilir; Bu nedenle Grup ServiceFabricAllowedUsers ve ServiceFabricAdministrators'dır. |
|ClientRoleEnabled|bool, varsayılan FALSE olur.|Statik|İstemci rolü etkin olup olmadığını gösterir. olarak ayarlandığında true; istemciler kendi kimliği temel rol atanır. V2 için; Etkinleştirme bu istemci AdminClientCommonNames/AdminClientIdentities değil, yalnızca salt okunur işlemler yürütün anlamına gelir. |
|ClusterCertThumbprints|Varsayılan bir dize ise ""|Dinamik|Kümeye katılmak için izin verilen sertifikaların parmak izleriyle; ad virgülle ayrılmış listesi. |
|ClusterCredentialType|dize, "None" varsayılan|İzin Verilmiyor|Kümenin güvenliğini sağlamak için kullanılacak güvenlik kimlik bilgileri türünü belirtir. Geçerli değerler "Hiçbiri/X509/Windows" |
|ClusterIdentities|Varsayılan bir dize ise ""|Dinamik|Küme düğümlerinin Windows kimlikleri; Küme üyeliği yetkilendirme için kullanılır. Bir virgülle ayrılmış listesi olduğu; bir etki alanı hesap adı veya grup adı her girdidir |
|ClusterSpn|Varsayılan bir dize ise ""|İzin Verilmiyor|Hizmet asıl adı kümenin; ne zaman fabric tek etki alanı kullanıcısı (gMSA/etki alanı kullanıcı hesabı) çalışır. Kira dinleyicileri ve dinleyicileri fabric.exe içinde SPN'dir: Federasyon dinleyicileri; İç çoğaltma dinleyicileri; çalışma zamanı hizmet dinleyici ve adlandırma ağ geçidi dinleyicisi. Fabric makine hesabı çalıştırdığında bu boş bırakılmalıdır; yan bağlanma ve bu durumda, dinleyici Aktarım adresi SPN dinleyicisinden işlem. |
|CrlCheckingFlag|uint, varsayılan 0x40000000 olduğu|Dinamik|Varsayılan Sertifika zinciri doğrulama bayrağı; Bileşen özgü bayrağı tarafından geçersiz kılınmış olabilir; Örneğin Federasyon/X509CertChainFlags 0x10000000 CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN 0x40000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT 0x80000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ YALNIZCA ayarı 0 devre dışı bırakır CRL denetimi tam desteklenen değerlerin listesi için CertGetCertificateChain CertOpenStore tarafından belgelenmiştir: https://msdn.microsoft.com/library/windows/desktop/aa376078(v=vs.85).aspx |
|CrlDisablePeriod|Zaman aralığı, Common::TimeSpan::FromMinutes(15) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Ne kadar süreyle CRL denetimini belirli bir sertifika için çevrimdışı hata karşılaştıktan sonra; devre dışı CRL çevrimdışı hata göz ardı edilebilir değilse. |
|CrlOfflineHealthReportTtl|Zaman aralığı, Common::TimeSpan::FromMinutes(1440) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. |
|DisableFirewallRuleForDomainProfile| bool, varsayılan true'dur. |Statik| Güvenlik duvarı kuralı etki alanı profili için etkinleştirilmemelidir olmadığını gösterir |
|DisableFirewallRuleForPrivateProfile| bool, varsayılan true'dur. |Statik| Güvenlik duvarı kuralı özel profil için etkinleştirilmemelidir olmadığını gösterir | 
|DisableFirewallRuleForPublicProfile| bool, varsayılan true'dur. | Statik|Güvenlik duvarı kuralı için genel profil etkinleştirilmemelidir olmadığını gösterir |
|FabricHostSpn| Varsayılan bir dize ise "" |Statik| Hizmet asıl adı {fabrichost kaynağından; ne zaman fabric tek etki alanı kullanıcısı (gMSA/etki alanı kullanıcı hesabı) çalışır ve {fabrichost kaynağından makine hesabı altında çalışır. {Fabrichost kaynağından için SPN, IPC dinleyici olduğu; {fabrichost kaynağından makine hesabı altında çalıştığından varsayılan olarak boş bırakılmalıdır |
|IgnoreCrlOfflineError|bool, varsayılan FALSE olur.|Dinamik|Sunucu tarafı gelen istemci sertifikalarını doğrularken CRL çevrimdışı hata yoksay verilip verilmeyeceğini |
|IgnoreSvrCrlOfflineError|bool, varsayılan true'dur.|Dinamik|İstemci tarafı gelen sunucu sertifikalarını doğrularken CRL çevrimdışı hata yok saymak etkinleştirilip etkinleştirilmeyeceğini; Varsayılan olarak true. İptal edilen sertifikaları ile saldırıları DNS ödün gerektirir; ile daha zor istemci sertifikalarını iptal edildi. |
|ServerAuthCredentialType|dize, "None" varsayılan|Statik|FabricClient ve küme arasındaki iletişimin güvenliğini sağlamak için kullanılacak güvenlik kimlik bilgileri türünü belirtir. Geçerli değerler "Hiçbiri/X509/Windows" |
|ServerCertThumbprints|Varsayılan bir dize ise ""|Dinamik|İstemcilere konuşmak için küme tarafından kullanılan sunucu sertifikaların parmak izleriyle; istemciler bu kümenin kimliğini doğrulamak için kullanır. Bu ad virgülle ayrılmış listesidir. |
|SettingsX509StoreName| "MY" varsayılan bir dize ise| Dinamik|X509 sertifika deposunu yapılandırma koruması dokusu tarafından kullanılan |
|UseClusterCertForIpcServerTlsSecurity|bool, varsayılan FALSE olur.|Statik|IPC Sunucusu TLS güvenli hale getirmek için küme sertifikası kullanıp kullanmayacağınızı birim taşıma |
|X509Folder|/var/lib/waagent varsayılan bir dize ise|Statik|Klasör burada X509 sertifikaları ve özel anahtarları yer |

## <a name="securityadminclientx509names"></a>Güvenlik/AdminClientX509Names

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, varsayılan, Yok'tur|Dinamik|Bu, "Name" ve "Değer" çifti listesidir. Konu ortak adı veya DnsName X509, her "Name" olan Yönetim istemci işlemleri için yetkili sertifikaları. Verilen "adı", "Value" sabitleme veren için sertifika parmak izleri virgülle ayrı listesi yoktur boş, yönetici istemci sertifikaları doğrudan verenleri listesinde olması gerekir. |

## <a name="securityclientaccess"></a>Güvenlik/ClientAccess

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|ActivateNode |Varsayılan bir dize ise "Yönetici" |Dinamik| Etkinleştirme bir düğüm için güvenlik yapılandırma. |
|CancelTestCommand |Varsayılan bir dize ise "Yönetici" |Dinamik| Uçuş modunda ise belirli bir TestCommand - iptal eder. |
|CodePackageControl |Varsayılan bir dize ise "Yönetici" |Dinamik| Kod paketleri yeniden başlatılmasının Güvenlik Yapılandırması'nı tıklatın. |
|CreateApplication |Varsayılan bir dize ise "Yönetici" | Dinamik|Uygulama oluşturma Güvenlik Yapılandırması. |
|CreateComposeDeployment|Varsayılan bir dize ise "Yönetici"| Dinamik|Compose dosyaları tarafından açıklanan bir compose dağıtımı oluşturur |
|CreateGatewayResource|Varsayılan bir dize ise "Yönetici"| Dinamik|Bir ağ geçidi kaynağı oluşturma |
|CreateName |Varsayılan bir dize ise "Yönetici" |Dinamik|Adlandırma URI'si oluşturma Güvenlik Yapılandırması. |
|CreateNetwork|Varsayılan bir dize ise "Yönetici" |Dinamik|Kapsayıcı ağ oluşturur. |
|CreateService |Varsayılan bir dize ise "Yönetici" |Dinamik| Hizmet oluşturma için güvenliği yapılandırma. |
|CreateServiceFromTemplate |Varsayılan bir dize ise "Yönetici" |Dinamik|Şablondan hizmet oluşturma için güvenlik yapılandırma. |
|CreateVolume|Varsayılan bir dize ise "Yönetici"|Dinamik|Bir birim oluşturur |
|DeactivateNode |Varsayılan bir dize ise "Yönetici" |Dinamik| Bir düğüm devre dışı bırakmak için Güvenlik Yapılandırması'nı tıklatın. |
|DeactivateNodesBatch |Varsayılan bir dize ise "Yönetici" |Dinamik| Birden çok düğüm devre dışı bırakmak için Güvenlik Yapılandırması'nı tıklatın. |
|Sil |Varsayılan bir dize ise "Yönetici" |Dinamik| Güvenlik yapılandırmalarını görüntüsü için istemci silme işlemi depolayın. |
|DeleteApplication |Varsayılan bir dize ise "Yönetici" |Dinamik| Uygulama silme işlemi için güvenlik yapılandırma. |
|DeleteComposeDeployment|Varsayılan bir dize ise "Yönetici"| Dinamik|Compose dağıtımı siler |
|DeleteGatewayResource|Varsayılan bir dize ise "Yönetici"| Dinamik|Bir ağ geçidi kaynağı siler |
|DeleteName |Varsayılan bir dize ise "Yönetici" |Dinamik|Güvenlik Yapılandırması adlandırma URI'si silme işlemi. |
|DeleteNetwork|Varsayılan bir dize ise "Yönetici" |Dinamik|Kapsayıcı ağ siler |
|DeleteService |Varsayılan bir dize ise "Yönetici" |Dinamik|Hizmet silme işlemi için güvenlik yapılandırma. |
|DeleteVolume|Varsayılan bir dize ise "Yönetici"|Dinamik|Bir birim siler.| 
|EnumerateProperties |Varsayılan bir dize ise "yönetici\|\|kullanıcı" | Dinamik|Property sabit listesi Adlandırma Güvenlik Yapılandırması'nı tıklatın. |
|EnumerateSubnames |Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Güvenlik yapılandırması için adlandırma URI'si sabit listesi. |
|FileContent |Varsayılan bir dize ise "Yönetici" |Dinamik| Güvenlik Yapılandırması görüntüsü için istemci dosya aktarımı (kümeye harici) depolar. |
|FileDownload |Varsayılan bir dize ise "Yönetici" |Dinamik| Görüntü deposu istemci dosyası indirme başlatma (kümeye harici) için güvenliği yapılandırma. |
|FinishInfrastructureTask |Varsayılan bir dize ise "Yönetici" |Dinamik| Sonlandırma altyapı görevleri için güvenlik yapılandırma. |
|GetChaosReport | Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Belirtilen zaman aralığı içinde Chaos durumunu getirir. |
|GetClusterConfiguration | Varsayılan bir dize ise "yönetici\|\|kullanıcı" | Dinamik|Bir bölüme GetClusterConfiguration sevk. |
|GetClusterConfigurationUpgradeStatus | Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Bir bölüme GetClusterConfigurationUpgradeStatus sevk. |
|GetFabricUpgradeStatus |Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Küme yükseltme durumunu yoklama için Güvenlik Yapılandırması'nı tıklatın. |
|GetNodeDeactivationStatus |Varsayılan bir dize ise "Yönetici" |Dinamik| Güvenlik Yapılandırması devre dışı bırakma durumu denetleniyor. |
|GetNodeTransitionProgress | Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Düğüm geçişi komutu ilerleme durumunu almak için Güvenlik Yapılandırması'nı tıklatın. |
|GetPartitionDataLossProgress | Varsayılan bir dize ise "yönetici\|\|kullanıcı" | Dinamik|Invoke veri kaybı API çağrısı için ilerleme getirir. |
|GetPartitionQuorumLossProgress | Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Invoke çekirdek kayıp API çağrısı için ilerleme getirir. |
|GetPartitionRestartProgress | Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Bir yeniden başlatma bölüm API çağrısı için ilerleme getirir. |
|GetSecrets|Varsayılan bir dize ise "Yönetici"|Dinamik|Gizli dizi değerlerini alma |
|GetServiceDescription |Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Uzun yoklama hizmet bildirimleri ve hizmet açıklamaları okuma için güvenliği yapılandırma. |
|GetStagingLocation |Varsayılan bir dize ise "Yönetici" |Dinamik| Güvenlik Yapılandırması görüntüsü için hazırlama konumu alma istemci depolayın. |
|GetStoreLocation |Varsayılan bir dize ise "Yönetici" |Dinamik| Güvenlik Yapılandırması görüntüsü için istemci deposu konumu alma depolayın. |
|GetUpgradeOrchestrationServiceState|Varsayılan bir dize ise "Yönetici"| Dinamik|Bir bölüme GetUpgradeOrchestrationServiceState sevk |
|GetUpgradesPendingApproval |Varsayılan bir dize ise "Yönetici" |Dinamik| Bir bölüme GetUpgradesPendingApproval sevk. |
|GetUpgradeStatus |Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Uygulama yükseltme durumu yoklama için Güvenlik Yapılandırması'nı tıklatın. |
|InternalList |Varsayılan bir dize ise "Yönetici" | Dinamik|Güvenlik Yapılandırması görüntüsü için istemci Dosya listeleme işlemi (iç) depolar. |
|InvokeContainerApi|Varsayılan bir dize ise "Yönetici"|Dinamik|Kapsayıcı API çağırma |
|InvokeInfrastructureCommand |Varsayılan bir dize ise "Yönetici" |Dinamik| Altyapı görev management komutlar için güvenliği yapılandırma. |
|InvokeInfrastructureQuery |Varsayılan bir dize ise "yönetici\|\|kullanıcı" | Dinamik|Altyapı görevleri sorgulamak için Güvenlik Yapılandırması'nı tıklatın. |
|Liste |Varsayılan bir dize ise "yönetici\|\|kullanıcı" | Dinamik|Güvenlik Yapılandırması görüntüsü için istemci Dosya listeleme işlemi depolayın. |
|MoveNextFabricUpgradeDomain |Varsayılan bir dize ise "Yönetici" |Dinamik| Küme yükseltme açık bir yükseltme etki alanı sürdürülüyor için Güvenlik Yapılandırması'nı tıklatın. |
|MoveNextUpgradeDomain |Varsayılan bir dize ise "Yönetici" |Dinamik| Uygulama yükseltme ile açık bir yükseltme etki alanı sürdürülüyor için Güvenlik Yapılandırması'nı tıklatın. |
|MoveReplicaControl |Varsayılan bir dize ise "Yönetici" | Dinamik|Çoğaltma taşıyın. |
|NameExists |Varsayılan bir dize ise "yönetici\|\|kullanıcı" | Dinamik|Güvenlik Yapılandırması adlandırma URI'si bulunup bulunmadığını denetler. |
|NodeControl |Varsayılan bir dize ise "Yönetici" |Dinamik| Güvenlik Yapılandırması; başlatmak için durduruluyor; ve düğümleri yeniden başlatılıyor. |
|NodeStateRemoved |Varsayılan bir dize ise "Yönetici" |Dinamik| Kaldırılan düğüm durumu raporlama için Güvenlik Yapılandırması'nı tıklatın. |
|Ping |Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| İstemci ping için güvenliği yapılandırma. |
|PredeployPackageToNode |Varsayılan bir dize ise "Yönetici" |Dinamik| Dağıtım öncesi API. |
|PrefixResolveService |Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Uyumlu tabanlı bir hizmet ön ek çözümleme için güvenliği yapılandırma. |
|PropertyReadBatch |Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Güvenlik Yapılandırması adlandırma özelliği için okuma işlemleri. |
|PropertyWriteBatch |Varsayılan bir dize ise "Yönetici" |Dinamik|Güvenlik yapılandırmalarını adlandırma özelliği için yazma işlemleri. |
|ProvisionApplicationType |Varsayılan bir dize ise "Yönetici" |Dinamik| Uygulama sağlama türü için Güvenlik Yapılandırması'nı tıklatın. |
|ProvisionFabric |Varsayılan bir dize ise "Yönetici" |Dinamik| MSI ve/veya küme bildirim sağlamak için Güvenlik Yapılandırması'nı tıklatın. |
|Sorgu |Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Sorgular için güvenlik yapılandırması. |
|RecoverPartition |Varsayılan bir dize ise "Yönetici" | Dinamik|Bir bölüm kurtarmak için Güvenlik Yapılandırması'nı tıklatın. |
|RecoverPartitions |Varsayılan bir dize ise "Yönetici" | Dinamik|Bölümler kurtarmak için Güvenlik Yapılandırması'nı tıklatın. |
|RecoverServicePartitions |Varsayılan bir dize ise "Yönetici" |Dinamik| Hizmet bölüm kurtarmak için Güvenlik Yapılandırması'nı tıklatın. |
|RecoverSystemPartitions |Varsayılan bir dize ise "Yönetici" |Dinamik| Sistem hizmeti bölümleri kurtarmak için Güvenlik Yapılandırması'nı tıklatın. |
|RemoveNodeDeactivations |Varsayılan bir dize ise "Yönetici" |Dinamik| Güvenlik Yapılandırması birden çok düğümde geri alma devre dışı bırakma. |
|ReportFabricUpgradeHealth |Varsayılan bir dize ise "Yönetici" |Dinamik| Küme yükseltme geçerli yükseltme işlemi ilerleme durumu ile sürdürülüyor için Güvenlik Yapılandırması'nı tıklatın. |
|ReportFault |Varsayılan bir dize ise "Yönetici" |Dinamik| Hata raporlama için Güvenlik Yapılandırması'nı tıklatın. |
|ReportHealth |Varsayılan bir dize ise "Yönetici" |Dinamik| Sistem durumu raporlama için Güvenlik Yapılandırması'nı tıklatın. |
|ReportUpgradeHealth |Varsayılan bir dize ise "Yönetici" |Dinamik| Uygulama yükseltme ile geçerli yükseltme işlemi ilerleme durumu yeniden başlatma için Güvenlik Yapılandırması'nı tıklatın. |
|ResetPartitionLoad |Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Güvenlik yapılandırması için bir failoverUnit sıfırlama yük. |
|ResolveNameOwner |Varsayılan bir dize ise "yönetici\|\|kullanıcı" | Dinamik|Adlandırma URI'si sahibi çözmek için Güvenlik Yapılandırması'nı tıklatın. |
|ResolvePartition |Varsayılan bir dize ise "yönetici\|\|kullanıcı" | Dinamik|Sistem Hizmetleri çözmek için Güvenlik Yapılandırması'nı tıklatın. |
|ResolveService |Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Güvenlik Yapılandırması şikayet tabanlı hizmet çözümlemesi için. |
|ResolveSystemService|Varsayılan bir dize ise "yönetici\|\|kullanıcı"|Dinamik| Sistem Hizmetleri çözmek için güvenliği yapılandırma |
|RollbackApplicationUpgrade |Varsayılan bir dize ise "Yönetici" |Dinamik| Geri toplu uygulama yükseltmeleri için Güvenlik Yapılandırması'nı tıklatın. |
|RollbackFabricUpgrade |Varsayılan bir dize ise "Yönetici" |Dinamik| Küme yükseltme geri alma için Güvenlik Yapılandırması'nı tıklatın. |
|ServiceNotifications |Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Olay tabanlı hizmet bildirimleri için güvenliği yapılandırma. |
|SetUpgradeOrchestrationServiceState|Varsayılan bir dize ise "Yönetici"| Dinamik|Bir bölüme SetUpgradeOrchestrationServiceState sevk |
|StartApprovedUpgrades |Varsayılan bir dize ise "Yönetici" |Dinamik| Bir bölüme StartApprovedUpgrades sevk. |
|StartChaos |Varsayılan bir dize ise "Yönetici" |Dinamik| Zaten başlatılmadığında Chaos - başlatır. |
|StartClusterConfigurationUpgrade |Varsayılan bir dize ise "Yönetici" |Dinamik| Bir bölüme StartClusterConfigurationUpgrade sevk. |
|StartInfrastructureTask |Varsayılan bir dize ise "Yönetici" | Dinamik|Altyapısı görevlerini başlatmak için Güvenlik Yapılandırması'nı tıklatın. |
|StartNodeTransition |Varsayılan bir dize ise "Yönetici" |Dinamik| Düğüm geçişi başlatmak için Güvenlik Yapılandırması'nı tıklatın. |
|StartPartitionDataLoss |Varsayılan bir dize ise "Yönetici" |Dinamik| Veri kaybı bir bölüme sevk. |
|StartPartitionQuorumLoss |Varsayılan bir dize ise "Yönetici" |Dinamik| Çekirdek kayıp bir bölüme sevk. |
|StartPartitionRestart |Varsayılan bir dize ise "Yönetici" |Dinamik| Bir bölüm çoğaltmalarını bazıları veya tümü aynı anda başlatır. |
|StopChaos |Varsayılan bir dize ise "Yönetici" |Dinamik| Başlatılmış olup olmadığını Chaos - durdurur. |
|ToggleVerboseServicePlacementHealthReporting | Varsayılan bir dize ise "yönetici\|\|kullanıcı" |Dinamik| Ayrıntılı ServicePlacement HealthReporting geçiş için güvenlik yapılandırma. |
|UnprovisionApplicationType |Varsayılan bir dize ise "Yönetici" |Dinamik| Uygulama türünün sağlamasını kaldırma işlemini için Güvenlik Yapılandırması'nı tıklatın. |
|UnprovisionFabric |Varsayılan bir dize ise "Yönetici" |Dinamik| MSI ve/veya küme bildirim sağlamanın kaldırılması için Güvenlik Yapılandırması'nı tıklatın. |
|UnreliableTransportControl |Varsayılan bir dize ise "Yönetici" |Dinamik| Ekleme ve kaldırma davranışları için güvenilir olmayan aktarım. |
|UpdateService |Varsayılan bir dize ise "Yönetici" |Dinamik|Hizmet güncelleştirmeleri için güvenlik yapılandırma. |
|UpgradeApplication |Varsayılan bir dize ise "Yönetici" |Dinamik| Başlatma ve uygulama yükseltmeleri kesintiye Güvenlik Yapılandırması'nı tıklatın. |
|UpgradeComposeDeployment|Varsayılan bir dize ise "Yönetici"| Dinamik|Compose dağıtımı yükseltme |
|UpgradeFabric |Varsayılan bir dize ise "Yönetici" |Dinamik| Küme yükseltme başlatmak için Güvenlik Yapılandırması'nı tıklatın. |
|Karşıya Yükle |Varsayılan bir dize ise "Yönetici" | Dinamik|Güvenlik Yapılandırması görüntüsü için istemci yükleme işlemi depolayın. |

## <a name="securityclientcertificateissuerstores"></a>Güvenlik/ClientCertificateIssuerStores

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|IssuerStoreKeyValueMap, varsayılan, Yok'tur |Dinamik|X509 sertifikayı depolayan istemci sertifikaları için; Adı = clientIssuerCN; Değer = depoları virgülle ayrılmış listesi |

## <a name="securityclientx509names"></a>Güvenlik/ClientX509Names

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, varsayılan, Yok'tur|Dinamik|Bu, "Name" ve "Değer" çifti listesidir. Konu ortak adı veya DnsName X509, her "Name" olan istemci işlemleri için yetkili sertifikaları. Verilen "adı", "Value" virgülle ayrı sabitleme veren için sertifika parmak izleri listesidir değilse boş, istemci sertifikaları doğrudan verenleri listesinde olması gerekir.|

## <a name="securityclustercertificateissuerstores"></a>Güvenlik/ClusterCertificateIssuerStores

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|IssuerStoreKeyValueMap, varsayılan, Yok'tur |Dinamik|X509 sertifikayı depolayan küme sertifikaları için; Adı = clusterIssuerCN; Değer = depoları virgülle ayrılmış listesi |

## <a name="securityclusterx509names"></a>Güvenlik/ClusterX509Names

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, varsayılan, Yok'tur|Dinamik|Bu, "Name" ve "Değer" çifti listesidir. Konu ortak adı veya DnsName X509, her "Name" olan küme işlemleri için yetkili sertifikaları. Verilen "adı", "Value" virgülle ayrı sabitleme veren için sertifika parmak izleri listesidir değilse boş küme sertifikaları doğrudan verenleri listesinde olması gerekir.|

## <a name="securityservercertificateissuerstores"></a>Güvenlik/ServerCertificateIssuerStores

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|IssuerStoreKeyValueMap, varsayılan, Yok'tur |Dinamik|X509 yayıncı sertifikası için sunucu sertifikaları; depolar Adı = serverIssuerCN; Değer = depoları virgülle ayrılmış listesi |

## <a name="securityserverx509names"></a>Güvenlik/ServerX509Names

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|PropertyGroup|X509NameMap, varsayılan, Yok'tur|Dinamik|Bu, "Name" ve "Değer" çifti listesidir. Konu ortak adı veya DnsName X509, her "Name" olan sunucu işlemleri için yetkili sertifikaları. Verilen "adı", "Value" virgülle ayrı sabitleme veren için sertifika parmak izleri listesidir değilse boş sunucu sertifikaları doğrudan verenleri listesinde olması gerekir.|

## <a name="setup"></a>Kurulum

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|ContainerNetworkName|Varsayılan bir dize ise ""| Statik |Bir kapsayıcı ağı ayarlarken kullanılacak ağ adı.|
|ContainerNetworkSetup|bool, varsayılan FALSE olur.| Statik |Bir kapsayıcı ağ ayarlanıp ayarlanmayacağını belirtir.|
|FabricDataRoot |String | İzin Verilmiyor |Service Fabric veri kök dizini. Varsayılan Azure d:\svcfab için |
|FabricLogRoot |String | İzin Verilmiyor |Service fabric günlük kök dizini. SF günlüklerinden ve izlemelerinden yerleştirildiği budur. |
|NodesToBeRemoved|Varsayılan bir dize ise ""| Dinamik |Yapılandırma yükseltmesinin bir parçası kaldırılması gerektiğini düğümleri. (Yalnızca için tek başına dağıtımlarında)|
|ServiceRunAsAccountName |String | İzin Verilmiyor |Hesap adı altında çalıştırılacağı fabric konak hizmeti. |
|SkipContainerNetworkResetOnReboot|bool, varsayılan FALSE olur.|NotAllowed|Mı sıfırlama kapsayıcı ağ yeniden başlatıldığında atlanacak.|
|SkipFirewallConfiguration |Bool, varsayılan değer false'tur | İzin Verilmiyor |Güvenlik Duvarı ayarlarını veya sistem tarafından ayarlanmış olması gerekip gerekmediğini belirtir. Bu, yalnızca windows güvenlik duvarı kullanıyorsanız geçerlidir. Daha sonra üçüncü taraf güvenlik duvarları kullanıyorsanız, sistem ve uygulamalara için bağlantı noktalarını açmanız gerekir |

## <a name="tokenvalidationservice"></a>TokenValidationService

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|Sağlayıcılar |"DSTS" varsayılan bir dize ise |Statik|Belirteç doğrulama sağlayıcılarının etkinleştirmek için virgülle ayrılmış listesi (geçerli sağlayıcıları şunlardır: DSTS; AAD). Şu anda yalnızca tek bir sağlayıcı dilediğiniz zaman etkinleştirilebilir. |

## <a name="traceetw"></a>İzleme/Etw

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|Düzey |Int, varsayılan 4'tür | Dinamik |Değerler 1, izleme etw düzeyi gerçekleştirebileceğiniz 2, 3, 4. Desteklenmesi için izleme düzeyi 4 olarak tutmanız gerekir |

## <a name="transactionalreplicator"></a>TransactionalReplicator

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|BatchAcknowledgementInterval | Zamanı saniye cinsinden 0,015 varsayılandır | Statik | Saniye cinsinden zaman aralığı belirtin. Bir bildirim geri göndermeden önce bir işlem aldıktan sonra çoğaltıcı beklediği süreyi belirler. Bu süre içinde alınan diğer işlemler, potansiyel olarak çoğaltıcı hacmini azaltır ancak azalan ağ trafiği -> tek bir iletide gönderilen, onayları sahip olur. |
|MaxCopyQueueSize |Uint 16384 varsayılandır | Statik |Bu, en yüksek değeri ilk çoğaltma işlemleri tutar kuyruk boyutunu tanımlar. Bu 2'in kuvveti olması gerektiğini unutmayın. Çalışma zamanı sırasında bu boyutu işlem için kuyruk büyürse birincil ve ikincil çoğaltıcılar kısıtlanır. |
|MaxPrimaryReplicationQueueMemorySize |Uint, varsayılan 0'dır | Statik |Birincil çoğaltma kuyruğu bayt cinsinden en büyük değerini budur. |
|MaxPrimaryReplicationQueueSize |Uint 8192 varsayılandır | Statik |Birincil çoğaltma kuyrukta var olabilecek işlemlerinin maksimum sayısı budur. Bu 2'in kuvveti olması gerektiğini unutmayın. |
|(Maxreplicationmessagesize) |uint, varsayılan 52428800 olduğu | Statik | Çoğaltma işlemleri en büyük ileti boyutu. Varsayılan 50 MB'tır. |
|MaxSecondaryReplicationQueueMemorySize |Uint, varsayılan 0'dır | Statik |İkincil çoğaltma kuyruğu bayt cinsinden en büyük değerini budur. |
|MaxSecondaryReplicationQueueSize |Uint 16384 varsayılandır | Statik |Bu ikincil çoğaltma kuyrukta var olabilecek işlemleri en büyük sayısıdır. Bu 2'in kuvveti olması gerektiğini unutmayın. |
|ReplicatorAddress |"localhost:0" varsayılan bir dize ise | Statik | Uç nokta biçiminde bir dize-'Windows Fabric Çoğaltıcısı tarafından işlemleri gönderme ve alma için diğer yinelemeler ile bağlantı kurmak için kullanılan IP: BağlantıNoktası'. |

## <a name="transport"></a>Aktarım
| **Parametre** | **İzin verilen değerler** |**Yükseltme İlkesi** |**Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|ConnectionOpenTimeout|Zaman aralığı, Common::TimeSpan::FromSeconds(60) varsayılandır|Statik|Saniye cinsinden zaman aralığı belirtin. Zaman aşımı (güvenlik anlaşması güvenli modda dahil), hem gelen hem de kabul tarafında bağlantı Kur |
|FrameHeaderErrorCheckingEnabled|bool, varsayılan true'dur.|Statik|Varsayılan hata güvenli olmayan modda çerçeve başlığındaki denetimi için ayarlar; bileşen ayarı bu geçersiz kılar. |
|MessageErrorCheckingEnabled|bool, varsayılan FALSE olur.|Statik|Varsayılan ileti üst bilgisi ve gövdesi'güvenli olmayan modda denetlemede hata için ayar; bileşen ayarı bu geçersiz kılar. |
|ResolveOption|dize, "belirsiz" varsayılandır|Statik|FQDN nasıl çözümleneceğini belirler.  Geçerli değerler "belirtilmeyen/IPv4/IPv6". |
|SendTimeout|Zaman aralığı, Common::TimeSpan::FromSeconds(300) varsayılandır|Dinamik|Saniye cinsinden zaman aralığı belirtin. Takılan bağlantıyı algılamak için zaman aşımı gönderin. TCP hata raporları, bazı ortamda güvenilir değildir. Bu kullanılabilir ağ bant genişliği ve giden veri boyutuna göre ayarlanması gerekebilir (\*MaxMessageSize\/\*SendQueueSizeLimit). |

## <a name="upgradeorchestrationservice"></a>UpgradeOrchestrationService

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|AutoupgradeEnabled | Bool, varsayılan değer true şeklindedir |Statik| Otomatik yoklama ve hedef durumu dosyasını temel alarak yükseltme eylemi. |
|AutoupgradeInstallEnabled|bool, varsayılan FALSE olur.|Statik|Eylem hedef durumu dosyasını temel alarak otomatik yoklama, sağlama ve kod yüklemesini yükseltin.|
|GoalStateExpirationReminderInDays|int, varsayılan 30'dur|Statik|Sonra hedef durumu anımsatıcı gösterilecek kalan gün sayısını ayarlar.|
|MinReplicaSetSize |int, varsayılan 0'dır |Statik |MinReplicaSetSize UpgradeOrchestrationService için.|
|PlacementConstraints | Varsayılan bir dize ise "" |Statik| PlacementConstraints UpgradeOrchestrationService için. |
|QuorumLossWaitDuration | Zamanı saniye cinsinden MaxValue varsayılandır |Statik| Saniye cinsinden zaman aralığı belirtin. QuorumLossWaitDuration UpgradeOrchestrationService için. |
|ReplicaRestartWaitDuration | Zamanı saniye cinsinden varsayılan değer 60 dakikadır|Statik| Saniye cinsinden zaman aralığı belirtin. ReplicaRestartWaitDuration UpgradeOrchestrationService için. |
|StandByReplicaKeepDuration | Zaman içinde varsayılan 60 saniyedir*24*7 dakika |Statik| Saniye cinsinden zaman aralığı belirtin. StandByReplicaKeepDuration UpgradeOrchestrationService için. |
|TargetReplicaSetSize |int, varsayılan 0'dır |Statik |TargetReplicaSetSize UpgradeOrchestrationService için. |
|UpgradeApprovalRequired | Bool, varsayılan değer false'tur | Statik|Devam etmeden önce yönetici onayı iste kod yükseltme yapmak için ayarlanıyor. |

## <a name="upgradeservice"></a>UpgradeService

| **Parametre** | **İzin verilen değerler** | **Yükseltme İlkesi** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
|BaseUrl | Varsayılan bir dize ise "" |Statik|BaseUrl UpgradeService için. |
|Lclusterıd | Varsayılan bir dize ise "" |Statik|Lclusterıd UpgradeService için. |
|CoordinatorType | "WUTest" varsayılan bir dize ise|İzin Verilmiyor|CoordinatorType UpgradeService için. |
|MinReplicaSetSize | Varsayılan Int, 2'dir |İzin Verilmiyor| MinReplicaSetSize UpgradeService için. |
|OnlyBaseUpgrade | Bool, varsayılan değer false'tur |Dinamik|OnlyBaseUpgrade UpgradeService için. |
|PlacementConstraints |Varsayılan bir dize ise "" |İzin Verilmiyor|Yükseltme hizmeti PlacementConstraints. |
|PollIntervalInSeconds|Zaman aralığı, Common::TimeSpan::FromSeconds(60) varsayılandır |Dinamik|Saniye cinsinden zaman aralığı belirtin. ARM yönetim işlemleri için UpgradeService yoklama zaman aralığıdır. |
|TargetReplicaSetSize | Int, varsayılan 3'tür |İzin Verilmiyor| TargetReplicaSetSize UpgradeService için. |
|TestCabFolder | Varsayılan bir dize ise "" |Statik| TestCabFolder UpgradeService için. |
|X509FindType | Varsayılan bir dize ise ""|Dinamik| X509FindType UpgradeService için. |
|X509FindValue | Varsayılan bir dize ise "" |Dinamik| X509FindValue UpgradeService için. |
|X509SecondaryFindValue | Varsayılan bir dize ise "" |Dinamik| X509SecondaryFindValue UpgradeService için. |
|X509StoreLocation | Varsayılan bir dize ise "" |Dinamik| X509StoreLocation UpgradeService için. |
|X509StoreName | Varsayılan bir dize ise "My"|Dinamik|X509StoreName UpgradeService için. |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için [Azure kümesine yapılandırmasını yükseltme](service-fabric-cluster-config-upgrade-azure.md) ve [tek başına küme yapılandırmasını yükseltme](service-fabric-cluster-config-upgrade-windows-server.md).
