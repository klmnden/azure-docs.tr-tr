---
title: Stream Analytics verileri Azure Data Lake depolama Gen1 Stream | Microsoft Docs
description: Azure Stream Analytics akışı verilerini Azure Data Lake depolama Gen1 kullanın.
services: data-lake-store,stream-analytics
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: edb58e0b-311f-44b0-a499-04d7e6c07a90
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: twooley
ms.openlocfilehash: d3dbacd58b3bda3fbf8ee8ad5f175eccc2cb2a24
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60194952"
---
# <a name="stream-data-from-azure-storage-blob-into-azure-data-lake-storage-gen1-using-azure-stream-analytics"></a>Azure depolama blobundan Azure Stream Analytics'i kullanarak Azure Data Lake depolama Gen1 Stream veri
Bu makalede Azure Data Lake depolama Gen1 Azure Stream Analytics işi için çıkış olarak kullanmak öğreneceksiniz. Bu makalede, Azure Storage blobundan (giriş) verileri okur ve verileri için Data Lake depolama Gen1 yazan basit bir senaryo gösterilmektedir (çıktı).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Depolama hesabı**. Stream Analytics işine ilişkin veri girişi için bir blob kapsayıcısından bu hesabı kullanır. Bu öğreticide, adlı bir depolama hesabına sahip olduğunuz varsayılır **storageforasa** hesabında bir kapsayıcı adı verilen ve **storageforasacontainer**. Kapsayıcıyı oluşturduktan sonra örnek veri dosyası için karşıya yükleyin. 
  
* **Bir Data Lake depolama Gen1 hesabı**. Konumundaki yönergeleri [Azure Data Lake depolama Gen1 ile çalışmaya başlama Azure portalını kullanarak](data-lake-store-get-started-portal.md). Adlı bir Data Lake depolama Gen1 hesabı sahip **myadlsg1**. 

## <a name="create-a-stream-analytics-job"></a>Stream Analytics işi oluşturma
Bir giriş kaynağı ve bir çıkış hedefini içeren bir Stream Analytics işi oluşturun. Bu öğretici için kaynak Azure blob kapsayıcısı ve hedef Data Lake depolama Gen1.

