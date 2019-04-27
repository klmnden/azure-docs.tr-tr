---
title: Azure için MongoDB, React ve Node.js Öğreticisi
description: Video temelli bu öğretici serisiyle Azure Cosmos DB üzerinde React ve Node ile MongoDB için kullandığınız aynı API'leri kullanarak bir MongoDB uygulaması oluşturmayı öğrenirsiniz.
author: johnpapa
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/05/2017
ms.author: jopapa
ms.reviewer: sngun
ms.openlocfilehash: ef245c2ef5da2b3dc9780e772a71e8ef896e1aff
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60824278"
---
# <a name="create-a-mongodb-app-with-react-and-azure-cosmos-db"></a>React ve Azure Cosmos DB ile bir MongoDB uygulaması oluşturma  

Bu çok parçalı videolu öğretici, React ön ucuyla hero izleme uygulaması oluşturmayı gösterir. Uygulama, Node ve Express sunucusu için kullanılan, Cosmos veritabanı ile yapılandırılmış bağlandığı [Azure Cosmos DB'nin MongoDB API'si](mongodb-introduction.md)ve ardından React ön ucunu uygulamanın sunucu bölümüne bağlar. Öğreticinin ayrıca nasıl üzerine ve tıklama Cosmos DB Azure portalında ölçeklendirme yapın ve herkesin, sık kullanılan hero'ları izlemek için uygulamayı internet'e dağıtmak nasıl gösterir. 

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) destekleyen Azure Cosmos DB MongoDB yerine kullanmak istemcileri etkinleştirme ile MongoDB, protokol uyumluluk bağlayabilirsiniz.  

Bu çok bölümlü öğretici aşağıdaki görevleri içerir:

> [!div class="checklist"]
> * Giriş
> * Projeyi ayarlama
> * React ile kullanıcı arabirimini oluşturma
> * Azure Portal’ı kullanarak Azure Cosmos DB hesabı oluşturma
> * Azure Cosmos DB’ye bağlanmak için Mongoose kullanma
> * Uygulamaya Yanıt Verme, Güncelleştirme ve Silme işlemleri ekleme

Aynı uygulamayı Angular ile oluşturmak ister misiniz? Bkz. [Angular öğretici video serisi](tutorial-develop-mongodb-nodejs.md).

## <a name="prerequisites"></a>Önkoşullar
* [Node.js](https://www.nodejs.org)

### <a name="finished-project"></a>Tamamlanmış Proje
Tamamlanmış uygulamayı [github'dan](https://github.com/Azure-Samples/react-cosmosdb) alabilirsiniz.

## <a name="introduction"></a>Giriş 

Bu videoda, Burke Holland Azure Cosmos DB’yi tanıtır ve bu video serisindeki uygulamayı oluşturma konusunda size yol gösterir. 

> [!VIDEO https://www.youtube.com/embed/58IflnJbYJc]

## <a name="project-setup"></a>Proje ayarları

Bu videoda aynı projede Express ve React’in nasıl ayarlanacağı gösterilmiştir. Burke, daha sonra da projedeki kodları açıklar.

> [!VIDEO https://www.youtube.com/embed/ytFUPStJJds]

## <a name="build-the-ui"></a>Kullanıcı Arabirimini oluşturma

Bu videoda React ile uygulamanın kullanıcı arabirimini (UI) nasıl oluşturacağınız gösterilmiştir. 

> [!NOTE]
> Bu videoda başvurulan CSS [react-cosmosdb GitHub deposunda](https://github.com/Azure-Samples/react-cosmosdb/blob/master/src/index.css) bulunabilir.

> [!VIDEO https://www.youtube.com/embed/SzHzX0fTUUQ]

## <a name="connect-to-azure-cosmos-db"></a>Azure Cosmos DB’ye bağlanma

Bu videoda, Azure portalında bir Azure Cosmos DB hesabı oluşturma, MongoDB ve Mongoose paketlerini yükleme ve ardından uygulamayı Azure Cosmos DB bağlantı dizesini kullanarak yeni oluşturulan hesapla bağlama gösterilmiştir. 

> [!VIDEO https://www.youtube.com/embed/0U2jV1thfvs]

## <a name="read-and-create-heroes-in-the-app"></a>Uygulamada hero'ları okuma ve oluşturma

Bu video, hero'ları okuyup Cosmos DB veritabanında hero'ları oluşturmayı, sonrasında da Postman ve React kullanıcı arabirimlerini kullanarak bu yöntemleri test etmeyi gösterir. 

> [!VIDEO https://www.youtube.com/embed/AQK9n_8fsQI] 

## <a name="delete-and-update-heroes-in-the-app"></a>Uygulamada hero'ları silme ve güncelleştirme

Bu video, uygulamada hero'ları silmeyi ve güncelleştirmeyi, ardından güncelleştirmeleri kullanıcı arabiriminde görüntülemeyi gösterir. 

> [!VIDEO https://www.youtube.com/embed/YmaGT7ztTQM] 

## <a name="complete-the-app"></a>Uygulamayı tamamlayın

Bu video, uygulamayı tamamlamayı ve kullanıcı arabirimini arka uç API'siyle bağlamayı gösterir. 

> [!VIDEO https://www.youtube.com/embed/TcSm2ISfTu8]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu öğretici tarafından oluşturulan tüm kaynakları silin: 

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * React, Node, Express ve Azure Cosmos DB ile uygulama oluşturma 
> * Azure Cosmos DB hesabı oluşturma
> * Uygulamayı Azure Cosmos DB hesabına bağlama
> * Uygulamayı Postman kullanarak test etme
> * Uygulamayı çalıştırma ve veritabanına hero'ları ekleme

Bir sonraki öğreticiye geçip MongoDB verilerini Azure Cosmos DB’de içeri aktarmayı öğrenin.  

> [!div class="nextstepaction"]
> [Azure Cosmos DB’ye MongoDB verileri aktarma](mongodb-migrate.md)
 
