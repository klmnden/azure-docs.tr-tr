---
title: Bir Linux VM görüntüsü yakalama | Microsoft Docs
description: Bir Linux tabanlı Azure Klasik dağıtım modeliyle oluşturulan sanal makinenin (VM) bir resim yakalayabilir öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
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
ms.author: cynthn
ms.openlocfilehash: ae87af45ffc442c0de6c7f703694a994e536cdb8
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929221"
---
# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Klasik bir Linux sanal makinesini görüntü olarak yakalama
> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. [Bu adımları Resource Manager modeli kullanarak gerçekleştirmeyi](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) öğrenin.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Bu makalede, bir Klasik Azure Linux diğer sanal makineler oluşturmak için bir görüntü olarak çalıştıran sanal makine (VM) yakalama işlemini göstermektedir. Bu görüntü, işletim sistemi diski ve VM'ye veri diski içerir. Görüntüyü başka bir VM oluşturduğunuzda, yapılandırmanız gereken şekilde ağ yapılandırmasına dahil değildir.

Azure görüntü altında depolar **görüntüleri**, karşıya yüklediğiniz herhangi bir görüntü yanı sıra. Görüntüleri hakkında daha fazla bilgi için bkz. [azure'da sanal makine görüntüleri hakkında][About Virtual Machine Images in Azure].

## <a name="before-you-begin"></a>Başlamadan önce
Bu adımlarda, Klasik dağıtım modelini kullanarak Azure VM oluşturulur ve veri diskleri ekleme dahil olmak üzere işletim sistemi, yapılandırılmış varsayılır. Bir sanal makine oluşturmanız gerekiyorsa, okuma [Linux sanal makinesi oluşturma][How to Create a Linux Virtual Machine].

## <a name="capture-the-virtual-machine"></a>Sanal makine yakalanamadı
1. [VM'ye bağlanın](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tercih ettiğiniz bir SSH istemcisi kullanarak.
2. SSH penceresinde aşağıdaki komutu yazın. Çıkışı `waagent` bu yardımcı program sürümüne bağlı olarak biraz değişebilir:

    ```bash
    sudo waagent -deprovision+user
    ```

    Yukarıdaki komut, sistem temizleyin ve çıkış için uygun hale getiren dener. Bu işlem, aşağıdaki görevleri gerçekleştirir:

   * SSH ana bilgisayar anahtarları (Provisioning.regeneratesshhostkeypair değerini 'y' yapılandırma dosyası ise) kaldırır
   * /Etc/resolv.conf ad sunucusu yapılandırmasını temizler
   * Kaldırır `root` (Provisioning.deleterootpassword değerini 'y' yapılandırma dosyasında ise) / etc/shadow kullanıcı parolasından
   * Önbelleğe alınan istemci kiralamalarını kaldırır
   * Ana bilgisayar adını localhost.localdomain adına sıfırlar
   * Siler son sağlanan kullanıcı hesabı (/var/lib/waagent alınan) **ve ilişkili verileri**.

     > [!NOTE]
     > Sağlamayı kaldırma, dosyaları ve "görüntüyü Genelleştirme için" verileri siler. Yalnızca bir VM'de yeni görüntüyü şablon olarak yakalamak için istediğinize şu komutu çalıştırın. Görüntü tüm önemli bilgileri temizlenir veya üçüncü taraflara yeniden dağıtım için uygun olan garanti etmez.

3. Tür **y** devam etmek için. Ekleyebileceğiniz `-force` bu doğrulama adımı önlemek için parametre.
4. Tür **çıkış** SSH İstemcisi'ni kapatın.

   > [!NOTE]
   > Kalan adımları varsayılmaktadır [Azure CLI'nin yüklü](../../../cli-install-nodejs.md) istemci bilgisayarınızda. Aşağıdaki adımların tümü de yapılabilir [Azure portalında](http://portal.azure.com).

5. İstemci bilgisayarınızdan Azure CLI ve Azure aboneliğinizde oturum açın. Ayrıntılı bilgi edinmek için [Azure clı'dan Azure aboneliğine Bağlan](/cli/azure/authenticate-azure-cli).

   > [!NOTE]
   > Portalda Azure portalında oturum açın.

6. Hizmet Yönetimi modunda olduğunuzdan emin olun:

    ```azurecli
    azure config mode asm
    ```

7. Zaten sağlaması, VM'yi kapatın. Aşağıdaki örnekte adlı VM'yi kapatır `myVM`:

    ```azurecli
    azure vm shutdown myVM
    ```
   Gerekirse, aboneliğinizde kullanılarak oluşturulan tüm sanal makinelerin listesini görüntüleyebilirsiniz. `azure vm list`

   > [!NOTE]
   > Azure portalını kullanıyorsanız, sanal Makineyi seçin ve tıklayın **Durdur** sanal makineyi.

8. VM durdurulduğunda görüntüyü yakalayın. Aşağıdaki örnekte adlı VM yakalar `myVM` ve adlı genelleştirilmiş bir görüntü oluşturur `myNewVM`:

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    `-t` Alt özgün sanal makineyi siler.

    > [!NOTE]
    > Azure portalında seçerek bir resim yakalayabilir **görüntü** hub menüsünde. Görüntü için aşağıdaki bilgileri sağlamanız gerekir: ad, kaynak grubu, konum, işletim sistemi türü ve depolama blob yolu.

9. Yeni görüntü artık yeni bir VM yapılandırmak için kullanılan görüntüleri listesinde kullanılabilir. Komutu ile görüntüleyebilirsiniz:

   ```azurecli
   azure vm image list
   ```

   Üzerinde [Azure portalında](http://portal.azure.com), yeni görüntüyü görünür **VM görüntüleri (Klasik)** ait olan **işlem** Hizmetleri. Erişebildiğiniz **VM görüntüleri (Klasik)** tıklayarak **tüm hizmetleri** Azure üstüne hizmet listesini ve ardından arama **işlem** Hizmetleri.   

   ![Görüntü yakalama başarılı](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Sonraki adımlar
Sanal makineler oluşturmak için kullanılacak görüntüyü hazırdır. Azure CLI komutunu kullanabilirsiniz `azure vm create` ve oluşturduğunuz görüntü adı sağlayın. Daha fazla bilgi için [Azure CLI ile klasik dağıtım modelini kullanarak](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

Alternatif olarak, [Azure portalında](http://portal.azure.com) kullanarak özel bir VM oluşturmak için **görüntü** yöntemi ve oluşturduğunuz görüntüsü seçin. Daha fazla bilgi için [özel bir VM oluşturmak nasıl][How to Create a Custom Virtual Machine].

**Ayrıca bkz:** [Azure Linux Aracısı Kullanım Kılavuzu](../../extensions/agent-linux.md)

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How to Create a Custom Virtual Machine]:create-custom-classic.md
[How to Attach a Data Disk to a Virtual Machine]:attach-disk-classic.md
[How to Create a Linux Virtual Machine]:create-custom-classic.md
