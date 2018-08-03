---
title: Bir Azure VM'de HPC paketi üstbilgi düğümü oluştur | Microsoft Docs
description: Azure portal ve Resource Manager dağıtım modeli, bir Azure sanal Makinesinde Microsoft HPC Pack 2012 R2 baş düğümü oluşturmak için kullanmayı öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,hpc-pack
ms.assetid: e6a13eaf-9124-47b4-8d75-2bc4672b8f21
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 70c1d95f704315ee6a481367188e2bb620916702
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428965"
---
# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Azure VM’de Market görüntüsü ile bir HPC Pack kümesinin baş düğümünü oluşturma
Kullanım bir [Microsoft HPC Pack 2012 R2 sanal makine görüntüsü](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) Azure Marketi'nde ve Azure portalında bir HPC kümesinin baş düğümünü oluşturma. Bu HPC Pack VM görüntüsü, önceden yüklenmiş HPC Pack 2012 R2 güncelleştirme 3 ile Windows Server 2012 R2 Datacenter dayanır. Bu baş düğüm, kavram kanıtı dağıtımı azure'da HPC Pack kullanın. Ardından, işlem düğümleri HPC iş yüklerini çalıştırmak için kümeye ekleyebilirsiniz.

> [!TIP]
> Baş düğüm ve işlem düğümleri içeren azure'da eksiksiz bir HPC Pack 2012 R2 kümesine dağıtmak için otomatikleştirilmiş bir yöntem kullanmanızı öneririz. Seçenekleriniz [HPC Pack Iaas dağıtım betiği](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) ve Resource Manager şablonları gibi [Windows iş yükleri için HPC Pack küme](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/). Resource Manager şablonları için kullanılabilir ayrıca [Microsoft HPC Pack 2016 kümesi](https://github.com/MsHpcPack/HPCPack2016/tree/master/newcluster-templates). 
> 
> 

## <a name="planning-considerations"></a>Planlama konusunda dikkat edilmesi gerekenler
Aşağıdaki resimde gösterildiği gibi HPC paketi üstbilgi düğümü bir Active Directory etki alanında bir Azure sanal ağı dağıtın.

![HPC paketi üstbilgi düğümü][headnode]

* **Active Directory etki alanı**: Hizmetleri VM'de baş düğümü katılmalıdır azure'da bir Active Directory etki alanına HPC başlamadan önce HPC Pack 2012 R2. Bu makalede, bir kavram kanıtı dağıtımı, gösterildiği gibi baş düğüm için HPC Hizmetleri başlatmadan önce bir etki alanı denetleyicisi olarak oluşturduğunuz VM yükseltebilirsiniz. Başka bir seçenek ayrı bir etki alanı denetleyicisi ve üstbilgi düğüm VM'ine katılın azure'daki orman dağıtmaktır.

* **Dağıtım modeli**: yeni dağıtımların çoğu için Microsoft, Resource Manager dağıtım modelini kullanmasını önerir. Bu makalede, bu dağıtım modeli kullandığınız varsayılır.

* **Azure sanal ağı**: baş düğüm dağıtmak için Resource Manager dağıtım modeli kullandığınız zaman belirttiğinizde veya bir Azure sanal ağı oluşturun. Baş düğüm için mevcut bir Active Directory etki alanına katılmak sanal ağ kullanın. Ayrıca, daha sonra VM işlem düğümü kümeye eklemek için gerekir.

## <a name="steps-to-create-the-head-node"></a>Baş düğümünü oluşturma adımları
Azure portalında Resource Manager dağıtım modelini kullanarak HPC paketi üstbilgi düğümü için bir Azure VM oluşturmak için kullanılacak üst düzey adımlar aşağıda verilmiştir. 

