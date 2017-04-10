---
title: "Azure Portal ile Azure DNS’i kullanmaya başlama | Microsoft Docs"
description: "Azure DNS&quot;te DNS bölgesi ve kaydı oluşturma hakkında bilgi edinin. Bu kılavuzda, Azure portalı kullanarak ilk DNS bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım talimatlar sunulmaktadır."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: 79f0c9297c4be70f705f325274f3d9241ea4bc3f
ms.lasthandoff: 03/29/2017

---

# <a name="get-started-with-azure-dns-using-the-azure-portal"></a>Azure portal ile Azure DNS’i kullanmaya başlama

> [!div class="op_single_selector"]
> * [Azure portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Bu makalede, Azure portalı kullanarak ilk DNS bölgesi ve kaydınızı oluşturma adımları gösterilmektedir. Ayrıca, Azure PowerShell veya platformlar arası Azure CLI kullanarak aşağıdaki adımları gerçekleştirebilirsiniz.

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Son olarak, DNS bölgenizi Internet'te yayımlamak için etki alanının ad sunucularını yapılandırmanız gerekir. Bu adımların her biri aşağıda açıklanmıştır.

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Azure portalında oturum açın
2. Hub menüsünde **Yeni > Ağ >** ve ardından **DNS bölgesi**’ne tıklayarak DNS bölgesi oluştur dikey penceresini açın.

    ![DNS bölgesi](./media/dns-getstarted-portal/openzone650.png)

4. **DNS bölgesi oluştur** dikey penceresinde DNS bölgenizi adlandırın. Örneğin, *contoso.com*.
 
    ![Bölge oluşturma](./media/dns-getstarted-portal/newzone250.png)

5. Ardından, kullanmak istediğiniz kaynak grubunu belirtin. Yeni bir kaynak grubu oluşturabilir veya zaten var olan bir kaynak grubunu seçebilirsiniz. Yeni bir kaynak grubu oluşturmayı seçerseniz, kaynak grubunun konumunu belirtmek için **Konum** açılır menüsünü kullanın. Bu ayar kaynak grubunun konumunu ifade eder ve DNS bölgesini etkilemez. DNS bölgesinin konumu her zaman "genel" şeklindedir ve gösterilmez.

6. Yeni bölgenizi panoda kolayca bulmak istiyorsanız **Panoya sabitle** onay kutusunu işaretli bırakabilirsiniz. Sonra **Oluştur**’a tıklayın.

    ![Panoya sabitle](./media/dns-getstarted-portal/pindashboard150.png)

7. Oluştur’a tıkladıktan sonra yeni bölgenizin panoda yapılandırıldığını görürsünüz.

    ![Oluşturma](./media/dns-getstarted-portal/creating150.png)

8. Yeni bölgeniz oluşturulduğunda, panoda yeni bölgenize ait dikey pencere açılır.


## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

Aşağıdaki örnek yeni bir 'A' kaydı oluşturma işlemini göstermektedir. Diğer kayıt türleri ve var olan kayıtların değiştirilmesi hakkında bilgi için bkz. [Azure portalı kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-portal.md). 


1. **DNS bölgesi** dikey penceresinin üzerindeki **+ Kayıt kümesi**’ni seçerek **Kayıt kümesi ekle** dikey penceresini açın.

    ![Yeni kayıt kümesi](./media/dns-getstarted-portal/newrecordset500.png)

4. **Kayıt kümesi ekle** dikey penceresinde kayıt kümenizi adlandırın. Örneğin, kayıt kümenize "**www**" adını verebilirsiniz.

    ![Kayıt kümesi ekleme](./media/dns-getstarted-portal/addrecordset500.png)

5. Oluşturmak istediğiniz kayıt türünü seçin. Bu örnek için **A**’yı seçin.
6. **TTL** ayarını yapın. Varsayılan yaşam süresi bir saattir.
7. Kaydın IP adresini ekleyin.
8. DNS kaydını oluşturmak için dikey pencerenin altındaki **Tamam**’ı seçin.


## <a name="view-records"></a>Kayıtları görüntüleme

DNS bölgesi dikey penceresinin alt bölümünde DNS bölgesine ait kayıtları görebilirsiniz. Her bölgede oluşturulan varsayılan DNS ve SOA kayıtlarının yanı sıra, oluşturduğunuz tüm kayıtları görürsünüz.

![bölge](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a>Ad sunucularını güncelleştirme

DNS bölgenizin ve kayıtlarınızın doğru şekilde ayarlandığına karar verdikten sonra, Azure DNS ad sunucularını kullanmak için etki alanınızın adını yapılandırmanız gerekir. Bunun yapılması, İnternet üzerindeki diğer kullanıcıların DNS kayıtlarınızı bulmasını sağlar.

Bölgenizin ad sunucuları Azure portalda belirtilir:

![bölge](./media/dns-getstarted-portal/viewzonens500.png)

Bu ad sunucuları, etki alanı adı kayıt şirketi (etki alanı adını satın aldığınız şirket) ile birlikte yapılandırılmalıdır. Kayıt şirketiniz, etki alanı için ad sunucularını ayarlama seçeneğini sunar. Daha fazla bilgi için bkz. [Etki alanınızı Azure DNS’e devretme](dns-domain-delegation.md).


## <a name="next-steps"></a>Sonraki adımlar

Azure DNS hakkında daha fazla bilgi için bkz. [Azure DNS'e genel bakış](dns-overview.md).

Azure DNS’te DNS kayıtlarını yönetme hakkında daha fazla bilgi için bkz. [Azure portalı kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-portal.md).


