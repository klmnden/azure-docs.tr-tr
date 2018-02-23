---
title: "Bir Linux VM bir görüntüsünü yakalama | Microsoft Docs"
description: "Bir Linux tabanlı Azure sanal Klasik dağıtım modeli kullanılarak oluşturulmuş makine (VM) bir görüntüsünü yakalama öğrenin."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 17d7ffee-a58e-4290-9de1-64c3cf1ddc05
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
ms.openlocfilehash: be463b18c049c8b92c21cfde82defcf76718a5f0
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Klasik bir Linux sanal makinesini görüntü olarak yakalama
> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. [Bu adımları Resource Manager modeli kullanarak gerçekleştirmeyi](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) öğrenin.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Bu makalede bir Klasik Azure Linux diğer sanal makineler oluşturmak için bir resim olarak çalıştıran sanal makine (VM) yakalama gösterilmektedir. Bu görüntü işletim sistemi diski ve veri diskleri VM'ye bağlı içerir. Diğer VM görüntüsünü oluşturduğunuzda, yapılandırmanız gereken şekilde ağ yapılandırmasını içermez.

Azure depolar altındaki görüntüyü **görüntüleri**, karşıya tüm görüntüleri yanı sıra. Görüntüleri hakkında daha fazla bilgi için bkz: [azure'da sanal makine görüntülerini hakkında][About Virtual Machine Images in Azure].

## <a name="before-you-begin"></a>Başlamadan önce
Bu adımlarda, Klasik dağıtım modeli kullanarak bir Azure VM oluşturulur ve yapılandırılmış olan veri disklerinin ekleme de dahil olmak üzere işletim sistemi varsayılmaktadır. Bir VM oluşturmak ihtiyacınız varsa okuyun [Linux sanal makine oluşturmak nasıl][How to Create a Linux Virtual Machine].

## <a name="capture-the-virtual-machine"></a>Sanal makine yakalanamadı
1. [VM'ye bağlanın](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tercih ettiğiniz bir SSH istemcisi kullanarak.
2. SSH penceresinde aşağıdaki komutu yazın. Çıktısını `waagent` bu yardımcı program sürümüne bağlı olarak biraz değişebilir:

    ```bash
    sudo waagent -deprovision+user
    ```

    Yukarıdaki komut, sistem temizlemek ve sağlama işleminin için uygun hale dener. Bu işlem, aşağıdaki görevleri gerçekleştirir:

   * (Provisioning.RegenerateSshHostKeyPair yapılandırma dosyasındaki ' y' ise) SSH ana bilgisayar anahtarlarını kaldırır
   * Ad sunucusu yapılandırmasında /etc/resolv.conf temizler
   * Kaldırır `root` (Provisioning.DeleteRootPassword 'y' yapılandırma dosyasında ise) / etc/gölge kullanıcı parolasından
   * Önbelleğe alınan istemci kiralamalarını kaldırır
   * Ana bilgisayar adını localhost.localdomain adına sıfırlar
   * Siler son sağlanan (/var/lib/waagent elde edilen) kullanıcı hesabı **ve veri ilişkili**.

     > [!NOTE]
     > Sağlamayı kaldırmayı dosyaları ve "görüntü genelleştirmek için" verileri siler. Yalnızca yeni bir görüntü şablonu olarak yakalama düşündüğünüz bir VM'de bu komutu çalıştırın. Görüntü tüm hassas bilgilerin temizlenmiş veya üçüncü taraflar için yeniden dağıtım için uygun garanti etmez.

3. Tür **y** devam etmek için. Ekleyebileceğiniz `-force` bu onay adım önlemek için parametre.
4. Tür **çıkış** SSH istemcisi kapatın.

   > [!NOTE]
   > Geri kalan adımları zaten sahip varsayın [Azure CLI yüklenmiş](../../../cli-install-nodejs.md) istemci bilgisayarınızda. Aşağıdaki adımların tümünü de yapılabilir [Azure portal](http://portal.azure.com).

5. İstemci bilgisayarından, Azure CLI ve Azure aboneliğinizde oturum açın. Ayrıntılar için [Azure clı'dan Azure aboneliğine Bağlan](/cli/azure/authenticate-azure-cli).

   > [!NOTE]
   > Azure portalında portalında oturum açın.

6. Hizmet Yönetimi modunda olduğundan emin olun:

    ```azurecli
    azure config mode asm
    ```

7. Zaten sağlaması kaldırılıyor. sağlaması VM kapatma. Aşağıdaki örnek adlı VM kapatır `myVM`:

    ```azurecli
    azure vm shutdown myVM
    ```
   Gerekirse, aboneliğinizde kullanılarak oluşturulan tüm VM'lerin bir listesini görüntüleyebilirsiniz `azure vm list`

   > [!NOTE]
   > Azure portalını kullanıyorsanız, VM seçin ve'ı tıklatın **durdurmak** VM kapatma için.

8. VM durdurulduğunda görüntüyü yakalayın. Aşağıdaki örnek adlı VM yakalar `myVM` ve adlı genelleştirilmiş bir yansıma oluşturur `myNewVM`:

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    `-t` Alt özgün sanal makine siler.

    > [!NOTE]
    > Azure portalında seçerek bir görüntüsünü yakalayabilirsiniz **görüntü** hub menüsünde. Görüntü için aşağıdaki bilgileri sağlamanız gerekir: adı, kaynak grubu, konum, işletim sistemi türü ve depolama blob yolu.

9. Yeni görüntüyü yeni bir VM yapılandırmak için kullanılan görüntü listesinde kullanıma sunulmuştur. Komutuyla görüntüleyebilirsiniz:

   ```azurecli
   azure vm image list
   ```

   Üzerinde [Azure portal](http://portal.azure.com), yeni görüntüyü görünür **VM görüntüleri (Klasik)** ait olan **işlem** Hizmetleri. Erişebileceğiniz **VM görüntüleri (Klasik)** tıklayarak **tüm hizmetleri** Azure üstünde hizmet listesi ve ardından arayarak **işlem** Hizmetleri.   

   ![Görüntü yakalama başarılı](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Sonraki adımlar
Görüntü VM'ler oluşturmak için kullanılmak üzere hazırdır. Azure CLI komutunu kullanabilirsiniz `azure vm create` ve oluşturduğunuz görüntü adı sağlayın. Daha fazla bilgi için bkz: [Klasik dağıtım modeli ile Azure CLI kullanarak](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

Alternatif olarak, kullanın [Azure portal](http://portal.azure.com) kullanarak özel bir VM oluşturmak için **görüntü** yöntemi ve oluşturduğunuz görüntüsü seçin. Daha fazla bilgi için bkz: [özel VM oluşturma][How to Create a Custom Virtual Machine].

**Ayrıca bkz:** [Azure Linux Aracısı Kullanıcı Kılavuzu](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How to Create a Custom Virtual Machine]:create-custom-classic.md
[How to Attach a Data Disk to a Virtual Machine]:attach-disk-classic.md
[How to Create a Linux Virtual Machine]:create-custom-classic.md
