---
title: "Bağlı Fabrika çözümü - Azure özelleştirme | Microsoft Docs"
description: "Bağlı Fabrika önceden yapılandırılmış çözümün davranışını özelleştirmek nasıl açıklaması."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: c#
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/14/2017
ms.author: dobett
ms.openlocfilehash: 48c8036d0bc9534ce94529b96d32b004769246c1
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="customize-how-the-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Bağlı Fabrika çözüm OPC UA sunucularınızdan veri biçimini Özelleştir

Bağlı Fabrika çözüm toplar ve çözüme bağlı OPC UA sunuculardan verileri görüntüler. Göz atın ve komutları OPC UA sunucularına çözümünüzde gönderir. OPC UA hakkında daha fazla bilgi için bkz. [Connected factory SSS](iot-suite-faq-cf.md).

Genel donanım verimliliği (OEE) ve Fabrika, satır ve istasyon düzeylerinde panosunda görüntüleyebileceğiniz ana performans göstergelerini (KPI'lar) çözümüne toplanan veri örneklerindendir. Aşağıdaki ekran görüntüsü için OEE ve KPI değerleri gösterir **derleme** istasyon, üzerinde **üretim satırı 1**, **Münih** Fabrika:

![OEE ve KPI değerleri çözümde örneği][img-oee-kpi]

Çözüm, belirli veri öğeleri olarak adlandırılan OPC UA sunucularından alınan ayrıntılı bilgileri görüntülemenize olanak tanır *istasyonları*. Aşağıdaki ekran çizimleri belirli bir istasyonu üretilen öğelerinden sayısının gösterir:

![Çizimler üretilen öğe sayısı][img-manufactured-items]

Aşağıdakilerden grafikleri tıklarsanız, verileri daha fazla zaman serisi Öngörüler (TSI) kullanarak da gözden geçirebilirsiniz:

![Zaman serisi Öngörüler kullanarak verileri keşfedin][img-tsi]

Bu makalede açıklanır:

- Nasıl veri çözümde çeşitli görünümleri için kullanılabilir hale getirilir.
- Çözüm şekilde nasıl özelleştirebileceğiniz verileri görüntüler.

## <a name="data-sources"></a>Veri kaynakları

Bağlı Fabrika çözüm çözüme bağlı OPC UA sunuculardan verileri görüntüler. Varsayılan yükleme Fabrika benzetimi birkaç OPC UA sunucuları içerir. Kendi OPC UA sunucuları ekleyebilirsiniz, [bir ağ geçidi üzerinden bağlanma] [ lnk-connect-cf] çözümünüze.

Pano çözümünüzde bağlı OPC UA sunucu gönderebilir veri öğelerini göz atabilirsiniz:

1. Seçin **tarayıcı** gitmek için **OPC UA sunucuyu seçin** görünümü:

    ![OPC UA sunucu görünümü seçin gidin][img-select-server]

1. Bir sunucu seçin ve tıklatın **Bağlan**. Tıklatın **İlerle** zaman güvenlik uyarısı görüntülenmez.

    > [!NOTE]
    > Bu uyarı yalnızca bir kez her sunucu için görünür ve çözüm panosu ve sunucu arasında bir güven ilişkisi oluşturur.

1. Sunucu çözüme gönderebilirsiniz veri öğelerini gözatabilirsiniz. Çözüme gönderilen öğeleri bir onay işareti bulunur:

    ![Yayımlanan öğeler][img-published]

1. Kullanıyorsanız bir *yönetici* çözümde bağlı Fabrika çözümde kullanılabilir hale getirmek için bir veri öğesi yayımlamayı seçebilirsiniz. Yönetici olarak, veri öğeleri değerini değiştirin ve OPC UA Server'da yöntemlerini çağırın.

## <a name="map-the-data"></a>Verileri eşleme

Bağlı Fabrika çözüm eşler ve çeşitli görünümlere çözümdeki OPC UA sunucudan yayımlanan veri öğelerini toplar. Bir çözüm sağladığınızda Azure hesabınıza bağlı Fabrika çözümü dağıtır. Visual Studio bağlı Fabrika çözüm JSON dosyasında bu eşleme bilgilerini depolar. Görüntüleyin ve bu bağlantılı Fabrika Visual Studio çözümü JSON yapılandırma dosyasında değiştirin. Değişikliği yaptıktan sonra çözümü yeniden dağıtabilirsiniz.

Yapılandırma dosyası kullanabilirsiniz:

- Varolan sanal oluşturucuları, Üretim satırları ve istasyonları düzenleyin.
- Çözümü arasında bağlantı gerçek OPC UA sunuculardan verileri eşleyin.

Eşleme ve belirli gereksinimlerinizi karşılamak için veri toplama hakkında daha fazla bilgi için bkz: [bağlı Fabrika yapılandırma önceden yapılandırılmış çözüm ](iot-suite-connected-factory-configure.md).

## <a name="deploy-the-changes"></a>Değişiklikleri dağıtma

Değişiklikler yapma tamamladığınızda **ContosoTopologyDescription.json** dosyası, Azure hesabınıza bağlı Fabrika çözümü yeniden dağıtmanız gerekir.

**Azure-IOT-bağlı-Fabrika** deposu içeren bir **build.ps1** PowerShell komut dosyasını yeniden oluşturun ve çözümü dağıtmak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Aşağıdaki makaleleri okuyarak bağlı Fabrika önceden yapılandırılmış çözüm hakkında daha fazla bilgi:

* [Önceden yapılandırılmış bağlı fabrika çözümü yönergeleri][lnk-rm-walkthrough]
* [Bağlı üreteci için bir ağ geçidi dağıtma][lnk-connect-cf]
* [azureiotsuite.com sitesindeki izinler][lnk-permissions]
* [Bağlı fabrika hakkında SSS](iot-suite-faq-cf.md)
* [SSS][lnk-faq]


[img-oee-kpi]: ./media/iot-suite-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-suite-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-suite-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-suite-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-suite-connected-factory-customize/published.png
[img-munich]: ./media/iot-suite-connected-factory-customize/munich.png
[img-server-uris]: ./media/iot-suite-connected-factory-customize/serveruris.png
[lnk-kpi]: ./media/iot-suite-connected-factory-customize/kpidisplay.png

[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-v1-permissions.md
[lnk-faq]: iot-suite-v1-faq.md