---
title: Ayrıntılı rol tabanlı erişim için küme yapılandırmaları - Azure HDInsight geçirme
description: HDInsight küme yapılandırmaları için ayrıntılı rol tabanlı erişim için geçişin bir parçası olarak gerekli değişiklikler hakkında bilgi edinin.
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 357be801914017aceb7e827a3b49960cf7c3e386
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565413"
---
# <a name="migrate-to-granular-role-based-access-for-cluster-configurations"></a>Küme yapılandırmaları için ayrıntılı rol tabanlı erişime geçme

Hassas bilgileri almak için daha fazla ayrıntılı rol tabanlı erişimi destekleyen bazı önemli değişiklikler sunuyoruz. Bu bölüm, bazı değiştikçe **eylem gerekli** birini kullanmanız durumunda [etkilenen varlıkların/senaryoları](#am-i-affected-by-these-changes).

## <a name="what-is-changing"></a>Değişen nedir?

Daha önce gizli dizileri HDInsight API aracılığıyla sahip, katkıda bulunan veya okuyucu işlediği küme kullanıcılar tarafından elde edilemedi [RBAC rollerini](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)olan herkes için kullanılabilir oldukları gibi `*/read` izni.
Bundan sonra bu gizli dizileri erişim gerektiren `Microsoft.HDInsight/clusters/configurations/*` izni, bunlar artık erişilebilir okuyucu rolü olan kullanıcılar tarafından anlamına gelir. Bir kullanıcının rolünü daha fazla yükseltilmiş erişim elde etmek için kullanılabilecek değerleri sağlamalıdır gizli olarak tanımlanır. Bunlar, küme ağ geçidi HTTP kimlik bilgilerini, depolama hesabı anahtarlarını ve veritabanı kimlik bilgileri gibi değerleri içerir.

Ayrıca yeni bir sunuyoruz [HDInsight küme işleci](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#hdinsight-cluster-operator) rol sahibi veya katkıda bulunan Yönetimsel izinlere sahip olmadan gizli dizileri almak mümkün olacaktır. Özetlersek:

| Role                                  | Daha önce                                                                                       | Bundan sonra       |
|---------------------------------------|--------------------------------------------------------------------------------------------------|-----------|
| Okuyucu                                | -Okuma erişimi, gizli diziler de dahil olmak üzere                                                                   | -Okuma erişimi **hariç** gizli dizileri |           |   |   |
| HDInsight küme işleci<br>(Yeni Rol) | Yok                                                                                              | -Gizli diziler de dahil olmak üzere, okuma/yazma erişimi         |   |   |
| Katılımcı                           | -Gizli diziler de dahil olmak üzere, okuma/yazma erişimi<br>-Oluşturun ve tüm Azure kaynağı türleriyle yönetin.     | Değişiklik yok |
| Sahip                                 | -Gizli diziler de dahil olmak üzere okuma/yazma erişimi<br>-Tüm kaynaklar tam erişim<br>-Başkalarına temsilci erişimi | Değişiklik yok |

Bunları vermek için kullanıcıya HDInsight küme işleci rol ataması ekleme hakkında daha fazla bilgi için okuma/yazma erişimi küme gizli dizileri için bkz: bölüm, aşağıdaki [HDInsight küme işleci rol ataması için kullanıcı ekleme](#add-the-hdinsight-cluster-operator-role-assignment-to-a-user).

## <a name="am-i-affected-by-these-changes"></a>Ben bu değişikliklerden etkilenir miyim?

Aşağıdaki varlıklar ve senaryoları etkilenir:

- [API](#api): Kullanan kullanıcılar `/configurations` veya `/configurations/{configurationName}` uç noktaları.
- [Visual Studio Code için Azure HDInsight Araçları](#azure-hdinsight-tools-for-visual-studio-code) sürüm 1.1.1 veya daha düşük.
- [Intellij için Azure Araç Seti](#azure-toolkit-for-intellij) 3.20.0 sürüm veya daha düşük.
- [Azure Data Lake ve Stream Analytics araçları Visual Studio için](#azure-data-lake-and-stream-analytics-tools-for-visual-studio) 2.3.9000.1 önceki bir sürümü.
- [Eclipse için Azure Araç Seti](#azure-toolkit-for-eclipse) 3.15.0 sürüm veya daha düşük.
- [.NET için SDK'sı](#sdk-for-net)
    - [sürümleri 1.x veya 2.x](#versions-1x-and-2x): Kullanan kullanıcılar `GetClusterConfigurations`, `GetConnectivitySettings`, `ConfigureHttpSettings`, `EnableHttp` veya `DisableHttp` ConfigurationsOperationsExtensions sınıftaki yöntemleri.
    - [sürümleri 3.x ve en fazla](#versions-3x-and-up): Kullanan kullanıcılar `Get`, `Update`, `EnableHttp`, veya `DisableHttp` yöntemlerinden `ConfigurationsOperationsExtensions` sınıfı.
- [Python SDK'sı](#sdk-for-python): Kullanan kullanıcılar `get` veya `update` yöntemlerinden `ConfigurationsOperations` sınıfı.
- [Java için SDK](#sdk-for-java): Kullanan kullanıcılar `update` veya `get` yöntemlerinden `ConfigurationsInner` sınıfı.
- [Go için SDK](#sdk-for-go): Kullanan kullanıcılar `Get` veya `Update` yöntemlerinden `ConfigurationsClient` yapısı.
- [Az.HDInsight PowerShell](#azhdinsight-powershell) 2.0.0 sürümü.
Senaryonuzun geçiş adımlarını görmek için aşağıdaki bölümlere bakın (ya da yukarıdaki bağlantıları kullanın).

### <a name="api"></a>API

Aşağıdaki API'leri değiştirildi veya kullanım dışı:

- [**GET /configurations/ {configurationName}** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-configuration) (hassas bilgileri kaldırıldı)
    - Daha önce bireysel yapılandırma türleri (gizli anahtarları dahil) almak için kullanılır.
    - Bu API çağrısı artık tek tek yapılandırma türlerine sahip gizli diziler atlanmış döndürür. Gizli öğeleri dahil olmak üzere tüm yapılandırmaları almak için yeni bir GÖNDERİ /configurations çağrı kullanın. Ağ Geçidi ayarlarını almak için yeni bir GÖNDERİ /getGatewaySettings çağrı kullanın.
- [**GET /configurations** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-configuration) (kullanım dışı)
    - Daha önce (gizli anahtarları dahil) tüm yapılandırmaları almak için kullanılır.
    - Bu API çağrısı artık desteklenir. İleride tüm yapılandırmaları almak için yeni bir GÖNDERİ /configurations çağrı kullanın. Atlanmış gizli parametrelerle yapılandırmaları almak için GET /configurations/ {configurationName} çağrısı kullanın.
- [**POST /configurations/ {configurationName}** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#update-gateway-settings) (kullanım dışı)
    - Daha önce Ağ Geçidi kimlik bilgilerini güncelleştirmek için kullanılır.
    - Bu API çağrısı kullanım dışı ve artık desteklenmiyor. Bunun yerine yeni bir GÖNDERİ /updateGatewaySettings kullanın.

Aşağıdaki değiştirilen API'ler eklenmiştir:</span>

- [**POST /configurations**](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#list-configurations)
    - Gizli diziler de dahil olmak üzere tüm yapılandırmaları almak için bu API'yi kullanın.
- [**POST /getGatewaySettings**](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-gateway-settings)
    - Ağ Geçidi ayarlarını almak için bu API'yi kullanın.
- [**POST /updateGatewaySettings**](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#update-gateway-settings)
    - (Kullanıcı adı ve/veya parolası) ağ geçidi ayarlarını güncelleştirmek için bu API'yi kullanın.

### <a name="azure-hdinsight-tools-for-visual-studio-code"></a>Visual Studio Code için Azure HDInsight araçları

Aşağıda, güncelleştirme veya sürüm 1.1.1 kullanıyorsanız [Visual Studio Code için Azure HDInsight Araçları'nın en son sürümü](https://marketplace.visualstudio.com/items?itemName=mshdinsight.azure-hdinsight&ssr=false) kesintilerini önlemek için.

### <a name="azure-toolkit-for-intellij"></a>IntelliJ için Azure Araç Takımı

Aşağıda, güncelleştirme veya sürüm 3.20.0 kullanıyorsanız [eklentisi Intellij için Azure Araç Takımı'nın en son sürümünü](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij) kesintilerini önlemek için.

### <a name="azure-data-lake-and-stream-analytics-tools-for-visual-studio"></a>Azure Data Lake ve Visual Studio için Stream Analytics araçları

Güncelleştirme sürümüne 2.3.9000.1 veya sonraki sürümleri [Azure Data Lake ve Stream Analytics araçları Visual Studio için](https://marketplace.visualstudio.com/items?itemName=ADLTools.AzureDataLakeandStreamAnalyticsTools&ssr=false#overview) kesintilerini önlemek için.  Güncelleştirme ile ilgili Yardım için bkz, belgelerimize [güncelleştirme Data Lake araçları Visual Studio için](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-visual-studio-tools-get-started#update-data-lake-tools-for-visual-studio).

### <a name="azure-toolkit-for-eclipse"></a>Eclipse için Azure Araç Seti

Aşağıda, güncelleştirme veya sürüm 3.15.0 kullanıyorsanız [Eclipse için Azure Araç Takımı'nın en son sürümünü](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) kesintilerini önlemek için.

### <a name="sdk-for-net"></a>.NET için SDK

#### <a name="versions-1x-and-2x"></a>Sürümleri 1.x ve 2.x'i

Güncelleştirme [sürüm 2.1.0](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/2.1.0) .NET için HDInsight SDK'sının. Bu değişikliklerden etkilenen bir yöntem kullanılıyorsa çok az kod değişiklikleri gerekebilir:

- `ClusterOperationsExtensions.GetClusterConfigurations` olacak **artık dönüş hassas parametreleri** depolama anahtarları (çekirdek-site) veya HTTP kimlik bilgilerini (ağ geçidi) gibi.
    - Hassas parametreleri de dahil olmak üzere tüm yapılandırmaları kullanın `ClusterOperationsExtensions.ListConfigurations` ileride.  Not 'Reader' rolüne sahip kullanıcılar bu yöntemi kullanmak mümkün olmayacaktır. Bu küme için hassas bilgileri hangi kullanıcıların erişeceği ayrıntılı denetim sağlar.
    - Yalnızca HTTP ağ geçidi kimlik bilgilerini almak için kullanın `ClusterOperationsExtensions.GetGatewaySettings`.

- `ClusterOperationsExtensions.GetConnectivitySettings` kullanım dışı bırakıldı ve almıştır `ClusterOperationsExtensions.GetGatewaySettings`.

- `ClusterOperationsExtensions.ConfigureHttpSettings` kullanım dışı bırakıldı ve almıştır `ClusterOperationsExtensions.UpdateGatewaySettings`.

- `ConfigurationsOperationsExtensions.EnableHttp` ve `DisableHttp` artık kullanım dışı bırakılmıştır. Bu yöntemler artık gerekmeyen için HTTP artık her zaman etkindir.

#### <a name="versions-3x-and-up"></a>Sürümleri 3.x ve üstü

Güncelleştirme [sürüm 5.0.0](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/5.0.0) veya .NET için HDInsight SDK'sının daha yeni. Bu değişikliklerden etkilenen bir yöntem kullanılıyorsa çok az kod değişiklikleri gerekebilir:

- [`ConfigurationOperationsExtensions.Get`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.get?view=azure-dotnet) olacak **artık dönüş hassas parametreleri** depolama anahtarları (çekirdek-site) veya HTTP kimlik bilgilerini (ağ geçidi) gibi.
    - Hassas parametreleri de dahil olmak üzere tüm yapılandırmaları kullanın [ `ConfigurationOperationsExtensions.List` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.list?view=azure-dotnet) ileride.  Not 'Reader' rolüne sahip kullanıcılar bu yöntemi kullanmak mümkün olmayacaktır. Bu küme için hassas bilgileri hangi kullanıcıların erişeceği ayrıntılı denetim sağlar. 
    - Yalnızca HTTP ağ geçidi kimlik bilgilerini almak için kullanın [ `ClusterOperationsExtensions.GetGatewaySettings` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.clustersoperationsextensions.getgatewaysettings?view=azure-dotnet). 
- [`ConfigurationsOperationsExtensions.Update`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.update?view=azure-dotnet) kullanım dışı bırakıldı ve almıştır [ `ClusterOperationsExtensions.UpdateGatewaySettings` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.clustersoperationsextensions.updategatewaysettings?view=azure-dotnet). 
- [`ConfigurationsOperationsExtensions.EnableHttp`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.enablehttp?view=azure-dotnet) ve [ `DisableHttp` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.disablehttp?view=azure-dotnet) artık kullanım dışı bırakılmıştır. Bu yöntemler artık gerekmeyen için HTTP artık her zaman etkindir.

### <a name="sdk-for-python"></a>Python için SDK

Güncelleştirme [sürüm 1.0.0](https://pypi.org/project/azure-mgmt-hdinsight/1.0.0/) veya Python için HDInsight SDK'sının daha yeni. Bu değişikliklerden etkilenen bir yöntem kullanılıyorsa çok az kod değişiklikleri gerekebilir:

- [`ConfigurationsOperations.get`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurationsoperations#get-resource-group-name--cluster-name--configuration-name--custom-headers-none--raw-false----operation-config-) olacak **artık dönüş hassas parametreleri** depolama anahtarları (çekirdek-site) veya HTTP kimlik bilgilerini (ağ geçidi) gibi.
    - Hassas parametreleri de dahil olmak üzere tüm yapılandırmaları kullanın [ `ConfigurationsOperations.list` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurationsoperations#list-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-) ileride.  Not 'Reader' rolüne sahip kullanıcılar bu yöntemi kullanmak mümkün olmayacaktır. Bu küme için hassas bilgileri hangi kullanıcıların erişeceği ayrıntılı denetim sağlar. 
    - Yalnızca HTTP ağ geçidi kimlik bilgilerini almak için kullanın [ `ClusterOperations.get_gateway_settings` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clustersoperations#get-gateway-settings-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-).
- [`ConfigurationsOperations.update`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurationsoperations#update-resource-group-name--cluster-name--configuration-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-) kullanım dışı bırakıldı ve almıştır [ `ClusterOperations.update_gateway_settings` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clustersoperations#update-gateway-settings-resource-group-name--cluster-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-).

### <a name="sdk-for-java"></a>Java için SDK'sı

Güncelleştirme [sürüm 1.0.0](https://search.maven.org/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight/) veya Java için HDInsight SDK'sının daha yeni. Bu değişikliklerden etkilenen bir yöntem kullanılıyorsa çok az kod değişiklikleri gerekebilir:

- [`ConfigurationsInner.get`](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018__06__01__preview.implementation._configurations_inner.get) olacak **artık dönüş hassas parametreleri** depolama anahtarları (çekirdek-site) veya HTTP kimlik bilgilerini (ağ geçidi) gibi.
    - Hassas parametreleri de dahil olmak üzere tüm yapılandırmaları kullanın [ `ConfigurationsInner.list` ](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018_06_01_preview.implementation.configurationsinner.list?view=azure-java-stable) ileride.  Not 'Reader' rolüne sahip kullanıcılar bu yöntemi kullanmak mümkün olmayacaktır. Bu küme için hassas bilgileri hangi kullanıcıların erişeceği ayrıntılı denetim sağlar. 
    - Yalnızca HTTP ağ geçidi kimlik bilgilerini almak için kullanın [ `ClustersInner.getGatewaySettings` ](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018_06_01_preview.implementation.clustersinner.getgatewaysettings?view=azure-java-stable).
- [`ConfigurationsInner.update`](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018__06__01__preview.implementation._configurations_inner.update) kullanım dışı bırakıldı ve almıştır [ `ClustersInner.updateGatewaySettings` ](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018_06_01_preview.implementation.clustersinner.updategatewaysettings?view=azure-java-stable).

### <a name="sdk-for-go"></a>Go için SDK'sı

Güncelleştirme [sürüm 27.1.0](https://github.com/Azure/azure-sdk-for-go/tree/master/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight) ya da Git için HDInsight SDK'sının daha yeni. Bu değişikliklerden etkilenen bir yöntem kullanılıyorsa çok az kod değişiklikleri gerekebilir:

- [`ConfigurationsClient.get`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.Get) olacak **artık dönüş hassas parametreleri** depolama anahtarları (çekirdek-site) veya HTTP kimlik bilgilerini (ağ geçidi) gibi.
    - Hassas parametreleri de dahil olmak üzere tüm yapılandırmaları kullanın [ `ConfigurationsClient.list` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.List) ileride.  Not 'Reader' rolüne sahip kullanıcılar bu yöntemi kullanmak mümkün olmayacaktır. Bu küme için hassas bilgileri hangi kullanıcıların erişeceği ayrıntılı denetim sağlar. 
    - Yalnızca HTTP ağ geçidi kimlik bilgilerini almak için kullanın [ `ClustersClient.get_gateway_settings` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ClustersClient.GetGatewaySettings).
- [`ConfigurationsClient.update`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.Update) kullanım dışı bırakıldı ve almıştır [ `ClustersClient.update_gateway_settings` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ClustersClient.UpdateGatewaySettings).

### <a name="azhdinsight-powershell"></a>Az.HDInsight PowerShell
Güncelleştirme [Az PowerShell sürüm 2.0.0](https://www.powershellgallery.com/packages/Az) veya daha sonra kesintileri önlemek için.  Bu değişikliklerden etkilenen bir yöntem kullanılıyorsa çok az kod değişiklikleri gerekebilir.
- `Grant-AzHDInsightHttpServicesAccess` artık kullanımdan kaldırıldı ve yeni tarafından değiştirilmiştir `Set-AzHDInsightGatewayCredential` cmdlet'i.
- `Get-AzHDInsightJobOutput` Depolama anahtarı için ayrıntılı rol tabanlı erişimi desteklemek için güncelleştirildi.
    - HDInsight Küme Operatörü, Katkıda Bulunan veya Sahip rolleri olan kullanıcılar etkilenmeyecek.
    - Yalnızca okuyucu rolüne sahip kullanıcılar belirtmeniz gerekecektir `DefaultStorageAccountKey` parametresi açıkça.
- `Revoke-AzHDInsightHttpServicesAccess` artık kullanılmıyor. Bu cmdlet artık gerekli olmadığı için HTTP artık her zaman etkindir.
 Bkz: [az. HDInsight Geçiş Kılavuzu](https://github.com/Azure/azure-powershell/blob/master/documentation/migration-guides/Az.2.0.0-migration-guide.md#azhdinsight) daha fazla ayrıntı için.

## <a name="add-the-hdinsight-cluster-operator-role-assignment-to-a-user"></a>Bir kullanıcıya HDInsight küme işleci rolü ataması ekleme

Bir kullanıcıyla [katkıda bulunan](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) veya [sahibi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) rol atayabilir [HDInsight küme işleci](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#hdinsight-cluster-operator) duyarlı okuma/yazma erişimi isteyen kullanıcılara rol HDInsight küme yapılandırma değerleri (örneğin, Küme Ağ Geçidi kimlik bilgilerini ve depolama hesabı anahtarları).

### <a name="using-the-azure-cli"></a>Azure CLI kullanma

Bu rol ataması Ekle en basit yolu kullanmaktır `az role assignemnt create` Azure CLI komutu.

> [!NOTE]
> Bu komut yalnızca Yöneticiler bu izinleri verebilir olarak katkıda bulunan veya sahip rolleriyle bir kullanıcı tarafından çalıştırılmalıdır. `--assignee` HDInsight küme işleci rolü atamak istediğiniz kullanıcının e-posta adresidir.

#### <a name="grant-role-at-the-resource-cluster-level"></a>Kaynak (küme) düzeyinde rolü izni

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee <user@domain.com> --scope /subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.HDInsight/clusters/<ClusterName>
```

#### <a name="grant-role-at-the-resource-group-level"></a>Kaynak grubu düzeyinde rolü izni

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com -g <ResourceGroupName>
```

#### <a name="grant-role-at-the-subscription-level"></a>Abonelik düzeyinde rolü izni

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com
```

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Alternatif olarak, HDInsight küme işleci rol ataması için bir kullanıcı eklemek için Azure portalını kullanabilirsiniz. Belgelere bakın [RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme - rol ataması Ekle](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment).
