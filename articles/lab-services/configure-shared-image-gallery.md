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
ms.openlocfilehash: de857498aeb51c9b3711c90338d983e85b61cb70
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065427"
---
# <a name="configure-a-shared-image-gallery-in-azure-devtest-labs"></a>Azure DevTest Labs’de paylaşılan resim galerisi yapılandırma
DevTest Labs destekler [paylaşılan görüntü Galerisi](../virtual-machines/windows/shared-image-galleries.md) özelliği. Laboratuvar kullanıcılarının, laboratuvar kaynakları oluştururken paylaşılan bir konumdan görüntülere erişmesine imkan tanıyor. Ayrıca, özel olarak yönetilen sanal makine görüntülerinizle birlikte yapı ve organizasyon oluşturmanıza yardımcı oluyor. Paylaşılan görüntü Galerisi özellik şunları destekler:

- Görüntülerin yönetilen bir şekilde global olarak yinelenmesi
- Daha kolay yönetim için görüntülerin sürümlerinin oluşturulması ve gruplanması
- Alanlar Arası Yedekli Depolama (ZRS) hesapları ile kullanılabilirlik alanlarını destekleyen bölgelerde görüntülerinizin yüksek oranda kullanılabilmesini sağlayın. ZRS, bölgesel arızalara karşı daha iyi koruma sağlar.
- Rol tabanlı erişim denetimini (RBAC) kullanarak abonelikler ve hatta kiracılar arasında paylaşım.

Daha fazla bilgi için [paylaşılan görüntü Galerisi belgeleri](../virtual-machines/windows/shared-image-galleries.md). 
 
Muhafaza etmek istediğiniz çok sayıda yönetilen görüntünüz varsa ve bunlara şirketiniz genelinde erişilebilmesini istiyorsanız paylaşılan görüntü galerisini, görüntülerinizi güncelleştirmeyi ve paylaşmayı kolaylaştıran bir depo olarak kullanabilirsiniz. Laboratuvar sahibi olarak var olan bir paylaşılan görüntü galerisini laboratuvarınıza ekleyebilirsiniz. Bu galeri eklendiğinde laboratuvar kullanıcıları yeni görüntülerden makine oluşturabilir. Bu özelliğin temel avantajlarından biri, artık DevTest Labs’in laboratuvarlar, abonelikler ve bölgeler genelinde görüntü paylaşma olanağından yararlanabilmesidir. 

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

    ![Paylaşılan resim galerileri menüsü](./media/configure-shared-image-gallery/shared-image-galleries-menu.png)
1. Tıklayarak, laboratuvarınız için var olan bir paylaşılan görüntü Galerisine ekleme **iliştirme** düğmesi ve açılır menüde, Galeri seçme.

    ![İliştir](./media/configure-shared-image-gallery/attach-options.png)
1. Ekli Galerisi'ne gidip Galerinize yapılandırma **etkinleştirmek veya devre dışı** paylaşılan VM oluşturmak için görüntüler.

    ![Etkinleştirme veya devre dışı bırak](./media/configure-shared-image-gallery/enable-disable.png)
1. Laboratuvar kullanıcıları ardından tıklayarak etkin görüntüleri kullanarak bir sanal makine oluşturma **+ Ekle** ve görüntüyü bulma **tabanınızı seçin** sayfası.

    ![Laboratuvar kullanıcıları](./media/configure-shared-image-gallery/lab-users.png)
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
