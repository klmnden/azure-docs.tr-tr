---
title: Azure Data Lake Store Event Hubs verilerini yakalama | Microsoft Docs
description: "Event Hubs verilerini yakalamak için kullanım Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 221ee6990fe0b5bfc9e745fc85543c4e04e41bd3
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="use-azure-data-lake-store-to-capture-data-from-event-hubs"></a>Event Hubs verilerini yakalamak için kullanım Azure Data Lake Store

Azure Event Hubs tarafından alınan verileri yakalamak için Azure Data Lake Store kullanmayı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Bir Azure Data Lake Store hesabı**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md).

*  **Bir olay hub'ları ad alanı**. Yönergeler için bkz: [bir olay hub'ları ad alanı oluşturma](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace). Data Lake Store hesabı ve Event Hubs ad alanı aynı Azure abonelikte olduğundan emin olun.


## <a name="assign-permissions-to-event-hubs"></a>Olay hub'ları için izinleri atayın

Bu bölümde, Event Hubs verilerini yakalamak istediğiniz bir klasörde hesabı oluşturun. Bir Data Lake Store hesabına veri yazabilirsiniz böylece Event Hubs'a de izinleri atayın. 

1. Event Hubs verilerini yakalamak ve tıklayın istediğiniz Data Lake Store hesabını açın **Veri Gezgini**.

    ![Data Lake Store Veri Gezgini](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Data Lake Store Veri Gezgini")

2.  Tıklatın **yeni klasör** ve verileri yakalamak istediğiniz klasörü için bir ad girin.

    ![Data Lake Store içinde yeni bir klasör oluşturun](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "Data Lake Store içinde yeni bir klasör oluşturun")

3. Data Lake Store'un kökünde izinleri atayın. 

    a. Tıklatın **Veri Gezgini**, Data Lake Store hesabı kök seçin ve ardından **erişim**.

    ![Data Lake Store kök izinlerini atamak](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "Data Lake Store kök izinlerini atama")

    b. Altında **erişim**, tıklatın **Ekle**, tıklatın **kullanıcı veya Grup Seç**, arayın ve sonra `Microsoft.EventHubs`. 

    ![Data Lake Store kök izinlerini atamak](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Data Lake Store kök izinlerini atama")
    
    **Seç**'e tıklayın.

    c. Altında **izinleri atamak**, tıklatın **Select izinleri**. Ayarlama **izinleri** için **yürütme**. Ayarlama **eklemek** için **bu klasör ve tüm alt öğeleri**. Ayarlama **olarak eklemek** için **erişim izni girdisi ve varsayılan izin girdisi**.

    ![Data Lake Store kök izinlerini atamak](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "Data Lake Store kök izinlerini atama")

    **Tamam** düğmesine tıklayın.

4. Data Lake Store hesabı altında veri yakalamak istediğiniz klasör izinlerini atayın.

    a. Tıklatın **Veri Gezgini**, Data Lake Store hesabında klasör seçin ve ardından **erişim**.

    ![Data Lake Store klasör izinlerini atamak](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "Data Lake Store klasörün izinlerini atama")

    b. Altında **erişim**, tıklatın **Ekle**, tıklatın **kullanıcı veya Grup Seç**, arayın ve sonra `Microsoft.EventHubs`. 

    ![Data Lake Store klasör izinlerini atamak](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Data Lake Store klasörün izinlerini atama")
    
    **Seç**'e tıklayın.

    c. Altında **izinleri atamak**, tıklatın **Select izinleri**. Ayarlama **izinleri** için **okuma, yazma,** ve **yürütme**. Ayarlama **eklemek** için **bu klasör ve tüm alt öğeleri**. Son olarak, ayarlamak **olarak eklemek** için **erişim izni girdisi ve varsayılan izin girdisi**.

    ![Data Lake Store klasör izinlerini atamak](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "Data Lake Store klasörün izinlerini atama")
    
    **Tamam** düğmesine tıklayın. 

## <a name="configure-event-hubs-to-capture-data-to-data-lake-store"></a>Olay hub'ı Data Lake Store'a veri yakalamak için yapılandırma

Bu bölümde, bir olay hub'ları ad alanı içindeki bir Event Hub oluşturun. Bir Azure Data Lake Store hesabına veri yakalamak için olay hub'ı yapılandırmanız da. Bu bölümde, bir olay hub'ları ad alanı zaten oluşturduğunuzu varsayar.

2. Gelen **genel bakış** bölmesi olay hub'ları ad **+ olay hub'ı**.

    ![Olay hub'ı oluşturma](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "olay hub'ı Oluştur")

3. Olay hub'ı Data Lake Store'a veri yakalamak için yapılandırmak için aşağıdaki değerleri girin.

    ![Olay hub'ı oluşturma](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "olay hub'ı Oluştur")

    a. Olay hub'ı için bir ad sağlayın.
    
    b. Bu öğretici için ayarlanmış **bölüm sayısı** ve **ileti bekletme** varsayılan değerlere.
    
    c. Ayarlama **yakalama** için **üzerinde**. Ayarlama **zaman penceresi** (sık yakalamak nasıl) ve **boyutu penceresi** (yakalamak için veri boyutu). 
    
    d. İçin **yakalama sağlayıcısı**seçin **Azure Data Lake Store** ve daha önce oluşturduğunuz Data Lake Store seçin. İçin **Data Lake yolu**, Data Lake Store hesabında oluşturduğunuz klasör adını girin. Yalnızca klasöre göreli yol sağlamanız gerekir.

    e. Bırakın **örnek yakalama dosyası adı biçimlerini** varsayılan değere. Bu seçenek yakalama klasörü altında oluşturulan klasör yapısını yönetir.

    f. **Oluştur**'a tıklayın.

## <a name="test-the-setup"></a>Test Kurulumu

Artık Azure Event Hub'ına veri göndererek çözümü test edebilirsiniz. Bölümündeki yönergeleri izleyin [olayları Azure Event Hubs'a gönderme](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md). Veri göndermeye başla sonra belirttiğiniz klasör yapısını kullanarak Data Lake Store'da yansıtılan veri bakın. Örneğin, bir klasör yapısı, Data Lake Store'da aşağıdaki ekran görüntüsünde gösterildiği gibi görürsünüz.

![Data Lake Store'da EventHub veri örneği](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "Data Lake Store'da örnek EventHub veri")

> [!NOTE]
> Event Hubs'a gelen iletileri yoksa bile, olay hub'ları başlıklarını boş dosyalarıyla Data Lake Store hesabına veri yazar. Dosyalar, olay hub'ları oluştururken sağladığınız aynı zaman aralığında yazılır.
> 
>

## <a name="analyze-data-in-data-lake-store"></a>Data Lake Store'da verileri analiz etme

Data Lake Store'da verileri olduktan sonra analitik işleri işlemi ve crunch veri çalıştırabilirsiniz. Bkz: [USQL Avro örnek](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) kullanarak Azure Data Lake Analytics bunu konusunda.
  

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Storage Bloblarından Data Lake Store'a veri kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
