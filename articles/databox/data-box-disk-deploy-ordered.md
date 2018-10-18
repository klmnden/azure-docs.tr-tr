---
title: 'Öğretici: Microsoft Azure Data Box Disk siparişi verme | Microsoft Docs'
description: Bu öğreticide Azure'a veri aktarma amacıyla Azure Data Box Disk'e kaydolmayı ve sipariş vermeyi öğreneceksiniz.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 09/04/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: bd90d3c4c9207374d6a6085df6a3962ef42b68a9
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091451"
---
# <a name="tutorial-order-an-azure-data-box-disk-preview"></a>Öğretici: Azure Data Box Disk (Önizleme) siparişi verme

Azure Data Box Disk, şirket içi verilerinizi Azure'a hızlı, kolay ve güvenilir bir şekilde aktarmanızı sağlayan bir hibrit bulut çözümüdür. Verilerinizi Microsoft tarafından sağlanan katı hal sürücülerine (SSD) aktarır ve diskleri geri gönderirsiniz. Bu veriler daha sonra Azure'a yüklenir. 

Bu öğreticide Azure Data Box Disk siparişi verme adımları anlatılmaktadır. Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> * Data Box Disk'e kaydolma
> * Data Box Disk sipariş etme
> * Siparişi izleme
> * Siparişi iptal etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!IMPORTANT]
> - Data Box Disk önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 
> - Önizleme sırasında Data Box Disk, ABD, Batı ve Kuzey Avrupa, Kanada ve Avustralya ülkelerindeki müşterilere gönderilebilir. Daha fazla bilgi için bkz. [Bölge kullanılabilirliği](data-box-disk-overview.md#region-availability).

## <a name="sign-up"></a>Kaydolma 

Data Box Disk önizleme aşamasındadır ve hizmete kaydolmanız gerekir. Data Box hizmetine kaydolmak için aşağıdaki adımları izleyin:

1. Azure portalda oturum açın: [https://aka.ms/azuredataboxfromdiskdocs](https://aka.ms/azuredataboxfromdiskdocs).
2. Önizlemeyi etkinleştirmek istediğiniz aboneliği seçin. Veri boyutu, verilerin bulunduğu ülke, zaman aralığı ve veri aktarımı sıklığı hakkındaki soruları yanıtlayın. **Beni kaydet!** bağlantısına tıklayın.
3. Kaydolduktan ve önizlemeyi etkinleştirdikten sonra Data Box Disk siparişi verebilirsiniz.

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
    |Adı|Siparişi takip etmek için kullanılacak kolay bir ad girin.<br> Ad harf, rakam ve tirelerden oluşan 3-24 karakter arası uzunlukta olabilir. <br> Ad bir harf veya sayıyla başlamalı ve bitmelidir. |
    |Kaynak grubu| Var olan bir taneyi kullanın veya yenisini oluşturun. <br> Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır. |
    |Hedef Azure bölgesi| Depolama hesabınız için bir bölge seçin.<br> Şu anda depolama hesapları ABD, Batı ve Kuzey Avrupa, Kanada ve Avustralya’nın tüm bölgelerinde desteklenmektedir. |
    |Depolama hesapları|Belirtilen Azure bölgesine göre filtrelenen listeden var olan bir depolama hesabını seçin. <br>Dilerseniz yeni bir Genel amaçlı v1 veya Genel amaçlı v2 hesabı da oluşturabilirsiniz. |
    |TB cinsinden tahmini veri boyutu| TB cinsinden tahmini veri boyutunu girin. <br>Microsoft, veri boyutuna uygun sayıda 8 TB boyuta sahip SSD'ler (7 TB kullanılabilir kapasite) gönderir. <br>5 diskin maksimum kullanılabilir kapasitesi 35 TB olacaktır. |

13. **İleri**’ye tıklayın. 

    ![Sipariş ayrıntılarını belirtin](media/data-box-disk-deploy-ordered/data-box-order-details.png)

14. **Teslimat adresi** sekmesinde adınızı, soyadınızı, şirket adını, posta adresini ve geçerli bir telefon numarası girin. **Adresi doğrula**'ya tıklayın. Hizmet, teslimat adresinde hizmetin kullanılabilirlik durumunu doğrular. Hizmet belirtilen teslimat adresinde kullanılabilir durumdaysa bu konuda bir bildirim gönderilir. 

    ![Teslimat adresi belirtme](media/data-box-disk-deploy-ordered/data-box-shipping-address.png)
15. **Bildirim ayrıntıları** sayfasında e-posta adresi belirtin. Hizmet, belirtilen e-posta adreslerine sipariş durumundaki güncelleştirmelerle ilgili bilgi gönderir. 

    Grup yöneticisinin ayrılması durumunda da bildirim almaya devam etmek için bir grup e-postası kullanmanız önerilir.

16. **Özet** sayfasında sipariş, iletişim, bildirim ve gizlilik koşullarıyla ilgili bilgileri gözden geçirin. Gizlilik koşullarını kabul ettiğinizi belirten kutuyu işaretleyin.

17. **Sipariş**'e tıklayın. Siparişin oluşturulması birkaç dakika sürer.

 
## <a name="track-the-order"></a>Siparişi izleme

Siparişi verdikten sonra durumunu Azure önizleme portalından takip edebilirsiniz. Siparişinize gidin ve durumunu görüntülemek için **Genel bakış** sayfasını inceleyin. Portalda işin durumu **Sipariş edildi** olarak görünür. 

![Data Box Disk durumu, sipariş verildi](media/data-box-disk-deploy-ordered/data-box-portal-ordered.png) 

Diskler kullanılabilir durumda değilse bir bildirim gönderilir. Diskler kullanılabilir durumdaysa Microsoft tarafından gönderilecek diskler belirlenir ve disk paketi hazırlanır. Diskler hazırlanırken şu eylemler gerçekleştirilir:

- Diskler AES-128 BitLocker şifrelemesi kullanılarak şifrelenir.  
- Diskler yetkisiz erişimi önlemek için kilitlenir.
- Bu işlem sırasında disklerin kilidini açan destek anahtarı oluşturulur.

Disklerin hazırlanması tamamlandığında portalda sipariş durumu **İşlenen** olarak değişir.

Microsoft ardından disklerinizi hazırlar ve bölgeye uygun gönderim şirketine teslim eder. Diskler gönderildikten sonra bir takip numarası iletilir. Portalda sipariş durumu **Yola çıktı** olarak değişir.



## <a name="cancel-the-order"></a>Siparişi iptal etme

Bu siparişi iptal etmek için Azure önizleme portalında **Genel bakış**'a gidin ve komut çubuğundan **İptal**'e tıklayın. 

Yalnızca diskler sipariş edildikten ve sipariş gönderim için işleme aşamasındayken iptal edebilirsiniz. Sipariş işleme alındıktan sonra iptal edemezsiniz. 

![Siparişi iptal etme](media/data-box-disk-deploy-ordered/cancel-order1.png)

İptal edilen bir siparişi silmek için **Genel bakış**'a gidin ve komut çubuğundan **Sil**'e tıklayın. 


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Data Box Disk'e kaydolma
> * Data Box Disk sipariş etme
> * Siparişi izleme
> * Siparişi iptal etme

Data Box Disk'inizi ayarlama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box Diskinizi ayarlama](./data-box-disk-deploy-set-up.md)


