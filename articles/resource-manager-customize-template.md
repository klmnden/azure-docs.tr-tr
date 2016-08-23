<properties
    pageTitle="Dışarı aktarılan Resource Manager şablonunu özelleştirme | Microsoft Azure"
    description="Dışarı aktarılan bir Azure Resource Manager şablonuna parametreleri ekleyin ve Azure PowerShell veya Azure CLI aracılığıyla yeniden dağıtın."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/01/2016"
    ms.author="tomfitz"/>

# Dışarı aktarılan bir Azure Resource Manager şablonunu özelleştirme 

Bu makalede, ek değerleri parametre olarak geçirebilmeniz için dışarı aktarılan bir şablonu nasıl değiştireceğiniz gösterilir. [Resource Manager şablonunu dışarı aktarma](resource-manager-export-template.md) makalesinde gerçekleştirilen adımları temel alır, ancak öncelikle makaleyi tamamlamanız gerekli değildir. Bu makalede gerekli şablon ve betikleri bulabilirsiniz.

## Dışarı aktarılan bir şablonu görüntüleme

[Resource Manager şablonunu dışarı aktarmayı](resource-manager-export-template.md) tamamladıysanız, indirdiğiniz şablonu açın. Şablon adı **template.json** şeklindedir. Visual Studio veya Visual Code varsa, şablonu düzenlemek için bunlardan birini kullanabilirsiniz. Bunlar yoksa, herhangi bir JSON ya da metin düzenleyicisi kullanabilirsiniz.

Önceki kılavuzu tamamlamadıysanız, **template.json** adında bir dosya oluşturun ve dışarı aktarılan şablondan aşağıdaki içeriği dosyaya ekleyin.

```
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
        },
        "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
        }
    },
    "variables": {},
    "resources": [
        {
            "comments": "Generalized from resource in walkthrough.",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetworks_VNET_name')]",
            "apiVersion": "2015-06-15",
            "location": "northeurope",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "10.0.0.0/16"
                    ]
                },
                "subnets": [
                    {
                        "name": "default",
                        "properties": {
                            "addressPrefix": "10.0.0.0/24"
                        }
                    }
                ]
            },
            "dependsOn": []
        },
        {
            "comments": "Generalized from resource in walkthrough.",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccounts_storagetf05092016_name')]",
            "apiVersion": "2015-06-15",
            "location": "northeurope",
            "tags": {},
            "properties": {
                "accountType": "Standard_RAGRS"
            },
            "dependsOn": []
        }
    ]
}
```

Her dağıtım için aynı adres ön ekini ve aynı alt ağ ön ekini kullanan bir sanal ağ ile aynı bölgede aynı türde depolama hesabı oluşturmak istiyorsanız, template.json şablonu düzgün çalışır. Ancak Resource Manager, şablonları bundan çok daha fazla esneklikle dağıtabileceğiniz seçenekler sunar. Örneğin, dağıtım sırasında, oluşturulacak depolama hesabı türünü ya da sanal ağ adresi ön eki ve alt ağ ön eki için kullanılacak değerler türünü belirtebilirsiniz.

## Şablonu özelleştirme

Bu bölümde, bu kaynakları diğer ortamlara dağıttığınızda şablonu yeniden kullanabilmeniz için, dışarı aktarılan şablona parametreler ekleyeceksiniz. Ayrıca, şablonu dağıttığınızda bir hata ile karşılaşma olasılığını azaltmak için şablonunuza bazı özellikler ekleyeceksiniz. Artık depolama hesabınız için benzersiz bir ad düşünmeniz gerekmeyecek. Bunun yerine, şablon benzersiz adı kendi oluşturur. Depolama hesabı türü için belirtilebilecek değerleri yalnızca geçerli seçeneklerle kısıtlarsınız.

1. Dağıtım sırasında belirtmek isteyebileceğiniz değerleri geçirebilmek için, **parameters** bölümünü aşağıdaki parametre tanımlarıyla değiştirin. **storageAccount_accountType** için **allowedValues** değerlerini not alın. Yanlışlıkla geçersiz bir değer sağlarsanız, dağıtım başlamadan önce bu hata tanınır. Ayrıca, depolama hesabı adı için yalnızca bir ön ek sağladığınızı ve ön ekin 11 karakterle sınırlı olduğuna dikkat edin. Ön eki 11 karakterle sınırladığınızda, tam adın depolama hesabı için maksimum karakter sayısını aşmayacağından emin olursunuz. Ön ek, depolama hesaplarınıza bir adlandırma kuralı uygulamanızı sağlar. Sonraki adımda benzersiz bir ad oluşturmayı göreceksiniz.

        "parameters": {
          "storageAccount_prefix": {
            "type": "string",
            "maxLength": 11
          },
          "storageAccount_accountType": {
            "defaultValue": "Standard_RAGRS",
            "type": "string",
            "allowedValues": [
              "Standard_LRS",
              "Standard_ZRS",
              "Standard_GRS",
              "Standard_RAGRS",
              "Premium_LRS"
            ]
          },
          "virtualNetwork_name": {
            "type": "string"
          },
          "addressPrefix": {
            "defaultValue": "10.0.0.0/16",
            "type": "string"
          },
          "subnetName": {
            "defaultValue": "subnet-1",
            "type": "string"
          },
          "subnetAddressPrefix": {
            "defaultValue": "10.0.0.0/24",
            "type": "string"
          }
        },

