---
title: Azure IOT merkezi bir uygulama bir cihaz şablonunda ayarlama | Microsoft Docs
description: Bir aygıt şablonu ölçümleri, ayarlar, özellikler, kuralları ve Pano ayarlamanız öğrenin.
author: viv-liu
ms.author: viviali
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: bda056a75ae9d696dab389b85fe1bfb2935ee1a8
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261993"
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

- Bir tüm cihaz hakkında görünümü oluşturan verir Panosu

Bu cihaz şablonundan bir işleç oluşturabilir ve gerçek fan aygıtları adlarıyla gibi bağlayın **fan 1** ve **fan 2**. Bu fanlar ölçümleri, ayarları ve özellikleri, kuralları ve uygulamanızın kullanıcılarının izlemek ve yönetmek bir Pano vardır.

> [!NOTE]
Yalnızca oluşturucular ve yöneticiler oluşturabilir, düzenleme ve aygıt şablonları silin. Herhangi bir kullanıcı cihazları oluşturabilirsiniz **aygıt Explorer** var olan aygıt şablonları sayfasından.

## <a name="create-a-new-device-template"></a>Yeni cihaz şablonu oluşturma

1. Gidin **uygulama Oluşturucu** sayfası.

1. Boş bir şablon oluşturmak için seçtiğiniz **cihaz şablonu oluştur**ve ardından **özel**.

1. Yeni cihaz şablonunuz için bir ad girin ve seçin **oluşturma**.

    ![Cihaz Ayrıntıları sayfası](./media/howto-set-up-template/devicedetailspage.png)

1. Bulunduğunuz artık **cihaz ayrıntıları** yeni bir sanal cihaz sayfası. Yeni bir cihaz şablonu oluşturduğunuzda, bir sanal cihaz sizin için otomatik olarak oluşturulur. Veri raporlarını ve gerçek bir cihazı gibi yalnızca denetlenebilir.

Şimdi sekmelerin her birinde Ara **cihaz ayrıntıları** sayfası.

## <a name="measurements"></a>Ölçümler

Cihazınızı gelen veriler ölçümlerdir. Cihazınızı özelliklerini eşleştirmek için cihaz şablonunuzu birden çok ölçümleri ekleyebilirsiniz. Şu anda, telemetri ve olay desteklenen ölçümleri türleridir.

- **Telemetri** ölçümlerdir Cihazınızı zaman içinde topladığı ve olan sayısal veri noktalarını temsil sürekli akışı. Örneğin, sıcaklık.
- **Olay** ölçümlerdir cihazda anlamlı bir şeyi temsil zaman içinde nokta veri. Olayları olay önemi temsil eder, kendileriyle ilişkili önem derecesi vardır. Örneğin, motor hata fanı
- **Durum** ölçümler bir süre boyunca aygıt ya da bileşenlerinden durumunu temsil eder. Örneğin, işletme sahibi olacak şekilde tanımlanmasına ve iki olası durumlar durduruldu modu fan.

### <a name="create-a-telemetry-measurement"></a>Telemetri ölçüm oluştur
Yeni bir telemetri ölçüm eklemek için tıklayın **+ yeni ölçüm** ölçü türünü seçin seçenekleriyle formu açan düğmesi. Seçin **Telemetri** ve ayrıntıları girin **oluşturma Telemetri** formu.

> [!NOTE]
> Gerçek bir aygıt ölçüm adına bağlı olarak ödeme dikkat olduğunda aygıtı bildirir. Adı tam olarak eşleşmelidir **alan adı** bir ölçü.

Örneğin, yeni sıcaklık telemetri ölçüm ekleyebilirsiniz:

![Ölçümleri formu](./media/howto-set-up-template/measurementsform.png)

Seçtiğiniz sonra **kaydetmek**, **sıcaklık** ölçüm ölçümleri listede görünür ve bir işleç aygıt toplama sıcaklık veri görselleştirme görebilirsiniz.

![Ölçümleri grafiği](./media/howto-set-up-template/measurementsgraph.png)

