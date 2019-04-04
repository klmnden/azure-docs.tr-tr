---
title: Azure BizTalk Hizmetleri için sürüm notları | Microsoft Docs
description: Azure BizTalk Hizmetleri için bilinen sorunları listeler
services: biztalk-services
documentationcenter: ''
author: msftman
manager: erikre
editor: ''
ms.assetid: f4906fdc-4cd9-4a57-a007-a88c2e51a18f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: 92fc8e9edfc745ae89c2b4d44e193566292d4f08
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918884"
---
# <a name="release-notes-for-azure-biztalk-services"></a>Azure BizTalk Hizmetleri için sürüm notları

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]
> 
> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

Microsoft Azure BizTalk Hizmetleri için sürüm notları, bu sürümdeki bilinen sorunları içerir.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>BizTalk Services'ın Kasım güncelleştirmesindeki yenilikler
* Bekleme sırasında şifreleme BizTalk Hizmetleri portalında etkinleştirilebilir. Bkz: [etkinleştirme BizTalk Hizmetleri portalında bekleyen şifreleme](/previous-versions/azure/dn874052(v=azure.100)).

## <a name="update-history"></a>Güncelleştirme geçmişi
### <a name="october-update"></a>Ekim güncelleştirme
* Kuruluş hesapları desteklenmektedir:  
  * **Senaryo**: Bir Microsoft hesabı kullanarak BizTalk hizmeti dağıtım kayıtlı (gibi user@live.com). Bu senaryoda, BizTalk Hizmetleri portalını kullanarak BizTalk hizmeti yalnızca Microsoft Account kullanıcıları yönetebilir. Bir kuruluş hesabı kullanılamaz.  
  * **Senaryo**: Azure Active Directory'de bir kurumsal hesap kullanarak BizTalk hizmeti dağıtım kayıtlı (gibi user@fabrikam.com veya user@contoso.com). Bu senaryoda, yalnızca Azure Active Directory kullanıcıları aynı kuruluş içindeki BizTalk Hizmetleri portalını kullanarak BizTalk hizmeti yönetebilir. Bir Microsoft hesabı kullanılamaz.  
* BizTalk Hizmetleri Portalı'nda, BizTalk hizmeti oluşturduğunuzda, otomatik olarak kaydedilir.
  * **Senaryo**: BizTalk hizmeti oluşturma, Azure'da oturum açın ve ardından **Yönet** ilk kez. BizTalk Hizmetleri portalında oturum açtığında, BizTalk hizmeti otomatik olarak kaydeder ve dağıtımlarınız için hazırdır.  
    Bkz: [kaydetme ve BizTalk bir BizTalk hizmeti dağıtımını Güncelleştirme Hizmetleri portalı](/previous-versions/azure/hh689837(v=azure.100)).  

