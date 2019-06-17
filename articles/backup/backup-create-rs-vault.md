---
title: 'Azure yedekleme: Kurtarma Hizmetleri kasası oluşturma'
description: yedeklemeleri ve kurtarma noktalarını depolayan bir kurtarma Hizmetleri kasası oluşturma
services: backup
author: sogup
manager: vijayts
keywords: Kurtarma Hizmetleri kasası; Azure VM yedeklemesi; Azure VM geri yükleme;
ms.service: backup
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: sogup
ms.openlocfilehash: 9fba7d679b7d0edb3c99207c99b23f9616c6fa0e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66477578"
---
# <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri kasası, yedeklemeleri ve zaman içinde oluşturulan kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri kasası Ayrıca, korunan sanal makinelerle ilişkili yedekleme ilkeleri içerir.

Kurtarma Hizmetleri kasası oluşturmak için:

1. Oturum açmak için aboneliğinizde [Azure portalında](https://portal.azure.com/).

2. Sol menüden **tüm hizmetleri**.

    ![Tüm hizmetleri seçin](./media/backup-create-rs-vault/click-all-services.png)

3. İçinde **tüm hizmetleri** iletişim kutusuna **kurtarma Hizmetleri**. Kaynakların listesini girişinizi göre filtreler. Kaynak listesinde seçin **kurtarma Hizmetleri kasaları**.

    ![Girin ve kurtarma Hizmetleri kasaları seçin](./media/backup-create-rs-vault/all-services.png)

    Abonelikte kurtarma Hizmetleri kasalarının listesi görünür.

4. Üzerinde **kurtarma Hizmetleri kasaları** panoyu seçin **Ekle**.

    ![Bir kurtarma Hizmetleri kasası Ekle](./media/backup-create-rs-vault/add-button-create-vault.png)

    **Kurtarma Hizmetleri kasası** iletişim kutusu açılır. İçin değerler sağlayın **adı**, **abonelik**, **kaynak grubu**, ve **konumu**.

    ![Kurtarma Hizmetleri kasasını yapılandırma](./media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Ad**: Kasayı tanımlamak için bir kolay ad girin. Adın Azure aboneliği için benzersiz olmalıdır. En az iki, ancak 50'den fazla karakter içeren bir ad belirtin. Adı bir harf ile başlamalı ve yalnızca harf, rakam ve kısa çizgi oluşur.
   - **Abonelik**: Kullanılacak aboneliği seçin. Yalnızca bir abonelik üyesi değilseniz, bu adı görürsünüz. Hangi aboneliğin emin değilseniz varsayılan (önerilen) aboneliği kullanın. Çalışmanızı birden çok seçenek eksikse veya Okul hesabı birden fazla Azure aboneliği ile ilişkili olduğunda.
   - **Kaynak grubu**: Mevcut bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Aboneliğinizdeki kullanılabilir kaynak grupları listesini görmek için seçin **var olanı kullan**ve sonra aşağı açılan liste kutusundan bir kaynak seçin. Yeni bir kaynak grubu oluşturmak için Seç **Yeni Oluştur** ve adını girin. Kaynak grupları hakkında eksiksiz bilgiler için bkz. [Azure Resource Manager'a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
   - **Konum**: Kasa için coğrafi bölgeyi seçin. Sanal makineleri, kasa korumak için bir kasa oluşturmak için **gerekir** sanal makinelerle aynı bölgede olmalıdır.

      > [!IMPORTANT]
      > Sanal makinenizin konumunu emin değilseniz, iletişim kutusunu kapatın. Portaldaki sanal makineler listesine gidin. Birçok bölgede sanal makineniz varsa her bölgede bir kurtarma Hizmetleri kasası oluşturun. Kasa için başka bir konuma oluşturmadan önce kasayı ilk konumda oluşturun. Yedekleme verilerini depolamak için depolama hesaplarının belirtilmesi gerek yoktur. Kurtarma Hizmetleri kasası ve Azure Backup hizmeti, otomatik olarak işler.
      >
      >

5. Kurtarma Hizmetleri kasası oluşturmak hazır olduğunuzda **Oluştur**.

    ![Kurtarma Hizmetleri kasası oluşturma](./media/backup-create-rs-vault/click-create-button.png)

    Bu, Kurtarma Hizmetleri kasası oluşturmak için biraz zaman alabilir. Durum bildirimlerini izleyin **bildirimleri** portalının sağ üst köşesindeki alan. Kasanız oluşturulduktan sonra kurtarma Hizmetleri kasaları listesinde görünür. Kasanız görmüyorsanız seçin **Yenile**.

     ![Yedekleme kasalarının listesi Yenile](./media/backup-create-rs-vault/refresh-button.png)

## <a name="set-storage-redundancy"></a>Küme depolama yedekliliği

Azure yedekleme kasası için depolama otomatik olarak işler. Bu depolamanın nasıl çoğaltılacağını belirtmenize gerek.

1. **Kurtarma Hizmetleri kasaları** dikey penceresinden yeni kasaya tıklayın. Altında **ayarları** bölümünde **özellikleri**.
2. İçinde **özellikleri**altında **yedekleme yapılandırması**, tıklayın **güncelleştirme**.

3. Depolama çoğaltma türü seçin ve tıklayın **Kaydet**.

     ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

   - Bu, Azure'ı birincil yedek depolama uç noktası olarak kullanıyorsanız, devam varsayılan öneririz **coğrafi olarak yedekli** ayarı.
   - Azure’u birincil yedek depolama uç noktası olarak kullanmıyorsanız, Azure depolama maliyetlerini azaltan **Yerel olarak yedekli** seçeneğini belirleyin.
   - Daha fazla bilgi edinin [coğrafi](../storage/common/storage-redundancy-grs.md) ve [yerel](../storage/common/storage-redundancy-lrs.md) yedeklilik.

> [!NOTE]
> Değiştirme **depolama çoğaltma türü** bir kurtarma Hizmetleri kasası için yedekleme Kasası'nda yapılandırmadan önce yapılması (yerel olarak yedekli / coğrafi olarak yedekli) içerir. Yedeklemeyi yapılandırma, değiştirmek için bu seçeneği devre dışıdır ve değiştiremezsiniz sonra **depolama çoğaltma türü**. 

## <a name="next-steps"></a>Sonraki adımlar

[Hakkında bilgi edinin](backup-azure-recovery-services-vault-overview.md) kurtarma Hizmetleri kasaları.
[Hakkında bilgi edinin](backup-azure-delete-vault.md) Sil kurtarma Hizmetleri kasaları.