### <a name="create-an-event-measurement"></a>Bir olay ölçüm oluştur
Yeni bir olay ölçüm eklemek için tıklayın **+ yeni ölçüm** ölçü türünü seçin seçenekleriyle formu açan düğmesi. Seçin **olay** ve ayrıntıları girin **oluşturma olay** formu.

Bu form üzerindeki sağlamak **görünen adı**, **alan adı**ve **önem** olay. Önem derecesi - kullanılabilir üç düzeyi arasından seçim yapabilirsiniz **hata**, **uyarı**, ve **bilgi**.  

Örneğin, yeni bir 'Fan Motor hata' olay ekleyebilirsiniz.

![Olay ölçümleri formu](./media/howto-set-up-template/eventmeasurementsform.png)

Seçtiğiniz sonra **kaydetmek**, **Fan Motor hata** ölçüm ölçümleri listede görünür ve bir işleç cihaz gönderiyor olay verileri görselleştirme görebilirsiniz.

![Olay ölçümleri grafik](./media/howto-set-up-template/eventmeasurementschart.png)

Olay hakkında ek ayrıntıları görüntülemek için grafik olay simgesini tıklatın:

![Olay ölçümleri ayrıntıları](./media/howto-set-up-template/eventmeasurementsdetail.png)


### <a name="create-a-state-measurement"></a>Bir durum ölçüm oluştur
Yeni bir durum ölçüm eklemek için tıklayın **+ yeni ölçüm** ölçü türünü seçin seçenekleriyle formu açan düğmesi. Seçin **durumu** ve ayrıntıları girin **oluşturma durumu** formu.

Bu form üzerindeki sağlamak **görünen adı**, **alan adı**ve olası **değerleri** durumu. Her **değeri** değeri grafikler ve tablolar gösterilirken kullanılan bir görünen ad de sahip olabilir.

Örneğin, aygıt gönderebilir iki olası değerler olan yeni bir 'Fan modu' durum ekleyebilirsiniz **işletim** ve **durduruldu**.

![Durum ölçümleri formu](./media/howto-set-up-template/statemeasurementsform.png)

Seçtiğiniz sonra **kaydetmek**, **Fan modu** durumu ölçüm ölçümleri listede görünür ve işleci cihaz gönderiyor durumu verileri görselleştirme görebilirsiniz.

![Durum ölçümleri grafiği](./media/howto-set-up-template/statemeasurementschart.png)

Kısa bir süre içinde cihazdan çok fazla veri noktası gönderildiği olursa, durum ölçümü aşağıda gösterildiği gibi farklı bir görselle görüntülenir. Grafiğe tıklarsanız, söz konusu süre içindeki tüm veri noktaları kronolojik sırada görüntülenir. Grafikte çizilen ölçüm görmek için zaman aralığı aşağı daraltabilirsiniz.

![Durum ölçümleri ayrıntısı](./media/howto-set-up-template/statemeasurementsdetail.png)


## <a name="settings"></a>Ayarlar

Ayarları bir cihaz denetler. Bunlar cihaz girişleri sağlamak için uygulamanızın işleçleri etkinleştirin. Birden çok ayarları kutucukları olarak görünür, cihaz şablonunuza ekleyebileceğiniz **ayarları** kullanılacak işleçleri sekmesi. Ekleyebileceğiniz ayarları altı tür vardır: sayı, metin, tarih, Değiştir, seçim listesi ve bölüm etiketi.

> [!NOTE]
> Gerçek bir cihazı bağlı olduğunda ayarı adı dikkat cihaz raporları. Adı tam olarak eşleşmelidir **alan adı** bir ayarı.

Ayarları üç durumdan birinde olabilir. Bu durumu aygıt tarafından raporlanır.

- **Eşitlenen**: Aygıt ayar değeri yansıtmak üzere değiştirilmiştir.

- **Bekleyen**: aygıt şu anda ayarı değerine değiştiriliyor.

- **Hata**: cihazı bir hata döndürdü.

Örneğin, yeni bir fan hızı ayarı ekleyebilirsiniz:

![Ayarları formu](./media/howto-set-up-template/settingsform.png)

Seçme sonra **kaydetmek**, **Fan hızı** ayarı bir kutucuk görünür ve cihaz fan hızını değiştirmek için kullanılmak üzere hazırdır.

