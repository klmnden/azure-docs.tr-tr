---
title: "3. adım: yeni bir Machine Learning deneme oluşturma | Microsoft Docs"
description: "Adım 3 / geliştirme Tahmine dayalı bir çözüm izlenecek yol: Azure Machine Learning Studio'da yeni bir eğitim deneme oluşturun."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 660e3c27-55ef-4c33-a4e9-dff4d1224630
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: a8f1764204740a8f5ef757e5e2ad63cfd43af150
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Kılavuz Adımı 3: Yeni bir Azure Machine Learning denemesi oluşturma
Bu kılavuzu, üçüncü adımdır [Azure Machine learning'de Tahmine dayalı analiz çözümü geliştirme](walkthrough-develop-predictive-solution.md)

1. [Bir Machine Learning çalışma alanı oluşturma](walkthrough-1-create-ml-workspace.md)
2. [Mevcut verileri yükleme](walkthrough-2-upload-data.md)
3. **Yeni bir deneme oluşturma**
4. [Modelleri eğitme ve değerlendirme](walkthrough-4-train-and-evaluate-models.md)
5. [Web hizmetini dağıtma](walkthrough-5-publish-web-service.md)
6. [Web hizmetine erişim](walkthrough-6-access-web-service.md)

- - -
Bu kılavuzda bir sonraki adım, Machine Learning Studio'da biz karşıya veri kümesi kullanan bir denemeyi oluşturmaktır.  

1. Studio'da sırasıyla **+ yeni** pencerenin altındaki.
2. Seçin **deneme**ve ardından "Boş deneme" seçin. 

    ![Yeni bir deneme oluşturma][0]

2. Tuvale üstündeki varsayılan deneme adını seçin ve anlamlı yeniden adlandırın.

    ![Denemeyi yeniden adlandırma][5]
   
   > [!TIP]
   > Doldurmak için iyi bir uygulamadır **Özet** ve **açıklama** denemesinde için **özellikleri** bölmesi. Bu özellikler, böylece daha sonra görünüyor herkes amaçlar ve yöntemler anlayabileceği deneme belge şansı.
   > 
   > ![Deneme özellikleri][6]
   > 
3. Deneme tuvalinin solundaki modül paletindeki genişletin **kaydedilen veri kümeleri**.
4. Altında oluşturduğunuz veri kümesi bulma **My veri kümeleri** ve tuvale sürükleyin. Veri kümesi adı girerek bulabileceğiniz **arama** kutusunun palet üstünde.  

    ![Veri kümesi için deneme ekleyin][7]

## <a name="prepare-the-data"></a>Verileri hazırlama
Veri ilk 100 satırı ve tüm veri kümesi için bazı istatistiksel bilgileri görüntüleyebilirsiniz: (küçük daire altındaki) kümesinin çıkış bağlantı noktasına tıklayın ve **Görselleştir**.  

Veri dosyasındaki sütun başlıkları ile eşleşmedi geldiğinden Studio genel başlıkları sağlamıştır (Col1, Col2, *vb.*). İyi başlıkları bir model oluşturmak için gerekli değildir, ancak bunlar denemede verilerle çalışmayı kolaylaştırır. Ayrıca, biz sonunda bu model bir web hizmeti yayımladığınızda, başlıkları hizmetinin kullanıcı sütunlara tanımlamaya yardımcı olur.  

Sütun başlıklarını kullanarak ekleyebiliriz [Düzenle meta veri] [ edit-metadata] modülü.
Kullandığınız [Düzenle meta veri] [ edit-metadata] modülü bir veri kümesi ile ilişkili meta verileri değiştirmek için. Bu durumda, bu sütun başlıkları için daha fazla kolay adlar sağlamak için kullanırız. 

Kullanılacak [Düzenle meta veri][edit-metadata], (Bu durumda, bunların tümünün.) değiştirmek için hangi sütunların ilk belirtin Ardından, bu sütunları (sütun başlıklarını değiştirme bu durumda,.) üzerinde gerçekleştirilecek eylemi belirtin

1. Modül paletindeki "meta veri" yazın **arama** kutusu. [Düzenle meta veri] [ edit-metadata] modül listesinde görünür.

2. ' I tıklatın ve sürükleyin [Düzenle meta veri] [ edit-metadata] tuvale modülü ve daha önce eklediğimiz dataset bırakın.

3. Veri kümesine Bağlan [Düzenle meta veri][edit-metadata]: (küçük daire alt kümesi) kümesinin çıkış bağlantı noktasına tıklayın, giriş bağlantı noktasına sürükleyin [Düzenle meta veri] [ edit-metadata] (modül üstündeki küçük daire) fare düğmesini bırakın. Ya da tuval üzerinde hareket ettirin olsa bile veri kümesi ve modül bağlı kalır.
   
   Denemeyi bu gibi görünmelidir:  
   
   ![Düzen meta verileri ekleme][1]
   
   Kırmızı bir ünlem işareti Biz bu modül için özellikler henüz ayarlamadıysanız olduğunu gösterir. Biz bu sonraki gerçekleştirirsiniz.
   
   > [!TIP]
   > Modüle çift tıklayıp metin girerek bir modüle yorum ekleyebilirsiniz. Bu, modülün denemenizde ne işe yaradığını bir bakışta görmenize yardımcı olabilir. Bu durumda, çift [Düzenle meta veri] [ edit-metadata] modülü ve Açıklama "Ekle sütun başlıkları" türü. Metin kutusunu kapatmak için tuval üzerinde başka bir yerde'ı tıklatın. Açıklamayı görüntülemek için modülü aşağı oka tıklayın.
   > 
   > ![Meta veriler modülü açıklama eklendi Düzenle][8]
   > 
4. Seçin [Düzenle meta veri][edit-metadata]hem de **özellikleri** tuvalin sağındaki bölmesi **başlatma Sütun seçiciyi**.

5. İçinde **seçin sütunları** iletişim kutusunda, tüm satırları seçin **kullanılabilir sütunlar** tıklatıp > taşımak için **seçili sütun**.
   İletişim kutusu aşağıdaki gibi görünmelidir:

   ![Seçili tüm sütunlarla Sütun Seçici][2]

6. Tıklatın **Tamam** onay işareti.

7. Geri **özellikleri** bölmesinde, Ara **yeni sütun adları** parametresi. Bu alanda virgülle ve sütun sırasını ayrılmış kümesindeki 21 sütunların adlarının bir listesini girin. Veri kümesi belgelerindeki UCI Web sitesinde sütun adları elde edebilirsiniz veya kolaylık sağlamak için kopyalama ve yapıştırma aşağıdaki listede:  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   Özellikler bölmesinde şöyle görünür:
   
   ![Özellikleri düzenleme meta veriler için][3]

> [!TIP]
> Sütun başlıklarını doğrulamak istiyorsanız, denemeyi çalıştırın (tıklatın **çalıştırmak** deneme tuvalinin altında). Çalışan bittiğinde (yeşil bir onay işareti görünür [Düzenle meta veri][edit-metadata]), çıkış bağlantı noktasına tıklayın [Düzenle meta veri] [ edit-metadata] modülü ve select **Görselleştir**. Denemeyi verilerine ilerlemesini görüntülemek için aynı şekilde herhangi bir modül çıktısını görüntüleyin.
> 
> 

## <a name="create-training-and-test-datasets"></a>Eğitim oluşturmak ve veri kümelerini test etme
Modeli eğitmek için bazı veriler ve bazı test etmek için ihtiyacımız var.
Denemeyi sonraki adımda size dataset iki ayrı kümelerine bölünmüş şekilde: modelimizi ve test için bir tane eğitim için bir tane.

Bunu yapmak için kullanırız [bölünmüş veri] [ split] modülü.  

1. Bul [bölünmüş veri] [ split] modülünü tuvale sürükleyin ve buna bağlanmak [Düzenle meta veri] [ edit-metadata] modülü.

2. Varsayılan olarak, 0,5 bölünmüş orandır ve **Randomized bölünmüş** parametrenin ayarlanmış. Bu rastgele yarı veri çıkışı bir bağlantı noktası üzerinden olduğu anlamına gelir [bölünmüş veri] [ split] modülü ve yarısı diğer aracılığıyla. Bu parametreleri ayarlayabilirsiniz yanı sıra **rastgele doldurma** eğitim ve test verileri arasında bölünmüş değiştirmek için parametre. Bu örnekte, biz bunları olarak bırakın-değil.
   
   > [!TIP]
   > Özellik **ilk çıkış veri kümesinde satır kesir** çıkışı üzerinden verilerin ne kadar olacağını belirler *sol* çıkış bağlantı noktası. İçin 0,7 oranını ayarlamak, örneği için % 70 ' veri çıkış sol bağlantı noktası üzerinden ve doğru bağlantı noktası üzerinden % 30 olur.  
   > 
   > 

3. Çift [bölünmüş veri] [ split] modülü ve açıklama girin, "eğitim ve test veri Böl % 50". 

Çıkışları kullanırız [bölünmüş veri] [ split] modülü ancak biz gibi çalışır, ancak şimdi eğitim verilerini ve sağa çıktı olarak test verilerini sol çıkış kullanacak şekilde seçin.  

Bölümünde belirtildiği gibi [önceki adımı](walkthrough-2-upload-data.md), düşük olarak yüksek kredi riski misclassifying maliyetidir beş kez yüksek olarak düşük kredi riski misclassifying maliyeti daha yüksek. Bu hesap için Biz bu maliyet işlevi yansıtır yeni bir veri kümesi oluşturur. Yeni veri kümesi her düşük riskli örnek çoğaltılmaz sırasında beş kez her yüksek riskli örnek çoğaltılır.   

Bu çoğaltma yapabiliriz R kodu kullanarak:  

1. Bulma ve sürükleyin [R betiği yürütün] [ execute-r-script] deneme tuvale modülü. 

2. Sol çıkış bağlantı noktasına bağlanmak [bölünmüş veri] [ split] ilk giriş bağlantı noktasını ("Dataset1") modülüne [R betiği yürütün] [ execute-r-script] modülü.

3. Çift [R betiği yürütün] [ execute-r-script] modülü ve açıklamayı girin "maliyet ayarlaması Ayarla".

4. İçinde **özellikleri** bölmesinde, varsayılan metinde silme **R betiği** parametre ve bu komut dosyasını girin:
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R betiği R betiği yürütün Modülü][9]

