---
title: "Azure Analysis Services öğreticisi 13. ders: Dağıtma | Microsoft Docs"
description: "Azure Analysis Services’a öğretici projesinin nasıl dağıtılacağını açıklar."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 8e3e1be572aa66ab46f894a2e5f395d1e6f2ea23
ms.contentlocale: tr-tr
ms.lasthandoff: 06/03/2017

---
<a id="lesson-13-deploy" class="xliff"></a>

# 13. Ders: Dağıtma

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Bu derste, Azure’da bir Analysis Services sunucusu ya da şirket içinde SQL Server vNext Analysis Services sunucusu ve model için bir ad belirterek dağıtım özelliklerini yapılandırırsınız. Daha sonra modeli bu örneğe dağıtırsınız. Modeliniz dağıtıldıktan sonra kullanıcılar bir raporlama istemci uygulaması kullanarak modele bağlanabilir. Daha fazla bilgi için bkz. [Azure Analysis Services’a Dağıtma](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).  
  
Bu dersi tamamlamak için tahmini süre: **Beş dakika**  
  
<a id="prerequisites" class="xliff"></a>

## Ön koşullar  
Bu konu, sırayla tamamlanması gereken bir tablo modelleme öğreticisinin bir parçasıdır. Bu dersteki görevleri gerçekleştirmeden önce, bir önceki dersi tamamlamış olmanız gerekir: [12. Ders: Excel’de çözümleme](../tutorials/aas-lesson-12-analyze-in-excel.md).  

**Önemli:** AdventureWorksDW2014 örnek veritabanını şirket içi bir SQL Server’a yüklediyseniz ve modelinizi bir Azure Analysis Services sunucusuna dağıtacaksanız [Şirket içi veri ağ geçidi](../analysis-services-gateway.md) gereklidir.
  
<a id="deploy-the-model" class="xliff"></a>

## Modeli dağıtma  
  
<a id="to-configure-deployment-properties" class="xliff"></a>

#### Dağıtım özelliklerini yapılandırmak için  

  
1.  **Çözüm Gezgini**’nde **AW İnternet Satışları** projesine sağ tıklayıp **Özellikler**’e tıklayın.  
  
2.  **AW İnternet Satışları Özellik Sayfaları** iletişim kutusundaki **Dağıtım Sunucusu** altında bulunan **Server** özelliğine Azure veya şirket içindeki bir Analysis Services sunucusunun adını girin.  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
 
    > [!IMPORTANT]  
    > Uzak Analysis Services örneğini dağıtmak için örnek üzerinde Yönetici izinlerine sahip olmanız gerekir.  
  
3.  **Database** özelliğine **Adventure Works İnternet Satışları** yazın.  
  
4.  **Model Name** özelliğine **Adventure Works İnternet Satışları Modeli** yazın.  
  
5.  Seçimlerinizi doğrulayıp **Tamam**’a tıklayın.  
  
<a id="to-deploy-the-adventure-works-internet-sales" class="xliff"></a>

#### Adventure Works İnternet Satışlarını dağıtmak için
  
1.  **Çözüm Gezgini**’nde **AW İnternet Satışları** projesine sağ tıklayıp **Derleme**’ye tıklayın.  

2.  **AW İnternet Satışları** projesine sağ tıklayıp **Dağıt**’a tıklayın.

    Azure Analysis Services'a dağıtım yaparken hesabınızı girmeniz istenebilir. Kuruluş hesabınızı ve parolanızı girin, örneğin nancy@adventureworks.com. Bu hesap, sunucu örneğindeki Yöneticiler grubuna üye olmalıdır.
  
    Dağıtım iletişim kutusu görünür ve burada meta veriler ile modele dahil edilen her tablonun dağıtım durumu gösterilir.  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. Dağıtım başarıyla tamamlandığında devam edin ve **Kapat**’a tıklayın.  
  
<a id="conclusion" class="xliff"></a>

## Sonuç  
Tebrikler! İlk Analysis Services Tablo modelinizi yazma ve dağıtma işlemini tamamladınız. Bu öğretici, bir tablo modeli oluştururken en yaygın görevleri tamamlamanıza yardımcı olmuştur. Adventure Works İnternet Satışları modeliniz artık dağıtıldığına göre SQL Server Management Studio’yu kullanarak modeli yönetebilir, işlem betikleri ve bir yedek plan oluşturabilirsiniz. Kullanıcılar artık ayrıca Microsoft Excel veya Power BI gibi bir raporlama istemci uygulaması kullanarak modele bağlanabilir.  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
<a id="whats-next" class="xliff"></a>

## Sırada ne var?
*  [Ek Ders - Dinamik güvenlik](../tutorials/aas-supplemental-lesson-dynamic-security.md)

*  [Ek Ders - Ayrıntı satırları](../tutorials/aas-supplemental-lesson-detail-rows.md)

*  [Ek Ders - Düzensiz hiyerarşiler](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)

