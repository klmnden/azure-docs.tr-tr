<properties
   pageTitle="Resource Manager Şablonu Kılavuzu | Microsoft Azure"
   description="Temel Azure IaaS mimarisi sağlayan bir kaynak Resource Manager şablonu ile ilgili adım adım yönergeler."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager=""
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="navale;tomfitz"/>
   

# Resource Manager Şablonu kılavuzu

Bir şablon oluştururken “nasıl başlayacağım?” sorusu ilk akla gelen sorulardan biridir. [Şablon Yazma makalesinde](resource-group-authoring-templates.md#template-format) açıklanan temel yapı izlenerek boş bir şablondan başlanabilir ve ardından kaynaklar ile ilgili parametreler ve değişkenler eklenebilir. Bunun dışında [hızlı başlama galerisi](https://github.com/Azure/azure-quickstart-templates) kullanılarak oluşturulmak istenen senaryolara benzer senaryolar aranması iyi bir alternatif oluşturabilir. Çok sayıda şablonu birleştirebilir veya kendi belirli senaryonuza uyacak mevcut bir senaryoyu düzenleyebilirsiniz. 

Ortak altyapıya bir bakalım.

* Aynı depolama hesabını kullanan iki sanal makine aynı kullanılabilirlik kümesinde ve bir sanal ağın aynı alt ağında yer alıyor.
* Her sanal makine için tek bir NIC ile VM IP adresi sağlanıyor.
* Bağlantı noktası 80 üzerinde bir yük dengeleme kuralıyla bir yük dengeleyici yer alıyor

![architecture](./media/resource-group-overview/arm_arch.png)

Bu konu, söz konusu altyapı için bir Resource Manager şablonu oluşturma adımlarını adım adım açıklamaktadır. Oluşturduğunuz son şablon ise [Load Balancer ve yük dengeleme kuralları içinde 2 VM](https://azure.microsoft.com/documentation/templates/201-2-vms-loadbalancer-lbrules/) olarak adlandırılan Hızlı başlatma şablonunu temel alıyor.

Ancak bunların tamamının aynı anda oluşturulması çok karmaşıktır, bu nedenle ilk olarak bir depolama hesabı oluşturup bunu dağıtalım. Depolama hesabı oluşturma konusunda uzmanlaştıktan sonra altyapıyı tamamlamak için diğer kaynakları ekleyeceksiniz ve şablonu yeniden dağıtacaksınız.

>[AZURE.NOTE] Şablonu oluştururken istediğiniz türde bir düzenleyici kullanabilirsiniz. Visual Studio şablon geliştirmeyi kolaylaştıran araçlar sağlar, ancak bu öğreticiyi tamamlamak için Visual Studio gerekmez. Web Uygulaması ve SQL Database dağıtımı oluşturmak için Visual Studio kullanımı ile ilgili bir öğreti için bkz. [Visual Studio üzerinden Azure kaynak gruplarını oluşturma ve dağıtma](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). 

## Resource Manager şablonu oluşturma

Şablon, dağıtacağınız tüm kaynakları tanımlayan bir JSON dosyasıdır. Bunun yanında dağıtım sırasında belirlenen parametreleri, diğer değerler ve ifadelerden oluşturulan değişkenleri ve dağıtım çıktılarını tanımlamanıza izin verir. 

En basit şablonla başlayalım:

```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {  },
      "variables": {  },
      "resources": [  ],
      "outputs": {  }
    }
 ```

Bu dosyayı **azuredeploy.json** olarak kaydedin (json dosyası olduğu sürece şablonu istediğiniz adla kaydedebileceğinizi unutmayın).

## Depolama hesabı oluşturma
**Kaynaklar** bölümünün içinde aşağıda gösterildiği şekilde depolama hesabını tanımlayan bir nesne ekleyin. 

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "accountType": "Standard_LRS"
    }
  }
]
```

Bu özellikler ve değerlerin nereden geldiğini merak ediyor olabilirsiniz. **Tür**, **ad**, **apiVersion**, ve **konum** özellikleri tüm kaynak türleri için geçerli standart öğelerdir. [Kaynaklar](resource-group-authoring-templates.md#resources)’da yer alan ortak öğeler hakkında bilgi edinebilirsiniz. **Ad**, geliştirme sırasında geçirdiğiniz parametre değerine ayarlanır ve **konum** ise kaynak grubu tarafından kullanılan konumu belirtir. **tür** ve **apiVersion**’un nasıl belirlendiğini aşağıdaki bölümlerde göreceğiz.

**Özellikler** bölümü belirli bir kaynak türü için benzersiz tüm özellikleri içerir. Bu bölümde belirlediğiniz değerler, ilgili kaynak türünü oluştururken REST API’sindeki PUT işlemiyle tam olarak eşleşir. Bir depolama hesabı oluştururken bir **accountType** sağlamanız gerekir. [Bir Depolama hesabı oluştururken REST API](https://msdn.microsoft.com/library/azure/mt163564.aspx)’de, REST işleminin özellikler bölümünün de **accountType** özelliğini içerdiğini ve izin verilen değerlerin belgelendiğini unutmayın. Bu örnekte hesap türü **Standard_LRS** olarak ayarlanmıştır ancak başka bir değer belirleyebilirsiniz veya kullanıcıların hesap türünü bir parametre olarak geçirmesine izin verebilirsiniz.

Şimdi **parametreler** bölümüne geri gidelim ve depolama hesabının adını nasıl tanımladığınızı görelim. [Parametreler](resource-group-authoring-templates.md#parameters)’de parametre kullanımı ile ilgili daha fazla bilgiye erişebilirsiniz. 

```json
"parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
}
```
Burada depolama hesabının adını içeren tür dizesinin bir parametresini tanımladınız. Bu parametre için değer şablon dağıtımı sırasında sağlanacaktır.

## Şablon dağıtma
Yeni bir depolama hesabı oluşturmak için tam bir şablonumuz var. Hatırlayacağınız üzere şablon **azuredeploy.json** dosyası olarak kaydedilmişti:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
  },  
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('storageAccountName')]",
      "apiVersion": "2015-06-15",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    }
  ]
}
```