### <a name="august-14-update"></a>14 Ağustos güncelleştirme
* Sözleşme ve ayırma-ticari ortak sözleşmeleri ve köprüleri köprüsü, BizTalk Services Portalı'nda artık birbirinden ayrılmıştır. Artık sözleşmeleri ve köprüleri ayrı olarak oluşturduğunuz ve çalışma zamanında köprüleri, EDI iletisi değerlere göre bir sözleşmesinin çözümleyin. Bkz: [Azure BizTalk Services oluşturma sözleşmelerde](/previous-versions/azure/hh689908(v=azure.100)), [BizTalk Hizmetleri portalını kullanarak bir EDI köprüsü oluşturma](/previous-versions/azure/dn793986(v=azure.100)), [BizTalk Hizmetleri portalını kullanarak bir AS2 köprüsü oluşturma](/previous-versions/azure/dn793993(v=azure.100))ve [ Nasıl köprüleri zamanında sözleşmeleri çözümlemezseniz?](/previous-versions/azure/dn794001(v=azure.100))  
* Anlaşmaları için şablonlar oluşturma seçeneğini kullanımdan kaldırılmıştır.  
* Gönderme tarafı sözleşme, artık her şema için farklı bir sınırlayıcı kümesi belirtebilirsiniz. Bu yapılandırma, gönderme tarafı sözleşmesi Protokolü Ayarları altında belirtilir. Daha fazla bilgi için [Oluştur X12 bir Azure BizTalk Services'da sözleşme](/previous-versions/azure/hh689847(v=azure.100)) ve [Azure BizTalk Services hizmetinde bir EDIFACT sözleşmesi oluşturun](/previous-versions/azure/dn606267(v=azure.100)). İki yeni varlıklar, aynı amaçla TPM OM API için de eklenir. Bkz: [X12DelimiterOverrides](/previous-versions/azure/dn798749(v=azure.100)) ve [EDIFACTDelimiterOverride](/previous-versions/azure/dn798748(v=azure.100)).  
* Türetilen türler dahil olmak üzere standart XSD yapıları artık desteklenmektedir. Bkz [kullanım standart XSD oluşturur, eşlemelerinde](/previous-versions/azure/dn793987(v=azure.100)) ve [kullanım türetilmiş türlerini eşleme senaryolar ve örnekler](/previous-versions/azure/).  
* AS2 ileti imzalama için yeni MIC algoritmaları ve yeni şifreleme algoritmaları destekler. Bkz: [Azure BizTalk Services hizmetinde bir AS2 sözleşmesi oluşturma](/previous-versions/azure/hh689890(v=azure.100)).  

## <a name="known-issues"></a>Bilinen Sorunlar

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>BizTalk Hizmetleri portalı güncelleştirmesinden sonra bağlantı sorunları
  BizTalk Hizmetleri portalını hizmete değişiklikleri geri almak için BizTalk Hizmetleri yükseltilirken açın varsa, BizTalk Hizmetleri portalını ile ilgili bağlantı sorunlarını karşılaşıyor.  
  Geçici bir çözüm olarak, tarayıcıyı yeniden, tarayıcı önbelleğini silin veya gizli modda portalını başlatın.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio IDE bir hata veya uyarı BizTalk Hizmetleri projesinde tıklarsanız yapıt bulunamıyor
Visual Studio 2012 güncelleştirme 3 RC sorunu düzeltmek için 1'i yükleyin.  

### <a name="custom-binding-project-reference"></a>Özel bağlama proje başvurusu
Visual Studio çözümü içinde bir BizTalk Services projesi ile aşağıdaki durumlar göz önünde bulundurun:  

* Aynı Visual Studio çözümü içinde bir BizTalk Services projesi ve özel bağlama projesi yoktur. BizTalk hizmeti projesini bu özel bağlama proje dosyasına bir başvuru içeriyor.
* BizTalk hizmeti projesini özel bir bağlama/davranışı DLL bir başvuru içeriyor.

'Visual Studio çözümünde başarılı bir şekilde oluşturacağınız'. Daha sonra 'Yeniden derle' veya 'çözümü Temizle'. Yeniden oluşturmanız veya yeniden temizlemek, bundan sonra aşağıdaki hata oluşur:  
  Dosya kopyalanamıyor <Path to DLL> "bin\Debug\FileName.dll" için. Başka bir işlem tarafından kullanıldığından işlem ' % s'dosya 'bin\Debug\FileName.dll' erişemiyor.  

#### <a name="workaround"></a>Geçici çözüm
* Varsa [Visual Studio 2012 Update 3](https://docs.microsoft.com/visualstudio/releasenotes/vs2012-update3-vs) olan yüklü, aşağıdaki iki seçeneğiniz vardır:
  
  * Visual Studio'yu yeniden başlatın veya
  * Çözümü yeniden başlatın. Ardından, yalnızca bir derleme çözümü gerçekleştirin.  
* Varsa [Visual Studio 2012 Update 3](https://docs.microsoft.com/visualstudio/releasenotes/vs2012-update3-vs) değil yüklü ve açık Görev Yöneticisi İşlemler sekmesinde MSBuild.exe işlem'i tıklatın ve sonra işlemi Sonlandır düğmesine tıklayın.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Yazdırılamayan karakterler HTTP üst bilgilerini yükselttiyseniz BasicHttpRelay uç noktalar ile yönlendirme Köprüsü ve BizTalk Hizmetleri portalından desteklenmiyor
İletiler için yükseltilen özellikleri bir parçası olarak yazdırılamayan karakterleri kullanırsanız, bu iletileri BasicHttpRelay bağlamayı kullanan geçiş hedeflere yönlendirilemiyor. İzleme kapsamında bloblar için URL olarak kodlanmış ve hedefler için beklemediğiniz kodlanmış Ayrıca, yükseltilen özellikler, kullanılabilir.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>Zaman uyumsuz MDN Gönder seçeneği işaretlenmemiş olsa bile MDN zaman uyumsuz olarak gönderilir
Seçerseniz, bu senaryo – göz önünde bulundurun **zaman uyumsuz MDN Gönder** onay kutusunu ve zaman uyumsuz MDN göndermek için bir URL belirtin, ardından kaldırın **zaman uyumsuz MDN Gönder** onay kutusunu yeniden MDN yine de gönderilir zaman uyumsuz Mdn'leri gönderme seçeneği izin seçilmemiş olsa bile URL belirttiniz.  
Geçici çözüm olarak, belirtilen URL önce işaretini temizlemeniz gerekir **zaman uyumsuz MDN Gönder** onay kutusunu ve ardından AS2 sözleşmesi dağıtın.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Boşluk karakterleri geçerli bir değişim ötesinde bir boş ileti askıya alma uç noktaya gönderilmesini neden
Ötesinde IEA segmenti boşluk varsa, ayırıcı Bu geçerli değişim sonu gibi davranır ve boşluk olarak bir sonraki iletiyi sonraki kümesini bakar. Bu, geçerli değişim olmadığından, rota hedefi için bir başarılı iletisi gönderilir ve boş bir ileti askıya alma uç nokta gönderilir mümkün görebilirsiniz.  

### <a name="tracking-in-biztalk-services-portal"></a>BizTalk Hizmetleri portalında izleme
İzleme olayları, en fazla EDI ileti işleme ve herhangi bir ilişki yakalanır. Bir ileti dışında Protokolü aşama başarısız olursa izleme başarılı olarak gösterilir. Bu durumda günlük bölümünde bakın **ayrıntıları** sütununda **izleme** hata ayrıntıları.
X12 ayarları gönderip ([Oluştur X12 bir Azure BizTalk Services'da sözleşme](/previous-versions/azure/hh689847(v=azure.100))) protokolü sahneye çıkarak bilgileri sağlayın.  

### <a name="update-agreement"></a>Güncelleştirme Sözleşmesi
BizTalk Hizmetleri portalında, bir anlaşma yapılandırıldığında bir kimliğinin niteleyicisi değiştirmenizi sağlar. Bu, tutarsız özelliklerinde neden olabilir. Örneğin, ZZ:1234567 ve ZZ:7654321 Niteleyici'ı kullanarak bir sözleşme yok. BizTalk Hizmetleri portalını profili ayarlarında ZZ:1234567 01:ChangedValue olacak şekilde değiştirin. Sözleşme açın ve 01:ChangedValue ZZ:1234567 yerine görüntülenir.
Niteleyici kimlik değiştirme, anlaşmayı sildikten, güncelleştirme **kimlikleri** iş ortağı profili ve sonra anlaşmayı yeniden oluşturun.  

> AZURE. Bu davranış uyarı X12 ve AS2 etkiler.  
> 
> 

### <a name="as2-attachments"></a>AS2 ekler
AS2 iletilerini desteklenmeyen eklerini gönderme veya alma. Özellikle, ekleri sessizce yoksayılır ve ileti gövdesi normal bir AS2 iletisinin işlendiği.  

### <a name="resources-remembering-path"></a>Kaynaklar: Yol hatırlama
Eklerken **kaynakları**, iletişim kutusu penceresine daha önce bir kaynak eklemek için kullanılan yolu hatırlayabilirsiniz değil. Daha önce kullanılan yolu için BizTalk Hizmetleri portalında web sitesine eklemeyi deneyin **Güvenilen siteler** Internet Explorer'da.  

### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Bir köprü varlık adını yeniden adlandırma ve değişiklikleri kaydetmeden projeyi kapatın, varlığın yeniden açmayı hatayla sonuçlanır
Aşağıdaki sırayla bir senaryoyu düşünün:  

* Bir köprü (örneğin bir XML One-Way köprüsü) bir BizTalk hizmeti projeye Ekle  
* Köprü, varlık adı özelliği için bir değer belirterek yeniden adlandırın. Bu, belirttiğiniz ad ile ilişkili .bridgeconfig dosyayı yeniden adlandırır.  
* .Bcs dosyasını (Visual Studio sekmede kapatarak) değişiklikleri kaydetmeden kapatın.  
* .Bcs dosyayı yeniden Çözüm Gezgini'nden açın.  
  İlişkili .bridgeconfig dosyanın belirtilen yeni ad olsa da varlık adının tasarım yüzeyinde hala eski adı olduğunu göreceksiniz. Köprü yapılandırmasının köprüsü bileşen çift tıklayarak açmayı denerseniz, aşağıdaki hatayı alıyorsunuz:  
  `‘<old name>’ Entity’s associated file ‘<old name>.bridgeconfig’ does not exist`
  Bu senaryoyu uygulamaya önlemek için BizTalk hizmeti projesini varlıklarda yeniden adlandırdıktan sonra değişiklikleri kaydettiğinizden emin olun.  
  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>Visual Studio projesinden bir yapıt çıkarılmıştır bile BizTalk hizmeti projesini başarıyla oluşturur
Visual Studio projesinden hariç tutmak ve (örneğin, bir istek iletisi türü belirterek), yapı köprü yapılandırmasında içerir veya BizTalk hizmeti projeye bir yapıtı (örneğin, bir XSD dosyası) eklediğiniz bir senaryoyu düşünün. Silinen yapı konumundaki Visual Studio projesinde burada eklenmiştir aynı disk üzerinde mevcut olduğu sürece böyle bir durumda, proje oluşturma herhangi bir hata vermeyiz.
  
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>BizTalk hizmeti projesini için şema kullanılabilirlik köprüleri yapılandırılırken denetlemez
Projeye eklenen bir şema başka bir şema alınıyorsa bir BizTalk hizmeti projesinde, BizTalk hizmeti projesini içeri aktarılan şema projeye eklenir denetlemez. Böyle bir projeyi oluşturmayı denerseniz, derleme hatalarını almıyor.
  
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>Yanıt iletisi için bir XML istek-yanıt köprü her zaman UTF-8 karakter dosyasındakiyle
Bu sürüm için bir XML istek-yanıt köprüsü yanıt iletisinden karakter her zaman UTF-8'e ayarlanır.
  
### <a name="user-defined-datatypes"></a>Kullanıcı tanımlı veri türleri
BizTalk bağdaştırıcı hizmeti özellik BizTalk bağdaştırıcı paketi bağdaştırıcılara kullanıcı tanımlı veri türleri bağdaştırıcısı işlemleri için kullanabilir.
Kullanıcı tanımlı veri türleri kullanırken (.dll) dosyaları sürücüsü: \Program BizTalk bağdaştırıcı Service\BAServiceRuntime\bin\ veya için Genel Derleme Önbelleği (GAC) BizTalk bağdaştırıcı hizmeti barındıran sunucuda kopyalayın. Aksi takdirde, istemcide aşağıdaki hata ortaya çıkabilir:  
```
<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
<faultcode>s:Client</faultcode>
<faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
<detail>
  <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
    <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
  </AFConnectRuntimeFault>
</detail>
</s:Fault>
```  
  
> [!IMPORTANT]
> Bir dosyayı genel derleme önbelleğine yüklemek için Gacutil.exe'yi kullanabilmek için önerilir. Bu araç ve Visual Studio komut satırı seçeneklerini nasıl kullanılacağını GACUtil.exe belgeler.  
> 
> 

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>BizTalk bağdaştırıcı hizmeti Web sitesi yeniden başlatılıyor
Yükleme **BizTalk bağdaştırıcı hizmeti çalışma zamanı*** oluşturur **BizTalk bağdaştırıcı hizmeti** içeren IIS web sitesinde **BAService** uygulama. **BAService** uygulama dahili olarak geçiş bağlama şirket içinde hizmet uç noktası buluta erişim ağını genişletmek için kullanır. Yalnızca şirket içi hizmeti başladığında, karşılık gelen geçiş uç nokta bir barındırılan hizmet şirket içi için Service Bus üzerinde olarak kaydedilir.  

Durdur ve uygulama başlatma, uygulama otomatik olarak başlatma yapılandırması uygulanır değil. Bu nedenle **BAService** olan durduruldu, her zaman yeniden başlatmanız gerekir **BizTalk bağdaştırıcı hizmeti** bunun yerine, web sitesi. Başlangıç durdurmak veya **BAService** uygulama.

### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Özel karakterler adresi ve varlık adlarının LOB bileşenleri kullanılmamalıdır
LOB bileşenlerinin adresi ve varlık adları özel karakterler kullanmamalısınız. Bunu yaparsanız, BizTalk hizmeti projesini dağıtımı sırasında bir hata alırsınız. Belirli '%' gibi karakterler, BizTalk bağdaştırıcı hizmeti Web sitesi durdurulmuş bir duruma geçebilir ve el ile başlatılması gerekir.

### <a name="test-map-with-get-context-property"></a>Testi Haritası ile bağlam özelliğini Al
Dönüşüm içeriyorsa bir **alma bağlam özelliği** eşleme işlemi **testi Haritası** başarısız olur. Geçici bir çözüm değiştirin **alma bağlam özelliği** amaçlı olarak sahte veri içeren dize birleştirme Map işlemi ile eşleme işlemi. Hedef şema doldurmak ve diğer dönüştürme işlevselliğini test etmek izin verin.

### <a name="test-map-property-does-not-display"></a>Test eşleme özelliği göstermez
**Testi Haritası** özellikleri, Visual Studio'da görüntülemez. Bu oluşabilir **özellikleri** penceresi ve **Çözüm Gezgini** penceresi değil aynı anda tutturulmuştur. Bu sorunu çözmek için yerleştirme **özellikleri** ve **Çözüm Gezgini** windows.  

### <a name="datetime-reformat-drop-down-is-grayed-out"></a>DateTime yeniden biçimlendirme açılan gri
Bir DateTime yeniden eşleme işlemi tasarım yüzeyine eklenir ve yapılandırılmış, biçim açılır listede gri olabilir. Bilgisayarın görüntü ayarlanmışsa bu durum ortaya çıkabilir **Orta – kaynağının % 125** veya **Larger – % 150**. Görüntü çözmek için kümesine **daha küçük – %100 (varsayılan)** aşağıdaki adımları kullanarak:  

1. Açık **Denetim Masası** tıklatıp **Görünüm ve kişiselleştirme**.
2. Tıklayın **görünen**.
3. Tıklayın **daha küçük – %100 (varsayılan)** tıklatıp **Uygula**.

**Biçimi** açılır listede beklendiği gibi artık çalışmalıdır.

### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>BizTalk Hizmetleri portalında yinelenen sözleşmeleri
Aşağıdaki senaryoyu göz önünde bulundurun:

1. Ticari ortak Yönetimi OM API'sini kullanarak bir anlaşma oluşturun.
2. BizTalk Hizmetleri portalında iki farklı sekmelerde Sözleşmesi'ni açın.
3. Her iki sekmede anlaşmamdan dağıtın.
4. Sonuç olarak, her iki anlaşmaları BizTalk Hizmetleri portalında yinelenen girişler výsledek dağıtılsın

**Geçici çözüm**. BizTalk Hizmetleri Portalı'nda yinelenen anlaşmaları herhangi birini açın ve dağıtımını kaldırabilir.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Yapıt deposunda sertifika bile güncelleştirildikten sonra köprüleri güncelleştirilmiş bir sertifika kullanmayın
Aşağıdaki senaryoları düşünün:  

**Senaryo 1: Hizmet uç noktası için bir köprü gelen ileti aktarım güvenliğini sağlamak için parmak izi temel alarak sertifikaları kullanma**  
Parmak izi tabanlı sertifikalar BizTalk hizmeti projenizde kullandığınız bir senaryo düşünün. Aynı ada ancak farklı bir parmak izi ile BizTalk Hizmetleri portalında bir sertifikayı güncelleştirmek, ancak henüz BizTalk hizmeti projesini uygun şekilde güncelleştirmez. Böyle bir senaryoda, eski sertifika verileri kanal önbellekte olabileceğinden, iletileri işlemek üzere köprü devam edebilir. Bundan sonra ileti işleme başarısız olur.  

**Geçici çözüm**: BizTalk hizmeti projesini bir sertifikayı güncelleştirmek ve projeyi yeniden dağıtın.  

**Senaryo 2: Hizmet uç noktası için bir köprü gelen ileti aktarım güvenliğini sağlamak için sertifikaları tanımlamak için ad tabanlı davranışları kullanma**

Ad tabanlı davranışları sertifikaları BizTalk hizmeti projenizde tanımlamak için kullandığınız bir senaryo düşünün. BizTalk Hizmetleri portalında bir sertifikayı güncelleştirmek ancak BizTalk hizmeti projesini uygun şekilde güncelleştirmez. Böyle bir senaryoda, eski sertifika verileri kanal önbellekte olabileceğinden, iletileri işlemek üzere köprü devam edebilir. Bundan sonra ileti işleme başarısız olur.  

**Geçici çözüm**: BizTalk hizmeti projesini bir sertifikayı güncelleştirmek ve projeyi yeniden dağıtın.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>SQL veritabanı çevrimdışı olsa bile, iletileri işlemek üzere köprüleri devam
(Çalışan bilgi dağıtılan yapılarını ve işlem hatlarını gibi depolayan) Microsoft Azure SQL veritabanı, olsa bile, çevrimdışı BizTalk Hizmetleri köprüleri bir süredir iletileri işlemeye devam eder. BizTalk Hizmetleri Köprü yapılandırması ve önbelleğe alınmış yapıları kullanıyor olmasıdır.
SQL veritabanı çevrimdışı olduğunda tüm iletileri işlemek üzere köprüleri istemiyorsanız, durdurmak veya BizTalk hizmeti askıya alma BizTalk Hizmetleri PowerShell cmdlet'lerini kullanabilirsiniz. Bkz: [Azure BizTalk Hizmeti Yönetimi örnek](https://go.microsoft.com/fwlink/p/?LinkID=329019) işlemlerini yönetmek Windows PowerShell cmdlet'leri için.  

### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Ek bir BOM karakter içeren bir köprünün özel kod bileşeni içinde XML ileti okuma
Bir köprünün özel kod içindeki bir XML ileti okumak için istediğiniz bir senaryo düşünün. .NET API System.Text.Encoding.UTF8.GetString(bytes) kullanıyorsanız ek bir BOM karakter çıkış iletisi başında yer alır. Ek BOM karakter içerecek şekilde çıkış istemiyorsanız, bu nedenle, kullanmalısınız ```System.IO.StreamReader().ReadToEnd()```.

### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>WCF kullanarak bir köprüye gönderme ölçeklendirilemez
WCF kullanarak bir köprüye gönderilen iletileri değil ölçeklendirin. Ölçeklenebilir bir istemci istiyorsanız, bunun yerine bir HttpWebRequest kullanmanız gerekir.

### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>YÜKSELTME: BizTalk Hizmetleri Preview'dan yükseltme yaptıktan sonra genel kullanılabilirlik (GA için) sağlayıcı hatası belirteci
Etkin toplu işleri bir EDI veya AS2 sözleşmesi yoktur. BizTalk hizmeti genel kullanıma Preview sürümünden yükselttiğinizde, aşağıdaki ortaya çıkabilir:

* Hata: Belirteç sağlayıcısı bir güvenlik belirteci sağlayamadı. Belirteç sağlayıcısı, döndürülen ileti: Uzak ad çözümlenemedi.
* Batch görevleriniz iptal edilir.

**Geçici çözüm**: BizTalk hizmeti genel kullanılabilirlik (GA için) güncelleştirildikten sonra anlaşmayı yeniden dağıtın.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>YÜKSELTME: BizTalk Services SDK'sı yükselttikten sonra eski köprüsü simgeleri araç kutusu gösterir
Köprüleri temsil eden eski simgeler 'D, BizTalk Hizmetleri SDK'ın önceki bir sürümünü yükseltmeden sonra araç köprüleri için eski simgeleri göstermek devam eder. Ancak, BizTalk hizmeti projesi Tasarımcı yüzeyine bir köprü eklerseniz, surface yeni simgeyi gösterir.  

**Geçici çözüm**. Altında .tbd dosyaları silerek bu sorunu geçici olarak çalışabilir <system drive>: \Users\<kullanıcı > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>YÜKSELTME: Genel kullanım için BizTalk Portal Önizleme Update'ten EDI özelliği kullanılabilir olmadığını belirten bir hata gösterebilir.
BizTalk Services genel kullanıma Preview'dan yükseltilirken BizTalk Hizmetleri portalında açarsanız, portalda şu hatayı alabilirsiniz:  

Bu özellik kullanılabilir değil Microsoft Azure BizTalk Services'ın bu sürümünde bir parçası olarak. Kullanmak için bu özellikler için uygun bir sürümünün geçin.  

**Çözüm**: Portaldan Kapat oturumunuzu tarayıcıyı açın ve ardından portalında oturum açın.  

### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>YÜKSELTME: BizTalk Services genel kullanıma yükseltildikten sonra yeni izleme verilerini görünmüyor
BizTalk Hizmetleri Önizleme abonelikte dağıtılmış bir XML köprüsü sahip olduğu bir senaryoyu ele alalım. Köprüye ileti gönderme ve izleme verileri karşılık gelen BizTalk Hizmetleri portalında kullanılabilir. Artık, genel kullanım için BizTalk Hizmetleri portalını ve BizTalk Hizmetleri çalışma zamanı BITS yükseltilir ve daha önce dağıttığınız aynı köprüsü uç noktası için bir ileti gönderme, izleme verilerini yükseltme sonrasında gönderilen iletiler için görünmez.  

### <a name="pipelines-versus-bridges"></a>İşlem hatları köprüleri karşılaştırması
Bu belge boyunca 'Köprüsü' ve 'işlem hattı' terim birbirlerinin yerine kullanılır. Her ikisi de aslında, BizTalk Services üzerinde dağıtılmış ileti işleme birimi, aynı anlama gelir.  

### <a name="concepts"></a>Kavramlar
[BizTalk Services](/previous-versions/azure/hh689864(v=azure.100))   

