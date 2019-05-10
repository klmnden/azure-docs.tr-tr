---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: e4b366075cb16f62a0e16b5b06da6fb19ffefdb9
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65508273"
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
