---
title: Bir Azure Cosmos DB hesaba MongoDB bağlantı dizesini | Microsoft Docs
description: MongoDB bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına MongoDB uygulamanızı bağlayacağınızı öğrenin.
keywords: mongodb bağlantı dizesi
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: conceptual
ms.date: 12/19/2017
ms.author: sngun
ms.openlocfilehash: 441f6725b96dc7a099304e06db6d46a65e7f9c81
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34795967"
---
# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a>Azure Cosmos DB MongoDB uygulamaya bağlama
MongoDB bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına MongoDB uygulamanızı bağlayacağınızı öğrenin. Daha sonra verileri olarak Azure Cosmos DB veritabanı kullanabilirsiniz MongoDB uygulamanız için deposu. 

Bu öğretici, bağlantı dizesi bilgilerini almak için iki yöntem sunar:

- [Hızlı Başlangıç yöntemi](#QuickstartConnection), .NET, Node.js, MongoDB Kabuk, Java ve Python sürücüleri ile kullanmak için
- [Özel bağlantı dizesi yöntemi](#GetCustomConnection), diğer sürücülerin ile kullanmak için

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure hesabı. Bir Azure hesabınız yoksa, oluşturma bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/) şimdi. 
- Bir Azure Cosmos DB hesabı. Yönergeler için bkz: [.NET ve Azure portal ile bir MongoDB API web uygulaması oluşturma](create-mongodb-dotnet.md).

## <a id="QuickstartConnection"></a>Hızlı Başlangıç'ı kullanarak MongoDB bağlantı dizesi alma
1. Bir Internet tarayıcısında oturum [Azure portal](https://portal.azure.com).
2. İçinde **Azure Cosmos DB** dikey penceresinde API MongoDB hesabı seçin. 
3. Hesap dikey pencerenin sol bölmede tıklatın **Hızlı Başlangıç**. 
4. Platformunuzu seçin (**.NET**, **Node.js**, **MongoDB Kabuk**, **Java**, **Python**). Sürücü veya listelenen, aracı endişelenmeyin--görmüyorsanız, biz sürekli olarak daha fazla bağlantı kod parçacıkları belge. Lütfen aşağıda ne görmek istediğiniz üzerinde açıklama. Kendi bağlantısı oluşturabilir öğrenmek için okuma [hesabın bağlantı dizesi bilgilerini al](#GetCustomConnection).
5. Kopyalayıp MongoDB uygulamanıza kod parçacığını yapıştırın.

    ![Hızlı Başlangıç dikey penceresi](./media/connect-mongodb-account/QuickStartBlade.png)

## <a id="GetCustomConnection"></a> Özelleştirme için MongoDB bağlantı dizesi alma
1. Bir Internet tarayıcısında oturum [Azure portal](https://portal.azure.com).
2. İçinde **Azure Cosmos DB** dikey penceresinde API MongoDB hesabı seçin. 
3. Hesap dikey pencerenin sol bölmede tıklatın **bağlantı dizesi**. 
4. **Bağlantı dizesi** dikey pencere açılır. Hesabınıza preconstructed bağlantı dizesi dahil olmak üzere MongoDB için bir sürücü kullanarak bağlanmak için gereken tüm bilgileri içeriyor.

    ![Bağlantı dizesi dikey penceresi](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Bağlantı dizesi gereksinimleri
> [!Important]
> Azure Cosmos DB sıkı güvenlik gereksinimlerine ve standartları vardır. Azure Cosmos DB hesapların gerektirdiği kimlik doğrulaması ve güvenli iletişim aracılığıyla *SSL*. 
>
>

Azure Cosmos DB destekleyen standart MongoDB bağlantı dizesi URI biçimi, birkaç belirli gereksinimleri: Azure Cosmos DB hesapları, kimlik doğrulama ve SSL üzerinden güvenli iletişim gerektirir. Bu nedenle, bağlantı dizesi biçimi şöyledir:

    mongodb://username:password@host:port/[database]?ssl=true

Bu dize değerlerini mevcuttur **bağlantı dizesi** daha önce gösterilen dikey penceresinde:

* Kullanıcı adı (gerekli): Azure Cosmos DB hesap adı.
* Parola (gerekli): Azure Cosmos DB hesap parolası.
* Ana bilgisayar (gerekli): Azure Cosmos DB hesabı'nın FQDN'si.
* Bağlantı noktası (gerekli): 10255 değerini bulur.
* Veritabanı (isteğe bağlı): bağlantı kullanan veritabanı. Hiçbir veritabanı sağladıysanız varsayılan veritabanı "test".
* SSL = true (gerekli)

Örneğin, gösterilen hesap göz önünde bulundurun **bağlantı dizesi** dikey. Geçerli bir bağlantı dizesi şöyledir:

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@contoso123.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Studio 3T kullanın (MongoChef)](mongodb-mongochef.md) Azure Cosmos DB API MongoDB hesabı ile.
* Azure Cosmos DB API MongoDB için görüntüleyerek keşfedin [örnekleri](mongodb-samples.md).