[Kaynak Dağıtma makalesi](resource-group-template-deploy.md)nde gördüğünüz üzere bir şablonu dağıtmak için çeşitli yöntemler vardır. Azure PowerShell kullanarak şablonu dağıtmak için şunu kullanın:

```powershell
# create a new resource group
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West Europe"

# deploy the template to the resource group
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile azuredeploy.json
```

Veya Azure CLI kullanarak şablonu dağıtmak için şunu kullanın:

```
azure group create -n ExampleResourceGroup -l "West Europe"

azure group deployment create -f azuredeploy.json -g ExampleResourceGroup -n ExampleDeployment
```

Artık bir depolama hesabı sahibisiniz!

Sonraki adımlar, bu öğretinin başında açıklanan ve mimarinin dağıtılması için gerekli tüm kaynaklarının eklenmesini içermektedir. Bu kaynakları, üzerinde çalıştığınız aynı şablona ekleyeceksiniz.

## Kullanılabilirlik Kümesi
Depolama hesabının tanımlanmasından sonra sanal makineler için bir kullanılabilirlik kümesi ekleyin. Bu durumda herhangi bir ek özellik gerekli değildir, bu nedenle tanım oldukça basittir. Güncelleme etki alanı sayacı ve hata etki alanı sayacı değerlerini tanımlamak istiyorsanız tüm özellikler bölümü için bkz. [Bir Kullanılabilirlik Kümesi oluşturmak için REST API](https://msdn.microsoft.com/library/azure/mt163607.aspx).

```json
{
   "type": "Microsoft.Compute/availabilitySets",
   "name": "[variables('availabilitySetName')]",
   "apiVersion": "2015-06-15",
   "location": "[resourceGroup().location]",
   "properties": {}
}
```

**Ad** alanının bir değişken değerine ayarlandığını unutmayın. Bu şablon için kullanılabilirlik kümesinin adı farklı yerlerde gereklidir. Bu değeri bir kez tanımlayıp birden çok farklı yerde kullanarak şablonunuzu kolayca kullanabilirsiniz.

**Tür** için belirlediğiniz değeri hem kaynak sağlayıcısı hem de kaynak türünü içerir. Kullanılabilirlik kümeleri için kaynak sağlayıcısı **Microsoft.Compute**, kaynak türü ise **availabilitySets**’tir. Aşağıdaki PowerShell komutunu çalıştırarak, kullanılabilir kaynak sağlayıcıları listesini alabilirsiniz:

```powershell
    Get-AzureRmResourceProvider -ListAvailable
```

Veya, Azure CLI kullanıyorsanız, aşağıdaki komutu çalıştırabilirsiniz:
```
    azure provider list
```
Bu konuda oluşturma işlemlerini depolama hesapları, sanal makineler ve sanal ağ oluşturma ile gerçekleştireceğiniz için aşağıdakilerle çalışacaksınız:

- Microsoft.Storage
- Microsoft.Compute
- Microsoft.Network

Belirli bir sağlayıcı için kaynak türlerini görmek için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes
```

Veya, Azure CLI için aşağıdaki komut kullanılabilir türleri JSON biçiminde döndürecek ve bir dosyaya kaydedecektir.

```
    azure provider show Microsoft.Compute --json > c:\temp.json
```

**Microsoft.Compute** içinde türlerden biri olarak **availabilitySets** görünüyor olmalıdır. Türün tam adı **Microsoft.Compute/availabilitySets**’tir. Şablonunuzda kaynaklar için kaynak türü adını belirleyebilirsiniz.

## Genel IP
Genel bir IP adresi tanımlayın. Ayarlanacak özellikler için tekrar [Genel IP adresleri için REST API](https://msdn.microsoft.com/library/azure/mt163590.aspx)’ye bakın.

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[parameters('publicIPAddressName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsNameforLBIP')]"
    }
  }
}
```

Ayırma yöntemi **Dinamik** olarak ayarlanmıştır ancak istediğiniz bir değere veya bir parametre değeri kabul edecek şekilde ayarlayabilirsiniz. Şablonunuzun kullanıcılarını, etki alanı adı etiketi için bir değer geçirmek üzere etkinleştirdiniz.

Şimdi **apiVersion**’un nasıl belirlendiğine göz atalım. Belirlediğiniz değer basit bir şekilde, kaynak oluştururken kullanmak istediğiniz REST API’nin sürümüyle eşleşir. Bu nedenle ilgili kaynak türü için REST API belgelerine göz atabilirsiniz. Veya belirli bir tür için aşağıdaki PowerShell komutunu çalıştırabilirsiniz.

```powershell
    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Network).ResourceTypes | Where-Object ResourceTypeName -eq publicIPAddresses).ApiVersions
```
Aşağıdaki değerleri döndürür:

    2015-06-15
    2015-05-01-preview
    2014-12-01-preview

Azure CLI ile API sürümlerini görmek için daha önce gösterilen aynı **azure provider show** komutunu çalıştırın.

Yeni bir şablon oluştururken en güncel API sürümünü seçin.

## Sanal ağ ve alt ağ
Bir alt ağ ile bir sanal ağ oluşturun. Ayarlanacak tüm özellikler için [Sanal ağlar için REST API](https://msdn.microsoft.com/library/azure/mt163661.aspx)’ye göz atın.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/virtualNetworks",
   "name": "[parameters('vnetName')]",
   "location": "[resourceGroup().location]",
   "properties": {
     "addressSpace": {
       "addressPrefixes": [
         "10.0.0.0/16"
       ]
     },
     "subnets": [
       {
         "name": "[variables('subnetName')]",
         "properties": {
           "addressPrefix": "10.0.0.0/24"
         }
       }
     ]
   }
}
```

## Yük dengeleyici
Şimdi dışa dönük bir yük dengeleyici oluşturacaksınız. Bu yük dengeleyici genel IP adresini kullandığı için **dependsOn** bölümünde genel IP adresi üzerinde bir bağımlılık belirtmeniz gerekir. Bu, yük dengeleyicinin genel IP adresinin dağıtımı tamamlanmadan dağıtılmayacağı anlamına gelir. Bu bağımlılığı tanımlamadığınızda, Resource Manager kaynakları paralel olarak dağıtmayı deneyeceği ve yük dengeleyiciyi henüz mevcut olmayan genel IP adreslerine ayarlamayı deneyeceği için bir hata alacaksınız. 

Bunun yanında VM’ler içinde bir arka uç adres havuzu, birkaç adet RDP’ye gelen NAT kuralı ve bu kaynak tanımı içinde bağlantı noktası 80 üzerinde bir tcp araştırması ile bir yük dengeleme kuralı oluşturacaksınız  Tüm özellikler için [Yük dengeleyici için REST API](https://msdn.microsoft.com/library/azure/mt163574.aspx)’ye göz atın.

```json
{
   "apiVersion": "2015-06-15",
   "name": "[parameters('lbName')]",
   "type": "Microsoft.Network/loadBalancers",
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
   ],
   "properties": {
     "frontendIPConfigurations": [
       {
         "name": "LoadBalancerFrontEnd",
         "properties": {
           "publicIPAddress": {
             "id": "[variables('publicIPAddressID')]"
           }
         }
       }
     ],
     "backendAddressPools": [
       {
         "name": "BackendPool1"
       }
     ],
     "inboundNatRules": [
       {
         "name": "RDP-VM0",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50001,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       },
       {
         "name": "RDP-VM1",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50002,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       }
     ],
     "loadBalancingRules": [
       {
         "name": "LBRule",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "backendAddressPool": {
             "id": "[variables('lbPoolID')]"
           },
           "protocol": "tcp",
           "frontendPort": 80,
           "backendPort": 80,
           "enableFloatingIP": false,
           "idleTimeoutInMinutes": 5,
           "probe": {
             "id": "[variables('lbProbeID')]"
           }
         }
       }
     ],
     "probes": [
       {
         "name": "tcpProbe",
         "properties": {
           "protocol": "tcp",
           "port": 80,
           "intervalInSeconds": 5,
           "numberOfProbes": 2
         }
       }
     ]
   }
}
```

## Ağ arabirimi
Her biri bir VM için olmak üzere 2 ağ arabirimi oluşturacaksınız. Ağ arabirimleri için birbirinin kopyası girişleri eklemek yerine kopyalama döngüsü (nicLoop olarak adlandırılır) üzerinde belirtmek üzere [copyIndex () işlevini](resource-group-create-multiple.md) kullanabilir ve `numberOfInstances`değişkenlerde açıklandığı şekilde ağ arabirimi sayısını oluşturabilirsiniz. Ağ arabirimi sanal ağın ve yük dengeleyicinin oluşturulmasına bağlıdır. Yük dengeleyici adres havuzunu ve gelen NAT kurallarını yapılandırmak üzere sanal ağ oluşturulmasında tanımlanan alt ağı ve yük dengeleyici kimliğini kullanır.
Tüm özellikler için [Ağ arabirimleri için REST API](https://msdn.microsoft.com/library/azure/mt163668.aspx)’ye göz atın.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/networkInterfaces",
   "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
   "location": "[resourceGroup().location]",
   "copy": {
     "name": "nicLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "dependsOn": [
     "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]",
     "[concat('Microsoft.Network/loadBalancers/', parameters('lbName'))]"
   ],
   "properties": {
     "ipConfigurations": [
       {
         "name": "ipconfig1",
         "properties": {
           "privateIPAllocationMethod": "Dynamic",
           "subnet": {
             "id": "[variables('subnetRef')]"
           },
           "loadBalancerBackendAddressPools": [
             {
               "id": "[concat(variables('lbID'), '/backendAddressPools/BackendPool1')]"
             }
           ],
           "loadBalancerInboundNatRules": [
             {
               "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyindex())]"
             }
           ]
         }
       }
     ]
   }
}
```

