---
title: "Azure şablonu arasındaki karmaşık değerleri geçirmek | Microsoft Docs"
description: "Gösterir karmaşık nesne durumu verileri Azure Resource Manager şablonları ve bağlı şablonları ile paylaşmak için kullanmayı yaklaşımları önerilir."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fd2f5e2d-d56d-4e01-a57d-34f3eaead4a9
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: tomfitz
ms.openlocfilehash: e163f3c2e9a78b057dc2a7a42924c59d0aac3fab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="share-state-to-and-from-azure-resource-manager-templates"></a>Azure Resource Manager şablonları gelen ve giden paylaşım durumu
Bu konu, yönetme ve şablonlar içindeki durumu paylaşımı için en iyi uygulamaları gösterir. Bu konuda gösterilen değişkenleri ve parametreleri tanımlayabilirsiniz nesnelerin türü örnekleridir rahat Dağıtım gereksinimlerinizi düzenlemek için. Bu örnekler, ortamınız için anlamlı özellik değerlerini kendi nesneleriyle uygulayabilirsiniz.

Bu konuda daha büyük bir Teknik İnceleme bir parçasıdır. Tam kağıt okumak için karşıdan [World sınıfı Resource Manager şablonları konuları ve kanıtlanmış yöntemler](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

## <a name="provide-standard-configuration-settings"></a>Standart yapılandırma ayarlarını belirtin
Toplam esneklik ve sayısız Çeşitlemeler sağlayan bir şablon sunmak yerine, genel bir desen bilinen yapılandırmaları seçimi sağlamaktır. Etkin kullanıcılar korumalı alan, küçük, Orta ve büyük gibi standart ısı boyutları seçebilirsiniz. Diğer ısı boyutları community edition veya enterprise edition gibi ürün teklifleri gösterilebilir. Diğer durumlarda, harita azaltmak gibi iş yüküne özgü yapılandırmaları bir teknolojisi – olabilir veya hiç sql.

Karmaşık nesneleriyle bazen "özellik paketleri" bilinen veri topluluklarını değişkenleri oluşturmak ve bu verileri kaynak bildirimi şablonunuzda sürücü için kullanabilirsiniz. Bu yaklaşım, müşteri için yapılandırılmış farklı boyutlarda iyi, bilinen yapılandırmaları sağlar. Bilinen yapılandırmaları, şablonun kullanıcıları gerekir, kendi boyutlandırma küme belirlemek, platform kaynak kısıtlamaları faktörü ve sonuçta elde edilen bölümleme depolama hesapları ve diğer kaynakları (nedeniyle, küme boyutu ve kaynak tanımlamak için matematik yapın kısıtlamaları). Daha iyi bir deneyim müşteri için yapmanın yanı sıra birkaç bilinen yapılandırmaları daha kolay desteklemek ve yoğunluğu daha yüksek düzeyde sunmanıza yardımcı olabilir.

Aşağıdaki örnek veri koleksiyonları için karmaşık nesneler içeren değişkenleri tanımlayın gösterilmektedir. Koleksiyonları, sanal makine boyutu, ağ ayarları, işletim sistemi ayarlarını ve kullanılabilirlik ayarları için kullanılan değerleri tanımlayın.

```json
"variables": {
  "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
  "tshirtSizeSmall": {
    "vmSize": "Standard_A1",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
    "vmCount": 2,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 1,
      "pool": "db",
      "map": [0,0],
      "jumpbox": 0
    }
  },
  "tshirtSizeMedium": {
    "vmSize": "Standard_A3",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
    "vmCount": 2,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 2,
      "pool": "db",
      "map": [0,1],
      "jumpbox": 0
    }
  },
  "tshirtSizeLarge": {
    "vmSize": "Standard_A4",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
    "vmCount": 3,
    "slaveCount": 2,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 2,
      "pool": "db",
      "map": [0,1,1],
      "jumpbox": 0
    }
  },
  "osSettings": {
    "scripts": [
      "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
    ],
    "imageReference": {
      "publisher": "Canonical",
      "offer": "UbuntuServer",
      "sku": "14.04.2-LTS",
      "version": "latest"
    }
  },
  "networkSettings": {
    "vnetName": "[parameters('virtualNetworkName')]",
    "addressPrefix": "10.0.0.0/16",
    "subnets": {
      "dmz": {
        "name": "dmz",
        "prefix": "10.0.0.0/24",
        "vnet": "[parameters('virtualNetworkName')]"
      },
      "data": {
        "name": "data",
        "prefix": "10.0.1.0/24",
        "vnet": "[parameters('virtualNetworkName')]"
      }
    }
  },
  "availabilitySetSettings": {
    "name": "pgsqlAvailabilitySet",
    "fdCount": 3,
    "udCount": 5
  }
}
```

Dikkat **tshirtSize** değişkeni, bir parametre aracılığıyla sağlanan ısı boyutu art arda ekler (**küçük**, **orta**, **büyük** ) metne **tshirtSize**. Bu ısı boyut ilişkili karmaşık nesne değişkeni almak için bu değişkeni kullanın.

Ardından şablonunda daha sonra bu değişkenleri başvuruda bulunabilir. Adlandırılmış değişkenleri ve bunların özelliklerini başvuru olanağı şablon söz dizimi basitleştirir ve bağlamı anlamak daha kolay hale getirir. Aşağıdaki örnek değerleri ayarlamak için daha önce gösterilen nesneleri kullanarak dağıtmak için bir kaynak tanımlar. Örneğin, VM boyutu değeri alarak ayarlanır `variables('tshirtSize').vmSize` disk boyutunu alınır için değer while `variables('tshirtSize').diskSize`. Ek olarak, bağlantılı bir şablon için URI değeri ile ayarlanır `variables('tshirtSize').vmTemplate`.

```json
"name": "master-node",
"type": "Microsoft.Resources/deployments",
"apiVersion": "2015-01-01",
"dependsOn": [
    "[concat('Microsoft.Resources/deployments/', 'shared')]"
],
"properties": {
    "mode": "Incremental",
    "templateLink": {
      "uri": "[variables('tshirtSize').vmTemplate]",
      "contentVersion": "1.0.0.0"
    },
    "parameters": {
      "adminPassword": {
        "value": "[parameters('adminPassword')]"
      },
      "replicatorPassword": {
        "value": "[parameters('replicatorPassword')]"
      },
      "osSettings": {
        "value": "[variables('osSettings')]"
      },
      "subnet": {
        "value": "[variables('networkSettings').subnets.data]"
      },
      "commonSettings": {
        "value": {
          "region": "[parameters('region')]",
          "adminUsername": "[parameters('adminUsername')]",
          "namespace": "ms"
        }
      },
      "storageSettings": {
        "value":"[variables('tshirtSize').storage]"
      },
      "machineSettings": {
        "value": {
          "vmSize": "[variables('tshirtSize').vmSize]",
          "diskSize": "[variables('tshirtSize').diskSize]",
          "vmCount": 1,
          "availabilitySet": "[variables('availabilitySetSettings').name]"
        }
      },
      "masterIpAddress": {
        "value": "0"
      },
      "dbType": {
        "value": "MASTER"
      }
    }
  }
}
```

## <a name="pass-state-to-a-template"></a>Bir şablon durumuna geçişi
Bir şablonu doğrudan dağıtım sırasında sağladığınız parametreler aracılığıyla içine durumu paylaşır.

Aşağıdaki tabloda şablonlarındaki yaygın olarak kullanılan parametreleri listeler.

| Ad | Değer | Açıklama |
| --- | --- | --- |
| location |Azure bölgeleri kısıtlanmış listesinden dize |Kaynakları dağıtıldığı konumu. |
| storageAccountNamePrefix |Dize |VM'in disklerini yerleştirildiği depolama hesabı için benzersiz DNS adı |
| domainName |Dize |Etki alanı adı biçiminde VM genel olarak erişilebilir jumpbox: **{domainName}. { konum}.cloudapp.com** örneğin: **mydomainname.westus.cloudapp.azure.com** |
| adminUsername |Dize |Sanal makineleri için kullanıcı adı |
| Admınpassword |Dize |VM'ler için parola |
| tshirtSize |Sunulan ısı boyutları kısıtlanmış listesinden dize |Adlandırılmış ölçek birimi boyutu sağlama. Örneğin, "Küçük", "Medium", "Büyük" |
| virtualNetworkName |Dize |Tüketici kullanmak isterse sanal ağ adı. |
| enableJumpbox |(Etkin/devre dışı) kısıtlanmış listeden dize |Jumpbox ortamı için etkinleştirilip etkinleştirilmeyeceğini tanımlayan bir parametre. Değerler: "etkin", "disabled" |

**TshirtSize** önceki bölümünde kullanılan parametre olarak tanımlanır:

```json
"parameters": {
  "tshirtSize": {
    "type": "string",
    "defaultValue": "Small",
    "allowedValues": [
      "Small",
      "Medium",
      "Large"
    ],
    "metadata": {
      "Description": "T-shirt size of the MongoDB deployment"
    }
  }
}
```

## <a name="pass-state-to-linked-templates"></a>Durum bağlı şablonları geçirin
Bağlı şablonları bağlanırken genellikle statik bir karışımını kullanın ve değişkenleri oluşturulur.

### <a name="static-variables"></a>Statik değişkenler
Statik değişkenler genellikle bir şablon kullanılan URL'leri gibi temel değerlerini sağlamak için kullanılır.

Aşağıdaki şablonu alıntı içinde `templateBaseUrl` Github'da şablonu için kök konumunu belirtir. Sonraki satıra yeni bir değişken oluşturur `sharedTemplateUrl` temel URL paylaşılan kaynakları şablonu bilinen adı ile birleştirir. Bu satır, bir karmaşık nesne değişkeni burada temel URL bilinen yapılandırma şablonu konumuna birleştirilmiş ve depolanan bir ısı boyutu depolamak için kullanılan `vmTemplate` özelliği.

Bu yaklaşımın avantajı, şablon konumu değişirse, yalnızca tüm bağlı şablonlarda geçirir tek bir yerde statik değişkeni değiştirmeniz gerektiğini ' dir.

```json
"variables": {
  "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
  "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
  "tshirtSizeSmall": {
    "vmSize": "Standard_A1",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
    "vmCount": 2,
    "slaveCount": 1,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 1,
      "pool": "db",
      "map": [0,0],
      "jumpbox": 0
    }
  }
}
```

### <a name="generated-variables"></a>Oluşturulan değişkenleri
Statik değişkenler yanı sıra çeşitli değişkenler dinamik olarak oluşturulur. Bu bölümde bazı oluşturulan değişkenlerin ortak türlerini tanımlar.

#### <a name="tshirtsize"></a>tshirtSize
Yukarıdaki örneklerde ile oluşturulan bu değişken hakkında bilgi sahibi.

#### <a name="networksettings"></a>networkSettings
Bir Kapasite yetenek veya uçtan uca kapsamlı çözüm şablonu, bağlı şablonları bir ağdaki genellikle mevcut kaynakları oluşturun. Bir kolay yaklaşım ağ ayarlarını depolamak ve bunlara bağlı şablonları geçirmek için karmaşık bir nesne kullanmaktır.

Ağ Ayarları iletişim kurulurken bir örnek aşağıda görülebilir.

```json
"networkSettings": {
  "vnetName": "[parameters('virtualNetworkName')]",
  "addressPrefix": "10.0.0.0/16",
  "subnets": {
    "dmz": {
      "name": "dmz",
      "prefix": "10.0.0.0/24",
      "vnet": "[parameters('virtualNetworkName')]"
    },
    "data": {
      "name": "data",
      "prefix": "10.0.1.0/24",
      "vnet": "[parameters('virtualNetworkName')]"
    }
  }
}
```

#### <a name="availabilitysettings"></a>availabilitySettings
Bağlantılı şablonlarında oluşturulan kaynakları, genellikle bir kullanılabilirlik kümesine yerleştirilir. Aşağıdaki örnekte, kullanılabilirlik kümesi adı belirtildi ve ayrıca kullanmak için hata etki alanı ve güncelleştirme etki alanı sayısı.

```json
"availabilitySetSettings": {
  "name": "pgsqlAvailabilitySet",
  "fdCount": 3,
  "udCount": 5
}
```

Önek olarak bir ad kullanabilirsiniz birden çok kullanılabilirlik kümesine (örneğin, bir ana düğüm için) ve veri düğümlerini için başka bir gereksinim duyarsanız, birden çok kullanılabilirlik kümesine belirtin veya belirli bir ısı boyutu için bir değişken oluşturmak için daha önce gösterilen modelini izler.

#### <a name="storagesettings"></a>storageSettings
Depolama ayrıntıları genellikle bağlı şablonları ile paylaşılır. Aşağıdaki örnekte bir *storageSettings* nesnesi, depolama hesabı ve kapsayıcı adları hakkında ayrıntılar sağlar.

```json
"storageSettings": {
    "vhdStorageAccountName": "[parameters('storageAccountName')]",
    "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
    "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
}
```

#### <a name="ossettings"></a>osSettings
Bağlantılı şablonları ile işletim sistemi ayarlarını çeşitli düğüm türleri için farklı bilinen yapılandırma türleri arasında geçmesi gerekebilir. Karmaşık bir nesne depolamak ve işletim sistemi bilgilerini paylaşmak için kolay bir yoludur ve dağıtımı için birden çok işletim sistemi seçenekleri desteklemeyi kolaylaştırır.

Aşağıdaki örnek, bir nesne için gösterir *osSettings*:

```json
"osSettings": {
  "imageReference": {
    "publisher": "Canonical",
    "offer": "UbuntuServer",
    "sku": "14.04.2-LTS",
    "version": "latest"
  }
}
```

#### <a name="machinesettings"></a>machineSettings
Oluşturulan bir değişken *machineSettings* bir VM oluşturmak için çekirdek değişkenleri bir karışımını içeren karmaşık bir nesne. Değişkenleri, yönetici kullanıcı adı ve parola, VM adları ve bir işletim sistemi görüntüsü başvurusu için bir önek içerir.

```json
"machineSettings": {
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "machineNamePrefix": "mongodb-",
    "osImageReference": {
        "publisher": "[variables('osFamilySpec').imagePublisher]",
        "offer": "[variables('osFamilySpec').imageOffer]",
        "sku": "[variables('osFamilySpec').imageSKU]",
        "version": "latest"
    }
},
```

Unutmayın *osImageReference* değerleri alır *osSettings* ana şablonda tanımlanan değişken. Anlamı kolayca değiştirebilirsiniz işletim sistemi için bir VM — tamamen veya bir şablon tüketici tercihine göre.

#### <a name="vmscripts"></a>vmScripts
*VmScripts* nesne indirmek ve dış ve iç başvuruları dahil olmak üzere bir VM örneğinde yürütmek için komut dosyaları hakkında ayrıntılar içerir. Dışında altyapı başvurular içerir.
İç başvurular yüklü yazılımı yüklüyse ve yapılandırmayı içerir.

Kullandığınız *scriptsToDownload* VM'ye karşıdan yüklemek için komut dosyaları listelemek için özellik. Bu nesne, aynı zamanda farklı türde eylemler için komut satırı bağımsız değişkenleri başvurular içeriyor. Bu Eylemler, tek tek her düğüm için varsayılan yükleme, tüm düğümler dağıtıldıktan sonra çalıştırılan bir yükleme ve verilen bir şablon için belirli herhangi bir ek betiği yürütülürken içerir.

Bu, yüksek kullanılabilirlik sağlamak bir arbiter gerektirir MongoDB dağıtmak için kullanılan bir şablondan örneğidir. *ArbiterNodeInstallCommand* eklendi *vmScripts* arbiter yüklemek için.

Uygun değerlerle betik yürütmek için belirli bir metni tanımlamak değişkenleri nerede değişkenleri bölümüdür.

```json
"vmScripts": {
    "scriptsToDownload": [
        "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
        "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
    ],
    "regularNodeInstallCommand": "[variables('installCommand')]",
    "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
    "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
},
```

## <a name="return-state-from-a-template"></a>Bir şablondan dönüş durumu
Yalnızca veri geçirebilirsiniz bir şablona geri çağırma şablon verileri de paylaşabilir. İçinde **çıkarır** bölüm bağlantılı şablonu, kaynak şablonu tarafından kullanılabilecek anahtar/değer çiftleri sağlayabilir.

Aşağıdaki örnek, bağlantılı bir şablonda oluşturulan özel IP adresi geçirmek gösterilmiştir.

```json
"outputs": {
    "masterip": {
        "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
        "type": "string"
     }
}
```

Ana Şablon içerisinde, aşağıdaki sözdizimi ile bu verileri kullanabilirsiniz:

    "[reference('master-node').outputs.masterip.value]"

Bu ifadede çıkışları bölüm veya ana şablon kaynakları bölümünü kullanabilirsiniz. Çalışma zamanı durumunu kullandığından deyim değişkenler bölümünde kullanamazsınız. Bu değer ana şablondan döndürmek için kullanın:

```json
"outputs": {
  "masterIpAddress": {
    "value": "[reference('master-node').outputs.masterip.value]",
    "type": "string"
  }
```

Bir sanal makine veri diski döndürmek için bağlantılı bir şablon çıktıları bölümünü kullanarak bir örnek için bkz: [bir sanal makine için birden fazla veri diski oluşturma](resource-group-create-multiple.md).

## <a name="define-authentication-settings-for-virtual-machine"></a>Sanal makine için kimlik doğrulama ayarlarını tanımlayın
Bir sanal makine için kimlik doğrulama ayarlarını belirtmek için yapılandırma ayarları daha önce gösterilen aynı yöntemi kullanabilirsiniz. Parametre geçirme için kimlik doğrulama türünü oluşturun.

```json
"parameters": {
  "authenticationType": {
    "allowedValues": [
      "password",
      "sshPublicKey"
    ],
    "defaultValue": "password",
    "metadata": {
      "description": "Authentication type"
    },
    "type": "string"
  }
}
```

Farklı kimlik doğrulama türleri için değişkenleri ekleyin ve hangi tür depolamak için bir değişken parametre değeri temel alınarak bu dağıtım için kullanılır.

```json
"variables": {
  "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
  "osProfilepassword": {
    "adminPassword": "[parameters('adminPassword')]",
    "adminUsername": "notused",
    "computerName": "[parameters('vmName')]",
    "customData": "[base64(variables('customData'))]"
  },
  "osProfilesshPublicKey": {
    "adminUsername": "notused",
    "computerName": "[parameters('vmName')]",
    "customData": "[base64(variables('customData'))]",
    "linuxConfiguration": {
      "disablePasswordAuthentication": "true",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "[parameters('sshPublicKey')]",
            "path": "/home/notused/.ssh/authorized_keys"
          }
        ]
      }
    }
  }
}
```

Sanal makine tanımlarken, ayarladığınız **osProfile** oluşturduğunuz değişkene.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  ...
  "osProfile": "[variables('osProfile')]"
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Şablon bölümleri hakkında bilgi edinmek için [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md)
* Bir şablonu içinde kullanılabilen işlevlerin görmek için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md)
