---
title: Bir genel yük dengeleyiciye standart Azure portalını kullanarak bölge olarak yedekli genel IP adresi ön uç ile oluşturma | Microsoft Docs
description: Azure portal ile bölge olarak yedekli genel IP adresi ön uç ile bir genel yük dengeleyiciye standart oluşturmayı öğrenin
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: 9a51638ea6d85178e6631ac278c116e4c7e05d61
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34200267"
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-azure-portal"></a>Bir genel yük dengeleyiciye standart Azure portalını kullanarak bölge olarak yedekli genel IP adresi ön uç ile oluşturma

Bu makalede adımları genel oluşturmada size [yük dengeleyici standart](https://aka.ms/azureloadbalancerstandard) ortak IP standart bir adresi kullanarak bir bölge olarak yedekli ön ile. Standart bir yük dengeleyici üzerindeki bir tek bir ön uç IP adresi bölge-varsayılan olarak gereksizdir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
 Kullanılabilirlik bölgeler için destek, select Azure kaynaklarını ve bölgeler ve VM boyutu aileleri için kullanılabilir. Başlamak hakkında daha fazla bilgi ve hangi Azure kaynaklarını, bölgeler ve kullanılabilirlik bölgeleri deneyebilirsiniz VM boyutu aileleri için bkz: [kullanılabilirlik bölgeleri genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-zone-redundant-load-balancer"></a>Bölge olarak yedekli yük dengeleyici oluşturma

1. Bir tarayıcı üzerinden Azure portalına gidin: [ http://portal.azure.com ](http://portal.azure.com) ve Azure hesabınızda oturum açın.
2. Ekranın sol üst tarafında seçin **kaynak oluşturma** > **ağ** > **yük dengeleyici.**
3. İçinde **oluşturma yük dengeleyici** sayfasında **adı** türü **myLoadBalancer**.
4. **Tür** bölümünde **Genel**’i seçin.
5. SKU altında seçin **standart**.
6. Tıklatın **genel IP adresi**, tıklatın **Yeni Oluştur**ve **ortak IP adresi oluştur** sayfasında adı, türü altında **myPublicIPStandard**.
    >[!NOTE] 
    > Ortak IP Bu adımda oluşturduğunuz standart SKU'su ve bölge varsayılan olarak yedekli. 
8. Altında **konumu**seçin **Doğu US2**ve ardından **Tamam**. Yük dengeleyici dağıtımı başlar ve birkaç dakika içinde başarıyla tamamlanır.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [standart yük dengeleyici ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md).



