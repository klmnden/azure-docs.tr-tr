---
title: Azure Data Lake Store ile Event Hubs verilerini yakalama | Microsoft Docs
description: Event Hubs verilerini yakalama için kullanım Azure Data Lake Store
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: bda52acc12aad3cad20143c319f557f11d760c42
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435161"
---
# <a name="use-azure-data-lake-store-to-capture-data-from-event-hubs"></a>Event Hubs verilerini yakalama için kullanım Azure Data Lake Store

Azure Data Lake Store, Azure Event hubs'ı tarafından alınan verileri yakalamak için kullanmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Bir Azure Data Lake Store hesabı**. Hesap oluşturmaya ilişkin yönergeler için bkz. [Azure Data Lake Store kullanmaya başlama](data-lake-store-get-started-portal.md).

*  **Bir Event Hubs ad alanı**. Yönergeler için [bir Event Hubs ad alanı oluşturma](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace). Data Lake Store hesabı ve Event Hubs ad alanı aynı Azure aboneliğinde olduğundan emin olun.


## <a name="assign-permissions-to-event-hubs"></a>Event Hubs'a izin atama

Bu bölümde, Event Hubs verilerini yakalama için istediğiniz bir klasörü hesap içinde oluşturun. Bir Data Lake Store hesabına veri yazabilmesi için Event Hubs ayrıca izinleri atayın. 

