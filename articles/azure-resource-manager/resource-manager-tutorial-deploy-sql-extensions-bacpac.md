---
title: Azure Resource Manager şablonlarıyla SQL BACPAC dosyalarını içeri aktarma | Microsoft Docs
description: Azure Resource Manager şablonları ile SQL BACPAC dosyalarını içeri aktarmak için SQL veritabanı uzantısını kullanmayı öğrenin.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 04/08/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 7339c975bf979907bd1f9bbb46546b58ef7ae6e5
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59282364"
---
# <a name="tutorial-import-sql-bacpac-files-with-azure-resource-manager-templates"></a>Öğretici: Azure Resource Manager şablonları ile SQL BACPAC dosyalarını içeri aktarın

Azure Resource Manager şablonları ile bir BACPAC dosyasını içeri aktarmak için Azure SQL veritabanı Uzantıları'nı kullanmayı öğrenin. Dağıtım yapıtları dağıtımı tamamlamak için gereken ana şablon dosyası yanı sıra tüm dosyalarıdır. BACPAC dosyasını bir yapıdır. Bu öğreticide bir Azure SQL sunucusu, bir SQL veritabanı dağıtmak için bir şablon oluşturmak ve bir BACPAC dosyasını içeri aktarın. Azure sanal makine uzantıları Azure Resource Manager şablonlarını kullanarak dağıtma hakkında daha fazla bilgi için bkz: [# Öğreticisi: Sanal makine uzantıları Azure Resource Manager şablonları ile dağıtma](./resource-manager-tutorial-deploy-vm-extensions.md).

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * BACPAC dosyası hazırlama
> * Hızlı başlangıç şablonunu açma
> * Şablonu düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* [Visual Studio Code](https://code.visualstudio.com/) ve Resource Manager Araçları uzantısı. Bkz. [Uzantıyı yükleme](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Güvenliği artırmak istiyorsanız SQL Server yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için [Öğreticisi: Resource Manager şablon dağıtımı Azure anahtar kasası tümleştirme](./resource-manager-tutorial-use-key-vault.md). Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="prepare-a-bacpac-file"></a>BACPAC dosyası hazırlama

Bir BACPAC dosyası üzerinde paylaşılan bir [Azure depolama hesabı](https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac) genel erişimi. Kendiniz bir dosya oluşturmak isterseniz bkz. [Azure SQL Veritabanı’nı bir BACPAC dosyasına dışarı aktarma](../sql-database/sql-database-export.md). Dosyayı kendi belirleyeceğiniz bir konumda yayımlarsanız öğreticinin ilerleyen bölümlerinde şemayı güncelleştirmeniz gerekir.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Bu öğreticide kullanılan şablon depolanan bir [Azure depolama hesabı](https://armtutorials.blob.core.windows.net/createsql/azuredeploy.json). 

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://armtutorials.blob.core.windows.net/createsql/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.

    Şablonda tanımlanan üç kaynak vardır:

   * `Microsoft.Sql/servers`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers).
   * `Microsoft.SQL/servers/securityAlertPolicies`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/securityalertpolicies).
   * `Microsoft.SQL.servers/databases`.  Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).

     Şablonu özelleştirmeden önce temel noktaları kavramak faydalı olacaktır.
4. **Dosya**>**Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy.json** adıyla kaydedin.

## <a name="edit-the-template"></a>Şablonu düzenleme

İki ek kaynaklar için şablonu ekleyin.

* SQL veritabanı uzantısının BACPAC dosyalarını içeri aktarması için Azure hizmetlerine erişim izni vermeniz gerekir. SQL Server tanımına aşağıdaki JSON kodunu ekleyin:

    ```json
    {
        "type": "firewallrules",
        "name": "AllowAllAzureIps",
        "location": "[parameters('location')]",
        "apiVersion": "2015-05-01-preview",
        "dependsOn": [
            "[variables('databaseServerName')]"
        ],
        "properties": {
            "startIpAddress": "0.0.0.0",
            "endIpAddress": "0.0.0.0"
        }
    }
    ```

    Şablonun şu şekilde olması gerekir:

    ![Azure Resource Manager SQL uzantılarını dağıtma BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-firewall.png)

* Aşağıdaki JSON kodunu kullanarak veritabanı tanımına bir SQL Veritabanı uzantısı kaynağı ekleyin:

    ```json
    "resources": [
        {
            "name": "Import",
            "type": "extensions",
            "apiVersion": "2014-04-01",
            "dependsOn": [
                "[resourceId('Microsoft.Sql/servers/databases', variables('databaseServerName'), variables('databaseName'))]"
            ],
            "properties": {
                "storageKeyType": "SharedAccessKey",
                "storageKey": "?",
                "storageUri": "https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac",
                "administratorLogin": "[variables('databaseServerAdminLogin')]",
                "administratorLoginPassword": "[variables('databaseServerAdminLoginPassword')]",
                "operationMode": "Import",
            }
        }
    ]
    ```

    Şablonun şu şekilde olması gerekir:

    ![Azure Resource Manager SQL uzantılarını dağıtma BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac.png)

    Kaynak tanımını anlamak için bkz. [SQL Veritabanı uzantısı başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases/extensions). Önemli öğeler şunlardır:

    * **dependsOn**: SQL veritabanı oluşturulduktan sonra uzantı kaynak oluşturulması gerekir.
    * **storageKeyType**: Depolama anahtarı türü. Değer `StorageAccessKey` veya `SharedAccessKey` olabilir. Sağlanan BACPAC dosyası genel erişime açık bir Azure Depolama hesabında paylaşıldığından burada `SharedAccessKey' kullanılmıştır.
    * **Depolama anahtarı**: Depolama anahtarı. Depolama anahtarı türü SharedAccessKey olarak belirlenirse başına "?" eklenmelidir.
    * **storageUri**: Kullanılacak depolama URI'si. Sağlanan BACPAC dosyasını kullanmazsanız buradaki değerleri güncelleştirmeniz gerekir.
    * **administratorLoginPassword**: SQL yönetici parolası. Oluşturulan bir parola kullanın. [Ön koşullara](#prerequisites) bakın.

## <a name="deploy-the-template"></a>Şablonu dağıtma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Dağıtım yordamı için [Şablonu dağıtma](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) bölümüne bakın. Bunun yerine aşağıdaki PowerShell dağıtım betiğini kullanın:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the SQL admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -TemplateFile "$HOME/azuredeploy.json"
```

Oluşturulan bir parola kullanın. [Ön koşullara](#prerequisites) bakın.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Portalda yeni dağıtılan kaynak grubundaki SQL veritabanını seçin. **Sorgu düzenleyicisi (önizleme)** öğesini seçip yönetici kimlik bilgilerini girin. İki tablonun veritabanına aktarılmış olması gerekir:

![Azure Resource Manager SQL uzantılarını dağıtma BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir SQL Server ve bir SQL Veritabanı oluşturup bir BACPAC dosyasını içeri aktardınız. BACPAC dosyası, bir Azure depolama hesabında depolanır. URL herkesle dosya erişebilirsiniz. BACPAC dosyası (yapı) güvenliğini sağlamayı öğrenmek için bkz:

> [!div class="nextstepaction"]
> [Yapıtlar güvenliğini sağlama](./resource-manager-tutorial-secure-artifacts.md)