1. [Azure Portal](https://portal.azure.com)'da oturum açın.

2. Sol bölmeden tıklayın **Stream Analytics işleri**ve ardından **Ekle**.

    ![Stream Analytics işi oluşturma](./media/data-lake-store-stream-analytics/create.job.png "bir Stream Analytics işi oluşturma")

    > [!NOTE]
    > Depolama hesabı ile aynı bölgede iş oluşturma veya bölgeler arasında veri taşıma ek ücret ödenmesini emin olun.
    >

## <a name="create-a-blob-input-for-the-job"></a>İş için bir Blob girdi oluşturma

1. Sayfanın sol bölmeden bir Stream Analytics işi için Aç'ı **girişleri** sekmesine ve ardından **Ekle**.

    ![İşinize bir girdi Ekle](./media/data-lake-store-stream-analytics/create.input.1.png "işinize bir girdi Ekle")

2. Üzerinde **yeni giriş** dikey penceresinde aşağıdaki değerleri sağlayın.

    ![İşinize bir girdi Ekle](./media/data-lake-store-stream-analytics/create.input.2.png "işinize bir girdi Ekle")

   * İçin **giriş diğer adı**, giriş işlemi için benzersiz bir ad girin.
   * İçin **kaynak türünü**seçin **veri akışı**.
   * İçin **kaynak**seçin **Blob Depolama**.
   * İçin **abonelik**seçin **geçerli abonelik kullanım blob depolamadan**.
   * İçin **depolama hesabı**, önkoşulların bir parçası oluşturduğunuz depolama hesabını seçin. 
   * İçin **kapsayıcı**, seçilen depolama hesabında oluşturulan kapsayıcıyı seçin.
   * İçin **olay serileştirme biçimi**seçin **CSV**.
   * İçin **sınırlayıcı**seçin **sekmesini**.
   * İçin **kodlama**seçin **UTF-8**.

     **Oluştur**’a tıklayın. Portal şimdi giriş ekler ve onu bağlantısını test eder.


## <a name="create-a-data-lake-storage-gen1-output-for-the-job"></a>İş için bir Data Lake depolama Gen1 çıkışı oluşturma

1. Stream Analytics işine ilişkin sayfayı Aç'a tıklayın **çıkışları** sekmesinde **Ekle**seçip **Data Lake depolama Gen1**.

    ![İşinize bir çıktı eklemek](./media/data-lake-store-stream-analytics/create.output.1.png "işinize bir çıktı ekleyin")

2. Üzerinde **yeni çıkış** dikey penceresinde aşağıdaki değerleri sağlayın.

    ![İşinize bir çıktı eklemek](./media/data-lake-store-stream-analytics/create.output.2.png "işinize bir çıktı ekleyin")

    * İçin **çıkış diğer adı**, iş çıktısı için benzersiz bir ad girin. Bu sorgular, sorgu çıktısı bu Data Lake depolama Gen1 hesabına yönlendirmek için kullanılan kolay bir addır.
    * Data Lake depolama Gen1 hesap erişim yetkisi vermek için istenir. Tıklayın **yetkilendirmek**.

3. Üzerinde **yeni çıkış** dikey penceresinde aşağıdaki değerleri sağlayın geçin.

    ![İşinize bir çıktı eklemek](./media/data-lake-store-stream-analytics/create.output.3.png "işinize bir çıktı ekleyin")

   * İçin **hesap adı**, zaten oluşturduğunuz projenin çıkış gönderilmesi için istediğiniz Data Lake depolama Gen1 hesabını seçin.
   * İçin **yol ön eki deseni**, belirtilen Data Lake depolama Gen1 hesap dosyalarınızı yazmak için kullanılan bir dosya yolu girin.
   * İçin **tarih biçimi**, ön eki yolunda bir tarih belirteci kullandıysanız, dosyalarınızı düzenlenir tarih biçimi seçin.
   * İçin **saat biçimi**, ön eki yolunda bir zaman belirteç kullandıysanız dosyalarınızı düzenlenir saat biçimini belirtin.
   * İçin **olay serileştirme biçimi**seçin **CSV**.
   * İçin **sınırlayıcı**seçin **sekmesini**.
   * İçin **kodlama**seçin **UTF-8**.
    
     **Oluştur**’a tıklayın. Portal şimdi çıkış ekler ve onu bağlantısını test eder.
    
## <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

1. Stream Analytics işini çalıştırmak için bir sorgu çalıştırın **sorgu** sekmesi. Bu öğreticide, giriş işlemiyle yer tutucuları değiştirerek örnek sorgu çalıştırabilirsiniz ve aşağıdaki ekran görüntüsünde gösterildiği gibi diğer adlar, çıktı.

    ![Sorguyu çalıştırmak](./media/data-lake-store-stream-analytics/run.query.png "sorgu çalıştırma")

2. Tıklayın **Kaydet** ekran ve ardından gelen ilk **genel bakış** sekmesinde **Başlat**. İletişim kutusundan **özel zaman**ve geçerli tarih ve saat ayarlayın.

    ![İş saati ayarlayın](./media/data-lake-store-stream-analytics/run.query.2.png "iş saati ayarlayın")

    Tıklayın **Başlat** işi başlatmak için. Bu birkaç dakika işi başlatmak için kadar sürebilir.

3. Blob verileri seçmek için bir iş tetiklemek için blob kapsayıcısına örnek veri dosyasını kopyalayın. Bir örnek verileri dosyadan alabilirsiniz [Azure Data Lake Git deposu](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Bu öğreticide, github'dan dosyasını kopyalayın **vehicle1_09142014.csv**. Çeşitli istemciler gibi kullanabileceğiniz [Azure Depolama Gezgini](https://storageexplorer.com/), veriler bir blob kapsayıcısına yükleyin.

4. Gelen **genel bakış** sekmesindeki **izleme**, verilerin nasıl işlendiği bakın.

    ![İşi izleme](./media/data-lake-store-stream-analytics/run.query.3.png "işi izleme")

5. Son olarak, proje çıktı verilerini Data Lake depolama Gen1 hesabında kullanılabilir olduğunu doğrulayabilirsiniz. 

    ![Çıkış doğrulayın](./media/data-lake-store-stream-analytics/run.query.4.png "çıkış doğrulayın")

    Veri Gezgini bölmesinde Data Lake depolama Gen1 belirtildiği gibi bir klasör yolu için çıktı yazılır olduğuna dikkat edin. çıktı ayarları (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake depolama Gen1 kullanılacak bir HDInsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md)
