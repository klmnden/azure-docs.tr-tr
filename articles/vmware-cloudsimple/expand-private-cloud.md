---
title: VMware CloudSimple özel bulut çözümüyle Azure'ı genişletin
description: Mevcut veya yeni kümede Kapasite eklemek için var olan bir CloudSimple özel buluta genişletin işlemini açıklamaktadır
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/06/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 293a09c57ca95e2774e44ff4bc9f9f2c31be2f49
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332714"
---
# <a name="expand-a-cloudsimple-private-cloud"></a>Bir CloudSimple özel buluta genişletin

CloudSimple dinamik olarak bir özel buluta genişletme esnekliği sağlar. Daha küçük bir yapılandırma ile başlar ve daha yüksek kapasite gerektiği genişletin. Geçerli ihtiyaçlarına göre özel bir bulut oluşturmak ve tüketim büyüdükçe genişletin.

Bir özel bulut bir veya daha fazla vSphere kümelerindeki oluşur. Her küme 3-16 düğümleri olabilir.  Özel Bulutu genişletirken, mevcut kümeye düğüm eklemek veya yeni bir küme oluşturun. Ek düğümler, mevcut küme genişletmek için var olan düğümleri aynı türde (SKU) olması gerekir. Yeni bir küme oluşturmak için farklı türde düğümler olabilir. Özel bulut sınırları hakkında daha fazla bilgi için bkz: sınırlar bölümünde [CloudSimple özel buluta genel bakış](cloudsimple-private-cloud.md) makalesi.

Varsayılan özel bir bulut oluşturulur **Datacenter** vCenter üzerinde.  Her bir veri merkezi bir üst düzey yönetim varlık olarak görev yapar.  Yeni bir küme için var olan veri merkezine eklenmesi veya yeni bir veri merkezine oluşturma seçeneği CloudSimple sağlar.

Yeni küme yapılandırmasının bir parçası olarak VMware altyapısı CloudSimple yapılandırır.  Ayarlar vSAN disk grupları, VMware yüksek kullanılabilirlik ve dağıtılmış kaynak Zamanlayıcı (DRS) için depolama ayarlarını içerir.

Bir özel bulut birden çok kez genişletilebilir. Yalnızca genel düğüm sınırlar içinde haberdar olun genişletme yapılabilir. Her zaman mevcut kümeye eklemek veya yeni bir özel bulut genişletin.

## <a name="before-you-begin"></a>Başlamadan önce

Düğüm özel Bulutunuzdaki genişletmeden önce sağlanması gerekir.  Düğümleri sağlama ile ilgili daha fazla bilgi için bkz: [düğümleri CloudSimple - Azure ile VMware çözümü için sağlama](create-nodes.md) makalesi.  Yeni bir küme oluşturmak için en az üç kullanılabilir düğüm aynı SKU'ların olması gerekir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="expand-a-private-cloud"></a>Bir özel buluta genişletin

1. [CloudSimple portala erişmek](access-cloudsimple-portal.md).

2. Açık **kaynakları** sayfasında ve genişletmek istediğiniz özel Bulutu seçin.

3. Özet bölümünde tıklayın **genişletme**.

    ![Özel buluta genişletin](media/resources-expand-private-cloud.png)

4. Varolan kümenizi genişletin veya yeni bir vSphere küme oluşturmak isteyip istemediğinizi seçin. Sayfasındaki Özet bilgilerini, siz değişiklik yaptığınız anda güncelleştirilir.

    * Mevcut kümenizin genişletmek için **var olan kümeyi genişletin**. Eklenecek düğüm sayısını girin ve genişletmek için istediğiniz kümeyi seçin. Her küme en fazla 16 düğüme sahip olabilir.
    * Yeni bir kümeye eklemek için tıklatın **yeni küme oluştur**. Küme için bir ad girin. Mevcut bir veri merkezi seçin veya yeni bir veri merkezi oluşturmak için bir ad girin. Düğüm türü seçin. Yeni bir vSphere kümeyi oluştururken, ancak mevcut bir vSphere kümesine genişletme değil, farklı düğüm türü seçebilirsiniz. Düğüm sayısını seçin. Her yeni küme en az üç düğüm olması gerekir.

    ![Özel buluta genişletin - düğüm ekleme](media/resources-expand-private-cloud-add-nodes.png)

5. Tıklayın **Gönder** özel buluta genişletin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da VMware sanal makinelerini kullanma](quickstart-create-vmware-virtual-machine.md)
* Daha fazla bilgi edinin [özel Bulutları](cloudsimple-private-cloud.md)