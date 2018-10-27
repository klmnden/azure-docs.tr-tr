---
title: Bir Azure IOT Central uygulamasına cihaz şablonunda ayarlama | Microsoft Docs
description: Ölçümler, ayarları, özellikleri, kuralları ve bir pano ile bir cihaz şablonu ayarlama konusunda bilgi edinin.
author: viv-liu
ms.author: viviali
ms.date: 10/26/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 61bc9da45ac420e5683be1ea3ad253eae9c0ba5a
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158950"
---
# <a name="set-up-a-device-template"></a>Cihaz şablonu ayarlama

Cihaz özellikleri ve davranışları için bir Azure IOT Central uygulamasına bağlanan cihaz türü tanımlayan bir şema şablonudur.

Örneğin, bir oluşturucusu olan bir IOT bağlı fanı için bir cihaz şablon oluşturabilirsiniz:

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

1. Git **uygulama Oluşturucusu** sayfası.

2. Boş bir şablon oluşturmak için Seç **cihaz şablonu oluşturma**ve ardından **özel**.

3. Yeni cihaz şablonunuz için bir ad (örneğin buzdolabı-1) girin ve seçin **Oluştur**.

   ![Şablon adı olarak "buzdolabı" cihaz Ayrıntıları sayfası](./media/howto-set-up-template/devicedetailspage.png)

4. Bulunduğunuz artık **cihaz ayrıntıları** sayfasında yeni bir simülasyon cihazı. Bir cihaz şablonu oluşturduğunuzda bir simülasyon cihazı sizin için otomatik olarak oluşturulur. Bu veri raporlarını ve yalnızca gerçek bir cihaz gibi denetlenebilir.

Artık her sekme bakalım **cihaz ayrıntıları** sayfası.

## <a name="measurements"></a>Ölçümler

Ölçümler, bir CİHAZDAN geliyorsa verilerdir. Birden çok ölçümleri cihazınızın yeteneği karşılamak için cihaz şablonunuza ekleyebilirsiniz.

- **Telemetri** Cihazınızı zaman içinde topladığı sayısal veri noktaları ölçümlerdir. Bunlar, sürekli bir akış temsil. Sıcaklık buna bir örnektir.
- **Olay** ölçümlerdir bir cihazda anlamın temsil eden zaman içinde nokta veri. Önem derecesi, bir olayın önem temsil eder. Fan motor hata buna bir örnektir.
- **Durum** ölçümleri bir süre cihaz veya bileşenlerinin durumunu temsil eder. Örneğin, bir fanı modu sahip olacak şekilde tanımlanabilir **işletim** ve **durduruldu** iki olası durum olarak.

### <a name="create-a-telemetry-measurement"></a>Telemetri ölçü oluşturma
Yeni bir telemetri ölçüm eklemek için seçin **şablonu Düzen**ve ardından **+ yeni ölçüm** düğmesi. Seçin **Telemetri** ölçüm yazın ve ayrıntılarını girin **oluşturma Telemetri** formu.

> [!NOTE]
> Cihaz şablonu alan adlarını, karşılık gelen cihaz kodu sırada gerçek bir cihaz bağlandığında uygulamada görüntülenecek telemetri ölçüm için özellik adları eşleşmelidir. Aşağıdaki bölümlerde cihaz şablonlarını tanımlamak devam ederken ayarları, cihaz özellikleri ve komutları yapılandırırken de aynısını yapın.

Örneğin, yeni bir sıcaklık telemetri ölçüm ekleyebilirsiniz:
| Görünen Ad        | Alan Adı    |  Birimler    | Min   |Maks|
| --------------------| ------------- |-----------|-------|---|
| Sıcaklık         | Temp          |  degC     |  0    |100|

!["Telemetri oluşturma" form sıcaklık ölçüm için Ayrıntılar](./media/howto-set-up-template/measurementsform.png)

Seçtikten sonra **Bitti**, **sıcaklık** ölçüm ölçümleri listesinde görünür. Kısa bir süre içinde bir sanal cihaz oluşturulan sıcaklık verilerini görselleştirme görebilirsiniz. Bir cihaz şablonu oluşturduğunuzda, bir sanal cihaz uygulamanızın davranışını fiziksel/gerçek cihaz bağlı önce test olanak tanıyan şablonu oluşturulur.


