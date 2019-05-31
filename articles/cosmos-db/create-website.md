---
title: Bir şablon - Azure Cosmos DB ile bir web uygulaması dağıtma
description: Azure Cosmos DB hesabı, Azure App Service Web Apps ve Azure Resource Manager şablonu kullanarak örnek bir web uygulamasına dağıtmayı öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: sngun
ms.openlocfilehash: 93cdea453050df8899abf9233991715ae237bcd4
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257231"
---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>Azure Cosmos DB ile bir Azure Resource Manager şablonu kullanarak Azure App Service Web Apps'e dağıtın
Bu öğretici bir Azure Resource Manager şablonu dağıtma ve tümleştirme için nasıl kullanılacağını gösterir [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)e [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) web uygulaması ve bir örnek web uygulaması.

Azure Resource Manager şablonlarını kullanarak, dağıtımını ve yapılandırmasını, Azure kaynaklarınızın kolayca otomatikleştirebilirsiniz.  Bu öğreticide, bir web uygulamasını dağıtma ve otomatik olarak Azure Cosmos DB hesabı bağlantı bilgilerini yapılandırma gösterilmektedir.

Bu öğreticiyi tamamladıktan sonra aşağıdaki soruları yanıtlamak mümkün olacaktır:  

* Bir Azure Resource Manager şablonu dağıtma ve Azure Cosmos DB hesabı ve Azure App Service'te bir web uygulaması tümleştirme için nasıl kullanabilirim?
* Bir Azure Resource Manager şablonu dağıtma ve Azure Cosmos DB hesabı, bir web uygulamasını App Service Web Apps ve Webdeploy uygulama tümleştirme için nasıl kullanabilirim?

<a id="Prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar
> [!TIP]
> Bu öğreticide, Azure Resource Manager şablonları ya da JSON deneyiminiz olmasa varsaymaz olsa da başvurulan şablonları veya dağıtım seçenekleri değiştirmek istemeniz durumunda sonra bu alanların her birini bilgisi gereklidir.
> 
> 

Bu öğreticide yönergeleri izlemeden önce sahip olduğunuzdan emin olun Azure aboneliği. Azure abonelik tabanlı bir platformdur.  Bir aboneliği edinme hakkında daha fazla bilgi için bkz. [satın alma seçenekleri](https://azure.microsoft.com/pricing/purchase-options/), [üye Tekliflerimizi](https://azure.microsoft.com/pricing/member-offers/), veya [ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/).

## <a id="CreateDB"></a>1. adım: Şablon dosyalarını indirme
Bu öğretici şablon dosyaları yükleyerek başlayalım.

1. İndirme [Web uygulamaları, bir Azure Cosmos DB hesabı oluşturma ve bir tanıtım uygulaması örneği dağıtma](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) şablonu yerel bir klasöre (örneğin, C:\Azure Cosmos DBTemplates). Bu şablon, bir Azure Cosmos DB hesabı, bir App Service web uygulaması ve bir web uygulaması dağıtır.  Azure Cosmos DB hesabına bağlanmak için web uygulaması da otomatik olarak yapılandırır.
2. İndirme [bir Azure Cosmos DB hesabı ve Web uygulamaları örnek oluşturma](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) şablonu yerel bir klasöre (örneğin, C:\Azure Cosmos DBTemplates). Bu şablon, bir Azure Cosmos DB hesabı, bir App Service web uygulaması dağıtır ve sitenin uygulama ayarlarını kolayca yüzey Azure Cosmos DB bağlantı bilgilerini değiştirir ancak bir web uygulaması içermez.  

<a id="Build"></a>

## <a name="step-2-deploy-the-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>2. adım: Azure Cosmos DB hesabı, App Service web uygulaması ve tanıtım uygulaması örneği dağıtma
Artık ilk şablonunuzu dağıtalım.

> [!TIP]
> Şablon aşağıdaki şablonda girdiğiniz Azure Cosmos DB hesabı adını ve web uygulaması adı geçerli bir) ve (b) kullanılabilir olduğunu doğrulamaz.  Dağıtım gönderiliyor önce sağlamak için plan adları kullanılabilirliğini doğrulamak önerilir.
> 
> 

