---
title: 'Öğretici: Azure portalını kullanarak Stream Analytics işi oluşturma ve bunu yönetme | Microsoft Docs'
description: Bu öğreticide, telefon araması akışındaki sahte aramaların analiz edilmesi için Azure Stream Analytics’in nasıl kullanılacağını gösteren uçtan uca bir resim sağlanır.
services: stream-analytics
author: sidramadoss
ms.author: sidram
manager: kfile
ms.service: stream-analytics
ms.workload: data-services
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/04/2018
ms.openlocfilehash: 1955fc033e0351be9da89bbee11dc41d6281a63a
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47433999"
---
# <a name="create-a-stream-analytics-job-to-analyze-phone-call-data-and-visualize-results-in-a-power-bi-dashboard"></a>Telefon araması verilerini analiz etmek ve sonuçları bir Power BI panosunda görselleştirmek için Stream Analytics işi oluşturma
 
Bu öğreticide, bir istemci uygulaması tarafından oluşturulan bir örnek telefon aramasının analiz edilmesi için Azure Stream Analytics’in nasıl kullanılacağı gösterilir. İstemci uygulaması tarafından oluşturulan telefon araması verileri aynı sahte aramalardan bazılarını içerir ve söz konusu aramaları filtrelemek için bir Stream Analytics işi tanımlarız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Örnek telefon araması verileri oluşturma ve Azure Olay Hub'larına gönderme  
> * Akış Analizi işi oluşturma   
> * İşin girişlerini ve çıkışlarını yapılandırma  
> * Sahte çağrıları filtrelemek için sorgu tanımlama  
> * İşi test etme ve başlatma  
> * Sonuçları Power BI’da görselleştirme 

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunlara sahip olduğunuzdan emin olun:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.  
* [Azure Portal](https://portal.azure.com/)’da oturum açın.  
* Microsoft İndirme Merkezi’nden [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) telefon araması olay oluşturucu uygulamasını indirebilir veya [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator)’dan kaynak kodu alabilirsiniz.  

## <a name="create-an-azure-event-hub"></a>Azure Olay Hub’ı oluşturma 

Stream Analytics’in sahte arama veri akışını analiz edebilmesi için verileri Azure'a göndermeniz gerekir. Bu öğreticide, [Azure Event Hub’ları](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs) kullanarak verileri Azure’a göndereceksiniz. Bu öğretici için bir olay hub'ı oluşturup olay oluşturucu uygulamasının, arama verilerini bu olay hub'ına göndermesini sağlarsınız. Olay hub'ı oluşturmak için aşağıdaki adımları çalıştırın:

1. Azure portalında oturum açın.  
2. **Kaynak oluştur** > **Nesnelerin İnterneti** > **Olay Hub'ları** seçeneğini belirleyin.  

   ![Olay hub'ı bulma](media/stream-analytics-manage-job/find-eh.png)
3. **Ad alanı oluştur** bölmesini aşağıdaki değerlerle doldurun:  

   |**Ayar**  |**Önerilen değer** |**Açıklama**  |
   |---------|---------|---------|
   |Adı     | myEventHubNS        |  Olay hub'ı ad alanını tanımlamak için benzersiz bir ad.       |
   |Abonelik     |   \<Aboneliğiniz\>      |   Olay hub'ını oluşturmak istediğiniz Azure aboneliğini seçin.      |
   |Kaynak grubu     |   MyASADemoRG      |  **Yeni Oluştur**’u seçin ve hesabınız için yeni bir kaynak grubu adı girin.       |
   |Konum     |   Batı ABD 2      |    Olay hub'ı ad alanının dağıtılabildiği konum.     |

4. Kalan ayarlarda varsayılan seçenekleri kullanın ve **Oluştur**’u seçin.  

   ![Olay hub'ı ad alanı oluşturma](media/stream-analytics-manage-job/create-ehns.png)

5. Ad alanının dağıtımı tamamlandığında **Tüm kaynaklar**’a gidin > Azure kaynak listesinde "myEventHubNS"yi bulun > açmak için seçin.  
6. Daha sonra **+Olay Hub'ı** > **Ad** bölümünde olay hub’ını “MyEventHub” olarak adlandırın. Farklı bir ad kullanabilirsiniz. Geri kalan ayarlar için varsayılan seçenekleri kullanın, **Oluştur**’u seçip dağıtımın başarılı olmasını bekleyin.

   ![Olay hub'ı oluşturma](media/stream-analytics-manage-job/create-eh.png)

### <a name="grant-access-to-the-event-hub-and-get-a-connection-string"></a>Olay hub’ına erişim verme ve bir bağlantı dizesi alma

Bir uygulamanın Azure Olay Hub’larına veri gönderebilmesi için olay hub’ında ilgili erişime izin veren bir ilke olması gerekir. Erişim ilkesi, yetkilendirme bilgilerini içeren bir bağlantı dizesi oluşturur.

1. Önceki adımda “MyEventHub” olarak adlandırıp oluşturduğunuz **Olay Hub’ına** gidin > olay hub’ı bölmesinden **Paylaşılan erişim ilkeleri** seçeneğini belirleyin > **+Ekle** seçeneğini belirleyin.  
2. İlke adını **Mypolicy** olarak ayarlayın > **Yönet**’i seçin > **Oluştur**’u seçin.  

   ![Olay hub'ı paylaşılan erişim ilkesi oluşturma](media/stream-analytics-manage-job/create-ehpolicy.png)

3. İlke dağıtıldıktan sonra ilkeyi açmak için seçin, **Bağlantı dizesi - birincil anahtarı** bulun ve bağlantı dizesinin yanındaki **kopyala** seçeneğini belirleyin.  
4. Bağlantı dizesini bir metin düzenleyicisine yapıştırın. Sonraki bölümde bu bağlantı dizesine ihtiyacınız olacaktır.  

   Bağlantı dizesi şu şekilde görünür:

   `Endpoint=sb://<Your event hub namespace>.servicebus.windows.net/;SharedAccessKeyName=<Your shared access policy name>;SharedAccessKey=<generated key>;EntityPath=<Your event hub name>` 

   Bağlantı dizesinin noktalı virgülle ayrılmış birden çok anahtar-değer çifti içerdiğine dikkat edin: Endpoint, SharedAccessKeyName, SharedAccessKey ve EntityPath.  

5. Bağlantı dizesinden EntityPath çiftini kaldırın. (Kendisinden önceki noktalı virgülü kaldırmayı unutmayın.)

## <a name="start-the-event-generator-application"></a>Olay oluşturucu uygulamasını başlatma

TelcoGenerator uygulamasını başlatmadan önce bunu, daha önce oluşturduğunuz Azure Olay Hub’ına veri gönderecek şekilde yapılandırmanız gerekir.

1. [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) dosyasının içeriklerini ayıklayın.  
2. Tercih ettiğiniz metin düzenleyicisinde `TelcoGenerator\TelcoGenerator\telcodatagen.exe.config` dosyasını açın. (Birden fazla .config dosyası olduğundan doğru olanı açtığınızdan emin olun.)  

3. Config dosyasındaki <appSettings> öğesini şu bilgilerle güncelleştirin:

   * EventHubName anahtarının değerini bağlantı dizesindeki EntityPath değerine ayarlayın.  
   * Microsoft.ServiceBus.ConnectionString anahtarının değerini, önceki bölümdeki 5. adımdan aldığınız EntityPath değeri olmadan bağlantı dizesine ayarlayın.

4. Dosyayı kaydedin.  
5. Daha sonra bir komut penceresi açıp, TelcoGenerator uygulamasının sıkıştırmasını açtığınız klasöre geçin ve aşağıdaki komutu girin:

   ```
   telcodatagen.exe 1000 0.2 2
   ```

   Bu komut aşağıdaki parametreleri alır:
   * **Saat başına arama verisi kaydının sayısı**.  
   * **Sahtekarlık olasılığının yüzdesi**: uygulamanın ne sıklıkta sahte arama benzetimi gerçekleştirmesi gerektiği. 0.2 değeri arama kayıtlarının %20'sinin sahte görüneceğini anlamına gelir.  
   * **Saat cinsinden süre**: uygulamanın çalışması gereken saat sayısı. Ayrıca, komut satırında işlemi sonlandırarak (Ctrl+C) uygulamayı dilediğiniz zaman durdurabilirsiniz.

   Birkaç saniye sonra uygulama, telefon araması kayıtlarını olay hub'ına gönderirken ekranda bu kayıtları görüntülemeye başlar. Telefon araması verileri aşağıdaki alanları içerir:

   |**Kayıt**  |**Tanım**  |
   |---------|---------|
   |CallrecTime    |  Arama başlangıç zamanı için zaman damgası.       |
   |SwitchNum     |  Aramayı bağlamak için kullanılan telefon anahtarı. Bu örnekte, anahtarlar kaynak ülkeyi (ABD, Çin, İngiltere, Almanya veya Avustralya) temsil eden dizelerdir.       |
   |CallingNum     |  Arayanın telefon numarası.       |
   |CallingIMSI     |  Uluslararası Mobil Abone Kimliği (IMSI). Bu, arayanın benzersiz tanımlayıcısıdır.       |
   |CalledNum     |   Arama alıcısının telefon numarası.      |
   |CalledIMSI     |  Uluslararası Mobil Abone Kimliği (IMSI). Bu, arama alıcısının benzersiz tanımlayıcısıdır.       |

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma 

Arama olaylarından oluşan bir akışa sahip olduğunuza göre artık olay hub'ından veri okuyan bir Stream Analytics işi oluşturabilirsiniz.

1. Stream Analytics işi oluşturmak için Azure portalına gidin  

2. **Kaynak oluştur** > **Nesnelerin İnterneti** > **Stream Analytics işleri** seçeneğini belirleyin.  

3. **Yeni Stream Analytics işi** bölmesini aşağıdaki değerlerle doldurun:  

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |İş adı     |  ASATutorial       |   Olay hub'ı ad alanını tanımlamak için benzersiz bir ad.      |
   |Abonelik    |  \<Aboneliğiniz\>   |   İşi oluşturmak istediğiniz Azure aboneliğini seçin.       |
   |Kaynak grubu   |   MyASADemoRG      |   **Var olanı kullan** seçeneğini belirleyin ve hesabınız için yeni bir kaynak grubu adı girin.      |
   |Konum   |    Batı ABD 2     |      İşin dağıtılabileceği konum. En iyi performans için ve bölgeler arasında veri aktarımına yönelik ödeme yapmamanız için işin ve olay hub’ının aynı bölgeye yerleştirilmesi önerilir.      |
   |Barındırma ortamı    | Bulut        |     Stream Analytics işleri buluta veya uca dağıtılabilir. Bulut, Azure Cloud’a dağıtım yapmanıza Edge ise IoT Edge cihazına dağıtım yapmanıza olanak tanır.    |
   |Akış birimleri     |    1       |      Akış birimleri, bir işin yürütülmesi için gereken bilgi işlem kaynaklarını temsil eder. Varsayılan olarak, bu değer 1 olarak ayarlanır. Akış birimlerini ölçeklendirme hakkında bilgi edinmek için [akış birimlerini anlama ve ayarlama](stream-analytics-streaming-unit-consumption.md) başlıklı makaleye bakın.      |

   ![Bir iş oluşturma](media/stream-analytics-manage-job/create-a-job.png)   

4. Geri kalan ayarlar için varsayılan seçenekleri kullanın, **Oluştur**’u seçip dağıtımın başarılı olmasını bekleyin.

## <a name="configure-job-input"></a>İş girişi yapılandırma

Sonraki adım, işin verileri okuması için bir giriş kaynağı tanımlamaktır. Bu öğreticide, önceki bölümde giriş olarak oluşturduğunuz olay hub'ını kullanacaksınız. İşinize girişi yapılandırmak için aşağıdaki adımları çalıştırın:

1. Azure portalından **Tüm kaynakları** bölmesini açın ve ASATutorial Stream Analytics işini bulun.  

2. Stream Analytics iş bölmesinin **İş Topolojisi** bölümünde **Girişler** seçeneğini belirleyin.  

3. **+Akış girişi ekle** (Başvuru girişi, bu öğreticide kullanmayacağınız statik arama verilerini belirtir) ve **Olay hub'ı** seçeneğini belirleyip bölmeyi şu değerlerle doldurun:  

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Girdi diğer adı     |  CallStream       |  Girişinizi tanımlayan bir kolay ad girin. Giriş diğer adı alfasayısal karakter, kısa çizgi ve alt çizgi içerebilir, ayrıca 3 ila 63 karakter uzunluğunda olmalıdır.       |
   |Abonelik    |   \<Aboneliğiniz\>      |   Olay hub’ını oluşturduğunuz Azure aboneliğini seçin. Olay hub’ı Stream Analytics işiyle aynı abonelikte veya bundan farklı bir abonelikte olabilir.       |
   |Olay hub’ı ad alanı    |  MyEventHubNS       |  Önceki bölümde oluşturduğunuz olay hub’ı ad alanını seçin. Geçerli aboneliğinizde kullanılabilen tüm olay hub’ı ad alanları açılır menüde listelenir.       |
   |Olay Hub'ı adı    |   MyEventHub      |  Önceki bölümde oluşturduğunuz olay hub’ını seçin. Geçerli aboneliğinizde kullanılabilen tüm olay hub’ları açılır menüde listelenir.       |
   |Olay Hub'ı ilke adı   |  Mypolicy       |  Önceki bölümde oluşturduğunuz olay hub’ı paylaşılan erişim ilkesini seçin. Geçerli aboneliğinizde kullanılabilen tüm olay hub’ı ilkeleri açılır menüde listelenir.       |

   ![Girişi yapılandırma](media/stream-analytics-manage-job/configure-input.png) 

4. Geri kalan ayarlar için varsayılan seçenekleri kullanın, **Kaydet**’i seçip dağıtımın başarılı olmasını bekleyin.

## <a name="configure-job-output"></a>İş çıkışını yapılandırma 

Son adım, işin dönüştürülmüş verileri yazabileceği bir çıkış havuzu tanımlamaktır. Bu öğreticide Power BI’da sonuçların çıkışını alır ve verileri görselleştirirsiniz. Çıkışı işinize yönelik olarak yapılandırmak için aşağıdaki adımları çalıştırın:

1. Azure portalından **Tüm kaynaklar** bölmesini açın ve ASATutorial Stream Analytics işini bulun.  

2. Stream Analytics iş bölmesinin **İş Topolojisi** bölümünde **Çıkışlar** seçeneğini belirleyin.  

3. **+Ekle** > **Power BI** seçeneğini belirleyin ve formu aşağıdaki bilgilerle (Çıkış diğer adını, Veri Kümesi adını ve Tablo adını tabloda gösterildiği şekilde tanımlamak için bir kolay ad sağlayabilirsiniz) doldurarak **Yetkilendir** seçeneğini belirleyin:  

   |**Ayar**  |**Önerilen değer**  |
   |---------|---------|---------|
   |Çıktı diğer adı  |  MyPBIoutput  |
   |Veri kümesi adı  |   ASAdataset  | 
   |Tablo adı |  ASATable  | 

   ![Çıkışı yapılandırma](media/stream-analytics-manage-job/configure-output.png)  

4. **Yetkilendir** seçeneğini belirledikten sonra bir açılır pencere görünür ve Power BI hesabınızda kimlik doğrulaması için sizden kimlik bilgilerini sağlamanız istenir. Yetkilendirme başarılı olduktan sonra **Kaydet** seçeneğine tıklayarak ayarları kaydedin. 

## <a name="define-a-query-to-analyze-input-data"></a>Giriş verilerini analiz etmek için sorgu tanımlama

Gelen bir veri akışını okumak için bir Stream Analytics işi ayarladıktan sonraki adım, verileri gerçek zamanlı olarak analiz eden bir dönüştürme oluşturmaktır. Dönüştürme sorgusunu [Stream Analytics sorgu dilini](https://msdn.microsoft.com/library/dn834998.aspx) kullanarak tanımlarsınız. Bu öğreticide, telefon verilerinden elde edilen sahte aramalarını algılayan bir sorgu tanımlarsınız. 

Bu örnekte, sahte aramaların ayrı konumlarda ancak aynı kullanıcıdan geldiğini, iki arama arasındaki sürenin ise beş saniye olduğunu varsayıyoruz. Örneğin, bir kullanıcı mantıksal olarak aynı anda hem ABD’den hem de Avustralya’dan arama yapamaz. Stream Analytics işiniz için dönüştürme sorgusu tanımlamak amacıyla aşağıdaki adımları çalıştırın:

1. Azure portalından **Tüm kaynaklar** bölmesini açın ve daha önce oluşturduğunuz **ASATutorial** Stream Analytics işini açın.  

2. Stream Analytics iş bölmesinin **İş Topolojisi** bölümünde **Sorgu** seçeneğini belirleyin. Açılır pencere, iş için yapılandırılan girişleri ve çıkışları listeler, giriş akışını dönüştürmek için sorgu oluşturmanızı sağlar.  

3. Daha sonra, düzenleyicideki mevcut sorguyu aşağıdaki verilerle değiştirin. Bu sorgu, arama verilerinin 5 saniyelik aralığına denk gelen bir iç birleşim gerçekleştirir:

   ```sql
   SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
   INTO "MyPBIoutput"
   FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
   JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime
   ON CS1.CallingIMSI = CS2.CallingIMSI
   AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
   WHERE CS1.SwitchNum != CS2.SwitchNum
   GROUP BY TumblingWindow(Duration(second, 1))
   ```

   Sahte aramaları denetlemek için `CallRecTime` değerine göre akış verilerinde iç birleşim uygulamanız gerekir. Böylece `CallingIMSI` değerinin (kaynak numara) aynı olduğu ancak `SwitchNum` değerinin (kaynak ülke) farklı olduğu arama kayıtlarını bulabilirsiniz. Akış verileriyle bir JOIN işlemi kullandığınızda birleştirme, eşleşen satırların zaman içinde ne kadar ayrılabildiğine ilişkin bazı sınırlar sağlamalıdır. Veri akışı sonsuz olduğundan ilişki için zaman sınırları, birleştirme işleminin ON yan tümcesi içinde [DATEDIFF](https://msdn.microsoft.com/azure/stream-analytics/reference/datediff-azure-stream-analytics) işlevi kullanılarak belirtilir. 

   Bu sorgu, DATEDIFF işlevi dışında yalnızca normal bir SQL birleştirme işlemi gibidir. Bu sorguda kullanılan DATEDIFF işlevi Stream Analytics’e özeldir ve `ON...BETWEEN` yan tümcesi içinde görünmelidir.  

4. Sorguyu **kaydedin**.  

   ![sorgu tanımlama](media/stream-analytics-manage-job/define-query.png) 

## <a name="test-your-query"></a>Sorgunuzu test etme

Bir sorguyu sorgu düzenleyicisinden test edebilirsiniz ve sorgu testi için örnek verilere ihtiyacınız vardır. Bu kılavuz için event hub'ına gelen telefon araması akışındaki örnek verileri ayıklayacaksınız. Sorguyu test etmek için aşağıdaki adımları çalıştırın:

1. TelcoGenerator uygulamasının çalıştığından ve telefon araması kayıtları oluşturduğundan emin olun.  

2. **Sorgu**bölmesinde CallStream girişinin yanındaki noktaları seçin ve sonra **Girişten alınan örnek veriler** seçeneğini belirleyin. Bu, giriş akışından okunacak örnek veri miktarını belirtmenize olanak tanıyan bir bölmeyi açar.  

   ![Örnek girişi verileri](media/stream-analytics-manage-job/sample-input-data.png)  

3. **Dakika**’yı 3 olarak ayarlayıp **Tamam**’ı seçin. Üç dakikalık veriler, giriş akışından örneklenir ve örnek veriler hazır olduğunda bilgilendirilirsiniz. Bildirim çubuğundan örneklemenin durumunu görüntüleyebilirsiniz. 

   Örnek veriler geçici olarak depolanır ve sorgu penceresi açıkken kullanılabilir. Sorgu penceresini kapatırsanız örnek veriler atılır ve yeni bir örnek veri kümesi oluşturmanız gerekir. Alternatif olarak, [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json)’dan örnek veriler içeren bir .json dosyası alabilir ve bunu CallStream girişi için örnek veri olarak kullanmak üzere karşıya yükleyebilirsiniz.  

4. Sorguyu test etmek için **Test**’i seçtiğinizde çıkış sonuçlarını şu ekran görüntüsünde gösterildiği görürsünüz:  

   ![Test çıkışı](media/stream-analytics-manage-job/test-output.png)

## <a name="start-the-job-and-visualize-output"></a>İş başlatma ve çıkışı görselleştirme

1. İşlemi başlatmak için işinizin **Genel Bakış** bölmesine gidin ve **Başlat** seçeneğini belirleyin.  

2. İş çıkışı başlangıç saati için **Şimdi**’yi seçip **Başlat** seçeneğini belirleyin. İş birkaç dakika içinde başlar ve bildirim çubuğunda durumu görüntüleyebilirsiniz.  

3. İş başarıyla tamamlandıktan sonra [Powerbi.com](https://powerbi.com/) adresine gidip iş veya okul hesabınızla oturum açın. Stream Analytics işi sorgusu, sonuçların çıkışını veriyorsa veri kümenizin zaten oluşturulmuş olduğunu görürsünüz. **Veri Kümeleri** sekmesine gidin, burada “ASAdataset” adlı bir veri kümesini görüntüleyebilirsiniz.  

4. Çalışma alanınızdan **+Oluştur** seçeneğini belirleyin. Yeni bir pano oluşturun ve bunu Sahte Aramalar olarak adlandırın. Bu panoya iki kutucuk eklersiniz. Bu kutucuklardan biri belirli bir örnekte sahte aramaların sayısının görüntülenmesi için kullanılır, diğeri ise çizgi grafik görselleştirmesi içerir.  

5. Pencerenin üst tarafında **Kutucuk ekle** seçeneğini belirleyin > **Özel Akış Verileri** > İleri’yi seçin > Görselleştirme türü için **ASAdataset**’i seçin > Görselliğini türünü seçin **Kart** > ve Alanlar’ı **fradulentcalls** olarak belirleyin. **İleri** seçeneğini belirleyin > kutucuk için bir ad girin ve **Uygula**’yı seçin.  

   ![Kutucuk oluşturma](media/stream-analytics-manage-job/create-tiles.png)

6. 4. adımı aşağıdaki seçeneklerle tekrar uygulayın:
   * Görselleştirme Türü’ne geldiğinizde Çizgi grafik seçeneğini belirleyin.  
   * Eksen ekleyin ve **windowend** seçeneğini belirleyin.  
   * Değer ekleyip **fraudulentcalls** seçeneğini belirleyin.  
   * **Görüntülenecek zaman penceresini** için son 10 dakikayı seçin.  

7. Her iki kutucuk da eklendikten sonra panonuz aşağıdaki ekran görüntüsündeki gibi görünür. Olay hub’ı gönderen uygulamanız ve Stream Analytics uygulamanız çalışıyorsa, yeni veriler ulaştıkça Power BI panonuzun düzenli olarak güncelleştirildiğini fark edersiniz.  

   ![Power BI sonuçları](media/stream-analytics-manage-job/power-bi-results.png)

## <a name="embedding-your-powerbi-dashboard-in-a-web-application"></a>Power BI Panonuzu bir Web Uygulamasına Ekleme

Öğreticinin bu bölümünde, PowerBI ekibi tarafından panonuza eklenmesi için oluşturulan örnek bir [ASP.NET](http://asp.net/) web uygulamasını kullanacaksınız. Pano ekleme hakkında daha fazla bilgi için [Power BI ile ekleme](https://docs.microsoft.com/power-bi/developer/embedding) başlıklı makaleye bakın.

Bu öğreticide, User Owns Data (Verilerin Sahibi Kullanıcıdır) uygulamasına yönelik adımları uygulayacağız. Uygulamayı ayarlamak için [PowerBI-Developer-Samples](https://github.com/Microsoft/PowerBI-Developer-Samples) Github deposuna gidin ve **User Owns Data** (Verilerin Sahibi Kullanıcıdır) bölümündeki yönergeleri uygulayın. (**integrate-dashboard-web-app** alt bölümündeki yönlendirme ve ana sayfa URL’lerini kullanın.) Pano örneğini kullandığımızdan [GitHub deposunda](https://github.com/Microsoft/PowerBI-Developer-Samples/tree/master/User%20Owns%20Data/integrate-dashboard-web-app) bulunan integrate-dashboard-web-app örnek kodunu kullanın.
Uygulamayı tarayıcınızda çalıştırmaya başladıktan sonra, daha önce oluşturduğunuz panoyu web sayfasına eklemek için şu adımları uygulayın:

1. Uygulamaya, PowerBI hesabınızdaki panolara erişme izni veren **Power BI'da oturum aç** seçeneğini belirleyin.  

2. Hesabınızın Panolarını bir tabloda görüntüleyen **Pano Al** düğmesini seçin. Daha önce oluşturduğunuz panonun adını (powerbi-embedded-dashboard) bulun ve ilgili **EmbedUrl**’yi kopyalayın.  

3. Son olarak, **EmbedUrl**’yi ilgili metin alanına yapıştırıp **Panoyu Ekle** seçeneğini belirleyin. Artık bir web uygulamasının içine eklenen panoyu görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, basit bir Stream Analytics işi oluşturdunuz, gelen verileri analiz ettiniz ve sonuçları bir Power BI panosunda sundunuz. Stream Analytics işleri hakkında daha fazla bilgi edinmek için sonraki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Stream Analytics işlerinde Azure İşlevleri’ni çalıştırma](stream-analytics-with-azure-functions.md)
