---
title: Azure veri kutusu Edge Cihazınızı iade | Microsoft Docs
description: Azure veri kutusu sınır cihazı geri dönün ve sipariş aygıt için silme işlemini açıklamaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 05/07/2019
ms.author: alkohli
ms.openlocfilehash: 9aeae0ab68d809b36a3316054f12a5a9657721f1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65468612"
---
# <a name="return-your-azure-data-box-edge-device"></a>Azure veri kutusu Edge Cihazınızı döndürür

Bu makalede, veri temizleme ve Azure veri kutusu Edge Cihazınızı dönün açıklar. Cihaz geri döndüğünüzde sonra cihaz ile ilişkili kaynak de silebilirsiniz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Cihazda veri diskleri devre dışı verileri silme
> * Cihazınızı döndürmek için bir destek bileti açın
> * Kullanıcının cihazı paketi ve bir toplama zamanlama
> * Azure portalında bir kaynağı silme

## <a name="erase-data-from-the-device"></a>CİHAZDAN verileri silme

Cihazınızın verileri disklerden verilerini silmek için Cihazınızı sıfırlamak gerekir. Yerel web kullanıcı Arabirimi veya PowerShell arabirimini kullanarak Cihazınızı sıfırlayabilirsiniz.

Sıfırlamadan, yerel verilerin bir kopyasını cihazda gerekirse oluşturun. Verileri CİHAZDAN bir Azure depolama kapsayıcısına kopyalayabilirsiniz.

Yerel web kullanıcı arabirimini kullanarak Cihazınızı sıfırlamak için aşağıdaki adımları uygulayın.

1. Yerel web kullanıcı Arabirimi, Git **Bakım > Cihazınızı sıfırladığınızda**.
2. Seçin **sıfırlama cihaz**.

    ![Cihaz sıfırlama](media/data-box-edge-return-device/device-reset-1.png)

3. Onayınız istendiğinde seçin ve uyarı gözden **Evet** devam etmek için.

    ![Sıfırlama onaylayın](media/data-box-edge-return-device/device-reset-2.png)  

Sıfırlama cihaz verileri disklerden verileri siler. Veri miktarına bağlı olarak Cihazınızda, bu işlem yaklaşık 30-40 dakika sürer.

Alternatif olarak, cihaz ve PowerShell arabirimine bağlanma `Reset-HcsAppliance` cmdlet'i, veri disklerini verileri silmek için. Daha fazla bilgi için [Cihazınızı sıfırlama](data-box-edge-connect-powershell-interface.md#reset-your-device).

> [!NOTE]
> - Değişimi ya da yeni bir cihaz için yükseltme, yalnızca yeni cihaz aldığınız sonra Cihazınızı sıfırladığınızda öneririz.
> - Cihazı yalnızca Sıfırla yerel verileri cihaz dışında siler. Bulutta olan veriler silinmez ve toplar [ücretleri](https://azure.microsoft.com/pricing/details/storage/). Bu veriler gibi bir bulut depolama yönetimi aracı üzerinden ayrı olarak silinmesi gereken [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).

## <a name="open-a-support-ticket"></a>Bir destek bileti açın

Dönüş işlemi başlatmak için aşağıdaki adımları uygulayın.

1. Microsoft, cihazın iade edilmesi istediğiniz belirten Support bir destek bileti açın. Sorun türü olarak seçin **veri kutusu Edge donanım**.

    ![Destek bileti açın](media/data-box-edge-return-device/open-support-ticket-1.png)  

2. Bir Microsoft Support mühendisi sizinle iletişim kuracaktır. Sevkiyat ayrıntılarını sağlayın.
3. Bir iade sevkiyat kutusu gerekirse isteyebilirsiniz. Yanıt **Evet** sorusuna **döndürmek için boş bir kutunun gereken**.


## <a name="schedule-a-pickup"></a>Bir toplama zamanlaması

1. Cihazı kapatın. Yerel web kullanıcı Arabirimi, Git **Bakım > güç ayarları**.
2. Seçin **kapatma**. Onayınız istendiğinde tıklayın **Evet** devam etmek için. Daha fazla bilgi için [güç yönetimi](data-box-gateway-manage-access-power-connectivity-mode.md#manage-power).
3. Güç kabloyu ve tüm ağ kablolarını CİHAZDAN kaldırın.
4. Sevkiyat paket kendi veya Azure'dan aldığınız boş kutusunu kullanarak hazırlayın. Cihaz ve cihaz kutusunda geldiği güç kablosu yerleştirin.
5. Paketi Azure'dan aldığınız Sevkiyat Etiketi eklemesi.
6. Bir toplama bölgesel operatörünüz ile zamanlayın. Cihaz ABD'de döndüren, operatörünüz UPS olur. Bir toplama zamanlamak için:

    1. Yerel UPS (ücretsiz ülkelere özgü arama numarası) çağırın.
    2. Çağrınızda, izleme, yazdırılan etikette gösterilen şekilde numarası ters sevkiyat teklif.
    3. İzleme numarası tırnak işareti değil ise UPS alımı sırasında ek bir ücret ödeme yapmanızı gerektirir.

    Toplama zamanlama yerine, ayrıca en yakın bırakma konumu veri kutusu ucuna devre dışı bırakabilir.

## <a name="delete-the-resource"></a>Kaynak silinemedi

Cihaz Azure veri merkezinde alındıktan sonra cihaz hasar görmesine veya kurcalama tüm belirtileri için Denetlenmekte.

- Cihaz sağlam ve kullanıma hazır olan alınırsa, o kaynak için fatura ölçümünde durdurur. Microsoft Support cihaz döndürüldü onaylamak için sizinle iletişime geçecektir. Ardından Azure portalında cihaz ile ilişkili kaynak silebilirsiniz.
- Önemli ölçüde hasar cihaz geldiğinde, cezaları uygulanabilir. Ayrıntılar için bkz [kayıp veya hasarlı cihaz hakkında SSS](https://azure.microsoft.com/pricing/details/databox/edge/) ve [Ürün hizmet kullanım koşulları](https://www.microsoft.com/licensing/product-licensing/products).  


Azure portalında cihaz silebilirsiniz:
-   Sipariş sonra ve önce cihazın Microsoft tarafından hazırlanır.
-   Cihaz Microsoft'a geri döndüğünüzde sonra Azure veri merkezinde fiziksel İnceleme geçirir ve cihaz döndürüldü onaylamak için Microsoft Support çağırır.

Cihaz başka bir abonelik veya konum karşı etkinleştirildikten sonra Microsoft siparişinizi yeni abonelik veya konum için bir iş günü içinde taşınır. Sipariş taşındıktan sonra bu kaynak silebilirsiniz.


Cihaz ve Azure portalında kaynak silmek için aşağıdaki adımları uygulayın.

1. Azure portalında kaynağınıza gidin ve ardından **genel bakış**. Komut çubuğundan seçin **Sil**.

    ![Sil'i seçin](media/data-box-edge-return-device/delete-resource-1.png)

2. İçinde **cihazı Sil** dikey penceresinde, seçin ve silmek istediğiniz cihazın adını yazın **Sil**.

    ![Silmeyi onayla](media/data-box-edge-return-device/delete-resource-2.png)

Sonra cihaz bildirilir ve ilişkili kaynak başarıyla silindi.

## <a name="next-steps"></a>Sonraki adımlar

- [Bant genişliğini yönetmeyi](data-box-edge-manage-bandwidth-schedules.md) öğrenin.
