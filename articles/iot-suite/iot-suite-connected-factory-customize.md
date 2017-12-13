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
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: d9dfd856a95d0b1f925487f4ca9d27e617093405
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
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

Visual Studio çözümü bağlı Fabrika kopyasını kopyalamak için aşağıdaki git komutu kullanın:

`git clone https://github.com/Azure/azure-iot-connected-factory.git`

Dosya **ContosoTopologyDescription.json** bağlı Fabrika çözüm panosunda OPC UA sunucu veri öğeleri eşleme görünümleri tanımlar. Bu yapılandırma dosyasında bulabilirsiniz **Contoso\Topology** klasöründe **WebApp** Visual Studio çözümü projesinde.

JSON dosyasının içeriği Fabrika, üretim hattı ve istasyon düğümleri hiyerarşi olarak düzenlenir. Bu hiyerarşi Gezinti hiyerarşisinde bağlı Fabrika panosunda tanımlar. Hiyerarşinin her düğümde değerler panosunda görüntülenen bilgileri belirler. Örneğin, JSON dosyası Münih Fabrika için aşağıdaki değerleri içerir:

```json
"Guid": "73B534AE-7C7E-4877-B826-F1C0EA339F65",
"Name": "Munich",
"Description": "Braking system",
"Location": {
    "City": "Munich",
    "Country": "Germany",
    "Latitude": 48.13641,
    "Longitude": 11.57754
},
"Image": "munich.jpg"
```

Adını, açıklamasını ve konum bu Pano görünümünde görünür:

![Pano Münih verileri][img-munich]

Her fabrika, üretim hattı ve istasyon bir görüntü özelliği vardır. Bu JPEG dosyaları bulabilirsiniz **Content\img** klasöründe **WebApp** projesi. Bu görüntü dosyalar bağlı Fabrika panosunda görüntülenir.

Her istasyon OPC UA veri öğeleri eşlemesinden tanımlayan çeşitli ayrıntılı özellikleri içerir. Bu özellikler aşağıdaki bölümlerde açıklanmıştır:

### <a name="opcuri"></a>OpcUri

**OpcUri** OPC UA sunucunun benzersiz olarak tanıtan OPC UA uygulama URI bir değerdir. Örneğin, **OpcUri** değeri 1 üretim satırında Münih derleme istasyon aşağıdaki gibi görünür: **urn: scada2194:ua:munich:productionline0:assemblystation**.

Çözüm panosunda URI'ler bağlı OPC UA sunucuları görüntüleyebilirsiniz:

![OPC UA sunucusu URI görüntülemek][img-server-uris]

### <a name="simulation"></a>Benzetim

Bilgileri **benzetimi** düğümü, varsayılan olarak sağlanan OPC UA sunucularında çalışan OPC UA benzetimi için özeldir. Gerçek bir OPC UA sunucusu için kullanılmaz.

### <a name="kpi1-and-kpi2"></a>Kpi1 ve kpı2

Bu düğümler Pano iki KPI değerler istasyon verileri nasıl katkıda bulunan açıklanmaktadır. Varsayılan dağıtımında, bu KPI saat başına birim ve saatte kWh değerlerdir. Çözüm KPI değerlerinde bir istasyon düzeyinde hesaplar ve bunları üretim hattı ve Fabrika düzeylerinde toplar.

Her KPI minimum, maksimum ve hedef değer vardır. Her bir KPI değeri Uyarı eylemleri gerçekleştirmek bağlı Fabrika çözüm için de tanımlayabilirsiniz. Aşağıdaki kod parçacığında derleme istasyon KPI tanımlarında Münih 1 üretim satırdaki gösterir:

```json
"Kpi1": {
  "Minimum": 150,
  "Target": 300,
  "Maximum": 600
},
"Kpi2": {
  "Minimum": 50,
  "Target": 100,
  "Maximum": 200,
  "MinimumAlertActions": [
    {
      "Type": "None"
    }
  ]
}
```

Aşağıdaki ekran görüntüsünde KPI verileri Panoda gösterir.

![KPI bilgilerini panosunda][lnk-kpi]

### <a name="opcnodes"></a>OpcNodes

**OpcNodes** düğümleri OPC UA sunucusundan yayımlanan veri öğelerini tanımlamak ve bu verileri işlemek nasıl belirtin.

**NodeId** değer OPC UA sunucusundan belirli OPC UA nodeId tanımlar. Derleme istasyon üretim hattı Münih 1 için ilk düğüm bir değere sahip **ns = 2; ı 385 =**. A **nodeId** değeri OPC UA sunucusundan okumak için veri öğesi belirtir ve **SymbolicName** bu verileri Panoda kullanmak için kullanıcı dostu bir ad sağlar.

Her düğüm ile ilişkili diğer değerler aşağıdaki tabloda özetlenmiştir:

| Değer | Açıklama |
| ----- | ----------- |
| İlgi Düzeyi  | KPI'yı ve OEE değerleri için bu verileri katkıda bulunur. |
| İşlem kodu     | Verileri nasıl toplanır. |
| Birimler      | Panoda kullanılacak birim.  |
| Görünür    | Bu değer panosunda görüntülenip görüntülenmeyeceğini belirtir. Bazı değerler hesaplamalarında kullanılır ancak görüntülenmez.  |
| Maksimum    | Panodaki bir uyarıyı tetikleyen en yüksek değer. |
| MaximumAlertActions | Yanıt bir uyarı olarak gerçekleştirilecek bir eylem. Örneğin, bir komut istasyona gönderir. |
| ConstValue | Bir hesaplanmasında kullanılan sabit bir değer. |

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