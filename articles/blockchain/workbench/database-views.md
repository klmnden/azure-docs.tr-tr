---
title: Azure Blockchain Workbench uygulamasında veritabanı görünümleri
description: Azure Blockchain Workbench SQL DB veritabanı görünümleri genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/28/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 9071cf524a0f3d319d108cb5c961fa886cf8747f
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399894"
---
# <a name="database-views-in-azure-blockchain-workbench"></a>Azure Blockchain Workbench uygulamasında veritabanı görünümleri

Azure Blockchain Workbench, dağıtılan defterler için gelen veri sunan bir *dışı* SQL DB veritabanı. Dışı veritabanı, SQL ve mevcut araçları gibi kullanmayı mümkün kılar [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)blok zinciri verilerle etkileşimde bulunmak için.

Azure Blockchain Workbench, sorgularınızı gerçekleştirilirken yararlı olacaktır verilerine erişim sağlaması veritabanı görünümleri sunmaktadır. Bu görünümler oluşturmaya başlamak raporları, analiz, hızlıca alabilir ve aksi takdirde blok zinciri veri veritabanı personeli yeniden eğitme gerek kalmadan mevcut araçları ile kullanmak kolay hale getirmek için yoğun normalleştirilmişlikten çıkarılmış.

Bu bölümde, veritabanı görünümleri ve içerdikleri veriler genel bir bakış içerir.

> [!NOTE]
> Veritabanı mümkün sırasında bu görünümleri dışında bulunan veritabanı tablolarını doğrudan kullanımı desteklenmiyor.
>

## <a name="vwapplication"></a>vwApplication

Bu görünüm, hakkında ayrıntılı bilgi sağlar. **uygulamaları** Azure Blockchain Workbench'i yüklendi.

| Ad                             | Type          | Can Be Null | Açıklama                                                                                                                                                                                                                                                   |
|----------------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                    | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                  | nvarchar(50)  | Hayır          | Uygulamanın adı |
| ApplicationDescription           | nvarchar(255) | Evet         | Uygulama açıklaması |
| ApplicationDisplayName           | nvarchar(255) | Hayır          | Bir kullanıcı arabiriminde görüntülenecek adı |
| ApplicationEnabled               | bit           | Hayır          | Uygulama şu anda etkin olup olmadığını tanımlar<br /> **Not:** Bir uygulama veritabanında devre dışı olarak yansıtılan olsa da, ilişkili sözleşmelerin uygun blok zinciri kalır ve bu sözleşmeler hakkındaki verileri veritabanında kalır. |
| UploadedDtTm                     | datetime2(7)  | Hayır          | Tarih ve saat sözleşme karşıya yüklendi |
| UploadedByUserId                 | int           | Hayır          | Uygulamanın karşıya kullanıcının kimliği. |
| UploadedByUserExternalId         | nvarchar(255) | Hayır          | Uygulamanın karşıya kullanıcı dış tanımlayıcı. Varsayılan olarak, bu consortium için Azure Active Directory'den kullanıcı kimliğidir.                                                                                                |
| UploadedByUserProvisioningStatus | int           | Hayır          | Sağlama işlemi kullanıcının geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 – kullanıcı API tarafından oluşturuldu<br />1 – bir anahtar veritabanındaki kullanıcıyla ilişkilendirilmiş<br />2 – kullanıcı tam olarak sağlandığından                         |
| UploadedByUserFirstName          | nvarchar(50)  | Evet         | Sözleşme karşıya kullanıcının ilk adını |
| UploadedByUserLastName           | nvarchar(50)  | Evet         | Sözleşme karşıya kullanıcının Soyadı |
| UploadedByUserEmailAddress       | nvarchar(255) | Evet         | Sözleşme karşıya kullanıcının e-posta adresi |

## <a name="vwapplicationrole"></a>vwApplicationRole

Bu görünüm, Azure Blockchain Workbench uygulamalarında tanımlı roller hakkında ayrıntılı bilgi sağlar.

İçinde bir *varlık aktarım* uygulama, örneğin, rolleri gibi *alıcı* ve *satıcı* rolleri tanımlanabilir.

| Ad                   | Type             | Can Be Null | Açıklama                                       |
|------------------------|------------------|-------------|---------------------------------------------------|
| ApplicationId          | int              | Hayır          | Uygulama için benzersiz bir tanımlayıcı           |
| ApplicationName        | nvarchar(50)     | Hayır          | Uygulamanın adı                       |
| ApplicationDescription | nvarchar(255)    | Evet         | Uygulama açıklaması                  |
| ApplicationDisplayName | nvarchar(255)    | Hayır          | Bir kullanıcı arabiriminde görüntülenecek adı      |
| Rol Kimliği                 | int              | Hayır          | Uygulama bir rol için benzersiz bir tanımlayıcı |
| Rol adı               | nvarchar50)      | Hayır          | Rol adı                              |
| RoleDescription        | Description(255) | Evet         | Rol açıklaması                         |

## <a name="vwapplicationroleuser"></a>vwApplicationRoleUser

Bu görünüm, Azure Blockchain Workbench uygulamalar ve bunlarla ilişkili kullanıcı tanımlı roller hakkında ayrıntılı bilgi sağlar.

İçinde bir *varlık aktarım* uygulama, örneğin, *John Smith* ile ilişkili *alıcı* rol.