1. Yeni bir Active Directory ormanı Azure'da Vm'leri ayrı bir etki alanı denetleyicisiyle oluşturmak isterseniz, kullanmak için bir seçenek olan bir [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc). Basit bir kavram kanıtı dağıtımı için bu adımı atlayabilir ve üstbilgi düğüm VM'ine kendi etki alanı denetleyicisi olarak yapılandırmak uygundur. Bu seçenek, bu makalenin sonraki bölümlerinde açıklanmıştır.
1. Üzerinde [HPC Pack 2012 R2, Windows Server 2012 R2 sayfasında](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) Azure Market'teki tıklayın **sanal makine oluştur**. 
1. Portalında, üzerinde **HPC Pack 2012 R2, Windows Server 2012 R2'de** sayfasında **Resource Manager** dağıtım modeli ve ardından **Oluştur**.
   
    ![HPC Pack görüntüsü][marketplace]
1. Portal ayarları yapılandırın ve VM'yi oluşturmak için kullanın. Azure'da yeniyseniz, öğreticiyi izleyebilirsiniz [Azure portalında bir Windows sanal makine oluşturma](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bir kavram kanıtı dağıtımı için genellikle varsayılan kabul edebilir veya önerilen ayarlar.
   
   > [!NOTE]
   > Baş düğüm, azure'da var olan bir Active Directory etki alanına katılmak isterseniz, sanal ağı, etki alanı için VM'yi oluştururken belirttiğiniz emin olun.
   > 
   > 
1. Sonra VM'yi oluşturun ve VM'nin çalıştığından, [VM'ye bağlanmak](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Uzak Masaüstü tarafından. 
1. VM, aşağıdaki seçeneklerden birini seçerek bir Active Directory etki alanı ormanına Katıl:
   
   * Mevcut bir etki alanı ormanı ile bir Azure sanal ağındaki bir VM oluşturduysanız, VM, standart sunucu yöneticisi veya Windows PowerShell araçlarını kullanarak ormana katılın. Yeniden başlatın.
   * (Mevcut bir etki alanı ormanı) olmadan yeni bir sanal ağdaki bir VM oluşturduysanız, VM bir etki alanı denetleyicisi olarak Yükselt. Baş düğüm üzerinde Active Directory etki alanı Hizmetleri rolünü yükleyebilir ve için standart adımları kullanın. Ayrıntılı adımlar için bkz. [bir yeni Windows Server 2012 Active Directory ormanı yükleme](https://technet.microsoft.com/library/jj574166.aspx).
1. HPC Pack Hizmetleri, VM çalıştıran ve bir Active Directory ormanına katıldığından sonra şu şekilde başlatın:
   
    a. Baş düğüme VM yerel Yöneticiler grubunun bir üyesi olan bir etki alanı hesabı kullanarak bağlanın. Örneğin, baş düğüm VM oluşturduğunuz sırada ayarladığınız yönetici hesabı kullanın.
   
    b. Varsayılan baş düğüm yapılandırması için Windows PowerShell'i yönetici başlatın ve aşağıdaki komutu yazın:
   
    ```PowerShell
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```
   
    Bu, başlatmak HPC Pack Hizmetleri için birkaç dakika sürebilir.
   
    Ek baş düğümü yapılandırma seçenekleri için türü `get-help HPCHNPrepare.ps1`.

## <a name="next-steps"></a>Sonraki adımlar
* Artık, HPC Pack kümesinin baş düğümünü ile çalışabilirsiniz. Örneğin, HPC Küme Yöneticisi'ni başlatın ve tamamlayın [dağıtım Yapılacaklar listesi](https://technet.microsoft.com/library/jj884141.aspx).
* Küme işlem kapasitesine isteğe bağlı artırmak istiyorsanız, ekleme [Azure veri bloğu düğümleri](classic/hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) bir bulut hizmetinde. 
* Küme üzerinde bir test iş yükü çalıştırmayı deneyin. HPC Pack bir örnek için bkz [Başlangıç Kılavuzu](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/hpcpack-cluster-headnode/marketplace.png
