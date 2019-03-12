---
title: İndirip signın erişmek için bir komut dosyası kullanarak günlükleri nasıl Öğreticisi | Microsoft Docs
description: İndirme ve signın günlüklerine erişmek için bir PowerShell Betiği kullanmak hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 4afe0c73-aee8-47f1-a6cb-2d71fd6719d1
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: aedbc625bedcbe66b43b66ce96e1b17746b9a47c
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57531298"
---
# <a name="tutorial-how-to-download-and-use-a-script-to-access-sign-in-logs"></a>Öğretici: Karşıdan yükleme ve oturum açma günlüklerine erişmek için bir betik kullan

Azure portalının dışında çalışmak istiyorsanız oturum açma etkinlik verilerini indirebilirsiniz. **İndirme** seçeneği Azure portalında, bir CSV dosyası en son 5000 kayıt oluşturur. Daha fazla esneklik gerekiyorsa, örneğin, 5000'den fazla kayıt aynı anda yüklemek veya zamanlanan aralıklarla günlükleri indirmek için kullanabileceğiniz **betik** verilerinizi yüklemek için bir PowerShell betiği oluşturmak için düğme.

Bu öğreticide, son 24 saat tüm oturum açma günlükleri indirmek ve her gün çalışacak şekilde zamanlamak için bir komut dosyası oluşturmayı öğrenin. 

## <a name="prerequisites"></a>Önkoşullar

İhtiyacın var

* Azure Active Directory kiracısı (ö1/ö2) premium lisansına sahip. Yükseltme öncesinde tüm etkinlikleri veri yoksa, birkaç gün raporlarda görünmesi için bir premium lisansı yükselttikten sonra verilerin gerektiğine dikkat edin. 
* İçinde olan bir kullanıcı **genel yönetici**, **Güvenlik Yöneticisi**, **güvenlik okuyucusu** veya **rapor okuyucu** kiracının rol. Ayrıca, herhangi bir kullanıcı kendi oturum açma etkinliklerine erişebilir. 

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
