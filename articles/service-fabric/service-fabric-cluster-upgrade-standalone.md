---
title: Bir Azure Service Fabric tek başına Küme yükseltme | Microsoft Docs
description: Sürüm veya bir Azure Service Fabric tek başına küme yapılandırmasını yükseltme hakkında bilgi edinin.  P
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/12/2018
ms.author: aljo
ms.openlocfilehash: 1d96a2e81917af5e80bb847ea25610ccb71ad70f
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661506"
---
# <a name="upgrading-and-updating-a-service-fabric-standalone-cluster"></a>Yükseltme ve Service Fabric tek başına küme güncelleştiriliyor

Herhangi bir modern sistemi için upgradability yönelik uzun vadeli ürününüzün başarısını ulaşmak için anahtardır. Bir Azure Service Fabric tek başına küme sahip olduğunuz bir kaynaktır. Bu makalede, hangi yükseltme veya güncelleştirilebilen açıklanır.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Kümenizde çalışan yapı sürümü denetleme
Kümeniz her zaman çalıştığından emin olun bir [Service Fabric sürümü desteklenen](service-fabric-versions.md). Microsoft, Service Fabric, yeni bir sürüm olduğunu bildirir, önceki sürümü en az Duyurunun tarihinden 60 gün sonra destek sonu için işaretlenir. Yeni yayınlar duyurulan [Service Fabric Ekibi blogunda](https://blogs.msdn.microsoft.com/azureservicefabric/). Bu noktada seçmek yeni sürüm kullanılabilir.

Kümenizi, kümenizin olmasını istediğiniz bir desteklenen yapı sürümü el ile seçebilirsiniz veya Microsoft tarafından yayımlanır yayımlanmaz otomatik yapı yükseltmeleri almak için ayarlayabilirsiniz. Daha fazla bilgi için okuma [kümenizde çalışan Service Fabric sürümüne yükseltme](service-fabric-cluster-upgrade-windows-server.md).

## <a name="customize-configuration-settings"></a>Yapılandırma ayarlarını özelleştirme

Birçok farklı [yapılandırma ayarlarını](service-fabric-cluster-manifest.md) ayarlanabilir *ClusterConfig.json* güvenilirlik düzeyi küme ve düğüm özelliklerinin gibi dosyası.  Daha fazla bilgi edinmek için [tek başına küme yapılandırmasını yükseltme](service-fabric-cluster-config-upgrade-windows-server.md).  Diğer birçok, daha gelişmiş ayarları da özelleştirilebilir.  Daha fazla bilgi için okuma [Service Fabric kümesi yapı ayarları](service-fabric-cluster-fabric-settings.md).

## <a name="define-node-properties"></a>Düğüm özellikleri tanımlama
Bazen, belirli iş yükleri yalnızca belirli türdeki düğümleri üzerinde çalıştığından emin olmak isteyebilirsiniz. Örneğin, başkalarının olmayabilir ancak bazı iş yükü GPU veya SSD gerektirebilir. Her bir kümedeki düğüm türleri için küme düğümleri için özel düğüm özellikleri ekleyebilirsiniz. Yerleştirme kısıtlamaları deyimleri için bir veya daha fazla düğüm Özellikler'i seçin, tek tek hizmetlerine bağlı olan. Yerleştirme kısıtlamaları Hizmetleri çalıştırdığı tanımlar.

Yerleştirme kısıtlamaları, düğüm özellikleri ve bunları tanımlama hakkında daha fazla bilgi için okuma [düğümü özellikleri ve yerleştirme kısıtlamaları](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).
 

## <a name="add-capacity-metrics"></a>Kapasite ölçüm Ekle
Her düğüm türü için yükü raporlamak üzere uygulamalarınızda kullanmak istediğiniz özel kapasite ölçümleri ekleyebilirsiniz. Yükü raporlamak için kapasite ölçümlerini kullanımı hakkında ayrıntılı bilgi için üzerinde Service Fabric Küme Kaynak Yöneticisi belgelerine bakın. [açıklayan bilgisayarınızı küme](service-fabric-cluster-resource-manager-cluster-description.md) ve [ölçümleri ve yük](service-fabric-cluster-resource-manager-metrics.md).

## <a name="patch-the-os-in-the-cluster-nodes"></a>İşletim sisteminde küme düğümlerine yama yapma
Düzeltme eki düzenleme uygulaması (POA) işletim sistemi düzeltme eki uygulama kapalı kalma süresi olmadan bir Service Fabric kümesinde otomatikleştiren bir Service Fabric uygulamasıdır. [Düzenleme uygulama Windows için düzeltme eki](service-fabric-patch-orchestration-application.md) düzeltme ekleri her zaman kullanılabilen hizmetler tutarken düzenlenmiş bir şekilde yüklemek için küme üzerinde dağıtılabilir. 


## <a name="next-steps"></a>Sonraki adımlar
* Bazı özelleştirmeyi öğrenin [service fabric kümesi yapı ayarları](service-fabric-cluster-fabric-settings.md)
* Bilgi edinmek için nasıl [kümenizin ölçeğini daraltma ve genişletme](service-fabric-cluster-scale-up-down.md)
* Hakkında bilgi edinin [uygulama yükseltmeleri](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
