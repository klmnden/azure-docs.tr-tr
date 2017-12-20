---
title: "Bir sanal ağ geçidini silmek: Azure portal: Resource Manager | Microsoft Docs"
description: "Resource Manager dağıtım modelinde Azure portalını kullanarak bir sanal ağ geçidini silin."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: cherylmc
ms.openlocfilehash: b67fdfc82bbc132772186e3500079cfcfdafe02b
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="delete-a-virtual-network-gateway-using-the-portal"></a>Portalı kullanarak bir sanal ağ geçidini Sil

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-delete-vnet-gateway-portal.md)
> * [PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [PowerShell (klasik)](vpn-gateway-delete-vnet-gateway-classic-powershell.md)

Bu makalede Resource Manager dağıtım modeli kullanılarak dağıtılan bir Azure VPN ağ geçitleri silme yönergelerini sağlar. Birkaç VPN ağ geçidi yapılandırması için bir sanal ağ geçidi silmek istediğinizde uygulayabileceğiniz çeşitli yaklaşımlar vardır.

- Her şeyi silin ve üzerinde bir test ortamı durumda olduğu gibi başlatmak istiyorsanız, kaynak grubunu silebilirsiniz. Bir kaynak grubunu silmek gruptaki tüm kaynakları siler. Bu yöntem olduğundan tüm kaynakların kaynak grubunda saklamak istemiyorsanız yalnızca önerilir. Bu yaklaşımı kullanarak yalnızca birkaç kaynakları seçmeli olarak silemezsiniz.

- Bazı kaynakların kaynak grubunda tutmak istiyorsanız, bir sanal ağ geçidi silinmesi biraz daha karmaşık olabilir. Sanal ağ geçidi silmeden önce ağ geçidinde bağımlı herhangi bir kaynağa silmeniz gerekir. İzleyeceğiniz adımlar oluşturduğunuz bağlantı türünü ve her bağlantı için bağımlı kaynakları bağlıdır.

> [!IMPORTANT]
> Aşağıdaki yönergeleri nasıl Resource Manager dağıtım modeli kullanılarak dağıtılan Azure VPN ağ geçitleri silineceğini açıklar. Klasik dağıtım modeli kullanılarak dağıtılan bir VPN ağ geçidi silmek için lütfen Azure PowerShell açıklandığı gibi kullanın [burada](vpn-gateway-delete-vnet-gateway-classic-powershell.md).


## <a name="delete-a-vpn-gateway"></a>VPN ağ geçidi silme

Bir sanal ağ geçidi silmek için önce sanal ağ geçidi için ilgili her bir kaynağın silmeniz gerekir. Belirli bir sırada bağımlılıkları nedeniyle kaynakları silinmesi gerekir.

[!INCLUDE [delete gateway](../../includes/vpn-gateway-delete-vnet-gateway-portal-include.md)]

Bu noktada, sanal ağ geçidi silinir. Sonraki adımlar artık kullanılmakta olan tüm kaynakları silmesini yardımcı olur.

### <a name="to-delete-the-local-network-gateway"></a>Yerel ağ geçidi silmek için

1. İçinde **tüm kaynakları**, her bağlantı ile ilişkili yerel ağ geçitleri bulun.
2. Üzerinde **genel bakış** yerel ağ geçidi için dikey tıklayın **silmek**.

### <a name="to-delete-the-public-ip-address-resource-for-the-gateway"></a>Ağ geçidi için genel IP adresi kaynağı silmek için

1. İçinde **tüm kaynakları**, ağ geçidi için ilişkili ortak IP adresi kaynağı bulun. Sanal ağ geçidi etkin-etkin iki genel IP adresleri görürsünüz. 
2. Üzerinde **genel bakış** sayfasında için genel IP adresi, **silmek**, ardından **Evet** onaylamak için.

### <a name="to-delete-the-gateway-subnet"></a>Ağ geçidi alt ağını silmek için

1. İçinde **tüm kaynakları**, sanal ağ bulun. 
2. Üzerinde **alt ağlar** dikey penceresinde tıklatın **GatewaySubnet**, ardından **silmek**. 
3. Tıklatın **Evet** ağ geçidi alt ağı silmek istediğinizi onaylamak için.

## <a name="deleterg"></a>Kaynak grubunu silerek bir VPN ağ geçidini Sil

Kaynak grubunda kaynaklarınızın hiçbirini saklanması ilgili değildir ve baştan başlamak istiyorsanız, tüm kaynak grubunu silebilirsiniz. Bu, her şeyi kaldırmak için hızlı bir yoludur. Aşağıdaki adımlar yalnızca Resource Manager dağıtım modeli için geçerlidir.

1. İçinde **tüm kaynakları**, kaynak grubunu bulun ve dikey penceresini açmak için tıklatın.
2. **Sil**'e tıklayın. Delete dikey penceresinde, etkilenen kaynakları görüntüleyin. Tüm bu kaynaklar silmek istediğinizden emin olun. Aksi takdirde, içindeki adımları kullanın [bir VPN ağ geçidi silmek](#deletegw) bu makalenin üst.
3. Devam etmek için silin ve ardından istediğiniz kaynak grubunun adını yazın **silmek**.
