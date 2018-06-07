---
title: 'Azure Analysis Services öğreticisi 13. ders: Dağıtma | Microsoft Docs'
description: Azure Analysis Services’a öğretici projesinin nasıl dağıtılacağını açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 9b953428525e7970fef7224e65200cf9811b6304
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34596289"
---
# <a name="deploy"></a>Dağıtma

Bu derste, Azure Analysis Services sunucusunun model için bir ad belirtmesini sağlayarak dağıtım özelliklerini yapılandırırsınız. Daha sonra modeli bu örneğe dağıtırsınız. Modeliniz dağıtıldıktan sonra kullanıcılar bir raporlama istemci uygulaması kullanarak modele bağlanabilir. Daha fazla bilgi için bkz. [Azure Analysis Services’a Dağıtma](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).  
  
Bu dersin tahmini tamamlanma süresi: **5 dakika**  
  
## <a name="prerequisites"></a>Önkoşullar  
Bu makale, sırayla tamamlanması gereken bir tablosal modelleme öğreticisinin parçasıdır. Bu dersteki görevleri gerçekleştirmeden önce, bir önceki dersi tamamlamış olmanız gerekir: [12. Ders: Excel’de çözümleme](../tutorials/aas-lesson-12-analyze-in-excel.md).  

> [!IMPORTANT]  
> Uzak Analysis Services sunucusunu dağıtmak için sunucu üzerinde [Yönetici izinlerine](../analysis-services-server-admins.md) sahip olmanız gerekir.  

> [!IMPORTANT]  
> AdventureWorksDW2014 örnek veritabanını şirket içi bir SQL Server’a yüklediyseniz ve modelinizi bir Azure Analysis Services sunucusuna dağıtacaksanız [Şirket içi veri ağ geçidi](../analysis-services-gateway.md) gereklidir.
  
## <a name="deploy-the-model"></a>Modeli dağıtma  
  
#### <a name="to-configure-deployment-properties"></a>Dağıtım özelliklerini yapılandırmak için  

  
1.  **Çözüm Gezgini**’nde **AW İnternet Satışları** projesine sağ tıklayıp **Özellikler**’e tıklayın.  
  
2.  **AW İnternet Satışları Özellik Sayfaları** iletişim kutusundaki **Dağıtım Sunucusu** altında bulunan **Server** özelliğine tüm sunucuyu girin.  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
  
3.  **Database** özelliğine **Adventure Works İnternet Satışları** yazın.  
  
4.  **Model Name** özelliğine **Adventure Works İnternet Satışları Modeli** yazın.  
  
5.  Seçimlerinizi doğrulayıp **Tamam**’a tıklayın.  
  
#### <a name="to-deploy-the-adventure-works-internet-sales"></a>Adventure Works İnternet Satışlarını dağıtmak için
  
1.  **Çözüm Gezgini**’nde **AW İnternet Satışları** projesine sağ tıklayıp **Derleme**’ye tıklayın.  

2.  **AW İnternet Satışları** projesine sağ tıklayıp **Dağıt**’a tıklayın.

    Azure Analysis Services'a dağıtım yaparken hesabınızı girmeniz istenebilir. Kuruluş hesabınızı ve parolanızı girin, örneğin nancy@adventureworks.com. Bu hesap, sunucudaki Yöneticiler grubuna üye olmalıdır.
  
    Dağıtım iletişim kutusu görünür ve burada meta veriler ile modele dahil edilen her tablonun dağıtım durumu gösterilir.  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. Dağıtım başarıyla tamamlandığında devam edin ve **Kapat**’a tıklayın.  
  

Bu ders, SSDT’den bir tablosal model dağıtmanın en yaygın ve en kolay yöntemini açıklar. Dağıtım Sihirbazı ya da XMLA ve ÇYN ile otomatikleştirme gibi gelişmiş dağıtım seçenekleri daha iyi esneklik, tutarlılık ve zamanlanmış dağıtım sağlar. Daha fazla bilgi edinmek için bkz. [Tablosal model çözümü dağıtımı](https://docs.microsoft.com/sql/analysis-services/tabular-models/tabular-model-solution-deployment-ssas-tabular).

## <a name="conclusion"></a>Sonuç  
Tebrikler! İlk Analysis Services Tablo modelinizi yazma ve dağıtma işlemini tamamladınız. Bu öğretici, bir tablo modeli oluştururken en yaygın görevleri tamamlamanıza yardımcı olmuştur. Adventure Works İnternet Satışları modeliniz artık dağıtıldığına göre SQL Server Management Studio’yu kullanarak modeli yönetebilir, işlem betikleri ve bir yedek plan oluşturabilirsiniz. Kullanıcılar artık ayrıca Microsoft Excel veya Power BI gibi bir raporlama istemci uygulaması kullanarak modele bağlanabilir.  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a>Sırada ne var?
[Power BI Desktop ile bağlanma](../analysis-services-connect-pbi.md)   
[Ek Ders - Dinamik güvenlik](../tutorials/aas-supplemental-lesson-dynamic-security.md)   
[Ek Ders - Ayrıntı satırları](../tutorials/aas-supplemental-lesson-detail-rows.md)   
[Ek Ders - Düzensiz hiyerarşiler](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   
