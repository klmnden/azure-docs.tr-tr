---
title: Azure'da bir Linux VHD indirme | Microsoft Docs
description: Azure CLI ve Azure portalını kullanarak bir Linux VHD indirin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: f72d49a3ab204ce64eb89d0f05630b640c138e0a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61390392"
---
# <a name="download-a-linux-vhd-from-azure"></a>Azure'da Linux VHD'si indirin

Bu makalede, Azure CLI ve Azure portalını kullanarak Azure Linux sanal sabit disk (VHD) dosya indirme öğrenin. 

Zaten yapmadıysanız, yükleme [Azure CLI](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="stop-the-vm"></a>VM’yi durdurma

Çalışan bir VM'ye ekli ise bir VHD Azure'dan karşıdan yüklenemiyor. Bir VHD yüklemek için sanal Makineyi durdurun gerekir. Bir VHD'yi kullanılabilir olarak kullanmak istiyorsanız bir [görüntü](tutorial-custom-images.md) diğer sanal makineleri ile yeni diskler oluşturmak için sağlamasını kaldırma ve sanal Makineyi durdurun dosyasında yer alan işletim sistemini genelleştirir gerekir. VHD için yeni bir örnek var olan bir sanal makine veya veri diski disk olarak kullanmak için yalnızca durdurun ve VM'yi serbest bırakın gerekir.

VHD, diğer sanal makineler oluşturmak için bir görüntü olarak kullanmak için aşağıdaki adımları tamamlayın:

1. SSH, hesap adı ve VM'nin genel IP adresini buna bağlanmaya ve onu sağlamasını kaldırmak için kullanın. Genel IP adresiyle bulabilirsiniz [az ağ public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az-network-public-ip-show). \+ Kullanıcı parametresi, son sağlanan kullanıcı hesabı da kaldırır. Hesap kimlik bilgilerini VM'ye saklanacağı bu tutulacaksa + kullanıcı parametresi. Aşağıdaki örnek, son sağlanan kullanıcı hesabı kaldırır:

    ```bash
    ssh azureuser@<publicIpAddress>
    sudo waagent -deprovision+user -force
    exit 
    ```

2. Azure hesabınızda oturum açın [az login](https://docs.microsoft.com/cli/azure/reference-index).
3. Durdur ve VM'yi serbest bırakın.

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. VM'yi Genelleştirme. 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

VHD için yeni bir örnek var olan bir sanal makine veya veri diski disk olarak kullanmak için aşağıdaki adımları tamamlayın:

1.  [Azure Portal](https://portal.azure.com/) oturum açın.
2.  Hub menüsünde, **Virtual Machines**’e tıklayın.
3.  Listeden VM'yi seçin.
4.  Sanal makine için dikey penceresinde tıklayın **Durdur**.

    ![VM'yi durdurma](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>SAS URL'si oluşturun

Oluşturmak istediğiniz VHD dosyasını indirmek için bir [paylaşılan erişim imzası (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL'si. URL oluşturulduğunda, sona erme süresini URL'sine atanır.

1.  Sanal makine için dikey pencerenin menüsünde **diskleri**.
2.  VM için işletim sistemi diski seçin ve ardından **dışarı**.
3.  Tıklayın **URL'yi oluşturmak**.

    ![URL oluştur](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a>VHD indirme

1.  Oluşturulan URL'si altında VHD dosyası yükleme'ye tıklayın.

    ![VHD indirme](./media/download-vhd/export-download.png)

2.  Tıklaymanız gerekebilir **Kaydet** yüklemeyi başlatmak için tarayıcıda. VHD dosyası için varsayılan ad *abcd*.

    ![Tarayıcıda Kaydet'e tıklayın](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Azure CLI ile bir özel diskten Linux VM oluşturma ve karşıya yükleme](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- [Azure CLI'yı Azure disklerini yönetme](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

