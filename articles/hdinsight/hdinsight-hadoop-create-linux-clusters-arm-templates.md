---
title: "Şablonları - Azure Hdınsight kullanarak Hadoop kümeleri oluşturma | Microsoft Docs"
description: "Resource Manager şablonları kullanarak için Hdınsight kümeleri oluşturma hakkında bilgi edinin"
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00a80dea-011f-44f0-92a4-25d09db9d996
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: jgao
ms.openlocfilehash: 82733e2a3025f932961122bad9d70c26896837b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a>Resource Manager şablonları kullanarak Hdınsight'ta Hadoop kümeleri oluşturma
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Bu makalede, Azure Resource Manager şablonları ile Azure Hdınsight kümeleri oluşturmak için çeşitli yollar hakkında bilgi edineceksiniz. Daha fazla bilgi için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](../azure-resource-manager/resource-group-template-deploy.md). Diğer küme oluşturma araçları ve özellikleri hakkında bilgi edinmek için bu sayfanın üst kısmındaki sekme seçicisini tıklatın veya bkz [küme oluşturma yöntemleri](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).

## <a name="prerequisites"></a>Ön koşullar
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bu makaledeki yönergeleri izlemek için ihtiyacınız vardır:

* Bir [Azure aboneliği](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure PowerShell ve/veya Azure CLI.

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="resource-manager-templates"></a>Resource Manager şablonları
Resource Manager şablonu uygulamanız için aşağıdakileri tek ve eşgüdümlü bir işlemle oluşturmak kolay hale getirir:
* Hdınsight kümeleri ile bağımlı kaynakları (örneğin, varsayılan depolama hesabı)
* Diğer kaynaklar (örneğin, Apache Sqoop kullanmak için Azure SQL veritabanı)

Şablonda, uygulama için gereken kaynakları tanımlayın. Ayrıca, farklı ortamlar için değer girmesini dağıtım parametrelerini belirtin. JSON ve dağıtımınız için değerleri oluşturmak için kullandığınız deyimleri şablon oluşur.

Hdınsight şablon örnekleri konumunda bulabilirsiniz [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/resources/templates/?term=hdinsight). Platformlar arası kullanmak [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) ile [Resource Manager uzantısını](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) veya bir metin düzenleyicisi şablonu istasyonunuzda bir dosyaya kaydedin. Farklı yöntemler kullanarak şablonu çağırma öğrenin.

Resource Manager şablonları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Yazar Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md)
* [Bir uygulamayı Azure Resource Manager şablonları ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a>Şablonları oluştur

Azure Portalı'nı kullanarak, bir kümenin tüm özelliklerini yapılandırmak ve şablonunun dağıtmadan önce kaydedin. Şablon daha sonra yeniden kullanabilirsiniz.

**Azure portalını kullanarak bir şablon oluşturmak için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Tıklatın **yeni** sol menüsünde **Intelligence + analiz**ve ardından **Hdınsight**.
3. Özellikler girmek için yönergeleri izleyin. Kullanabilirsiniz **hızlı Oluştur** veya **özel** seçeneği.
4. Üzerinde **Özet** sekmesini tıklatın, **karşıdan şablonu ve parametre**:

    ![Hdınsight Hadoop kümesi Resource Manager şablonu indirme oluşturma](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download.png)

    Şablon dosyası, Parametreler dosyası ve kod örnekleri şablonu dağıtmak için kullanılan bir listeye bakın:

    ![Hdınsight Hadoop Resource Manager şablonu indirme seçeneklerini küme oluşturma](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download-options.png)

    Buradan, şablonunu indirebilir, şablon kitaplığınızı kaydetmek veya şablonu dağıtabilirsiniz.

    Bir şablon kitaplığınızdaki erişmek için tıklatın **daha fazla hizmet** sol menüsünden ve ardından **şablonları** (altında **diğer** kategori).

    > [!Note]
    > Şablonu ve parametre dosyanın birlikte kullanılması gerekir. Aksi takdirde, beklenmedik sonuçlar alabilirsiniz. Örneğin, varsayılan **clusterKind** özellik değeri olduğundan her zaman **hadoop**, şablon indirmeden önce hangi rağmen belirttiğiniz.



## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

Bu yordam, Hdınsight'ta Hadoop kümesi oluşturur.

1. JSON dosyasını kaydedin [ek](#appx-a-arm-template) istasyonunuzu için. PowerShell komut dosyası dosya adıdır `C:\HDITutorials-ARM\hdinsight-arm-template.json`.
2. Parametreler ve değişkenler gerekirse ayarlayın.
3. Aşağıdaki PowerShell komut dosyası kullanarak şablonu çalıştırın:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>"
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and variables
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage account
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

    PowerShell komut dosyası, yalnızca küme adını yapılandırır. Şablonda sabit kodlanmış depolama hesap adı değil. Küme kullanıcı parolasını girmeniz istenir. (Varsayılan kullanıcı adı **yönetici**.) SSH kullanıcı parolasını girmeniz istenir. (Varsayılan SSH kullanıcı adı **sshuser**.)  

Daha fazla bilgi için bkz: [PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).

## <a name="deploy-with-cli"></a>CLI ile dağıtma
Aşağıdaki örnek, Azure komut satırı arabirimi (CLI) kullanır. Bir küme, bağımlı depolama hesabı ve kapsayıcı Resource Manager şablonu çağırarak oluşturur:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"

Girmeniz istenir:
* Küme adı.
* Küme kullanıcı parolası. (Varsayılan kullanıcı adı **yönetici**.)
* SSH kullanıcı parolası. (Varsayılan SSH kullanıcı adı **sshuser**.)

Aşağıdaki kod, satır içi parametreleri sağlar:

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

## <a name="deploy-with-the-rest-api"></a>REST API ile dağıtma
Bkz: [REST API'si ile dağıtma](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="deploy-with-visual-studio"></a>Visual Studio ile dağıtma
 Bir kaynak grubu projesi oluşturun ve kullanıcı arabirimi aracılığıyla Azure'a dağıtmak için Visual Studio'yu kullanın. Projenize eklemek için kaynak türünü seçin. Bu kaynaklar, Resource Manager şablonu otomatik olarak eklenir. Proje şablonu dağıtmak için bir PowerShell betiğini de sağlar.

Visual Studio kaynak gruplarıyla kullanmaya giriş bilgileri için bkz: [Visual Studio üzerinden Azure kaynak gruplarını oluşturma ve dağıtma](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Hdınsight kümesi oluşturmanın birkaç yolu öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* .NET istemci kitaplığını kaynaklarına dağıtma ilişkin bir örnek için bkz: [kaynakları .NET kitaplıkları ve bir şablon kullanarak dağıtmak](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Bir uygulama dağıtımının ayrıntılı örneği için bkz: [sağlamak ve mikro beklendiği azure'da dağıtmak](../app-service/app-service-deploy-complex-application-predictably.md).
* Çözümünüzü farklı ortamlarda dağıtmaya yönelik kılavuz için bkz. [Microsoft Azure’da geliştirme ve test ortamları](../solution-dev-test-environments.md).
* Azure Resource Manager şablonu bölümleri hakkında bilgi edinmek için [şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).
* Bir Azure Resource Manager şablonunda kullanabileceğiniz işlevleri bir listesi için bkz: [şablon işlevleri](../azure-resource-manager/resource-group-template-functions.md).

## <a name="appendix-resource-manager-template-to-create-a-hadoop-cluster"></a>Ek: Hadoop kümesi oluşturmak için Resource Manager şablonu
Aşağıdaki Azure Resource Manager şablonu ile bağımlı Azure depolama hesabı Linux tabanlı Hadoop kümesi oluşturur.

> [!NOTE]
> Bu örnek Hive meta depo ve Oozie meta depo için yapılandırma bilgilerini içerir. Aşağıdaki bölümü silmek veya Şablon kullanmadan önce bölüm yapılandırın.
>
>

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
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
        "defaultValue": "sshuser",
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
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "The type of the HDInsight cluster to create."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
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
                }
            ]
            }
        }
        }
    ],
    "outputs": {
        "cluster": {
        "type": "object",
        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
        }
    }
    }

