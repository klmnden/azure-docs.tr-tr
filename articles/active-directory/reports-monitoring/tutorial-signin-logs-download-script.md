---
title: İndirip signın erişmek için bir komut dosyası kullanarak günlükleri nasıl Öğreticisi | Microsoft Docs
description: İndirme ve signın günlüklerine erişmek için bir PowerShell Betiği kullanmak hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4afe0c73-aee8-47f1-a6cb-2d71fd6719d1
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 8b9097a62ca4bfa67fb5eb35e06f7834df6691e7
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622814"
---
# <a name="tutorial-how-to-download-and-use-a-script-to-access-sign-in-logs"></a>Öğretici: nasıl indirin ve oturum açma günlüklerine erişmek için bir betik kullan

Azure portalının dışında çalışmak istiyorsanız oturum açma etkinlik verilerini indirebilirsiniz. **İndirme** seçeneği Azure portalında, bir CSV dosyası en son 5000 kayıt oluşturur. Daha fazla esneklik gerekiyorsa, örneğin, 5000'den fazla kayıt aynı anda yüklemek veya zamanlanan aralıklarla günlükleri indirmek için kullanabileceğiniz **betik** verilerinizi yüklemek için bir PowerShell betiği oluşturmak için düğme.

Bu öğreticide, son 24 saat tüm oturum açma günlükleri indirmek ve her gün çalışacak şekilde zamanlamak için bir komut dosyası oluşturmayı öğrenin. 

## <a name="prerequisites"></a>Önkoşullar

İhtiyacın var

* Azure Active Directory kiracısı (ö1/ö2) premium lisansına sahip. 
* İçinde olan bir kullanıcı **genel yönetici**, **Güvenlik Yöneticisi**, **güvenlik okuyucusu** veya **rapor okuyucu** kiracının rol. Ayrıca, herhangi bir kullanıcı kendi oturum açma etkinliklerine erişebilir. 
* İndirdiğiniz betiğin Windows 10 makinenizde çalıştırmak istiyorsanız [AzureRM modülünü kurun ve yürütme İlkesi](concept-sign-ins.md#running-the-script-on-a-windows-10-machine).

## <a name="tutorial"></a>Öğretici

1. Gidin [Azure portalında](https://portal.azure.com) ve dizininizi seçin.
2. Seçin **Azure Active Directory** seçip **oturum açma işlemleri** gelen **izleme** bölümü. 
3. Kullanım **tarih aralığı** filtre açılır ve select **24 saat** son 24 saat verilerini almak için. 
4. Seçin **Uygula** ve filtrenin beklendiği gibi uygulandığını doğrulayın. 
5. Seçin **betik** üstteki menüden Filtreler uygulanmış Powershell betiğini indirin.

     ![Komut dosyası indir](./media/tutorial-signin-logs-download-script/download-script.png)
     
6. Açık **Görev Zamanlayıcı** uygulama Windows makinesi ve seçim **temel görevi Oluştur**.
7. Bir ad ve görev için bir açıklama girin ve tıklayın **sonraki**.
8. Seçin **günlük** görevin günlük olarak çalıştırılması ve başlangıç tarihini ve saatini girin izin vermek için bir radyo düğmesi.
9. Eylem menüde **programı Başlat** ve indirilen komut dosyası seçip **sonraki**. 
10. Zamanlanmış görev gözden geçirin ve seçin **son** görevi oluşturur.

     ![Görev oluşturma](./media/tutorial-signin-logs-download-script/create-task.png)

Artık, görev her gün çalışacak ve oturum açma kayıtları, son 24 saat biçiminde bir dosyasına kaydedin. **AAD_SignInReport_YYYYMMDD_HHMMSS.csv**. Farklı bir dosya adı altında kaydetmek için ya da indirilen kayıt sayısını değiştirmek için indirilen PowerShell betiğini de düzenleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory rapor bekletme ilkeleri](reference-reports-data-retention.md)
* [Azure Active Directory raporlama API’siyle çalışmaya başlama](concept-reporting-api.md)
* [Raporlama API'sini sertifikalar ile erişme](tutorial-access-api-with-certificates.md)
