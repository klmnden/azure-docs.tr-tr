---
title: İçeri/dışarı aktarma veri web hizmetleri - Azure Machine Learning Studio | Microsoft Docs
description: Bir web hizmetinden veri alıp göndermek için verileri içeri aktar ve dışarı veri modülleri'ni kullanmayı öğrenin.
services: machine-learning
documentationcenter: ''
author: xiaoharper
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 3a7ac351-ebd3-43a1-8c5d-18223903d08e
ms.service: machine-learning
ms.subservice: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 28d16bce6dbb5063c085e8c4393777ee9d152768
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60345150"
---
# <a name="deploy-azure-machine-learning-studio-web-services-that-use-data-import-and-data-export-modules"></a>Veri içeri aktarma ve veri dışarı aktarma modüllerini kullanan Azure Machine Learning Studio web hizmetlerini dağıtma

Tahmine dayalı bir deneme oluştururken, genellikle bir web hizmeti giriş ve çıkış ekleyin. Denemeyi dağıttığınızda, tüketicilerin gönderebilir ve giriş ve çıkışları aracılığıyla web hizmetinden veri al. Bazı uygulamalar için bir tüketicinin verileri bir veri akışı'ndan kullanılabilir veya Azure Blob Depolama gibi bir dış veri kaynağı zaten bulunan. Bu gibi durumlarda, web hizmeti giriş ve çıkışları kullanarak verileri okumasına ve yazmasına gerek yoktur. Bunlar, bunun yerine, toplu işlem yürütme hizmeti (BES) veri kaynağından verileri içeri aktarma modülü kullanarak verileri okumak için kullanabilir ve puanlama sonuç verileri dışarı aktarma modülünü kullanarak farklı veri konumuna yazma.

Verileri içeri aktar ve dışarı aktarma veri modülleri, okuma ve konumlar gibi HTTP, Hive sorgusu, bir Azure SQL veritabanı, Azure tablo depolama, Azure Blob Depolama, veri akışı aracılığıyla bir Web URL'si sağlayan çeşitli veri veya bir şirket içi SQL veritabanı için yazma.

Bu konuda kullanır "örnek 5: Eğitim, Test, değerlendirmek için ikili sınıflandırma: Yetişkinlere yönelik veri kümeleri"örnek veri kümesini censusdata adlı bir Azure SQL tablosuna zaten yüklendi varsayar.

## <a name="create-the-training-experiment"></a>Eğitim denemenizi oluşturma
Açtığınızda "örnek 5: Eğitim, Test, değerlendirmek için ikili sınıflandırma: Yetişkinlere yönelik veri kümeleri"örnek örnek yetişkinlere yönelik Görselleştirmenizdeki gelir ikili sınıflandırma veri kümesini kullanır. Ve deneme tuvaline aşağıdaki görüntüye benzer olacaktır:

![Denemeyi ilk yapılandırması.](./media/web-services-that-use-import-export-modules/initial-look-of-experiment.png)

Azure SQL tablosundan verileri okumak için:

1. Veri kümesi modülü silin.
2. İçeri aktarma bileşenleri arama kutusuna yazın.
3. Sonuç listesinden eklemek bir *verileri içeri aktarma* modülünü deneme tuvaline.
4. Çıkışına bağlayın *verileri içeri aktarma* modülü giriş, *eksik verileri temizleme* modülü.
5. Özellikler bölmesinde seçin **Azure SQL veritabanı** içinde **veri kaynağı** açılır.
6. İçinde **veritabanı sunucu adı**, **veritabanı adı**, **kullanıcı adı**, ve **parola** alanlar için uygun bilgi girin, Veritabanı.
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
8. Deneme tuvalinin altındaki tıklatın **çalıştırma**.

## <a name="create-the-predictive-experiment"></a>Tahmine dayalı deneme oluşturma
Sonraki web hizmetini dağıttığınız Tahmine dayalı denemeye ayarlayın.

1. Deneme tuvalinin altındaki tıklatın **Web hizmetinin ayarı** seçip **Tahmine dayalı Web hizmeti [önerilen]**.
2. Kaldırma *Web hizmeti girişini* ve *Web hizmeti çıkış modülleri* Tahmine dayalı denemeye öğesinden.
3. Dışarı aktarma bileşenleri arama kutusuna yazın.
4. Sonuç listesinden eklemek bir *verileri dışarı aktarma* modülünü deneme tuvaline.
5. Çıkışına bağlayın *Score Model* modülü giriş, *verileri dışarı aktarma* modülü.
6. Özellikler bölmesinde seçin **Azure SQL veritabanı** veri hedef açılır.
7. İçinde **veritabanı sunucu adı**, **veritabanı adı**, **Server kullanıcı hesabı adı**, ve **Server kullanıcı hesabı parolası** alanları girin Veritabanınız için uygun bilgileri.
8. İçinde **virgülle ayrılmış listesi sütunları kaydedilecek** alanında, Puanlanmış etiketler yazın.
9. İçinde **veri tablo adı alanı**, dbo yazın. ScoredLabels. Tablo mevcut değilse, denemeyi çalıştırma ya da web hizmeti olarak adlandırılır oluşturulur.
10. İçinde **virgülle ayrılmış listesi datatable sütunları** alan, ScoredLabels yazın.

