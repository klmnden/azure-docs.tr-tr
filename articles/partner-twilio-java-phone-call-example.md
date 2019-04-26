---
title: (Java) Twilio'dan telefon görüşmesi yapma | Microsoft Docs
description: Bir web sayfasından azure'da bir Java uygulamasında Twilio kullanarak telefon görüşmesi hakkında bilgi edinin.
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
ms.openlocfilehash: 0d055b1a78622665137a6abad18681a728ae2b30
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60422687"
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Azure'da bir Java uygulamasında Twilio kullanarak telefon görüşmesi yapma
Aşağıdaki örnek, Azure'da barındırılan bir web sayfasından çağrı yapmak için Twilio nasıl kullanabileceğinizi gösterir. Elde edilen uygulama, aşağıdaki ekran görüntüsünde gösterildiği gibi kullanıcıdan telefon araması değerlerini ister.

![Twilio ve Java kullanan azure çağrı formu][twilio_java]

Bu konudaki kodu kullanmak için aşağıdakileri yapmanız gerekir:

1. Twilio hesap ve kimlik doğrulama alma belirteci. Fiyatlandırma, Twilio ile çalışmaya başlamak için değerlendirmek [ https://www.twilio.com/pricing ] [ twilio_pricing]. Adresindeki kaydolabilirsiniz [ https://www.twilio.com/try-twilio ] [ try_twilio]. Twilio tarafından sağlanan API hakkında daha fazla bilgi için bkz: [ https://www.twilio.com/api ] [ twilio_api].
2. Twilio JAR edinin. Konumunda [ https://github.com/twilio/twilio-java ] [ twilio_java_github], GitHub kaynakları indirmek ve kendi JAR oluşturmak veya önceden oluşturulmuş bir JAR (ile veya bağımlılıkları olmadan) indirin.
   Bu konudaki kodu, önceden oluşturulmuş bağımlılıkları ile TwilioJava 3.3.8 JAR kullanılarak yazılmıştır.
3. JAR Java derleme yolu ekleyin.
4. Bu Java uygulaması oluşturmak için Eclipse kullanıyorsanız, Twilio JAR Eclipse'nın dağıtım derleme özelliğini kullanarak uygulama dağıtım dosyanızda (WAR) içerir. Bu Java uygulaması oluşturmak için Eclipse kullanmıyorsanız, Twilio JAR aynı Azure rol Java uygulamanızı olarak içerdiği ve uygulama sınıf yolunuza eklendi emin olun.
5. Cacerts deponuzu içeren MD5 parmak izi 67:CB:9 D Equifax güvenli sertifika yetkilisi sertifikasıyla sağlamak: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (seri numarasını 35:DE:F4:CF ve SHA1 parmak izi D2:32:09:AD:23:D 3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Bu sertifika yetkilisi (CA) sertifikası, [ https://api.twilio.com ] [ twilio_api_service] tanımak için Twilio API'lerini kullanırken çağrılan hizmet. Bu CA sertifikası, JDK'ın cacert depoya ekleme hakkında daha fazla bilgi için bkz: [Java CA Store sertifika bir sertifika ekleme][add_ca_cert].

Ayrıca, bilgileri konusunda [bir Hello World uygulaması kullanarak Eclipse için Azure araç seti oluşturma][azure_java_eclipse_hello_world], ya da diğer tekniklerle kullanıyorsanız azure'da Java uygulamalarını barındırmak için Eclipse kullanarak değil, kesinlikle önerilir.

## <a name="create-a-web-form-for-making-a-call"></a>Arama yapmak için web formu oluşturma
Aşağıdaki kod, arama yapmak için kullanıcı verilerini almak için bir web formu oluşturma işlemini gösterir. Bu örnekte, yeni dinamik web projesi amacıyla adlı **TwilioCloud**, oluşturulduğu ve **callform.jsp** JSP dosyası olarak eklendi.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "https://www.w3.org/TR/html4/loose.dtd">
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

## <a name="create-the-code-to-make-the-call"></a>Çağrı yapmak için kod oluşturun
Kullanıcı tarafından callform.jsp görüntülenen form tamamladığında çağrılır, aşağıdaki kod, çağrı ileti oluşturur ve çağrı oluşturur. Bu örneğin amacı doğrultusunda, JSP dosyası adlı **makecall.jsp** ve eklenmişse **TwilioCloud** proje. (Kullanım Twilio hesabınız ve kimlik doğrulama belirteci atanmış yer tutucu değerlerini yerine **accountSID** ve **authToken** aşağıdaki kodda.)

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "https://www.w3.org/TR/html4/loose.dtd">
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
         String Url="https://twimlets.com/message";
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

Çağrıyı yapan yanı sıra makecall.jsp Twilio uç noktası, API sürümü ve çağrı durumunu görüntüler. Aşağıdaki anlık görüntüde bir örnek verilmiştir:

![Twilio ve Java kullanan azure çağrı yanıtı][twilio_java_response]

## <a name="run-the-application"></a>Uygulamayı çalıştırma
Uygulamanızı çalıştırmak için üst düzey adımlar aşağıda verilmiştir; Bu adımları şu yolda bulunabilir: ayrıntıları [bir Hello World uygulaması kullanarak Eclipse için Azure araç seti oluşturma][azure_java_eclipse_hello_world].

1. Azure, TwilioCloud WAR dışarı **approot** klasör. 
2. Değiştirme **startup.cmd** , TwilioCloud WAR sıkıştırmasını.
3. İşlem öykünücüsü için uygulamanızı derleyin.
4. Dağıtım işlem öykünücüsünde başlatın.
5. Bir tarayıcı açın ve çalıştırın `http://localhost:8080/TwilioCloud/callform.jsp`.
6. Değerleri girin, tıklayın **bu çağrı yapmak**ve ardından makecall.jsp sonuçları görebilirsiniz.

Azure, bulut dağıtımını yeniden derleyin dağıtmak hazır olduğunuzda, Azure'a dağıtma ve http:// çalıştırma*your_hosted_name*tarayıcıda.cloudapp.net/TwilioCloud/callform.jsp (, değeri  *your_hosted_name*).

## <a name="next-steps"></a>Sonraki adımlar
Bu kod, Azure üzerinde Java Twilio kullanarak temel işlevselliğini göstermek için sağlanmıştır. Üretimde Azure'a dağıtmadan önce daha fazla hata işleme veya diğer özellikler eklemek isteyebilirsiniz. Örneğin:

* Web formu kullanmak yerine, Azure depolama blobları veya SQL veritabanı telefon numaraları depolamak ve metin çağırmak için kullanabilirsiniz. Java'da Azure depolama BLOB'ları kullanma hakkında daha fazla bilgi için bkz: [Java'dan Blob Depolama hizmetini kullanma][howto_blob_storage_java]. 
* Kullanabileceğinizi **RoleEnvironment.getConfigurationSettings** hesap kimliği ve kimlik doğrulama belirteci makecall.jsp değerleri sabit kodlama yerine dağıtımınızın yapılandırma ayarlarından Twilio alınacak. Hakkında bilgi için **RoleEnvironment** sınıfı [JSP biçiminde Azure hizmeti çalışma zamanı kitaplığı kullanarak] [ azure_runtime_jsp] ve Azurehizmetiçalışmazamanıpaketibelgeleri[ http://dl.windowsazure.com/javadoc][azure_javadoc].
* Bir Twilio tarafından sağlanan URL makecall.jsp kod atar [ https://twimlets.com/message ] [ twimlet_message_url], **Url** değişkeni. Bu URL, Twilio çağrısı ile devam etmek nasıl bildiren bir Twilio biçimlendirme dili (TwiML) yanıt sağlar. Örneğin, döndürülen TwiML içerebilir bir **&lt;Say&gt;** çağrı alıcıya konuşulan metnin sonuçlanır fiil. Twilio tarafından sağlanan URL kullanmak yerine, Twilio'nın isteğine yanıt vermek için kendi hizmet oluşturabilirsiniz; Daha fazla bilgi için [kullanım Twilio ses ve SMS özellikleri Java için nasıl][howto_twilio_voice_sms_java]. TwiML hakkında daha fazla bilgi şu adreste bulunabilir: [ https://www.twilio.com/docs/api/twiml ] [ twiml]ve daha fazla bilgi **&lt;Say&gt;** ve diğer Twilio fiiller konumunda bulunabilir [ https://www.twilio.com/docs/api/twiml/say ] [ twilio_say].
* Twilio güvenlik yönergeleri okuyun [ https://www.twilio.com/docs/security ] [ twilio_docs_security].

Twilio hakkında ek bilgi için bkz: [ https://www.twilio.com/docs ] [ twilio_docs].

## <a name="see-also"></a>Ayrıca Bkz.
* [Ses ve SMS özellikleri Java için Twilio kullanma][howto_twilio_voice_sms_java]
* [Java CA sertifika Store için bir sertifika ekleme][add_ca_cert]

[twilio_pricing]: https://www.twilio.com/pricing
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_api]: https://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: https://github.com/twilio/twilio-java
[twimlet_message_url]: https://twimlets.com/message
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app 
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: https://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: https://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: https://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: https://www.twilio.com/docs/security
[twilio_docs]: https://www.twilio.com/docs
[twilio_say]: https://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
