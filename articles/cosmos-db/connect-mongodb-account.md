---
title: Bir MongoDB uygulamasını Azure Cosmos DB'ye bağlanma
description: Azure Cosmos DB için MongoDB uygulamanızı hesabınıza bağlanmayı öğreneceksiniz.
author: rimman
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 12/26/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 737e179c2c16937d00bc9b6601f12ebe392c1906
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60892544"
---
# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a>Bir MongoDB uygulamasını Azure Cosmos DB'ye bağlanma
MongoDB bağlantı dizesi kullanarak bir Azure Cosmos DB için MongoDB uygulamanızı bağlayın öğrenin. Verileri bir Azure Cosmos DB veritabanı kullanabilirsiniz, MongoDB uygulamanız için depolama. 

Bu öğreticide, bağlantı dizesi bilgilerini almak için iki yol sunar:

- [Hızlı Başlangıç yöntemi](#QuickstartConnection), .NET, Node.js, MongoDB kabuğu, Java ve Python sürücüleri ile kullanmak için
- [Özel bağlantı dizesi yöntemi](#GetCustomConnection), diğer sürücüleri ile kullanmak için

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure hesabı. Azure hesabınız yoksa, oluşturun bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/) şimdi. 
- Bir Cosmos hesabı. Yönergeler için [MongoDB ve .NET SDK'sı için Azure Cosmos DB'nin API'sini kullanarak bir web uygulaması derleme](create-mongodb-dotnet.md).

## <a id="QuickstartConnection"></a>Hızlı Başlangıç'ı kullanarak MongoDB bağlantı dizesini alma
1. Bir Internet tarayıcısında oturum açın [Azure portalında](https://portal.azure.com).
2. İçinde **Azure Cosmos DB** dikey penceresinde, API'yi seçin. 
3. Hesap dikey penceresinin sol bölmesinde **Hızlı Başlangıç**. 
4. Platformunuzu seçin (**.NET**, **Node.js**, **MongoDB Kabuğu**, **Java**, **Python**). Sürücünüz veya aracınızı listede, endişelenmeyin--görmüyorsanız, biz sürekli bağlantı kod parçacıklarını belge. Lütfen aşağıda ne görmek istediğiniz üzerinde yorum. Kendi bağlantısı oluşturabilir öğrenmek için okuyun [hesabının bağlantı dizesi bilgilerini alın](#GetCustomConnection).
5. Kopyalayıp MongoDB uygulamanıza kod parçacığını yapıştırın.

    ![Hızlı Başlangıç dikey penceresi](./media/connect-mongodb-account/QuickStartBlade.png)

## <a id="GetCustomConnection"></a> Özelleştirme için MongoDB bağlantı dizesini alma
1. Bir Internet tarayıcısında oturum açın [Azure portalında](https://portal.azure.com).
2. İçinde **Azure Cosmos DB** dikey penceresinde, API'yi seçin. 
3. Hesap dikey penceresinin sol bölmesinde **bağlantı dizesi**. 
4. **Bağlantı dizesi** dikey penceresi açılır. Preconstructed bağlantı dizesi dahil olmak üzere MongoDB için bir sürücü kullanarak hesabına bağlanmak gerekli tüm bilgileri var.

    ![Bağlantı Dizesi dikey penceresi](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Bağlantı dizesi gereksinimleri
> [!Important]
> Azure Cosmos DB, katı güvenlik gereksinimleri ve standartları vardır. Azure Cosmos DB hesapları gerektiren kimlik doğrulaması ve güvenli bağlantı aracılığıyla *SSL*. 
>
>

Azure Cosmos DB, standart MongoDB bağlantı dizesi URI biçimi, birkaç belirli gereksinimleri destekler: Azure Cosmos DB hesapları, kimlik doğrulaması ve SSL aracılığıyla güvenli bağlantı gerekir. Bu nedenle, bağlantı dizesi biçimi şöyledir:

    mongodb://username:password@host:port/[database]?ssl=true

Bu dize değerlerini kullanılabilir **bağlantı dizesi** daha önce gösterilen dikey penceresinde:

* Kullanıcı adı (gerekli): Cosmos hesap adı.
* Parola (gerekli): Cosmos hesap parolası.
* Ana bilgisayar (gerekli): Cosmos hesabı FQDN'si.
* Bağlantı noktası (gerekli): 10255.
* Veritabanı (isteğe bağlı): Veritabanı bağlantı kullanır. Hiçbir veritabanı sağlanmazsa, varsayılan "test" veritabanıdır
* SSL = true (gerekli)

Örneğin, gösterilen hesap düşünün **bağlantı dizesi** dikey penceresi. Geçerli bir bağlantı dizesi verilmiştir:

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@contoso123.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Studio 3T kullanma](mongodb-mongochef.md) Azure Cosmos DB'nin MongoDB API'si ile.
- Bilgi edinmek için nasıl [Robo 3T kullanma](mongodb-robomongo.md) Azure Cosmos DB'nin MongoDB API'si ile.
- MongoDB keşfedin [örnekleri](mongodb-samples.md) Azure Cosmos DB'nin MongoDB API'si ile.
