---
title: Truffle kullanarak bağlanma
description: Truffle kullanarak bir Azure blok zinciri Service ağa bağlanma
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/29/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 8b1a701beac867c5f331ffa1ee1dee615961c6b3
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66416293"
---
# <a name="quickstart-use-truffle-to-connect-to-an-azure-blockchain-service-network"></a>Hızlı Başlangıç: Bir Azure blok zinciri Service ağa bağlanmak için Truffle kullanın

Truffle, bir Azure blok zinciri hizmet düğümüne bağlanmak için kullanabileceğiniz bir blok zinciri geliştirme ortamıdır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Azure Blockchain üye oluştur](create-member.md)
* Yükleme [Truffle](https://github.com/trufflesuite/truffle). Truffle gerektirir yüklenmesi için çeşitli araçlar dahil olmak üzere [Node.js](https://nodejs.org), [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
* Yükleme [Python 2.7.15](https://www.python.org/downloads/release/python-2715/). Python Web3 için gereklidir.

## <a name="create-truffle-project"></a>Truffle projesi oluşturma

1. Node.js komut istemi veya kabuğunu açın.
1. Dizini Truffle proje dizini oluşturmak istediğiniz yere değiştirin.
1. Proje için bir dizin oluşturun ve yeni dizine yolunuzla değiştirin. Örneğin,

    ``` bash
    mkdir truffledemo
    cd truffledemo
    ```

1. Truffle projesi başlatın.

    ``` bash
    truffle init
    ```

1. JavaScript API'si Ethereum web3 proje klasörüne yükleyin. Şu anda sürüm web3 sürüm 1.0.0-beta.37 gereklidir.

    ``` bash
    npm install web3@1.0.0-beta.37
    ```

    Yükleme sırasında npm uyarılar alabilirsiniz.

1. Truffle'nın etkileşimli geliştirme konsolunu başlatın.

    ``` bash
    truffle develop
    ```

    Truffle, yerel geliştirme blok zinciri oluşturur ve etkileşimli bir konsol sağlar.

## <a name="connect-to-transaction-node"></a>İşlem düğümüne bağlanma

Kullanım *Web3* işlem düğümüne bağlanmak için. Alabileceğiniz *Web3* Azure portalında bağlantı dizesi.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure Blockchain Service üyelik için gidin. Seçin **işlem düğümleri** ve varsayılan işlem düğüm bağlantısı.

    ![Varsayılan işlem düğümünü seçin](./media/connect-truffle/transaction-nodes.png)

1. Seçin **örnek kod > Web3**.
1. JavaScript'ten kopyalama **HTTPS (erişim anahtarı: 1)** . Kod Truffle'nın etkileşimli geliştirme uçbirimini ihtiyacınız vardır.

    ![Web3 kod](./media/connect-truffle/web3-code.png)

1. Önceki adımdan gelen JavaScript kodu Truffle etkileşimli geliştirme konsola yapıştırın. Kod, Azure Blockchain hizmeti işlem düğüme bağlı bir web3 nesnesi oluşturur.

    Örnek çıktı:

    ```bash
    truffle(develop)> var Web3 = require("Web3");
    truffle(develop)> var provider = new Web3.providers.HttpProvider("https://myblockchainmember.blockchain.azure.com:3200/hy5FMu5TaPR0Zg8GxiPwned");
    truffle(develop)> var web3 = new Web3(provider);
    ```

    Üzerinde yöntem çağırabilirsiniz **web3** , işlem düğümü ile etkileşim kurmak için nesne.

1. Çağrı **getBlockNumber** geçerli blok sayısını döndürmek için yöntemi.

    ```bash
    web3.eth.getBlockNumber();
    ```

    Örnek çıktı:

    ```bash
    truffle(develop)> web3.eth.getBlockNumber();
    18567
    ```
1. Truffle geliştirme konsoldan çıkın.

    ```bash
    .exit
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Blockchain Service varsayılan işlem düğümüne bağlanmak için bir Truffle projesi oluşturdunuz.

Bir işlem consortium blockchain ağınıza gönderilecek Truffle kullanmak için sonraki öğreticiye deneyin.

> [!div class="nextstepaction"]
> [Bir işlem Gönder](send-transaction.md)