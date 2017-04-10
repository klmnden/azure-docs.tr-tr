---
title: "Azure Resource Manager şablonunu dışarı aktarma | Microsoft Belgeleri"
description: "Mevcut bir kaynak grubundaki şablonu dışarı aktarmak için Azure Resource Manager’ı kullanın."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 5f5ca940-eef8-4125-b6a0-f44ba04ab5ab
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/30/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: f41fbee742daf2107b57caa528e53537018c88c6
ms.openlocfilehash: cee4748a0b24e11cd8a8ee46471418680fcf7b33
ms.lasthandoff: 03/31/2017


---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Mevcut kaynaklardan Azure Resource Manager şablonunu dışarı aktarma
Resource Manager, aboneliğinizde var olan kaynaklardan bir Resource Manager şablonunu dışarı aktarmanızı sağlar. Bu oluşturulan şablonu şablon söz dizimi hakkında bilgi edinmek veya çözümünüzün yeniden dağıtımını gerektiği gibi otomatikleştirmek için kullanabilirsiniz.

Bir şablonu dışarı aktarmak için iki farklı yol vardır:

* Dağıtım için kullandığınız gerçek şablonu dışarı aktarabilirsiniz. Dışarı aktarılan şablonda, tüm parametreler ve değişkenler özgün şablondaki gibidir. Bu yaklaşım, kaynaklarınızı portal üzerinden dağıttığınızda yararlıdır. Şimdi, bu kaynakları oluşturmak için şablon yapısını nasıl düzenleyeceğinizden bahsedelim.
* Kaynak grubunun geçerli durumunu temsil eden bir şablonu dışarı aktarabilirsiniz. Dışarı aktarılan şablon, dağıtım için kullandığınız herhangi bir şablonu temel almaz. Bunun yerine, kaynak grubunun anlık görüntüsü olan bir şablon oluşturur. Dışarı aktarılan şablon birçok sabit kodlu değer ve büyük olasılıkla normalde tanımlayacağınızdan daha az sayıda parametre içerir. Bu yaklaşım, kaynak grubunu portal ya da betikler aracılığıyla değiştirdiğinizde yararlı olur. Şimdi kaynak grubunu bir şablon olarak yakalamalısınız.

Bu konuda, iki yaklaşım ortaya koyulmaktadır.

