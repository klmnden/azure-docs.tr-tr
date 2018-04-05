---
title: SendGrid e-posta hizmetine (Java) kullanma | Microsoft Docs
description: Bilgi nasıl Azure üzerinde SendGrid e-posta hizmeti ile e-posta gönderin. Java dilinde yazılan kod örnekleri.
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
ms.openlocfilehash: 85a0e302626ca14ac039ee6f662f372ddbeb62c5
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="how-to-send-email-using-sendgrid-from-java"></a>SendGrid Java kullanarak e-posta gönderme
Bu kılavuz, Azure üzerinde SendGrid e-posta hizmeti ile genel programlama görevleri gerçekleştirmek gösterilmiştir. Örnekler Java'da yazılmış. Kapsamdaki senaryolar dahil **e-posta oluşturma**, **e-posta gönderme**, **eklerini ekleme**, **filtreleri kullanarak**, ve **özelliklerini güncelleştirme**. SendGrid ve e-posta gönderme hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü.

## <a name="what-is-the-sendgrid-email-service"></a>SendGrid e-posta hizmeti nedir?
SendGrid olan bir [bulut tabanlı e-posta hizmeti] güvenilir sağlayan [işleme uygun e-posta teslimi], ölçeklenebilirlik ve gerçek zamanlı analiz özel tümleştirme kolaylaştırmak esnek API'leri yanı sıra. Ortak SendGrid kullanım senaryoları şunları içerir:

* Giriş müşterileri için otomatik olarak gönderme
* Aylık e ilanları ve özel teklifler müşteriler göndermek için dağıtım yönetme listeler
* Engellenen e-posta ve müşteri yanıtlama gibi için gerçek zamanlı ölçümleri toplama
* Eğilimleri belirlemenize yardımcı olmak için rapor oluşturma
* Müşteri sorguları iletme
* Uygulamanızdan e-posta bildirimleri

Daha fazla bilgi için bkz: <http://sendgrid.com>.

## <a name="create-a-sendgrid-account"></a>SendGrid hesabı oluşturma
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-the-javaxmail-libraries"></a>Nasıl yapılır: javax.mail kitaplıkları kullanma
Javax.mail kitaplıkları, örneğin almak <http://www.oracle.com/technetwork/java/javamail> ve bunları kodunuza içeri aktarın. Bir üst düzey, SMTP aracılığıyla e-posta göndermek için javax.mail kitaplığı kullanmak için aşağıdakileri yapmak için bir işlemdir:

1. SendGrid için smtp.sendgrid.net SMTP sunucu dahil SMTP değerlerini belirtin.

```
        import java.util.Properties;
        import javax.activation.*;
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

1. Genişletme *javax.mail.Authenticator* sınıfı ve uygulamanızda *getPasswordAuthentication* yöntemi, SendGrid kullanıcı adı ve parola döndürür.  

       private class SMTPAuthenticator extends javax.mail.Authenticator {
       public PasswordAuthentication getPasswordAuthentication() {
          String username = SMTP_AUTH_USER;
          String password = SMTP_AUTH_PWD;
          return new PasswordAuthentication(username, password);
       }
2. Kimliği doğrulanmış e-posta oturumu aracılığıyla oluşturma bir *javax.mail.Session* nesnesi.  

       Authenticator auth = new SMTPAuthenticator();
       Session mailSession = Session.getDefaultInstance(properties, auth);
3. İletinizi oluşturun ve Ata **için**, **gelen**, **konu** ve içerik değerleri. Bu gösterilen [nasıl yapılır: bir e-posta oluşturma](#how-to-create-an-email) bölümü.
4. İletisi göndermesi bir *javax.mail.Transport* nesnesi. Bu gösterilen [nasıl yapılır: bir e-posta Gönder] [nasıl yapılır: bir e-posta Gönder] bölümü.

## <a name="how-to-create-an-email"></a>Nasıl yapılır: bir e-posta oluşturma
Bir e-posta için değerlerin nasıl belirtileceği gösterir.

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

## <a name="how-to-send-an-email"></a>Nasıl yapılır: bir e-posta Gönder
Bir e-posta göndermek nasıl gösterir.

    Transport transport = mailSession.getTransport();
    // Connect the transport object.
    transport.connect();
    // Send the message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close the connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a>Nasıl yapılır: bir eki ekleyin
Aşağıdaki kod bir eke gösterilmiştir.

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

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Nasıl yapılır: altbilgi, izleme ve analiz etkinleştirmek için filtreleri kullanın
SendGrid kullanım yoluyla ek e-posta işlevselliği sağlar *filtreleri*. Bu izleme'ye tıklayın, Google analytics, izleme abonelik ve benzeri etkinleştirme gibi belirli işlevleri etkinleştirmek için e-posta iletisine eklenecek ayarlardır. Filtrelerin tam bir listesi için bkz: [filtresi ayarlarını][Filter Settings].

* Gönderilen e-posta alt kısmında görünen HTML metin sonuçlanan altbilgi filtre eklemek nasıl gösterir.

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"footer\":
          {\"settings\":
          {\"enable\":1,\"text/html\":
          \"<html><b>Thank you</b> for your business.</html>\"}}}}");
* Filtre başka bir örneği izleme tıklayın. E-posta metninizi aşağıdaki gibi bir köprü içerir ve tıklatın oranı izlemek istediğiniz varsayalım:

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

## <a name="how-to-update-email-properties"></a>Nasıl yapılır: güncelleştirme e-posta özellikleri
Bazı e-posta özellikleri kullanılarak üzerine yazılabilir **ayarlamak * özellik*** veya kullanarak eklenmiş **ekleme*özelliği ***.

Örneğin, belirtmek için **ReplyTo** adresleri aşağıdakileri kullanın:

    InternetAddress addresses[] =
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };

    message.setReplyTo(addresses);

Eklemek için bir **Cc** alıcı için aşağıdakini kullanın:

    message.addRecipient(Message.RecipientType.CC, new
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a>Nasıl yapılır: ek SendGrid Hizmetleri kullanın
SendGrid Azure uygulamanızı ek SendGrid işlevinden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Ayrıntılar için bkz: [SendGrid API belgelerine][SendGrid API documentation].

## <a name="next-steps"></a>Sonraki adımlar
SendGrid e-posta hizmeti temel bilgileri öğrendiğinize göre daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* Bir Azure dağıtımında SendGrid kullanarak oluşturulduğunu gösteren örnek: [bir Azure dağıtımında SendGrid Java kullanarak e-posta gönderme](store-sendgrid-java-how-to-send-email-example.md)
* SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html>
* SendGrid API belgelerine: <https://sendgrid.com/docs/API_Reference/index.html>
* SendGrid özel teklif Azure müşteriler için: <https://sendgrid.com/windowsazure.html>

[http://sendgrid.com]: https://sendgrid.com
[http://sendgrid.com/pricing.html]: http://sendgrid.com/pricing.html
[http://www.sendgrid.com/azure.html]: https://www.sendgrid.com/windowsazure.html
[http://sendgrid.com/features]: https://sendgrid.com/features
[http://www.oracle.com/technetwork/java/javamail]: http://www.oracle.com/technetwork/java/javamail/index.html
[Filter Settings]: https://sendgrid.com/docs/API_Reference/Web_API/filter_settings.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/index.html
[http://sendgrid.com/azure.html]: https://sendgrid.com/windowsazure.html
[bulut tabanlı e-posta hizmeti]: https://sendgrid.com/email-solutions
[işleme uygun e-posta teslimi]: https://sendgrid.com/transactional-email
