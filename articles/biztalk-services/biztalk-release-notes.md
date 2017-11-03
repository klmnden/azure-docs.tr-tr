---
title: "Azure BizTalk Services için sürüm notları | Microsoft Docs"
description: "Azure BizTalk Services için bilinen sorunları listeler"
services: biztalk-services
documentationcenter: 
author: msftman
manager: erikre
editor: 
ms.assetid: f4906fdc-4cd9-4a57-a007-a88c2e51a18f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: 18ed891a9bba2b4011d3492722a2366d96fb3c01
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="release-notes-for-azure-biztalk-services"></a>Azure BizTalk Services için sürüm notları

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Microsoft Azure BizTalk Services için sürüm notları bu sürümdeki bilinen sorunları içerir.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>BizTalk Services Kasım güncelleştirmede yenilikler nelerdir?
* Bekleyen şifreleme BizTalk Services Portalı'nda etkinleştirilebilir. Bkz: [etkinleştirmek BizTalk Services Portalı'nda bekleyen şifreleme](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Güncelleştirme geçmişini
### <a name="october-update"></a>Ekim güncelleştirme
* Kurumsal hesaplar desteklenir:  
  * **Senaryo**: bir Microsoft hesabı kullanarak bir BizTalk hizmeti dağıtımı kayıtlı (gibi user@live.com). Bu senaryoda, BizTalk Services Portalı'nı kullanarak BizTalk hizmeti yalnızca Microsoft Account kullanıcıları yönetebilirsiniz. Bir kurumsal hesap kullanılamaz.  
  * **Senaryo**: Azure Active Directory'de bir kurumsal hesap kullanarak bir BizTalk hizmeti dağıtımı kayıtlı (gibi user@fabrikam.com veya user@contoso.com). Bu senaryoda, yalnızca Azure Active Directory kullanıcıları aynı kuruluş içindeki BizTalk Services Portalı'nı kullanarak BizTalk hizmeti yönetebilirsiniz. Bir Microsoft hesabı kullanılamaz.  
* Klasik Azure portalında bir BizTalk hizmeti oluşturduğunuzda, BizTalk Services Portalı'nda otomatik olarak kaydedilir.
  * **Senaryo**: Klasik Azure portalında oturum BizTalk hizmeti oluşturma ve ardından **Yönet** çok ilk kez. BizTalk Services portalı açıldığında, BizTalk hizmeti otomatik olarak kaydeder ve dağıtımları için hazırdır.  
    Bkz: [kaydetme ve BizTalk hizmeti dağıtımını BizTalk Güncelleştirme Hizmetleri portalı](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>14 Ağustos güncelleştirme
* Şimdi sözleşmesi ve ayırma – ticari ortak sözleşmeleri ve köprüleri köprüsü BizTalk Services Portalı'nda birbirinden ayrılır. Şimdi anlaşmalar ve köprüleri ayrı ayrı oluşturduğunuzda ve çalışma zamanında köprüleri EDI ileti değerlere dayalı olarak bir anlaşması çözümlenemiyor. Bkz: [Azure BizTalk Services oluşturma sözleşmelerde](https://msdn.microsoft.com/library/azure/hh689908.aspx), [BizTalk Services Portalı'nı kullanarak bir EDI köprüsü oluşturma](https://msdn.microsoft.com/library/azure/dn793986.aspx), [BizTalk Services Portalı'nı kullanarak bir AS2 köprüsü oluşturma](https://msdn.microsoft.com/library/azure/dn793993.aspx), ve [nasıl köprüleri çözümlemek çalışma zamanında anlaşmaları?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* Anlaşmaları için şablonlar oluşturma seçeneğini kesilir.  
* Gönderme tarafı anlaşma için farklı sınırlayıcı kümeleri her şema için artık belirtebilirsiniz. Bu yapılandırma, gönderme tarafı anlaşması Protokolü Ayarları altında belirtilir. Daha fazla bilgi için bkz: [oluşturma X12 bir Azure BizTalk Services anlaşmasında](https://msdn.microsoft.com/library/azure/hh689847.aspx) ve [Azure BizTalk Services ' bir EDIFACT sözleşmesi oluşturun](https://msdn.microsoft.com/library/azure/dn606267.aspx). İki yeni varlıklar, aynı amaçla TPM OM API'sine de eklenir. Bkz: [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) ve [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Türetilmiş türler dahil olmak üzere standart XSD yapıları artık desteklenmektedir. Bkz: [kullanım standart XSD oluşturur, maps](https://msdn.microsoft.com/library/azure/dn793987.aspx) ve [kullanım türetilmiş türlerini eşleme senaryoları ve örnekler](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 ileti imzalama için yeni MIC algoritmalarını ve yeni şifreleme algoritmaları destekler. Bkz: [AS2 sözleşmesi Azure BizTalk Services oluşturma](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
  ## <a name="know-issues"></a>Bilinen sorunlar

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>BizTalk Services Portal güncelleştirmesi sonra bağlantı sorunları
  BizTalk Services değişiklikleri hizmete geri dönmek için BizTalk Services yükseltilirken açmak portalı varsa, BizTalk Services portalı ile ilgili bağlantı sorunlarını karşılaşıyor.  
  Geçici bir çözüm olarak, tarayıcıyı yeniden, tarayıcı önbelleğine silin veya özel modda Portalı'nı başlatın.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Bir hata veya uyarı BizTalk Services projesinde tıklatırsanız visual Studio IDE yapıyı bulunamıyor
Visual Studio 2012 güncelleştirme 3 RC sorunu düzeltmek için 1 yükleyin.  

### <a name="custom-binding-project-reference"></a>Özel bağlama proje başvurusu
BizTalk Services proje bir Visual Studio çözümüne ile aşağıdaki durumlar göz önünde bulundurun:  

* Aynı Visual Studio çözümünde BizTalk Services projesi ve özel bağlama proje yok. BizTalk hizmeti projesini bu özel bağlama proje dosyası bir başvuru içeriyor.
* BizTalk hizmeti projesini özel bağlama/davranış DLL bir başvuru içeriyor.

'Visual Studio'da Çözüm başarıyla yapı'. Ardından, 'Yeniden' veya 'çözüm temiz'. Yeniden oluşturmanız veya yeniden temiz bundan sonra aşağıdaki hata oluşur:  
  Dosya kopyalanamıyor <Path to DLL> "bin\Debug\FileName.dll" için. Başka bir işlem tarafından kullanıldığından işlem 'bin\Debug\FileName.dll' dosyasına erişemiyor.  

#### <a name="workaround"></a>Geçici çözüm
* Varsa [Visual Studio 2012 güncelleştirme 3'ü](https://www.microsoft.com/download/details.aspx?id=39305) olan yüklü, aşağıdaki iki seçeneğiniz vardır:
  
  * Visual Studio'yu yeniden başlatın veya
  * Çözümü yeniden başlatın. Sonra yalnızca bir yapı çözümü gerçekleştirin.  
* Varsa [Visual Studio 2012 güncelleştirme 3'ü](https://www.microsoft.com/download/details.aspx?id=39305) yüklenen, açık Görev Yöneticisi, İşlemler sekmesinde MSBuild.exe işlemi'ı tıklatın ve işlemi Sonlandır düğmesini tıklatın.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Yazdırılamayan karakterler HTTP üst bilgileri olarak yükselttiyseniz BasicHttpRelay Uç noktalara yönlendirme köprüleri ve BizTalk Services portalı desteklenmiyor
İletiler için yükseltilen özelliklerinin bir parçası yazdırılamayan karakterleri kullanırsanız, bu iletiler BasicHttpRelay bağlamayı kullanan geçiş hedeflere yönlendirilemez. İzleme bir parçası olarak BLOB'lar için URL olarak kodlanmış ve beklemediğiniz kodlanmış hedefler için de, yükseltilen özellikleri, kullanılabilir.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>Gönderme zaman uyumsuz MDN seçeneği işaretli olsa bile MDN zaman uyumsuz olarak gönderilir
Seçerseniz bu senaryoyu – göz önünde bulundurun **gönderme zaman uyumsuz MDN** onay kutusunu ve zaman uyumsuz MDN göndermek için bir URL belirtin ve ardından işaretini **gönderme zaman uyumsuz MDN** onay kutusunu yeniden MDN hala gönderilir belirtilen URL'ye zaman uyumsuz MDNs gönderme seçeneği seçilmemiş olsa da.  
Geçici bir çözüm olarak, belirtilen URL işaretleyerek önce temizlemeniz gerekir **gönderme zaman uyumsuz MDN** onay kutusunu ve ardından AS2 sözleşmesi dağıtın.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Geçerli bir değişim ötesinde boşluk karakterleri askıya alma uç noktasına gönderilecek boş bir iletiyi neden
Bir IEA kesimi ötesinde boşluklar varsa, ayrıştırıcı Bu geçerli değişim ucu olarak değerlendirir ve bir sonraki iletiyi olarak boşluk sonraki kümesini bakar. Bu, geçerli değişim olmadığından, başarılı bir ileti için rota hedef gönderilir ve boş bir ileti askıya alma endpoint gönderilir görebilirsiniz.  

### <a name="tracking-in-biztalk-services-portal"></a>BizTalk Services Portalı'nda izleme
İzleme olaylarını EDI ileti işleme ve hiçbir bağıntı kadar yakalanır. Bir ileti dışında Protokolü aşama başarısız olursa izleme başarılı olarak gösterilir. Bu durumda, günlük bölümünde bakın **ayrıntıları** sütununda **izleme** hata ayrıntıları.
X12 ayarları gönderip ([oluşturma X12 bir Azure BizTalk Services anlaşmasında](https://msdn.microsoft.com/library/azure/hh689847.aspx)) protokolü Sahne bilgileri sağlayın.  

### <a name="update-agreement"></a>Güncelleştirme anlaşması
BizTalk Services portalı anlaşmanın yapılandırıldığında, bir kimliği niteleyicisi değiştirmenizi sağlar. Bu, tutarsız özelliklerinde neden olabilir. Örneğin, ZZ:1234567 ve ZZ:7654321 niteleyici kullanarak bir anlaşma var. BizTalk Hizmetleri portalı profil ayarları ZZ:1234567 01:ChangedValue olacak şekilde değiştirin. Sözleşmesi'ni açın ve 01:ChangedValue ZZ:1234567 yerine görüntülenir.
Bir kimliği niteleyicisi değiştirmek, anlaşmayı silmek için güncelleştirme **kimlikleri** ortağı profili ve anlaşmayı yeniden oluşturun.  

> AZURE. Bu davranış uyarı X12 ve AS2 etkiler.  
> 
> 

### <a name="as2-attachments"></a>AS2 ekler
İletileri desteklenmez AS2 eklerini göndermek veya almak. Özellikle, ekleri sessizce göz ardı edilir ve ileti gövdesini normal AS2 ileti olarak işlenir.  

### <a name="resources-remembering-path"></a>Kaynaklar: Yolu anımsama
Eklerken **kaynakları**, iletişim kutusu penceresi kaynak eklemek için daha önce kullanılan yolu unutmayın değil. Daha önce kullanılan yolu anımsaması BizTalk Services portalı web sitesine eklemeyi deneyin **Güvenilen siteler** Internet Explorer'da.  

### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Varlık köprüsü yeniden adlandırın ve projeyi değişiklikleri kaydetmeden kapatın, varlık tekrar açmayı hatayla sonuçlanır
Aşağıdaki sırayla bir senaryoyu düşünün:  

* BizTalk hizmeti projeye Köprüsü (örneğin bir XML One-Way köprüsü) ekleme  
* Köprü varlık adı özelliği için bir değer belirterek yeniden adlandırın. Bu, belirttiğiniz adla ilişkili .bridgeconfig dosyayı yeniden adlandırır.  
* .Bcs dosyasını (Visual Studio sekmede kapatarak) değişiklikleri kaydetmeden kapatın.  
* .Bcs dosyasını yeniden Çözüm Gezgini'nden açın.  
  İlişkili .bridgeconfig dosyanın belirtilen yeni ad sahipken, varlık adı tasarım yüzeyine hala eski adı olduğunu fark edeceksiniz. Köprü yapılandırması köprüsü bileşen çift tıklayarak dosyayı açmaya çalışırsanız aşağıdaki hatayı alıyorsunuz:  
  `‘<old name>’ Entity’s associated file ‘<old name>.bridgeconfig’ does not exist`Bu senaryo içine çalışmasını önlemek için BizTalk hizmeti projesini varlıklarda yeniden adlandırdıktan sonra değişiklikleri kaydetmek emin olun.  
  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>Bir yapı bir Visual Studio Proje dışlanan olsa bile BizTalk hizmeti projesini başarıyla derlemeler
Visual Studio projesinden hariç tutmak ve (örneğin, bir istek iletisi türü belirterek) köprü yapılandırmasında, yapı içerir veya bir BizTalk hizmeti projesine bir yapı (örneğin, bir XSD dosyası) eklediğiniz bir senaryoyu düşünün. Silinen yapı konumundaki burada Visual Studio projeye eklendi aynı diskteki kullanılabilir olduğu sürece böyle bir durumda projeyi oluşturmayı herhangi bir hata veremezsiniz.
  
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>BizTalk hizmeti projesi için şema kullanılabilirlik köprüleri yapılandırılırken denetlemez
Projeye eklenen bir şema başka bir şema alıyorsa bir BizTalk hizmeti projesini BizTalk hizmeti projesini alınan şema projeye eklenmiş olup olmadığını denetlemez. Bu tür bir projeyi derleme çalışırsanız, yapı hataları almamış.
  
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>Yanıt için bir XML istek-yanıt köprüsü her zaman charset UTF-8 ' iletisidir
Bu sürüm için bir XML istek-yanıt köprüsü yanıt iletisinden charset her zaman UTF-8'e ayarlanır.
  
### <a name="user-defined-datatypes"></a>Kullanıcı tanımlı veri türleri
BizTalk bağdaştırıcı hizmeti özellik BizTalk bağdaştırıcı paketi bağdaştırıcılara kullanıcı tanımlı veri türleri bağdaştırıcısı işlemleri için kullanabilir.
Kullanıcı tanımlı veri türleri kullanırken, sürücü: \Program BizTalk Bağdaştırıcısı Service\BAServiceRuntime\bin\ veya için Genel Derleme Önbelleği (GAC) BizTalk bağdaştırıcı hizmeti hizmetini barındıran sunucuda dosyaları (.dll) kopyalayın. Aksi takdirde, istemcide aşağıdaki hata oluşabilir:  
```
<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
<faultcode>s:Client</faultcode>
<faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
<detail>
  <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
  </AFConnectRuntimeFault>
</detail>
</s:Fault>
```  
  
> [!IMPORTANT]
> Genel derleme önbelleğine dosya yüklemek için GACUtil.exe kullanılması önerilir. Bu araç ve Visual Studio komut satırı seçeneklerini kullanma GACUtil.exe belgeler.  
> 
> 

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>BizTalk bağdaştırıcı hizmeti Web sitesi yeniden başlatılıyor
Yükleme **BizTalk bağdaştırıcı hizmeti çalışma zamanı*** oluşturur **BizTalk bağdaştırıcı hizmeti** içeren IIS web sitesinde **BAService** uygulama. **BAService** uygulama dahili olarak kullanan geçiş bağlama bulut şirket içi hizmet uç noktasına ulaşabileceği genişletmek için. Yalnızca şirket içi hizmeti başladığında bir barındırılan hizmet şirket için hizmet veri yolu üzerinde karşılık gelen geçiş uç noktası kaydedilir.  

Durdur ve uygulama başlatma, bir uygulamayı otomatik olarak başlatmak için yapılandırma dikkate alınır değil. Böyle olduğunda **BAService** olan durduruldu, her zaman yeniden başlatmanız gerekir **BizTalk bağdaştırıcı hizmeti** yerine web sitesi. Başlat Durdur veya **BAService** uygulama.

### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Özel karakterler LOB bileşenleri adresi ve varlık adları için kullanılmamalıdır
LOB bileşenleri adresi ve varlık adları için özel karakterler kullanmamalısınız. Bunu yaparsanız, BizTalk hizmeti projesini dağıtma sırasında bir hata alırsınız. Belirli '%' gibi karakterler, BizTalk bağdaştırıcı hizmeti Web sitesi durdurulmuş bir duruma geçebilir ve el ile başlatılması gerekir.

### <a name="test-map-with-get-context-property"></a>Get içerik özelliği test Haritası
Bir dönüştürme içeriyorsa bir **alma içerik özelliği** eşleme işlemi **Test eşlemesi** başarısız olur. Geçici bir çözüm olarak değiştirmek **alma içerik özelliği** sahte verileri içeren dize birleştirme Map işlemi ile eşleme işlemi. Bu, hedef şemanın doldurmak ve diğer dönüştürme işlevselliğini sınamak izin.

### <a name="test-map-property-does-not-display"></a>Test eşleme özelliğini görüntülenmez
**Test eşlemesi** özellikleri Visual Studio'da görüntülemez. Bu durum ortaya çıkabilir **özellikleri** penceresi ve **Çözüm Gezgini** penceresi aynı anda yerleştirilmemişse. Bu sorunu çözmek için yerleştirme **özellikleri** ve **Çözüm Gezgini** windows.  

### <a name="datetime-reformat-drop-down-is-grayed-out"></a>DateTime yeniden biçimlendirin açılan gri
Bir DateTime yeniden biçimlendirin eşleme işlemi tasarım yüzeyine eklenir ve yapılandırılmış zaman biçimi aşağı açılan listesinde gri. Bilgisayarın görüntü ayarlarsanız bu durum oluşabilir **Orta – %125** veya **daha büyük – % 150**. Görüntü çözmek için kümesine **daha küçük – % 100 (varsayılan)** aşağıdaki adımları kullanarak:  

1. Açık **Denetim Masası** tıklatıp **Görünüm ve kişiselleştirme**.
2. Tıklatın **görüntü**.
3. Tıklatın **daha küçük – % 100 (varsayılan)** tıklatıp **Uygula**.

**Biçimi** aşağı açılan liste beklendiği gibi şimdi çalışmalıdır.

### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>BizTalk Services Portalı'nda yinelenen sözleşmeleri
Aşağıdaki senaryoyu göz önünde bulundurun:

1. Ticari ortak Yönetimi OM API kullanarak bir anlaşma oluşturun.
2. BizTalk Hizmetleri portalında iki farklı sekmelerdeki Sözleşmesi'ni açın.
3. Her iki sekmelerindeki sözleşmesi dağıtın.
4. Her iki anlaşmaları BizTalk Services Portalı'nda yinelenen girişler kaynaklanan sonuç olarak, dağıtılan

**Geçici çözüm**. BizTalk Services Portalı'nda yinelenen anlaşmaları herhangi birini açın ve dağıtımı geri.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Bir sertifika yapıt deposunda bile güncelleştirildikten sonra köprüleri güncelleştirilmiş sertifika kullanmayın
Aşağıdaki senaryoları göz önünde bulundurun:  

**Senaryo 1: hizmet uç noktası bir köprü ileti aktarımı güvenliğini sağlamak için sertifika parmak izi tabanlı kullanma**  
Parmak izi tabanlı sertifikalar, BizTalk hizmeti projenizde kullandığınız bir senaryo düşünün. Aynı adlı ancak farklı bir parmak izi ile BizTalk Services Portalı'nda bir sertifikayı güncelleştirmek, ancak BizTalk hizmeti projesini uygun şekilde güncelleştirmez. Böyle bir senaryoda köprüsü eski sertifika verileri kanal önbelleğinde olabileceğinden iletileri işlemeye devam edebilir. Bundan sonra ileti işleme başarısız olur.  

**Geçici çözüm**: BizTalk hizmeti projesindeki bir sertifikayı güncelleştirmek ve projeyi yeniden dağıtın.  

**Senaryo 2: hizmet uç noktası bir köprü ileti aktarımı güvenliğini sağlamak için sertifikaları tanımlamak için ada dayalı davranışları kullanarak**

Ad tabanlı davranışları sertifikaları BizTalk hizmeti projenizi tanımlamak için kullandığınız bir senaryo düşünün. BizTalk Services Portalı'nda bir sertifikayı güncelleştirmek ancak BizTalk hizmeti projesini uygun şekilde güncelleştirmez. Böyle bir senaryoda köprüsü eski sertifika verileri kanal önbelleğinde olabileceğinden iletileri işlemeye devam edebilir. Bundan sonra ileti işleme başarısız olur.  

**Geçici çözüm**: BizTalk hizmeti projesindeki bir sertifikayı güncelleştirmek ve projeyi yeniden dağıtın.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>SQL veritabanı çevrimdışı olduğunda bile iletileri işlemek köprüleri devam
(Dağıtılan yapıları ve ardışık düzen gibi çalışan bilgileri depolayan) Microsoft Azure SQL Database, olsa bile çevrimdışı BizTalk Services köprüleri iletileri bir süredir işlemeye devam eder. BizTalk Services Köprü yapılandırması ve önbelleğe alınmış yapıları kullandığından budur.
SQL veritabanı çevrimdışı olduğunda tüm iletileri işlemek için köprüleri istemiyorsanız, durdurabilir veya BizTalk hizmetini askıya almak için BizTalk Services PowerShell cmdlet'lerini kullanabilirsiniz. Bkz: [Azure BizTalk Hizmeti Yönetimi örnek](http://go.microsoft.com/fwlink/p/?LinkID=329019) işlemlerini yönetmek Windows PowerShell cmdlet'leri.  

### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Fazladan bir ürün reçetesi karakter içeren bir köprünün özel kod bileşeni içinde XML ileti okuma
Bir köprünün özel kod içinde bir XML ileti okumak istediğiniz bir senaryo düşünün. .NET API System.Text.Encoding.UTF8.GetString(bytes) kullanıyorsanız ek bir ürün reçetesi karakter iletisinin başlangıcı çıktıyı dahil edilir. Ek ürün reçetesi karakterini eklediğinizden çıkış istemiyorsanız, bu nedenle, kullanmalısınız ```System.IO.StreamReader().ReadToEnd()```.

### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>WCF kullanarak bir köprü iletileri gönderme ölçeklenmez
WCF kullanarak bir köprü gönderilen iletilerin ölçekli değildir. Ölçeklenebilir bir istemcinin istiyorsanız bunun yerine HttpWebRequest kullanmanız gerekir.

### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>Yükseltme: belirteç sağlayıcı hatası BizTalk Services Önizlemesi'nden Genel kullanılabilirlik (GA için) yükselttikten
Etkin toplu işleri bir EDI veya AS2 sözleşmesi yoktur. BizTalk hizmeti için GA Önizlemesi'nden yükselttiğinizde, aşağıdaki ortaya çıkabilir:

* Hata: Belirteç sağlayıcı güvenlik belirteci sağlayamadı. İleti döndürülen belirteç sağlayıcısı: uzak ad çözümlenemedi.
* Toplu görevler iptal edilir.

**Geçici çözüm**: sonra BizTalk hizmeti, genel kullanılabilirlik (GA için) güncelleştirilir, anlaşmayı yeniden dağıtın.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>Yükseltme: Araç kutusu BizTalk Services SDK'sı yükselttikten sonra eski köprüsü simgeleri gösterir
Eski simge köprüleri temsil eden vardı, BizTalk Services SDK'sı önceki bir sürümünü yükselttikten sonra araç köprüleri için eski simgeleri göstermek devam eder. Ancak, BizTalk hizmeti proje Tasarımcı yüzeyine bir köprü eklerseniz, surface yeni simgeyi gösterir.  

**Geçici çözüm**. Altında .tbd dosyaları silerek bu sorunu çözmek çalışabilirsiniz <system drive>: \Users\<kullanıcı > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>Yükseltme: BizTalk Portal güncelleştirmeye Preview sürümünden GA EDI yetenek kullanılamadığını belirten bir hata gösterebilir.
BizTalk Services için GA Önizlemesi'nden yükseltilirken BizTalk Hizmetleri portalında oturum açtıysanız, portalda şu hatayı alabilirsiniz:  

Bu özellik kullanılamıyor Microsoft Azure BizTalk Services'ın bu sürümünde bir parçası olarak. Kullanmak için uygun bir sürümü bu yetenekleri geçin.  

**Çözümleme**: portal, Kapat ve tarayıcıyı açın ve sonra portal günlüğüne oturumu kapatın.  

### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>Yükseltme: BizTalk Services GA olarak yükseltildikten sonra yeni izleme verilerini görünmüyor
BizTalk Hizmetleri Önizleme abonelikte dağıtılmış bir XML köprüsü sahip olduğu bir senaryoyu ele alalım. Köprüsüne iletileri gönderir ve izleme verilerini karşılık gelen BizTalk Hizmetleri portalında kullanılabilir. Şimdi, BizTalk Services portalı ve BizTalk Services çalışma zamanı BITS GA olarak yükseltilir ve daha önce dağıttığınız aynı köprüsü uç noktası için bir ileti göndermek istiyorsanız, izleme verilerini yükseltmeden sonra gönderilen iletileri gösterilmez.  

### <a name="pipelines-versus-bridges"></a>Ardışık Düzen köprüleri karşılaştırması
Bu belge boyunca 'köprüleri' ve 'ardışık düzen' terimi birbirlerinin yerine kullanılır. Her ikisi de temelde, BizTalk Services üzerinde dağıtılan bir ileti işleme birimidir aynı anlama gelir.  

### <a name="concepts"></a>Kavramlar
[BizTalk Hizmetleri](https://msdn.microsoft.com/library/azure/hh689864.aspx)   

