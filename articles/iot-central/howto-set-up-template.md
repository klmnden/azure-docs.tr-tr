---
title: Bir Azure IOT Central uygulamasına cihaz şablonunda ayarlama | Microsoft Docs
description: Ölçümler, ayarları, özellikleri, kuralları ve bir pano ile bir cihaz şablonu ayarlama konusunda bilgi edinin.
author: viv-liu
ms.author: viviali
ms.date: 06/19/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: d27fd9460685c08a2b13936415935f5aaf893797
ms.sourcegitcommit: dda9fc615db84e6849963b20e1dce74c9fe51821
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622406"
---
# <a name="set-up-a-device-template"></a>Cihaz şablonu ayarlama

Cihaz özellikleri ve davranışları için bir Azure IOT Central uygulamasına bağlanan cihaz türü tanımlayan bir şema şablonudur.

Örneğin, bir oluşturucu aşağıdaki özelliklere sahip bir bağlantılı fanı için bir cihaz şablon oluşturabilirsiniz:

- Sıcaklık telemetri ölçüm
- Konum ölçüm
- Fan motor hata olay ölçümü
- İşletim durumu ölçümü fanı
- Hız ayarını fanı
- Uyarıları göndermek kuralları
- Cihaz genel bir görünümünü sunan bir Pano

Bu cihaz şablondan operatörün oluşturabilir ve gerçek fanı adlarla gibi bağlıyorsunuz **fan-1** ve **fan-2**. Bu fanlar, ölçümleri, ayarları, özellikleri, kuralları ve uygulamanızın kullanıcılarının izlemek ve yönetmek bir Pano vardır.

> [!NOTE]
> Oluşturucular ve yöneticiler oluşturabilir, düzenleme ve cihaz şablonları silin. Herhangi bir kullanıcı aygıtları üzerinde oluşturabilirsiniz **Device Explorer** mevcut cihaz şablonları sayfasından.

## <a name="create-a-device-template"></a>Bir cihaz şablonu oluşturma

1. Gidin **cihaz şablonları** sayfası.

2. Bir şablon oluşturmak için seçerek başlayın **+ yeni**.

3. Hızlıca kullanmaya başlamak için var olan önceden oluşturulmuş şablonlardan birini seçin. Aksi takdirde seçin **özel**, bir ad girin ve tıklayın **Oluştur** kendi şablonunuzu sıfırdan oluşturmak için.

   ![Cihaz Şablon Kitaplığı](./media/howto-set-up-template/newtemplate.png)

4. Özel bir şablon oluştururken gördüğünüz **cihaz ayrıntıları** cihaz şablonunuza sayfası. Bir cihaz şablonu oluşturduğunuzda, IOT Central bir simülasyon cihazı otomatik olarak oluşturur. Bir sanal cihaz uygulamanızın davranışını, gerçek bir cihaz bağlanmadan önce sınamanızı sağlar.

Aşağıdaki bölümlerde sekmelerin her birinde üzerinde açıklanmıştır **cihaz şablonu** sayfası.

## <a name="measurements"></a>Ölçümler

Ölçümler, bir CİHAZDAN geliyorsa verilerdir. Birden çok ölçümleri cihazınızın yeteneği karşılamak için cihaz şablonunuza ekleyebilirsiniz.

- **Telemetri** Cihazınızı zaman içinde topladığı sayısal veri noktaları ölçümlerdir. Bunlar, sürekli bir akış temsil. Sıcaklık buna bir örnektir.
- **Olay** ölçümlerdir bir cihazda anlamın temsil eden zaman içinde nokta veri. Önem derecesi, bir olayın önem temsil eder. Fan motor hata buna bir örnektir.
- **Durum** ölçümleri bir süre cihaz veya bileşenlerinin durumunu temsil eder. Örneğin, bir fanı modu sahip olacak şekilde tanımlanabilir **işletim** ve **durduruldu** iki olası durum olarak.
- **Konum** ölçümleri bir süre boyunca, cihazın boylam ve enlem koordinatları olan. Örneğin, bir sporseverseniz bir konumdan diğerine taşınabilir.

### <a name="create-a-telemetry-measurement"></a>Telemetri ölçü oluşturma

Yeni bir telemetri ölçüm eklemek için seçin **+ yeni ölçüm**, seçin **Telemetri** ölçüm yazın ve formda ayrıntılarını girin.

