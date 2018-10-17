---
title: Bağlı Azure Resource Manager şablonları oluşturma | Microsoft Docs
description: Sanal makine oluşturmak için bağlı Azure Resource Manager şablonları oluşturma hakkında bilgi edinin
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 09/07/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: d25ca14b78465a6c4fec7e90bc20436e3ca553fc
ms.sourcegitcommit: 3150596c9d4a53d3650cc9254c107871ae0aab88
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47419636"
---
# <a name="tutorial-create-linked-azure-resource-manager-templates"></a>Öğretici: Bağlı Azure Resource Manager şablonları oluşturma

Bağlı Azure Resource Manager şablonları oluşturma hakkında bilgi edinin. Bağlı şablonları kullanarak bir şablonun başka bir şablonu çağırmasını sağlayabilirsiniz. Şablonları modüllere ayırmak için harika bir yoldur. Bu öğreticide, bir sanal makine, bir sanal ağ ve depolama hesabı gibi başka bir bağımlı kaynak oluşturan [Öğretici: Resource Manager şablonları kullanarak birden fazla kaynak örneği oluşturma](./resource-manager-tutorial-create-multiple-instances.md) içinde kullandığınız şablonun aynısını kullanacaksınız. Depolama hesabı kaynağını bağlı bir şablona ayıracaksınız.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Bağlı şablon oluşturma
> * Bağlı şablonu karşıya yükleme
> * Bağlı şablona bağlama
> * Bağımlılık yapılandırma
> * Bağlı şablondan değerleri alma
> * Şablonu dağıtma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* [Visual Studio Code](https://code.visualstudio.com/).
* Resource Manager Araçları uzantısı.  Bkz. [Uzantıyı yükleme](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* [Öğretici: Resource Manager şablonları kullanarak birden çok kaynak örneği oluşturma](./resource-manager-tutorial-create-multiple-instances.md) öğreticisini tamamlayın.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure Hızlı Başlangıç Şablonları, Resource Manager şablonları için bir depolama alanıdır. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma). Bu şablon, [Öğretici: Resource Manager şablonları kullanarak birden çok kaynak örneği oluşturma](./resource-manager-tutorial-create-multiple-instances.md) içinde kullanılan şablonla aynıdır. Aynı şablonun iki kopyasını aşağıdaki amaçlarla kullanılacak şekilde kaydedin:

- **Ana şablon**: Depolama hesabı dışındaki tüm kaynakları oluşturur.
- **Bağlı şablon**: Depolama hesabını oluşturur.

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.
4. **Dosya**>**Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy.json** adıyla kaydedin.
5. Dosyanın **linkedTemplate.json** adıyla başka bir kopyasını oluşturmak için **Dosya**>**Farklı Kaydet**’i seçin.

## <a name="create-the-linked-template"></a>Bağlı şablon oluşturma

Bağlı şablon bir depolama hesabı oluşturur. Bağlı şablon, bir depolama hesabı oluşturan bağımsız şablon ile neredeyse aynıdır. Bu öğreticide, bağlı şablonun bir değeri ana şablona geri geçirmesi gerekir. Bu değer `outputs` öğesinde tanımlanır.

1. Açık değilse, Visual Studio Code’da linkedTemplate.json dosyasını açın.
2. Aşağıdaki değişiklikleri yapın:

    - Depolama hesabı dışındaki tüm kaynakları kaldırın. Toplam dört kaynağı kaldırırsınız.
    - **outputs** öğesini güncelleştirdiğinizde aşağıdaki gibi görünür:

        ```json
        "outputs": {
            "storageUri": {
                "type": "string",
                "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
              }
        }
        ```
        **storageUri**, ana şablonda sanal makine kaynak tanımı için gereklidir.  Değeri bir çıkış değeri olarak ana şablona geri geçirin.
    - Hiçbir zaman kullanılmayan parametreleri kaldırın. Bu parametrelerin altında yeşil bir dalgalı çizgi bulunur. Yalnızca **location** adlı bir parametreniz kalmalıdır.
    - **variables** öğesini kaldırın. Bu öğreticide gerekli değildir.
    - **storageAccountName** adlı bir parametre ekleyin. Depolama hesabı adı, ana şablondan bağlı şablona bir parametre olarak geçirilir.

    İşiniz bittiğinde, şablon şöyle görünür:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountName":{
            "type": "string",
            "metadata": {
              "description": "Azure Storage account name."
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "sku": {
              "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
          }
        ],
        "outputs": {
            "storageUri": {
                "type": "string",
                "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
              }
        }
    }
    ```
3. Değişiklikleri kaydedin.

## <a name="upload-the-linked-template"></a>Bağlı şablonu karşıya yükleme

Dağıtımı gerçekleştirdiğiniz yerden şablona erişilebilmesi gerekir. Bu konum bir Azure depolama hesabı, Github veya Dropbox olabilir. Şablonlarınız hassas bilgiler içeriyorsa, bunlara erişimi koruduğunuzdan emin olun. Bu öğreticide, [Öğretici: Resource Manager şablonları kullanarak birden fazla kaynak örneği oluşturma](./resource-manager-tutorial-create-multiple-instances.md) içinde kullandığınız Cloud shell dağıtım yönetiminin aynısını kullanacaksınız. Ana şablon (azuredeploy.json) kabuğa yüklenir. Bağlı şablonun (linkedTemplate.json) başka bir yerde paylaşılması gerekir.  Bu öğreticideki görevleri azaltmak için, önceki bölümde tanımlanan bağlı şablon [bir Azure depolama hesabına](https://armtutorials.blob.core.windows.net/linkedtemplates/linkedStorageAccount.json) yüklenmiştir.

## <a name="call-the-linked-template"></a>Bağlı şablonu çağırma

Ana şablon azuredeploy.json olarak adlandırılır.

1. Açık değilse, Visual Studio Code’da azuredeploy.json dosyasını açın.
2. Depolama hesabı kaynak tanımını şablondan silin.
3. Depolama hesabı tanımının bulunduğu yere aşağıdaki json kod parçacığını ekleyin:

    ```json
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri":"https://armtutorials.blob.core.windows.net/linkedtemplates/linkedStorageAccount.json"
          },
          "parameters": {
              "storageAccountName":{"value": "[variables('storageAccountName')]"},
              "location":{"value": "[parameters('location')]"}
          }
      }
    },
    ```

    Şu ayrıntılara dikkat edin:

    - Ana şablonda bir `Microsoft.Resources/deployments` kaynağı, başka bir şablona bağlamak için kullanılır.
    - `deployments` kaynağının adı `linkedTemplate` şeklindedir. Bu ad [bağımlılık yapılandırma](#configure-dependency) için kullanılır.  
    - Bağlı şablonları çağırırken yalnızca [Artımlı](./deployment-modes.md) dağıtım modunu kullanabilirsiniz.
    - `templateLink/uri` bağlı şablon URI’sini içerir. Bağlı şablon, paylaşılan bir depolama hesabına yüklenmiştir. Şablonu İnternet üzerindeki başka bir konuma yüklerseniz URI’yi güncelleştirebilirsiniz.
    - Değerleri ana şablondan bağlı şablona geçirmek için `parameters` kullanın.
4. Değişiklikleri kaydedin.

## <a name="configure-dependency"></a>Bağımlılık yapılandırma

[Öğretici: Resource Manager şablonu kullanarak birden fazla kaynak örneği oluşturma](./resource-manager-tutorial-create-multiple-instances.md) öğreticisinden hatırlayacağınız gibi sanal makine kaynağı, depolama hesabına bağlıdır:

![Azure Resource Manager şablonları bağımlılık diyagramı](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-visual-studio-code-dependency-diagram.png)

Depolama hesabı artık bağlı şablonda tanımlandığı için, `Microsoft.Compute/virtualMachines` kaynağının aşağıdaki iki öğesini güncelleştirmeniz gerekir.

- `dependOn` öğesini yeniden yapılandırın. Depolama hesabı tanımı, bağlı şablona taşınır.
- `properties/diagnosticsProfile/bootDiagnostics/storageUri` öğesini yeniden yapılandırın. [Bağlı şablon oluşturma](#create-the-linked-template) bölümünde bir çıkış değeri eklediniz:

    ```json
    "outputs": {
        "storageUri": {
            "type": "string",
            "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
    }
    ```
    Bu değer ana şablon için gereklidir.

1. Açık değilse, Visual Studio Code’da azuredeploy.json dosyasını açın.
2. Sanal makine kaynak tanımını genişletin, **dependsOn** değerini aşağıdaki ekran görüntüsünde gösterildiği gibi güncelleştirin:

    ![Azure Resource Manager bağlı şablonları bağımlılığı yapılandırma ](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-linked-templates-configure-dependency.png)
    
    "linkedTemplate", dağıtım kaynağının adıdır.  
3. **properties/diagnosticsProfile/bootDiagnostics/storageUri** değerini önceki ekran görüntüsünde gösterildiği gibi güncelleştirin.

Daha fazla bilgi için bkz. [Azure kaynaklarını dağıtırken bağlı ve iç içe geçmiş şablonları kullanma](./resource-group-linked-templates.md)

## <a name="deploy-the-template"></a>Şablonu dağıtma

Dağıtım yordamı için [Şablonu dağıtma](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template) bölümüne bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bağlı şablonları geliştirip dağıtırsınız. Azure kaynaklarını birden fazla bölgede dağıtma ve güvenli dağıtım uygulamalarını kullanma hakkında bilgi edinmek için bkz.


> [!div class="nextstepaction"]
> [Azure Deployment Manager’ı kullanma](./deployment-manager-tutorial.md)

