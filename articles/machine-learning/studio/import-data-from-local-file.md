---
Başlık: Dosya titleSuffix veri içeri aktar: Azure Machine Learning Studio açıklaması: Azure Machine Learning Studio'da eğitim veri dosyasına sabit sürücünüzden karşıya yüklemeyi öğrenin. Bu çalışma alanında bir veri kümesi modülü oluşturur.
Hizmetler: Makine öğrenimi ms.service: Makine öğrenimi ms.component: studio ms.topic: makale

author: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18 ms.date: 03/20/2017
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-azure-machine-learning-studio"></a>Azure Machine Learning Studio'ya sabit sürücünüzde bir dosyadan eğitim verilerini içeri aktarma

Azure Machine Learning Studio'da eğitim verilerini olarak kullanılacak sabit sürücünüzden bir veri dosyası karşıya yüklemeyi öğrenin. Veri dosyasını içeri aktararak, çalışma alanınızda kullanıma hazır bir veri kümesi modülü vardır.

## <a name="steps-to-import-data-from-a-local-file"></a>Yerel bir dosyadan veri içeri aktarma adımları
Bir yerel sabit sürücünüzden verileri içeri aktarmak için aşağıdakileri yapın:

1. Tıklayın **+ yeni** Machine Learning Studio penceresinin alt kısmındaki.
2. Seçin **veri KÜMESİ** ve **yerel DOSYADAN**.
3. İçinde **yeni bir veri kümesi karşıya** iletişim kutusunda, karşıya yüklemek istediğiniz dosyaya göz atın
4. Bir ad girin, veri türünü tanımlar ve isteğe bağlı olarak bir açıklama girin. Bir açıklama önerilen - veri gelecekte kullanırken unutmayın istediğiniz veriler hakkında herhangi bir özelliği kaydetmenize olanak sağlar.
5. Onay kutusunu **bu var olan bir dataset yeni sürümüdür** , var olan bir dataset yeni veriler ile güncelleştirmenizi sağlar. Bu onay kutusuna tıklayın ve ardından var olan bir veri kümesinin adını girin.

![Yeni bir veri kümesi karşıya yükleme](./media/import-data-from-local-file/upload-dataset.png)

Karşıya yükleme sırasında dosyanız karşıya yükleniyor bir ileti görürsünüz. Karşıya yükleme, veri boyutu ve hizmete bağlantınızın hızına bağlı zaman. Dosya uzun sürmesi biliyorsanız beklerken Machine Learning Studio içinde başka şeyler yapabilirsiniz. Ancak, tarayıcının kapanması verileri karşıya yükleme başarısız olmasına neden olur.

## <a name="dataset-module-is-ready-for-use"></a>Veri kümesi modülü kullanıma hazırdır
Verilerinizi karşıya yüklendikten sonra bir veri kümesi modülde depolanır ve çalışma alanınızdaki tüm deneme sunulur.

Bir deney düzenlerken, oluşturduğunuz veri kümeleri bulabilirsiniz **My veri kümeleri** altında listesinde **kaydedilmiş veri kümeleri** modül paletindeki listesi. Daha fazla analiz ve makine öğrenimi için veri kümesini kullanmak istediğinizde veri kümesini deneme tuvaline sürükleyip bırakabilirsiniz.
