---
title: Azure - Azure Logic Apps ile IBM ana bilgisayarları üzerinde 3270 uygulamalara bağlanma
description: Tümleştirme ve Azure Logic Apps ve IBM 3270 bağlayıcısını kullanarak Azure ile 3270 ekran temelli uygulamalar otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ChristopherHouser
ms.author: chrishou
ms.reviewer: estfan, valthom
ms.topic: article
ms.date: 03/06/2019
tags: connectors
ms.openlocfilehash: 7388dc0c61dad9c31da0c178febcee4c8481bc50
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60538911"
---
# <a name="integrate-3270-screen-driven-apps-on-ibm-mainframes-with-azure-by-using-azure-logic-apps-and-ibm-3270-connector"></a>IBM ana bilgisayarları üzerinde 3270 ekran temelli uygulamalar, Azure Logic Apps ve IBM 3270 bağlayıcısını kullanarak Azure ile tümleştirin

> [!NOTE]
> Bu bağlayıcı bulunduğu [ *genel Önizleme*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

Azure Logic Apps ve IBM 3270 Bağlayıcısı ile erişebilir ve genellikle 3270 öykünücü ekranlarında giderek sürücü IBM ana bilgisayar uygulamalarını çalıştırın. Bu şekilde, IBM ana bilgisayar uygulamalarınızı Azure, Microsoft ve diğer uygulamaları, hizmetleri ve sistemleri ile Azure Logic Apps ile otomatik iş akışları oluşturarak tümleştirebilirsiniz. Bağlayıcı ile IBM ana bilgisayarları TN3270 protokolünü kullanarak iletişim kurar ve Azure kamu ve Azure Çin 21Vianet dışında tüm Azure Logic Apps bölgelerinde kullanılabilir. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

Bu makalede bu görünüşler 3270 Bağlayıcısı'nı kullanma: 

* Neden Azure Logic Apps ve bağlayıcı 3270 ekran temelli uygulamalar nasıl çalıştığını IBM 3270 bağlayıcıyı kullanma

* Önkoşullar ve Kurulum 3270 Bağlayıcısı'nı kullanma

* Mantıksal uygulamanızı 3270 bağlayıcı eylemler ekleme adımları

## <a name="why-use-this-connector"></a>Bu bağlayıcıyı neden kullanmalısınız?

IBM ana bilgisayarları şirket uygulamalarına erişmek için genellikle genellikle "Yeşil ekran" olarak adlandırılan bir 3270 bir terminal öykünücü kullanın. Bu yöntem, zaman içinde kendini kanıtlamış bir yoludur, ancak sınırlamaları vardır. Bu uygulamaları ile doğrudan bazı durumlarda, ekran ve iş mantığı ayırarak Host Integration Server (HIS) yardımcı mümkün olmayabilir rağmen. Veya belki de artık konak uygulamaların nasıl çalıştığı hakkında bilgi yok.

Bu senaryolar genişletmek için Azure Logic Apps IBM 3270 bağlayıcısında 3270 tasarım kaydı veya "yakalama" kullandığınız, aracı belirli bir görev için kullanılan konak ekranlar çalışır, bu görev, ana bilgisayar uygulaması aracılığıyla gezinti akışı tanımlamak ve tanımlayın Bu görev giriş ve çıkış parametreleri olan yöntemleri. Tasarım aracı bu bilgileri bir eylemi çağırmak mantıksal uygulamanızdan bu görevi temsil ettiğinde 3270 Bağlayıcısı'nı kullanan meta verileri dönüştürür.

Tasarım aracı meta veri dosyası oluşturduktan sonra azure'da tümleştirme hesabı için bu dosyaya ekleyin. 3270 bağlayıcı eylem eklediğinizde, bu şekilde mantıksal uygulamanız uygulamanızın meta verileri erişebilirsiniz. Bağlayıcı, meta veri dosyası tümleştirme hesabınızdan 3270 ekranlar arasında gezinti işler ve 3270 bağlayıcı eylem parametrelerini dinamik olarak sunan okur. Ardından konak uygulama verileri sağlayabilir ve bağlayıcı, mantıksal uygulamanızı sonuçları döndürür. Bu şekilde, eski uygulamalarınızı Azure, Microsoft ve diğer uygulamaları, hizmetleri ve Azure Logic Apps destekleyen sistemleri ile tümleştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Önerilen: Bir [tümleştirme hizmeti ortamı (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) 

  Bu ortamı oluşturmak ve mantıksal uygulamanızı çalıştırmak için konum olarak seçebilirsiniz. Bir işe mantıksal uygulamanızı Azure sanal ağları içinde korunan kaynaklara erişim sağlar.

* Otomatikleştirme ve ekran temelli 3270 uygulamanızı çalıştırmak için kullanılacak mantıksal uygulama

  IBM 3270 bağlayıcı Tetikleyiciler yok, bu nedenle gibi mantıksal uygulamanızı başlatmak için başka bir tetikleyici kullanın **yinelenme** tetikleyici. Ardından 3270 bağlayıcı eylemler ekleyebilirsiniz. Başlamak için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). 
  Bir işe kullanırsanız, o işe mantıksal uygulamanızın konum olarak seçin.

