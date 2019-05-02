---
title: Azure Data Box Disk sipariş için öğretici | Microsoft Docs
description: Bu öğreticide Azure'a veri aktarma amacıyla Azure Data Box Disk'e kaydolmayı ve sipariş vermeyi öğreneceksiniz.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 02/27/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: ad11dba43e7e1561a74e04cd4f05b26569cc10d9
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924929"
---
# <a name="tutorial-order-an-azure-data-box-disk"></a>Öğretici: Bir Azure Data Box Disk sipariş

Azure Data Box Disk, şirket içi verilerinizi Azure'a hızlı, kolay ve güvenilir bir şekilde aktarmanızı sağlayan bir hibrit bulut çözümüdür. Verilerinizi Microsoft tarafından sağlanan katı hal sürücülerine (SSD) aktarır ve diskleri geri gönderirsiniz. Bu veriler daha sonra Azure'a yüklenir.

Bu öğreticide Azure Data Box Disk siparişi verme adımları anlatılmaktadır. Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> * Data Box Disk sipariş etme
> * Siparişi izleme
> * Siparişi iptal etme

## <a name="prerequisites"></a>Önkoşullar

Dağıtmadan önce Data Box hizmeti ve Data Box Disk için aşağıdaki yapılandırma önkoşulları tamamlayın.

### <a name="for-service"></a>Hizmet için

