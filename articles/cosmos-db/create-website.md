---
title: "Bir şablon - Azure Cosmos DB ile bir web uygulaması dağıtma | Microsoft Docs"
description: "Bir Azure Cosmos DB hesabı, Azure App Service Web Apps ve Azure Resource Manager şablonu kullanarak örnek bir web uygulamasına dağıtmayı öğrenin."
services: cosmos-db, app-service\web
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 087d8786-1155-42c7-924b-0eaba5a8b3e0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 7ceb4bf97c29a18d6879af55615eea46037c51ce
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>Azure Cosmos DB ve Azure App Service Web Apps bir Azure Resource Manager şablonu kullanarak dağıtın
Bu öğretici bir Azure Resource Manager şablonu dağıtma ve tümleştirmek için nasıl kullanılacağını gösterir [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), bir [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) web uygulaması ve bir örnek web uygulaması.

Azure Resource Manager şablonları kullanarak dağıtımını ve yapılandırmasını Azure kaynaklarınızı kolayca otomatikleştirebilirsiniz.  Bu öğretici, bir web uygulaması dağıtma ve otomatik olarak Azure Cosmos DB hesap bağlantı bilgilerini yapılandırma gösterilmektedir.

Bu öğreticiyi tamamladıktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır:  

* Nasıl bir Azure Resource Manager şablonu dağıtma ve Azure Cosmos DB hesabınız ve Azure App Service'in web uygulamasında tümleştirmek için kullanabilir miyim?
* Nasıl bir Azure Resource Manager şablonu dağıtma ve bir Azure Cosmos DB hesap, App Service Web Apps bir web uygulamasını ve Webdeploy uygulama tümleştirmek için kullanabilir miyim?

<a id="Prerequisites"></a>

## <a name="prerequisites"></a>Ön koşullar
> [!TIP]
> Bu öğreticide Azure Resource Manager şablonları veya JSON konusunda deneyim varsaymaz olsa da, başvurulan şablonları veya dağıtım seçenekleri, değiştirmek istediğiniz sonra bu alanlardan her birini bilgisi gerekir.
> 
> 

Bu öğreticide yönergeleri izlemeden önce aşağıdakileri yapın:

* Azure aboneliği. Azure abonelik tabanlı bir platformdur.  Bir aboneliği edinme hakkında daha fazla bilgi için bkz: [satın alma seçenekleri](https://azure.microsoft.com/pricing/purchase-options/), [üye teklifleri](https://azure.microsoft.com/pricing/member-offers/), veya [ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/).

## <a id="CreateDB"></a>1. adım: şablon dosyalarını indirme
Bu öğreticide kullanacağız şablon dosyalarını yükleyerek başlayalım.

1. Karşıdan [Azure Cosmos DB hesap, Web uygulamaları oluşturmak ve bir gösterim uygulama örneği dağıtma](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) şablonu yerel bir klasöre (örneğin C:\Azure Cosmos DBTemplates). Bu şablon, bir Azure Cosmos DB hesabı, bir App Service web uygulaması ve bir web uygulamasına dağıtır.  Ayrıca, Azure Cosmos DB hesabınıza bağlanmak için web uygulamasını da otomatik olarak yapılandırır.
2. Karşıdan [bir Azure Cosmos DB hesap ve Web uygulamaları örnek oluşturma](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) şablonu yerel bir klasöre (örneğin C:\Azure Cosmos DBTemplates). Bu şablon, bir App Service web uygulaması Azure Cosmos DB hesap dağıtır ve kolayca yüzey Azure Cosmos DB bağlantısı bilgilerini sitenin uygulama ayarlarını değiştirir, ancak bir web uygulaması içermez.  

<a id="Build"></a>

## <a name="step-2-deploy-the-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>2. adım: Azure Cosmos DB hesabı, App Service web uygulaması ve tanıtım uygulama örneği dağıtma
Şimdi şimdi bizim ilk şablonunu dağıtın.

> [!TIP]
> Şablon aşağıda girilen Azure Cosmos DB hesap adını ve web uygulaması adı bir) geçerli ve (b) kullanılabilir olduğunu doğrulamaz.  Dağıtım göndermeden önce sağlamak için plan adları kullanılabilirliğini doğrulamanız önerilir.
> 
> 

