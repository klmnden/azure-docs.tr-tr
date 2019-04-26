---
title: Microsoft Azure Data Box'ı geri gönderme | Microsoft Belgeleri
description: Azure Data Box’ınızı Microsoft'a nasıl geri göndereceğinizi öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 03/19/2019
ms.author: alkohli
ms.openlocfilehash: 72d6ce58a986ddd0d0976d99de5ca3426d78f0b9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60463198"
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

1. Cihazın kapalı olduğundan ve kabloların çıkartılmış olduğundan emin olun. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
2. E-ink ekranda gönderi etiketinin görüntülendiğinden emin olun ve taşıyıcınızdan bir teslim alma randevusu alın. Etiketin zarar görmüş veya kayıp veya E-mürekkep ekranda görüntülenen değil, Microsoft Support başvurun. Destek önerir sonra gidebilirsiniz **genel bakış > Sevkiyat Etiketi indirin** Azure portalında. Sevkiyat Etiketi indirin ve cihaza eklemesi. 
3. UPS ile bir toplama cihaz döndüren, zamanlayın. Bir toplama zamanlamak için:

    - Yerel UPS (ücretsiz ülkelere özgü arama numarası) çağırın.
    - Çağrınızda, izleme E-mürekkep görüntülenmesini veya yazdırılan etiketinizin gösterildiği numarası ters sevkiyat teklif.
    - İzleme numarası tırnak içinde değil, UPS alımı sırasında ek bir ücret ödeme yapmanızı gerektirir.

    Toplama zamanlama yerine, ayrıca Data Box'yakın bırakma konumu devre dışı bırakabilir.
4. Data Box nakliyeciniz tarafından toplandıktan ve tarandıktan sonra, portaldaki sipariş durumu **Toplandı** olarak güncelleştirilir. Ayrıca bir takip numarası da görüntülenir.

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


