---
title: Azure IOT merkezi bir uygulama bir cihaz şablonunda ayarlama | Microsoft Docs
description: Cihaz şablonu ölçümleri, ayarlar, özellikler, kuralları ve bir Pano ayarlamanız öğrenin.
author: viv-liu
ms.author: viviali
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: f71d4c7dc94fedfd598ab87c51366ba9fb1f184a
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063044"
---
# <a name="set-up-a-device-template"></a>Cihaz şablon ayarlama

Microsoft Azure IOT merkezi bir uygulamaya bağlayan aygıt türü davranışlarını ve özelliklerini tanımlayan örnek bir aygıt şablonudur.

Örneğin, bir oluşturucu sahip bir IOT bağlı fan için bir aygıt şablon oluşturabilirsiniz:

- Sıcaklık telemetri ölçüm

- Fan motor hata olay ölçümü

- İşletim durumu ölçüm fanı

- Fan hızı ayarı

- Konum özelliği

- Uyarıları göndermek kuralları

- Cihaz genel bir görünümünü verir Panosu

Bu cihaz şablonundan bir işleç oluşturabilir ve gerçek fan aygıtları adlarıyla gibi bağlayın **fan 1** ve **fan 2**. Bu fanlar ölçümleri, ayarları, özellikler, kuralları ve uygulamanızın kullanıcılarının izlemek ve yönetmek bir Pano vardır.

> [!NOTE]
> Yalnızca oluşturucular ve yöneticiler oluşturabilir, düzenleme ve aygıt şablonları silin. Herhangi bir kullanıcı aygıtları üzerinde oluşturabilirsiniz **aygıt Explorer** var olan aygıt şablonları sayfasından.

## <a name="create-a-device-template"></a>Bir aygıt şablonu oluşturun

1. Git **uygulama Oluşturucu** sayfası.

2. Boş bir şablon oluşturmak için seçin **cihaz şablonu oluştur**ve ardından **özel**.

3. Yeni cihaz şablonunuz için bir ad girin ve **oluşturma**.

   ![Şablon adı olarak "buzdolabı" cihaz Ayrıntıları sayfası](./media/howto-set-up-template/devicedetailspage.png)

4. Bulunduğunuz artık **cihaz ayrıntıları** yeni bir sanal cihaz sayfası. Bir aygıt şablonu oluşturduğunuzda, bir sanal cihaz sizin için otomatik olarak oluşturulur. Veri raporlarını ve gerçek bir cihazı gibi yalnızca denetlenebilir.

Şimdi sekmelerin her birinde Ara **cihaz ayrıntıları** sayfası.

## <a name="measurements"></a>Ölçümler

Cihazınızı gelen verilerle ölçümlerdir. Cihazınızı özelliklerini eşleştirmek için cihaz şablonunuzu birden çok ölçümleri ekleyebilirsiniz.

- **Telemetri** ölçümlerdir Cihazınızı zaman içinde topladığı sayısal veri noktaları. Bunlar, sürekli bir akış olarak temsil. Sıcaklık örneğidir.
- **Olay** cihazda anlamlı bir şeyi temsil eden zaman içinde nokta verilerine ölçümlerdir. Önem düzeyi olay önemi temsil eder. Fan motor hata örneğidir.
- **Durum** ölçümler bir süre boyunca aygıt ya da bileşenlerinden durumunu temsil eder. Örneğin, fan modu sahip olarak tanımlanabilir **işletim** ve **durduruldu** iki olası durumlar olarak.

### <a name="create-a-telemetry-measurement"></a>Telemetri ölçüm oluştur
Yeni bir telemetri ölçüm eklemek için seçin **+ yeni ölçüm** düğmesi. Seçin **Telemetri** ölçüm olarak yazın ve ayrıntıları girin **oluşturma Telemetri** formu.

