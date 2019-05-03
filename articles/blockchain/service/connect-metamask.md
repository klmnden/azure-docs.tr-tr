---
title: Bir Azure blok zinciri Service ağına MetaMask bağlama
description: MetaMask kullanarak Azure blok zinciri Service ağına bağlayın ve akıllı bir sözleşme dağıtın.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: db029cee6edcd14d29c83964e5bf75aa45077e7e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026903"
---
# <a name="quickstart-use-metamask-to-connect-and-deploy-a-smart-contract"></a>Hızlı Başlangıç: MetaMask kullanarak bağlanmak ve akıllı bir sözleşme dağıtma

Bu hızlı başlangıçta, bir Azure blok zinciri Service ağa bağlanma ve akıllı bir sözleşme dağıtmak için Remix MetaMask kullanacaksınız. Metamask Ether Cüzdan yönetmek ve akıllı sözleşme eylemleri gerçekleştirmek için bir tarayıcı uzantısıdır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Azure Blockchain üye oluştur](create-member.md)
* Yükleme [MetaMask tarayıcı uzantısı](https://metamask.io)
* Bir MetaMask oluşturmak [Cüzdan](https://metamask.zendesk.com/hc/en-us/articles/360015488971-New-to-MetaMask-Learn-How-to-Setup-MetaMask-the-First-Time)

## <a name="get-endpoint-address"></a>Uç nokta adresi alın

Blok zinciri ağa bağlanmak için Azure blok zinciri hizmet uç noktası adresi ihtiyacınız var. Uç nokta adresi ve erişim anahtarlarını Azure portalında bulabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure Blockchain Service üyelik için gidin. Seçin **işlem düğümleri** ve varsayılan işlem düğüm bağlantısı.

    ![Varsayılan işlem düğümünü seçin](./media/connect-metamask/transaction-nodes.png)

1. Seçin **bağlantı dizelerini > erişim anahtarları**.
1. Uç nokta adresini kopyalamak **HTTPS (erişim anahtarı: 1)**. Adres için sonraki bölüme ihtiyacınız var.

    ![Bağlantı dizesi](./media/connect-metamask/connection-string.png)

## <a name="connect-metamask"></a>MetaMask bağlanma

1. MetaMask tarayıcı uzantısı'nı açıp oturum açın.
1. Ağ açılır menüden seçin **özel RPC**.

    ![Özel RPC](./media/connect-metamask/custom-rpc.png)

1. İçinde **yeni ağ > Yeni bir RPC URL**, önceki bölümde kopyaladığınız, uç nokta adresini girin.
1. **Kaydet**’i seçin.

    Bağlantı başarılı oldu, özel ağ ağ açılır listede görüntülenir.

    ![Yeni ağ](./media/connect-metamask/new-network.png)

## <a name="deploy-smart-contract"></a>Akıllı sözleşme dağıtma

Remix bir tarayıcı tabanlı Solidity geliştirme ortamıdır. MetaMask ve Remix birlikte kullanarak, dağıtabilir ve akıllı sözleşmelerinde eylemleri gerçekleştirin.

1. Tarayıcınızda `https://remix.ethereum.org` adresine gidin.
1. **Çalıştır**'ı seçin. 

    MetaMask kümeleri, **ortam** için **eklenen Web3** ve **hesabı** ağınıza.

    ![Çalıştır sekmesi](./media/connect-metamask/injected-web3.png)

1. Seçin **yeni dosya oluştur**.

    Yeni dosya adı `simple.sol`.

    ![Dosya oluştur](./media/connect-metamask/create-file.png)

    **Tamam**’ı seçin.

1. Aşağıdaki Remix Düzenleyicisi'nde yapıştırın **basit akıllı sözleşme** kod.

    ```solidity
    pragma solidity ^0.5.0;
             
    contract simple {
        uint balance;
                 
        constructor() public{
            balance = 0;
        }
                 
        function add(uint _num) public {
            balance += _num;
        }
                 
        function get() public view returns (uint){
            return balance;
        }
    }
    ```

    **Basit sözleşme** bildirir adlandırılmış bir durum değişkeninin **Bakiye**. Tanımlı iki işlevi vardır. **Ekleme** işlevi için bir numara ekler **Bakiye**. **Alma** işlevi değerini döndürür **Bakiye**.

1. Sözleşme derlemek için işaretleyin **derleme > derlemek başlangıç**. Başarılı olursa yeşil bir sözleşme adı kutusu görüntülenir.

    ![Derle](./media/connect-metamask/compile.png)

1. Sözleşme yürütülecek seçin **çalıştırma** sekmesi. Seçin **basit** sonra Sözleşme **Dağıt**.

    ![Özel RPC](./media/connect-metamask/deploy.png)

1. Sizi işlem gerçekleştirmek için yetersiz Bakiye MetaMask bildirim görüntülenir.

    Ortak blok zinciri ağ için Ether işlem maliyeti için ödeme yapmam gerekir. Bu bir konsorsiyum özel bir ağda olduğundan, doğalgaz fiyat sıfır olarak ayarlayabilirsiniz.

1.  Seçin **gaz Ücret > Düzenle > Gelişmiş**ayarlayın **gaz fiyat** 0.

    ![Gaz fiyat](./media/connect-metamask/gas-price.png)

    **Kaydet**’i seçin.

1. Seçin **Onayla** blok zincirine akıllı sözleşme dağıtılacak.
1. İçinde **dağıtılan sözleşmeleri** bölümünde, genişletmek **basit** sözleşme.

    ![Dağıtılan Sözleşmesi](./media/connect-metamask/deployed-contract.png)

    İki eylem **ekleme** ve **alma** sözleşmede tanımlanan işlevleri için eşleme.

1. Gerçekleştirmek için bir **ekleme** blok zinciri, işleme Ekle seçeneğini bir sayı girin **ekleme**.
1. Benzer şekilde, sözleşmenin dağıttığınızda, sizi işlem gerçekleştirmek için yetersiz Bakiye MetaMask bildirim görüntülenir.

    Bu bir konsorsiyum özel bir ağda olduğundan, doğalgaz fiyat sıfır olarak ayarlayabilirsiniz.

1.  Seçin **gaz Ücret > Düzenle > Gelişmiş**ayarlayın **gaz fiyat** 0 ve **Kaydet**.
1. Seçin **Onayla** blok zinciri üzerinde işlem gerçekleştirmek için.
1. Seçin **alma** eylem. Bu sorgu düğümünün verilerine bir çağrıdır. Bir işlem gerekli değildir.
1. Remix hata ayıklama bölmesinde uygun blok zinciri işlemler hakkında ayrıntı görebilirsiniz.

    ![Geçmiş hata ayıklama](./media/connect-metamask/debug.png)

    Gördüğünüz **basit** oluşturma işlemi için sözleşme **simple.add**ve çağrısı **simple.get**.

1. Ayrıca, işlem MetaMask geçmişinde görebilirsiniz. MetaMask tarayıcı uzantısı'nı açın.
1. İçinde **geçmişi** bölümünde, dağıtılan sözleşme ve işlem günlüğü görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Blockchain hizmeti işlem düğümüne bağlanmak için akıllı bir sözleşme dağıtıp bir işlem için blok zinciri Gönder MetaMask tarayıcı uzantısı kullanılır. Dağıtma ve Truffle kullanarak bir işlem göndermek için sonraki öğreticiye deneyin.

> [!div class="nextstepaction"]
> [Bir işlem Gönder](send-transaction.md)