| Ad                       | Type          | Can Be Null | Açıklama                                                                                                                                                                                                                           |
|----------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId              | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı                                                                                                                                                                                               |
| ApplicationName            | nvarchar(50)  | Hayır          | Uygulamanın adı                                                                                                                                                                                                           |
| ApplicationDescription     | nvarchar(255) | Evet         | Uygulama açıklaması                                                                                                                                                                                                      |
| ApplicationDisplayName     | nvarchar(255) | Hayır          | Bir kullanıcı arabiriminde görüntülenecek adı                                                                                                                                                                                          |
| ApplicationRoleId          | int           | Hayır          | Uygulama bir rol için benzersiz bir tanımlayıcı                                                                                                                                                                                     |
| ApplicationRoleName        | nvarchar50)   | Hayır          | Rol adı                                                                                                                                                                                                                  |
| ApplicationRoleDescription | nvarchar(255) | Evet         | Rol açıklaması                                                                                                                                                                                                             |
| UserId                     | int           | Hayır          | Rol ile ilişkili kullanıcı kimliği |
| UserExternalId             | nvarchar(255) | Hayır          | Dış tanımlayıcı rolüyle ilişkilendirilmiş kullanıcı. Varsayılan olarak, bu consortium için Azure Active Directory'den kullanıcı kimliğidir.                                                                     |
| UserProvisioningStatus     | int           | Hayır          | Sağlama işlemi kullanıcının geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 – kullanıcı API tarafından oluşturuldu<br />1 – bir anahtar veritabanındaki kullanıcıyla ilişkilendirilmiş<br />2 – kullanıcı tam olarak sağlandığından |
| UserFirstName              | nvarchar(50)  | Evet         | Rolle ilişkili kullanıcı adını |
| UserLastName               | nvarchar(255) | Evet         | Son rolüyle ilişkilendirilmiş kullanıcı adı |
| UserEmailAddress           | nvarchar(255) | Evet         | Rol ile ilişkili kullanıcının e-posta adresi |

## <a name="vwconnectionuser"></a>vwConnectionUser

Bu görünüm, Azure Blockchain Workbench içinde tanımlanan bağlantıları ve bunlarla ilişkili kullanıcıları ayrıntıları sağlar. Her bağlantı için bu görünümü olarak aşağıdaki verileri içerir:

-   İlişkili muhasebe ayrıntıları
-   İlişkili kullanıcı bilgileri

| Ad                     | Type          | Can Be Null | Açıklama                                                                                                                                                                                                                           |
|--------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ConnectionId             | int           | Hayır          | Azure Blockchain Workbench bağlantı için benzersiz tanımlayıcı |
| ConnectionEndpointUrl    | nvarchar(50)  | Hayır          | Bir bağlantı için uç nokta URL'si |
| ConnectionFundingAccount | nvarchar(255) | Evet         | Varsa bir bağlantıyla ilişkili fon hesabı |
| LedgerId                 | int           | Hayır          | Bir kayıt defteri için benzersiz tanımlayıcı |
| LedgerName               | nvarchar(50)  | Hayır          | Bir kayıt defteri adı |
| LedgerDisplayName        | nvarchar(255) | Hayır          | Kullanıcı Arabiriminde görüntülemek için bir kayıt defteri adı |
| UserId                   | int           | Hayır          | Bağlantı ile ilişkili kullanıcı kimliği |
| UserExternalId           | nvarchar(255) | Hayır          | Dış bağlantı ile ilişkili kullanıcı tanımlayıcısı. Varsayılan olarak, bu consortium için Azure Active Directory'den kullanıcı kimliğidir. |
| UserProvisioningStatus   | int           | Hayır          |Sağlama işlemi kullanıcının geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 – kullanıcı API tarafından oluşturuldu<br />1 – bir anahtar veritabanındaki kullanıcıyla ilişkilendirilmiş<br />2 – kullanıcı tam olarak sağlandığından |
| UserFirstName            | nvarchar(50)  | Evet         | Bağlantıyla ilişkili kullanıcı adını |
| UserLastName             | nvarchar(255) | Evet         | Son bağlantı ile ilişkili kullanıcı adı |
| UserEmailAddress         | nvarchar(255) | Evet         | Bağlantıyla ilişkili kullanıcının e-posta adresi |

## <a name="vwcontract"></a>vwContract

Bu görünüm, dağıtılan sözleşmeler hakkındaki ayrıntıları sağlar. Her bir sözleşme için bu görünümü olarak aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İlişkili muhasebe logrequest olayını işlevi
-   Eylem başlatan kullanıcı ayrıntıları
-   Blok zinciri blok ve işlem ile ilgili ayrıntıları

