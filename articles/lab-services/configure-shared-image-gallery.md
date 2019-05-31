---
title: Azure DevTest Labs'de bir paylaşılan görüntü Galerisi yapılandırma | Microsoft Docs
description: Azure DevTest Labs'de bir paylaşılan görüntü Galerisi yapılandırma hakkında bilgi edinin
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2019
ms.author: spelluru
ms.openlocfilehash: 51b394043f88789865edea5be6376ae536f88848
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420446"
---
# <a name="configure-a-shared-image-gallery-in-azure-devtest-labs"></a>Azure DevTest Labs'de bir paylaşılan görüntü Galerisi yapılandırın
DevTest Labs destekler [paylaşılan görüntü Galerisi](/virtual-machines/windows/shared-image-galleries.md) özelliği. Bu Laboratuvar kaynaklarını oluşturulurken, paylaşılan bir konumdan resimlere erişmek Laboratuvar kullanıcıları sağlar. Ayrıca, yapı ve yönetilen özel VM görüntülerinizi etrafında kuruluş oluşturmanıza yardımcı olur. Paylaşılan görüntü Galerisi özellik şunları destekler:

- Yönetilen küresel çoğaltma görüntüleri
- Sürüm oluşturma ve daha kolay yönetim için görüntülerin gruplandırma
- Görüntülerinizin bölgesel olarak yedekli depolama (ZRS) hesapları ile yüksek kullanılabilirlik alanlarını destekleyen bölgelerde olun. ZRS, bölgesel hatalarına karşı daha iyi esneklik sunar.
- Abonelikler arasında ve hatta rol tabanlı erişim denetimi (RBAC) kullanarak kiracılar arasında paylaştırma.

Daha fazla bilgi için [paylaşılan görüntü Galerisi belgeleri](../virtual-machines/windows/shared-image-galleries.md). 
 
Çok sayıda sürdürmeniz gerekir ve şirket içinde kullanılabilir hale getirmek istediğiniz yönetilen görüntüler varsa, paylaşılan görüntü Galerisi, güncelleştirme ve kendi görüntülerinizi paylaşmak kolay bir deposu olarak kullanabilirsiniz. Laboratuvar sahibi olarak, laboratuvarınız için var olan bir paylaşılan görüntü Galerisine ekleyebilirsiniz. Bu galeri bağlandıktan sonra Laboratuvar kullanıcıları bu son görüntülerden makineler oluşturabilirsiniz. Bu özelliğin önemli bir avantajı, DevTest Labs Laboratuvar arasında abonelikler arasında ve bölgeler arasında görüntülerini paylaşarak avantajı artık sürebilir ' dir. 

## <a name="considerations"></a>Dikkat edilmesi gerekenler
- Bir laboratuvara aynı anda yalnızca bir paylaşılan görüntü Galerisi ekleyebilirsiniz. Başka bir galeri eklemek istiyorsanız, var olan bir ayırma ve başka bir ekleme gerekir. 
- DevTest Labs Laboratuvar Galerisi'nde karşıya yükleme görüntüleri şu anda desteklemiyor. 
- Paylaşılan görüntü galeri görüntüsü kullanarak bir sanal makine oluştururken, DevTest Labs her zaman bu resmin yayımlanan en son sürümünü kullanır.
- DevTest Labs paylaşılan görüntü Galerisi görüntülerini Laboratuvar bulunduğu bölgeye çoğaltır emin olmak için bir en iyi girişim otomatik olarak yapar ancak her zaman mümkün değildir. Kullanıcıların bu görüntülerden VM oluşturma sırasında karşılaşılan sorunları önlemek için görüntü zaten Laboratuvar bölgeye çoğaltılır emin olun."

## <a name="use-azure-portal"></a>Azure portalı kullanma
1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Seçin **tüm hizmetleri** sol gezinti menüsünde.
1. Listeden **DevTest Labs**'i seçin.
1. Labs listesinden seçin, **Laboratuvar**.
1. Seçin **yapılandırması ve ilkelerini** içinde **ayarları** sol menüde bölümü.
1. Seçin **paylaşılan resim galerileri** altında **sanal makine tabanları** sol menüsünde.
1. Tıklayarak, laboratuvarınız için var olan bir paylaşılan görüntü Galerisine ekleme **iliştirme** düğmesi ve açılır menüde, Galeri seçme.
1. Ekli Galerisi'ne gidip Galerinize yapılandırma **etkinleştirmek veya devre dışı** paylaşılan VM oluşturmak için görüntüler.
1. Laboratuvar kullanıcıları ardından tıklayarak etkin görüntüleri kullanarak bir sanal makine oluşturma **+ Ekle** ve görüntüyü bulma **tabanınızı seçin** sayfası.

