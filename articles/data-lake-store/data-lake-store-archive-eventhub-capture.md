---
title: Azure Data Lake depolama Gen1 Event Hubs verilerini yakalama | Microsoft Docs
description: Event Hubs verilerini yakalama için Azure Data Lake depolama Gen1 kullanın
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: bb67c1769510710b368bef4dc0b501f939b3427e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58884524"
---
# <a name="use-azure-data-lake-storage-gen1-to-capture-data-from-event-hubs"></a>Event Hubs verilerini yakalama için Azure Data Lake depolama Gen1 kullanın

Azure Event hubs'ı tarafından alınan verileri yakalamak için Azure Data Lake depolama Gen1 kullanmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Bir Azure Data Lake depolama Gen1 hesap**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake depolama Gen1 ile çalışmaya başlama](data-lake-store-get-started-portal.md).

*  **Bir Event Hubs ad alanı**. Yönergeler için [bir Event Hubs ad alanı oluşturma](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace). Data Lake depolama Gen1 hesabı ve Event Hubs ad alanı aynı Azure aboneliğinde olduğundan emin olun.


## <a name="assign-permissions-to-event-hubs"></a>Event Hubs'a izin atama

Bu bölümde, Event Hubs verilerini yakalama için istediğiniz bir klasörü hesap içinde oluşturun. Bir Data Lake depolama Gen1 hesaba veri yazabilmesi için Event Hubs ayrıca izinleri atayın. 

