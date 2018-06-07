---
title: Uzaktan izleme çözümü ile - Azure kullanmaya başlama | Microsoft Docs
description: Bu öğretici benzetimli senaryoları Uzaktan izleme Çözüm Hızlandırıcısı tanıtmak için kullanır. Uzaktan izleme Çözüm Hızlandırıcısı ilk kez dağıttığınızda bu senaryoları oluşturulur.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/01/2018
ms.topic: conceptual
ms.openlocfilehash: e354e110df8d168e153c50de6f7aad08dd416464
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627816"
---
# <a name="explore-the-capabilities-of-the-remote-monitoring-solution-accelerator"></a>Uzaktan izleme Çözüm Hızlandırıcısı özelliklerini keşfedin

Bu öğretici, Uzaktan izleme çözümü anahtar özelliklerini gösterir. Bu özellikler tanıtmak için müşteri senaryoları Contoso adlı bir şirket için sanal bir IOT uygulaması kullanarak öğreticiyi gösterir.

Tipik IOT senaryolarını anlamanıza öğretici yardımcı Giden Kutusu Uzaktan izleme çözümü sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Görselleştirme ve Pano cihazlarda filtre
> * Bir uyarıya yanıt
> * Aygıtlarınızı bellenimi güncelleştirme
> * Varlıklarınızı düzenleme
> * Durdurmak ve başlatmak sanal cihazlar

Aşağıdaki video Uzaktan izleme çözümünün bir kılavuz gösterir:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Part-28-An-introduction-to-Azure-IoT-through-the-new-Remote-Monitoring-Preconfigured-Solution/Player]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için Azure aboneliğinizde Uzaktan izleme çözümü dağıtılan bir örneğini gerekir.

Uzaktan izleme çözümü dağıtılmış henüz henüz tamamlanmış olmalıdır, [Uzaktan izleme Çözüm Hızlandırıcısı dağıtmak](iot-accelerators-remote-monitoring-deploy.md) Öğreticisi.

## <a name="the-contoso-sample-iot-deployment"></a>Contoso örnek IOT dağıtım

Contoso örnek IOT dağıtım Uzaktan izleme temel senaryolar anlamak için kullanabileceğiniz çözümü Giden kutusu sağlar. Bu senaryolar, gerçekçi IOT dağıtımlarında temel alır. Büyük olasılıkla, özel gereksinimlerinizi karşılamak için Uzaktan izleme çözümü özelleştirmek seçeceğiniz, ancak Contoso örnek temel bilgi edinmenize yardımcı olur.

> [!NOTE]
> Çözüm Hızlandırıcısı, dosyayı dağıtmak için CLI kullandıysanız `deployment-{your deployment name}-output.json` dağıtılan örneğe erişmek için kullanılan URL gibi dağıtım hakkında bilgi içerir.

Contoso örnek sanal cihazlar ve bunlar üzerinde hareket için kurallar kümesi sağlar. Temel senaryoları anladığınızda, çözüm özelliklerin daha fazla araştırma sürdürebilirsiniz [Uzaktan izleme çözümünü kullanarak cihaz izleme Gelişmiş gerçekleştirme](iot-accelerators-remote-monitoring-monitor.md).

Contoso varlıklar farklı ortamlarda çeşitli yöneten bir şirkettir. Contoso uzaktan izlemek ve merkezi bir uygulamadan birden çok varlığı yönetmek için bulut tabanlı IOT uygulamaları gücünü kullanmayı planlıyor. Aşağıdaki bölümler Contoso örnek ilk yapılandırma özetini sağlar:

> [!NOTE]
> Contoso demo sanal cihazlar sağlamak ve kuralları oluşturmak için yalnızca bir yoludur. Sağlama diğer seçenekler, kendi özel cihazlarının oluşturulmasını içerir. Kendi aygıtlarını ve kuralları oluşturma hakkında daha fazla bilgi için bkz: [yönetme ve cihazlarınızı yapılandırmak](iot-accelerators-remote-monitoring-manage.md) ve [eşik tabanlı kurallar kullanarak sorunlarını algılamak](iot-accelerators-remote-monitoring-automate.md).

