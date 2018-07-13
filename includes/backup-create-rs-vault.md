---
title: include dosyası
description: include dosyası
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 5/14/2018
ms.author: markgal
ms.custom: include file
ms.openlocfilehash: 5590da80a1c217e7902e8e010688e40f5624898c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38730507"
---
## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma
Kurtarma Hizmetleri kasası, yedeklemeleri ve zaman içinde oluşturulan kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri kasası Ayrıca, korunan sanal makinelerle ilişkili yedekleme ilkeleri içerir.

Kurtarma Hizmetleri kasası oluşturmak için:

1. Oturum açmak için aboneliğinizde [Azure portalında](https://portal.azure.com/).
2. Sol taraftaki menüde **tüm hizmetleri**.

    ![Ana menüde tüm hizmetleri seçeneğini belirleyin](./media/backup-create-rs-vault/click-all-services.png) <br/>

3. Tüm hizmetleri iletişim kutusuna *kurtarma Hizmetleri*. Yazmaya başladığınızda, giriş kaynakların listesini filtrelenir. Gördüğünüzde, seçmek **kurtarma Hizmetleri kasaları**.

    ![Kurtarma Hizmetleri tüm hizmetleri iletişim kutusunda yazın](./media/backup-create-rs-vault/all-services.png) <br/>

    Abonelikte kurtarma Hizmetleri kasalarının listesi görünür.
4. Üzerinde **kurtarma Hizmetleri kasaları** menüsünde **Ekle**.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-create-rs-vault/add-button-create-vault.png)

    **Kurtarma Hizmetleri kasaları** menüsü açılır. Bilgilerini sağlamak için ister **adı**, **abonelik**, **kaynak grubu**, ve **konumu**.

    !["Kurtarma Hizmetleri kasaları" bölmesi](./media/backup-create-rs-vault/create-new-vault-dialog.png)
5. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin. Adın Azure aboneliği için benzersiz olmalıdır. En az iki, ancak 50'den fazla karakter içeren bir ad yazın. Adı bir harf ile başlamalı ve yalnızca harf, rakam ve kısa çizgi içerebilir.
6. İçin **abonelik**, kullanmak istediğiniz aboneliği seçin. Yalnızca bir abonelik üyesiyseniz, bu ad görünür. Hangi aboneliğin emin değilseniz varsayılan Git (veya önerilen) aboneliği. Çalışmanızı birden çok seçenek eksikse veya Okul hesabı birden çok Azure aboneliği ile ilişkili olduğunda.
7. İçin **kaynak grubu** mevcut bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Kaynak grupları, aboneliğinizdeki kullanılabilir listesini görmek için seçin **var olanı kullan**ve açılan menüsüne tıklayın. Yeni bir kaynak grubu oluşturmak için Seç **Yeni Oluştur** ve adını yazın. Kaynak grupları hakkında eksiksiz bilgiler için bkz. [Azure Resource Manager'a genel bakış](../articles/azure-resource-manager/resource-group-overview.md).
8. İçin **konumu** kasa için coğrafi bölgeyi seçin. Sanal makineleri, kasa korumak için bir kasa oluşturuyorsanız *gerekir* sanal makinelerle aynı bölgede olmalıdır.

   > [!IMPORTANT]
   > Sanal makinenizin bulunduğu konumu emin değilseniz kasa oluşturma iletişim kutusunu kapatın ve portaldaki sanal makineler listesine gidin. Birden çok bölgede sanal makineniz varsa her bölgede bir Kurtarma Hizmetleri kasası oluşturun. Sonraki konuma geçmeden önce, kasayı ilk konumda oluşturun. Yedekleme verilerini depolamak için depolama hesaplarının belirtilmesi gerek yoktur. Kurtarma Hizmetleri kasası ve Azure Backup hizmeti, otomatik olarak işler.
   >
   >

9. Kurtarma Hizmetleri kasası oluşturmak hazır olduğunuzda, tıklayın **Oluştur**.

    ![Yedekleme kasalarının listesi](./media/backup-create-rs-vault/click-create-button.png)

    Kurtarma Hizmetleri kasasının oluşturulması biraz zaman alabilir. Bildirimleri bölümündeki (portalın sağ üst alanındaki) durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra kurtarma Hizmetleri kasaları listesinde görünür. Kasanız hala görmüyorsanız, tıklayın **Yenile**.

     ![Yedekleme kasalarının listesi](./media/backup-create-rs-vault/refresh-button.png)
