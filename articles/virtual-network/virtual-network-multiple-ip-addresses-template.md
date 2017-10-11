---
title: "Azure sanal makineleri - şablonu için birden çok IP adresi | Microsoft Docs"
description: "Bir Azure Resource Manager şablonu kullanarak bir sanal makine için birden çok IP adresi atama hakkında bilgi edinin."
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/08/2016
ms.author: jdial
ms.openlocfilehash: d4b189fb23dda1167c4f6b17b618c718d32dd98f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak sanal makineleri için birden çok IP adresi atayın

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Bu makalede, Resource Manager şablonu kullanarak Azure Resource Manager dağıtım modeli sanal makine (VM) oluşturma açıklanmaktadır. Birden çok ortak ve özel IP adresleri, bir VM Klasik dağıtım modeli üzerinden dağıtırken aynı NIC'ye atanamaz. Azure dağıtım modelleri hakkında daha fazla bilgi için okuma [dağıtım modellerini anlama](../resource-manager-deployment-model.md) makalesi.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name="template-description"></a>Şablon açıklaması

Bir şablonu dağıtmayı, hızlı ve tutarlı bir şekilde Azure kaynakları ile farklı yapılandırma değerlerini oluşturmanıza olanak sağlar. Okuma [Resource Manager şablonu Kılavuzu](../azure-resource-manager/resource-manager-template-walkthrough.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure Resource Manager şablonları ile bilmiyorsanız makalesi. [Birden çok IP adreslerine sahip bir VM'yi dağıtmak](https://azure.microsoft.com/resources/templates/101-vm-multiple-ipconfig) şablonu bu makalede kullanılan.

<a name="resources"></a>Şablon dağıtma aşağıdaki kaynaklar oluşturur:

|Kaynak|Ad|Açıklama|
|---|---|---|
|Ağ arabirimi|*myNic1*|Bu makalede senaryo bölümünde açıklanan üç IP yapılandırmaları oluşturulur ve bu NIC'ye atanan|
|Ortak IP adresi kaynağı|2 oluşturulur: *myPublicIP* ve *myPublicIP2*|Bu kaynaklar için atanan ve ortak statik IP adresleri atanır *IPConfig-1* ve *IPConfig 2* senaryoda açıklanan IP yapılandırmaları.|
|VM|*myVM1*|Standart DS3 VM.|
|Sanal ağ|*myVNet1*|Bir sanal ağ bir alt ağ ile *mySubnet*.|
|Depolama hesabı|Dağıtımı için benzersiz|Bir depolama hesabı.|

<a name="parameters"></a>Şablonu dağıtırken aşağıdaki parametrelerin değerlerini belirtmeniz gerekir:

|Ad|Açıklama|
|---|---|
|adminUsername|Yönetici kullanıcı adı. Kullanıcı adı uymalıdır [Azure kullanıcı adı gereksinimleri](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json).|
|Admınpassword|Yönetici parolası parola ile uyumlu [Azure parola gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
|dnsLabelPrefix|PublicIPAddressName1 için DNS adı. DNS adı, bir VM'ye atanan genel IP adresleri çözer. Ad, VM oluşturma Azure bölgesi (konum) içinde benzersiz olmalıdır.|
|dnsLabelPrefix1|PublicIPAddressName2 için DNS adı. DNS adı, bir VM'ye atanan genel IP adresleri çözer. Ad, VM oluşturma Azure bölgesi (konum) içinde benzersiz olmalıdır.|
|OSVersion|VM için Windows/Linux sürümü. Seçili verilen Windows/Linux sürümü tam olarak düzeltme eki görüntüsü işletim sistemidir.|
|imagePublisher|Seçilen VM Windows/Linux görüntü yayımcısı.|
|imageOffer|Seçilen VM için Windows/Linux görüntü.|

Şablon tarafından dağıtılan kaynakların her biri birkaç varsayılan ayarlarla yapılandırılır. Aşağıdaki yöntemlerden birini kullanarak bu ayarları görüntüleyebilirsiniz:

- **Github'da şablonu görüntüleme:** şablonlarıyla aşinaysanız içindeki ayarlar görüntüleyebilirsiniz [şablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json).
- **Dağıttıktan sonra ayarları görüntüleyin:** şablonlarıyla bilmiyorsanız, aşağıdaki bölümlerden birine adımları kullanarak şablonu dağıtmak ve ardından dağıtım sonrasında ayarları görüntüleyin.

Şablonu dağıtmak için Azure portal, PowerShell veya Azure komut satırı arabirimi (CLI) kullanabilirsiniz. Tüm yöntemleri aynı sonucu verir. Şablonu dağıtmak için aşağıdaki bölümleri birindeki adımları tamamlayın:

## <a name="deploy-using-the-azure-portal"></a>Azure portalını kullanarak dağıtımı

Azure portalını kullanarak şablonu dağıtmak için aşağıdaki adımları tamamlayın:

1. Şablon isterseniz değiştirin. Şablon kaynakları dağıtır ve ayarları listelenen [kaynakları](#resources) bu makalenin. Şablonlar ve bunlara Yazar hakkında daha fazla bilgi için okuma [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-network%2ftoc.json)makalesi.
2. Şablon aşağıdaki yöntemlerden biriyle dağıtın:
    - **Portalda şablonu seçin:** bölümündeki adımları tamamlamanız [özel şablon kaynaklardan dağıtmak](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-resources-from-custom-template) makalesi. Adlı önceden var olan şablonu seçin *vm birden çok ipconfig 101*.
    - **Doğrudan:** doğrudan portal şablonu açmak için aşağıdaki düğmeye tıklayın:<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-vm-multiple-ipconfig%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

Yönteminden bağımsız olarak seçtiğiniz için değerler girmeniz gerekir [parametreleri](#parameters) daha önce bu makalede listelenmektedir. VM dağıtıldıktan sonra VM'ye bağlanın ve dağıtılan içindeki adımları tamamlayarak işletim sistemine özel IP adresleri ekleme [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin. Genel IP adreslerine işletim sistemine eklemeyin.

## <a name="deploy-using-powershell"></a>PowerShell kullanarak dağıtın

PowerShell kullanarak şablonu dağıtmak için aşağıdaki adımları tamamlayın:

1. İçindeki adımları tamamlayarak şablonu dağıtmak [PowerShell ile şablon dağıtma](../azure-resource-manager/resource-group-template-deploy-cli.md) makalesi. Bu makalede, bir şablonu dağıtmak için birden çok seçenek anlatılmaktadır. Kullanarak dağıtmak isterseniz `-TemplateUri parameter`, bu şablon için bir URI *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Kullanarak dağıtmak isterseniz `-TemplateFile` parametresi içeriğini kopyalayın [şablon dosyası](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) makinenizde yeni bir dosyaya github'dan. Şablon içeriği isterseniz değiştirin. Şablon kaynakları dağıtır ve ayarları listelenen [kaynakları](#resources) bu makalenin. Şablonlar ve bunlara Yazar hakkında daha fazla bilgi için okuma [Azure Resource Manager şablonları yazma ](../azure-resource-manager/resource-group-authoring-templates.md)makalesi.

    Şablonu ile dağıtmak için belirlediğiniz seçenek bağımsız olarak listelenen parametre değerleri için değerler girmeniz gerekir [parametreleri](#parameters) bu makalenin. Bir parametre dosyası kullanarak parametrelerini seçerseniz, içeriğini kopyalayın [parametreler dosyası](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) bilgisayarınızda yeni bir dosyaya github'dan. Dosyasındaki değerleri değiştirin. Değeri olarak oluşturduğunuz dosyayı kullanmak `-TemplateParameterFile` parametresi.

    OSVersion, ImagePublisher ve imageOffer parametreleri için geçerli değerleri belirlemek için adımları tamamlamanız [erişin ve seçin Windows VM görüntüleri makale](../virtual-machines/windows/cli-ps-findimage.md) makalesi.

    >[!TIP]
    >Bir dnslabelprefix olup emin değilseniz girin `Test-AzureRmDnsAvailability -DomainNameLabel <name-you-want-to-use> -Location <location>` öğrenmek için komutu. Kullanılabilir değilse, komut döndürür `True`.

2. VM dağıtıldıktan sonra VM'ye bağlanın ve dağıtılan içindeki adımları tamamlayarak işletim sistemine özel IP adresleri ekleme [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin. Genel IP adreslerine işletim sistemine eklemeyin.

## <a name="deploy-using-the-azure-cli"></a>Azure CLI kullanarak dağıtın

Azure CLI 1.0 kullanarak şablonu dağıtmak için aşağıdaki adımları tamamlayın:

1. İçindeki adımları tamamlayarak şablonu dağıtmak [bir şablonu Azure CLI ile dağıtma](../azure-resource-manager/resource-group-template-deploy-cli.md) makalesi. Makale şablonu dağıtmak için birden çok seçenekleri açıklar. Kullanarak dağıtmak isterseniz `--template-uri` (-f), bu şablon için bir URI *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Kullanarak dağıtmak isterseniz `--template-file` (-f) parametresi içeriğini kopyalayın [şablon dosyası](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) makinenizde yeni bir dosyaya github'dan. Şablon içeriği isterseniz değiştirin. Şablon kaynakları dağıtır ve ayarları listelenen [kaynakları](#resources) bu makalenin. Şablonlar ve bunlara Yazar hakkında daha fazla bilgi için okuma [Azure Resource Manager şablonları yazma ](../azure-resource-manager/resource-group-authoring-templates.md)makalesi.

    Şablonu ile dağıtmak için belirlediğiniz seçenek bağımsız olarak listelenen parametre değerleri için değerler girmeniz gerekir [parametreleri](#parameters) bu makalenin. Bir parametre dosyası kullanarak parametrelerini seçerseniz, içeriğini kopyalayın [parametreler dosyası](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) bilgisayarınızda yeni bir dosyaya github'dan. Dosyasındaki değerleri değiştirin. Değeri olarak oluşturduğunuz dosyayı kullanmak `--parameters-file` (-e) parametre.

    OSVersion, ImagePublisher ve imageOffer parametreleri için geçerli değerleri belirlemek için adımları tamamlamanız [erişin ve seçin Windows VM görüntüleri makale](../virtual-machines/windows/cli-ps-findimage.md) makalesi.

2. VM dağıtıldıktan sonra VM'ye bağlanın ve dağıtılan içindeki adımları tamamlayarak işletim sistemine özel IP adresleri ekleme [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin. Genel IP adreslerine işletim sistemine eklemeyin.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
