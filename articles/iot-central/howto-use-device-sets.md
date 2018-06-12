---
title: Aygıt kullanma Azure IOT merkezi uygulamanızda ayarlar | Microsoft Docs
description: Bir operatör olarak cihazı kullanmak nasıl Azure IOT merkezi uygulamanızda ayarlar.
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: ef1fa64a276926a35dbf98646317bfe29200bb22
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261687"
---
# <a name="use-device-sets-in-your-azure-iot-central-application"></a>Azure IOT merkezi uygulamanızda cihaz kümeleri kullanma

Bu makalede, nasıl bir operatör olarak cihazı kullanmak için Microsoft Azure IOT merkezi uygulamanızda ayarlar açıklanır.

Bir cihaz kümesi hepsi bazı belirtilen ölçütlerle eşleşen birlikte gruplandırılmış aygıtlar listesidir. Cihaz yönetme, görselleştirin ve cihazları daha küçük, mantıksal gruplar halinde gruplayarak ölçekte aygıtları analiz Yardım ayarlar. Örneğin, Seattle aynen sorumlu olduğu tüm aygıtlar bulunacağını Seattle teknisyen etkinleştirmek için tüm klima cihazların bir listesini oluşturun. Bu makalede oluşturma ve cihaz kümelerini yapılandırma gösterilmektedir.

## <a name="create-a-device-set"></a>Bir cihaz kümesi oluşturma

Bir cihaz kümesi oluşturmak için:

1. Seçin **aygıt kümeleri** sol gezinti menüsünde.

1. **+ Yeni** öğesine tıklayın.

    ![Yeni bir cihaz kümesi](media/howto-use-device-sets/image1.png)

1. Cihaz kümenizi tüm uygulama benzersiz bir ad verin. Ayrıca, bir açıklama ekleyebilirsiniz. Bir cihaz kümesi yalnızca tek bir aygıtta şablon aygıtlardan içerebilir. Bu küme için kullanmak üzere cihaz şablonu seçin.

1. Bir özellik, bir karşılaştırma işleci ve bir değer seçerek ayarlayın cihaz için cihazları belirlemek üzere sorgu oluşturun. Birden çok sorgular ve karşılayan cihazları ekleyebilirsiniz **tüm** ölçütleri aygıt kümesinde yerleştirilir. Oluşturduğunuz cihaz kümesi için herkes görüntüleme, değiştirme veya cihaz kümesini silmek için uygulamaya erişimi olan herkes erişilebilir.

    ![Sorgu aygıtı ayarlama](media/howto-use-device-sets/image2.png)

    > [!NOTE]
    > Aygıt dinamik bir sorguyu kümesidir. Cihaz listesini görüntülemek her zaman, listede farklı aygıtlar olabilir. Liste, hangi aygıtların şu anda sorgunun ölçütlerine üzerinde bağlıdır.

1. **Kaydet**'i seçin.

## <a name="configure-the-dashboard-for-your-device-set"></a>Pano aygıt kümeniz için yapılandırma

Cihaz kümenizi oluşturduktan sonra yapılandırabileceğiniz kendi **Pano**. **Pano** burada yerleştirebileceğiniz görüntüler ve bağlantıları giriş sayfasıdır. Cihaz kümesindeki aygıtlar listesinde kılavuzları de ekleyebilirsiniz.

1. Seçin **aygıt kümeleri** sol gezinti menüsünde.

1. Seçin **Pano** sekmesi.

1. Aç **tasarım modu**.

    ![Tasarım modu](media/howto-use-device-sets/image3.png)

1. Görüntü ekleme hakkında daha fazla bilgi için bkz: [hazırlama ve karşıya yükleme görüntüleri Azure IOT merkezi uygulamanıza](howto-prepare-images.md).

1. Bir bağlantı kutucuğu ekleyin:
    1. Seçin **bağlantı** sağ bölmede.

        ![Bağlantıyı seçin](media/howto-use-device-sets/image6.png)

    1. Bağlantı vermek bir **başlık**.
    1. Bağlantı tıklatıldığında açılması için bir URL seçin.
    1. Bağlantı aşağıda gösteren bir açıklama vermek **başlık**.
    1. **Kaydet**'i seçin.

        ![Bağlantı Kaydet](media/howto-use-device-sets/image7.png)

    1. Taşıma ve bağlantı döşeme üzerinde yeniden boyutlandırma **Pano**.