> [!NOTE]
> Yeni bir kutucuk oluşturduktan sonra yeni ayarınız çıkışı deneyebilirsiniz. Tasarım modunda üst kapalı ilk olarak, geçiş için ekranın sağ:

![Ayarları döşeme](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Özellikler

Aygıt konumu ve seri numarası gibi cihaz ile ilişkili aygıt meta verileri özelliklerdir. Birden çok özellik kutucukları olarak görünür, cihaz şablonunuza ekleyebileceğiniz **özellikleri** sekmesi. Yeni bir cihaz oluşturma ve herhangi bir zamanda bu değerleri düzenleyebilirsiniz bir işleç özellik değerlerini belirtebilirsiniz. Ekleyebileceğiniz özellikleri altı tür vardır: sayı, metin, tarih, Değiştir, cihaz özelliği ve etiket.

İki tür özellik vardır:

- **Cihaz özellikleri** özellikleri aygıt tarafından raporlanır.
- **Uygulama özellikleri** tamamen uygulamada depolanan özellikleri. Cihazın uygulama özellikleri olanağıyla gerekir.

> [!NOTE]
> Gerçek bir cihaz özelliğinin adı bağlı, ödeme dikkat olduğunda cihaz özellikleri için cihaz bildirir. Adı tam olarak eşleşmelidir **alan adı** özelliğinin. Ad aygıt şablonun benzersiz olduğu sürece uygulama özellikleri için alan adı, istediğiniz herhangi bir şey olabilir.

Örneğin, yeni bir özellik olarak cihaz konumu ekleyebilirsiniz:

![Özellikleri formu](./media/howto-set-up-template/propertiesform.png)

Seçme sonra **kaydetmek**, aygıt konumu bir kutucuk görüntülenir:

![Özellikler bölmesi](./media/howto-set-up-template/propertiestile.png)

> [!NOTE]
> Yeni bir kutucuk oluşturduktan sonra özellik değeri değiştirebilirsiniz. Tasarım modunda üst kapalı ilk olarak, geçiş için ekranın sağ.

### <a name="create-a-location-property-powered-by-azure-maps"></a>Azure haritalar tarafından desteklenen bir konum özelliği oluşturma
Azure IOT merkezi konum verilerinize coğrafi bağlam verin ve tüm enlem ve boylam koordinatları sokak adresi ya da yalnızca enlem ve boylam koordinatları eşleyin. Bu özellik, Azure IOT merkezi Azure haritalar tarafından desteklenir.

İki tür ekleyebilirsiniz yerleşim özellikleri şunlardır:
- **Konumu olarak uygulama özelliği** depolanacağı tamamen uygulamada. Cihazın uygulama özellikleri olanağıyla gerekir.
- **Konumu olarak bir cihaz özelliği** olduğu bildirilir ve cihaz tarafından.

####<a name="adding-location-as-an-application-property"></a>Konum uygulama özelliği ekleme 
Bir konum oluşturabilirsiniz Özelliği Azure kullanarak bir uygulama özelliği olarak Azure IOT merkezi uygulamanızda eşler. Örneğin, cihaz yükleme adresi ekleyebilirsiniz. 

1. Cihaz özelliği sekmesine gidin; Tasarım modunda açık olduğundan emin olun.

![Konum özelliği](./media/howto-set-up-template/locationcloudproperty1.png)

2. Özellik sekmesinde konumu tıklatın.
3. Görünen ad, alan adı ve konumu ilk değeri isteğe bağlı olarak yapılandırın. 

![Konum özelliği formu](./media/howto-set-up-template/locationcloudproperty2.png)

Bir konum eklemek için iki desteklenen biçimler şunlardır:
- **Adresi olarak konumu**
- **Konumu koordinatları olarak** 

4. Kaydet'i tıklatın. 

![Konum özelliği alanı](./media/howto-set-up-template/locationcloudproperty3.png)

Şimdi bir işleç konumu alanın formda konum değeri güncelleştirebilirsiniz. 

####<a name="adding-location-as-a-device-property"></a>Konum bir cihaz özelliği ekleme 

Cihaz tarafından raporlanan cihaz özelliği olarak, bir konum özelliği oluşturabilirsiniz.
Örneğin, aygıt konumu izlemek istiyorsunuz.

1.  Cihaz özelliği sekmesine gidin; Tasarım modunda açık olduğundan emin olun.
2.  Cihaz özelliği kitaplığından'ı tıklatın.

![Konum özelliği alanı](./media/howto-set-up-template/locationdeviceproperty1.png)

3.  Görünen adı, alan adı, yapılandırmak ve veri türü olarak "Konum" seçin. 

> [!NOTE]
Alan adı, raporları cihaz özelliğinin adı için tam olarak eşleşmelidir. 

![Konum özelliği alanı](./media/howto-set-up-template/locationdeviceproperty2.png)

![Konum özelliği işleci görünümü](./media/howto-set-up-template/locationdeviceproperty2.png)

Konum özelliği yapılandırdığınıza göre cihaz Pano konumda görselleştirmek için bir harita eklemeniz mümkün olacaktır. Bkz: nasıl yapılır [konumu Ekle Pano Azure eşlemesinde](howto-set-up-template.md).




## <a name="rules"></a>Kurallar

Kuralları cihazları yakın gerçek zamanlı izleme işleçleri etkinleştirin. Kuralları otomatik olarak çağırma **Eylemler** kural harekete geçirdiğinde gibi bir e-posta gönderme. Kullanılabilir bir kural türü Bugün:

- **Telemetri kuralı:** seçili cihaz telemetrisi belirtilen eşiğin kestiği telemetri kural tetikler. Daha fazla bilgi edinmek [telemetri kuralları](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Pano

Pano, bir aygıt hakkındaki bilgileri görmek için bir işleç gidebilecekleri ' dir. Oluşturucu olarak, cihazın nasıl davranmakta anlamak işleçleri yardımcı bu sayfaya döşeme ekleyebilirsiniz. Birden çok Pano kutucukları aygıtını şablonunuzu ekleyebilirsiniz. Pano kutucukları ekleyebilirsiniz altı tür vardır: görüntü, çizgi grafiği, çubuk grafiği, KPI, ayarları ve özellikleri ve etiket.

Örneğin, ekleyebileceğiniz bir **ayarları ve özellikleri** döşeme ayarları ve özellikleri geçerli değerlerini seçimi göstermek için:

![Pano cihaz ayrıntıları formu](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Şimdi bir işleç Pano görüntülediğinde, cihaz ayarları ve özellikleri görüntüler bu kutucuğu görebilirsiniz:

![Pano kutucuğu](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-location-azure-map-in-dashboard"></a>Konum eklemek Azure eşlemesinde Panosu

Konum özelliği adımları olduğu gibi yapılandırdıysanız [Azure Maps]((howto-set-up-template.md) tarafından desteklenen bir konum özelliği oluşturmak, bir eşleme kullanılarak konumu görselleştirmek kuramaz aygıt Panonuzda sağ.

1.  Cihaz Pano sekmesine gidin; Tasarım modunda açık olduğundan emin olun.
2.  Cihaz Panoda kitaplıktan harita seçin. 

![Pano konum Azure eşleme Seç](./media/howto-set-up-template/locationcloudproperty4map.png)

3.  Bir başlık verin ve cihaz özelliğinin bir parçası olarak daha önce yapılandırdığınız konum özelliği seçin.

![Pano konumunu Azure haritasını yapılandırma](./media/howto-set-up-template/locationcloudproperty5map.png)

4.  Kaydet ve seçtiğiniz konum görüntüleme döşeme harita görürsünüz. 

![Pano konumunu Azure harita Görselleştirme](./media/howto-set-up-template/locationcloudproperty6map.png) 

İstenen boyuta eşlemeye yeniden boyutlandırma kuramaz.

Şimdi bir işleç Pano görüntülediğinde, bir konum eşleme dahil olmak üzere yapılandırmış olduğunuz bu tüm Pano kutucukları görebilirsiniz!

![Pano konumu Azure harita Panosu](./media/howto-set-up-template/locationcloudproperty7map.png) 



## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanızda cihaz şablonu ayarlamak öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Yeni bir cihaz şablonu sürüm oluşturma](howto-version-devicetemplate.md)
