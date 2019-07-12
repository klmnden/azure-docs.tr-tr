---
title: Azure veri paylaşımı Önizleme sorunlarını giderme
description: Azure veri paylaşımı önizlemesi ile ilgili sorunları giderme hakkında bilgi edinin
services: data-share
author: joannapea
ms.service: data-share
ms.topic: overview
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: c02f72d6a327c4dcb94ac8844005613cfe316986
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67838393"
---
# <a name="troubleshoot-common-issues-in-azure-data-share-preview"></a>Azure veri paylaşımı önizlemesinde yaygın sorunlarını giderme

Bu makalede, Azure veri paylaşımı önizlemesi için sık karşılaşılan sorunları gidermek gösterilmektedir. 

## <a name="azure-data-share-invitations"></a>Azure veri paylaşımı davetleri 

Bazı durumlarda, yeni bir kullanıcı tıkladığında **daveti kabul** gönderilen e-posta davetini bunlar davetleri boş bir liste ile sunulacak. 

![Davetiniz](media/no-invites.png)

Yukarıdaki hata hizmeti ile bilinen bir sorundur ve şu anda ele alıyor. Geçici bir çözüm olarak izleyin aşağıdaki adımları. 

1. Azure portalında gidin **abonelikler**
1. Azure veri paylaşımı için kullanmakta olduğunuz aboneliği seçin
1. Tıklayarak **kaynak sağlayıcıları**
1. Microsoft.DataShare arayın
1. Tıklayın **kaydetme**

Sahip olmanız gerekir [Azure katkıda bulunan RBAC rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) bu adımları tamamlamak için. 

Hala davet paylaşan veri göremiyorsanız, veri sağlayıcınıza başvurun ve Azure oturum açma e-posta adresinize bir davet gönderilir olun ve *değil* e-posta diğer adınızı. 

> [!IMPORTANT]
> Zaten bir Azure veri paylaşımı davetinizi kabul etti ve depolama yapılandırmadan önce Hizmeti'nden çıkıldı, ayrıntılı yönergeleri izleyin [bir veri kümesi eşlemeyi yapılandırma](how-to-configure-mapping.md) yapılandırmayı tamamlamak hakkında bilgi edinmek için nasıl yapılır Kılavuzu, alınan veri paylaşır ve veri almaya başlayın. 

## <a name="error-when-creating-or-receiving-a-new-data-share"></a>Oluştururken ya da yeni bir veri paylaşımı alma hatası

"Hata: İşlem, bir geçersiz durum kodu 'BadRequest' döndürdü"

"Hata: AuthorizationFailed"

"Hata: depolama hesabı için rol ataması"

![Ayrıcalık hata](media/error-write-privilege.png)

Yeni bir veri paylaşımı oluştururken, yukarıdaki hatalardan birini alıyorsunuz veya depolama hesabı için izinler yetersiz olduğundan yeni bir veri paylaşımı alma, yüklemesiyse. Gerekli izni *Microsoft.Authorization/role atamaları/yazma*, depolama sahip rolü mevcut veya özel bir rol atanabilir. Depolama hesabı oluşturduğunuz bile bunu otomatik olarak, depolama hesabının sahibi yapmaz. Depolama hesabının sahibi kendiniz vermek için aşağıdaki adımları izleyin. Alternatif olarak, özel bir rol için kendiniz ekleyebilirsiniz bu izne sahip oluşturulabilir.  

1. Azure portalında depolama hesabına gidin
1. Seçin **erişim denetimi (IAM)**
1. Tıklayın **Ekle**
1. Kendiniz sahibi olarak ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya başlayın öğrenmek için devam [verilerinizi paylaşmak](share-your-data.md) öğretici.