1. Oturum açma [Azure Portal](https://portal.azure.com), yeni'yi tıklatın ve "Şablon dağıtımı" için arama yapın.
    ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment1.png)
2. Şablon dağıtımı öğeyi seçin ve tıklatın **oluşturma** ![şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment2.png)
3. Tıklatın **Düzen şablonu**DocDBWebsiteTodo.json şablon dosyasının içeriğini yapıştırın ve tıklatın **kaydetmek**.
   ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment3.png)
4. Tıklatın **Düzenle parametreleri**, her zorunlu parametreler için değerler sağlayın ve tıklayın **Tamam**.  Parametreleri aşağıdaki gibidir:
   
   1. SITENAME: App Service web uygulaması adı belirtir ve web uygulamasının erişmek için kullanacağınız URL oluşturmak için kullanılır (örneğin "mydemodocdbwebapp" belirttiğiniz sonra mydemodocdbwebapp.azurewebsites.net olarak, erişim web uygulaması URL'si olacaktır).
   2. HOSTINGPLANNAME: App Service barındırma planı oluşturmak için adını belirtir.
   3. KONUMU: Azure Cosmos DB ve web uygulaması kaynak oluşturmak Azure konumu belirtir.
   4. DATABASEACCOUNTNAME: oluşturmak için Azure Cosmos DB hesabının adını belirtir.   
      
      ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment4.png)
5. Varolan bir kaynak grubu seçin veya yeni bir kaynak grubu yapmak için bir ad girin ve kaynak grubu için bir konum seçin.

    ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment5.png)
6. Tıklatın **yasal koşulları gözden geçir**, **satın alma**ve ardından **oluşturma** dağıtımına başlamak için.  Seçin **panoya Sabitle** elde edilen dağıtımı Azure portal giriş sayfasında kolayca görünür olacak şekilde.
   ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment6.png)
7. Dağıtım tamamlandığında, kaynak grubu dikey penceresi açılır.
   ![Kaynak grubu dikey penceresinin ekran görüntüsü](./media/create-website/TemplateDeployment7.png)  
8. Uygulamayı kullanmak için yalnızca web uygulaması URL'ye gidin (yukarıdaki örnekte, URL http://mydemodocdbwebapp.azurewebsites.net olur).  Şu web uygulaması görürsünüz:
   
   ![Örnek Todo uygulaması](./media/create-website/image2.png)
9. Devam edin ve görevler çeşitli web uygulaması oluşturma ve Azure Portal'daki kaynak grubu dikey penceresine dönün. Kaynaklar listesinde Azure Cosmos DB hesap kaynak'ı tıklatın ve ardından **sorgu Gezgini**.
    ![Özet ekran görüntüsünde vurgulanmış web uygulaması ile Mercek](./media/create-website/TemplateDeployment8.png)  
10. Varsayılan sorguyu çalıştırmak "seçin * c" ve sonuçları inceleyin.  Sorgu alınan JSON gösterimi, yukarıdaki 7. adımda oluşturulan Yapılacaklar öğelerini olduğunu dikkat edin.  Deneme sorgularıyla çekinmeyin; Örneğin, SELECT çalıştırmayı deneyin * c WHERE c.isComplete = tamamlandı olarak işaretlenmiş tüm yapılacaklar öğelerini döndürmek için true.
    
    ![Sorgu sonuçları gösteren sorgu Gezgini ve sonuçları dikey ekran görüntüsü](./media/create-website/image5.png)
11. Örnek Todo uygulaması değiştirmek veya Azure Cosmos DB portal deneyimi keşfedin çekinmeyin.  Hazır olduğunuzda, şirketinizdeki başka bir şablonu dağıtın.

<a id="Build"></a> 

## <a name="step-3-deploy-the-document-account-and-web-app-sample"></a>3. adım: belge hesabı ve web uygulaması örneği dağıtma
Şimdi şimdi bizim ikinci şablonunu dağıtın.  Bu şablon nasıl, uygulama ayarları veya özel bağlantı dizesi olarak bir web uygulamasına Azure Cosmos DB bağlantısı bilgilerini hesap uç noktası ve ana anahtar gibi ekleyemezsiniz göstermek yararlıdır. Örneğin, bir Azure Cosmos DB hesabıyla dağıtmak ve dağıtım sırasında otomatik olarak doldurulur bağlantı bilgilerini istediğiniz kendi web uygulaması sahip belki de.

