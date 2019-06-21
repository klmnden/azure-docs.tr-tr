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
ms.openlocfilehash: 58f50a1a2b90b4b5f9708bf0f1a7cb51db8e47ae
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275971"
---
# <a name="set-up-a-device-template"></a>Cihaz şablonu ayarlama

Cihaz özellikleri ve davranışları için bir Azure IOT Central uygulamasına bağlanan cihaz türü tanımlayan bir şema şablonudur.

Örneğin, bir oluşturucu a: olan bir IOT bağlı fanı için bir cihaz şablon oluşturabilirsiniz

- Sıcaklık telemetri ölçüm

- Fan motor hata olay ölçümü

- İşletim durumu ölçümü fanı

- Hız ayarını fanı

- Konum özelliği

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

### <a name="create-a-telemetry-measurement"></a>Telemetri ölçü oluşturma

Yeni bir telemetri ölçüm eklemek için seçin **+ yeni ölçüm**, seçin **Telemetri** ölçüm yazın ve formda ayrıntılarını girin.

> [!NOTE]
> Cihaz şablonu alan adlarını, karşılık gelen cihaz kodu sırada gerçek bir cihaz bağlandığında uygulamada görüntülenecek telemetri ölçüm için özellik adları eşleşmelidir. Aşağıdaki bölümlerde cihaz şablonlarını tanımlamak devam ederken ayarları, cihaz özellikleri ve komutları yapılandırırken de aynısını yapın.
.PNG gibi yeni bir sıcaklık telemetri ölçüm ekleyebilirsiniz:

| Görünen ad        | Alan Adı    |  Birimler    | Min   |Maks|
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

| Görünen ad        | Alan Adı    |  Varsayılan önem derecesi |
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

| Görünen ad | Alan Adı    |  1 değeri   | Görünen ad | Değer 2    |Görünen ad  | 
| -------------| ------------- |----------- | -------------| -----------| -------------|
| Fan Modu     | fanmode       |  1         | İşletim    |     0      | Durduruldu      |

![Fan modu için Ayrıntıları "Düzenleme durumu" formu](./media/howto-set-up-template/statemeasurementsform.png)

Seçtikten sonra **Kaydet**, **fanı modu** durumu ölçüm ölçümleri listesinde görünür. Kısa bir süre içerisinde, sanal cihaz durumu verileri görselleştirme görürsünüz.

Cihaz, küçük bir süre içinde çok fazla veri noktasının gönderirse, durumu ölçümü ile farklı bir görsel görünür. Grafiği kronolojik sırada bu süre içinde tüm veri noktalarına görüntülemeyi seçin. Grafiği çizilen ölçüm görmek için zaman aralığını da daraltabilirsiniz.

> [!NOTE]
> Veri türü durumu ölçümü dizedir.

## <a name="settings"></a>Ayarlar

Ayarları bir cihaz denetler. Bunlar cihaza girişleri olanak tanır. Birden çok ayarları kutucukları olarak görünür, cihaz şablonunuza ekleyebilirsiniz **ayarları** işleçleri kullanmak için sekmesinde. Birçok türde ayarlar ekleyebilirsiniz: sayı, metin, tarih, Değiştir, seçim listesini ve bölüm etiketi.

Ayarları üç durumdan birinde olabilir. Cihaz bu durumu bildirir.

- **Eşitlenen**: Cihaz ayar değeri yansıtacak şekilde değişti.

- **Bekleyen**: Cihaz ayar değeri şu anda değişiyor.

- **Hata**: Cihazı bir hata döndürdü.

Seçerek yeni bir fanı hızı ayarı gibi ekleyebilirsiniz **ayarları** ve yeni girerek **numarası** ayarı:

| Görünen ad  | Alan Adı    |  Birimler  | Ondalık sayı |İlk|
| --------------| ------------- |---------| ---------|---- |
| Fan hızı     | fanSpeed      | RPM     | 2        | 0   |

![Hızlı ayarları için Ayrıntılar "Sayı Yapılandır" formu](./media/howto-set-up-template/settingsform.png)

Seçtikten sonra **Kaydet**, **fanı hızı** ayarı bir kutucuk olarak görünür. Ayarını kullanabilirsiniz operatörün **Device Explorer** cihazın fanı hızını değiştirmek için sayfa.

## <a name="properties"></a>Özellikler

