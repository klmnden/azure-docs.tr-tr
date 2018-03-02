---
title: "Azure Portal ile Azure DNS’i kullanmaya başlama | Microsoft Docs"
description: "Azure DNS'te DNS bölgesi ve kaydı oluşturma hakkında bilgi edinin. Bu kılavuzda, Azure portalı kullanarak ilk DNS bölgenizi ve kaydınızı oluşturup yönetmeniz için adım adım talimatlar sunulmaktadır."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: kumud
ms.openlocfilehash: 22bf52f7452f182510c3714f7d1c2ca884446953
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="get-started-with-azure-dns-using-the-azure-portal"></a>Azure portal ile Azure DNS’i kullanmaya başlama

> [!div class="op_single_selector"]
> * [Azure portalı](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Bu makalede, Azure portalı kullanarak ilk DNS bölgesi ve kaydınızı oluşturma adımları gösterilmektedir. Ayrıca, Azure PowerShell veya platformlar arası Azure CLI kullanarak aşağıdaki adımları gerçekleştirebilirsiniz.

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Son olarak, DNS bölgenizi Internet'te yayımlamak için etki alanının ad sunucularını yapılandırmanız gerekir. Bu adımların her biri, aşağıdaki adımlarda açıklanmıştır.

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Azure Portal’da oturum açın.
2. Hub menüsünde **Kaynak oluştur > Ağ >** ve ardından **DNS bölgesi**’ne tıklayarak **DNS bölgesi oluştur** sayfasını açın.

    ![DNS bölgesi](./media/dns-getstarted-portal/openzone650.png)

4. **DNS bölgesi oluştur** sayfasında aşağıdaki değerleri girin ve **Oluştur**’a tıklayın:


   | **Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Ad**|contoso.com|DNS bölgesinin adı|
   |**Abonelik**|[Aboneliğiniz]|DNS bölgesini oluşturmak için bir abonelik seçin.|
   |**Kaynak grubu**|**Yeni oluştur:** contosoDNSRG|Bir kaynak grubu oluşturun. Kaynak grubu adı, seçili abonelik içinde benzersiz olmalıdır. Kaynak grupları hakkında daha fazla bilgi için, [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups)’a genel bakış makalesini okuyun.|
   |**Konum**|Batı ABD||

> [!NOTE]
> Kaynak grubu, kaynak grubunun konumunu ifade eder ve DNS bölgesini etkilemez. DNS bölgesinin konumu her zaman "genel" şeklindedir ve gösterilmez.

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

Aşağıdaki örnek yeni bir 'A' kaydı oluşturma işlemini göstermektedir. Diğer kayıt türleri ve var olan kayıtların değiştirilmesi hakkında bilgi için bkz. [Azure portalı kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-portal.md). 

1. Oluşturulan DNS Bölgesi ile, Azure Portal **Sık Kullanılanlar** bölmesinde, **Tüm kaynaklar**’a tıklayın. Tüm kaynaklar sayfasında **contoso.com** DNS bölgesine tıklayın. Seçili abonelikte zaten çeşitli kaynaklar varsa, DNS Bölgesine kolaylıkla erişmek için **Ada göre filtrele...** kutusuna **contoso.com** girebilirsiniz.

1. **DNS bölgesi** sayfasının üzerindeki **+ Kayıt kümesi**’ni seçerek **Kayıt kümesi ekle** sayfasını açın.

1. **Kaynak kümesi ekle** sayfasında aşağıdaki değerleri girin ve **Tamam**’a tıklayın. Bu örnekte, bir A kaydı oluşturuyorsunuz.

   |**Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Ad**|www|Kaydın adı|
   |**Tür**|A| Oluşturulacak DNS kaydının türü; kabul edilebilir değerler A, AAAA, CNAME, MX, NS, SRV, TXT ve PTR’dir.  Kayıt türleri hakkında daha fazla bilgi için, [DNS bölgelerine ve kayıtlarına genel bakış](dns-zones-records.md) bağlantısını ziyaret edin|
   |**TTL**|1|DNS isteğinin yaşam süresi.|
   |**TTL birimi**|Saat|TTL değeri için zaman ölçümü.|
   |**IP adresi**|IP Adresi Değeri| Bu değer, DNS kaydının çözümlendiği IP adresidir.|

## <a name="view-records"></a>Kayıtları görüntüleme

DNS bölgesi sayfasının alt bölümünde DNS bölgesine ait kayıtları görebilirsiniz. Her bölgede oluşturulan varsayılan DNS ve SOA kayıtlarının yanı sıra, oluşturduğunuz tüm kayıtları görürsünüz.

![bölge](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a>Ad sunucularını güncelleştirme

DNS bölgenizin ve kayıtlarınızın doğru şekilde ayarlandığına karar verdikten sonra, Azure DNS ad sunucularını kullanmak için etki alanınızın adını yapılandırmanız gerekir. Bunun yapılması, İnternet üzerindeki diğer kullanıcıların DNS kayıtlarınızı bulmasını sağlar.

Bölgenizin ad sunucuları Azure portalda belirtilir:

![bölge](./media/dns-getstarted-portal/viewzonens500.png)

Bu ad sunucuları, etki alanı adı kayıt şirketi (etki alanı adını satın aldığınız şirket) ile birlikte yapılandırılmalıdır. Kayıt şirketiniz, etki alanı için ad sunucularını ayarlama seçeneğini sunar. Daha fazla bilgi için bkz. [Etki alanınızı Azure DNS’e devretme](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Bu makalede oluşturulan tüm kaynakları silmek için, aşağıdaki adımları tamamlayın:

1. Azure Portal **Sık Kullanılanlar** bölmesinde, **Tüm kaynaklar**’a tıklayın. Tüm kaynaklar sayfasında **MyResourceGroup** kaynak grubuna tıklayın. Seçili abonelikte zaten çeşitli kaynaklar varsa, kaynak grubuna kolaylıkla erişmek için **Ada göre filtrele...** kutusuna **MyResourceGroup** girebilirsiniz.
1. **MyResourceGroup** sayfasında **Sil** düğmesine tıklayın.
1. Portal, silmek istediğinizi onaylamak için kaynak grubunun adını yazmanızı gerektirir. **Sil**’e tıklayın, kaynak grubu adı olarak *MyResourceGroup* yazın ve ardından **Sil**’e tıklayın. Bir kaynak grubunun silinmesiyle, kaynak grubu içerisindeki tüm kaynaklar silinir, bu nedenle, silmeden önce kaynak grubunun içeriğini onaylamayı hiçbir zaman unutmayın. Portal, kaynak grubu içinde yer alan tüm kaynakları siler ve sonra kaynak grubunu siler. Bu işlem birkaç dakika sürer.


## <a name="next-steps"></a>Sonraki adımlar

Azure DNS hakkında daha fazla bilgi için bkz. [Azure DNS'e genel bakış](dns-overview.md).

Azure DNS’te DNS kayıtlarını yönetme hakkında daha fazla bilgi için bkz. [Azure portalı kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-portal.md).

