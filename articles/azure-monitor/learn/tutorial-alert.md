---
title: Azure Application Insights'tan uyarıları göndermek | Microsoft Docs
description: Azure Application Insights kullanarak uygulamanızdaki hataları yanıt uyarılar gönderme Öğreticisi.
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 04/10/2019
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 05285a177827cd0dd1e0e39e779a395ccfdfc0cd
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59578773"
---
# <a name="monitor-and-alert-on-application-health-with-azure-application-insights"></a>İzleme ve uyarılar Azure Application Insights ile uygulama durumu

Azure Application Insights sayesinde uygulamanızı izlemek ve bunu geçerli olduğunda uyarıları göndermek hataları yaşıyor veya performans sorunları yaşıyorsa kullanılamaz.  Bu öğreticide, uygulamanızın kullanılabilirliğini sürekli olarak denetlemek için testler oluşturma işlemine götürür.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Uygulamanın yanıt sürekli olarak denetlemek için kullanılabilirlik testi oluşturun
> * Bir sorun oluştuğunda yöneticilere e-posta Gönder

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

Oluşturma bir [Application Insights kaynağı](https://docs.microsoft.com/azure/azure-monitor/learn/dotnetcore-quick-start#enable-application-insights).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-availability-test"></a>Kullanılabilirlik testi oluşturun

Application ınsights kullanılabilirlik testleri, otomatik olarak uygulamanızı dünyanın her yerindeki çeşitli konumlardan test olanak tanır.   Bu öğreticide, web uygulamanızın kullanılabilir olduğundan emin olmak için bir url testi gerçekleştirir.  Ayrıca, ayrıntılı işlem test etmek için izlenecek tam yol da oluşturabilirsiniz. 

1. **Application Insights**’ı ve sonra aboneliğinizi seçin.  

2. Seçin **kullanılabilirlik** altında **Araştır** menüsünü seçin ve ardından **Oluştur test**.

    ![Kullanılabilirlik testi Ekle](media/tutorial-alert/add-test-001.png)

3. Test için bir ad yazın ve diğer varsayılan değerleri bırakın.  Bu seçim isteklerini uygulama URL'si için 5 dakikada beş farklı coğrafi konumlardan tetikler.

4. Seçin **uyarılar** açmak için **uyarılar** nerede tanımlayabilirsiniz nasıl test başarısız olursa yanıt ayrıntılarını açılır. Seçin **neredeyse gerçek zamanlı** ve durum kümesine **etkin.**

    Tür uyarı ölçütleri ne zaman gönderileceği bir eposta adresi karşılanıyorsa.  İsteğe bağlı olarak adresi yazabilirsiniz ne zaman çağırmak için bir Web kancası Uyarı ölçütleri karşılanıyorsa.

    ![Test oluşturma](media/tutorial-alert/create-test-001.png)

5. Test paneline dönün, üç nokta simgesini seçin ve yapılandırma için neredeyse gerçek zamanlı uyarı girmek için uyarıyı Düzenle.

    ![Uyarıyı düzenle](media/tutorial-alert/edit-alert-001.png)

6. Büyüktür veya eşittir 3 başarısız konum ayarlayın. Oluşturma bir [eylem grubu](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) yapılandırma, uyarı eşik aşıldığında kimin bildirim.

    ![Uyarı UI Kaydet](media/tutorial-alert/save-alert-001.png)

7. Uyarınız yapılandırdıktan sonra her konumdan ayrıntılarını görüntülemek için test adına tıklayın. Testler, belirtilen zaman aralığı için başarı/hata görselleştirmek için her iki çizgi grafik ve dağılım çizim biçimde görüntülenebilir.

    ![Test ayrıntıları](media/tutorial-alert/test-details-001.png)

8. Herhangi bir testi ayrıntılarına dağılım grafiği, nokta tıklayarak detaya gidebilirsiniz. Bu uçtan uca işlem Ayrıntıları görünümü başlatılır. Aşağıdaki örnek, bir başarısız istek ayrıntılarını gösterir.

    ![Test sonucu](media/tutorial-alert/test-result-001.png)
  
## <a name="next-steps"></a>Sonraki adımlar

Sorunları hakkında uyarmak öğrendiniz, kullanıcıların uygulamanızla nasıl etkileşim çözümleneceği hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Kullanıcıları anlama](../../azure-monitor/learn/tutorial-users.md)