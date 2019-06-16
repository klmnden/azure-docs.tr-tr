---
title: Azure IoT Central'da yeni bir cihaz türü tanımlama | Microsoft Docs
description: Bu öğreticide, bir oluşturucu olarak Azure IoT Central uygulamanızda yeni bir cihaz türünü nasıl tanımlayacağınız gösterilir. Türünüz için telemetriyi, durumu, özellikleri ve ayarları tanımlarsınız.
author: dominicbetts
ms.author: dobett
ms.date: 06/07/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 54884728533aeff0472ed99660be00478227fbcd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056772"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application"></a>Öğretici: Azure IOT Central, uygulamanızda yeni bir cihaz türünü tanımlayın

Bu öğreticide, bir oluşturucu olarak Microsoft Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlamak için cihaz şablonunu nasıl tanımlayacağınız gösterilir. Cihaz şablonu, cihaz türünüz için telemetriyi, durumu, özellikleri ve ayarları tanımlar.

Gerçek bir cihaza bağlamadan önce uygulamanızı test edebilmenize olanak tanımak için, siz cihaz şablonunu oluşturduğunuzda IoT Central şablondan bir simülasyon cihazı oluşturur.

Bu öğreticide, bir **Bağlı Klima** cihaz şablonu oluşturursunuz. Bağlı klima cihazı:

* Sıcaklık ve nem gibi telemetri verilerini gönderir.
* Olup açık veya kapalı olma gibi durumu raporu.
* Üretici yazılımı sürümü ve seri numarası gibi cihaz özelliklerine sahiptir.
* Hedef sıcaklık gibi ayarlara sahiptir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni cihaz şablonu oluşturma
> * Cihazınıza telemetri verileri ekleme
> * Simülasyon telemetrisini görüntüleme
> * Olay ölçümünü tanımlama
> * Simülasyon olaylarını görüntüleme
> * Durum ölçümünü tanımlama
> * Simülasyon durumunu görüntüleme
> * Ayarları ve özellikleri kullanma
> * Komutları kullanma
> * Simülasyon cihazınızı panoda görüntüleme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için bir Azure IoT Central uygulamanızın olması gerekir. [Azure IoT Central uygulamasını oluşturma](quick-deploy-iot-central.md) hızlı başlangıcını tamamladıysanız, hızla başlangıçta oluşturduğunuz uygulamayı yeniden kullanabilirsiniz. Aksi takdirde, aşağıdaki adımları tamamlayarak boş bir Azure IoT Central uygulaması oluşturun:

1. Azure IoT Central [Uygulama Yöneticisi](https://aka.ms/iotcentral) sayfasına gidin.

2. Azure aboneliğinize erişmek için kullandığınız e-posta adresini ve parolayı girin:

    ![Kuruluş hesabınızı girin](./media/tutorial-define-device-type/sign-in.png)

3. Yeni bir Azure IOT Central uygulaması oluşturmaya başlamak için seçin **yeni uygulama**:

    ![Azure IoT Central Uygulama Yöneticisi sayfası](./media/tutorial-define-device-type/iotcentralhome.png)

4. Yeni bir Azure IoT Central uygulaması oluşturmak için:
    
   * **Deneme**’yi seçin. Deneme uygulaması oluşturmak için Azure aboneliği gerekmez.
    
      Dizinler ve abonelikler hakkında daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
    
   * **Özel Uygulama**'yı seçin.
    
   * İsteğe bağlı olarak **Contoso Klimaları** gibi kolay bir uygulama adı seçebilirsiniz. Azure IoT Central sizin için benzersiz bir URL ön eki oluşturur. Bu URL ön ekini daha akılda kalır bir şeyle değiştirebilirsiniz.
    
   * **Oluştur**’u seçin.

     ![Azure IoT Central Uygulama Oluştur sayfası](./media/tutorial-define-device-type/iotcentralcreate.png)

     Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).

## <a name="create-a-device-template"></a>Bir cihaz şablonu oluşturma

Bir oluşturucu olarak, uygulamanızda cihaz şablonları oluşturabilir ve bunları düzenleyebilirsiniz. Cihaz şablonunu oluşturduğunuzda, Azure IoT Central şablon için bir simülasyon cihazı oluşturur. Sanal cihaz uygulamanızın davranışını, gerçek bir cihaz bağlanmadan önce test olanak tanıyan telemetri oluşturur.

