---
title: Azure portalını kullanarak Stream Analytics işi oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta bir Stream Analytic işi oluşturma, girdileri ve çıktıları yapılandırma ve bir sorgu tanımlama yoluyla çalışmaya nasıl başlayacağınız gösterilmektedir.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 06/21/2019
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
ms.openlocfilehash: e05d293760b88cd02fdffae60e762f040a4d1311
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449226"
---
# <a name="quickstart-create-a-stream-analytics-job-by-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma

Bu hızlı başlangıçta bir Stream Analytics işini oluşturmaya nasıl başlanacağı gösterilmektedir. Bu hızlı başlangıçta, gerçek zamanlı akış verileri ve bir sıcaklık 27 ' büyük olan filtreleri iletileri okuyan bir Stream Analytics işi tanımlayın. Stream Analytics işinizi IOT Hub'ından veri okuma, verileri dönüştürme ve verileri blob depolamadaki bir kapsayıcıda geri yazma. Bu hızlı başlangıçta kullanılan giriş veri Raspberry Pi çevrimiçi simülatör tarafından oluşturulur. 

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

* [Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="prepare-the-input-data"></a>Girdi verilerini hazırlama

Stream Analytics işini tanımlamadan önce giriş verileri hazırlamanız gerekir. Gerçek zamanlı sensör verilerini, daha sonra iş giriş olarak yapılandırılmış IOT Hub'ına alınır. İş için gereken girdi verilerini hazırlamak için aşağıdaki adımları tamamlayın:

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. **Kaynak oluştur** > **Nesnelerin İnterneti** > **Iot Hub** seçeneğini belirleyin.

3. İçinde **IOT hub'ı** bölmesinde aşağıdaki bilgileri girin:
   
   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Abonelik  | \<Aboneliğiniz\> |  Kullanmak istediğiniz Azure aboneliğini seçin. |
   |Kaynak grubu   |   asaquickstart-resourcegroup  |   **Yeni Oluştur**’u seçin ve hesabınız için yeni bir kaynak grubu adı girin. |
   |Bölge  |  \<Kullanıcılarınıza en yakın bölgeyi seçin\> | Burada, IOT Hub'ınıza barındırabilirsiniz coğrafi bir konum seçin. Kullanıcılarınıza en yakın konumu kullanın. |
   |IOT hub'ı adı  | MyASAIoTHub  |   IOT Hub'ınız için bir ad seçin.   |

   ![IoT Hub'ı oluşturma](./media/stream-analytics-quick-create-portal/create-iot-hub.png)

4. Seçin **sonraki: Boyut ve ölçek kümesi**.

5. **Fiyatlandırma ve ölçek katmanınızı** seçin. Bu hızlı başlangıçta seçin **F1 - ücretsiz** aboneliğinizde hala kullanılabilir durumdaysa katmanı. Daha fazla bilgi için [IOT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/).

   ![Boyut ve IOT Hub'ınıza ölçeklendirin](./media/stream-analytics-quick-create-portal/iot-hub-size-and-scale.png)

6. **İncele ve oluştur**’u seçin. IOT Hub bilgilerinizi gözden geçirin ve tıklayın **Oluştur**. IOT Hub'ınızın oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

7. IOT hub'ı Gezinti menüsüne **Ekle** altında **IOT cihazları**. Ekleme bir **cihaz kimliği** tıklatıp **Kaydet**.

   ![IOT Hub'ınıza bir cihaz ekleyin](./media/stream-analytics-quick-create-portal/add-device-iot-hub.png)

8. Cihaz oluşturulduktan sonra CİHAZDAN açın **IOT cihazları** listesi. Kopyalama **bağlantı dizesi – birincil anahtar** ve daha sonra kullanmak üzere Not Defteri'ne kaydedin.

   ![IOT Hub cihaz bağlantı dizesini kopyalayın](./media/stream-analytics-quick-create-portal/save-iot-device-connection-string.png)

## <a name="create-blob-storage"></a>BLOB Depolama oluşturma

1. Azure portalının sol üst köşesinden **Kaynak oluştur** > **Depolama** > **Depolama hesabı**’nı seçin.

2. İçinde **depolama hesabı oluşturma** bölmesinde, bir depolama hesabı adı, konum ve kaynak grubu girin. Aynı konum ve kaynak grubu oluşturduğunuz IOT Hub'ı seçin. Ardından **gözden geçir + Oluştur** hesabı oluşturmak için.

   ![Depolama hesabı oluştur](./media/stream-analytics-quick-create-portal/create-storage-account.png)

3. Depolama hesabınız oluşturulduktan sonra seçin **Blobları** kutucuğundan **genel bakış** bölmesi.

   ![Depolama hesabına genel bakış](./media/stream-analytics-quick-create-portal/blob-storage.png)

4. Gelen **Blob hizmeti** sayfasında **kapsayıcı** ve gibi kapsayıcınız için bir ad verin *kapsayıcı1*. Bırakın **genel erişim düzeyi** olarak **özel (anonim erişim yok)** seçip **Tamam**.

   ![Blob kapsayıcısı oluşturma](./media/stream-analytics-quick-create-portal/create-blob-container.png)

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. Azure Portal’da oturum açın.

2. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.  

3. Seçin **Analytics** > **Stream Analytics işi** sonuçları listesinde.  

4. Stream Analytics işi sayfasını aşağıdaki bilgilerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |İş adı   |  MyASAJob   |   Stream Analytics işinizi tanımlamak için bir ad girin. Stream Analytics işinin adı yalnızca alfasayısal karakter, kısa çizgi ve alt çizgi içerebilir ve 3 ila 63 karakter uzunluğunda olmalıdır. |
   |Abonelik  | \<Aboneliğiniz\> |  Bu iş için kullanmak istediğiniz Azure aboneliğini seçin. |
   |Kaynak grubu   |   asaquickstart-resourcegroup  |   Aynı kaynak grubu IOT Hub'ınızı seçin. |
   |Location  |  \<Kullanıcılarınıza en yakın bölgeyi seçin\> | Stream Analytics işinizi barındırabileceğiniz coğrafi konumu seçin. Daha iyi performans elde etmek ve veri aktarımı maliyetini azaltmak için kullanıcılarınıza en yakın konumu seçin. |
   |Akış birimleri  | 1  |   Akış birimleri, bir işin yürütülmesi için gereken bilgi işlem kaynaklarını temsil eder. Varsayılan olarak, bu değer 1 olarak ayarlanır. Akış birimlerini ölçeklendirme hakkında bilgi edinmek için [akış birimlerini anlama ve ayarlama](stream-analytics-streaming-unit-consumption.md) başlıklı makaleye bakın.   |
   |Barındırma ortamı  |  Bulut  |   Stream Analytics işleri buluta veya uca dağıtılabilir. Bulut Azure Bulutuna dağıtmanıza olanak tanır ve uç IOT Edge cihazına dağıtmak sağlar. |

   ![İş oluştur](./media/stream-analytics-quick-create-portal/create-asa-job.png)

5. **Panoya sabitle** kutusunu işaretleyerek işinizi panonuza yerleştirin ve sonra **Oluştur**’u seçin.  

6. Görmelisiniz bir *dağıtım sürüyor...*  üst görüntülenen bildirim tarayıcı pencerenizin sağ. 

## <a name="configure-job-input"></a>İş girişi yapılandırma

Bu bölümde, IOT Hub cihaz giriş Stream Analytics işinin yapılandıracaksınız. Hızlı Başlangıç önceki bölümde oluşturduğunuz IOT Hub'ı kullanın.

1. Stream Analytics işinize gidin.  

2. Seçin **girişleri** > **Stream giriş Ekle** > **IOT hub'ı**.  

3. Doldurun **IOT hub'ı** aşağıdaki değerlerle sayfası:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Girdi diğer adı  |  IoTHubInput   |  İşin girdisini tanımlamak için bir ad girin.   |
   |Abonelik   |  \<Aboneliğiniz\> |  Oluşturduğunuz depolama hesabını içeren Azure aboneliğini seçin. Depolama hesabı, aynı veya farklı bir abonelikte olabilir. Bu örnekte, aynı abonelikte depolama hesabı oluşturduğunuz varsayılır. |
   |IoT Hub  |  MyASAIoTHub |  Önceki bölümde oluşturduğunuz IOT Hub'ın adını girin. |

4. Diğer seçenekleri varsayılan değerlerinde bırakın ve ayarları kaydetmek **Kaydet**’i seçin.  

   ![Girdi verilerini yapılandırma](./media/stream-analytics-quick-create-portal/configure-asa-input.png)
 
## <a name="configure-job-output"></a>İş çıkışını yapılandırma

1. Daha önce oluşturduğunuz Stream Analytics işine gidin.  

2. Seçin **çıkışları** > **Ekle** > **Blob Depolama**.  

3. **Blob depolama** sayfasını aşağıdaki değerlerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Çıktı diğer adı |   BlobOutput   |   İşin çıktısını tanımlamak için bir ad girin. |
   |Abonelik  |  \<Aboneliğiniz\>  |  Oluşturduğunuz depolama hesabını içeren Azure aboneliğini seçin. Depolama hesabı, aynı veya farklı bir abonelikte olabilir. Bu örnekte, aynı abonelikte depolama hesabı oluşturduğunuz varsayılır. |
   |Depolama hesabı |  asaquickstartstorage |   Depolama hesabının adını seçin veya girin. Depolama hesabı adları aynı abonelikte oluşturulursa otomatik olarak algılanır.       |
   |Kapsayıcı |   kapsayıcı1  |  Depolama hesabınızda oluşturduğunuz mevcut kapsayıcıyı seçin.   |

4. Diğer seçenekleri varsayılan değerlerinde bırakın ve ayarları kaydetmek **Kaydet**’i seçin.  

   ![Çıktı yapılandırma](./media/stream-analytics-quick-create-portal/configure-asa-output.png)
 
## <a name="define-the-transformation-query"></a>Dönüşüm sorgusunu tanımlama

1. Daha önce oluşturduğunuz Stream Analytics işine gidin.  

2. **Sorgu**’yu seçin ve sorguyu aşağıdaki gibi güncelleştirin:  

   ```sql
   SELECT *
   INTO BlobOutput
   FROM IoTHubInput
   HAVING Temperature > 27
   ```

3. Bu örnekte sorgu, IOT Hub'ından veri okuyan ve blob'daki yeni bir dosyaya kopyalar. **Kaydet**’i seçin.  

   ![İş dönüşümü yapılandırma](./media/stream-analytics-quick-create-portal/add-asa-query.png)

## <a name="run-the-iot-simulator"></a>IOT simülatör'ü çalıştırma

1. Açık [Raspberry Pi Azure IOT çevrimiçi simülatör](https://azure-samples.github.io/raspberry-pi-web-simulator/).

2. Satırı 15 yer tutucuyu, önceki bölümde kaydettiğiniz Azure IOT Hub cihaz bağlantı dizesiyle değiştirin.

3. **Çalıştır**’a tıklayın. Çıktı, IOT Hub'ına gönderilen iletileri ve sensör verilerini göstermelidir.

   ![Raspberry Pi Azure IOT çevrimiçi simülatör](./media/stream-analytics-quick-create-portal/ras-pi-connection-string.png)

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Stream Analytics işini başlatıp çıktıyı denetleyin

1. İşe genel bakış sayfasına dönün ve **Başlat**’ı seçin.

2. Altında **başlangıç işi**seçin **artık**, için **iş çıkışı başlangıç zamanı** alan. Ardından, **Başlat** işinizi başlatmak için.

3. Birkaç dakika sonra portalda işin çıktısı olarak yapılandırdığınız depolama hesabını ve kapsayıcıyı bulun. Çıktı dosyasını artık kapsayıcıda görebilirsiniz. İşin ilk kez başlatılması birkaç dakika sürer ve başlatıldıktan sonra veriler ulaştıkça çalışmaya devam eder.  

   ![Dönüştürülmüş çıktı](./media/stream-analytics-quick-create-portal/check-asa-results.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, Stream Analytics işi ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, durdurup daha sonra gerektiğinde yeniden başlatabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak bu hızlı başlangıçla oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'nı ve ardından oluşturduğunuz kaynağın adını seçin.  

2. Kaynak grubu sayfanızda, **Sil**'i seçin, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure portalı kullanarak basit bir Stream Analytics işi dağıttınız. Stream Analytics işlerini kullanarak da dağıtabilirsiniz [PowerShell](stream-analytics-quick-create-powershell.md), [Visual Studio](stream-analytics-quick-create-vs.md), ve [Visual Studio Code](quick-create-vs-code.md).

Diğer girdi kaynaklarını yapılandırma ve gerçek zamanlı algılama hakkında bilgi almak için aşağıdaki makaleye geçin:

> [!div class="nextstepaction"]
> [Azure Stream Analytics kullanarak gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md)

