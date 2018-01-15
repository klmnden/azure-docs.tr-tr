---
title: "Azure Analysis Services öğreticisi 11. ders: Rol oluşturma | Microsoft Docs"
description: "Azure Analysis Services öğretici projesinde rollerin nasıl oluşturulacağını açıklar."
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 01/08/2018
ms.author: owend
ms.openlocfilehash: 5fb0e2dd56e373ecf723a3672d9538bcc6dc68e3
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="create-roles"></a>Rol oluşturma

Bu derste rol oluşturacaksınız. Roller, erişimi yalnızca rol üyesi olan kullanıcılarla sınırlayarak model veritabanı nesnesi ve veri güvenliği sağlar. Her rol tek bir izinle tanımlanır: Hiçbiri, Okuma, Okuma ve İşlem, İşlem veya Yönetici. Roller, Rol Yöneticisi kullanılarak model yazma sırasında tanımlanabilir. Bir model dağıtıldıktan sonra SQL Server Management Studio’yu (SSMS) kullanarak rolleri yönetebilirsiniz. Daha fazla bilgi için bkz. [Roller](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).
  
> [!NOTE]  
> Bu öğreticinin tamamlanması için rol oluşturmak gerekli değildir. Varsayılan olarak, o anda oturum açmış olduğunuz hesap, model üzerinde Yönetici ayrıcalıklarına sahiptir. Ancak kuruluşunuzdaki diğer kullanıcıların bir raporlama istemcisi kullanarak göz atması için, Okuma izinlerine sahip en az bir rol oluşturmanız ve bu kullanıcıları üye olarak eklemeniz gerekir.  
  
Üç rol oluşturursunuz:  
  
-   **Satış Yöneticisi** – Bu rol, kuruluşunuzda tüm model nesneleri ve veriler için Okuma iznine sahip olmasını istediğiniz kullanıcıları içerebilir.  
  
-   **Satış Analisti ABD** – Bu rol, kuruluşunuzda yalnızca Birleşik Devletler'deki satışlar ile ilgili verilere göz atabilmesini istediğiniz kullanıcıları içerebilir. Bu rol için, üyelerin yalnızca Birleşik Devletler’e ait verilere göz atmasına izin veren bir *Satır Filtresi* tanımlamak üzere bir DAX formülü kullanırsınız.  
  
-   **Yönetici** – Bu rol, model veritabanında yönetici görevleri gerçekleştirmek için sınırsız erişim ve izinler veren Yönetici iznine sahip olmasını istediğiniz kullanıcıları içerebilir.  
  
Kuruluşunuzdaki Windows kullanıcı ve grup hesapları benzersiz olduğu için, kuruluşunuzdan üyelere hesaplar ekleyebilirsiniz. Ancak bu öğretici için, üyeleri boş da bırakabilirsiniz. Her bir rolün etkisini daha sonra 12. Ders: Excel’de çözümleme dersinde test edeceksiniz.  
  
Bu dersin tahmini tamamlanma süresi: **15 dakika**  
  
## <a name="prerequisites"></a>Önkoşullar  
Bu konu, sırayla tamamlanması gereken bir tablo modelleme öğreticisinin bir parçasıdır. Bu dersteki görevleri gerçekleştirmeden önce, bir önceki dersi tamamlamış olmanız gerekir: [10. Ders: Bölümleme oluşturma](../tutorials/aas-lesson-10-create-partitions.md).  
  
## <a name="create-roles"></a>Rol oluşturma  
  
#### <a name="to-create-a-sales-manager-user-role"></a>Satış yöneticisi kullanıcı rolü oluşturmak için  
  
1.  Tablo Model Gezgini'nde **Roller** > **Roller**’e sağ tıklayın.  
  
2.  Rol Yöneticisi'nde **Yeni**’ye tıklayın.  
  
3.  Yeni role tıklayın ve ardından **Ad** sütununda rolü **Satış Yöneticisi** olarak adlandırın.  
  
4.  **İzinler** sütununda açılan listeye tıklayın ve ardından **Okuma** iznini seçin. 

    ![aas-lesson11-new-role](../tutorials/media/aas-lesson11-new-role.png) 
  
5.  İsteğe bağlı: **Üyeler** sekmesine ve ardından **Ekle**’ye tıklayın. **Kullanıcı veya Grup Seçin** iletişim kutusunda, kuruluşunuzda role dahil etmek istediğiniz Windows kullanıcılarını veya gruplarını girin.  
  
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
  
6.  İsteğe bağlı: **Üyeler** sekmesine ve ardından **Ekle**’ye tıklayın. **Kullanıcı veya Grup Seçin** iletişim kutusunda, kuruluşunuzda role dahil etmek istediğiniz Windows kullanıcılarını veya gruplarını girin.  
  
#### <a name="to-create-an-administrator-user-role"></a>Yönetici kullanıcı rolü oluşturmak için  
  
1.  **Yeni**’ye tıklayın.  
  
2.  Rolü **Yönetici** olarak yeniden adlandırın.  
  
3.  Bu role **Yönetici** izni verin.  
  
4.  İsteğe bağlı: **Üyeler** sekmesine ve ardından **Ekle**’ye tıklayın. **Kullanıcı veya Grup Seçin** iletişim kutusunda, kuruluşunuzda role dahil etmek istediğiniz Windows kullanıcılarını veya gruplarını girin. 
  
  
## <a name="whats-next"></a>Sırada ne var?
[12. Ders: Excel’de çözümleme](../tutorials/aas-lesson-12-analyze-in-excel.md).

  
  
