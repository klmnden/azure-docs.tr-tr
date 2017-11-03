---
title: "Yay önyükleme uygulamasını Azure App Service'e dağıtma | Microsoft Docs"
description: "Bu öğretici yay önyükleme Başlarken web uygulamasını Azure App Service'e dağıtmak için geliştiricilere adımlarında yol gösterir."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.openlocfilehash: 8776142d5452bf5057990702c89aa1a541382ffc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-a-spring-boot-application-to-the-azure-app-service"></a>Azure Uygulama Hizmeti’ne Spring Boot Uygulaması dağıtma

**[Yay Framework]**  Java geliştiriciler kuruluş düzeyinde uygulamalar oluşturmanıza yardımcı olan bir açık kaynak çözümüdür ve bu platformu üzerine kurulmuştur daha popüler projeleri biri [yay önyükleme], tek başına Java uygulamaları oluşturmak için basitleştirilmiş bir yaklaşım sağlar.

Bu öğretici ancak örnek yay önyükleme Başlarken web uygulaması oluşturma ve dağıttıktan anlatılır [Azure App Service].

### <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Bir Azure aboneliği; bir Azure aboneliği zaten sahip değilseniz, etkinleştirebilir, [MSDN abone Avantajlarınızı] veya kaydolun bir [ücretsiz Azure hesabı].
* Güncel bir [Java Geliştirme Seti (JDK)].
* Apache'nın [Maven] aracını (sürüm 3) yapılandırma.
* A [Git] istemci.

## <a name="create-the-spring-boot-getting-started-web-app"></a>Yay önyükleme Başlarken web uygulaması oluşturma

Aşağıdaki adımlar, basit bir yay önyükleme web uygulaması oluşturma ve yerel olarak test etmek için gereken adımlarda size yol gösterir.

1. Bir komut istemi açın ve uygulamanızı tutun ve bu dizine değiştirmek için yerel bir dizin oluşturun; Örneğin:
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   --ya da--
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Kopya [yay önyükleme Başlarken] örnek proje yeni oluşturduğunuz; dizine örneğin:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. Projeyi Dizin Değiştir; Örneğin:
   ```
   cd gs-spring-boot
   cd complete
   ```

1. Maven kullanarak JAR dosyasını oluşturun; Örneğin:
   ```
   mvn package
   ```

1. Web uygulaması oluşturulduktan sonra JAR dosyasına dizini değiştirin ve web uygulaması başlatın; Örneğin:
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. Web uygulaması http://localhost: 8080 bir web tarayıcısı kullanarak göz atarak test veya curl kullanılabilir varsa aşağıdaki örnekteki gibi sözdizimini kullanın:
   ```
   curl http://localhost:8080
   ```

