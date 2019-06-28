---
title: Azure VMware çözümüyle CloudSimple özel bulut oluşturma
description: İşletimsel esneklik ve devamlılığı ile VMware iş yüklerini buluta genişletmek için CloudSimple özel bulut oluşturma işlemini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e5c03c1d8a865b792ce79e3e2b576a629b71e02c
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67333087"
---
# <a name="create-a-cloudsimple-private-cloud"></a>CloudSimple özel bulut oluşturma

Özel bulut oluşturmaya yönelik çeşitli ağ altyapısı için ortak gereksinimlerini yardımcı olur:

* **Büyüme**. Var olan altyapınız için bir donanım yenileme noktası ulaştınız, özel bulut, gerekli hiçbir yeni donanım yatırımı ile genişletmek sağlar.

* **Hızlı genişletme**. Herhangi bir geçici veya planlanmamış kapasite gereksinimleri ortaya çıktığında, bir özel bulut, ek kapasite ile gecikme oluşturmanıza olanak sağlar.

* **Artırılmış koruma**. Üç veya daha fazla düğümden oluşan bir özel Bulutla otomatik yedeklilik ve yüksek kullanılabilirlik koruma alın.

* **Uzun vadeli altyapı için gerekirse**. Veri merkezleri kapasitesine olan veya maliyetlerinizi düşürmek için yapılandırılacak istiyorsanız özel bir bulut veri merkezleri devre dışı bırakma ve kalan çalışırken bir bulut tabanlı bir çözüm, işletme işlemleri ile uyumlu geçiş olanak sağlar.

Bir özel bulut oluşturduğunuzda, bir tek vSphere kümesi ve tüm yönetim kümedeki oluşturulan VM'ler alın.

## <a name="before-you-begin"></a>Başlamadan önce

Özel bulut oluşturmadan önce düğümleri sağlanması gerekir.  Düğümleri sağlama ile ilgili daha fazla bilgi için bkz: [düğümleri CloudSimple - Azure ile VMware çözümü için sağlama](create-nodes.md) makalesi.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="access-the-cloudsimple-portal"></a>CloudSimple portalına erişim

Erişim [CloudSimple portalı](access-cloudsimple-portal.md).

## <a name="create-a-new-private-cloud"></a>Yeni bir özel bulut oluşturma

1. Üzerinde **kaynakları** sayfasında **yeni özel bulut**.

    ![Create a Private Cloud - başlatma](media/create-pc-button.png)

2. Özel bulut kaynakları barındıracak konumu seçin.

3. Özel bulut için sağlanan CS28 veya CS36 düğüm türü you'ev seçin. İkinci seçeneği, en fazla işlem ve bellek kapasitesini içerir.

4. Özel bulut için düğüm sayısını seçin. En fazla kullanılabilir düğüm sayısını, you'ev seçebilirsiniz [sağlanan](create-nodes.md).

    ![Create a Private Cloud - temel ayarları](media/create-private-cloud-basic-info.png)

5. Tıklayın **sonraki: Gelişmiş Seçenekleri**.

6. VSphere/vSAN alt ağ için CIDR aralığı girin. CIDR aralığı herhangi bir şirket içi veya Azure diğer alt ağlara (sanal ağlar) veya ağ geçidi alt ağı ile çakışmayacak emin olun.  Azure sanal ağları üzerinde tanımlı herhangi bir CIDR aralığı kullanmayın.
    
    **CIDR aralığı seçenekleri:** /24, /23, /22 veya /21. Bir/24 CIDR aralığı desteklediği en çok dokuz düğümleri, bir /23 CIDR aralığı desteklediği en fazla 41 düğümleri ve bir /22 ve /21 CIDR aralığı en fazla 64 düğüm (en fazla sayı düğümü özel bulut) destekler.

    > [!CAUTION]
    > VSphere/vSAN CIDR aralığı IP adresleri, kullanım için özel bulut altyapısı tarafından ayrılır.  Herhangi bir sanal makinede bu aralıktaki IP adresini kullanmayın.

7. Tıklayın **sonraki: Gözden geçir ve Oluştur**.

8. Ayarları gözden geçirin. Herhangi bir ayarı değiştirmeniz gerekiyorsa, tıklayın **önceki**.

9. **Oluştur**’a tıklayın.

Özel bulut sağlama, tıkladığınızda başlayacak oluşturun.  İlerlemesini izleyebilirsiniz [görevleri](https://docs.azure.cloudsimple.com/activity/#tasks) CloudSimple portalı sayfasında.  Sağlama iki saatten 30 dakika sürebilir.  Sağlama tamamlandığında bir e-posta alırsınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel buluta genişletin](expand-private-cloud.md)