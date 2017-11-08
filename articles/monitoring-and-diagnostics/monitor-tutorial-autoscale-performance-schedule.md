---
title: "Otomatik ölçeklendirme Azure kaynakları temel performans verileri ya da bir zamanlama | Microsoft Docs"
description: "Otomatik ölçeklendirme ayarının ölçüm verileri ve zamanlama kullanarak bir uygulama hizmeti planı oluştur"
author: anirudhcavale
manager: orenr
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: ancav
ms.custom: mvc
ms.openlocfilehash: 3a85e288fa6f7d6c7138b7fea8319bd8dee01c2c
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="create-an-autoscale-setting-for--azure-resources-based-on-performance-data-or-a-schedule"></a>Performans verileri veya bir zamanlamaya göre Azure kaynakları için bir otomatik ölçeklendirme ayarı oluşturma

Otomatik ölçeklendirme ayarları hazır koşullara göre hizmet örneklerini Ekle/Kaldır olanak sağlar. Bu ayarları Portalı aracılığıyla oluşturulabilir. Bu yöntem oluşturma ve otomatik ölçeklendirme ayarı yapılandırmak için bir tarayıcı tabanlı kullanıcı arabirimi sağlar. 

Bu öğreticide şunları yapacaksınız: 
> [!div class="checklist"]
> * Bir Web uygulaması ve uygulama hizmeti planı oluşturun
> * Otomatik ölçeklendirme kurallarını ölçek ve ölçek genişletme için bir Web uygulaması alır istekleri sayısına göre yapılandırma
> * Bir ölçeklendirme eylemi tetikler ve örneklerinin sayısını artırmak izleyin
> * Bir ölçek eylemi tetikler ve örneklerinin sayısını azaltmak izleyin
> * Kaynaklarınızı temizleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-web-app-and-app-service-plan"></a>Bir Web uygulaması ve uygulama hizmeti planı oluşturun
Tıklatın **yeni** sol gezinti bölmesinden seçeneği

Aramak ve seçmek *Web uygulaması* öğesi ve tıklayın **oluştur**

Bir uygulama adı gibi seçin *MyTestScaleWebApp*. Yeni bir kaynak grubu oluşturma * myResourceGroup' ve seçtiğiniz kaynak grubuna yerleştirin.

Birkaç dakika içinde kaynaklarınızı sağlanmalıdır. Web uygulaması ve karşılık gelen App Service planı Bu öğreticinin geri kalanında kullanın.

    ![Create a new app service in the portal](./media/monitor-tutorial-autoscale-performance-schedule/Web-App-Create.png)

## <a name="navigate-to-autoscale-settings"></a>Otomatik ölçeklendirme ayarlarına gidin
1. Sol gezinti bölmesinden seçin **İzleyici** seçeneği. Sayfa Seç yüklediğinde sonra **otomatik ölçeklendirme** sekmesi.
2. Otomatik ölçeklendirme desteği, aboneliğinizin kaynaklarınıza listesini burada listelenir. Öğreticide daha önce oluşturulmuş uygulama hizmeti planı belirleyin ve tıklayın.

    ![Otomatik ölçeklendirme ayarlarına gidin](./media/monitor-tutorial-autoscale-performance-schedule/monitor-blade-autoscale.png)

3. Otomatik ölçeklendirme ayarı tıklatın **etkinleştirmek otomatik ölçeklendirme** düğmesi

Aşağıdaki resim gibi aramak için otomatik ölçeklendirme ekran doldurulmuş sonraki birkaç adımları Yardım:

   ![Otomatik ölçeklendirme ayarı Kaydet](./media/monitor-tutorial-autoscale-performance-schedule/Autoscale-Setting-Save.png)

 ## <a name="configure-default-profile"></a>Varsayılan profili yapılandırma
1. Sağlayan bir **adı** otomatik ölçeklendirme ayarı için
2. Varsayılan profil olun **ölçek modu** 'Belirli örnek sayısı için Ölçek' olarak ayarlanmış
3. Örnek sayısı kümesine **1**. Bu ayar, başka bir profil etkin olduğunda veya varsayılan profil etkin örnek sayısı için 1 değerini döndürür sağlar.

  ![Otomatik ölçeklendirme ayarlarına gidin](./media/monitor-tutorial-autoscale-performance-schedule/autoscale-setting-profile.png)