1. Aşağıdaki ileti görürsünüz: **Tebrikler İlkbahar önyüklemesinden!**

   ![Örnek uygulaması Gözat][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a>Java ile kullanmak için bir Azure web uygulaması oluşturma

Aşağıdaki adımlar bir Azure Web uygulaması oluşturma, Java için gereken ayarları yapılandırın ve FTP kimlik bilgilerinizi yapılandırma adımlarında size yol gösterir.

1. Gözat [Azure portal] ve oturum açın.

1. Azure portalındaki hesabınızda oturum açtıktan sonra menü simgesini **uygulama hizmetleri**:
   
   ![Azure portalına][AZ01]

1. Zaman **uygulama hizmetleri** sayfası görüntülenirse, tıklatın **+ Ekle** yeni bir uygulama hizmeti oluşturmak için.

   ![Uygulama hizmeti oluşturma][AZ02]

1. Web uygulama şablonları görüntülendiğinde, temel Microsoft Web uygulaması için bağlantıyı tıklatın.

   ![Web Uygulama Şablonları][AZ03]

1. Bilgi sayfası Web uygulaması şablonu görüntülenen için tıklattığınızda **oluşturma**.

   ![Web Uygulaması Oluşturma][AZ04]

1. Web uygulamanız için benzersiz bir ad sağlayın ve herhangi bir ek ayarları belirtin ve ardından **oluşturma**.

   ![Web uygulaması ayarları oluşturma][AZ05]

1. Web uygulamanız oluşturulduktan sonra menü simgesini **uygulama hizmetleri**ve yeni oluşturulan web uygulamanız'ye tıklayın:

   ![Liste Web uygulamaları][AZ06]

1. Web uygulamanızı görüntülendiğinde, aşağıdaki adımları kullanarak Java sürümü belirtin:

   a. Tıklatın **uygulama ayarları** menü öğesi.

   b. Seçin **Java 8** Java sürümü için.

   c. Seçin **Newest** alt Java Sürüm için.

   d. Seçin **yeni Tomcat 8.5** web kapsayıcısı için. (Aslında bu kapsayıcı kullanılmaz; Azure kapsayıcı yay önyükleme uygulamanızdan kullanır.)

   e. **Kaydet** düğmesine tıklayın.

   ![Uygulama ayarları][AZ07]

1. Aşağıdaki adımları kullanarak, FTP dağıtımı kimlik bilgileri belirtin:

   a. Tıklatın **dağıtım kimlik bilgileri** menü öğesi.

   b. Kullanıcı adı ve parola belirtin.

   c. **Kaydet** düğmesine tıklayın.

   ![Dağıtım kimlik bilgilerini belirtin][AZ08]

1. Aşağıdaki adımları kullanarak FTP bağlantı bilgileri alın:

   a. Tıklatın **dağıtım kimlik bilgileri** menü öğesi.

   b. Tam FTP kullanıcı adı ve URL kopyalayın ve Bu öğretici için sonraki bölüme kaydedin.

   ![FTP URL ve kimlik bilgileri][AZ09]

## <a name="deploy-your-spring-boot-web-app-to-azure"></a>Yay önyükleme web uygulamanızı Azure'a dağıtma

Aşağıdaki adımları yay önyükleme web uygulamanızı Azure'a dağıtmak için adımlarda size yol gösterecek.

1. Windows Not Defteri gibi bir metin düzenleyicide açın ve aşağıdaki metni yeni bir belgeye yapıştırın ve ardından dosyayı farklı Kaydet *web.config*:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. Kaydettiğiniz sonra *web.config* dosya sistemi için bu öğreticinin önceki bölümünden URL, kullanıcı adı ve parola kullanarak FTP üzerinden web uygulamanıza bağlayın. Örneğin:
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. Uzak dizin, web uygulamanızın kök klasörüne değiştirme (olduğu anda */site/wwwroot*), ardından yay önyükleme uygulamanızdan JAR dosyasını kopyalamanız ve *web.config* daha önce gelen. Örneğin:
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. JAR dağıttıktan sonra ve *web.config* dosyaları web uygulamanız için Azure Portalı'nı kullanarak, web uygulaması yeniden vermeniz gerekir:

   ![][AZ10]

1. Web uygulaması bir web tarayıcısı kullanarak, web uygulamanızın URL'sine göz atarak test veya curl kullanılabilir varsa aşağıdaki örnekteki gibi sözdizimini kullanın:
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. Aşağıdaki ileti görürsünüz: **Tebrikler İlkbahar önyüklemesinden!**

   ![Örnek uygulaması Gözat][SB02]

## <a name="next-steps"></a>Sonraki adımlar

Azure üzerinde yay önyükleme uygulamalarında kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure kapsayıcı Hizmeti'nde Linux'ta yay önyükleme uygulamasını dağıtma](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md)

* [Azure kapsayıcı hizmeti Kubernetes kümesinde yay önyükleme uygulamasını dağıtma](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-kubernetes.md)

Azure’u Java ile kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure Java Geliştirici Merkezi] ve [Visual Studio Team Services için Java Araçları].

FTP kullanarak Azure depoying web uygulamaları hakkında ek bilgi için bkz [FTP/S kullanarak Azure App Service için uygulamanızı dağıtma].

Yay önyükleme örnek proje hakkında daha fazla ayrıntı için bkz: [yay önyükleme Başlarken].

Kendi yay önyükleme uygulamaları ile çalışmaya başlama hakkında bilgi için bkz: **yay Initializr** https://start.spring.io/ adresindeki.

Web uygulamanız için ek ayarlarını yapılandırma hakkında daha fazla bilgi için bkz: [Azure App Service'te web uygulamalarını yapılandırma].

<!-- URL List -->

[Azure App Service]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[Azure Java Geliştirici Merkezi]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Azure App Service'te web uygulamalarını yapılandırma]: /azure/app-service/web-sites-configure
[FTP/S kullanarak Azure App Service için uygulamanızı dağıtma]: https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp
[ücretsiz Azure hesabı]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Geliştirme Seti (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Visual Studio Team Services için Java Araçları]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN abone Avantajlarınızı]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[yay önyükleme]: http://projects.spring.io/spring-boot/
[yay önyükleme Başlarken]: https://github.com/spring-guides/gs-spring-boot
[Yay Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB01.png
[SB02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB02.png

[AZ01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ01.png
[AZ02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ02.png
[AZ03]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ03.png
[AZ04]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ04.png
[AZ05]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ05.png
[AZ06]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ06.png
[AZ07]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ07.png
[AZ08]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ08.png
[AZ09]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ09.png
[AZ10]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ10.png
