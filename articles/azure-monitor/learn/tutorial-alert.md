---
title: Azure Application Insights'tan uyarıları göndermek | Microsoft Docs
description: Azure Application Insights kullanarak uygulamanızdaki hataları yanıt uyarılar gönderme Öğreticisi.
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 300f0ddc8b738b5fd8578ed0b33cc15000c1098a
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54101794"
---
# <a name="monitor-and-alert-on-application-health-with-azure-application-insights"></a>İzleme ve uyarılar Azure Application Insights ile uygulama durumu

Azure Application Insights sayesinde uygulamanızı izlemek ve bunu geçerli olduğunda uyarıları göndermek hataları yaşıyor veya performans sorunları yaşıyorsa kullanılamaz.  Bu öğretici, uygulamanızın kullanılabilirliğini sürekli olarak denetlemek için testler oluşturma işleminde size yol gösterir ve yanıt olarak uyarıları farklı türde göndermek için sorunları algılandı.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Uygulamanın yanıt sürekli olarak denetlemek için kullanılabilirlik testi oluşturun
> * Bir sorun oluştuğunda yöneticilere e-posta Gönder
> * Performans ölçümlerine bağlı uyarılar oluşturma 
> * Bir zamanlamaya göre özetlenmiş telemetri göndermek için bir mantıksal uygulama kullanın.


## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

- [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
    - ASP.NET ve web geliştirme
    - Azure geliştirme
    - Azure’a .NET uygulaması dağıtma ve [Application Insights SDK’sını etkinleştirme](../../azure-monitor/app/asp-net.md). 


## <a name="log-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-availability-test"></a>Kullanılabilirlik testi oluşturun
Application ınsights kullanılabilirlik testleri, otomatik olarak uygulamanızı dünyanın her yerindeki çeşitli konumlardan test olanak tanır.   Bu öğreticide, uygulamanın kullanılabilir olduğundan emin olmak için basit bir test gerçekleştirir.  Ayrıca, ayrıntılı işlem test etmek için izlenecek tam yol da oluşturabilirsiniz. 

1. **Application Insights**’ı ve sonra aboneliğinizi seçin.  
1. Seçin **kullanılabilirlik** altında **Araştır** menüsünü seçin ve ardından **Ekle test**.
 
    ![Kullanılabilirlik testi Ekle](media/tutorial-alert/add-test.png)

2. Test için bir ad yazın ve diğer varsayılan değerleri bırakın.  Bu giriş sayfası her 5 dakikada bir uygulamanın 5 farklı coğrafi konumlardan ister. 
3. Seçin **uyarılar** açmak için **uyarılar** nerede tanımlayabilirsiniz nasıl test başarısız olursa yanıt ayrıntılarını paneli. Tür uyarı ölçütleri ne zaman gönderileceği bir eposta adresi karşılanıyorsa.  İsteğe bağlı olarak adresi yazabilirsiniz ne zaman çağırmak için bir Web kancası Uyarı ölçütleri karşılanıyorsa.

    ![Test oluşturma](media/tutorial-alert/create-test.png)
 
4. Test paneline dönün ve birkaç dakika sonra kullanılabilirlik testi sonuçlarını görmeye başlayacaksınız.  Her konumdan ayrıntılarını görüntülemek için test adına tıklayın.  Dağılım Grafiği, her test süresi ve başarı gösterir.

    ![Test ayrıntıları](media/tutorial-alert/test-details.png)

5.  Belirli bir testin ayrıntılarını dağılım grafiği, nokta tıklayarak detaya gidebilirsiniz.  Aşağıdaki örnek, bir başarısız istek ayrıntılarını gösterir.

    ![Test sonucu](media/tutorial-alert/test-result.png)
  
6. Uyarı ölçütü karşılanırsa, aşağıdakine benzer bir posta, belirttiğiniz adrese gönderilir.

    ![Uyarı e-posta](media/tutorial-alert/alert-mail.png)


## <a name="create-an-alert-from-metrics"></a>Ölçümleri bir uyarı oluştur
Bir kullanılabilirlik testi uyarı göndermeye ek olarak uygulamanız için bir uyarı toplanmakta olan herhangi bir performans ölçümlerinden oluşturabilirsiniz.

2. Seçin **uyarılar** gelen **yapılandırma** menüsü.  Bu, Azure Uyarıları'paneli açılır.  Burada diğer hizmetler için yapılandırılan diğer uyarı kuralları olabilir.
3. Tıklayın **ölçüm uyarısı Ekle**.  Bu, yeni bir uyarı kuralı oluşturmak için paneli açılır.

    ![Ölçüm uyarısı Ekle](media/tutorial-alert/add-metric-alert.png)

4. Yazın bir **adı** uyarı kuralı ve uygulamanız için açılan listeyi seçin **kaynak**.
5. Seçin bir **ölçüm** örneği.  Son 24 saat içinde bu istek değeri belirtmek için bir grafik görüntülenir.  Bu ölçüm için koşul ayarını destekler.

    ![Uyarı kuralı ekle](media/tutorial-alert/add-alert-01.png)

6. Belirtin bir **koşul** ve **eşiği** uyarı. Bu ölçüm, bir uyarı oluşturulmasını aşan gerekir sayısıdır. 
6. Altında **şu şekilde bildir** denetleyin **e-posta sahipleri, Katkıda Bulunanlar ve okuyucular** kutusunu Uyarı koşulu karşılandığında ve herhangi ek alıcılar e-posta adresini eklediğinizde bu kullanıcılara bir e-posta gönderin.  Ayrıca, bir Web kancası veya koşul karşılandığında çalıştıran bir mantıksal uygulama burada da belirtebilirsiniz.  Bu, algılanan sorunu hafifletmeye denemek için kullanılabilir veya 

    ![Uyarı kuralı ekle](media/tutorial-alert/add-alert-02.png)


## <a name="proactively-send-information"></a>Proaktif olarak bilgi gönder
Uyarıları, sorunları, uygulamanızda tanımlanan belirli bir dizi olarak oluşturulur ve Acil dikkat gerektiren kritik durumlar için uyarılar genellikle ayırın.  Proaktif bir şekilde uygulamanızı otomatik olarak bir zamanlamaya göre çalışan bir mantıksal uygulama ile ilgili bilgi elde edebilir.  Örneğin, yöneticiler daha fazla değerlendirme gerektirir günlük özet bilgilerle birlikte gönderilen bir e-posta olabilir.

Application Insights ile bir mantıksal uygulama oluşturma hakkında daha fazla bilgi edinmek için bkz: [otomatikleştirmek Application ınsights'ı işler Logic Apps'i kullanarak](../../azure-monitor/app/automate-with-logic-apps.md)

## <a name="next-steps"></a>Sonraki adımlar
Sorunları hakkında uyarmak öğrendiniz, kullanıcıların uygulamanızla nasıl etkileşim çözümleneceği hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Kullanıcıları anlama](../../azure-monitor/learn/tutorial-users.md)