---
title: Kullanımı cihaz Azure IOT Central uygulamanızda ayarlar | Microsoft Docs
description: Operatör cihazı kullanmak nasıl Azure IOT Central uygulamanızda ayarlar.
author: ellenfosborne
ms.author: elfarber
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: e1e7b91e0808b9e23e653acd43b95f24a46c7d27
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503202"
---
# <a name="use-device-sets-in-your-azure-iot-central-application"></a>Azure IOT Central uygulamanızda cihaz kümelerini kullanma

Bu makalede, nasıl bir operatör olarak, cihazı kullanmak için Azure IOT Central uygulamanızda ayarlar açıklanır.

Bir cihaz kümesi, çünkü bunlar bazı belirtilen ölçütlerle eşleşen birlikte gruplanır cihazlar listesidir. Cihaz yönetme, görselleştirin ve cihazları daha küçük, mantıksal gruplar halinde gruplayarak cihazları uygun ölçekte çözümleme Yardım ayarlar. Örneğin, Seattle'da bir teknisyen için sorumlu oldukları cihazları bulmayı etkinleştirmek için tüm klima cihazlarını listelemek için ayarlanmış bir cihazı oluşturabilirsiniz. Bu makalede oluşturma ve cihaz kümeleri yapılandırma gösterilmektedir.

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

1. **Kaydet**’i seçin.

## <a name="configure-the-dashboard-for-your-device-set"></a>Pano, cihaz kümesi için yapılandırın

Cihaz kümenizi oluşturduktan sonra yapılandırabileceğiniz kendi **Pano**. **Pano** resimleri ve bağlantıları nereye giriş sayfasıdır. Cihaz kümesindeki aygıtlar listesinde Kılavuzlar de ekleyebilirsiniz.

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
    1. **Kaydet**’i seçin.

        ![Bağlantı kaydetme](media/howto-use-device-sets/image7.png)

    1. Taşıma ve üzerinde bağlantı kutucuğu yeniden boyutlandırmak **Pano**.

1. Bir kılavuz ekleyin. Kılavuz, seçtiğiniz sütunlardan cihaz cihazların bir tablodur.
    1. Seçin **kılavuz** sağ bölmede.
    1. Kılavuzunuzun vermek bir **başlık**.
    1. Seçim yaparak gösterilecek sütunları seçin **Ekle/Kaldır**. Açılan bölmede seçin sağ oku seçin ve gösterilen istediğiniz sütunu seçin.
    1. **Tamam**’ı seçin.
    1. **Kaydet**’i seçin.

        ![Kılavuz Kaydet](media/howto-use-device-sets/image9.png)

    1. Sürükle ve bırak, yerleştirmek için kılavuz **Pano**.

        > [!NOTE]
        > Birden çok resimleri, bağlantılar ve Kılavuzlar ekleyebilirsiniz.
  
    1. **Done** (Bitti) öğesini seçin.

Azure IOT Central kutucukları kullanma hakkında daha fazla bilgi için bkz. [Pano kutucukları kullanacak](howto-use-tiles.md).

### <a name="configure-a-location-map-in-your-device-sets-dashboard"></a>Konum eşleme cihaz kümeleri Panonuzda yapılandırın

Cihazları cihaz kümenizdeki konumunu görselleştirmek için bir harita ekleyebilirsiniz.

Pano cihazınıza bir harita ayarlar eklemek için bir konum ölçüm veya konum özelliği cihaz şablonunuzda yapılandırmış olmanız gerekir. Daha fazla bilgi için bkz. [konumu ölçüm oluşturmak](howto-set-up-template.md) veya [Location özelliği oluşturma](howto-set-up-template.md).

1. Cihazınızda **Pano**seçin **harita** kitaplığından.
2. Başlık eklemek ve konum ölçüm ve daha önce yapılandırdığınız özelliği seçin.
3. Seçin **Kaydet** ve harita kutucuğunu son bilinen konumları cihazların cihaz kümenizdeki görüntüler.
4. İşleci, operatörün cihaz kümeleri panoyu görüntülediğinde konum eşleme dahil olmak üzere yapılandırmış olduğunuz tüm kutucukları görür.

Harita kutucuğunu panoya yeniden boyutlandırabilirsiniz. Harita üzerinde bir PIN'i seçerek, cihaz bilgilerini, adını ve konumunu görüntüler. Cihaz özellik sayfasına gitmek için açılan seçin.

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
