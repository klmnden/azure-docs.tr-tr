---
title: Azure Machine Learning ile Azure Stream Analytics tümleştirmesi
description: Bu makalede, bir kullanıcı tanımlı işlevini kullanarak, Azure Machine Learning tümleşik basit bir Azure Stream Analytics işi hızlı bir şekilde ayarlamak açıklar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/11/2019
ms.custom: seodec18
ms.openlocfilehash: 2d74488f60f21e3644a7a04579bfab7e70882b01
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621535"
---
# <a name="performing-sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning-studio-preview"></a>Azure Stream Analytics ve Azure Machine Learning Studio (Önizleme) kullanarak yaklaşım analizi gerçekleştirme
Bu makalede, Azure Machine Learning Studio tümleşik basit bir Azure Stream Analytics iş hızlı bir şekilde ayarlamak açıklar. Cortana Intelligence Galerisi'nde akış metin verileri analiz ve gerçek zamanlı yaklaşım puanını belirlemek için Machine Learning yaklaşım analizi modeli kullanın. Cortana Intelligence Suite'i kullanarak yaklaşım analizi model oluşturmanın ayrıntılı olarak incelenmektedir hakkında endişelenmeden, bu görevi gerçekleştirmenize olanak tanır.

Bu makaleden bu gibi senaryolarda öğrenecekleriniz uygulayabilirsiniz:

* Twitter verilerini akış üzerinde gerçek zamanlı duygu analizi.
* Müşteri kayıtlarını çözümleme destek personeli ile sohbetleri.
* Forumlar, bloglar ve videolar hakkındaki yorumları değerlendiriliyor. 
* Çok sayıda diğer gerçek zamanlı ve Tahmine dayalı Puanlama senaryolar.

Gerçek hayattaki bir senaryoda, doğrudan bir Twitter veri akışından verileri elde edersiniz. Akış analizi işi, Azure Blob Depolama alanında bir CSV dosyasından tweet'leri alır. böylece öğretici basitleştirmek için onu yazılır. Kendi CSV dosyası oluşturabilirsiniz veya örnek CSV dosyası, aşağıdaki görüntüde gösterildiği gibi kullanabilirsiniz:

![Bir CSV dosyasında gösterilen örnek tweetleri](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

Oluşturduğunuz akış analizi işi yaklaşım analizi model kullanıcı tanımlı işlevi (UDF) blob depolama alanından örnek metin verilere uygular. ' % S'çıkış (yaklaşım analizi sonuç) farklı bir CSV dosyasında aynı blob deposuna yazılır. 

Aşağıdaki şekil, bu yapılandırma gösterilmektedir. , Daha gerçekçi bir senaryo için belirtildiği gibi blob depolama ile Twitter verilerini Azure Event Hubs'a giriş akış değiştirebilirsiniz. Ayrıca, yapı bir [Microsoft Power BI](https://powerbi.microsoft.com/) birleşik yaklaşım, gerçek zamanlı görselleştirme.    

![Stream Analytics, Machine Learning tümleştirme genel bakış](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce şunlara sahip olduğunuzdan emin olun:

* Etkin bir Azure aboneliği.
* Bazı veriler içeren bir CSV dosyası. Öğesinden daha önce gösterilen dosyayı indirebilirsiniz [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), ya da kendi dosya oluşturabilirsiniz. Bu makalede GitHub dosyasından kullandığınız varsayılır.

Yüksek düzeyde, bu makalede gösterilen görevleri tamamlamak için aşağıdakileri yapın:

1. Bir Azure depolama hesabı ve blob depolama kapsayıcısı oluşturun ve CSV biçimli bir giriş dosyasını kapsayıcıya yüklemek.
3. Yaklaşım analizi modeli Cortana Intelligence Galerisi'nde Azure Machine Learning Studio çalışma alanınıza eklemek ve bu modeli, Machine Learning çalışma alanı içinde bir web hizmeti olarak dağıtın.
5. Giriş metin için yaklaşım belirlemek için bir işlev olarak bu web hizmetini çağıran bir Stream Analytics işi oluşturun.
6. Stream Analytics işini başlatın ve çıktıyı denetleyin.

## <a name="create-a-storage-container-and-upload-the-csv-input-file"></a>Bir depolama kapsayıcısı oluşturun ve CSV giriş dosyasını karşıya yükle
Bu adım için Github'da kullanılabilir bir gibi herhangi bir CSV dosyasına kullanabilirsiniz.

1. Azure portalında **kaynak Oluştur** > **depolama** > **depolama hesabı**.

2. Bir ad sağlayın (`samldemo` örnekte). Ad yalnızca küçük harflerden ve rakamlardan kullanabilirsiniz ve Azure genelinde benzersiz olmalıdır. 

3. Mevcut bir kaynak grubunu belirtin ve konumu belirtin. Konum için Bu öğreticide oluşturulan tüm kaynakları aynı konumu kullanmanızı öneririz.

    ![Depolama hesabı ayrıntılarını girin](./media/stream-analytics-machine-learning-integration-tutorial/create-storage-account1.png)

4. Azure portalında depolama hesabı seçin. Depolama hesabı dikey penceresinde **kapsayıcıları** ve ardından  **+ &nbsp;kapsayıcı** blob depolama alanı oluşturmak için.

    ![Giriş BLOB depolama kapsayıcısı oluşturma](./media/stream-analytics-machine-learning-integration-tutorial/create-storage-account2.png)

5. Kapsayıcı için bir ad sağlayın (`azuresamldemoblob` örnekte) doğrulayın **erişim türü** ayarlanır **Blob**. İşiniz bittiğinde, **Tamam**’a tıklayın.

    ![BLOB kapsayıcı ayrıntılarını belirtin](./media/stream-analytics-machine-learning-integration-tutorial/create-storage-account3.png)

6. İçinde **kapsayıcıları** dikey penceresinde, bu kapsayıcı için dikey pencereyi açar Yeni kapsayıcıyı seçin.

7. **Karşıya Yükle**'ye tıklayın.

    ![Bir kapsayıcı için 'Karşıya Yükle' düğmesine](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

8. İçinde **blobu karşıya yükleme** dikey penceresinde, karşıya yükleme **sampleinput.csv** daha önce indirdiğiniz dosyayı. İçin **Blob türü**seçin **blok blobu** ve blok boyutu Bu öğretici için yeterliyse 4 MB olarak ayarlayın.

9. Tıklayın **karşıya** dikey pencerenin alt kısmındaki düğmesi.

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Cortana Intelligence Galeriden yaklaşım analizi modeli ekleme

Örnek veriler bir blob içinde olduğuna göre Cortana Intelligence Galerisi'nde yaklaşım analizi modeli etkinleştirebilirsiniz.

1. Git [yaklaşım Tahmine dayalı analiz modeli](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) Cortana Intelligence Galerisi'nde sayfası.  

2. Tıklayın **Studio'da Aç**.  
   
   ![Stream Analytics Machine Learning, Machine Learning Studio'yu Aç](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. Çalışma alanına dönmek oturum açın. Bir konum seçin.

4. Tıklayın **çalıştırma** sayfanın alt kısmındaki. İşlem sürerken, yaklaşık bir dakika sürer.

   ![Machine Learning Studio'da deneme çalıştırma](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. İşlem başarıyla çalıştırdıktan sonra seçin **Web hizmeti Dağıt** sayfanın alt kısmındaki.

   ![Machine Learning Studio'da deneme bir web hizmeti olarak dağıtma](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. Yaklaşım analizi modeli kullanıma hazır olduğunu doğrulamak için tıklayın **Test** düğmesi. "Microsoft seviyorum gibi" giriş metin sağlayın. 

   ![Machine Learning Studio'da deneme test](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

    Test çalışıyorsa, aşağıdaki örneğe benzer bir sonuç bakın:

   ![Machine Learning Studio'da test sonuçları](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. İçinde **uygulamaları** sütun tıklayın **Excel 2010 veya önceki çalışma kitabı** bir Excel çalışma kitabı indirmek için bağlantı. Çalışma kitabı, API anahtarını ve daha sonra Stream Analytics işi ayarlamak için gereken URL içerir.

    ![Stream Analytics Machine Learning, Hızlı Bakış](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  


## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Machine Learning modelini kullanan bir Stream Analytics işi oluşturma

Şimdi, blob depolama alanında CSV dosyasından örnek tweetleri okuyan bir Stream Analytics işi oluşturabilirsiniz. 

### <a name="create-the-job"></a>İşi oluşturma

1. [Azure Portal](https://portal.azure.com) gidin.  

2. Tıklayın **kaynak Oluştur** > **nesnelerin interneti** > **Stream Analytics işi**. 

3. İş adı `azure-sa-ml-demo`bir abonelik belirtin, mevcut bir kaynak grubu belirtin veya yeni bir tane oluşturun ve iş konumunu seçin.

   ![Yeni Stream Analytics işine ilişkin ayarlar belirtin](./media/stream-analytics-machine-learning-integration-tutorial/create-stream-analytics-job-1.png)
   

### <a name="configure-the-job-input"></a>İş Girişi yapılandırma
İş giriş blob depolama alanına daha önce yüklenen CSV dosyasından alır.

1. İş, altında oluşturulduktan sonra **iş topolojisi** işi dikey penceresinde **girişleri** seçeneği.    

2. İçinde **girişleri** dikey penceresinde tıklayın **Stream giriş eklemek** >**Blob Depolama**

3. Doldurun **Blob Depolama** dikey penceresini aşağıdaki değerlerle:

   
   |Alan  |Değer  |
   |---------|---------|
   |**Giriş diğer adı** | Adı kullanmak `datainput` seçip **blob depolama aboneliğinizde seçin**       |
   |**Depolama hesabı**  |  Daha önce oluşturduğunuz depolama hesabını seçin.  |
   |**Kapsayıcı**  | Daha önce oluşturduğunuz kapsayıcıya seçin (`azuresamldemoblob`)        |
   |**Olay serileştirme biçimi**  |  Seçin **CSV**       |

   ![Yeni Stream Analytics işi girişi için ayarlar](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-create-sa-input-new-portal.png)

1. **Kaydet**’e tıklayın.

### <a name="configure-the-job-output"></a>İş çıkışını yapılandırma
İş, sonuçlar burada giriş aynı blob depolamaya gönderir. 

1. Altında **iş topolojisi** işi dikey penceresinde **çıkışları** seçeneği.  

2. İçinde **çıkışları** dikey penceresinde tıklayın **Ekle** >**Blob Depolama**ve ardından diğer ad ile bir çıktı ekleyin `datamloutput`. 

3. Doldurun **Blob Depolama** dikey penceresini aşağıdaki değerlerle:

   |Alan  |Değer  |
   |---------|---------|
   |**Çıkış diğer adı** | Adı kullanmak `datamloutput` seçip **blob depolama aboneliğinizde seçin**       |
   |**Depolama hesabı**  |  Daha önce oluşturduğunuz depolama hesabını seçin.  |
   |**Kapsayıcı**  | Daha önce oluşturduğunuz kapsayıcıya seçin (`azuresamldemoblob`)        |
   |**Olay serileştirme biçimi**  |  Seçin **CSV**       |

   ![Yeni Stream Analytics işi çıktı için ayarları](./media/stream-analytics-machine-learning-integration-tutorial/create-stream-analytics-output.png) 

4. **Kaydet**’e tıklayın.   


### <a name="add-the-machine-learning-function"></a>Machine Learning işlevi ekleme 
Daha önce bir web hizmetine bir Machine Learning modeli yayımladı. Stream analiz iş çalıştırıldığında, bu senaryoda, her örnek tweet girdisinden yaklaşım analizi için web hizmetine gönderir. Machine Learning web hizmeti bir yaklaşım döndürür (`positive`, `neutral`, veya `negative`) ve pozitif olan tweet'i bir olasılık. 

Öğreticinin bu bölümünde, Stream analiz işteki bir fonksiyon tanımlayın. İşlevi, web hizmetine bir tweet gönderin ve yanıt almak için çağrılabilir. 

1. Daha önce Excel çalışma kitabında yüklediğiniz web hizmeti URL'sini ve API anahtarı olduğundan emin olun.

2. İşi dikey pencerenize gidin > **işlevleri** >  **+ Ekle** > **AzureML**

3. Doldurun **Azure Machine Learning işlevi** dikey penceresini aşağıdaki değerlerle:

   |Alan  |Değer  |
   |---------|---------|
   | **İşlev diğer adı** | Adı kullanın `sentiment` seçip **sağlayan Azure Machine Learning işlevi ayarlarını elle** size sağlayan bir seçenek URL'sini ve anahtarını girin.      |
   | **URL**| Web hizmeti URL'sini yapıştırın.|
   |**Anahtar** | API anahtarını yapıştırın. |
  
   ![Stream Analytics işi için Machine Learning işlevi eklemek için ayarlar](./media/stream-analytics-machine-learning-integration-tutorial/add-machine-learning-function.png)  
    
4. **Kaydet**’e tıklayın.

### <a name="create-a-query-to-transform-the-data"></a>Verileri dönüştürmek için bir sorgu oluşturun

Stream Analytics, bildirim temelli, SQL tabanlı sorgu giriş incelemek ve işlemek için kullanır. Bu bölümde, her tweet girişten okur ve ardından ilgili yaklaşım analizi gerçekleştirmek için Machine Learning işlevi çağıran bir sorgu oluşturun. Sorgu sonucu (blob depolama) tanımlanan çıkış ardından gönderir.

1. Proje genel bakış dikey penceresine dönün.

2.  Altında **iş topolojisi**, tıklayın **sorgu** kutusu.

3. Aşağıdaki sorguyu girin:

    ```SQL
    WITH sentiment AS (  
    SELECT text, sentiment1(text) as result 
    FROM datainput  
    )  

    SELECT text, result.[Score]  
    INTO datamloutput
    FROM sentiment  
    ```    

    Sorgu, daha önce oluşturduğunuz işlev çağırır (`sentiment`) girişte her tweet üzerinde yaklaşım analizi gerçekleştirmek için. 

4. Tıklayın **Kaydet** sorguyu kaydetmek için.


## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Stream Analytics işini başlatıp çıktıyı denetleyin

Stream Analytics işi şimdi başlayabilirsiniz.

### <a name="start-the-job"></a>İşi başlatma
1. Proje genel bakış dikey penceresine dönün.

2. Tıklayın **Başlat** dikey penceresinin üstünde.

3. İçinde **başlangıç işi**seçin **özel**ve ardından bir CSV dosyasını blob depolamaya yüklediğiniz gün seçin. İşiniz bittiğinde tıklayın **Başlat**.  


### <a name="check-the-output"></a>Çıktıyı denetleyin
1. Etkinlik görene kadar birkaç dakika iş izin **izleme** kutusu. 

2. Blob depolama içeriğini incelemek için normalde kullandığınız bir aracınız varsa, incelemek için bu aracı kullanın `azuresamldemoblob` kapsayıcı. Alternatif olarak, Azure portalında aşağıdaki adımları uygulayın:

    1. Portalı'nda bulması `samldemo` depolama hesabı ve hesap içinde Bul `azuresamldemoblob` kapsayıcı. Kapsayıcıyı iki dosyada bakın: örnek tweetleri içeren dosyayı ve Stream Analytics işi tarafından oluşturulan bir CSV dosyası.
    2. Oluşturulan dosyayı sağ tıklayın ve ardından **indirme**. 

   ![Blob depolama alanından CSV iş çıktısını indirme](./media/stream-analytics-machine-learning-integration-tutorial/download-output-csv-file.png)  

3. Oluşturulan CSV dosyasını açın. Aşağıdaki örneğe benzer bir şey görürsünüz:  
   
   ![Stream Analytics, Machine Learning, CSV görüntüleyin](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  


### <a name="view-metrics"></a>Ölçümleri görüntüle
Azure Machine Learning işlevi ile ilgili ölçümleri de görüntüleyebilirsiniz. Aşağıdaki işlev ile ilgili ölçümleri görüntülenen **izleme** işi dikey pencerede kutusunda:

* **İşlev istekleri** Machine Learning web hizmetine gönderilen isteklerin sayısını gösterir.  
* **İşlev olayları** olay istek sayısını gösterir. Varsayılan olarak, bir Machine Learning web hizmetine her isteği en fazla 1.000 olayları içerir.  


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [REST API tümleştirin ve makine öğrenimi](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)



