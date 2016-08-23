<properties
    pageTitle="Azure Resource Manager şablonunu dışarı aktarma | Microsoft Azure"
    description="Mevcut bir kaynak grubundaki şablonu dışarı aktarmak için Azure Resource Manager’ı kullanın."
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
    ms.date="06/28/2016"
    ms.author="tomfitz"/>

# Mevcut kaynaklardan Azure Resource Manager şablonunu dışarı aktarma

Resource Manager, aboneliğinizde var olan kaynaklardan bir Resource Manager şablonunu dışarı aktarmanızı sağlar. Bu oluşturulan şablonu şablon söz dizimi hakkında bilgi edinmek veya çözümünüzün yeniden dağıtımını gerektiği gibi otomatikleştirmek için kullanabilirsiniz.

Bir şablonu dışarı aktarmak için iki farklı yol vardır:

- Dağıtım için kullanılan gerçek şablonu dışarı aktarabilirsiniz. Dışarı aktarılan şablon özgün şablonda tanımlananlarla tam olarak aynı parametre ve değişkenlerin tümünü içerir. Bu yaklaşım özellikle kaynakları portal üzerinden dağıttığınızda ve şimdi bu kaynakları oluşturmak için şablonun nasıl oluşturulacağını görmek istediğinizde yararlıdır.
- Kaynak grubunun geçerli durumunu temsil eden bir şablonu dışarı aktarabilirsiniz. Dışarı aktarılan şablon, dağıtım için kullanılan herhangi bir şablonu temel almaz. Bunun yerine, kaynak grubunun anlık görüntüsü olan bir şablon oluşturur. Dışarı aktarılan şablon birçok sabit kodlu değer içerir ve büyük olasılıkla normalde tanımlayacağınız kadar fazla parametre içermez. Bu yaklaşım, kaynak grubunu portal ya da komut dosyaları aracılığıyla değiştirdiğinizde ve artık kaynak grubunu şablon olarak yakalamanız gerektiğinde yararlı olur.

Her iki yaklaşım da bu konuda gösterilmiştir. [Dışarı aktarılan bir Azure Resource Manager şablonunu özelleştirme](resource-manager-customize-template.md) makalesinde kaynak grubunun geçerli durumundan oluşturulmuş bir şablonu alıp, çözümünüzü yeniden dağıtmak için daha yararlı hale getirme işlemini göreceksiniz.

Bu öğreticide, Azure portalında oturum açacak, bir depolama hesabı oluşturacak ve bu depolama hesabı için şablonu dışarı aktaracaksınız. Kaynak grubunu değiştirmek için sanal ağ ekleyeceksiniz. Son olarak, geçerli durumunu temsil eden yeni bir şablonu dışarı aktaracaksınız. Bu makale basitleştirilmiş bir altyapıya odaklanıyor olsa da, daha karmaşık bir çözüm için şablonu dışarı aktarmak üzere bu aynı adımları kullanabilirsiniz.

## Depolama hesabı oluşturma