> [!NOTE]
> Gerçek bir cihazı bağlı olduğunda cihaz raporlarına ölçüm adı dikkat edin. Adı tam olarak eşleşmelidir **alan adı** bir ölçüm için girişi.

Örneğin, yeni sıcaklık telemetri ölçüm ekleyebilirsiniz:

!["Telemetri oluşturma" form sıcaklık ölçüm için ayrıntılarla](./media/howto-set-up-template/measurementsform.png)

Siz seçtikten sonra **kaydetmek**, **sıcaklık** ölçüm ölçümleri listede görünür. Bir işleç cihaz toplama sıcaklık veri görselleştirme görebilirsiniz.

![Ölçüm grafiği](./media/howto-set-up-template/measurementsgraph.png)

### <a name="create-an-event-measurement"></a>Bir olay ölçüm oluştur
Yeni bir olay ölçüm eklemek için seçin **+ yeni ölçüm** düğmesi. Seçin **olay** ölçüm olarak yazın ve ayrıntıları girin **oluşturma olay** formu.

Sağlamak **görünen adı**, **alan adı**, ve **önem** olayla ilgili ayrıntıları. Önem derecesi kullanılabilir üç düzeyi arasından seçim yapabilirsiniz: **hata**, **uyarı**, ve **bilgi**.  

Örneğin, yeni bir ekleyebilirsiniz **Fan Motor hata** olay.

![Fan motor olayla ilgili ayrıntıları "Olay oluşturma" formla](./media/howto-set-up-template/eventmeasurementsform.png)

Siz seçtikten sonra **kaydetmek**, **Fan Motor hata** ölçüm ölçümleri listede görünür. Bir işleç aygıt gönderme olay verileri görselleştirme görebilirsiniz.

![Olay ölçüm grafik](./media/howto-set-up-template/eventmeasurementschart.png)

Olay hakkında daha fazla ayrıntı görüntülemek için grafik olay simgesini seçin:

!["Fan Motor hata" olayla ilgili ayrıntıları](./media/howto-set-up-template/eventmeasurementsdetail.png)


### <a name="create-a-state-measurement"></a>Bir durum ölçüm oluştur
Yeni bir durum ölçüm eklemek için seçin **+ yeni ölçüm** düğmesi. Seçin **durumu** ölçüm olarak yazın ve ayrıntıları girin **oluşturma durumu** formu.

İlgili ayrıntıları sağlayın **görünen adı**, **alan adı**, ve **değerleri** durumu. Her değer ayrıca değeri grafikler ve tablolar görüntülendiğinde kullanılacak bir görünen ad olabilir.

Örneğin, yeni bir ekleyebilirsiniz **Fan modu** cihaz gönderebilir, iki olası değerlere sahip durumu **işletim** ve **durduruldu**.

![Fan modu için ayrıntılarla "Düzenle durumu" formu](./media/howto-set-up-template/statemeasurementsform.png)

Siz seçtikten sonra **kaydetmek**, **Fan modu** durumu ölçüm ölçümleri listede görünür. İşleç aygıt gönderme durumu verileri görselleştirme görebilirsiniz.

![Durum ölçüm grafik](./media/howto-set-up-template/statemeasurementschart.png)

Cihaz kısa bir süre içinde çok fazla veri noktası gönderirse, durumu ölçüm farklı bir görsel ile aşağıdaki ekran görüntüsünde gösterildiği gibi görünür. Grafikte tıklatırsanız, bu süre içinde tüm veri noktaları kronolojik olarak görüntülenir. Grafikte çizilen ölçüm görmek için zaman aralığı aşağı daraltabilirsiniz.

!["Statik Fan mod" durumu ölçüm için Ayrıntılar](./media/howto-set-up-template/statemeasurementsdetail.png)


## <a name="settings"></a>Ayarlar

