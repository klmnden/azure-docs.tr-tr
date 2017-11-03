---
title: "Bir Azure VM'de HPC paketi üstbilgi düğümü oluşturma | Microsoft Docs"
description: "Bir Microsoft HPC Pack 2012 R2 baş düğüm bir Azure VM oluşturmak için Azure portalı ve Resource Manager dağıtım modeli kullanmayı öğrenin."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,hpc-pack
ms.assetid: e6a13eaf-9124-47b4-8d75-2bc4672b8f21
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: b2bb9caf82a580dc5f67ea0b0b1c2e9a46363e9c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Azure VM’de Market görüntüsü ile bir HPC Pack kümesinin baş düğümünü oluşturma
Kullanım bir [Microsoft HPC Pack 2012 R2 sanal makine görüntüsü](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) Azure Marketi ve Azure portalında bir HPC küme baş düğümüne oluşturmak için. Bu HPC Pack VM görüntüsü, Windows Server 2012 R2 Datacenter HPC Pack 2012 R2 güncelleştirme 3'de önceden yüklenmiş olan dayanır. Azure HPC Pack dağıtımının kavram kanıtı için bu baş düğümü kullanın. İşlem düğümleri kümeye HPC iş yüklerini çalıştırmak için daha sonra ekleyebilirsiniz.

> [!TIP]
> Tam bir HPC Pack 2012 R2 küme baş düğüm ve işlem düğümlerini içeren azure'da dağıtmak için otomatik olarak yöntemi kullanmanızı öneririz. Seçenekleriniz [HPC Pack Iaas dağıtım betiği](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) ve Resource Manager şablonları gibi [HPC paketi küme iş yükleri için](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/). Resource Manager şablonları için kullanılabilen de [Microsoft HPC Pack 2016 kümeleri](https://github.com/MsHpcPack/HPCPack2016/tree/master/newcluster-templates). 
> 
> 

## <a name="planning-considerations"></a>Planlama konusunda dikkat edilmesi gerekenler
Aşağıdaki çizimde gösterildiği gibi bir Active Directory etki alanı içindeki bir Azure sanal ağı HPC paketi üstbilgi düğümü dağıtın.

![HPC paketi üstbilgi düğümü][headnode]

* **Active Directory etki alanı**: baş düğüm katılmalıdır Azure Active Directory etki alanı HPC başlamadan önce HPC Pack 2012 R2 Hizmetleri VM. Bu makalede, kanıt kavramı dağıtım için gösterildiği gibi baş düğüm için HPC Hizmetleri başlatmadan önce bir etki alanı denetleyicisi olarak oluşturduğunuz VM yükseltebilirsiniz. Başka bir seçenek ayrı etki alanı denetleyicisi ve üstbilgi düğüm VM'ine katılma Azure ormanda dağıtmaktır.

* **Dağıtım modeli**: çoğu yeni dağıtımlar için Microsoft Resource Manager dağıtım modeli kullanmanızı önerir. Bu makale, bu dağıtım modeli kullanan varsaymaktadır.

* **Azure sanal ağı**: baş düğüm dağıtmak için Resource Manager dağıtım modeli kullandığınızda, belirtin veya bir Azure sanal ağı oluşturun. Baş düğüm mevcut bir Active Directory etki alanına katılmak sanal ağ kullanın. Ayrıca, daha sonra işlem düğümü VM'ler kümeye eklemek için gerekir.

## <a name="steps-to-create-the-head-node"></a>Baş düğüm oluşturma adımları
Resource Manager dağıtım modeli kullanarak bir Azure VM'de HPC paketi üstbilgi düğümü için oluşturmak üzere Azure portalını kullanmak için üst düzey adımlar aşağıda belirtilmiştir. 