1. [Azure portalda](https://portal.azure.com), **Yeni** > **Veri + Depolama** > **Depolama hesabı**’nı seçin.

      ![depolama oluşturma](./media/resource-manager-export-template/create-storage.png)

2. **storage** adlı, adınızın baş harflerini ve tarihi içeren bir depolama hesabı oluşturun. Depolama hesabı adının Azure’da benzersiz olması gerekir. Başlangıçta zaten kullanımda olan bir adı kullanmaya çalışırsanız, adı değiştirmeyi deneyin. Kaynak grubu için **ExportGroup** kullanın. Diğer özellikler için varsayılan değerleri kullanabilirsiniz. **Oluştur**’u seçin.

      ![depolama için değerler sağlama](./media/resource-manager-export-template/provide-storage-values.png)

Dağıtım tamamlandıktan sonra, aboneliğiniz depolama hesabını içerir.

## Şablonu dağıtım geçmişinden dışarı aktarma

1. Yeni kaynak grubunuz için kaynak grubu dikey penceresine gidin. Son dağıtım sonucunun listelendiğini göreceksiniz. Bu bağlantıyı seçin.

      ![kaynak grubu dikey penceresi](./media/resource-manager-export-template/resource-group-blade.png)

2. Grup için dağıtım geçmişini görürsünüz. Sizin durumunuzda, büyük olasılıkla yalnızca bir dağıtım listeleniyordur. Bu dağıtımı seçin.

     ![son dağıtım](./media/resource-manager-export-template/last-deployment.png)

3. Dağıtımın bir özeti gösterilir. Özet, dağıtımın ve işlemlerinin durumunu ve sağladığınız parametreler için değerleri içerir. Dağıtım için kullanılan şablonu görmek isterseniz **Şablonu görüntüle**’yi seçin.

     ![dağıtım özetini görüntüleme](./media/resource-manager-export-template/deployment-summary.png)

4. Resource Manager sizin için aşağıdaki beş dosyayı alır:

   1. **Şablon** - Çözümünüze ait altyapıyı tanımlayan şablon. Portal üzerinden depolama hesabı oluşturduğunuzda, Resource Manager bunu dağıtmak için bir şablon kullandı ve bu şablonu gelecekte başvurmak üzere kaydetti.
   2. **Parametreler**: Dağıtım sırasında değerleri geçirmek için kullanabileceğiniz bir parametre dosyası. Bu dosya, ilk dağıtım sırasında sağladığınız değerleri içerir, ancak şablonu yeniden dağıtırken bu değerleri değiştirebilirsiniz.
   3. **CLI**: Şablonu dağıtmak için kullanabileceğiniz bir Azure komut satırı arabirimi (CLI) betik dosyası.
   4. **PowerShell**: Şablonu dağıtmak için kullanabileceğiniz bir Azure PowerShell betiği.
   5. **.NET**: Şablonu dağıtmak için kullanabileceğiniz bir .NET sınıfı.

     Dosyalara dikey pencerelerdeki bağlantılar aracılığıyla ulaşılabilir. Varsayılan olarak şablon seçilidir.

       ![şablonu görüntüleme](./media/resource-manager-export-template/view-template.png)

     Şimdi şablona dikkat edin. Şablonunuz şuna benzemelidir:

        {     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",     "contentVersion": "1.0.0.0",     "parameters": {       "name": {         "type": "String"       },       "accountType": {         "type": "String"       },       "location": {         "type": "String"       },       "encryptionEnabled": {         "defaultValue": false,         "type": "Bool"       }     },     "resources": [       {         "type": "Microsoft.Storage/storageAccounts",         "sku": {           "name": "[parameters('accountType')]"         },         "kind": "Storage",         "name": "[parameters('name')]",         "apiVersion": "2016-01-01",         "location": "[parameters('location')]",         "properties": {           "encryption": {             "services": {               "blob": {                 "enabled": "[parameters('encryptionEnabled')]"               }             },             "keySource": "Microsoft.Storage"           }         }       }     ]   }
 
Depolama hesabınızı oluşturmak için kullanılan gerçek şablon budur. Farklı türlerde depolama hesapları dağıtmanızı sağlayan parametreler içerir. Bir şablonun yapısı hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md). Bir şablonda kullanabileceğiniz işlevlerin tam listesi için bkz. [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).


## Sanal ağ ekleme

Önceki bölümde indirdiğiniz şablon bu özgün dağıtım için altyapıyı temsil etti, ancak bu dağıtım sonrasında yaptığınız değişiklikleri hesaba katmayacak.
Bu sorunu anlamak için portal aracılığıyla bir sanal ağ ekleyerek kaynak grubunu değiştirelim.

1. Kaynak grubu dikey penceresinde **Ekle**’yi seçin.

      ![kaynak ekle](./media/resource-manager-export-template/add-resource.png)

2. Kullanılabilir kaynaklardan **Sanal ağ** seçeneğini belirleyin.

      ![sanal ağ seçme](./media/resource-manager-export-template/select-vnet.png)

2. Sanal ağınızı **VNET** olarak adlandırın ve diğer özellikler için varsayılan değerleri kullanın. **Oluştur**’u seçin.

      ![uyarı ayarlama](./media/resource-manager-export-template/create-vnet.png)

3. Sanal ağ kaynak grubunuza başarıyla dağıtıldıktan sonra dağıtım geçmişinize tekrar bakın. Şimdi iki dağıtım göreceksiniz. İkinci dağıtımı görmüyorsanız kaynak grubu dikey pencerenizi kapatıp yeniden açmanız gerekebilir. Daha yeni olan dağıtımı seçin.

      ![dağıtım geçmişi](./media/resource-manager-export-template/deployment-history.png)

4. Bu dağıtım için şablona bakın. Bunun, yalnızca sanal ağı eklemek için yaptığınız değişiklikleri tanımladığına dikkat edin.

Dağıtılacak birçok farklı şablonu hatırlamak yerine, çözümünüz için tüm altyapıyı tek bir işleme dağıtan şablonla çalışmak genellikle en iyi uygulamadır.


## Şablonu kaynak grubundan dışarı aktarma

Her bir dağıtım yalnızca kaynak grubunuzda yapmış olduğunuz değişiklikleri gösterir, ancak herhangi bir zamanda, tüm kaynak grubunuzun özniteliklerini göstermek için bir şablonu dışarı aktarabilirsiniz.  

1. Bir kaynak grubu için şablonu görüntülemek üzere **Şablonu dışarı aktar**’ı seçin.

      ![kaynak grubunu dışarı aktarma](./media/resource-manager-export-template/export-resource-group.png)

     Şablonu dışarı aktarma işlevini tüm kaynak türleri desteklemez. Kaynak grubunuz yalnızca bu makalede gösterilen depolama hesabı ve sanal ağı içeriyorsa bir hata görmezsiniz. Ancak, diğer kaynak türlerini oluşturduysanız dışarı aktarma ile ilgili bir sorun olduğunu bildiren bir hata görebilirsiniz. Bu sorunların nasıl ele alınacağını [Dışarı aktarma sorunlarını düzeltme](#fix-export-issues) bölümünde öğreneceksiniz.

      

2. Çözümü yeniden dağıtmak için kullanabileceğiniz beş dosyayı görmeye devam edeceksiniz, ancak bu kez şablon biraz farklıdır. Bu şablon yalnızca iki parametreye sahiptir: depolama hesabı adı için bir tane ve sanal ağ adı için bir tane.

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

     Resource Manager, dağıtım sırasında kullanılan şablonları almadı. Bunun yerine, kaynakların geçerli yapılandırmasını temel alan yeni bir şablon oluşturdu. Örneğin, depolama hesabı konumu ve çoğaltma değeri aşağıdaki şekilde ayarlanır:

        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },

3. Üzerinde yerel olarak çalışabilmek için şablonu indirin.

      ![şablonu indirme](./media/resource-manager-export-template/download-template.png)

4. Yüklediğiniz .zip dosyasını bulun ve içeriğini çıkarın. İndirilen bu şablonu kullanarak altyapınızı yeniden dağıtabilirsiniz.

## Dışarı aktarma sorunlarını düzeltme

Şablonu dışarı aktarma işlevini tüm kaynak türleri desteklemez. Bazı kaynak türleri hassas verilerin gösterilmesini önlemek için özellikle dışarı aktarılmaz. Örneğin, site yapılandırmanızda bir bağlantı dizesi varsa dışarı aktarılmış bir şablonda açıkça gösterilmesini büyük olasılıkla istemezsiniz. Eksik kaynakları şablonunuza el ile ekleyerek bu sorunu çözebilirsiniz.

> [AZURE.NOTE] Yalnızca dağıtım geçmişiniz yerine bir kaynak grubundan dışarı aktarım yaparken dışarı aktarım sorunlarıyla karşılaşırsınız. Son dağıtımınız kaynak grubunun geçerli durumunu doğru şekilde temsil ediyorsa, şablonu kaynak grubu yerine dağıtım geçmişinden dışarı aktarmanız gerekir. Yalnızca tek bir şablonda tanımlanmamış kaynak grubunda değişiklikler yaptığınızda kaynak grubundan dışarı aktarma yapın.

Örneğin, şablonu bir web uygulaması, SQL Veritabanı ve site yapılandırmasında bir bağlantı dizesi içeren bir kaynak grubundan dışarı aktarırsanız aşağıdaki iletiyi görürsünüz.

![hatayı göster](./media/resource-manager-export-template/show-error.png)

İleti seçildiğinde tam olarak hangi kaynak türlerinin dışarı aktarılmadığı gösterilir. 
     
![hatayı göster](./media/resource-manager-export-template/show-error-details.png)

Bazı yaygın düzeltmeler aşağıda gösterilmektedir. Bu kaynakları uygulamak için parametreleri şablona eklemeniz gerekir. Daha fazla bilgi için bkz. [Dışarı aktarılan bir şablonu özelleştirme ve yeniden dağıtma](resource-manager-customize-template.md).

### Bağlantı dizesi

Web siteleri kaynağında veritabanına bağlantı dizesi için bir tanım ekleyin:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "type": "config",
      "name": "connectionstrings",
      "dependsOn": [
          "[concat('Microsoft.Web/Sites/', parameters('<site-name>'))]"
      ],
      "properties": {
          "DefaultConnection": {
            "value": "[concat('Data Source=tcp:', reference(concat('Microsoft.Sql/servers/', parameters('<database-server-name>'))).fullyQualifiedDomainName, ',1433;Initial Catalog=', parameters('<database-name>'), ';User Id=', parameters('<admin-login>'), '@', parameters('<database-server-name>'), ';Password=', parameters('<admin-password>'), ';')]",
              "type": "SQLServer"
          }
      }
    }
  ]
}
```    

### Web sitesi uzantısı

Web sitesi kaynağında yüklenecek kod için bir tanım ekleyin:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "name": "MSDeploy",
      "type": "extensions",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [
        "[concat('Microsoft.Web/sites/', parameters('<site-name>'))]"
      ],
      "properties": {
        "packageUri": "[concat(parameters('<artifacts-location>'), '/', parameters('<package-folder>'), '/', parameters('<package-file-name>'), parameters('<sas-token>'))]",
        "dbType": "None",
        "connectionString": "",
        "setParameters": {
          "IIS Web Application Name": "[parameters('<site-name>')]"
        }
      }
    }
  ]
}
```