> [!NOTE]
  Veri telemetri ölçü bir kayan türüdür nokta sayısı.

### <a name="create-an-event-measurement"></a>Bir olay ölçü oluşturma
Yeni bir olay ölçüm eklemek için seçin **şablonu Düzen**ve ardından **+ yeni ölçüm** düğmesi. Seçin **olay** ölçüm yazın ve ayrıntılarını girin **olay oluşturma** formu.

Sağlamak **görünen ad**, **alan adı**, ve **önem derecesi** olayla ilgili ayrıntıları. Önem derecesi kullanılabilir üç düzeyi arasından seçim yapabilirsiniz: **hata**, **uyarı**, ve **bilgi**.  

Örneğin, yeni bir ekleyebilirsiniz **Fan Motor hata** olay.

| Görünen Ad        | Alan Adı    |  Varsayılan önem derecesi | 
| --------------------| ------------- |-----------|
| Fan Motoru Hatası     | fanmotorerror |  Hata    | 

!["Olay oluşturma" form fan motor olayla ilgili ayrıntıları](./media/howto-set-up-template/eventmeasurementsform.png)

Seçtikten sonra **Bitti**, **Fan Motor hata** ölçüm ölçümleri listesinde görünür. Operatörün cihaz gönderen olay verileri görselleştirmesini görebilirsiniz.

![Olay ölçüm grafiği](./media/howto-set-up-template/eventmeasurementschart.png)

Olay hakkında daha fazla ayrıntı görüntülemek için grafikteki olay simgesini seçin.

!["Fan Motor Error" olayla ilgili ayrıntıları](./media/howto-set-up-template/eventmeasurementsdetail.png)

> [!NOTE]
  Veri türü olay ölçümü dizedir.

### <a name="create-a-state-measurement"></a>Bir durum ölçü oluşturma
Yeni bir durum ölçüm eklemek için seçin **şablonu Düzen**ve ardından **+ yeni ölçüm** düğmesi. Seçin **durumu** ölçüm yazın ve ayrıntılarını girin **oluşturma durumu** formu.

İçin ayrıntıları sağlayın **görünen ad**, **alan adı**, ve **değerleri** durumu. Her değer, değer grafikleri ve tabloları göründüğünde kullanılacak bir görünen ad da sahip olabilirsiniz.

Örneğin, yeni bir ekleyebilirsiniz **fanı modu** cihaza gönderebilir, iki olası değerleri içeren durum **işletim** ve **durduruldu**.


| Görünen Ad | Alan Adı    |  Değer 1   | Görünen Ad | Değer 2    |Görünen Ad  | 
| -------------| ------------- |----------- | -------------| -----------| -------------|
| Fan Modu     | fanmode       |  1         | İşletim    |     0      | Durduruldu      |

![Fan modu için Ayrıntıları "Düzenleme durumu" formu](./media/howto-set-up-template/statemeasurementsform.png)

Seçtikten sonra **Bitti**, **fanı modu** durumu ölçüm ölçümleri listesinde görünür. İşleci, cihazın gönderdiği durumu verilerini görselleştirme görebilirsiniz.

![Durum ölçüm grafiği](./media/howto-set-up-template/statemeasurementschart.png)

Cihaz küçük bir süre içinde çok fazla veri noktasının gönderirse, durumu ölçümü ile farklı bir görsel, aşağıdaki ekran görüntüsünde gösterildiği gibi görünür. Grafikte tıklarsanız, bu süre içinde tüm veri noktalarına kronolojik sırada görüntülenir. Grafiği çizilen ölçüm görmek için zaman aralığını da daraltabilirsiniz.

> [!NOTE]
  Veri türü durumu ölçümü dizedir.

## <a name="settings"></a>Ayarlar

Ayarları bir cihaz denetler. Uygulamanızın cihaza girişleri operatörleri tanırlar. Birden çok ayarları kutucukları olarak görünür, cihaz şablonunuza ekleyebilirsiniz **ayarları** işleçleri kullanmak için sekmesinde. Birçok türde ayarlar ekleyebilirsiniz: sayı, metin, tarih, Değiştir, seçim listesini ve bölüm etiketi. 

Ayarları üç durumdan birinde olabilir. Cihaz bu durumu bildirir.

