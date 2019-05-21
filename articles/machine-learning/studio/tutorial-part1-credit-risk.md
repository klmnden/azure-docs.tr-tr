---
title: '1. Öğretici: Kredi riski tahmini'
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio'da bir kredi riski değerlendirmesi için Tahmine dayalı analiz çözümü oluşturmayı gösteren ayrıntılı bir öğretici. Bu öğretici üç bölümden öğretici serisinin birinci bölümüdür.  Bu, bir çalışma alanı oluşturun, karşıya yüklemek ve bir deneme oluşturma işlemini göstermektedir.
keywords: kredi riski, tahmine dayalı analiz çözümü, risk değerlendirmesi
author: sdgilley
ms.author: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: tutorial
ms.date: 02/11/2019
ms.openlocfilehash: f9746dae4cdf10a10922be41602f4ecd7f032f5b
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65949799"
---
# <a name="tutorial-1-predict-credit-risk---azure-machine-learning-studio"></a>1. Öğretici: Kredi riskini - Azure Machine Learning Studio tahmin

Bu öğreticide, bir Tahmine dayalı analiz çözümü geliştirme işleminin genişletilmiş bir göz atın. Machine Learning Studio'da basit bir model geliştirmektir.  Ardından bir Azure Machine Learning web hizmeti olarak modeli dağıtacağız.  Bu dağıtılmış modelin yeni verileri kullanarak tahmin yapabileceği. Bu öğretici **üç bölümlü öğretici serisinin birinci kısmında**.

Bir kişinin kredi başvurusunda verdiği bilgilere dayanarak kredi riskini tahmin etmeniz gerektiğini varsayalım.  

Kredi riski değerlendirmesi karmaşık bir sorundur ancak bu öğreticide, bir bit basitleştirir. Microsoft Azure Machine Learning Studio'yu kullanarak Tahmine dayalı analiz çözümü nasıl oluşturabileceğinize dair bir örnek olarak kullanacaksınız. Azure Machine Learning Studio ve Machine Learning web hizmeti bu çözüm için kullanacaksınız.  

Üç bölümü olan Bu öğreticide, genel kullanıma açık kredi risk verileriyle başlayın.  Ardından geliştirin ve Tahmine dayalı bir model eğitip.  Son olarak modeli bir web hizmeti olarak dağıtalım.

Öğreticinin bu bölümünde: 
 
> [!div class="checklist"]
> * Machine Learning Studio çalışma alanı oluşturma
> * Var olan verileri yükleme
> * Deneme oluşturma