Bu öğreticide, Azure Portal’da oturum açar, bir depolama hesabı oluşturur ve bu depolama hesabı için şablonu dışarı aktarırsınız. Kaynak grubunu değiştirmek için sanal ağ eklersiniz. Son olarak, geçerli durumunu temsil eden yeni bir şablonu dışarı aktarırsınız. Bu makale basitleştirilmiş bir altyapıya odaklanıyor olsa da, daha karmaşık bir çözüm için şablonu dışarı aktarmak üzere bu aynı adımları kullanabilirsiniz.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
1. [Azure portal](https://portal.azure.com)’da, **Yeni** > **Depolama** > **Depolama hesabı**’nı seçin.
   
      ![depolama oluşturma](./media/resource-manager-export-template/create-storage.png)
2. **storage** adlı, adınızın baş harflerini ve tarihi içeren bir depolama hesabı oluşturun. Depolama hesabı adının Azure’da benzersiz olması gerekir. Ad zaten kullanılıyorsa adın kullanıldığını belirten bir hata iletisi görürsünüz. Adın bir varyasyonunu deneyin. Kaynak grubu için, **Yeni oluştur**’u seçin ve **ExportGroup** olarak adlandırın. Diğer özellikler için varsayılan değerleri kullanabilirsiniz. **Oluştur**’u seçin.
   
      ![depolama için değerler sağlama](./media/resource-manager-export-template/provide-storage-values.png)

Dağıtım birkaç dakika sürebilir. Dağıtım tamamlandıktan sonra, aboneliğiniz depolama hesabını içerir.

## <a name="view-a-template-from-deployment-history"></a>Dağıtım geçmişinden bir şablonu görüntüleme
1. Yeni kaynak grubunuz için kaynak grubu dikey penceresine gidin. Son dağıtım sonucunun listelendiğini görürsünüz. Bu bağlantıyı seçin.
   
      ![kaynak grubu dikey penceresi](./media/resource-manager-export-template/resource-group-blade.png)
2. Grup için dağıtım geçmişini görürsünüz. Sizin durumunuzda, dikey pencerede büyük olasılıkla yalnızca bir dağıtım listelenir. Bu dağıtımı seçin.
   
     ![son dağıtım](./media/resource-manager-export-template/last-deployment.png)
3. Dikey pencerede, dağıtımın bir özeti görüntülenir. Özet, dağıtımın ve işlemlerinin durumunu ve sağladığınız parametreler için değerleri içerir. Dağıtım için kullandığınız şablonu görmek için **Şablonu görüntüle**’yi seçin.
   
     ![dağıtım özetini görüntüleme](./media/resource-manager-export-template/deployment-summary.png)
4. Resource Manager sizin için aşağıdaki yedi dosyayı alır:
   
   1. **Şablon** - Çözümünüze ait altyapıyı tanımlayan şablon. Portal üzerinden depolama hesabı oluşturduğunuzda, Resource Manager bunu dağıtmak için bir şablon kullandı ve bu şablonu gelecekte başvurmak üzere kaydetti.
   2. **Parametreler**: Dağıtım sırasında değerleri geçirmek için kullanabileceğiniz bir parametre dosyası. Bu dosya, ilk dağıtım sırasında sağladığınız değerleri içerir, ancak şablonu yeniden dağıtırken bu değerleri değiştirebilirsiniz.
   3. **CLI**: Şablonu dağıtmak için kullanabileceğiniz bir Azure komut satırı arabirimi (CLI) betik dosyası.
   3. **CLI 2.0** - Şablonu dağıtmak için kullanabileceğiniz bir Azure komut satırı arabirimi (CLI) betik dosyası.
   4. **PowerShell**: Şablonu dağıtmak için kullanabileceğiniz bir Azure PowerShell betiği.
   5. **.NET**: Şablonu dağıtmak için kullanabileceğiniz bir .NET sınıfı.
   6. **Ruby** - Şablonu dağıtmak için kullanabileceğiniz bir Ruby sınıfı.
      
      Dosyalara dikey pencerelerdeki bağlantılar aracılığıyla ulaşılabilir. Varsayılan olarak şablon, dikey pencerede görüntülenir.
      
       ![şablonu görüntüleme](./media/resource-manager-export-template/view-template.png)
      
      Şimdi şablona dikkat edin. Şablonunuz şuna benzemelidir:
      
      ```json
      {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "name": {
            "type": "String"
          },
          "accountType": {
            "type": "String"
          },
          "location": {
            "type": "String"
          },
          "encryptionEnabled": {
            "defaultValue": false,
            "type": "Bool"
          }
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "sku": {
              "name": "[parameters('accountType')]"
            },
            "kind": "Storage",
            "name": "[parameters('name')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "properties": {
              "encryption": {
                "services": {
                  "blob": {
                    "enabled": "[parameters('encryptionEnabled')]"
                  }
                },
                "keySource": "Microsoft.Storage"
              }
            }
          }
        ]
      }
      ```

Depolama hesabınızı oluşturmak için kullanılan gerçek şablon budur. Farklı türlerde depolama hesapları dağıtmanızı sağlayan parametreler içerir. Bir şablonun yapısı hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md). Bir şablonda kullanabileceğiniz işlevlerin tam listesi için bkz. [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).

## <a name="add-a-virtual-network"></a>Sanal ağ ekleme
Önceki bölümde indirdiğiniz şablon, bu özgün dağıtımın altyapısını temsil ediyordu. Ancak bu şablon, dağıtımdan sonra yaptığınız değişiklikleri içermez.
Bu sorunu anlamak için portal aracılığıyla bir sanal ağ ekleyerek kaynak grubunu değiştirelim.

1. Kaynak grubu dikey penceresinde **Ekle**’yi seçin.
   
      ![kaynak ekle](./media/resource-manager-export-template/add-resource.png)
2. Kullanılabilir kaynaklardan **Sanal ağ** seçeneğini belirleyin.
   
      ![sanal ağ seçme](./media/resource-manager-export-template/select-vnet.png)
3. Sanal ağınızı **VNET** olarak adlandırın ve diğer özellikler için varsayılan değerleri kullanın. **Oluştur**’u seçin.
   
      ![uyarı ayarlama](./media/resource-manager-export-template/create-vnet.png)