Özellikler, cihaz konumu ve seri numarası gibi cihaz ile ilişkili meta verilerdir. Kutucukları olarak görünür, cihaz şablonunuzu birden çok özellik eklemek **özellikleri** sekmesi. Bir özellik, sayı, metin, tarih, Değiştir, cihaz özelliği, etiket veya konumu gibi bir tür olabilir. Bunlar bir cihaz oluşturur ve herhangi bir zamanda bu değerleri düzenleyebilirsiniz operatörün özellikleri için değer belirtebilirsiniz. Cihaz özellikleri salt okunurdur ve CİHAZDAN uygulamaya gönderilir. Operatörün cihaz özelliklerini değiştiremezsiniz. Gerçek bir cihaz bağlandığında, özellik güncelleştirmeleri uygulamada cihazdır.

İki özellik kategorisi vardır:

- _Cihaz özellikleri_ , cihaz IOT Central uygulamaya bildirir. Cihaz özellikleri, cihaz tarafından bildirilen salt okunur değerler ve gerçek bir cihaz bağlandığında uygulama içinde güncelleştirilir.
- _Uygulama özellikleri_ uygulamada depolanır ve operatör tarafından düzenlenebilir. Cihaz, uygulama özellikleri tanımaz.

Örneğin, cihazın son hizmet verilen tarih yeni bir ekleyebilirsiniz **tarih** özelliği (uygulama özelliği) **özellikleri** sekmesinde:

| Görünen ad  | Alan Adı | İlk Değer   |
| --------------| -----------|-----------------|
| Son hizmet tarihi      | lastServiced        | 01/29/2019     |

!["Son Serviced Yapılandır" form "Özellikler" sekmesinde](./media/howto-set-up-template/propertiesform.png)

Seçtikten sonra **Kaydet**, son tarih hizmet cihaz bir kutucuk olarak görünür.

Kutucuk oluşturduktan sonra uygulama özellik değeri değiştirebilirsiniz **Device Explorer**.

### <a name="create-a-location-property-through-azure-maps"></a>Bir konum özelliği üzerinden Azure haritalar'ı oluşturma

Konum verilerinizi Azure IOT Central, coğrafi bağlam vermek ve herhangi bir enlem ve boylam koordinatlarını sokak adresi eşleyin. Veya enlem ve boylam harita koordinatları. Azure haritalar IOT Central bu özelliği sağlar.

İki tür yerleşim özellikleri ekleyebilirsiniz:

- **Konumu olarak bir uygulama özelliği**, uygulama içinde depolanır. Cihaz, uygulama özellikleri tanımaz.
- **Konumu olarak bir cihaz özelliği**, hangi cihaz uygulamaya bildirir.

#### <a name="add-location-as-an-application-property"></a>Konumu bir uygulama özelliği Ekle

IOT Central uygulamanızda Azure haritalar'ı kullanarak bir uygulama özelliği olarak location özelliği oluşturabilirsiniz. Örneğin, cihaz yükleme adresi ekleyebilirsiniz:

1. Gidin **özellikleri** sekmesi.

2. Kitaplıkta, seçin **konumu**.

3. Yapılandırma **görünen ad**, **alan adı**ve (isteğe bağlı olarak) **başlangıç değeri** konumu için.

    | Görünen ad  | Alan Adı | İlk Değer |
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

    | Görünen ad  | Alan Adı | Veri Türü |
    | --------------| -----------|-----------|
    | Cihaz konumu | deviceLocation | location  |

   > [!NOTE]
   > İlgili cihaz kod özellik adları alan adları eşleşmelidir

   !["Cihaz özelliklerini yapılandırma" form ile konum için ayrıntıları](./media/howto-set-up-template/locationdeviceproperty2.png)