### <a name="contoso-devices"></a>Contoso cihazları

Contoso akıllı aygıtlar farklı türde kullanır. Bu aygıtların çeşitli telemetri akışları gönderen şirket için farklı rolleri yerine. Ayrıca, her cihaz türü farklı cihaz özellikleri vardır ve yöntemleri desteklenir.

Aşağıdaki tabloda sağlanan aygıt türlerinin bir özeti gösterilir:

| Cihaz türü        | Telemetri                                  | Özellikler                                  | Etiketler                    | Yöntemler                                                                                      |
| ------------------ | ------------------------------------------ | ------------------------------------------- | ----------------------- | -------------------------------------------------------------------------------------------- |
| Soğutucu            | Sıcaklık, nem, baskısı            | Türü, üretici yazılımı sürümüne modeli               | Konum, Floor, Kampüs | Yeniden başlatma, bellenim güncelleştirme, Acil Durum Vana yayın artış baskısı                          |
| Prototipi oluşturulurken cihaz | Sıcaklık, baskı, coğrafi konum        | Türü, üretici yazılımı sürümüne modeli               | Konum, modu          | Yeniden başlatma, bellenim güncelleştirme taşıma aygıt, Dur aygıt, sıcaklık sürüm, sıcaklık artış |
| Altyapısı             | Tank yakıt düzeyi, Coolant algılayıcı, Titreşim | Türü, üretici yazılımı sürümüne modeli               | Konum, Floor, Kampüs | Üretici yazılımı güncelleştirmesi, boş tank, dolgu tank                                              |
| Kamyon              | Coğrafi konum, hız, kargo sıcaklık     | Türü, üretici yazılımı sürümüne modeli               | Konum, yükleme          | Daha düşük kargo sıcaklık, artış Kargo sıcaklık, bellenim güncelleştirme                         |
| Fırsatınızdır           | Floor, Titreşim, sıcaklık              | Tür, üretici yazılımı sürümü, Model, coğrafi konum | Konum, Kampüs        | Stop fırsatınızdır, başlangıç fırsatınızdır, bellenim güncelleştirme                                               |

> [!NOTE]
> Contoso demo örnek hükümleri iki başına cihazı türü. Her tür için normal olarak Contoso tarafından tanımlanan sınırlar biri düzgün çalışmaz ve bazı tür arızalı bir vardır. Sonraki bölümde, Contoso cihazlar için tanımlayan kuralları hakkında bilgi edinin. Bu kurallar doğru davranış sınırlarını tanımlayın.

### <a name="contoso-rules"></a>Contoso kuralları

Contoso'da işleçleri bir aygıtın düzgün çalışıp çalışmadığını belirlemek üzere eşikleri bilirsiniz. Örneğin, rapor baskısı 250 PSI büyükse bir Soğutucu düzgün çalışmıyor. Aşağıdaki tabloda, eşik tabanlı kuralları Contoso her cihaz türü için tanımlar gösterilmektedir:

| Kural Adı | Açıklama | Eşik | Severity | Etkilenen cihazlar |
| --------- | ----------- | --------- | -------- | ---------------- |
| Soğutucu baskısı çok yüksek | Chillers normal baskısı düzeyleri daha yüksek ulaşırsa uyarıları   |P > 250 PSI       | Kritik | Chillers            |
| Prototipi oluşturulurken aygıt temp çok yüksek  | Prototipi oluşturulurken aygıtları normal sıcaklık düzeyleri daha yüksek ulaşırsa uyarıları  |T &GT; 80&deg; F |Kritik | Prototipi oluşturulurken cihazları |
| Altyapısı tank boş  | Altyapısı yakıt tank boş kalırsa uyarıları                     | F < 5 galon | Bilgi     | Altyapıları             |
| Normal Kargo sıcaklık değerinden yüksek | Kamyon'ın Kargo sıcaklık normalden yüksekse uyarıları                 | T &LT; 45&deg; F |Uyarı  | Kamyonlar              |
| Fırsatınızdır titreşimi durduruldu      | Fırsatınızdır tamamen durduğunda uyarıları (Titreşim düzeyine bağlı)                     | V < 0,1 mm |Uyarı  | Asansörler           |