1. Bir kılavuz ekleyin. Bir kılavuz seçtiğiniz sütunlardan aygıtta aygıtların bir tablodur.
    1. Seçin **kılavuz** sağ bölmede.

        ![Kılavuz seçin](media/howto-use-device-sets/image8.png)

    1. Kılavuzunuzun vermek bir **başlık**.
    1. Ayarlar düğmesini seçerek gösterilecek sütunları seçin. Açılır panelinde gösterilmesini istediğinizi ve seçmek için sağ oka seçin sütun seçin.
    1. **Tamam**’ı seçin.
    1. **Kaydet**'i seçin.

        ![Kılavuz Kaydet](media/howto-use-device-sets/image9.png)

    1. Sürükleme ve bırakma üzerinde yerleştirmek için kılavuz **Pano**.

    > [!NOTE]
    > Birden çok görüntüleri, bağlantılar ve kılavuzları ekleyebilirsiniz.
  
    1. Kapatma **Tasarım modunda**.

    ![Tasarım modunda devre dışı](media/howto-use-device-sets/image10.png)


### <a name="configuring-location-map-in-your-device-sets-dashboard"></a>Konum eşleme Cihazınızı yapılandırma Panosu ayarlar 
Bir harita aygıtlarınızı konumunu görselleştirmek için bir konum eşleme ayarlar ekleyebilirsiniz. 

Konum eşleme, cihaz için cihaz şablonunuzda yapılandırılan konum özelliği Pano sahip olmanız gerekir ayarlar eklemek için bkz: [Azure haritalar tarafından desteklenen bir konum özelliği oluşturma](howto-set-up-template.md).


1. Cihaz kümeleri Panoda kitaplıktan harita seçin. 

    ![Cihaz Pano eşlemeleri ayarlar](media/howto-use-device-sets/LocationMaps1.png)


2. Bir başlık verin ve cihaz özelliğinin bir parçası olarak daha önce yapılandırdığınız konum özelliği seçin.

    ![Pano eşlemeleri yapılandırma](media/howto-use-device-sets/LocationMaps2.png)

3. Kaydetme ve aygıt kümesinde aygıtlarınızı konumunu görüntüleme döşeme harita görürsünüz.

    ![Pano eşlemeleri Kaydet](media/howto-use-device-sets/LocationMaps3.png)


5. Bir işleç cihaz kümeleri Pano görüntülediğinde, kârlılığı tüm kutucukları görebilir artık bir bakışta tüm aygıtları konumu görselleştirmek için eşleme konumu da dahil olmak üzere yapılandırdığınız!

    ![Pano eşlemeleri işleci görünümü](media/howto-use-device-sets/LocationMaps4.png)

    İstenen boyuta eşlemeye yeniden boyutlandırma kuramaz.




## <a name="configure-the-list-for-your-device-set"></a>Listesi, cihaz kümesi için yapılandırın

Cihaz kümenizi oluşturduktan sonra yapılandırabileceğiniz **listesi**. **Listesi** tüm aygıtları Aygıt ayarlayın, seçtiğiniz sütunları olan bir tabloda gösterir.

1. Seçin **aygıt kümeleri** sol gezinti menüsünde.

1. Seçin **listesi** sekmesi.

1. Seçin **sütun seçenekleri**.

    ![Sütun seçenekleri](media/howto-use-device-sets/image11.png)

1. Göstermek istediğiniz sütun ve sağ ok tuşunu seçerek seçmek için gösterilecek sütunları seçin.

    ![Sütun seçin](media/howto-use-device-sets/image12.png)

1. **Tamam**’ı seçin.

## <a name="analytics"></a>Analiz

Cihaz kümelerinde analytics sol gezinti menüsünde ana analytics sekmesi ile aynıdır. Üzerinde makalede analytics hakkında daha fazla bilgiyi [analytics oluşturma](howto-create-analytics.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanızda aygıt kümelerini kullanma öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Telemetri kuralları oluşturma](howto-create-telemetry-rules.md)
