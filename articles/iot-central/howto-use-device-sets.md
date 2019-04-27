---
title: Kullanımı cihaz Azure IOT Central uygulamanızda ayarlar | Microsoft Docs
description: Operatör cihazı kullanmak nasıl Azure IOT Central uygulamanızda ayarlar.
author: ellenfosborne
ms.author: elfarber
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: a28cf68eb449b563d93a139b830752748c448dd6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60517422"
---
# <a name="use-device-sets-in-your-azure-iot-central-application"></a>Azure IOT Central uygulamanızda cihaz kümelerini kullanma

Bu makalede, nasıl bir operatör olarak, cihazı kullanmak için Azure IOT Central uygulamanızda ayarlar açıklanır.

Bunların tümü bazı belirtilen ölçütlerle eşleşen çünkü birlikte gruplanır cihazların listesini cihaz kümesidir. Cihaz yönetme, görselleştirin ve cihazları daha küçük, mantıksal gruplar halinde gruplayarak cihazları uygun ölçekte çözümleme Yardım ayarlar. Örneğin, Seattle teknisyen sorumlu olduğu tüm cihazları bulmak Seattle teknisyen etkinleştirmek için tüm klima cihazların bir listesini oluşturun. Bu makalede oluşturma ve cihaz kümeleri yapılandırma gösterilmektedir.

## <a name="create-a-device-set"></a>Bir cihaz kümesi oluşturma

Bir cihaz kümesi oluşturmak için:

1. Seçin **cihaz kümeleri** sol gezinti menüsünde.

1. Seçin **+ yeni**.

    ![Yeni bir cihaz kümesi](media/howto-use-device-sets/image1.png)

1. Cihaz kümenizi uygulamanın tamamında benzersiz bir ad verin. Ayrıca, bir açıklama da ekleyebilirsiniz. Bir cihaz kümesi yalnızca bir tek cihaz şablonu cihazlardan içerebilir. Bu küme için kullanılacak cihaz şablonu seçin.

1. Bir özellik, bir karşılaştırma işleci ve bir değer seçerek ayarlayın cihaz için cihazları belirlemek üzere bir sorgu oluşturun. Birden çok sorgular ve karşılayan cihazlar ekleyebilirsiniz **tüm** ölçütleri cihaz kümesine yerleştirilir. Oluşturduğunuz cihaz kümesi herkes görüntüleme, değiştirme veya cihaz kümesini silmek için uygulama erişimi olan herkes için erişilebilir.

    ![Sorgu cihazı ayarlama](media/howto-use-device-sets/image2.png)

    > [!NOTE]
    > Cihaz dinamik sorgu kümesidir. Cihaz listesini görüntüleme her seferinde, listede farklı cihazları olabilir. Liste, hangi cihazların şu anda sorgunun ölçütlerine üzerinde bağlıdır.

1. **Kaydet**'i seçin.

## <a name="configure-the-dashboard-for-your-device-set"></a>Pano, cihaz kümesi için yapılandırın

Cihaz kümenizi oluşturduktan sonra yapılandırabileceğiniz kendi **Pano**. **Pano** nereye yerleştirmeniz resimleri ve bağlantıları giriş sayfasıdır. Cihaz kümesindeki aygıtlar listesinde Kılavuzlar de ekleyebilirsiniz.

1. Seçin **cihaz kümeleri** sol gezinti menüsünde.

1. Cihaz kümenizi seçin.

1. **Pano** sekmesini seçin.

1. **Düzenle**’yi seçin.

    ![Tasarım modu](media/howto-use-device-sets/image3.png)

1. Görüntü ekleme hakkında daha fazla bilgi için bkz: [hazırlama ve karşıya yükleme görüntüleri, Azure IOT Central uygulamasına](howto-prepare-images.md).

