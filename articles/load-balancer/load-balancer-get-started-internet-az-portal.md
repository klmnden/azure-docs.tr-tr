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
ms.openlocfilehash: 10a264609469245d4743886b58730304da3df7bb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-azure-portal"></a>Bir genel yük dengeleyiciye standart Azure portalını kullanarak bölge olarak yedekli genel IP adresi ön uç ile oluşturma

Bu makalede adımları genel oluşturmada size [yük dengeleyici standart](https://aka.ms/azureloadbalancerstandard) ortak IP standart bir adresi kullanarak bir bölge olarak yedekli ön ile.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="register-for-availability-zones-preview"></a>Kullanılabilirlik bölgeleri Önizleme için kaydolun
 
Kullanılabilirlik bölgeleri önizlemede ve geliştirme için hazır olduğunu ve test senaryoları. Destek select Azure kaynaklarını ve bölgeler ve VM boyutu aileleri için kullanılabilir. Başlamak hakkında daha fazla bilgi ve hangi Azure kaynaklarını, bölgeler ve kullanılabilirlik bölgeleri deneyebilirsiniz VM boyutu aileleri için bkz: [kullanılabilirlik bölgeleri genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

Oturum açtığınızda Azure portalında https://portal.azure.com.

## <a name="create-a-zone-redundant-load-balancer"></a>Bölge olarak yedekli yük dengeleyici oluşturma

1. Bir tarayıcı üzerinden Azure portalına gidin: [ http://portal.azure.com ](http://portal.azure.com) ve Azure hesabınızda oturum açın.
2. Ekranın sol üst tarafında seçin **kaynak oluşturma** > **ağ** > **yük dengeleyici.**
3. İçinde ** oluşturma yük dengeleyici, altında **adı** türü **myPublicLB**.
4. **Tür** bölümünde **Genel**’i seçin.
5. SKU altında seçin **standart (Önizleme)**.
6. ' I tıklatın **genel IP adresi**, tıklatın **Yeni Oluştur**, **genel bir IP adresi oluşturma** sayfasında adı, türü altında **myPublicIPStandard**, ve için **kullanılabilirlik bölge (Önizleme)**seçin **bölge olarak yedekli**.
7. Altında **konumu**seçin **Doğu US2**ve ardından **Tamam**. Yük dengeleyici dağıtımı başlar ve birkaç dakika içinde başarıyla tamamlanır.

    ![bölge olarak yedekli yük dengeleyici standart Azure portalıyla oluşturma](./media/load-balancer-get-started-internet-az-portal/create-zone-redundant-load-balancer-standard.png)


## <a name="next-steps"></a>Sonraki adımlar
- Bilgi nasıl [bir kullanılabilirlik bölgesinde bir genel IP oluşturun](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)



