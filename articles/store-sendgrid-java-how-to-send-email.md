---
title: SendGrid e-posta hizmeti (Java) kullanmaya nasıl | Microsoft Docs
description: Bilgi e-posta SendGrid e-posta hizmeti ile Azure üzerinde nasıl gönderin. Java dilinde yazılan kod örneklerini.
services: ''
documentationcenter: java
author: thinkingserious
manager: sendgrid
editor: mollybos
ms.assetid: cc75c43e-ede9-492b-98c2-9147fcb92c21
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/30/2014
ms.author: elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork
ms.openlocfilehash: 0cb75c1acb731432ed524560698e3355699b2500
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60931220"
---
# <a name="how-to-send-email-using-sendgrid-from-java"></a>Java'dan SendGrid kullanarak e-posta gönderme
Bu kılavuzda, Azure üzerinde SendGrid e-posta hizmeti ile genel programlama görevlerini gerçekleştirmek gösterilmiştir. Örnek Java dilinde yazılır. Senaryoları ele alınmaktadır **e-posta oluşturma**, **e-posta gönderme**, **ekler eklenirken**, **filtreleri kullanarak**ve **özelliklerini güncelleştirme**. SendGrid ve e-posta gönderme hakkında daha fazla bilgi için bkz. [sonraki adımlar](#next-steps) bölümü.

## <a name="what-is-the-sendgrid-email-service"></a>SendGrid e-posta hizmeti nedir?
SendGrid olduğu bir [bulut tabanlı e-posta hizmeti] sağlayan güvenilir [İşlem tabanlı e-posta teslimi], ölçeklenebilirlik ve gerçek zamanlı analiz, özel tümleştirmeyi kolaylaştıran esnek API'ler ile birlikte. SendGrid kullanım senaryoları şunları içerir:

* Otomatik olarak okundu bilgilerini müşterilere gönderme
* Müşteriler aylık e-ilanlara ve özel teklifler göndermek için dağıtım yönetme listeler
* Engellenen e-posta ve müşteri yanıt hızı gibi şeyler için gerçek zamanlı ölçümler toplama
* Eğilimleri belirlemenize yardımcı olması için rapor oluşturma
* İletme müşteri sorguları
* Uygulamanızdan e-posta bildirimleri

Daha fazla bilgi için bkz. <https://sendgrid.com>.

## <a name="create-a-sendgrid-account"></a>SendGrid hesabı oluşturma
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-the-javaxmail-libraries"></a>Nasıl yapılır: Javax.mail kitaplıklarını kullanma
Javax.mail kitaplıkları, örneğin almak <https://www.oracle.com/technetwork/java/javamail> ve kodunuzun aktarın. Yüksek düzeyde, aşağıdakileri yapmak için kullanarak SMTP e-posta göndermek için javax.mail kitaplığı kullanma işlemini verilmiştir:

1. Dahil olan bı'in SendGrid smtp.sendgrid.net SMTP sunucusunun SMTP değerlerini belirtin.

```
        import java.util.Properties;
        import javax.mail.*;
        import javax.mail.internet.*;

        public class MyEmailer {
           private static final String SMTP_HOST_NAME = "smtp.sendgrid.net";
           private static final String SMTP_AUTH_USER = "your_sendgrid_username";
           private static final String SMTP_AUTH_PWD = "your_sendgrid_password";

           public static void main(String[] args) throws Exception{
               new MyEmailer().SendMail();
           }

           public void SendMail() throws Exception
           {
              Properties properties = new Properties();
                 properties.put("mail.transport.protocol", "smtp");
                 properties.put("mail.smtp.host", SMTP_HOST_NAME);
                 properties.put("mail.smtp.port", 587);
                 properties.put("mail.smtp.auth", "true");
                 // …
```

1. Genişletme *javax.mail.Authenticator* sınıfı ve uygulamanızda *getPasswordAuthentication* yöntemi, SendGrid kullanıcı adınızı ve parolanızı döndürür.  

       private class SMTPAuthenticator extends javax.mail.Authenticator {
       public PasswordAuthentication getPasswordAuthentication() {
          String username = SMTP_AUTH_USER;
          String password = SMTP_AUTH_PWD;
          return new PasswordAuthentication(username, password);
       }
2. Bir kimliği doğrulanmış bir e-posta oturumu aracılığıyla oluşturmak bir *javax.mail.Session* nesne.  

       Authenticator auth = new SMTPAuthenticator();
       Session mailSession = Session.getDefaultInstance(properties, auth);
3. İletinizi oluşturun ve atayın **için**, **gelen**, **konu** ve içerik değerleri. Bu gösterilen [nasıl yapılır: E-posta oluşturma](#how-to-create-an-email) bölümü.
4. İletisi göndermesi bir *javax.mail.Transport* nesne. Bu gösterilen [nasıl yapılır: E-posta gönderme] [# nasıl yapılır-send-e-posta] bölümü.

## <a name="how-to-create-an-email"></a>Nasıl yapılır: E-posta oluşturma
E-posta için değerlerin nasıl belirtileceğini gösterir.

    MimeMessage message = new MimeMessage(mailSession);
    Multipart multipart = new MimeMultipart("alternative");
    BodyPart part1 = new MimeBodyPart();
    part1.setText("Hello, Your Contoso order has shipped. Thank you, John");
    BodyPart part2 = new MimeBodyPart();
    part2.setContent(
        "<p>Hello,</p>
        <p>Your Contoso order has <b>shipped</b>.</p>
        <p>Thank you,<br>John</br></p>",
        "text/html");
    multipart.addBodyPart(part1);
    multipart.addBodyPart(part2);
    message.setFrom(new InternetAddress("john@contoso.com"));
    message.addRecipient(Message.RecipientType.TO,
       new InternetAddress("someone@example.com"));
    message.setSubject("Your recent order");
    message.setContent(multipart);

## <a name="how-to-send-an-email"></a>Nasıl yapılır: E-posta Gönder
E-posta gönderme işlemini gösterir.

    Transport transport = mailSession.getTransport();
    // Connect the transport object.
    transport.connect();
    // Send the message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close the connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a>Nasıl yapılır: Bir ek ekleyin
Aşağıdaki kod bir ek eklemek nasıl gösterir.

    // Local file name and path.
    String attachmentName = "myfile.zip";
    String attachmentPath = "c:\\myfiles\\";
    MimeBodyPart attachmentPart = new MimeBodyPart();
    // Specify the local file to attach.
    DataSource source = new FileDataSource(attachmentPath + attachmentName);
    attachmentPart.setDataHandler(new DataHandler(source));
    // This example uses the local file name as the attachment name.
    // They could be different if you prefer.
    attachmentPart.setFileName(attachmentName);
    multipart.addBodyPart(attachmentPart);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Nasıl yapılır: Altbilgi, izleme ve analiz etkinleştirmek için filtreleri kullanın
SendGrid kullanarak ek e-posta işlevselliği sağlar *filtreleri*. Bu izleme'ye tıklayın, Google analytics, izleme aboneliği ve benzeri etkinleştirme gibi belirli işlevleri etkinleştirmek için bir e-posta iletisi eklenebilir ayarlarıdır. Filtreler tam bir listesi için bkz. [Filtresi Ayarları][Filter Settings].

* HTML metni gönderilen e-postanın en altında görünen sonuçlanır bir alt filtre nasıl ekleneceğini gösterir.

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"footer\":
          {\"settings\":
          {\"enable\":1,\"text/html\":
          \"<html><b>Thank you</b> for your business.</html>\"}}}}");
* Başka bir örneği bir filtre izleme tıklayın. Aşağıdaki gibi bir köprü e-posta metninizi içerir ve tıklatın hızı izlemek istediğinizi varsayalım:

      messagePart.setContent(
          "Hello,
          <p>This is the body of the message. Visit
          <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
          Thank you.",
          "text/html");
* İzlemeyi etkinleştirmek için aşağıdaki kodu kullanın:

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"clicktrack\":
          {\"settings\":
          {\"enable\":1}}}}");

## <a name="how-to-update-email-properties"></a>Nasıl yapılır: E-posta özelliklerini güncelleştir
Bazı e-posta özellikleri kullanılarak geçersiz kılınabilir **özelliğini** veya kullanılarak eklenen **özelliği Ekle**.

Örneğin, belirtmek için **ReplyTo** adresleri aşağıdakileri kullanır:

    InternetAddress addresses[] =
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };

    message.setReplyTo(addresses);

Eklemek için bir **Cc** alıcı aşağıdakileri kullanın:

    message.addRecipient(Message.RecipientType.CC, new
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a>Nasıl yapılır: Ek SendGrid hizmetlerini kullanma
SendGrid, Azure uygulamanızı ek SendGrid işlevinden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Tüm Ayrıntılar için bkz. [SendGrid API belgeleri][SendGrid API documentation].

## <a name="next-steps"></a>Sonraki adımlar
SendGrid e-posta hizmeti ile ilgili temel bilgileri öğrendiniz, daha fazla bilgi için bu bağlantıları izleyin.

* SendGrid kullanarak bir Azure dağıtımında gösteren örnek: [Bir Azure dağıtımında Java'dan SendGrid kullanarak e-posta gönderme](store-sendgrid-java-how-to-send-email-example.md)
* SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html>
* SendGrid API belgeleri: <https://sendgrid.com/docs/API_Reference/index.html>
* Azure müşterileri için SendGrid özel tekliftir: <https://sendgrid.com/windowsazure.html>

[https://sendgrid.com]: https://sendgrid.com
[https://sendgrid.com/pricing.html]: https://sendgrid.com/pricing.html
[http://www.sendgrid.com/azure.html]: https://www.sendgrid.com/windowsazure.html
[https://sendgrid.com/features]: https://sendgrid.com/features
[https://www.oracle.com/technetwork/java/javamail]: https://www.oracle.com/technetwork/java/javamail/index.html
[Filter Settings]: https://sendgrid.com/docs/API_Reference/Web_API/filter_settings.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/index.html
[https://sendgrid.com/azure.html]: https://sendgrid.com/windowsazure.html
[bulut tabanlı e-posta hizmeti]: https://sendgrid.com/email-solutions
[İşlem tabanlı e-posta teslimi]: https://sendgrid.com/transactional-email
