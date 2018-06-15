---
title: Twilio (Java) telefon görüşmesi yapma | Microsoft Docs
description: Azure üzerinde bir Java uygulamasında Twilio kullanarak bir web sayfasından telefon görüşmesi öğrenin.
services: ''
documentationcenter: java
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 0381789e-e775-41a0-a784-294275192b1d
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 04ecb80a2a9e15b549b47138caf71c7e64bda500
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23865942"
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Azure üzerinde bir Java uygulamasında Twilio kullanarak bir telefon araması yapma
Aşağıdaki örnek, Azure üzerinde barındırılan bir web sayfasından arama yapmak için Twilio nasıl kullanabileceğinizi gösterir. Sonuçta elde edilen uygulama telefon araması değerleri için aşağıdaki ekran görüntüsünde gösterildiği gibi ister.

![Twilio ve Java kullanarak azure çağrı formu][twilio_java]

Bu konudaki kodu kullanmak için aşağıdakileri yapmanız gerekir:

1. Twilio hesabı ve kimlik doğrulama alma belirteci. Twilio ile çalışmaya başlamak için fiyatlandırma değerlendirmek [http://www.twilio.com/pricing][twilio_pricing]. Konumundaki kaydolabilirsiniz [https://www.twilio.com/try-twilio][try_twilio]. Twilio tarafından sağlanan API'si hakkında daha fazla bilgi için bkz: [http://www.twilio.com/api][twilio_api].
2. Twilio JAR edinin. Konumundaki [https://github.com/twilio/twilio-java][twilio_java_github], GitHub kaynakları indirmek ve kendi JAR oluşturabilir veya önceden oluşturulmuş bir JAR (veya ile bağımlılıklar olmadan) yükleyebilirsiniz.
   Bu konu kodda, önceden oluşturulmuş bağımlılıkları ile TwilioJava 3.3.8 JAR kullanılarak yazılmıştır.
3. JAR, Java derleme yolu ekleyin.
4. Bu Java uygulaması oluşturmak için Eclipse kullanıyorsanız, Twilio JAR Eclipse'nın dağıtım derleme özelliğini kullanarak, uygulama dağıtım dosyanızda (WAR) içerir. Bu Java uygulaması oluşturmak için Eclipse kullanmıyorsanız Twilio JAR Java uygulamanız aynı Azure rolünü içerdiği ve uygulamanızı sınıfı yoluna eklenen emin olun.
5. Cacerts anahtar deposu MD5 parmak izi 67:CB:9 D ile Equifax güvenli sertifika yetkilisi sertifikasına sahip olmasını: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (seri numarasını 35:DE:F4:CF ve SHA1 parmak izi D2:32:09:AD:23:D 3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Sertifika yetkilisi (CA) sertifikası budur [https://api.twilio.com] [ twilio_api_service] Twilio API'leri kullandığınızda çağrılan hizmet. Bu CA sertifikası JDK'ın cacert depoya ekleme hakkında daha fazla bilgi için bkz: [Java CA sertifika deposu için bir sertifika ekleme][add_ca_cert].

Ayrıca, bilgileri aşina [Hello World uygulama kullanarak bir Azure araç setini Eclipse için oluşturma][azure_java_eclipse_hello_world], veya kullanıyorsanız azure'da Java uygulamaları barındırmak için başka teknikler ile Eclipse kullanmayan, kesinlikle önerilir.

## <a name="create-a-web-form-for-making-a-call"></a>Arama yapmak için web formu oluşturma
Aşağıdaki kod, arama yapmak için kullanıcı verilerini almak için web formu oluşturulacağını gösterir. Bu örnekte, yeni bir dinamik web projesi adlı **TwilioCloud**, oluşturulduğu ve **callform.jsp** bir JSP dosyası olarak eklendi.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Çağrı yapmak için kod oluşturma
Kullanıcı tarafından callform.jsp görüntülenen form tamamlandığında olarak adlandırılır, aşağıdaki kod, arama iletisi oluşturur ve çağrı oluşturur. Bu örneğin amaçları doğrultusunda JSP dosyası adlı **makecall.jsp** ve eklendi **TwilioCloud** projesi. (Kullanım Twilio hesabı ve kimlik doğrulama belirteci atanan yer tutucu değerlerini yerine **accountSID** ve **authToken** aşağıdaki kodda.)

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";

         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");

         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");

         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");

         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");

         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;

         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");

         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);

         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Çağrıyı yapan yanı sıra makecall.jsp Twilio uç noktası, API sürümü ve çağrı durumunu görüntüler. Aşağıdaki ekran örneğidir:

![Twilio ve Java kullanarak azure çağrı yanıtı][twilio_java_response]

## <a name="run-the-application"></a>Uygulamayı çalıştırma
Uygulamanızı çalıştırmak için üst düzey adımları şunlardır; Bu adımları bulunabilir ayrıntıları [Hello World uygulama kullanarak bir Azure araç setini Eclipse için oluşturma][azure_java_eclipse_hello_world].

1. Azure'a TwilioCloud WAR dışarı **approot** klasör. 
2. Değiştirme **startup.cmd** TwilioCloud WAR sıkıştırmasını açmak için.
3. İşlem öykünücüsü için uygulamanızı derleyin.
4. Dağıtımınızı işlem öykünücüsünde başlatın.
5. Bir tarayıcı açın ve Çalıştır **http://localhost:8080/TwilioCloud/callform.jsp**.
6. Değerleri girin, tıklatın **bu çağırmaya**ve ardından makecall.jsp sonuçlarında görebilirsiniz.

Azure, bulut dağıtımını yeniden derleyebilirsiniz dağıtmaya hazır olduğunuzda, Azure'a dağıtma ve http:// çalıştırma*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp tarayıcıda (ve değer için alternatif  *your_hosted_name*).

## <a name="next-steps"></a>Sonraki adımlar
Bu kod, Azure üzerinde Twilio Java kullanarak temel işlevselliğini göstermek için sağlanmıştır. Azure'a üretimde dağıtmadan önce daha fazla hata işleme veya diğer özellikler eklemek isteyebilirsiniz. Örneğin:

* Bir web formu kullanmak yerine, Azure storage bloblarında veya SQL veritabanı telefon numaralarını depolamak ve metin çağırmak için kullanabilirsiniz. Azure storage bloblarında Java kullanma hakkında daha fazla bilgi için bkz: [Java'dan Blob Depolama hizmetini kullanmayı][howto_blob_storage_java]. SQL veritabanı Java kullanma hakkında daha fazla bilgi için bkz: [SQL veritabanında kullanarak Java][howto_sql_azure_java].
* Kullanabileceğinizi **RoleEnvironment.getConfigurationSettings** hesap Kimliğini ve kimlik doğrulama belirteci makecall.jsp değerleri sabit kodlama yerine dağıtımınızın yapılandırma ayarları Twilio alınamadı. Hakkında bilgi için **RoleEnvironment** sınıfı için bkz: [jsp biçiminde Azure hizmeti çalışma zamanı kitaplığını kullanarak] [ azure_runtime_jsp] ve Azurehizmetiçalışmazamanıpaketibelgeleri[http://dl.windowsazure.com/javadoc][azure_javadoc].
* Twilio tarafından sağlanan URL makecall.jsp kodunu atar [http://twimlets.com/message][twimlet_message_url], **Url** değişkeni. Bu URL çağrısı ile devam etmek nasıl Twilio bildiren bir Twilio biçimlendirme dili (TwiML) yanıt sağlar. Örneğin, döndürülen TwiML içerebilir bir  **&lt;Say&gt;**  çağrısı alıcıya konuşulan metin sonuçlanır fiil. Twilio tarafından sağlanan URL kullanmak yerine, kendi hizmet Twilio'nın isteğine yanıt vermek için derleme; Daha fazla bilgi için bkz: [ses ve Java SMS yetenekler için kullanım Twilio nasıl][howto_twilio_voice_sms_java]. TwiML hakkında daha fazla bilgi bulunabilir [http://www.twilio.com/docs/api/twiml][twiml]ve hakkında daha fazla bilgi  **&lt;Say&gt;**  ve diğer Twilio fiiller bulunabilir [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Twilio güvenlik yönergeleri okuyun [https://www.twilio.com/docs/security][twilio_docs_security].

Twilio hakkında ek bilgi için bkz: [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Ayrıca Bkz.
* [Ses ve Java SMS yetenekler için Twilio kullanma][howto_twilio_voice_sms_java]
* [Bir sertifika Java CA sertifika depolama alanına ekleme][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
