---
title: Azure IoT Central'da yeni bir cihaz türü tanımlama | Microsoft Docs
description: Bu öğreticide, bir oluşturucu olarak Azure IoT Central uygulamanızda yeni bir cihaz türünü nasıl tanımlayacağınız gösterilir. Türünüz için telemetriyi, durumu, özellikleri ve ayarları tanımlarsınız.
author: dominicbetts
ms.author: dobett
ms.date: 10/30/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 1ed1790d9fe1cdaa8d00b45e0684531984906c7f
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55661828"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application"></a>Öğretici: Azure IOT Central, uygulamanızda yeni bir cihaz türünü tanımlayın

Bu öğreticide, bir oluşturucu olarak Microsoft Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlamak için cihaz şablonunu nasıl tanımlayacağınız gösterilir. Cihaz şablonu, cihaz türünüz için telemetriyi, durumu, özellikleri ve ayarları tanımlar.

Gerçek bir cihaza bağlamadan önce uygulamanızı test edebilmenize olanak tanımak için, siz cihaz şablonunu oluşturduğunuzda IoT Central şablondan bir simülasyon cihazı oluşturur.

Bu öğreticide, bir **Bağlı Klima** cihaz şablonu oluşturursunuz. Bağlı klima cihazı:

* Sıcaklık ve nem gibi telemetri verilerini gönderir.
* Açık veya kapalı olduğu hava durumu gibi durumu bildirir.
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