| Ad                                     | Type           | Can Be Null | Açıklama                                                                                                                                                                                                                                                   |
|------------------------------------------|----------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ConnectionId                             | int            | Hayır          | Azure Blockchain Workbench bağlantı için benzersiz tanımlayıcı.                                                                                                                                                                                         |
| ConnectionEndpointUrl                    | nvarchar(50)   | Hayır          | Bir bağlantı için uç nokta URL'si |
| ConnectionFundingAccount                 | nvarchar(255)  | Evet         | Varsa bir bağlantıyla ilişkili fon hesabı |
| LedgerId                                 | int            | Hayır          | Bir kayıt defteri için benzersiz tanımlayıcı |
| LedgerName                               | nvarchar(50)   | Hayır          | Bir kayıt defteri adı |
| LedgerDisplayName                        | nvarchar(255)  | Hayır          | Kullanıcı Arabiriminde görüntülemek için bir kayıt defteri adı |
| ApplicationId                            | int            | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                          | nvarchar (50)  | Hayır          | Uygulamanın adı |
| ApplicationDisplayName                   | nvarchar (255) | Hayır          | Bir kullanıcı arabiriminde görüntülenecek adı |
| ApplicationEnabled                       | bit            | Hayır          | Uygulama şu anda etkin olup olmadığını tanımlar.<br /> **Not:** Bir uygulama veritabanında devre dışı olarak yansıtılan olsa da, ilişkili sözleşmelerin uygun blok zinciri kalır ve bu sözleşmeler hakkındaki verileri veritabanında kalır.  |
| Workflowıd                               | int            | Hayır          | Bir sözleşme ile ilişkili iş akışı için benzersiz bir tanımlayıcı |
| WorkflowName                             | nvarchar(50)   | Hayır          | Bir sözleşme ile ilişkili iş akışının adı |
| WorkflowDisplayName                      | nvarchar(255)  | Hayır          | Kullanıcı arabiriminde görüntülenen sözleşme ile ilişkili iş akışının adı |
| WorkflowDescription                      | nvarchar(255)  | Evet         | Bir sözleşme ile ilişkili iş akışı tanımı |
| ContractCodeId                           | int            | Hayır          | Sözleşme ile ilişkili sözleşme kodu için benzersiz bir tanımlayıcı |
| ContractFileName                         | int            | Hayır          | Bu iş akışı için akıllı sözleşme kodu içeren dosyanın adı. |
| ContractUploadedDtTm                     | int            | Hayır          | Tarih ve saat sözleşme kodu yüklendi |
| ContractId                               | int            | Hayır          | Anlaşma için benzersiz tanımlayıcı |
| ContractProvisioningStatus               | int            | Hayır          | Anlaşma için sağlama işlemini geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 – sözleşme veritabanı API'si tarafından oluşturuldu<br />1 – sözleşme muhasebe gönderildi<br />2-anlaşma için bir kayıt defteri başarıyla dağıtıldı<br />3 veya 4 - sözleşme muhasebe dağıtılması başarısız oldu<br />5 - anlaşma için bir kayıt defteri başarıyla dağıtıldı <br /><br />Sürüm 1.5 başlayarak, değerleri 0 ile 5 desteklenir. İçin geriye dönük uyumluluk görünümü geçerli sürümde **vwContractV0** yalnızca destekler değerleri 0 ile 2 kullanılabilir. |
| ContractLedgerIdentifier                 | nvarchar (255) |             | Sözleşme dağıtılan kullanıcının e-posta adresi |
| ContractDeployedByUserId                 | int            | Hayır          | Dış tanımlayıcı sözleşme dağıtılan kullanıcı. Varsayılan olarak, bu kullanıcı için Azure Active Directory Kimliğini temsil eden GUID'i kimliğidir.                                                                                                          |
| ContractDeployedByUserExternalId         | nvarchar(255)  | Hayır          | Sözleşme dağıtılan kullanıcı için dış tanımlayıcı. Varsayılan olarak, bu kullanıcı için Azure Active Directory Kimliğini temsil eden GUID'i kimliğidir.                                                                                                         |
| ContractDeployedByUserProvisioningStatus | int            | Hayır          | Geçerli kullanıcı için sağlama işlemini durumunu tanımlar. Olası değerler şunlardır: <br />0 – kullanıcı API tarafından oluşturuldu<br />1 – bir anahtar veritabanındaki kullanıcıyla ilişkilendirilmiş <br />2 – kullanıcı tam olarak sağlandığından                     |
| ContractDeployedByUserFirstName          | nvarchar(50)   | Evet         | Sözleşme dağıtılan kullanıcının ilk adını |
| ContractDeployedByUserLastName           | nvarchar(255)  | Evet         | Sözleşme dağıtılan kullanıcının Soyadı |
| ContractDeployedByUserEmailAddress       | nvarchar(255)  | Evet         | Sözleşme dağıtılan kullanıcının e-posta adresi |

## <a name="vwcontractaction"></a>vwContractAction

Bu görünüm, sözleşmeler üzerinde gerçekleştirilen eylemler ile ilgili bilgi çoğunu gösterir ve kolayca yaygın raporlama senaryolarına kolaylaştırmak için tasarlanmıştır. Gerçekleştirilen her eylemi için bu görünümü olarak aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İlişkili akıllı sözleşme işlevi ve parametre tanımı
-   İlişkili muhasebe logrequest olayını işlevi
-   Belirli bir örneği değerleri için parametreler
-   Eylem başlatan kullanıcı ayrıntıları
-   Blok zinciri blok ve işlem ile ilgili ayrıntıları

