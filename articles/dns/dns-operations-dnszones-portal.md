---
title: "Azure DNS'de - Azure portalında DNS bölgelerini yönetme | Microsoft Docs"
description: "DNS bölgelerini Azure portalını kullanarak yönetebilirsiniz. Bu makalede, güncelleştirme, silme ve DNS bölgelerini Azure DNS üzerinde oluşturmak açıklar"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: gwallace
ms.openlocfilehash: cfb1debf9447cd66856b73166a133d5d498fcc79
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a>Azure portalında DNS bölgelerini yönetme

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Bu makalede Azure portalını kullanarak DNS bölgelerini yönetme gösterilmektedir. Platformlar arası kullanarak DNS bölgelerini yönetebilmeniz için [Azure CLI](dns-operations-dnszones-cli.md) veya Azure [PowerShell](dns-operations-dnszones.md).

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Azure portalında oturum açın
2. Hub menüsünde ve tıklayın **bir kaynak oluşturmak > Ağ >** ve ardından **DNS bölgesi** oluşturma DNS bölge dikey penceresini açın.

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

## <a name="list-dns-zones"></a>Liste DNS bölgeleri

Azure portalında gidin **daha fazla hizmet** > **ağ** > **DNS bölgeleri**. Her DNS bölgesinin kendi kaynağı, kayıt kümesi sayısı gibi bilgiler ve ad sunucuları bu görünümden görüntülenebilir ' dir. Sütun **ad sunucuları** tıklatın eklemek için varsayılan görünümünde değil **sütunları**seçin **ad sunucuları** tıklatıp **Bitti**.

![DNS bölgelerini listeleme](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>Bir DNS bölgesi Sil

Bir DNS bölgesi portalında gidin. Üzerinde **DNS bölgesi** dikey penceresinde tıklatın **bölgeyi Sil**. DNS bölgesini silmek isteyen onaylamanız istenir. Bir DNS bölgesi silme bölgede bulunan tüm kayıtları siler.

## <a name="next-steps"></a>Sonraki adımlar

DNS bölgesi ve kayıtları ile ziyaret ederek çalışmayı öğrenin [Azure Azure portalını kullanarak DNS ile çalışmaya başlama](dns-getstarted-portal.md).