[!INCLUDE [iot-central-experimental-note](../../includes/iot-central-experimental-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için bir Azure IoT Central uygulamanızın olması gerekir. [Azure IoT Central uygulamasını oluşturma](quick-deploy-iot-central.md) hızlı başlangıcını tamamladıysanız, hızla başlangıçta oluşturduğunuz uygulamayı yeniden kullanabilirsiniz. Aksi takdirde, aşağıdaki adımları tamamlayarak boş bir Azure IoT Central uygulaması oluşturun:

1. Azure IoT Central [Uygulama Yöneticisi](https://aka.ms/iotcentral) sayfasına gidin.

2. Azure aboneliğinize erişmek için kullandığınız e-posta adresini ve parolayı girin:

   ![Kuruluş hesabınızı girin](./media/tutorial-define-device-type/sign-in.png)

3. Yeni bir Azure IoT Central uygulaması oluşturmaya başlamak için **Yeni Uygulama**'yı seçin:

    ![Azure IoT Central Uygulama Yöneticisi sayfası](./media/tutorial-define-device-type/iotcentralhome.png)

4. Yeni bir Azure IoT Central uygulaması oluşturmak için:
    
    * **Deneme**’yi seçin. Deneme uygulaması oluşturmak için Azure aboneliği gerekmez.
    
       Dizinler ve abonelikler hakkında daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
    
    * **Özel Uygulama**'yı seçin.
    
    * İsteğe bağlı olarak **Contoso Klimaları** gibi kolay bir uygulama adı seçebilirsiniz. Azure IoT Central sizin için benzersiz bir URL ön eki oluşturur. Bu URL ön ekini daha akılda kalır bir şeyle değiştirebilirsiniz.
    
    * **Oluştur**’u seçin.

    ![Azure IoT Central Uygulama Oluştur sayfası](./media/tutorial-define-device-type/iotcentralcreate.png)

    Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).

## <a name="create-a-new-custom-device-template"></a>Yeni özel cihaz şablonu oluşturma

Bir oluşturucu olarak, uygulamanızda cihaz şablonları oluşturabilir ve bunları düzenleyebilirsiniz. Cihaz şablonunu oluşturduğunuzda, Azure IoT Central şablon için bir simülasyon cihazı oluşturur. Sanal cihaz uygulamanızın davranışını, gerçek bir cihaz bağlanmadan önce test olanak tanıyan telemetri oluşturur.

Uygulamanıza yeni cihaz şablonu eklemek için **Uygulama Oluşturucu** sayfasına gitmelisiniz. Bunu yapmak için sol gezinti menüsünde **Uygulama oluşturucu**'yu seçin.

![Uygulama Oluşturucu sayfası](./media/tutorial-define-device-type/builderhome.png)

## <a name="add-a-device-and-define-telemetry"></a>Cihaz ekleme ve telemetri tanımlama

Aşağıdaki adımlar, uygulamanıza sıcaklık telemetrisi gönderen cihazlar için yeni bir **Bağlı Klima** cihaz şablonunu nasıl oluşturacağınızı gösterir:

1. **Uygulama Oluşturucu** sayfasında **Cihaz Şablonu Oluştur**'u seçin:

    ![Uygulama Oluşturucu sayfası, Cihaz Şablonu Oluştur](./media/tutorial-define-device-type/builderhomedevices.png)

2. **Cihaz Şablonları** sayfasında **Özel**'i seçin. **Özel** cihaz şablonu, bağlı klimanızın tüm özelliklerini ve davranışlarını tanımlamanıza olanak sağlar:

    ![Cihazlar](./media/tutorial-define-device-type/builderhomedevicescustom.png)

3. **Yeni Cihaz Şablonu** sayfasında, cihazınızın adı olarak **Bağlı Klima** girin ve ardından **Oluştur**'u seçin. Device Explorer'da cihazınızın operatörlerin görebileceği bir resmini de karşıya yükleyebilirsiniz:

    ![Özel Cihaz](./media/tutorial-define-device-type/createcustomdevice.png)

4. **Bağlı Klima** cihaz şablonunda, telemetriyi tanımladığınız **Ölçümler** sayfasına geldiğinizden emin olun. Tanımladığınız her cihaz şablonunun aşağıdakileri yapmanızı sağlayan ayrı sayfaları vardır:

    * Telemetri, olay ve durum gibi cihaz tarafından gönderilen ölçümleri belirtme.
    
    * Cihazı denetlemek için kullanılan ayarları tanımlama.
    
    * Cihaz meta verisi olan özellikleri tanımlama.

    * Doğrudan cihazda çalıştırılacak komutları tanımlama.
    
    * Cihazla ilişkilendirilen kuralları tanımlama.
    
    * Operatörleriniz için cihaz panosunu özelleştirme.

    Cihaz şablonunu tanımlamak istediğinizde **Şablonu Düzenle**'yi seçin. İşiniz bittiğinde **Bitti**'yi seçin. 

    ![Klima ölçümleri](./media/tutorial-define-device-type/airconmeasurements.png)

    > [!NOTE]
    > Cihazın veya cihaz şablonunun adını değiştirmek için, sayfanın en üstündeki metne tıklayın.

5. Sıcaklık telemetri ölçümünü eklemek için **Yeni Ölçüm**'ü seçin. Ardından ölçüm türü olarak **Telemetri** seçin:

    ![Bağlı klima ölçümleri](./media/tutorial-define-device-type/airconmeasurementsnew.png)

6. Cihaz şablonu için oluşturduğunuz her telemetri türü aşağıdakiler gibi [yapılandırma seçenekleri](howto-set-up-template.md) içerir:

    * Görüntü seçenekleri.

    * Telemetrinin ayrıntıları.

    * Simülasyon parametreleri.

    **Sıcaklık** telemetrinizi yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Ayar              | Değer         |
    | -------------------- | -----------   |
    | Görünen Ad         | Sıcaklık   |
    | Alan Adı           | sıcaklık   |
    | Birimler                | F             |
    | Min                  | 60            |
    | Maks                  | 110           |
    | Ondalık basamak sayısı       | 0             |

    Telemetri görüntüsü için renk de seçebilirsiniz. Telemetri tanımını kaydetmek için **Kaydet**'i seçin:

    ![Sıcaklık simülasyonunu yapılandırma](./media/tutorial-define-device-type/temperaturesimulation.png)

7. Kısa bir süre sonra, **Ölçümler** sayfasında bağlı klima simülasyon cihazınızdan gelen sıcaklık telemetrisinin grafiği gösterilir. Görünürlüğü, toplamayı yönetmek veya telemetri tanımını düzenlemek için denetimleri kullanın:

    ![Sıcaklık simülasyonunu görüntüleme](./media/tutorial-define-device-type/viewsimulation.png)

8. Ayrıca, **Çizgi**, **Yığılmış** ve **Zaman Aralığını Düzenle** denetimlerini kullanarak grafiği de özelleştirebilirsiniz:

    ![Grafiği özelleştirme](./media/tutorial-define-device-type/customizechart.png)

## <a name="define-event-measurement"></a>Olay ölçümünü tanımlama

Cihaz tarafından bir hata veya bileşen arızası gibi önemli bir duruma işaret etmek için gönderilmiş, zamanın belirli bir noktasına ilişkin verileri tanımlamak için Olay kullanabilirsiniz. Telemetri ölçümler gibi Azure IOT Central, gerçek bir cihaz bağlanmadan önce uygulamanızın davranışını test sağlamak için cihaz olaylarını benzetimini yapabilirsiniz. Cihaz türünüzün olay ölçümlerini **Ölçümler** görünümünde tanımlarsınız.

1. **Fan Motoru Hatası** olay ölçümünü eklemek için **Yeni Ölçüm**'ü seçin. Ardından ölçüm türü olarak **Olay** seçin:

    ![Bağlı klima ölçümleri](./media/tutorial-define-device-type/eventnew.png)

2. Cihaz şablonu için oluşturduğunuz her Olay türü aşağıdakiler gibi [yapılandırma seçenekleri](howto-set-up-template.md) içerir:

   * Görünen Ad

   * Alan Adı.

   * Önem Derecesi.

    **Fan Motoru Hatası** olayınızı yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Ayar              | Değer             |
    | -------------------- | -----------       |
    | Görünen Ad         | Fan Motoru Hatası   |
    | Alan Adı           | fanmotorerr       |
    | Severity             | Hata             |

    Olay tanımını kaydetmek için **Kaydet**'i seçin:

    ![Olay ölçümünü yapılandırma](./media/tutorial-define-device-type/eventconfiguration.png)

3. Kısa bir süre sonra, **Ölçümler** sayfasında bağlı klima simülasyon cihazınızdan rastgele oluşturulan olayların grafiği gösterilir. Görünürlüğü yönetmek veya olay tanımını düzenlemek için denetimleri kullanın:

    ![Olay simülasyonunu görüntüleme](./media/tutorial-define-device-type/eventview.png)

1. Olay hakkındaki diğer ayrıntıları görmek için grafikte olaya tıklayın:

    ![Olay Ayrıntılarını Görüntüleme](./media/tutorial-define-device-type/eventviewdetail.png)

## <a name="define-state-measurement"></a>Durum ölçümünü tanımlama

Belirli bir zaman süresince cihazın veya bileşeninin durumunu tanımlamak ve görselleştirmek için Durum ölçümünü kullanabilirsiniz. Telemetri ölçümler gibi Azure IOT Central, gerçek bir cihaz bağlanmadan önce uygulamanızın davranışını test sağlamak için cihaz durumu benzetimini yapabilirsiniz. Cihaz türünüzün durum ölçümlerini **Ölçümler** görünümünde tanımlarsınız.

1. **Fan Modu** ölçümünü eklemek için **Yeni Ölçüm**'ü seçin. Ardından ölçüm türü olarak **Durum** seçin:

    ![Bağlı klima durum ölçümleri](./media/tutorial-define-device-type/statenew.png)

2. Cihaz şablonu için oluşturduğunuz her Durum türü aşağıdakiler gibi [yapılandırma seçenekleri](howto-set-up-template.md) içerir:

   * Görünen Ad

   * Alan Adı.

   * İsteğe bağlı görünen etiketlerin değerleri.

   * Her değerin rengi.

    **Fan Modu** durumunuzu yapılandırmak için, aşağıdaki tabloda yer alan bilgileri kullanın:

    | Ayar              | Değer             |
    | -------------------- | -----------       |
    | Görünen Ad         | Fan Modu          |
    | Alan Adı           | fanmode           |
    | Değer                | 1                 |
    | Görünen etiket        | İşletim         |
    | Değer                | 0                 |
    | Görünen etiket        | Durduruldu           |

    Durum ölçümü tanımını kaydetmek için **Kaydet**'i seçin:

    ![Durum ölçümünü yapılandırma](./media/tutorial-define-device-type/stateconfiguration.png)

3. Kısa bir süre sonra, **Ölçümler** sayfasında bağlı klima simülasyon cihazınızdan rastgele oluşturulan durumların grafiği gösterilir. Görünürlüğü yönetmek veya durum tanımını düzenlemek için denetimleri kullanın:

    ![Durum simülasyonunu görüntüleme](./media/tutorial-define-device-type/stateview.png)

4. Kısa bir süre içinde cihazdan çok fazla veri noktası gönderildiği olursa, durum ölçümü aşağıda gösterildiği gibi farklı bir görselle görüntülenir. Grafiğe tıklarsanız, söz konusu süre içindeki tüm veri noktaları kronolojik sırada görüntülenir. Ayrıca zaman aralığını daraltarak ölçümü daha ayrıntılı bir şekilde de görebilirsiniz.

    ![Durum ayrıntılarını görüntüleme](./media/tutorial-define-device-type/stateviewdetail.png)

## <a name="settings-properties-and-commands"></a>Ayarlar, özellikler ve komutlar

Ayarlar, özellikler ve komutlar, cihaz şablonunda tanımlanan ve tek tek her cihazla ilişkilendirilen farklı değerlerdir:

* Uygulamanızdan cihaza yapılandırma verilerini göndermek için _ayarları_ kullanırsınız. Örneğin, operatör bir ayar kullanarak cihazın iki saniye olan telemetri aralığını beş saniye olarak değiştirebilir. Operatör ayarı değiştirdiğinde, cihaz ayar değişikliğini uyguladığını kabul edene kadar bu ayar kullanıcı arabiriminde beklemede olarak işaretlenir.

* Cihazınızla ilişkilendirilmiş meta verileri tanımlamak için _özellikleri_ kullanırsınız. İki özellik kategorisi vardır:
    
    * Uygulamanızda cihazınız hakkındaki bilgileri kaydetmek için _uygulama özelliklerini_ kullanırsınız. Örneğin uygulama özelliklerini kullanarak bir cihazın konumunu ve son servis tarihini kaydedebilirsiniz. Bu özellikler uygulamada depolanır ve cihazla eşitlenmez. Bir operatör özelliklere değerler atayabilir.

    * Cihazın uygulamanıza özellik değerlerini göndermesini sağlamak için _cihaz özelliklerini_ kullanırsınız. Bu özellikler yalnızca cihaz tarafından değiştirilebilir. Operatör açısından cihaz özellikleri salt okunurdur. Bu bağlantılı klima senaryosunda üretici yazılımı sürümü ve cihaz seri numarası, cihaz tarafından bildirilen cihaz özellikleridir. 
    
    Daha fazla bilgi için cihaz şablonu ayarlama kılavuzunun [Özellikler][lnk-define-template] bölümünü inceleyin.

* Uygulamanız üzerinden cihazınızı uzaktan yönetmek için _komutlar_ kullanırsınız. Cihazlarınızı denetlemek için komutları doğrudan buluttan çalıştırabilirsiniz. Örneğin bir operatör, cihazı yeniden başlatmak için yeniden başlat gibi bir komut çalıştırabilir.

## <a name="use-settings"></a>Ayarları kullanma

Operatörün cihaza yapılandırma verilerini gönderebilmesini sağlamak için *ayarları* kullanırsınız. Bu bölümde, **Bağlı Klima** cihaz şablonunuza operatörün bağlı klimanın hedef sıcaklığını ayarlamasına olanak tanıyan bir ayar ekleyeceksiniz.

1. **Bağlı Klima** cihaz şablonunuzun **Ayarlar** sayfasına gidin:

    ![Ayar eklemeye hazırlanma](./media/tutorial-define-device-type/deviceaddsetting.png)

    Sayılar veya metinler gibi farklı türlerin ayarlarını oluşturabilirsiniz.

2. Cihazınıza bir sayı ayarı eklemek için **Sayı**'yı seçin.

3. **Sıcaklık Ayarlama** ayarınızı yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Alan                | Değer           |
    | -------------------- | -----------     |
    | Görünen Ad         | Sıcaklığı Ayarla |
    | Alan Adı           | setTemperature  |
    | Ölçü Birimi      | F               |
    | Ondalık Basamak Sayısı       | 1               |
    | En Küçük Değer        | 20              |
    | En Büyük Değer        | 200             |
    | İlk Değer        | 80              |
    | Açıklama          | Klimanın hedef sıcaklığını ayarlayın |

    Ardından **Kaydet**'i seçin:

    ![Sıcaklığı Ayarla ayarını yapılandırma](./media/tutorial-define-device-type/configuresetting.png)

    > [!NOTE]
    > Cihaz bir ayar değişikliğini kabul ettikten sonra, ayarın durumu **eşitlendi** olarak değişir.

4. Ayarlar kutucuklarını taşıyarak ve yeniden boyutlandırarak **Ayarlar** sayfasının düzenini özelleştirebilirsiniz:

    ![Ayarlar düzenini özelleştirme](./media/tutorial-define-device-type/settingslayout.png)

## <a name="use-properties"></a>Özellikleri kullanma 

*Uygulama özelliklerini*, cihazınız hakkındaki bilgileri uygulamada depolamak için kullanırsınız. Bu bölümde, cihazın konumu ve son hizmet tarihini depolamak için **Bağlı Klima** cihaz şablonunuza uygulama özellikleri eklersiniz. Bunların ikisinin de cihazın değiştirilebilir özellikleri olduğuna dikkat edin. Ayrıca cihaz tarafından bildirilen cihaz seri numarası ve ürün yazılımı sürümü gibi değiştirilemeyen salt okunur cihaz özelikleri vardır.
 
1. **Bağlı Klima** cihaz şablonunuzun **Özellikler** sayfasına gidin:

    ![Özellik eklemeye hazırlanma](./media/tutorial-define-device-type/deviceaddproperty.png)

    Sayılar veya metinler gibi farklı türlerde cihaz özellikleri oluşturabilirsiniz. Cihaz şablonunuza konum özelliği eklemek için **Konum**'u seçin.

1. Konum özelliğinizi yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Alan                | Değer                |
    | -------------------- | -------------------- |
    | Görünen Ad         | Konum             |
    | Alan Adı           | location             |
    | İlk Değer        | Seattle, WA          |
    | Açıklama          | Cihaz konumu      |

    Diğer alanları varsayılan değerlerinde bırakın.

    ![Cihaz özelliklerini yapılandırma](./media/tutorial-define-device-type/configureproperties.png)

    **Kaydet**'i seçin.

1. Cihaz şablonunuza son hizmet tarihi özelliği eklemek için **Tarih**'i seçin.

1. Son hizmet tarihi özelliğinizi yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Alan                | Değer                   |
    | -------------------- | ----------------------- |
    | Görünen Ad         | Son Hizmet Tarihi       |
    | Alan Adı           | serviceDate             |
    | İlk Değer        | 1.1.2018                |
    | Açıklama          | Son hizmet tarihi           |

    ![Cihaz özelliklerini yapılandırma](./media/tutorial-define-device-type/configureproperties2.png)

    **Kaydet**'i seçin.

5. Özellik kutucuklarını taşıyarak ve yeniden boyutlandırarak **Özellikler** sayfasının düzenini özelleştirebilirsiniz:

    ![Özellikler düzenini özelleştirme](./media/tutorial-define-device-type/propertieslayout.png)

1. Cihaz şablonunuza üretici yazılımı sürümü gibi bir cihaz özelliği eklemek için **Cihaz Özelliği**'ni seçin.

1.  Üretici yazılımı sürümünü yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Alan                | Değer                   |
    | -------------------- | ----------------------- |
    | Görünen Ad         | Üretici yazılımı sürümü        |
    | Alan Adı           | firmwareVersion         |
    | Veri Türü            | metin                    |
    | Açıklama          | Klimanın üretici yazılımı sürümü |

    ![Üretici yazılımı sürümünü yapılandırma](./media/tutorial-define-device-type/configureproperties3.png)
    
    **Kaydet**'i seçin.

1. Cihaz şablonunuza seri numarası gibi bir cihaz özelliği eklemek için **Cihaz Özelliği**'ni seçin.

1. Seri numarasını yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın:

    | Alan                | Değer                   |
    | -------------------- | ----------------------- |
    | Görünen Ad         | Seri numarası           |
    | Alan Adı           | serialNumber            |
    | Veri Türü            | metin                    |
    | Açıklama          | Klimanın seri numarası  |

    ![Seri numarasını yapılandırma](./media/tutorial-define-device-type/configureproperties4.png)
    
    **Kaydet**'i seçin.
    
    > [!NOTE]
    > Cihaz Özelliği, cihazdan uygulamaya gönderilir. Üretici yazılımı sürümü ve seri numarası, gerçek cihazınız IoT Central'a bağlandığında güncelleştirilir.

## <a name="use-commands"></a>Komutları kullanma

Operatörün doğrudan cihazda komut çalıştırabilmesi için _komutları_ kullanırsınız. Bu bölümde, **Bağlı Klima** cihaz şablonunuza, operatörün bağlı klimada belirli bir ileti yankısı yapmasını sağlayan bir komut eklersiniz.

1. Şablonu düzenlemek için **Bağlı Klima** cihaz şablonunuzun **Komutlar** sayfasına gidin. 

1. Cihazınıza komut eklemek ve yeni komutu yapılandırmaya başlamak için **Yeni Komut**'a tıklayın.

   Gereksinimlerinize bağlı olarak farklı türde komutlar oluşturabilirsiniz. 

1. Yeni komutunuzu yapılandırmak için, aşağıdaki tabloda yer alan bilgileri kullanın:

    | Alan                | Değer           |
    | -------------------- | -----------     |
    | Görünen Ad         | Yankı Komutu    |
    | Alan Adı           | echo            |
    | Varsayılan Zaman Aşımı      | 30              |
    | Görüntüleme Türü         | metin            |
    | Açıklama          | Cihaz Komutu  |  

    **Giriş Alanları** bölümündeki **+** öğesine tıklayarak komutunuza başka girişler ekleyebilirsiniz.

    ![Ayar eklemeye hazırlanma](media/tutorial-define-device-type/commandsecho1.png)

     **Kaydet**'i seçin.

1. Komut kutucuklarını taşıyarak ve yeniden boyutlandırarak **Komutlar** sayfasının düzenini özelleştirebilirsiniz:

    ![Ayarlar düzenini özelleştirme](media/tutorial-define-device-type/commandstileresize1.png)

## <a name="view-your-simulated-device"></a>Simülasyon cihazınızı görüntüleme

Artık **Bağlı Klima** cihaz şablonunuzu tanımladığınıza göre, **Panosunu** özelleştirerek tanımladığınız ölçümleri, ayarları ve özellikleri içermesini sağlayabilirsiniz. Ardından bir operatör olarak panonun önizlemesine bakabilirsiniz:

1. **Bağlı Klima** cihaz şablonunuzun **Pano** sayfasını seçin:

    ![Bağlı klima cihazı panoları](./media/tutorial-define-device-type/aircondashboards.png)

1. **Çizgi Grafik** bileşenini seçerek bunu **Pano**'ya ekleyin:

    ![Pano bileşenleri](./media/tutorial-define-device-type/dashboardcomponents1.png)

1. Aşağıdaki tabloda yer alan bilgileri kullanarak **Çizgi Grafik** bileşenini yapılandırın:

    | Ayar      | Değer       |
    | ------------ | ----------- |
    | Unvan        | Sıcaklık |
    | Zaman Aralığı   | Son 30 dakika |
    | Ölçüler     | sıcaklık (**sıcaklık** öğesinin yanındaki **Görünürlük** düğmesini seçin) |

    ![Çizgi grafik ayarları](./media/tutorial-define-device-type/linechartsettings.png)

    Ardından **Kaydet**'i seçin.

1. Aşağıdaki tabloda yer alan bilgileri kullanarak **Olay Geçmişi** bileşenini yapılandırın:

    | Ayar      | Değer       |
    | ------------ | ----------- |
    | Unvan        | Olaylar |
    | Zaman Aralığı   | Son 30 dakika |
    | Ölçüler     | Fan Motoru Hatası (**Fan Motoru Hatası**'nın yanındaki **Görünürlük** düğmesini seçin) |

    ![Çizgi grafik ayarları](./media/tutorial-define-device-type/dashboardeventchartsetting.png)

    Ardından **Kaydet**'i seçin.

1. Aşağıdaki tabloda yer alan bilgileri kullanarak **Durum Geçmişi** bileşenini yapılandırın:

    | Ayar      | Değer       |
    | ------------ | ----------- |
    | Unvan        | Fan Modu |
    | Zaman Aralığı   | Son 30 dakika |
    | Ölçüler | Fan Modu (**Fan Modu** öğesinin yanındaki **Görünürlük** düğmesini seçin) |

    ![Çizgi grafik ayarları](./media/tutorial-define-device-type/dashboardstatechartsetting.png)

    Ardından **Kaydet**'i seçin.

1. Panoya sıcaklık ayarlama ayarını eklemek için **Ayarlar ve Özellikler**'i seçin. Panoda görmek istediğiniz ayarları veya özellikleri eklemek için **Ekle/Kaldır**'a tıklayın. 

    ![Pano bileşenleri](./media/tutorial-define-device-type/dashboardcomponents4.png)

1. Aşağıdaki tabloda yer alan bilgileri kullanarak **Ayarlar ve Özellikler** bileşenini yapılandırın:

    | Ayar                 | Değer         |
    | ----------------------- | ------------- |
    | Unvan                   | Hedef sıcaklığı ayarlama |
    | Ayarlar ve Özellikler | Sıcaklığı Ayarla |

    Ayarlar ve Özellikler sayfalarında önceden tanımlamış olduğunuz ayarlar ve özellikler, Kullanılabilir Sütunlar bölümünde gösterilir. 

    ![Sıcaklık özelliği ayarlarını belirleme](./media/tutorial-define-device-type/propertysettings4.png)

    Ardından **Tamam**’ı seçin.

1. Panoya cihaz seri numarası eklemek için **Ayarlar ve Özellikler**'i seçin:

    ![Pano bileşenleri](./media/tutorial-define-device-type/dashboardcomponents3.png)

1. Aşağıdaki tabloda yer alan bilgileri kullanarak **Ayarlar ve Özellikler** bileşenini yapılandırın:

    | Ayar                 | Değer         |
    | ----------------------- | ------------- |
    | Unvan                   | Seri numarası |
    | Ayarlar ve Özellikler | Seri numarası |

    ![Seri numarası özellik ayarları](./media/tutorial-define-device-type/propertysettings5.png)

    Ardından **Tamam**’ı seçin.

1. Panoya cihaz üretici yazılımı sürümünü eklemek için **Ayarlar ve Özellikler**'i seçin:

    ![Pano bileşenleri](./media/tutorial-define-device-type/dashboardcomponents4.png)

1. Aşağıdaki tabloda yer alan bilgileri kullanarak **Ayarlar ve Özellikler** bileşenini yapılandırın:

    | Ayar                 | Değer            |
    | ----------------------- | ---------------- |
    | Unvan                   | Üretici yazılımı sürümü |
    | Ayarlar ve Özellikler | Üretici yazılımı sürümü |

    ![Seri numarası özellik ayarları](./media/tutorial-define-device-type/propertysettings6.png)

    Ardından **Tamam**’ı seçin.

1. Bir operatör olarak panoyu görüntülemek için, sayfanın sağ üst kısmında **Şablonu Düzenle**'yi kapatın.

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

Artık Azure IoT Central uygulamanızda bir cihaz şablonu tanımladığınıza göre, önerilen sonraki adımlar şunlardır:

* [Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules.md)
* [Operatörün görünümlerini özelleştirme](tutorial-customize-operator.md)

[lnk-define-template]: /azure/iot-central/howto-set-up-template#properties