| Ad                                     | Type          | Can Be Null | Açıklama                                                                                                                                                                                                                                                                                                    |
|------------------------------------------|---------------|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                            | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                          | nvarchar(50)  | Hayır          | Uygulamanın adı |
| ApplicationDisplayName                   | nvarchar(255) | Hayır          | Bir kullanıcı arabiriminde görüntülenecek adı |
| ApplicationEnabled                       | bit           | Hayır          | Uygulama şu anda etkin olduğunda bu alan tanımlar. – Uygulama veritabanında devre dışı olarak yansıtılan, ilişkili sözleşmelerin uygun blok zinciri kalır ve bu sözleşmeler hakkındaki verileri veritabanında kalır olsa da unutmayın.                                                  |
| Workflowıd                               | int           | Hayır          | Bir iş akışı için benzersiz bir tanımlayıcı |
| WorkflowName                             | nvarchar(50)  | Hayır          | İş akışının adı |
| WorkflowDisplayName                      | nvarchar(255) | Hayır          | Bir kullanıcı arabiriminde görüntülenecek iş akışının adı |
| WorkflowDescription                      | nvarchar(255) | Evet         | İş akışı tanımı |
| ContractId                               | int           | Hayır          | Anlaşma için benzersiz bir tanımlayıcı |
| ContractProvisioningStatus               | int           | Hayır          | Anlaşma için sağlama işlemini geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 – sözleşme veritabanı API'si tarafından oluşturuldu<br />1 – sözleşme muhasebe gönderildi<br />2-anlaşma için bir kayıt defteri başarıyla dağıtıldı<br />3 veya 4 - sözleşme muhasebe dağıtılması başarısız oldu<br />5 - anlaşma için bir kayıt defteri başarıyla dağıtıldı <br /><br />Sürüm 1.5 başlayarak, değerleri 0 ile 5 desteklenir. İçin geriye dönük uyumluluk görünümü geçerli sürümde **vwContractActionV0** yalnızca destekler değerleri 0 ile 2 kullanılabilir. |
| ContractCodeId                           | int           | Hayır          | Kod uygulaması sözleşme için benzersiz bir tanımlayıcı |
| ContractLedgerIdentifier                 | nvarchar(255) | Evet         | Dağıtılan belirli bir dağıtılmış kayıt defteri için akıllı bir sözleşme sürümü ile ilişkili benzersiz bir tanımlayıcı. Örneğin, Ethereum. |
| ContractDeployedByUserId                 | int           | Hayır          | Sözleşme dağıtılan kullanıcının benzersiz tanımlayıcısı |
| ContractDeployedByUserFirstName          | nvarchar(50)  | Evet         | Sözleşme dağıtılan kullanıcı adı |
| ContractDeployedByUserLastName           | nvarchar(255) | Evet         | Sözleşme dağıtılan kullanıcının Soyadı |
| ContractDeployedByUserExternalId         | nvarchar(255) | Hayır          | Dış sözleşme dağıtılan kullanıcı tanımlayıcısı. Varsayılan olarak, bu kimliği kimliklerini Azure Active Directory consortium içinde temsil eden GUID'dir.                                                                                                                                                |
| ContractDeployedByUserEmailAddress       | nvarchar(255) | Evet         | Sözleşme dağıtılan kullanıcının e-posta adresi |
| WorkflowFunctionId                       | int           | Hayır          | Bir iş akışı işlevi için benzersiz bir tanımlayıcı |
| WorkflowFunctionName                     | nvarchar(50)  | Hayır          | İşlev adı |
| WorkflowFunctionDisplayName              | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek bir işlevin adı |
| WorkflowFunctionDescription              | nvarchar(255) | Hayır          | İşlev açıklaması |
| ContractActionId                         | int           | Hayır          | Bir sözleşme eylemi için benzersiz tanımlayıcı |
| ContractActionProvisioningStatus         | int           | Hayır          | Sözleşme eylemi için sağlama işlemini geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 – sözleşme eylem veritabanı API'si tarafından oluşturuldu<br />1 – sözleşme eylemi muhasebe gönderildi<br />2-sözleşme eylem için bir kayıt defteri başarıyla dağıtıldı<br />3 veya 4 - sözleşme muhasebe dağıtılması başarısız oldu<br />5 - anlaşma için bir kayıt defteri başarıyla dağıtıldı <br /><br />Sürüm 1.5 başlayarak, değerleri 0 ile 5 desteklenir. İçin geriye dönük uyumluluk görünümü geçerli sürümde **vwContractActionV0** yalnızca destekler değerleri 0 ile 2 kullanılabilir. |
| ContractActionTimestamp                  | DateTime(2,7) | Hayır          | Sözleşme eylem zaman damgası |
| ContractActionExecutedByUserId           | int           | Hayır          | Sözleşme eylem yürütüldü kullanıcının benzersiz tanımlayıcısı |
| ContractActionExecutedByUserFirstName    | int           | Evet         | Sözleşme eylemi gerçekleştiren kullanıcının adı |
| ContractActionExecutedByUserLastName     | nvarchar(50)  | Evet         | Sözleşme eylemi gerçekleştiren kullanıcının Soyadı |
| ContractActionExecutedByUserExternalId   | nvarchar(255) | Evet         | Sözleşme eylemi gerçekleştiren kullanıcının dış tanımlayıcısı. Varsayılan olarak, bu kimliği kimliklerini Azure Active Directory consortium içinde temsil eden GUID'dir. |
| ContractActionExecutedByUserEmailAddress | nvarchar(255) | Evet         | Sözleşme eylemi gerçekleştiren kullanıcının e-posta adresi |
| WorkflowFunctionParameterId              | int           | Hayır          | İşlevinin bir parametresi için benzersiz bir tanımlayıcı |
| WorkflowFunctionParameterName            | nvarchar(50)  | Hayır          | İşlevinin bir parametresi adı |
| WorkflowFunctionParameterDisplayName     | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek bir işlev parametresinin adı |
| WorkflowFunctionParameterDataTypeId      | int           | Hayır          | Bir iş akışı işlevi parametresi ile ilişkili veri türü için benzersiz tanımlayıcı |
| WorkflowParameterDataTypeName            | nvarchar(50)  | Hayır          | Bir iş akışı işlevi parametresi ile ilişkili bir veri türü adı |
| ContractActionParameterValue             | nvarchar(255) | Hayır          | Akıllı sözleşmede depolanan parametre değeri |
| BlockHash                                | nvarchar(255) | Evet         | Blok karma |
| BlockNumber                              | int           | Evet         | Genel muhasebe blok sayısı |
| BlockTimestamp                           | DateTime(2,7) | Evet         | Bloğun zaman damgası |
| TransactionID                            | int           | Hayır          | İşlem için benzersiz bir tanımlayıcı |
| TransactionFrom                          | nvarchar(255) | Evet         | İşlem kaynağı taraf |
| TransactionTo                            | nvarchar(255) | Evet         | İle işlem temelli taraf |
| TransactionHash                          | nvarchar(255) | Evet         | Bir işlem karması |
| TransactionIsWorkbenchTransaction        | bit           | Evet         | Azure Blockchain Workbench işlem hareket ise tanımlayan bir bit |
| TransactionProvisioningStatus            | int           | Evet         | Geçerli işlem için sağlama işlemini durumunu tanımlar. Olası değerler şunlardır: <br />0 – işlem veritabanı API'si tarafından oluşturuldu<br />1 – işlem defterine gönderildi<br />2 – işlem için bir kayıt defteri başarıyla dağıtıldı                 |
| TransactionValue                         | decimal(32,2) | Evet         | İşlem değeri |