> [!NOTE]
> Cihaz şablonu alan adlarını, karşılık gelen cihaz kodu sırada gerçek bir cihaz bağlandığında uygulamada görüntülenecek telemetri ölçüm için özellik adları eşleşmelidir. Aşağıdaki bölümlerde cihaz şablonlarını tanımlamak devam ederken ayarları, cihaz özellikleri ve komutları yapılandırırken de aynısını yapın.
.PNG gibi yeni bir sıcaklık telemetri ölçüm ekleyebilirsiniz:

| Görünen Ad        | Alan Adı    |  Birimler    | Min   |Maks|
| --------------------| ------------- |-----------|-------|---|
| Sıcaklık         | Temp          |  degC     |  0    |100|

!["Telemetri oluşturma" form sıcaklık ölçüm için Ayrıntılar](./media/howto-set-up-template/measurementsform.png)

Seçtikten sonra **Kaydet**, **sıcaklık** ölçüm ölçümleri listesinde görünür. Kısa bir süre içinde bir sanal CİHAZDAN sıcaklık verilerini görselleştirme bakın.

Telemetri görüntülerken, toplama aşağıdaki seçeneklerden birini seçebilirsiniz: Ortalama, Minimum, maksimum, Sum ve sayısı. **Ortalama** grafikteki varsayılan toplama olarak seçilir.

> [!NOTE]
> Veri telemetri ölçü bir kayan türüdür nokta sayısı.

### <a name="create-an-event-measurement"></a>Bir olay ölçü oluşturma

Yeni bir olay ölçüm eklemek için seçin **+ yeni ölçüm** seçip **olay** ölçüm türü. Ayrıntıları girin **olay oluşturma** formu.

Sağlamak **görünen ad**, **alan adı**, ve **önem derecesi** olayla ilgili ayrıntıları. Önem derecesi kullanılabilir üç düzeyi arasından seçim yapabilirsiniz: **Hata**, **uyarı**, ve **bilgi**.

Örneğin, yeni bir ekleyebilirsiniz **Fan Motor hata** olay.

| Görünen Ad        | Alan Adı    |  Varsayılan önem derecesi |
| --------------------| ------------- |-----------|
| Fan Motoru Hatası     | fanmotorerror |  Hata    |

!["Olay oluşturma" form fan motor olayla ilgili ayrıntıları](./media/howto-set-up-template/eventmeasurementsform.png)

Seçtikten sonra **Kaydet**, **Fan Motor hata** ölçüm ölçümleri listesinde görünür. Kısa bir süre içinde bir sanal CİHAZDAN olay verilerini görselleştirme bakın.

Bir olay hakkında daha fazla ayrıntı görüntülemek için grafik olay simgesini seçin:

!["Fan Motor Error" olayla ilgili ayrıntıları](./media/howto-set-up-template/eventmeasurementsdetail.png)

> [!NOTE]
> Veri türü olay ölçümü dizedir.

### <a name="create-a-state-measurement"></a>Bir durum ölçü oluşturma

Yeni bir durum ölçüm eklemek için seçin **+ yeni ölçüm** düğmesini tıklatın ve seçin **durumu** ölçüm türü. Ayrıntıları girin **oluşturma durumu** formu.

İçin ayrıntıları sağlayın **görünen ad**, **alan adı**, ve **değerleri** durumu. Her değer, değer grafikleri ve tabloları göründüğünde kullanılacak bir görünen ad da sahip olabilirsiniz.

Örneğin, yeni bir ekleyebilirsiniz **fanı modu** cihaza gönderebilir, iki olası değerleri içeren durum **işletim** ve **durduruldu**.

| Görünen Ad | Alan Adı    |  1 değeri   | Görünen Ad | Değer 2    |Görünen Ad  | 
| -------------| ------------- |----------- | -------------| -----------| -------------|
| Fan Modu     | fanmode       |  1\.         | İşletim    |     0      | Durduruldu      |

![Fan modu için Ayrıntıları "Düzenleme durumu" formu](./media/howto-set-up-template/statemeasurementsform.png)

Seçtikten sonra **Kaydet**, **fanı modu** durumu ölçüm ölçümleri listesinde görünür. Kısa bir süre içerisinde, sanal cihaz durumu verileri görselleştirme görürsünüz.

Cihaz, küçük bir süre içinde çok fazla veri noktasının gönderirse, durumu ölçümü ile farklı bir görsel görünür. Grafiği kronolojik sırada bu süre içinde tüm veri noktalarına görüntülemeyi seçin. Grafiği çizilen ölçüm görmek için zaman aralığını da daraltabilirsiniz.

> [!NOTE]
> Veri türü durumu ölçümü dizedir.

