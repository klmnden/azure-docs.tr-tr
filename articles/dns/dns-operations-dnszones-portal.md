---
title: Azure DNS'de - Azure portalı DNS bölgelerini yönetme | Microsoft Docs
description: Azure portalını kullanarak DNS bölgelerini yönetebilirsiniz. Bu makalede, güncelleştirme, silme ve Azure DNS DNS bölgeleri oluşturma açıklanır
services: dns
documentationcenter: na
author: vhorne
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: victorh
ms.openlocfilehash: a9325dff84635955600bc78687ec0156495ae893
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46954390"
---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a>Azure portalında DNS bölgelerini yönetme

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Klasik Azure CLI](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI](dns-operations-dnszones-cli.md)

Bu makalede, Azure portalını kullanarak DNS bölgelerini yönetme işlemini göstermektedir. Platformlar arası kullanarak DNS bölgelerini de yönetebilirsiniz [Azure CLI](dns-operations-dnszones-cli.md) ya da Azure [PowerShell](dns-operations-dnszones.md).

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Azure portalında oturum açın
2. Hub menüsünde tıklayıp **kaynak Oluştur > Ağ >** ve ardından **DNS bölgesi** oluşturma DNS bölgesi dikey penceresini açın.

    ![DNS bölgesi](./media/dns-operations-dnszones-portal/openzone650.png)

4. **DNS bölgesi oluştur** dikey penceresinde aşağıdaki değerleri girin ve **Oluştur**’a tıklayın:


   | **Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Ad**|contoso.com|DNS bölgesinin adı|
   |**Abonelik**|[Aboneliğiniz]|DNS bölgesini oluşturmak için bir abonelik seçin.|
   |**Kaynak grubu**|**Yeni oluştur:** contosoDNSRG|Bir kaynak grubu oluşturun. Kaynak grubu adı, seçili abonelik içinde benzersiz olmalıdır. Kaynak grupları hakkında daha fazla bilgi için, [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups)’a genel bakış makalesini okuyun.|
   |**Konum**|Batı ABD||

> [!NOTE]
> Kaynak grubu, kaynak grubunun konumunu ifade eder ve DNS bölgesini etkilemez. DNS bölgesinin konumu her zaman "genel" şeklindedir ve gösterilmez.

## <a name="list-dns-zones"></a>DNS bölgelerini listeleme

Azure portalında gidin **diğer hizmetler** > **ağ** > **DNS bölgelerini**. Her DNS bölgesinin kendi kaynak, kayıt kümesi sayısı gibi bilgileri olan ve bu görünümden ad sunucularını görünebilen ' dir. Sütun **ad SUNUCULARINI** tıklatın eklemek için varsayılan görünümünde değil **sütunları**seçin **ad sunucuları** tıklatıp **Bitti**.

![DNS bölgelerini listeleme](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>DNS bölgesini silme

Portalda bir DNS bölgesine gidin. Üzerinde **DNS bölgesi** dikey penceresinde tıklayın **bölgeyi Sil**. DNS bölgesini silmek isteyen onaylamanız istenir. Bir DNS bölgesi silindiğinde, bölge içinde bulunan tüm kayıtları siler.

## <a name="next-steps"></a>Sonraki adımlar

DNS bölgenizin ve kayıtlarını ile ziyaret ederek çalışmayı öğrenin [Azure portalını kullanarak Azure DNS ile çalışmaya başlama](dns-getstarted-portal.md).