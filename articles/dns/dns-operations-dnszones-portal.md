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
ms.openlocfilehash: d0a20de8738e8c7b2719a9de85d5fd16aa5778cf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60926348"
---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a>Azure portalında DNS bölgelerini yönetme

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure klasik CLI](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI](dns-operations-dnszones-cli.md)

Bu makalede, Azure portalını kullanarak DNS bölgelerini yönetme işlemini göstermektedir. Platformlar arası kullanarak DNS bölgelerini de yönetebilirsiniz [Azure CLI](dns-operations-dnszones-cli.md) ya da Azure [PowerShell](dns-operations-dnszones.md).

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Azure portalında oturum açın
2. Hub menüsünde gidin **kaynak Oluştur > Ağ > DNS bölgesi** açmak için **DNS bölgesi oluştur** dikey penceresi.

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

Azure portalında gidin **diğer hizmetler** > **ağ** > **DNS bölgelerini**. Her DNS bölgesinin kendi kaynak ve kayıt kümelerini gibi bilgileri ve ad sunucuları bu görünümden görüntülenebilir. Sütun **ad SUNUCULARINI** varsayılan görünüm değil. Eklemek için tıklatın **sütunları**seçin **ad sunucuları**ve ardından **Bitti**.

![DNS bölgelerini listeleme](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>DNS bölgesini silme

Portalda bir DNS bölgesine gidin. Üzerinde **DNS bölgesi** dikey penceresinde tıklayın **bölgeyi Sil**. Ardından, DNS bölgesini silmek isteyen onaylamanız istenir. Bir DNS bölgesi silindiğinde bölgede bulunan tüm kayıtları siler.

## <a name="next-steps"></a>Sonraki adımlar

DNS bölgenizin ve kayıtlarını ile ziyaret ederek çalışmayı öğrenin [Azure portalını kullanarak Azure DNS ile çalışmaya başlama](dns-getstarted-portal.md).