* [3270 tasarım aracı yükleyip](https://aka.ms/3270-design-tool-download).
Tek önkoşul olan [Microsoft .NET Framework 4.6.1](https://aka.ms/net-framework-download).

  Bu aracı ekleyin ve 3270 bağlayıcı eylemleri çalıştırmak uygulamanıza ekranlar, Gezinti yolu, yöntemleri ve görevler için parametreleri kayıt yardımcı olur. Araç anabilgisayar uygulamanızı yönlendirmek için kullanılacak bağlayıcı için gerekli meta veriler sağlayan bir konak tümleştirme Tasarımcısı XML (HIDX) dosyası oluşturur.
  
  İndirme ve bu aracı yüklemeden sonra ana bilgisayara bağlanmak için şu adımları izleyin:

  1. 3270 tasarım Aracı'nı açın. Gelen **oturumu** menüsünde **konak oturumları**.
  
  1. TN3270 ana sunucu bilgileri sağlar.

* Bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), depoladığınız HIDX dosyanız bir harita olarak mantıksal uygulamanız bu dosyanın meta verileri ve yöntemi tanımları erişebilmesi için bir yerde olduğu. 

  Kullandığınız mantıksal uygulama tümleştirme hesabınıza bağlı olduğundan emin olun. Ayrıca, bir işe kullanırsanız, mantıksal uygulamanızın kullandığı aynı işe tümleştirme hesabının konumudur emin olun.

* Ana bilgisayar uygulamanızı barındıran TN3270 sunucuya erişim

<a name="define-app-metadata"></a>

## <a name="create-metadata-overview"></a>Meta veri genel bakış oluşturma

Meta veri olarak sağlayabilen uygulamanızla ilgili bu bilgileri 3270 bağlayıcı gerekir 3270 ekran temelli bir uygulama, ekranlar ve veri alanlarını senaryolarınız için benzersizdir. Bu meta veriler, mantıksal uygulamanızı tanımlamak ve ekranlar tanımanıza yardımcı olur, ekranlar arasında gezinme açıklar bilgiler açıklanmaktadır giriş verileri nerede ve beklediğiniz sonuçları nerede. Belirtin ve bu meta verileri oluşturmak için belirli bunlar size 3270 tasarım aracı kullanın. *modları*, ya da daha sonra daha fazla ayrıntı açıklandığı aşamaları:

* **Yakalama**: Bu modda, bir banka bakiyesi alma uygulamanızla ana bilgisayar, örneğin, belirli bir görevi tamamlamak için gereken ekranları kaydedin.

* **Gezinti**: Bu modda planla veya nasıl belirli görev için ana bilgisayar uygulamanızın ekranlar arasında gezinmek için yolu belirtin.

* **yöntemleri**: Bu modda, yöntem, örneğin, tanımladığınız `GetBalance`, ekran Gezinti yolu, yani açıklar. Ayrıca, yöntem giriş haline gelir ve parametreleri çıkış alanları her ekranda de seçin.

### <a name="unsupported-elements"></a>Desteklenmeyen öğe

Tasarım aracı, bu öğeleri desteklemez:

* Kısmi IBM temel eşleme desteği (BMS) eşlemeleri: Tasarım aracı BMS harita içe aktarırsanız, kısmi ekran tanımları yok sayar.
* / Out parametreleri: / Out parametreleri tanımlayamazsınız.
* Menü işleme: Önizleme sırasında desteklenmiyor
* Dizi işleme: Önizleme sırasında desteklenmiyor

<a name="capture-screens"></a>

## <a name="capture-screens"></a>Ekran yakalama

Bu modda, bu ekran benzersiz olarak tanımlayan her 3270 ekranında bir öğe işaretleyin. Örneğin, bir metin satırı veya belirli bir metni ve boş alan gibi koşulları daha karmaşık bir dizi belirtebilirsiniz. Ana bilgisayar sunucusu için Canlı bir bağlantı üzerinden bu ekranları kaydedin veya bu bilgileri bir IBM temel eşleme desteği (BMS) eşlemesinden almak. Canlı bağlantı TN3270 öykünücü ana bilgisayara bağlamak için kullanır. Her bağlayıcı eylemi oturumunuza bağlanma ile başlar ve oturumunuzdan kesme ile biten tek bir görevi eşlenmelidir.

1. Henüz yapmadıysanız, 3270 tasarım Aracı'nı açın. Araç çubuğunda **yakalama** böylece yakalama modu girin.

1. Kaydı başlatmak için F5 tuşuna basın veya **kaydı** menüsünde **Başlat kaydı**. 

1. Gelen **oturumu** menüsünde **Connect**.

1. İçinde **yakalama** adım uygulamanızı kaydederken, belirli bir görev için aracılığıyla uygulamanıza ilk ekran başlangıç bölmesi.

1. Görev tamamladıktan sonra normalde yaptığınız gibi uygulamanızdan oturumu kapatın.

1. Gelen **oturumu** menüsünde **Bağlantıyı Kes**.

1. Kaydı durdurmak için Shift + F5 tuşlarına basın anahtarları veya **kaydı** menüsünde **Kaydı Durdur**.

   Bir görev için ekranlar yakaladıktan sonra tasarımcı araç ekranları temsil eden küçük resimleri gösterir. Bu küçük resimleri ile ilgili bazı notlar:

   * Yakalanan filtrelerinizi dahil, "Boş" adlı bir ekran sahip.

     İlk bağlandığınızda [CICS](https://www.ibm.com/it-infrastructure/z/cics), çalıştırmak istediğiniz işlemin adını girmek için önce "Clear" anahtar göndermeniz gerekir. "Clear" anahtar gönderdiğiniz yeri ekran herhangi yok *tanıma öznitelikleri*, ekran tanıma Düzenleyicisi'ni kullanarak ekleyebilirsiniz. bir ekran başlığını gibi. Küçük resimleri bu ekranı temsil etmek için "Boş" adlı bir ekran içerir. Daha sonra bu ekranı Ekran temsil etmek için işlem adını girebileceğiniz kullanabilirsiniz.

   * Varsayılan olarak, yakalanan bir ekran için bir ad ilk sözcük ekranda kullanır. Bu ad zaten varsa, alt çizgi ve bir sayı, örneğin, "WBGB" ve "WBGB_1" adıyla tasarım aracı ekler.

1. Yakalanan bir ekrana daha anlamlı bir ad vermek için bu adımları izleyin:

   1. İçinde **ana ekranları** bölmesinde, yeniden adlandırmak istediğiniz ekranı seçin.

   1. Aynı bölmede ekranın aynı bölmede bulma **ekran adı** özelliği.

   1. Geçerli ekranın adını daha açıklayıcı bir adla değiştirin.

1. Artık her ekran tanımlamak için alanları belirtin.

   3270 veri akışı ile her ekranda benzersiz metni seçmeniz gerekir, böylece ekranları varsayılan tanımlayıcıları yok. Karmaşık senaryolar için birden çok koşulu, örneğin, benzersiz bir metin ve belirli bir koşul ile bir alan belirtebilirsiniz.

Tanıma alanları seçtikten sonra sonraki moda taşıyın.

### <a name="conditions-for-identifying-repeated-screens"></a>Yinelenen ekranlar tanımlamak için koşullar

Bağlayıcısının gidin ve ekranlar arasında ayırt etmek için genellikle benzersiz metin yakalanan ekranlar arasında bir tanımlayıcı olarak kullanabileceğiniz bir ekran üzerinde bulabilirsiniz. Yinelenen ekranlar için daha fazla tanımlama yöntemleri gerekebilir. Örneğin, bir ekran diğer ekran bir hata iletisi döndürür. geçerli bir değer döndürür dışında aynı görünen iki ekran olduğunu varsayalım.

Tasarım Aracı'nda eklediğiniz *tanıma öznitelikleri*, örneğin, ekran tanıma düzenleyicisini kullanarak bir ekran başlığını "Alma hesap bakiyesini" gibi. Çatalı oluşturulan bir yol varsa ve aynı ekranda hem dalları döndürür ancak farklı sonuçlar ile diğer tanıma öznitelikleri gerekir. Bağlayıcı, çalışma zamanında bu öznitelikler güncel dalı ve çatal belirlemek için kullanır. Kullanabileceğiniz koşullar şunlardır:

* Belirli değer: Bu değer, belirtilen konumda belirtilen dizeyle eşleşir.
* Belirli bir değer değil: Bu değer, belirtilen dizenin belirtilen konumda eşleşmiyor.
* Boş: Bu alan boştur.
* Boş değil: Bu alan boş değildir.

Daha fazla bilgi için bkz. [örnek Gezinti planı](#example-plan) bu konuda.

<a name="define-navigation"></a>

## <a name="define-navigation-plans"></a>Gezinti planlarını tanımlayın

Bu modda, akış veya belirli göreviniz için ana bilgisayar uygulamanızın ekranlar arasında gezinme için adımları tanımlayın. Örneğin, bazı durumlarda, başka bir yol, bir hata oluşturur ancak doğru sonucu burada bir yolunu üretir uygulamanızın alabileceği birden fazla yol sahip olabilir. Her ekran için bir sonraki ekrana gibi hareket etmek için gerekli tuş vuruşları belirtin `CICSPROD <enter>`.

> [!TIP]
> Aynı bağlantı kullanan ve ekranlar kesin çeşitli görevleri otomatik hale getirme, tasarım aracı özel bağlanma ve bağlantıyı kesme planı türler sağlar. Bu planlar tanımladığınızda, bunları gezinti planınızın başlangıcını ve bitişini ekleyebilirsiniz.

### <a name="guidelines-for-plan-definitions"></a>Planı tanımları için yönergeler

* Bağlanma ve bağlantıyı kesme ile biten ile başlayan tüm ekranlara içerir.

* Bir dizi ekranlar tüm işlemler için ortak yeniden kullanmanıza olanak sağlayan bağlanma ve bağlantıyı kesme planlarını ya da tek başına bir plan oluşturun.

  * Connect planınız son ekran Gezinti planınızdaki bulunduğu ilk ekrana sayfada olmalıdır.

  * Bağlantı kesme planınızı ilk ekranda aynı ekranda son ekran Gezinti planınızdaki olması gerekir.

* Yakalanan filtrelerinizi çok sayıda yinelenen ekran içerir, böylece seçin ve planınızdaki tüm yinelenen ekranlar yalnızca bir örneğini kullanın. Yinelenen ekranları bazı örnekleri aşağıda verilmiştir:

  * Oturum açma ekranında, örneğin, **MSG 10** ekranı
  * CICS için Hoş Geldiniz ekranı
  * "Clear" veya **boş** ekranı

<a name="create-plans"></a>

### <a name="create-plans"></a>Planları oluşturun

1. 3270 tasarım Aracı'nın araç çubuğunda **Gezinti** böylece gezinti moduna girin.

1. Planınızı başlatmak için **Gezinti** bölmesinde seçin **yeni Plan**.

1. Altında **yeni plana ad seç**, planınız için bir ad girin. Gelen **türü** listesinde, planı türünü seçin:

   | Plan türü | Açıklama |
   |-----------|-------------|
   | **İşlem** | Tek başına veya birleştirilmiş planları için |
   | **Bağlanma** | Connect planları |
   | **Bağlantıyı Kes** | Bağlantı kesme planları |
   |||

1. Gelen **ana ekranları** bölmesinin, gezinti plana yakalanan küçük yüzey içinde Sürükle **Gezinti** bölmesi.

   İşlem adı girebileceğiniz boş ekran temsil etmek için "Boş" Ekran kullanın.

1. Ekranlara açıklayan tanımlamakta görev sırasını düzenleyin.

1. Çatallar ve tasarım Aracı'nın araç çubuğunda birleşimler dahil olmak üzere, ekranlar arasında akış yolu tanımlamak için seçin **akış**.

1. İlk ekrana akışı seçin. Sürükleme ve sonraki ekranda flow'a bağlantı çizin.

1. Her ekran için değer sağlamanız **ÜRETTİĞİ anahtarı** özelliği (dikkat tanımlayıcısı) ve **sabit metin** özelliği akışı sonraki ekrana taşınır.

   Yalnızca ÜRETTİĞİ anahtarı veya Yardım anahtar hem sabit metin olabilir.

Gezinti planınızı bitirdikten sonra [definovat metody sonraki modunda](#define-method).

<a name="example-plan"></a>

### <a name="example"></a>Örnek

Bu örnekte, "adımları sahip WBGB" adlı bir CICS işlem çalıştırırsanız varsayalım: 

* İlk ekranda, bir ad ve bir hesap girin.
* İkinci ekranda hesap bakiyesine sahip olursunuz.
* "Boş" ekrana çıkış.
* CICS "MSG-10" ekrana oturumunu kapatın.

Ayrıca, bu adımları yineleyin, ancak hata gösteren ekran yakalamak için bu nedenle yanlış veri girin varsayalım. Yakaladığınız ekran şunlardır:

* MSG-10
* CICS Hoş Geldiniz
* boş
* WBGB_1 (giriş)
* WBGB_2 (hata)
* Empty_1
* MSG-10_1

Burada birçok ekranları benzersiz adlar alma olsa da, bazı ekranlar aynı ekranda, örneğin, "MSG-10" ve "Boş" dir. Yinelenen bir ekran için yalnızca bir örneği planınızdaki bu ekran için kullanın. Tek başına bir plan, Connect planı, bağlantıyı kes-planı ve birleşik bir plana nasıl görünebileceği gösteren örnekleri aşağıda verilmiştir:

* Tek başına planı

  ![Tek başına Gezinti plan](./media/connectors-create-api-3270/standalone-plan.png)

* Plan bağlanma

  ![Plan bağlanma](./media/connectors-create-api-3270/connect-plan.png)

* Plan bağlantısını kes

  ![Plan bağlantısını kes](./media/connectors-create-api-3270/disconnect-plan.png)

* Birleşik planı

  ![Birleşik planı](./media/connectors-create-api-3270/combined-plan.png)

#### <a name="example-identify-repeated-screens"></a>Örnek: Yinelenen ekranlar tanımlayın

Bağlayıcısının gidin ve ekranlar ayırt etmek için genellikle benzersiz metin yakalanan ekranlar arasında bir tanımlayıcı olarak kullanabileceğiniz bir ekran üzerinde bulabilirsiniz. Yinelenen ekranlar için daha fazla tanımlama yöntemleri gerekebilir. Örnek planı, benzer ekranları edinebileceğiniz çatal sahiptir. Bir ekran diğer ekran bir hata iletisi döndürür. bir hesap bakiyeniz döndürür.

Tasarım aracı tanıma öznitelikleri eklemenize olanak sağlayan, örneğin, bir ekran başlığını "Hesap bakiyesi Al", ekran tanıma kullanarak Düzenleyicisi adlı. Benzer ekranları birlikte bir durumda, diğer öznitelikleri gerekir. Bağlayıcı, çalışma zamanında bu öznitelikler dal ve çatal belirlemek için kullanır.

* Hesap bakiyesine ekranı olan geçerli girişi döndüren dalında "boş değil" bir koşula sahip bir alan ekleyebilirsiniz.

* Bir hata ile döndüren dalında "empty" bir koşul sahip bir alan ekleyebilirsiniz.

<a name="define-method"></a>

## <a name="define-methods"></a>Definovat metody

Bu modda, gezinti planınızla ilişkili yöntemi tanımlayın. Her yöntem parametresi için bir dize, tamsayı, tarih veya saatte gibi veri türünü belirtmeniz ve benzeri. İşiniz bittiğinde, yönteminiz dinamik konak üzerindeki test edebilir ve yöntem beklendiği gibi çalıştığını onaylayın. Ardından meta veri dosyası veya artık yöntemi tanımları oluşturma ve IBM 3270 bağlayıcı için bir eylemi çalıştırmak için kullanılacak olan konak tümleştirme Tasarımcısı XML (HIDX) dosyası oluşturur.

1. 3270 tasarım Aracı'nın araç çubuğunda **yöntemleri** böylece yöntemleri moduna girin. 

1. İçinde **Gezinti** bölmesinde istediğiniz giriş alanları içeren Ekran'ı seçin.

1. Yönteminizi ilk giriş parametresi eklemek için aşağıdaki adımları izleyin:

   1. İçinde **yakalama** 3270 öykünücü ekranında bölmesinde seçin tüm alan, alanın içine değil yalnızca metin ilk giriş olarak istiyor.

      > [!TIP]
      > Üzerinde tam alan seçtiğinizden emin olun ve tüm alanları görüntülemek için **görünümü** menüsünde **tüm alanları**.

   1. Tasarım Aracı'nın araç çubuğunda **giriş alanı**. 

   Daha fazla giriş parametreleri eklemek için her parametre için önceki adımları yineleyin.

1. İlk çıkış parametresi, yöntemin eklemek için aşağıdaki adımları izleyin:

   1. İçinde **yakalama** 3270 öykünücü ekranında bölmesinde seçin tüm alan, alanın içine değil yalnızca metin ilk çıkış istediğiniz.

      > [!TIP]
      > Üzerinde tam alan seçtiğinizden emin olun ve tüm alanları görüntülemek için **görünümü** menüsünde **tüm alanları**.

   1. Tasarım Aracı'nın araç çubuğunda **çıkış alanını**.

   Daha fazla çıktı parametreleri eklemek için her parametre için önceki adımları yineleyin.

1. Bu özellikler için her bir parametre, yöntemin tüm parametreleri ekledikten sonra tanımlayın:

   | Özellik adı | Olası değerler | 
   |---------------|-----------------|
   | **Veri türü** | Uzun, kısa, Byte, tarih saat, ondalık, Int, String |
   | **Alan dolgu yöntemi** | Parametreler gerekiyorsa, boşluk ile doldurarak bu dolgu türleri desteği: <p><p>- **Tür**: Karakter, sıralı olarak alana girin. <p>- **Dolgu**: Gerekirse, boşluk ile doldurma karakterleri içeren adlardan alanın içeriğini değiştirin. <p>- **EraseEofType**: Alan temizleyin ve karakter alana sıralı olarak girin. |
   | **Biçim dizesi** | Bazı parametre veri türlerini nasıl metin ekranından bir .NET veri türüne dönüştürmeye 3270 bağlayıcı bildirir bir biçim dizesi kullanın: <p><p>- **DateTime**: DateTime biçimi dizesi aşağıdaki [.NET özel tarih ve saat biçim dizeleri](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Örneğin, tarih `06/30/2019` biçim dizesini kullanan `MM/dd/yyyy`. <p>- **Ondalık**: Ondalık biçim dizesini kullanan [COBOL resim yan tümcesi](https://www.ibm.com/support/knowledgecenter/SS6SG3_5.2.0/com.ibm.cobol52.ent.doc/PGandLR/ref/rlddepic.html). Örneğin, sayı `100.35` biçim dizesini kullanan `999V99`. |
   |||

## <a name="save-and-view-metadata"></a>Kaydet ve meta verilerini görüntüleyin

Yönteminizi tanımladıktan sonra ancak yönteminizi test etmeden önce şu ana kadar Kaydırmayı Aç/Kapat (.rap) dosyasına tanımlanan tüm bilgileri kaydedin.
Herhangi bir modda sırasında herhangi bir zamanda bu RAP dosyasına kaydedebilirsiniz. Tasarım aracı da açın ve bu konumda tasarım Aracı'nın yükleme klasörüne göz atma ve "WoodgroveBank.rap" dosyayı açmayı gözden bir örnek RAP dosya içerir:

`..\Program Files\Microsoft Host Integration Server - 3270 Design Tool\SDK\WoodgroveBank.rap`

Ancak, örnek RAP dosyası veya dosya tasarım Aracı'nın yükleme klasöründe kalarak HIDX dosya örnek RAP dosyasından oluşturma değişiklikleri kaydetmeyi denerseniz, "erişim reddedildi" hatası alabilirsiniz. Varsayılan olarak, yükseltilmiş izinler olmadan Program dosyaları klasöründeki tasarım aracı yüklenir. Bir hata alırsanız, bu çözümlerden birini deneyin:

* Örnek dosyayı farklı bir konuma kopyalayın.
* Tasarım aracını bir yönetici olarak çalıştırın.
* Kendiniz SDK klasör sahibi olun.

## <a name="test-your-method"></a>Test, yöntemi

1. Yönteminizi hala yöntemleri modunda çalışırken dinamik ana bilgisayar karşı çalıştırmak için F5 tuşuna basın veya tasarım Aracı'nın araç çubuğundan **çalıştırma**.

   > [!TIP]
   > Herhangi bir zamanda modları değiştirebilirsiniz. Üzerinde **dosya** menüsünde **modu**ve ardından istediğiniz modu seçin.

1. Parametreleri değerleri girin ve seçin **Tamam**.

1. Sonraki ekrana devam etmeyi tercih **sonraki**.

1. İşlemi tamamladığınızda, seçin **Bitti**, çıkış parametresi değerleri gösterir.

<a name="add-metadata-integration-account"></a>

## <a name="generate-and-upload-hidx-file"></a>Oluşturma ve HIDX dosyasını karşıya yükleyin

Hazır olduğunuzda, tümleştirme hesabınıza yükleyebilirsiniz. böylece HIDX dosyası oluşturur. 3270 tasarım aracı RAP dosyanızı nereye kaydettiğiniz yeni bir alt klasöründe HIDX dosyası oluşturur.

1. 3270 tasarım Aracı'nın araç çubuğunda **kod üret**.

1. RAP dosyanızı içeren klasöre gidin ve araç HIDX dosyanızı oluşturduktan sonra oluşturulan alt klasörü açın. Araç HIDX dosya oluşturulan onaylayın.

1. Oturum [Azure portalında](https://portal.azure.com)ve, tümleştirme hesabı bulunamadı.

1. Tümleştirme hesabınıza bir harita olarak HIDX dosyanıza ekleyin [eşlemeleri eklemek için benzer adımları](../logic-apps/logic-apps-enterprise-integration-liquid-transform.md), ancak eşleme türü seçtiğinizde **HIDX**.

Mantıksal uygulamanızı ilk kez, IBM 3270 eylem eklediğinizde, bu konunun ilerleyen bölümlerinde, mantıksal uygulamanız ve ana bilgisayar sunucusu arasında bir bağlantı gibi adları tümleştirme hesabı ve ana bilgisayar sunucusu için bağlantı bilgilerini sağlayarak oluşturmanız istenir . Bağlantıyı oluşturduktan sonra daha önce eklenen HIDX dosyanızı, çalıştırılacak yöntemi ve kullanılacak parametreleri seçebilirsiniz.

Tüm adımları tamamladığınızda, uygulamanız için sürücü ekranlar, IBM ana bilgisayar kurmak için mantıksal uygulamanızı oluşturma eylemini kullanma, verileri girin, sonuçlar döndürebilir ve benzeri. Ayrıca, mantıksal uygulamanız için diğer uygulamaları, hizmetleri ve sistemleri tümleştirmek için diğer eylemler ekleme devam edebilirsiniz.

<a name="run-action"></a>

## <a name="run-ibm-3270-actions"></a>IBM 3270 eylemleri çalıştırma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Son adım, bir eylem eklemek istediğiniz altında seçin **yeni adım**seçip **Eylem Ekle**. 

1. Arama kutusunun altındaki seçin **Kurumsal**. Arama kutusuna filtreniz olarak "3270" girin. Eylem listesinden şu eylemi seçin: **TN3270 bağlantısı üzerinden bir ana bilgisayar programı çalıştırır**

   ![3270 eylemini seçin](./media/connectors-create-api-3270/select-3270-action.png)

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. 
   Artı işaretini seçin ( **+** ), görünür ve ardından **Eylem Ekle**.

1. Bağlantı henüz yok, bağlantınız için gerekli bilgileri sağlayın ve seçin **Oluştur**.

   | Özellik | Gereklidir | Value | Açıklama |
   |----------|----------|-------|-------------|
   | **Bağlantı Adı** | Evet | <*Bağlantı adı*> | Bağlantınız için bir ad |
   | **Tümleştirme hesabı kimliği** | Evet | <*Tümleştirme hesabı adı*> | Tümleştirme hesabının adı |
   | **Tümleştirme hesabı SAS URL'si** | Evet | <*Tümleştirme hesabı SAS URL'si*> | Tümleştirme hesabının ayarlarından Azure portalında oluşturabileceğiniz tümleştirme hesabınızın paylaşılan erişim imzası (SAS) URL'si. <p>1. Üzerinde tümleştirme hesabı menünüzde **ayarları**seçin **geri çağırma URL'si**. <br>2. Sağ bölmede, kopyalamak **oluşturulan geri çağırma URL'si** değeri. |
   | **Sunucu** | Evet | <*TN3270 sunucu adı*> | TN3270 hizmetiniz için sunucu adı |
   | **Bağlantı Noktası** | Hayır | <*TN3270-server-port*> | TN3270 sunucunuz tarafından kullanılan bağlantı noktası. Boş bırakılırsa, bağlayıcıyı kullanan `23` varsayılan değer olarak. |
   | **Cihaz türü** | Hayır | <*IBM terminal modeli*> | Model adı veya IBM terminal öykünmek için numarası. Boş bırakılırsa, bağlayıcı varsayılan değerleri kullanır. |
   | **Kod sayfası** | Hayır | <*kod sayfası numarası*> | Konak için kod sayfası numarası. Boş bırakılırsa, bağlayıcıyı kullanan `37` varsayılan değer olarak. |
   | **Mantıksal birim adı** | Hayır | <*Birim adı mantıksal*> | Konaktan istemek için belirli bir mantıksal birim adı |
   | **SSL etkinleştirilsin mi?** | Hayır | Veya kapat | Etkinleştirmek veya devre dışı SSL şifrelemesi kapatabilirsiniz. |
   | **Ana ssl sertifikasını doğrulamak?** | Hayır | Veya kapat | Açma veya sunucu sertifikası doğrulamasını devre dışı bırakın. |
   ||||

   Örneğin:

   ![Bağlantı özellikleri](./media/connectors-create-api-3270/connection-properties.png)

1. Eylem gerekli bilgileri sağlayın:

   | Özellik | Gereklidir | Value | Açıklama |
   |----------|----------|-------|-------------|
   | **Hıdx adı** | Evet | <*HIDX dosya adı*> | Kullanmak istediğiniz 3270 HIDX dosyayı seçin. |
   | **Yöntem adı** | Evet | <*Yöntem adı*> | HIDX dosyanın kullanmak istediğiniz yöntemi seçin. Bir yöntem seçtikten sonra **yeni parametre Ekle** ile bu yöntemi kullanmak üzere parametreleri seçebilmeniz listesi görüntülenir. |
   ||||

   Örneğin:

   **HIDX dosyasını seçin**

   ![HIDX dosyasını seçin](./media/connectors-create-api-3270/select-hidx-file.png)

   **Yöntemi seçin**

   ![Yöntemini seçin](./media/connectors-create-api-3270/select-method.png)

   **Parametreleri seçin**

   ![Parametreleri seçin](./media/connectors-create-api-3270/add-parameters.png)

1. İşiniz bittiğinde, kaydedin ve mantıksal uygulamanızı çalıştırın.

   Sonra mantıksal uygulama çalışıyorsa, çalışma görünmesi yer alan adımları tamamlanır. 
   Başarısız adımları "X" harfinin gösterirken başarılı adımlar onay işaretlerini gösterir.

1. Giriş ve çıkışları her adımı gözden geçirmek için bu adımı genişletin.

1. Çıkışları gözden geçirmek için seçin **bkz ham çıkışları**.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırı hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, gözden geçirme [bağlayıcının başvuru sayfası](/connectors/si3270/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