## <a name="appendix-resource-manager-template-to-create-a-spark-cluster"></a>Ek: Resource Manager şablonu bir Spark kümesi oluşturmak

Bu bölüm, bir Hdınsight Spark kümesi oluşturmak için kullanabileceğiniz bir Resource Manager şablonu sağlar. Bu şablon için yapılandırmaları içerir `spark-defaults` ve `spark-thrift-sparkconf` (Spark 1.6 kümelerinde için) ve `spark2-defaults` ve `spark2-thrift-sparkconf` (Spark 2 kümeleri için). Bu, ek olarak, Hdınsight hesaplar ve yapılandırmaları gibi ayarlar `spark.executor.instances`, `spark.executor.memory`, ve `spark.executor.cores` küme boyutuna göre. 

Herhangi bir parametre bir bölümde şablonunun parçası olarak ayarlarsanız, Hdınsight hesaplamak değil ve aynı bölüm diğer parametreleri ayarlayın. Örneğin, parametre `spark.executor.instances` yer `spark-defaults` yapılandırma. Başka bir parametre ayarlarsanız (örneğin, `spark.yarn.exector.memoryOverhead`) içinde `spark-defaults` yapılandırması, Hdınsight değil hesaplamak ve ayarlama `spark.executor.instances` parametresini de.

    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "The name of the HDInsight cluster to create."
            }
        },
        "clusterLoginUserName": {
            "type": "string",
            "defaultValue": "admin",
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
        "location": {
            "type": "string",
            "defaultValue": "southcentralus",
            "metadata": {
                "description": "The location where all azure resources will be deployed."
            }
        },
        "clusterVersion": {
            "type": "string",
            "defaultValue": "3.5",
            "metadata": {
                "description": "HDInsight cluster version."
            }
        },
        "clusterWorkerNodeCount": {
            "type": "int",
            "defaultValue": 4,
            "metadata": {
                "description": "The number of nodes in the HDInsight cluster."
            }
        },
        "clusterKind": {
            "type": "string",
            "defaultValue": "SPARK",
            "metadata": {
                "description": "The type of the HDInsight cluster to create."
            }
        },
        "sshUserName": {
            "type": "string",
            "defaultValue": "sshuser",
            "metadata": {
                "description": "These credentials can be used to remotely access the cluster."
            }
        },
        "sshPassword": {
            "type": "securestring",
            "metadata": {
                "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        }
    },
    "variables": {
        "defaultApiVersion": "2017-06-01",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
    {
            "apiVersion": "2015-03-01-preview",
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "dependsOn": [],
            "properties": {
                "clusterVersion": "[parameters('clusterVersion')]",
                "osType": "Linux",
                "tier": "standard",
                "clusterDefinition": {
                    "kind": "[parameters('clusterKind')]",
                    "configurations": {
                        "gateway": {
                            "restAuthCredential.isEnabled": true,
                            "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                            "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                        },
                        "spark-defaults": {
                            "spark.executor.cores": "2"
                        },
                        "spark-thrift-sparkconf": {
                            "spark.yarn.executor.memoryOverhead": "896"
                        }
                    }
                },
                "storageProfile": {
                    "storageaccounts": [
                        {
                            "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                            "isDefault": true,
                            "container": "[parameters('clusterName')]",
                            "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                        }
                    ]
                },
                "computeProfile": {
                    "roles": [
                        {
                            "name": "headnode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 2,
                            "hardwareProfile": {
                                "vmSize": "Standard_D12"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                }
                            },
                            "virtualNetworkProfile": null,
                            "scriptActions": []
                        },
                        {
                            "name": "workernode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 4,
                            "hardwareProfile": {
                                "vmSize": "Standard_D4"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                    }
                                },
                                "virtualNetworkProfile": null,
                                "scriptActions": []
                            }
                        ]
                    }
                }
            }
        ]
    }
