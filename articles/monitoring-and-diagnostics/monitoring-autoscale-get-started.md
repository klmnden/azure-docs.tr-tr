---
title: Otomatik ölçeklendirme Azure kullanmaya başlama
description: Kaynak Web uygulaması ölçeklendirmek öğrenin, Azure'da bulut hizmeti, sanal makine veya sanal makine ölçek kümesi.
author: rajram
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/07/2017
ms.author: rajram
ms.component: autoscale
ms.openlocfilehash: 2781e718e3829c13dcc8cdd998936cfba30d8550
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263655"
---
# <a name="get-started-with-autoscale-in-azure"></a>Otomatik ölçeklendirme Azure kullanmaya başlama
Bu makalede, Microsoft Azure Portalı'nda, kaynak için otomatik ölçeklendirme ayarlarınızı ayarlama açıklar.

Azure İzleyici otomatik ölçeklendirme, yalnızca sanal makine ölçek kümeleri, bulut Hizmetleri, Azure uygulama hizmeti planları ve uygulama hizmeti ortamları için geçerlidir. 

## <a name="discover-the-autoscale-settings-in-your-subscription"></a>Otomatik ölçeklendirme ayarlarını aboneliğinizde Bul
Otomatik ölçeklendirme Azure İzleyicisi'nde geçerli olduğu tüm kaynakları bulabilir. Adım adım kılavuz için aşağıdaki adımları kullanın:

1. Açık [Azure portalı.][1]
2. Sol bölmede Azure İzleyici simgesine tıklayın.
  ![Açık Azure İzleyicisi][2]
