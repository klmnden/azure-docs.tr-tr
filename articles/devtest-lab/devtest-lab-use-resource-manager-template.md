---
title: "Görüntüleme ve bir sanal makinenin Azure Resource Manager şablonu kullanma | Microsoft Docs"
description: "Diğer sanal makineleri oluşturmak için bir sanal makineden Azure Resource Manager şablonu kullanmayı öğrenin"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a759d9ce-655c-4ac6-8834-cb29dd7d30dd
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: tarcher
ms.openlocfilehash: 0807ab367b91be5acd261f2b58ca2112b2c9e380
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-a-virtual-machines-azure-resource-manager-template"></a>Bir sanal makinenin Azure Resource Manager şablonu kullanın

Oluştururken bir sanal makine (VM) DevTest Labs içinde [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), VM kaydetmeden önce Azure Resource Manager şablonu görüntüleyebilirsiniz. Daha fazla laboratuvarı sanal makineleri aynı ayarlarla oluşturmak için şablon ardından temeli olarak kullanılabilir.

Bu makalede, Resource Manager şablonu bir VM oluşturulurken görüntüleme ve daha sonra aynı VM oluşturmayı otomatikleştirmek için dağıtmayı açıklar.

## <a name="multi-vm-vs-single-vm-resource-manager-templates"></a>Çoklu VM tek VM Resource Manager şablonları karşılaştırması
Resource Manager şablonu kullanarak DevTest Labs'de Vm'leri oluşturmanın iki yolu vardır: Microsoft.DevTestLab/labs/virtualmachines kaynak sağlamak veya Microsoft.Commpute/virtualmachines kaynak sağlanamadı. Her farklı senaryolarda kullanılır ve farklı izinleri gerektirir.

- ("Kaynak" özelliği şablondaki bildirilen gibi) bir Microsoft.DevTestLab/labs/virtualmachines kaynak türünü kullanan resource Manager şablonları tek tek Laboratuvar VM'ler sağlayabilirsiniz. Her VM sonra DevTest Labs sanal makineler listesindeki tek bir öğe olarak görüntülenir:

   ![Sanal makineleri tek öğeleri DevTest Labs sanal makineler listesi olarak listesi](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-item.png)

   Bu tür bir Resource Manager şablonu Azure PowerShell komutu aracılığıyla sağlanabilir **New-AzureRmResourceGroupDeployment** veya Azure CLI komutu aracılığıyla **az grup dağıtımı oluşturmak**. DevTest Labs kullanıcı rolü ile atanan kullanıcıların dağıtım gerçekleştiremezler yönetici izinleri gerektirir. 

- Bir Microsoft.Compute/virtualmachines kaynak türünü kullanan resource Manager şablonları DevTest Labs sanal makineler listesi tek bir ortamda olarak birden çok VM sağlayabilirsiniz:

   ![Sanal makineleri tek öğeleri DevTest Labs sanal makineler listesi olarak listesi](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-environment.png)

   Sanal makineleri aynı ortamda birlikte yönetilebilir ve aynı yaşam döngüsü paylaşın. Bir DevTest Labs kullanıcı rolünün atandığı kullanıcılar yönetici Laboratuvar bu şekilde yapılandırılan olduğu sürece bu şablonları kullanarak ortamları oluşturabilirsiniz.

Bu makalenin sonraki bölümlerinde Mirosoft.DevTestLab/labs/virtualmachines kullanan Resource Manager şablonları açıklanır. Laboratuvar VM oluşturma (örneğin, claimable VM'ler) ya da altın görüntü oluşturma (örneğin, görüntü fabrikada) otomatikleştirmek için bu Laboratuvar yöneticileri tarafından kullanılır.

[En iyi uygulamalar Azure Resource Manager şablonları oluşturmak için](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) güvenilir ve kullanımı kolay pek çok kılavuzları ve Azure Resource Manager şablonları oluşturmanıza yardımcı olacak öneriler sunar.

## <a name="view-and-save-a-virtual-machines-resource-manager-template"></a>Görüntüleme ve bir sanal makinenin Resource Manager şablonu kaydetme
1. Bölümündeki adımları izleyin [bir laboratuar ortamında ilk VM oluşturma](devtest-lab-create-first-vm.md) bir sanal makine oluşturmaya başlamak için.
1. Sanal makineniz için gereken bilgileri girin ve bu VM için istediğiniz herhangi bir yapı ekleyin.
1. Yapılandırma ayarları pencerenin alt kısmındaki seçin **görünüm ARM şablonu**.

   ![Görünüm ARM şablonu düğmesi](./media/devtest-lab-use-arm-template/devtestlab-lab-view-rm-template.png)
1. Kopyalayın ve daha sonra başka bir sanal makine oluşturmak için kullanmak üzere Resource Manager şablonu kaydedin.

   ![Daha sonra kullanmak üzere kaydetmek için resource manager şablonu](./media/devtest-lab-use-arm-template/devtestlab-lab-copy-rm-template.png)

Resource Manager şablonu kaydettikten sonra kullanmadan önce şablonu parametreleri bölümünü güncelleştirmeniz gerekir. Gerçek Resource Manager şablonu dışında parametreleri yalnızca özelleştirir parameter.json oluşturabilirsiniz. 

![Bir JSON dosyası kullanarak parametre özelleştirme](./media/devtest-lab-use-arm-template/devtestlab-lab-custom-params.png)

## <a name="deploy-a-resource-manager-template-to-create-a-vm"></a>Bir VM oluşturmak için Resource Manager şablonu dağıtma
Resource Manager şablonu kaydedilir ve gereksinimlerinize göre özelleştirilmiş sonra VM oluşturmayı otomatikleştirmek için kullanabilirsiniz. [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) kaynaklarınızı Azure'a dağıtmak için Resource Manager şablonları ile Azure PowerShell kullanmayı açıklar. [Resource Manager şablonları ve Azure CLI kaynaklarla dağıtmak](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli) kaynaklarınızı Azure'a dağıtmak için Resource Manager şablonları ile Azure CLI kullanmayı açıklar.

> [!NOTE]
> Yalnızca Laboratuvar sahip izinlerine sahip bir kullanıcı Azure PowerShell kullanarak sanal makineleri bir Resource Manager şablonu oluşturabilirsiniz. Resource Manager şablonu kullanarak VM oluşturmayı otomatikleştirmek istediğiniz ve yalnızca kullanıcı izinlerine sahip, kullanabileceğiniz [ **az Laboratuvar vm oluşturma** CLI komutunu](https://docs.microsoft.com/cli/azure/lab/vm#az_lab_vm_create).

### <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Resource Manager şablonları ile çoklu VM ortamları oluşturma](devtest-lab-create-environment-from-arm.md).
* Daha fazla hızlı başlangıç Resource Manager şablonları DevTest Labs Otomasyon keşfedin [ortak DevTest Labs GitHub deposuna](https://github.com/Azure/azure-quickstart-templates).
