---
title: Bağlı Fabrika topolojisini yapılandırma | Microsoft Docs
description: Bir bağlı Fabrika Çözüm Hızlandırıcısı topolojisini yapılandırmak nasıl.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: c6c5e27dad7f80a329edbd8fbcb95647dc4cd15a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626743"
---
# <a name="configure-the-connected-factory-solution-accelerator"></a>Bağlı Fabrika Çözüm Hızlandırıcısı yapılandırın

Bağlı Fabrika Çözüm Hızlandırıcısı, kurgusal bir şirket Contoso için sanal bir Pano gösterir. Bu şirket oluşturucuları genel çok sayıda genel konumlarda sahiptir.

Bu makalede Contoso bağlı Fabrika çözüm topolojisini yapılandırmak nasıl açıklamak için örnek olarak kullanır.

## <a name="simulated-factories-configuration"></a>Benzetimli oluşturucuları yapılandırma

Her Contoso Fabrika üç istasyonları her oluşur üretim satır yok. Her istasyon, belirli bir rol ile gerçek bir OPC UA sunucusudur:

* Derleme istasyon
* Test istasyon
* Paketleme istasyon

Bu OPC UA sunucular OPC UA düğümünüz ve [OPC yayımcı](https://github.com/Azure/iot-edge-opc-publisher) bağlı Fabrika bu düğümler değerlerini gönderir. Buna aşağıdakiler dahildir:

* Geçerli güç tüketimini gibi geçerli işlem durumu.
* Üretim bilgilerini ürünleri sayısı gibi üretti.

Bir istasyonu düzey görünümü için genel bir görünümden Contoso Fabrika topoloji incelemek için panoyu kullanabilirsiniz. Bağlı Fabrika Pano sağlar:

* Her katman topolojideki OEE ve KPI rakamlarını görselleştirme.
* Geçerli değerler istasyonları OPC UA düğümler görselleştirme.
* İstasyon düzeyinden OEE ve KPI rakamları genel düzeyine toplama.
* Uyarılar ve değerleri özel eşikler ulaşırsa gerçekleştirilecek eylemler görselleştirmesini.

## <a name="connected-factory-topology"></a>Bağlı Fabrika topolojisi

Hiyerarşik oluşturucuları, Üretim satırları ve istasyonları topolojidir:

* Genel düzeyde alt öğesi olarak Fabrika düğüm yok.
* Oluşturucuları üretim hattı düğümlerin alt öğesi olarak vardır.
* Üretim satırları istasyon düğümlerin alt öğesi olarak vardır.
* İstasyonları (OPC UA sunucuları) OPC UA düğümlerin alt öğesi olarak vardır.

Topoloji kümedeki her düğüm tanımlayan özellikleri ortak bir dizi sahiptir:

* Topoloji düğümü için benzersiz bir tanımlayıcı.
* Bir ad.
* Bir açıklama.
* Bir görüntü.
* Düğümün alt öğelerinin topolojisi.
* En az, hedef ve OEE ve KPI rakamları ve yürütmek için uyarı eylemleri için en yüksek değerleri.

## <a name="topology-configuration-file"></a>Topoloji yapılandırma dosyası

Önceki bölümde listelenen özelliklerini yapılandırmak için bağlı Fabrika çözüm olarak adlandırılan bir yapılandırma dosyası kullanır [ContosoTopologyDescription.json](https://github.com/Azure/azure-iot-connected-factory/blob/master/WebApp/Contoso/Topology/ContosoTopologyDescription.json).

Bu dosya çözüm kaynak kodunda bulabilirsiniz `WebApp/Contoso/Topology` klasör.

Aşağıdaki kod parçacığını bir özetini gösterir `ContosoTopologyDescription.json` yapılandırma dosyası:

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

Ortak özelliklerini `<global_configuration>`, `<factory_configuration>`, `<production_line_configuration>`, ve `<station_configuration>` şunlardır:

* **Ad** (dizeyi yazın)

  Panoda göstermek topoloji düğümü için yalnızca bir word olmalıdır açıklayıcı bir ad tanımlar.

* **Açıklama** (dizeyi yazın)

  Daha fazla ayrıntı topoloji düğümünde açıklar.

* **Görüntü** (dizeyi yazın)

  WebApp çözümü topoloji düğüm hakkında bilgi panosunda zaman gösterilen göstermek için bir resim yolu.

* **OeeOverall**, **OeePerformance**, **OeeAvailability**, **OeeQuality**, **Kpi1**, **kpı2** (tür `<performance_definition>`)

  Bu en az tanımlamak, hedef özellikleri ve Uyarıları oluşturmak için kullanılan şekil düzeyde değerlerini işletimsel. Bu özellikler aynı zamanda bir uyarı algılanırsa, yürütülecek eylemleri tanımlayın.

`<factory_configuration>` Ve `<production_line_configuration>` öğelerinin bir özellik vardır:

* **GUID** (dizeyi yazın)

  Topoloji düğümü benzersiz olarak tanımlar.

`<factory_configuration>` bir özelliğe sahiptir:

* **Konum** (tür `<location_definition>`)

  Fabrika nerede olduğunu belirtir.

`<station_configuration>` özelliklere sahiptir:

* **OpcUri** (dizeyi yazın)

  Bu özellik için URI OPC UA uygulama OPC UA sunucusunun ayarlamanız gerekir.
  OPC UA belirtimine göre küresel olarak benzersiz olması gerektiği için bu özelliği istasyon topoloji düğümü tanımlamak için kullanılır.

* **OpcNodes**, bir dizi OPC UA düğümlerinin olduğu (tür `<opc_node_description>`)

`<location_definition>` özelliklere sahiptir:

* **Şehir** (dizeyi yazın)

  Şehir konumuna yakın adı

* **Ülke** (dizeyi yazın)

  Konumun ülke

* **Enlem** (tür double)

  Konumun enlem

* **Boylam** (tür double)

  Boylam konumu

`<performance_definition>` özelliklere sahiptir:

* **Minimum** (tür double)

  Alt eşik değeri ulaşabilirsiniz. Geçerli değeri bu eşiğin altına ise bir uyarı üretilir.

* **Hedef** (tür double)

  İdeal hedef değer.

* **En fazla** (tür double)

  Üst eşik değeri ulaşabilirsiniz. Geçerli değeri bu eşiğin üzerindeyse bir uyarı üretilir.

* **MinimumAlertActions** (tür `<alert_action>`)

  En az bir uyarı yanıt olarak gerçekleştirilen eylemleri kümesini tanımlar.

* **MaximumAlertActions** (tür `<alert_action>`)

  En fazla uyarı yanıt olarak gerçekleştirilen eylemleri kümesini tanımlar.

`<alert_action`> özelliklere sahiptir:

* **Tür** (dizeyi yazın)

  Uyarı eylemi türü. Aşağıdaki türlerden bilinmektedir:

  * **AcknowledgeAlert**: uyarı durumu için alınan değiştirmeniz gerekir.
  * **CloseAlert**: aynı türden tüm eski uyarıları artık panosunda gösterilen.
  * **CallOpcMethod**: OPC UA yöntemi çağrılmalıdır.
  * **OpenWebPage**: ek bağlamsal bilgi gösteren bir tarayıcı penceresi açılması gerekir.

* **Açıklama** (dizeyi yazın)

  Panoda gösterilen eylemi açıklaması.

* **Parametre** (dizeyi yazın)

  Eylem yürütme için gerekli parametreleri. Değer eylem türüne bağlıdır.

  * **AcknowledgeAlert**: gerekli parametre.
  * **CloseAlert**: gerekli parametre.
  * **CallOpcMethod**: "NodeId üst düğümün çağırmak için URI OPC UA sunucusunun nodeId yönteminin." biçiminde çağrılacak OPC UA yöntem parametreleri ve düğüm bilgi
  * **OpenWebPage**: tarayıcı penceresinde göstermek için URL.

`<opc_node_description>` OPC UA düğümler istasyon (OPC UA sunucu) hakkında bilgi içerir. Varolan OPC UA düğüm temsil eder, ancak depolama hesaplama mantığında Factory bağlı olarak kullanılan düğümleri da geçerlidir. Aşağıdaki özelliklere sahiptir:

* **NodeId** (dizeyi yazın)

  OPC UA istasyonun (OPC UA sunucunun) düğümünde adres alanı adresi. Sözdizimi olmalıdır bir nodeId OPC UA belirtimi belirtilmiş.

* **SymbolicName** (dizeyi yazın)

  Bu OPC UA düğüm değerini gösterildiğinde Panoda görüntülenecek ad.

* **İlgi** (dize türünde dizi)

  Hesaplama OEE veya KPI OPC UA düğüm değeri ilgili olduğunu gösterir. Her dizi öğesi aşağıdaki değerlerden biri olabilir:

  * **OeeAvailability_Running**: OEE kullanılabilirlik hesaplamasını ilgili bir değerdir.
  * **OeeAvailability_Fault**: OEE kullanılabilirlik hesaplamasını ilgili bir değerdir.
  * **OeePerformance_Ideal**: değeri OEE performans hesaplama için geçerlidir ve genellikle sabit bir değer.
  * **OeePerformance_Actual**: OEE performans hesaplama için ilgili bir değerdir.
  * **OeeQuality_Good**: OEE kalite hesaplama için ilgili bir değerdir.
  * **OeeQuality_Bad**: OEE kalite hesaplama için ilgili bir değerdir.
  * **Kpi1**: KPI1 hesaplama için ilgili bir değerdir.
  * **Kpı2**: kpı2 hesaplama için ilgili bir değerdir.

* **OpCode** (dizeyi yazın)

  OPC UA düğüm değerini zaman serisi Insight sorgular ve OEE/KPI hesaplamaları nasıl işlendiğini gösterir. Her zaman serisi Insight sorgu sorgu parametresi ve bir sonuç teslim belirli bir timespan hedefler. OpCode kontrol eder nasıl sonucu hesaplanır ve aşağıdaki değerlerden biri olabilir:

  * **Diff**: timespan son ve ilk değer arasındaki fark.
  * **Ortalama**: tüm süre değerlerinin ortalamasını.
  * **Sum**: timespan tüm değerlerinin toplamı.
  * **Son**: şu anda kullanılmıyor.
  * **Count**: timespan değerleri sayısı.
  * **Max**: timespan düzeyde değeri.
  * **Min**: timespan en az değer.
  * **Const**: ConstValue özelliği tarafından belirtilen değeri sonucudur.
  * **SubMaxMin**: maksimum ve minimum değer arasındaki fark.
  * **TimeSpan**: timespan.

* **Birimleri** (dizeyi yazın)

  Değerin görüntülenmesi için bir birim Panoda tanımlar.

* **Görünür** (boolean türü)

  Değer panosunda gösterilecek kullanılmadığını denetler.

* **ConstValue** (tür double)

  Varsa **OpCode** olan **Const**, bu özellik düğüm değeri olur.

* **Minimum** (tür double)

  Geçerli değeri bu değerin altına düşerse, en az bir uyarı üretilir.

* **En fazla** (tür double)

  Geçerli değeri bu değer geçirirse, en fazla bir uyarı üretilir.

* **MinimumAlertActions** (tür `<alert_action>`)

  En az bir uyarı yanıt olarak gerçekleştirilen eylemleri kümesini tanımlar.

* **MaximumAlertActions** (tür `<alert_action>`)

  En fazla uyarı yanıt olarak gerçekleştirilen eylemleri kümesini tanımlar.

İstasyon düzeyinde de gördüğünüz **benzetimi** nesneleri. Bu nesneler yalnızca bağlı Fabrika benzetimi yapılandırmak için kullanılır ve gerçek topolojisini yapılandırmak için kullanılmamalıdır.

## <a name="how-the-configuration-data-is-used-at-runtime"></a>Yapılandırma verilerini çalışma zamanında nasıl kullanılır

Yapılandırma dosyasında kullanılan tüm özellikleri, bunların nasıl kullanıldığı bağlı olarak farklı kategoride gruplandırılabilir. Bu kategoriler şunlardır:

### <a name="visual-appearance"></a>Görsel görünümünü

Bu kategorideki özellikleri bağlı Fabrika Pano görünümünü tanımlayın. Örneklere şunlar dahildir:

* Ad
* Açıklama
* Görüntü
* Konum
* Birimler
* Görünür

### <a name="internal-topology-tree-addressing"></a>İç topolojisi ağaç adresleme

WebApp topoloji düğümlerinin tümünün bilgilerini içeren bir iç veri sözlüğü tutar. Özellikler **GUID** ve **OpcUri** anahtarları olarak bu sözlük erişmek için kullanılır ve benzersiz olması gerekir.

### <a name="oeekpi-computation"></a>OEE/KPI hesaplama

Bağlı Fabrika benzetimi OEE/KPI rakamlarını tarafından parametreli:

* OPC UA düğümü hesaplamadaki dahil edilecek değerleri.
* Şekil, telemetri değerleri nasıl hesaplanır.

Bağlı Fabrika kullanan OEE formüller tarafından yayınlanan olarak http://oeeindustrystandard.oeefoundation.org.

OPC UA düğüm nesneleri istasyonları OEE/KPI hesaplama kullanım için etiketleme etkinleştirin. **İlgi** özelliği için hangi OEE/KPI şekil OPC UA düğüm değerinin kullanılması gerektiğini gösterir. **OpCode** özelliği tanımlar değeri hesaplamanın nasıl eklenir.

### <a name="alert-handling"></a>Uyarı işleme

Bağlı Fabrika basit minimum/maksimum eşik tabanlı uyarı oluşturma düzeneğini destekler. Bu uyarılara yanıt olarak yapılandırabilirsiniz önceden tanımlanmış eylemler vardır. Aşağıdaki özellikler bu düzenek kontrol edin:

* Maksimum
* Minimum
* MaximumAlertActions
* MinimumAlertActions

## <a name="correlating-to-telemetry-data"></a>Telemetri verileri ilişkilendirme

Son değer görselleştirme veya zaman serisi Insight sorgular oluşturma gibi belirli işlemler, WebApp adresleme düzeni alınan telemetri verilerini gerekir. Bağlı Fabrika gönderilen telemetriyi de iç veri yapılarını depolanması gerekir. Bu işlemler etkinleştirme iki özellikleri istasyon (OPC UA sunucusu) ve OPC UA düğüm düzeyinde şunlardır:

* **OpcUri**

  Tanımlar (genel benzersiz) OPC UA sunucunun telemetri gelir. Alınan iletileri, bu özellik olarak gönderilen **ApplicationUri**.

* **nodeId**

  OPC UA Server'daki düğüm değerini tanımlar. Özelliğin biçimi olmalıdır OPC UA belirtiminde belirtildiği gibi. Alınan iletileri, bu özellik olarak gönderilen **nodeId**.

Denetleme [bu](https://github.com/Azure/iot-edge-opc-publisher) telemetri verilerini bağlı OPC Publisher'ı kullanarak fabrika ayarlarına nasıl alınan hakkında daha fazla bilgi için GitHub sayfası.

## <a name="example-how-kpi1-is-calculated"></a>Örnek: KPI1 hesaplanan nasıl

Yapılandırmada `ContosoTopologyDescription.json` dosyası denetler OEE/KPI rakamları nasıl hesaplanır. Aşağıdaki örnek, bu dosya özelliklerinde KPI1 hesaplama nasıl kontrol gösterir.

Fabrika KPI1 bağlı, son bir saat içinde başarıyla üretilen ürünleri sayısını ölçmek için kullanılır. Her istasyon (OPC UA sunucusu) bağlı Fabrika benzetimde OPC UA düğümü sağlar (`NodeId: "ns=2;i=385"`), bu KPI hesaplamak için telemetri sağlar.

Bu OPC UA düğüm yapılandırması aşağıdaki kod parçacığını gibi görünür:

```json
{
  "NodeId": "ns=2;i=385",
  "SymbolicName": "NumberOfManufacturedProducts",
  "Relevance": [ "Kpi1", "OeeQuality_Good" ],
  "OpCode": "SubMaxMin"
},
```

Bu yapılandırma, zaman serisinin kullanarak bu düğümün telemetri değerlerini sorgulama etkinleştirir. Zaman serisi Öngörüler sorgu alır:

* Değerlerinin sayısı.
* En az değer.
* Üst düzeyde değeri.
* Tüm değerlerin ortalamasını.
* Tümü için tüm değerlerin toplamını benzersiz **OpcUri** (**ApplicationUri**), **nodeId** çiftler halinde belirli bir timespan.

Bir özelliği **NumberOfManufactureredProducts** düğümü değerdir yalnızca artırır. Timespan üretilen ürünleri sayısını hesaplamak için Fabrika bağlı kullanan **OpCode** **SubMaxMin**. Hesaplamanın en düşük değer timespan başlangıcında ve en büyük değer timespan sonunda alır.

**OpCode** maksimum ve minimum değer fark sonucunu hesaplamak için hesaplama mantık yapılandırmada yapılandırır. Daha sonra bu sonuçlar olan alt kök (Genel) düzeyinde kadar birikmiş ve panosunda gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

A önerilen sonraki adımdır öğrenmek için nasıl [bağlı Fabrika Çözüm Hızlandırıcısı için Windows veya Linux üzerinde bir ağ geçidi dağıtma](iot-accelerators-connected-factory-gateway-deployment.md).