Gerçek cihaz bağlandıktan sonra bir cihaz özelliği eklediğiniz konum cihaz tarafından gönderilen değer ile güncelleştirilir. Location özelliği yapılandırdığınıza göre şunları yapabilirsiniz [cihaz pano konumu görselleştirmek için bir harita Ekle](#add-an-azure-maps-location-in-the-dashboard).

## <a name="commands"></a>Komutlar

Komutlar, bir cihazı uzaktan yönetmek için kullanılır. Bunlar, cihazda komutlarını çalıştırmak işleçleri etkinleştirin. Birden çok komut kutucukları olarak görünür, cihaz şablonunuza ekleyebilirsiniz **komutları** işleçleri kullanmak için sekmesinde. Cihaz Oluşturucu olarak ihtiyaçlarınıza göre komutlar tanımlar esnekliğine sahip olursunuz.

Bir komutu bir ayardan farklı mı?

* **Ayar**: Bir ayarı bir cihaz için uygulamak istediğiniz bir yapılandırmadır. Siz değiştirene kadar bu yapılandırma kalıcı hale getirmek için bir cihaz kullanmanız gerekir. Örneğin, Dondurucu Sıcaklığın ayarlamak istediğiniz ve Dondurucu bile yeniden başlatıldığında, ayarlamak istediğiniz.

* **Komut**: Anında bir komutu cihazda uzaktan IOT Central ' çalıştırma için komutları kullanın. Bir cihazı bağlı değilse, komut zaman aşımına uğrar ve başarısız olur. Örneğin, bir cihazı yeniden başlatmak istiyor.

Örneğin, yeni bir ekleyebilirsiniz **Yankı** komutunu seçerek **komutları** sekmesini seçip, **+ yeni komut**ve yeni komut ayrıntıları girerek:

| Görünen ad  | Alan Adı | Varsayılan Zaman Aşımı | Veri Türü |
| --------------| -----------|---------------- | --------- |
| Yankı Komutu  | echo       |  30             | metin      |

![Yankı ayrıntılarını "Komutunu yapılandırma" formu](./media/howto-set-up-template/commandsecho1.png)

Seçtikten sonra **Kaydet**, **Yankı** komut bir kutucuk görünür ve gelen kullanılmak üzere hazırdır **Device Explorer** , gerçek bir cihaz bağlandığında. Alan adları, komutun başarıyla çalıştırılacak komutları sırayla ilgili cihaz kod özellik adları eşleşmelidir.

## <a name="rules"></a>Kurallar

Kuralları cihazları neredeyse gerçek zamanlı izleme olanak tanır. Kuralları otomatik olarak kuralı tetiklendiğinde e-posta gönderme gibi eylemleri çağırır. Bir kural türü bugün kullanılabilir:

- **Telemetri kuralı**, seçili cihaz telemetrisi belirtilen eşiği aştığında olduğunda tetiklenir. [Telemetri kuralları hakkında daha fazla bilgi](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Pano

Bir cihaz hakkında bilgi için bir işleç gidebilecekleri panodur. Bir oluşturucu işleçleri cihazın nasıl davrandığını anlamanıza yardımcı olması için bu sayfada kutucuklar ekleyebilirsiniz. Birden çok Pano kutucukları, aygıt şablonunuza ekleyebilirsiniz. Pano kutucukları görüntü, çizgi grafik, çubuk grafik, ana performans göstergesinin (KPI), ayarları ve özellikleri gibi birçok türleri ekleyin ve etiketi.

Örneğin, ekleyebilirsiniz bir **ayarları ve özellikleri** seçerek seçim ayarları ve özellikleri geçerli değerlerini görüntülemek için kutucuk **Pano** sekmesi ve kitaplıktan bir kutucuğa:

!["Cihaz ayrıntıları Yapılandır" form ayarlarına ve özelliklerine ilişkin ayrıntılı](./media/howto-set-up-template/dashboardsettingsandpropertiesform1.png)

Şimdi ne zaman bir işleç görünümleri Pano **Device Explorer**, kutucuğu görebilir.

### <a name="add-an-azure-maps-location-in-the-dashboard"></a>Azure haritalar konum Pano Ekle

Konum özelliği yapılandırdıysanız, cihaz Panonuzda bir eşlemesi'ni kullanarak konum görselleştirebilirsiniz.

1. Gidin **Pano** sekmesi.

1. Cihaz Panoda seçin **harita** kitaplığından.

1. Haritayı bir başlık verin. Aşağıdaki örnek başlığa sahip **yükleme konumu**. Ardından, daha önce yapılandırılan konum özelliği seçin **özellikleri** sekmesi. Aşağıdaki örnekte, **yükleme adresi** seçilir.

   ![Form başlığı ve özelliklerine ilişkin ayrıntıları "Eşlemesi yapılandırmak"](./media/howto-set-up-template/locationcloudproperty5map.png)

4. **Kaydet**’i seçin. Harita kutucuğunu şimdi seçtiğiniz konum görüntülenir.

Harita, istenen boyuta yeniden boyutlandırabilirsiniz. Şimdi ne zaman bir işleç görünümleri Pano **Device Explorer**, tüm Pano kutucuklarının yapılandırdıysanız, bir konum eşlemesi gibi görülebilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central, uygulamanızdaki bir cihaz şablonu ayarlamak öğrendiniz, şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [Yeni bir cihaz şablon sürümü oluşturma](howto-version-devicetemplate.md)
> [bir MXChip IOT DevKit cihazını Azure IOT Central uygulamanızı bağlamak](howto-connect-devkit.md)
> [Azure genel istemci uygulamaya bağlama IOT Central uygulamasına (Node.js)](howto-connect-nodejs.md)