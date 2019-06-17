---
title: Bağlı Fabrika çözümü - Azure özelleştirme | Microsoft Docs
description: Bağlı Fabrika çözüm Hızlandırıcısını davranışını özelleştirmek nasıl bir açıklaması.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: csharp
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: dobett
ms.openlocfilehash: 6062f8b3992732e0e0f9bbdae9549e69c393f4ff
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080481"
---
# <a name="customize-how-the-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Bağlı Fabrika çözümü OPC UA sunucularınızdan verilerin biçimini özelleştirme

Bağlı Fabrika çözümü toplar ve verileri çözüme bağlı OPC UA sunucularını görüntüler. Göz atabilir ve komutları OPC UA sunucularına çözümünüzde gönderin. OPC UA hakkında daha fazla bilgi için bkz. [Bağlı Fabrika SSS](iot-accelerators-faq-cf.md).

Çözümdeki toplanmış veri fabrikası, satır ve istasyon düzeyi panosunda görüntüleyebileceğiniz ana performans göstergeleri (KPI'ler) ve genel donanım verimliliğini (OEE) verilebilir. OEE ve KPI değerlerini aşağıdaki ekran görüntüsünde gösterilmektedir **derleme** istasyon, üzerinde **üretim emri satırı 1**, **Tahliye Vanasını** fabrikası:

![OEE ve KPI değerlerini çözümdeki örneği][img-oee-kpi]

Belirli veri öğeleri olarak adlandırılan OPC UA sunucusundan alınan ayrıntılı bilgileri görüntülemek çözüm sayesinde *istasyonları*. Aşağıdaki ekran çizimleri, belirli bir istasyondan üretilen öğe sayısını gösterir:

![Çizimler üretilen öğe sayısı][img-manufactured-items]

Grafikler birine tıklayın, daha fazla zaman serisi öngörüleri (TSI) kullanarak verileri keşfedebilirsiniz:

![Time Series Insights'ı kullanarak verileri keşfedin][img-tsi]

Bu makalede açıklanır:

- Verilerin çözümde çeşitli görünümlere kullanımına nasıl sunulacağını.
- Çözüm şekilde nasıl özelleştirebileceğiniz verileri görüntüler.

## <a name="data-sources"></a>Veri kaynakları

Bağlı Fabrika çözümü çözüme bağlı OPC UA sunucuları verileri görüntüler. Varsayılan yükleme Fabrika simülasyon birkaç OPC UA sunucuları içerir. Kendi OPC UA sunucuları ekleyebilirsiniz, [bir ağ geçidi üzerinden Bağlan] [lnk-connect-cf] çözümünüze.

Panodaki çözümünüze bağlı bir OPC UA sunucusu gönderebileceğiniz veri öğelerini göz atabilirsiniz:

1. Seçin **tarayıcı** gitmek için **bir OPC UA sunucusu seçin** görüntüle:

    ![Bir OPC UA sunucu görünümü için seçim gidin][img-select-server]

1. Bir sunucu seçin ve tıklayın **Connect**. Tıklayın **İlerle** zaman güvenlik uyarısı görüntülenir.

    > [!NOTE]
    > Bu uyarı, yalnızca bir kez her sunucu için görünür ve çözüm panosunu ve sunucu arasında bir güven ilişkisi oluşturur.

1. Artık sunucu çözümü gönderebilirsiniz veri öğelerini de göz atabilirsiniz. Çözüme gönderilen öğeleri bir onay işareti bulunur:

    ![Yayımlanan öğeler][img-published]

1. Eğer bir *yönetici* çözümde, bağlı Fabrika çözümüne kullanılabilir hale getirmek için bir veri öğesi yayımlamayı seçebilirsiniz. Yönetici olarak, veri öğelerinin değerini değiştirebilir ve OPC UA sunucu yöntemlerini çağırın.

## <a name="map-the-data"></a>Veri eşlemesi

Bağlı Fabrika çözümü eşler ve çeşitli görünümlere çözümdeki OPC UA sunucusundan yayımlanan veri öğelerini toplar. Çözüm sağladığınızda, Azure hesabınıza bağlı Fabrika çözümü dağıtır. Visual Studio Connected Factory çözümünde bir JSON dosyası Bu eşleme bilgilerini depolar. Görüntüleyebilir ve Visual Studio bağlı Fabrika çözümüne bu JSON yapılandırma dosyasını değiştirin. Bir değişiklik yaptıktan sonra çözümü yeniden dağıtabilirsiniz.

Yapılandırma dosyasını kullanabilirsiniz:

- Mevcut sanal fabrikaları, üretim hatlarının ve istasyon düzenleyin.
- Çözümüne bağlama gerçek OPC UA sunucusundan verileri eşleyin.

Eşleme ve belirli gereksinimlerinizi karşılamak için veri toplama hakkında daha fazla bilgi için bkz. [bağlı Fabrika çözüm Hızlandırıcısını yapılandırma ](iot-accelerators-connected-factory-configure.md).

## <a name="deploy-the-changes"></a>Değişiklikleri dağıtın

Tamamladığınızda değişiklikleri yapma **ContosoTopologyDescription.json** dosya, Azure hesabınıza bağlı Fabrika çözümü yeniden dağıtmanız gerekir.

**Azure-iot-connected-factory** deposu içeren bir **build.ps1** yeniden oluşturun ve çözümü dağıtmak için kullanabileceğiniz bir PowerShell Betiği.

## <a name="next-steps"></a>Sonraki Adımlar

Bağlı Fabrika çözüm Hızlandırıcısını hakkında daha fazla bilgi için aşağıdaki makaleleri okuyarak öğrenin:

* [Azureiotsolutions.com sitesindeki izinler][lnk-permissions]
* [Bağlı Fabrika hakkında SSS](iot-accelerators-faq-cf.md)
* [SSS][lnk-faq]


[img-oee-kpi]: ./media/iot-accelerators-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-accelerators-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-accelerators-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-accelerators-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-accelerators-connected-factory-customize/published.png


[lnk-permissions]: iot-accelerators-permissions.md
[lnk-faq]: iot-accelerators-faq.md