---
title: "Azure App Service’te Java web uygulaması oluşturma | Microsoft Belgeleri"
description: "Bu öğreticide Azure App Service’e bir Java web uygulamasını nasıl dağıtacağınız gösterilmiştir."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: d6e73cc3-8b71-4742-a197-3edeabc6a289
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: get-started-article
ms.date: 11/01/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: acacfbead6cf0d68ccfeb5e818a2b04f2be9b902


---
# <a name="create-a-java-web-app-in-azure-app-service"></a>Azure App Service’te bir Java web uygulaması oluşturma
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Bu öğreticide [Azure Portal]’ı ile [Azure App Service’te Java web uygulaması] oluşturma adımları gösterilmiştir. Azure Portal, Azure kaynaklarınızı yönetmek için kullanabileceğiniz bir web arabirimidir.

> [!NOTE]
> Bu öğreticiyi tamamlamak için Microsoft Azure hesabınızın olması gerekir. Bir hesabınız yoksa, [Visual Studio abone avantajlarınızı etkinleştirebilir ] veya [ücretsiz deneme için kaydolabilirsiniz].
> 
> Bir Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak istiyorsanız [App Service’i Deneyin]’e gidin. Burada, App Service'te hemen bir kısa süreli başlangıç web uygulaması oluşturabilirsiniz; kredi kartı gerekmez ve hiçbir taahhüt yoktur.
> 
> 

## <a name="java-application-options"></a>Java uygulaması seçenekleri
Bir App Service web uygulamasında Java uygulaması kurmanın çeşitli yöntemleri vardır. 

1. Bir uygulama oluşturun ve **Uygulama ayarları**’nı yapılandırın.
   
    App Service, varsayılan yapılandırmayla çeşitli Tomcat ve Jetty sürümleri sağlar. Barındıracağınız uygulama yerleşik sürümlerden biriyle birlikte çalışacaksa, bu web kapsayıcısı ayarlama yöntemi en kolayıdır ve web kapsayıcısına bir war dosyası yüklemek istediğinizde mükemmeldir. Bu yöntem için, Azure Portal'da bir uygulama oluşturun ve Java sürümünüzle birlikte istediğiniz Java web kapsayıcısını seçmek için uygulamanızın **Uygulama ayarları** dikey penceresine gidin. Bu yöntemi kullandığınızda, Java ve web kapsayıcınız Program Dosyaları’ndan çalıştırılır. Diğer yöntemler web kapsayıcısını ve potansiyel olarak JVM’yi disk alanınıza yerleştirir. Bu modeli kullandığınızda, dosya sisteminin bu bölümündeki dosyaları düzenleme erişiminiz olmaz. Bu, *server.xml* dosyasını yapılandırma veya kitaplık dosyalarını */lib* klasörüne yerleştirme gibi işlemleri yapmayacağınız anlamına gelir. Daha fazla bilgi için, bu öğreticinin ilerleyen bölümlerindeki [ Java web uygulaması oluşturma ve yapılandırma](#appsettings) bölümüne bakın.
