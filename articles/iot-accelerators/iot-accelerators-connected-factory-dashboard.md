---
title: Bağlı Fabrika panosunu - Azure | Microsoft Docs
description: Bağlı Fabrika panosunu, özelliklerin nasıl kullanılacağını anlayın.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: dobett
ms.openlocfilehash: 05befc72f512f502456f798f25b6c72571451363
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61451043"
---
# <a name="use-features-in-the-connected-factory-solution-accelerator-dashboard"></a>Bağlı Fabrika çözüm Hızlandırıcı Pano özelliklerini kullanma

[Endüstriyel IOT cihazlarımı yönetmek için bulut tabanlı bir çözüm dağıtma](quickstart-connected-factory-deploy.md) hızlı başlangıç, panoya gidin ve yanıt için uyarılar nasıl gösterdi. Bu nasıl yapılır kılavuzunda, endüstriyel IOT cihazlarınızı yönetmek ve izlemek için kullanabileceğiniz bazı ek Pano özelliklerini gösterir.

## <a name="apply-filters"></a>Filtreleri uygulama

Panoda ya da görüntülenen bilgileri filtreleyebilirsiniz içinde **Fabrika konumları** paneli veya **alarmlar** paneli:

1. Fabrika konumları veya alarmlar panelinde kullanılabilir filtrelerin listesini görüntülemek için **huni** simgesine tıklayın.

