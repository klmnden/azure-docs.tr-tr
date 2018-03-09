---
title: "Azure akış analizi ve makine öğrenme tümleştirme | Microsoft Docs"
description: "Bir kullanıcı tanımlı işlev ve Machine Learning Stream Analytics işinde kullanma"
keywords: 
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: jhubbard
editor: cgronlun
ms.assetid: cfced01f-ccaa-4bc6-81e2-c03d1470a7a2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/01/2018
ms.author: sngun
ms.openlocfilehash: 10d514aeb50dcd24f28ed879875b23b25578cebb
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="performing-sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Azure akış analizi ve Azure Machine Learning kullanarak düşünceleri analiz gerçekleştirme
Bu makalede, Azure Machine Learning tümleştiren basit bir Azure akış analizi işi hızlı bir şekilde ayarlamak açıklar. Machine Learning düşünceleri analiz modeli Cortana Intelligence Galeriden akış metin verileri çözümlemek ve gerçek zamanlı düşünceleri puan belirlemek için kullanın. Cortana Intelligence Suite kullanarak düşünceleri analiz modeli oluşturmanın ayrıntılı olarak incelenmektedir hakkında endişelenmeden bu görevi gerçekleştirirken olanak sağlar.

Ne bunlar gibi senaryolar için bu makaleden öğrenin uygulayabilirsiniz:

* Twitter veri akışı üzerinde gerçek zamanlı düşünceleri çözümleniyor.
* Müşteri kayıtlarını çözümleme destek personeli ile sohbetleri.
* Forum, bloglar ve videolar açıklamaları değerlendiriliyor. 
* Birçok diğer gerçek zamanlı, Tahmine dayalı Puanlama senaryoları.

Gerçek hayattaki bir senaryoda, doğrudan bir Twitter veri akışından veri almalıdır. Akış analizi işi, Azure Blob Depolama alanında bir CSV dosyasından tweet'leri alır. böylece biz yazılmış öğretici basitleştirmek için. Örnek bir CSV dosyası aşağıdaki görüntüde gösterildiği gibi kullanabilirsiniz veya kendi CSV dosyası oluşturabilirsiniz:

![bir CSV dosyasında örnek tweet'leri](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

Oluşturduğunuz akış analizi işi düşünceleri analiz modeli kullanıcı tanımlı işlevi (UDF) blob depolama alanından örnek metin verilere uygular. Çıktı (düşünceleri analiz sonucunu) aynı blob Mağazası'nda farklı bir CSV dosyası yazılır. 

Aşağıdaki şekilde, bu yapılandırma gösterilmektedir. Belirtilenler daha gerçekçi bir senaryo için blob depolama Twitter verilerini Azure Event Hubs'a giriş akış ile değiştirebilirsiniz. Ayrıca, oluşturacağınız bir [Microsoft Power BI](https://powerbi.microsoft.com/) gerçek zamanlı görsel olarak birleşik düşünceleri.    

![Stream Analytics Machine Learning tümleştirmesine genel bakış](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce aşağıdakilere sahip olduğunuzdan emin olun:

* Etkin bir Azure aboneliği.
* Bazı veriler içeren bir CSV dosyası. Öğesinden daha önce gösterilen dosyayı indirebilirsiniz [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), ya da kendi dosyası oluşturabilirsiniz. Bu makale için GitHub dosyasından kullandığınız varsayılmıştır.

Yüksek düzeyde, bu makalede gösterilen görevleri tamamlamak için aşağıdakileri yapın:

1. Bir Azure depolama hesabı ve blob depolama kapsayıcısını oluşturun ve CSV olarak biçimlendirilmiş bir giriş dosyası kapsayıcısına yükleyin.
3. Düşünceleri analiz modeli Cortana Intelligence Galeriden Azure Machine Learning çalışma alanına eklemek ve bu model Machine Learning çalışma alanında bir web hizmeti olarak dağıtabilirsiniz.
5. Metin girişi için düşünceleri belirlemek için bir işlevi olarak bu web hizmeti çağrıları Stream Analytics işi oluşturun.
6. Stream Analytics işini başlatmak ve çıktısını denetleyin.

## <a name="create-a-storage-container-and-upload-the-csv-input-file"></a>Depolama kapsayıcısı oluşturun ve CSV giriş dosyasını karşıya yükle
Bu adım için Github'da kullanılabilir bir gibi herhangi bir CSV dosyası kullanabilirsiniz.

1. Azure portalında tıklatın **kaynak oluşturma** > **depolama** > **depolama hesabı**.

2. Bir ad sağlayın (`samldemo` örnekte). Ad yalnızca küçük harfler ve sayılar kullanabilirsiniz ve Azure arasında benzersiz olması gerekir. 

3. Varolan bir kaynak grubu belirtin ve bir konum belirtin. Konum için Bu öğreticide oluşturduğunuz tüm kaynakların aynı konumu kullanmanızı öneririz.

    ![Depolama hesabı ayrıntılarını sağlayın](./media/stream-analytics-machine-learning-integration-tutorial/create-sa1.png)

4. Azure Portal'da depolama hesabı seçin. Depolama hesabı dikey penceresinde tıklayın **kapsayıcıları** ve ardından  **+ &nbsp;kapsayıcı** blob depolama alanı oluşturmak için.

    ![BLOB kapsayıcı oluşturun](./media/stream-analytics-machine-learning-integration-tutorial/create-sa2.png)

5. Kapsayıcı için bir ad (`azuresamldemoblob` örnekte) doğrulayın **erişim türüne** ayarlanır **Blob**. İşiniz bittiğinde, **Tamam**’a tıklayın.

    ![BLOB kapsayıcı ayrıntıları belirtin](./media/stream-analytics-machine-learning-integration-tutorial/create-sa3.png)

6. İçinde **kapsayıcıları** dikey penceresinde, bu kapsayıcı için bir dikey pencere açılır yeni kapsayıcıyı seçin.

7. **Karşıya Yükle**'ye tıklayın.

    ![Bir kapsayıcı 'Karşıya' düğmesi](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

8. İçinde **karşıya yükleme blob** dikey penceresinde, karşıya yükleme **sampleinput.csv** daha önce indirilen dosya. İçin **Blob türü**seçin **blok blobu** ve blok boyutu Bu öğretici için yeterliyse 4 MB olarak ayarlayın.

9. Tıklatın **karşıya** dikey pencerenin altındaki düğmesini.

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Cortana Intelligence Galeriden düşünceleri analiz modeli ekleme

Örnek verileri bir blob'a olduğundan, Cortana Intelligence Galerisi düşünceleri analiz modelinde etkinleştirebilirsiniz.

1. Git [Tahmine dayalı düşünceleri analiz modeli](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) Cortana Intelligence Galerisi'nde sayfasında.  

2. Tıklatın **Studio'da Aç**.  
   
   ![Stream Analytics Machine Learning, açık Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. Çalışma alanına gidin için oturum açın. Bir konum seçin.

4. Tıklatın **çalıştırmak** sayfanın sonundaki. İşlem çalışır ve hangi yaklaşık bir dakika sürer.

   ![denemeyi Machine Learning Studio'da çalıştırın](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. İşlem başarıyla çalıştırıldıktan sonra seçin **Web hizmeti Dağıt** sayfanın sonundaki.

   ![denemeyi Machine Learning Studio'da bir web hizmeti olarak dağıtma](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. Düşünceleri analiz modeli kullanıma hazır olduğunu doğrulamak için tıklatın **Test** düğmesi. Metin "ı Microsoft memnuniyet"gibi giriş sağlar. 

   ![Machine Learning Studio'da Test deneme](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

    Test çalışırsa, aşağıdaki örneğe benzer bir sonuç bakın:

   ![Machine Learning Studio'da test sonuçları](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. İçinde **uygulamaları** sütun tıklatın **Excel 2010 veya önceki çalışma kitabı** bir Excel çalışma kitabı indirmek için bağlantı. Çalışma kitabını içeren bir API anahtarı ve daha sonra Stream Analytics işi ayarlamak için gereken URL.

    ![Stream Analytics Machine Learning, Hızlı Bakış](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  


## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Machine Learning modelini kullanan bir Stream Analytics işi oluşturma

Artık blob depolamada CSV dosyasından örnek tweet'leri okuyan akış analizi işi oluşturabilirsiniz. 

### <a name="create-the-job"></a>Proje oluşturma

1. [Azure Portal](https://portal.azure.com) gidin.  

2. Tıklatın **kaynak oluşturma** > **nesnelerin interneti** > **Stream Analytics işi**. 

3. İş adı `azure-sa-ml-demo`bir abonelik belirtin, varolan bir kaynak grubu belirtin veya yeni bir tane oluşturun ve iş konumunu seçin.

   ![Yeni akış analizi işi için ayarları belirtin](./media/stream-analytics-machine-learning-integration-tutorial/create-job-1.png)
   

### <a name="configure-the-job-input"></a>İş Girişi yapılandırın
İş kendi giriş blob depolama alanına daha önce yüklediğiniz CSV dosyasından alır.

1. İş, altında oluşturulduktan sonra **iş topoloji** iş dikey penceresinde tıklayın **girişleri** seçeneği.    

2. İçinde **girişleri** dikey penceresinde tıklatın **akış girişi ekleme** >**Blob Depolama**

3. Doldurmak **Blob Storage** dikey penceresinde bu değerleri içeren:

   
   |Alan  |Değer  |
   |---------|---------|
   |**Giriş diğer adı** | Adı kullanmak `datainput` seçip **seçin, aboneliğiniz depolama blob**       |
   |**Depolama hesabı**  |  Daha önce oluşturduğunuz depolama hesabını seçin.  |
   |**kapsayıcı**  | Daha önce oluşturduğunuz kapsayıcısı seçin (`azuresamldemoblob`)        |
   |**Olayı seri hale getirme biçimi**  |  Seçin **CSV**       |

   ![Yeni iş girişi için ayarlar](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-create-sa-input-new-portal.png)

4. **Kaydet**’e tıklayın.

### <a name="configure-the-job-output"></a>İş çıktısı yapılandırın
İş yeri giriş sonuçları aynı blob depolama alanına gönderir. 

1. Altında **iş topoloji** iş dikey penceresinde tıklayın **çıkışları** seçeneği.  

2. İçinde **çıkışları** dikey penceresinde tıklatın **Ekle** >**Blob storage**ve ardından diğer ada sahip bir çıkış ekleyin `datamloutput`. 

3. Doldurmak **Blob Storage** dikey penceresinde bu değerleri içeren:

   |Alan  |Değer  |
   |---------|---------|
   |**Çıkış diğer adları** | Adı kullanmak `datainput` seçip **seçin, aboneliğiniz depolama blob**       |
   |**Depolama hesabı**  |  Daha önce oluşturduğunuz depolama hesabını seçin.  |
   |**kapsayıcı**  | Daha önce oluşturduğunuz kapsayıcısı seçin (`azuresamldemoblob`)        |
   |**Olayı seri hale getirme biçimi**  |  Seçin **CSV**       |

   ![Yeni iş çıktısı için ayarları](./media/stream-analytics-machine-learning-integration-tutorial/create-output2.png) 

4. **Kaydet**’e tıklayın.   


### <a name="add-the-machine-learning-function"></a>Machine Learning işlevi ekleme 
Daha önce bir web hizmetine bir Machine Learning modeli yayımladı. Akış analizi işi çalıştığında, Senaryomuzda her örnek tweet girdisinden düşünceleri çözümleme için web hizmetine gönderir. Machine Learning web hizmeti bir düşünceleri döndürür (`positive`, `neutral`, veya `negative`) ve pozitif olması tweet bir olasılık. 

Öğreticinin bu bölümünde, akış analizi işi işlevinde tanımlarsınız. İşlev, web hizmetine tweet gönderecek ve yanıt geri almak için çağrılabilir. 

1. Excel çalışma kitabında daha önce yüklediğiniz web hizmeti URL'sini ve API anahtarını olduğundan emin olun.

2. İş dikey penceresine gidin > **işlevleri** > **+ Ekle** > **AzureML**

3. Doldurmak **Azure Machine Learning işlevi** dikey penceresinde bu değerleri içeren:

   |Alan  |Değer  |
   |---------|---------|
   | **İşlev diğer adı** | Adı kullanın `sentiment` seçip **sağlayan Azure Machine Learning işlevi ayarlarını el ile** , size bir seçenek anahtarı ve URL girin.      |
   | **URL**| Web hizmeti URL'sini yapıştırın.|
   |**Anahtar** | API anahtarını yapıştırın. |
  
   ![Machine Learning işlevi Stream Analytics işi eklemek için ayarlar](./media/stream-analytics-machine-learning-integration-tutorial/add-function.png)  
    
4. **Kaydet**’e tıklayın.

### <a name="create-a-query-to-transform-the-data"></a>Verileri dönüştürmek için bir sorgu oluşturun

Akış analizi giriş inceleyin ve işleme için bir bildirim temelli, SQL tabanlı sorgu kullanır. Bu bölümde, her tweet girişten okuyan ve düşünceleri analizi yapmak için Machine Learning işlevi çağıran bir sorgu oluşturun. Sorgu sonucu sonra (blob depolama) tanımlanan çıkış gönderir.

1. İş genel bakış dikey penceresine geri dönün.

2.  Altında **iş topoloji**, tıklatın **sorgu** kutusu.

3. Aşağıdaki sorguyu girin:

    ```
    WITH sentiment AS (  
    SELECT text, sentiment(text) as result from datainput  
    )  

    Select text, result.[Score]  
    Into datamloutput
    From sentiment  
    ```    

    Sorgu daha önce oluşturduğunuz işlevi çağırır (`sentiment`) her tweet giriş üzerinde düşünceleri analiz gerçekleştirmek için. 

4. Tıklatın **kaydetmek** sorguyu kaydetmek için.


## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Stream Analytics işini başlatmak ve çıktısını denetleyin

Stream Analytics işi şimdi başlayabilirsiniz.

### <a name="start-the-job"></a>İşi Başlat
1. İş genel bakış dikey penceresine geri dönün.

2. Tıklatın **Başlat** dikey pencerenin üstündeki.

3. İçinde **başlangıç işi**seçin **özel**ve ardından bir blob depolama alanına CSV dosyasını karşıya önce gün seçin. İşiniz bittiğinde tıklatın **Başlat**.  


### <a name="check-the-output"></a>Çıktı denetleyin
1. Etkinliğinde görene kadar birkaç dakika için iş izin **izleme** kutusu. 

2. Blob depolama içeriğini incelemek için normalde kullandığınız bir aracı varsa, incelemek için bu aracı kullanın `azuresamldemoblob` kapsayıcı. Alternatif olarak, Azure portalında aşağıdaki adımları uygulayın:

    1. Portalı'nda bulmak `samldemo` depolama hesabı ve hesaptaki Bul `azuresamldemoblob` kapsayıcı. Kapsayıcı iki dosyada bkz: örnek tweet'leri içeren dosya ve akış analizi işi tarafından oluşturulan bir CSV dosyası.
    2. Oluşturulan dosyaya sağ tıklayın ve ardından **karşıdan**. 

   ![Blob depolama alanından CSV iş çıktısı indirme](./media/stream-analytics-machine-learning-integration-tutorial/download-output-csv-file.png)  

3. Oluşturulan CSV dosyasını açın. Aşağıdaki örneğe benzer bir şey görürsünüz:  
   
   ![Stream Analytics Machine Learning, CSV görünümü](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  


### <a name="view-metrics"></a>Metrikleri görüntüleyin
Azure Machine Learning işlevi ödemeyle ilgili ölçümlerini de görüntüleyebilirsiniz. Aşağıdaki işlevi ödemeyle ilgili ölçümlerini görüntülenen **izleme** iş dikey penceresinde kutusunda:

* **İşlev istekleri** Machine Learning web hizmetine gönderilen istek sayısını gösterir.  
* **İşlev olayları** olayları isteği sayısını gösterir. Varsayılan olarak, her bir Machine Learning web hizmeti isteğine en çok 1.000 olayları içerir.  


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API tümleştirme ve makine öğrenme](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)



