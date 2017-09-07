---
title: "Azure sanal makine ölçek kümesinde uygulama dağıtma | Microsoft Docs"
description: "Azure Resource Manager şablonunu kullanarak bir sanal makine ölçek kümesinde basit bir otomatik ölçeklendirme uygulaması dağıtmayı öğrenin."
services: virtual-machine-scale-sets
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/24/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: 07883a33382cc660b043c99872312a9e77228253
ms.contentlocale: tr-tr
ms.lasthandoff: 08/24/2017

---

# <a name="deploy-an-autoscaling-app-using-a-template"></a>Şablon kullanarak bir otomatik ölçeklendirme uygulaması dağıtma

[Azure Resource Manager şablonları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment), ilgili kaynak gruplarını dağıtmanın harika bir yoludur. Bu öğretici, [Basit bir ölçek kümesi dağıtma](virtual-machine-scale-sets-mvss-start.md) öğreticisini temel alır ve Azure Resource Manager şablonu kullanılarak bir ölçek kümesinde basit bir otomatik ölçeklendirme uygulamasının nasıl dağıtılacağını açıklar.  PowerShell, CLI veya portalı kullanarak da otomatik ölçeklendirme ayarlayabilirsiniz. Daha fazla bilgi edinmek için bkz. [Otomatik ölçeklendirmeye genel bakış](virtual-machine-scale-sets-autoscale-overview.md).

## <a name="two-quickstart-templates"></a>İki hızlı başlangıç şablonu
Ölçek kümesi dağıtırken bir [VM Uzantısını](../virtual-machines/virtual-machines-windows-extensions-features.md) kullanarak bir platform görüntüsü üzerine yeni yazılım yükleyebilirsiniz. VM uzantısı, dağıtım sonrası yapılandırma ve Azure sanal makinelerinde uygulama dağıtımı gibi otomasyon görevleri sunan küçük bir uygulamadır. [Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) bölümünde, VM uzantıları kullanılarak bir ölçek kümesine nasıl otomatik ölçeklendirme uygulaması dağıtılacağını gösteren iki farklı örnek sağlanmıştır.

### <a name="python-http-server-on-linux"></a>Linux’ta Python HTTP sunucusu
[Linux’ta Python HTTP sunucusu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) örnek şablonu, bir Linux ölçek kümesinde çalışan basit bir otomatik ölçeklendirme uygulaması dağıtır.  Özel betik VM uzantısı kullanılarak ölçek kümesindeki her VM’de bir Python web çerçevesi olan [Bottle](http://bottlepy.org/docs/dev/) ve basit bir HTTP sunucusu dağıtılır. Tüm VM’lerdeki ortalama CPU kullanımı %60’ı aştığında ölçek kümesinin ölçeği otomatik olarak artırılırken, ortalama CPU kullanımı %30’un altında düştüğünde ölçek azaltılır.

Ölçek kümesi kaynağına ek olarak *azuredeploy.json* örnek şablonu da sanal ağ, genel IP adresi, yük dengeleyici ve otomatik ölçeklendirme ayarları kaynakları bildirir.  Bir şablonda bu kaynakları oluşturma hakkında daha fazla bilgi edinmek için bkz. [Otomatik ölçeklendirmeli Linux ölçek kümesi](virtual-machine-scale-sets-linux-autoscale.md).

*azuredeploy.json* şablonunda, `Microsoft.Compute/virtualMachineScaleSets` kaynağının `extensionProfile` özelliği özel bir betik uzantısını belirtir. `fileUris`, betiğin konumunu belirtir. Bu örnekte iki dosya vardır: Basit bir HTTP sunucusu tanımlayan *workserver.py* ve Bottle’ı yükleyip HTTP sunucusunu başlatan *installserver.sh*. `commandToExecute`, ölçek kümesi dağıtıldıktan sonra çalıştırılacak komutu belirtir.

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "lapextension",
                "properties": {
                  "publisher": "Microsoft.Azure.Extensions",
                  "type": "CustomScript",
                  "typeHandlerVersion": "2.0",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "fileUris": [
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
                    ],
                    "commandToExecute": "bash installserver.sh"
                  }
                }
              }
            ]
          }