### <a name="operate-the-contoso-sample-deployment"></a>Contoso örnek dağıtım çalışmayabilir

Contoso örnekteki ilk Kurulum şimdi gördünüz. Aşağıdaki bölümlerde bir işleç Çözüm Hızlandırıcısı nasıl kullanabileceğinizi üç Contoso örnek senaryoda açıklanmaktadır.

## <a name="respond-to-a-pressure-alert"></a>Bir baskısı uyarısını yanıt

Bu senaryo tanımlamak ve Soğutucu aygıt tarafından tetiklenen uyarı vermesi gösterilmektedir. Soğutucu 43 ' kat 2 oluşturulmasında Redmond içinde bulunur.

Bir operatör olarak panosunda bir Soğutucu baskısı için ilgili bir uyarı olduğunu görürsünüz. Kaydırma ve yakınlaştırma daha fazla ayrıntı için harita üzerinde.

1. Üzerinde **Pano** sayfasında **uyarıları** kılavuz, görebilir **Soğutucu baskısı çok yüksek** uyarı. Soğutucu de haritada vurgulanmıştır:

    ![Pano baskısı uyarı ve cihaz haritada gösterir](./media/iot-accelerators-remote-monitoring-explore/dashboardalarm.png)

1. Gitmek için **Bakım** sayfasında, **Bakım** Gezinti menüsünde. Üzerinde **Bakım** sayfasında Soğutucu baskısı uyarıyı tetikleyen kural ayrıntılarını görüntüleyebilirsiniz.

1. Uyarı listesinden uyarıyı tetikleyen sayısı, bildirimler ve açık ve kapalı uyarıları gösterir:

    ![Bakım Sayfası tetiklemesi uyarıların listesini gösterir](./media/iot-accelerators-remote-monitoring-explore/maintenancealarmlist.png)

1. Son uyarı listesinde en son sunucudur. Tıklatın **Soğutucu baskısı çok yüksek** telemetri ve ilişkili aygıtları görüntülemek için uyarı. Telemetri Soğutucu baskısı ani gösterir:

    ![Seçilen uyarı için telemetri Bakım Sayfası gösterir](./media/iot-accelerators-remote-monitoring-explore/maintenancetelemetry.png)

Şimdi, uyarı ve ilişkili aygıt tetiklenen sorunu tanımladınız. Bir operatör olarak, sonraki adım uyarı onaylamak ve sorunu gidermek için değildir.

1. Uyarıdaki şimdi çalıştığınız belirtmek için değiştirme **uyarı durumu** için **onaylanan**:

    ![Seçin ve uyarı Onayla](./media/iot-accelerators-remote-monitoring-explore/maintenanceacknowledge.png)

1. Soğutucu üzerinde çalışmak için onu seçin ve ardından **işleri**. Seçin **Run yöntemi**, ardından **EmergencyValveRelease**, bir iş adı eklemek **ChillerPressureRelease**ve seçin **Uygula**. Bu ayarlar hemen yürüten bir proje oluşturun:

    ![Cihazı seçin ve bir eylem zamanlama](./media/iot-accelerators-remote-monitoring-explore/maintenanceschedule.png)

1. İş durumunu görüntülemek için dönmek **Bakım** sayfasında ve işlerinin listesini görüntülemek **işleri** görünümü. İş Soğutucu Vana Basıncı yayımlamayı çalıştırıldı görebilirsiniz:

    ![İşler görünümünde işlerin durumunu](./media/iot-accelerators-remote-monitoring-explore/maintenancerunningjob.png)

Son olarak, telemetri değerleri Soğutucu normal olarak olduğunu onaylayın.

1. Uyarıları kılavuz görüntülemek için gidin **Pano** sayfası.

1. Cihaz telemetrisi görüntülemek için harita üzerinde orijinal uyarı cihazı seçin ve onaylayın geri için Normal'dir.

