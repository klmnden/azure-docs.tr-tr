---
title: Azure Blockchain hizmetini kullanarak işlemleri Gönder
description: Akıllı bir sözleşme dağıtmak ve özel bir işlem göndermek için Azure blok zinciri hizmetini kullanmak nasıl gösteren öğretici.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/29/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 9037c7b5498a5e0a37b05e5ee09891bf8066393d
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417494"
---
# <a name="tutorial-send-transactions-using-azure-blockchain-service"></a>Öğretici: Azure Blockchain hizmetini kullanarak işlemleri Gönder

Bu öğreticide, işlem düğümleri Gizlilik Sözleşmesi ve işlem test etmek için oluşturma gerekir.  Bir yerel geliştirme ortamı oluşturun ve akıllı bir sözleşme dağıtmak ve özel bir işlem göndermek için Truffle kullanacaksınız.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * İşlem düğümleri Ekle
> * Truffle akıllı bir sözleşme dağıtım yapma
> * Bir işlem Gönder
> * İşlem gizlilik doğrula

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Tam [Azure portalını kullanarak bir blok zinciri üye oluştur](create-member.md)
* Tam [hızlı başlangıç: Truffle consortium ağa bağlamak için kullanın](connect-truffle.md)
* Yükleme [Truffle](https://github.com/trufflesuite/truffle). Truffle gerektirir yüklenmesi için çeşitli araçlar dahil olmak üzere [Node.js](https://nodejs.org), [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
* Yükleme [Python 2.7.15](https://www.python.org/downloads/release/python-2715/). Python Web3 için gereklidir.
* Yükleme [Visual Studio kodu](https://code.visualstudio.com/Download)
* Yükleme [Visual Studio kod Solidity uzantısı](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity)

## <a name="create-transaction-nodes"></a>İşlem düğümleri oluştur

Varsayılan olarak, bir işlem düğümü vardır. İki tane daha eklemek için ekleyeceğiz. Düğümlerden biri özel harekete katılan. Diğer özel bir işlemde dahil edilmez.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure Blockchain üyelik için bulun ve seçin **işlem düğümleri > Ekle**.
1. Adlı yeni bir işlem düğümü ayarlarını tamamlamak `alpha`.

    ![İşlem düğümü oluşturma](./media/send-transaction/create-node.png)

    | Ayar | Değer | Açıklama |
    |---------|-------|-------------|
    | Ad | `alpha` | İşlem düğümü adı. Adı, işlem düğümü uç nokta için DNS adresi oluşturmak için kullanılır. Örneğin, `alpha-mymanagedledger.blockchain.azure.com`. |
    | Parola | güçlü parola | Parola, işlem düğümü uç noktasına temel kimlik doğrulaması ile erişmek için kullanılır.

1. **Oluştur**’u seçin.

    Yeni bir işlem düğümü sağlama yaklaşık 10 dakika sürer.

1. 2 adlı bir işlem düğümü eklemek için 4 arasındaki adımları yineleyin `beta`.

Düğümleri sağlanan sırada öğreticiyle devam edebilirsiniz. Sağlama tamamlandığında üç işlem düğümleri gerekir.

## <a name="open-truffle-console"></a>Truffle konsolunu açın

1. Node.js komut istemi veya kabuğunu açın.
1. Yolunuza önkoşul Truffle proje dizinine değiştirme [hızlı başlangıç: Truffle consortium ağa bağlanacak](connect-truffle.md). Örneğin,

    ```bash
    cd truffledemo
    ```

1. Truffle'nın etkileşimli geliştirme konsolunu başlatın.

    ``` bash
    truffle develop
    ```

    Truffle, yerel geliştirme blok zinciri oluşturur ve etkileşimli bir konsol sağlar.

## <a name="create-ethereum-account"></a>Ethereum hesabı oluşturma

Varsayılan işlem düğümüne bağlanın ve Ethereum hesap oluşturma Web3 kullanın. Web3 bağlantı dizesini Azure portalından alabilirsiniz.

1. Azure portalında, varsayılan işlem düğümüne gidin ve seçin **işlem düğümleri > örnek kod > Web3**.
1. JavaScript'ten kopyalama **HTTPS (erişim anahtarı: 1)** ![Web3 örnek kod](./media/send-transaction/web3-code.png)

1. Varsayılan işlem düğümünü Web3 JavaScript kodunu Truffle etkileşimli geliştirme konsola yapıştırın. Kod, Azure Blockchain hizmeti işlem düğüme bağlı bir Web3 nesnesi oluşturur.

    ```bash
    truffle(develop)> var Web3 = require("Web3");
    truffle(develop)> var provider = new Web3.providers.HttpProvider("https://myblockchainmember.blockchain.azure.com:3200/hy5FMu5TaPR0Zg8GxiPwned");
    truffle(develop)> var web3 = new Web3(provider);
    ```

    İşlem düğümünüzü ile etkileşim kurmak için Web3 nesne üzerinde yöntemleri çağırabilir.

1. Varsayılan işlem düğümünde yeni bir hesap oluşturun. Parola parametresi kendi güçlü bir parola ile değiştirin.

    ```bash
    web3.eth.personal.newAccount("1@myStrongPassword");
    ```

    Döndürülen hesabı adresini not ve parola sağlayın. Sonraki bölümde parola ve hesap adresi Ethereum ihtiyacınız var.

1. Çıkış Truffle geliştirme ortamı.

    ```bash
    .exit
    ```

## <a name="configure-truffle-project"></a>Truffle projesi yapılandırma

Truffle projeyi yapılandırmak için Azure portalından bazı işlem düğümü bilgileri gerekir.

### <a name="transaction-node-public-key"></a>İşlem düğümü ortak anahtarı

Her işlem düğümü, bir ortak anahtar vardır. Ortak anahtarı bir özel işlem düğümüne göndermenizi sağlar. Bir işlem için varsayılan işlem düğümünden göndermek için *alfa* işlem düğümü, ihtiyacınız *alfa* işlem düğümün Ortak anahtar.

İşlem düğümü listeden ortak anahtar alabilirsiniz. Alfa düğümü için ortak anahtarı kopyalayın ve daha sonra öğreticide değerini kaydedin.

![İşlem düğümü listesi](./media/send-transaction/node-list.png)

### <a name="transaction-node-endpoint-addresses"></a>İşlem düğümü uç nokta adresleri

1. Azure portalında her işlem düğümüne gidin ve seçin **işlem düğümleri > bağlantı dizeleri**.
1. Kopyalayın ve uç nokta URL'NİZDEN kaydedin **HTTPS (erişim anahtarı: 1)** her işlem düğümü için. Uç nokta adresleri öğreticinin ilerleyen bölümlerinde akıllı sözleşme yapılandırma dosyası gerekir.

    ![İşlem uç nokta adresi](./media/send-transaction/endpoint.png)

### <a name="edit-configuration-file"></a>Yapılandırma dosyasını düzenleyin

1. Visual Studio Code'u başlatmak ve Truffle directory klasörü kullanarak projeyi açın **Dosya > Klasör Aç** menüsü.
1. Truffle yapılandırma dosyasını açın `truffle-config.js`.
1. Dosyanın içeriğini aşağıdaki yapılandırma bilgileri ile değiştirin. Bitiş adresi ve hesap bilgilerini içeren değişkeni ekleyin. Açılı ayraç bölümler önceki bölümlerinden toplanan değerlerle değiştirin.

    ``` javascript
    var defaultnode = "<default transaction node connection string>";
    var alpha = "<alpha transaction node connection string>";
    var beta = "<beta transaction node connection string>";
    
    var myAccount = "<Ethereum account address>";
    var myPassword = "<Ethereum account password>";
    
    var Web3 = require("web3");
    
    module.exports = {
      networks: {
        defaultnode: {
          provider:(() =>  {
          const AzureBlockchainProvider = new Web3.providers.HttpProvider(defaultnode);
    
          const web3 = new Web3(AzureBlockchainProvider);
          web3.eth.personal.unlockAccount(myAccount, myPassword);
    
          return AzureBlockchainProvider;
          })(),
    
          network_id: "*",
          gas: 0,
          gasPrice: 0,
          from: myAccount
        },
        alpha: {
          provider: new Web3.providers.HttpProvider(alpha),
          network_id: "*",
          gas: 0,
          gasPrice: 0
        },
        beta: {
          provider: new Web3.providers.HttpProvider(beta),
          network_id: "*",
          gas: 0,
          gasPrice: 0
        }
      }
    }
    ```

1. Değişiklikleri kaydetmek `truffle-config.js`.

## <a name="create-smart-contract"></a>Akıllı sözleşmesi oluşturma

1. İçinde **sözleşmeleri** klasör adında yeni bir dosya oluşturun `SimpleStorage.sol`. Aşağıdaki kodu ekleyin.

    ```solidity
    pragma solidity >=0.4.21 <0.6.0;
    
    contract SimpleStorage {
        string public storedData;
    
        constructor(string memory initVal) public {
            storedData = initVal;
        }
    
        function set(string memory x) public {
            storedData = x;
        }
    
        function get() view public returns (string memory retVal) {
            return storedData;
        }
    }
    ```
    
1. İçinde **geçişler** klasör adında yeni bir dosya oluşturun `2_deploy_simplestorage.js`. Aşağıdaki kodu ekleyin.

    ```solidity
    var SimpleStorage = artifacts.require("SimpleStorage.sol");
    
    module.exports = function(deployer) {
    
      // Pass 42 to the contract as the first constructor parameter
      deployer.deploy(SimpleStorage, "42", {privateFor: ["<alpha node public key>"], from:"<Ethereum account address>"})  
    };
    ```

1. Açılı ayraçlar değerleri değiştirin.

    | Değer | Açıklama
    |-------|-------------
    | \<alpha node public key\> | Alfa düğümünün ortak anahtarı
    | \<Ethereum account address\> | Varsayılan işlem düğümünde oluşturulan Ethereum hesap adresi

    Bu örnekte, ilk değerini **storeData** değeri, 42'ye ayarlanır.

    **privateFor** sözleşme olduğu kullanılabilir düğüm tanımlar. Bu örnekte, özel işlemler için varsayılan işlem düğümün hesabına çevirebilirsiniz **alfa** düğümü. Ortak anahtarları için tüm özel işlem katılımcıları eklersiniz. Dahil etmezseniz **privateFor:** ve **öğesinden:** , akıllı sözleşme hareketleri geneldir ve tüm consortium üyeleri tarafından görülebilir.

1. Tüm dosyaları seçerek kaydedin **Dosya > Tümünü Kaydet**.

## <a name="deploy-smart-contract"></a>Akıllı sözleşme dağıtma

Truffle dağıtım yapma `SimpleStorage.sol` varsayılan işlem düğümü ağa.

```bash
truffle migrate --network defaultnode
```

Truffle ilk derler ve daha sonra dağıtır **SimpleStorage** akıllı sözleşme.

Örnek çıktı:

```
admin@desktop:/mnt/c/truffledemo$ truffle migrate --network defaultnode

2_deploy_simplestorage.js
=========================

   Deploying 'SimpleStorage'
   -------------------------
   > transaction hash:    0x3f695ff225e7d11a0239ffcaaab0d5f72adb545912693a77fbfc11c0dbe7ba72
   > Blocks: 2            Seconds: 12
   > contract address:    0x0b15c15C739c1F3C1e041ef70E0011e641C9D763
   > account:             0x1a0B9683B449A8FcAd294A01E881c90c734735C3
   > balance:             0
   > gas used:            0
   > gas price:           0 gwei
   > value sent:          0 ETH
   > total cost:          0 ETH


   > Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:                   0 ETH


Summary
=======
> Total deployments:   2
> Final cost:          0 ETH
```

## <a name="validate-contract-privacy"></a>Sözleşme gizlilik doğrula

Sözleşme gizlilik nedeniyle sözleşme değerleri biz bildirilen düğümlerden yalnızca sorgulanabilir **privateFor**. Hesap bu düğümü bulunduğundan bu örnekte biz varsayılan işlem düğümünü sorgulayabilirsiniz. 

1. Truffle konsolunu kullanarak varsayılan işlem düğümüne bağlanın.

    ```bash
    truffle console --network defaultnode
    ```

1. Truffle konsolunda sözleşme örneğinin değerini döndüren bir kod yürütün.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```

    Varsayılan işlem düğümünü sorgulama başarılı olursa 42 değeri döndürülür. Örneğin:

    ```
    admin@desktop:/mnt/c/truffledemo$ truffle console --network defaultnode
    truffle(defaultnode)> SimpleStorage.deployed().then(function(instance){return instance.get();})
    '42'
    ```

1. Truffle konsoldan çıkın.

    ```bash
    .exit
    ```

Biz bildirilen bu yana **alfa** düğümün Ortak anahtar **privateFor**, biz sorgulayabilirsiniz **alfa** düğümü.

1. Truffle konsol kullanarak bağlanır **alfa** düğümü.

    ```bash
    truffle console --network alpha
    ```

1. Truffle konsolunda sözleşme örneğinin değerini döndüren bir kod yürütün.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```

    Sorgulama, **alfa** düğüm başarılı olur, 42 değeri döndürülür. Örneğin:

    ```
    admin@desktop:/mnt/c/truffledemo$ truffle console --network alpha
    truffle(alpha)> SimpleStorage.deployed().then(function(instance){return instance.get();})
    '42'
    ```

1. Truffle konsoldan çıkın.

    ```bash
    .exit
    ```

Biz değil bildirmek beri **beta** düğümün Ortak anahtar **privateFor**, biz sorgu mümkün olmayacaktır **beta** sözleşme gizlilik nedeniyle düğümü.

1. Truffle konsol kullanarak bağlanır **beta** düğümü.

    ```bash
    truffle console --network beta
    ```

1. Sözleşme örneğinin değerini döndüren bir kod yürütün.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```

1. Sorgulama **beta** sözleşme özel olduğundan bir düğümün başarısız olması. Örneğin:

    ```
    admin@desktop:/mnt/c/truffledemo$ truffle console --network beta
    truffle(beta)> SimpleStorage.deployed().then(function(instance){return instance.get();})
    Thrown:
    Error: Returned values aren't valid, did it run Out of Gas?
        at XMLHttpRequest._onHttpResponseEnd (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request.ts:345:8)
        at XMLHttpRequest._setReadyState (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request.ts:219:8)
        at XMLHttpRequestEventTarget.dispatchEvent (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request-event-target.ts:44:13)
        at XMLHttpRequest.request.onreadystatechange (/mnt/c/truffledemo/node_modules/web3-providers-http/src/index.js:96:13)
    ```

1. Truffle konsoldan çıkın.

    ```bash
    .exit
    ```
    
## <a name="send-a-transaction"></a>Bir işlem Gönder

1. Adlı bir dosya oluşturun `sampletx.js`. Proje kök dizininde kaydedin.
1. Aşağıdaki komut dosyasında sözleşmenin ayarlar **storedData** 65 için değişken değeri. Yeni dosyaya kod ekleyin.

    ```javascript
    var SimpleStorage = artifacts.require("SimpleStorage");
    
    module.exports = function(done) {
      console.log("Getting deployed version of SimpleStorage...")
      SimpleStorage.deployed().then(function(instance) {
        console.log("Setting value to 65...");
        return instance.set("65", {privateFor: ["<alpha node public key>"], from:"<Ethereum account address>"});
      }).then(function(result) {
        console.log("Transaction:", result.tx);
        console.log("Finished!");
        done();
      }).catch(function(e) {
        console.log(e);
        done();
      });
    };
    ```

    Açılı ayraçlar değerleri değiştirin, ardından dosyayı kaydedin.

    | Değer | Açıklama
    |-------|-------------
    | \<alpha node public key\> | Alfa düğümünün ortak anahtarı
    | \<Ethereum account address\> | Varsayılan işlem düğümünde oluşturduğunuz Ethereum hesap adresi.

    **privateFor** işlem olduğu kullanılabilir düğüm tanımlar. Bu örnekte, özel işlemler için varsayılan işlem düğümün hesabına çevirebilirsiniz **alfa** düğümü. Tüm özel işlem katılımcıları için ortak anahtarları eklemeniz gerekir.

1. Varsayılan işlem düğümü için bir betik yürütmek için Truffle kullanın.

    ```bash
    truffle exec sampletx.js --network defaultnode
    ```

1. Truffle konsolunda sözleşme örneğinin değerini döndüren bir kod yürütün.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```

    İşlem başarılı olduysa, 65 değeri döndürülür. Örneğin:
    
    ```
    Getting deployed version of SimpleStorage...
    Setting value to 65...
    Transaction: 0x864e67744c2502ce75ef6e5e09d1bfeb5cdfb7b880428fceca84bc8fd44e6ce0
    Finished!
    ```

1. Truffle konsoldan çıkın.

    ```bash
    .exit
    ```
    
## <a name="validate-transaction-privacy"></a>İşlem gizlilik doğrula

İşlem gizlilik nedeniyle işlem düğümlerinde biz bildirilen yalnızca gerçekleştirilebilir **privateFor**. Bu örnekte biz size bildirilen bu yana işlemleri gerçekleştirebilirsiniz **alfa** düğümün Ortak anahtar **privateFor**. 

1. İşlem yürütmek için Truffle kullanın **alfa** düğümü.

    ```bash
    truffle exec sampletx.js --network alpha
    ```
    
1. Sözleşme örneğinin değerini döndüren bir kod yürütün.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```
    
    İşlem başarılı olduysa, 65 değeri döndürülür. Örneğin:

    ```
    Getting deployed version of SimpleStorage...
    Setting value to 65...
    Transaction: 0x864e67744c2502ce75ef6e5e09d1bfeb5cdfb7b880428fceca84bc8fd44e6ce0
    Finished!
    ```
    
1. Truffle konsoldan çıkın.

    ```bash
    .exit
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında silerek kaynakları silebilirsiniz `myResourceGroup` oluşturduğunuz kaynak grubunu Azure Blockchain hizmeti tarafından.

Kaynak grubunu silmek için:

1. Azure portalında gidin **kaynak grubu** sol gezinti bölmesi ve silmek istediğiniz kaynak grubunu seçin.
1. **Kaynak grubunu sil**'i seçin. Kaynak grubu adı girerek silme doğrulamak **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, iki işlem düğümleri Gizlilik Sözleşmesi ve işlem göstermek için eklendi. Varsayılan düğüm, özel bir akıllı sözleşme dağıtmak için kullanılır. Gizlilik sorgulanırken sözleşme değerleri ve gerçekleştirme işlemleri üzerinde blok zinciri tarafından test.

> [!div class="nextstepaction"]
> [Azure Blockchain Service kullanarak blok zinciri uygulamaları geliştirme](develop.md)