## Sanal makine
[Ağ arabirimleri](#network-interface) oluşturma sırasında yaptığınız gibi copyIndex () işlevini kullanarak 2 sanal makine oluşturacaksınız.
VM oluşturma depolama hesabı, ağ arabirimi ve kullanılabilirlik kümesine bağlıdır. Bu VM, görüntü yayımlayıcı, teklif, sku ve sürümü tanımlamak için kullanılan `storageProfile` özellik- `imageReference` içinde tanımlanan bir market görüntüsünden oluşturulacaktır. Son olarak VM için tanılamayı etkinleştirmek üzere bir tanılama profili yapılandırılacaktır. 

Bir market görüntüsü için ilgili özellikleri bulmak üzere [Linux sanal makine görüntüleri](./virtual-machines/virtual-machines-linux-cli-ps-findimage.md) veya [Windows sanal makine görüntülerini seçme](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md) makalelerini izleyin.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Compute/virtualMachines",
   "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
   "copy": {
     "name": "virtualMachineLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
     "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
     "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
   ],
   "properties": {
     "availabilitySet": {
       "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
     },
     "hardwareProfile": {
       "vmSize": "[parameters('vmSize')]"
     },
     "osProfile": {
       "computerName": "[concat(parameters('vmNamePrefix'), copyIndex())]",
       "adminUsername": "[parameters('adminUsername')]",
       "adminPassword": "[parameters('adminPassword')]"
     },
     "storageProfile": {
       "imageReference": {
         "publisher": "[parameters('imagePublisher')]",
         "offer": "[parameters('imageOffer')]",
         "sku": "[parameters('imageSKU')]",
         "version": "latest"
       },
       "osDisk": {
         "name": "osdisk",
         "vhd": {
           "uri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/vhds/','osdisk', copyindex(), '.vhd')]"
         },
         "caching": "ReadWrite",
         "createOption": "FromImage"
       }
     },
     "networkProfile": {
       "networkInterfaces": [
         {
           "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
         }
       ]
     },
     "diagnosticsProfile": {
       "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net')]"
       }
     }
}
```

>[AZURE.NOTE] **3. taraf satıcılar** tarafından yayımlanan görüntüler için `plan` adlı başka bir özellik belirlemeniz gerekecektir. Bununla ilgili bir örnek hızlı başlama galerisindeki [bu şablonda](https://github.com/Azure/azure-quickstart-templates/tree/master/checkpoint-single-nic) bulunabilir. 

Şablonunuz için kaynakları tanımlamayı tamamladınız.

## Parametreler

Parametreler bölümünde, şablonu dağıtırken belirlenebilecek değerleri tanımlayın. Yalnızca dağıtım sırasında değişmesi gerektiğini düşündüğünüz değerler için parametre tanımlayın. Dağıtım sırasında sağlanmamışsa, kullanılan bir parametre için bir varsayılan değer sağlayabilirsiniz. Bunun yanında **imageSKU** parametresinde görüntülendiği şekilde izin verilen değerleri tanımlayabilirsiniz.

```json
"parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password"
      }
    },
    "dnsNameforLBIP": {
      "type": "string",
      "metadata": {
        "description": "DNS for Load Balancer IP"
      }
    },
    "vmNamePrefix": {
      "type": "string",
      "defaultValue": "myVM",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "string",
      "defaultValue": "MicrosoftWindowsServer",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "type": "string",
      "defaultValue": "WindowsServer",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter"
      ],
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "type": "string",
      "defaultValue": "myLB",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicNamePrefix": {
      "type": "string",
      "defaultValue": "nic",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "publicIPAddressName": {
      "type": "string",
      "defaultValue": "myPublicIP",
      "metadata": {
        "description": "Public IP Name"
      }
    },
    "vnetName": {
      "type": "string",
      "defaultValue": "myVNET",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "type": "string",
      "defaultValue": "Standard_D1",
      "metadata": {
        "description": "Size of the VM"
      }
    }
  }
