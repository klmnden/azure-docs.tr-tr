---
title: Uyarı ve akıllı grup durumlarını yönetme
description: Uyarı ve akıllı Grup örnek durumları yönetme
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: 2aa521b0552b60e5a875a5f46ab9887c6e5b6e3e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60919726"
---
# <a name="manage-alert-and-smart-group-states"></a>Uyarı ve akıllı grup durumlarını yönetme
Azure İzleyici'de uyarıları artık sahip bir [uyarı durumu ve izleme koşulu](https://aka.ms/azure-alerts-overview) ve benzer şekilde, akıllı grubunuz bir [akıllı Grup durumu](https://aka.ms/smart-groups). Durumu değişiklikleri şimdi geçmişine ilgili uyarı veya akıllı grubuyla ilişkili yakalanır. Bu makalede, bir uyarı hem akıllı bir grup için durum değiştirme işleminde size yol gösterir.

## <a name="change-the-state-of-an-alert"></a>Bir uyarının durumunu değiştirin
1. Bir uyarının durumunu aşağıdaki farklı yollarla değiştirebilirsiniz: 
    * Tüm uyarılar sayfasında durumunu değiştirmek için istediğiniz uyarıları yanındaki onay kutusuna tıklayın ve durumunu değiştir'e tıklayın.   
    ![İzleme](./media/alerts-managing-alert-states/state-all-alerts.jpg)
    * Belirli bir uyarı örneği için uyarı ayrıntıları sayfasındaki durumunu değiştir tıklayabilirsiniz.   
    ![İzleme](./media/alerts-managing-alert-states/state-alert-details.jpg)
    * Belirli bir uyarı örneği için uyarı ayrıntıları sayfasındaki akıllı grubu bölmesinde istediğiniz uyarıları yanındaki onay kutusunu tıklatabilirsiniz    
    ![İzleme](./media/alerts-managing-alert-states/state-alert-details-sg.jpg)

    * Akıllı grubu Ayrıntıları sayfasında üye Uyarılar listesinde değiştirme Stateto Değiştir'i tıklatın ve durumunu değiştirmek için istediğiniz uyarıları yanındaki onay kutusunu durumunu tıklayabilir ve görebilirsiniz durumunu değiştir'e tıklayın.   
    ![İzleme](./media/alerts-managing-alert-states/state-sg-details-alerts.jpg)
1. Değişiklik durumu'ye tıkladığınızda, açılan pencere durumu (yeni/onaylanan/kapalı) seçin ve gerekirse bir açıklama girin, izin açılır.   
![İzleme](./media/alerts-managing-alert-states/state-alert-change.jpg)
1. Bunu yaptıktan sonra durum değişikliği geçmişi ilgili uyarı kaydedilir. Bu işlem, İlgili Ayrıntılar sayfasını açarak ve Geçmiş bölümü denetimi de görüntülenebilir.    
![İzleme](./media/alerts-managing-alert-states/state-alert-history.jpg)

## <a name="change-the-state-of-a-smart-group"></a>Akıllı bir grubu durumunu değiştirme
1. Akıllı bir grubun durumunu aşağıdaki farklı yollarla değiştirebilirsiniz:
    1. Akıllı grubu Listesi sayfasında durumunu değiştir'i tıklatın ve durumunu değiştirmek için istediğiniz akıllı grupları yanındaki onay kutusunu tıklatabilirsiniz.  
    ![İzleme](./media/alerts-managing-alert-states/state-sg-list.jpg)
    1. Akıllı grubu ayrıntıları sayfasındaki durumunu değiştir tıklayabilirsiniz.        
    ![İzleme](./media/alerts-managing-alert-states/state-sg-details.jpg)
1. Değişiklik durumu'ye tıkladığınızda, açılan pencere durumu (yeni/onaylanan/kapalı) seçin ve gerekirse bir açıklama girin, izin açılır. 
![İzleme](./media/alerts-managing-alert-states/state-sg-change.jpg)
   > [!NOTE]
   >  Akıllı bir grubu durumunu değiştirmeyi üyesine uyarıları durumunu değiştirmez.

1. Bunu yaptıktan sonra durum değişikliği ilgili akıllı Grup geçmişinde kaydedilir. Bu işlem, İlgili Ayrıntılar sayfasını açarak ve Geçmiş bölümü denetimi de görüntülenebilir.     
![İzleme](./media/alerts-managing-alert-states/state-sg-history.jpg)

