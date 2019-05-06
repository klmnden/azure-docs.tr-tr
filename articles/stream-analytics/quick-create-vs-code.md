---
title: Visual Studio Code'da (Önizleme) Azure Stream Analytics bulut işi oluşturma
description: Bu hızlı başlangıçta bir Stream Analytics işi oluşturma, girdileri ve çıktıları yapılandırma ve Visual Studio Code ile bir sorgu tanımlanıyor başlama işlemini gösterir.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 05/06/2019
ms.topic: quickstart
ms.custom: mvc
ms.openlocfilehash: 511dab7090f6114c7769d504166f3e2c137d43ca
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65071889"
---
# <a name="quickstart-create-an-azure-stream-analytics-cloud-job-in-visual-studio-code-preview"></a>Hızlı Başlangıç: Visual Studio Code'da (Önizleme) Azure Stream Analytics bulut işi oluşturma

Bu hızlı başlangıçta oluşturun ve Visual Studio Code için Azure Stream Analytics uzantısını kullanarak Stream Analytics işini çalıştırma gösterilmektedir. Örnek Proje bir IOT Hub CİHAZDAN akış verilerini okur. Blob depolama üzerinde 27 ° ve yeni bir dosya için olayları sonuç çıktısı Yazar ortalama sıcaklık hesaplar olan bir işi tanımlarsınız.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