## <a name="vwcontractproperty"></a>vwContractProperty

Bu görünüm bir sözleşme ile ilişkili özellikler ilgili bilgilerin çoğunu gösterir ve kolayca yaygın raporlama senaryolarına kolaylaştırmak için tasarlanmıştır. Gerçekleştirilen her bir özellik için bu görünümü olarak aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İş akışı dağıtılan kullanıcı ayrıntıları
-   İlişkili akıllı sözleşme özellik tanımı
-   Özellikler için belirli bir örnek değerler
-   Ayrıntılar için Sözleşme durumu özelliği

| Ad                               | Type          | Can Be Null | Açıklama                                                                                                                                                                                                                                                                        |
|------------------------------------|---------------|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                      | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                    | nvarchar(50)  | Hayır          | Uygulamanın adı |
| ApplicationDisplayName             | nvarchar(255) | Hayır          | Bir kullanıcı arabiriminde görüntülenecek adı |
| ApplicationEnabled                 | bit           | Hayır          | Uygulama şu anda etkin olup olmadığını tanımlar.<br />**Not:** Bir uygulama veritabanında devre dışı olarak yansıtılan olsa da, ilişkili sözleşmelerin uygun blok zinciri kalır ve bu sözleşmeler hakkındaki verileri veritabanında kalır.                      |
| Workflowıd                         | int           | Hayır          | İş akışı için benzersiz tanımlayıcı |
| WorkflowName                       | nvarchar(50)  | Hayır          | İş akışının adı |
| WorkflowDisplayName                | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenen iş akışının adı |
| WorkflowDescription                | nvarchar(255) | Evet         | İş akışı tanımı |
| ContractId                         | int           | Hayır          | Anlaşma için benzersiz tanımlayıcı |
| ContractProvisioningStatus         | int           | Hayır          | Anlaşma için sağlama işlemini geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 – sözleşme veritabanı API'si tarafından oluşturuldu<br />1 – sözleşme muhasebe gönderildi<br />2-anlaşma için bir kayıt defteri başarıyla dağıtıldı<br />3 veya 4 - sözleşme muhasebe dağıtılması başarısız oldu<br />5 - anlaşma için bir kayıt defteri başarıyla dağıtıldı <br /><br />Sürüm 1.5 başlayarak, değerleri 0 ile 5 desteklenir. İçin geriye dönük uyumluluk görünümü geçerli sürümde **vwContractPropertyV0** yalnızca destekler değerleri 0 ile 2 kullanılabilir. |
| ContractCodeId                     | int           | Hayır          | Kod uygulaması sözleşme için benzersiz bir tanımlayıcı |
| ContractLedgerIdentifier           | nvarchar(255) | Evet         | Dağıtılan belirli bir dağıtılmış kayıt defteri için akıllı bir sözleşme sürümü ile ilişkili benzersiz bir tanımlayıcı. Örneğin, Ethereum. |
| ContractDeployedByUserId           | int           | Hayır          | Sözleşme dağıtılan kullanıcının benzersiz tanımlayıcısı |
| ContractDeployedByUserFirstName    | nvarchar(50)  | Evet         | Sözleşme dağıtılan kullanıcı adı |
| ContractDeployedByUserLastName     | nvarchar(255) | Evet         | Sözleşme dağıtılan kullanıcının Soyadı |
| ContractDeployedByUserExternalId   | nvarchar(255) | Hayır          | Dış sözleşme dağıtılan kullanıcı tanımlayıcısı. Varsayılan olarak, bu kimliği kimliklerini Azure Active Directory consortium içinde temsil eden GUID olur. |
| ContractDeployedByUserEmailAddress | nvarchar(255) | Evet         | Sözleşme dağıtılan kullanıcının e-posta adresi |
| WorkflowPropertyId                 | int           |             | Bir iş akışının bir özellik için benzersiz bir tanımlayıcı |
| WorkflowPropertyDataTypeId         | int           | Hayır          | Özelliğin veri türü kimliği |
| WorkflowPropertyDataTypeName       | nvarchar(50)  | Hayır          | Özelliğin veri türü adı |
| WorkflowPropertyName               | nvarchar(50)  | Hayır          | İş akışı özelliğin adı |
| WorkflowPropertyDisplayName        | nvarchar(255) | Hayır          | İş akışı özellik görünen adı |
| WorkflowPropertyDescription        | nvarchar(255) | Evet         | Özellik açıklaması |
| ContractPropertyValue              | nvarchar(255) | Hayır          | Sözleşmenin bir özellik değeri |
| Statename'i                          | nvarchar(50)  | Evet         | Bu özellik, sözleşmeyi durumunu içeriyorsa, durumu için görünen ad var. Durumu ile ilişkili değilse, değeri null olacaktır. |
| StateDisplayName                   | nvarchar(255) | Hayır          | Bu özellik durumu içeriyorsa, durumu için görünen ad var. Durumu ile ilişkili değilse, değeri null olacaktır. |
| StateValue                         | nvarchar(255) | Evet         | Bu özellik durumu içeriyorsa, bu durum değerdir. Durumu ile ilişkili değilse, değeri null olacaktır. |

