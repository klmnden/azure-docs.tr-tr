---
title: Dynamics 365 Customer Engagement uygulaması için Test Sürüşü sekmesi - Azure Marketi'nde teklif | Microsoft Docs
description: Test sürüşü için Dynamics 365 müşteri katılımı AppSource Market'te uygulama teklif için yapılandırma.
services: Azure, Marketplace, AppSource, Cloud Partner Portal, Dynamics 365 for Customer Engagement
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/25/2018
ms.author: pbutlerm
ms.openlocfilehash: 373312b4c7f05fe41c9ca8165b8ff6f1b0e56f1b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60322864"
---
# <a name="dynamics-365-for-customer-engagement-application-test-drive-tab"></a>Dynamics 365 müşteri katılımı uygulama Test Sürüşü sekmesi

Kullanım **Test Sürüşü** müşterileriniz için bir deneme sürümü deneyimi oluşturmak için sekmesinde.  Müşteriler teklife ilişkin temel özellikler ve avantajları, gerçek uygulama senaryoda gösterildiği uygulamalı, kendi kendine yol gösteren denemesini sağlar.  Deneme seçenekleri, Test Sürüşü en verimli oluşturma yüksek kaliteli bir müşteri adayları ve artan dönüştürme bu müşteri adayları var.  Daha fazla bilgi için [Test Sürüşü nedir?](../test-drive/what-is-test-drive.md)

Dynamics 365 uygulamaları için Test Sürüşü deneyimi otomatik olarak Microsoft tarafından barındırılan bir çözüm olarak çalıştırılır.  Daha fazla bilgi için [barındırılan Test Sürüşü](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/hosted-test-drive).

Test Sürüşü sekmesinde üç olası bölümü vardır: **Sürücü test**, **ayrıntıları**, ve **teknik yapılandırma**.  Test Sürüşü işlevselliği etkinleştirdikten sonra son iki bölüm yalnızca görüntülenir.  Gerekli alan adına eklenmiş bir yıldız işareti (*) gösterir. 


## <a name="test-drive-section"></a>Test sürücü bölümü

Bu işlevselliği etkinleştirmek için işaretleyin **Evet** için **Test Sürüşü etkinleştirme**.


## <a name="details-section"></a>Ayrıntıları bölümü

Temel Test Sürüşü bilgileri sağlayacak **ayrıntıları** bölümü.   

![Test Sürüşü ayrıntıları bölümünü](./media/test-drive-tab-details.png)

Aşağıdaki tablo, Dynamics 365 uygulaması için test sürüşü kurmak için gereken alanları açıklar.

|      Alan                    |    Açıklama                  |
|    ---------                  |  ---------------                |
|      Açıklama              |   Test Sürüşünüz yapılabilir açıklanmaktadır. Bu açıklama Biçimlendirilecek temel HTML etiketlerini kullanabilirsiniz. Örneğin, &lt;p&gt;, &lt;em&gt;, &lt;ul&gt;, &lt;li&gt;, &lt;ol&gt;ve bölüm başlıkları.  |
|  Kullanıcı el kitabı                  |   Müşterilerin Test Sürüşü deneyimi boyunca size yol için kullanabileceğiniz bir kullanıcı el ile yükleyin. Bu belgede bir .pdf dosyası olmalıdır.              |
|  Test (isteğe bağlı) sürücü tanıtım videosu |  Test Sürüşünüz video bir kılavuz sağlar. Bir müşteri, bunlar bir test sürüşü önce bu videoyu izleyebilirsiniz. YouTube veya Vimeo video URL'sini sağlayın. Seçerseniz **+ Video ekleme**, aşağıdaki bilgileri sağlamanız istenir:<ul><li>Ad</li><li>URL'si</li><li>Küçük resim (PNG biçiminde, 533 x 324 piksel)</li></ul>  |
|   |   |


## <a name="technical-configuration-section"></a>Teknik yapılandırma bölümü

Bu bölümde, test sürüşünüz hakkında teknik ayrıntılar sağlayacaktır.

![Test Sürüşü ayrıntıları bölümünü](./media/test-drive-tab-tech-config.png)

Alanları aşağıdaki amaçlara sahip olduğu:

|      Alan                    |    Açıklama                  |
|    ---------                  |  ---------------                |
| Test Sürüşü türü            | Seçin **Microsoft barındırılan (Dynamics 365 müşteri katılımı için)**.  |
| Maksimum eşzamanlı Test Sürüşleri    | Herhangi bir anda zaman etkin bir Test Sürüşü eş zamanlı örnekleri sayısı. Test Sürüşü kullanıcılar için kullanılabilir olan en az bu kadar çok Dynamics lisans olduğundan emin olmak ihtiyacınız olacak şekilde, Test Sürüşü etkin olduğu sürece her kullanıcı bir Dynamics lisansı kullanacaktır. Önerilen değeri 3-5.  |
| Test sürücü süresi (saat)   | En fazla saat sayısı kullanıcının Test Sürüşü örneği için etkin olacaktır. Bu süre aşılırsa sonra örnek kiracınızdan sağlaması. Uygulamanızı karmaşıklığına bağlı olarak 2-24 saat değerini önerilir. Çalışma zamanı dışında ve yeniden değerlendirmek istediğiniz başka bir Test Sürüşü kullanıcı her zaman talep edebilir.  |
| Örnek URL                  | Test Sürüşü başlangıçta gideceği URL'yi. Uygulamanızın vardır, Dynamics 365 örneğinizin URL'si genellikle budur ve örnek veriler üzerinde yüklü.  |
| Azure AD Kiracı kimliği            | Dynamics 365 örneğinizin Azure kiracısı GUİD'si. Bu değer, Azure portalında oturum açın alıp gidin **Azure Active Directory** > **Özellikleri Seç** > **dizinkimliğikopyalayın**.  |
| Azure AD uygulama kimliği               | Azure AD uygulamanızın GUID  |
| Azure AD uygulama anahtarı              | Örneğin, Azure AD uygulamanızın gizli anahtarı: `IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=` |
| Azure AD Kiracı adı          | Dynamics 365 örneğinizin Azure Kiracı adı. Örneğin < kiracıadı. > onmicrosoft.com, biçimi kullanın: `testdrive.onmicrosoft.com`  |
| Örnek Web API URL'si          | Dynamics 365 Örneğiniz için Web API URL'si. Bu değer, Microsoft Dynamics 365 örneğine günlüğe kaydetme ve giderek alabilirsiniz **ayarları** > **özelleştirme** > **Geliştirici Kaynakları** > **örnek Web API'si (kopya bu URL'yi)**. Örnek değer: `https://testdrive.crm.dynamics.com/api/data/v9.0`  |
| Rol adı                     | Test Sürüşünüz oluşturdunuz ve kullanıcılara, örneğin çalıştırdıklarında atanacak özel Dynamics 365 güvenlik rolünün adı `testdriveuser`. |
|  |  |

Tüm gerekli bilgileri sağladıktan sonra seçin **Kaydet**.


## <a name="next-steps"></a>Sonraki adımlar

Pazarlama ve satış bilgilerinde İleri sağlayacak [mağaza Ayrıntılar sekmesi](./cpp-storefront-details-tab.md).

