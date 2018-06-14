---
title: Azure Application Insights uyarıları göndermek | Microsoft Docs
description: Azure Application Insights kullanarak uygulamanızı hatalara yanıt uyarıları göndermek üzere Öğreticisi.
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 39e2f136e30ebb6dcfc003c435382f3384af1052
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
ms.locfileid: "23947231"
---
# <a name="monitor-and-alert-on-application-health-with-azure-application-insights"></a>İzleyici ve Azure Application Insights ile uygulama sistem durumu Uyarısı

Azure Application Insights uygulamanızı izlemek ve ya da olduğunda uyarıları göndermek sağlar hataları yaşıyor veya performans sorunlarını karşılaşan kullanılamaz.  Bu öğretici sürekli uygulamanızı kullanılabilirliğini denetlemek için testler oluşturma işlemi boyunca sürer ve yanıt olarak uyarıları farklı türde göndermek için ilgili sorunlar algıladı.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Sürekli olarak uygulama yanıt denetlemek için kullanılabilirlik testi oluşturma
> * Bir sorun ortaya çıktığında posta yöneticilere gönder
> * Performans ölçümleri temelinde uyarılar oluştur 
> * Bir zamanlamaya göre özetlenmiş telemetri göndermek için bir mantıksal uygulama kullanın.


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
    - ASP.NET ve web geliştirme
    - Azure geliştirme
    - .NET uygulaması azure'a dağıtma ve [Application Insights SDK'sı etkinleştirmek](app-insights-asp-net.md). 


## <a name="log-in-to-azure"></a>Azure'da oturum açma
Oturum açtığınızda Azure portalında [https://portal.azure.com](https://portal.azure.com).

## <a name="create-availability-test"></a>Kullanılabilirlik testi oluşturma
Application Insights kullanılabilirlik testleri, otomatik olarak dünyanın çeşitli konumlardan uygulamanızı test etmek izin verir.   Bu öğreticide, uygulama kullanılabilir olduğundan emin olmak için basit bir sınama gerçekleştirir.  Ayrıntılı işlem test etmek için izlenecek tam yol da oluşturabilirsiniz. 

1. Seçin **Application Insights** ve aboneliğinizi seçin.  
1. Seçin **kullanılabilirlik** altında **Araştır** menüsünü seçin ve ardından **Ekle test**.
 
    ![Kullanılabilirlik testi ekleyin](media/app-insights-tutorial-alert/add-test.png)

2. Test için bir ad yazın ve diğer Varsayılanları bırakabilir.  Bu uygulamanın her 5 dakikada bir giriş sayfası 5 farklı coğrafi konumlardan ister. 
3. Seçin **uyarıları** açmak için **uyarıları** burada tanımlayabilirsiniz nasıl test başarısız olursa yanıt ayrıntılarını paneli. Türü Uyarı ölçütleri ne zaman gönderileceği bir eposta adresi karşılanır.  İsteğe bağlı olarak adresi yazabilirsiniz ne zaman çağırmak için bir Web kancası Uyarı ölçütleri karşılanıyorsa.

    ![Testi Oluşturma](media/app-insights-tutorial-alert/create-test.png)
 
4. Test Masası'na dönün ve birkaç dakika sonra kullanılabilirlik test sonuçlarından görmeye başlayacaksınız.  Her konumdan ayrıntılarını görüntülemek için test adına tıklayın.  Dağılım Grafiği başarı ve her test süresini gösterir.

    ![Test Ayrıntıları](media/app-insights-tutorial-alert/test-details.png)

5.  Herhangi bir belirli test ayrıntılarını kendi nokta dağılım grafiğindeki tıklayarak ayrıntıya girebilirsiniz.  Aşağıdaki örnek, başarısız bir istek ayrıntılarını gösterir.

    ![Test sonucu](media/app-insights-tutorial-alert/test-result.png)
  
6. Uyarı ölçütü karşılanırsa aşağıdaki benzer bir posta belirttiğiniz adresine gönderilir.

    ![Uyarı posta](media/app-insights-tutorial-alert/alert-mail.png)


## <a name="create-an-alert-from-metrics"></a>Ölçümleri bir uyarı oluşturabilir.
Bir kullanılabilirlik sınamadan uyarılar gönderme ek olarak, uygulamanız için bir uyarı, toplanmakta olan tüm performans ölçümleri oluşturabilirsiniz.

2. Seçin **uyarıları** gelen **yapılandırma** menüsü.  Azure uyarıları paneli açılır.  Burada diğer hizmetler için yapılandırılmış diğer uyarı kuralları olabilir.
3. Tıklatın **ölçüm uyarı Ekle**.  Yeni bir uyarı kuralı oluşturmak için Denetim Masası açılır.

    ![Ölçüm uyarısı ekleme](media/app-insights-tutorial-alert/add-metric-alert.png)

4. Yazın bir **adı** uyarı kuralı ve uygulamanız için açılan listeyi seçin **kaynak**.
5. Seçin bir **ölçüm** örneği.  Son 24 saat içinde bu isteği değerini belirtmek için bir grafik görüntülenir.  Bu, ölçüm için koşul ayarı yardımcı olur.

    ![Uyarı kuralı Ekle](media/app-insights-tutorial-alert/add-alert-01.png)

6. Belirtin bir **koşulu** ve **eşik** uyarı. Bu ölçüm, bir uyarı oluşturulacak aşan gerekir sayısıdır. 
6. Altında **aracılığıyla bildir** denetleyin **sahipleri, Katkıda Bulunanlar ve okuyucular e-posta** kutusunu Uyarı koşulu karşılanır ve herhangi bir ek alıcıların e-posta adresi eklediğinizde bir posta bu kullanıcılara gönderin.  Ayrıca, bir Web kancası veya koşul karşılandığında çalıştırılan bir mantıksal uygulama burada belirtebilirsiniz.  Bu, algılanan sorun azaltmak girişimi için kullanılabilir veya 

    ![Uyarı kuralı Ekle](media/app-insights-tutorial-alert/add-alert-02.png)


## <a name="proactively-send-information"></a>Proaktif olarak bilgi gönder
Acil dikkat gerektiren önemli koşullar için uyarıları genellikle ayırmak ve uyarılar, uygulamanızda tanımlanan sorunların belirli bir dizi olarak oluşturulur.  Uygulamanız bir zamanlamaya göre otomatik olarak çalışan bir mantıksal uygulama ile ilgili bilgi proaktif olarak alabilir.  Örneğin, yöneticilere ek değerlendirme gerektirip günlük özet bilgi gönderilen bir posta olabilir.

Application Insights ile bir mantıksal uygulama oluşturma hakkında daha fazla bilgi için bkz: [otomatikleştirmek Application Insights işler Logic Apps kullanarak](automate-with-logic-apps.md)

## <a name="next-steps"></a>Sonraki adımlar
Artık sorunları hakkında uyarmak nasıl öğrendiğinize göre kullanıcılar uygulama ile nasıl etkileşim çözümlemeyi öğrenin için sonraki öğretici için ilerleyin.

> [!div class="nextstepaction"]
> [Kullanıcıların anlama](app-insights-tutorial-users.md)