## <a name="vwcontractstate"></a>vwContractState

Bu görünüm, belirli bir sözleşme durumunu ilgili bilgilerin çoğunu gösterir ve kolayca yaygın raporlama senaryolarına kolaylaştırmak için tasarlanmıştır. Bu görünümde her bir kayıt olarak aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İş akışı dağıtılan kullanıcı ayrıntıları
-   İlişkili akıllı sözleşme özellik tanımı
-   Ayrıntılar için Sözleşme durumu özelliği

| Ad                               | Type          | Can Be Null | Açıklama                                                                                                                                                                                                                                                                        |
|------------------------------------|---------------|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                      | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                    | nvarchar(50)  | Hayır          | Uygulamanın adı |
| ApplicationDisplayName             | nvarchar(255) | Hayır          | Bir kullanıcı arabiriminde görüntülenecek adı |
| ApplicationEnabled                 | bit           | Hayır          | Uygulama şu anda etkin olup olmadığını tanımlar.<br />**Not:** Bir uygulama veritabanında devre dışı olarak yansıtılan olsa da, ilişkili sözleşmelerin uygun blok zinciri kalır ve bu sözleşmeler hakkındaki verileri veritabanında kalır. |
| Workflowıd                         | int           | Hayır          | İş akışı için benzersiz bir tanımlayıcı |
| WorkflowName                       | nvarchar(50)  | Hayır          | İş akışının adı |
| WorkflowDisplayName                | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenen adı |
| WorkflowDescription                | nvarchar(255) | Evet         | İş akışı tanımı |
| ContractLedgerImplementationId     | nvarchar(255) | Evet         | Dağıtılan belirli bir dağıtılmış kayıt defteri için akıllı bir sözleşme sürümü ile ilişkili benzersiz bir tanımlayıcı. Örneğin, Ethereum. |
| ContractId                         | int           | Hayır          | Anlaşma için benzersiz bir tanımlayıcı |
| ContractProvisioningStatus         | int           | Hayır          |Anlaşma için sağlama işlemini geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 – sözleşme veritabanı API'si tarafından oluşturuldu<br />1 – sözleşme muhasebe gönderildi<br />2-anlaşma için bir kayıt defteri başarıyla dağıtıldı<br />3 veya 4 - sözleşme muhasebe dağıtılması başarısız oldu<br />5 - anlaşma için bir kayıt defteri başarıyla dağıtıldı <br /><br />Sürüm 1.5 başlayarak, değerleri 0 ile 5 desteklenir. İçin geriye dönük uyumluluk görünümü geçerli sürümde **vwContractStateV0** yalnızca destekler değerleri 0 ile 2 kullanılabilir. |
| ConnectionId                       | int           | Hayır          | Blok zinciri iş akışı örneği için benzersiz bir tanımlayıcı dağıtılır |
| ContractCodeId                     | int           | Hayır          | Kod uygulaması sözleşme için benzersiz bir tanımlayıcı |
| ContractDeployedByUserId           | int           | Hayır          | Sözleşme dağıtılan kullanıcının benzersiz tanımlayıcısı |
| ContractDeployedByUserExternalId   | nvarchar(255) | Hayır          | Dış sözleşme dağıtılan kullanıcı tanımlayıcısı. Varsayılan olarak, bu kimliği kimliklerini Azure Active Directory consortium içinde temsil eden GUID'dir. |
| ContractDeployedByUserFirstName    | nvarchar(50)  | Evet         | Sözleşme dağıtılan kullanıcı adı |
| ContractDeployedByUserLastName     | nvarchar(255) | Evet         | Sözleşme dağıtılan kullanıcının Soyadı |
| ContractDeployedByUserEmailAddress | nvarchar(255) | Evet         | Sözleşme dağıtılan kullanıcının e-posta adresi |
| WorkflowPropertyId                 | int           | Hayır          | İş akışı bir özellik için benzersiz bir tanımlayıcı |
| WorkflowPropertyDataTypeId         | int           | Hayır          | İş akışı özelliğinin veri türü kimliği |
| WorkflowPropertyDataTypeName       | nvarchar(50)  | Hayır          | İş akışı özelliğinin veri türünün adı |
| WorkflowPropertyName               | nvarchar(50)  | Hayır          | İş akışı özelliğin adı |
| WorkflowPropertyDisplayName        | nvarchar(255) | Hayır          | Özelliğini bir kullanıcı Arabiriminde görünen adı |
| WorkflowPropertyDescription        | nvarchar(255) | Evet         | Özellik açıklaması |
| ContractPropertyValue              | nvarchar(255) | Hayır          | Sözleşmede depolanan bir özellik için değer |
| Statename'i                          | nvarchar(50)  | Evet         | Bu özellik durumu içeriyorsa, bu görünen ad durumu. Durumu ile ilişkili değilse, değeri null olacaktır. |
| StateDisplayName                   | nvarchar(255) | Hayır          | Bu özellik durumu içeriyorsa, durumu için görünen ad var. Durumu ile ilişkili değilse, değeri null olacaktır. |
| StateValue                         | nvarchar(255) | Evet         | Bu özellik durumu içeriyorsa, bu durum değerdir. Durumu ile ilişkili değilse, değeri null olacaktır. |

