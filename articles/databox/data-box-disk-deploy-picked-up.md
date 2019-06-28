---
title: Azure Data Box Disk geri göndermeye Öğreticisi | Microsoft Docs
description: Azure Data Box Disk'inizi Microsoft'a nasıl gönderebileceğinizi öğrenmek için bu öğreticiyi kullanın
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 06/25/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 7e7a1f119a2f2b0e60645cb776b26c124910cacb
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448204"
---
# <a name="tutorial-return-azure-data-box-disk-and-verify-data-upload-to-azure"></a>Öğretici: Azure Data Box Disk geri dönün ve verileri karşıya yükleme azure'a doğrulayın

Bu serinin son Öğreticisi. Azure Data Box Disk dağıtın. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Data Box Disk'i Microsoft'a gönderme
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box Disk'ten verileri silinme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce tamamladığınızdan emin olun [Öğreticisi: Veri için Azure Data Box Disk kopyalama ve doğrulama](data-box-disk-deploy-copy-data.md).

## <a name="ship-data-box-disk-back"></a>Data Box Disk'i geri gönderme

1. Veri doğrulama tamamlandıktan sonra diskleri çıkarın. Bağlantı kablolarını çıkarın.
2. Tüm diskleri ve bağlantı kablolarını kabarcıklı naylona sarın ve bunları sevkiyat kutusuna yerleştirin. Donatılar eksikse ücretleri uygulanabilir.
    - İlk sevk gelen paketleme yeniden kullanın.  
    - İyi güvenli kabarcıklanma sona erdi kullanarak diskleri paketi öneririz.
    - Kutunun içinde tüm hareketleri azaltmak için snug uygun olduğundan emin olun.

Sonraki adımlar, cihazın burada iade ettiğiniz tarafından belirlenir.

### <a name="pick-up-in-us-canada"></a>BİZE, Kanada seçin

ABD veya Kanada cihaz döndüren, aşağıdaki adımları uygulayın.

