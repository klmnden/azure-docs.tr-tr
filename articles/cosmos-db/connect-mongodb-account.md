---
title: Azure Cosmos DB hesabı için MongoDB bağlantı dizesi
description: MongoDB bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına MongoDB uygulamanızı hesabınıza bağlanmayı öğreneceksiniz.
keywords: mongodb bağlantı dizesi
services: cosmos-db
author: slyons
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: conceptual
ms.date: 12/19/2017
ms.author: sclyon
ms.openlocfilehash: a78a77e16e9a810c0be03656aa48b02cc8e6e5e6
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52849267"
---
# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a>Bir MongoDB uygulamasını Azure Cosmos DB'ye bağlanma
MongoDB bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına MongoDB uygulamanızı hesabınıza bağlanmayı öğreneceksiniz. Verileri bir Azure Cosmos DB veritabanı kullanabilirsiniz, MongoDB uygulamanız için depolama. 

Bu öğreticide, bağlantı dizesi bilgilerini almak için iki yol sunar:

- [Hızlı Başlangıç yöntemi](#QuickstartConnection), .NET, Node.js, MongoDB kabuğu, Java ve Python sürücüleri ile kullanmak için
- [Özel bağlantı dizesi yöntemi](#GetCustomConnection), diğer sürücüleri ile kullanmak için

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure hesabı. Azure hesabınız yoksa, oluşturun bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/) şimdi. 
- Bir Azure Cosmos DB hesabı. Yönergeler için [.NET ve Azure portalıyla bir MongoDB API'si web uygulaması derleme](create-mongodb-dotnet.md).

## <a id="QuickstartConnection"></a>Hızlı Başlangıç'ı kullanarak MongoDB bağlantı dizesini alma
1. Bir Internet tarayıcısında oturum açın [Azure portalında](https://portal.azure.com).
2. İçinde **Azure Cosmos DB** dikey penceresinde, MongoDB hesabı için API'yi seçin. 
3. Hesap dikey penceresinin sol bölmesinde **Hızlı Başlangıç**. 
4. Platformunuzu seçin (**.NET**, **Node.js**, **MongoDB Kabuğu**, **Java**, **Python**). Sürücünüz veya aracınızı listede, endişelenmeyin--görmüyorsanız, biz sürekli bağlantı kod parçacıklarını belge. Lütfen aşağıda ne görmek istediğiniz üzerinde yorum. Kendi bağlantısı oluşturabilir öğrenmek için okuyun [hesabının bağlantı dizesi bilgilerini alın](#GetCustomConnection).
5. Kopyalayıp MongoDB uygulamanıza kod parçacığını yapıştırın.

    ![Hızlı Başlangıç dikey penceresi](./media/connect-mongodb-account/QuickStartBlade.png)

## <a id="GetCustomConnection"></a> Özelleştirme için MongoDB bağlantı dizesini alma
1. Bir Internet tarayıcısında oturum açın [Azure portalında](https://portal.azure.com).
2. İçinde **Azure Cosmos DB** dikey penceresinde, MongoDB hesabı için API'yi seçin. 
3. Hesap dikey penceresinin sol bölmesinde **bağlantı dizesi**. 
4. **Bağlantı dizesi** dikey penceresi açılır. Preconstructed bağlantı dizesi dahil olmak üzere MongoDB için bir sürücü kullanarak hesabına bağlanmak gerekli tüm bilgileri var.

    ![Bağlantı Dizesi dikey penceresi](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Bağlantı dizesi gereksinimleri
> [!Important]
> Azure Cosmos DB, katı güvenlik gereksinimleri ve standartları vardır. Azure Cosmos DB hesapları gerektiren kimlik doğrulaması ve güvenli bağlantı aracılığıyla *SSL*. 
>
>

Azure Cosmos DB destekleyen standart MongoDB bağlantı dizesi URI biçimi, birkaç belirli gereksinimleri: Azure Cosmos DB hesapları kimlik doğrulaması ve SSL ile güvenli iletişim gerektirir. Bu nedenle, bağlantı dizesi biçimi şöyledir:

    mongodb://username:password@host:port/[database]?ssl=true

Bu dize değerlerini kullanılabilir **bağlantı dizesi** daha önce gösterilen dikey penceresinde:

* Kullanıcı adı (gerekli): Azure Cosmos DB hesap adı.
* Parola (gerekli): Azure Cosmos DB hesap parolası.
* Ana bilgisayar (gerekli): Azure Cosmos DB hesabı'nın FQDN'si.
* Bağlantı noktası (gerekli): 10255 olarak.
* Veritabanı (isteğe bağlı): bağlantı kullanan veritabanı. Hiçbir veritabanı sağlanmazsa, varsayılan "test" veritabanıdır
* SSL = true (gerekli)

Örneğin, gösterilen hesap düşünün **bağlantı dizesi** dikey penceresi. Geçerli bir bağlantı dizesi verilmiştir:

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@contoso123.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Studio 3t'yi kullanma (MongoChef)](mongodb-mongochef.md) MongoDB hesabı için bir Azure Cosmos DB API ile.
* Azure Cosmos DB API görüntüleyerek MongoDB için keşfetmek [örnekleri](mongodb-samples.md).
