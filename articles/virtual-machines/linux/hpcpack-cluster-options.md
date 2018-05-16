---
title: Azure Linux HPC paketi küme seçenekleri | Microsoft Docs
description: Oluşturup bir Linux yüksek performanslı bilgi işlem (HPC) küme Azure bulutta yönetmek için Microsoft HPC paketi ile seçenekleri hakkında bilgi edinin
services: virtual-machines-linux,cloud-services
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: ac60624e-aefa-40c3-8bc1-ef6d5c0ef1a2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 05/14/2018
ms.author: danlep
ms.openlocfilehash: 281867e30c78c7ed36ac739c8ae1a902463199cd
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="options-with-hpc-pack-to-create-and-manage-a-cluster-for-linux-hpc-workloads-in-azure"></a>HPC Pack oluşturmak ve azure'da Linux HPC iş yükleri için bir kümeyi yönetmek için seçenekleri
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Bu makalede, Linux iş yüklerini çalıştırmak için HPC Pack kullanma Azure seçenekleri odaklanır. Ayrıca çalıştırmak için seçenek vardır [HPC paketi ile Windows HPC iş yüklerini](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="hpc-pack-cluster-in-azure-vms-and-vm-scale-sets"></a>HPC Pack kümede Azure Vm'leri ve VM ölçek kümesi
### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları
* (GitHub) [HPC Pack 2016 güncelleştirme 1'küme şablonları](https://github.com/MsHpcPack/HPCPack2016)
* (GitHub) [HPC Pack 2012 R2 küme şablonları](https://github.com/MsHpcPack/HPCPack2012R2)
* (Hızlı Başlangıç) [Linux işlem düğümleri ile bir HPC Pack 2012 R2 kümesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)

### <a name="azure-vm-images"></a>Azure VM görüntüleri
Gözat [Azure Marketi HPC Pack görüntülerinde](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?page=1&search=%22HPC%20%20Pack%22) kendi Azure HPC Pack kümede oluşturmak istiyorsanız.

## <a name="hpc-pack-2012-r2-cluster-in-classic-deployment-model"></a>Klasik dağıtım modelinde HPC Pack 2012 R2 küme
* [Linux HPC Kümesi ile HPC Pack Iaas dağıtım komut dosyası oluşturma](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [MPI uygulamaları çalıştırmak için Linux RDMA küme ayarlama](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Öğretici: Azure HPC Pack kümesinde Linux işlem düğümlerine kullanmaya başlama](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Öğretici: Çalıştır NAMD Microsoft HPC paketi ile Linux işlem düğümlerini Azure](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Öğretici: Azure Linux RDMA kümesinde Microsoft HPC paketi ile çalışma OpenFOAM](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Öğretici: Çalıştır yıldız-CCM + Linux RDMA üzerinde Microsoft HPC Pack ile Azure'da küme](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="job-submisssion"></a>İş submisssion
* [Azure'da bir HPC Pack kümeye iş göndermek](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)




