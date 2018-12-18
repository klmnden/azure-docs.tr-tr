---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: 0a9aaa86d44e71e429f2bfff13a56ddcb1ee2071
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53550587"
---
1. Data Box cihazda oturum. Kilidi açık olduğundan emin olun.

    ![Veri kutusu Panosu](media/data-box-add-device-ip/data-box-connect-via-rest-1.png)

2. Git **kümesi ağ arabirimleri**. İstemciye bağlanmak için kullanılan ağ arabirimi için cihazın IP adresini not edin.

    ![Veri kutusu Panosu](media/data-box-add-device-ip/data-box-connect-via-rest-2.png)

3. Git **Bağlan ve Kopyala** tıklatıp **Rest (Önizleme)**.

    ![Veri kutusu Panosu](media/data-box-add-device-ip/data-box-connect-via-rest-3.png)

4. Gelen **erişim depolama hesabı ve veri yükleme** iletişim kutusunda, kopyalama **Blob Hizmeti uç noktası**.

    ![Veri kutusu Panosu](media/data-box-add-device-ip/data-box-connect-via-rest-4.png)

5. Başlangıç **not defteri** yönetici olarak ve ardından açık **konakları** konumundaki dosya `C:\Windows\System32\Drivers\etc`.
6. İçin şu girişi ekleyin, **konakları** dosyası: `<device IP address> <Blob service endpoint>`
7. Başvuru için aşağıdaki görüntüde kullanın. Kaydet **konakları** dosya.

    ![Veri kutusu Panosu](media/data-box-add-device-ip/data-box-connect-via-rest-5.png)
