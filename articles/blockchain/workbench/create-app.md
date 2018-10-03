---
title: Azure Blockchain Workbench uygulamasında bir blok zinciri uygulaması oluşturma
description: Azure Blockchain Workbench uygulamasında bir blok zinciri uygulaması oluşturma
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: a7ca3f42874bc844bc0036e37a790ffebdc5f8d8
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48243489"
---
# <a name="create-a-blockchain-application-in-azure-blockchain-workbench"></a>Azure Blockchain Workbench uygulamasında bir blok zinciri uygulaması oluşturma

Azure Blockchain Workbench, çok kişili iş akışlarını yapılandırma ve akıllı sözleşme kodu tarafından tanımlanan temsil eden blok zinciri uygulamaları oluşturmak için kullanabilirsiniz.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Blok zinciri uygulaması yapılandırma
> * Bir akıllı sözleşme kodu dosyası oluştur
> * Blockchain Workbench'i bir blok zinciri uygulaması Ekle
> * Blok zinciri uygulamaya üyeleri Ekle

## <a name="prerequisites"></a>Önkoşullar

* Blockchain Workbench'i dağıtımı. Daha fazla bilgi için [Azure Blockchain Workbench dağıtım](deploy.md) dağıtımı hakkında ayrıntılı bilgi için.
* Azure Active Directory Kullanıcıları, Blockchain Workbench ile ilişkilendirilen Kiracı. Daha fazla bilgi için [Azure Blockchain Workbench uygulamasında Azure AD kullanıcı ekleme](manage-users.md#add-azure-ad-users).
* Blockchain Workbench'i yönetici hesabı. Daha fazla bilgi için ekleme [Blockchain Workbench'i Yöneticiler Azure Blockchain Workbench](manage-users.md#manage-blockchain-workbench-administrators).

## <a name="hello-blockchain"></a>Blok zinciri Merhaba!

Bir istek sahibi bir istek gönderir ve bir Yanıtlayıcı isteğe yanıt gönderme temel bir uygulama oluşturalım. Örneğin, bir istek olabilir, "Hello, nasılsın?" ve, "Ben harika!" yanıt olabilir. Temel alınan uygun blok zinciri, hem isteğin hem de yanıt kaydedilir. 

Uygulama dosyaları oluşturmak için aşağıdaki adımları izleyin veya yapabilecekleriniz [örneği, Github'dan indirin](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/hello-blockchain). 

## <a name="configuration-file"></a>Yapılandırma dosyası

Yapılandırma meta verilerini, üst düzey iş akışları ve blok zinciri uygulaması etkileşim modelini tanımlar. Yapılandırma meta verilerini, blok zinciri uygulaması etkileşim modeli ve iş akışı aşamaları temsil eder.

1. Sık kullandığınız düzenleyicinizde adlı bir dosya oluşturun `HelloBlockchain.json`.
2. Blok zinciri uygulaması yapılandırmasını tanımlamak için aşağıdaki JSON ekleyin.

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

Yapılandırma dosyasında birkaç bölümden oluşur. Her bölüm hakkında ayrıntıları aşağıdaki gibidir:

### <a name="application-metadata"></a>Uygulama meta verileri

Yapılandırma dosyasının başında, uygulama adı ve açıklaması gibi uygulama hakkında bilgi içerir.

### <a name="application-roles"></a>Uygulama rolleri

Uygulama roller bölümünden kimin hareket veya blok zinciri uygulaması içinde katılmak kullanıcı rolleri tanımlar. İşlevselliğine dayalı farklı roller kümesi tanımlarsınız. İstek-yanıt senaryoda, bir istek sahibi istekleri oluşturan bir varlık olarak işlevlerini ve yanıt veren bir varlık olarak bir Yanıtlayıcı arasında bir ayrım yoktur.

### <a name="workflows"></a>İş akışları

İş akışları, bir veya daha fazla aşamaları ve sözleşmenin eylemleri tanımlayın. İstek-yanıt senaryoda, bir istek sahibi (Rol) (işlev) bir istek göndermek için bir eylem (geçiş) alır, iş akışının ilk aşama (durum) olur. Sonraki aşamaya (durum), bir Yanıtlayıcı (Rol) alır (işlev) yanıt göndermek için bir eylem (geçiş) ' dir. Bir uygulamanın iş akışı özellikleri, İşlevler, içerebilir ve bir sözleşme akışını gereken durumları tanımlar. 

Yapılandırma dosyalarını içeriği hakkında daha fazla bilgi için bkz. [Azure blok zinciri iş akışı yapılandırma başvurusu](configuration.md).

## <a name="smart-contract-code-file"></a>Akıllı sözleşme kod dosyası

Nitelikli akıllı anlaşmalar blok zinciri iş mantığına temsil eder. Şu anda, Blockchain Workbench'i Ethereum blok zinciri muhasebe destekler. Ethereum kullanan [Solidity](https://solidity.readthedocs.io) nitelikli akıllı anlaşmalar için kendi kendine zorunlu iş mantığı yazmak için programlama dili olarak.

Solidity akıllı sözleşmeleri, nesne odaklı dildeki sınıflarda benzerdir. Her sözleşme durumu ve aşamaları ve akıllı sözleşme eylemlerini uygulamak için işlevler içerir.

Sık kullandığınız düzenleyicinizde adlı bir dosya oluşturun `HelloBlockchain.sol`.

### <a name="version-pragma"></a>Sürüm pragması

En iyi uygulama, hedeflediğiniz Solidity sürümünü belirtin. Sürümünü belirtme, gelecekteki Solidity sürümleri ile uyumsuzlukları önlemeye yardımcı olur. 

Aşağıdaki sürüm pragma üstüne ekleyin `HelloBlockchain.sol` akıllı sözleşme kod dosyası.


  ``` solidity
  pragma solidity ^0.4.20;
  ```

### <a name="base-class"></a>Temel sınıf

**WorkbenchBase** Blockchain Workbench, sözleşmenin oluşturmak temel sınıf sağlar. Blockchain Workbench'i belirli akıllı sözleşme kod için temel sınıf gereklidir. Devralınacak sözleşmeniz gereken **WorkbenchBase** temel sınıfı.

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
Temel sınıfı iki önemli işlevler içerir:

|Temel sınıf işlevi  | Amaç  | Çağrısı yapıldığında  |
|---------|---------|---------|
| ContractCreated() | Blockchain Workbench'i sözleşme oluşturulan bildirir | Sözleşme Oluşturucusu çıkmadan önce |
| ContractUpdated() | Blockchain Workbench'i anlaşması durum güncelleştirildi bildirir | Bir sözleşme işlev çıkmadan önce |

### <a name="configuration-and-smart-contract-code-relationship"></a>Yapılandırma ve akıllı sözleşme kodu ilişki

Blockchain Workbench'i akıllı sözleşme kod dosyasını ve yapılandırma dosyası bir blok zinciri uygulaması oluşturmak için kullanır. Hangi yapılandırma ve akıllı sözleşme kodda tanımlanır arasında bir ilişki yoktur. Uygulama oluşturmak için eşleştirilecek podrobnosti o kontraktu, işlevleri, parametreler ve türleri gerekir. Blockchain Workbench'i uygulama oluşturma önce dosyaları doğrular. 

### <a name="contract"></a>Sözleşme

Blockchain Workbench'i için sözleşmeler devralınacak gerek **WorkbenchBase** temel sınıfı. Sözleşme bildirirken, uygulama adı ve iş akışı adı bağımsız değişken olarak geçirmek gerekir.

Ekleme **sözleşme** başlığına, `HelloBlockchain.sol` akıllı sözleşme kod dosyası. 

```
contract HelloBlockchain is WorkbenchBase('HelloBlockchain', 'HelloBlockchain') {
```

Devralınacak sözleşmeniz gereken **WorkbenchBase** temel sınıfı ve parametreleri geçirin **ApplicationName** ve iş akışı **adı** yapılandırmasında tanımlandığı şekilde dosya. Bu durumda, uygulama adı ve iş akışı adı aynıdır.

### <a name="state-variables"></a>Durum değişkenleri

Durumu değişkenleri her sözleşme örneği durumu değerlerini depolar. Yapılandırma dosyasında tanımlanan iş akışı özelliklerini sözleşmeniz durumu değişkenlerinde eşleşmelidir.

Sözleşmede durumunu değişkenleri eklemek, `HelloBlockchain.sol` akıllı sözleşme kod dosyası. 

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

Oluşturucu, bir iş akışının yeni bir akıllı sözleşme örneği için giriş parametrelerini tanımlar. Oluşturucu, sözleşme aynı ada sahip bir işlev olarak bildirilir. Gerekli Parametreler oluşturucusu için yapılandırma dosyasında Oluşturucu parametresi olarak tanımlanır. Her iki dosyada sayısı, sırası ve parametre türü eşleşmelidir.

Oluşturucu işlevi iş mantığı sözleşme oluşturmadan önce gerçekleştirmek istediğiniz yazın. Örneğin, değerleri başlangıç durumu değişkenlerini başlatın.

Oluşturucu işlevi çıkmadan önce çağrı `ContractCreated()` işlevi. Bu işlev bir sözleşme oluşturulan Blockchain Workbench'i bildirir.

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

İşlevler, iş mantığı bir sözleşme içinde yürütülebilir birimleridir. İşlevi için gerekli parametreler, yapılandırma dosyasındaki işlevi parametre olarak tanımlanır. Her iki dosyada sayısı, sırası ve parametre türü eşleşmelidir. Yapılandırma dosyasındaki Blockchain Workbench'i akışındaki geçişleri ilişkili işlevlerdir. Geçiş, sözleşmesi tarafından belirlenen şekilde bir uygulamanın iş akışı bir sonraki adımına gitmek için gerçekleştirilen bir eylemdir.

İş mantığı işlevi gerçekleştirmek istediğiniz yazın. Örneğin, bir durum değişkeninin değerini değiştirme.

İşlev çıkmadan önce çağrı `ContractUpdated()` işlevi. İşlevi, Blockchain Workbench'i anlaşması durum güncelleştirildi bildirir. İşlevde yapılan durumu değişiklikleri geri almak istiyorsanız, revert() çağırın. Geri döndürme durum ContractUpdated() son çağrısından sonra yapılan değişiklikleri atar.

1. Sözleşmeniz aşağıdaki işlevleri ekleyin, `HelloBlockchain.sol` akıllı sözleşme kod dosyası. 

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

## <a name="add-blockchain-application-to-blockchain-workbench"></a>Blockchain Workbench'i blok zinciri uygulamaya ekleme

Blockchain Workbench'i bir blok zinciri uygulaması eklemek için yapılandırma ve uygulamayı tanımlamak için akıllı sözleşme dosyalarını karşıya yükleyin.

1. Bir web tarayıcısında Blockchain Workbench'i web adresine gidin. Örneğin, `https://{workbench URL}.azurewebsites.net/` Blockchain Workbench'i dağıtma web uygulaması oluşturulur. Web adresini, Blockchain Workbench'i bulma hakkında daha fazla bilgi için bkz. [Blockchain Workbench'i Web URL'si](deploy.md#blockchain-workbench-web-url)
2. Olarak oturum bir [Blockchain Workbench'i yönetici](manage-users.md#manage-blockchain-workbench-administrators).
3. Seçin **uygulamaları** > **yeni**. **Yeni uygulama** bölmesi görüntülenir.
4. Seçin **sözleşme yapılandırma karşıya** > **Gözat** bulunacak **HelloBlockchain.json** oluşturduğunuz yapılandırma dosyası. Yapılandırma dosyasını otomatik olarak doğrulanır. Seçin **Göster** doğrulama hataları görüntülemesi için bağlantı. Uygulamayı dağıtmadan önce doğrulama hatalarını düzeltin.
5. Seçin **sözleşme kodu karşıya** > **Gözat** bulunacak **HelloBlockchain.sol** akıllı sözleşme kod dosyası. Kod dosyası otomatik olarak doğrulanır. Seçin **Göster** doğrulama hataları görüntülemesi için bağlantı. Uygulamayı dağıtmadan önce doğrulama hatalarını düzeltin.
6. Seçin **Dağıt** yapılandırmanıza ve akıllı sözleşme dosyaları dayalı blok zinciri uygulaması oluşturmak için.

Blok zinciri uygulaması dağıtımını birkaç dakika sürer. Dağıtım tamamlandığında, yeni uygulama görüntülenen **uygulamaları**. 

> [!NOTE]
> Kullanarak blok zinciri uygulamaları oluşturabilirsiniz [Azure Blockchain Workbench REST API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench). 

## <a name="add-blockchain-application-members"></a>Blok zinciri uygulaması üyeleri Ekle

Uygulama üyeleri başlatmak ve sözleşmelerinde eylemleri için uygulamanıza ekleyin. Uygulama üyeleri eklemek için olması gereken bir [Blockchain Workbench'i yönetici](manage-users.md#manage-blockchain-workbench-administrators).

1. Seçin **uygulamaları** > **Merhaba, blok zinciri!**.
2. Uygulamayla ilişkilendirilen üye sayısı, sayfanın sağ üst köşesinde görüntülenir. Yeni bir uygulama için üye sayısı sıfır olacaktır.
3. Seçin **üyeleri** sayfanın sağ üst köşesindeki bağlantı. Geçerli uygulamanın üyelerin listesi görüntülenir.
4. Üyelik listesinde **üye ekleme**.
5. Seçin veya eklemek istediğiniz üyenin adını girin. Blockchain Workbench'i kiracısında mevcut Azure AD kullanıcıları yalnızca listelenir. Kullanıcı bulunamazsa, yapmanız [Azure AD kullanıcı ekleme](manage-users.md#add-azure-ad-users).
6. Seçin **rol** üyesi için. İlk üye için seçin **istek sahibi** rolü olarak.
7. Seçin **Ekle** uygulama ile ilişkili rol üyesi eklemek için.
8. Uygulama ile başka bir üye eklemek **Yanıtlayıcı** rol.

Blockchain Workbench'i kullanıcıları yönetme hakkında daha fazla bilgi için bkz. [Azure Blockchain Workbench kullanıcıları yönetme](manage-users.md)

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır bu makalede, temel bir istek ve yanıt uygulaması oluşturdunuz. Uygulamayı kullanmayı öğrenmek için sonraki nasıl yapılır makalesi için devam edin.

> [!div class="nextstepaction"]
> [Bir blok zinciri uygulaması kullanma](use.md)