2. Azure Market’teki bir şablonu kullanma
   
    Azure Market, Tomcat veya Jetty web kapsayıcılarıyla Java web uygulamalarını otomatik olarak oluşturan şablonları içerir. Şablonların oluşturduğu web kapsayıcısı yapılandırılabilir. Daha fazla bilgi için, bu öğreticinin [Azure Market’teki bir Java şablonunu kullanma](#marketplace) bölümüne bakın.
3. Bir uygulama oluşturun ve sonra yapılandırma dosyalarını el ile kopyalayın ve düzenleyin. 
   
    App Service tarafından sağlanan herhangi bir web kapsayıcısında dağıtılmayan özel bir Java uygulamasını barındırmak isteyebilirsiniz. Örneğin:
   
   * Java uygulamanız Tomcat veya Jetty’nin doğrudan App Service tarafından desteklenmeyen veya galeride sağlanmayan bir sürümünü gerektiriyor.
   * Java uygulamanız HTTP isteklerini alıyor ve önceden varolan bir web kapsayıcısına WAR dosyası olarak dağıtmıyor.
   * Web kapsayıcısını sıfırdan kendiniz yapılandırmak istiyorsunuz. 
   * Java’nın App Service'te desteklenmeyen bir sürümünü kullanmak ve kendiniz yüklemek istiyorsunuz.
     
     Bu gibi durumlar için, Azure Portal'ı kullanarak bir uygulama oluşturabilir ve ardından uygun çalışma zamanı dosyalarını el ile belirtebilirsiniz. Bu durumda, dosyalar App Service planınız için depolama alanı kotanızdan sayılacaktır. Daha fazla bilgi için bkz. [Azure’a özel bir Java web uygulaması yükleme].

## <a name="a-nameportala-create-and-configure-a-java-web-app"></a><a name="portal"></a> Java web uygulaması oluşturma ve yapılandırma
Bu bölümde bir web uygulaması oluşturma ve portalın **Uygulama ayarları** dikey penceresini kullanarak bunu Java için yapılandırma gösterilir.

1. [Azure Portal]’da oturum açın.
2. **Yeni > Web + Mobil > Web Uygulaması**’na tıklayın.
   
    ![Yeni Web Uygulaması][newwebapp]
3. Web uygulaması için **Web uygulaması** kutusuna bir ad girin.
   
    Web uygulamasının URL’si {ad}.azurewebsites.net şeklinde olacağından, bu ad azurewebsites.net etki alanında benzersiz olmalıdır. Girdiğiniz ad benzersiz değilse, metin kutusunda kırmızı bir ünlem işareti gösterilir.
4. Bir **Kaynak Grubu** seçin veya yeni bir tane oluşturun.
   
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için Azure Portal’ı kullanma].
5. Bir **App Service planı/Konum** seçin veya yeni bir tane oluşturun.
   
    App Service planları hakkında daha fazla bilgi için bkz. [Azure App Service planlarına genel bakış].
6. **Oluştur**’a tıklayın.
   
    ![Web Uygulaması Oluşturma][newwebapp2]
7. Web uygulaması oluşturulduğunda **Web Apps > {web uygulamanız}**’a tıklayın.
   
    ![Web Uygulaması Seçme][selectwebapp]
8. **Web uygulaması** dikey penceresinde, **Ayarlar**’a tıklayın.
9. **Uygulama ayarları**’na tıklayın.
10. İstediğiniz **Java sürümünü** seçin. 
11. İstediğiniz **Java alt sürümü** seçin. **En Yeni** seçeneğini belirlerseniz, uygulamanız bu Java ana sürümü için App Service'te kullanılabilen en yeni alt sürümü kullanır. **En Yeni** seçeneği, **Uygulama ayarları**’ndan oluşturulan Java uygulamalarına özeldir. Java uygulamanızı galeriden oluşturursanız, kendi kapsayıcınızı ve JVM değişikliklerini yönetmeniz gerekir. 
12. İstediğiniz **Web kapsayıcısını** seçin. **En Yeni** ile başlayan bir kapsayıcı adı seçerseniz, uygulamanız bu web kapsayıcısı ana sürümünün App Service’te kullanılabilir olan en yeni sürümünde kalır. 
    
    ![Web Kapsayıcısı Sürümleri][versions]
13. **Kaydet** düğmesine tıklayın.
    
    Birkaç dakika içinde, web uygulamanız Java tabanlı hale gelir ve seçtiğiniz web kapsayıcısını kullanacak şekilde yapılandırılmış olur.
14. **Web uygulamaları > {yeni web uygulamanız}**’a tıklayın.
15. Yeni siteye göz atmak için **URL**’ye tıklayın.
    
    Web sayfası Java tabanlı bir web uygulaması oluşturduğunuzu onaylar.

## <a name="a-namemarketplacea-use-a-java-template-from-the-azure-marketplace"></a><a name="marketplace"></a> Azure Market’teki bir Java şablonunu kullanma
Bu bölümde bir Java şablonu oluşturmak için Azure Market’i nasıl kullanacağınız gösterilmiştir. Aynı genel akış, Java tabanlı bir mobil veya API uygulaması oluşturmak için de kullanılabilir. 

1. [Azure Portal]’da oturum açın.
2. **Yeni > Market**’e tıklayın.
   
    ![Yeni Market][newmarketplace]