* [Azure Portal](https://portal.azure.com/) oturum açın.

* Yükleme [Visual Studio Code'u](https://code.visualstudio.com/).

## <a name="install-the-azure-stream-analytics-extension"></a>Azure Stream Analytics uzantıyı yükleme

1. Visual Studio Code'u açın.

2. Gelen **uzantıları** sol bölmesinde, arama **Stream Analytics** seçip **yükleme** üzerinde **Azure Stream Analytics** uzantısı.

3. Uzantıyı yükledikten sonra doğrulayın **Azure Stream Analytics Araçları** görünür olduğundan, **uzantıları etkinleştirildiğinde**.

   ![Uzantılar Visual Studio code'da altında Azure Stream Analytics araçları](./media/quick-create-vs-code/enabled-extensions.png)

## <a name="activate-the-azure-stream-analytics-extension"></a>Azure Stream Analytics uzantısını etkinleştirme

1. Seçin **Azure** VS Code etkinlik çubuğundaki. **Stream Analytics** kenar çubuğu görünür. Altında **Stream Analytics**seçin **Azure'da oturum aç**. 

   ![Visual Studio code'da azure'da oturum açın](./media/quick-create-vs-code/azure-sign-in.png)

2. Oturum açtıktan sonra Azure hesabınızın adını, VS Code penceresinin sol alt köşesindeki durum çubuğunda görünür.

> [!NOTE]
> Oturumunuzu yoksa, azure Stream Analytics araçları otomatik olarak sonraki oturum. Hesabınızda iki öğeli kimlik doğrulama varsa, PIN kullanma yerine telefon kimlik doğrulaması kullanmanız önerilir.
> Kaynakları listeleme sorun yaşarsanız, oturumunuzu kapatıp tekrar genellikle oturum açarken yardımcı olur. Oturumu kapatmak için komutu girin `Azure: Sign Out`.

## <a name="prepare-the-input-data"></a>Girdi verilerini hazırlama

Stream Analytics işini tanımlamadan önce daha sonra iş girdi olarak yapılandırılan verileri hazırlamanız gerekir. İş için gereken girdi verilerini hazırlamak için aşağıdaki adımları tamamlayın:

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. **Kaynak oluştur** > **Nesnelerin İnterneti** > **Iot Hub** seçeneğini belirleyin.

3. İçinde **IOT hub'ı** bölmesinde aşağıdaki bilgileri girin:
   
   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Abonelik  | \<Aboneliğiniz\> |  Kullanmak istediğiniz Azure aboneliğini seçin. |
   |Kaynak grubu   |   asaquickstart-resourcegroup  |   **Yeni Oluştur**’u seçin ve hesabınız için yeni bir kaynak grubu adı girin. |
   |Bölge  |  \<Kullanıcılarınıza en yakın bölgeyi seçin\> | Burada, IOT Hub'ınıza barındırabilirsiniz coğrafi bir konum seçin. Kullanıcılarınıza en yakın konumu kullanın. |
   |IoT Hub Adı  | MyASAIoTHub  |   IOT Hub'ınız için bir ad seçin.   |

   ![IoT Hub'ı oluşturma](./media/quick-create-vs-code/create-iot-hub.png)

4. Seçin **sonraki: Boyut ve ölçek kümesi**.

5. **Fiyatlandırma ve ölçek katmanınızı** seçin. Bu hızlı başlangıçta seçin **F1 - ücretsiz** aboneliğinizde hala kullanılabilir durumdaysa katmanı. Ücretsiz katmanı kullanılamıyorsa, kullanılabilir en düşük katmanlı seçin. Daha fazla bilgi için [IOT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/).

   ![Boyut ve IOT Hub'ınıza ölçeklendirin](./media/quick-create-vs-code/iot-hub-size-and-scale.png)

6. **İncele ve oluştur**’u seçin. IOT Hub bilgilerinizi gözden geçirin ve tıklayın **Oluştur**. IOT Hub'ınızın oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

7. IOT hub'ı Gezinti menüsüne **Ekle** altında **IOT cihazları**. Ekleme bir **cihaz kimliği** tıklatıp **Kaydet**.

   ![IOT Hub'ınıza bir cihaz ekleyin](./media/quick-create-vs-code/add-device-iot-hub.png)

8. Cihaz oluşturulduktan sonra CİHAZDAN açın **IOT cihazları** listesi. Kopyalama **bağlantı dizesi – birincil anahtar** ve daha sonra kullanmak üzere Not Defteri'ne kaydedin.

   ![IOT Hub cihaz bağlantı dizesini kopyalayın](./media/quick-create-vs-code/save-iot-device-connection-string.png)

## <a name="create-blob-storage"></a>BLOB Depolama oluşturma

1. Azure portalının sol üst köşesinden **Kaynak oluştur** > **Depolama** > **Depolama hesabı**’nı seçin.

2. İçinde **depolama hesabı oluşturma** bölmesinde, bir depolama hesabı adı, konum ve kaynak grubu girin. Aynı konum ve kaynak grubu oluşturduğunuz IOT Hub'ı seçin. Ardından **gözden geçir + Oluştur** hesabı oluşturmak için.

   ![Depolama hesabı oluştur](./media/quick-create-vs-code/create-storage-account.png)

3. Depolama hesabınız oluşturulduktan sonra seçin **Blobları** kutucuğundan **genel bakış** bölmesi.

   ![Depolama hesabına genel bakış](./media/quick-create-vs-code/blob-storage.png)

4. Gelen **Blob hizmeti** sayfasında **kapsayıcı** ve gibi kapsayıcınız için bir ad verin *kapsayıcı1*. Bırakın **genel erişim düzeyi** olarak **özel (anonim erişim yok)** seçip **Tamam**.

   ![Blob kapsayıcısı oluşturma](./media/quick-create-vs-code/create-blob-container.png)

## <a name="create-a-stream-analytics-project"></a>Stream Analytics projesi oluşturma

1. Visual Studio Code'da basın **Ctrl + Shift + P** komut paletini açın. Yazarak **ASA** seçip **ASA: Yeni proje oluşturma**.

   ![Yeni proje oluşturma](./media/quick-create-vs-code/create-new-project.png)

2. Projenizin adını gibi giriş **myASAproj** ve projeniz için bir klasör seçin.

    ![Proje adı oluşturma](./media/quick-create-vs-code/create-project-name.png)

3. Çalışma alanınızı yeni proje eklenecektir. Sorgu komut dosyasının ASA projesinde oluşur **(*.asaql)**, **JobConfig.json** dosyası ve bir **asaproj.json** yapılandırma dosyası.

   ![Stream Analytics projesi dosyaları VS code'da](./media/quick-create-vs-code/asa-project-files.png)

4. **Asaproj.json** yapılandırma dosyası, girişler, çıkışlar ve Azure Stream Analytics işi göndermek için gereken işi yapılandırma dosyası bilgileri içerir.

   ![VS code'da Stream Analytics işi yapılandırma dosyası](./media/quick-create-vs-code/job-configuration.png)

> [!Note]
> İçine komut paletinden girişler ve çıkışlar eklerken, karşılık gelen yollarla eklenecek **asaproj.json** otomatik olarak. Ekleyip giriş veya çıkış diskte doğrudan el ile ekleyin veya kaldırın gerekirse **asaproj.json**. Girişleri koymak seçebilirsiniz ve bir çıkış yerleştirin sonra her yolları belirterek farklı projelerde başvuru **asaproj.json**.

## <a name="define-an-input"></a>Bir girdi tanımlama

1. Seçin **Ctrl + Shift + P** komut paletini açın ve girmek için **ASA: Girdi ekleme**.

   ![VS Code'da Stream Analytics Girişi Ekle](./media/quick-create-vs-code/add-input.png)

2. Seçin **IOT hub'ı** için giriş türü.

   ![Giriş seçeneği olarak IOT hub'ı seçin](./media/quick-create-vs-code/iot-hub.png)

3. Giriş kullanacağı ASA sorgu betiğini seçin. Otomatik olarak dosya yolu ile doldurur **myASAproj.asaql**.

   ![Visual Studio Code'da bir ASA betiği seçin](./media/quick-create-vs-code/asa-script.png)

4. Giriş dosyası adıyla girin **IotHub.json**.

5. Düzen **IoTHub.json** şu değerlere sahip. Aşağıda belirtilen olmayan alanlar için varsayılan değerleri koruyun. CodeLens, bir dize girin, açılır listeden seçin veya doğrudan dosyasında metni değiştirme yardımcı olması için kullanabilirsiniz.

   |Ayar|Önerilen değer|Açıklama|
   |-------|---------------|-----------|
   |Ad|Girdi|İşin girdisini tanımlamak için bir ad girin.|
   |IotHubNamespace|MyASAIoTHub|Seçin veya IOT Hub'ınızın adını girin. IOT hub'ı adları, aynı abonelikte oluşturulursa otomatik olarak algılanır.|
   |Uç nokta|Mesajlaşma| |
   |SharedAccessPolicyName|iothubowner| |

## <a name="define-an-output"></a>Bir çıkış tanımlayın

1. Seçin **Ctrl + Shift + P** komut paletini açın. Then, enter **ASA: Çıkış Ekle**.

   ![VS Code'da Stream Analytics çıkış Ekle](./media/quick-create-vs-code/add-output.png)

2. Seçin **Blob Depolama** de Havuz türü için.

3. Bu giriş kullanacağı ASA sorgu betiğini seçin.

4. Çıkış dosyası adı olarak girin **BlobStorage.json**.

5. Düzen **BlobStorage.json** şu değerlere sahip. Aşağıda belirtilen olmayan alanlar için varsayılan değerleri koruyun. CodeLens, bir dize girin veya açılır listeden seçin yardımcı olması için kullanın.

   |Ayar|Önerilen değer|Açıklama|
   |-------|---------------|-----------|
   |Ad|Çıktı| İşin çıktısını tanımlamak için bir ad girin.|
   |Depolama Hesabı|asaquickstartstorage|Seçin ya da depolama hesabınızın adını girin. Depolama hesabı adları aynı abonelikte oluşturulursa otomatik olarak algılanır.|
   |Kapsayıcı|kapsayıcı1|Depolama hesabınızda oluşturduğunuz mevcut kapsayıcıyı seçin.|
   |Yol Deseni|çıkış|Kapsayıcıda oluşturulacak dosya yolunun adını girin.|

## <a name="define-the-transformation-query"></a>Dönüşüm sorgusunu tanımlama

1. Açık **myASAproj.asaql** proje klasörünüzden.

2. Aşağıdaki sorguyu ekleyin:

   ```sql
   SELECT * 
   INTO Output
   FROM Input
   HAVING Temperature > 27
   ```

## <a name="compile-the-script"></a>Kodu derlemek için

Derleme betiği iki şey yapar: autodeployment için Azure Resource Manager şablonları oluşturmak ve sözdizimini denetleyin.

Komut dosyası derleme tetiklemek için iki yolu vardır:

1. Çalışma alanından betiği seçin ve ardından derleme komut paletinden tetikleyin. 

   ![Kodu derlemek için VS Code komut paleti kullanın](./media/quick-create-vs-code/compile-script1.png)

2. Sağ tıklayın betik ve select **ASA: derleme betiği**.

    ![Derlemek için ASA betiğe sağ](./media/quick-create-vs-code/compile-script2.png)

3. Derleme, iki oluşturulan Azure Resource Manager şablonlarında bulabilirsiniz **Dağıt** projenizin klasör. Bu iki dosyayı autodeployment için kullanılır.

    ![Dosya Gezgini'nde Stream Analytics dağıtım şablonları](./media/quick-create-vs-code/deployment-templates.png)

## <a name="submit-a-stream-analytics-job-to-azure"></a>Stream Analytics işi azure'a Gönder

1. Visual Studio Code, Kod Düzenleyicisi penceresinde seçin **aboneliklerinizden seçin**.

   ![Abonelikleri metnin Kod Düzenleyicisi'ni seçin](./media/quick-create-vs-code/select-subscription.png)

2. Açılan listeden aboneliğinizi seçin.

3. İş ** seçin. Yeni iş oluştur öğesini seçin.

4. Proje adınızı girin **myASAjob** ve sonra kaynak grubunu ve konumu seçmek için yönergeleri izleyin.

5. Seçin **Azure'a Gönder**. Günlükleri çıkış penceresinde bulunabilir. 

6. İş oluşturulduğunda Stream Analytics Gezgini'nde görebilirsiniz.

## <a name="run-the-iot-simulator"></a>IOT simülatör'ü çalıştırma

1. Açık [Raspberry Pi Azure IOT çevrimiçi simülatör](https://azure-samples.github.io/raspberry-pi-web-simulator/) yeni tarayıcı sekmesinde veya penceresinde.

2. Satırı 15 yer tutucuyu, önceki bölümde kaydettiğiniz Azure IOT Hub cihaz bağlantı dizesiyle değiştirin.

3. **Çalıştır**’a tıklayın. Çıktı, IOT Hub'ına gönderilen iletileri ve sensör verilerini göstermelidir.

   ![Raspberry Pi Azure IOT çevrimiçi simülatör](./media/quick-create-vs-code/ras-pi-connection-string.png)

## <a name="start-the-stream-analytics-job-and-check-output"></a>Stream Analytics işini başlatıp çıktıyı denetleyin

1. Açık **Stream Analytics Gezgini'nde** Visual Studio code'da ve işinizi bulun **myASAJob**.

2. Proje adına sağ tıklayın. Ardından, **Başlat** bağlam menüsünden.

![VS Code'da Stream Analytics işini başlatın](./media/quick-create-vs-code/start-asa-job-vs-code.png)

3. Seçin **artık** işi başlatmak için ve açılan penceredeki.

4. İş durumu değişti Not **çalıştıran**. Proje adını sağ tıklatın ve seçin **portalında iş görünümünü açmak** giriş bakın ve olay ölçümleri çıktı. Bu işlem birkaç dakika sürebilir.

5. Sonuçları görüntülemek için Visual Studio Code uzantısı'nda veya Azure portalında, blob Depolama'ı açın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, akış işini ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, durdurup daha sonra gerektiğinde yeniden başlatabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak bu hızlı başlangıçla oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'nı ve ardından oluşturduğunuz kaynağın adını seçin.  

2. Kaynak grubu sayfanızda, **Sil**'i seçin, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Visual Studio Code kullanarak basit bir Stream Analytics işi dağıttınız. Stream Analytics işlerini kullanarak da dağıtabilirsiniz [Azure portalında](stream-analytics-quick-create-portal.md), [PowerShell](stream-analytics-quick-create-powershell.md)ve Visual Studio (stream-analytics-quick-oluşturma-vs.md). 

Visual Studio için Azure Stream Analytics araçları hakkında bilgi edinmek için aşağıdaki makaleye geçin:

> [!div class="nextstepaction"]
> [Azure Stream Analytics işleri görüntülemek için Visual Studio](stream-analytics-vs-tools.md)