1. Bağlantı kutucuğu ekleyin:
    1. Seçin **bağlantı** sağ bölmede.
    1. Bağlantı vermek bir **başlık**.
    1. Bağlantı seçildiğinde açılması için bir URL seçin.
    1. Bağlantınız aşağıda gösteren bir açıklama vermek **başlık**.
    1. **Kaydet**'i seçin.

        ![Bağlantı kaydetme](media/howto-use-device-sets/image7.png)

    1. Taşıma ve üzerinde bağlantı kutucuğu yeniden boyutlandırmak **Pano**.

1. Bir kılavuz ekleyin. Kılavuz, seçtiğiniz sütunlardan cihaz cihazların bir tablodur.
    1. Seçin **kılavuz** sağ bölmede.
    1. Kılavuzunuzun vermek bir **başlık**.
    1. Seçim yaparak gösterilecek sütunları seçin **Ekle/Kaldır**. Açılan bölmede seçin sağ oku seçin ve gösterilen istediğiniz sütunu seçin.
    1. **Tamam**’ı seçin.
    1. **Kaydet**'i seçin.

        ![Kılavuz Kaydet](media/howto-use-device-sets/image9.png)

    1. Sürükle ve bırak, yerleştirmek için kılavuz **Pano**.

        > [!NOTE]
        > Birden çok resimleri, bağlantılar ve Kılavuzlar ekleyebilirsiniz.
  
    1. **Done** (Bitti) öğesini seçin.

### <a name="configuring-location-map-in-your-device-sets-dashboard"></a>Pano ayarlar konum eşleme Cihazınızı yapılandırma

Bir haritada cihazlarınızı konumunu görselleştirmek için bir konum eşlemesi ayarlar ekleyebilirsiniz.

Cihaz için bir konum eşleme cihaz şablonunuzda yapılandırılan konum özelliği olmalıdır Pano ayarlar eklemek için bkz: [Azure haritalar tarafından desteklenen bir konum özelliği oluşturma](howto-set-up-template.md).

1. Cihaz ayarlayın, panoda kitaplıktan harita seçin.
2. Bir başlık verin ve location özelliği, cihaz özelliği bir parçası olarak daha önce yapılandırdığınız seçin.
3. Kaydet ve cihaz kümesinde cihazlarınızı konumunu görüntüleme kutucuğuna harita görürsünüz.
4. Şimdi Pano cihazın ayarlar bir işleç görünümleri işleci yapılandırdığınız tüm kutucukları görebildiğinden, bir bakışta tüm cihazları konumu görselleştirmek için eşleme konumu dahil olmak üzere!

> [!NOTE]
> Harita, istenen boyuta yeniden boyutlandırabilirsiniz. Haritada PIN'i seçerek, cihaz bilgilerini, adını ve konumunu görüntüler. Cihaz özellik sayfasına gitmek için açılan seçebilirsiniz.

## <a name="configure-the-list-for-your-device-set"></a>Listenin, cihaz kümesi için yapılandırın

Cihaz kümenizi oluşturduktan sonra yapılandırabileceğiniz **listesi**. **Listesi** tüm cihazların cihaz seçtiğiniz sütunlarını içeren bir tablo kümesinde gösterir.

1. Seçin **cihaz kümeleri** sol gezinti menüsünde.

1. Seçin **listesi** sekmesi.

1. Seçin **sütun seçenekleri**.

    ![Sütun seçenekleri](media/howto-use-device-sets/image11.png)

1. Göstermek istediğiniz sütunu ve sağ oku seçerek seçmek için gösterilecek sütunları seçin.

    ![Sütun seçin](media/howto-use-device-sets/image12.png)

1. **Tamam**’ı seçin.

## <a name="analytics"></a>Analiz

Cihaz kümeleri analytics'te sol gezinti menüsünde ana analiz sekmesinde ile aynıdır. Siz makalede analytics hakkında daha fazla bilgi edinebilirsiniz [analytics oluşturma](howto-use-device-sets.md).

## <a name="next-steps"></a>Sonraki adımlar

Cihaz kümeleri Azure IOT Central, uygulamanızda kullanmayı öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Telemetri kuralları oluşturma](howto-create-telemetry-rules.md)
