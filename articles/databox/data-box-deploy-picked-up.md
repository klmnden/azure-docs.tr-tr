---
title: Azure Data Box'ı geri göndermeye Öğreticisi | Microsoft Docs
description: Azure Data Box’ınızı Microsoft'a nasıl geri göndereceğinizi öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 7/08/2019
ms.author: alkohli
ms.openlocfilehash: db0f0ac3073687b7c1cd8ca60e459e4bb3aa03f4
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626353"
---
# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>Öğretici: Azure Data Box dönün ve verileri karşıya yükleme azure'a doğrulayın

Bu öğretici, Azure Data Box’ın nasıl iade edileceğini ve yüklenen verileri nasıl doğrulayabileceğinizi anlatır.

Bu öğreticide şu gibi konular hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Göndermeye hazırlama
> * Data Box'ı Microsoft'a gönderme
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box'tan verileri silme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce emin olun:

- Seçtiğiniz tamamladınız [Öğreticisi: Azure Data Box için verileri kopyalayıp doğrulayın](data-box-deploy-copy-data.md). 
- Kopyası işleri tamamlandı. Göndermeye hazırlama kopyası işleri sürmekte olan çalıştıramazsınız.

## <a name="prepare-to-ship"></a>Göndermeye hazırlama

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

## <a name="ship-data-box-back"></a>Data Box'ı geri gönderme

Cihazına veri kopyalama işleminin tamamlandığından emin olun ve **göndermeye hazırlama** çalıştırma başarılı olur. Burada cihaz sevkiyat bölgeye göre yordam farklılık gösterir.


### <a name="ship-in-us-canada-europe"></a>ABD, Kanada, Avrupa'da gönderin

Cihaz BİZE, Kanada veya Avrupa döndüren, aşağıdaki adımları uygulayın.

1. Cihaz kapalı ve kablolarını kaldırılır emin olun. 
2. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
3. E-ink ekranda gönderi etiketinin görüntülendiğinden emin olun ve taşıyıcınızdan bir teslim alma randevusu alın. Etiketin zarar görmüş veya kayıp veya E-mürekkep ekranda görüntülenen değil, Microsoft Support başvurun. Destek önerir sonra gidebilirsiniz **genel bakış > Sevkiyat Etiketi indirin** Azure portalında. Sevkiyat Etiketi indirin ve cihaza eklemesi. 
4. UPS ile bir toplama cihaz döndüren, zamanlayın. Bir toplama zamanlamak için:

    - Yerel UPS (ücretsiz ülkeye/bölgeye özgü arama numarası) çağırın.
    - Çağrınızda, izleme E-mürekkep görüntülenmesini veya yazdırılan etiketinizin gösterildiği numarası ters sevkiyat teklif.
    - İzleme numarası tırnak içinde değil, UPS alımı sırasında ek bir ücret ödeme yapmanızı gerektirir.

    Toplama zamanlama yerine, ayrıca Data Box'yakın bırakma konumu devre dışı bırakabilir.
4. Data Box nakliyeciniz tarafından toplandıktan ve tarandıktan sonra, portaldaki sipariş durumu **Toplandı** olarak güncelleştirilir. Ayrıca bir takip numarası da görüntülenir.

### <a name="ship-in-asia-pacific-region"></a>Asya Pasifik bölgesinde gönderin

#### <a name="ship-in-australia"></a>Avustralya'da gönderin

Bir ek güvenlik bildirimi Avustralya'da Azure veri merkezleri vardır. Tüm gelen sevkiyat, Gelişmiş bir bildirim olması gerekir. Avustralya'da sevk etmek için aşağıdaki adımları uygulayın.


1. Cihaz iade sevk irsaliyesi için sevkiyat için kullanılan özgün kutusunu saklar.
2. Cihazına veri kopyalama tamamlandı olduğundan emin olun ve **Çalıştır göndermeye hazırlama** başarılı olur.
3. Cihazı kapatıp güç ve kabloların kaldırın.
4. Biriktirme ve güvenli bir şekilde aygıtın arkasında cihaz ile sağlanan güç kablosu yerleştirin.
5. Bir toplama istemek için e-posta Quantium çözümler. Azure portalında belirtilen hizmet başvuru numarası bakın. Aşağıdaki e-posta şablonu kullanın:- *ters Sevkiyat Etiketi TAU koduyla iste*. E-postada aşağıdaki ayrıntıları eklediğinizden emin olun: 

    ```
    To: Azure@quantiumsolutions.com
    Subject: Pickup request for Azure｜Reference number：XXX XXX XXX
    Body: 
    - Company name：
    - Address:
    - Contact name:
    - Contact number:
    - Requested pickup date: mm/dd
    ```
6. Quantium çözümleri Avustralya bir iade sevkiyat etiketini posta gönderir.
7. İade etiketini yazdırabilir ve teslimat kutusundaki eklemesi.
8. Paket Courier'e veriyorsunuz.