## <a name="create-recurrance-profile"></a>Recurrance profili oluştur

1. Tıklayın **ölçek koşul Ekle** varsayılan profili altındaki bağlantı

2. Düzen **adı** 'Pazartesi ile Cuma arası profil' olması için bu profili

3. Olun **ölçek modu** 'Ölçek ölçüm üzerinde temel' ayarlayın

4. İçin **örneği sınırları** ayarlamak **Minimum** '1' olarak **maksimum** '2' olarak ve **varsayılan** olarak '1'. Bu ayar, bu profili 1'den daha az örneği veya 2'den fazla örnekleri için hizmet planı olmayan otomatik ölçeklendirme yapar sağlar. Profili bir karar vermek için yeterli veri yok, varsayılan örnek sayısı (Bu durumda 1) kullanır.

5. İçin **zamanlama**, Seç 'belirli günleri Repeat'

6. Pazartesi-Cuma 09:00 PST 18:00 PST yinelenecek profili ayarlayın. Bu ayar bu profili yalnızca etkin ve geçerli 09: 00 için 6 PM Pazartesi-Cuma olmasını sağlar. Diğer tüm saatler sırasında 'Default' profili, otomatik ölçeklendirme ayarı kullanır profilidir.

## <a name="create-a-scale-out-rule"></a>Genişleme kural oluşturma

1. 'Pazartesi Cuma profiline'

2. Tıklatın **bir kural eklemek** bağlantı

3. Ayarlama **ölçüm kaynağı** 'diğer kaynak' olmalıdır. Ayarlama **kaynak türü** 'Uygulama Hizmetleri' olarak ve **kaynak** gibi Bu öğreticide daha önce Web uygulaması oluşturuldu.

4. Ayarlama **zaman toplama** 'Toplam' olarak **ölçüm adı** 'İstekleri' olarak ve **zaman çizgisi istatistiği** 'Sum' olarak

5. Ayarlama **işleci** 'Değerinden', olarak **eşik** '10' olarak ve **süresi** '5' dakika olarak.

6. Seçin **işlemi** 'Artış count' tarafından olarak **örnek sayısını** '1' olarak ve **basılı güzel** '5' dakika olarak

7. Tıklatın **Ekle** düğmesi

Bu kural, Web uygulamanızı 5 dakika içinde 10'dan fazla istekleri alır ya da daha düşük bir ek örnek uygulama hizmeti yük yönetmek için planınız için eklenen sağlar.

   ![Genişleme kural oluşturma](./media/monitor-tutorial-autoscale-performance-schedule/Scale-Out-Rule.png)

## <a name="create-a-scale-in-rule"></a>Bir ölçek kuralı oluşturma
Her zaman bir ölçek bileşenini kural genişleme kural eşlik sahip olmanızı öneririz. Her ikisi de sahip kaynaklarınızı değil üzerinde sağlanır sağlar. Yol sağlama geçerli iş yükünü işlemek için gereken daha çalıştıran birden çok örneğe sahip. 

1. 'Pazartesi Cuma profiline'

2. Tıklatın **bir kural eklemek** bağlantı

3. Ayarlama **ölçüm kaynağı** 'diğer kaynak' olmalıdır. Ayarlama **kaynak türü** 'Uygulama Hizmetleri' olarak ve **kaynak** gibi Bu öğreticide daha önce Web uygulaması oluşturuldu.

4. Ayarlama **zaman toplama** 'Toplam' olarak **ölçüm adı** 'İstekleri' olarak ve **zaman çizgisi istatistiği** '' Average' olarak

5. Ayarlama **işleci** 'Değerinden', olarak **eşik** '5' olarak ve **süresi** '5' dakika olarak.

6. Seçin **işlemi** 'Azaltma count' tarafından olarak **örnek sayısını** '1' olarak ve **basılı güzel** '5' dakika olarak

7. Tıklatın **Ekle** düğmesi

    ![Bir ölçek kuralı oluşturma](./media/monitor-tutorial-autoscale-performance-schedule/Scale-In-Rule.png)

8. **Kaydet** otomatik ölçeklendirme ayarı

    ![Otomatik ölçeklendirme ayarı Kaydet](./media/monitor-tutorial-autoscale-performance-schedule/Autoscale-Setting-Save.png)

