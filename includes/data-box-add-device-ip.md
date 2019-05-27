---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: e4b366075cb16f62a0e16b5b06da6fb19ffefdb9
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66150716"
---
1. Data Box cihazda oturum. Kilidi açık olduğundan emin olun.

    ![Veri kutusu Panosu](media/data-box-add-device-ip/data-box-connect-via-rest-1.png)

2. Git **kümesi ağ arabirimleri**. İstemciye bağlanmak için kullanılan ağ arabirimi için cihazın IP adresini not edin.

    ![Veri kutusu Panosu](media/data-box-add-device-ip/data-box-connect-via-rest-2.png)

3. Git **Bağlan ve Kopyala** tıklatıp **Rest**.

    ![Veri kutusu Panosu](media/data-box-add-device-ip/data-box-connect-via-rest-3.png)

4. Gelen **erişim depolama hesabı ve veri yükleme** iletişim kutusunda, kopyalama **Blob Hizmeti uç noktası**.

    ![Veri kutusu Panosu](media/data-box-add-device-ip/data-box-connect-via-rest-4.png)

5. Başlangıç **not defteri** yönetici olarak ve ardından açık **konakları** konumundaki dosya `C:\Windows\System32\Drivers\etc`.
6. İçin şu girişi ekleyin, **konakları** dosyası: `<device IP address> <Blob service endpoint>`
7. Başvuru için aşağıdaki görüntüde kullanın. Kaydet **konakları** dosya.

    ![Veri kutusu Panosu](media/data-box-add-device-ip/data-box-connect-via-rest-5.png)
