---
title: Microsoft Azure Data Box'ı geri gönderme | Microsoft Belgeleri
description: Azure Data Box’ınızı Microsoft'a nasıl geri göndereceğinizi öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 01/24/2019
ms.author: alkohli
ms.openlocfilehash: 158c2a8919bccea03f5c7b67aef23cd07022c259
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54900519"
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
2. Cihaz ABD içinde gönderiliyorsa e-ink ekranda sevkiyat etiketinin görüntülendiğinden emin olun ve taşıyıcınızdan bir teslim alma randevusu alın. Etiket zarar görmüş veya kayıp ya da E-mürekkep ekranda görüntülenmez, Git **genel bakış > Sevkiyat Etiketi indirin** Azure portalında. Sevkiyat Etiketi indirin ve cihaza eklemesi.

    Cihaz Avrupa'da gönderiliyorsa E-ink ekranda sevkiyat etiketi gösterilmez. Bunun yerine iade etiketi, sevkiyat etiketinin altındaki şeffaf bölümde yer alır. Eski sevkiyat etiketini sökün ve yenisinin net bir şekilde göründüğünden emin olun.
    
3. UPS ile bir toplama cihaz döndüren, zamanlayın. Bir toplama zamanlamak için yerel UPS (ücretsiz ülkelere özgü arama numarası) çağrı yapma veya en yakın bırakma konumu Data Box'devre dışı bırakın.

4. Data Box nakliyeciniz tarafından toplandıktan ve tarandıktan sonra, portaldaki sipariş durumu **Toplandı** olarak güncelleştirilir. Ayrıca bir takip numarası da görüntülenir.

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

Microsoft cihazı alıp taradığında, sipariş durumu **Alındı** olarak güncelleştirilir. Ardından cihaz hasar veya kurcalama belirtileri için fiziksel doğrulama sürecinden geçer.

Doğrulama tamamlandıktan sonra Data Box, Azure veri merkezindeki ağa bağlanır. Veri kopyalaması otomatik olarak başlar. Verilerin boyutuna bağlı olarak, kopyalama işleminin tamamlanması birkaç saatten bir güne kadar sürebilir. Kopyalama işinin ilerleme durumunu portalda izleyebilirsiniz.

Kopyalama tamamlandıktan sonra, sipariş durumu **Tamamlandı** olarak güncelleştirilir.

Kaynaktan silmeden önce verilerinizin depolama hesaplarında olduğundan emin olun. Data Box'a veri kopyaladığınızda, türlerine bağlı olarak bu veriler Azure Depolama hesabınızda aşağıdaki yollardan birine yüklenir.

- Blok blobları ve sayfa blobları için: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
- Azure Dosyaları için: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

Alternatif olarak Azure portalda Azure depolama hesabınıza gidip oradan ilerleyebilirsiniz.

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


