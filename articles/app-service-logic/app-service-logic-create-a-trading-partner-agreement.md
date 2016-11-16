---
title: "Azure Uygulama Hizmeti’nde Ticari Ortak Sözleşmesi Oluşturma | Microsoft Belgeleri"
description: "Ticari Ortak Sözleşmeleri Oluşturma"
services: logic-apps
documentationcenter: .net,nodejs,java
author: rajram
manager: erikre
editor: 
ms.assetid: 319e46fa-fd81-4730-a742-768bf1676972
ms.service: logic-apps
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/23/2016
ms.author: rajram
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: e41ac0e91bd66fbc7df08b4397e78377021fcbca


---
# <a name="creating-a-trading-partner-agreement"></a>Ticari Ortak Sözleşmesi Oluşturma
[!INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Ticari ortaklar B2B (işletmeler arası) iletişimde rol alan varlıklardır. İki ortak bir ilişki oluşturduğunda buna *Sözleşme* adı verilir. Tanımlanan sözleşme, iki ortağın gerçekleştirmek istediği iletişimi temel alır ve protokole ya da aktarıma özeldir. Azure App Service tarafından desteklenen çeşitli B2B protokolleri ve aktarımları şunlardır:

* AS2 (Uygulanabilirlik Deyimi 2)
* EDIFACT (Birleşmiş Milletler/Yönetim, Ticaret ve Taşıma için Elektronik Veri Değişimi (UN/EDIFACT))
* X12 (ASC X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>B2B senaryolarını destekleyen BizTalk API Uygulamaları
Aşağıdaki API Uygulamaları Azure Portalı'nda zengin ve sezgisel bir deneyim kullanarak şu olanakları sağlar:

## <a name="biztalk-trading-partner-management-tpm"></a>BizTalk Ticari Ortak Yönetimi (TPM)
* İş Ortakları, Profiller ve Kimliklerin oluşturulması ve yönetimi
* EDI şemalarının depolama ve yönetimi
* Sertifikaların depolama ve yönetimi (AS2 protokolünde kullanılır)
* AS2 sözleşmelerinin oluşturulması ve yönetimi
* EDIFACT Sözleşmelerinin oluşturulması ve yönetimi (gönderme tarafında toplu iş oluşturmayı içerir)
* X12 Sözleşmelerinin oluşturulması ve yönetimi (gönderme tarafında toplu iş oluşturmayı içerir)

![][1]

## <a name="as2-connector"></a>AS2 Bağlayıcısı
* İlgili TPM API App örneğinde tanımlanan AS2 Sözleşmelerini yürütür
* Sorun giderme için AS2 işleme/izleme bilgilerini ortaya çıkarır

## <a name="biztalk-edifact"></a>BizTalk EDIFACT
* İlgili TPM API App örneğinde tanımlanan EDIFACT Sözleşmelerini yürütür
* Sorun giderme için EDIFACT işleme/izleme bilgilerini ortaya çıkarır
* İlgili TPM API App örneğindeki EDIFACT Sözleşmelerinde tanımlandığı gibi toplu işlemlerin (başlatma ve durdurma) durum yönetimini sağlar

## <a name="biztalk-x12"></a>BizTalk X12
* İlgili TPM API App örneğinde tanımlanan X12 Sözleşmelerini yürütür 
* Sorun giderme için X12 işleme/izleme bilgilerini ortaya çıkarır
* İlgili TPM API App örneğindeki X12 Sözleşmelerinde tanımlandığı gibi toplu işlemlerin (başlatma ve durdurma) durum yönetimini sağlar

Daha önce belirtildiği gibi AS2, X12 ve EDIFACT API Uygulamaları’nın beklenen şekilde çalışması için bir TPM API Uygulaması gereklidir.

## <a name="getting-started"></a>Başlarken
Ticari ortak sözleşmeleri oluşturmak için:

1. **BizTalk Ticari Ortak Yönetimi** bağlayıcısının bir örneğini oluşturun. Bunun çalışması için boş bir SQL Veritabanı gereklidir. Başlamadan önce boş bir veritabanı mevcut ve kullanıma hazır olmalıdır.
2. Sözleşmelerin gerektirdiği şekilde şemaları ve sertifikaları karşıya yükleyin. Bunu yapmak için oluşturulan TPM örneğine göz atın ve 'Şemalar' ve/veya 'Sertifikalar' bölümüne gidin
3. Oluşturulan TPM örneğine göz atma ve **Ortaklar** bölümüne girme
4. İstediğiniz ortakları oluşturun. Ayrıca profilleri uygun şekilde düzenleyin ve gerekli kimlikleri ekleyin
5. Bundan sonra **Sözleşmeler** bölümünü kullanarak sözleşmeler oluşturun. Bir Sözleşme oluşturduğunuzda kullanılacak protokolü seçmeniz gerekir. Kalan yapılandırma seçenekleri seçtiğiniz protokolü temel alır.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png




<!--HONumber=Nov16_HO2-->


