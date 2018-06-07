---
title: Bağlı Fabrika çözümü - Azure özelleştirme | Microsoft Docs
description: Bağlı Fabrika Çözüm Hızlandırıcısı davranışını özelleştirmek nasıl açıklaması.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: csharp
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: dobett
ms.openlocfilehash: e91f36c9d6f0cb3195e6903d55cd676f1e9ccf5a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626559"
---
# <a name="customize-how-the-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Bağlı Fabrika çözüm OPC UA sunucularınızdan veri biçimini Özelleştir

Bağlı Fabrika çözüm toplar ve çözüme bağlı OPC UA sunuculardan verileri görüntüler. Göz atın ve komutları OPC UA sunucularına çözümünüzde gönderir. OPC UA hakkında daha fazla bilgi için bkz. [Bağlı Fabrika SSS](iot-accelerators-faq-cf.md).

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

Bağlı Fabrika çözüm eşler ve çeşitli görünümlere çözümdeki OPC UA sunucudan yayımlanan veri öğelerini toplar. Bir çözüm sağladığınızda Azure hesabınıza bağlı Fabrika çözümü dağıtır. Visual Studio bağlı Fabrika çözüm JSON dosyasında bu eşleme bilgilerini depolar. Görüntüleyin ve bu bağlı Fabrika Visual Studio çözümü JSON yapılandırma dosyasında değiştirin. Değişikliği yaptıktan sonra çözümü yeniden dağıtabilirsiniz.

Yapılandırma dosyası kullanabilirsiniz:

- Varolan sanal oluşturucuları, Üretim satırları ve istasyonları düzenleyin.
- Çözümü arasında bağlantı gerçek OPC UA sunuculardan verileri eşleyin.

Eşleme ve belirli gereksinimlerinizi karşılamak için veri toplama hakkında daha fazla bilgi için bkz: [bağlı Fabrika Çözüm Hızlandırıcısı yapılandırma ](iot-accelerators-connected-factory-configure.md).

## <a name="deploy-the-changes"></a>Değişiklikleri dağıtma

Değişiklikler yapma tamamladığınızda **ContosoTopologyDescription.json** dosyası, Azure hesabınıza bağlı Fabrika çözümü yeniden dağıtmanız gerekir.

**Azure-IOT-bağlı-Fabrika** deposu içeren bir **build.ps1** PowerShell komut dosyasını yeniden oluşturun ve çözümü dağıtmak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Aşağıdaki makaleleri okuyarak bağlı Fabrika Çözüm Hızlandırıcısı hakkında daha fazla bilgi:

* [Bağlı Fabrika çözüm hızlandırıcısı adım adım kılavuzu][lnk-rm-walkthrough]
* [Bir ağ geçidi için Fabrika bağlı dağıtma][lnk-connect-cf]
* [azureiotsuite.com sitesindeki izinler][lnk-permissions]
* [Bağlı Fabrika hakkında SSS](iot-accelerators-faq-cf.md)
* [SSS][lnk-faq]


[img-oee-kpi]: ./media/iot-accelerators-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-accelerators-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-accelerators-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-accelerators-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-accelerators-connected-factory-customize/published.png


[lnk-rm-walkthrough]:iot-accelerators-connected-factory-sample-walkthrough.md
[lnk-connect-cf]:iot-accelerators-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-accelerators-permissions.md
[lnk-faq]: iot-accelerators-faq.md