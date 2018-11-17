---
title: '3. adım: yeni bir Machine Learning denemesi oluşturma | Microsoft Docs'
description: "Adım 3 / geliştirme Tahmine dayalı çözüm Kılavuzu: Azure Machine Learning Studio'da yeni bir eğitim denemesini oluşturun."
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.custom: (previous ms.author hshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 660e3c27-55ef-4c33-a4e9-dff4d1224630
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.openlocfilehash: 2fdeab83d1e668fbbb68155c1695ffb40d71c15b
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51824447"
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Kılavuz Adımı 3: Yeni bir Azure Machine Learning denemesi oluşturma
Bu kılavuz, üçüncü adımıdır [bir Azure Machine learning'de Tahmine dayalı analiz çözümü geliştirin](walkthrough-develop-predictive-solution.md)

1. [Bir Machine Learning çalışma alanı oluşturma](walkthrough-1-create-ml-workspace.md)
2. [Mevcut verileri yükleme](walkthrough-2-upload-data.md)
3. **Yeni bir deneme oluşturma**
4. [Modelleri eğitme ve değerlendirme](walkthrough-4-train-and-evaluate-models.md)
5. [Web hizmetini dağıtma](walkthrough-5-publish-web-service.md)
6. [Web hizmetine erişme](walkthrough-6-access-web-service.md)

- - -
Bu izlenecek yolda bir sonraki adım, bir denemeyi Machine Learning Studio'da dosyamızı veri kümesi kullanan oluşturmaktır.  

1. Studio'da **+ yeni** pencerenin alt kısmındaki.
2. Seçin **deneme**ve ardından "Boş deneme"'i seçin. 

    ![Yeni bir deneme oluşturma][0]

2. Tuvalin üst kısmındaki varsayılan deneme adını seçin ve anlamlı bir şekilde yeniden adlandırın.

    ![Denemeyi yeniden adlandırma][5]
   
   > [!TIP]
   > Doldurmak için iyi bir uygulamadır **özeti** ve **açıklama** deneme için **özellikleri** bölmesi. Bu özellikler, böylece herkes daha sonra görünüyor hedeflerinizi ve yöntemleri anlayacaksınız deneme belge için bir şans verin.
   > 
   > ![Deneme özellikleri][6]
   > 
3. Deneme tuvalin solundaki modül paletindeki içinde genişletin **kaydedilmiş veri kümeleri**.
4. Altında oluşturulan veri kümesi bulma **My veri kümeleri** ve tuvale sürükleyin. Adını girerek bir veri kümesi bulabilirsiniz **arama** kutusunun üstünde paleti.  

    ![Deneme için veri kümesi Ekle][7]

## <a name="prepare-the-data"></a>Verileri hazırlama
İlk 100 veri satırlarını ve tüm veri kümesi için bazı istatistiksel bilgileri görüntüleyebilirsiniz: (küçük daire altındaki) veri kümesinin çıkış bağlantı noktasına tıklayıp **Görselleştir**.  

Veri dosyasındaki sütun başlıkları ile gelmemiştir olduğundan, genel başlıkları Studio sağlanan (Sütun1, Sütun2, *vb.*). İyi başlıkları bir model oluşturmak için gerekli değildir, ancak bunlar denemede verilerle çalışmayı kolaylaştırır. Ayrıca, biz sonunda bu modeli bir web hizmetinde yayımladığınızda, başlıkları hizmetinin kullanıcı sütunlara tanımlamaya yardımcı.  

Sütun başlıkları kullanarak ekleyebiliriz [meta verileri Düzenle] [ edit-metadata] modülü.
Kullandığınız [meta verileri Düzenle] [ edit-metadata] modülü bir veri kümesi ile ilişkili meta verileri değiştirmek için. Bu durumda, bu sütun başlıkları için daha kolay adlar sağlamak için kullanacağız. 

Kullanılacak [meta verileri Düzenle][edit-metadata], hangi sütunların (Bu durumda, bunların tümünün.) değiştirileceğini ilk belirtin Ardından, sizin (sütun başlıklarını değiştirme bu durumda,.) Bu sütunlarda gerçekleştirilecek eylemi belirtin

1. "Meta veri" yazın modül paletinde **arama** kutusu. [Meta verileri Düzenle] [ edit-metadata] modül listesinde görünür.

2. Tıklayın ve sürükleyin [meta verileri Düzenle] [ edit-metadata] tuvale modülü daha önce eklediğimiz veri kümesi bırakın.

3. Veri kümesine bağlanmak [meta verileri Düzenle][edit-metadata]: (veri kümesinin alt kısmındaki küçük daire) veri kümesinin çıkış bağlantı noktasına tıklayın, giriş bağlantı noktasına sürükleyin [meta verileri Düzenle] [ edit-metadata] (üst modülünün küçük daire), ardından fare düğmesini bırakın. Veri kümesi ve modül, ya da tuvalde yerleri bile bağlı kalır.
   
   Deneme şimdi aşağıdakine benzer görünmelidir:  
   
   ![Düzenleme meta verileri ekleme][1]
   
   Kırmızı ünlem işareti, biz bu modül için özellikleri henüz ayarlamasını yapmadığınızı fark gösterir. Biz bu İleri yaparsınız.
   
   > [!TIP]
   > Modüle çift tıklayıp metin girerek bir modüle yorum ekleyebilirsiniz. Bu, modülün denemenizde ne işe yaradığını bir bakışta görmenize yardımcı olabilir. Bu durumda, çift [meta verileri Düzenle] [ edit-metadata] modülü ve yorum "Ekle sütun başlıklarının" yazın. Metin kutusunu kapatmak için tuvalde başka bir yerde tıklayın. Açıklamayı görüntülemek için modülü aşağı oka tıklayın.
   > 
   > ![Meta veri modülü ile açıklama eklendi Düzenle][8]
   > 
4. Seçin [meta verileri Düzenle][edit-metadata]hem de **özellikleri** tuvalin sağındaki bölmesi **Sütun seçiciyi Başlat**.

5. İçinde **sütunları seçin** iletişim kutusunda, tüm satırları Seç **kullanılabilir sütunlar** tıklayın > taşımak için **seçili sütun**.
   İletişim kutusu şu şekilde görünmelidir:

   ![Seçili tüm sütunları içeren sütun Seçici][2]

6. Tıklayın **Tamam** işaretleyin.

7. Geri **özellikleri** bölmesinde Ara **yeni sütun adları** parametresi. Bu alanda 21 sütunlar ve sütun sırasını virgülle ayrılmış veri kümesi için adlarının listesini girin. Sütun adları UCI Web sitesinde veri kümesi belgelerinden elde edebilirsiniz veya kolaylık sağlamak için kopyalama ve yapıştırma aşağıdaki listede:  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   Özellikler bölmesi şöyle görünür:
   
   ![Özellikleri Düzenle meta veriler için][3]

> [!TIP]
> Sütun başlıkları doğrulamak istiyorsanız, denemeyi çalıştırma (tıklayın **ÇALIŞTIRMA** deneme tuvalinin altında). Çalıştırma tamamlandığında (yeşil bir onay işareti görünür [meta verileri Düzenle][edit-metadata]), çıkış bağlantı noktasına tıklayın [meta verileri Düzenle] [ edit-metadata] Modülü seçip **Görselleştir**. Herhangi bir modülün çıkışına denemeyi verilerine ilerlemesini görüntülemek için aynı şekilde görüntüleyebilirsiniz.
> 
> 

## <a name="create-training-and-test-datasets"></a>Eğitim oluşturun ve veri kümeleri test
Modeli eğitmek için bazı veriler ve test etmek için bazı ihtiyacımız var.
Denemeyi sonraki adımda size veri kümesini iki ayrı veri kümelerine bölmek için: modelimizi ve test için bir eğitim için bir tane.

Bunu yapmak için kullandığımız [verileri bölme] [ split] modülü.  

1. Bulma [verileri bölme] [ split] modülünü tuvale sürükleyin ve buna bağlanmak [meta verileri Düzenle] [ edit-metadata] modülü.

2. Varsayılan olarak, 0,5 bölünmüş oranıdır ve **Randomized bölünmüş** parametrenin ayarlanmış. Bu rastgele yarım veri çıkışı bir bağlantı noktası üzerinden olduğu anlamına gelir [verileri bölme] [ split] modülü ve yarısı üzerinden diğer. Bu parametreleri ayarlayabilirsiniz yanı sıra **rastgele doldurma** parametresi, eğitim ve test verileri arasında bölünmüş değiştirmek için. Bu örnekte, biz bunları olarak bırakın-olduğu.
   
   > [!TIP]
   > Özellik **ilk çıkış veri kümesinde satır kesiri** çıkışını verilerin ne kadar belirler *sol* çıkış bağlantı noktası. İçin 0,7 oranını ayarlamak, örneği için verilerin %70 çıkış sol bağlantı noktası üzerinden ve doğru bağlantı noktası üzerinden % 30 olur.  
   > 
   > 

3. Çift [verileri bölme] [ split] modülü ve açıklamayı girin, "% 50, eğitim ve test verileri bölme". 

Çıktıları kullanabiliriz [verileri bölme] [ split] modülü ancak biz gibi ancak eğitim verilerini ve sağa çıktı olarak test verilerini sol çıkış kullanacak şekilde seçelim.  

Belirtildiği gibi [önceki adımda](walkthrough-2-upload-data.md), düşük olarak yüksek kredi riski misclassifying maliyeti, beş kat daha yüksek bir düşük kredi riski yüksek olarak misclassifying maliyeti. Bu hesap için Biz bu maliyet işlevi yansıtan yeni bir veri kümesi oluşturur. Yeni veri kümesi her düşük riskli örnek çoğaltılmaz sırada her yüksek riskli örnek beş kez çoğaltılır.   

Bu çoğaltma yapabileceğimiz R kodunu kullanarak:  

1. Bulun ve sürükleyin [R betiği yürütme] [ execute-r-script] modülünü deneme tuvaline sürükleyin. 

2. Sol çıkış bağlantı noktasına bağlanmak [verileri bölme] [ split] ilk giriş bağlantı noktasına ("Dataset1") Modülü [R betiği yürütme] [ execute-r-script] modülü.

3. Çift [R betiği yürütme] [ execute-r-script] modülü ve açıklamayı girin "kümesi, maliyet ayarlaması".

4. İçinde **özellikleri** bölmesinde, varsayılan metni silmek **R betiği** parametresi bu betiği girin:
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R betiği yürütme modülünde R betiği][9]

Bu aynı çoğaltma işlemi her çıktısı için yapmanız gereken [verileri bölme] [ split] modülü eğitim ve test verilerini aynı maliyet ayarlaması edinecek olmanızdır. Bunu yapmanın en kolay yolu çoğaltarak olan [R betiği yürütme] [ execute-r-script] modülü yalnızca yaptık ve diğer bağlanma çıkış bağlantı noktasına [verileri bölme] [ split] modülü.

1. Sağ [R betiği yürütme] [ execute-r-script] modülü ve select **kopyalama**.

2. Deneme tuvalinin sağ tıklayıp **Yapıştır**.

3. Yeni modül istediğiniz konuma sürükleyin ve ardından sağ çıkış bağlantı noktasına bağlanın [verileri bölme] [ split] ilk giriş bağlantı noktasına bu yeni modül [R betiği yürütme] [ execute-r-script] modülü. 

4. Tuvalin altındaki tıklatın **çalıştırma**. 

> [!TIP]
> R betiği yürütme modül kopyası, özgün modül olarak aynı betiği içerir. Tuvalde bir modül kopyalayıp, özgün tüm özelliklerini kopyalama korur.  
> 
> 

Bizim deneme şimdi aşağıdakine benzer:

![Bölünmüş modülü ve R betikleri ekleme][4]

Denemelerinizi içinde R betikleri kullanma hakkında daha fazla bilgi için bkz. [R ile denemenizi genişletme](extend-your-experiment-with-r.md).

**Sonraki: [eğitme modelleri ve değerlendirme](walkthrough-4-train-and-evaluate-models.md)**

[0]: ./media/walkthrough-3-create-new-experiment/create-new-experiment.png
[5]: ./media/walkthrough-3-create-new-experiment/rename-experiment.png
[6]: ./media/walkthrough-3-create-new-experiment/experiment-properties.png
[7]: ./media/walkthrough-3-create-new-experiment/add-dataset-to-experiment.png
[8]: ./media/walkthrough-3-create-new-experiment/edit-metadata-with-comment.png
[9]: ./media/walkthrough-3-create-new-experiment/execute-r-script.png
[1]: ./media/walkthrough-3-create-new-experiment/experiment-with-edit-metadata-module.png
[2]: ./media/walkthrough-3-create-new-experiment/select-columns.png
[3]: ./media/walkthrough-3-create-new-experiment/edit-metadata-properties.png
[4]: ./media/walkthrough-3-create-new-experiment/experiment.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
