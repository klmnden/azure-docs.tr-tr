---
title: Bölgesel bir ön uç IP adresi - Azure portalı ile bir Standard Load Balancer oluşturma
titlesuffix: Azure Load Balancer
description: Azure portalı ile bölgesel genel IP adresi ön uç ile bir genel Load Balancer Standard oluşturma konusunda bilgi edinin
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: kumud
ms.openlocfilehash: e109504fe8657436d73870cc022ed4bc81c559f5
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53095345"
---
#  <a name="create-a-public-load-balancer-standard-with-zonal-public-ip-address-frontend-using-azure-portal"></a>Azure portalını kullanarak bölgesel genel IP adresi ön uç ile bir genel Load Balancer Standard oluşturma

Bu makalede adımları genel oluşturma işleminde [Load Balancer Standard](https://aka.ms/azureloadbalancerstandard) bölgesel bir ön uç ile. Kullanılabilirlik alanları standart Load Balancer ile nasıl çalıştığını anlamak için bkz: [Standard Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md). 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> Kullanılabilirlik bölgeleri, seçili Azure kaynakları ve bölgeler ve sanal makine boyutu aileleri için kullanılabilir. Kullanmaya başlamak nasıl daha fazla bilgi ve hangi Azure kaynakları, bölgeleri ve kullanılabilirlik alanları ile deneyebilirsiniz sanal makine boyutu aileleri için bkz. [kullanılabilirlik alanlarına genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-load-balancer-with-zonal-frontend-ip-address"></a>Bölgesel bir ön uç IP adresine sahip yük dengeleyici oluşturma

1. Bir tarayıcıdan Azure portalına gidin: [ http://portal.azure.com ](http://portal.azure.com) ve Azure hesabınızla oturum açın.
2. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **ağ** > **yük dengeleyici.**
3. İçinde **yük dengeleyici Oluştur** sayfasındaki **adı** türü **myLoadBalancer**.
4. **Tür** bölümünde **Genel**’i seçin.
5. SKU altında seçin **standart**.
6. Tıklayın **bir genel IP adresi seçin**, tıklayın **Yeni Oluştur**hem de **genel IP adresi oluşturma** sayfasında, adı, türü altında **myPublicIPZonal**, SKU için seçin **standart**, kullanılabilirlik bölgesi için seçin **1**.
    
>[!NOTE] 
> Genel IP Bu adımda oluşturduğunuz standart SKU'nun varsayılan olarak açıktır.

7. İçin **kaynak grubu**, tıklayın **Yeni Oluştur**, Anahtar'a tıklayın ve **myResourceGroupZLB** kaynak grubunun adı.
8. İçin **konumu**seçin **Batı Avrupa**ve ardından **Tamam**. Yük dengeleyici dağıtımı başlar ve birkaç dakika içinde başarıyla tamamlanır.

    ![Azure portalı ile bölgesel olarak yedekli Load Balancer Standard oluşturma](./media/load-balancer-get-started-internet-availability-zones-zonal-portal/load-balancer-zonal-frontend.png)


## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Standard Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md).