1. Olayı kapatmak için gidin **Bakım** sayfasında, uyarıyı seçin ve durum kümesine **kapalı**:

    ![Seçin ve uyarıyı Kapat](./media/iot-accelerators-remote-monitoring-explore/maintenanceclose.png)

## <a name="update-device-firmware"></a>Cihaz üretici yazılımını güncelleştirmek

Contoso yeni bir cihaz türü alanına test ediyor. Test döngüsünün bir parçası olarak, bu cihaz üretici yazılımını iş doğru güncelleştirmeleri emin olmanız gerekir. Aşağıdaki adımlar Uzaktan izleme çözümü birden çok aygıta üretici yazılımını güncelleştirmek için nasıl kullanılacağını gösterir.

Gerekli cihaz yönetim görevlerini gerçekleştirmek için kullanın **aygıtları** sayfası. Tüm prototipi oluşturulurken aygıtlar için filtre uygulayarak başlatın:

1. Gidin **aygıtları** sayfası. Seçin **prototipi oluşturulurken aygıtları** filtrelemek **filtreleri** açılır:

    ![Cihazlar sayfasında bir filtre kullanın](./media/iot-accelerators-remote-monitoring-explore/devicesprototypingfilter.png)

    > [!TIP]
    > Tıklatın **Yönet** kullanılabilir filtreleri yönetmek için.

1. Prototipi oluşturulurken aygıtlardan birini seçin:

    ![Cihazlar sayfasında bir cihaz seçin](./media/iot-accelerators-remote-monitoring-explore/devicesselect.png)

1. Tıklatın **işleri** düğmesini tıklatın, seçin **Run yöntemi**ve ardından **bellenim güncelleştirme**. İçin değerler girin **iş adı**, **bellenim sürümü**, ve **bellenim URI**. Seçin **Uygula** işini şimdi çalıştırmak üzere zamanlamak için:

    ![Cihazdaki üretici yazılımı güncelleştirmesini zamanla](./media/iot-accelerators-remote-monitoring-explore/devicesschedulefirmware.png)

    > [!NOTE]
    > Sanal cihazlar ile olarak istediğiniz herhangi bir URL kullanabilirsiniz **bellenim URI** değeri ve istediğiniz için herhangi bir değer **bellenim sürümü**. Sanal cihazlar URL erişmez.

1. İş etkiler kaç cihaz not edin ve seçin **Uygula**:

    ![Bellenim güncelleştirme işi Gönder](./media/iot-accelerators-remote-monitoring-explore/devicessubmitupdate.png)

Kullanabileceğiniz **Bakım** çalışırken işi izlemek için sayfa.

1. İşlerin listesini görüntülemek için gidin **Bakım** sayfasında ve tıklayın **işleri**.

1. Oluşturduğunuz proje için ilgili olay bulun. Bellenim güncelleştirme işlemini doğru bir şekilde başlatıldı doğrulayın.

<!-- 05/01 broken 
You can create a filter to verify the firmware version updated correctly.

1. To create a filter, navigate to the **Devices** page and select **Manage device groups**:

    ![Manage device groups](./media/iot-accelerators-remote-monitoring-explore/devicesmanagefilters.png)

1. Create a filter that includes only devices with the new firmware version:

    ![Create device filter](./media/iot-accelerators-remote-monitoring-explore/devicescreatefilter.png)

1. Return to the **Devices** page and verify that the device has the new firmware version. -->

## <a name="organize-your-assets"></a>Varlıklarınızı düzenleme

Contoso alan hizmet etkinlikler için iki farklı ekipler sahiptir:

* Akıllı yapı takım chillers, asansörler ve altyapıları yönetir.
* Akıllı araç takımı kamyonlar ve prototipi oluşturulurken cihazları yönetir.

Düzenlemek ve cihazlarınızı yönetmek için bir işleç olarak kolaylaştırmak için uygun takım adıyla etiketi istiyor.

Cihazlarla kullanım için etiket adları oluşturabilirsiniz.

1. Tüm cihazları görüntülemek için gidin **aygıtları** sayfasında ve seçin **tüm cihazlar** Filtresi:

    ![Tüm cihazları Göster](./media/iot-accelerators-remote-monitoring-explore/devicesalldevices.png)

