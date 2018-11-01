---
title: Azure portalını kullanarak bölgesel olarak yedekli genel IP adresi ön uç ile bir genel Load Balancer Standard oluşturma | Microsoft Docs
description: Azure portalı ile bölgesel olarak yedekli genel IP adresi ön uç ile bir genel Load Balancer Standard oluşturma konusunda bilgi edinin
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: 70514433d11bbe7606d75a3e2c1f6dffc251621f
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50740952"
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-azure-portal"></a>Azure portalını kullanarak bölgesel olarak yedekli genel IP adresi ön uç ile bir genel Load Balancer Standard oluşturma

Bu makalede adımları genel oluşturma işleminde [Load Balancer Standard](https://aka.ms/azureloadbalancerstandard) genel IP standart bir adres kullanarak bölgesel olarak yedekli bir ön uç ile. Bölgesel olarak yedekli varsayılan olarak standart yük dengeleyici üzerindeki bir tek bir ön uç IP adresi.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
 Kullanılabilirlik bölgeleri, seçili Azure kaynakları ve bölgeler ve sanal makine boyutu aileleri için kullanılabilir. Kullanmaya başlamak nasıl daha fazla bilgi ve hangi Azure kaynakları, bölgeleri ve kullanılabilirlik alanları ile deneyebilirsiniz sanal makine boyutu aileleri için bkz. [kullanılabilirlik alanlarına genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-zone-redundant-load-balancer"></a>Bölge yedekli yük dengeleyici oluşturma

1. Bir tarayıcıdan Azure portalına gidin: [ http://portal.azure.com ](http://portal.azure.com) ve Azure hesabınızla oturum açın.
2. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **ağ** > **yük dengeleyici.**
3. İçinde **yük dengeleyici Oluştur** sayfasındaki **adı** türü **myLoadBalancer**.
4. **Tür** bölümünde **Genel**’i seçin.
5. SKU altında seçin **standart**.
6. Tıklayın **genel IP adresi**, tıklayın **Yeni Oluştur**hem de **genel IP adresi oluşturma** sayfasında, adı, türü altında **myPublicIPStandard**.
    >[!NOTE] 
    > Genel IP Bu adımda oluşturduğunuz standart SKU'su ve bölgesel olarak yedekli varsayılan olarak. 
8. Altında **konumu**seçin **Doğu ABD 2**ve ardından **Tamam**. Yük dengeleyici dağıtımı başlar ve birkaç dakika içinde başarıyla tamamlanır.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Standard Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md).



