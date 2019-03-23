---
title: 'Azure Resource Manager: Tek veritabanı - Azure SQL veritabanı oluşturma | Microsoft Docs'
description: Azure Resource Manager şablonu kullanarak Azure SQL veritabanı içinde tek bir veritabanı oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
manager: craigg
ms.date: 03/22/2019
ms.openlocfilehash: 01e14f86b16db6d998d60e74211ae5ad77381461
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58373024"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-resource-manager-template"></a>Hızlı Başlangıç: Azure Resource Manager şablonu kullanarak Azure SQL veritabanı tek veritabanı oluşturma

Oluşturma bir [tek veritabanı](sql-database-single-database.md) Azure SQL veritabanında bir veritabanı oluşturmak için hızlı ve kolay bir dağıtım seçeneğidir. Bu hızlı başlangıçta, Azure Resource Manager şablonu kullanarak tek veritabanı oluşturma işlemini göstermektedir. Daha fazla bilgi için [Azure Resource Manager belgelerini](/azure/azure-resource-manager/).

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-a-single-database"></a>Tek veritabanı oluşturma

Bir dizi işlem, bellek, GÇ ve depolama kaynakları iki birini kullanarak tek bir veritabanı olan [satın alma modeli](sql-database-purchase-models.md). Tek bir veritabanı oluşturduğunuzda, aynı zamanda tanımlamış bir [SQL veritabanı sunucusu](sql-database-servers.md) yönetip içine yerleştirdiğiniz [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) belirli bir bölgede.

Bu hızlı başlangıçta kullanılan şablon dandır [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/201-sql-threat-detection-server-policy-optional-db/). Aşağıdaki JSON dosyası bu makalede kullanılan şablonudur. Daha fazla Azure SQL veritabanı şablonu örnekleri bulunabilir [burada](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular).

[!code-json[create-azure-sql-database](~/quickstart-templates/201-sql-threat-detection-server-policy-optional-db/azuredeploy.json)]

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-sql-threat-detection-server-policy-optional-db%2Fazuredeploy.json"><img src="./media/sql-database-single-database-get-started-template/deploy-to-azure.png" alt="deploy to azure"/></a>

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