3. Tıklatın **otomatik ölçeklendirme** otomatik ölçeklendirme olduğu geçerli otomatik ölçeklendirme durumlarıyla birlikte uygulanabilir tüm kaynakları görüntülemek için.
  ![Otomatik ölçeklendirme Azure İzleyicisi'nde Bul][3]

Belirli bir kaynak grubunun, belirli kaynak türlerine veya belirli bir kaynak kaynakları seçmek için üst kapsam listede aşağı Filtre bölmesini kullanabilirsiniz.

Her bir kaynağın geçerli örnek sayısına ve otomatik ölçeklendirme durum bulacaksınız. Otomatik ölçeklendirme Durum aşağıdakilerden biri olabilir:

- **Yapılandırılmamış**: otomatik ölçeklendirme henüz bu kaynak için etkinleştirdiğiniz değil.
- **Etkin**: Bu kaynak için otomatik ölçeklendirme etkin.
- **Devre dışı**: Bu kaynak için otomatik ölçeklendirme devre dışı bırakmış.

## <a name="create-your-first-autoscale-setting"></a>İlk otomatik ölçeklendirme ayarı oluşturun

Şimdi, ilk otomatik ölçeklendirme ayarı oluşturmak için bir basit adım adım kılavuz şimdi gidin.

1. Açık **otomatik ölçeklendirme** dikey penceresinde Azure izlemek ve ölçeklendirmek istediğiniz bir kaynak seçin. (Bir web uygulaması ile ilişkili bir uygulama hizmeti planı aşağıdaki adımları kullanın. Yapabilecekleriniz [ilk ASP.NET web uygulamanızı 5 dakika içinde oluşturma.] [4])
2. Geçerli örnek sayısı 1 olduğuna dikkat edin. Tıklatın **etkinleştirmek otomatik ölçeklendirme**.
  ![Yeni web uygulaması için ölçek ayarı][5]
3. Ölçek ayarı için bir ad sağlayın ve ardından **bir kural eklemek**. İçerik bölmesinde sağ tarafında olarak açın ölçek kuralı seçeneklerini dikkat edin. Varsayılan olarak, bu kaynak CPU yüzdesi yüzde 70 aşarsa, örnek sayınız 1 ile ölçeklendirme seçeneği ayarlar. Kendi varsayılan değerlerinde bırakın ve tıklatın **Ekle**.
  ![Bir web uygulaması için ölçek ayar oluşturun][6]
4. İlk ölçek kuralı şimdi oluşturduğunuzu düşünün. Not UX en iyi yöntemler önerir ve "En az bir ölçek kuralı için önerilir olduğunu." durumları Bunu yapmak için:
  
    a. Tıklatın **bir kural eklemek**. 

    b. Ayarlama **işleci** için **değerinden**.

    c. Ayarlama **eşik** için **20**.

    d. Ayarlama **işlemi** için **azaltmak sayısına göre**.

   Ölçek ayarını şimdi olmalıdır ölçek genişletme/ölçekler olduğunu içinde tabanlı CPU kullanımı.
   ![CPU üzerinde göre ölçeklendirin][8]
5. **Kaydet**’e tıklayın.

Tebrikler! İlk ölçek ayarınız web uygulamanızı CPU kullanım amaçlarına göre otomatik ölçeklendirme artık başarıyla oluşturdunuz.

> [!NOTE] 
> Aynı adımlar bir sanal makine ölçek kümesi ile çalışmaya başlama veya Bulut hizmet rolü için geçerlidir.

## <a name="other-considerations"></a>Diğer konular
### <a name="scale-based-on-a-schedule"></a>Bir zamanlamaya göre ölçeği
CPU üzerinde göre ölçeklendirin yanı sıra, Ölçek farklı için haftanın belirli günleri ayarlayabilirsiniz.

1. Tıklatın **ölçek koşul Ekle**.
2. Ölçek modu ve kuralları ayarlama varsayılan koşulu ile aynı olur.
3. Seçin **yineleyin belirli günleri** zamanlama için.
4. Gün ve ölçek koşul ne zaman uygulanacağını için başlangıç/bitiş saati seçin.

![Zamanlamaya göre ölçeği koşulu][9]
### <a name="scale-differently-on-specific-dates"></a>Farklı belirli tarihleri ölçeklendirme
CPU üzerinde göre ölçeklendirin yanı sıra, Ölçek farklı belirli tarihler için ayarlayabilirsiniz.

1. Tıklatın **ölçek koşul Ekle**.
2. Ölçek modu ve kuralları ayarlama varsayılan koşulu ile aynı olur.
3. Seçin **belirt başlangıç/bitiş tarihlerini** zamanlama için.
4. Başlangıç/bitiş tarihleri ve ölçek koşul ne zaman uygulanacağını için başlangıç/bitiş saati seçin.

![Tarihleri temel alınarak ölçek koşulu][10]

### <a name="view-the-scale-history-of-your-resource"></a>Kaynağınız ölçek geçmişini görüntüleyin
Kaynağınız yukarı veya aşağı ölçeklendirilir her etkinlik günlüğünde bir olay kaydedilir. Geçerek son 24 saat için kaynağınız ölçek geçmişini görüntüleyebilirsiniz **çalıştırma geçmişi** sekmesi.

![Çalıştırma geçmişi][11]

(En fazla 90 gün için) tam ölçek geçmişini görüntülemek isteyip istemediğinizi seçin **daha fazla ayrıntı görmek için burayı tıklatın**. Etkinlik günlüğü kaynak ve kategori için önceden seçilmiş otomatik ölçeklendirme ile açılır.

### <a name="view-the-scale-definition-of-your-resource"></a>Kaynağınız ölçek tanımını görüntüleme
Otomatik ölçeklendirme bir Azure Resource Manager kaynaktır. Ölçek tanımı JSON'de geçerek görüntüleyebileceğiniz **JSON** sekmesi.

![Ölçek tanımı][12]

JSON'da doğrudan gerekirse değişiklik yapabilirsiniz. Bunları kaydettikten sonra bu değişiklikleri yansıtılır.

### <a name="disable-autoscale-and-manually-scale-your-instances"></a>Otomatik ölçeklendirme devre dışı bırakın ve el ile örneklerinizi ölçeklendirin
Geçerli ölçek ayarını devre dışı bırakın ve el ile kaynağınız ölçeklendirmek istediğiniz zaman zamanlar olabilir.

Tıklatın **otomatik ölçeklendirme devre dışı bırakma** üstündeki düğmesi.
![Otomatik ölçeklendirme devre dışı bırak][13]

> [!NOTE] 
> Bu seçenek yapılandırmanızı devre dışı bırakır. Otomatik ölçeklendirme yeniden etkinleştirildikten sonra ancak, geri için edinebilirsiniz. 

Şimdi, el ile ölçeklendirmek istediğiniz örneklerinin sayısını ayarlayabilirsiniz.

![El ile ölçek kümesi][14]

Tıklatarak her zaman için otomatik ölçeklendirme dönebilirsiniz **etkinleştirmek otomatik ölçeklendirme** ve ardından **kaydetmek**.

## <a name="next-steps"></a>Sonraki adımlar
- [Bir etkinlik günlüğü aboneliğinizi tüm otomatik ölçeklendirme motoru işlemleri izlemek için uyarı oluştur](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Bir etkinlik günlüğü aboneliğinizi tüm başarısız otomatik ölçeklendirme ölçek/genişletme işlemleri izlemek için uyarı oluştur](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/monitoring-autoscale-get-started/azure-monitor-launch.png
[3]: ./media/monitoring-autoscale-get-started/discover-autoscale-azure-monitor.png
[4]: https://docs.microsoft.com/azure/app-service/app-service-web-get-started-dotnet
[5]: ./media/monitoring-autoscale-get-started/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-get-started/create-scale-setting-web-app.png
[7]: ./media/monitoring-autoscale-get-started/scale-in-recommendation.png
[8]: ./media/monitoring-autoscale-get-started/scale-based-on-cpu.png
[9]: ./media/monitoring-autoscale-get-started/scale-condition-schedule.png
[10]: ./media/monitoring-autoscale-get-started/scale-condition-dates.png
[11]: ./media/monitoring-autoscale-get-started/scale-history.png
[12]: ./media/monitoring-autoscale-get-started/scale-definition-json.png
[13]: ./media/monitoring-autoscale-get-started/disable-autoscale.png
[14]: ./media/monitoring-autoscale-get-started/set-manualscale.png

