---
title: Ayrıntılı rol tabanlı erişim için küme yapılandırmaları - Azure HDInsight geçirme
description: Ayrıntılı rol tabanlı erişim için küme yapılandırmaları geçirmek için gereken değişiklikleri hakkında bilgi edinin.
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/19/2019
ms.openlocfilehash: 0422d848ccdf9ba82e68813de64eec863ee4ad29
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60006242"
---
# <a name="migrate-to-granular-role-based-access-for-cluster-configurations"></a>Ayrıntılı rol tabanlı erişim için küme yapılandırmaları geçirme

Hassas bilgileri almak için daha fazla ayrıntılı rol tabanlı erişimi destekleyen bazı önemli değişiklikler sunuyoruz. Bu bölüm, bazı değiştikçe **eylem gerekli** birini kullanmanız durumunda [etkilenen varlıkların/senaryoları](#am-i-affected-by-these-changes).

## <a name="what-is-changing"></a>Değişen nedir?

Daha önce gizli dizileri HDInsight API aracılığıyla sahip, katkıda bulunan veya okuyucu işlediği küme kullanıcılar tarafından elde edilemedi [RBAC rollerini](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles).
Bundan sonra bu Sırları artık okuyucu rolüne sahip kullanıcılar için erişilebilir. Biz de, sahibi veya katkıda bulunan Yönetimsel izinlere sahip olmadan gizli dizileri almak için yeni 'Hdınsight küme işleci' rolü Tanıtımı. Özetlersek:

| Rol                                  | Daha önce                                                                                       | Şimdi       |
|---------------------------------------|--------------------------------------------------------------------------------------------------|-----------|
| Okuyucu                                | -Okuma erişimi, gizli diziler de dahil olmak üzere                                                                   | -Okuma erişimi **hariç** gizli dizileri |           |   |   |
| HDInsight küme işleci<br>(Yeni Rol) | Yok                                                                                              | -Gizli diziler de dahil olmak üzere, okuma/yazma erişimi         |   |   |
| Katılımcı                           | -Gizli diziler de dahil olmak üzere, okuma/yazma erişimi<br>-Oluşturun ve tüm Azure kaynağı türleriyle yönetin.     | Değişiklik yok |
| Sahip                                 | -Gizli diziler de dahil olmak üzere okuma/yazma erişimi<br>-Tüm kaynaklar tam erişim<br>-Başkalarına temsilci erişimi | Değişiklik yok |

Bunları vermek için kullanıcıya HDInsight küme işleci rol ataması ekleme hakkında daha fazla bilgi için okuma/yazma erişimi küme gizli dizileri için bkz: bölüm, aşağıdaki [HDInsight küme işleci rol ataması için kullanıcı ekleme](#add-the-hdinsight-cluster-operator-role-assignment-to-a-user).

## <a name="am-i-affected-by-these-changes"></a>Ben bu değişikliklerden etkilenir miyim?

Aşağıdaki varlıklar ve senaryoları etkilenir:

- [API](#api): Kullanan kullanıcılar `/configurations` veya `/configurations/{configurationName}` uç noktaları.
- [Visual Studio Code için Azure HDInsight Araçları](#azure-hdinsight-tools-for-visual-studio-code) sürüm ___ ve altı.
- [Intellij için Azure Araç Seti](#azure-toolkit-for-intellij) sürüm __ ve altı.
- [Eclipse için Azure Araç Seti](#azure-toolkit-for-eclipse) sürüm __ ve altı.
- [.NET için SDK'sı](#sdk-for-net)
    - [sürümleri 1.x veya 2.x](#versions-1x-and-2x): Kullanan kullanıcılar `GetClusterConfigurations`, `GetConnectivitySettings`, `ConfigureHttpSettings`, `EnableHttp` veya `DisableHttp` ConfigurationsOperationsExtensions sınıftaki yöntemleri.
    - [sürümleri 3.x ve en fazla](#versions-3x-and-up): Kullanan kullanıcılar `EnableHttp`, `DisableHttp`, `Update`, veya `Get` yöntemlerinden `ConfigurationsOperationsExtensions` sınıfı.
- [Python SDK'sı](#sdk-for-python): Kullanan kullanıcılar `get` veya `update` ConfigurationsOperations sınıftaki yöntemleri.
- [Java için SDK](#sdk-for-java): Kullanan kullanıcılar `update` veya `get` ConfigurationsInner sınıftaki yöntemleri.
- [Go için SDK](#sdk-for-go): Kullanan kullanıcılar `Get` veya `Update` ConfigurationsClient struct yöntemleri.

Aşağıdaki bölümlerde (veya bağlantıları kullanın) geçiş görmek için senaryonuz için adımları bakın.

### <a name="api"></a>API

Aşağıdaki API'leri değiştirildi veya kullanım dışı:

- [**GET /configurations/ {configurationName}** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-configuration) (hassas bilgileri kaldırıldı) https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/configurations/{configurationName}?api-version={api-version}
    - Daha önce bireysel yapılandırma türleri (gizli anahtarları dahil) almak için kullanılır.
    - Bu API çağrısı artık tek tek yapılandırma türlerine sahip gizli diziler atlanmış döndürür. Gizli öğeleri dahil olmak üzere tüm yapılandırmaları kullanın yeni [POST /configurations]() çağırın. Ağ Geçidi ayarlarını almak için yeni kullanın [POST /getGatewaySettings]() çağırın.
- [**GET /configurations** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-configurations) (kullanım dışı) https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/configurations?api-version={api-version}
    - Daha önce (gizli anahtarları dahil) tüm yapılandırmaları almak için kullanılır.
    - Bu API çağrısı artık desteklenir. İleride tüm yapılandırmaları almak için yeni kullanın [POST /configurations]() çağırın. Atlanmış gizli parametrelerle yapılandırmaları almak için kullanın [GET /configurations/ {configurationName}]() çağırın.
- [**POST /configurations/ {configurationName}** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#change-connectivity-settings) (kullanım dışı) https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/configurations/{configurationName}?api-version={api-version}
    - Daha önce Ağ Geçidi kimlik bilgilerini güncelleştirmek için kullanılır.
    - Bu API çağrısı kullanım dışı ve artık desteklenmiyor. Yeni [POST /updateGatewaySettings]() yerine.

Aşağıdaki değiştirilen API'ler eklenmiştir:</span>

- **POST /configurations** https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/configurations?api-version={api-version}
    - Gizli diziler de dahil olmak üzere tüm yapılandırmaları almak için bu API'yi kullanın.
- **POST /getGatewaySettings** https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/getGatewaySettings?api-version={api-version}
    - Ağ Geçidi ayarlarını almak için bu API'yi kullanın.
- **POST /updateGatewaySettings** https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/updateGatewaySettings?api-version={api-version}
    - (Kullanıcı adı ve/veya parolası) ağ geçidi ayarlarını güncelleştirmek için bu API'yi kullanın.

### <a name="azure-hdinsight-tools-for-visual-studio-code"></a>Visual Studio Code için Azure HDInsight araçları

Sürüm 1.1.1 kullanıyorsanız veya daha eski Lütfen güncelleştirme [Visual Studio Code için Azure HDInsight Araçları'nın en son sürümü](https://marketplace.visualstudio.com/items?itemName=mshdinsight.azure-hdinsight&ssr=false) kesintilerini önlemek için.

### <a name="azure-toolkit-for-intellij"></a>IntelliJ için Azure Araç Takımı

Sürüm 3.21.0 kullanıyorsanız veya daha eski Lütfen güncelleştirme [eklentisi Intellij için Azure Araç Takımı'nın en son sürümünü](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij) kesintilerini önlemek için.

### <a name="azure-toolkit-for-eclipse"></a>Eclipse için Azure Araç Seti

2019-03-29 sürüm kullanıyorsanız veya daha eski Lütfen kesintilerinden kaçınmak Eclipse için Azure Araç Seti en son sürümüne güncelleştirin.

### <a name="sdk-for-net"></a>.NET için SDK

#### <a name="versions-1x-and-2x"></a>Sürümleri 1.x ve 2.x'i

Lütfen güncelleştirme [sürüm 2.1.0](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/2.1.0) .NET için HDInsight SDK'sının. Bu değişikliklerden etkilenen bir yöntem kullanılıyorsa çok az kod değişiklikleri gerekebilir:

- `ClusterOperationsExtensions.GetClusterConfigurations` olacak **artık dönüş hassas parametreleri** depolama anahtarları (çekirdek-site) veya HTTP kimlik bilgilerini (ağ geçidi) gibi.
    - Hassas parametreleri de dahil olmak üzere tüm yapılandırmaları kullanın `ClusterOperationsExtensions.ListConfigurations` ileride.  Not 'Reader' rolüne sahip kullanıcılar bu yöntemi kullanmak mümkün olmayacaktır. Bu küme için hassas bilgileri hangi kullanıcıların erişeceği ayrıntılı denetim sağlar.
    - Yalnızca HTTP ağ geçidi kimlik bilgilerini almak için kullanın `ClusterOperationsExtensions.GetGatewaySettings`.

- `ClusterOperationsExtensions.GetConnectivitySettings` kullanım dışı bırakıldı ve almıştır `ClusterOperationsExtensions.GetGatewaySettings`.

- `ClusterOperationsExtensions.ConfigureHttpSettings` kullanım dışı bırakıldı ve almıştır `ClusterOperationsExtensions.UpdateGatewaySettings`.

- `ConfigurationsOperationsExtensions.EnableHttp` ve `DisableHttp` artık kullanım dışı bırakılmıştır. Bu yöntemler artık gerekmeyen için HTTP artık her zaman etkindir.

#### <a name="versions-3x-and-up"></a>Sürümleri 3.x ve üstü

Lütfen güncelleştirme [sürüm 5.0.0](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/5.0.0) .NET için HDInsight SDK'sının. Bu değişikliklerden etkilenen bir yöntem kullanılıyorsa çok az kod değişiklikleri gerekebilir:

- [`ConfigurationOperationsExtensions.Get`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.get?view=azure-dotnet) olacak **artık dönüş hassas parametreleri** depolama anahtarları (çekirdek-site) veya HTTP kimlik bilgilerini (ağ geçidi) gibi.
    - Hassas parametreleri de dahil olmak üzere tüm yapılandırmaları kullanın `ConfigurationOperationsExtensions.List` ileride.  Not 'Reader' rolüne sahip kullanıcılar bu yöntemi kullanmak mümkün olmayacaktır. Bu küme için hassas bilgileri hangi kullanıcıların erişeceği ayrıntılı denetim sağlar. 
    - Yalnızca HTTP ağ geçidi kimlik bilgilerini almak için kullanın `ClusterOperationsExtensions.GetGatewaySettings`. 
- [`ConfigurationsOperationsExtensions.Update`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.update?view=azure-dotnet) kullanım dışı bırakıldı ve almıştır `ClusterOperationsExtensions.UpdateGatewaySettings`. 
- [`ConfigurationsOperationsExtensions.EnableHttp`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.enablehttp?view=azure-dotnet) ve [ `DisableHttp` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.disablehttp?view=azure-dotnet) artık kullanım dışı bırakılmıştır. Bu yöntemler artık gerekmeyen için HTTP artık her zaman etkindir.

### <a name="sdk-for-python"></a>Python için SDK

Lütfen güncelleştirme [sürüm 1.0.0](https://pypi.org/project/azure-mgmt-hdinsight/1.0.0/) Python için HDInsight SDK'sının. Bu değişikliklerden etkilenen bir yöntem kullanılıyorsa çok az kod değişiklikleri gerekebilir:

- [`ConfigurationsOperations.get`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurations_operations.configurationsoperations?view=azure-python#get-resource-group-name--cluster-name--configuration-name--custom-headers-none--raw-false----operation-config-) olacak **artık dönüş hassas parametreleri** depolama anahtarları (çekirdek-site) veya HTTP kimlik bilgilerini (ağ geçidi) gibi.
    - Hassas parametreleri de dahil olmak üzere tüm yapılandırmaları kullanın [ `ConfigurationsOperations.list` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurations_operations.configurationsoperations?view=azure-python#list-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-) ileride.  Not 'Reader' rolüne sahip kullanıcılar bu yöntemi kullanmak mümkün olmayacaktır. Bu küme için hassas bilgileri hangi kullanıcıların erişeceği ayrıntılı denetim sağlar. 
    - Yalnızca HTTP ağ geçidi kimlik bilgilerini almak için kullanın [ `ConfigurationsOperations.get_gateway_settings` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clusters_operations.clustersoperations?view=azure-python#get-gateway-settings-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-).
- [`ConfigurationsOperations.update`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clusters_operations.clustersoperations?view=azure-python#update-resource-group-name--cluster-name--tags-none--custom-headers-none--raw-false----operation-config-) kullanım dışı bırakıldı ve almıştır [ `ClusterOperationsExtensions.update_gateway_settings` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clusters_operations.clustersoperations?view=azure-python#update-gateway-settings-resource-group-name--cluster-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-).

### <a name="sdk-for-java"></a>Java için SDK'sı

Lütfen güncelleştirme [sürüm 1.0.0](https://search.maven.org/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight/) Java için HDInsight SDK'sının. Bu değişikliklerden etkilenen bir yöntem kullanılıyorsa çok az kod değişiklikleri gerekebilir:

- [`ConfigurationsInner.get`](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018__06__01__preview.implementation._configurations_inner.get) olacak **artık dönüş hassas parametreleri** depolama anahtarları (çekirdek-site) veya HTTP kimlik bilgilerini (ağ geçidi) gibi.
    - Hassas parametreleri de dahil olmak üzere tüm yapılandırmaları kullanın `ConfigurationsInner.list` ileride.  Not 'Reader' rolüne sahip kullanıcılar bu yöntemi kullanmak mümkün olmayacaktır. Bu küme için hassas bilgileri hangi kullanıcıların erişeceği ayrıntılı denetim sağlar. 
    - Yalnızca HTTP ağ geçidi kimlik bilgilerini almak için kullanın `ConfigurationsOperations.get_gateway_settings`.
- [`ConfigurationsInner.update`](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018__06__01__preview.implementation._configurations_inner.update) kullanım dışı bırakıldı ve almıştır `ClusterOperationsExtensions.update_gateway_settings`.

### <a name="sdk-for-go"></a>Go için SDK'sı

Lütfen güncelleştirme [sürüm 27.1.0](https://github.com/Azure/azure-sdk-for-go/tree/master/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight) Git için HDInsight SDK'sının. Bu değişikliklerden etkilenen bir yöntem kullanılıyorsa çok az kod değişiklikleri gerekebilir:

- [`ConfigurationsClient.get`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.Get) olacak **artık dönüş hassas parametreleri** depolama anahtarları (çekirdek-site) veya HTTP kimlik bilgilerini (ağ geçidi) gibi.
    - Hassas parametreleri de dahil olmak üzere tüm yapılandırmaları kullanın [ `ConfigurationsClient.list` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.List) ileride.  Not 'Reader' rolüne sahip kullanıcılar bu yöntemi kullanmak mümkün olmayacaktır. Bu küme için hassas bilgileri hangi kullanıcıların erişeceği ayrıntılı denetim sağlar. 
    - Yalnızca HTTP ağ geçidi kimlik bilgilerini almak için kullanın [ `ClustersClient.get_gateway_settings` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ClustersClient.GetGatewaySettings).
- [`ConfigurationsClient.update`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.Update) kullanım dışı bırakıldı ve almıştır [ `ClustersClient.update_gateway_settings` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ClustersClient.UpdateGatewaySettings).

## <a name="add-the-hdinsight-cluster-operator-role-assignment-to-a-user"></a>Bir kullanıcıya HDInsight küme işleci rolü ataması ekleme

Bir kullanıcıyla [katkıda bulunan](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) veya [sahibi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) rol vermek HDInsight küme operatör rolündeki kullanıcılara, Ağ Geçidi kimlik bilgileri kümesi gibi HDInsight küme parolaları için okuma/yazma erişimine sahip olmasını istediğiniz ve depolama hesabı anahtarları.

### <a name="using-the-azure-cli"></a>Azure CLI kullanma

Bu rol ataması Ekle en basit yolu, Azure CLI aşağıdaki komutu kullanarak verilmiştir:

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com
```

> [!NOTE]
> Bu komut yalnızca Yöneticiler bu izinleri verebilir olarak katkıda bulunan veya sahip rolleriyle bir kullanıcı tarafından çalıştırılmalıdır. `--assignee` HDInsight küme işleci rolü atamak istediğiniz kullanıcının e-posta adresidir.

Yukarıdaki komut, abonelik düzeyinde bu rolü sağlar. Bunun yerine kaynak grubu düzeyinde bu rol yalnızca vermek için komutu değiştirebilirsiniz şu şekilde:

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com -g <ResourceGroupName>
```

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Alternatif olarak, HDInsight küme işleci rol ataması için bir kullanıcı eklemek için Azure portalını kullanabilirsiniz. Belgelere bakın [RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme - rol ataması Ekle](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment).
