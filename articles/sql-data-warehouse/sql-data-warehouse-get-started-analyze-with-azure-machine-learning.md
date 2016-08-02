<properties
   pageTitle="Azure Machine Learning ile veri çözümleme | Microsoft Azure"
   description="Çözüm geliştirmek üzere Azure SQL Data Warehouse ile Azure Machine Learning kullanma öğreticisi."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="shivaniguptamsft"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/18/2016"
   ms.author="shigu;barbkess;sonyama"/>

# Azure Machine Learning ile veri çözümleme

> [AZURE.SELECTOR]
- [Power BI][]
- [Azure Machine Learning][]

Bu öğreticide Azure SQL Data Warehouse verilerinizi kullanarak Azure Machine Learning ile tahmine dayalı Machine Learning modeli oluşturma işlemini nasıl gerçekleştireceğiniz gösterilecek. Biz bu öğreticide bir müşterinin bisiklet alma olasılığı hakkında tahminde bulunarak Adventure Works adlı bisiklet satış mağazası için hedeflenen bir pazarlama kampanyası oluşturacağız.

> [AZURE.VIDEO integrating-azure-machine-learning-with-azure-sql-data-warehouse]

## Önkoşullar
Bu öğreticide ilerleyebilmeniz için, şunlar gereklidir:

- AdventureWorksDW örnek veritabanı bulunan bir SQL Data Warehouse

[SQL Data Warehouse oluşturma][] makalesinde örnek verilerle bir veritabanını nasıl hazırlayacağınızı gösterilmiştir. Zaten bir SQL Data Warehouse veritabanınız var ancak örnek verileriniz yoksa [örnek verileri el ile yükleyebilirsiniz][]


## 1. Adım: Verileri Alma
Biz, AdventureWorksDW veritabanında bulunan dbo.vTargetMail görünümündeki verileri okuyacağız.

1. [Azure Machine Learning Studio][]'da oturum açıp denemelerim öğesine tıklayın.
2. **+NEW (+YENİ)** düğmesine tıklayıp **Blank Experiment (Boş Deneme)** öğesini seçin.
3. Denemeniz için bir ad girin: Hedeflenen Pazarlama.
4. Modüller bölmesindeki **Reader (Okuyucu)** modülünü tuvale sürükleyin.
5. Özellikler bölmesinde SQL Data Warehouse veritabanınıza ilişkin bilgileri belirtin.
6. İstediğiniz verileri okumak için veritabanı **sorgusunu** belirtin.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Deneme tuvalinin altında bulunan **Run (Çalıştır)** düğmesine tıklayarak denemeyi çalıştırın.
![Denemeyi çalıştırma][1]


Denemeyi çalıştırma işlemi başarıyla sonlandıktan sonra, Okuyucu modülünün altındaki çıkış bağlantı noktasına tıklayıp içeri aktarılan verileri görmek için **Visualiza (Görselleştir)** seçeneğine tıklayın.
![İçeri aktarılan verileri görüntüleme][3]


## 2. Adım: Verileri Temizleme
Biz modelle ilgili olmayan bazı sütunları kaldıracağız.

1. **Project Columns (Proje Sütunları)** modülünü tuvale sürükleyin.
2. Hangi sütunları kaldırmak istediğinizi belirtmek için Properties (Özellikler) bölmesindeki **Launch column selector (Sütun seçiciyi başlat)** öğesine tıklayın.
![Proje Columns (Proje Sütunları)][4]

3. Şu iki sütunu dışlayın: CustomerAlternateKey ve GeographyKey.
![Gereksiz sütunları kaldırma][5]


## 3. Adım: Model Oluşturma
Biz verilerin %80'ini Machine Learning modelini eğitmek ve %20'sini de modeli test etmek üzere kullanacak şekilde 80'e 20 oranında böleceğiz. Bu ikili sınıflandırma sorunu için "İki Sınıflı" algoritmalardan yararlanacağız.