- **Eşitlenen**: cihaz ayarı değerini yansıtmak üzere değiştirildi.

- **Bekleyen**: cihaz ayarı değeri şu anda değişiyor.

- **Hata**: cihazı bir hata döndürdü.

Seçerek yeni bir fanı hızı ayarı gibi ekleyebilirsiniz **şablonu Düzen** ve yeni girerek **numarası** ayarı:

| Görünen Ad  | Alan Adı    |  Birimler  | Ondalık sayı |İlk|
| --------------| ------------- |---------| ---------|---- |
| Fan hızı     | fanSpeed      | RPM     | 2        | 0   |

![Hızlı ayarları için Ayrıntılar "Sayı Yapılandır" formu](./media/howto-set-up-template/settingsform.png)

Seçtikten sonra **Kaydet**, **fanı hızı** ayarı bir kutucuk görünür ve cihazın fanı hızını değiştirmek için kullanılmak üzere hazırdır.

Bir kutucuğu oluşturduktan sonra seçin **Bitti** ekranın sağ üst kısmındaki. Gerçek cihaz bir uygulamaya bağlandıktan sonra ayar değeri eşitlenen değişir.

!["Ayarlar" sekmesini döşeme için "Tasarım modu" anahtarı ile](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Özellikler

Cihaz konumu ve seri numarası gibi cihaz ile ilişkili cihaz meta verilerini özelliklerdir. Birden çok özellik kutucukları olarak görünür, cihaz şablonunuza ekleyebilirsiniz **özellikleri** sekmesi. Çeşitli özellikler ekleyebilirsiniz: sayı, metin, tarih, Değiştir, cihaz özelliği, etiket ve konum. Bunlar bir cihaz oluşturur ve herhangi bir zamanda bu değerleri düzenleyebilirsiniz operatörün özellikleri için değer belirtebilirsiniz. Ancak, cihaz özellikleri salt okunurdur ve CİHAZDAN uygulamaya gönderilir ve işleç tarafından değiştirilemez. Gerçek cihaz bağlandığında, cihaz özelliği kutucuğuna uygulamada güncelleştirilecektir. 

İki özellik kategorisi vardır:

- **Cihaz** cihaz IOT Central uygulamasına raporlarına özellikleri. Bunlar cihaz tarafından bildirilen salt okunur değer ve gerçek cihaz bağlıyken uygulama içinde güncelleştirilir. 
- **Uygulama** yalnızca uygulama içinde depolanır ve operatör tarafından düzenlenebilir özellikleri. Cihaz, uygulama özellikleri tanımaz.

Örneğin, cihaz konumu yeni bir ekleyebilirsiniz **metin** seçerek özelliği (bir uygulama) **şablonu Düzen** girerek yeni özelliği:

| Görünen Ad  | Alan Adı | Öndeki boşlukları Kırp  | Sondaki boşlukları Kırp  | Büyük/küçük harfe duyarlılık| Min. Uzunluk | En Büyük Uzunluk |
| --------------| -----------|---------| ---------|---- |----|----|
| Konum      | LOC        | Kapalı     |  Kapalı     | Karma  | 0 | 100|

!["Metin Yapılandır" form "Özellikler" sekmesinde](./media/howto-set-up-template/propertiesform.png)

Seçtikten sonra **Kaydet**, cihaz konumu bir kutucuk olarak görünür:

![Konum kutucuğu](./media/howto-set-up-template/propertiestile.png)

Kutucuk oluşturduktan sonra uygulama özellik değeri değiştirebilirsiniz. İlk olarak, seçin **Bitti** ekranın sağ üst kısmındadır.

### <a name="create-a-location-property-through-azure-maps"></a>Bir konum özelliği üzerinden Azure haritalar'ı oluşturma
Konum verilerinizi Azure IOT Central, coğrafi bağlam vermek ve herhangi bir enlem ve boylam koordinatlarını sokak adresi eşleyin. Veya yalnızca harita enlem ve boylam koordinatları olabilir. Azure haritalar IOT Central bu özelliği sağlar.

İki tür yerleşim özellikleri ekleyebilirsiniz:
- **Konumu olarak bir uygulama özelliği**, yalnızca uygulama içinde depolanır. Cihaz, uygulama özellikleri tanımaz.
- **Konumu olarak bir cihaz özelliği**, hangi cihaz uygulamaya bildirir.

#### <a name="add-location-as-an-application-property"></a>Konumu bir uygulama özelliği Ekle 
IOT Central uygulamanızda Azure haritalar'ı kullanarak bir uygulama özelliği olarak location özelliği oluşturabilirsiniz. Örneğin, cihaz yükleme adresi ekleyebilirsiniz. 

1. Üzerinde **özellikleri** sekmesinde **şablonu Düzen**.

   !["Özellikler" sekmesinde tasarım modu açık](./media/howto-set-up-template/locationcloudproperty1.png)

2. Kitaplıkta, seçin **konumu**.
3. Yapılandırma **görünen ad**, **alan adı**ve (isteğe bağlı olarak) **başlangıç değeri** konumu için. 

    | Görünen Ad  | Alan Adı | İlk Değer |
    | --------------| -----------|---------| 
    | Yükleme adresi | insta_address | Microsoft, 1 Microsoft Way, Redmond, WA 98052   |

   ![Konum ayrıntılarını "Konumu Yapılandır" formu](./media/howto-set-up-template/locationcloudproperty2.png)

   Bir konum eklemek için desteklenen iki biçim vardır:
   - **Adresi olarak konumu**
   - **Koordinatları olarak konumu** 

4. Seçin **Kaydet** ve **Bitti**. Artık bir işleç konum alanı formda konum değeri güncelleştirebilirsiniz. 

#### <a name="add-location-as-a-device-property"></a>Konumu bir cihaz özelliği Ekle 

Location özelliğini cihaz raporlarına bir cihaz özelliği oluşturabilirsiniz. Örneğin, cihaz konumu izlemek istiyorsanız:

1. Üzerinde **özellikleri** sekmesinde **şablonu Düzen**.

   !["Özellikler" sekmesinde tasarım modu açık](./media/howto-set-up-template/locationdeviceproperty1.png)

2. Seçin **cihaz özelliği** kitaplığından.
3. Alan adı ve görünen adı'nı yapılandırın ve seçin **konumu** veri türü olarak. 

    | Görünen Ad  | Alan Adı | Veri Türü |
    | --------------| -----------|-----------| 
    | Cihaz konumu | deviceLoc| location  |

   > [!NOTE]
   > İlgili cihaz kod özellik adları alan adları eşleşmelidir

   !["Cihaz özelliklerini yapılandırma" form ile konum için ayrıntıları](./media/howto-set-up-template/locationdeviceproperty2.png)

Gerçek cihaz bağlandıktan sonra bir cihaz özelliği eklediğiniz konumu, cihaz tarafından gönderilen değer ile güncelleştirilecektir. Yükleme, bir uygulama özelliği eklediğiniz konumu düzenlenebilir bir kutucuk adresidir. Location özelliği yapılandırdığınıza göre şunları yapabilirsiniz [cihaz pano konumu görselleştirmek için bir harita Ekle](#add-an-azure-maps-location-in-the-dashboard).

## <a name="commands"></a>Komutlar

Komutlar, bir cihazı uzaktan yönetmek için kullanılır. Bunlar, işleçler, uygulamanızın anında cihazda komutlarını çalıştırmak için etkinleştirin. Birden çok komut kutucukları olarak görünür, cihaz şablonunuza ekleyebilirsiniz **komutları** işleçleri kullanmak için sekmesinde. Cihaz Oluşturucu olarak ihtiyaçlarınıza göre komutlar tanımlar esnekliğine sahip olursunuz.

Bir komutu bir ayardan farklı mı? 

* **Ayar**: bir cihaz için uygulamak istediğiniz bir yapılandırma bir ayardır ve siz değiştirene kadar bu yapılandırma kalıcı hale getirmek için cihaz istiyor. Örneğin, Dondurucu Sıcaklığın ayarlamak istediğiniz ve Dondurucu bile yeniden başlatıldığında, ayarlamak istediğiniz. 

* **Komut**: anında bir komutu cihazda uzaktan IOT Central ' çalıştırma için komutları kullanın. Bir cihazı bağlı değilse, komut zaman aşımına uğrar ve başarısız olur. Örneğin, bir cihazı yeniden başlatmak istiyor.  


Örneğin, yeni bir ekleyebilirsiniz **Yankı** komutunu seçerek **şablon düzenleme**, ardından **+ yeni komut**ve yeni komutu girerek:

| Görünen Ad  | Alan Adı | Varsayılan Zaman Aşımı | Veri Türü |
| --------------| -----------|---------------- | --------- | 
| Yankı Komutu  | echo       |  30             | metin      |

![Yankı ayrıntılarını "Komutunu yapılandırma" formu](./media/howto-set-up-template/commandsecho.png)

Seçtikten sonra **Kaydet** ve **Bitti**, **Yankı** komut bir kutucuk görünür ve gerçek Cihazınızı bağlandıktan sonra cihaz echo için kullanılmak üzere hazırdır. Alan adları, komutun başarıyla çalıştırılacak komutları sırayla ilgili cihaz kod özellik adları eşleşmelidir.


## <a name="rules"></a>Kurallar

Kuralları cihazları neredeyse gerçek zamanlı izleme olanak tanır. Kuralları otomatik olarak kuralı tetiklendiğinde e-posta gönderme gibi eylemleri çağırır. Bir kural türü bugün kullanılabilir:

- **Telemetri kuralı**, seçili cihaz telemetrisi belirtilen eşiği aştığında olduğunda tetiklenir. [Telemetri kuralları hakkında daha fazla bilgi](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Pano

Bir cihaz hakkında bilgi için bir işleç gidebilecekleri panodur. Bir oluşturucu işleçleri cihazın nasıl davrandığını anlamanıza yardımcı olması için bu sayfada kutucuklar ekleyebilirsiniz. Birden çok Pano kutucukları, aygıt şablonunuza ekleyebilirsiniz. Pano kutucukları görüntü, çizgi grafik, çubuk grafik, ana performans göstergesi (KPI), ayarları ve özellikleri gibi birçok türleri ekleyin ve etiketi.

Örneğin, ekleyebilirsiniz bir **ayarları ve özellikleri** seçerek seçim ayarları ve özellikleri geçerli değerlerini görüntülemek için kutucuk **Şablonu Düzenle** ve kitaplıktan bir kutucuğa:

!["Cihaz ayrıntıları Yapılandır" form ayarlarına ve özelliklerine ilişkin ayrıntılı](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Artık bir işleç panoyu görüntülediğinde, cihaz ayarlarını ve özelliklerini görüntüler. Bu kutucuk görebilirsiniz:

![Görüntülenen ayarları ve özellikleri için kutucuğun bulunduğu "Pano" sekmesi](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-an-azure-maps-location-in-the-dashboard"></a>Azure haritalar konum Pano Ekle

Bir konum özelliği daha önce söz yapılandırdıysanız [bir konum özelliği üzerinden Azure haritalar'ı oluşturma](#create-a-location-property-through-azure-maps), cihaz Panonuzda bir eşlemesi'ni kullanarak konum görselleştirebilirsiniz.

1. Üzerinde **Pano** sekmesinde **şablonu Düzen**.

   ![Tasarım modu açık olan "Pano" sekmesi](./media/howto-set-up-template/locationcloudproperty4map.png)

2. Cihaz Panoda seçin **harita** kitaplığından. 
3. Bir başlık verebiliyorum. Aşağıdaki örnekte başlığa yükleme konumuna sahip, Özellikler sekmesi altında daha önce yapılandırdığınız location özelliği seçin. Aşağıdaki örnekte **yükleme adresi** seçilir.

   ![Form başlığı ve özelliklerine ilişkin ayrıntıları "Eşlemesi yapılandırmak"](./media/howto-set-up-template/locationcloudproperty5map.png)

4. **Kaydet**’i seçin. Harita kutucuğunu şimdi seçtiğiniz konum görüntülenir. 

   ![Seçilen konum eşlemesi kutucuğu](./media/howto-set-up-template/locationcloudproperty6map.png) 

Harita, istenen boyuta yeniden boyutlandırabilirsiniz. Artık bir işleç panoyu görüntülediğinde, bir konum eşlemesi dahil olmak üzere, yapılandırdığınız tüm Pano kutucuklarının görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central, uygulamanızdaki bir cihaz şablonu ayarlamak öğrendiniz, şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [Yeni bir cihaz şablon sürümü oluşturma](howto-version-devicetemplate.md)
