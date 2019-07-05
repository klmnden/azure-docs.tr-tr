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
ms.date: 06/28/2019
ms.openlocfilehash: 4ef0f9ff6f8620109f2ef6f6bd5f549281b4de54
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67472154"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-resource-manager-template"></a>Hızlı Başlangıç: Azure Resource Manager şablonu kullanarak Azure SQL veritabanı tek veritabanı oluşturma

Oluşturma bir [tek veritabanı](sql-database-single-database.md) Azure SQL veritabanında bir veritabanı oluşturmak için hızlı ve kolay bir dağıtım seçeneğidir. Bu hızlı başlangıçta, Azure Resource Manager şablonu kullanarak tek veritabanı oluşturma işlemini göstermektedir. Daha fazla bilgi için [Azure Resource Manager belgelerini](/azure/azure-resource-manager/).

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-a-single-database"></a>Tek veritabanı oluşturma

Bir dizi işlem, bellek, GÇ ve depolama kaynakları iki birini kullanarak tek bir veritabanı olan [satın alma modeli](sql-database-purchase-models.md). Tek bir veritabanı oluşturduğunuzda, aynı zamanda tanımlamış bir [SQL veritabanı sunucusu](sql-database-servers.md) yönetip içine yerleştirdiğiniz [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) belirli bir bölgede.

Aşağıdaki JSON dosyası bu makalede kullanılan şablonudur. Şablon içinde depolanan [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/SQLServerAndDatabase/azuredeploy.json). Daha fazla Azure SQL veritabanı şablonu örnekleri bulunabilir [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular).

[!code-json[create-azure-sql-database-server-and-database](~/resourcemanager-templates/SQLServerAndDatabase/azuredeploy.json)]

1. Seçin **deneyin** Azure Cloud Shell'i açmak için aşağıdaki PowerShell kod bloğunun.

    ```azurepowershell-interactive
    $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
    $location = Read-Host -Prompt "Enter an Azure location (i.e. centralus)"
    $adminUser = Read-Host -Prompt "Enter the SQL server administrator username"
    $adminPassword = Read-Host -Prompt "Enter the SQl server administrator password" -AsSecureString

    $resourceGroupName = "${projectName}rg"


    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "D:\GitHub\azure-docs-json-samples\SQLServerAndDatabase\azuredeploy.json" -projectName $projectName -adminUser $adminUser -adminPassword $adminPassword

    Read-Host -Prompt "Press [ENTER] to continue ..."
    ```

1. Seçin **kopyalama** PowerShell Betiği panoya kopyalanmak için.
1. Kabuk bölmesinde sağ tıklayın ve ardından **Yapıştır**.

    Veritabanı sunucusu ve veritabanı oluşturmak için birkaç dakika sürer.

## <a name="query-the-database"></a>Veritabanını sorgulama

Veritabanını sorgulamak için bkz. [veritabanını sorgulama](./sql-database-single-database-get-started.md#query-the-database).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynak grubu, veritabanı sunucusu ve tek veritabanı için gitmek isterseniz tutmak [sonraki adımlar](#next-steps). Sonraki adımlar bağlanın ve farklı yöntemler kullanarak veritabanını sorgulama işlemini göstermektedir.

Azure Powershell ile kaynak grubunu silmek için:

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName

Read-Host -Prompt "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Sonraki adımlar

- Şirket içi veya uzak Araçlar tek bir veritabanına bağlanmak için sunucu düzeyinde güvenlik duvarı kuralı oluşturun. Daha fazla bilgi için [sunucu düzeyinde güvenlik duvarı kuralı oluşturma](sql-database-server-level-firewall-rule.md).
- Bir sunucu düzeyinde güvenlik duvarı kuralı oluşturduktan sonra [bağlanma ve sorgulama](sql-database-connect-query.md) birkaç farklı araçları ve dilleri kullanarak veritabanınızı.
  - [SQL Server Management Studio kullanarak bağlanma ve sorgulama](sql-database-connect-query-ssms.md)
  - [Azure Data Studio kullanarak bağlanma ve sorgulama](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Azure CLI kullanarak tek bir veritabanı oluşturmak için bkz [Azure CLI örnekleri](sql-database-cli-samples.md).
- Azure PowerShell kullanarak tek bir veritabanı oluşturmak için bkz [Azure PowerShell örnekleri](sql-database-powershell-samples.md).