Gerekirse, Quantium çözüm Destek e-posta gönderebilirsiniz Azure@quantiumsolutions.com veya telefon.


Siparişinizi telefonla ile ilgili daha fazla soru için:

- İlk toplama için bir e-posta gönderin.
- Telefonda emri adınızı sağlayın.

#### <a name="ship-in-japan"></a>Japonya'da gönderin 

1. Cihaz iade sevk irsaliyesi için sevkiyat için kullanılan özgün kutusunu saklar.
2. Cihazı kapatıp güç ve kabloların kaldırın.
3. Biriktirme ve güvenli bir şekilde aygıtın arkasında cihaz ile sağlanan güç kablosu yerleştirin.
4. Şirketiniz konsinye unutmayın ad ve adres bilgilerini gönderen bilgilerinizi olarak yazın.
5. Aşağıdaki e-posta şablonu kullanarak Quantium çözüm e-posta.

    - Japonya Post Chakubarai konsinye Not dahil edilmedi veya eksik, bu e-postada unutmayın. Quantium çözümleri Japonya Japonya konsinye Not alımı sırasında getirmek için Post ister.
    - Birden çok siparişler varsa, tek tek toplama emin olmak için e-posta.

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Azure Data Box｜Job name： 
    Body: 
    - Japan Post Yu-Pack tracking number (reference number)：
    - Requested pickup date：mmdd (Select a requested time slot from below).
    a. 08：00-13：00 
    b. 13：00-15：00 
    c. 15：00-17：00 
    d. 17：00-19：00 
    ```

3. Bir toplama ayrılmış sonra bir e-posta onayı Quantium çözümlerinden alırsınız. E-posta onayı, ayrıca Chakubarai konsinye not hakkındaki bilgileri içerir.

Gerekirse, aşağıdaki bilgileri Quantium çözümü desteği (Japonca) başvurabilirsiniz: 

- E-posta:Customerservice.JP@quantiumsolutions.com 
- Telefon: 03-5755-0150 


## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

Microsoft cihazı alıp taradığında, sipariş durumu **Alındı** olarak güncelleştirilir. Ardından cihaz hasar veya kurcalama belirtileri için fiziksel doğrulama sürecinden geçer.

Doğrulama tamamlandıktan sonra Data Box, Azure veri merkezindeki ağa bağlanır. Veri kopyalaması otomatik olarak başlar. Verilerin boyutuna bağlı olarak, kopyalama işleminin tamamlanması birkaç saatten bir güne kadar sürebilir. Kopyalama işinin ilerleme durumunu portalda izleyebilirsiniz.

Kopyalama tamamlandıktan sonra, sipariş durumu **Tamamlandı** olarak güncelleştirilir.

Kaynaktan silmeden önce verilerinizi Azure'a karşıya yüklendiğini doğrulayın. Verilerinizi olabilir:

- Azure depolama hesabınızda veya hesaplarınızda. Data Box'a veri kopyaladığınızda, türlerine bağlı olarak bu veriler Azure Depolama hesabınızda aşağıdaki yollardan birine yüklenir.

  - Blok blobları ve sayfa blobları için: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
  - Azure Dosyaları için: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

    Alternatif olarak Azure portalda Azure depolama hesabınıza gidip oradan ilerleyebilirsiniz.

- Yönetilen disk kaynak grupları. Yönetilen diskler oluştururken VHD'ler sayfa blobları karşıya ve sonra yönetilen disklere dönüştürülmüş. Yönetilen diskler, sipariş oluşturma sırasında belirtilen kaynak gruplarına eklenir. 

    - Azure'da yönetilen disklere kopyanızı başarılı olduysa, gidebilirsiniz **sipariş ayrıntıları** Azure portalı ve kaynak gruplarını Not Yönetilen diskler için belirtilen olun.

        ![Yönetilen disk kaynak gruplarını tanımlayın](media/data-box-deploy-copy-data-from-vhds/order-details-managed-disk-resource-groups.png)

        Belirtilen bir kaynak grubuna gidin ve yönetilen disklerinizi bulun.

        ![Yönetilen kaynak grubuna bağlı disk](media/data-box-deploy-copy-data-from-vhds/managed-disks-resource-group.png)

    - Bir VHDX veya dinamik ve fark VHD kopyaladıysanız, VHDX/VHD bir sayfa blobu ancak VHD dönüştürme yönetilen diski başarısız olarak hazırlama depolama hesabına yüklenir. Git, hazırlama **depolama hesabı > Blobları** ve ardından uygun bir kapsayıcı - standart SSD, HDD standart veya Premium SSD seçin. VHD'ler sayfa BLOB'ları hazırlama depolama hesabınızdaki olarak karşıya yüklenir.

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme
 
Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Göndermeye hazırlama
> * Data Box'ı Microsoft'a gönderme
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box'tan verileri silme

Data Box’ı yerel web arabirimini kullanarak yönetmeyi öğrenmek için şu makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Data Box'ı yönetmek için yerel web arabirimini kullanma](./data-box-local-web-ui-admin.md)


