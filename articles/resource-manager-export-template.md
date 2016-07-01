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
    ms.date="05/10/2016"
    ms.author="tomfitz"/>

# Mevcut kaynaklardan Azure Resource Manager şablonunu dışarı aktarma

Azure Resource Manager’ı nasıl oluşturacağınızı anlamak göz korkutucu olabilir. Neyse ki, aboneliğinizdeki mevcut kaynaklardan bir şablonu dışarı aktarabildiğiniz için, Resource Manager size bu görevde yardımcı olur. Bu oluşturulan şablonu şablon söz dizimi hakkında bilgi edinmek veya çözümünüzün yeniden dağıtımını gerektiği gibi otomatikleştirmek için kullanabilirsiniz.

Bu öğreticide, Azure portalında oturum açacak, bir depolama hesabı oluşturacak ve bu depolama hesabı için şablonu dışarı aktaracaksınız. Kaynak grubunu değiştirmek için sanal ağ ekleyeceksiniz. Son olarak, geçerli durumunu temsil eden yeni bir şablonu dışarı aktaracaksınız. Bu makale basitleştirilmiş bir altyapıya odaklanıyor olsa da, daha karmaşık bir çözüm için şablonu dışarı aktarmak üzere bu aynı adımları kullanabilirsiniz.

## Depolama hesabı oluşturma

1. [Azure portalda](https://portal.azure.com), **Yeni** > **Veri + Depolama** > **Depolama hesabı**’nı seçin.

      ![depolama oluşturma](./media/resource-manager-export-template/create-storage.png)

2. **storage** adlı, adınızın baş harflerini ve tarihi içeren bir depolama hesabı oluşturun. Depolama hesabı adının Azure’da benzersiz olması gerekir. Başlangıçta zaten kullanımda olan bir adı kullanmaya çalışırsanız, adı değiştirmeyi deneyin. Kaynak grubu için **ExportGroup** kullanın. Diğer özellikler için varsayılan değerleri kullanabilirsiniz. **Oluştur**’u seçin.

      ![depolama için değerler sağlama](./media/resource-manager-export-template/provide-storage-values.png)

Dağıtım tamamlandıktan sonra, aboneliğiniz depolama hesabını içerir.

## Dağıtım için şablonu dışarı aktarma

1. Yeni kaynak grubunuz için kaynak grubu dikey penceresine gidin. Son dağıtım sonucunun listelendiğini göreceksiniz. Bu bağlantıyı seçin.

      ![kaynak grubu dikey penceresi](./media/resource-manager-export-template/resource-group-blade.png)

2. Grup için dağıtım geçmişini görürsünüz. Sizin durumunuzda, büyük olasılıkla yalnızca bir dağıtım listeleniyordur. Bu dağıtımı seçin.

     ![son dağıtım](./media/resource-manager-export-template/last-deployment.png)

3. Dağıtımın bir özeti gösterilir. Özet, dağıtımın ve işlemlerinin durumunu ve sağladığınız parametreler için değerleri içerir. Dağıtım için kullanılan şablonu görmek isterseniz **Şablonu görüntüle**’yi seçin.

     ![dağıtım özetini görüntüleme](./media/resource-manager-export-template/deployment-summary.png)

4. Resource Manager sizin için aşağıdaki beş dosyayı alır:

   - Çözümünüz için altyapıyı tanımlayan şablon. Portal üzerinden depolama hesabı oluşturduğunuzda, Resource Manager bunu dağıtmak için bir şablon kullandı ve bu şablonu gelecekte başvurmak üzere kaydetti.

   - Dağıtım sırasında değerleri geçirmek için kullanabileceğiniz bir parametre dosyası. Bu dosya, ilk dağıtım sırasında sağladığınız değerleri içerir, ancak şablonu yeniden dağıtırken bu değerleri değiştirebilirsiniz.

   - Şablonu dağıtmak için kullanabileceğiniz bir Azure PowerShell betiği.

   - Şablonu dağıtmak için kullanabileceğiniz bir Azure komut satırı arabirimi (CLI) betik dosyası.

   - Şablonu dağıtmak için kullanabileceğiniz bir .NET sınıfı.

     Dosyalara dikey pencerelerdeki bağlantılar aracılığıyla ulaşılabilir. Varsayılan olarak şablon seçilidir.

       ![şablonu görüntüleme](./media/resource-manager-export-template/view-template.png)

     Şimdi şablona dikkat edin. Şablonunuz şuna benzemelidir:

        {      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",      "contentVersion": "1.0.0.0",      "parameters": {        "name": {          "type": "String"        },        "accountType": {          "type": "String"        },        "location": {          "type": "String"        },        "encryptionEnabled": {          "defaultValue": false,          "type": "Bool"        }      },      "resources": [        {          "type": "Microsoft.Storage/storageAccounts",          "sku": {            "name": "[parameters('accountType')]"          },          "kind": "Storage",          "name": "[parameters('name')]",          "apiVersion": "2016-01-01",          "location": "[parameters('location')]",          "properties": {            "encryption": {              "services": {                "blob": {                  "enabled": "[parameters('encryptionEnabled')]"                }              },              "keySource": "Microsoft.Storage"            }          }        }      ]    }

     Şablonun depolama hesabı adı, türü ve konumu için parametreleri tanımladığına dikkat edin. Bir parametre şifrelemenin etkin olup olmadığını ve varsayılan değerin **false** olup olmadığını da gösterir. **resources** bölümünde, dağıtılacak depolama hesabının tanımını görürsünüz.

Köşeli ayraçlar, dağıtım sırasında değerlendirilen bir ifade içerir. Şablondaki ayraçlı ifadeler dağıtım sırasında parametre değerlerini almak için kullanılır. Çok daha fazla ifade kullanabilir ve bu makalenin sonraki bölümlerinde, diğer ifadelerin örneklerini görebilirsiniz. Tam liste için bkz. [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).

Bir şablonun yapısı hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).

## Sanal ağ ekleme

Önceki bölümde indirdiğiniz şablon bu özgün dağıtım için altyapıyı temsil etti, ancak bu dağıtım sonrasında yaptığınız değişiklikleri hesaba katmayacak.
Bu sorunu anlamak için portal aracılığıyla bir sanal ağ ekleyerek kaynak grubunu değiştirelim.

1. Kaynak grubu dikey penceresinde, **Ekle**’yi seçin ve kullanılabilir kaynaklardan **sanal ağ**’ı seçin.

2. Sanal ağınızı **VNET** olarak adlandırın ve diğer özellikler için varsayılan değerleri kullanın. **Oluştur**’u seçin.

      ![uyarı ayarlama](./media/resource-manager-export-template/create-vnet.png)

3. Sanal ağ kaynak grubunuza başarıyla dağıtıldıktan sonra dağıtım geçmişinize tekrar bakın. Şimdi iki dağıtım göreceksiniz. Daha yeni olan dağıtımı seçin.

      ![dağıtım geçmişi](./media/resource-manager-export-template/deployment-history.png)

4. Bu dağıtım için şablona bakın. Bunun, yalnızca sanal ağı eklemek için yaptığınız değişiklikleri tanımladığına dikkat edin.

Dağıtılacak birçok farklı şablonu hatırlamak yerine, çözümünüz için tüm altyapıyı tek bir işleme dağıtan şablonla çalışmak genellikle en iyi uygulamadır.


## Kaynak grubu için şablonu dışarı aktarma

Her bir dağıtım yalnızca kaynak grubunuzda yapmış olduğunuz değişiklikleri gösterir, ancak herhangi bir zamanda, tüm kaynak grubunuzun özniteliklerini göstermek için bir şablonu dışarı aktarabilirsiniz.  

1. Bir kaynak grubu için şablonu görüntülemek üzere **Şablonu dışarı aktar**’ı seçin.

      ![kaynak grubunu dışarı aktarma](./media/resource-manager-export-template/export-resource-group.png)

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

     Resource Manager, dağıtım sırasında kullanılan şablonları almadı. Bunun yerine, kaynakların geçerli yapılandırmasını temel alan yeni bir şablon oluşturdu. Resource Manager parametre olarak geçirmek istediğiniz değerleri bilmediğinden, kaynak grubundaki değerleri temel alan çoğu değeri sabit koda dönüştürür. Örneğin, depolama hesabı konumu ve çoğaltma değeri aşağıdaki şekilde ayarlanır:

        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },

3. Üzerinde yerel olarak çalışabilmek için şablonu indirin.

      ![şablonu indirme](./media/resource-manager-export-template/download-template.png)

4. Yüklediğiniz .zip dosyasını bulun ve içeriğini çıkarın. İndirilen bu şablonu kullanarak altyapınızı yeniden dağıtabilirsiniz.

## Sonraki adımlar

Tebrikler! Portalda oluşturduğunuz kaynaklardan bir şablonu dışarı aktarmayı öğrendiniz.

- Bu öğreticinin ikinci bölümünde, daha fazla parametre ekleyerek indirdiğiniz yeni şablonu özelleştirecek ve bir betik ile yeniden dağıtacaksınız. Bkz. [Dışarı aktarılan bir şablonu özelleştirme ve yeniden dağıtma](resource-manager-customize-template.md).
- Bir şablonu PowerShell aracılığıyla nasıl dışarı aktaracağınızı görmek için bkz. [Azure Resource Manager ile Azure PowerShell’i Kullanma](powershell-azure-resource-manager.md).
- Bir şablonu Azure CLI aracılığıyla nasıl dışarı aktaracağınızı görmek için bkz. [Azure Resource Manager ile Mac, Linux ve Windows için Azure CLI’yi Kullanma](xplat-cli-azure-resource-manager.md).



<!--HONumber=Jun16_HO2-->


