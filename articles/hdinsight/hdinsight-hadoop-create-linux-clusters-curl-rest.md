---
title: Azure REST API - Azure kullanarak Hadoop kümeleri oluşturma | Microsoft Docs
description: Azure Resource Manager şablonları Azure REST API göndererek Hdınsight kümeleri oluşturmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 98be5893-2c6f-4dfa-95ec-d4d8b5b7dcb5
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/02/2018
ms.author: larryfr
ms.openlocfilehash: 9c2779c692e944bed62d7ae9b76bb8929e62e5f3
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32775862"
---
# <a name="create-hadoop-clusters-using-the-azure-rest-api"></a>Azure REST API'sini kullanarak Hadoop kümeleri oluşturma

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Bir Azure Resource Manager şablonu ve Azure REST API'sini kullanarak bir Hdınsight kümesi oluşturmayı öğrenin.

Azure REST API'sini Hdınsight kümeleri gibi yeni kaynaklar oluşturma dahil olmak üzere Azure platformu barındırılan Hizmetleri yönetim işlemlerini gerçekleştirmenizi sağlar.

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!NOTE]
> Bu adımlarda belge kullanımı [curl (https://curl.haxx.se/) ](https://curl.haxx.se/) Azure REST API'si ile iletişim kurmak için yardımcı programı.

## <a name="create-a-template"></a>Şablon oluşturma

Azure Resource Manager şablonları tanımlayan JSON belgeleri olan bir **kaynak grubu** ve ortamdaki tüm kaynaklar (örneğin, Hdınsight.) Bu şablona dayalı yaklaşım, tek bir şablonda Hdınsight için gereken kaynaklar tanımlamanıza olanak sağlar.

Bir birleşme şablonu ve parametre dosyalarının aşağıdaki JSON belgedir [ https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), SSH kullanıcı hesabını güvenli hale getirmek için bir parola kullanarak Linux tabanlı bir küme oluşturur.

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

Bu örnekte, bu belgede yer alan adımlar, kullanılır. Örneği değiştirecek *değerleri* içinde **parametreleri** kümeniz için değerlerle bölümü.

> [!IMPORTANT]
> Şablonu çalışan düğümleri (4) varsayılan sayısı bir Hdınsight kümesi için kullanır. 32'den fazla çalışan düğümlerine planlıyorsanız, bir baş düğüm boyutu en az 8 çekirdek ve 14 GB ram ile seçmeniz gerekir.
>
> Düğümü boyutları ve ilişkili maliyetler hakkında daha fazla bilgi için bkz: [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="log-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

Konusunda belgelenen adımları izleyin [Azure CLI 2.0 ile çalışmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) ve kullanarak aboneliğinze bağlanın `az login` komutu.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

> [!NOTE]
> Bu adımları özetlenmiş bir sürümü olan *parolayla hizmet sorumlusu oluşturma* bölümünü [kaynaklara erişmek için bir hizmet sorumlusu oluşturmak için kullanım Azure CLI](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md) belge. Bu adımlar Azure REST API'sine kimliğini doğrulamak için kullanılan bir hizmet sorumlusu oluşturur.

1. Bir komut satırından, Azure aboneliklerinize listelemek için aşağıdaki komutu kullanın.

   ```bash
   az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table
   ```

    Listede, kullanıp not etmek istediğiniz aboneliği seçin **ABONELİK_KİMLİĞİ** ve __Tenant_ID__ sütun. Bu değerleri kaydedin.

2. Azure Active Directory'de bir uygulama oluşturmak için aşağıdaki komutu kullanın.

   ```bash
   az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'
   ```

    İçin değerleri `--display-name`, `--homepage`, ve `--identifier-uris` kendi değerlere sahip. Yeni Active Directory giriş için bir parola sağlayın.

   > [!NOTE]
   > `--home-page` Ve `--identifier-uris` değerleri internet'te barındırılan gerçek bir web sayfasına başvuru gerekmez. Benzersiz bir URI olmalıdır.

   Bu komuttan döndürülen değer __uygulama kimliği__ yeni uygulama için. Bu değer kaydedin.

3. Kullanarak bir hizmet sorumlusu oluşturmak için aşağıdaki komutu kullanın **uygulama kimliği**.

   ```bash
   az ad sp create --id <App ID> --query 'objectId'
   ```

     Bu komuttan döndürülen değer __nesne kimliği__. Bu değer kaydedin.

4. Ata **sahibi** hizmet asıl kullanarak rolü **nesne kimliği** değeri. Kullanım **abonelik kimliği** daha önce aldığınız.

   ```bash
   az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/
   ```

## <a name="get-an-authentication-token"></a>Kimlik Doğrulama belirtecini alma

Kimlik Doğrulama belirtecini almak için aşağıdaki komutu kullanın:

```bash
curl -X "POST" "https://login.microsoftonline.com/$TENANTID/oauth2/token" \
-H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=$APPID" \
--data-urlencode "grant_type=client_credentials" \
--data-urlencode "client_secret=$PASSWORD" \
--data-urlencode "resource=https://management.azure.com/"
```

Ayarlama `$TENANTID`, `$APPID`, ve `$PASSWORD` elde ya da daha önce kullanılan değerler.

Bu istek başarılı olursa, 200 serisi yanıtı alır ve bir JSON belgesi yanıt gövdesi içerir.

Bu istek tarafından döndürülen JSON belgesi adlı bir öğe içeriyorsa **access_token**. Değeri **access_token** REST API için kimlik doğrulama istekleri için kullanılır.

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

* Ayarlama `$SUBSCRIPTIONID` abonelik kimliği alınan hizmet asıl oluşturulurken.
* Ayarlama `$ACCESSTOKEN` önceki adımda aldığınız erişim belirtecini için.
* Değiştir `DATACENTERLOCATION` kaynak grubu ve kaynakları, oluşturmak istediğiniz veri merkezi ile. Örneğin, 'Orta Güney ABD'.
* Ayarlama `$RESOURCEGROUPNAME` bu grup için kullanmak istediğiniz adı:

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME?api-version=2015-01-01" \
    -H "Authorization: Bearer $ACCESSTOKEN" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DATACENTERLOCATION"
}'
```

Bu istek başarılı olursa, 200 serisi yanıtını alma ve grubu ile ilgili bilgileri içeren bir JSON belgesi yanıt gövdesi içerir. `"provisioningState"` Ögesinin değerini `"Succeeded"`.

## <a name="create-a-deployment"></a>Bir dağıtımı oluşturma

Şablonu kaynak grubuna dağıtmak için aşağıdaki komutu kullanın.

* Ayarlama `$DEPLOYMENTNAME` bu dağıtım için kullanmak istediğiniz adı.

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [!NOTE]
> Bir dosyaya şablon kaydettiyseniz yerine aşağıdaki komutu kullanabilirsiniz `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Bu istek başarılı olursa, 200 serisi yanıtını alma ve dağıtım işlemiyle ilgili bilgi içeren bir JSON belgesi yanıt gövdesi içerir.

> [!IMPORTANT]
> Dağıtım gönderildi ancak tamamlanmadı. Tamamlamak için dağıtım için genellikle yaklaşık 15, birkaç dakika sürebilir.

## <a name="check-the-status-of-a-deployment"></a>Bir dağıtım durumunu denetleyin

Dağıtım durumunu denetlemek için aşağıdaki komutu kullanın:

```bash
curl -X "GET" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json"
```

Bu komut, dağıtım işlemi hakkında bilgi içeren bir JSON belgesi döndürür. `"provisioningState"` Öğesi dağıtım durumunu içerir. Bu öğe bir değeri varsa `"Succeeded"`, dağıtım başarıyla tamamlandıktan sonra.

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Sonraki adımlar

Hdınsight kümesi başarıyla oluşturuldu, kümenizi ile çalışmayı öğrenmek için aşağıdakileri kullanın.

### <a name="hadoop-clusters"></a>Hadoop kümeleri

* [HDInsight ile Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hadoop/hdinsight-use-pig.md)
* [Hdınsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase kümeleri

* [HDInsight üzerinde HBase kullanmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Hdınsight'ta HBase için Java uygulamaları geliştirme](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm kümeleri

* [Hdınsight üzerinde Storm için Java topolojisi geliştirme](storm/apache-storm-develop-java-topology.md)
* [Hdınsight üzerinde Storm Python bileşenleri kullanma](storm/apache-storm-develop-python-topology.md)
* [Dağıtma ve hdınsight'ta Storm topolojileri izleme](storm/apache-storm-deploy-monitor-topology-linux.md)