Ayarları bir cihaz denetler. Bunlar cihaz girişleri sağlamak için uygulamanızın işleçleri etkinleştirin. Birden çok ayarları kutucukları olarak görünür, cihaz şablonunuza ekleyebileceğiniz **ayarları** kullanılacak işleçleri sekmesi. Altı tür ayarlarını ekleyebilirsiniz: sayı, metin, tarih, Değiştir, seçim listesi ve bölüm etiketi.

> [!NOTE]
> Gerçek bir cihazı bağlı olduğunda cihaz raporlarına ayarı adı dikkat edin. Adı tam olarak eşleşmelidir **alan adı** bir ayar için girişi.

Ayarları üç durumdan birinde olabilir. Cihaz bu durumu bildirir.

- **Eşitlenen**: Aygıt ayar değeri yansıtmak üzere değiştirilmiştir.

- **Bekleyen**: aygıt şu anda ayarı değerine değiştiriliyor.

- **Hata**: cihazı bir hata döndürdü.

Örneğin, yeni bir fan hızı ayarı ekleyebilirsiniz:

![Hızlı ayarları ayrıntılarını "Numarası Yapılandır" formla](./media/howto-set-up-template/settingsform.png)

Siz seçtikten sonra **kaydetmek**, **Fan hızı** ayarı bir kutucuk görünür ve cihaz fan hızını değiştirmek için kullanılmak üzere hazırdır.

Bir kutucuk oluşturduktan sonra yeni ayarınız çıkışı deneyebilirsiniz. İlk olarak, ekranın sağ üst kısmındaki Tasarım modunda kapalı geçin.