### <a name="create-a-location-measurement"></a>Bir konum ölçü oluşturma

Yeni bir konum ölçüm eklemek için seçin **+ yeni ölçüm**, seçin **konumu** ölçüm yazın ve ayrıntılarını girin **oluşturma ölçüm** formu.

Örneğin, yeni bir konum telemetri ölçüm ekleyebilirsiniz:

| Görünen Ad        | Alan Adı    |
| --------------------| ------------- |
| Varlık konumu      |  assetloc     |

![Konum ölçüm ayrıntılarını "Konumu oluşturma" formu](./media/howto-set-up-template/locationmeasurementsform.png)

Seçtikten sonra **Kaydet**, **konumu** ölçüm ölçümleri listesinde görünür. Kısa bir süre içinde sanal cihaz konum verileri görselleştirmesini görürsünüz.

Konum görüntülerken, aşağıdaki seçeneklerden birini seçebilirsiniz: son konumunuzu ve konum geçmişinizi. **Konum geçmişinizi** yalnızca seçili zaman aralığı üzerinde uygulanır.

Veri konumu ölçüm boylam, enlem ve isteğe bağlı bir yükseklik içeren bir nesne türüdür. Aşağıdaki kod parçacığı, JavaScript yapısını gösterir:

```javascript
assetloc: {
  lon: floating point number,
  lat: floating point number,
  alt?: floating point number
}
```

## <a name="settings"></a>Ayarlar

Ayarları bir cihaz denetler. Bunlar cihaza girişleri olanak tanır. Birden çok ayarları kutucukları olarak görünür, cihaz şablonunuza ekleyebilirsiniz **ayarları** işleçleri kullanmak için sekmesinde. Birçok türde ayarlar ekleyebilirsiniz: sayı, metin, tarih, geçiş ve bölüm etiketi.

Ayarları üç durumdan birinde olabilir. Cihaz bu durumu bildirir.

- **Eşitlenen**: Cihaz ayar değeri yansıtacak şekilde değişti.

- **Bekleyen**: Cihaz ayar değeri şu anda değişiyor.

- **Hata**: Cihazı bir hata döndürdü.

Seçerek yeni bir fanı hızı ayarı gibi ekleyebilirsiniz **ayarları** ve yeni girerek **numarası** ayarı:

| Görünen Ad  | Alan Adı    |  Birimler  | Ondalık sayı |İlk|
| --------------| ------------- |---------| ---------|---- |
| Fan hızı     | fanSpeed      | RPM     | 2        | 0   |

![Hızlı ayarları için Ayrıntılar "Sayı Yapılandır" formu](./media/howto-set-up-template/settingsform.png)

Seçtikten sonra **Kaydet**, **fanı hızı** ayarı bir kutucuk olarak görünür. Ayarını kullanabilirsiniz operatörün **Device Explorer** cihazın fanı hızını değiştirmek için sayfa.

## <a name="properties"></a>Özellikler

Özellikler sabit cihaz konumu ve seri numarası gibi cihaz ile ilişkili meta verilerdir. Kutucukları olarak görünür, cihaz şablonunuzu birden çok özellik eklemek **özellikleri** sekmesi. Bir özelliği, sayı, metin, tarih, Değiştir, cihaz özelliği, etiket veya sabit bir konum gibi bir türü vardır. Operatör, bunlar bir cihaz oluşturur ve herhangi bir zamanda bu değerleri düzenleyebilirsiniz özellikleri için değerleri belirtir. Cihaz özellikleri salt okunurdur ve CİHAZDAN uygulamaya gönderilir. Operatörün cihaz özelliklerini değiştiremezsiniz. Gerçek bir cihaz bağlandığında, cihaz özelliği kutucuğuna uygulamada güncelleştirir.

İki özellik kategorisi vardır:

- _Cihaz özellikleri_ , cihaz IOT Central uygulamaya bildirir. Cihaz özellikleri, cihaz tarafından bildirilen salt okunur değerler ve gerçek bir cihaz bağlandığında uygulama içinde güncelleştirilir.
- _Uygulama özellikleri_ uygulamada depolanır ve operatör tarafından düzenlenebilir. Uygulama özellikleri, yalnızca uygulama içinde depolanır ve bir cihaz tarafından hiçbir zaman görülmedi.

Örneğin, cihazın son hizmet verilen tarih yeni bir ekleyebilirsiniz **tarih** özelliği (uygulama özelliği) **özellikleri** sekmesinde:

| Görünen Ad  | Alan Adı | İlk Değer   |
| --------------| -----------|-----------------|
| Son hizmet tarihi      | lastServiced        | 01/29/2019     |