Son web hizmetini çağıran bir uygulama yazdığınızda, farklı giriş sorgusu veya hedef tablo, çalışma zamanında belirtmek isteyebilirsiniz. Bu girişler ve çıkışlar yapılandırmak için Web hizmeti parametrelerini ayarlamak için kullanılmakta olan *verileri içeri aktarma* Modülü *veri kaynağı* özelliği ve *verileri dışarı aktarma* modu verileri hedef özelliği.  Web hizmeti parametreleri hakkında daha fazla bilgi için bkz. [Azure Machine Learning studio Web hizmeti parametreleri giriş](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) Cortana Intelligence ve Machine Learning Web günlüğü.

Sorgu Al ve hedef tablo için Web hizmeti parametreleri yapılandırmak için:

1. İçin Özellikler bölmesindeki *verileri içeri aktarma* modülü, üstteki simgeye tıklayın sağ **veritabanı sorgusu** seçin ve alan **web hizmeti parametre**.
2. İçin Özellikler bölmesindeki *verileri dışarı aktarma* modülü, üstteki simgeye tıklayın sağ **veri tablosu adı** seçin ve alan **web hizmeti parametre**.
3. Sayfanın alt kısmında *verileri dışarı aktarma* modülü Özellikler bölmesi, **Web hizmeti parametrelerini** bölümünde veritabanı sorgusu tıklayın ve sorguyu yeniden adlandır.
4. Tıklayın **veri tablosu adı** ve yeniden adlandırmak **tablo**.

İşiniz bittiğinde, denemenizi aşağıdaki görüntüye benzer olmalıdır:

![Son deneme görünümünü.](./media/web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Artık denemeyi bir web hizmeti olarak dağıtabilirsiniz.

## <a name="deploy-the-web-service"></a>Web hizmetini dağıtma
Klasik veya yeni bir web hizmeti için dağıtabilirsiniz.

### <a name="deploy-a-classic-web-service"></a>Klasik Web hizmetini dağıtma
Klasik Web hizmeti olarak dağıtma ve bunu kullanmak için bir uygulama oluşturmak için:

1. Deneme tuvalinin alt kısmında, Çalıştır'a tıklayın.
2. Çalıştırma tamamlandığında tıklayın **Web hizmeti Dağıt** seçip **Web hizmeti dağıtma [Klasik]**.
3. Web hizmeti panosunda API anahtarınızı bulun. Kopyalayın ve daha sonra kullanmak üzere kaydedin.
4. İçinde **varsayılan uç nokta** tablo, tıklayın **toplu iş yürütme** API Yardım sayfası açmak için bağlantı.
5. Visual Studio'da oluşturma bir C# konsol uygulaması: **Yeni** > **proje** > **Visual C#**   >  **Windows Klasik Masaüstü**  >   **Konsol uygulaması (.NET Framework)**.
6. API Yardım sayfasında bulabilirsiniz **örnek kodu** sayfanın alt kısmındaki bölümde.
7. Kopyalama ve yapıştırma C# Program.cs dosyanıza örnek kod ve blob depolama için tüm başvuruları kaldırın.
8. Değerini güncelleştirin *apiKey* değişken daha önce kaydedilmiş API anahtarına sahip.
9. İstek bildirimi bulun ve geçirilen bir Web hizmeti parametreleri güncelleştirin *verileri içeri aktarma* ve *verileri dışarı aktarma* modüller. Bu durumda, özgün sorguyu ancak yeni bir tablo adı tanımlayın.

        var request = new BatchExecutionRequest()
        {
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. Uygulamayı çalıştırın.

Tamamlanma çalıştırma, yeni bir tablo Puanlama sonuçlarını içeren bir veritabanına eklenir.

### <a name="deploy-a-new-web-service"></a>Yeni bir Web hizmetini dağıtma

> [!NOTE]
> Yeni bir web hizmetini dağıtmak için yeterli olan aboneliği, web hizmetini dağıtma olması gerekir. Daha fazla bilgi için [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönetme](manage-new-webservice.md).

Yeni bir Web hizmeti dağıtma ve bunu kullanmak için bir uygulama oluşturmak için:

1. Deneme tuvalinin altındaki tıklatın **çalıştırma**.
2. Çalıştırma tamamlandığında tıklayın **Web hizmeti Dağıt** seçip **Web hizmeti dağıtma [Yeni]**.
3. Deneme dağıtma sayfasında, web hizmetiniz için bir ad girin ve fiyatlandırma planı seçin ve ardından'a tıklayın **Dağıt**.
4. Üzerinde **hızlı** sayfasında **Tüket**.
5. İçinde **örnek kodu** bölümünde **Batch**.
6. Visual Studio'da oluşturma bir C# konsol uygulaması: **Yeni** > **proje** > **Visual C#**   >  **Windows Klasik Masaüstü**  >   **Konsol uygulaması (.NET Framework)**.
7. Kopyalama ve yapıştırma C# örnek kodu Program.cs dosyanıza.
8. Değerini güncelleştirin *apiKey* değişken ile **birincil anahtar** bulunan **temel tüketim bilgileri** bölümü.
9. Bulun *scoreRequest* bildirimi ve geçirilen bir Web hizmeti parametreleri güncelleştirin *verileri içeri aktarma* ve *verileri dışarı aktarma* modüller. Bu durumda, özgün sorguyu ancak yeni bir tablo adı tanımlayın.

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