1. Event Hubs verilerini yakalama ve ardından istediğiniz Data Lake Store hesabını açın **Veri Gezgini**.

    ![Data Lake Store Veri Gezgini](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Data Lake Store Veri Gezgini")

1.  Tıklayın **yeni klasör** ve ardından verileri yakalamak istediğiniz klasörü için bir ad girin.

    ![Data Lake Store içinde yeni bir klasör oluşturun](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "Data Lake Store içinde yeni bir klasör oluşturun")

1. Data Lake Store kökünde izinleri atayın. 

    a. Tıklayın **Veri Gezgini**, Data Lake Store hesabının kök seçin ve ardından **erişim**.

    ![Data Lake Store kök izinleri atamanız](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "Data Lake Store kök için izin atama")

    b. Altında **erişim**, tıklayın **Ekle**, tıklayın **kullanıcı veya Grup Seç**ve ardından arama `Microsoft.EventHubs`. 

    ![Data Lake Store kök izinleri atamanız](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Data Lake Store kök için izin atama")
    
    **Seç**'e tıklayın.

    c. Altında **atama izinleri**, tıklayın **Select izinleri**. Ayarlama **izinleri** için **yürütme**. Ayarlama **eklemek** için **bu klasör ve tüm alt öğeleri**. Ayarlama **eklemek** için **erişim izni girdisi ve varsayılan izin girdisi**.

    > [!IMPORTANT]
    > Azure Event hubs'ı tarafından alınan verileri yakalamak için yeni bir klasör hiyerarşisi oluştururken, hedef klasöre erişim sağlamak için kolay bir yol budur.  Ancak, çok sayıda alt dosyalar ve klasörlerle en üst düzey klasöre tüm alt öğelere izinler eklemek uzun sürebilir.  Kök klasörünüzde çok sayıda dosya ve klasörleri varsa, bunu daha hızlı bir şekilde eklemek için olabilir **yürütme** izinlerini `Microsoft.EventHubs` ayrı olarak her bir klasörü, nihai hedef klasörün yolu. 

    ![Data Lake Store kök izinleri atamanız](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "Data Lake Store kök için izin atama")

    **Tamam** düğmesine tıklayın.

1. Veri yakalamak istediğiniz Data Lake Store hesabı altındaki klasör için izinleri atayın.

    a. Tıklayın **Veri Gezgini**Data Lake Store hesabında klasör seçin ve ardından **erişim**.

    ![Data Lake Store klasör izinlerini atamak](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "Data Lake Store klasör için izin atama")

    b. Altında **erişim**, tıklayın **Ekle**, tıklayın **kullanıcı veya Grup Seç**ve ardından arama `Microsoft.EventHubs`. 

    ![Data Lake Store klasör izinlerini atamak](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Data Lake Store klasör için izin atama")
    
    **Seç**'e tıklayın.

    c. Altında **atama izinleri**, tıklayın **Select izinleri**. Ayarlama **izinleri** için **okuma, yazma,** ve **yürütme**. Ayarlama **eklemek** için **bu klasör ve tüm alt öğeleri**. Son olarak, ayarlama **eklemek** için **erişim izni girdisi ve varsayılan izin girdisi**.

    ![Data Lake Store klasör izinlerini atamak](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "Data Lake Store klasör için izin atama")
    
    **Tamam** düğmesine tıklayın. 

## <a name="configure-event-hubs-to-capture-data-to-data-lake-store"></a>Event Hubs'ı Data Lake Store verilerini yakalamak için yapılandırma

Bu bölümde, bir olay hub'ı bir Event Hubs ad alanı içinde oluşturun. Ayrıca, bir Azure Data Lake Store hesabına veri yakalamak için olay hub'ı de yapılandırın. Bu bölüm bir Event Hubs ad alanı zaten oluşturmuş olduğunuzu varsayar.

1. Gelen **genel bakış** Event Hubs ad alanının bölmesi **+ olay hub'ı**.

    ![Olay hub'ı oluşturma](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "olay hub'ı oluşturma")

1. Event Hubs'ı Data Lake Store verilerini yakalamak için yapılandırmak için aşağıdaki değerleri sağlayın.

    ![Olay hub'ı oluşturma](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "olay hub'ı oluşturma")

    a. Olay hub'ı için bir ad belirtin.
    
    b. Bu öğretici için kümesine **bölüm sayısı** ve **ileti bekletme** için varsayılan değerleri.
    
    c. Ayarlama **yakalama** için **üzerinde**. Ayarlama **zaman penceresi** (sık yakalama nasıl) ve **boyutu penceresi** (yakalamak için veri boyutu). 
    
    d. İçin **yakalama sağlayıcısı**seçin **Azure Data Lake Store** ve daha önce oluşturduğunuz Data Lake Store seçin. İçin **Data Lake yolu**, oluşturduğunuz Data Lake Store hesabında klasör adını girin. Klasörün göreli yolunu sağlamak yeterlidir.

    e. Bırakın **örnek yakalama dosya adı biçimleri** için varsayılan değer. Bu seçenek, yakalama klasör altında oluşturulan klasör yapısını yönetir.

    f. **Oluştur**’a tıklayın.

## <a name="test-the-setup"></a>Test Kurulumu

Artık Azure olay Hub'ına veri gönderen tarafından çözümü test edebilirsiniz. Konumundaki yönergeleri [olayları Azure Event Hubs'a gönderme](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md). Veri göndermeye başlamak sonra belirttiğiniz klasör yapısını kullanarak Data Lake Store içinde gösterilen verileri görürsünüz. Örneğin, Data Lake Store, aşağıdaki ekran görüntüsünde gösterildiği gibi bir klasör yapısı görürsünüz.

![Data Lake Store EventHub veri örnekleme](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "örnek EventHub verileri Data Lake Store")

> [!NOTE]
> Event Hubs'a gelen iletileri yoksa bile, Event Hubs Data Lake Store hesabına boş dosyalarını yalnızca üst bilgilerini yazar. Dosyalar, aynı zaman aralığında olay hub'ları oluştururken sağladığınız yazılır.
> 
>

## <a name="analyze-data-in-data-lake-store"></a>Data Lake Store verileri analiz etme

Verileri Data Lake Store içinde hale geldikten sonra analiz işleri işlem ve işleme için veri çalıştırabilirsiniz. Bkz: [USQL Avro örnek](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) Azure Data Lake Analytics'i kullanarak bunu nasıl.
  

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Data Lake Store için Azure depolama Bloblarından veri kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
