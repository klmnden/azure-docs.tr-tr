---
title: Kullanımı cihaz Azure IOT Central uygulamanızda ayarlar | Microsoft Docs
description: Operatör cihazı kullanmak nasıl Azure IOT Central uygulamanızda ayarlar.
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: 28706ad77f48ae826b621ebdd920d26f3b87178a
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45731999"
---
# <a name="use-device-sets-in-your-azure-iot-central-application"></a>Azure IOT Central uygulamanızda cihaz kümelerini kullanma

Bu makalede, nasıl bir operatör olarak, cihazı kullanmak için Microsoft Azure IOT Central uygulamanızda ayarlar açıklanır.

Bunların tümü bazı belirtilen ölçütlerle eşleşen çünkü birlikte gruplanır cihazların listesini cihaz kümesidir. Cihaz yönetme, görselleştirin ve cihazları daha küçük, mantıksal gruplar halinde gruplayarak cihazları uygun ölçekte çözümleme Yardım ayarlar. Örneğin, Seattle kendisi sorumlu olduğu tüm cihazları bulmak Seattle teknisyen etkinleştirmek için tüm klima cihazların bir listesini oluşturun. Bu makalede oluşturma ve cihaz kümeleri yapılandırma gösterilmektedir.

## <a name="create-a-device-set"></a>Bir cihaz kümesi oluşturma

Bir cihaz kümesi oluşturmak için:

1. Seçin **cihaz kümeleri** sol gezinti menüsünde.

1. **+ Yeni** öğesine tıklayın.

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

1. Seçin **Pano** sekmesi.

1. Tıklayın **şablonu Düzen**.

    ![Tasarım modu](media/howto-use-device-sets/image3.png)

1. Görüntü ekleme hakkında daha fazla bilgi için bkz: [hazırlama ve karşıya yükleme görüntüleri, Azure IOT Central uygulamasına](howto-prepare-images.md).

1. Bağlantı kutucuğu ekleyin:
    1. Seçin **bağlantı** sağ bölmede.
    1. Bağlantı vermek bir **başlık**.
    1. Bağlantıya tıklandığında açılması için bir URL seçin.
    1. Bağlantınız aşağıda gösteren bir açıklama vermek **başlık**.
    1. **Kaydet**'i seçin.

        ![Bağlantı kaydetme](media/howto-use-device-sets/image7.png)

    1. Taşıma ve üzerinde bağlantı kutucuğu yeniden boyutlandırmak **Pano**.

1. Bir kılavuz ekleyin. Kılavuz, seçtiğiniz sütunlardan cihaz cihazların bir tablodur.
    1. Seçin **kılavuz** sağ bölmede.

        ![Kılavuz seçin](media/howto-use-device-sets/image8.png)

    1. Kılavuzunuzun vermek bir **başlık**.
    1. Ayarlar düğmesini seçerek gösterilecek sütunları seçin. Açılan bölmede seçin sağ oku seçin ve gösterilen istediğiniz sütunu seçin.
    1. **Tamam**’ı seçin.
    1. **Kaydet**'i seçin.

        ![Kılavuz Kaydet](media/howto-use-device-sets/image9.png)

    1. Sürükle ve bırak, yerleştirmek için kılavuz **Pano**.

    > [!NOTE]
    > Birden çok resimleri, bağlantılar ve Kılavuzlar ekleyebilirsiniz.
  
    1. **Bitti**’ye tıklayın.

    ![Tasarım modunda devre dışı](media/howto-use-device-sets/image10.png)


### <a name="configuring-location-map-in-your-device-sets-dashboard"></a>Pano ayarlar konum eşleme Cihazınızı yapılandırma 
Bir haritada cihazlarınızı konumunu görselleştirmek için bir konum eşlemesi ayarlar ekleyebilirsiniz.

Cihaz için bir konum eşleme cihaz şablonunuzda yapılandırılan konum özelliği olmalıdır Pano ayarlar eklemek için bkz: [Azure haritalar tarafından desteklenen bir konum özelliği oluşturma](howto-set-up-template.md).


1. Cihaz ayarlayın, panoda kitaplıktan harita seçin.

    ![Pano Maps cihazın ayarlar](media/howto-use-device-sets/LocationMaps1.png)

2. Bir başlık verin ve location özelliği, cihaz özelliği bir parçası olarak daha önce yapılandırdığınız seçin.
3. Kaydet ve cihaz kümesinde cihazlarınızı konumunu görüntüleme kutucuğuna harita görürsünüz.
4. Operatörün cihaz kümeleri panoyu görüntülediğinde tüm kutucukları görebiliyor artık bir bakışta tüm cihazları konumu görselleştirmek için eşleme konumu dahil olmak üzere yapılandırdınız! 
    
[!NOTE] İstenen boyutunuz haritaya yeniden boyutlandırabilirsiniz olacaktır. Eşlem içindeki bir PIN tıklayarak cihaz bilgilerini, adını ve konumunu görüntüler. Cihaz özellik sayfasına gitmek için açılan tıklayabilirsiniz.  


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

Cihaz kümeleri analytics'te sol gezinti menüsünde ana analiz sekmesinde ile aynıdır. Siz makalede analytics hakkında daha fazla bilgi edinebilirsiniz [analytics oluşturma](howto-create-analytics.md).

## <a name="next-steps"></a>Sonraki adımlar

Cihaz kümeleri Azure IOT Central, uygulamanızda kullanmayı öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Telemetri kuralları oluşturma](howto-create-telemetry-rules.md)
