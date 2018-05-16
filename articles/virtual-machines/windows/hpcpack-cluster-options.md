---
title: Microsoft HPC Pack küme Azure seçeneklerinde | Microsoft Docs
description: Oluşturmak ve bir Windows yüksek performanslı bilgi işlem (HPC) küme Azure bulutta yönetmek için Microsoft HPC paketi ile seçenekleri hakkında bilgi edinin
services: virtual-machines-windows,cloud-services,batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 02c5566d-2129-483c-9ecf-0d61030442d7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/14/2018
ms.author: danlep
ms.openlocfilehash: 1232228d5e2fcd05f41a096a933072dffcca2119
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="options-with-hpc-pack-to-create-and-manage-a-cluster-for-windows-hpc-workloads-in-azure"></a>HPC Pack oluşturmak ve azure'da Windows HPC iş yükleri için bir kümeyi yönetmek için seçenekleri
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Bu makalede Windows iş yüklerini çalıştırmak için HPC Pack kümeleri oluşturmak için Azure seçeneklerini odaklanır. Ayrıca çalıştırmak için HPC Pack kümeleri oluşturmak için seçenekler vardır [Linux HPC iş yükleri](../linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="hpc-pack-cluster-in-azure-vms-and-vm-scale-sets"></a>HPC Pack kümede Azure Vm'leri ve VM ölçek kümesi
### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları
* (GitHub) [HPC Pack 2016 güncelleştirme 1'küme şablonları](https://github.com/MsHpcPack/HPCPack2016)
* (GitHub) [HPC Pack 2012 R2 küme şablonları](https://github.com/MsHpcPack/HPCPack2012R2)
* (Hızlı Başlangıç) [Bir HPC Pack 2012 R2 kümesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)
* (Hızlı Başlangıç) [Özel işlem düğümü görüntüsüyle bir HPC Pack 2012 R2 kümesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure VM görüntüleri
Gözat [Azure Marketi HPC Pack görüntülerinde](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?page=1&search=%22HPC%20%20Pack%22) kendi Azure HPC Pack kümede oluşturmak istiyorsanız.


### <a name="tutorials"></a>Öğreticiler
* [Öğretici: Azure bir HPC Pack 2016 küme dağıtma](hpcpack-2016-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure Active Directory ile Azure HPC Pack 2016 kümede yönetme](hpcpack-cluster-active-directory.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)


## <a name="hpc-pack-2012-r2-cluster-deployment-in-the-classic-deployment-model"></a>Klasik dağıtım modelinde HPC Pack 2012 R2 Küme dağıtımı
* [HPC Kümesi ile HPC Pack Iaas dağıtım komut dosyası oluşturma](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Öğretici: Azure Excel ve SOA iş yüklerini çalıştırmak için bir HPC paketi küme kullanmaya başlama](excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure HPC Pack kümede işlem düğümlerine yönetme](classic/hpcpack-cluster-node-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Büyüme ve Azure işlem kaynakları bir HPC Pack kümesindeki Daralt](classic/hpcpack-cluster-node-autogrowshrink.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [MPI uygulamalarını çalıştırmak için kullanılan HPC Pack içeren Windows RDMA kümesi ayarlama](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)


## <a name="burst-to-azure-from-hpc-pack-2012-r2"></a>HPC Pack 2012 R2'den Azure'a veri bloğu
* [Microsoft HPC paketi ile Azure çalışan örneklerine veri bloğu](https://technet.microsoft.com/library/gg481749.aspx)
* [HPC paketi ile Azure toplu işlem için veri bloğu](https://technet.microsoft.com/library/mt612877.aspx)
* [Öğretici: Azure HPC paketi ile karma küme ayarlayın](../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

## <a name="job-submission"></a>İş gönderme

* [Azure'da bir HPC Pack kümeye iş göndermek](hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)






