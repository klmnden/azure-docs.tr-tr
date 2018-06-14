---
title: Azure portalıyla zoned Windows VM oluşturma | Microsoft Docs
description: Azure portal ile bir kullanılabilirlik bölgesindeki bir Windows VM oluşturma
services: virtual-machines-windows
documentationcenter: virtual-machines
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/27/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: 3d3561cf1ad760930821fabeef9839c25d55f2a9
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30321997"
---
# <a name="create-a-windows-virtual-machine-in-an-availability-zone-with-the-azure-portal"></a>Azure portal ile bir kullanılabilirlik bölgesindeki bir Windows sanal makine oluşturma

Bir Azure kullanılabilirlik bölgesinde bir sanal makine oluşturmak için Azure portalını kullanarak aracılığıyla bu makalede adımlar. [Kullanılabilirlik alanı](../../availability-zones/az-overview.md), bir Azure bölgesinde fiziksel olarak ayrılmış bir alandır. Uygulamalarınızı beklenmeyen hatalardan veya tüm veri merkezinin kaybedilmesinden korumak için kullanılabilirlik alanlarından yararlanın.

Bir kullanılabilirlik bölgeyi kullanmak için sanal makinenizde oluşturmanız bir [Azure bölgesi desteklenen](../../availability-zones/az-overview.md#regions-that-support-availability-zones).

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 

3. Sanal makine bilgilerini girin. Burada girilen kullanıcı adı ve parola, sanal makinede oturum açarken kullanılır. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır. Kullanılabilirlik bölgeyi destekleyen Doğu ABD 2 gibi bir konum seçin. İşlem tamamlandığında **Tamam**’a tıklayın.

    ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/create-portal-availability-zone/create-windows-vm-portal-basic-blade.png)

4. VM için bir boyut seçin. Filtre özelliklerini temel alarak veya önerilen boyutu seçin. Kullanmak istediğiniz bölgede boyutu kullanılabilir olduğunu onaylayın.

    ![VM boyutunu seçin](./media/create-portal-availability-zone/create-windows-vm-portal-sizes.png)  

5. Altında **ayarları** > **yüksek kullanılabilirlik**, numaralandırılmış bölgelerinden birini seçin **kullanılabilirlik bölge** açılan listesinde, kalan Varsayılanları tutun ve tıklatın **Tamam**.

    ![Bir kullanılabilirlik bölgeyi seçin](./media/create-portal-availability-zone/create-windows-vm-portal-availability-zone.png)

6. Özet sayfasında, tıklatın **oluşturma** sanal makine dağıtımı başlatmak için.

7. VM, Azure portalı panosuna sabitlenir. Dağıtım tamamlandıktan sonra VM özeti otomatik olarak açılır.

## <a name="confirm-zone-for-managed-disk-and-ip-address"></a>Yönetilen disk ve IP adresi için bölge onaylayın

VM bir kullanılabilirlik bölgesinde dağıtıldığında, yönetilen bir disk VM için aynı kullanılabilirlik bölgede oluşturulur. Varsayılan olarak, bir ortak IP adresi da bu bölgede oluşturulur.

Bu kaynaklar Portalı'nda Bölge ayarlarını onaylayabilirsiniz.  

1. Tıklatın **kaynak grupları** ve ardından kaynak grubu adı VM için gibi *myResourceGroup*.

2. Disk kaynak adına tıklayın. **Genel bakış** sayfası kaynağının konumu ve kullanılabilirlik bölge ayrıntılarını içerir.

    ![Yönetilen disk kullanılabilirlik bölgenin](./media/create-portal-availability-zone/create-windows-vm-portal-disk.png)

3. Genel IP adresi kaynak adına tıklayın. **Genel bakış** sayfası kaynağının konumu ve kullanılabilirlik bölge ayrıntılarını içerir.

    ![IP adresi için kullanılabilirlik bölge](./media/create-portal-availability-zone/create-windows-vm-portal-ip.png)



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir kullanılabilirlik bölgesinde bir VM oluşturma öğrendiniz. Azure VM’leri için [bölgeler ve kullanılabilirlik](regions-and-availability.md) hakkında daha fazla bilgi edinin.
