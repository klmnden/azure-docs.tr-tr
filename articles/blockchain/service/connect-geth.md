---
title: Azure Blockchain hizmetine bağlanmak için Geth kullanın
description: Geth kullanarak bir Azure blok zinciri Service ağa bağlanma
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 0716a9326a54ae31d4f355fe5f4c88488339b390
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026924"
---
# <a name="quickstart-use-geth-to-connect-to-a-transaction-node"></a>Hızlı Başlangıç: Bir işlem düğümüne bağlanmak için Geth kullanın

Geth, bir Azure blok zinciri hizmetinin işlem düğümünde bir Geth örneğe eklemek için kullanabileceğiniz bir Git Ethereum istemcisidir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Yükleme [Geth](https://github.com/ethereum/go-ethereum/wiki/geth)
* [Azure Blockchain üye oluştur](create-member.md)

## <a name="get-the-geth-connection-string"></a>Geth bağlantı dizesini alın

Geth bağlantı dizesini Azure portalında bulabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure Blockchain Service üyelik için gidin. Seçin **işlem düğümleri** ve varsayılan işlem düğüm bağlantısı.

    ![Varsayılan işlem düğümünü seçin](./media/connect-geth/transaction-nodes.png)

1. Seçin **bağlantı dizeleri**.
1. Bağlantı dizesini kopyalayın **HTTPS (erişim anahtarı: 1)**. Komut için sonraki bölüme ihtiyacınız vardır.

    ![Bağlantı dizesi](./media/connect-geth/connection-string.png)

## <a name="connect-to-geth"></a>İçin Geth bağlanma

1. Bir komut istemi veya kabuk açın.
1. Kullanım Geth, işlem düğümü üzerinde çalışan Geth örneği eklemek için alt ekleyin. Ek alt komut için bağımsız değişken olarak bağlantı dizesini yapıştırın. Örneğin,

    ```
    geth attach <connection string>
    ```

1. İşlem düğümün Ethereum konsola bağlandıktan sonra web3 JavaScript Dapp API'si veya yönetim API'si çağırabilirsiniz.

    Örneğin, aşağıdaki API'yi chainId bulmak için kullanın.

    ```bash
    admin.nodeInfo.protocols.istanbul.config.chainId
    ```

    Bu örnekte, chainId 297 ' dir.

    ![Azure Blockchain hizmet seçeneği](./media/connect-geth/geth-attach.png)

1. Konsoldan kesmek için şunu yazın `exit`.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure blok zinciri hizmeti işlem düğümü Geth örneğinde iliştirilebilir Geth istemci kullanılır. Dağıtma ve Truffle kullanarak bir işlem göndermek için sonraki öğreticiye deneyin.

> [!div class="nextstepaction"]
> [Bir işlem Gönder](send-transaction.md)