1. Oturum açma [Azure portalı](https://portal.azure.com), yeni tıklayın ve "Şablon dağıtımı için" için arama yapın.
    ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment1.png)
2. Şablonu dağıtım öğesi'ni seçip tıklayın **Oluştur** ![şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment2.png)
3. Tıklayın **şablonu Düzen**DocDBWebsiteTodo.json şablon dosyasının içeriğini yapıştırın ve tıklayın **Kaydet**.
   ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment3.png)
4. Tıklayın **parametreleri Düzenle**, her zorunlu parametreler için değerler sağlayın ve tıklayın **Tamam**.  Parametreleri aşağıdaki gibidir:
   
   1. SİTE ADI: App Service web uygulaması adını belirtir ve web uygulaması (örneğin, "mydemodocdbwebapp", ardından web uygulaması tarafından erişim URL'si olan mydemodocdbwebapp.azurewebsites.net belirtirseniz) erişmek için kullandığınız URL'yi oluşturmak için kullanılır.
   2. HOSTINGPLANNAME: App Service barındırma planı oluşturmak için adını belirtir.
   3. KONUM: Azure Cosmos DB ile web uygulaması kaynakları oluşturmak Azure konumu belirtir.
   4. DATABASEACCOUNTNAME: Azure Cosmos DB hesabı oluşturmak için adını belirtir.   
      
      ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment4.png)
5. Mevcut bir kaynak grubu seçin veya yeni bir kaynak grubu için bir ad belirtin ve kaynak grubu için bir konum seçin.

    ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment5.png)
6. Tıklayın **yasal koşulları gözden geçir**, **satın alma**ve ardından **Oluştur** dağıtımına başlamak için.  Seçin **panoya Sabitle** için ortaya çıkan dağıtım, Azure portal giriş sayfasında kolayca görülebilir.
   ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment6.png)
7. Dağıtım tamamlandığında kaynak grubu bölmesi açılır.
   ![Kaynak grubu bölmesinin ekran görüntüsü](./media/create-website/TemplateDeployment7.png)  
8. Uygulamayı kullanmak için web uygulaması URL'sine gidin. (yukarıdaki örnekte, URL şu şekilde olacaktır http://mydemodocdbwebapp.azurewebsites.net).  Aşağıdaki web uygulaması görürsünüz:
   
   ![Örnek TODO uygulaması](./media/create-website/image2.png)
9. Devam edin ve birkaç görev web uygulaması oluşturun ve ardından Azure portalında kaynak grubu bölmesine dönün. Azure Cosmos DB hesap kaynağında kaynaklar listesine tıklayın ve ardından **Veri Gezgini**.
10. Varsayılan sorguyu çalıştırma "seçin * c" ve sonuçları inceleyin.  Sorgu, yukarıdaki 7. adımda oluşturduğunuz todo öğelerini JSON temsili almıştır dikkat edin.  Deneme sorgularıyla çekinmeyin; Örneğin, SELECT çalıştıran deneyin * c WHERE c.isComplete = tamamlandı olarak işaretlenmiş tüm todo öğeleri döndürmek için true.
11. Azure Cosmos DB portal deneyimi keşfedin veya örnek Todo uygulaması değiştirmek çekinmeyin.  Hazır olduğunuzda, başka bir şablon dağıtalım.

<a id="Build"></a> 

## <a name="step-3-deploy-the-document-account-and-web-app-sample"></a>3. adım: Belge hesabı ve web uygulaması örneği dağıtma
Şimdi ikinci şablonunuzu dağıtalım.  Bu şablonu nasıl, uygulama ayarları veya özel bir bağlantı dizesi olarak web uygulamasıyla Azure Cosmos DB hesabınızın uç noktası ve ana anahtarı gibi bağlantı bilgilerini ekleyebilir göstermek kullanışlıdır. Örneğin, bir Azure Cosmos DB hesabı dağıtabiliyorum ve dağıtım sırasında otomatik olarak doldurulur bağlantı bilgilerini istediğiniz kendi web Uygulamam var belki de.

