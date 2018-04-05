---
title: Azure Cosmos DB için Visual Studio Bağlı Hizmeti
description: Geliştiricilerin Azure Cosmos DB hesabını kolayca bağlayarak kaynakları Visual Studio Bağlı Hizmetleri üzerinden yönetmesini sağlar
services: cosmos-db
documentationcenter: ''
author: jejiang
manager: DJ
+tags: cosmos-db
editor: Jenny Jiang
ms.assetid: ''
ms.service: cosmos-db
ms.custom: Azure Cosmos DB
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 09/19/2017
ms.author: jejiang
ms.openlocfilehash: 93be368d34f02e64d11abe9a04b11272ce18124d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-cosmos-db-visual-studio-connected-service-preview"></a>Azure Cosmos DB: Visual Studio Bağlı Hizmeti (Önizleme)

Visual Studio Bağlı Hizmetleri geliştiricilerin Azure Cosmos DB hesaplarını kolayca bağlamalarını ve kaynaklarını yönetmelerini sağlar.

Bağlı Hizmet içindeki Veri Gezgini'ni kullanarak ayrıca saklı yordamlar, UDF'ler ve tetikleyiciler oluşturabilir, bu sayede sunucu tarafı iş mantığını gerçekleştirebilirsiniz. Veri Gezgini, API'lerdeki tüm yerleşik programlı veri erişimini açığa çıkarır ancak verilerinize kolayca erişmenizi sağlar.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki öğelere sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz. 
* Bir Azure Cosmos DB hesabı. Hesabınız yoksa [Bir Azure Cosmos DB hesabı oluşturun](create-sql-api-dotnet.md) sayfasındaki adımları izleyerek Azure portalında hesap oluşturabilir veya [Bağlı Hizmet aracında bir Azure Cosmos DB hesabı oluşturma](#Create-an-Azure-Cosmo-DB-account-in-Connected-Service-tool) sayfasını inceleyebilirsiniz. 
* Geliştirme amacıyla yerel ortam kullanmak istiyorsanız [Azure Cosmos DB Öykünücüsü](local-emulator.md)'nü kullanabilirsiniz. Ortam Azure Cosmos DB hizmetine öykünür.
* [Visual Studio](http://www.visualstudio.com/).
* En son Azure Cosmos DB Bağlı Hizmeti bitleri. Azure Cosmos DB Bağlı Hizmetini aşağıdaki ekran görüntüsünde gösterilen şekilde Visual Studio Market'ten indirebilirsiniz. Bilgisayarınızda **Visual Studio**'yu açın. **Araçlar** menüsünde **Uzantılar ve Güncelleştirmeler...** öğesini seçin ve ardından **Çevrimiçi** / **Visual Studio Market**'i belirleyin. Bitleri aramak için **cosmosdb** yazın.

    Azure Cosmos DB Bağlı Hizmetini uygulamasını [Visual Studio Market](https://go.microsoft.com/fwlink/?linkid=858709)'ten de yükleyebilirsiniz.

    ![Bağlı Hizmet bitlerini indirme ekran görüntüsü.png](./media/connected-service/connected-service-downloadbits.png) 

Azure Cosmos DB Connected Service uzantısını indirdikten sonra, uzantıyı yüklemek için Visual Studio’yu kapatın.

## <a id="SetupVS"></a>Visual Studio çözümünüzü kurma
1. Bilgisayarınızda **Visual Studio**'yu açın.
2. **Dosya** menüsünde **Yeni**'yi seçin ve ardından **Proje**'yi seçin.
3. **Yeni Proje** iletişim kutusunda **Visual C#** / **Console Uygulaması (.NET Framework)** veya **WPF Uygulaması (.NET Framework)** öğesini seçin, projenize bir ad verin ve **Tamam**'a tıklayın.

    ![Yeni Proje penceresinin ekran görüntüsü](./media/connected-service/connected-service-new-project.png)
    
## <a name="add-connected-service-and-add-account"></a>Bağlı Hizmeti uygulamasını ve hesabı ekleme
1. Çözüm Gezgini'nde proje düğümüne sağ tıklayıp **Ekle** / **Bağlı Hizmet**'i seçin. Ayrıca **Proje** menüsüne tıklayıp **Bağlı Hizmet Ekle**'yi de seçebilirsiniz.

    ![Bağlı Hizmet Ekle penceresinin ekran görüntüsü](./media/connected-service/connected-service-add-connectedservice-rightclick.png)
2. Bağlı hizmet sayfasında **Bağlı Hizmetler** / **Azure Cosmos DB**'ye tıklayarak **Azure Cosmos DB** sayfasını açın.

    ![Azure Cosmos DB penceresinin ekran görüntüsü](./media/connected-service/connected-service-choose-azure-cosmosdb.png)
3. İlk kez oturum açmak veya hesap eklemek için aşağı oka tıklayın. Oturum açtıktan sonra tüm Azure Cosmos DB hesapları boş alanda gösterilir. Projenize eklemek istediğiniz Azure Cosmos DB hesabını seçin.

    ![Oturum açma adının ve listelenen db hesabı penceresinin ekran görüntüsü](./media/connected-service/connected-service-add-db-account.png)
4. Bir Azure Cosmos DB hesabını ekledikten sonra projeye bir Azure Cosmos DB hesabı bağlı hizmeti klasörü eklenir. Adım 1-3 arasını tekrarlayarak birden fazla Azure Cosmos DB hesabı ekleyebilirsiniz.

    ![Bağlı hizmet klasörü penceresinin ekran görüntüsü](./media/connected-service/connected-service-add-connectedservice-folder.png)

5. Bir Azure Cosmos DB bağlı hizmeti ekledikten sonra projenizi değiştirerek Azure Cosmos DB öğesini aşağıdaki şekillerden biriyle erişimi etkinleştirin:

    * Azure Cosmos DB istemcisinin ihtiyaç duyduğu NuGet paketlerinden bazıları yüklenmiştir. Bunları paket yapılandırma dosyalarınızda görebilirsiniz. 

        ![Yeni Azure Cosmos DB paketi yapılandırması](./media/connected-service/connected-service-packages-config.png)   
    
    * Azure Cosmos DB bağlantı uri ve anahtar değeri proje yapılandırma dosyasına eklenir (bu örnekte App.config). 

        ![Yeni Azure Cosmos DB uygulaması yapılandırması](./media/connected-service/connected-service-app-config.png) 

## <a name="open-azure-cosmos-db-explorer"></a>Azure Cosmos DB Gezgini'ni açın
1. Proje düğümüne sağ tıklayın ve **Cosmos DB Gezgini'ni Aç...** öğesine tıklayın.

    ![Açık Veri Gezgini penceresini açma girişiminin ekran görüntüsü](./media/connected-service/connected-service-right-click-open-data-exporer.png)
2. **Cosmos DB Hesabı Seç** sayfasında açılır listeye tıklayıp Azure Cosmos DB hesaplarından birini seçin.

    ![Bağlı Hizmet Hesabı Eklendi penceresinin ekran görüntüsü](./media/connected-service/connected-service-open-explorer.png)
3. **Açık** öğesine tıkladığınızda veri gezgini penceresi açılır.

## <a id="Create-an-Azure-Cosmo-DB-account-in-Connected-Service-tool"></a>Bağlı Hizmet aracında bir Azure Cosmos DB hesabı oluşturun
1. Bağlı hizmet sayfasında, bölmenin sol alt bölümünde yer alan **Yeni Cosmos DB Hesabı Oluştur**'a tıklayarak **Cosmos DB Hesabı Oluştur** sayfasını açın.

    ![Azure Cosmos DB Hesabı Oluştur giriş noktası penceresinin ekran görüntüsü](./media/connected-service/connected-service-click-new-db-account.png)
2. **Cosmos DB Hesabı Oluştur** sayfasında Azure Cosmos DB hesabınız için istediğiniz yapılandırmayı belirtin.

    Aşağıdaki ekran görüntüsünde yer alan bilgileri kullanarak **Cosmos DB Hesabı Oluştur** sayfasındaki alanları doldurun. 
 
    ![Yeni Azure Cosmos DB sayfası](./media/connected-service/connected-service-create-new-account.png)        
3. Hesabı oluşturmak için **Oluştur**’a tıklayın.

## <a name="use-data-explorer"></a>Veri Gezgini'ni kullanın

Veri Gezgini'ni açtıktan sonra aşağıdakileri yapabilirsiniz:
* Veritabanı oluşturma ve silme
* Koleksiyon oluşturma ve silme
* Belge oluşturma, silme ve filtreleme
* Saklı yordam oluşturma ve silme
* Sunucu tarafı iş mantığını gerçekleştirmek için tetikleyicileri ve kullanıcı tanımlı işlevleri oluşturma ve silme. 

![Yeni Azure Cosmos DB sayfası](./media/connected-service/connected-service-dataexplorerui.png)

## <a name="demo"></a>Tanıtım

Visual Studio'da Azure Cosmos DB Bağlı Hizmetini nasıl kullanacağınızı görmek için şu videoyu izleyin: [Visual Studio'da Azure Cosmos DB Bağlı Hizmetini kullanma](https://go.microsoft.com/fwlink/?linkid=858711)

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede aşağıdakileri öğrendiniz:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma
> * Bağlı Hizmeti uygulamasını ve hesabı ekleme
> * Azure Cosmos DB Gezgini'ni açın
> * Veri Gezgini'ni kullanın

Bağlı Hizmetleri Azure Cosmos DB hesabınızla birlikte çalışır hale getirdiniz. Aşağıdaki öğreticilerden birine geçerek çözümünüzü geliştirmeye başlayabilirsiniz:

* [.NET’te SQL API’siyle geliştirme](tutorial-develop-sql-api-dotnet.md).
* [Azure Cosmos DB: SQL API’yi kullanmaya başlama öğreticisi](sql-api-get-started.md).
* Azure Cosmos DB ile ölçek ve performans testi mi yapmak istiyorsunuz? Bkz. [Azure Cosmos DB ile Performans ve Ölçek Testi](performance-testing.md).
* [Azure Cosmos DB hesabını nasıl izleyebileceğinizi](monitor-accounts.md) öğrenin.