4. Sanal ağ kaynak grubunuza başarıyla dağıtıldıktan sonra dağıtım geçmişinize tekrar bakın. İki dağıtım göreceksiniz. İkinci dağıtımı görmüyorsanız kaynak grubu dikey pencerenizi kapatıp yeniden açmanız gerekebilir. Daha yeni olan dağıtımı seçin.
   
      ![dağıtım geçmişi](./media/resource-manager-export-template/deployment-history.png)
5. Bu dağıtım için şablonu görüntüleyin. Bu şablon, yalnızca sanal ağı tanımlar. Daha önce dağıttığınız depolama hesabını içermez. Artık kaynak grubunuzdaki tüm kaynakları temsil eden bir şablonunuz yoktur.

## <a name="export-the-template-from-resource-group"></a>Şablonu kaynak grubundan dışarı aktarma
Kaynak grubunuzun geçerli durumunu almak için kaynak grubunun anlık görüntüsünü gösteren bir şablonu dışarı aktarın.  

> [!NOTE]
> 200’den fazla kaynağı olan bir kaynak grubu için bir şablonu dışarı aktaramazsınız.
> 
> 

1. Bir kaynak grubu için şablonu görüntülemek üzere **Otomasyon betiği**’ni seçin.
   
      ![kaynak grubunu dışarı aktarma](./media/resource-manager-export-template/export-resource-group.png)
   
     Şablonu dışarı aktarma işlevini tüm kaynak türleri desteklemez. Kaynak grubunuz yalnızca bu makalede gösterilen depolama hesabı ve sanal ağı içeriyorsa bir hata görmezsiniz. Ancak, diğer kaynak türlerini oluşturduysanız dışarı aktarma ile ilgili bir sorun olduğunu bildiren bir hata görebilirsiniz. Bu sorunların nasıl ele alınacağını [Dışarı aktarma sorunlarını düzeltme](#fix-export-issues) bölümünden öğrenebilirsiniz.
2. Çözümü yeniden dağıtmak için kullanabileceğiniz altı dosyayı yeniden görürsünüz, ancak bu kez şablon biraz farklıdır. Bu şablon yalnızca iki parametreye sahiptir: depolama hesabı adı için bir tane ve sanal ağ adı için bir tane.

   ```json
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
   ```
   
   Resource Manager, dağıtım sırasında kullandığınız şablonları almadı. Bunun yerine, kaynakların geçerli yapılandırmasını temel alan yeni bir şablon oluşturdu. Örneğin şablon, depolama hesabı konumu ve çoğaltma değerini aşağıdaki şekilde ayarlar:

   ```json 
   "location": "northeurope",
   "tags": {},
   "properties": {
     "accountType": "Standard_RAGRS"
   },
   ```
3. Bu şablonla çalışmaya devam etmek için kullanabileceğiniz iki seçenek vardır. Şablonu indirebilir ve JSON düzenleyicisiyle üzerinde yerel olarak çalışabilirsiniz. Alternatif olarak, şablonu kitaplığınıza kaydedip portal aracılığıyla üzerinde çalışabilirsiniz.
   
     [VS Code](resource-manager-vs-code.md) veya [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) gibi bir JSON düzenleyicisini kullanabiliyorsanız, şablonu yerel olarak indirip bu düzenleyiciyi kullanmayı seçebilirsiniz. JSON düzenleyicisi kullanmıyorsanız şablonu portal aracılığıyla düzenlemeyi tercih edebilirsiniz. Bu konu başlığının geri kalanında, şablonu portalda kitaplığınıza kaydettiğiniz varsayılacaktır. Bununla birlikte, ister JSON düzenleyicisiyle yerel olarak çalışın ister portal aracılığıyla çalışın şablon üzerinde aynı söz dizimi değişikliklerini yaparsınız.
   
     Yerel olarak çalışmak için **İndir**’i seçin.
   
      ![şablonu indirme](./media/resource-manager-export-template/download-template.png)
   
     Portal üzerinden çalışmak için **Kitaplığa ekle**’yi seçin.
   
      ![kitaplığa ekleme](./media/resource-manager-export-template/add-to-library.png)
   
     Bir şablonu kitaplığa eklerken, şablona bir ad verin ve açıklama yazın. Ardından **Kaydet**’i seçin.
   
     ![şablon değerlerini ayarlama](./media/resource-manager-export-template/set-template-values.png)
4. Kitaplığınıza kaydedilen bir şablonu görüntülemek için **Diğer hizmetler**’i seçin, sonuçları filtrelemek için **Şablonlar** yazın ve **Şablonlar**’ı seçin.
   
      ![şablonları bulma](./media/resource-manager-export-template/find-templates.png)
5. Kaydettiğiniz ada sahip şablonu seçin.
   
      ![şablon seçme](./media/resource-manager-export-template/select-library-template.png)

## <a name="customize-the-template"></a>Şablonu özelleştirme
Dışarı aktarılan şablon, her dağıtım için aynı depolama hesabını ve sanal ağı oluşturmak isterseniz düzgün şekilde çalışır. Bununla birlikte Resource Manager, şablonları çok daha fazla esneklikle dağıtabileceğiniz seçenekler sunar. Örneğin, dağıtım sırasında, oluşturulacak depolama hesabı türünü ya da sanal ağ adresi ön eki ve alt ağ ön eki için kullanılacak değerler türünü belirtebilirsiniz.

Bu bölümde, bu kaynakları diğer ortamlara dağıttığınızda şablonu yeniden kullanabilmeniz için, dışarı aktarılan şablona parametreler eklersiniz. Ayrıca, şablonu dağıttığınızda bir hata ile karşılaşma olasılığını azaltmak için şablonunuza bazı özellikler eklersiniz. Artık depolama hesabınız için benzersiz bir ad düşünmeniz gerekmez. Bunun yerine, şablon benzersiz adı kendi oluşturur. Depolama hesabı türü için belirtilebilecek değerleri yalnızca geçerli seçeneklerle kısıtlarsınız.

1. Şablonu özelleştirmek için, **Düzenle**’yi seçin.
   
     ![şablonu gösterme](./media/resource-manager-export-template/show-template.png)
2. Şablonu seçin.
   
     ![şablonu düzenleme](./media/resource-manager-export-template/edit-template.png)
3. Dağıtım sırasında belirtmek isteyebileceğiniz değerleri geçirebilmek için **parameters** bölümünü yeni parametre tanımlarıyla değiştirin. **storageAccount_accountType** için **allowedValues** değerlerini not alın. Yanlışlıkla geçersiz bir değer sağlarsanız, dağıtım başlamadan önce bu hata tanınır. Ayrıca, depolama hesabı adı için yalnızca bir ön ek sağladığınızı ve ön ekin 11 karakterle sınırlı olduğuna dikkat edin. Ön eki 11 karakterle sınırlayarak depolama hesabı tam adının maksimum karakter sayısını aşmayacağından emin olabilirsiniz. Ön ek, depolama hesaplarınıza bir adlandırma kuralı uygulamanızı sağlar. Sonraki adımda benzersiz bir ad oluşturmayı göreceksiniz.

   ```json
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
   ```

4. Şablonunuzdaki **variables** bölümü şu anda boştur. **variables** bölümünde, şablonunuzun geri kalanı için söz dizimini basitleştiren değerler oluşturabilirsiniz. Bu bölümü, yeni bir değişken tanımı ile değiştirin. **storageAccount_name** değişkeni, kaynak grubunun tanımlayıcısına göre oluşturulan benzersiz bir dizeyi parametre ön ekiyle birleştirir. Artık bir parametre değeri sağlarken benzersiz bir ad bulmanıza gerek yoktur.

   ```json
   "variables": {
     "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
   },
   ```

5. Kaynak tanımlarında parametreler ve değişken kullanmak için **resources** bölümünü yeni kaynak tanımlarıyla değiştirin. Kaynak özelliğine atanan değer dışında, kaynak tanımlarında çok az değişiklik gerçekleştiğine dikkat edin. Özellikler, dışarı aktarılan şablondaki özelliklerle aynıdır. Yaptığınız, özellikler sabit kodlanmış değerler yerine parametre değerlerine atamaktır. Kaynakların konumu, **resourceGroup().location** ifadesi aracılığıyla kaynak grubu olarak aynı konumu kullanacak şekilde ayarlanır. Depolama hesabı adı için oluşturduğunuz değişkene **variables** ifadesi aracılığıyla başvurulur.

   ```json
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
   ```

6. Şablonu düzenlemeyi tamamladığınızda **Tamam**’ı seçin.
7. Şablonda yapılan değişiklikleri kaydetmek için **Kaydet**’i seçin.
   
     ![şablonu kaydetme](./media/resource-manager-export-template/save-template.png)
8. Güncelleştirilmiş şablonu dağıtmak için **Dağıt**’ı seçin.
   
     ![şablonu dağıtma](./media/resource-manager-export-template/deploy-template.png)
9. Parametre değerlerini sağlayın ve kaynakların dağıtılacağı yeni bir kaynak grubu seçin.

## <a name="update-the-downloaded-parameters-file"></a>İndirilen parametreler dosyasını güncelleştirme
İndirilen dosyalarla (portal kitaplığı yerine) çalışıyorsanız indirilen parametre dosyasını güncelleştirmeniz gerekir. Parametre dosyası artık şablonunuzdaki parametrelerle eşleşmez. Bir parametre dosyası kullanmak zorunda değilsiniz. Dosya kullanırsanız, bir ortamı yeniden dağıtırken işiniz kolaylaşabilir. Parametrelerin çoğu için şablonda tanımlanan varsayılan değerleri kullandığınızda parametre dosyanız için yalnızca iki değer gerekir.

parameters.json dosyasının içeriğini aşağıdaki kodla değiştirin:

```json
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

## <a name="fix-export-issues"></a>Dışarı aktarma sorunlarını düzeltme
Şablonu dışarı aktarma işlevini tüm kaynak türleri desteklemez. Resource Manager, hassas verilerin açığa çıkarılmasını önlemek için bazı kaynak türlerini özellikle dışarı aktarmaz. Örneğin, site yapılandırmanızda bir bağlantı dizesi varsa dışarı aktarılmış bir şablonda açıkça gösterilmesini büyük olasılıkla istemezsiniz. Bu sorunu çözümlemek için, eksik kaynakları şablonunuza el ile tekrar ekleyin.

> [!NOTE]
> Yalnızca, dağıtım geçmişiniz yerine bir kaynak grubundan dışarı aktarma yaparken dışarı aktarma sorunlarıyla karşılaşırsınız. Son dağıtımınız kaynak grubunun geçerli durumunu doğru şekilde temsil ediyorsa, şablonu kaynak grubu yerine dağıtım geçmişinden dışarı aktarmanız gerekir. Yalnızca tek bir şablonda tanımlanmamış kaynak grubunda değişiklikler yaptığınızda kaynak grubundan dışarı aktarma yapın.
> 
> 

Örneğin, şablonu bir web uygulaması, SQL Veritabanı ve site yapılandırmasında bağlantı dizesi içeren bir kaynak grubu için dışarı aktarırsanız aşağıdaki iletiyi görürsünüz:

![hatayı göster](./media/resource-manager-export-template/show-error.png)

İleti seçildiğinde, tam olarak hangi kaynak türlerinin dışarı aktarılmadığı gösterilir. 

![hatayı göster](./media/resource-manager-export-template/show-error-details.png)

Bu konu başlığında sık kullanılan düzeltmelere yer verilmiştir.

### <a name="connection-string"></a>Bağlantı dizesi
Web siteleri kaynağında veritabanına bağlantı dizesi için bir tanım ekleyin:

```json
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

### <a name="web-site-extension"></a>Web sitesi uzantısı
Web sitesi kaynağında yüklenecek kod için bir tanım ekleyin:

```json
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

### <a name="virtual-machine-extension"></a>Sanal makine uzantısı
Sanal makine uzantılarının örnekleri için bkz. [Azure Windows VM Uzantısı Yapılandırma Örnekleri](../virtual-machines/windows/extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="virtual-network-gateway"></a>Sanal ağ geçidi
Bir sanal ağ geçidi kaynak türü ekleyin.

```json
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

### <a name="local-network-gateway"></a>Yerel ağ geçidi
Bir yerel ağ geçidi kaynak türü ekleyin.

```json
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

### <a name="connection"></a>Bağlantı
Bir bağlantı kaynak türü ekleyin.

```json
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


## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! Portalda oluşturduğunuz kaynaklardan bir şablonu dışarı aktarmayı öğrendiniz.

* Bir şablonu [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md) veya [REST API](resource-group-template-deploy-rest.md) aracılığıyla dağıtabilirsiniz.
* Bir şablonu PowerShell aracılığıyla nasıl dışarı aktaracağınızı görmek için bkz. [Azure Resource Manager ile Azure PowerShell’i Kullanma](powershell-azure-resource-manager.md).
* Bir şablonu Azure CLI aracılığıyla nasıl dışarı aktaracağınızı görmek için bkz. [Azure Resource Manager ile Mac, Linux ve Windows için Azure CLI’yi Kullanma](xplat-cli-azure-resource-manager.md).


