---
author: WenJason
ms.service: databox
ms.topic: include
origin.date: 12/07/2018
ms.date: 02/25/2019
ms.author: v-jay
ms.openlocfilehash: 0a9aaa86d44e71e429f2bfff13a56ddcb1ee2071
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60728406"
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
