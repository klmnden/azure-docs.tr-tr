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
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34664964"
---
## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma
Kurtarma Hizmetleri kasası yedeklemeleri ve zaman içinde oluşturulan kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri kasası, korunan sanal makinelerle ilişkili yedekleme ilkelerini de içerir.

Kurtarma Hizmetleri kasası oluşturmak için:

1. Oturum açtığınızda, aboneliğinizde [Azure portal](https://portal.azure.com/).
2. Sol menüde seçin **tüm hizmetleri**.

    ![Ana menüde tüm hizmetleri seçeneği seçin](./media/backup-create-rs-vault/click-all-services.png) <br/>

3. Tüm hizmetler iletişim kutusuna *kurtarma Hizmetleri*. Yazmaya başladığınızda, girişinizi kaynakların listesini filtreler. Gördüğünüzde, seçeneğini **kurtarma Hizmetleri kasaları**.

    ![Kurtarma Hizmetleri tüm hizmetleri iletişim kutusunda yazın](./media/backup-create-rs-vault/all-services.png) <br/>

    Abonelikteki kurtarma Hizmetleri kasalarının listesi görüntülenir.
4. Üzerinde **kurtarma Hizmetleri kasaları** menüsünde, select **Ekle**.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-create-rs-vault/add-button-create-vault.png)

    **Kurtarma Hizmetleri kasaları** menüsü açılır. Bilgilerini sağlamak için ister **adı**, **abonelik**, **kaynak grubu**, ve **konumu**.

    !["Kurtarma Hizmetleri kasaları" bölmesi](./media/backup-create-rs-vault/create-new-vault-dialog.png)
5. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin. Ad Azure aboneliği benzersiz olması gerekir. En az iki, ancak 50'den fazla karakter içeren bir ad yazın. Adı bir harfle başlamalıdır ve yalnızca harf, rakam ve kısa çizgi içerebilir.
6. İçin **abonelik**, kullanmak istediğiniz aboneliği seçin. Yalnızca bir abonelik üyesi varsa, bu ad görünür. Hangi aboneliğin emin değilseniz varsayılan gidin (veya önerilen) aboneliği. Çalışmanızı birden çok seçenek eksikse veya Okul hesabı birden çok Azure aboneliği ile ilişkili olduğunda.
7. İçin **kaynak grubu** varolan bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Kaynak grupları, aboneliğinizdeki kullanılabilir listesini görmek için seçin **var olanı kullan**ve açılır menüsünü tıklatın. Yeni bir kaynak grubu oluşturmak için seçin **Yeni Oluştur** ve adını yazın. Kaynak grupları hakkında tam bilgi için bkz: [Azure Resource Manager'a genel bakış](../articles/azure-resource-manager/resource-group-overview.md).
8. İçin **konumu** kasa için coğrafi bölgeyi seçin. Sanal makineler, kasa korumak için bir kasa oluşturuyorsanız *gerekir* sanal makinelerle aynı bölgede olması.

   > [!IMPORTANT]
   > VM bulunduğu konumu emin değilseniz kasa oluşturma iletişim kutusunu kapatın ve portaldaki sanal makinelerin listesini gidin. Birden çok bölgede sanal makineniz varsa her bölgede bir Kurtarma Hizmetleri kasası oluşturun. Sonraki konuma geçmeden önce, kasayı ilk konumda oluşturun. Yedek verileri depolamak için depolama hesapları belirtmek için gerek yoktur. Kurtarma Hizmetleri kasası ve Azure Backup hizmeti, otomatik olarak işler.
   >
   >

9. Kurtarma Hizmetleri kasası oluşturmak hazır olduğunuzda tıklatın **oluşturma**.

    ![Yedekleme kasalarının listesi](./media/backup-create-rs-vault/click-create-button.png)

    Kurtarma Hizmetleri kasasının oluşturulması biraz zaman alabilir. Bildirimleri bölümündeki (portalın sağ üst alanı) durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra kurtarma Hizmetleri kasaları listesinde görünür. Kasanızı hala göremiyorsanız tıklatın **yenileme**.

     ![Yedekleme kasalarının listesi](./media/backup-create-rs-vault/refresh-button.png)
