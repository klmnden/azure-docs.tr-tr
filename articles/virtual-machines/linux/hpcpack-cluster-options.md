---
title: Azure'da Linux HPC Pack küme seçenekleri | Microsoft Docs
description: Oluşturmak ve bir Linux yüksek performanslı bilgi işlem (HPC) kümesi Azure bulutunda yönetmek için Microsoft HPC Pack ile seçenekler hakkında bilgi edinin
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
ms.openlocfilehash: 79600909387b1876b112219b5b9b1e45ba4054b7
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340084"
---
# <a name="options-with-hpc-pack-to-create-and-manage-a-cluster-for-linux-hpc-workloads-in-azure"></a>Azure'da Linux HPC iş yükleri için bir küme oluşturma ve yönetme için HPC Pack ile seçenekleri
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Bu makalede Linux iş yüklerinizi çalıştırmak için HPC Pack kullanılacak Azure seçenekleri odaklanır. Ayrıca çalıştırmak için seçenek vardır [HPC Pack ile Windows HPC iş yüklerini](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="hpc-pack-cluster-in-azure-vms-and-vm-scale-sets"></a>HPC Pack kümesinde Azure VM ve VM ölçek kümeleri
### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları
* (GitHub) [HPC Pack 2016 güncelleştirme 1'küme şablonları](https://github.com/MsHpcPack/HPCPack2016)
* (GitHub) [HPC Pack 2012 R2 kümesi şablonları](https://github.com/MsHpcPack/HPCPack2012R2)
* (Hızlı Başlangıç) [Linux işlem düğümleriyle bir HPC Pack 2012 R2 kümesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)

### <a name="azure-vm-images"></a>Azure VM görüntüleri
Gözat [HPC Pack görüntüleri Azure Market'te](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?page=1&search=%22HPC%20%20Pack%22) kendi HPC Pack kümesinde oluşturmak istiyorsanız.

## <a name="hpc-pack-2012-r2-cluster-in-classic-deployment-model"></a>Klasik dağıtım modelinde HPC Pack 2012 R2 kümesi
* [HPC Pack Iaas dağıtım betiği içeren bir Linux HPC kümesi oluşturun](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Öğretici: azure'da HPC Pack kümesinde Linux işlem düğümleri kullanmaya başlayın](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Öğretici: Linux üzerinde Microsoft HPC Pack ile NAMD çalıştırma azure'deki işlem düğümlerinde](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Öğretici: Azure'da bir Linux RDMA kümesinde Microsoft HPC Pack ile çalışma OpenFOAM](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Öğretici: Çalıştırma STAR-CCM + üzerinde bir Linux RDMA Microsoft HPC Pack ile Azure'da küme](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="job-submisssion"></a>İş gönderimi
* [Azure'da bir HPC Pack kümesine işlerini gönderme](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)




