---
title: Microsoft Azure Data Box Disk'i geri gönderme | Microsoft Docs
description: Azure Data Box Disk'inizi Microsoft'a nasıl gönderebileceğinizi öğrenmek için bu öğreticiyi kullanın
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 0dd0474ad1ad360fd82cfdf746d2e9837f74833a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58108384"
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
2. Tüm diskleri ve bağlantı kablolarını kabarcıklı naylona sarın ve bunları sevkiyat kutusuna yerleştirin.
3. Kutuya yapıştırılmış şeffaf plastik kılıftaki iade sevkiyat etiketini kullanın. Etiket hasar görmüş veya kaybolmuşsa, Azure portalından yeni bir sevkiyat etiketi indirin ve cihaza yapıştırın. **Genel bakış > Sevkiyat etiketi indir** bölümüne gidin.

    ![Sevkiyat etiketini indirme](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

    Bu eylem aşağıda gösterildiği gibi bir sevkiyat etiketi indirir.

    ![Örnek sevkiyat etiketi](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)

4. Sevkiyat kutusunu mühürleyin ve iade sevkiyat etiketinin görünür olduğundan emin olun.
5. Cihazı ABD'de iade ediyorsanız UPS ile bir toplama zamanı ayarlayın. Cihazı Avrupa'da DHL ile iade ediyorsanız, DHL'in web sitesini ziyaret edip bir havayolu fatura numarası belirterek toplama isteğinde bulunun. Ülkenin DHL Ekspres web sitesine gidin ve **Kurye Çağırma Rezervasyonu > eİade Sevkiyatı**'nı seçin.

    ![İade Sevk irsaliyesi DHL](media/data-box-disk-deploy-picked-up/dhl-ship-1.png)
    
    Konşimento numarasını belirtin ve toplama ayarlaması yapmak için **Toplama Zamanlama**'ya tıklayın.

      ![Toplamayı zamanlama](media/data-box-disk-deploy-picked-up/dhl-ship-2.png)

7. Diskler nakliyeciniz tarafından toplandıktan sonra, portaldaki sipariş durumu **Toplandı** olarak güncelleştirilir. Ayrıca bir takip numarası da görüntülenir.

    ![Diskler toplandı](media/data-box-disk-deploy-picked-up/data-box-portal-pickedup.png)

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

Microsoft diski alıp taradığında, iş durumu **Alındı** olarak güncelleştirilir. 

![Diskler alındı](media/data-box-disk-deploy-picked-up/data-box-portal-received.png)

Diskler Azure veri merkezindeki bir sunucuya bağlandıktan sonra veriler otomatik olarak kopyalanır. Verilerin boyutuna bağlı olarak, kopyalama işleminin tamamlanması birkaç saatten bir güne kadar sürebilir. Kopyalama işinin ilerleme durumunu portalda izleyebilirsiniz.

Kopyalama tamamlandıktan sonra, sipariş durumu **Tamamlandı** olarak güncelleştirilir.

![Veri kopyalama tamamlandı](media/data-box-disk-deploy-picked-up/data-box-portal-completed.png)

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