### Sanal makine uzantısı

Sanal makine uzantılarının örnekleri için bkz. [Azure Windows VM Uzantısı Yapılandırma Örnekleri](./virtual-machines/virtual-machines-windows-extensions-configuration-samples.md).

### Sanal ağ geçidi

Bir sanal ağ geçidi kaynak türü ekleyin.

```
{
  "type": "Microsoft.Network/virtualNetworkGateways",
  "name": "[parameters('<gateway-name>')]",
  "apiVersion": "2015-06-15",
  "location": "[resourceGroup().location]",
  "properties": {
    "gatewayType": "[parameters('<gateway-type>')]",
    "ipConfigurations": [
      {
        "name": "default",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('<vnet-name>'), parameters('<new-subnet-name>'))]"
          },
          "publicIpAddress": {
            "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('<new-public-ip-address-Name>'))]"
          }
        }
      }
    ],
    "enableBgp": false,
    "vpnType": "[parameters('<vpn-type>')]"
  },
  "dependsOn": [
    "Microsoft.Network/virtualNetworks/codegroup4/subnets/GatewaySubnet",
    "[concat('Microsoft.Network/publicIPAddresses/', parameters('<new-public-ip-address-Name>'))]"
  ]
},
```

### Yerel ağ geçidi

Bir yerel ağ geçidi kaynak türü ekleyin.

