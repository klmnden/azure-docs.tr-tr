---
title: "Azure Analysis Services öğreticisi - 12. ders: Excel’de çözümleme | Microsoft Docs"
description: "Azure Analysis Services öğretici projesinde, Excel’de çözümleme özelliğinin nasıl kullanılacağını açıklar."
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
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: be23d25fe9765025b86e86687fb38b2dab61269e
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="lesson-12-analyze-in-excel"></a>12. Ders: Excel’de çözümleme

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Bu derste, Excel’de çözümleme özelliğini kullanarak Microsoft Excel’i açar, model çalışma alanı ile otomatik bir bağlantı oluşturur ve çalışma sayfasına otomatik olarak bir PivotTable eklersiniz. Excel’de çözümleme özelliği, modelinizi dağıtmadan önce model tasarımınızın etkililiğini test etmenin hızlı ve kolay bir yolunu sağlamak üzere tasarlanmıştır. Bu derste herhangi bir veri çözümlemesi yapmayacaksınız. Dersin amacı, model yazarı olarak model tasarımınızı test etmek için kullanabileceğiniz araçları tanımanızı sağlamaktır.   
  
Bu dersi tamamlamak için Excel’in SSDT ile aynı bilgisayarda yüklü olması gerekir.
  
Bu dersi tamamlamak için tahmini süre: **Beş dakika**  
  
## <a name="prerequisites"></a>Ön koşullar  
Bu konu, sırayla tamamlanması gereken bir tablo modelleme öğreticisinin bir parçasıdır. Bu dersteki görevleri gerçekleştirmeden önce, bir önceki dersi tamamlamış olmanız gerekir: [11. Ders: Rol oluşturma](../tutorials/aas-lesson-11-create-roles.md).  
  
## <a name="browse-using-the-default-and-internet-sales-perspectives"></a>Varsayılan ve İnternet Satışları perspektiflerini kullanarak göz atma  
Bu ilk görevlerde, tüm model nesnelerinizi içeren varsayılan perspektifi ve daha önceki İnternet Satışları perspektifini kullanarak modelinize göz atacaksınız. İnternet Satışları perspektifi, Müşteri tablo nesnesini içermez.  
  
#### <a name="to-browse-by-using-the-default-perspective"></a>Varsayılan perspektifi kullanarak göz atmak için  
  
1.  **Model** menüsü > **Excel'de çözümleme** öğesine tıklayın.  
  
2.  **Excel'de çözümleme** iletişim kutusunda **Tamam**’a tıklayın.  
  
    Excel yeni bir çalışma kitabı ile açılır. Geçerli kullanıcı hesabı kullanılarak bir veri kaynağı bağlantısı oluşturulur ve görüntülenebilir alanları tanımlamak için Varsayılan perspektif kullanılır. Çalışma sayfasına bir PivotTable otomatik olarak eklenir.  
  
3.  Excel'deki **PivotTable Alan Listesi** içinde, **DimDate** ve **FactInternetSales** ölçü gruplarının göründüğüne dikkat edin. **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory** ve **FactInternetSales** tabloları da ilgili sütunlarıyla birlikte görünür.  
  
4.  Çalışma kitabını kaydetmeden Excel’i kapatın.  
  
#### <a name="to-browse-by-using-the-internet-sales-perspective"></a>İnternet Satışları perspektifini kullanarak göz atmak için  
  
1.  **Model** menüsüne ve ardından **Excel'de çözümleme** öğesine tıklayın.  
  
2.  **Excel’de çözümleme** iletişim kutusunda **Geçerli Windows Kullanıcısı**’nı işaretli bırakın, ardından **Perspektif** açılır liste kutusunda **İnternet Satışları**’nı seçip **Tamam**’a tıklayın. 
    
    ![aas-lesson12-perspective](../tutorials/media/aas-lesson12-perspective.png)
    
3.  Excel'deki **PivotTable Alanları** listesinde DimCustomer tablosunun alan listesi dışında bırakıldığına dikkat edin.  
    
    ![aas-lesson12-fields](../tutorials/media/aas-lesson12-fields.png)
    
4.  Çalışma kitabını kaydetmeden Excel’i kapatın.  
  
## <a name="browse-by-using-roles"></a>Rolleri kullanarak göz atma  
Roller herhangi bir tablo modelinin önemli bir parçasıdır. Kullanıcıların üye olarak eklendiği en az bir rol olmadan kullanıcılar, modelinizi kullanarak verilere erişemez ve verileri çözümleyemez. Excel’de çözümleme özelliği, tanımladığınız rolleri test etmenizin bir yolunu sağlar.  
  
#### <a name="to-browse-by-using-the-sales-manager-user-role"></a>Satış Yöneticisi kullanıcı rolünü kullanarak göz atmak için  
  
1.  SSDT’de **Model** menüsüne ve ardından **Excel'de çözümleme** öğesine tıklayın.  
  
2.  **Modele bağlanmak için kullanılacak kullanıcı adını veya rolü belirtin** alanında **Rol**’ü seçin ve ardından açılır liste kutusunda **Satış Yöneticisi**’ni seçip **Tamam**’a tıklayın.  
  
    Excel yeni bir çalışma kitabı ile açılır. Bir PivotTable otomatik olarak oluşturulur. Pivot Tablo Alanı Listesi, yeni modelinizde bulunan tüm veri alanlarını içerir.  
      
3.  Çalışma kitabını kaydetmeden Excel’i kapatın.  
  
## <a name="whats-next"></a>Sırada ne var?
Sonraki derse gidin: [13. Ders: Dağıtma](../tutorials/aas-lesson-13-deploy.md).

  
  
  