```

## Değişkenler

Değişkenler bölümünde şablonunuzda birden çok yerde kullanılan değerleri veya diğer ifadeler veya değişkenlerden oluşturulan değerleri tanımlayabilirsiniz. Değişkenler, şablonunuzun söz dizimini basitleştirmek üzere sıkça kullanılır.

```json
"variables": {
    "availabilitySetName": "myAvSet",
    "subnetName": "Subnet-1",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
    "numberOfInstances": 2,
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LoadBalancerFrontEnd')]",
    "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/BackendPool1')]",
    "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
  }
```

Şablonu tamamladınız! Şablonunuzu, [Yük dengeleyici ve yük dengeleyici kuralları şablonu ile 2 VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules) altındaki [hızlı başlama galerisi](https://github.com/Azure/azure-quickstart-templates)ndeki tam şablonla karşılaştırabilirsiniz. Şablonunuz, farklı sürüm numaralarını temel aldığı için biraz farklı olabilir. 

Depolama hesabını dağıtırken kullandığınız aynı komutları kullanarak şablonu yeniden dağıtabilirsiniz. Resource Manager mevcut olan ve değiştirilmemiş kaynakların yeniden oluşturulmasını atlayacağı için yeniden dağıtım öncesinde depolama hesabını silmenize gerek yoktur.

## Sonraki adımlar

- ARM şablonlarının json dosyasından okunmayacak kadar büyümesi mümkün olduğundan, [Azure Resource Manager Şablonu Görselleştirici (ARMViz)](http://armviz.io/#/) ARM şablonlarını görselleştirmek için harika bir araçtır.
- Bir şablonun yapısı hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
- Bir şablonu dağıtmayı öğrenmek için bkz [Azure Resource Manager şablonu ile bir Kaynak Grubu dağıtma](resource-group-template-deploy.md)



<!--HONumber=Sep16_HO3-->


