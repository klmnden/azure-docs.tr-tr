---
title: Ağır Azure veri kutusu siparişi için öğretici | Microsoft Docs
description: Dağıtım önkoşulları ve bir Azure veri kutusu ağır sipariş öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 05/29/2019
ms.author: alkohli
ms.openlocfilehash: 8453a3592c1822489a3724dacdf8f0ff5e8492f1
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427916"
---
# <a name="tutorial-order-azure-data-box-heavy-preview"></a>Öğretici: Sipariş Azure veri kutusu ağır (Önizleme)


Azure veri kutusu ağır şirket içi verilerinizi Azure'da hızlı, kolay ve güvenilir bir şekilde alma olanak tanıyan bir karma çözümüdür. Verilerinizi bir Microsoft tarafından sağlanan 770 TB (yaklaşık kullanılabilir kapasite) depolama cihazına aktarabilir ve ardından cihazı geri gönderin. Bu veriler daha sonra Azure'a yüklenir.

Bu öğreticide, nasıl bir Azure veri kutusu ağır sıralayabilirsiniz açıklanmaktadır. Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> * Veri kutusu ağır için Önkoşullar
> * Ağır bir veri kutusu siparişi
> * Siparişi izleme
> * Siparişi iptal etme

## <a name="prerequisites"></a>Önkoşullar

Cihazı dağıtmadan önce Data Box hizmeti ve cihazı için aşağıdaki yapılandırma önkoşullarını tamamlayın.

### <a name="for-installation-site"></a>Yüklemeyi site için

Başlamadan önce aşağıdakilerden emin olun:

- Cihaz standart açılan kapılar ve entryways aracılığıyla uyar. Ancak, cihaz, entryways sığdığından emin olun. Cihaz boyutlar: genişliği: 26" uzunluğu: 48" height: 28”.
- Bir zemin katından giriş yaptığınızı dışında yüklediyseniz, cihazın bir Asansör veya bir ramp aracılığıyla erişmeniz gerekir. Cihaz yaklaşık 500 ~ lbs hafiftir.
- Yakınlık bu ayak izine sahip bir cihaz barındırabilecek bir kullanılabilir ağ bağlantısına sahip bir veri merkezinde düz bir siteye sahip olduğunuzdan emin olun.


### <a name="for-service"></a>Hizmet için

