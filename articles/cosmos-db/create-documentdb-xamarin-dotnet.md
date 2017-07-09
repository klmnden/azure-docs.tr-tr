---
title: "Azure Cosmos DB: Xamarin ve Facebook kimlik doğrulaması ile web uygulaması derleme | Microsoft Docs"
description: "Azure Cosmos DB’ye bağlanmak ve veritabanını sorgulamak için kullanabileceğiniz bir .NET kod örneği sunar"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: 4ea97c2aca6769843d0210ffeae6f95531a21f10
ms.contentlocale: tr-tr
ms.lasthandoff: 06/01/2017


---
# <a name="azure-cosmos-db-build-a-web-app-with-net-xamarin-and-facebook-authentication"></a>Azure Cosmos DB: .NET, Xamarin ve Facebook kimlik doğrulaması ile web uygulaması derleme

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıç belgesinde Azure portalı kullanarak bir Azure Cosmos DB hesabını, belge veritabanını ve koleksiyonunu nasıl oluşturacağınız anlatılmıştır. Daha sonra [DocumentDB .NET API](documentdb-sdk-dotnet.md), [Xamarin](https://www.xamarin.com/) ve Azure Cosmos DB yetkilendirme altyapısı üzerinde oluşturulmuş bir yapılacaklar listesi web uygulaması derleyip dağıtacaksınız. Yapılacaklar listesi web uygulaması, kullanıcıların Facebook Kimlik Doğrulaması kullanarak oturum açmasına ve kendi yapılacaklar listesi öğelerini yönetmesine olanak tanır.

## <a name="prerequisites"></a>Ön koşullar

Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Koleksiyon ekleme

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir DocumentDB API uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Git bash gibi bir git terminal penceresi açın ve `cd` ile çalışma dizinine gidin.  

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. 

    ```bash
    git clone https://github.com/Azure/azure-documentdb-dotnet.git
    ```

3. Ardından Visual Studio’nun samples/xamarin/UserItems/xamarin.forms klasöründen DocumentDBTodo.sln dosyasını açın. 

## <a name="review-the-code"></a>Kodu gözden geçirin

Xamarin klasöründeki kod şunları içerir:

* Xamarin uygulaması. Uygulama, kullanıcının yapılacaklar listesi öğelerini UserItems adlı bölümlendirilmiş bir koleksiyonda depolar.
* Kaynak belirteci aracı API’si. Azure Cosmos DB kaynak belirteçleri ile uygulamada oturum açmış kullanıcılar arasında aracılık yapan basit bir ASP.NET Web API. Kaynak belirteçleri, uygulamaya oturum açmış kullanıcının verilerini sağlayan kısa süreli erişim belirteçleridir.

Kimlik doğrulaması ve veri akışı aşağıdaki diyagramda gösterilmiştir.

* UserItems koleksiyonu '/userid' bölüm anahtarı ile oluşturulur. Bir koleksiyon için bölüm anahtarı belirtilmesi, Azure Cosmos DB’nin kullanıcı ve öğe sayısı arttıkça sonsuz ölçeklendirme yapmasını sağlar.
* Xamarin uygulaması, kullanıcıların Facebook kimlik bilgileriyle oturum açmasına olanak tanır.
* Xamarin uygulaması, ResourceTokenBroker API’si ile kimlik doğrulaması yapmak için Facebook erişim belirtecini kullanır
* Kaynak belirteci aracı API’si, App Service Kimlik Doğrulama özelliğini kullanarak isteğin kimliğini doğrular ve kimliği doğrulanmış kullanıcının bölüm anahtarını paylaşan tüm belgelere okuma/yazma erişimine sahip bir Azure Cosmos DB kaynak belirteci ister.
* Kaynak belirteci aracısı, kaynak belirtecini istemci uygulamasına döndürür.
* Uygulama, kaynak belirtecini kullanarak kullanıcının yapılacaklar listesi öğelerine erişir.

![Yapılacaklar listesi uygulaması ve örnek veriler](./media/create-documentdb-xamarin-dotnet/tokenbroker.png)
    
## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. [Azure portalında](http://portal.azure.com/), Azure Cosmos DB hesabınızın sol taraftaki gezinti menüsünden **Anahtarlar**'a ve ardından **Okuma/Yazma Anahtarları**'na tıklayın. Ekranın sağ tarafındaki kopyalama düğmelerini kullanarak URI ve Birincil Anahtar değerlerini kopyalayarak sonraki adımda Web.config dosyasına yapıştırın.

    ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar dikey penceresi](./media/create-documentdb-xamarin-dotnet/keys.png)

2. Visual Studio 2017’de, azure-documentdb-dotnet/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker klasöründeki Web.config dosyasını açın. 

3. Portaldaki URI değerinizi kopyalayın (kopyalama düğmesini kullanarak) ve Web.config dosyasına accountUrl değeri olarak yapıştırın. 

    `<add key="accountUrl" value="{Azure Cosmos DB account URL}"/>`

4. Ardından portaldaki BİRİNCİL ANAHTAR değerinizi kopyalayıp Web.config dosyasına accountKey değeri olarak yapıştırın. 

    `<add key="accountKey" value="{Azure Cosmos DB secret}"/>`

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="build-and-deploy-the-web-app"></a>Web uygulaması derleme ve dağıtma

1. Azure portalında Kaynak belirteci aracı API’sini barındıracak bir App Service web sitesi oluşturun.
2. Azure portalında, Kaynak belirteci aracı API’si web sitesinin Uygulama Ayarları dikey penceresini açın. Aşağıdaki uygulama ayarlarını doldurun:

    * accountUrl - Azure Cosmos DB hesabınızın Anahtarlar sekmesindeki Azure Cosmos DB hesap URL’si.
    * accountKey - Azure Cosmos DB hesabınızın Anahtarlar sekmesindeki Azure Cosmos DB hesabı ana anahtarı.
    * Oluşturduğunuz veritabanı ve koleksiyonun databaseId ile collectionId değerleri

3. Oluşturduğunuz web sitesinde ResourceTokenBroker çözümünü yayımlayın.

4. Xamarin projesini açıp TodoItemManager.cs dosyasına gidin. Kaynak belirteci aracı web sitesi için accountURL, collectionId, databaseId değerlerinin yanı sıra taban https url’si olarak resourceTokenBrokerURL değerini girin.

5. Facebook kimlik doğrulamasını ayarlamak ve ResourceTokenBroker web sitesini yapılandırmak için [App Service uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md) öğreticisini tamamlayın.

    Xamarin uygulamasını çalıştırın.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin: 

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından yeni oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak koleksiyon oluşturmayı ve bir Xamarin uygulaması derleyip dağıtmayı öğrendiniz. Şimdi Cosmos DB hesabınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)

