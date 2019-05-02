---
title: Azure Data Box siparişi için öğretici | Microsoft Docs
description: Dağıtım önkoşullarını ve Azure Data Box siparişi etmeyi öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 04/23/2019
ms.author: alkohli
ms.openlocfilehash: 05d522550b96813c6b8326d83f09d7028466c835
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64926225"
---
# <a name="tutorial-order-azure-data-box"></a>Öğretici: Azure Data Box'ı sırası

Azure Data Box, şirket içi verilerinizi Azure'a hızlı, kolay ve güvenilir bir şekilde aktarmanızı sağlayan bir hibrit çözümdür. Verilerinizi Microsoft tarafından sağlanan 80 TB'lık (kullanılabilir kapasite) depolama cihazına aktarabilir ve cihazı geri gönderebilirsiniz. Bu veriler daha sonra Azure'a yüklenir.

Bu öğreticide Azure Data Box sipariş etme adımları açıklanır. Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> * Data Box dağıtımı önkoşulları
> * Data Box sipariş etme
> * Siparişi izleme
> * Siparişi iptal etme

## <a name="prerequisites"></a>Önkoşullar

Cihazı dağıtmadan önce Data Box hizmeti ve cihazı için aşağıdaki yapılandırma önkoşullarını tamamlayın.

### <a name="for-service"></a>Hizmet için

