---
title: 'Öğretici: Azure Active Directory günlüklerini bir Azure depolama hesabında arşivleme (önizleme) | Microsoft Docs'
description: Azure Tanılama'yı kullanarak Azure Active Directory günlüklerini bir depolama hesabına göndermeyi öğrenin (önizleme)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: ea26da91e2a0b88e8afa54a4210b7a2365e4949b
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240291"
---
# <a name="tutorial-archive-azure-active-directory-logs-to-an-azure-storage-account-preview"></a>Öğretici: Azure Active Directory günlüklerini bir Azure depolama hesabında arşivleme (önizleme)

Bu öğreticide bir Azure depolama hesabına Azure Active Directory günlüğü yönlendirme amacıyla Azure İzleyici tanılama ayarlarını yapmayı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar 

Gerekenler:

* Azure depolama hesabına sahip bir Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz denemeye kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Bir Azure Active Directory kiracısı.
* Kiracıda genel yönetici veya güvenlik yöneticisi olan bir kullanıcı.

## <a name="archive-logs-to-an-azure-storage-account"></a>Günlükleri Azure depolama hesabında arşivleme

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. **Azure Active Directory** -> **Etkinlik** -> **Denetim günlükleri**'ne tıklayın. 
3. **Dışarı Aktarma Ayarları**'na tıklayarak Tanılama Ayarları dikey penceresini açın. Var olan ayarları değiştirmek istiyorsanız **Ayarı düzenle**'ye, yeni bir ayar eklemek istiyorsanız **Tanılama ayarı ekle**'ye tıklayın. En fazla üç ayara sahip olabilirsiniz. 
    ![Dışarı aktarma ayarları](./media/reporting-azure-monitor-diagnostics-azure-storage-account/ExportSettings.png "Dışarı aktarma ayarları")

4. Ayarın amacını hatırlatacak bir kolay ad ekleyin. Örneğin: "Azure depolama hesabına gönder". 
5. **Bir depolama hesabına arşivle** onay kutusunu işaretleyin ve **Depolama hesabı**'na tıklayıp Azure depolama hesabını seçin. 
6. Bir Azure aboneliği ve günlükleri yönlendirmek istediğiniz depolama hesabını seçtikten sonra **Tamam**'a tıklayarak yapılandırma sayfasını kapatın.
7. Denetim günlüklerini depolama hesabına göndermek için **AuditLogs** onay kutusunu işaretleyin. 
8. Oturum açma günlüklerini depolama hesabına göndermek için **SignInLogs** onay kutusunu işaretleyin.
9. Günlük verilerinizi saklama süresini belirlemek için kaydırıcıyı kullanın. Varsayılan olarak "0" değeri kullanılır ve depolama hesapları sonsuza kadar depolama hesabında saklanır. İsterseniz bir değer belirleyebilir ve bu süreden daha eski olayların otomatik olarak silinmesini sağlayabilirsiniz.
10. Ayarı kaydetmek için **Kaydet**’e tıklayın.
    ![Tanılama ayarları](./media/reporting-azure-monitor-diagnostics-azure-storage-account/DiagnosticSettings.png "Tanılama ayarları")

11. Yaklaşık 15 dakika sonra günlüklerin depolama hesabınıza gönderilip gönderilmediğini kontrol edin. Azure portala gidin, **Depolama hesapları**'na tıklayın, önceki adımlarda kullandığınız depolama hesabını seçin ve **Bloblar**'a tıklayın. 
12. **Denetim günlükleri** için **insights-log-audit** öğesine tıklayın. **Oturum açma günlükleri** için **insights-logs-signin** öğesine tıklayın.
    ![Depolama hesabı](./media/reporting-azure-monitor-diagnostics-azure-storage-account/StorageAccount.png "Depolama hesabı")

## <a name="next-steps"></a>Sonraki adımlar

* [Azure izleyici denetim günlükleri şemasını yorumlama](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Azure izleyici oturum açma günlükleri şemasını yorumlama](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Sık sorulan sorular ve bilinen sorunlar](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)