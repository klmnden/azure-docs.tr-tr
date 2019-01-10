---
title: 'Azure Analysis Services Öğreticisi 11. Ders: Rol oluşturma | Microsoft Docs'
description: Azure Analysis Services öğretici projesinde rollerin nasıl oluşturulacağını açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5b89051cab7e89f79a2b62a392173e6dc234e48d
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54189754"
---
# <a name="create-roles"></a>Rol oluşturma

Bu derste rol oluşturacaksınız. Roller, erişimi yalnızca rol üyesi olan kullanıcılarla sınırlayarak model veritabanı nesnesi ve veri güvenliği sağlar. Her rol tek bir izinle tanımlanır: Hiçbiri, okuma, okuma ve işlem, işlem veya yönetici. Roller, Rol Yöneticisi kullanılarak model yazma sırasında tanımlanabilir. Bir model dağıtıldıktan sonra SQL Server Management Studio’yu (SSMS) kullanarak rolleri yönetebilirsiniz. Daha fazla bilgi için bkz. [Roller](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).
  
> [!NOTE]  
> Bu öğreticinin tamamlanması için rol oluşturmak gerekli değildir. Varsayılan olarak, o anda oturum açmış olduğunuz hesap, model üzerinde Yönetici ayrıcalıklarına sahiptir. Ancak kuruluşunuzdaki diğer kullanıcıların bir raporlama istemcisi kullanarak göz atması için, Okuma izinlerine sahip en az bir rol oluşturmanız ve bu kullanıcıları üye olarak eklemeniz gerekir.  
  
Üç rol oluşturursunuz:  
  
-   **Satış Yöneticisi** – Bu rol, kuruluşunuzda tüm model nesneleri ve veriler için Okuma iznine sahip olmasını istediğiniz kullanıcıları içerebilir.  
  
-   **Satış Analisti ABD** – Bu rol, kuruluşunuzda yalnızca Birleşik Devletler'deki satışlar ile ilgili verilere göz atabilmesini istediğiniz kullanıcıları içerebilir. Bu rol için, üyelerin yalnızca Birleşik Devletler’e ait verilere göz atmasına izin veren bir *Satır Filtresi* tanımlamak üzere bir DAX formülü kullanırsınız.  
  
-   **Yönetici** – Bu rol, model veritabanında yönetici görevleri gerçekleştirmek için sınırsız erişim ve izinler veren Yönetici iznine sahip olmasını istediğiniz kullanıcıları içerebilir.  
  
Kuruluşunuzdaki Windows kullanıcı ve grup hesapları benzersiz olduğu için, kuruluşunuzdan üyelere hesaplar ekleyebilirsiniz. Ancak bu öğretici için, üyeleri boş da bırakabilirsiniz. Daha sonra 12. Ders her rolün etkisini test edin: Excel'de analiz edin.  
  
Bu dersi tamamlamak için tahmini süre: **15 dakika**  
  
## <a name="prerequisites"></a>Önkoşullar  
Bu konu başlığı, sırayla tamamlanması gereken bir tablosal modelleme öğreticisinin parçasıdır. Bu dersteki görevleri gerçekleştirmeden önce bir önceki dersi tamamlamış olmanız gerekir: [10. Ders: Bölüm oluşturma](../tutorials/aas-lesson-10-create-partitions.md).  
  
## <a name="create-roles"></a>Rol oluşturma  
  
#### <a name="to-create-a-sales-manager-user-role"></a>Satış yöneticisi kullanıcı rolü oluşturmak için  
  
1.  Tablo Model Gezgini'nde **Roller** > **Roller**’e sağ tıklayın.  
  
2.  Rol Yöneticisi'nde **Yeni**’ye tıklayın.  
  
3.  Yeni role tıklayın ve ardından **Ad** sütununda rolü **Satış Yöneticisi** olarak adlandırın.  
  
4.  **İzinler** sütununda açılan listeye tıklayın ve ardından **Okuma** iznini seçin. 

    ![aas-lesson11-new-role](../tutorials/media/aas-lesson11-new-role.png) 
  
5.  İsteğe bağlı: **Üyeler** sekmesine ve ardından **Ekle**'ye tıklayın. **Kullanıcı veya Grup Seçin** iletişim kutusunda, kuruluşunuzda role dahil etmek istediğiniz Windows kullanıcılarını veya gruplarını girin.  
  
#### <a name="to-create-a-sales-analyst-us-user-role"></a>Satış Analisti ABD kullanıcı rolü oluşturmak için  
  
1.  Rol Yöneticisi'nde **Yeni**’ye tıklayın.    
  
2.  Rolü **Satış Analisti ABD** olarak adlandırın.  
  
3.  Bu role **Okuma** izni verin.  
  
4.  Satır Filtreleri sekmesine tıklayın ve yalnızca **DimGeography** tablosunun DAX Filtresi sütununa aşağıdaki formülü yazın:  
  
    ```Administrator
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    Satır Filtresi formülü bir Boole (TRUE/FALSE) değerine çözümlenmelidir. Bu formülde, yalnızca Ülke Bölge Kodu “US” olan satırların kullanıcıya görünür olacağını belirlersiniz.  
    ![aas-lesson11-role-filter](../tutorials/media/aas-lesson11-role-filter.png) 
  
6.  İsteğe bağlı: **Üyeler** sekmesine ve ardından **Ekle**'ye tıklayın. **Kullanıcı veya Grup Seçin** iletişim kutusunda, kuruluşunuzda role dahil etmek istediğiniz Windows kullanıcılarını veya gruplarını girin.  
  
#### <a name="to-create-an-administrator-user-role"></a>Yönetici kullanıcı rolü oluşturmak için  
  
1.  **Yeni**’ye tıklayın.  
  
2.  Rolü **Yönetici** olarak yeniden adlandırın.  
  
3.  Bu role **Yönetici** izni verin.  
  
4.  İsteğe bağlı: **Üyeler** sekmesine ve ardından **Ekle**'ye tıklayın. **Kullanıcı veya Grup Seçin** iletişim kutusunda, kuruluşunuzda role dahil etmek istediğiniz Windows kullanıcılarını veya gruplarını girin. 
  
  
## <a name="whats-next"></a>Sırada ne var?
[12. Ders: Excel'de Çözümle](../tutorials/aas-lesson-12-analyze-in-excel.md).

  
  
