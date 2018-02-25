---
title: "Service Fabric kümesi güvenlik: istemci rolleri | Microsoft Docs"
description: "Bu makalede, iki istemci rolleri ve rollere sağlanan izinleri açıklar."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: coreysa
editor: 
ms.assetid: 7bc808d9-3609-46a1-ac12-b4f53bff98dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: e71275d437aabdd5699f0d462fddc5a190ff19db
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="role-based-access-control-for-service-fabric-clients"></a>Service Fabric istemciler için rol tabanlı erişim denetimi
Azure Service Fabric Service Fabric kümeye bağlı istemciler için iki farklı erişim denetim türlerini destekler: Yönetici ve kullanıcı. Erişim denetimi, belirli küme işlemleri farklı küme daha güvenli hale getirme kullanıcı grupları için erişimi sınırlamak Küme Yöneticisi sağlar.  

**Yöneticiler** yönetim özellikleri (okuma/yazma özellikleri dahil) tam erişimi vardır. Varsayılan olarak, **kullanıcılar** yalnızca yönetim özellikleri (örneğin, sorgu özellikleri) okuma erişimi ve uygulamaları ve Hizmetleri çözümlenebilmesi gerekir.

Küme oluşturma sırasında her biri için ayrı sertifikaların sağlayarak iki istemci rolleri (Yönetici ve istemci) belirtin. Bkz: [Service Fabric kümesi güvenlik](service-fabric-cluster-security.md) güvenli bir Service Fabric kümesi ayarlama hakkında bilgi.

## <a name="default-access-control-settings"></a>Varsayılan erişim denetimi ayarları
Yönetici erişim denetim türü için tüm FabricClient API'leri tam erişimi vardır. Service Fabric kümesi üzerinde aşağıdaki işlemleri dahil olmak üzere tüm okuma ve yazma işlemleri gerçekleştirebilirsiniz:

### <a name="application-and-service-operations"></a>Uygulama ve hizmet işlemleri
* **CreateService**: hizmet oluşturma                             
* **CreateServiceFromTemplate**: hizmet şablonundan oluşturma                             
* **UpdateService**: hizmet güncelleştirmeleri                             
* **DeleteService**: Hizmet silme                             
* **ProvisionApplicationType**: uygulama sağlama türünü                             
* **CreateApplication**: uygulama oluşturma                               
* **DeleteApplication**: uygulama silme                             
* **UpgradeApplication**: başlatılması veya uygulama yükseltmelerini kesintiye                             
* **UnprovisionApplicationType**: uygulama türü sağlamanın kaldırılması                             
* **MoveNextUpgradeDomain**: uygulama yükseltmelerini açıkça güncelleştirme etki alanı ile sürdürülüyor                             
* **ReportUpgradeHealth**: uygulama yükseltmelerini geçerli yükseltme ilerleme durumu ile sürdürülüyor                             
* **ReportHealth**: sistem durumu raporlama                             
* **PredeployPackageToNode**: dağıtım öncesi API                            
* **CodePackageControl**: kod paketler yeniden başlatılıyor                             
* **RecoverPartition**: bir bölüm kurtarma                             
* **RecoverPartitions**: bölümleri kurtarma                             
* **RecoverServicePartitions**: hizmet bölümlerini kurtarma                             
* **RecoverSystemPartitions**: sistem hizmeti bölümleri kurtarma                             

### <a name="cluster-operations"></a>Küm işlemleri
* **ProvisionFabric**: MSI ve/veya küme bildirim sağlama                             
* **UpgradeFabric**: Küme yükseltme başlatılıyor                             
* **UnprovisionFabric**: MSI ve/veya küme bildirimi sağlamanın kaldırılması                         
* **MoveNextFabricUpgradeDomain**: Küme yükseltme açıkça güncelleştirme etki alanı ile sürdürülüyor                             
* **ReportFabricUpgradeHealth**: Küme yükseltme geçerli yükseltme ilerleme durumu ile sürdürülüyor                             
* **StartInfrastructureTask**: altyapı görevleri başlatma                             
* **FinishInfrastructureTask**: altyapı görevleri tamamlama                             
* **InvokeInfrastructureCommand**: altyapı görev yönetimi komutları                              
* **ActivateNode**: bir düğümü etkinleştirme                             
* **DeactivateNode**: bir düğümü devre dışı bırakma                             
* **DeactivateNodesBatch**: birden çok düğüm devre dışı bırakma                             
* **RemoveNodeDeactivations**: birden çok düğümde geri alma devre dışı bırakma                             
* **GetNodeDeactivationStatus**: devre dışı bırakma durumu denetleniyor                             
* **NodeStateRemoved**: düğüm durumu raporlama kaldırıldı                             
* **ReportFault**: hata raporlama                             
* **FileContent**: görüntü deposu istemci dosya aktarımı (kümeye harici)                             
* **FileDownload**: görüntü deposu istemci dosya indirme başlatma (kümeye harici)                             
* **InternalList**: görüntü deposu istemci dosya listesi işlemi (iç)                             
* **Silme**: görüntü deposu istemci silme işlemi                              
* **Karşıya yükleme**: Image store istemcisi karşıya yükleme işlemi                             
* **NodeControl**: başlatma, durdurma ve düğüm yeniden başlatma                             
* **MoveReplicaControl**: çoğaltmaları bir düğümden diğerine taşıma                             

### <a name="miscellaneous-operations"></a>Çeşitli işlemler
* **Ping**: istemci ping isteği                             
* **Sorgu**: izin verilen tüm sorgular
* **NameExists**: URI varlığı denetimleri adlandırma                             

Kullanıcı erişim denetimi türü varsayılan olarak, aşağıdaki işlemleri sınırlı olduğu: 

* **EnumerateSubnames**: URI numaralandırması adlandırma                             
* **EnumerateProperties**: özellik numaralandırma adlandırma                             
* **PropertyReadBatch**: özellik adlandırma okuma işlemleri                             
* **GetServiceDescription**: uzun yoklama hizmet bildirimleri ve okuma hizmeti açıklamaları                             
* **ResolveService**: hizmet uyumlu tabanlı çözümleme                             
* **ResolveNameOwner**: çözümleme adlandırma URI'si sahibi                             
* **ResolvePartition**: Sistem Hizmetleri çözme                             
* **ServiceNotifications**: olay tabanlı hizmet bildirimleri                             
* **GetUpgradeStatus**: Uygulama yükseltme durumunu yoklama                             
* **GetFabricUpgradeStatus**: Küme yükseltme durumunu yoklama                             
* **InvokeInfrastructureQuery**: altyapısı görevlerini sorgulama                             
* **Liste**: görüntü deposu istemci dosya listesi işlemi                             
* **ResetPartitionLoad**: bir yük devretme biriminin yükünü sıfırlama                             
* **ToggleVerboseServicePlacementHealthReporting**: toggling verbose service placement health reporting                             

Yönetici erişim denetimi önceki işlemleri de erişebilir.

## <a name="changing-default-settings-for-client-roles"></a>İstemci rolleri için varsayılan ayarlarını değiştirme
Küme bildirim dosyasında istemciye gerekirse yönetim yetenekleri sağlayabilirsiniz. Giderek Varsayılanları değiştirebilirsiniz **yapı ayarları** sırasında seçeneği [küme oluşturma](service-fabric-cluster-creation-via-portal.md), önceki ayarlar sağlayarak ve **adı**, **yönetici**, **kullanıcı**, ve **değeri** alanları.

## <a name="next-steps"></a>Sonraki adımlar
[Service Fabric kümesi güvenliği](service-fabric-cluster-security.md)

[Service Fabric kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)

