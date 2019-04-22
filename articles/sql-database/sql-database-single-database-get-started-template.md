---
title: 'Azure Resource Manager: Tek veritabanı - Azure SQL veritabanı oluşturma | Microsoft Docs'
description: Azure Resource Manager şablonu kullanarak Azure SQL veritabanı içinde tek bir veritabanı oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: mumian
ms.author: jgao
ms.reviewer: carlrab
manager: craigg
ms.date: 04/09/2019
ms.openlocfilehash: 8d060ce60194e47814308bfd67bd14db996650b0
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59425789"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-resource-manager-template"></a>Hızlı Başlangıç: Azure Resource Manager şablonu kullanarak Azure SQL veritabanı tek veritabanı oluşturma

Oluşturma bir [tek veritabanı](sql-database-single-database.md) Azure SQL veritabanında bir veritabanı oluşturmak için hızlı ve kolay bir dağıtım seçeneğidir. Bu hızlı başlangıçta, Azure Resource Manager şablonu kullanarak tek veritabanı oluşturma işlemini göstermektedir. Daha fazla bilgi için [Azure Resource Manager belgelerini](/azure/azure-resource-manager/).

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-a-single-database"></a>Tek veritabanı oluşturma

Bir dizi işlem, bellek, GÇ ve depolama kaynakları iki birini kullanarak tek bir veritabanı olan [satın alma modeli](sql-database-purchase-models.md). Tek bir veritabanı oluşturduğunuzda, aynı zamanda tanımlamış bir [SQL veritabanı sunucusu](sql-database-servers.md) yönetip içine yerleştirdiğiniz [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) belirli bir bölgede.

Aşağıdaki JSON dosyası bu makalede kullanılan şablonudur. Şablon, bir Azure depolama hesabında depolanır. Daha fazla Azure SQL veritabanı şablonu örnekleri bulunabilir [burada](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular).

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "serverName": {
      "type": "string",
      "defaultValue": "[concat('server-', uniqueString(resourceGroup().id, deployment().name))]",
      "metadata": {
        "description": "Name for the SQL server"
      }
    },
    "shouldDeployDb": {
      "type": "string",
      "allowedValues": [
        "Yes",
        "No"
      ],
      "defaultValue": "Yes",
      "metadata": {
        "description": "Whether an Azure SQL Database should be deployed under the server"
      }
    },
    "databaseName": {
      "type": "string",
      "defaultValue": "[concat('db-', uniqueString(resourceGroup().id, deployment().name), '-1')]",
      "metadata": {
        "description": "Name for the SQL database under the SQL server"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for server and optional DB"
      }
    },
    "emailAddresses": {
      "type": "array",
      "defaultValue": [
        "user1@example.com",
        "user2@example.com"
      ],
      "metadata": {
        "description": "Email addresses for receiving alerts"
      }
    },
    "adminUser": {
      "type": "string",
      "metadata": {
        "description": "Username for admin"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for admin"
      }
    }
  },
  "variables": {
    "databaseServerName": "[toLower(parameters('serverName'))]",
    "databaseName": "[parameters('databaseName')]",
    "shouldDeployDb": "[parameters('shouldDeployDb')]",
    "databaseServerLocation": "[parameters('location')]",
    "databaseServerAdminLogin": "[parameters('adminUser')]",
    "databaseServerAdminLoginPassword": "[parameters('adminPassword')]",
    "emailAddresses": "[parameters('emailAddresses')]"
  },
  "resources": [
    {
      "type": "Microsoft.Sql/servers",
      "name": "[variables('databaseServerName')]",
      "location": "[variables('databaseServerLocation')]",
      "apiVersion": "2015-05-01-preview",
      "properties": {
        "administratorLogin": "[variables('databaseServerAdminLogin')]",
        "administratorLoginPassword": "[variables('databaseServerAdminLoginPassword')]",
        "version": "12.0"
      },
      "tags": {
        "DisplayName": "[variables('databaseServerName')]"
      },
      "resources": [
        {
          "type": "securityAlertPolicies",
          "name": "DefaultSecurityAlert",
          "apiVersion": "2017-03-01-preview",
          "dependsOn": [
            "[variables('databaseServerName')]"
          ],
          "properties": {
            "state": "Enabled",
            "emailAddresses": "[variables('emailAddresses')]",
            "emailAccountAdmins": true
          }
        }
      ]
    },
    {
      "condition": "[equals(variables('shouldDeployDb'), 'Yes')]",
      "type": "Microsoft.Sql/servers/databases",
      "name": "[concat(string(variables('databaseServerName')), '/', string(variables('databaseName')))]",
      "location": "[variables('databaseServerLocation')]",
      "apiVersion": "2017-10-01-preview",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('databaseServerName'))]"
      ],
      "properties": {},
      "tags": {
        "DisplayName": "[variables('databaseServerName')]"
      }
    }
  ]
}
```

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Farmtutorials.blob.core.windows.net%2Fcreatesql%2Fazuredeploy.json"><img src="./media/sql-database-single-database-get-started-template/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Aşağıdaki değerleri seçin veya girin.  

    ![Resource Manager şablonu, azure sql veritabanı oluşturma](./media/sql-database-single-database-get-started-template/create-azure-sql-database-resource-manager-template.png)

    Belirtilmediyse, varsayılan değerleri kullanın.

    * **Abonelik**: Bir Azure aboneliği seçin.
    * **Kaynak grubu**: seçin **Yeni Oluştur**, kaynak grubu için benzersiz bir ad girin ve ardından **Tamam**. 
    * **Konum**: Bir konum seçin.  Örneğin, **Orta ABD**.
    * **Yönetici kullanıcı**: bir SQL veritabanı sunucusu yönetici kullanıcı adı belirtin.
    * **Yönetici parolası**: bir yönetici parolasını belirtin. 
    * **Yukarıdaki hüküm ve koşulları durumu için kabul ediyorum**: Seçin.
3. **Satın al**'ı seçin.

## <a name="query-the-database"></a>Veritabanını sorgulama

Veritabanını sorgulamak için bkz. [veritabanını sorgulama](./sql-database-single-database-get-started.md#query-the-database).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynak grubu, veritabanı sunucusu ve tek veritabanı için gitmek isterseniz tutmak [sonraki adımlar](#next-steps). Sonraki adımlar bağlanın ve farklı yöntemler kullanarak veritabanını sorgulama işlemini göstermektedir.

Azure CLI veya Azure Powershell kullanarak kaynak grubunu silmek için:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName 
```

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName 
```

## <a name="next-steps"></a>Sonraki adımlar

- Şirket içi veya uzak Araçlar tek bir veritabanına bağlanmak için sunucu düzeyinde güvenlik duvarı kuralı oluşturun. Daha fazla bilgi için [sunucu düzeyinde güvenlik duvarı kuralı oluşturma](sql-database-server-level-firewall-rule.md).
- Bir sunucu düzeyinde güvenlik duvarı kuralı oluşturduktan sonra [bağlanma ve sorgulama](sql-database-connect-query.md) birkaç farklı araçları ve dilleri kullanarak veritabanınızı.
  - [SQL Server Management Studio kullanarak bağlanma ve sorgulama](sql-database-connect-query-ssms.md)
  - [Azure Data Studio kullanarak bağlanma ve sorgulama](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Azure CLI kullanarak tek bir veritabanı oluşturmak için bkz [Azure CLI örnekleri](sql-database-cli-samples.md).
- Azure PowerShell kullanarak tek bir veritabanı oluşturmak için bkz [Azure PowerShell örnekleri](sql-database-powershell-samples.md).