1. **Split (Bölme)** modülünü tuvale sürükleyin.
2. Properties (Özellikler) bölmesindeki ilk çıkış veri kümesinde bulunan satırlar için kesir değerini 0,8 olarak girin.
![Verileri eğitim ve test kümesi olarak bölme][6]
3. **Two-Class Boosted Decision Tree (İki Sınıflı Gelişmiş Karar Ağacı)** modülünü tuvale sürükleyin.
4. **Train Model (Model Eğitme)** modülünü tuvale sürükleyip girişleri belirtin. Ardından Özellikler bölmesindeki **Sütun seçiciyi başlat** öğesine tıklayın.
      - İlk giriş: ML algoritması
      - İkinci giriş: Algoritmayı eğitmeye yönelik veriler.
![Train Model (Model Eğitme) modülünü bağlama][7]
5. Tahminde bulunulacak sütun olarak **BikeBuyer** sütununu seçin.
![Tahminde bulunulacak sütunu seçme][8]


## 4. Adım: Model Puanlama
Şimdi modelin test verileri üzerindeki işlevini test edeceğiz. Hangisinin daha iyi sonuç verdiğini görmek üzere kendi seçtiğimiz algoritmayla başka bir algoritmayı karşılaştıracağız.

1. **Score Model (Model Puanlama)** modülünü tuvale sürükleyin.
    İlk giriş: Eğitilmiş model İkinci giriş: Test verileri ![Modeli puanlama][9]
2. **Two-Class Bayes Point Machine (İki Sınıflı Bayes Noktası Makinesi)** modülünü deneme tuvaline sürükleyin. Bu algoritma ile Two-Class Boosted Decision Tree'nin (İki Sınıflı Gelişmiş Karar Ağacı'nın) işlevlerini karşılaştıracağız.
3. Train Model (Model Eğitme) ve Score Model (Model Puanlama) modüllerini kopyalayıp tuvale yapıştırın.
4. İki algoritmayı karşılaştırmak için **Evaluate Model (Model Değerlendirme)** modülünü tuvale sürükleyin.
5. Denemeyi çalıştırmak için **Run (Çalıştır)** düğmesine basın.
![Denemeyi çalıştırma][10]
6. Evaluate Model (Model Değerlendirme) modülünün altında bulunan çıkış bağlantı noktasına ve ardından Visualize (Görselleştir) düğmesine tıklayın.
![Değerlendirme sonuçlarını görselleştirme][11]

Sağlanan ölçümler şunlardır: ROC eğrisi, duyarlık geri çekme diyagramı ve yükseltme eğrisi. Bu ölçümlere bakarak birinci modelin ikinciye göre daha iyi sonuç verdiğini görebiliriz. Birinci modelin nasıl bir tahminde bulunduğunu görmek için Model Puanlama modülünün çıkış bağlantı noktasına ve ardından Visualize (Görselleştir) düğmesine tıklayın.
![Puanlama sonuçlarını görselleştirme][12]

Test veri kümenize iki sütunun daha eklendiğini göreceksiniz.

- Puanlanmış Olasılıklar: müşterinin bir bisiklet alıcısı olma olasılığı.
- Puanlanmış Etiketler: model tarafından yapılan sınıflandırma; bisiklet alıcısı (1) veya değil (0). Etiketlemeye ilişkin bu olasılık eşiği %50 olarak belirlenmiş olup ayarlanabilir.

BikeBuyer (gerçek) sütununu Puanlanmış Etiketler (tahmin) ile karşılaştırarak modelin ne derece iyi sonuç verdiğini görebilirsiniz. Sonraki adımlarda bu modeli yeni müşteriler için tahminde bulunmak üzere kullanabilir, bir web hizmeti olarak yayımlayabilir veya sonuçları sonradan SQL Data Warehouse'a yazabilirsiniz.

## Sonraki Adımlar

Tahmine dayalı makine öğrenimi modellerinin oluşturulmasına ilişkin daha fazla bilgi edinmek için bkz. [Azure'da Machine Learning'e giriş][].

<!--Image references-->
[1]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning Studio]:https://studio.azureml.net/
[Azure'da Machine Learning'e giriş]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[örnek verileri el ile yükleyebilirsiniz]: sql-data-warehouse-get-started-manually-load-samples.md
[SQL Data Warehouse oluşturma]: sql-data-warehouse-get-started-provision.md
[Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Azure Machine Learning]: ./sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md



<!---HONumber=Jun16_HO2-->


