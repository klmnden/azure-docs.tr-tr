---
title: VMware CloudSimple özel bulut çözümüyle Azure Daralt
description: CloudSimple özel bulut küçültme işlemini açıklamaktadır.
author: sharaths-cs
ms.author: b-shsury
ms.date: 07/01/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6e639feb603f1654b4dcd40f16d8e3094307839a
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67544523"
---
# <a name="shrink-a-cloudsimple-private-cloud"></a>CloudSimple özel bulut Daralt

CloudSimple dinamik olarak bir özel bulut daraltmak için esneklik sağlar.  Bir özel bulut bir veya daha fazla vSphere kümelerindeki oluşur. Her küme 3-16 düğümleri olabilir. Bir özel bulut küçültülürken mevcut kümeden düğüm kaldırma veya bir kümenin tamamını silin. 

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki koşullar için bir özel bulutun küçültme karşılanması gerekir.  Yönetim kümesi (ilk küme) oluşturulan bir özel bulutun oluşturulduğu silinemiyor.

* VSphere kümenin üç düğümü olmalıdır.  Yalnızca üç düğümü olan bir küme küçültülemez.
* Tüketilen toplam depolama alanı, toplam kapasite sonra kümenin küçültme aşmamalıdır. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="shrink-a-private-cloud"></a>Bir özel bulut Daralt 

1. [CloudSimple portala erişmek](access-cloudsimple-portal.md).

2. Açık **kaynakları** sayfası.

3. Özel bulutta, küçültmek istediğiniz tıklayın

4. Özet sayfasında, tıklayın **küçültme**.

    ![Özel bulut Daralt](media/shrink-private-cloud.png)

5. Küçültme veya silmek istediğiniz kümeyi seçin. 

    ![Özel bulut - select küme Daralt](media/shrink-private-cloud-select-cluster.png)

6. Seçin **bir düğümü kaldırmak** veya **tüm küme silme**. 

7. Küme kapasitesini doğrulayın

8. Tıklayın **Gönder** özel bulut daraltmak için.

Özel bulutun küçültme başlatılır.  Görev ilerleme durumunu izleyebilirsiniz.  Küçültme işlemi üzerinde vSAN resynced gereken verilere bağlı olarak birkaç saat sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da VMware sanal makinelerini kullanma](quickstart-create-vmware-virtual-machine.md)
* Daha fazla bilgi edinin [özel Bulutları](cloudsimple-private-cloud.md)