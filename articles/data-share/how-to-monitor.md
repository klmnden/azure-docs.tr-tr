---
title: Azure veri paylaşımı Önizleme izleme
description: Azure veri paylaşımı Önizleme izleme
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 869c1ed41d7f78df184461bc1d8cab6c6eb8d426
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789104"
---
# <a name="monitor-azure-data-share-preview"></a>İzleyici Azure veri paylaşımı Önizleme 

Bu makalede, Azure veri paylaşımı Önizlemesi'ni kullanarak veri paylaşımlarınızın nasıl izleyeceğiniz açıklanmıştır. Bir veri sağlayıcısı, veri ilişkileri paylaşım çeşitli yönlerini izleyebilirsiniz. Veri tüketicilerinin davetinizi veri paylaşımına olup, bir paylaşım abonelik oluşturduğunuza ve verilerinizi kullanmaya olarak kabul edip gibi ayrıntıları izlemek kullanılabilir. 

Bir veri tüketici Azure aboneliğinize tetiklenen anlık görüntüleri izleyebilirsiniz. 

## <a name="monitor-invitation-status"></a>Davet durumu İzleyicisi

-> Davetleri gönderilen paylaşımlarına giderek veri paylaşımı davetiyelerinizi durumunu görüntüleyin. 

![Davet durumu](./media/invitation-status.png "davet durumu") 

Davetinizi kullanılabilir üç durumu vardır:

* Bekleyen - veri paylaşımı alıcı henüz daveti kabul etmedi.
* Kabul edildi - veri paylaşımı alıcı daveti kabul etti.
* Reddedilen - veri paylaşımı alıcı davet reddetti.

> [!IMPORTANT]
> Davet zaten kabul ettikten sonra silerseniz, erişimi iptal etmek için eşdeğer değildir. Veri tüketicileri depolama hesabınızda kopyalanan gelen gelecekteki anlık görüntüleri durdurmak istiyorsanız, erişimi iptal *paylaşmak abonelikleri* sekmesi. 

## <a name="monitor-share-subscriptions"></a>İzleyici paylaşımı abonelikler

Gönderilen paylaşımlarına giderek paylaşımı aboneliklerinizin durumunu görüntüle -> abonelikler paylaşın. Bu daveti kabul ettikten sonra veri tüketicileri tarafından oluşturulan Etkin Abonelikler hakkında ayrıntılar verecektir. Gelecekteki güncelleştirmelerin paylaşımı abonelik ve seçerek, veri tüketici durdurabilirsiniz *iptal*. 

## <a name="snapshot-history"></a>Anlık görüntü geçmişi 

Geçmiş sekmesinde de kiracıya veri tüketici kopyalanmış anlık görüntüleri görüntüleyebilirsiniz. Sıklığı ve süresi her anlık görüntü aralığı izleyebilirsiniz. 

![Geçmiş anlık görüntü](./media/sent-shares.png "geçmiş anlık görüntüsü") 

Çalıştırma başlangıç tarihi tıklayarak çalıştırın her anlık görüntü hakkında daha fazla ayrıntı görüntüleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki Adımlar 

Daha fazla bilgi edinin [Azure veri paylaşımı terminolojisi](terminology.md)