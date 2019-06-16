---
title: Bağlı Fabrika topolojisi - Azure'ı yapılandırma | Microsoft Docs
description: Bağlı Fabrika çözüm Hızlandırıcısını topolojisi yapılandırılır.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: 042277899ff22066cfa890e64f5c6c0f2e0134f9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080467"
---
# <a name="configure-the-connected-factory-solution-accelerator"></a>Bağlı Fabrika çözüm Hızlandırıcısını yapılandırın

Bağlı Fabrika Çözüm Hızlandırıcısı, kurgusal bir şirkete ilişkin Contoso sanal bir Panosu gösterilmektedir. Bu şirket, Fabrikalar genel olarak çok sayıda genel konumlarda sahiptir.

Contoso, bu makalede bağlı Fabrika çözümünün topolojisini yapılandırmak nasıl açıklamak için örnek olarak kullanılmıştır.

## <a name="simulated-factories-configuration"></a>Benzetimli fabrikaları yapılandırma

Contoso fabrikası her üç istasyonlardan her oluşur üretim hatlarının sahiptir. Her istasyon, gerçek bir OPC UA sunucusu belirli bir role şöyledir:

* Derleme istasyonu
* Test istasyonu
* Paketleme istasyonu

OPC UA düğümleri bu OPC UA sunucunuz varsa ve [OPC yayımcı](https://github.com/Azure/iot-edge-opc-publisher) bağlı Fabrika için bu düğümler değerlerini gönderir. Buna aşağıdakiler dahildir:

* Geçerli çalışma durumu gibi geçerli güç tüketimi.
* Ürün sayısı gibi üretim bilgileri oluşturdu.

Pano, istasyon düzeyi görünümünde aşağı genel bir görünüm Contoso fabrikası topolojisinden incelemek için kullanabilirsiniz. Bağlı Fabrika panosunu sağlar:

* OEE ve KPI topolojisi her katmanda rakamlarını ilişkin bir görselleştirme.
* Görselleştirmeyi istasyonları, OPC UA düğümleri geçerli değerleri.
* İstasyon düzeyinden OEE ve KPI rakamları küresel düzeye toplama.
* Görselleştirme, uyarılar ve değerler özel eşikler ulaşırsanız gerçekleştirilecek eylemler.

## <a name="connected-factory-topology"></a>Bağlı Fabrika topolojisi

Hiyerarşik fabrikaları ve üretim hatlarının istasyonları topolojisi:

* Genel düzeyde, alt öğe olarak Fabrika düğümü vardır.
* Fabrikalar, alt öğe olarak üretim hattı düğümünüz vardır.
* Üretim hatlarının alt öğe olarak istasyon düğümünüz vardır.
* İstasyonları (OPC UA sunucuları) alt öğeleri olarak OPC UA düğümünüz vardır.

Her düğüm topolojisini tanımlayan özellikleri ortak bir dizi sahiptir:

* Topoloji düğüm için benzersiz bir tanımlayıcı.
* Bir ad.
* Bir açıklama.
* Bir görüntü.
* Topoloji düğümünün alt.
* En az, hedef ve OEE ve KPI rakamları ve yürütmek için uyarı eylemleri için en yüksek değerleri.

## <a name="topology-configuration-file"></a>Topoloji yapılandırma dosyası

Bağlı Fabrika çözümünün önceki bölümde listelenen özellikleri yapılandırmak için adlı bir yapılandırma dosyası kullanan [ContosoTopologyDescription.json](https://github.com/Azure/azure-iot-connected-factory/blob/master/WebApp/Contoso/Topology/ContosoTopologyDescription.json).

Bu dosya çözüm kaynak kodunda bulabileceğiniz `WebApp/Contoso/Topology` klasör.

Aşağıdaki kod parçacığı bir özetini gösterir `ContosoTopologyDescription.json` yapılandırma dosyası:

```json
{
  <global_configuration>,
  "Factories": [
    <factory_configuration>,
    "ProductionLines": [
      <production_line_configuration>,
      "Stations": [
        <station_configuration>,
        <more station_configurations>
      ],
      <more production_line_configurations>
    ]
    <more factory_configurations>
  ]
}
```

Ortak özelliklerini `<global_configuration>`, `<factory_configuration>`, `<production_line_configuration>`, ve `<station_configuration>` şunlardır:

* **Ad** (dizeyi yazın)

  Panoda göstermek topoloji düğümü için yalnızca bir sözcük olmalıdır açıklayıcı bir ad tanımlar.

* **Açıklama** (dizeyi yazın)

  Topoloji düğüm daha ayrıntılı açıklanmaktadır.

* **Görüntü** (dizeyi yazın)

  Topoloji düğüm hakkında bilgi Panoda gösterilen zaman gösterilecek WebApp çözümü, bir görüntü yolu.

* **OeeOverall**, **OeePerformance**, **OeeAvailability**, **OeeQuality**, **Kpi1**, **kpı2** (tür `<performance_definition>`)

  Bu en az tanımlayın, hedef özellikleri ve Uyarılar oluşturmak için kullanılan şekil düzeyde değerlerini işletimsel. Bu özellikler, ayrıca bir uyarı algılanırsa yürütmek için eylemleri tanımlayın.

`<factory_configuration>` Ve `<production_line_configuration>` öğeleri bir özellik vardır:

* **GUID** (dizeyi yazın)

  Topoloji düğümün benzersiz olarak tanımlar.

`<factory_configuration>` bir özelliğe sahiptir:

* **Konum** (tür `<location_definition>`)

  Fabrika nerede olduğunu belirtir.

`<station_configuration>` özelliklere sahiptir:

* **OpcUri** (dizeyi yazın)

  Bu özellik, OPC UA uygulama URI'sini için OPC UA sunucusu ayarlamanız gerekir.
  OPC UA belirtimi tarafından genel olarak benzersiz olmalıdır çünkü bu özellik istasyon topolojisi düğümü tanımlamak için kullanılır.

* **OpcNodes**, OPC UA düğümleri dizisi olan (tür `<opc_node_description>`)

`<location_definition>` özelliklere sahiptir:

* **Şehir** (dizeyi yazın)

  Konumuna yakın bir şehir adı

* **Ülke** (dizeyi yazın)

  Konumun bulunduğu ülke

* **Enlem** (tür double)

  Konumun enlem

* **Boylam** (tür double)

  Konumun bulunduğu boylam

`<performance_definition>` özelliklere sahiptir:

* **En az** (tür double)

  Alt eşik değeri ulaşabilirsiniz. Geçerli değeri bu eşiğin altında olursa bir uyarı üretilir.

* **Hedef** (tür double)

  İdeal hedef değer.

* **En fazla** (tür double)

  Üst eşik değeri ulaşabilirsiniz. Geçerli değeri bu eşiğin üzerindeyse, bir uyarı üretilir.

* **MinimumAlertActions** (tür `<alert_action>`)

  En az bir uyarıya yanıt olarak gerçekleştirilebilecek eylemler kümesini tanımlar.

* **MaximumAlertActions** (tür `<alert_action>`)

  En çok uyarıya yanıt olarak gerçekleştirilebilecek eylemler kümesini tanımlar.

`<alert_action`> özelliklere sahiptir:

* **Tür** (dizeyi yazın)

  Uyarı eylemi türü. Aşağıdaki türleri bilinmektedir:

  * **AcknowledgeAlert**: uyarı durumu için alınan değiştirmeniz gerekir.
  * **CloseAlert**: aynı türdeki tüm eski uyarılar artık Panoda gösterilen.
  * **CallOpcMethod**: OPC UA metodunu çağrılmalıdır.
  * **OpenWebPage**: ek bağlamsal bilgiler gösteren bir tarayıcı penceresi açık olmalıdır.

* **Açıklama** (dizeyi yazın)

  Panoda gösterilen eylemi açıklaması.

* **Parametre** (dizeyi yazın)

  Eylemi yürütmek için gerekli parametreler. Değer eylem türüne bağlıdır.

  * **AcknowledgeAlert**: parametresi gerekli.
  * **CloseAlert**: parametresi gerekli.
  * **CallOpcMethod**: düğüm bilgileri ve "NodeId çağırmak için OPC UA sunucusu URI'si nodeId yönteminin üst düğümün." biçiminde çağırmak için OPC UA yönteminin parametreleri
  * **OpenWebPage**: tarayıcı penceresinde göstermek için URL.

`<opc_node_description>` (OPC UA server) bir istasyonu OPC UA düğümleri hakkında bilgi içerir. Var olan OPC UA düğüm temsil eder, ancak depolama Connected Factory hesaplama mantığı olarak kullanılan düğümleri de geçerlidir. Bunu, aşağıdaki özelliklere sahiptir:

* **NodeId** (dizeyi yazın)

  OPC UA istasyonun (OPC UA sunucusunun) düğümünde adres alanı adresi. Söz dizimi olmalıdır bir nodeId için OPC UA belirtiminde belirtilen.

* **SymbolicName** (dizeyi yazın)

  Bu OPC UA düğüm değeri gösterilirken Panoda görüntülenecek ad.

* **İlgi** (dize türünde dizi)

  OEE veya KPI OPC UA düğüm değeri hesaplama ilgili olduğunu gösterir. Her dizi öğesi aşağıdaki değerlerden biri olabilir:

  * **OeeAvailability_Running**: OEE kullanılabilirliği hesaplanması için ilgili bir değerdir.
  * **OeeAvailability_Fault**: OEE kullanılabilirliği hesaplanması için ilgili bir değerdir.
  * **OeePerformance_Ideal**: değer OEE performansı hesaplanması için geçerlidir ve genellikle bir sabit değerdir.
  * **OeePerformance_Actual**: OEE performansı hesaplanması için ilgili bir değerdir.
  * **OeeQuality_Good**: OEE kalitesi hesaplanması için ilgili bir değerdir.
  * **OeeQuality_Bad**: OEE kalitesi hesaplanması için ilgili bir değerdir.
  * **Kpi1**: değer KPI1 hesaplanması için geçerlidir.
  * **Kpı2**: değer kpı2 hesaplanması için geçerlidir.

* **OpCode** (dizeyi yazın)

  OPC UA düğüm değerini, zaman serisi görüşleri sorgular ve OEE/KPI hesaplamalarda nasıl işlendiğini gösterir. Her zaman serisi görüşleri sorgu bir sonuç gönderir ve bir sorgu parametresidir belirli bir timespan hedefler. OpCode denetimleri: sonuç nasıl hesaplanır ve aşağıdaki değerlerden biri olabilir:

  * **Fark**: timespan son ve ilk değeri arasındaki fark.
  * **Ortalama**: tüm süre değerlerinin ortalamasını.
  * **Sum**: tüm süre değerlerinin toplamını.
  * **Son**: şu anda kullanılmıyor.
  * **Sayısı**: sayıda timespan değeri.
  * **En fazla**: düzeyde timespan değeri.
  * **Min**: en az timespan değeri.
  * **Const**: ConstValue özelliği tarafından belirtilen değeri sonucudur.
  * **SubMaxMin**: maksimum ve minimum değer arasındaki farkı.
  * **TimeSpan**: TimeSpan değeri.

* **Birimleri** (dizeyi yazın)

  Panoda görünen değeri birimi tanımlar.

* **Görünür** (boolean türü)

  Değeri panoya gösterilmesi gerekip gerekmediğini denetler.

* **ConstValue** (tür double)

  Varsa **OpCode** olduğu **Const**, bu özellik bir düğümün değerini ise.

* **En az** (tür double)

  Ardından, geçerli değeri bu değerin altına düşerse, en az bir uyarı oluşturulur.

* **En fazla** (tür double)

  Ardından geçerli değeri bu değer oluşturuyorsa, en fazla bir uyarı oluşturulur.

* **MinimumAlertActions** (tür `<alert_action>`)

  En az bir uyarıya yanıt olarak gerçekleştirilebilecek eylemler kümesini tanımlar.

* **MaximumAlertActions** (tür `<alert_action>`)

  En çok uyarıya yanıt olarak gerçekleştirilebilecek eylemler kümesini tanımlar.

Ayrıca istasyon düzeyinde görürsünüz **benzetimi** nesneleri. Bu nesneler, yalnızca Connected Factory benzetim yapılandırmak için kullanılır ve gerçek bir topolojiyi yapılandırmak için kullanılmamalıdır.

## <a name="how-the-configuration-data-is-used-at-runtime"></a>Yapılandırma verileri çalışma zamanında nasıl kullanılır

Yapılandırma dosyasında kullanılan tüm özellikleri, nasıl kullanıldıkları bağlı olarak farklı kategoride gruplandırılabilir. Bu kategorileri şunlardır:

### <a name="visual-appearance"></a>Görsel görünümünü

Bağlı Fabrika panosunu öğesinin görsel görünümüne özellikleri bu kategorideki tanımlayın. Örneklere şunlar dahildir:

* Ad
* Açıklama
* Image
* Location
* Birimler
* Görünür

### <a name="internal-topology-tree-addressing"></a>Ağaç iç topolojisi adresleme

WebApp tüm topoloji düğümlerinin bilgilerini içeren bir iç veri sözlüğü tutar. Özellikleri **GUID** ve **OpcUri** anahtarlar Bu sözlük erişmek için kullanılır ve benzersiz olması gerekir.

### <a name="oeekpi-computation"></a>OEE/KPI hesaplama

Bağlı Fabrika simülasyonu için OEE/KPI rakamları tarafından parametre haline getirilen:

* Hesaplamaya dahil edilecek OPC UA düğüm değerleri.
* Şekil, telemetri değerleri nasıl hesaplanır.

Bağlı fabrika tarafından yayınlanan olarak OEE formülleri kullanır [ http://www.oeefoundation.org ](http://www.oeefoundation.org).

OPC UA düğüm nesneleri istasyon OEE/KPI hesaplamada kullanım için etiketleme etkinleştirin. **İlgi** özelliği için hangi OEE/KPI şekil OPC UA düğüm değerinin kullanılması gerektiğini belirtir. **OpCode** özelliği tanımlayan değer hesaplamanın nasıl dahildir.

### <a name="alert-handling"></a>Uyarı işleme

Bağlı Fabrika basit en düşük/en yüksek eşik tabanlı uyarı oluşturma mekanizması destekler. Bir dizi önceden tanımlanmış eylem bu uyarılara yanıt olarak yapılandırabileceğiniz vardır. Aşağıdaki özellikler, bu mekanizma denetler:

* Maksimum
* Minimum
* MaximumAlertActions
* MinimumAlertActions

## <a name="correlating-to-telemetry-data"></a>Telemetri verileri ilişkilendirme

Son değer görselleştirme veya zaman serisi görüşleri sorgular oluşturma gibi belirli işlemleri WebApp alınan telemetri verilerini bir adresleme şemasını gerekir. Bağlı Fabrika için gönderilen telemetriyi de iç veri yapılarını depolanmış olması gerekir. İstasyon (OPC UA sunucusu) ve OPC UA düğüm düzeyinde işlemlerini etkinleştirme iki özellik şunlardır:

* **OpcUri**

  Tanımlar (genel olarak benzersiz) telemetri OPC UA sunucusu gelir. Alınan iletileri, bu özellik olarak gönderilen **ApplicationUri**.

* **NodeId**

  OPC UA sunucusu düğümünün değerini tanımlar. Özelliğin biçimi olmalıdır OPC UA belirtiminde belirtilen. Alınan iletileri, bu özellik olarak gönderilen **nodeId**.

Denetleme [bu](https://github.com/Azure/iot-edge-opc-publisher) telemetri veri Fabrikasına bağlı OPC yayımcısını kullanma nasıl alınır hakkında daha fazla bilgi için GitHub sayfası.

## <a name="example-how-kpi1-is-calculated"></a>Örnek: KPI1 nasıl hesaplanır

Yapılandırmada `ContosoTopologyDescription.json` dosya denetimleri OEE/KPI rakamları nasıl hesaplanır. Aşağıdaki örnek, bu dosyanın özelliklerinde KPI1 hesaplama nasıl kontrol gösterir.

Bağlı Fabrika KPI1 içinde son bir saat içinde başarıyla üretilen ürün sayısını ölçmek için kullanılır. Bağlı Fabrika benzetimdeki her istasyon (OPC UA server) bir OPC UA düğümü sağlar (`NodeId: "ns=2;i=385"`), işlem bu KPI için telemetri sağlar.

Bu OPC UA düğüm yapılandırması aşağıdaki kod parçacığı gibi görünür:

```json
{
  "NodeId": "ns=2;i=385",
  "SymbolicName": "NumberOfManufacturedProducts",
  "Relevance": [ "Kpi1", "OeeQuality_Good" ],
  "OpCode": "SubMaxMin"
},
```

Bu yapılandırma, Time Series Insights'ı kullanarak bu düğümün telemetri değerlerini sorgulama etkinleştirir. Time Series Insights sorguyu alır:

* Değer sayısı.
* Minimum değer.
* Düzeyde değeri.
* Tüm değerlerin ortalamasını.
* Tüm tüm değerlerin toplamını benzersiz **OpcUri** (**ApplicationUri**), **nodeId** çiftlerinde belirtilen bir zaman aralığı.

Bir özelliği **NumberOfManufactureredProducts** düğüm değerdir yalnızca artırır. Zaman aralığı içinde üretilen ürün sayısını hesaplamak için bağlı Fabrika kullanan **OpCode** **SubMaxMin**. Hesaplamanın en düşük değer timespan başlangıcında ve sonunda TimeSpan değerini, en yüksek değer alır.

**OpCode** yapılandırmada maksimum ve minimum değerin farkı sonucunu hesaplamak için hesaplama mantığı yapılandırır. Ardından sonuçları olan alt kadar kök (Genel) düzeyinde toplanan ve panosunda gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

Bir önerilen sonraki adım, bilgi edinmek için nasıl [bağlı Fabrika çözümü özelleştirme](iot-accelerators-connected-factory-customize.md).