## <a name="vwuser"></a>vwUser

Bu görünüm, Azure Blockchain Workbench'i kullanabilmeniz için sağlanan consortium üyeler ayrıntıları sağlar. Varsayılan olarak, verileri ilk sağlama kullanıcısı ile doldurulur.

| Ad               | Type          | Can Be Null | Açıklama                                                                                                                                                                                                                               |
|--------------------|---------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kimlik                 | int           | Hayır          | Bir kullanıcı için benzersiz bir tanımlayıcı |
| externalID =         | nvarchar(255) | Hayır          | Bir kullanıcı için dış tanımlayıcı. Varsayılan olarak, bu kullanıcı için Azure Active Directory Kimliğini temsil eden GUID'i kimliğidir. |
| ProvisioningStatus | int           | Hayır          |Sağlama işlemi kullanıcının geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 – kullanıcı API tarafından oluşturuldu<br />1 – bir anahtar veritabanındaki kullanıcıyla ilişkilendirilmiş<br />2 – kullanıcı tam olarak sağlandığından |
| FirstName          | nvarchar(50)  | Evet         | Kullanıcı adı |
| LastName           | nvarchar(50)  | Evet         | Kullanıcının soyadı |
| EmailAddress       | nvarchar(255) | Evet         | Kullanıcının e-posta adresi |

## <a name="vwworkflow"></a>vwWorkflow

Bu görünümün Ayrıntılar çekirdek iş akışı meta veriler hem de iş akışının işlevleri ve parametreleri temsil eder. Raporlama için tasarlanmış, iş akışı ile ilişkili uygulama hakkındaki meta verileri de içerir. Bu görünüm, iş akışlarında raporlama kolaylaştırmak için birden çok temel alınan tablodaki verileri içerir. Her iş akışı için bu görünümü olarak aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İlişkili iş akışı başlatma durumu bilgileri

| Ad                              | Type          | Can Be Null | Açıklama                                                                                                                                |
|-----------------------------------|---------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                     | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                   | nvarchar(50)  | Hayır          | Uygulamanın adı |
| ApplicationDisplayName            | nvarchar(255) | Hayır          | Bir kullanıcı arabiriminde görüntülenecek adı |
| ApplicationEnabled                | bit           | Hayır          | Uygulama etkin olup olmadığını tanımlar |
| Workflowıd                        | int           | Evet         | Bir iş akışı için benzersiz bir tanımlayıcı |
| WorkflowName                      | nvarchar(50)  | Hayır          | İş akışının adı |
| WorkflowDisplayName               | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenen adı |
| WorkflowDescription               | nvarchar(255) | Evet         | İş akışı tanımı. |
| WorkflowConstructorFunctionId     | int           | Hayır          | İş akışı için oluşturucu olarak hizmet veren iş akışı işlevinin tanımlayıcısı |
| WorkflowStartStateId              | int           | Hayır          | Durumu için benzersiz bir tanımlayıcı |
| WorkflowStartStateName            | nvarchar(50)  | Hayır          | Durum adı |
| WorkflowStartStateDisplayName     | nvarchar(255) | Hayır          | Kullanıcı arabiriminde durumu için görüntülenecek adı |
| WorkflowStartStateDescription     | nvarchar(255) | Evet         | İş akışı durumu açıklaması |
| WorkflowStartStateStyle           | nvarchar(50)  | Evet         | İş akışı Bu durumdayken olduğunu tamamlanma yüzdesi bu değeri tanımlar |
| WorkflowStartStateValue           | int           | Hayır          | Durum değeri |
| WorkflowStartStatePercentComplete | int           | Hayır          | Bu durum kullanıcı arabiriminde işlemek nasıl istemcilere bir ipucu sağlar. bir metin açıklaması. Desteklenen durumları *başarı* ve *hatası* |

## <a name="vwworkflowfunction"></a>vwWorkflowFunction

Bu görünümün Ayrıntılar çekirdek iş akışı meta veriler hem de iş akışının işlevleri ve parametreleri temsil eder. Raporlama için tasarlanmış, iş akışı ile ilişkili uygulama hakkındaki meta verileri de içerir. Bu görünüm, iş akışlarında raporlama kolaylaştırmak için birden çok temel alınan tablodaki verileri içerir. Her iş akışı işlevi için bu görünümü olarak aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İş akışı işlev ayrıntıları

| Ad                                 | Type          | Can Be Null | Açıklama                                                                          |
|--------------------------------------|---------------|-------------|--------------------------------------------------------------------------------------|
| ApplicationId                        | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                      | nvarchar(50)  | Hayır          | Uygulamanın adı |
| ApplicationDisplayName               | nvarchar(255) | Hayır          | Bir kullanıcı arabiriminde görüntülenecek adı |
| ApplicationEnabled                   | bit           | Hayır          | Uygulama etkin olup olmadığını tanımlar |
| Workflowıd                           | int           | Hayır          | Bir iş akışı için benzersiz bir tanımlayıcı |
| WorkflowName                         | nvarchar(50)  | Hayır          | İş akışının adı |
| WorkflowDisplayName                  | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenen iş akışının adı |
| WorkflowDescription                  | nvarchar(255) | Evet         | İş akışı tanımı |
| WorkflowFunctionId                   | int           | Hayır          | Bir işlev için benzersiz bir tanımlayıcı |
| WorkflowFunctionName                 | nvarchar(50)  | Evet         | İşlev adı |
| WorkflowFunctionDisplayName          | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek bir işlevin adı |
| WorkflowFunctionDescription          | nvarchar(255) | Evet         | İş akışı işlevinin açıklaması |
| WorkflowFunctionIsConstructor        | bit           | Hayır          | İş akışı işlevi iş akışı için oluşturucu olup olmadığını tanımlar |
| WorkflowFunctionParameterId          | int           | Hayır          | Bir işlev parametresi için benzersiz bir tanımlayıcı |
| WorkflowFunctionParameterName        | nvarchar(50)  | Hayır          | İşlevinin bir parametresi adı |
| WorkflowFunctionParameterDisplayName | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek bir işlev parametresinin adı |
| WorkflowFunctionParameterDataTypeId  | int           | Hayır          | Bir iş akışı işlevi parametresi ile ilişkili veri türü için benzersiz bir tanımlayıcı |
| WorkflowParameterDataTypeName        | nvarchar(50)  | Hayır          | Bir iş akışı işlevi parametresi ile ilişkili bir veri türü adı |