!["Son Serviced Yapılandır" form "Özellikler" sekmesinde](./media/howto-set-up-template/propertiesform.png)

Seçtikten sonra **Kaydet**, son tarih hizmet cihaz bir kutucuk olarak görünür.

Kutucuk oluşturduktan sonra uygulama özellik değeri değiştirebilirsiniz **Device Explorer**.

### <a name="create-a-location-property"></a>Konum özelliği oluşturma

Konum verilerinizi Azure IOT Central için coğrafik bağlam sağlamak ve bir enlem ve boylam koordinatları veya posta adresi eşleyin. Azure haritalar IOT Central bu özelliği sağlar.

İki tür yerleşim özellikleri ekleyebilirsiniz:

- **Konumu olarak bir uygulama özelliği**, uygulama içinde depolanır. Uygulama özellikleri, yalnızca uygulama içinde depolanır ve bir cihaz tarafından hiçbir zaman görülmedi.
- **Konumu olarak bir cihaz özelliği**, hangi cihaz uygulamaya bildirir. Bu özelliğin türünü en iyi için statik bir konum olarak kullanılır.

> [!NOTE]
> Konum özelliği olarak geçmişini kaydetmez. Geçmiş isterseniz, bir konum ölçüm kullanın.

#### <a name="add-location-as-an-application-property"></a>Konumu bir uygulama özelliği Ekle

IOT Central uygulamanızda Azure haritalar'ı kullanarak bir uygulama özelliği olarak location özelliği oluşturabilirsiniz. Örneğin, cihaz yükleme adresi ekleyebilirsiniz:

1. Gidin **özellikleri** sekmesi.

2. Kitaplıkta, seçin **konumu**.

3. Yapılandırma **görünen ad**, **alan adı**ve (isteğe bağlı olarak) **başlangıç değeri** konumu için.

    | Görünen Ad  | Alan Adı | İlk Değer |
    | --------------| -----------|---------|
    | Yükleme adresi | installAddress | Microsoft, 1 Microsoft Way, Redmond, WA 98052   |

   ![Konum ayrıntılarını "Konumu Yapılandır" formu](./media/howto-set-up-template/locationcloudproperty2.png)

   Bir konum eklemek için desteklenen iki biçim vardır:
   - **Adresi olarak konumu**
   - **Koordinatları olarak konumu**

4. **Kaydet**’i seçin. Bir işleç konum değeri güncelleştirebilirsiniz **Device Explorer**.

#### <a name="add-location-as-a-device-property"></a>Konumu bir cihaz özelliği Ekle

Location özelliğini cihaz raporlarına bir cihaz özelliği oluşturabilirsiniz. Örneğin, cihaz konumu izlemek istiyorsanız:

1. Gidin **özellikleri** sekmesi.

2. Seçin **cihaz özelliği** kitaplığından.

3. Alan adı ve görünen adı'nı yapılandırın ve seçin **konumu** veri türü olarak:

    | Görünen Ad  | Alan Adı | Veri Türü |
    | --------------| -----------|-----------|
    | Cihaz konumu | deviceLocation | location  |

   > [!NOTE]
   > İlgili cihaz kod özellik adları alan adları eşleşmelidir

   !["Cihaz özelliklerini yapılandırma" form ile konum için ayrıntıları](./media/howto-set-up-template/locationdeviceproperty2.png)