```

### <a name="aspnet-mvc-application-on-windows"></a>Windows’da ASP.NET MVC uygulaması
[Windows’da ASP.NET MVC uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) örnek şablonu, Windows ölçek kümesinde IIS’de çalışan basit bir ASP.NET MVC uygulaması dağıtır.  IIS ve MVC uygulaması, [PowerShell istenen durum yapılandırması (DSC)](virtual-machine-scale-sets-dsc.md) VM uzantısı kullanılarak dağıtılır.  VM örneğindeki CPU kullanımı 5 dakika boyunca aralıksız olarak %50’nin üzerinde kalırsa ölçek kümesinin ölçeği artar. 

Ölçek kümesi kaynağına ek olarak *azuredeploy.json* örnek şablonu da sanal ağ, genel IP adresi, yük dengeleyici ve otomatik ölçeklendirme ayarları kaynakları bildirir. Bu şablon, uygulama yükseltme işlemini de gösterir.  Bir şablonda bu kaynakları oluşturma hakkında daha fazla bilgi edinmek için bkz. [Otomatik ölçeklendirmeli Windows ölçek kümesi](virtual-machine-scale-sets-windows-autoscale.md).

*azuredeploy.json* şablonunda, `Microsoft.Compute/virtualMachineScaleSets` kaynağının `extensionProfile` özelliği bir [istenen durum yapılandırması (DSC)](virtual-machine-scale-sets-dsc.md) uzantısı belirtir ve bu da bir WebDeploy paketinden IIS’yi ve varsayılan bir web uygulamasını yükler.  *IISInstall.ps1* betiği sanal makinede IIS’yi yükler ve *DSC* klasöründe bulunur.  MVC web uygulaması *WebDeploy* klasöründe bulunur.  Yükleme betiği ve web uygulamasının yolları, *azuredeploy.parameters.json* dosyasındaki `powershelldscZip` ve `webDeployPackage` parametrelerinde tanımlanır. 

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Powershell.DSC",
                "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.9",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('powershelldscUpdateTagVersion')]",
                  "settings": {
                    "configuration": {
                      "url": "[variables('powershelldscZipFullPath')]",
                      "script": "IISInstall.ps1",
                      "function": "InstallIIS"
                    },
                    "configurationArguments": {
                      "nodeName": "localhost",
                      "WebDeployPackagePath": "[variables('webDeployPackageFullPath')]"
                    }
                  }
                }
              }
            ]
          }
```

## <a name="deploy-the-template"></a>Şablonu dağıtma
[Linux’ta Python HTTP sunucusu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) veya [Windows’da ASP.NET MVC uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) şablonunu dağıtmanın en basit yolu, GitHub’daki benioku dosyalarında bulunan **Azure’a Dağıt** düğmesini kullanmaktır.  Örnek şablonları dağıtmak için PowerShell veya Azure CLI aracını da kullanabilirsiniz.

### <a name="powershell"></a>PowerShell
[Linux’ta Python HTTP sunucusu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) veya [Windows’da ASP.NET MVC uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) dosyalarını GitHub deposundan yerel bilgisayarınızdaki bir klasöre kopyalayın.  *azuredeploy.parameters.json* dosyasını açıp `vmssName`, `adminUsername` ve `adminPassword` parametrelerini güncelleştirin. Aşağıdaki PowerShell betiğini *azuredeploy.json* şablonuyla aynı klasördeki *deploy.ps1* öğesine kaydedin. Örnek şablonu dağıtmak için bir PowerShell komut penceresinden *deploy.ps1* betiğini çalıştırın.