Yeni bir cihaz şablonu uygulamanıza eklemek için Git gerekir **cihaz şablonları** sayfası. Yapılacağını seçin **cihaz şablonları** sol gezinti menüsünde.

![Cihaz şablonlarını](./media/tutorial-define-device-type/devicetemplates.png)

## <a name="add-a-device-template"></a>Bir cihaz şablonu Ekle

Aşağıdaki adımlar, uygulamanıza sıcaklık telemetrisi gönderen cihazlar için yeni bir **Bağlı Klima** cihaz şablonunu nasıl oluşturacağınızı gösterir:

1. Üzerinde **cihaz şablonları** sayfasında **+ yeni**:

    ![Cihaz şablonlarını, cihaz şablonu oluştur](./media/tutorial-define-device-type/newtemplate.png)

2. Sayfada, aralarından seçim yapabileceğiniz şablonları gösterilir.

    ![Cihaz Şablon Kitaplığı](./media/tutorial-define-device-type/devicetemplatelibrary.png)

3. Seçin **özel**, girin **bağlı bir klima** cihaz şablonu ve ardından adı olarak **Oluştur**. Device Explorer'da cihazınızın operatörlerin görebileceği bir resmini de karşıya yükleyebilirsiniz:

    ![Özel Cihaz](./media/tutorial-define-device-type/createcustomdevice.png)

4. İçinde **bağlı bir klima** cihaz şablonu emin olduğunuz üzerinde **ölçümleri** sekmesini telemetri burada tanımlarsınız. Tanımladığınız her bir cihaz şablonu ayrı sekmeler için size sahiptir:

   * Belirtin _ölçümleri_, telemetri ve olay durumu, cihaz tarafından gönderilen gibi.

   * Tanımlama _ayarları_ cihazı denetlemek için kullanılır.

   * Tanımlama _özellikleri_ cihaz meta verilerini olan.

   * Tanımlama _komutları_ doğrudan cihazda çalıştırılacak.

   * Tanımlama _kuralları_ cihazla ilişkilendirilmiş.

   * Cihaz özelleştirme _Pano_ , işleçler için.

     ![Klima ölçümleri](./media/tutorial-define-device-type/airconmeasurements.png)

     > [!NOTE]
     > Cihaz şablonunun adını değiştirmek için sayfanın üst kısmındaki şablon adı seçin.

5. Sıcaklık telemetri ölçüm eklemek için seçin **+ yeni ölçüm**. Ardından ölçüm türü olarak **Telemetri** seçin:

    ![Bağlı klima ölçümleri](./media/tutorial-define-device-type/airconmeasurementsnew.png)

6. Cihaz şablonu için oluşturduğunuz her telemetri türü aşağıdakiler gibi [yapılandırma seçenekleri](howto-set-up-template.md) içerir:

   * Görüntü seçenekleri.

   * Telemetrinin ayrıntıları.

   * Simülasyon parametreleri.

     **Sıcaklık** telemetrinizi yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

     | Ayar              | Değer         |
     | -------------------- | -----------   |
     | Görünen ad         | Sıcaklık   |
     | Alan Adı           | sıcaklık   |
     | Birimler                | F             |
     | Min                  | 60            |
     | Maks                  | 110           |
     | Ondalık basamak sayısı       | 0             |

     Telemetri görüntüsü için renk de seçebilirsiniz. Telemetri tanımı kaydetmeyi seçin **Kaydet**:

     ![Sıcaklık simülasyonunu yapılandırma](./media/tutorial-define-device-type/temperaturesimulation.png)

7. Kısa bir süre sonra **ölçümleri** sekmesi sıcaklık telemetri bağlı sanal klima cihazınızdan bir grafiğini gösterir. Görünürlüğü, toplamayı yönetmek veya telemetri tanımını düzenlemek için denetimleri kullanın:
 
    > [!NOTE]
    > Telemetri, **ortalama** varsayılan toplama ayarlanır. 

    ![Sıcaklık simülasyonunu görüntüleme](./media/tutorial-define-device-type/viewsimulation.png)

8. Ayrıca, **Çizgi**, **Yığılmış** ve **Zaman Aralığını Düzenle** denetimlerini kullanarak grafiği de özelleştirebilirsiniz:

    ![Grafiği özelleştirme](./media/tutorial-define-device-type/customizechart.png)

## <a name="add-an-event-measurement"></a>Bir olay ölçüm Ekle