## <a name="trigger-scale-out-action"></a>Tetikleyici ölçeklendirme eylemi
Yeni oluşturduğunuz otomatik ölçeklendirme ayarında genişleme koşul tetiklemek için Web uygulaması 5 dakikadan daha kısa bir süre içinde 10'dan fazla istekleri olması gerekir.

1. Bir tarayıcı penceresi açın ve Bu öğreticide daha önce oluşturduğunuz Web uygulamasına gidin. URL Azure Portal'da Web uygulamanız için Web uygulaması kaynağınız gittikten tıklayarak bulabilirsiniz **Gözat** 'Genel' sekmesinde düğmesi.

2. Hızlı art arda 10 kereden fazla sayfayı yeniden yükleyin

3. Sol gezinti bölmesinden seçin **İzleyici** seçeneği. Sayfa Seç yüklediğinde sonra **otomatik ölçeklendirme** sekmesi.

4. Uygulama hizmeti planı Bu öğreticide kullanılan listeden seçin

5. Otomatik ölçeklendirme ayarı tıklatın **çalıştırma geçmişi** sekmesi

6. Uygulama hizmeti planı örnek sayısını zamanla yansıtan bir grafik bakın

7. Birkaç dakika içinde örnek sayısı 1'den 2'ye yükselmeye.

8. Grafiğin bu otomatik ölçeklendirme ayarı tarafından yapılan her ölçek eylemi için etkinlik günlük girişlerini bakın.

## <a name="trigger-scale-in-action"></a>Tetikleyici ölçek eylemi
Ölçek bileşenini koşulunda 10 dakikalık bir dönemdeki Web uygulaması için 5'ten az isteği yoksa Tetikleyicileri ayarındaki. 

1. Hiçbir istek, Web uygulamanızın gönderildiğinden emin olun.

2. Azure Portalı'nı yükleme

3. Sol gezinti bölmesinden seçin **İzleyici** seçeneği. Sayfa Seç yüklediğinde sonra **otomatik ölçeklendirme** sekmesi.

4. Uygulama hizmeti planı Bu öğreticide kullanılan listeden seçin

5. Otomatik ölçeklendirme ayarı tıklatın **çalıştırma geçmişi** sekmesi

6. Uygulama hizmeti planı örnek sayısını zamanla yansıtan bir grafik bakın.

7. Birkaç dakika içinde örnek sayısı 2'den 1 olarak bırakın. İşlemi en az 100 dakika sürer.  

8. Grafiğin karşılık gelen etkinlik bu otomatik ölçeklendirme ayarı tarafından yapılan her ölçek eylemi için günlük girişlerini kümesidir

    ![Görünümü ölçek bileşenini eylemleri](./media/monitor-tutorial-autoscale-performance-schedule/Scale-In-Chart.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

1. Azure portalında sol taraftaki menüden **tüm kaynakları** ve sonra Bu öğreticide oluşturduğunuz Web uygulaması seçin.

2. Kaynak sayfanızda tıklatın **silmek**, yazarak Silmeyi Onayla **Evet** metin kutusuna ve ardından **silmek**.

3. Sonra App Service planı kaynak seçin ve tıklatın **Sil**

4. Yazarak Silmeyi Onayla **Evet** metin kutusuna ve ardından **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide,  
> [!div class="checklist"]
> * Oluşturulan Web uygulaması ve uygulama hizmeti planı
> * Ölçek bileşenini için yapılandırılmış otomatik ölçeklendirme kurallarını ve ölçek genişletme alınan Web uygulaması isteklerin sayısına dayalı
> * Bir ölçeklendirme eylemi tetiklenir ve örneklerinin sayısını artırmak izlenen
> * Bir ölçek eylemi tetiklenir ve örneklerinin sayısını azaltmak izlenen
> * Kaynaklarınızın temizlendi


Otomatik ölçeklendirme ayarları hakkında daha fazla bilgi edinmek için devam etmek için [otomatik ölçeklendirme genel bakış](monitoring-overview-autoscale.md).

> [!div class="nextstepaction"]
> [İzleme verilerinizi arşivleme](monitor-tutorial-archive-monitoring-data.md)