1. Ayrı etki alanı denetleyicisi ile sanal makineleri yeni bir Active Directory ormanı oluşturmak istiyorsanız, kullanmak için bir seçenek olan bir [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc). Kavram kanıtı dağıtımı basit bir kanıt bu adımı atlayın ve üstbilgi düğüm VM'ine kendisini bir etki alanı denetleyicisi olarak yapılandırmak uygundur. Bu seçenek bu makalenin sonraki bölümlerinde açıklanmıştır.
2. Üzerinde [HPC Pack 2012 R2 Windows Server 2012 R2 sayfasında](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) Azure Marketi'nde tıklatın **sanal makine oluşturma**. 
3. Portalda, üzerinde **HPC Pack 2012 R2'de Windows Server 2012 R2** sayfasında, **Resource Manager** dağıtım modeli ve ardından **oluşturma**.
   
    ![HPC Pack görüntü][marketplace]
4. Ayarları yapılandırın ve VM oluşturmak üzere portalı kullanın. Azure'da yeniyseniz, öğreticiyi izleyin [Azure portalında bir Windows sanal makine oluşturma](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Kanıt kavram kanıtı dağıtımı için genellikle varsayılan ayarı kabul edebilirsiniz veya önerilen ayarlar.
   
   > [!NOTE]
   > Baş düğüm Azure mevcut bir Active Directory etki alanına katılmak istiyorsanız, o etki alanı için sanal ağ VM oluşturulurken belirttiğinizden emin olun.
   > 
   > 
5. Sonra VM oluşturma ve VM'nin çalıştığından, [VM'e bağlanmak](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Uzak Masaüstü tarafından. 
6. VM, aşağıdaki seçeneklerden birini seçerek bir Active Directory etki alanı ormanı Katıl:
   
   * Var olan bir etki alanı ormanı ile bir Azure sanal ağında VM oluşturduysanız, VM standart sunucu yöneticisi veya Windows PowerShell araçlarını kullanarak ormana katılın. Yeniden başlatın.
   * Yeni bir sanal ağ (olmadan var olan bir etki alanı ormanı) VM oluşturduysanız VM bir etki alanı denetleyicisi olarak yükseltin. Standart adımları yükleyip baş düğümünde Active Directory etki alanı Hizmetleri rolünü yapılandırmak için kullanın. Ayrıntılı adımlar için bkz: [yeni bir Windows Server 2012 Active Directory ormanı yüklemek](https://technet.microsoft.com/library/jj574166.aspx).
7. VM çalıştıran ve bir Active Directory ormanına katılmadığı sonra HPC Pack Hizmetleri gibi başlatın:
   
    a. Baş düğüm VM yerel Administrators grubunun üyesi olan bir etki alanı hesabı kullanarak bağlanın. Örneğin, baş düğüm VM oluşturduğunuz sırada ayarladığınız yönetici hesabı kullanın.
   
    b. Varsayılan bir baş düğüm yapılandırma için Windows PowerShell'i yönetici olarak başlatın ve aşağıdaki komutu yazın:
   
    ```PowerShell
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```
   
    HPC Pack hizmetleri başlatmak birkaç dakika sürebilir.
   
    Ek baş düğümü yapılandırma seçenekleri için yazın `get-help HPCHNPrepare.ps1`.

## <a name="next-steps"></a>Sonraki adımlar
* Şimdi, HPC paketi küme baş düğümüne ile çalışabilirsiniz. Örneğin, HPC Küme Yöneticisi'ni başlatın ve tamamlayın [dağıtım Yapılacaklar listesi](https://technet.microsoft.com/library/jj884141.aspx).
* Küme bilgi işlem kapasitesi isteğe bağlı artırmak istiyorsanız, ekleme [Azure veri bloğu düğümleri](classic/hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) bir bulut hizmetinde. 
* Küme üzerinde bir test iş yükü çalıştırmayı deneyin. Bir örnek için bkz: HPC paketi [Başlangıç Kılavuzu](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/hpcpack-cluster-headnode/marketplace.png