Olaylar, bir hata veya bileşeni arızası gibi bir olay olduğunda, cihazın gönderdiği zaman içinde nokta verileri tanımlamak için kullanın. Azure IOT Central, gerçek bir cihaz bağlanmadan önce uygulamanızın davranışını test sağlamak için cihaz olaylarını benzetimini yapabilirsiniz. Olay ölçümleri cihaz şablonunuzda için tanımlama **ölçümleri** görünümü.

1. Eklenecek **Fan Motor hata** olay ölçümü, select **+ yeni ölçüm**. Ardından ölçüm türü olarak **Olay** seçin:

    ![Bağlı klima ölçümleri](./media/tutorial-define-device-type/eventnew.png)

2. Cihaz şablonu için oluşturduğunuz her Olay türü aşağıdakiler gibi [yapılandırma seçenekleri](howto-set-up-template.md) içerir:

   * Görünen Ad

   * Alan Adı.

   * Önem Derecesi.

     **Fan Motoru Hatası** olayınızı yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

     | Ayar              | Değer             |
     | -------------------- | -----------       |
     | Görünen ad         | Fan Motoru Hatası   |
     | Alan Adı           | fanmotorerr       |
     | Severity             | Hata             |

     Olay tanımı kaydetmeyi seçin **Kaydet**:

     ![Olay ölçümünü yapılandırma](./media/tutorial-define-device-type/eventconfiguration.png)

3. Kısa bir süre sonra **ölçümleri** sekmesi bağlı sanal klima cihazınızın rastgele oluşturulan olaylar içeren bir grafik gösterir. Görünürlüğü yönetmek veya olay tanımını düzenlemek için denetimleri kullanın:

    ![Olay simülasyonunu görüntüleme](./media/tutorial-define-device-type/eventview.png)

1. Olayı hakkındaki ek ayrıntıları görmek için grafikteki bir olay seçin:

    ![Olay Ayrıntılarını Görüntüleme](./media/tutorial-define-device-type/eventviewdetail.png)

## <a name="define-a-state-measurement"></a>Bir durum ölçüm tanımlama

Durum tanımlayın ve bir süre cihazı veya alt bileşen durumunu görselleştirmek için kullanabilirsiniz. Azure IOT Central, gerçek bir cihaz bağlanmadan önce uygulamanızın davranışını test sağlamak için cihaz durumu benzetimini yapabilirsiniz. Cihaz türünüzün durum ölçümlerini **Ölçümler** görünümünde tanımlarsınız.

1. Eklemek için bir **fanı modu** durumu ölçümü, select **+ yeni ölçüm**. Ardından ölçüm türü olarak **Durum** seçin:

    ![Bağlı klima durum ölçümleri](./media/tutorial-define-device-type/statenew.png)

2. Durum içeren bir cihaz şablonu için tanımladığınız her tür [yapılandırma seçenekleri](howto-set-up-template.md) gibi:

   * Görünen Ad

   * Alan Adı.

   * İsteğe bağlı görünen etiketlerin değerleri.

   * Her değerin rengi.

     **Fan Modu** durumunuzu yapılandırmak için, aşağıdaki tabloda yer alan bilgileri kullanın:

     | Ayar              | Değer             |
     | -------------------- | -----------       |
     | Görünen ad         | Fan Modu          |
     | Alan Adı           | fanmode           |
     | Değer                | 1                 |
     | Görünen etiket        | İşletim         |
     | Değer                | 0                 |
     | Görünen etiket        | Durduruldu           |

     Durum ölçüm tanımını kaydetmek için seçin **Kaydet**:

     ![Durum ölçümünü yapılandırma](./media/tutorial-define-device-type/stateconfiguration.png)

3. Kısa bir süre sonra **ölçümleri** sekmesi durumlardan bağlı sanal klima cihazınızın rastgele oluşturulmuş bir grafik gösterir. Görünürlüğü yönetmek veya durum tanımını düzenlemek için denetimleri kullanın:

    ![Durum simülasyonunu görüntüleme](./media/tutorial-define-device-type/stateview.png)

4. Cihaz tarafından gönderilen küçük bir süre içinde çok fazla veri noktasının varsa, durumu ölçümü ile farklı bir görsel gösterilir. Bu süre içinde tüm veri noktalarına kronolojik sırada görüntülenir, görmek için grafiği seçin. Ayrıca zaman aralığını daraltarak ölçümü daha ayrıntılı bir şekilde de görebilirsiniz.