Gerçek cihaz bağlandıktan sonra cihaz tarafından gönderilen değerine sahip bir cihaz özelliği güncelleştirildikçe konumu, eklendi. Konum özelliğinizi yapılandırdıktan sonra [cihaz pano konumu görselleştirmek için bir harita Ekle](#add-a-location-in-the-dashboard).

## <a name="commands"></a>Komutlar

Komutlar, bir cihazı uzaktan yönetmek için kullanılır. Bunlar, cihazda komutlarını çalıştırmak işleçleri etkinleştirin. Birden çok komut kutucukları olarak görünür, cihaz şablonunuza ekleyebilirsiniz **komutları** işleçleri kullanmak için sekmesinde. Cihaz Oluşturucu olarak ihtiyaçlarınıza göre komutlar tanımlar esnekliğine sahip olursunuz.

Bir komutu bir ayardan farklı mı?

* **Ayar**: Bir ayarı bir cihaz için uygulamak istediğiniz bir yapılandırmadır. Siz değiştirene kadar bu yapılandırma kalıcı hale getirmek için bir cihaz kullanmanız gerekir. Örneğin, Dondurucu Sıcaklığın ayarlamak istediğiniz ve Dondurucu bile yeniden başlatıldığında, ayarlamak istediğiniz.

* **Komut**: Anında bir komutu cihazda uzaktan IOT Central ' çalıştırma için komutları kullanın. Bir cihazı bağlı değilse, komut zaman aşımına uğrar ve başarısız olur. Örneğin, bir cihazı yeniden başlatmak istiyor.

Örneğin, yeni bir ekleyebilirsiniz **Yankı** komutunu seçerek **komutları** sekmesini seçip, **+ yeni komut**ve yeni komut ayrıntıları girerek:

| Görünen Ad  | Alan Adı | Varsayılan Zaman Aşımı | Veri Türü |
| --------------| -----------|---------------- | --------- |
| Yankı Komutu  | echo       |  30             | text      |

![Yankı ayrıntılarını "Komutunu yapılandırma" formu](./media/howto-set-up-template/commandsecho1.png)

Seçtikten sonra **Kaydet**, **Yankı** komut bir kutucuk görünür ve gelen kullanılmak üzere hazırdır **Device Explorer** , gerçek bir cihaz bağlandığında. Alan adları, komut başarılı bir şekilde çalıştırılması için komutları sırayla ilgili cihaz kod özellik adları eşleşmelidir.

## <a name="rules"></a>Kurallar

Kuralları cihazları neredeyse gerçek zamanlı izleme olanak tanır. Kuralları otomatik olarak kuralı tetiklendiğinde e-posta gönderme gibi eylemleri çağırır. Bir kural türü bugün kullanılabilir:

- **Telemetri kuralı**, seçili cihaz telemetrisi belirtilen eşiği aştığında olduğunda tetiklenir. [Telemetri kuralları hakkında daha fazla bilgi](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Pano

Pano, burada bir cihaz hakkında bilgi için bir işleç gittiği yerdir. Bir oluşturucu, operatörlerin cihaz nasıl davranmakta anlamak için bu sayfayı kutucuklar ekleyin. Pano kutucukları görüntü, çizgi grafik, çubuk grafik, ana performans göstergesinin (KPI), ayarları ve özellikleri gibi birçok türleri ekleyin ve etiketi.

Örneğin, ekleyebilirsiniz bir **ayarları ve özellikleri** seçerek seçim ayarları ve özellikleri geçerli değerlerini görüntülemek için kutucuk **Pano** sekmesi ve kitaplıktan bir kutucuğa:

!["Cihaz ayrıntıları Yapılandır" form ayarlarına ve özelliklerine ilişkin ayrıntılı](./media/howto-set-up-template/dashboardsettingsandpropertiesform1.png)

Şimdi ne zaman bir işleç görünümleri Pano **Device Explorer**, kutucuğu görebilir.

### <a name="add-a-location-in-the-dashboard"></a>Panoda konum ekleyin

Bir konum ölçüm yapılandırdıysanız, cihaz Panonuzda bir harita konumla görselleştirebilirsiniz.

1. Gidin **Pano** sekmesi.

1. Cihaz Panoda seçin **harita** kitaplığından.

1. Haritayı bir başlık verin. Aşağıdaki örnek başlığa sahip **cihaz geçerli konumu**. Ardından, daha önce yapılandırılan konum ölçümü seçin **ölçümleri** sekmesi. Aşağıdaki örnekte, **varlık konumu** ölçüm seçili:

   ![Form başlığı ve özelliklerine ilişkin ayrıntıları "Eşlemesi yapılandırmak"](./media/howto-set-up-template/locationcloudproperty5map.png)

1. **Kaydet**’i seçin. Harita kutucuğunu şimdi seçtiğiniz konum görüntülenir.

Harita kutucuğunu yeniden boyutlandırabilirsiniz. Ne zaman bir işleç Pano görünümleri **Device Explorer**, tüm Pano kutucuklarının yapılandırdıysanız, bir konum eşlemesi gibi görülebilir.

Azure IOT Central kutucukları kullanma hakkında daha fazla bilgi için bkz. [Pano kutucukları kullanacak](howto-use-tiles.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central, uygulamanızdaki bir cihaz şablonu ayarlamak öğrendiniz, şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [Yeni bir cihaz şablon sürümü oluşturma](howto-version-device-template.md)
> [bir MXChip IOT DevKit cihazını Azure IOT Central uygulamanızı bağlamak](howto-connect-devkit.md)
> [Azure genel istemci uygulamaya bağlama IOT Central uygulamasına (Node.js)](howto-connect-nodejs.md)