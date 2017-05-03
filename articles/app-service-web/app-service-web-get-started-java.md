---
title: "Beş dakika içinde Azure’da ilk Java web uygulamanızı oluşturma | Microsoft Docs"
description: "Örnek bir Java uygulaması dağıtarak, App Service&quot;ta web uygulamaları çalıştırmanın ne kadar kolay olduğunu öğrenin."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: cephalin;robmcm
translationtype: Human Translation
ms.sourcegitcommit: 2c33e75a7d2cb28f8dc6b314e663a530b7b7fdb4
ms.openlocfilehash: 2673a9c0d91510756a97b2dba3801d2925905c9a
ms.lasthandoff: 04/21/2017


---
# <a name="create-your-first-java-web-app-in-azure-in-five-minutes"></a>Beş dakika içinde Azure’da ilk Java web uygulamanızı oluşturma

[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Bu Hızlı Başlangıç, ilk Java web uygulamanızı birkaç dakika içinde [Azure App Service](../app-service/app-service-value-prop-what-is.md)’a dağıtmanıza yardımcı olur. Bu öğreticiyle çalışmanız bittiğinde, bulutta çalışır hale gelecek basit bir Java tabanlı web uygulamanız olur.

![Web Uygulamasına Göz Atma](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticide, Java web uygulaması oluşturmak ve Azure’da dağıtmak için Java EE Geliştiricileri için Eclipse IDE’nin nasıl kullanılacağı gösterilir. Henüz Eclipse’i yüklemediyseniz, ücretsiz olarak http://www.eclipse.org/ adresinden indirebilirsiniz.

Java web uygulamalarını Azure’a yayımlama işlemini basitleştirmek için, bu öğreticideki adımlarda [Eclipse için Azure Araç Seti](/azure/azure-toolkit-for-eclipse) kullanılır. Araç setini yükleme yönergeleri için bkz. [Eclipse için Azure Araç Setini Yükleme](/azure/azure-toolkit-for-eclipse-installation).

> [!NOTE]
>
> Bu öğreticideki adımları tamamlamak için JetBrains’den [IntelliJ IDEA](https://www.jetbrains.com/idea/)’yı da kullanabilirsiniz. O dağıtım ortamı için birkaç adımda küçük farklılıklar olabilir; öte yandan bu IDE için yayımlama işlemini basitleştirmek üzere kullanabileceğiniz bir de [IntelliJ için Azure Araç Seti](/azure/azure-toolkit-for-intellij) vardır.
>

Bu öğreticideki adımları tamamlamak için bir Azure aboneliğinizin olması gerekir. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir Azure hesabı](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

## <a name="create-a-dynamic-web-project-in-eclipse"></a>Eclipse’te Dinamik Web Projesi oluşturma

Eclipse IDE’de **Dosya**’ya, **Yeni**’ye ve ardından **Dinamik Web Projesi**’ne tıklayın.

![Yeni Dinamik Web Projesi](./media/app-service-web-get-started-java/file-new-dynamic-web-project-menu.png)

Dinamik Web Projesi iletişim kutusu görüntülendiğinde, uygulamayı **MyFirstJavaOnAzureWebApp** olarak adlandırın ve **Son**’a tıklayın.
   
![Dinamik Web Projesi iletişim kutusu](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

> [!NOTE]
>
> [Apache Tomcat](https://tomcat.apache.org/) gibi yerel bir çalışma zamanı ortamı yüklediyseniz, **Hedef çalışma zamanı** alanında bunu belirtebilirsiniz.
>

Dinamik web projeniz oluşturulduktan sonra, Proje Gezgini’nde projenizi genişleterek, **WebContent** klasörüne tıklayarak, **Yeni**’ye ve sonra da **JSP Dosyası**’na tıklayarak yeni bir JSP sayfası ekleyin.

![Yeni JSP Dosyası Menüsü](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

Yeni JSP Dosyası iletişim kutusu görüntülendiğinde, dosyayı **index.jsp** olarak adlandırın, üst klasörü **MyFirstJavaOnAzureWebApp/WebContent** olarak koruyun ve ardından **İleri**’ye tıklayın.

![Yeni JSP Dosyası İletişim Kutusu](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

Yeni JSP Dosyası iletişim kutusunun ikinci sayfasında, dosyayı **index.jsp** olarak adlandırın, üst klasörü **MyFirstJavaOnAzureWebApp/WebContent** olarak koruyun ve ardından **Son**’a tıklayın.

![Yeni JSP Dosyası İletişim Kutusu](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-2.png)

Yeni sayfanız Eclipse’te açıldığında, mevcut `<body></body>` bölümünü aşağıdaki kodla değiştirin:

```jsp
<body>
<h1><% out.println("Java on Azure!"); %></h1>
</body>
```

Sayfada yaptığınız değişiklikleri kaydedin.

![JSP Kodunu Düzenleme](./media/app-service-web-get-started-java/creating-index-jsp-page.png)

## <a name="publish-your-web-app-to-azure"></a>Web uygulamanızı Azure’a yayımlama

Web uygulamanızı Azure’a dağıtmak için, Eclipse için Azure Araç Seti tarafından sağlanan çeşitli özelliklerden yararlanırsınız.

Yayımlama işlemine başlamak için aşağıdaki yöntemlerden birini kullanın:

* Eclipse **Proje Gezgini**’nde projenize sağ tıklayın, ardından **Azure**’a ve **Azure Web App Olarak Yayımla** öğesine tıklayın.

   ![Azure Web App Olarak Yayımla Bağlam Menüsü](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

* Eclipse araç çubuğundaki **Yayımla** simgesine tıklayın ve sonra da **Azure Web App Olarak Yayımla**’ya tıklayın.

   ![Azure Web App Olarak Yayımla Açılan Menüsü](./media/app-service-web-get-started-java/publish-as-azure-web-app-drop-down-menu.png)

Henüz Azure hesabınızda oturum açmadıysanız, oturum açmanız istenir. Bunu yapmak için aşağıdaki adımları kullanın:

1. Azure hesabınızda oturum açarken iki farklı seçenek vardır; bu öğretici için, **Etkileşimli**’yi seçin.

   ![Azure Oturum Açma İletişim Kutusu](./media/app-service-web-get-started-java/azure-signin-dialog-box.png)

1. Azure kimlik bilgilerinizi girin ve ardından **Oturum aç**’a tıklayın.

   ![Azure Oturum Açma İletişim Kutusu](./media/app-service-web-get-started-java/azure-login-dialog-box.png)

1. Azure aboneliklerinizi seçin ve ardından **Seç**’e tıklayın.

   ![Azure Oturum Açma İletişim Kutusu](./media/app-service-web-get-started-java/select-azure-subscriptions-dialog-box.png)

> [!NOTE]
>
> **Etkileşimli** ve **Otomatik** oturum açma seçenekleri hakkındaki ayrıntılı yönergeler, [Eclipse için Azure Araç Seti’nin Azure Oturum Açma Yönergeleri](https://go.microsoft.com/fwlink/?linkid=846174) makalesinde sağlanmıştır.
>

Azure hesabınızda oturum açtıktan sonra, **Web Uygulaması Dağıtın** iletişim kutusu görüntülenir. Azure’a ilk kez bir web uygulaması yayımlıyorsanız, listelenmiş hiçbir Uygulama Hizmeti görmüyor olmalısınız. Böyle bir durumda veya yeni bir Uygulama Hizmeti oluşturmak istemeniz durumunda, bir sonraki adımınız yeni Uygulama Hizmetini oluşturmak olacaktır. Bunu yapmak için **Oluştur**’a tıklayın.

![Web Uygulaması Dağıtım İletişim Kutusu](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

**Uygulama Hizmeti Oluştur** iletişim kutusu görüntülenir; sağlamanız gereken ilk veriler şunlardır:

* Web uygulamanız için benzersiz bir ad. Bu da daha sonra web uygulamanızın DNS adresine dönüşecektir; örneğin: **MyJavaWebApp** adı *myjavawebapp.azurewebsites.net* olacaktır.

* Web uygulamanızın hangi web kapsayıcısını kullanacağı; örneğin: **En Yeni Tomcat 8.5**.

* Azure aboneliğiniz.

   ![Uygulama Hizmeti Oluştur İletişim Kutusu](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

Mevcut Uygulama Hizmeti Planlarınız yoksa veya yeni hizmet planı oluşturmak istiyorsanız, aşağıdaki bilgileri sağlamanız gerekir:

* Yeni hizmet planı için benzersiz bir ad; bu ad, gelecekte Azure Araç Seti’ni kullanarak web uygulamalarını yayımladığınızda gösterilir ve hesabınızı yönetirken [Azure Portal](https://portal.azure.com)’da listelenir.

* Hizmet planınızın oluşturulacağı coğrafi konum.

* Hizmet planınız için fiyatlandırma katmanı.

   ![Uygulama Hizmeti Planı Oluştur](./media/app-service-web-get-started-java/create-app-service-plan.png)

Ardından, **Kaynak grubu** sekmesine tıklayın. Mevcut Kaynak Gruplarınız yoksa veya yenisini oluşturmak isterseniz, yeni kaynak grubunuz için benzersiz bir ad sağlamanız gerekir; aksi takdirde, açılan menüden mevcut bir kaynak grubu seçin.

![Uygulama Hizmeti Planı Oluştur](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

Son olarak, **JDK** sekmesine tıklayın. Geliştiricilerin üçüncü taraf veya özel Java Geliştirici Setleri (JDK) belirtmesine olanak tanıyan çeşitli seçenekler listelenmiştir, ancak bu öğretici için **Varsayılan**’ı seçmeli ve ardından **Oluştur**’a tıklamalısınız.

![Uygulama Hizmeti Planı Oluştur](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)

Azure Araç Seti yeni uygulama hizmetinizi oluşturmaya başlar ve işlem sürerken bir ilerleme durumu iletişim kutusu görüntüler.

![Uygulama Hizmeti Oluştur İlerleme Çubuğu](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

Yeni uygulama hizmetiniz oluşturulduğunda belirtmeniz gereken son seçenek, web uygulamanızın yeni web sitenizin köküne dağıtılıp dağıtılmayacağıdır. Örneğin, *wingtiptoys.azurewebsites.net* konumunda bir uygulama hizmetiniz varsa köke dağıtmamayı seçerseniz, **MyFirstJavaOnAzureWebApp** adlı web uygulamanız *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp* konumuna dağıtılır.

![Web Uygulamasını Köke Dağıtma](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

Önceki adımlarını tümünü bitirdiğinizde, web uygulamanızı Azure’da yayımlamak için **Dağıt**’a tıklayın.

![Web Uygulamasını Azure’da Dağıtma](./media/app-service-web-get-started-java/deploy-web-app-to-azure.png)

Tebrikler! Web uygulamanızı başarılı bir şekilde Azure’da dağıttınız! Şimdi Azure web sitesinde web uygulamanızın önizlemesine bakabilirsiniz:

![Web Uygulamasına Göz Atma](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="updating-your-web-app"></a>Web uygulamanızı güncelleştirme

Web uygulamanızı başarıyla Azure’da yayımladıktan sonra, web uygulamanızın güncelleştirilmesi çok daha basit bir işlemdir ve aşağıdaki adımlar değişiklikleri web uygulamanıza yayımlamanız için size yol gösterir.

İlk olarak, önceki örnek JSP kodunu değiştirerek başlığın yerine bugünün tarihini koyun:

```jsp
<%@ page
    language="java"
    contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"
    import="java.text.SimpleDateFormat"
    import="java.util.Date" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<% SimpleDateFormat date = new SimpleDateFormat("yyyy/MM/dd"); %>
<title><% out.println(date.format(new Date())); %></title>
</head>
<body>
<h1><% out.println("Java on Azure!"); %></h1>
</body>
</html>
```

![JSP Kodunu Güncelleştirme](./media/app-service-web-get-started-java/updating-index-jsp-page.png)

Sayfada yaptığınız değişiklikleri kaydettikten sonra, Eclipse **Proje Gezgini**’nde projenize sağ tıklayın, ardından **Azure**’a ve **Azure Web App Olarak Yayımla** öğesine tıklayın.

![Güncelleştirilen Web Uygulamasını Yayımlama](./media/app-service-web-get-started-java/publish-updated-web-app-context-menu.png)

**Web Uygulamasını Dağıtın** iletişim kutusu görüntülendiğinde, öncesinden gelen uygulama hizmetiniz listelenir. Web uygulamanızı güncelleştirmek için, tek yapmanız gereken uygulama hizmetinizi vurgulamak ve **Dağıt**’a tıklayarak değişikliklerinizi yayımlamaktır.

![Web Uygulamasını Azure’da Dağıtma](./media/app-service-web-get-started-java/deploy-web-app-to-azure.png)

> [!NOTE]
>
> Web uygulamanızı uygulama hizmetinizin köküne dağıtıyorsanız, değişikliklerinizi yayımlarken her seferinde **Köke dağıt** seçeneğini yeniden işaretlemeniz gerekir.
>

Değişikliklerinizi yayımladıktan sonra, tarayıcınızda sayfa başlığının bugünün tarihiyle değiştirildiğini fark edersiniz.

![Web Uygulamasına Göz Atma](./media/app-service-web-get-started-java/browse-web-app-2.png)

## <a name="deleting-your-web-app"></a>Web uygulamanızı silme

Web uygulamasını silmek için, Azure Araç Seti’nin bir parçası olan **Azure Gezgini**’ni kullanabilirsiniz. **Azure Gezgini** görünümü henüz Eclipse’te görünür durumda değilse, görüntülemek için aşağıdaki adımları izleyin:

1. **Pencere**’ye tıklayın, **Görünümü Göster**’e ve sonra da **Diğer**’e tıklayın.

   ![Görünümü Göster menüsü](./media/app-service-web-get-started-java/show-azure-explorer-view-1.png)

2. **Görünümü Göster** iletişim kutusu görüntülendiğinde, **Azure Gezgini**’ni seçin ve **Tamam**’a tıklayın.

   ![Görünümü Göster İletişim Kutusu](./media/app-service-web-get-started-java/show-azure-explorer-view-2.png)

Web uygulamanızı Azure Gezgini’nden silmek için, **Web Uygulamaları** düğümünü genişletmeniz, ardından web uygulamanıza sağ tıklamanız ve **Sil**’i seçmeniz gerekir.

![Web Uygulamasını Silme](./media/app-service-web-get-started-java/delete-web-app-context-menu.png)

Web uygulamanızın silinip silinmeyeceği sorulduğunda **Tamam**’a tıklayın.

## <a name="next-steps"></a>Sonraki Adımlar

Java IDE’leri için Azure Araç Setleri hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

* [Eclipse için Azure Araç Seti (Bu Makale)](../azure-toolkit-for-eclipse.md)
  * [Eclipse için Azure Araç Seti Yenilikleri](../azure-toolkit-for-eclipse-whats-new.md)
  * [Eclipse için Azure Araç Setini Yükleme](../azure-toolkit-for-eclipse-installation.md)
  * [Eclipse için Azure Araç Setinde Oturum Açma Yönergeleri](https://go.microsoft.com/fwlink/?linkid=846174)
* [IntelliJ için Azure Araç Seti](../azure-toolkit-for-intellij.md)
  * [IntelliJ için Azure Araç Seti Yenilikleri](../azure-toolkit-for-intellij-whats-new.md)
  * [IntelliJ için Azure Araç Setini Yükleme](../azure-toolkit-for-intellij-installation.md)
  * [IntelliJ için Azure Araç Setinde Oturum Açma Yönergeleri](https://go.microsoft.com/fwlink/?linkid=846179)

Azure’u Java ile kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure Java Geliştirici Merkezi](https://azure.microsoft.com/develop/java/) ve [Visual Studio Team Services için Java Araçları](https://java.visualstudio.com/).