Başlamadan önce aşağıdakilerden emin olun:
- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.
- Data Box hizmeti için kullandığınız aboneliğin aşağıdaki türlerden birinde olduğundan emin olun:
    - Microsoft Kurumsal Anlaşma (EA). [EA abonelikleri](https://azure.microsoft.com/pricing/enterprise-agreement/) hakkındaki yazıları okuyun.
    - Bulut Çözümü Sağlayıcısı (CSP). [Azure CSP programı](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview) hakkında daha fazla bilgi edinin.
    - Microsoft Azure Sponsorluğu. [Azure sponsorluğu programı](https://azure.microsoft.com/offers/ms-azr-0036p/) hakkında daha fazla bilgi edinin.

- Data Box siparişi oluşturmak için, abonelik üzerinde sahip veya katkıda bulunan erişimine sahip olduğunuzdan emin olun.

### <a name="for-device"></a>Cihaz için

Başlamadan önce aşağıdakilerden emin olun:
- Veri merkezi ağına bağlı bir konak bilgisayarınız olmalıdır. Data Box verileri bu bilgisayardan kopyalayacaktır. [Azure Data Box sistem gereksinimleri](data-box-system-requirements.md) altında açıklandığı gibi konak bilgisayarınızın desteklenen bir işletim sistemini çalıştırması gerekir.
- Veri merkezinizin yüksek hızlı ağı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı yoksa 1 GbE veri bağlantısı kullanılabilir; ancak, kopyalama hızı etkilenir.


## <a name="order-data-box"></a>Data Box sipariş etme

Cihaz sipariş etmek için Azure portalında aşağıdaki adımları izleyin.

1. Microsoft Azure kimlik bilgilerini kullanarak şu URL’de oturum açın: [https://portal.azure.com](https://portal.azure.com).
2. **+ Kaynak oluştur**’a tıklayın ve *Azure Data Box* araması yapın. **Azure Data Box**'a tıklayın.
    
   [![Azure Data Box arama 1](media/data-box-deploy-ordered/search-azure-data-box1.png)](media/data-box-deploy-ordered/search-azure-data-box1.png#lightbox)

3. **Oluştur**’a tıklayın.

4. Data Box'ın bölgenizde kullanılabilir olup olmadığını kontrol edin. Aşağıdaki bilgileri girin veya seçin ve sonra **Uygula**'ya tıklayın. 

    |Ayar  |Değer  |
    |---------|---------|
    |Abonelik     | Data Box hizmeti için bir kurumsal Anlaşma, CSP veya Azure sponsorluğu aboneliği seçin. <br> Abonelik fatura hesabınıza bağlıdır.       |
    |Aktarım türü     | **Azure’a içeri aktar**’ı seçin.        |
    |Kaynak ülke     |   Verilerinizin bulunduğu ülkeyi seçin.         |
    |Hedef Azure bölgesi     |     Verileri aktarmak istediğiniz Azure bölgesini seçin.        |

5. **Data Box**'ı seçin. Tek bir sipariş için en fazla kullanılabilir kapasite 80 TB'dir. Daha büyük veri boyutları için birden fazla sipariş oluşturabilirsiniz.

      [![Data Box seçeneğini belirtin 1](media/data-box-deploy-ordered/select-data-box-option1.png)](media/data-box-deploy-ordered/select-data-box-option1.png#lightbox)

6. **Sipariş** bölümünde **Sipariş ayrıntıları**'nı belirtin. Aşağıdaki bilgileri girin veya seçin ve sonra **İleri**'ye tıklayın.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Ad     |  Siparişi takip etmek için kullanılacak kolay bir ad girin. <br> Ad harf, rakam ve tirelerden oluşan 3-24 karakter arası uzunlukta olabilir. <br> Ad bir harf veya sayıyla başlamalı ve bitmelidir.      |
    |Kaynak grubu     |   Var olan bir taneyi kullanın veya yenisini oluşturun. <br> Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır.         |
    |Hedef Azure bölgesi     | Depolama hesabınız için bir bölge seçin. <br> Daha fazla bilgi için [bölge kullanılabilirliği](data-box-overview.md#region-availability)’ne gidin.        |
    |Depolama hedefi     | Depolama hesabı veya yönetilen diskleri veya her ikisini de seçin. <br> Belirtilen Azure bölgesine göre filtrelenen listeden var olan bir veya daha fazla depolama hesabı seçin. Data Box en çok 10 depolama hesabına bağlanabilir. <br> Ayrıca yeni bir oluşturabilirsiniz **genel amaçlı v1**, **genel amaçlı v2**, veya **Blob Depolama hesabı**. <br>Depolama hesaplarını sanal ağlarla desteklenir. Güvenli Depolama hesapları ile çalışmak Data Box hizmeti izin vermek için depolama hesabı ağ güvenlik duvarı ayarlarını içindeki güvenilir hizmetler sağlar. Daha fazla bilgi için bkz. nasıl [güvenilir bir hizmet olarak Azure Data Box ekleme](../storage/common/storage-network-security.md#exceptions).|

    Depolama hesabı depolama hedefi kullanıyorsanız, aşağıdaki ekran görüntüsüne bakın:

    ![Depolama hesabı için veri kutusu siparişi](media/data-box-deploy-ordered/order-storage-account.png)

    Şirket içi Vhd'lerden yönetilen disk oluşturmak için Data Box'ı kullanıyorsanız, aşağıdaki bilgileri vermeniz gerekir:

    |Ayar  |Değer  |
    |---------|---------|
    |Kaynak grupları     | Şirket içi Vhd'lerden yönetilen disk oluşturmak istiyorsanız, yeni kaynak grubu oluşturun. Yalnızca kaynak grubu daha önce bir Data Box Siparişiniz için yönetilen disk oluşturulurken Data Box hizmeti tarafından oluşturulmuşsa, mevcut bir kaynak grubunu kullanabilirsiniz. <br> Noktalı virgülle ayırarak birden fazla kaynak grubu belirtin. En fazla 10 kaynak grubu desteklenir.|

    ![Veri kutusu siparişi için yönetilen disk](media/data-box-deploy-ordered/order-managed-disks.png)

    Yönetilen diskler için belirtilen depolama hesabı, hazırlama depolama hesabı olarak kullanılır. Data Box hizmeti VHD'lerin sayfası olarak, yönetilen disklere dönüştürme ve kaynak grubuna taşımadan önce hazırlama depolama hesabına blobları karşıya yükleme. Daha fazla bilgi için [doğrulama verileri Azure'a karşıya](data-box-deploy-picked-up.md#verify-data-upload-to-azure).

7. **Teslimat adresi**’ne adınızı, soyadınızı, şirket adını, posta adresini ve geçerli bir telefon numarasını girin. **Adresi doğrula**'ya tıklayın. Hizmet, teslimat adresinde hizmetin kullanılabilirlik durumunu doğrular. Hizmet belirtilen teslimat adresinde kullanılabilir durumdaysa bu konuda bir bildirim gönderilir. **İleri**’ye tıklayın.

8. **Bildirim ayrıntıları** sayfasında e-posta adresi belirtin. Hizmet, belirtilen e-posta adreslerine sipariş durumundaki güncelleştirmelerle ilgili bilgi gönderir.

    Grup yöneticisinin ayrılması durumunda da bildirim almaya devam etmek için bir grup e-postası kullanmanız önerilir.

9. **Özet** sayfasında sipariş, iletişim, bildirim ve gizlilik koşullarıyla ilgili bilgileri gözden geçirin. Gizlilik koşullarını kabul ettiğinizi belirten kutuyu işaretleyin.

10. **Sipariş**'e tıklayın. Siparişin oluşturulması birkaç dakika sürer.


## <a name="track-the-order"></a>Siparişi izleme

Siparişi verdikten sonra durumunu Azure portalından takip edebilirsiniz. Data Box Siparişiniz gidin ve ardından Git **genel bakış** durumunu görüntülemek için. Portalda siparişin durumu **Sipariş edildi** olarak görünür.

Cihaz yoksa bir bildirim alırsınız. Cihaz varsa, Microsoft gönderilecek cihazı belirler ve gönderimi hazırlar. Cihaz hazırlanırken şu eylemler gerçekleştirilir:

- Cihazla ilişkili her depolama hesabı için SMB paylaşımları oluşturulur.
- Her paylaşım için, kullanıcı adı veya parola gibi erişim kimlik bilgileri oluşturulur.
- Cihaz kilidinin açılmasına yardımcı olan cihaz parolası da oluşturulur.
- Data Box, herhangi bir noktada cihaza yetkisiz erişimi önlemek için kilitlenmiştir.

Cihazın hazırlanması tamamlandığında portalda sipariş durumu **İşlendi** olarak değişir.

![Data Box siparişi işlendi](media/data-box-overview/data-box-order-status-processed.png)

Microsoft ardından cihazınızı hazırlar ve bölgeye uygun gönderim şirketine teslim eder. Cihaz gönderildikten sonra bir takip numarası iletilir. Portalda sipariş durumu **Yola çıktı** olarak değişir.

![Data Box siparişi yola çıktı](media/data-box-overview/data-box-order-status-dispatched.png)

## <a name="cancel-the-order"></a>Siparişi iptal etme

Bu siparişi iptal etmek için Azure portalında **Genel bakış**'a gidin ve komut çubuğundan **İptal**'e tıklayın.

Sipariş verdikten sonra, sipariş durumu işlendi olmadan önce herhangi bir noktada siparişi iptal edebilirsiniz.
 
İptal edilen bir siparişi silmek için **Genel bakış**'a gidin ve komut çubuğundan **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Data Box dağıtımı önkoşulları
> * Data Box sipariş etme
> * Siparişi izleme
> * Siparişi iptal etme

Data Box’ınızı ayarlama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box’ınızı ayarlama](./data-box-deploy-set-up.md)


