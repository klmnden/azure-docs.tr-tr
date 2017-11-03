---
title: "İçeri/dışarı aktarma veri Azure Machine Learning web Hizmetleri kullanılarak | Microsoft Docs"
description: "Veri içeri aktarma ve dışarı aktarma veri modülleri göndermek ve bir web hizmetinden veri almak için nasıl kullanılacağını öğrenin."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: 3a7ac351-ebd3-43a1-8c5d-18223903d08e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 40e1dbf4fc1618deec5e7f85fb956f2881e08319
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Veri Alma ve Veri Gönderme modüllerini kullanan Azure ML web hizmetleri dağıtma

Tahmine dayalı bir deneme oluştururken, bir web hizmeti giriş ve çıkış genellikle ekleyin. Denemeyi dağıttığınızda, Tüketicileri gönderebilir ve web hizmeti girişleri ve çıkışları aracılığıyla veri almak. Bazı uygulamalar için bir tüketicinin verileri veri akışlarından kullanılabilir veya zaten Azure Blob Depolama alanı gibi bir dış veri kaynağında bulunan. Bu durumlarda, bunlar web hizmeti girişleri ve çıkışları kullanarak veri okumak veya yazmak zorunda değilsiniz. Bunlar, bunun yerine, toplu yürütme hizmeti (BES) bir veri içeri aktarma modülü kullanılarak veri kaynağından verileri okumak için kullanabilir ve puanlama sonuçları dışarı aktarma veri modülünü kullanarak farklı veri konuma yazma.

Veri içeri aktarma ve dışarı aktarma veri modüller, okuma ve konumlar gibi Web URL HTTP, Hive sorgusu, bir Azure SQL veritabanı, Azure Table storage, Azure Blob Depolama, veri akışı aracılığıyla sağlayan çeşitli veri veya şirket içi SQL veritabanına yazma.

Bu konuda kullanır "örnek 5: Eğitimi, Test, değerlendir ikili sınıflandırma: yetişkinlere yönelik veri kümesi" örnek ve veri kümesi zaten yüklü censusdata adlı bir Azure SQL tablosuna varsayar.

## <a name="create-the-training-experiment"></a>Eğitim denemenizi oluşturma
Açtığınızda "örnek 5: Eğitimi, Test, değerlendir ikili sınıflandırma: yetişkinlere yönelik veri kümesi" örnek örnek yetişkin Census gelir ikili sınıflandırma veri kümesi kullanır. Ve deneme tuvalde aşağıdaki görüntüye şuna benzer:

![Deneme'nin ilk yapılandırması.](./media/web-services-that-use-import-export-modules/initial-look-of-experiment.png)

Azure SQL tablosundan verileri okumak için:

1. Veri kümesi modülü silin.
2. Bileşenleri arama kutusunda, içeri aktarma yazın.
3. Sonuçları listesinden bir *veri içeri aktarma* modülünü deneme tuvaline.
4. Çıkışına bağlayın *veri içeri aktarma* modülü giriş, *Clean Missing Data* modülü.
5. Özellikler bölmesinde seçin **Azure SQL veritabanı** içinde **veri kaynağı** açılır.
6. İçinde **veritabanı sunucusu adı**, **veritabanı adı**, **kullanıcı adı**, ve **parola** alanlar için uygun bilgileri girin, Veritabanı.
7. Veritabanı sorgusu alanına şu sorguyu girin.
   
     [yaş] seçin
   
        [workclass],
        [fnlwgt],
        [education],
        [education-num],
        [marital-status],
        [occupation],
        [relationship],
        [race],
        [sex],
        [capital-gain],
        [capital-loss],
        [hours-per-week],
        [native-country],
        [income]
     dbo.censusdata;
8. Deneme tuvalinin altındaki tıklatın **çalıştırmak**.

## <a name="create-the-predictive-experiment"></a>Tahmine dayalı deneme oluşturma
Ardından, web hizmetini dağıtma Tahmine dayalı denemeye ayarlayın.

1. Deneme tuvalinin altındaki tıklatın **Web hizmetinin ayarı** seçip **Tahmine dayalı Web hizmeti [önerilen]**.
2. Kaldırma *Web hizmeti girişi* ve *Web hizmeti çıkış modülleri* Tahmine dayalı denemeye gelen. 
3. Bileşenleri arama kutusunda, dışa aktarma yazın.
4. Sonuçları listesinden bir *verileri dışa aktar* modülünü deneme tuvaline.
5. Çıkışına bağlayın *Score Model* modülü giriş, *verileri dışa aktar* modülü. 
6. Özellikler bölmesinde seçin **Azure SQL veritabanı** veri hedef açılır.
7. İçinde **veritabanı sunucusu adı**, **veritabanı adı**, **Server kullanıcı hesabı adı**, ve **Server kullanıcı hesabı parolasını** alanları girin Veritabanınız için uygun bilgileri.
8. İçinde **virgülle ayrılmış kaydedilecek sütunlar listesi** alan, skoru etiketleri yazın.
9. İçinde **veri tablo adı alanı**, dbo yazın. ScoredLabels. Tablo mevcut değilse, deneme çalıştırdığınızda veya web hizmeti adlı oluşturulur.
10. İçinde **virgülle ayrılmış datatable sütunlar listesi** alan, ScoredLabels yazın.

Son web hizmeti çağıran bir uygulama yazdığınızda, farklı giriş sorgu veya hedef tablo çalışma zamanında belirtmek isteyebilirsiniz. Bu girişleri ve çıkışları yapılandırmak için ayarlamak için Web hizmeti parametreleri özelliğini kullanın *veri içeri aktarma* Modülü *veri kaynağı* özelliği ve *verileri dışa aktar* modu verileri hedef özelliği.  Web hizmeti parametreleri hakkında daha fazla bilgi için bkz: [AzureML Web hizmeti parametreleri girişi](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) Cortana Intelligence ve makine öğrenme blogu.

İçeri aktarma sorgu ve hedef tablo için Web hizmeti parametreleri yapılandırmak için:

1. İçin Özellikler bölmesinde *veri içeri aktar* modülü, üst simgeyi tıklatın sağ **veritabanı sorgusu** alan ve seçin **web hizmeti parametre**.
2. İçin Özellikler bölmesinde *verileri dışa aktar* modülü, üst simgeyi tıklatın sağ **veri tablo adı** alan ve seçin **web hizmeti parametre**.
3. Alt kısmındaki *verileri dışa aktar* modülü Özellikler bölmesi, **Web hizmeti parametreleri** bölümünde veritabanı sorgusu tıklayın ve sorguyu yeniden adlandırın.
4. Tıklatın **veri tablo adı** ve yeniden adlandırmak **tablo**.

İşiniz bittiğinde, denemenizi aşağıdaki görüntüye benzer görünmelidir:

![Deneme son görünümünü.](./media/web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Şimdi deneme bir web hizmeti olarak dağıtabilirsiniz.

## <a name="deploy-the-web-service"></a>Web hizmetini dağıtma
Klasik veya yeni web hizmeti için dağıtabilirsiniz.

### <a name="deploy-a-classic-web-service"></a>Klasik Web hizmetini dağıtma
Klasik Web hizmeti olarak dağıtabilir ve bunu kullanmak için bir uygulama oluşturmak için:

1. Deneme tuvalinin altındaki Çalıştır'ı tıklatın.
2. Çalıştır tamamlandığında tıklayın **Web hizmeti Dağıt** seçip **Web hizmeti Dağıt [Klasik]**.
3. Web hizmeti panosunda API anahtarınızı bulun. Kopyalayın ve daha sonra kullanmak üzere kaydedin.
4. İçinde **varsayılan uç nokta** tablo, tıklatın **toplu iş yürütme** API Yardım sayfasında açmak için bağlantı.
5. Visual Studio'da bir C# konsol uygulaması oluşturun: **yeni** > **proje** > **Visual C#** > **Windows Klasik Masaüstü** > **konsol uygulaması (.NET Framework)**.
6. API Yardım sayfasında Bul **örnek kod** sayfanın alt kısmına.
7. Kopyalama ve C# örnek kodu, Program.cs dosyasına yapıştırın ve blob depolama alanına yönelik tüm başvuruları kaldırın.
8. Değerini güncelleştirmek *apikey ile yapılan* daha önce kaydedilmiş API anahtarı ile değişken.
9. İstek bildirimini bulun ve Web hizmeti için geçirilen parametrelerin değerlerini güncelleştirme *veri içeri aktarma* ve *verileri dışa aktar* modüller. Bu durumda, özgün sorgu kullanır, ancak yeni bir tablo adı tanımlayın.
   
        var request = new BatchExecutionRequest() 
        {           
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. Uygulamayı çalıştırın. 

Çalıştır tamamlama sonrasında yeni bir tablo Puanlama sonuçlarını içeren veritabanına eklenir.

### <a name="deploy-a-new-web-service"></a>Yeni bir Web hizmetini dağıtma

> [!NOTE] 
> Yeni bir web hizmeti dağıtmak için yeterli izinleri olan Abonelikteki, web hizmetini dağıtma olmalıdır. Daha fazla bilgi için bkz: [Azure Machine Learning Web Hizmetleri Portalı'nı kullanarak bir Web hizmetini yönetmek](manage-new-webservice.md). 

Yeni bir Web hizmeti olarak dağıtabilir ve bunu kullanmak için bir uygulama oluşturmak için:

1. Deneme tuvalinin altındaki tıklatın **çalıştırmak**.
2. Çalıştır tamamlandığında tıklayın **Web hizmeti Dağıt** seçip **Web hizmeti dağıtma [Yeni]**.
3. Deneme dağıtma sayfasında, web hizmetiniz için bir ad girin ve bir fiyatlandırma planı seçin ve ardından **dağıtma**.
4. Üzerinde **Hızlı Başlangıç** sayfasında, **Tüket**.
5. İçinde **örnek kod** 'yi tıklatın **toplu**.
6. Visual Studio'da bir C# konsol uygulaması oluşturun: **yeni** > **proje** > **Visual C#** > **Windows Klasik Masaüstü** > **konsol uygulaması (.NET Framework)**.
7. Kopyalama ve C# örnek kodu, Program.cs dosyasına yapıştırın.
8. Değerini güncelleştirmek *apikey ile yapılan* ile değişken **birincil anahtar** bulunan **temel tüketim bilgileri** bölümü.
9. Bulun *scoreRequest* bildirimi ve Web hizmeti için geçirilen parametrelerin değerlerini güncelleştirme *veri içeri aktarma* ve *verileri dışa aktar* modüller. Bu durumda, özgün sorgu kullanır, ancak yeni bir tablo adı tanımlayın.
   
        var scoreRequest = new
        {       
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };
10. Uygulamayı çalıştırın. 

