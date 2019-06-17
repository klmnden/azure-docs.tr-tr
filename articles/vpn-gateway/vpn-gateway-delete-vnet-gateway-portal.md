---
title: 'Bir sanal ağ geçidini silin: Azure portalı: Resource Manager | Microsoft Docs'
description: Azure portalını kullanarak Resource Manager dağıtım modelinde bir sanal ağ geçidini silin.
services: vpn-gateway
documentationcenter: na
author: cherylmc
ms.service: vpn-gateway
ms.date: 10/23/2018
ms.author: cherylmc
ms.topic: conceptual
ms.openlocfilehash: 387b4e982772f22453876e1ea8b9e7c4039601c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60845700"
---
# <a name="delete-a-virtual-network-gateway-using-the-portal"></a>Portalı kullanarak bir sanal ağ geçidini silme

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-delete-vnet-gateway-portal.md)
> * [PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [PowerShell (klasik)](vpn-gateway-delete-vnet-gateway-classic-powershell.md)

Bu makalede, Resource Manager dağıtım modeli kullanılarak dağıtılan Azure VPN ağ geçitleri silmeye yönelik yönergeleri sağlar. Birkaç farklı yaklaşımların bir sanal ağ geçidi bir VPN ağ geçidi yapılandırması için silmek istediğiniz zaman izleyebileceğiniz vardır.

- Her şeyi silin ve baştan, bir test ortamı durumunda olduğu gibi istediğiniz kaynak grubunu silebilirsiniz. Bir kaynak grubu sildiğinizde grup içindeki tüm kaynakları siler. Tüm kaynakların kaynak grubunda korumak istemiyorsanız, bu yöntem yalnızca önerilir. Seçime bağlı olarak, bu yaklaşımı kullanarak yalnızca birkaç kaynak silinemiyor.

- Kaynaklardan bazıları, kaynak grubunuzda tutmak istiyorsanız, bir sanal ağ geçidi siliniyor biraz daha karmaşık olabilir. Sanal ağ geçidi silebilmeniz için önce ağ geçidinde bağımlı olan tüm kaynakları silmeniz gerekir. Oluşturduğunuz bağlantı türünü ve her bağlantı için bağımlı kaynakları kullandığınızda izleyeceğiniz adımlar bağlıdır.

> [!IMPORTANT]
> Aşağıdaki yönergeler, Resource Manager dağıtım modeli kullanılarak dağıtılan Azure VPN ağ geçitlerini silme açıklanmaktadır. Klasik dağıtım modeli kullanılarak dağıtılan bir VPN ağ geçidini silmek için lütfen Azure PowerShell açıklandığı kullanın [burada](vpn-gateway-delete-vnet-gateway-classic-powershell.md).


## <a name="delete-a-vpn-gateway"></a>VPN ağ geçidi silme

Bir sanal ağ geçidini silmek için önce sanal ağ geçidi için ilgili her bir kaynak silmeniz gerekir. Belirli bir sırada bağımlılıklar nedeniyle kaynakları silinmesi gerekir.

[!INCLUDE [delete gateway](../../includes/vpn-gateway-delete-vnet-gateway-portal-include.md)]

Bu noktada, sanal ağ geçidi silinir. Sonraki adımlar artık kullanılmayan tüm kaynakları silmesini yardımcı olur.

### <a name="to-delete-the-local-network-gateway"></a>Yerel ağ geçidi silinemedi

1. İçinde **tüm kaynakları**, her bir bağlantı ile ilişkili yerel ağ geçitleri bulun.
2. Üzerinde **genel bakış** yerel ağ geçidi dikey penceresine tıklayın **Sil**.

### <a name="to-delete-the-public-ip-address-resource-for-the-gateway"></a>Ağ geçidi genel IP adresi kaynağı silmek için

1. İçinde **tüm kaynakları**, ağ geçidine ilişkili genel IP adresi kaynağı bulun. Etkin-etkin sanal ağ geçidi, iki genel IP adresleri görürsünüz. 
2. Üzerinde **genel bakış** sayfasında için genel IP adresi, **Sil**, ardından **Evet** onaylamak için.

### <a name="to-delete-the-gateway-subnet"></a>Ağ geçidi alt ağı silmek için

1. İçinde **tüm kaynakları**, sanal ağ'ı bulun. 
2. Üzerinde **alt ağlar** dikey penceresinde tıklayın **GatewaySubnet**, ardından **Sil**. 
3. Tıklayın **Evet** ağ geçidi alt ağını silmek istediğinizi onaylayın.

## <a name="deleterg"></a>Kaynak grubunu silerek bir VPN ağ geçidini silme

Kaynak grubunda kaynaklarınızın herhangi etme konusunda endişe değildir ve baştan başlamak istiyorsanız, bir kaynak grubunun tamamını silebilirsiniz. Bu, her şeyi kaldırmak için hızlı bir yoludur. Aşağıdaki adımlar, yalnızca Resource Manager dağıtım modeli için geçerlidir.

1. İçinde **tüm kaynakları**, kaynak grubunu bulun ve dikey penceresini açmak için tıklayın.
2. Tıklayın **Sil**. Silme dikey penceresinde, etkilenen kaynaklar'ı görüntüleyin. Tüm bu kaynaklar silmek istediğinizden emin olun. Aksi takdirde, silme adımları bu makalenin başında bir VPN ağ geçidi kullanın.
3. Devam etmek için silin ve ardından istediğiniz kaynak grubunun adını yazın. **Sil**.
