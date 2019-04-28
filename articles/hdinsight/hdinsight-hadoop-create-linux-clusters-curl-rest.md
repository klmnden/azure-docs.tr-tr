---
title: Azure REST API - Azure kullanarak Apache Hadoop kümeleri oluşturma
description: Azure REST API'si için Azure Resource Manager şablonları göndererek HDInsight kümeleri oluşturmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/02/2018
ms.author: hrasheed
ms.openlocfilehash: acf121c2954b3f324682578dd3ab2b4d8b1f63f2
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62124924"
---
# <a name="create-apache-hadoop-clusters-using-the-azure-rest-api"></a>Azure REST API'sini kullanarak Apache Hadoop kümeleri oluşturma

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Bir Azure Resource Manager şablonu ve Azure REST API'sini kullanarak bir HDInsight kümesi oluşturmayı öğrenin.

Azure REST API'si, HDInsight kümeleri gibi yeni kaynaklar oluşturma da dahil olmak üzere, Azure platformunda barındırılan hizmetler üzerinde yönetim işlemlerini gerçekleştirmenize olanak sağlar.

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!NOTE]  
> Bu adımları belge kullanım [curl (https://curl.haxx.se/) ](https://curl.haxx.se/) Azure REST API'si ile iletişim kurmasına yardımcı programı.

## <a name="create-a-template"></a>Şablon oluşturma

Azure Resource Manager şablonları açıklayan bir JSON belgeleri olan bir **kaynak grubu** ve içindeki tüm kaynakları (örneğin, HDInsight.) Şablona dayalı bu yaklaşım, tek bir şablonda HDInsight için ihtiyacınız olan kaynaklardan tanımlamanızı sağlar.

Şablon ve parametreleri dosyası bir birleşme aşağıdaki JSON belgesidir [ https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), SSH kullanıcı hesabının güvenliğini sağlamak için bir parola kullanarak Linux tabanlı bir küme oluşturur.

   ```json
   {
       "properties": {
           "template": {
               "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
               "contentVersion": "1.0.0.0",
               "parameters": {
                   "clusterType": {
                       "type": "string",
                       "allowedValues": ["hadoop",
                       "hbase",
                       "storm",
                       "spark"],
                       "metadata": {
                           "description": "The type of the HDInsight cluster to create."
                       }
                   },
                   "clusterName": {
                       "type": "string",
                       "metadata": {
                           "description": "The name of the HDInsight cluster to create."
                       }
                   },
                   "clusterLoginUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                       }
                   },
                   "clusterLoginPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "sshUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used to remotely access the cluster."
                       }
                   },
                   "sshPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "clusterStorageAccountName": {
                       "type": "string",
                       "metadata": {
                           "description": "The name of the storage account to be created and be used as the cluster's storage."
                       }
                   },
                   "clusterWorkerNodeCount": {
                       "type": "int",
                       "defaultValue": 4,
                       "metadata": {
                           "description": "The number of nodes in the HDInsight cluster."
                       }
                   }
               },
               "variables": {
                   "defaultApiVersion": "2015-05-01-preview",
                   "clusterApiVersion": "2015-03-01-preview"
               },
               "resources": [{
                   "name": "[parameters('clusterStorageAccountName')]",
                   "type": "Microsoft.Storage/storageAccounts",
                   "location": "[resourceGroup().location]",
                   "apiVersion": "[variables('defaultApiVersion')]",
                   "dependsOn": [],
                   "tags": {

                   },
                   "properties": {
                       "accountType": "Standard_LRS"
                   }
               },
               {
                   "name": "[parameters('clusterName')]",
                   "type": "Microsoft.HDInsight/clusters",
                   "location": "[resourceGroup().location]",
                   "apiVersion": "[variables('clusterApiVersion')]",
                   "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                   "tags": {

                   },
                   "properties": {
                       "clusterVersion": "3.6",
                       "osType": "Linux",
                       "clusterDefinition": {
                           "kind": "[parameters('clusterType')]",
                           "configurations": {
                               "gateway": {
                                   "restAuthCredential.isEnabled": true,
                                   "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                   "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                               }
                           }
                       },
                       "storageProfile": {
                           "storageaccounts": [{
                               "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                               "isDefault": true,
                               "container": "[parameters('clusterName')]",
                               "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                           }]
                       },
                       "computeProfile": {
                           "roles": [{
                               "name": "headnode",
                               "targetInstanceCount": "2",
                               "hardwareProfile": {
                                   "vmSize": "Standard_D3"
                               },
                               "osProfile": {
                                   "linuxOperatingSystemProfile": {
                                       "username": "[parameters('sshUserName')]",
                                       "password": "[parameters('sshPassword')]"
                                   }
                               }
                           },
                           {
                               "name": "workernode",
                               "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                               "hardwareProfile": {
                                   "vmSize": "Standard_D3"
                               },
                               "osProfile": {
                                   "linuxOperatingSystemProfile": {
                                       "username": "[parameters('sshUserName')]",
                                       "password": "[parameters('sshPassword')]"
                                   }
                               }
                           }]
                       }
                   }
               }],
               "outputs": {
                   "cluster": {
                       "type": "object",
                       "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                   }
               }
           },
           "mode": "incremental",
           "Parameters": {
               "clusterName": {
                   "value": "newclustername"
               },
               "clusterType": {
                   "value": "hadoop"
               },
               "clusterStorageAccountName": {
                   "value": "newstoragename"
               },
               "clusterLoginUserName": {
                   "value": "admin"
               },
               "clusterLoginPassword": {
                   "value": "changeme"
               },
               "sshUserName": {
                   "value": "sshuser"
               },
               "sshPassword": {
                   "value": "changeme"
               }
           }
       }
   }
   ```

Bu örnekte, bu belgedeki adımlarda kullanılır. Örneği değiştirecek *değerleri* içinde **parametreleri** kümenizin değerlerle bölümü.

> [!IMPORTANT]  
> Şablon, bir HDInsight kümesi için varsayılan (4) çalışan düğümü sayısını kullanır. 32'den fazla çalışan düğümlerinde planlıyorsanız, bir baş düğüm boyutu en az 8 çekirdek ve 14 GB ram ile seçmeniz gerekir.
>
> Düğüm boyutları ve ilişkili maliyetler hakkında daha fazla bilgi için bkz. [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="log-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

Konusunda belgelenen adımları [Azure CLI ile çalışmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) ve kullanarak aboneliğinze bağlanın `az login` komutu.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

> [!NOTE]  
> Bu adımlarla bir MMC'ye sürümünü *parola ile hizmet sorumlusu oluşturma* bölümünü [kaynaklara erişmek için bir hizmet sorumlusu oluşturmak için Azure CLI'yı kullanmak](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md) belge. Bu adımlar Azure REST API'sine kimliğini doğrulamak için kullanılan bir hizmet sorumlusu oluşturur.

1. Bir komut satırından Azure aboneliklerinizi listelemek için aşağıdaki komutu kullanın.

   ```bash
   az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table
   ```

    Listeden unutmayın ve istediğiniz aboneliği seçin **Subscription_ID** ve __Kiracı__ sütunları. Bu değerleri kaydedin.

2. Azure Active Directory'de uygulama oluşturmak için aşağıdaki komutu kullanın.

   ```bash
   az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'
   ```

    Değerleri Değiştir `--display-name`, `--homepage`, ve `--identifier-uris` kendi değerlerinizle. Yeni Active Directory giriş için bir parola sağlayın.

   > [!NOTE]  
   > `--home-page` Ve `--identifier-uris` değerleri, internet'te barındırılan gerçek bir web sayfasına başvurmak gerekmez. Benzersiz bir URI'leri olmaları gerekir.

   Bu komuttan döndürülen değer __uygulama kimliği__ yeni uygulama için. Bu değer kaydedin.

3. Kullanarak bir hizmet sorumlusu oluşturmak için aşağıdaki komutu kullanın **uygulama kimliği**.

   ```bash
   az ad sp create --id <App ID> --query 'objectId'
   ```

     Bu komuttan döndürülen değer __nesne kimliği__. Bu değer kaydedin.

4. Ata **sahibi** kullanarak hizmet sorumlusu için rol **nesne kimliği** değeri. Kullanım **abonelik kimliği** daha önce edindiğiniz.

   ```bash
   az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/
   ```

## <a name="get-an-authentication-token"></a>Bir kimlik doğrulama belirtecini alma

Bir kimlik doğrulama belirteci almak için aşağıdaki komutu kullanın:

```bash
curl -X "POST" "https://login.microsoftonline.com/$TENANTID/oauth2/token" \
-H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=$APPID" \
--data-urlencode "grant_type=client_credentials" \
--data-urlencode "client_secret=$PASSWORD" \
--data-urlencode "resource=https://management.azure.com/"
```

Ayarlama `$TENANTID`, `$APPID`, ve `$PASSWORD` elde edilen ya da daha önce kullanılan değerlerine.

Bu istek başarılı olursa, 200 serisi yanıt ve yanıt gövdesi JSON belgesini içerir.

Bu istek tarafından döndürülen JSON belgesini adlı bir öğe içeren **access_token**. Değerini **access_token** REST API için kimlik doğrulama istekleri için kullanılır.

```json
{
    "token_type":"Bearer",
    "expires_in":"3599",
    "expires_on":"1463409994",
    "not_before":"1463406094",
    "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
}
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu oluşturmak için aşağıdakileri kullanın.

* Ayarlama `$SUBSCRIPTIONID` abonelik kimliği alınan hizmet sorumlusu oluşturulurken.
* Ayarlama `$ACCESSTOKEN` için önceki adımda alınan erişim belirteci.
* Değiştirin `DATACENTERLOCATION` istediğiniz kaynak grubu ve kaynakları oluşturmak için veri merkezi ile. Örneğin, 'Orta Güney ABD'.
* Ayarlama `$RESOURCEGROUPNAME` bu grup için kullanmak istediğiniz adı:

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME?api-version=2015-01-01" \
    -H "Authorization: Bearer $ACCESSTOKEN" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DATACENTERLOCATION"
}'
```

Bu istek başarılı olursa, 200 serisi yanıt ve yanıt gövdesi grubu ile ilgili bilgileri içeren bir JSON belgesini içerir. `"provisioningState"` Ögesinin değerini `"Succeeded"`.

## <a name="create-a-deployment"></a>Bir dağıtım oluşturun

Kaynak grubu için şablonu dağıtmak için aşağıdaki komutu kullanın.

* Ayarlama `$DEPLOYMENTNAME` bu dağıtım için kullanmak istediğiniz adı.

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [!NOTE]  
> Şablonu bir dosyaya kaydettiyseniz, aşağıdaki komutu yerine kullanabileceğiniz `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Bu istek başarılı olursa, 200 serisi yanıt ve dağıtım işlemi hakkında bilgi içeren bir JSON belgesi yanıt gövdesi içerir.

> [!IMPORTANT]  
> Dağıtım gönderildi ancak tamamlanmadı. Bu, genellikle yaklaşık 15, dağıtımın tamamlanması birkaç dakika sürebilir.

## <a name="check-the-status-of-a-deployment"></a>Bir dağıtım durumunu denetleyin

Dağıtım durumunu denetlemek için aşağıdaki komutu kullanın:

```bash
curl -X "GET" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json"
```

Bu komut, dağıtım işlemi hakkında bilgi içeren bir JSON belgesini döndürür. `"provisioningState"` Öğesi dağıtım durumunu içerir. Bu öğe değerini içeriyorsa `"Succeeded"`, dağıtım başarıyla tamamlandıktan sonra.

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

Bir HDInsight kümesi başarıyla oluşturuldu, kümenizi ile çalışma hakkında bilgi almak için aşağıdakileri kullanın.

### <a name="apache-hadoop-clusters"></a>Apache Hadoop kümelerini

* [Apache Hive, HDInsight ile kullanma](hadoop/hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase kümeleri

* [HDInsight üzerinde Apache HBase kullanmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md)
* [HDInsight üzerinde Apache HBase için Java uygulamaları geliştirin](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Apache Storm kümeleri

* [HDInsight üzerinde Apache Storm için Java topolojileri geliştirme](storm/apache-storm-develop-java-topology.md)
* [HDInsight üzerinde Apache Storm, Python bileşenlerini kullanma](storm/apache-storm-develop-python-topology.md)
* [HDInsight üzerinde Apache Storm topolojileri dağıtma ve izleme](storm/apache-storm-deploy-monitor-topology-linux.md)