1. Filtreler paneli görüntülenir:

    [![Bağlı Fabrika çözümü Hızlandırıcı filtreleri](./media/iot-accelerators-connected-factory-dashboard/filterpanel-inline.png)](./media/iot-accelerators-connected-factory-dashboard/filterpanel-expanded.png#lightbox)

1. Gerektirir ve filtreyi seçin **Uygula**. Filtre alanlarına serbest metin yazmak mümkündür.

1. Filtre sonra uygulanır. Bir filtre uygulanır ek huni simgesi gösterir:

    [![Bağlı Fabrika çözüm Hızlandırıcı filtresi uygulandı](./media/iot-accelerators-connected-factory-dashboard/filterapplied-inline.png)](./media/iot-accelerators-connected-factory-dashboard/filterapplied-expanded.png#lightbox)

    > [!NOTE]
    > Etkin bir filtre görüntülenen OEE ve KPI değerlerini etkilemez, yalnızca liste içeriğini filtreler.

1. Bir filtreyi temizlemek için huniye tıklayın ve **Temizle** filtre paneli içinde.

## <a name="browse-an-opc-ua-server"></a>OPC UA sunucusuna göz atma

Çözüm Hızlandırıcısını dağıttığınızda, bir dizi panodan göz atabileceğiniz sanal OPC UA sunucularını otomatik olarak hazırlarsınız. Sanal sunucular ile gerçek sunucuları dağıtmaya gerek olmadan çözüm Hızlandırıcısını denemenizi kolaylaştırır. Çözüme gerçek bir OPC UA sunucusu bağlamak istiyorsanız bkz [OPC UA Cihazınızı bağlı Fabrika çözüm hızlandırıcısına bağlamayı](iot-accelerators-connected-factory-gateway-deployment.md) öğretici.

1. Tıklayın **tarayıcı simgesine** Pano gezinti çubuğunda:

    [![Bağlı Fabrika çözümü Hızlandırıcı sunucu tarayıcısı](./media/iot-accelerators-connected-factory-dashboard/browser-inline.png)](./media/iot-accelerators-connected-factory-dashboard/browser-expanded.png#lightbox)

1. Çözüm Hızlandırıcısı sizin için dağıtılan sunucular gösterilir listeden sunuculardan birini seçin:

    [![Bağlı Fabrika çözüm Hızlandırıcı sunucu listesi](./media/iot-accelerators-connected-factory-dashboard/serverlist-inline.png)](./media/iot-accelerators-connected-factory-dashboard/serverlist-expanded.png#lightbox)

1. **Bağlan**’a tıklayın; güvenlik iletişim kutusu görüntülenir. Benzetimi için tıklatın güvenli **İlerle**.

1. Sunucu ağacındaki düğümlerden herhangi birini genişletmek için düğüme tıklayın. Yayımlama telemetrisi olan düğümlerin yanında bir onay işareti vardır:

    [![Bağlı Fabrika çözümü Hızlandırıcı sunucu ağacı](./media/iot-accelerators-connected-factory-dashboard/servertree-inline.png)](./media/iot-accelerators-connected-factory-dashboard/servertree-expanded.png#lightbox)

1. Düğümü okumak, yazmak, yayımlamak veya çağırmak için bir öğeye sağ tıklayın. Kullanabileceğiniz eylemler, izinlerinize ve düğümün özniteliklerine bağlıdır. Okuma seçeneği belirli bir düğümün değerini gösteren bağlam panelini görüntüler. Yazma seçeneği yeni değer girebileceğiniz bir bağlam paneli görüntüler. Çağırma seçeneği çağrı için parametreleri girebileceğiniz bir düğüm görüntüler.

## <a name="publish-a-node"></a>Düğümü yayımlama

*Sanal OPC UA sunucusuna* göz attığınızda, yeni düğümleri yayımlamayı da seçebilirsiniz. Çözümde bu düğümlerden gelen telemetriyi analiz edebilirsiniz. Bunlar *sanal OPC UA sunucuları* gerçek cihaza dağıtmaya gerek olmadan çözüm Hızlandırıcı ile denemeler kolaylaştırır:

1. OPC UA sunucusu tarayıcı ağacında yayımlamak istediğiniz düğüme göz atın.

1. Düğüme sağ tıklayın. Tıklayın **yayımlama**:

    [![Bağlı Fabrika Çözüm Hızlandırıcısı yayımlama düğümü](./media/iot-accelerators-connected-factory-dashboard/publishnode-inline.png)](./media/iot-accelerators-connected-factory-dashboard/publishnode-expanded.png#lightbox)

1. Yayımlama işleminin başarılı olduğunu bildiren bir bağlam paneli görüntülenir. Düğüm, istasyon düzeyi görünümünde yanında bir onay işareti ile görünür:

    [![Bağlı Fabrika Çözüm Hızlandırıcısı yayımlama başarısı](./media/iot-accelerators-connected-factory-dashboard/publishsuccess-inline.png)](./media/iot-accelerators-connected-factory-dashboard/publishsuccess-expanded.png#lightbox)

## <a name="command-and-control"></a>Komut ve denetim

Bağlı Fabrika, endüstri cihazlarınızı doğrudan buluttan kumanda etmenize ve denetlemenize olanak tanır. Cihaz tarafından oluşturulan alarmları yanıtlamak için bu özelliği kullanabilirsiniz. Örneğin, cihaza bir baskı yayın Vana açmak için bir komut gönderebilir. Kullanılabilir komutları, OPC UA sunucuları tarayıcı ağacındaki **StationCommands** düğümünde bulabilirsiniz. Bu senaryoda, Münih’teki üretim hattının montaj istasyonundaki basınç tahliye vanasını açıyorsunuz. Komut ve denetim işlevselliği kullanmak için olmalıdır **yönetici** çözüm Hızlandırıcı rolüyle:

1. Gözat **StationCommands** düğümü Tahliye Vanasını, üretim emri satırı 0 derleme istasyonu OPC UA sunucusu tarayıcı ağacında.

1. Kullanmak istediğiniz komutu seçin. Sağ **OpenPressureReleaseValve** düğümü. Tıklayın **çağrı**:

    [![Bağlı Fabrika çözümü Hızlandırıcı Çağır komutu](./media/iot-accelerators-connected-factory-dashboard/callcommand-inline.png)](./media/iot-accelerators-connected-factory-dashboard/callcommand-expanded.png#lightbox)

1. Hangi yöntemin çağrı ve parametre ayrıntıları olduğunuz bildiren bir bağlam paneli görüntülenir. Tıklayın **çağrı**:

    [![Bağlı Fabrika çözüm Hızlandırıcı çağrı parametreleri](./media/iot-accelerators-connected-factory-dashboard/callpanel-inline.png)](./media/iot-accelerators-connected-factory-dashboard/callpanel-expanded.png#lightbox)

1. Bağlam paneli, yöntem çağrısının başarılı olduğunu bildirecek şekilde güncelleştirilir. Çağrı sonucu güncelleştirilen basınç düğümünün değerini okuyarak çağrının başarılı olduğunu doğrulayabilirsiniz.

    [![Bağlı Fabrika çözümü Hızlandırıcı çağrı başarısı](./media/iot-accelerators-connected-factory-dashboard/callsuccess-inline.png)](./media/iot-accelerators-connected-factory-dashboard/callsuccess-expanded.png#lightbox)

## <a name="behind-the-scenes"></a>Arka planda

Bir çözüm hızlandırıcısını dağıttığınızda, dağıtım işlemi seçtiğiniz Azure aboneliğinde birden çok kaynak oluşturur. Azure üzerinde bu kaynakları görüntüleyebilir [portalı](https://portal.azure.com). Dağıtım işlemi, çözüm hızlandırıcınız için seçtiğiniz adı temel alan bir ada sahip bir **kaynak grubu** oluşturur:

[![Bağlı Fabrika çözüm Hızlandırıcı kaynak grubu](./media/iot-accelerators-connected-factory-dashboard/resourcegroup-inline.png)](./media/iot-accelerators-connected-factory-dashboard/resourcegroup-expanded.png#lightbox)

Her bir kaynağın ayarlarını, kaynak grubundaki kaynaklar listesinde seçerek görüntüleyebilirsiniz.

Çözüm Hızlandırıcısı kaynak kodunu da görüntüleyebilirsiniz [azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory) GitHub deposu.

İşiniz bittiğinde, çözüm Hızlandırıcısını Azure aboneliğinizden silebilirsiniz [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators#dashboard) site. Bu site, çözüm hızlandırıcısını oluşturduğunuzda sağlanan tüm kaynakları kolayca silmenize imkan tanır.

> [!NOTE]
> Çözüm Hızlandırıcısını için ilgili her şeyi sildiğinizden emin olmak için bunu silin [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators#dashboard) site. Portalda kaynak grubunu silmeyin.

## <a name="next-steps"></a>Sonraki adımlar

Çalışan bir çözüm hızlandırıcısı dağıttığınıza göre aşağıdaki makaleleri okuyarak IoT çözüm hızlandırıcılarıyla çalışmaya başlayabilirsiniz:

* [Bağlı Fabrika Çözüm Hızlandırıcısı Kılavuzu](iot-accelerators-connected-factory-sample-walkthrough.md)
* [Bağlı Fabrika çözüm hızlandırıcısına Cihazınızı bağlama](iot-accelerators-connected-factory-gateway-deployment.md)
* [Azureiotsolutions.com sitesindeki izinler](iot-accelerators-permissions.md)
