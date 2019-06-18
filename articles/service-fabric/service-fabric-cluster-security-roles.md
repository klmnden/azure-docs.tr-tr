---
title: 'Service Fabric kümesi güvenliği: istemci rolleri | Microsoft Docs'
description: Bu makalede iki istemci rolleri ve sağlanan rollere izinleri açıklar.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: coreysa
editor: ''
ms.assetid: 7bc808d9-3609-46a1-ac12-b4f53bff98dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: ed000dc4be1ae45382d688d4a596ec745c69d0bb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60711170"
---
# <a name="role-based-access-control-for-service-fabric-clients"></a>Service Fabric istemciler için rol tabanlı erişim denetimi
Azure Service Fabric, bir Service Fabric kümesine bağlanan istemciler için iki farklı erişim denetim türlerini destekler: Yönetici ve kullanıcı. Küme Yöneticisi, belirli küme işlemleri farklı kümeye daha güvenli hale getirme, kullanıcı grupları için erişimi sınırlandırmak erişim denetimi sağlar.  

**Yöneticiler** yönetim özellikleri (okuma/yazma özellikleri dahil olmak üzere) tam erişime sahiptir. Varsayılan olarak, **kullanıcılar** yalnızca yönetim özelliklerine (örneğin, sorgu işlevleri) okuma erişimi ve uygulamaları ve Hizmetleri çözümlenebilmesi gerekir.

Küme oluşturma sırasında her biri için ayrı sertifikalar sağlayarak iki istemci rolleri (Yönetici ve istemci) belirtin. Bkz: [Service Fabric küme güvenliği](service-fabric-cluster-security.md) güvenli bir Service Fabric kümesi oluşturma hakkında bilgi.

## <a name="default-access-control-settings"></a>Varsayılan erişim denetimi ayarları
Yönetici erişim denetim türü FabricClient API'leri tam erişimi vardır. Aşağıdaki işlemleri dahil olmak üzere Service Fabric kümesinde herhangi bir okuma ve yazma işlemleri gerçekleştirebilirsiniz:

### <a name="application-and-service-operations"></a>Uygulama ve hizmet işlemleri
* **CreateService**: hizmet oluşturma                             
* **CreateServiceFromTemplate**: hizmet şablonundan oluşturma                             
* **UpdateService**: hizmet güncelleştirmeleri                             
* **DeleteService**: Hizmet silme                             
* **ProvisionApplicationType**: uygulama sağlama türü                             
* **CreateApplication**: uygulama oluşturma                               
* **DeleteApplication**: uygulama silme                             
* **UpgradeApplication**: başlangıç veya uygulama yükseltmeleri engellemeden                             
* **UnprovisionApplicationType**: uygulama türünün sağlamasını kaldırma işlemini                             
* **MoveNextUpgradeDomain**: Uygulama yükseltme ile açık bir güncelleme etki alanı sürdürülüyor                             
* **ReportUpgradeHealth**: uygulama yükseltmeleri geçerli yükseltme işlemi ilerleme durumu ile sürdürülüyor                             
* **ReportHealth**: sağlık raporlaması                             
* **PredeployPackageToNode**: dağıtım öncesi API                            
* **CodePackageControl**: kod paketleri yeniden başlatılıyor                             
* **RecoverPartition**: bir bölüm kurtarma                             
* **RecoverPartitions**: bölümler kurtarma                             
* **RecoverServicePartitions**: hizmeti bölümlerini kurtarma                             
* **RecoverSystemPartitions**: sistem hizmeti bölümleri kurtarma                             

