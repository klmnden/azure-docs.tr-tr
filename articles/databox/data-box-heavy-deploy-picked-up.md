---
title: Azure veri kutusu ağır geri gönderin için öğretici | Microsoft Docs
description: Azure veri kutusu ağır Microsoft'a göndermeye hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: 84db33e4c7ac612353c590ac9d2904ac3bc48d38
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592383"
---
# <a name="tutorial-return-azure-data-box-heavy-and-verify-data-upload-to-azure"></a>Öğretici: Azure veri kutusu ağır dönün ve verileri karşıya yükleme azure'a doğrulayın


Bu öğreticide, Azure veri kutusu ağır dönün ve verileri Azure'a karşıya doğrulamanın nasıl yapılacağı açıklanır.

Bu öğreticide şu gibi konular hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Göndermeye hazırlama
> * Veri kutusu ağır Microsoft'a gönderin
> * Azure'a verilerin yüklendiğini doğrulama
> * Verileri veri kutusu ağır silinme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce emin olun:

- Tamamladınız [Öğreticisi: Azure Data Box için verileri kopyalayıp doğrulayın](data-box-heavy-deploy-copy-data.md).
- Kopyası işleri tamamlandı. Göndermeye hazırlama kopyası işleri sürmekte olan çalıştıramazsınız.

## <a name="prepare-to-ship"></a>Göndermeye hazırlama

[!INCLUDE [data-box-heavy-prepare-to-ship](../../includes/data-box-heavy-prepare-to-ship.md)]

## <a name="ship-data-box-heavy-back"></a>Sevk veri kutusu ağır geri

1. Cihaz kapalı ve kabloların kaldırılır emin olun. Biriktirme ve güvenli bir şekilde erişebileceğiniz cihazın yeniden tepsisinde 4 güç kablosu yerleştirin.
2. Cihaz aracılığıyla ABD'deki FedEx ve AB'deki DHL LTL freight ile birlikte gelir.

    1. Ulaşın [veri kutusu işlemleri](mailto:DataBoxOps@microsoft.com) alma ile ilgili bilgilendirmek için ve iade Sevkiyat Etiketi alın.
    2. Sevkiyat operatörünüz toplamayı zamanla için yerel numarayı arayın.
    3. Sevkiyat Etiketi sevkiyat dış üzerinde göze çarpacak şekilde görüntülendiğinden emin olun.
    4. Önceki sevkiyat eski sevkiyat etiketlerinden CİHAZDAN kaldırıldığından emin emin olun.
3. Veri kutusu ağır toplanma ve, operatörünüz tarafından taranan sonra portalda siparişinizin durumu güncelleştirmeleri için **toplanmış**. Ayrıca bir takip numarası da görüntülenir.

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

Microsoft cihazı alıp taradığında, sipariş durumu **Alındı** olarak güncelleştirilir. Ardından cihaz hasar veya kurcalama belirtileri için fiziksel doğrulama sürecinden geçer.

Doğrulama tamamlandıktan sonra veri kutusu ağır Azure veri merkezi ağa bağlı. Veri kopyalaması otomatik olarak başlar. Verilerin boyutuna bağlı olarak, kopyalama işleminin tamamlanması birkaç saatten bir güne kadar sürebilir. Kopyalama işinin ilerleme durumunu portalda izleyebilirsiniz.

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

## <a name="erasure-of-data-from-data-box-heavy"></a>Verileri veri kutusu ağır silinme
 
Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler. Silme işlemi tamamlandıktan sonra [siparişi geçmişi indirme](data-box-portal-admin.md#download-order-history).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Göndermeye hazırlama
> * Veri kutusu ağır Microsoft'a gönderin
> * Azure'a verilerin yüklendiğini doğrulama
> * Verileri veri kutusu ağır silinme

Veri kutusu ağır yerel web kullanıcı Arabirimi üzerinden yönetme hakkında bilgi edinmek için aşağıdaki makaleye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box'ı yönetmek için yerel web arabirimini kullanma](./data-box-local-web-ui-admin.md)


