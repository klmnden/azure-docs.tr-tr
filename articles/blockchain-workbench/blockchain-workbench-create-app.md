---
title: İçinde Azure Blockchain çalışma ekranı blockchain uygulaması oluşturma
description: İçinde Azure Blockchain çalışma ekranı blockchain uygulama oluşturma
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/9/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 485f761e22871396dace71333868ba7712e41f67
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="create-a-blockchain-application-in-azure-blockchain-workbench"></a>İçinde Azure Blockchain çalışma ekranı blockchain uygulaması oluşturma

Çok kişili iş akışlarını yapılandırma ve akıllı sözleşme kodu tarafından tanımlanan temsil blockchain uygulamaları oluşturmak için Azure Blockchain çalışma ekranı kullanabilirsiniz.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir blockchain uygulamasını yapılandırma
> * Bir akıllı sözleşme kod dosyası oluşturma
> * Blockchain çalışma ekranı blockchain uygulamaya ekleyin
> * Üye blockchain uygulamaya ekleme

## <a name="prerequisites"></a>Önkoşullar

* Blockchain çalışma ekranı dağıtımı. Daha fazla bilgi için bkz: [Azure Blockchain çalışma ekranının dağıtım](blockchain-workbench-deploy.md) dağıtımı hakkında ayrıntılı bilgi için.
* Blockchain çalışma ekranı ile ilişkilendirilen Kiracı Azure Active Directory Kullanıcıları. Daha fazla bilgi için bkz: [Azure Blockchain çalışma ekranı Azure AD kullanıcı ekleme](blockchain-workbench-manage-users.md#add-azure-ad-users).
* Blockchain çalışma ekranı yönetici hesabı. Daha fazla bilgi için bkz: eklemek [Blockchain çalışma ekranı yöneticileri Azure Blockchain çalışma ekranındaki](blockchain-workbench-manage-users.md#manage-blockchain-workbench-administrators).

Şimdi bir istek sahibi bir istek gönderir ve bir Yanıtlayıcı Gönder isteğine yanıt olarak temel bir uygulama oluşturun. Örneğin, bir istek olabilir, "Merhaba, nasıl misiniz?" ve olmalıdır, "Ben harika!" yanıt verebilir. İstek ve yanıt üzerinde temel alınan blockchain kaydedilir. 

Uygulama dosyaları oluşturmak için aşağıdaki adımları izleyin veya yapabilecekleriniz [örnek Github'dan indirdiğinizde](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/hello-blockchain). 

## <a name="configuration-file"></a>Yapılandırma dosyası

Yapılandırma meta verilerini üst düzey iş akışları ve blockchain uygulamasının etkileşim modelini tanımlar. Yapılandırma meta verilerini blockchain uygulamasının etkileşim modeline ve iş akışı aşamaları temsil eder.

1. Sık kullanılan düzenleyicinizde adlı bir dosya oluşturun `HelloBlockchain.json`.
2. Blockchain uygulama yapılandırmasını tanımlamak için aşağıdaki JSON ekleyin.

    ``` json
    {
      "ApplicationName": "HelloBlockchain",
      "DisplayName": "Hello, Blockchain!",
      "Description": "A simple application to send request and get response",
      "ApplicationRoles": [
        {
          "Name": "Requestor",
          "Description": "A person sending a request."
        },
        {
          "Name": "Responder",
          "Description": "A person responding to a request"
        }
      ],
      "Workflows": [
        {
          "Name": "HelloBlockchain",
          "DisplayName": "Request Response",
          "Description": "A simple workflow to send a request and receive a response.",
          "Initiators": [ "Requestor" ],
          "StartState": "Request",
          "Properties": [
            {
              "Name": "State",
              "DisplayName": "State",
              "Description": "Holds the state of the contract.",
              "Type": {
                "Name": "state"
              }
            },
            {
              "Name": "Requestor",
              "DisplayName": "Requestor",
              "Description": "A person sending a request.",
              "Type": {
                "Name": "Requestor"
              }
            },
            {
              "Name": "Responder",
              "DisplayName": "Responder",
              "Description": "A person sending a response.",
              "Type": {
                "Name": "Responder"
              }
            },
            {
              "Name": "RequestMessage",
              "DisplayName": "Request Message",
              "Description": "A request message.",
              "Type": {
                "Name": "string"
              }
            },
            {
              "Name": "ResponseMessage",
              "DisplayName": "Response Message",
              "Description": "A response message.",
              "Type": {
                "Name": "string"
              }
            }
          ],
          "Constructor": {
            "Parameters": [
              {
                "Name": "message",
                "Description": "...",
                "DisplayName": "Request Message",
                "Type": {
                  "Name": "string"
                }
              }
            ]
          },
          "Functions": [
            {
              "Name": "SendRequest",
              "DisplayName": "Request",
              "Description": "...",
              "Parameters": [
                {
                  "Name": "requestMessage",
                  "Description": "...",
                  "DisplayName": "Request Message",
                  "Type": {
                    "Name": "string"
                  }
                }
              ]
            },
            {
              "Name": "SendResponse",
              "DisplayName": "Response",
              "Description": "...",
              "Parameters": [
                {
                  "Name": "responseMessage",
                  "Description": "...",
                  "DisplayName": "Response Message",
                  "Type": {
                    "Name": "string"
                  }
                }
              ]
            }
          ],
          "States": [
            {
              "Name": "Request",
              "DisplayName": "Request",
              "Description": "...",
              "PercentComplete": 50,
              "Value": 0,
              "Style": "Success",
              "Transitions": [
                {
                  "AllowedRoles": ["Responder"],
                  "AllowedInstanceRoles": [],
                  "Description": "...",
                  "Function": "SendResponse",
                  "NextStates": [ "Respond" ],
                  "DisplayName": "Send Response"
                }
              ]
            },
            {
              "Name": "Respond",
              "DisplayName": "Respond",
              "Description": "...",
              "PercentComplete": 90,
              "Value": 1,
              "Style": "Success",
              "Transitions": [
                {
                  "AllowedRoles": [],
                  "AllowedInstanceRoles": ["Requestor"],
                  "Description": "...",
                  "Function": "SendRequest",
                  "NextStates": [ "Request" ],
                  "DisplayName": "Send Request"
                }
              ]
            }
          ]
        }
      ]
    }
    ```

3. `HelloBlockchain.json` dosyasını kaydedin.

Yapılandırma dosyasında birkaç bölümden oluşur. Her bölüm hakkındaki ayrıntıları aşağıdaki gibidir:

### <a name="application-metadata"></a>Uygulama meta verileri

Yapılandırma dosyasının başında uygulama adı ve açıklaması gibi uygulama hakkında bilgi içerir.

### <a name="application-roles"></a>Uygulama rolleri

Uygulama rolleri bölümü kimin hareket veya blockchain uygulama içinde katılmak kullanıcı rolleri tanımlar. Farklı roller işlevselliğine bağlı kümesini tanımlar. İstek-yanıt senaryosunda, bir istek sahibi işlevselliğini istekleri üreten bir varlık olarak ve yanıtları üreten bir varlık olarak Yanıtlayıcı arasında fark yoktur.

### <a name="workflows"></a>İş akışları

İş akışları bir veya daha fazla aşamaları sözleşmenin eylemleri tanımlayın. İstek-yanıt senaryosunda, ilk (durum) iş akışı (işlev) istek göndermek için bir eylem (geçiş) bir istek sahibi (rolü) alır aşamadır. Sonraki (durum) Yanıtlayıcı (rolü) bir yanıt (işlev) göndermek için bir eylem (geçiş) geçme aşamasıdır. Özellikler, İşlevler, bir uygulamanın iş akışı içerebilir ve bir sözleşme akışını gereken durumları tanımlar. 

Yapılandırma dosyaları içeriği hakkında daha fazla bilgi için bkz: [Azure Blockchain iş akışı yapılandırma başvurusu](blockchain-workbench-configuration-overview.md).

## <a name="smart-contract-code-file"></a>Akıllı sözleşme kod dosyası

Akıllı sözleşmeleri blockchain uygulamanın iş mantığı temsil eder. Şu anda Blockchain çalışma ekranı Ethereum blockchain muhasebe destekler. Ethereum kullanan [Kesintisizlik](https://solidity.readthedocs.io) akıllı sözleşmeleri için kendi kendine zorunlu iş mantığı yazmak için programlama dili olarak.

Kesintisizlik akıllı sözleşmelerinde, nesne yönelimli dillerde sınıfları benzerdir. Her sözleşme durumu ve aşamaları ve akıllı sözleşme eylemlerini uygulamak için işlevler içerir.

Sık kullanılan düzenleyicinizde adlı bir dosya oluşturun `HelloBlockchain.sol`.

### <a name="version-pragma"></a>Sürüm pragması

Bir en iyi uygulama olarak, hedeflediğiniz Kesintisizlik sürümünü belirtin. Sürümünü belirtme Kesintisizlik sürümleri gelecekte uyumsuzluklar önlemeye yardımcı olur. 

Aşağıdaki sürüm pragma en üstünde eklemek `HelloBlockchain.sol` akıllı sözleşme kod dosyası.


  ``` solidity
  pragma solidity ^0.4.20;
  ```

### <a name="base-class"></a>Taban sınıfı

**WorkbenchBase** Blockchain bir güncelleştirme sözleşme oluşturmak çalışma ekranı temel sınıf sağlar. Taban sınıfı için Blockchain çalışma ekranı belirli akıllı sözleşme kodu gereklidir. Sizin devralınmalıdır gerekiyor **WorkbenchBase** temel sınıfı.

İçinde `HelloBlockchain.sol` akıllı sözleşme kod dosyası, ekleme **WorkbenchBase** dosyasının başında sınıfı. 

```
contract WorkbenchBase {
    event WorkbenchContractCreated(string applicationName, string workflowName, address originatingAddress);
    event WorkbenchContractUpdated(string applicationName, string workflowName, string action, address originatingAddress);

    string internal ApplicationName;
    string internal WorkflowName;

    function WorkbenchBase(string applicationName, string workflowName) internal {
        ApplicationName = applicationName;
        WorkflowName = workflowName;
    }

    function ContractCreated() internal {
        WorkbenchContractCreated(ApplicationName, WorkflowName, msg.sender);
    }

    function ContractUpdated(string action) internal {
        WorkbenchContractUpdated(ApplicationName, WorkflowName, action, msg.sender);
    }
}
```
Taban sınıf iki önemli işlevleri içerir:

|Taban sınıf işlevi  | Amaç  | Çağrısı yapıldığında  |
|---------|---------|---------|
| ContractCreated() | Blockchain bir sözleşmeyi oluşturan çalışma ekranı bildirir | Sözleşme Oluşturucusu çıkmadan önce |
| ContractUpdated() | Blockchain Sözleşme durumu güncelleştirilmiş çalışma ekranı bildirir | Bir sözleşme işlevi çıkmadan önce |

### <a name="configuration-and-smart-contract-code-relationship"></a>Yapılandırma ve akıllı sözleşme kodu ilişkisi

Blockchain çalışma ekranı blockchain uygulaması oluşturmak için yapılandırma dosyası ve akıllı sözleşme kod dosyası kullanır. Ne yapılandırması ve akıllı sözleşme kodda tanımlanan arasında bir ilişki yoktur. Sözleşme ayrıntıları, İşlevler, parametreleri ve türleri uygulaması oluşturmak için eşleştirmek için gereklidir. Blockchain çalışma ekranı uygulama oluşturma önce dosyaları doğrular. 

### <a name="contract"></a>Sözleşme

Blockchain çalışma ekranı için sözleşmeleri devralınmalıdır gerek **WorkbenchBase** temel sınıfı. Sözleşme bildirirken uygulama adı ve iş akışı adını bağımsız değişken olarak geçmesi gerekir.

Ekleme **sözleşme** başlığına, `HelloBlockchain.sol` akıllı sözleşme kod dosyası. 

```
contract HelloBlockchain is WorkbenchBase('HelloBlockchain', 'HelloBlockchain') {
```

Sizin devralınmalıdır gerekiyor **WorkbenchBase** temel sınıfı ve parametreleri **ApplicationName** ve iş akışı **adı** yapılandırmasında tanımlandığı şekilde dosya. Bu durumda, uygulama adı ve iş akışı adı aynıdır.

### <a name="state-variables"></a>Durum değişkenleri

Durum değişkenleri her sözleşme örneği durumu değerini depolar. Durum değişkenleri, sözleşmede yapılandırma dosyasında tanımlanan iş akışı özellikleri eşleşmelidir.

Sözleşmede durumu değişkenleri eklemek, `HelloBlockchain.sol` akıllı sözleşme kod dosyası. 

```
    //Set of States
    enum StateType { Request, Respond}
    
    //List of properties
    StateType public  State;
    address public  Requestor;
    address public  Responder;
    
    string public RequestMessage;
    string public ResponseMessage;
```

### <a name="constructor"></a>Oluşturucusu

Oluşturucusu giriş parametreleri için bir iş akışı yeni bir akıllı sözleşme örneğini tanımlar. Oluşturucusu sözleşme aynı ada sahip bir işlevi olarak bildirildi. Oluşturucu için gerekli parametreler, yapılandırma dosyasında Oluşturucu parametreleri olarak tanımlanır. Her iki dosya sayısı, order ve parametre türü eşleşmelidir.

Oluşturucu işlevinde sözleşmesi oluşturma önce gerçekleştirmek istediğiniz herhangi bir iş mantığı yazma. Örneğin, değerleri başlangıç durumu değişkenlerini Başlat.

Yapıcı işlevi çıkmadan önce çağırın `ContractCreated()` işlevi. Bu işlev Blockchain bir sözleşmeyi oluşturan çalışma ekranı bildirir.

Oluşturucu işlevi, sözleşmede ekleyin, `HelloBlockchain.sol` akıllı sözleşme kod dosyası. 

```
    // constructor function
    function HelloBlockchain(string message) public
    {
        Requestor = msg.sender;
        RequestMessage = message;
        State = StateType.Request;
    
        // call ContractCreated() to create an instance of this workflow
        ContractCreated();
    }
```

### <a name="functions"></a>İşlevler

İşlevlerini iş mantığı bir sözleşme içinde yürütülebilir birimleridir. İşlevi için gerekli parametreler yapılandırma dosyasındaki işlevi parametre olarak tanımlanır. Her iki dosya sayısı, order ve parametre türü eşleşmelidir. Yapılandırma dosyasındaki Blockchain çalışma ekranı akışındaki geçişler için ilişkili işlevlerdir. Bir geçiş anlaşma tarafından belirlenen şekilde bir uygulamanın iş akışı bir sonraki adımına taşımak için gerçekleştirilen bir eylemdir.

İş mantığı işlevinde gerçekleştirmek istediğiniz yazma. Örneğin, bir durum değişkeninin değerini değiştirme.

İşlev çıkmadan önce çağırın `ContractUpdated()` işlevi. İşlev Sözleşme durumu güncelleştirilmiş Blockchain çalışma ekranı bildirir. İşlev yapılan durumu değişiklikleri geri almak istiyorsanız, revert() çağırın. Geri iptali durum ContractUpdated() son çağrısından sonra yapılan değişiklikler.

1. Aşağıdaki işlevleri, sözleşmede ekleyin, `HelloBlockchain.sol` akıllı sözleşme kod dosyası. 

    ```
        // call this function to send a request
        function SendRequest(string requestMessage) public
        {
            if (Requestor != msg.sender)
            {
                revert();
            }
    
            RequestMessage = requestMessage;
            State = StateType.Request;
    
            // call ContractUpdated() to record this action
            ContractUpdated('SendRequest');
        }
    
        // call this function to send a response
        function SendResponse(string responseMessage) public
        {
            Responder = msg.sender;
    
            // call ContractUpdated() to record this action
            ResponseMessage = responseMessage;
            State = StateType.Respond;
            ContractUpdated('SendResponse');
        }
    }
    ```

2. Kaydetme, `HelloBlockchain.sol` akıllı sözleşme kod dosyası.

## <a name="add-blockchain-application-to-blockchain-workbench"></a>Blockchain çalışma ekranı blockchain uygulama ekleme

Blockchain çalışma ekranı blockchain uygulamaya eklemek için yapılandırma ve uygulamayı tanımlamak için akıllı sözleşme dosyaları karşıya yükleyin.

1. Bir web tarayıcısında Blockchain çalışma ekranı web adresine gidin. Örneğin, `https://{workbench URL}.azurewebsites.net/` Blockchain çalışma ekranı dağıttığınızda web uygulaması oluşturulur. Blockchain çalışma ekranı web adresini bulmak hakkında daha fazla bilgi için bkz: [Blockchain çalışma ekranı Web URL'si](blockchain-workbench-deploy.md#blockchain-workbench-web-url)
2. Blockchain çalışma ekranı yönetici olarak oturum açın. Kullanıcıları yönetme hakkında daha fazla bilgi için bkz: [Azure Blockchain çalışma ekranındaki kullanıcıları yönetme](blockchain-workbench-manage-users.md).
3. Seçin **uygulamaları** > **yeni**. **Yeni uygulama** bölmesinde görüntülenir.
4. Seçin **sözleşme yapılandırma karşıya** > **Gözat** bulmak için **HelloBlockchain.json** oluşturduğunuz yapılandırma dosyası. Yapılandırma dosyası otomatik olarak onaylanır. Seçin **Göster** doğrulama hataları görüntülemesi için bağlantı. Uygulamayı dağıtmadan önce doğrulama hatalarını düzeltin.
5. Seçin **sözleşme kodu karşıya** > **Gözat** bulmak için **HelloBlockchain.sol** akıllı sözleşme kod dosyası. Kod dosyası otomatik olarak onaylanır. Seçin **Göster** doğrulama hataları görüntülemesi için bağlantı. Uygulamayı dağıtmadan önce doğrulama hatalarını düzeltin.
6. Seçin **dağıtma** yapılandırma ve akıllı sözleşme dosyaları dayalı blockchain uygulaması oluşturmak için.

Blockchain uygulama dağıtımını birkaç dakika sürer. Dağıtım tamamlandığında, yeni uygulama görüntülenen **uygulamaları**. 

> [!NOTE]
> Kullanarak blockchain uygulamaları oluşturabilirsiniz [Azure Blockchain çalışma ekranı REST API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench). 

## <a name="add-blockchain-application-members"></a>Blockchain uygulama üyeleri Ekle

Uygulamanızın başlatmak ve sözleşmelerinde eylemleri uygulama üyeleri ekleyin. Uygulama üye eklemek için gereken bir [Blockchain çalışma ekranı yönetici](blockchain-workbench-manage-users.md#manage-blockchain-workbench-administrators).

1. Seçin **uygulamaları** > **Hello, Blockchain!**.
2. Uygulamaya ilişkili üye sayısı, sayfanın sağ üst köşesinde görüntülenir. Yeni bir uygulama için üye sayısı sıfır olur.
3. Seçin **üyeleri** sayfanın sağ alt köşesindeki bağlantı. Üyeleri uygulama için geçerli bir listesi görüntülenir.
4. Üyelik listesinde **üye eklemek**.
5. Eklemek istediğiniz üyenin adını girin veya seçin. Yalnızca Blockchain çalışma ekranı Kiracı mevcut Azure AD kullanıcılar listelenir. Kullanıcı bulunamazsa, gerek [Azure AD kullanıcı ekleme](blockchain-workbench-manage-users.md#add-azure-ad-users).
6. Seçin **rol** üyesi için. İlk üye için seçin **istek sahibi** rolü olarak.
7. Seçin **Ekle** ilişkili rol üyesiyle uygulama eklemek için.
8. Uygulama ile başka bir üye eklemek **Yanıtlayıcı** rol.

Blockchain çalışma ekranı kullanıcıları yönetme hakkında daha fazla bilgi için bkz: [Azure Blockchain çalışma ekranı kullanıcıları yönetme](blockchain-workbench-manage-users.md)

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır bu makalede, temel bir istek ve yanıt uygulaması oluşturduğunuzu düşünün. Uygulama kullanmayı öğrenmek için sonraki nasıl yapılır makalesi için devam edin.

> [!div class="nextstepaction"]
> [Bir blockchain uygulaması kullanma](blockchain-workbench-use.md)