1. Event Hubs verilerini yakalama ve ardından istediğiniz Data Lake depolama Gen1 hesabı açın **Veri Gezgini**.

    ![Data Lake depolama Gen1 Veri Gezgini](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Data Lake depolama Gen1 Veri Gezgini")

1.  Tıklayın **yeni klasör** ve ardından verileri yakalamak istediğiniz klasörü için bir ad girin.

    ![Yeni bir klasör oluşturmak Data Lake depolama Gen1](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "Data Lake depolama Gen1 içinde yeni bir klasör oluşturun")

1. Data Lake depolama Gen1 kökünde izinleri atayın. 

    a. Tıklayın **Veri Gezgini**, Data Lake depolama Gen1 hesabının kök seçin ve ardından **erişim**.

    ![Data Lake depolama Gen1 kök izinleri atamanız](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "Data Lake depolama Gen1 kök için izin atama")

    b. Altında **erişim**, tıklayın **Ekle**, tıklayın **kullanıcı veya Grup Seç**ve ardından arama `Microsoft.EventHubs`. 

    ![Data Lake depolama Gen1 kök izinleri atamanız](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Data Lake depolama Gen1 kök için izin atama")
    
    **Seç**'e tıklayın.

    c. Altında **atama izinleri**, tıklayın **Select izinleri**. Ayarlama **izinleri** için **yürütme**. Ayarlama **eklemek** için **bu klasör ve tüm alt öğeleri**. Ayarlama **eklemek** için **erişim izni girdisi ve varsayılan izin girdisi**.

    > [!IMPORTANT]
    > Azure Event hubs'ı tarafından alınan verileri yakalamak için yeni bir klasör hiyerarşisi oluşturma, hedef klasöre erişim sağlamak için kolay bir yolu budur.  Ancak, çok sayıda alt dosyalar ve klasörlerle en üst düzey klasöre tüm alt öğelere izinler eklemek uzun sürebilir.  Kök klasörünüzde çok sayıda dosya ve klasörleri varsa, bunu daha hızlı bir şekilde eklemek için olabilir **yürütme** izinlerini `Microsoft.EventHubs` ayrı olarak her bir klasörü, nihai hedef klasörün yolu. 

    ![Data Lake depolama Gen1 kök izinleri atamanız](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "Data Lake depolama Gen1 kök için izin atama")

    **Tamam** düğmesine tıklayın.

1. Veri yakalamak istediğiniz Data Lake depolama Gen1 hesabı altındaki klasör için izinleri atayın.

    a. Tıklayın **Veri Gezgini**Data Lake depolama Gen1 hesabında klasörü seçin ve ardından **erişim**.

    ![Data Lake depolama Gen1 klasör izinlerini atamak](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "Data Lake depolama Gen1 klasörü için izin atama")

    b. Altında **erişim**, tıklayın **Ekle**, tıklayın **kullanıcı veya Grup Seç**ve ardından arama `Microsoft.EventHubs`. 

    ![Data Lake depolama Gen1 klasör izinlerini atamak](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Data Lake depolama Gen1 klasörü için izin atama")
    
    **Seç**'e tıklayın.

    c. Altında **atama izinleri**, tıklayın **Select izinleri**. Ayarlama **izinleri** için **okuma, yazma,** ve **yürütme**. Ayarlama **eklemek** için **bu klasör ve tüm alt öğeleri**. Son olarak, ayarlama **eklemek** için **erişim izni girdisi ve varsayılan izin girdisi**.

    ![Data Lake depolama Gen1 klasör izinlerini atamak](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "Data Lake depolama Gen1 klasörü için izin atama")
    
    **Tamam** düğmesine tıklayın. 

## <a name="configure-event-hubs-to-capture-data-to-data-lake-storage-gen1"></a>Event Hubs'ı Data Lake depolama Gen1 verileri yakalamak için yapılandırma

Bu bölümde, bir olay hub'ı bir Event Hubs ad alanı içinde oluşturun. Ayrıca, bir Azure Data Lake depolama Gen1 hesabına verilerini yakalamak için olay hub'ı de yapılandırın. Bu bölüm bir Event Hubs ad alanı zaten oluşturmuş olduğunuzu varsayar.

1. Gelen **genel bakış** Event Hubs ad alanının bölmesi **+ olay hub'ı**.

    ![Olay hub'ı oluşturma](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "olay hub'ı oluşturma")

1. Event Hubs'ı Data Lake depolama Gen1 verileri yakalamak üzere yapılandırmak için aşağıdaki değerleri sağlayın.

    ![Olay hub'ı oluşturma](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "olay hub'ı oluşturma")

    a. Olay hub'ı için bir ad belirtin.
    
    b. Bu öğretici için kümesine **bölüm sayısı** ve **ileti bekletme** için varsayılan değerleri.
    
    c. Ayarlama **yakalama** için **üzerinde**. Ayarlama **zaman penceresi** (sık yakalama nasıl) ve **boyutu penceresi** (yakalamak için veri boyutu). 
    
    d. İçin **yakalama sağlayıcısı**seçin **Azure Data Lake Store** ve ardından daha önce oluşturduğunuz Data Lake depolama Gen1 hesabını seçin. İçin **Data Lake yolu**, Data Lake depolama Gen1 hesap içinde oluşturduğunuz klasörün adını girin. Klasörün göreli yolunu sağlamak yeterlidir.

    e. Bırakın **örnek yakalama dosya adı biçimleri** için varsayılan değer. Bu seçenek, yakalama klasör altında oluşturulan klasör yapısını yönetir.

    f. **Oluştur**’a tıklayın.

## <a name="test-the-setup"></a>Test Kurulumu

Artık Azure olay Hub'ına veri gönderen tarafından çözümü test edebilirsiniz. Konumundaki yönergeleri [olayları Azure Event Hubs'a gönderme](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md). Veri göndermeye başlamak sonra verileri görüntülemek Data Lake depolama Gen1 yansıtılan klasörü kullanılarak yapı belirttiğiniz. Örneğin, Data Lake depolama Gen1 hesabınızda aşağıdaki ekran görüntüsünde gösterildiği gibi bir klasör yapısı görürsünüz.

![Data Lake depolama Gen1 EventHub veri örnekleme](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "Data Lake depolama Gen1 örnek EventHub veri")

> [!NOTE]
> Event Hubs'a gelen iletileri yoksa bile, Event Hubs Data Lake depolama Gen1 hesaba boş dosyalarını yalnızca üst bilgilerini yazar. Dosyalar, aynı zaman aralığında olay hub'ları oluştururken sağladığınız yazılır.
> 
>

## <a name="analyze-data-in-data-lake-storage-gen1"></a>Data Lake depolama Gen1 verileri analiz etme

Veriler Data Lake depolama Gen1 hale geldikten sonra analiz işleri işlem ve işleme için veri çalıştırabilirsiniz. Bkz: [USQL Avro örnek](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) Azure Data Lake Analytics'i kullanarak bunu nasıl.
  

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Data Lake depolama Gen1 için Azure depolama Bloblarından veri kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