```powershell
param(
 [Parameter(Mandatory=$True)]
 [string]
 $subscriptionId,

 [Parameter(Mandatory=$True)]
 [string]
 $resourceGroupName,

 [string]
 $resourceGroupLocation,

 [Parameter(Mandatory=$True)]
 [string]
 $deploymentName,

 [string]
 $templateFilePath = "template.json",

 [string]
 $parametersFilePath = "parameters.json"
)

<#
.SYNOPSIS
    Registers RPs
#>
Function RegisterRP {
    Param(
        [string]$ResourceProviderNamespace
    )

    Write-Host "Registering resource provider '$ResourceProviderNamespace'";
    Register-AzureRmResourceProvider -ProviderNamespace $ResourceProviderNamespace;
}

#******************************************************************************
# Script body
# Execution begins here
#******************************************************************************
$ErrorActionPreference = "Stop"

# sign in
Write-Host "Logging in...";
Login-AzureRmAccount;

# select subscription
Write-Host "Selecting subscription '$subscriptionId'";
Select-AzureRmSubscription -SubscriptionID $subscriptionId;

# Register RPs
$resourceProviders = @("microsoft.compute","microsoft.insights","microsoft.network");
if($resourceProviders.length) {
    Write-Host "Registering resource providers"
    foreach($resourceProvider in $resourceProviders) {
        RegisterRP($resourceProvider);
    }
}

#Create or check for existing resource group
$resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
if(!$resourceGroup)
{
    Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
    if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
    }
    Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else{
    Write-Host "Using existing resource group '$resourceGroupName'";
}

# Start the deployment
Write-Host "Starting deployment...";
if(Test-Path $parametersFilePath) {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
} else {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
}
```

### <a name="azure-cli"></a>Azure CLI
```azurecli
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'

# -e: immediately exit if any command has a non-zero exit status
# -o: prevents errors in a pipeline from being masked
# IFS new value is less likely to cause confusing bugs when looping arrays or arguments (e.g. $@)

usage() { echo "Usage: $0 -i <subscriptionId> -g <resourceGroupName> -n <deploymentName> -l <resourceGroupLocation>" 1>&2; exit 1; }

declare subscriptionId=""
declare resourceGroupName=""
declare deploymentName=""
declare resourceGroupLocation=""

# Initialize parameters specified from command line
while getopts ":i:g:n:l:" arg; do
    case "${arg}" in
        i)
            subscriptionId=${OPTARG}
            ;;
        g)
            resourceGroupName=${OPTARG}
            ;;
        n)
            deploymentName=${OPTARG}
            ;;
        l)
            resourceGroupLocation=${OPTARG}
            ;;
        esac
done
shift $((OPTIND-1))

#Prompt for parameters is some required parameters are missing
if [[ -z "$subscriptionId" ]]; then
    echo "Subscription Id:"
    read subscriptionId
    [[ "${subscriptionId:?}" ]]
fi

if [[ -z "$resourceGroupName" ]]; then
    echo "ResourceGroupName:"
    read resourceGroupName
    [[ "${resourceGroupName:?}" ]]
fi

if [[ -z "$deploymentName" ]]; then
    echo "DeploymentName:"
    read deploymentName
fi

if [[ -z "$resourceGroupLocation" ]]; then
    echo "Enter a location below to create a new resource group else skip this"
    echo "ResourceGroupLocation:"
    read resourceGroupLocation
fi

#templateFile Path - template file to be used
templateFilePath="template.json"

if [ ! -f "$templateFilePath" ]; then
    echo "$templateFilePath not found"
    exit 1
fi

#parameter file path
parametersFilePath="parameters.json"

if [ ! -f "$parametersFilePath" ]; then
    echo "$parametersFilePath not found"
    exit 1
fi

if [ -z "$subscriptionId" ] || [ -z "$resourceGroupName" ] || [ -z "$deploymentName" ]; then
    echo "Either one of subscriptionId, resourceGroupName, deploymentName is empty"
    usage
fi

#login to azure using your credentials
az account show 1> /dev/null

if [ $? != 0 ];
then
    az login
fi

#set the default subscription id
az account set --name $subscriptionId

set +e

#Check for existing RG
az group show $resourceGroupName 1> /dev/null

if [ $? != 0 ]; then
    echo "Resource group with name" $resourceGroupName "could not be found. Creating new resource group.."
    set -e
    (
        set -x
        az group create --name $resourceGroupName --location $resourceGroupLocation 1> /dev/null
    )
    else
    echo "Using existing resource group..."
fi

#Start deployment
echo "Starting deployment..."
(
    set -x
    az group deployment create --name $deploymentName --resource-group $resourceGroupName --template-file $templateFilePath --parameters $parametersFilePath
)

if [ $?  == 0 ];
 then
    echo "Template has been successfully deployed"
fi
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]