> [!TIP]
> Şablon aşağıda girilen Azure Cosmos DB hesap adını ve web uygulaması adı bir) geçerli ve (b) kullanılabilir olduğunu doğrulamaz.  Dağıtım göndermeden önce sağlamak için plan adları kullanılabilirliğini doğrulamanız önerilir.
> 
> 

1. İçinde [Azure Portal](https://portal.azure.com), yeni'yi tıklatın ve "Şablon dağıtımı" için arama yapın.
    ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment1.png)
2. Şablon dağıtımı öğeyi seçin ve tıklatın **oluşturma** ![şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment2.png)
3. Tıklatın **Düzen şablonu**DocDBWebSite.json şablon dosyasının içeriğini yapıştırın ve tıklatın **kaydetmek**.
   ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment3.png)
4. Tıklatın **Düzenle parametreleri**, her zorunlu parametreler için değerler sağlayın ve tıklayın **Tamam**.  Parametreleri aşağıdaki gibidir:
   
   1. SITENAME: App Service web uygulaması adı belirtir ve web uygulamasının erişmek için kullanacağınız URL oluşturmak için kullanılır (örneğin "mydemodocdbwebapp" belirttiğiniz sonra mydemodocdbwebapp.azurewebsites.net olarak, erişim web uygulaması URL'si olacaktır).
   2. HOSTINGPLANNAME: App Service barındırma planı oluşturmak için adını belirtir.
   3. KONUMU: Azure Cosmos DB ve web uygulaması kaynak oluşturmak Azure konumu belirtir.
   4. DATABASEACCOUNTNAME: oluşturmak için Azure Cosmos DB hesabının adını belirtir.   
      
      ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment4.png)
5. Varolan bir kaynak grubu seçin veya yeni bir kaynak grubu yapmak için bir ad girin ve kaynak grubu için bir konum seçin.

    ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment5.png)
6. Tıklatın **yasal koşulları gözden geçir**, **satın alma**ve ardından **oluşturma** dağıtımına başlamak için.  Seçin **panoya Sabitle** elde edilen dağıtımı Azure portal giriş sayfasında kolayca görünür olacak şekilde.
   ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment6.png)
7. Dağıtım tamamlandığında, kaynak grubu dikey penceresi açılır.
   ![Kaynak grubu dikey penceresinin ekran görüntüsü](./media/create-website/TemplateDeployment7.png)  
8. Web uygulaması kaynak kaynaklar listesinde tıklayın ve ardından **uygulama ayarları** ![kaynak grubunun ekran görüntüsü](./media/create-website/TemplateDeployment9.png)  
9. Nasıl uygulama ayarları Azure Cosmos DB uç noktası ve her Azure Cosmos DB ana anahtarları için mevcut dikkat edin.

    ![Uygulama ayarları ekran görüntüsü](./media/create-website/TemplateDeployment10.png)  
10. Azure Portal keşfetmeye devam etmek çekinmeyin veya bizim Azure Cosmos DB birini izleyin [örnekleri](http://go.microsoft.com/fwlink/?LinkID=402386) kendi Azure Cosmos DB uygulaması oluşturmak için.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! Azure Cosmos DB, App Service web uygulaması ve Azure Resource Manager şablonları kullanarak örnek bir web uygulamasına dağıttıktan sonra.

* Azure Cosmos DB hakkında daha fazla bilgi edinmek için tıklayın [burada](http://azure.com/docdb).
* Azure App Service Web apps hakkında daha fazla bilgi edinmek için tıklayın [burada](http://go.microsoft.com/fwlink/?LinkId=325362).
* Azure Resource Manager şablonları hakkında daha fazla bilgi edinmek için tıklayın [burada](https://msdn.microsoft.com/library/azure/dn790549.aspx).

## <a name="whats-changed"></a>Yapılan değişiklikler
* Web Sitelerinden App Service’e kadar değiştirme kılavuzu için bkz. [Azure App Service ve Mevcut Azure Hizmetlerine Etkileri](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](http://go.microsoft.com/fwlink/?LinkId=523751) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 