Ardından bu deneme için kullanabileceğiniz [bölüm 2'de modellerinin eğitilmesi](tutorial-part2-credit-risk-train.md) ve ardından [bölüm 3 dağıtabilmek](tutorial-part3-credit-risk-deploy.md).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


## <a name="prerequisites"></a>Önkoşullar

Bu öğretici, Machine Learning Studio en az bir kez önce kullandığınız ve makine öğrenimi kavramlarıyla ilgili olduğunu varsayar. Bununla birlikte, bir uzman olduğunuz da varsayılmaz.

Hiç kullanmadıysanız **Azure Machine Learning Studio** hızlı ile başlamak isteyebilirsiniz önce [Azure Machine Learning Studio'da ilk veri bilimi denemeniz Oluştur](create-experiment.md). Bu hızlı başlangıçta Machine Learning Studio'yu ilk kez alır. Öğreticide modülleri sürükleyip denemenize bırakma, birbirine bağlama, denemeyi çalıştırma ve sonuçları görme konularında temel bilgiler verilir.


> [!TIP] 
> Bu öğreticide, geliştirdiğiniz denemenin çalışan bir kopyasını bulabilirsiniz [Azure AI Gallery](https://gallery.azure.ai). Git **[Öğreticisi - kredi tahmin risk](https://gallery.azure.ai/Experiment/Walkthrough-Credit-risk-prediction-1)** tıklatıp **Studio'da Aç** Machine Learning Studio çalışma alanınıza denemenin bir kopyasını indirmek için.
> 


## <a name="create-a-machine-learning-studio-workspace"></a>Machine Learning Studio çalışma alanı oluşturma

Machine Learning Studio kullanmak için bir Microsoft Azure Machine Learning Studio çalışma alanına sahip olmanız gerekir. Bu çalışma alanı, denemeleri oluşturmak, yönetmek ve yayımlamak için ihtiyacınız olan araçları içerir.  

Bir çalışma alanı oluşturmak için bkz [oluşturma ve paylaşma bir Azure Machine Learning Studio çalışma](create-workspace.md).

Machine Learning Studio çalışma alanınızı oluşturduktan sonra açın ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)). Birden fazla çalışma alanı varsa, araç penceresinin sağ üst köşesindeki çalışma alanını seçebilirsiniz.

![Studio çalışma alanı seçin](./media/tutorial-part1-credit-risk/open-workspace.png)

> [!TIP]
> Çalışma alanının sahibi olan, başkalarının çalışma alanına davet ederek üzerinde çalıştığınız denemeleri paylaşabilirsiniz. Machine Learning Studio'da üzerinde bunu yapabilirsiniz **ayarları** sayfası. Microsoft hesabı veya kuruluş hesabı her bir kullanıcı için yeterlidir.
> 
> Üzerinde **ayarları** sayfasında **kullanıcılar**, ardından **daha fazla kullanıcı davet** pencerenin alt kısmındaki.
> 

## <a name="upload"></a>Mevcut verileri yükleme

Kredi riski için Tahmine dayalı bir model geliştirmektir eğitmek ve sonra da modeli test etmek için kullanabileceğiniz veri gerekir. Bu öğreticide, "UCI Statlog (Alman kredi veri) veri kümesi" UC Irvine Machine Learning depodan kullanacaksınız. Burada bulabilirsiniz:  
<a href="https://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">https://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a>

Adlı dosyayı kullanacaksınız **german.data**. Bu dosya yerel sabit sürücünüze indirin.  

**German.data** veri kümesi iade için son 1000 Başvuranlar için 20 değişkenlerin satırları içerir. 20 Bu değişkenleri temsil eden veri kümesinin özellikler kümesi ( *özellik vektör*), her iade başvuran için tanımlayıcı özellikleri sağlar. Her satırda başka bir sütuna Başvuranın hesaplanan kredi riski, düşük kredi riskini ve 300 bir yüksek riskli tanımlanmış 700 başvuran ile temsil eder.

UCI Web sitesi öznitelikleri özellik vektör bu veriler için bir açıklama sağlar. Bu veriler, finansal bilgi, kredi geçmişi, iş durumu ve kişisel bilgileri içerir. Her başvuran için ikili bir derecelendirme düşük olup olmadığını gösteren belirli veya yüksek olan kredi riski. 

Bu veriler, Tahmine dayalı analiz modeli eğitmek için kullanacaksınız. İşiniz bittiğinde, model bir özellik vektör yeni bir kişiye ait kabul edin ve düşük veya yüksek kredi riski olup olmadığını tahmin olmalıdır.  

İlginç bir sürpriz aşağıda verilmiştir.

Açıklama UCI Web sitesinde veri kümesinin ne bahsetmeleri bir kişinin kredi riski misclassify varsa bunu maliyetlerini.
Model gerçekten düşük kredi riski kişi için yüksek kredi riskini tahmin, modeli bir misclassification yaptı.

Ancak ters misclassification Finans kurumu için beş kat daha maliyetlidir: model yüksek kredi riski aslında bir kişi için düşük kredi riskini tahmin durumunda.

Bu nedenle bu türdeki misclassification maliyetini beş kat daha yüksek değerinden başka bir şekilde misclassifying böylece modelinizi eğitmek istiyorsunuz.

Eğitim denemenizi modelinde yüksek kredi riski biriyle temsil eden (beş) girişler çoğaltarak olduğunda bunu yapmak için bir basit yolu. 

Yüksek riskli gerçekten olduklarında model birisi düşük kredi riski olarak misclassifies, daha sonra modeli, aynı misclassification beş kat kez her yinelemesi için yapar. Bu, bu hata eğitim sonuçlarında maliyetini artırır.


### <a name="convert-the-dataset-format"></a>Veri kümesi biçimine dönüştürün

Özgün veri kümesinden boşluk ayrılmış bir biçim kullanır. Daha iyi machine Learning Studio'da bir virgülle ayrılmış değer (CSV) dosyası ile çalışarak, boşluk virgülle değiştirerek veri kümesi dönüştürmeniz.  

Bu verileri dönüştürmek için birçok yolu vardır. Aşağıdaki Windows PowerShell komutunu kullanarak bir yoludur:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

UNIX sed komutunu kullanarak başka bir yoludur:  

    sed 's/ /,/g' german.data > german.csv  

Her iki durumda da adlı dosyadaki verileri virgülle ayrılmış bir sürümünü oluşturduğunuz **german.csv** denemenizde kullanabileceğiniz.

### <a name="upload-the-dataset-to-machine-learning-studio"></a>Machine Learning Studio'ya veri kümesini karşıya yükleme

CSV biçiminde verilerin dönüştürüldükten sonra Machine Learning Studio'ya yüklemesi gerekir. 

1. Machine Learning Studio'ya giriş sayfasını açın ([https://studio.azureml.net](https://studio.azureml.net)). 

2. Menüye tıklayarak ![menü](./media/tutorial-part1-credit-risk/menu.png) penceresinin sol üst köşesinde tıklayın **Azure Machine Learning**seçin **Studio**ve oturum açın.

3. Tıklayın **+ yeni** pencerenin alt kısmındaki.

4. Seçin **veri KÜMESİ**.

5. Seçin **yerel DOSYADAN**.

    ![Yerel bir dosyadan bir veri kümesi Ekle](./media/tutorial-part1-credit-risk/add-dataset.png)

6. İçinde **yeni bir veri kümesi karşıya** iletişim kutusunda, Gözat'a tıklayın ve bulma **german.csv** oluşturduğunuz dosya.

7. Veri kümesi için bir ad girin. Bu öğreticide, çağrı "UCI Almanca kredi kartı verileri".

8. Veri türü için **üst bilgi içeren genel bir CSV dosyası (. nh.csv)**.

9. İsterseniz bir açıklama ekleyin.

10. Tıklayın **Tamam** işaretleyin.  

    ![Veri kümesi karşıya yükleme](./media/tutorial-part1-credit-risk/upload-dataset.png)

Bu, bir deneme kullanabileceğiniz bir veri kümesi modüle verileri yükler.

Tıklayarak Studio'ya yüklediğiniz veri kümelerini yönetebilirsiniz **veri KÜMELERİ** Studio penceresinin sol tarafında için sekmesinde.

![Veri kümelerini Yönet](./media/tutorial-part1-credit-risk/dataset-list.png)

Bir denemenin diğer veri türleri içeri aktarma hakkında daha fazla bilgi için bkz. [eğitim verilerinizi Azure Machine Learning Studio'ya alma](import-data.md).

## <a name="create-an-experiment"></a>Deneme oluşturma

Bu öğreticinin sonraki adımda, bir denemeyi Machine Learning Studio'da yüklediğiniz veri kümesini kullanan oluşturmaktır.  

1. Studio'da **+ yeni** pencerenin alt kısmındaki.
1. Seçin **deneme**ve ardından "Boş deneme"'i seçin. 

    ![Yeni bir deneme oluşturma](./media/tutorial-part1-credit-risk/create-new-experiment.png)


1. Tuvalin üst kısmındaki varsayılan deneme adını seçin ve anlamlı bir şekilde yeniden adlandırın.

    ![Denemeyi yeniden adlandırma](./media/tutorial-part1-credit-risk/rename-experiment.png)

   > [!TIP]
   > Doldurmak için iyi bir uygulamadır **özeti** ve **açıklama** deneme için **özellikleri** bölmesi. Bu özellikler, böylece herkes daha sonra görünüyor hedeflerinizi ve yöntemleri anlayacaksınız deneme belge için bir şans verin.
   > 
   > ![Deneme özellikleri](./media/tutorial-part1-credit-risk/experiment-properties.png)
   > 

1. Deneme tuvalin solundaki modül paletindeki içinde genişletin **kaydedilmiş veri kümeleri**.
1. Altında oluşturulan veri kümesi bulma **My veri kümeleri** ve tuvale sürükleyin. Adını girerek bir veri kümesi bulabilirsiniz **arama** kutusunun üstünde paleti.  

    ![Deneme için veri kümesi Ekle](./media/tutorial-part1-credit-risk/add-dataset-to-experiment.png)


### <a name="prepare-the-data"></a>Verileri hazırlama

İlk 100 veri satırlarını ve tüm veri kümesi için bazı istatistiksel bilgileri görüntüleyebilirsiniz: (Küçük daire altındaki) veri kümesinin çıkış bağlantı noktasına tıklayıp **Görselleştir**.  

Veri dosyasındaki sütun başlıkları ile gelmemiştir olduğundan, genel başlıkları Studio sağlanan (Sütun1, Sütun2, *vb.*). İyi başlıkları bir model oluşturmak için gerekli değildir, ancak bunlar denemede verilerle çalışmayı kolaylaştırır. Ayrıca, bu model sonunda bir web hizmetinde yayımladığınızda, başlıkları hizmetinin kullanıcı sütunlara tanımlamaya yardımcı.  

Sütun başlıkları kullanarak ekleyebilirsiniz [meta verileri Düzenle] [ edit-metadata] modülü.

Kullandığınız [meta verileri Düzenle] [ edit-metadata] modülü bir veri kümesi ile ilişkili meta verileri değiştirmek için. Bu durumda, bu sütun başlıkları için daha kolay adlar sağlamak için kullanın. 

Kullanılacak [meta verileri Düzenle][edit-metadata], hangi sütunların (Bu durumda, bunların tümünün.) değiştirileceğini ilk belirtin Ardından, sizin (sütun başlıklarını değiştirme bu durumda,.) Bu sütunlarda gerçekleştirilecek eylemi belirtin

1. "Meta veri" yazın modül paletinde **arama** kutusu. [Meta verileri Düzenle] [ edit-metadata] modül listesinde görünür.

1. Tıklayın ve sürükleyin [meta verileri Düzenle] [ edit-metadata] tuvale modülü daha önce eklediğiniz veri kümesi bırakın.

1. Veri kümesine bağlanmak [meta verileri Düzenle][edit-metadata]: (veri kümesinin alt kısmındaki küçük daire) veri kümesinin çıkış bağlantı noktasına tıklayın, giriş bağlantı noktasına sürükleyin [meta verileri Düzenle] [ edit-metadata] (üst modülünün küçük daire), ardından fare düğmesini bırakın. Veri kümesi ve modül, ya da tuvalde yerleri bile bağlı kalır.
 
    Deneme şimdi aşağıdakine benzer görünmelidir:  

    ![Düzenleme meta verileri ekleme](./media/tutorial-part1-credit-risk/experiment-with-edit-metadata-module.png)

    Bu modül için özellikleri henüz ayarlamadıysanız kırmızı ünlem işareti gösterir. Şimdi yaparsınız.

    > [!TIP]
    > Modüle çift tıklayıp metin girerek bir modüle yorum ekleyebilirsiniz. Bu, modülün denemenizde ne işe yaradığını bir bakışta görmenize yardımcı olabilir. Bu durumda, çift [meta verileri Düzenle] [ edit-metadata] modülü ve yorum "Ekle sütun başlıklarının" yazın. Metin kutusunu kapatmak için tuvalde başka bir yerde tıklayın. Açıklamayı görüntülemek için modülü aşağı oka tıklayın.
    > 
    > ![Meta veri modülü ile açıklama eklendi Düzenle](./media/tutorial-part1-credit-risk/edit-metadata-with-comment.png)
    > 

1. Seçin [meta verileri Düzenle][edit-metadata]hem de **özellikleri** tuvalin sağındaki bölmesi **Sütun seçiciyi Başlat**.

1. İçinde **sütunları seçin** iletişim kutusunda, tüm satırları Seç **kullanılabilir sütunlar** tıklayın > taşımak için **seçili sütun**.
   İletişim kutusu şu şekilde görünmelidir:

   ![Seçili tüm sütunları içeren sütun Seçici](./media/tutorial-part1-credit-risk/select-columns.png)


1. Tıklayın **Tamam** işaretleyin.

1. Geri **özellikleri** bölmesinde Ara **yeni sütun adları** parametresi. Bu alanda 21 sütunlar ve sütun sırasını virgülle ayrılmış veri kümesi için adlarının listesini girin. Sütun adları UCI Web sitesinde veri kümesi belgelerinden elde edebilirsiniz veya kolaylık sağlamak için kopyalama ve yapıştırma aşağıdaki listede:  

   ```   
   Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   ```

   Özellikler bölmesi şöyle görünür:

   ![Özellikleri Düzenle meta veriler için](./media/tutorial-part1-credit-risk/edit-metadata-properties.png)

   > [!TIP]
   > Sütun başlıkları doğrulamak istiyorsanız, denemeyi çalıştırma (tıklayın **ÇALIŞTIRMA** deneme tuvalinin altında). Çalıştırma tamamlandığında (yeşil bir onay işareti görünür [meta verileri Düzenle][edit-metadata]), çıkış bağlantı noktasına tıklayın [meta verileri Düzenle] [ edit-metadata] Modülü seçip **Görselleştir**. Herhangi bir modülün çıkışına denemeyi verilerine ilerlemesini görüntülemek için aynı şekilde görüntüleyebilirsiniz.
   > 
   > 

### <a name="create-training-and-test-datasets"></a>Eğitim oluşturun ve veri kümeleri test

Modeli eğitmek için bazı veriler ve bazı test etmek için ihtiyacınız.
Denemeyi sonraki adımda, iki ayrı veri kümelerine veri kümesi bölünemiyor şekilde: modelimizi ve test için bir eğitim için bir tane.

Bunu yapmak için kullandığınız [verileri bölme] [ split] modülü.  

1. Bulma [verileri bölme] [ split] modülünü tuvale sürükleyin ve buna bağlanmak [meta verileri Düzenle] [ edit-metadata] modülü.

1. Varsayılan olarak, 0,5 bölünmüş oranıdır ve **Randomized bölünmüş** parametrenin ayarlanmış. Bu rastgele yarım veri çıkışı bir bağlantı noktası üzerinden olduğu anlamına gelir [verileri bölme] [ split] modülü ve yarısı üzerinden diğer. Bu parametreleri ayarlayabilirsiniz yanı sıra **rastgele doldurma** parametresi, eğitim ve test verileri arasında bölünmüş değiştirmek için. Bu örnek için bunları olarak bırakın-olduğu.
   
   > [!TIP]
   > Özellik **ilk çıkış veri kümesinde satır kesiri** çıkışını verilerin ne kadar belirler *sol* çıkış bağlantı noktası. İçin 0,7 oranını ayarlamak, örneği için verilerin %70 çıkış sol bağlantı noktası üzerinden ve doğru bağlantı noktası üzerinden % 30 olur.  
   > 
   > 

1. Çift [verileri bölme] [ split] modülü ve açıklamayı girin, "% 50, eğitim ve test verileri bölme". 

Çıktıları kullanabilirsiniz [verileri bölme] [ split] modülü, ister, ancak eğitim verilerini ve sağa çıktı olarak test verilerini sol çıkış kullanacak şekilde seçelim.  

Belirtildiği gibi [önceki adımda](tutorial-part1-credit-risk.md#upload), düşük olarak yüksek kredi riski misclassifying maliyeti, beş kat daha yüksek bir düşük kredi riski yüksek olarak misclassifying maliyeti. Bu hesap için bu maliyet işlevi yansıtan yeni bir veri kümesi oluşturun. Yeni veri kümesi her düşük riskli örnek çoğaltılmaz sırada her yüksek riskli örnek beş kez çoğaltılır.   

Bu çoğaltma yapabilirsiniz R kodunu kullanarak:  

1. Bulun ve sürükleyin [R betiği yürütme] [ execute-r-script] modülünü deneme tuvaline sürükleyin. 

1. Sol çıkış bağlantı noktasına bağlanmak [verileri bölme] [ split] ilk giriş bağlantı noktasına ("Dataset1") Modülü [R betiği yürütme] [ execute-r-script] modülü.

1. Çift [R betiği yürütme] [ execute-r-script] modülü ve açıklamayı girin "kümesi, maliyet ayarlaması".

1. İçinde **özellikleri** bölmesinde, varsayılan metni silmek **R betiği** parametresi bu betiği girin:
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R betiği yürütme modülünde R betiği](./media/tutorial-part1-credit-risk/execute-r-script.png)

Bu aynı çoğaltma işlemi her çıktısı için gerçekleştirmeniz gereken [verileri bölme] [ split] modülü eğitim ve test verilerini aynı maliyet ayarlaması edinecek olmanızdır. Bunu yapmanın en kolay yolu çoğaltarak olan [R betiği yürütme] [ execute-r-script] yaptığınız modülü ve diğer bağlanma çıkış bağlantı noktasına [verileri bölme] [ split] modülü.

1. Sağ [R betiği yürütme] [ execute-r-script] modülü ve select **kopyalama**.

1. Deneme tuvalinin sağ tıklayıp **Yapıştır**.

1. Yeni modül istediğiniz konuma sürükleyin ve ardından sağ çıkış bağlantı noktasına bağlanın [verileri bölme] [ split] ilk giriş bağlantı noktasına bu yeni modül [R betiği yürütme] [ execute-r-script] modülü. 

1. Tuvalin altındaki tıklatın **çalıştırma**. 

> [!TIP]
> R betiği yürütme modül kopyası, özgün modül olarak aynı betiği içerir. Tuvalde bir modül kopyalayıp, özgün tüm özelliklerini kopyalama korur.  
> 
>

Bizim deneme şimdi aşağıdakine benzer:

![Bölünmüş modülü ve R betikleri ekleme](./media/tutorial-part1-credit-risk/experiment.png)

Denemelerinizi içinde R betikleri kullanma hakkında daha fazla bilgi için bkz. [R ile denemenizi genişletme](extend-your-experiment-with-r.md).


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [machine-learning-studio-clean-up](../../../includes/machine-learning-studio-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki adımları tamamladınız: 
 
> [!div class="checklist"]
> * Machine Learning Studio çalışma alanı oluşturma
> * Çalışma alanına mevcut verileri yükleme
> * Deneme oluşturma

Artık eğitme ve değerlendirme modelleri bu veriler için hazırsınız demektir.

> [!div class="nextstepaction"]
> [Öğretici 2 - eğitme ve değerlendirme modelleri](tutorial-part2-credit-risk-train.md)

<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