```
{
    "type": "Microsoft.Network/localNetworkGateways",
    "name": "[parameters('<local-network-gateway-name>')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "localNetworkAddressSpace": {
        "addressPrefixes": "[parameters('<address-prefixes>')]"
      }
    }
}
```

### Bağlantı

Bir bağlantı kaynak türü ekleyin.

```
{
    "apiVersion": "2015-06-15",
    "name": "[parameters('<connection-name>')]",
    "type": "Microsoft.Network/connections",
    "location": "[resourceGroup().location]",
    "properties": {
        "virtualNetworkGateway1": {
        "id": "[resourceId('Microsoft.Network/virtualNetworkGateways', parameters('<gateway-name>'))]"
      },
      "localNetworkGateway2": {
        "id": "[resourceId('Microsoft.Network/localNetworkGateways', parameters('<local-gateway-name>'))]"
      },
      "connectionType": "IPsec",
      "routingWeight": 10,
      "sharedKey": "[parameters('<shared-key>')]"
    }
},
```


## Sonraki adımlar

Tebrikler! Portalda oluşturduğunuz kaynaklardan bir şablonu dışarı aktarmayı öğrendiniz.

- Bu öğreticinin ikinci bölümünde, daha fazla parametre ekleyerek indirdiğiniz yeni şablonu özelleştirecek ve bir betik ile yeniden dağıtacaksınız. Bkz. [Dışarı aktarılan bir şablonu özelleştirme ve yeniden dağıtma](resource-manager-customize-template.md).
- Bir şablonu PowerShell aracılığıyla nasıl dışarı aktaracağınızı görmek için bkz. [Azure Resource Manager ile Azure PowerShell’i Kullanma](powershell-azure-resource-manager.md).
- Bir şablonu Azure CLI aracılığıyla nasıl dışarı aktaracağınızı görmek için bkz. [Azure Resource Manager ile Mac, Linux ve Windows için Azure CLI’yi Kullanma](xplat-cli-azure-resource-manager.md).



<!--HONumber=Aug16_HO1-->