1. Seçin **kamyonlar** ve **prototipi oluşturulurken** aygıtlar. Ardından **işleri**:

    ![Prototip ve kamyon cihazları seçin](./media/iot-accelerators-remote-monitoring-explore/devicesmultiselect.png)

1. Seçin **etiketi** ve adlı yeni bir metin etiketi oluşturmak **FieldService** bir değerle **ConnectedVehicle**. İş için bir ad seçin. Ardından **Uygula**:

    ![Prototip ve kamyon cihazlara etiket ekleme](./media/iot-accelerators-remote-monitoring-explore/devicesaddtag.png)

1. Seçin **Soğutucu**, **fırsatınızdır**, ve **altyapısı** aygıtlar. Ardından **işleri**:

    ![Soğutucu, altyapısı ve fırsatınızdır cihazları seçin](./media/iot-accelerators-remote-monitoring-explore/devicesmultiselect2.png)

1. Seçin **etiketi** ve adlı yeni bir metin etiketi oluşturmak **FieldService** bir değerle **SmartBuilding**. İş için bir ad seçin. Ardından **Uygula**:

    ![Soğutucu, altyapısı ve fırsatınızdır cihazlara etiket ekleme](./media/iot-accelerators-remote-monitoring-explore/devicesaddtag2.png)

Etiket değerlerini filtreleri oluşturmak için kullanabilirsiniz.

1. Üzerinde **aygıtları** sayfasında, **cihaz gruplarını yönetme**:

    ![Cihaz gruplarını yönetme](./media/iot-accelerators-remote-monitoring-explore/devicesmanagefilters.png)

1. Etiket adı kullanan yeni bir filtre oluşturun **FieldService** ve değer **SmartBuilding**. Filtre olarak Kaydet **akıllı yapı**.

1. Etiket adı kullanan yeni bir filtre oluşturun **FieldService** ve değer **ConnectedVehicle**. Filtre olarak Kaydet **bağlı araç**.

Şimdi Contoso işleci aygıtlarda değişikliği gerek kalmadan işletim ekibi bağlı cihazları sorgulayabilir.

## <a name="stop-simulated-devices"></a>Sanal cihazlar Durdur

Sanal cihazlar durdurmak için ayarlar menüsünü kullanabilirsiniz. Bu test etme ve çözüm keşfetme maliyetlerini azaltmak için yardımcı olur. Başlatma veya durdurma sanal cihazlar için:

1. Seçin **ayarları** simgesi.

1. Ardından geçiş **Flowing** açıp kapatma:

    ![Ayarlar menüsü](./media/iot-accelerators-remote-monitoring-explore/settings.png)

## <a name="customize-the-ui"></a>Kullanıcı arabirimini özelleştirme

Ayarlar menüsünden Uzaktan izleme Çözüm Hızlandırıcısı için temel customisations uygulayabilirsiniz. Şunları yapabilirsiniz:

- Açık ve koyu tema arasında geçiş yapın.
- Çözüm adını değiştirin.
- Özel bir logo karşıya yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide için öğrenilen:

>[!div class="checklist"]
> * Görselleştirme ve Pano cihazlarda filtre
> * Bir uyarıya yanıt
> * Aygıtlarınızı bellenimi güncelleştirme
> * Varlıklarınızı düzenleme
> * Durdurmak ve başlatmak sanal cihazlar

Uzaktan izleme çözümü denedikten, Uzaktan izleme çözümü gelişmiş özellikler hakkında bilgi edinmek için önerilen sonraki adımlar şunlardır:

* [Aygıtlarınızı izlemek](./iot-accelerators-remote-monitoring-monitor.md).
* [Cihazlarınızı yönetmek](./iot-accelerators-remote-monitoring-manage.md).
* [Çözümünüzü kurallarla otomatikleştirmek](./iot-accelerators-remote-monitoring-automate.md).
* [Çözümünüzü korumanıza](iot-accelerators-remote-monitoring-maintain.md).