### <a name="cluster-operations"></a>Küm işlemleri
* **ProvisionFabric**: MSI ve/veya küme sağlama bildirimi                             
* **UpgradeFabric**: Küme yükseltme başlatılıyor                             
* **UnprovisionFabric**: Sağlamasını kaldırma işlemini MSI ve/veya küme bildirimi                         
* **MoveNextFabricUpgradeDomain**: küme yükseltmeleriyle açık güncelleme etki alanı sürdürülüyor                             
* **ReportFabricUpgradeHealth**: Küme yükseltme geçerli yükseltme işlemi ilerleme durumu ile sürdürülüyor                             
* **StartInfrastructureTask**: altyapı bir görevini başlatmadan                             
* **FinishInfrastructureTask**: altyapı görevleri tamamlama                             
* **InvokeInfrastructureCommand**: altyapı görev management komutlar                              
* **ActivateNode**: düğüm etkinleştirme                             
* **DeactivateNode**: bir düğüm devre dışı bırakma                             
* **DeactivateNodesBatch**: birden çok düğüm devre dışı bırakma                             
* **RemoveNodeDeactivations**: birden çok düğümde geri alma devre dışı bırakma                             
* **GetNodeDeactivationStatus**: devre dışı bırakma durumunu denetleme                             
* **NodeStateRemoved**: düğüm durumu raporlama kaldırıldı                             
* **ReportFault**: hata raporlama                             
* **FileContent**: görüntü deposu istemci dosya aktarımı (Dış küme)                             
* **FileDownload**: görüntü deposu istemci dosyası indirme başlatma (kümeye dış)                             
* **InternalList**: görüntü deposu istemci Dosya listeleme işlemi (iç)                             
* **Silme**: görüntü deposu istemci silme işlemi                              
* **Karşıya yükleme**: görüntü deposu istemci karşıya yükleme işlemi                             
* **NodeControl**: başlatılmasını, durdurmasını ve düğümleri yeniden başlatılıyor                             
* **MoveReplicaControl**: çoğaltmalar bir düğümden diğerine taşıma                             

### <a name="miscellaneous-operations"></a>Çeşitli işlemler
* **Ping**: istemci ping                             
* **Sorgu**: izin verilen tüm sorgular
* **NameExists**: URI varlığı denetimleri adlandırma                             

Kullanıcı erişim denetimi türü varsayılan olarak şu işlemler için sınırlı verilmiştir: 

* **EnumerateSubnames**: URI numaralandırma adlandırma                             
* **EnumerateProperties**: property sabit listesi adlandırma                             
* **PropertyReadBatch**: özellik adlarının okuma işlemleri                             
* **GetServiceDescription**: uzun yoklama hizmet bildirimleri ve okuma hizmet açıklamaları                             
* **ResolveService**: şikayet tabanlı hizmet çözümleme                             
* **ResolveNameOwner**: çözümleme adlandırma URI'si sahibi                             
* **ResolvePartition**: Sistem Hizmetleri Çözümleme                             
* **ServiceNotifications**: olay tabanlı hizmet bildirimleri                             
* **GetUpgradeStatus**: Uygulama yükseltme durumunu yoklama                             
* **GetFabricUpgradeStatus**: Küme yükseltme durumunu yoklama                             
* **InvokeInfrastructureQuery**: altyapısı görevlerini sorgulama                             
* **Liste**: görüntü deposu istemci Dosya listeleme işlemi                             
* **ResetPartitionLoad**: bir yük devretme biriminin yük sıfırlanıyor                             
* **ToggleVerboseServicePlacementHealthReporting**: ayrıntılı hizmet yerleştirme sağlık raporlaması geçiş                             

Yönetici erişim denetimi önceki işlemleri de erişebilir.

## <a name="changing-default-settings-for-client-roles"></a>İstemci rolleri için varsayılan ayarları değiştirme
Küme bildirim dosyasında istemciye gerekirse yönetici özellikleri sağlayabilir. Varsayılanları giderek değiştirebilirsiniz **yapı ayarları** sırasında seçeneği [küme oluşturma](service-fabric-cluster-creation-via-portal.md), önceki ayarlar sağlayarak **adı**,  **Yönetici**, **kullanıcı**, ve **değer** alanları.

## <a name="next-steps"></a>Sonraki adımlar
[Service Fabric küme güvenliği](service-fabric-cluster-security.md)

[Service Fabric kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)

