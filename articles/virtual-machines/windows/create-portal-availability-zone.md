---
title: Azure portal ile ayrılmış bir Windows VM oluşturma | Microsoft Docs
description: Azure portalı ile bir kullanılabilirlik alanında Windows VM oluşturma
services: virtual-machines-windows
documentationcenter: virtual-machines
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/27/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: e813b26a91d25fbaa1298acd455f27d2cac705f6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61403697"
---
# <a name="create-a-windows-virtual-machine-in-an-availability-zone-with-the-azure-portal"></a>Azure portalı ile bir kullanılabilirlik alanında Windows sanal makine oluşturma

Bir Azure kullanılabilirlik alanı'nda bir sanal makine oluşturmak için Azure portalını kullanarak aracılığıyla bu makalede adımları. [Kullanılabilirlik alanı](../../availability-zones/az-overview.md), bir Azure bölgesinde fiziksel olarak ayrılmış bir alandır. Uygulamalarınızı beklenmeyen hatalardan veya tüm veri merkezinin kaybedilmesinden korumak için kullanılabilirlik alanlarından yararlanın.

Kullanılabilirlik alanı kullanmak için, [desteklenen bir Azure bölgesinde](../../availability-zones/az-overview.md#services-support-by-region) sanal makinenizi oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 

[https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 

3. Sanal makine bilgilerini girin. Burada girilen kullanıcı adı ve parola, sanal makinede oturum açarken kullanılır. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır. Kullanılabilirlik alanlarını destekleyen Doğu ABD 2'gibi bir konum seçin. İşlem tamamlandığında **Tamam**’a tıklayın.

    ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/create-portal-availability-zone/create-windows-vm-portal-basic-blade.png)

4. VM için bir boyut seçin. Önerilen bir boyut seçin veya filtre özelliklerini temel alarak. Kullanmak istediğiniz bölgede boyutu kullanılabilir olduğunu onaylayın.

    ![Bir VM boyutu seçin](./media/create-portal-availability-zone/create-windows-vm-portal-sizes.png)  

5. Altında **ayarları** > **yüksek kullanılabilirlik**, numaralandırılmış bölgelerinden birini **kullanılabilirlik alanı** açılır listesinde, geri kalan varsayılan ayarları tutun ve tıklayın **Tamam**.

    ![Bir kullanılabilirlik bölgesi seçin](./media/create-portal-availability-zone/create-windows-vm-portal-availability-zone.png)

6. Özet sayfasında, tıklayın **Oluştur** sanal makine dağıtımını başlatın.

7. VM, Azure portalı panosuna sabitlenir. Dağıtım tamamlandıktan sonra VM özeti otomatik olarak açılır.

## <a name="confirm-zone-for-managed-disk-and-ip-address"></a>Yönetilen disk ve IP adresi için bölge onaylayın

VM için yönetilen disk, sanal Makineyi bir kullanılabilirlik alanında dağıtıldığında, aynı kullanılabilirlik alanında oluşturulur. Varsayılan olarak, bir genel IP adresi de bu bölgede oluşturulur.

Portalda bu kaynaklar için bölge ayarlarını doğrulayabilirsiniz.  

1. Tıklayın **kaynak grupları** ve sonra kaynak grubu adı VM için gibi *myResourceGroup*.

2. Disk kaynağı adına tıklayın. **Genel bakış** sayfa kaynak konumu ve kullanılabilirlik bölgesi hakkındaki ayrıntıları içerir.

    ![Yönetilen disk için kullanılabilirlik alanı](./media/create-portal-availability-zone/create-windows-vm-portal-disk.png)

3. Genel IP adresi kaynağın adına tıklayın. **Genel bakış** sayfa kaynak konumu ve kullanılabilirlik bölgesi hakkındaki ayrıntıları içerir.

    ![IP adresi kullanılabilirlik alanı](./media/create-portal-availability-zone/create-windows-vm-portal-ip.png)



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kullanılabilirlik alanında nasıl sanal makine oluşturulacağını öğrendiniz. Azure VM’leri için [bölgeler ve kullanılabilirlik](regions-and-availability.md) hakkında daha fazla bilgi edinin.