2. Şablonunuzdaki **variables** bölümü şu anda boştur. Bu bölümü aşağıdaki kod ile değiştirin. **variables** bölümünde, şablon yazarı olarak, şablonunuzun geri kalanı için söz dizimini basitleştiren değerler oluşturabilirsiniz. **storageAccount_name** değişkeni, kaynak grubunun tanımlayıcısına göre oluşturulan benzersiz bir dizeye parametre ön ekini art arda ekler.

        "variables": {
          "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
        },

3. Kaynak tanımlarında parametreler ve değişken kullanmak için, **resources** bölümünü aşağıdaki tanımlarla değiştirin. Kaynak özelliğine atanan değer dışında, kaynak tanımlarında gerçekte çok az değişiklik gerçekleştiğine dikkat edin Özellikler, dışarı aktarılan şablondaki özelliklerle tamamen aynıdır. Yaptığınız, özellikler sabit kodlanmış değerler yerine parametre değerlerine atamaktır. Kaynakların konumu, **resourceGroup().location** ifadesi aracılığıyla kaynak grubu olarak aynı konumu kullanacak şekilde ayarlanır. Depolama hesabı adı için oluşturduğunuz değişkene **variables** ifadesi aracılığıyla başvurulur.

        "resources": [
          {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetwork_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "properties": {
              "addressSpace": {
                "addressPrefixes": [
                  "[parameters('addressPrefix')]"
                ]
              },
              "subnets": [
                {
                  "name": "[parameters('subnetName')]",
                  "properties": {
                    "addressPrefix": "[parameters('subnetAddressPrefix')]"
                  }
                }
              ]
            },
            "dependsOn": []
          },
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccount_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {},
            "properties": {
                "accountType": "[parameters('storageAccount_accountType')]"
            },
            "dependsOn": []
          }
        ]

## Parametre dosyasını düzeltme

İndirilen parametre dosyası artık şablonunuzdaki parametrelerle eşleşmiyor. Bir parametre dosyası kullanmak zorunda değilsiniz. Dosya kullanırsanız, bir ortamı yeniden dağıtırken işiniz kolaylaşabilir. Böylece, parametre dosyanıza yalnızca iki değer gerekecek şekilde, parametrelerin birçoğu için şablonda tanımlanan varsayılan değerleri kullanırsınız.

parameters.json dosyasının içeriğini aşağıdaki kodla değiştirin:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccount_prefix": {
      "value": "storage"
    },
    "virtualNetwork_name": {
      "value": "VNET"
    }
  }
}
```

Güncelleştirilmiş parametre dosyası, yalnızca varsayılan değere sahip olmayan parametreler için değerler sağlar. Varsayılan değerden farklı bir değer istediğinizde diğer parametreler için değerler sağlayabilirsiniz.

## Şablonunuzu dağıtma

Özelleştirilmiş şablonunuzu ve parametre dosyalarınızı dağıtmak için Azure PowerShell veya Azure komut satırı arabirimini (CLI) kullanabilirsiniz. Gerekirse, [Azure PowerShell](powershell-install-configure.md) veya [Azure CLI](xplat-cli-install.md)’yı yükleyin. Özgün şablonu dışarı aktardığınızda, şablonunuzla birlikte indirdiğiniz betikleri kullanabilir veya şablonu dağıtmak için kendi betiğinizi yazabilirsiniz.
Bu makalede her iki seçenek de gösterilmektedir.

2. Kendi betiğinizi kullanarak dağıtmak için, aşağıdakilerden birini kullanın.

     PowerShell için şunu çalıştırın:

        # login
        Add-AzureRmAccount

        # create a new resource group
        New-AzureRmResourceGroup -Name ExportGroup -Location "West Europe"

        # deploy the template to the resource group
        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExportGroup -TemplateFile {path-to-file}\template.json -TemplateParameterFile {path-to-file}\parameters.json

     Azure CLI için şunu çalıştırın:

        azure login

        azure group create -n ExportGroup -l "West Europe"

        azure group deployment create -f {path-to-file}\azuredeploy.json -e {path-to-file}\parameters.json -g ExportGroup -n ExampleDeployment

3. Dışarı aktarılan şablonu ve betikleri indirdiyseniz, **deploy.ps1** dosyasını (PowerShell için) veya **deploy.sh** dosyasını (Azure CLI için) bulun.

     PowerShell için şunu çalıştırın:

        Get-Item deploy.ps1 | Unblock-File

        .\deploy.ps1

     Azure CLI için şunu çalıştırın:

        .\deploy.sh

## Sonraki adımlar

- [Resource Manager Şablonu Kılavuzu](resource-manager-template-walkthrough.md), daha karmaşık bir çözüme yönelik şablon oluşturmak için, bu makalede öğrendiklerinizi temel alır. Kullanabileceğiniz kaynaklar ve sağlanacak değerlerin nasıl belirleneceği hakkında daha fazla bilgi edinmenize yardımcı olur.
- Bir şablonu PowerShell aracılığıyla nasıl dışarı aktaracağınızı görmek için bkz. [Azure Resource Manager ile Azure PowerShell’i Kullanma](powershell-azure-resource-manager.md).
- Bir şablonu Azure CLI aracılığıyla nasıl dışarı aktaracağınızı görmek için bkz. [Azure Resource Manager ile Mac, Linux ve Windows için Azure CLI’yi Kullanma](xplat-cli-azure-resource-manager.md).
- Şablonların nasıl yapılandırıldığı hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).



<!--HONumber=Aug16_HO1-->