## <a name="settings-properties-and-commands"></a>Ayarlar, özellikler ve komutlar

Ayarlar, özellikler ve komutlar, cihaz şablonunda tanımlanan ve tek tek her cihazla ilişkilendirilen farklı değerlerdir:

* Uygulamanızdan cihaza yapılandırma verilerini göndermek için _ayarları_ kullanırsınız. Örneğin, operatör bir ayar kullanarak cihazın iki saniye olan telemetri aralığını beş saniye olarak değiştirebilir. Bir işleç bir ayar değiştiğinde ayarı bekliyor olarak işaretlenmiş bir bildirim ile cihaz yanıt verene kadar kullanıcı arabiriminde.

* Cihazınızla ilişkilendirilmiş meta verileri tanımlamak için _özellikleri_ kullanırsınız. İki özellik kategorisi vardır:
    
  * Uygulamanızda cihazınız hakkındaki bilgileri kaydetmek için _uygulama özelliklerini_ kullanırsınız. Örneğin uygulama özelliklerini kullanarak bir cihazın konumunu ve son servis tarihini kaydedebilirsiniz. Bu özellikler, uygulama içinde depolanır ve cihazı ile eşitleme. Bir operatör özelliklere değerler atayabilir.

  * Cihazın uygulamanıza özellik değerlerini göndermesini sağlamak için _cihaz özelliklerini_ kullanırsınız. Bu özellikler yalnızca cihaz tarafından değiştirilebilir. Operatör açısından cihaz özellikleri salt okunurdur. Bu bağlantılı klima senaryosunda üretici yazılımı sürümü ve cihaz seri numarası, cihaz tarafından bildirilen cihaz özellikleridir.
    
    Daha fazla bilgi için [özellikleri](howto-set-up-template.md#properties) cihaz şablon ayarlama ile ilgili nasıl yapılır Kılavuzu'nda.

* Uygulamanız üzerinden cihazınızı uzaktan yönetmek için _komutlar_ kullanırsınız. Cihazlarınızı denetlemek için komutları doğrudan buluttan çalıştırabilirsiniz. Örneğin bir operatör, cihazı yeniden başlatmak için yeniden başlat gibi bir komut çalıştırabilir.

## <a name="use-settings"></a>Ayarları kullanma

Operatörün cihaza yapılandırma verilerini gönderebilmesini sağlamak için *ayarları* kullanırsınız. Bu bölümde, **Bağlı Klima** cihaz şablonunuza operatörün bağlı klimanın hedef sıcaklığını ayarlamasına olanak tanıyan bir ayar ekleyeceksiniz.

1. Gidin **ayarları** için sekmesinde, **bağlı bir klima** cihaz şablonu.

2. Sayılar veya metinler gibi farklı türlerin ayarlarını oluşturabilirsiniz. Seçin **numarası** cihazınıza numarası ayarı eklemek için.

3. **Sıcaklık Ayarlama** ayarınızı yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Alan                | Değer           |
    | -------------------- | -----------     |
    | Görünen ad         | Sıcaklığı Ayarla |
    | Alan Adı           | setTemperature  |
    | Ölçü Birimi      | F               |
    | Ondalık Basamak Sayısı       | 1               |
    | En Küçük Değer        | 20              |
    | En Büyük Değer        | 200             |
    | İlk Değer        | 80              |
    | Açıklama          | Klimanın hedef sıcaklığını ayarlayın |

    Ardından **Kaydet**:

    ![Sıcaklığı Ayarla ayarını yapılandırma](./media/tutorial-define-device-type/configuresetting.png)

    > [!NOTE]
    > Cihaz bir ayar değişikliğini kabul ettikten sonra, ayarın durumu **eşitlendi** olarak değişir.

4. Düzenini özelleştirebilirsiniz **ayarları** taşıyarak ve ayarları kutucukları yeniden boyutlandırma sekmesinde:

    ![Ayarlar düzenini özelleştirme](./media/tutorial-define-device-type/settingslayout.png)

## <a name="use-properties"></a>Özellikleri kullanma

*Uygulama özelliklerini*, cihazınız hakkındaki bilgileri uygulamada depolamak için kullanırsınız. Bu bölümde, cihazın konumu ve son hizmet tarihini depolamak için **Bağlı Klima** cihaz şablonunuza uygulama özellikleri eklersiniz. Bu özellikler, uygulama içinde düzenlenebilir. Cihaz, uygulamada salt okunur olan seri numarasını ve üretici yazılımı sürümü gibi özellikleri de bildirir.

1. Gidin **özellikleri** için sekmesinde, **bağlı bir klima** cihaz şablonu.

1. Sayılar veya metinler gibi farklı türlerde cihaz özellikleri oluşturabilirsiniz. Cihaz şablonunuza konum özelliği eklemek için **Konum**'u seçin. Konum özelliğinizi yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Alan                | Değer                |
    | -------------------- | -------------------- |
    | Görünen ad         | Location             |
    | Alan Adı           | location             |
    | İlk Değer        | Seattle, WA          |
    | Açıklama          | Cihaz konumu      |

    Diğer alanları varsayılan değerlerinde bırakın.

    ![Cihaz özelliklerini yapılandırma](./media/tutorial-define-device-type/configureproperties.png)

    **Kaydet**’i seçin.

1. Cihaz şablonunuza son hizmet tarihi özelliği eklemek için **Tarih**'i seçin.

1. Son hizmet tarihi özelliğinizi yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Alan                | Değer                   |
    | -------------------- | ----------------------- |
    | Görünen ad         | Son Hizmet Tarihi       |
    | Alan Adı           | serviceDate             |
    | İlk Değer        | 1/1/2019                |
    | Açıklama          | Son hizmet tarihi           |

    ![Cihaz özelliklerini yapılandırma](./media/tutorial-define-device-type/configureproperties2.png)

    **Kaydet**’i seçin.

1. Düzenini özelleştirebilirsiniz **özellikleri** taşıyarak ve özellik kutucukları yeniden boyutlandırma sekmesi.

1. Cihaz şablonunuza üretici yazılımı sürümü gibi bir cihaz özelliği eklemek için **Cihaz Özelliği**'ni seçin.

1. Üretici yazılımı sürümünü yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Alan                | Değer                   |
    | -------------------- | ----------------------- |
    | Görünen ad         | Üretici yazılımı sürümü        |
    | Alan Adı           | firmwareVersion         |
    | Veri Türü            | metin                    |
    | Açıklama          | Klimanın üretici yazılımı sürümü |

    ![Üretici yazılımı sürümünü yapılandırma](./media/tutorial-define-device-type/configureproperties3.png)

    **Kaydet**’i seçin.

1. Cihaz şablonunuza seri numarası gibi bir cihaz özelliği eklemek için **Cihaz Özelliği**'ni seçin.

1. Seri numarasını yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Alan                | Değer                   |
    | -------------------- | ----------------------- |
    | Görünen ad         | Seri numarası           |
    | Alan Adı           | serialNumber            |
    | Veri Türü            | metin                    |
    | Açıklama          | Klimanın seri numarası  |

    ![Seri numarasını yapılandırma](./media/tutorial-define-device-type/configureproperties4.png)

    **Kaydet**’i seçin.

    > [!NOTE]
    > Cihaz Özelliği, cihazdan uygulamaya gönderilir. Üretici yazılımı sürümü ve seri numarası, gerçek cihazınız IoT Central'a bağlandığında güncelleştirilir.

## <a name="use-commands"></a>Komutları kullanma

Operatörün doğrudan cihazda komut çalıştırabilmesi için _komutları_ kullanırsınız. Bu bölümde, **Bağlı Klima** cihaz şablonunuza, operatörün bağlı klimada belirli bir ileti yankısı yapmasını sağlayan bir komut eklersiniz.

1. Gidin **komutları** için sekmesinde, **bağlı bir klima** şablonunu düzenlemek için cihaz şablonu.

1. Seçin **+ yeni komut** cihazınıza komut ekleneceğini ve yeni komutunuz yapılandırmaya başlamak için.

1. Yeni komutunuzu yapılandırmak için, aşağıdaki tabloda yer alan bilgileri kullanın:

    | Alan                | Değer           |
    | -------------------- | -----------     |
    | Görünen ad         | Yankı Komutu    |
    | Alan Adı           | echo            |
    | Varsayılan Zaman Aşımı      | 30              |
    | Görüntüleme Türü         | metin            |
    | Açıklama          | Cihaz Komutu  |  

    Komut için ek girişler seçerek ekleyebilirsiniz **+** için **giriş alanları**.

    ![Ayar eklemeye hazırlanma](./media/tutorial-define-device-type/commandsecho1.png)

     **Kaydet**’i seçin.

1. Düzenini özelleştirebilirsiniz **komutları** taşıyarak ve komut kutucukları yeniden boyutlandırma sekmesi.

## <a name="view-your-simulated-device"></a>Simülasyon cihazınızı görüntüleme

Tanımladınız artık, **bağlı bir klima** cihaz şablonunu özelleştirebilirsiniz, **Pano** ölçümleri, ayarları ve tanımladığınız özellikleri kapsar. Ardından bir operatör olarak panonun önizlemesine bakabilirsiniz:

1. Seçin **Pano** için sekmesinde, **bağlı bir klima** cihaz şablonu.

1. Seçin **çizgi grafik** bileşen üzerine eklemek için **Pano**.

1. Aşağıdaki tabloda yer alan bilgileri kullanarak **Çizgi Grafik** bileşenini yapılandırın:

    | Ayar      | Değer       |
    | ------------ | ----------- |
    | Unvan        | Sıcaklık |
    | Zaman Aralığı   | Son 30 dakika |
    | Ölçüler     | Sıcaklık (seçin **görünürlük** yanındaki **sıcaklık**) |

    ![Çizgi grafik ayarları](./media/tutorial-define-device-type/linechartsettings.png)

    Daha sonra **Kaydet**’e tıklayın.

1. Seçin **olay geçmişi** aşağıdaki tabloda verilen bilgileri kullanarak bileşen:

    | Ayar      | Değer       |
    | ------------ | ----------- |
    | Unvan        | Fan Motor olayları |
    | Zaman Aralığı   | Son 30 dakika |
    | Ölçüler     | Fan Motor hata (seçin **görünürlük** yanındaki **Fan Motor hata**) |

    ![Olay grafiği ayarları](./media/tutorial-define-device-type/dashboardeventchartsetting.png)

    Daha sonra **Kaydet**’e tıklayın.

1. Aşağıdaki tabloda yer alan bilgileri kullanarak **Durum Geçmişi** bileşenini yapılandırın:

    | Ayar      | Değer       |
    | ------------ | ----------- |
    | Unvan        | Fan Modu |
    | Zaman Aralığı   | Son 30 dakika |
    | Ölçüler | Fan modu (seçin **görünürlük** yanındaki **fanı modu**) |

    ![Çizgi grafik ayarları](./media/tutorial-define-device-type/dashboardstatechartsetting.png)

    Daha sonra **Kaydet**’e tıklayın.

1. Cihaz ayarlarını ve özelliklerini panoya eklemek için **ayarları ve özellikleri**. Seçin **Ekle/Kaldır** ayarları veya Panoda görmek istediğiniz özellikleri eklemek için.

1. Aşağıdaki tabloda yer alan bilgileri kullanarak **Ayarlar ve Özellikler** bileşenini yapılandırın:

    | Ayar                 | Değer         |
    | ----------------------- | ------------- |
    | Unvan                   | Cihaz özellikleri |
    | Ayarlar ve Özellikler | Sıcaklığı Ayarla<br/>Seri numarası<br/>Üretici yazılımı sürümü |

    Ayarları ve özellikleri hakkında daha önce tanımladığınız **ayarları ve özellikleri** sayfaları gösterilir **kullanılabilir sütunlar**.

    ![Sıcaklık özelliği ayarlarını belirleme](./media/tutorial-define-device-type/propertysettings4.png)

    Daha sonra **Kaydet**’e tıklayın.

1. Artık sanal verileri için bağlı yapının Klimaları Panoda görebilirsiniz. Kutucuk ve Pano Düzeni düzenleyebilirsiniz:

    ![Panoyu görüntüleme](./media/tutorial-define-device-type/dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Yeni cihaz şablonu oluşturma
> * Cihazınıza telemetri verileri ekleme
> * Simülasyon telemetrisini görüntüleme
> * Cihaz olaylarını tanımlama
> * Simülasyon olaylarını görüntüleme
> * Durumunuzu tanımlama
> * Simülasyon durumunu görüntüleme
> * Ayarları ve özellikleri kullanma
> * Komutları kullanma
> * Simülasyon cihazınızı panoda görüntüleme

Azure IOT Central uygulamanızda bir cihaz şablonunu tanımladınız, önerilen sonraki adımlar şunlardır:

* [Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules.md)
* [Operatörün görünümlerini özelleştirme](tutorial-customize-operator.md)