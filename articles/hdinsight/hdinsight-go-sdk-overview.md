---
title: Azure HDInsight Go SDK'sı
description: Azure HDInsight Go SDK için başvuru
author: tylerfox
ms.service: hdinsight
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: tyfox
ms.custom: seodec18
ms.openlocfilehash: 2e5b7816fda89e25dcb0de26f526e5187e0640b9
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64700613"
---
# <a name="hdinsight-go-management-sdk-preview"></a>HDInsight Git Yönetimi SDK önizlemesi

## <a name="overview"></a>Genel Bakış
HDInsight Go SDK sınıfları ve işlevleri, HDInsight kümelerinizi yönetmenizi sağlar. Bu, oluşturma, silme, güncelleştirme, listesinde, yeniden boyutlandırma, betik eylemleri yürütmek, izlemek, HDInsight kümelerine ve daha özelliklerini alma işlemlerini içerir.

> [!NOTE]  
>Bu SDK için GoDoc başvuru malzemesi olduğunu da [buradan kullanılabilir](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight).

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure hesabı. Biri yoksa [ücretsiz bir deneme sürümü edinin](https://azure.microsoft.com/free/).
* [Git](https://golang.org/dl/).

## <a name="sdk-installation"></a>SDK'sını yükleme

GOPATH konumunuzdan çalıştırın `go get github.com/Azure/azure-sdk-for-go/tree/master/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight`

## <a name="authentication"></a>Kimlik Doğrulaması

SDK'sı, ilk Azure aboneliğinizle kimliğinin doğrulanması gerekiyor.  Hizmet sorumlusu oluşturma ve kimlik doğrulaması için kullanmak için aşağıdaki örneği takip edin. Bunu yaptıktan sonra bir örneğine sahip bir `ClustersClient`, yönetim işlemlerini gerçekleştirmek için kullanılan birçok işlev (aşağıdaki bölümde anlatılan) içerir.

> [!NOTE]  
> Başka bir yöntemle yanı sıra kimliğini doğrulamak için aşağıdaki örnekteki, büyük olasılıkla daha gereksinimleriniz için uygun olması. Tüm İşlevler burada özetlenmektedir: [Go için Azure SDK'da kimlik doğrulama işlevleri](https://docs.microsoft.com/go/azure/azure-sdk-go-authorization)

### <a name="authentication-example-using-a-service-principal"></a>Bir hizmet sorumlusunu kullanarak kimlik doğrulaması örneği

İlk oturum açma [Azure Cloud Shell](https://shell.azure.com/bash). Oluşturulan hizmet sorumlusu istediğiniz abonelik şu anda kullanmakta olduğunuz doğrulayın. 

```azurecli-interactive
az account show
```

Abonelik bilgilerinizi JSON olarak görüntülenir.

```json
{
  "environmentName": "AzureCloud",
  "id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "isDefault": true,
  "name": "XXXXXXX",
  "state": "Enabled",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "user": {
    "cloudShellID": true,
    "name": "XXX@XXX.XXX",
    "type": "user"
  }
}
```

Doğru aboneliğe oturum değil, çalıştırarak doğru olanı seçin: 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]  
> Zaten HDInsight kaynak sağlayıcısı tarafından başka bir işleve kaydettiğiniz değil durumunda (gibi Azure Portalı aracılığıyla bir HDInsight kümesi oluşturarak), önce kimliklerini yapmanız gerekir. Bu, gelen yapılabilir [Azure Cloud Shell](https://shell.azure.com/bash) aşağıdaki komutu çalıştırarak:
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

Ardından, hizmet sorumlunuzu için bir ad seçin ve aşağıdaki komutla oluşturun:

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

Hizmet sorumlusu bilgilerini JSON olarak görüntülenir.

```json
{
  "clientId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "clientSecret": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "subscriptionId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```
Aşağıdaki kod parçacığı ve doldurma kopyalama `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET`, ve `SUBSCRIPTION_ID` ile hizmet sorumlusu oluşturmak için komutu çalıştırdıktan sonra döndürülen JSON dizeleri.

```golang
package main

import (
    "context"
    "github.com/Azure/go-autorest/autorest/azure/auth"
    hdi "github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight"
    "github.com/Azure/go-autorest/autorest/to"
)

func main() {
    var err error

    // Tenant ID for your Azure Subscription
    var TENANT_ID = ""
    // Your Service Principal App Client ID
    var CLIENT_ID = ""
    // Your Service Principal Client Secret
    var CLIENT_SECRET = ""
    // Azure Subscription ID
    var SUBSCRIPTION_ID = ""

    var credentials = auth.NewClientCredentialsConfig(CLIENT_ID, CLIENT_SECRET, TENANT_ID)
    var client = hdi.NewClustersClient(SUBSCRIPTION_ID)

    client.Authorizer, err = credentials.Authorizer()
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
```

## <a name="cluster-management"></a>Küme yönetimi

> [!NOTE]  
> Bu bölümde, zaten kimlik doğrulaması ve oluşturulan varsayılır bir `ClusterClient` adlı bir değişkende depolayın ve örnek `client`. Kimlik doğrulaması ve elde etmek için yönergeler bir `ClusterClient` yukarıdaki kimlik doğrulaması bölümünde bulunabilir.

### <a name="create-a-cluster"></a>Küme oluşturma

Yeni bir küme çağırarak oluşturulabilir `client.Create()`. 

#### <a name="example"></a>Örnek

Bu örnek nasıl oluşturulacağını gösterir. bir [Apache Spark](https://spark.apache.org/) kümeye 2 baş düğümü ve 1 çalışan düğümü ile.

> [!NOTE]  
> Önce bir kaynak grubu ve depolama hesabı oluşturmak için aşağıda açıklandığı gibi ihtiyacınız. Zaten bu oluşturduysanız, bu adımları atlayabilirsiniz.

##### <a name="creating-a-resource-group"></a>Bir kaynak grubu oluşturma

Kullanarak bir kaynak grubu oluşturmanız [Azure Cloud Shell](https://shell.azure.com/bash) çalıştırarak
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a>Depolama hesabı oluşturma

Bir depolama hesabı kullanarak oluşturabileceğiniz [Azure Cloud Shell](https://shell.azure.com/bash) çalıştırarak:
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
Şimdi (Bu bir küme oluşturmak için gerekir), depolama hesabı anahtarı almak için aşağıdaki komutu çalıştırın:
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
2 baş düğüm ve 1 çalışan düğümü ile Git kod parçacığı bir Spark kümesi oluşturur. Yorumlar bölümünde açıklandığı gibi boş değişkenleri doldurun ve belirli gereksinimlerinize uyacak şekilde diğer parametreleri değiştirmek çekinmeyin.

```golang
// The name for the cluster you are creating
var clusterName = "";
// The name of your existing Resource Group
var resourceGroupName = "";
// Choose a username
var username = "";
// Choose a password
var password = "";
// Replace <> with the name of your storage account
var storageAccount = "<>.blob.core.windows.net";
// Storage account key you obtained above
var storageAccountKey = "";
// Choose a region
var location = "";
var container = "default";

var parameters = hdi.ClusterCreateParametersExtended {
    Location: to.StringPtr(location),
    Tags: make(map[string]*string),
    Properties: &hdi.ClusterCreateProperties {
        ClusterVersion: to.StringPtr("3.6"),
        OsType: hdi.Linux,
        ClusterDefinition: &hdi.ClusterDefinition {
            Kind: to.StringPtr("spark"),
            Configurations: map[string]map[string]interface{}{
                "gateway": {
                    "restAuthCredential.isEnabled": "True",
                    "restAuthCredential.username":  username,
                    "restAuthCredential.password":  password,
                },
            },
        },
        Tier: hdi.Standard,
        ComputeProfile: &hdi.ComputeProfile {
            Roles: &[]hdi.Role {
                hdi.Role {
                    Name: to.StringPtr("headnode"),
                    TargetInstanceCount: to.Int32Ptr(2),
                    HardwareProfile: &hdi.HardwareProfile {
                        VMSize: to.StringPtr("Large"),
                    },
                    OsProfile: &hdi.OsProfile {
                        LinuxOperatingSystemProfile: &hdi.LinuxOperatingSystemProfile {
                            Username: to.StringPtr(username),
                            Password: to.StringPtr(password),
                        },
                    },
                },
                hdi.Role {
                    Name: to.StringPtr("workernode"),
                    TargetInstanceCount: to.Int32Ptr(1),
                    HardwareProfile: &hdi.HardwareProfile {
                        VMSize: to.StringPtr("Large"),
                    },
                    OsProfile: &hdi.OsProfile {
                        LinuxOperatingSystemProfile: &hdi.LinuxOperatingSystemProfile {
                            Username: to.StringPtr(username),
                            Password: to.StringPtr(password),
                        },
                    },
                },
            },
        },
        StorageProfile: &hdi.StorageProfile {
            Storageaccounts: &[]hdi.StorageAccount {
                hdi.StorageAccount {
                    Name: to.StringPtr(storageAccount),
                    Key: to.StringPtr(storageAccountKey),
                    Container: to.StringPtr(container),
                    IsDefault: to.BoolPtr(true),
                },
            },
        },
    },
}
client.Create(context.Background(), resourceGroupName, clusterName, parameters)
```

### <a name="get-cluster-details"></a>Küme ayrıntıları

Belirli bir küme için özellikleri almak için:

```golang
client.Get(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Örnek

Kullanabileceğiniz `get` kümeniz başarıyla oluşturdunuz onaylamak için.

```golang
cluster, err := client.Get(context.Background(), resourceGroupName, clusterName)
if (err != nil) {
    fmt.Println("Error: ", err)
}
fmt.Println(*cluster.Name)
fmt.Println(*cluster.ID
```

Çıktı gibi görünmelidir:

```
<Cluster Name>
/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>
```

### <a name="list-clusters"></a>Kümeleri listeleme

#### <a name="list-clusters-under-the-subscription"></a>Abonelik altındaki kümeleri listeleme
```golang
client.List()
```
#### <a name="list-clusters-by-resource-group"></a>Kaynak grubu tarafından kümeleri listeleme
```golang
client.ListByResourceGroup("<Resource Group Name>")
```

> [!NOTE]  
> Her ikisi de `List()` ve `ListByResourceGroup()` dönüş bir `ClusterListResultPage` yapısı. Sonraki sayfayı almak için çağırabilirsiniz `Next()`. Bu kadar yinelenebilir `ClusterListResultPage.NotDone()` döndürür `false`, aşağıdaki örnekte gösterildiği gibi.

#### <a name="example"></a>Örnek
Aşağıdaki örnek, geçerli abonelik için tüm küme özelliklerini yazdırır:

```golang
page, err := client.List(context.Background())
if (err != nil) {
    fmt.Println("Error: ", err)
}
for (page.NotDone()) {
    for _, cluster := range page.Values() {
        fmt.Println(*cluster.Name)
    }
    err = page.Next();
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
}
```

### <a name="delete-a-cluster"></a>Küme silme

Bir kümeyi silmek için:

```golang
client.Delete(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

### <a name="update-cluster-tags"></a>Güncelleştirme kümesi etiketleri

Belirli bir küme etiketlerini güncelleştirebilirsiniz şu şekilde:

```golang
client.Update(context.Background(), "<Resource Group Name>", "<Cluster Name>", hdi.ClusterPatchParameters{<map[string]*string} of Tags>)
```
#### <a name="example"></a>Örnek

```golang
client.Update(context.Background(), "SDKTestRG", "SDKTest", hdi.ClusterPatchParameters{map[string]*string{"tag1Name" : to.StringPtr("tag1Value"), "tag2Name" : to.StringPtr("tag2Value")}})
```

### <a name="resize-cluster"></a>Küme yeniden boyutlandırma

Yeni boyutunu belirterek belirli bir kümenin çalışan düğümlerinin sayısını yeniden boyutlandırabilirsiniz şu şekilde:

```golang
client.Resize(context.Background(), "<Resource Group Name>", "<Cluster Name>", hdi.ClusterResizeParameters{<Num of Worker Nodes (int)>})
```

## <a name="cluster-monitoring"></a>Küme izleme

HDInsight Yönetimi SDK'sı, izleme kümelerinizdeki Operations Management Suite (OMS) aracılığıyla yönetmek için de kullanılabilir.

Nasıl oluşturduğunuz benzer şekilde `ClusterClient` yönetim işlemleri için kullanılacak oluşturmak gereken bir `ExtensionClient` işlemleri izleme için kullanılacak. Yukarıdaki kimlik doğrulaması bölümü tamamladıktan sonra oluşturabileceğiniz bir `ExtensionClient` şu şekilde:

```golang
extClient := hdi.NewExtensionsClient(SUBSCRIPTION_ID)
extClient.Authorizer, _ = credentials.Authorizer()
```

> [!NOTE]  
> Aşağıda örnekler izleme zaten başlatılmış varsayar bir `ExtensionClient` adlı `extClient` ve kendi `Authorizer` yukarıda da gösterildiği gibi.

### <a name="enable-oms-monitoring"></a>OMS izlemeyi etkinleştir

> [!NOTE]  
> OMS izlemeyi etkinleştirmek için mevcut bir Log Analytics çalışma alanı olması gerekir. Zaten bir oluşturmadıysanız, burada bunu nasıl edinebilirsiniz: [Azure portalında Log Analytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).

Kümenizde OMS izlemeyi etkinleştirmek için:

```golang
extClient.EnableMonitoring(context.Background(), "<Resource Group Name", "Cluster Name", hdi.ClusterMonitoringRequest {WorkspaceID: to.StringPtr("<Workspace Id>")})
```

### <a name="view-status-of-oms-monitoring"></a>OMS izleme durumunu görüntüle

Kümenizde OMS durumunu almak için:

```golang
extClient.GetMonitoringStatus(context.Background(), "<Resource Group Name", "Cluster Name")
```

### <a name="disable-oms-monitoring"></a>OMS izlemeyi devre dışı bırak

Kümenizde OMS devre dışı bırakmak için:

```golang
extClient.DisableMonitoring(context.Background(), "<Resource Group Name", "Cluster Name")
```

## <a name="script-actions"></a>Betik eylemleri

HDInsight küme özelleştirmek için özel betikler çağıran betik eylemleri adlı bir yapılandırma işlevi sağlar.

> [!NOTE]  
> Betik eylemleri kullanma hakkında daha fazla bilgi burada bulunabilir: [Betik eylemlerini kullanarak Linux tabanlı HDInsight kümeleri özelleştirme](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)

### <a name="execute-script-actions"></a>Betik eylemleri yürütme

Belirtilen bir kümede betik eylemleri yürütebilir şu şekilde:

```golang
var scriptAction1 = hdi.RuntimeScriptAction{Name: to.StringPtr("<Script Name>"), URI: to.StringPtr("<URL To Script>"), Roles: <&[]string of roles>} //valid roles are "headnode", "workernode", "zookeepernode", and "edgenode"
client.ExecuteScriptActions(context.Background(), "<Resource Group Name>", "<Cluster Name>", hdi.ExecuteScriptActionParameters{PersistOnSuccess: to.BoolPtr(true), ScriptActions: &[]hdi.RuntimeScriptAction{scriptAction1}}) //add more RuntimeScriptActions to the list to execute multiple scripts
```

'Betik eylemi Sil' ve 'Kalıcı betik eylemleri liste' işlemlerinde, oluşturmanız gerekir. bir `ScriptActionsClient`, nasıl oluşturduğunuz benzer şekilde `ClusterClient` yönetim işlemleri için kullanılacak. Yukarıdaki kimlik doğrulaması bölümü tamamladıktan sonra oluşturabileceğiniz bir `ScriptActionsClient` şu şekilde:

```golang
scriptActionsClient := hdi.NewScriptActionsClient(SUBSCRIPTION_ID)
scriptActionsClient.Authorizer, _ = credentials.Authorizer()
```

> [!NOTE]  
> Betik eylemleri örneklerde, zaten başlatılmış varsayılmaktadır bir `ScriptActionsClient` adlı `scriptActionsClient` ve kendi `Authorizer` yukarıda da gösterildiği gibi.

### <a name="delete-script-action"></a>Betik eylemi Sil

Belirtilen kümede belirtilen kalıcı betik eylemi silmek için:

```golang
scriptActionsClient.Delete(context.Background(), "<Resource Group Name>", "<Cluster Name>", "<Script Name>")
```

### <a name="list-persisted-script-actions"></a>Kalıcı betik eylemleri listeleyin

> [!NOTE]  
> Her ikisi de `ListByCluster()` döndürür bir `ScriptActionsListPage` yapısı. Sonraki sayfayı almak için çağırabilirsiniz `Next()`. Bu kadar yinelenebilir `ClusterListResultPage.NotDone()` döndürür `false`, aşağıdaki örnekte gösterildiği gibi.

Belirtilen kümenin tüm kalıcı betik eylemleri listelemek için:
```golang
scriptActionsClient.ListByCluster(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Örnek

```golang
page, err := scriptActionsClient.ListByCluster(context.Background(), resourceGroupName, clusterName)
if (err != nil) {
    fmt.Println("Error: ", err)
}
for (page.NotDone()) {
    for _, script := range page.Values() {
        fmt.Println(*script.Name) //There are functions to get other properties of RuntimeScriptActionDetail besides Name, such as Status, Operation, StartTime, EndTime, etc. See reference documentation.
    }
    err = page.Next();
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
}
```

### <a name="list-all-scripts-execution-history"></a>Tüm betikleri yürütme geçmişini listesi

Bu işlem için oluşturmanız gerekir. bir `ScriptExecutionHistoryClient`, nasıl oluşturduğunuz benzer şekilde `ClusterClient` yönetim işlemleri için kullanılacak. Yukarıdaki kimlik doğrulaması bölümü tamamladıktan sonra oluşturabileceğiniz bir `ScriptActionsClient` şu şekilde:

```golang
scriptExecutionHistoryClient := hdi.NewScriptExecutionHistoryClient(SUBSCRIPTION_ID)
scriptExecutionHistoryClient.Authorizer, _ = credentials.Authorizer()
```

> [!NOTE]  
> Zaten başlatılmış aşağıda varsayar bir `ScriptExecutionHistoryClient` adlı `scriptExecutionHistoryClient` ve kendi `Authorizer` yukarıda da gösterildiği gibi.

Belirtilen kümenin tüm betikleri yürütme geçmişini listelemek için:

```golang
scriptExecutionHistoryClient.ListByCluster(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Örnek

Bu örnek, tüm geçmiş betik yürütme için tüm ayrıntılarını yazdırır.

```golang
page, err := scriptExecutionHistoryClient.ListByCluster(context.Background(), resourceGroupName, clusterName)
if (err != nil) {
    fmt.Println("Error: ", err)
}
for (page.NotDone()) {
    for _, script := range page.Values() {
        fmt.Println(*script.Name) //There are functions to get other properties of RuntimeScriptActionDetail besides Name, such as Status, Operation, StartTime, EndTime, etc. See reference documentation.
    }
    err = page.Next();
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Keşfedin [GoDoc başvuru malzemesi](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight). SDK'sı uygulamasında tüm işlevleri için başvuru belgeleri GoDocs sağlar.