Başlamadan önce aşağıdakilerden emin olun:
- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.
- Data Box hizmeti için kullandığınız aboneliğin aşağıdaki türlerden birinde olduğundan emin olun:
    - Microsoft Kurumsal Anlaşma (EA). [EA abonelikleri](https://azure.microsoft.com/pricing/enterprise-agreement/) hakkındaki yazıları okuyun.
    - Bulut Çözümü Sağlayıcısı (CSP). [Azure CSP programı](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview) hakkında daha fazla bilgi edinin.
    - Microsoft Azure Sponsorluğu. [Azure sponsorluğu programı](https://azure.microsoft.com/offers/ms-azr-0036p/) hakkında daha fazla bilgi edinin.

- Veri kutusu ağır bir sipariş oluşturmak için abonelik sahibi veya katkıda bulunan erişiminiz olduğundan emin olun.

### <a name="for-device"></a>Cihaz için

Başlamadan önce aşağıdakilerden emin olun:
- Cihazınızı paketten çıkarılan.
- Veri merkezi ağına bağlı bir konak bilgisayarınız olmalıdır. Veri kutusu ağır verileri bu bilgisayardan kopyalayın. Ana bilgisayarınızı açıklandığı gibi desteklenen bir işletim sistemi çalıştırmalıdır [Azure veri kutusu ağır sistem gereksinimleri](data-box-system-requirements.md).
- Cihaz için yerel kullanıcı Arabirimi bağlanın ve RJ-45 kablosu ile dizüstü bilgisayar olması gerekir. Dizüstü bilgisayar, her cihazın düğümü kez yapılandırmak için kullanın.
- Veri merkezinizin yüksek hızlı ağı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir.
- 40 GB/sn ihtiyacınız veya cihaz düğüm başına 10 GB/sn kablo. İle uyumlu kablolar seçin [Mellanox MCX314A-BCCT](https://store.mellanox.com/products/mellanox-mcx314a-bcct-connectx-3-pro-en-network-interface-card-40-56gbe-dual-port-qsfp-pcie3-0-x8-8gt-s-rohs-r6.html) ağ arabirimi:

    - 40 GB/sn kablosunun, kablo cihazı sonuna QSFP + olması gerekir.
    - 10 GB/sn kablosu ile bir QSFP + SFP + bağdaştırıcısı (veya QSA bağdaştırıcısı) aygıta takılan ucu için bir son 10 G anahtarda içine takılan SFP + kablo gerekir.
    - Güç kabloları cihazla dahil edilir.

## <a name="order-data-box-heavy"></a>Sipariş veri kutusu ağır

Cihaz sipariş etmek için Azure portalında aşağıdaki adımları izleyin.

1. Microsoft Azure kimlik bilgilerini kullanarak şu URL’de oturum açın: [https://portal.azure.com](https://portal.azure.com).
2. Seçin **+ kaynak Oluştur** araması *Azure Data Box*. Seçin **Azure Data Box '** .
    
   [![Azure Data Box arama 1](media/data-box-deploy-ordered/search-azure-data-box1.png)](media/data-box-deploy-ordered/search-azure-data-box1.png#lightbox)

3. **Oluştur**’u seçin.

4. Data Box hizmeti Bölgenizde kullanılabilir olup olmadığını denetleyin. Girin veya seçin aşağıdaki bilgileri seçin **Uygula**.

    |Ayar  |Değer  |
    |---------|---------|
    |Abonelik     | Data Box hizmeti için bir kurumsal Anlaşma, CSP veya Azure sponsorluğu aboneliği seçin. <br> Abonelik fatura hesabınıza bağlıdır.       |
    |Aktarım türü     | **Azure’a içeri aktar**’ı seçin.        |
    |Kaynak ülke     | Verileriniz şu anda bulunduğu ülke/bölge seçin.         |
    |Hedef Azure bölgesi     | Verileri aktarmak istediğiniz Azure bölgesini seçin.        |

    [![Data Box ailesi kullanılabilirlik seçin](media/data-box-deploy-ordered/select-data-box-option1.png)](media/data-box-deploy-ordered/select-data-box-option1.png#lightbox)

5. Seçin **veri kutusu ağır**. Tek bir sipariş için en fazla kullanılabilir kapasite 770 TB'dir.

    [![Veri kutusu ağır seçin](media/data-box-heavy-deploy-ordered/select-data-box-heavy.png)

6. **Sipariş** bölümünde **Sipariş ayrıntıları**'nı belirtin. Girin veya seçin aşağıdaki bilgileri seçin **sonraki**.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Ad     | Siparişi takip etmek için kullanılacak kolay bir ad girin. <br> Ad harf, rakam ve tirelerden oluşan 3-24 karakter arası uzunlukta olabilir. <br> Ad bir harf veya sayıyla başlamalı ve bitmelidir.      |
    |Kaynak grubu     | Var olan bir taneyi kullanın veya yenisini oluşturun. <br> Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır.         |
    |Hedef Azure bölgesi     | Depolama hesabınız için bir bölge seçin. <br> Daha fazla bilgi için [bölge kullanılabilirliği](https://azure.microsoft.com/global-infrastructure/services/?products=databox)’ne gidin.        |
    |Depolama hedefi     | Depolama hesabı veya yönetilen diskleri veya her ikisini de seçin. <br> Belirtilen Azure bölgesine göre filtrelenen listeden var olan bir veya daha fazla depolama hesabı seçin. <br>Veri kutusu ağır en fazla 10 depolama hesapları ile bağlanabilir. <br> Ayrıca yeni bir oluşturabilirsiniz **genel amaçlı v1**, **genel amaçlı v2**, veya **Blob Depolama hesabı**. <br> Azure Data Lake depolama Gen 2 hesapları desteklenmez. Bkz: [Cihazınızda desteklenen depolama hesapları](data-box-heavy-system-requirements.md#supported-storage-accounts). <br>Depolama hesaplarını sanal ağlarla desteklenir. Güvenli Depolama hesapları ile çalışmak Data Box hizmeti izin vermek için depolama hesabı ağ güvenlik duvarı ayarlarını içindeki güvenilir hizmetler sağlar. Daha fazla bilgi için bkz. nasıl [güvenilir bir hizmet olarak Azure Data Box'ı ekleme hizmet](../storage/common/storage-network-security.md#exceptions).|

    Depolama hesabı depolama hedefi kullanıyorsanız, aşağıdaki ekran görüntüsüne bakın:

    ![Depolama hesabı için veri kutusu ağır sırası](media/data-box-heavy-deploy-ordered/order-storage-account.png)

    Depolama hedefi olarak depolama hesabının yanı sıra, ayrıca veri kutusu ağır şirket içi Vhd'lerden yönetilen disk oluşturmak için kullanıyorsanız, aşağıdaki bilgileri vermeniz gerekir:

    |Ayar  |Değer  |
    |---------|---------|
    |Kaynak grupları     | Şirket içi Vhd'lerden yönetilen disk oluşturmak istiyorsanız, yeni kaynak grubu oluşturun. Yalnızca kaynak grubu daha önce yönetilen disk için bir veri kutusu ağır sırası oluşturulurken Data Box hizmeti tarafından oluşturulmuşsa, mevcut bir kaynak grubunu kullanabilirsiniz. <br> Noktalı virgülle ayırarak birden fazla kaynak grubu belirtin. En fazla 10 kaynak grubu desteklenir.|

    ![Yönetilen disk için veri kutusu ağır sırası](media/data-box-heavy-deploy-ordered/order-managed-disks.png)

    Yönetilen diskler için belirtilen depolama hesabı, hazırlama depolama hesabı olarak kullanılır. Data Box hizmeti VHD'lerin sayfası olarak, yönetilen disklere dönüştürme ve kaynak grubuna taşımadan önce hazırlama depolama hesabına blobları karşıya yükleme. Daha fazla bilgi için [doğrulama verileri Azure'a karşıya](data-box-deploy-picked-up.md#verify-data-upload-to-azure).

7. **Teslimat adresi**’ne adınızı, soyadınızı, şirket adını, posta adresini ve geçerli bir telefon numarasını girin. Seçin **adresini doğrulayın**. 

    Hizmet, teslimat adresinde hizmetin kullanılabilirlik durumunu doğrular. Hizmet belirtilen teslimat adresinde kullanılabilir durumdaysa bu konuda bir bildirim gönderilir. **İleri**’yi seçin.

8. **Bildirim ayrıntıları** sayfasında e-posta adresi belirtin. Hizmet, belirtilen e-posta adreslerine sipariş durumundaki güncelleştirmelerle ilgili bilgi gönderir.

    Grup yöneticisinin ayrılması durumunda da bildirim almaya devam etmek için bir grup e-postası kullanmanız önerilir.

9. **Özet** sayfasında sipariş, iletişim, bildirim ve gizlilik koşullarıyla ilgili bilgileri gözden geçirin. Gizlilik koşullarını kabul ettiğinizi belirten kutuyu işaretleyin.

10. Seçin **sipariş**. Siparişin oluşturulması birkaç dakika sürer.


## <a name="track-the-order"></a>Siparişi izleme

Siparişi verdikten sonra durumunu Azure portalından takip edebilirsiniz. Veri kutusu ağır siparişinizi gidin ve ardından Git **genel bakış** durumunu görüntülemek için. Portalda siparişin durumu **Sipariş edildi** olarak görünür.

Cihaz yoksa bir bildirim alırsınız. Cihaz varsa, Microsoft gönderilecek cihazı belirler ve gönderimi hazırlar. Cihaz hazırlanırken şu eylemler gerçekleştirilir:

- Cihazla ilişkili her depolama hesabı için SMB paylaşımları oluşturulur.
- Her paylaşım için, kullanıcı adı veya parola gibi erişim kimlik bilgileri oluşturulur.
- Cihaz kilidinin açılmasına yardımcı olan cihaz parolası da oluşturulur.
- Veri kutusu ağır herhangi bir noktada cihaza yetkisiz erişimi önlemek için kilitli.

Cihazın hazırlanması tamamlandığında portalda sipariş durumu **İşlendi** olarak değişir.

![İşlenen veri kutusu ağır sırası](media/data-box-overview/data-box-order-status-processed.png)

Microsoft ardından cihazınızı hazırlar ve bölgeye uygun gönderim şirketine teslim eder. Cihaz gönderildikten sonra bir takip numarası iletilir. Portalda sipariş durumu **Yola çıktı** olarak değişir.

![Veri kutusu ağır sırası gönderilir](media/data-box-overview/data-box-order-status-dispatched.png)

## <a name="cancel-the-order"></a>Siparişi iptal etme

Bu siparişi iptal etmek için Azure portalında **Genel bakış**'a gidin ve komut çubuğundan **İptal**'e tıklayın.

Sipariş verdikten sonra, sipariş durumu işlendi olmadan önce herhangi bir noktada siparişi iptal edebilirsiniz.
 
İptal edilen bir siparişi silmek için **Genel bakış**'a gidin ve komut çubuğundan **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure veri kutusu ağır konuları hakkında gibi öğrendiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Sipariş veri kutusu ağır
> * Siparişi izleme
> * Siparişi iptal etme

Veri kutusu ağır hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Ayarlama, Azure veri kutusu ağır](./data-box-heavy-deploy-set-up.md)