!["Ayarlar" sekmesini döşemeye "Tasarım modu" anahtarıyla](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Özellikler

Aygıt konumu ve seri numarası gibi aygıtla ilişkili cihaz meta verilerini özelliklerdir. Birden çok özellik kutucukları olarak görünür, cihaz şablonunuza ekleyebileceğiniz **özellikleri** sekmesi. Bir cihaz oluşturma ve herhangi bir zamanda bu değerleri düzenleyebilirsiniz bir işleç özellik değerlerini belirtebilirsiniz. Altı tür özellikleri ekleyebilirsiniz: sayı, metin, tarih, Değiştir, cihaz özelliği ve etiket.

Özellikler iki kategorisi vardır:

- **Aygıt** cihaz raporlarına özellikleri.
- **Uygulama** tamamen uygulamada depolanan özellikleri. Cihaz, uygulama özellikleri tanımıyor.

> [!NOTE]
> Gerçek bir cihazı bağlı olduğunda, cihaz özelliklerini cihaz raporlarına özelliğinin adını dikkat edin. Adı tam olarak eşleşmelidir **alan adı** özelliği için girişi. Ad aygıt şablonun benzersiz olduğu sürece uygulama özellikleri için alan adı, istediğiniz herhangi bir şey olabilir.

Örneğin, yeni bir özellik olarak cihaz konumu ekleyebilirsiniz:

![Formunda "Özellikler" sekmesinde "Metin Yapılandır"](./media/howto-set-up-template/propertiesform.png)

Siz seçtikten sonra **kaydetmek**, aygıt konumu bir kutucuk görüntülenir:

![Konum döşeme](./media/howto-set-up-template/propertiestile.png)

Bir kutucuk oluşturduktan sonra özellik değeri değiştirebilirsiniz. İlk olarak, ekranın sağ üst bölümünde Tasarım modunda kapalı geçin.

### <a name="create-a-location-property-through-azure-maps"></a>Konum Özelliği Azure eşlemeleri aracılığıyla oluşturma
Azure IOT merkezi konum verilerinize coğrafi bağlam verin ve herhangi bir adres enlem ve boylam koordinatları eşleyin. Veya yalnızca eşleme enlem ve boylam koordinatları olabilir. Azure eşlemeleri bu özelliği IOT Orta de sağlar.

İki tür yerleşim özellikleri ekleyebilirsiniz:
- **Konumu olarak uygulama özelliği**, tamamen uygulamada depolanan. Cihaz, uygulama özellikleri tanımıyor.
- **Konumu olarak bir cihaz özelliği**, hangi cihaz bildirir.

#### <a name="add-location-as-an-application-property"></a>Bir uygulama özelliği olarak konumu Ekle 
IOT Orta uygulamanızda Azure eşlemeleri kullanarak bir uygulama özelliği olarak konum özelliği oluşturabilirsiniz. Örneğin, cihaz yükleme adresi ekleyebilirsiniz. 

1. Üzerinde **özellikleri** sekmesinde, emin **Tasarım modunda** olan **üzerinde**.

   !["Özellikler" sekmesinde tasarım modu açık](./media/howto-set-up-template/locationcloudproperty1.png)

2. Kitaplık'ta seçin **konumu**.
3. Yapılandırma **görünen adı**, **alan adı**ve (isteğe bağlı) **ilk değer** konumu. 

   !["Konum Yapılandır" form konumu için ayrıntılarla](./media/howto-set-up-template/locationcloudproperty2.png)

   Bir konum eklemek için iki desteklenen biçimler şunlardır:
   - **Adresi olarak konumu**
   - **Konumu koordinatları olarak** 

4. **Kaydet**’i seçin. 

   ![Eklenen yükleme adresiyle konumu özelliği](./media/howto-set-up-template/locationcloudproperty3.png)

Şimdi bir işleç konumu alanın formda konum değeri güncelleştirebilirsiniz. 

#### <a name="add-location-as-a-device-property"></a>Konumu bir cihaz özelliği Ekle 

Konum özelliğini cihaz raporlarına bir cihaz özelliği oluşturabilirsiniz. Örneğin, aygıt konumu izlemek istiyorsanız:

1. Üzerinde **özellikleri** sekmesinde, emin **Tasarım modunda** olan **üzerinde**.

   !["Özellikler" sekmesinde tasarım modu açık](./media/howto-set-up-template/locationdeviceproperty1.png)

2. Seçin **cihaz özelliği** kitaplığından.
3. Alan adı ve görünen ad yapılandırın ve seçin **konumu** veri türü olarak. 

   > [!NOTE]
   > Alan adı, cihaz raporlarına özelliğinin adı tam olarak eşleşmelidir. 

   !["Aygıt özelliklerini yapılandırma" form konumu için ayrıntılarla](./media/howto-set-up-template/locationdeviceproperty2.png)

Konum özelliği yapılandırdıysanız, yapabilecekleriniz [cihaz Pano konumda görselleştirmek için bir harita Ekle](#add-an-azure-maps-location-in-the-dashboard).

## <a name="commands"></a>Komutlar

Komutlar bir cihazı uzaktan yönetmek için kullanılır. Bunlar hemen cihazda komutları çalıştırmak için uygulamanızın işleçleri etkinleştirin. Birden çok komut kutucukları olarak görünür, cihaz şablonunuza ekleyebileceğiniz **komutları** kullanılacak işleçleri sekmesi. Cihaz Oluşturucu olarak komutları, gereksinimlerinize göre tanımlamak için esnekliğe sahip olursunuz.

Bir komut bir ayarından farklı mı? 

* **Ayarlama**: siz değiştirene kadar bu yapılandırma kalıcı hale getirmek için cihaz istediğiniz ve bir aygıta uygulamak istediğiniz bir yapılandırma bir ayardır. Örneğin, sizin Dondurucu sıcaklığını ayarlamak istediğiniz ve bile Dondurucu başlatıldığında bu ayarı istiyor. 

* **Komut**: anında bir komut cihazda uzaktan IOT merkezi çalıştırmak için komutları kullanın. Bir cihazı bağlı değilse, komut zaman aşımına uğradı ve başarısız olur. Örneğin, bir aygıtı yeniden başlatmak istiyor.  

Bir komutu çalıştırdığınızda, cihaz komut olup alınan bağlı olarak üç durumdan birinde olabilir. 

Örneğin, yeni bir ekleyebilirsiniz **Echo** komutu:

![Echo ayrıntılarını "Komutu Yapılandır" formla](./media/howto-set-up-template/commandsecho.png)

Siz seçtikten sonra **kaydetmek**, **Echo** komutu bir kutucuk görünür ve cihaz echo için kullanılmak üzere hazırdır.

Bir kutucuk oluşturduktan sonra yeni komutunuzu deneyebilirsiniz.

## <a name="rules"></a>Kurallar

Kuralları cihazları yakın gerçek zamanlı izleme işleçleri etkinleştirin. Kuralları otomatik olarak kuralı tetiklendiğinde bir e-posta gönderme gibi eylemleri çağırır. Bir kural türü bugün kullanılabilir:

- **Telemetri kural**, seçili cihaz telemetrisi belirtilen eşiğin kestiği zaman tetiklendi. [Telemetri kuralları hakkında daha fazla bilgi](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Pano

Pano, bir aygıt hakkındaki bilgileri görmek için bir işleç gidebilecekleri ' dir. Oluşturucu olarak, cihazın nasıl davranmakta anlamak işleçleri yardımcı olması için bu sayfadaki döşeme ekleyebilirsiniz. Birden çok Pano kutucukları aygıtını şablonunuzu ekleyebilirsiniz. Pano kutucukları altı tür ekleyebilirsiniz: görüntü, çizgi grafiği, çubuk grafiği, KPI, ayarları ve özellikleri ve etiket.

Örneğin, ekleyebileceğiniz bir **ayarları ve özellikleri** döşeme ayarları ve özellikleri geçerli değerlerini seçimi göstermek için:

![Ayrıntılar için ayarları ve özellikleri ile "Cihaz ayrıntıları Yapılandır" form](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Şimdi bir işleç Pano görüntülediğinde, cihaz ayarları ve özellikleri görüntüler bu kutucuğu görebilirsiniz:

![Görüntülenen ayarlar ve Özellikler bölme için "Pano" sekmesi](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-an-azure-maps-location-in-the-dashboard"></a>Panoda bir Azure eşlemeleri konumu Ekle

Bir konum özelliği daha önce içinde yapılandırdıysanız [bir konum özelliği üzerinden Azure eşlemeleri oluşturma](#create-a-location-property-through-azure-maps), cihaz Panonuzda bir harita kullanarak konumu görselleştirebilirsiniz.

1. Üzerinde **Pano** sekmesinde, emin **Tasarım modunda** olan **üzerinde**.

   ![Tasarım modu açık olan "Pano" sekmesi](./media/howto-set-up-template/locationcloudproperty4map.png)

2. Cihaz Panoda seçin **harita** kitaplığından. 
3. Bir başlık verin ve cihaz özelliklerinizi bir parçası olarak önceden yapılandırılmış konum özelliği seçin.

   ![Ayrıntılar için başlık ve özellikleri ile formunu "Harita yapılandırma"](./media/howto-set-up-template/locationcloudproperty5map.png)

4. **Kaydet**’i seçin. Harita kutucuğu artık seçtiğiniz konum görüntüler. 

   ![Seçilen konum eşleme döşeme](./media/howto-set-up-template/locationcloudproperty6map.png) 

Harita, istenen boyuta yeniden boyutlandırabilirsiniz.

Artık bir işleç Pano görüntülediğinde, bir konum eşleme dahil olmak üzere, yapılandırdığınız tüm Pano kutucukları görebilirsiniz.

![Panoda döşeme](./media/howto-set-up-template/locationcloudproperty7map.png) 

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanızı aygıt şablonunda kurma öğrendiğinize göre şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [Yeni bir cihaz şablonu sürüm oluşturma](howto-version-devicetemplate.md)