1. Kutuya yapıştırılmış şeffaf plastik kılıftaki iade sevkiyat etiketini kullanın. Etiket hasar görür veya varsa:
    - **Genel bakış > Sevkiyat etiketi indir** bölümüne gidin.

        ![Sevkiyat etiketini indirme](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

        Bu eylem aşağıda gösterildiği gibi bir sevkiyat etiketi indirir.

        ![Örnek sevkiyat etiketi](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)
    - Cihaz etiketi eklemesi.

2. Sevkiyat kutusunu mühürleyin ve iade sevkiyat etiketinin görünür olduğundan emin olun.
3. UPS ile bir toplama zamanlayın. Bir toplama zamanlamak için:

    - Yerel UPS (ücretsiz ülkeye/bölgeye özgü arama numarası) çağırın.
    - Çağrınızda, izleme yazdırılan etiketinizin gösterildiği numarası ters sevkiyat teklif.
    - İzleme numarası tırnak içinde değil, UPS alımı sırasında ek bir ücret ödeme yapmanızı gerektirir.
    - Toplama zamanlama yerine, ayrıca en yakın bırakma konumu Data Box Disk devre dışı bırakabilir.


### <a name="pick-up-in-europe"></a>Avrupa'da öğrenilip

Cihaz Avrupa'da döndüren, aşağıdaki adımları uygulayın.

1. Kutuya yapıştırılmış şeffaf plastik kılıftaki iade sevkiyat etiketini kullanın. Etiket hasar görür veya varsa:
    - **Genel bakış > Sevkiyat etiketi indir** bölümüne gidin.

        ![Sevkiyat etiketini indirme](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

        Bu eylem aşağıda gösterildiği gibi bir sevkiyat etiketi indirir.

        ![Örnek sevkiyat etiketi](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)
    - Cihaz etiketi eklemesi.

2. Sevkiyat kutusunu mühürleyin ve iade sevkiyat etiketinin görünür olduğundan emin olun.
3. Cihazı Avrupa'da DHL ile iade ediyorsanız, DHL'in web sitesini ziyaret edip bir havayolu fatura numarası belirterek toplama isteğinde bulunun.
4. Ülke/bölge DHL Express Web sitesine gidin ve seçin **Courier koleksiyonu kitap > sevkiyat eReturn**.

    ![İade Sevk irsaliyesi DHL](media/data-box-disk-deploy-picked-up/dhl-ship-1.png)
    
3. Konşimento numarasını belirtin ve toplama ayarlaması yapmak için **Toplama Zamanlama**'ya tıklayın.

      ![Toplamayı zamanlama](media/data-box-disk-deploy-picked-up/dhl-ship-2.png)

### <a name="pick-up-in-asia-pacific-region"></a>Asya Pasifik bölgesinde öğrenilip

Bu bölge, Japonca, Kore, Avustralya ve Singapur ilgili yönergeleri içerir.

#### <a name="pick-up-in-australia"></a>Avustralya'da öğrenilip

Bir ek güvenlik bildirimi Avustralya'da Azure veri merkezleri vardır. Tüm gelen sevkiyat, Gelişmiş bir bildirim olması gerekir. Avustralya'da alma için aşağıdaki adımları uygulayın.

1. E-posta `adbops@microsoft.com` isteği Sevkiyat Etiketi gelen benzersiz kimliği veya TAU kod için. İstek zaman etiketi almak için planlanan sunulduğundan Ücretlerde en az 3 gün yerleştirin.
2. E-posta konu olmalıdır - *ters Sevkiyat Etiketi TAU koduyla iste*. E-postada aşağıdaki ayrıntıları eklediğinizden emin olun: 

    - Sipariş adı
    - Adres
    - Kişi adı

#### <a name="pick-up-in-japan"></a>Japonya'da öğrenilip

1. Şirketiniz konsinye unutmayın ad ve adres bilgilerini gönderen bilgilerinizi olarak yazın.
2. Aşağıdaki e-posta şablonu kullanarak Quantium çözüm e-posta.

    - Japonya Post Chakubarai konsinye Not dahil edilmedi veya eksik, bu e-postada unutmayın. Quantium çözümleri Japonya Japonya konsinye Not alımı sırasında getirmek için Post ister.
    - Birden çok siparişler varsa, tek tek toplama emin olmak için e-posta.

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Azure Data Box Disk｜Job Name： 
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

#### <a name="pick-up-in-korea"></a>Kore'de öğrenilip

1. Dönüş konsinye Not eklediğinizden emin olun.
2. Toplama konsinye Not bulunduğunda istemek için:
    1. Çağrı *Quantium Solutions International* hattını 8231 070 1418 office saatleri (10: 00 için 17: 00, Pazartesi-Cuma). Teklif *Microsoft Azure alma* ve düzenlemek için bir koleksiyon için hizmet isteği sayısı.  
    2. Hattı meşgul ise, e-posta `microsoft@rocketparcel.com`, e-posta konusu ile *Microsoft Azure alma* ve başvuru olarak hizmet isteği sayısı.
    3. Courier koleksiyonu için gelmezse, çağrı *Quantium Solutions International* hattı alternatif düzenlemeleri için. 
    4. Onay alma zamanlama için bir e-postası alırsınız.
3. Yalnızca konsinye Not mevcut değilse, bu adımı uygulayın. Toplama istemek için:
    1. Çağrı *Quantium Solutions International* hattını 8231 070 1418 office saatleri (10: 00 için 17: 00, Pazartesi-Cuma). Teklif *Microsoft Azure alma* ve düzenlemek için bir koleksiyon için hizmet isteği sayısı. Bir koleksiyon için düzenlemek için yeni konsinye Not gerektiğini belirtin. (Müşteri) gönderen, alıcı bilgi (Azure veri merkezi) ve başvuru numarası (hizmet isteği) sağlar. 
    2. Hattı meşgul ise, e-posta `microsoft@rocketparcel.com`, e-posta konusu ile *Microsoft Azure alma* ve başvuru olarak hizmet isteği sayısı.
    3. Courier koleksiyonu için gelmezse, çağrı *Quantium Solutions International* hattı alternatif düzenlemeleri için. 
    4. Telefon istek yapıldığında sözlü onaylamanız istenir.

### <a name="pick-up-in-singapore"></a>Singapur içinde öğrenilip

1. Sevkiyat etiketi yazdırma ve kutunun ekleyin. Etiket hasar görür veya varsa:
    - **Genel bakış > Sevkiyat etiketi indir** bölümüne gidin.

        ![Sevkiyat etiketini indirme](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

        Bu eylem aşağıda gösterildiği gibi bir sevkiyat etiketi indirir.

        ![Örnek sevkiyat etiketi](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)
    - Cihaz etiketi eklemesi. Etiket görünür olduğundan emin olun.

2. Toplama istemek için:
    - Çağrı **SingPost** hattını **6845 6485** office saatleri (09: 00 için 17: 00, Pazartesi-Cuma).  
    - Teklif *Microsoft Azure alma* ve numarası (izleme iade sevkiyat etiketini) düzenlemek için bir koleksiyon için istek hizmeti. 
    - Toplama zamanlama için sözlü bir onay alırsınız. 
    - Courier koleksiyonu için gelmezse, çağrı **SingPost** adresindeki **6845 6485** alternatif düzenlemeleri için. 
3. Courier'e el. 


## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

Diskler nakliyeciniz tarafından toplandıktan sonra, portaldaki sipariş durumu **Toplandı** olarak güncelleştirilir. Ayrıca bir takip numarası da görüntülenir.

![Diskler toplandı](media/data-box-disk-deploy-picked-up/data-box-portal-pickedup.png)

Microsoft diski alıp taradığında, iş durumu **Alındı** olarak güncelleştirilir. 

![Diskler alındı](media/data-box-disk-deploy-picked-up/data-box-portal-received.png)

Diskler Azure veri merkezindeki bir sunucuya bağlandıktan sonra veriler otomatik olarak kopyalanır. Verilerin boyutuna bağlı olarak, kopyalama işleminin tamamlanması birkaç saatten bir güne kadar sürebilir. Kopyalama işinin ilerleme durumunu portalda izleyebilirsiniz.

Kopyalama tamamlandıktan sonra, sipariş durumu **Tamamlandı** olarak güncelleştirilir.

![Veri kopyalama tamamlandı](media/data-box-disk-deploy-picked-up/data-box-portal-completed.png)

Kopyalama hatalarla tamamlanırsa görmeniz [karşıya yükleme hatalarını giderme](data-box-disk-troubleshoot-upload.md).

Kaynaktan silmeden önce verilerinizin depolama hesaplarında olduğundan emin olun. Verilerinizi olabilir:

- Azure depolama hesabınızda veya hesaplarınızda. Data Box'a veri kopyaladığınızda, türlerine bağlı olarak bu veriler Azure Depolama hesabınızda aşağıdaki yollardan birine yüklenir.

  - Blok blobları ve sayfa blobları için: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
  - Azure Dosyaları için: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

    Alternatif olarak Azure portalda Azure depolama hesabınıza gidip oradan ilerleyebilirsiniz.

- Yönetilen disk kaynak grupları. Yönetilen diskler oluştururken VHD'ler sayfa blobları karşıya ve sonra yönetilen disklere dönüştürülmüş. Yönetilen diskler, sipariş oluşturma sırasında belirtilen kaynak gruplarına eklenir.

  - Azure'da yönetilen disklere kopyanızı başarılı olduysa, gidebilirsiniz **sipariş ayrıntıları** Azure portalı ve kaynak grubunu Not Yönetilen diskler için belirtilen olun.

      ![Sipariş ayrıntılarını görüntüle](media/data-box-disk-deploy-picked-up/order-details-resource-group.png)

    Belirtilen bir kaynak grubuna gidin ve yönetilen disklerinizi bulun.

      ![Yönetilen diskler için kaynak grubu](media/data-box-disk-deploy-picked-up/resource-group-attached-managed-disk.png)

  - Kopyaladığınız bir VHDX veya dinamik ve fark VHD, VHDX/VHD hazırlama depolama hesabına bir blok blobu yüklenir. Git, hazırlama **depolama hesabı > Blobları** ve ardından uygun bir kapsayıcı - StandardSSD, StandardHDD veya PremiumSSD seçin. VHDX/VHD hazırlama depolama hesabındaki blok blobları olarak gösteriliyor olmalıdır.

Verilerin Azure'a yüklendiğini doğrulamak için aşağıdaki adımları izleyin:

1. Disk siparişinizle ilişkilendirilmiş depolama hesabına gidin.
2. **Blob hizmeti > Bloblara göz atın** seçeneğine gidin. Kapsayıcı listesi gösterilir. *BlockBlob* ve *PageBlob* klasörlerinin altında oluşturduğunuz alt klasöre karşılık olarak, depolama hesabınızda aynı adlı kapsayıcılar oluşturulur.
    Klasör adları Azure adlandırma kurallarına uymuyorsa, Azure'a veri yükleme işlemi başarısız olur.

4. Veri kümesinin tamamının yüklendiğini doğrulamak için, Microsoft Azure Depolama Gezgini'ni kullanın. Disk kiralama siparişine karşılık gelen depolama hesabını ekleyin ve ardından blob kapsayıcılarının listesine bakın. Bir kapsayıcı seçin, **…Diğer** düğmesine tıklayın ve sonra da **Klasör istatistikleri**'ne tıklayın. **Etkinlikler** bölmesinde, blob sayısı ve toplam blob boyutu gibi o klasöre özgü istatistikler görüntülenir. Bayt cinsinden toplam blob boyutu, veri kümesinin boyutuyla eşleşmelidir.

    ![Depolama Gezgini'nde klasör istatistikleri](media/data-box-disk-deploy-picked-up/folder-statistics-storage-explorer.png)

## <a name="erasure-of-data-from-data-box-disk"></a>Data Box Disk'ten verileri silinme

Kopyalama tamamlandıktan ve siz de verilerin Azure depolama hesabında olduğunu doğruladıktan sonra, diskler NIST standardına uygun olarak güvenle silinir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box Disk konularını öğrendiniz:

> [!div class="checklist"]
> * Data Box Disk'i Microsoft'a gönderme
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box Disk'ten verileri silinme


Azure portalı yoluyla Data Box Disk'i yönetmeyi öğrenmek için bir sonraki nasıl yapılır makalesine geçin.

> [!div class="nextstepaction"]
> [Azure portalını kullanarak Azure Data Box Disk'i yönetme](./data-box-portal-ui-admin.md)