3. **Web + Mobil**’e tıklayın.
   
    **Web + Mobil**’i seçebileceğiniz **Market** dikey penceresini görmek için sola kaydırmanız gerekebilir.
4. Arama metin kutusuna **Apache Tomcat** veya **Jetty** gibi bir Java uygulama sunucusu adı yazın ve ardından Enter tuşuna basın.
5. Arama sonuçlarında, Java uygulaması sunucusuna tıklayın.
   
    ![Web Mobile Jetty][webmobilejetty]
6. İlk **Apache Tomcat** veya **Jetty** dikey penceresinde **Oluştur**’a tıklayın.
   
    ![Jetty Portal Dikey Penceresi][jettyblade]
7. Sonraki **Apache Tomcat** veya **Jetty** dikey penceresinde, **Web uygulaması** kutusuna web uygulaması adını girin.
   
    Web uygulamasının URL’si {ad}.azurewebsites.net şeklinde olacağından, bu ad azurewebsites.net etki alanında benzersiz olmalıdır. Girdiğiniz ad benzersiz değilse, metin kutusunda kırmızı bir ünlem işareti gösterilir.
8. Bir **Kaynak Grubu** seçin veya yeni bir tane oluşturun.
   
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için Azure Portal’ı kullanma].
9. Bir **App Service planı/Konum** seçin veya yeni bir tane oluşturun.
   
    App Service planları hakkında daha fazla bilgi için bkz. [Azure App Service planlarına genel bakış].
10. **Oluştur**’a tıklayın.
    
    ![Jetty Portal Oluşturma][jettyportalcreate2]
    
    Kısa bir süre içinde, normalde bir dakika içinde, Azure yeni web uygulamasını oluşturmayı tamamlar.
11. **Web uygulamaları > {yeni web uygulamanız}**’a tıklayın.
12. Yeni siteye göz atmak için **URL**’ye tıklayın.
    
    ![Jetty URL'si][jettyurl]
    
    Tomcat varsayılan sayfalar kümesi ile birlikte gelir; Tomcat’i seçerseniz, aşağıdaki örneğe benzer bir sayfa görürsünüz.
    
    ![Apache Tomcat kullanan web uygulaması][tomcat]
    
    Jetty’yi seçerseniz, aşağıdaki örneğe benzer bir sayfa görürsünüz. Jetty, varsayılan bir sayfa kümesine sahip değildir, bu nedenle boş Java sitesi için kullanılan aynı JSP burada yeniden kullanılır.
    
    ![Jetty kullanan web uygulaması][jetty]

Artık uygulama kapsayıcısı içeren bir web uygulaması oluşturduğunuza göre, uygulamanızı karşıya yükleme hakkında bilgi için [Sonraki adımlar](#next-steps) bölümüne bakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, Azure App Service'teki web uygulamanızda çalışan bir Java uygulama sunucusuna sahipsiniz. Kendi kodunuzu web uygulamasına dağıtmak için bkz. [Java web uygulamanıza bir uygulama veya web sayfası ekleme].

Azure'da Java uygulamaları geliştirme hakkında daha fazla bilgi için bkz. [Java Geliştirici Merkezi].

<!-- URL List -->

[Java web uygulamanıza bir uygulama veya web sayfası ekleme]: ./web-sites-java-add-app.md
[Azure App Service planlarına genel bakış]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure Portal]: https://portal.azure.com/
[Visual Studio abone avantajlarınızı etkinleştirebilir]: http://go.microsoft.com/fwlink/?LinkId=623901
[ücretsiz deneme için kaydolabilirsiniz]: http://go.microsoft.com/fwlink/?LinkId=623901
[App Service’i Deneyin]: http://go.microsoft.com/fwlink/?LinkId=523751
[Azure App Service’te Java web uygulaması]: http://go.microsoft.com/fwlink/?LinkId=529714
[Java Geliştirici Merkezi]: /develop/java/
[Azure kaynaklarınızı yönetmek için Azure Portal’ı kullanma]: ../azure-portal/resource-group-portal.md
[Azure’a özel bir Java web uygulaması yükleme]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png



<!--HONumber=Nov16_HO2-->