Bu aynı her çıktısını çoğaltma işlemi yapmak ihtiyacımız [bölünmüş veri] [ split] modülü böylece eğitim ve test verileri aynı maliyet ayarlama. Bunu yapmanın en kolay çoğaltarak yoludur [R betiği yürütün] [ execute-r-script] biz yalnızca yapılan modülü ve diğer bağlanma çıkış bağlantı noktasına [bölünmüş veri] [ split] modülü.

1. Sağ [R betiği yürütün] [ execute-r-script] modülü ve select **kopya**.

2. Deneme tuvalinin sağ tıklatıp **Yapıştır**.

3. Yeni modül yerine sürükleyin ve doğru çıkış bağlantı noktasına bağlayın [bölünmüş veri] [ split] ilk giriş bağlantı noktası için bu yeni modül [R betiği yürütün] [ execute-r-script] modülü. 

4. Tuvalin altındaki tıklatın **çalıştırmak**. 

> [!TIP]
> R betiği yürütün modülü kopyasını özgün modül olarak aynı komut dosyasını içerir. Tuvalde bir modül kopyalayıp, kopya özgün tüm özelliklerini korur.  
> 
> 

Bizim denemeyi şimdi şuna benzer:

![Bölünmüş modülü ve R betiklerini ekleme][4]

Denemelerinizi içinde R betiklerini kullanma hakkında daha fazla bilgi için bkz: [denemenizi R ile genişletme](extend-your-experiment-with-r.md).

**Sonraki: [eğitimi ve modelleri değerlendir](walkthrough-4-train-and-evaluate-models.md)**

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
