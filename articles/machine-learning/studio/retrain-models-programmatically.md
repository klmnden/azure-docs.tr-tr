---
Başlık: Retrain Machine Learning Studio'da, programlı olarak titleSuffix modelleri: Azure Machine Learning Studio açıklaması: Program aracılığıyla kullanarak modeli yeniden eğitme öğrenin C# ve Machine Learning Batch Execution hizmeti.
Hizmetler: Makine öğrenimi ms.service: Makine öğrenimi ms.component: studio ms.topic: makale

author: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18 ms.date: 04/19/2017
---
# <a name="retrain-azure-machine-learning-studio-models-programmatically"></a>Azure Machine Learning Studio modellerini programlama yoluyla yeniden eğitme
Bu izlenecek yolda, program aracılığıyla bir Azure Machine Learning Studio web hizmeti kullanarak yeniden eğitme hakkında bilgi edineceksiniz C# ve Machine Learning Batch Execution hizmeti.

Model retrained sonra aşağıdaki izlenecek yollar, Tahmine dayalı web hizmeti olarak modeli güncelleştirmek nasıl göster:

* Machine Learning Web Hizmetleri portalında bir Klasik web hizmetini dağıttıysanız bkz [bir Klasik web hizmetini yeniden eğitme](retrain-a-classic-web-service.md). 
* Yeni bir web hizmetini dağıttıysanız bkz [Machine Learning Yönetimi cmdlet'lerini kullanarak yeni bir web hizmetini yeniden eğitme](retrain-new-web-service-using-powershell.md).

Yeniden eğitme sürecinin genel bakış için bkz. [makine öğrenme modeli yeniden eğitme](retrain-machine-learning-model.md).

Var olan yeni Azure Resource Manager tabanlı web hizmetiyle başlatmak istiyorsanız, bkz. [mevcut bir Tahmine dayalı web hizmetini yeniden eğitme](retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Eğitim denemenizi oluşturma
Bu örnekte, kullanacağınız "örnek 5: Eğitim, Test, değerlendirmek için ikili sınıflandırma: Yetişkinlere yönelik veri kümesinden"Microsoft Azure Machine Learning örnekleri. 

Bir deneme oluşturmak için:

1. Oturum Microsoft Azure Machine Learning Studio'da. 
2. Üzerinde panosunu sağ alt köşesindeki, tıklayın **yeni**.
3. Microsoft Samples örnek 5'i seçin.
4. Deneme tuvalinin üst kısmındaki denemeyi yeniden adlandırma için deneme adını seçin "örnek 5: Eğitim, Test, değerlendirmek için ikili sınıflandırma: Yetişkinlere yönelik veri kümeleri".
5. Sayım Model türü.
6. Deneme tuvalinin altındaki tıklatın **çalıştırma**.
7. Tıklayın **Ayarla web hizmeti** seçip **web hizmetini yeniden eğitme**. 

Aşağıdaki ilk deneme gösterir.
   
   ![İlk deneme.][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Tahmine dayalı bir deneme oluşturma ve bir web hizmeti olarak yayımlama
Ardından bir Predicative deneme oluşturun.

1. Deneme tuvalinin altındaki tıklatın **Web hizmetinin ayarı** seçip **Tahmine dayalı Web hizmeti**. Bu model, eğitilen Model olarak kaydeder ve web hizmeti giriş ve çıkış modülleri ekler. 
2. **Çalıştır**’a tıklayın. 
3. Denemeyi çalışması bittikten sonra tıklayın **Web hizmeti dağıtma [Klasik]** veya **Web hizmeti dağıtma [Yeni]**.

> [!NOTE] 
> Yeni bir web hizmetini dağıtmak için yeterli olan aboneliği, web hizmetini dağıtma olması gerekir. Daha fazla bilgi edinmek, [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönetme](manage-new-webservice.md). 

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Eğitim denemesini eğitim web hizmeti olarak dağıtma
Eğitim modeli yeniden eğitme Retraining web hizmeti olarak oluşturduğunuz eğitim denemesini dağıtmanız gerekir. Bu web hizmetini gereken bir *Web hizmeti çıkış* bağlı Modülü *[modeli eğitme] [ train-model]* yeni üretmek için modülü, eğitim modeller.

1. Eğitim denemesini için geri dönmek için sol bölmedeki denemeleri simgesine tıkladıktan sonra Görselleştirmenizdeki modeli adlı denemeye tıklayın.  
2. Web hizmeti arama deneme öğeleri arama kutusuna yazın. 
3. Sürükleme bir *Web hizmeti girişini* denemeyi modülü tuval ve bağlanmak için çıktısını *eksik verileri temizleme* modülü.  Bu, yeniden eğitme verilerinizi özgün eğitim verilerinizi aynı şekilde işlenir sağlar.
4. İki *web hizmeti çıkış* modülleri deneme tuvaline sürükleyin. Çıkışını *modeli eğitme* modülü bir tane ve çıktısını *Evaluate Model* diğerine modülü. Web hizmeti çıktısı **modeli eğitme** yeni eğitim modeli sağlıyor. Çıktı bağlı **Evaluate Model** bu modül derleyicisinin çıktısının, performans sonuçlarını olduğu döndürür.
5. **Çalıştır**’a tıklayın. 

Sonraki eğitim denemesini eğitilen bir modelin ve model değerlendirme sonuçları üreten bir web hizmeti olarak dağıtmanız gerekir. Bunu yapmak için sonraki eylemleri kümesini, Klasik web hizmetini veya yeni bir web hizmeti ile çalışma hakkındaki bağlı.  

**Klasik web hizmeti**

Deneme tuvalinin altındaki tıklatın **Web hizmetinin ayarı** seçip **Web hizmeti dağıtma [Klasik]**. Web hizmeti **Pano** toplu iş yürütme için API anahtarını ve API Yardım sayfası görüntülenir. Yalnızca Batch yürütme yöntemi, eğitilen modeller oluşturmak için kullanılabilir.

**Yeni web hizmeti**

Deneme tuvalinin altındaki tıklatın **Web hizmetinin ayarı** seçip **Web hizmeti dağıtma [Yeni]**. Web hizmeti Azure Machine Learning Web Hizmetleri portalını dağıtma web hizmeti sayfası açılır. Web hizmetiniz için bir ad yazın ve ödeme planı seçin ve ardından tıklayın **Dağıt**. Yalnızca Batch yürütme yöntemi eğitilmiş modeller oluşturmak için kullanılabilir.

Her iki durumda da, denemeyi çalıştırma, tamamlandıktan sonra elde edilen iş akışı aşağıdaki gibi görünmelidir:

![Çalıştırdıktan sonra elde edilen iş akışı.][4]



## <a name="retrain-the-model-with-new-data-using-bes"></a>BES kullanarak yeni verilerle modeli yeniden eğitme
Bu örnek için kullanmakta olduğunuz C# yeniden eğitme uygulama oluşturmak için. Python veya R örnek kod, bu görevi gerçekleştirmek için de kullanabilirsiniz.

Yeniden eğitme API'lerini çağırmak için:

1. Oluşturma bir C# konsol uygulaması Visual Studio'da: **Yeni** > **proje** > **Visual C#**   >  **Windows Klasik Masaüstü**  >   **Konsol uygulaması (.NET Framework)**.
2. Machine Learning Web hizmetini portalında oturum açın.
3. Klasik web hizmeti ile çalışıyorsanız, tıklayın **Klasik Web Hizmetleri**.
   1. Çalıştığınız web hizmetine tıklayın.
   2. Varsayılan uç noktaya tıklayın.
   3. Tıklayın **tüketen**.
   4. Sayfanın alt kısmında **Tüket** sayfasında **örnek kodu** bölümünde **Batch**.
   5. Bu yordamın 5 adıma geçin.
4. Yeni bir web hizmeti ile çalışıyorsanız, tıklayın **Web Hizmetleri**.
   1. Çalıştığınız web hizmetine tıklayın.
   2. Tıklayın **tüketen**.
   3. AT Tüket altındaki sayfasında **örnek kodu** bölümünde **Batch**.
5. Örneği kopyalamak C# kod için toplu yürütme ve ad alanı korunur sağlamaktan Program.cs dosyasına yapıştırın.

Yorumlar bölümünde belirtildiği gibi System.NET.http.Formatting Nuget paketini ekleyin. Microsoft.WindowsAzure.Storage.dll başvuru eklemek için önce Microsoft Azure depolama hizmetleri için istemci kitaplığı yüklemeniz gerekebilir. Daha fazla bilgi için [Windows depolama hizmetleri](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>Apikey bildirimini güncelleştirin
Bulun **apikey** bildirimi.

    const string apiKey = "abc123"; // Replace this with the API key for the web service

İçinde **temel tüketim bilgileri** bölümünü **Tüket** sayfasında birincil anahtarını bulun ve kopyalayıp **apikey** bildirimi.

### <a name="update-the-azure-storage-information"></a>Azure depolama bilgilerini güncelleştir
BES örnek kodu (örneğin "C:\temp\CensusIpnput.csv") yerel bir sürücüden bir dosyayı Azure Storage'a yükler, işler ve sonuçları, Azure Depolama'ya geri yazar.  

Bu görevi gerçekleştirmek için Klasik Azure portalı ve güncelleştirme karşılık gelen değerleri kodda depolama hesabınızın depolama hesabı adı, anahtar ve kapsayıcı bilgileri almanız gerekir. 

1. Klasik Azure portalında oturum açın.
2. Sol gezinti sütununda **depolama**.
3. Depolama hesapları listesinden bir retrained modelini depolamak için seçin.
4. Sayfanın en altında tıklayın **erişim anahtarlarını Yönet**.
5. Kopyalayıp kaydedin **birincil erişim anahtarı** ve iletişim kutusunu kapatın. 
6. Sayfanın üst kısmında tıklayın **kapsayıcıları**.
7. Var olan bir kapsayıcı seçin veya yeni bir tane oluşturun ve adını kaydedin.

Bulun *StorageAccountName*, *StorageAccountKey*, ve *StorageContainerName* bildirimleri ve Azure Portalı'ndan kaydedilmiş değerleri güncelleştirin.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name

Ayrıca, giriş dosyası, kodda belirttiğiniz konumda kullanılabilir olduğundan emin olmalısınız. 

### <a name="specify-the-output-location"></a>Çıkış konumunu belirtme
Çıkış konumu istek yükünde belirtirken, dosya uzantısını belirtilen *RelativeLocation* ilearner belirtilmelidir. 

Aşağıdaki örneğe bakın:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

> [!NOTE]
> Çıkış konumlarınıza adları web hizmeti çıkış modüllerinin eklediğiniz ölçütüne göre bu izlenecek yolda gösterilenlerden farklı olabilir. İki çıkışı ile bu eğitim denemesini ayarladıktan sonra sonuçlar her ikisi için depolama konumu bilgilerini içerir.  
> 
> 

![Çıktı yeniden eğitme][6]

Diyagram 4: Yeniden eğitme çıktı.

## <a name="evaluate-the-retraining-results"></a>Yeniden eğitme sonuçları değerlendirin
Uygulamayı çalıştırdığınızda, çıktı değerlendirme sonuçlarını erişmek gerekli URL'si ve SAS belirteci içerir.

Performans sonuçlarını retrained modelinin birleştirerek gördüğünüz *BaseLocation*, *RelativeLocation*, ve *SasBlobToken* içinçıkışsonuçlarından*output2* (önceki yeniden eğitme çıkış resimde gösterildiği gibi) ve tam URL'sini tarayıcınızın adres çubuğuna yapıştırma.  

Yeni eğitim modeli yeterince iyi var olan dosyayla gerçekleştirip gerçekleştirmediğini belirlemek için sonuçları inceleyin.

Kopyalama *BaseLocation*, *RelativeLocation*, ve *SasBlobToken* çıktı sonuçları, bunları yeniden eğitme işlemi sırasında kullanın.

## <a name="next-steps"></a>Sonraki adımlar
Tıklayarak Tahmine dayalı web hizmetini dağıttıysanız **Web hizmeti dağıtma [Klasik]**, bkz: [bir Klasik web hizmetini yeniden eğitme](retrain-a-classic-web-service.md).

Tıklayarak Tahmine dayalı web hizmetini dağıttıysanız **Web hizmeti dağıtma [Yeni]**, bkz: [Machine Learning Yönetimi cmdlet'lerini kullanarak yeni bir web hizmetini yeniden eğitme](retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
