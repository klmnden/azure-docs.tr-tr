---
title: Bir statik genel IP adresiyle - Azure portalında bir VM oluşturma | Microsoft Docs
description: Azure portalını kullanarak statik genel IP adresiyle VM oluşturma konusunda bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/08/2018
ms.author: kumud
ms.openlocfilehash: f6914a9894db07a40b372a8c247a7623c3957d86
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64692418"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-the-azure-portal"></a>Azure portalını kullanarak statik genel IP adresiyle bir sanal makine oluşturun

Bir statik genel IP adresiyle bir sanal makine oluşturabilirsiniz. Genel bir IP adresi bir sanal makineye internet üzerinden iletişim kurmasına olanak tanır. Adresi hiçbir zaman değiştirdiğinden emin olmak için bir dinamik adres yerine bir statik genel IP adresi atayın. Daha fazla bilgi edinin [statik genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#allocation-method). Varolan bir sanal makineye gelen dinamik statik olarak atanmış bir genel IP adresini değiştirmek için veya özel IP adresleri ile çalışmak için bkz: [ekleme, değiştirme veya kaldırma IP adresleri](virtual-network-network-interface-addresses.md). Genel IP adreslerine sahip bir [nominal bir ücret](https://azure.microsoft.com/pricing/details/ip-addresses)İşte bir [sınırı](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) için abonelik başına kullanabileceğiniz ortak IP adresi sayısı.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. Seçin **işlem**ve ardından **Windows Server 2016 VM**, veya seçtiğiniz başka bir işletim sistemi.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    |Ayar|Değer|
    |---|---|
    |Ad|myVM|
    |Kullanıcı adı| Seçtiğiniz bir kullanıcı adını girin.|
    |Parola| Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Abonelik| Aboneliğinizi seçin.|
    |Kaynak grubu| **Mevcut olanı kullan**’ı seçin ve **myResourceGroup** seçeneğini belirleyin.|
    |Location| **Doğu ABD**’yi seçin|

4. Sanal makine için bir boyut seçin ve **Seç** seçeneğini belirleyin.
5. Altında **ayarları**seçin **genel IP adresi**.
6. Girin *Mypublicıpaddress*seçin **statik**ve ardından **Tamam**, aşağıdaki resimde gösterildiği gibi:

   ![Statik seçin](./media/virtual-network-deploy-static-pip-arm-portal/select-static.png)

   Standart SKU genel IP adresi olması gerekiyorsa seçin **standart** altında **SKU**. Daha fazla bilgi edinin [SKU genel IP adresi](virtual-network-ip-addresses-overview-arm.md#sku). Sanal makine genel bir Azure Load Balancer arka uç havuzuna eklenecek, sanal makinenin genel IP adresinin SKU yük dengeleyicinin genel IP adresinin SKU eşleşmesi gerekir. Ayrıntılar için bkz [Azure Load Balancer](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#skus).

6. Bir bağlantı noktası veya bağlantı noktası altında seçin **seçin ortak gelen bağlantı noktası**. İnternet'ten Windows Server sanal makineye uzaktan erişimi etkinleştirmek için Portal 3389 seçilir. İnternetten 3389 numaralı bağlantı noktasını açma üretim iş yükleri için önerilmez.

   ![Bir bağlantı noktası seçin](./media/virtual-network-deploy-static-pip-arm-portal/select-port.png)

7. Geri kalan varsayılan ayarları kabul edin ve seçin **Tamam**.
8. **Özet** sayfasında **Oluştur**'u seçin. Sanal makinenin dağıtılması birkaç dakika sürer.
9. Sanal makine dağıtıldıktan sonra girin *Mypublicıpaddress* portalın üst kısmındaki arama kutusuna. Zaman **Mypublicıpaddress** arama sonuçlarında görünür.
10. Atanan ve adresi atanan genel IP adresini görüntüleyebilirsiniz **myVM** sanal makine, aşağıdaki resimde gösterildiği gibi:

    ![Görünüm genel IP adresi](./media/virtual-network-deploy-static-pip-arm-portal/public-ip-overview.png)

    Azure, sanal makineyi oluşturduğunuz bölge içinde kullanılan adreslerinden genel bir IP adresi atanır. Azure [Genel](https://www.microsoft.com/download/details.aspx?id=56519), [US government](https://www.microsoft.com/download/details.aspx?id=57063), [Çin](https://www.microsoft.com/download/details.aspx?id=57062) ve [Almanya](https://www.microsoft.com/download/details.aspx?id=57064) bulutları için bu aralıkların (ön ekler) listesini indirebilirsiniz.

11. Seçin **yapılandırma** ataması olduğunu onaylamak için **statik**.

    ![Görünüm genel IP adresi](./media/virtual-network-deploy-static-pip-arm-portal/public-ip-configuration.png)

> [!WARNING]
> Sanal makinenin işletim sistemi içinde IP adresi ayarlarını değiştirmeyin. İşletim sistemi Azure genel IP adreslerini farkında değil. Özel IP adresi ayarları işletim sistemine ekleyebilirsiniz ancak sürece bunu öneririz gerektiği kadar edindikten sonra değil [bir işletim sistemine özel bir IP adresi Ekle](virtual-network-network-interface-addresses.md#private).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki **Ara** kutusuna *myResourceGroup* değerini girin. Arama sonuçlarında **myResourceGroup** seçeneğini gördüğünüzde bunu seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** için *myResourceGroup* girin ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) azure'da
- Tüm hakkında daha fazla bilgi [genel IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address)
- Daha fazla bilgi edinin [özel IP adresleri](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) ve atama bir [statik özel IP adresi](virtual-network-network-interface-addresses.md#add-ip-addresses) bir Azure sanal makinesi için
- Oluşturma hakkında daha fazla bilgi edinin [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makineler