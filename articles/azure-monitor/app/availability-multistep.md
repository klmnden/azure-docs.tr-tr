---
title: Çok adımlı web testleri web uygulamanızla ve Azure Application Insights izleme | Microsoft Docs
description: Azure Application Insights ile web uygulamalarınızı izlemek için Kurulum çok adımlı web testleri
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: d8bfe92af4e8afc4edae76efb2e1cb7b287c7aa9
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304876"
---
# <a name="multi-step-web-tests"></a>Çok adımlı web testleri

Kayıtlı bir dizi URL'nin ve çok adımlı web testleri aracılığıyla bir Web sitesiyle etkileşimleri izleyebilirsiniz. Bu makalede, Visual Studio Enterprise ile çok adımlı web testi oluşturma işlemi boyunca size yol gösterir.

> [!NOTE]
> Çok adımlı web testleri ilişkili ek ücrete sahiptir. Daha fazla consult öğrenmek [resmi fiyatlandırma Kılavuzu](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="pre-requisites"></a>Ön koşullar

* Visual Studio 2017 Enterprise veya büyük.
* Visual Studio web performansı ve yük testi araçları.

Test Araçları önkoşul bulunacak. Başlatma **Visual Studio yükleyicisi** > **tek tek bileşenler** > **hata ayıklama ve test**  >   **Web performansı ve yük testi Araçları**.

![Öğe için Web performansı ve yük testi araçları yanında bir onay kutusu seçili tek tek bileşenler ile Visual Studio yükleyicisi kullanıcı Arabirimi ekran görüntüsü](./media/availability-multistep/web-performance-load-testing.png)

## <a name="record-a-multi-step-web-test"></a>Çok adımlı web testi

Çok adımlı bir test oluşturmak için Visual Studio Enterprise kullanarak senaryoyu kaydedin ve kaydı Application Insights'a yükleyin. Application Insights senaryoyu aralıklarla yeniden yürütür ve yanıt doğrular.

> [!IMPORTANT]
> * Testlerinizde kodlanmış işlevler veya döngüler kullanamazsınız. Test tamamen .webtest betiğinde yer almalıdır. Ancak, standart eklentiler kullanabilirsiniz.
> * Çok adımlı web testlerinde yalnızca İngilizce karakterler desteklenir. Visual Studio’yu diğer dillerde kullanıyorsanız lütfen İngilizce olmayan karakterleri çevirmek/hariç tutmak için web testi tanımı dosyasını güncelleştirin.

Web oturumu kaydetmek için Visual Studio Enterprise kullanın.

1. Bir Web performansı ve yük testi projesi oluşturun. **Dosya** > **yeni** > **proje** > **Visual C#**   >  **Test**

    ![Visual Studio yeni proje kullanıcı Arabirimi](./media/availability-multistep/vs-web-performance-and-load-test.png)

2. Açık `.webtest` dosya ve kaydı başlatın.

    ![Visual Studio test UI kaydetme](./media/availability-multistep/open-web-test.png)

3. Kayıt işleminin bir parçası olarak benzetimini yapmak için testinizi istediğiniz adımları tıklayın.

    ![Tarayıcı kaydı kullanıcı Arabirimi](./media/availability-multistep/record.png)

4. Testi düzenleme nedenleri:

    * Alınan metin ve yanıt kodlarını denetlemek için doğrulama ekleme.
    * Tüm uneccesary etkileşimleri kaldırma. Ayrıca resimler için bağımlı istekleri kaldırın veya bir başarı testinizi dikkate size uygun olmayan izleme sitelerin ekleyin.
    
    Yalnızca test betiğini Düzenle - özel kod ekleyin veya başka web testlerinden çağrı göz önünde bulundurun. Testlere döngü eklemeyin. Standart web testi eklentileri kullanabilirsiniz.

5. Visual Studio'da doğrulamak ve çalıştığından emin olmak için testi çalıştırın.

    Web test çalıştırıcısı bir web tarayıcısı açar ve kaydettiğiniz eylemleri yineler. Her şeyin beklendiği gibi davrandığından emin olun.

## <a name="upload-the-web-test"></a>Web testini karşıya yükleyin

1. Application Insights portalında kullanılabilirlik bölmesinde seçin **oluşturma Test** > **Test türü** > **çok adımlı web testi**.

2. Test konumları, sıklığı ve uyarı parametrelerini ayarlayın.

### <a name="frequency--location"></a>Sıklık & konumu

|Ayar| Açıklama
|----|----|----|
|**Sınama sıklığı**| Testin her test konumdan ne sıklıkta çalıştırılacağını ayarlar. Beş dakikalık varsayılan sıklıkta ve beş test konumuyla, siteniz ortalama olarak dakikada bir test edilir.|
|**Test konumları**| Burada sunucularımızın URL'nize web istekleri göndermek gelen yerlerdir. **Önerilen test konumları bizim en düşük sayısı beştir** , sorunları, Web sitenizdeki ağ sorunlarını ayırt edebilirsiniz emin olmak amacıyla. En fazla 16 konum seçebilirsiniz.

### <a name="success-criteria"></a>Başarı ölçütleri

|Ayar| Açıklama
|----|----|----|
| **Test zaman aşımı** |Yavaş yanıtlar hakkında uyarı almak için bu değeri azaltın. Yanıtlar sitenizden bu süre içinde alınmadıysa test başarısız sayılır. **Bağımlı istekleri ayrıştır**’ı seçtiyseniz; tüm görüntüler, stil dosyaları, betikler ve diğer bağımlı kaynaklar bu süre içinde alınmış olmalıdır.|
| **HTTP yanıtı** | Başarılı sayılan döndürüldü durum kodu. 200, normal web sayfası döndürüldüğünü belirten koddur.|
| **İçerik eşleşmesi** | "Hoş Geldiniz!" gibi bir dize Her yanıtta büyük küçük harfe duyarlı bir tam eşleşme oluştuğunu test edebiliriz. Joker karakter bulunmayan düz bir dize olmalıdır. Sayfanızın içeriği değişirse bunu güncelleştirmeniz gerektiğini unutmayın. **İçerik eşleşmesi ile yalnızca İngilizce karakterler desteklenir** |

### <a name="alerts"></a>Uyarılar

|Ayar| Açıklama
|----|----|----|
|**Neredeyse gerçek zamanlı (Önizleme)** | Neredeyse gerçek zamanlı uyarılar kullanmanızı öneririz. Bu tür bir uyarı yapılandırma, bir kullanılabilirlik testi oluşturduktan sonra yapılır.  |
|**Klasik** | Artık, yeni kullanılabilirlik testleri için Klasik uyarılar kullanılması önerilir.|
|**Uyarı konumu eşiği**|En az 3/5 konumları öneririz. Uyarı konumu eşiği ve test konumları sayısı arasındaki en iyi ilişki **uyarı konumu eşiği** = **sayısı test konumları - 2, en az beş test konumuyla.**|

## <a name="advanced-configuration"></a>Gelişmiş Yapılandırma

### <a name="plugging-time-and-random-numbers-into-your-test"></a>Testinizi süresi ve rasgele rakamlar takma

Dış bir kaynağa ait stoklar gibi zamana bağımlı veriler alan bir aracı test ettiğinizi varsayalım. Web testinizi kaydettiğinizde, belirli zamanları kullanmanız gerekse de, bunları testin parametreleri (StartTime ve EndTime) olarak ayarlarsınız.

![My harika stok uygulama ekran görüntüsü](./media/availability-multistep/app-insights-72webtest-parameters.png)

Testi çalıştırdığınızda, EndTime her zaman geçerli zaman, StartTime da 15 dakika öncesi olmalıdır.

Web Test tarih saat eklentisi işlemek için bir yol sağlar kez parametreleştirin.

1. İstediğiniz her değişken parametre değeri için bir web testi eklentisi ekleyin. Web testi araç çubuğunda, **Web Testi Eklentisi Ekle**’yi seçin.
    
    ![Web Testi Eklentisi Ekle](./media/availability-multistep/app-insights-72webtest-plugin-name.png)
    
    Bu örnekte, Tarih Saat Eklentisinin iki örneğini kullanacağız. Bir örnek "15 dakika önce" için, bir örnek de "şimdi" için.

2. Her eklentinin özelliklerini açın. Buna bir ad verip geçerli saat olarak kullanılmak üzere ayarlayın. Bunlardan birini Dakika Ekle = 15 olarak ayarlayın.

    ![Bağlam parametreleri](./media/availability-multistep/app-insights-72webtest-plugin-parameters.png)

3. Web testi parametrelerinde, eklenti adına başvurmak için {{plug-in name}} kullanın.

    ![StartTime](./media/availability-multistep/app-insights-72webtest-plugins.png)

Artık testi portala yükleyin. Testin her çalıştırılışında dinamik değerler kullanacaktır.

### <a name="dealing-with-sign-in"></a>Oturum açmayla ilgilenme

Kullanıcılarınız uygulamanızda oturum açarsa, oturum açma benzetimi için bir dizi seçeneğiniz vardır; böylece, oturum açmanın ötesinde sayfaları test edebilirsiniz. Kullandığınız yaklaşım, uygulamanın sağladığı güvenlik türüne bağlıdır.

Her durumda, uygulamanızda yalnızca test amacıyla bir hesap oluşturmalısınız. Mümkünse, web testlerinin gerçek kullanıcıları etkileme olasılığını önlemek için test hesabının izinlerini kısıtlayın.

**Basit kullanıcı adı ve parola** web testini normal şekilde kaydedin. Önce tanımlama bilgilerini silin.

**SAML kimlik doğrulaması** web testlerinde kullanıma uygun SAML eklentisini kullanın. Eklenti tarafından erişim...

**İstemci gizli anahtarı** uygulamanızın istemci gizli anahtarını içeren bir oturum açma yolu varsa bu yolu kullanın. Azure Active Directory (AAD), gizli anahtarla oturum açmayı sağlayan bir hizmet örneğidir. AAD’de gizli anahtar, Uygulama Anahtarı’dır.

Aşağıda uygulama anahtarı kullanan bir Azure web uygulaması için web testi örneği verilmiştir:

![Örnek ekran görüntüsü](./media/availability-multistep/client-secret.png)

Gizli anahtar (AppKey) kullanarak AAD’den belirteç alın.
Yanıttan taşıyıcı belirteci ayıklayın.
Yetkilendirme üst bilgisinde taşıyıcı belirteç kullanarak API çağırın.
Web testi gerçek bir istemci olduğundan - diğer bir deyişle, - AAD'de kendi uygulamasına sahip olduğundan emin olun ve kullanmak, ClientID + uygulama anahtarı. Test edilen hizmetiniz de AAD'de kendi uygulamasına sahiptir: URI bu uygulamanın AppID kaynak alanın web testinde yansıtılır.

### <a name="open-authentication"></a>Açık Kimlik Doğrulaması
Microsoft veya Google hesabınızla oturum açma, bir açık kimlik doğrulaması örneğidir. OAuth kullanan çok sayıda uygulama, alternatif gizli anahtar da sağlar; bu nedenle ilk taktiğiniz bu olasılığın incelenmesi olmalıdır.

Testinizde OAuth kullanılarak oturum açılması gerekiyorsa, genel yaklaşım şöyledir:

Web tarayıcınız, kimlik doğrulama sitesi ve uygulamanız arasındaki trafiği incelemek için Fiddler gibi bir araç kullanın.
Farklı makineler veya tarayıcılar kullanarak veya uzun aralıklarla (süresi dolacak şekilde belirteçleri izin vermek için) iki veya daha fazla oturum açın.
Farklı oturumları karşılaştırarak, kimlik doğrulama sitesinden geri geçirilen belirteci tanımlayın; başka bir deyişle oturum açıldıktan sonra uygulama sunucunuza geçirilen belirteç.
Visual Studio’yu kullanarak web testini kaydetme
Belirteçleri parametreleyin; belirteç kimlik doğrulayıcıdan döndürüldüğünde ve sitede sorgu sırasında kullanıldığında parametre ayarı. (Visual Studio testi parametrelemeyi dener, ancak belirteçleri doğru parametrelemez.)

## <a name="troubleshooting"></a>Sorun giderme

Ayrılmış [sorunlarını giderme makalesine](troubleshoot-availability.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanılabilirlik uyarıları](availability-alerts.md)
* [URL ping web testleri](monitor-web-app-availability.md)