> [!TIP]
> Şablon, web uygulaması adı ve Azure Cosmos DB hesap adınızı aşağıda girilen bir) geçerli ve (b) kullanılabilir olduğunu doğrulamaz.  Dağıtım gönderiliyor önce sağlamak için plan adları kullanılabilirliğini doğrulamak önerilir.
> 
> 

1. İçinde [Azure portalı](https://portal.azure.com), yeni tıklayın ve "Şablon dağıtımı için" için arama yapın.
    ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment1.png)
2. Şablonu dağıtım öğesi'ni seçip tıklayın **Oluştur** ![şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment2.png)
3. Tıklayın **şablonu Düzen**DocDBWebSite.json şablon dosyasının içeriğini yapıştırın ve tıklayın **Kaydet**.
   ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment3.png)
4. Tıklayın **parametreleri Düzenle**, her zorunlu parametreler için değerler sağlayın ve tıklayın **Tamam**.  Parametreleri aşağıdaki gibidir:
   
   1. SİTE ADI: App Service web uygulaması adını belirtir ve (örneğin, "mydemodocdbwebapp", ardından web uygulaması tarafından erişim URL'si olan mydemodocdbwebapp.azurewebsites.net belirtirseniz), web uygulamasına erişmek için kullanacağı URL'yi oluşturmak için kullanılır.
   2. HOSTINGPLANNAME: App Service barındırma planı oluşturmak için adını belirtir.
   3. KONUM: Azure Cosmos DB ile web uygulaması kaynakları oluşturmak Azure konumu belirtir.
   4. DATABASEACCOUNTNAME: Azure Cosmos DB hesabı oluşturmak için adını belirtir.   
      
      ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment4.png)
5. Mevcut bir kaynak grubu seçin veya yeni bir kaynak grubu için bir ad belirtin ve kaynak grubu için bir konum seçin.

    ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment5.png)
6. Tıklayın **yasal koşulları gözden geçir**, **satın alma**ve ardından **Oluştur** dağıtımına başlamak için.  Seçin **panoya Sabitle** için ortaya çıkan dağıtım, Azure portal giriş sayfasında kolayca görülebilir.
   ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment6.png)
7. Dağıtım tamamlandığında kaynak grubu bölmesi açılır.
   ![Kaynak grubu bölmesinin ekran görüntüsü](./media/create-website/TemplateDeployment7.png)  
8. Web uygulaması kaynağı kaynaklar listesinde tıklatın ve ardından **uygulama ayarları** ![kaynak grubunun ekran görüntüsü](./media/create-website/TemplateDeployment9.png)  
9. Nasıl uygulama ayarları, Azure Cosmos DB uç noktası ve her bir Azure Cosmos DB ana anahtarı için mevcut dikkat edin.

    ![Uygulama ayarları görüntüsü](./media/create-website/TemplateDeployment10.png)  
10. Azure Portal'ı keşfetmeye devam etmek rahatça veya bizim Azure Cosmos DB birini izleyin [örnekleri](https://go.microsoft.com/fwlink/?LinkID=402386) kendi Azure Cosmos DB uygulaması oluşturmak için.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! Azure Cosmos DB, App Service web uygulaması ve Azure Resource Manager şablonlarını kullanarak örnek bir web uygulamasına dağıttınız.

* Azure Cosmos DB hakkında daha fazla bilgi edinmek için tıklayın [burada](https://azure.microsoft.com/services/cosmos-db/).
* Azure App Service Web apps hakkında daha fazla bilgi edinmek için tıklayın [burada](https://go.microsoft.com/fwlink/?LinkId=325362).
* Azure Resource Manager şablonları hakkında daha fazla bilgi edinmek için tıklayın [burada](https://msdn.microsoft.com/library/azure/dn790549.aspx).

## <a name="whats-changed"></a>Yapılan değişiklikler
* Web sitelerinden App Service'e kadar değiştirme kılavuzu için bkz: [Azure App Service ve mevcut Azure hizmetlerine etkileri](https://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://go.microsoft.com/fwlink/?LinkId=523751) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 