## <a name="vwworkflowproperty"></a>vwWorkflowProperty

Bu görünüm için bir iş akışı tanımlı özelliklerini temsil eder. Her bir özellik için bu görünümü olarak aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İş akışı özellik ayrıntıları

| Ad                         | Type          | Can Be Null | Açıklama                                                                                                                                                                                                                                                   |
|------------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName              | nvarchar(50)  | Hayır          | Uygulamanın adı |
| ApplicationDisplayName       | nvarchar(255) | Hayır          | Bir kullanıcı arabiriminde görüntülenecek adı |
| ApplicationEnabled           | bit           | Hayır          | Uygulama şu anda etkin olup olmadığını tanımlar.<br />**Not:** Bir uygulama veritabanında devre dışı olarak yansıtılan olsa da, ilişkili sözleşmelerin uygun blok zinciri kalır ve bu sözleşmeler hakkındaki verileri veritabanında kalır. |
| Workflowıd                   | int           | Hayır          | İş akışı için benzersiz bir tanımlayıcı |
| WorkflowName                 | nvarchar(50)  | Hayır          | İş akışının adı |
| WorkflowDisplayName          | nvarchar(255) | Hayır          | Bir kullanıcı arabirimi iş akışında için görüntülenecek adı |
| WorkflowDescription          | nvarchar(255) | Evet         | İş akışının açıklamasını |
| WorkflowPropertyID           | int           | Hayır          | Bir iş akışının bir özellik için benzersiz bir tanımlayıcı |
| WorkflowPropertyName         | nvarchar(50)  | Hayır          | Özelliğin adı |
| WorkflowPropertyDescription  | nvarchar(255) | Evet         | Özellik açıklaması |
| WorkflowPropertyDisplayName  | nvarchar(255) | Hayır          | Bir kullanıcı arabiriminde görüntülenecek adı |
| WorkflowPropertyWorkflowId   | int           | Hayır          | Bu özelliğin ilişkili olduğu iş akışı kimliği |
| WorkflowPropertyDataTypeId   | int           | Hayır          | Özelliği için tanımlanan veri türü kimliği |
| WorkflowPropertyDataTypeName | nvarchar(50)  | Hayır          | Özelliği için tanımlanan veri türünün adı |
| WorkflowPropertyIsState      | bit           | Hayır          | Bu iş akışının özellik iş akışı durumunu içeriyorsa, bu alan tanımlar. |

## <a name="vwworkflowstate"></a>vwWorkflowState

Bu görünüm, bir iş akışı ile ilişkili özellikleri temsil eder. Her bir sözleşme için bu görünümü olarak aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İş akışı durumu bilgileri

| Ad                         | Type          | Can Be Null | Açıklama                                                                                                                                                                                                                                                   |
|------------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationId                | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName              | nvarchar(50)  | Hayır          | Uygulamanın adı |
| ApplicationDisplayName       | nvarchar(255) | Hayır          | Uygulama açıklaması |
| ApplicationEnabled           | bit           | Hayır          | Uygulama şu anda etkin olup olmadığını tanımlar.<br />**Not:** Bir uygulama veritabanında devre dışı olarak yansıtılan olsa da, ilişkili sözleşmelerin uygun blok zinciri kalır ve bu sözleşmeler hakkındaki verileri veritabanında kalır. |
| Workflowıd                   | int           | Hayır          | İş akışı için benzersiz tanımlayıcı |
| WorkflowName                 | nvarchar(50)  | Hayır          | İş akışının adı |
| WorkflowDisplayName          | nvarchar(255) | Hayır          | İş akışı için kullanıcı arabiriminde görüntülenen adı |
| WorkflowDescription          | nvarchar(255) | Evet         | İş akışı tanımı |
| WorkflowStateID              | int           | Hayır          | Durumu için benzersiz tanımlayıcı |
| WorkflowStateName            | nvarchar(50)  | Hayır          | Durum adı |
| WorkflowStateDisplayName     | nvarchar(255) | Hayır          | Kullanıcı arabiriminde durumu için görüntülenecek adı |
| WorkflowStateDescription     | nvarchar(255) | Evet         | İş akışı durumu açıklaması |
| WorkflowStatePercentComplete | int           | Hayır          | İş akışı Bu durumdayken olduğunu tamamlanma yüzdesi bu değeri tanımlar |
| WorkflowStateValue           | nvarchar(50)  | Hayır          | Durum değeri |
| WorkflowStateStyle           | nvarchar(50)  | Hayır          | Bu durum kullanıcı arabiriminde işlemek nasıl istemcilere bir ipucu sağlar. bir metin açıklaması. Desteklenen durumları *başarı* ve *hatası* |