Başlamadan önce aşağıdakilerden emin olun:
- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.
- Data Box hizmeti için kullandığınız aboneliğin aşağıdaki türlerden birinde olduğundan emin olun:
    - Microsoft Kurumsal Anlaşma (EA). [EA abonelikleri](https://azure.microsoft.com/pricing/enterprise-agreement/) hakkındaki yazıları okuyun.
    - Bulut Çözümü Sağlayıcısı (CSP). [Azure CSP programı](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview) hakkında daha fazla bilgi edinin.
- Data Box siparişi oluşturmak için, abonelik üzerinde sahip veya katkıda bulunan erişimine sahip olduğunuzdan emin olun.

### <a name="for-device"></a>Cihaz için

Başlamadan önce aşağıdakilerden emin olun:
- Kullanılabilir bir istemci bilgisayar, verileri kopyalamak sahip. İstemci bilgisayarınızda:
    - [Desteklenen bir işletim sistemi](data-box-disk-system-requirements.md#supported-operating-systems-for-clients) çalıştırılmalıdır.
    - Windows istemciyse diğer [gerekli yazılımlar](data-box-disk-system-requirements.md#other-required-software-for-windows-clients) yüklenmiş olmalıdır.  

## <a name="order-data-box-disk"></a>Data Box Disk sipariş etme

Data Box Disk sipariş etmek için [Azure portalda](https://aka.ms/azuredataboxfromdiskdocs) aşağıdaki adımları gerçekleştirin.

1. Portalın sol üst köşesinde **+ Kaynak oluştur**'a tıklayın ve *Azure Data Box* aratın. **Azure Data Box**'a tıklayın.
    
   ![Azure Data Box arama 1](media/data-box-disk-deploy-ordered/search-data-box11.png)

2. **Oluştur**’a tıklayın.

3. Data Box'ın bölgenizde kullanılabilir olup olmadığını kontrol edin. Aşağıdaki bilgileri girin veya seçin ve sonra **Uygula**'ya tıklayın.

    ![Data Box Disk seçeneğini belirleyin](media/data-box-disk-deploy-ordered/select-data-box-sku-1.png)

    |Ayar|Değer|
    |---|---|
    |Abonelik|Data Box hizmetinin etkinleştirildiği aboneliği seçin.<br> Abonelik fatura hesabınıza bağlıdır. |
    |Aktarım türü| Azure'e İçeri Aktarma|
    |Kaynak ülke | Verilerinizin bulunduğu ülkeyi seçin.|
    |Hedef Azure bölgesi|Verileri aktarmak istediğiniz Azure bölgesini seçin.|

  
5.  **Data Box Disk**'i seçin. Çözümün 5 disklik tek bir sipariş için maksimum kapasitesi 35 TB olarak belirlenmiştir. Daha büyük veri boyutları için birden fazla sipariş oluşturabilirsiniz.

     ![Data Box Disk seçeneğini belirleyin](media/data-box-disk-deploy-ordered/select-data-box-sku-zoom.png)

6.  **Sipariş** bölümünde **Sipariş ayrıntıları**'nı belirtin. Aşağıdaki bilgileri girin veya seçin.

    |Ayar|Değer|
    |---|---|
    |Ad|Siparişi takip etmek için kullanılacak kolay bir ad girin.<br> Ad harf, rakam ve tirelerden oluşan 3-24 karakter arası uzunlukta olabilir. <br> Ad bir harf veya sayıyla başlamalı ve bitmelidir. |
    |Kaynak grubu| Var olan bir taneyi kullanın veya yenisini oluşturun. <br> Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır. |
    |Hedef Azure bölgesi| Depolama hesabınız için bir bölge seçin.<br> Şu anda depolama hesapları ABD, Batı ve Kuzey Avrupa, Kanada ve Avustralya’nın tüm bölgelerinde desteklenmektedir. |
    |TB cinsinden tahmini veri boyutu| TB cinsinden tahmini veri boyutunu girin. <br>Microsoft, veri boyutuna uygun sayıda 8 TB boyuta sahip SSD'ler (7 TB kullanılabilir kapasite) gönderir. <br>5 diskin maksimum kullanılabilir kapasitesi 35 TB olacaktır. |
    |Disk geçiş anahtarı| **Azure tarafından oluşturulan geçiş anahtarı yerine özel anahtar kullanın** seçeneğini işaretlerseniz disk geçiş anahtarını sağlayın. <br> En az bir sayısal ve bir özel karakter olan 12-ile 32 karakter alfasayısal bir anahtar sağlar. İzin verilen karakterler: `@?_+`. <br> Bu seçeneği atlayabilir ve disklerinizin kilidini açmak için Azure tarafından oluşturulan destek anahtarını kullanabilirsiniz.|
    |Depolama hedefi     | Depolama hesabı veya yönetilen diskleri veya her ikisini de seçin. <br> Belirtilen Azure bölgeye göre mevcut bir depolama hesabını filtrelenmiş listesinden bir depolama hesabı seçin. Data Box en çok 10 depolama hesabına bağlanabilir. <br> Ayrıca yeni bir oluşturabilirsiniz **genel amaçlı v1**, **genel amaçlı v2**, veya **Blob Depolama hesabı**. <br>Yapılandırılmış kurallara sahip depolama hesapları kullanılamaz. Depolama hesapları gereken **tüm ağlardan erişime izin ver** güvenlik duvarları ve sanal ağlar bölümünde.|

    Depolama hesabı depolama hedefi kullanıyorsanız, aşağıdaki ekran görüntüsüne bakın:

    ![Data Box Disk Siparişiniz için depolama hesabı](media/data-box-disk-deploy-ordered/order-storage-account.png)

    Şirket içi Vhd'lerden yönetilen disk oluşturmak için Data Box Disk kullanıyorsanız, ayrıca aşağıdaki bilgileri vermeniz gerekir:

    |Ayar  |Değer  |
    |---------|---------|
    |Kaynak grubu     | Şirket içi Vhd'lerden yönetilen disk oluşturmak istiyorsanız, yeni bir kaynak grubu oluşturun. Yalnızca, yönetilen disk için Data Box Disk Siparişiniz için Data Box hizmeti tarafından oluşturulmuş mevcut bir kaynak grubunu kullanın. <br> Yalnızca bir kaynak grubu desteklenir.|

    ![Data Box Disk Siparişiniz için yönetilen disk](media/data-box-disk-deploy-ordered/order-managed-disks.png)

    Yönetilen diskler için belirtilen depolama hesabı, hazırlama depolama hesabı olarak kullanılır. Data Box hizmeti VHD hazırlama depolama hesabına yükler ve ardından bunları da yönetilen disklere dönüştürür ve kaynak gruplarına taşır. Daha fazla bilgi için [doğrulama verileri Azure'a karşıya](data-box-disk-deploy-picked-up.md#verify-data-upload-to-azure).

13. **İleri**’ye tıklayın.

    ![Sipariş ayrıntılarını belirtin](media/data-box-disk-deploy-ordered/data-box-order-details.png)

14. **Teslimat adresi** sekmesinde adınızı, soyadınızı, şirket adını, posta adresini ve geçerli bir telefon numarası girin. **Adresi doğrula**'ya tıklayın. Hizmet, teslimat adresinde hizmetin kullanılabilirlik durumunu doğrular. Hizmet belirtilen teslimat adresinde kullanılabilir durumdaysa bu konuda bir bildirim gönderilir. 

    ![Teslimat adresi belirtme](media/data-box-disk-deploy-ordered/data-box-shipping-address.png)
15. **Bildirim ayrıntıları** sayfasında e-posta adresi belirtin. Hizmet, belirtilen e-posta adreslerine sipariş durumundaki güncelleştirmelerle ilgili bilgi gönderir. 

    Grup yöneticisinin ayrılması durumunda da bildirim almaya devam etmek için bir grup e-postası kullanmanız önerilir.

16. **Özet** sayfasında sipariş, iletişim, bildirim ve gizlilik koşullarıyla ilgili bilgileri gözden geçirin. Gizlilik koşullarını kabul ettiğinizi belirten kutuyu işaretleyin.

17. **Sipariş**'e tıklayın. Siparişin oluşturulması birkaç dakika sürer.

 
## <a name="track-the-order"></a>Siparişi izleme

Siparişi verdikten sonra durumunu Azure portalından takip edebilirsiniz. Siparişinize gidin ve durumunu görüntülemek için **Genel bakış** sayfasını inceleyin. Portalda işin durumu **Sipariş edildi** olarak görünür.

![Data Box Disk durumu, sipariş verildi](media/data-box-disk-deploy-ordered/data-box-portal-ordered.png) 

Diskler kullanılabilir durumda değilse bir bildirim gönderilir. Diskler kullanılabilir durumdaysa Microsoft tarafından gönderilecek diskler belirlenir ve disk paketi hazırlanır. Diskler hazırlanırken şu eylemler gerçekleştirilir:

- Diskler AES-128 BitLocker şifrelemesi kullanılarak şifrelenir.  
- Diskler yetkisiz erişimi önlemek için kilitlenir.
- Bu işlem sırasında disklerin kilidini açan destek anahtarı oluşturulur.

Disklerin hazırlanması tamamlandığında portalda sipariş durumu **İşlenen** olarak değişir.

Microsoft ardından disklerinizi hazırlar ve bölgeye uygun gönderim şirketine teslim eder. Diskler gönderildikten sonra bir takip numarası iletilir. Portalda sipariş durumu **Yola çıktı** olarak değişir.

## <a name="cancel-the-order"></a>Siparişi iptal etme

Bu siparişi iptal etmek için Azure portalında **Genel bakış**'a gidin ve komut çubuğundan **İptal**'e tıklayın.

Yalnızca diskler sipariş edildikten ve sipariş gönderim için işleme aşamasındayken iptal edebilirsiniz. Sipariş işleme alındıktan sonra iptal edemezsiniz.

![Siparişi iptal etme](media/data-box-disk-deploy-ordered/cancel-order1.png)

İptal edilen bir siparişi silmek için **Genel bakış**'a gidin ve komut çubuğundan **Sil**'e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Data Box Disk sipariş etme
> * Siparişi izleme
> * Siparişi iptal etme

Data Box Disk'inizi ayarlama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box Diskinizi ayarlama](./data-box-disk-deploy-set-up.md)