## <a name="use-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanma

### <a name="attach-a-shared-image-gallery-to-your-lab"></a>Laboratuvarınız için paylaşılan görüntü Galerisine ekleme
Laboratuvarınız için paylaşılan görüntü Galerisi eklemek için bir Azure Resource Manager şablonu kullanıyorsanız, Resource Manager şablonunuzu kaynaklar bölümü altında eklemek aşağıdaki örnekte gösterildiği gibi gerekir:

```json
"resources": [
{
    "apiVersion": "2018-10-15-preview",
    "type": "Microsoft.DevTestLab/labs",
    "name": "[parameters('newLabName')]",
    "location": "[resourceGroup (). location]",
    "resources": [
    {
        "apiVersion": "2018-10-15-preview",
        "name": "[variables('labVirtualNetworkName')]",
        "type": "virtualNetworks",
        "dependsOn": [
            "[resourceId('Microsoft.DevTestLab/labs', parameters('newLabName'))]"
        ]
    },
    {
        "apiVersion":"2018-10-15-preview",
        "name":"myGallery",
        "type":"sharedGalleries",
        "properties": {
            "galleryId":"[parameters('existingSharedGalleryId')]",
            "allowAllImages": "Enabled"
        },
        "dependsOn":[
            "[resourceId('Microsoft.DevTestLab/labs', parameters('newLabName'))]"
        ]
    }
    ]
} 

```

Bu Resource Manager şablonu örnekleri genel GitHub depomuzda bulunan bir tam Resource Manager şablonu örneği için bkz: [Bir laboratuvar oluşturulurken bir paylaşılan görüntü Galerisi yapılandırma](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-lab-shared-gallery-configured).

### <a name="create-a-vm-using-an-image-from-the-shared-image-gallery"></a>Paylaşılan görüntü galerisinden bir görüntü kullanarak VM oluşturma
Paylaşılan görüntü galeri görüntüsü kullanarak bir sanal makine oluşturmak için bir Azure Resource Manager şablonu kullanıyorsanız, aşağıdaki örneği kullanın:

```json

"resources": [
{
    "apiVersion": "2018-10-15-preview",
    "type": "Microsoft.DevTestLab/labs/virtualMachines",
    "name": "[variables('resourceName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "sharedImageId": "[parameters('existingSharedImageId')]",
        "size": "[parameters('newVMSize')]",
        "isAuthenticationWithSshKey": false,
        "userName": "[parameters('userName')]",
        "sshKey": "",
        "password": "[parameters('password')]",
        "labVirtualNetworkId": "[variables('labVirtualNetworkId')]",
        "labSubnetName": "[variables('labSubnetName')]"
    }
}
],

```

Daha fazla bilgi için bu Resource Manager şablonu bizim genel GitHub üzerindeki örneklere bakın.
[Paylaşılan görüntü galeri görüntüsü kullanarak bir sanal makine oluşturma](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-vm-username-pwd-sharedimage).

## <a name="use-api"></a>API kullanma

- API sürümü 2018-10-15-preview'ı kullanın.
- Galeriniz iliştirmek için aşağıdaki kod parçacığında gösterildiği gibi istek gönder:
    
    ``` 
    PUT [Lab Resource Id]/SharedGalleries/[newGalleryName]
    Body: 
    {
        “properties”:{
            “galleryId”: “[Shared Image Gallery resource Id]”,
            “allowAllImages”:”Enabled”
        }
    }
    ```
- Tüm görüntüleri, paylaşılan görüntü galerisinde görüntülemek için kaynak kimlikleri yanı sıra tüm paylaşılan görüntüleri listeleyebilirsiniz.

    ```
    GET [Lab Resource Id]/SharedGalleries/mySharedGallery/SharedImages
    ````
- Paylaşılan görüntüleri kullanarak bir VM oluşturmak için sanal makinelere ve sanal makine özelliklerinde PUT yapın, önceki çağrıdan aldığınız paylaşılan görüntülerini kimliği geçirin. Özellikleri. SharedImageId


## <a name="next-steps"></a>Sonraki adımlar
Yapıları üzerine aşağıdaki makalelere bakın:

- [Laboratuvarınız için zorunlu yapıtları belirtin](devtest-lab-mandatory-artifacts.md)
- [Özel yapıtlar oluşturma](devtest-lab-artifact-author.md)
- [Bir laboratuvara yapıt deposu ekleme](devtest-lab-artifact-author.md)
- [Yapıt hatalarını tanılama](devtest-lab-troubleshoot-artifact-failure.md)
