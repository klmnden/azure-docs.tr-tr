---
title: Azure Active Directory'de cloud App Discovery anlık görüntü raporlar oluşturun | Microsoft Docs
description: Bulma ve Cloud App Discovery, avantajları nelerdir ve nasıl çalıştığı ile uygulamaları yönetme hakkında bilgi sağlar.
services: active-directory
keywords: cloud app discovery'yi, uygulamaları yönetme
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/22/2017
ms.author: curtand
ms.reviewer: nigu
ms.custom: it-pro
ms.openlocfilehash: ad4591223c72893a4488f5515d8ceb83e0d7f8cf
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="create-cloud-app-discovery-snapshot-reports"></a>Cloud App Discovery anlık görüntü raporları oluşturma

Otomatik günlük Toplayıcı ayarlama önce bir günlük el ile karşıya yüklemek ve bunu ayrıştırabilir Cloud App Security izin verin. Bir günlük henüz yok ve günlüğünüzü aşağıdaki gibi görünmelidir, bir örnek görmek istiyorsanız ne günlüğünüzün gibi görünecek şekilde gerektiği görmek için bir örnek günlük dosyası yüklemek için aşağıdaki yordamı kullanın.

## <a name="to-create-a-snapshot-report"></a>Bir anlık görüntü rapor oluşturmak için

1. Internet üzerinden kuruluşunuzdaki kullanıcıların erişim güvenlik duvarı ve Ara sunucunuzdan günlük dosyalarını toplayın. Saatlerde kuruluşunuzdaki kullanıcı etkinliği temsilcisi olan yoğun trafik günlüklerini toplayın.
2. Üzerinde [Cloud App Security menü çubuğu](https://portal.cloudappsecurity.com)seçin **bulma**ve ardından **anlık görüntü rapor oluşturma**.
  
  ![Yeni anlık görüntü raporu oluştur](./media/cloudappdiscovery-set-up-snapshots/create-snapshot-command.png)
3. Girin bir **rapor adı** ve **açıklama**.
    
  ![Yeni bir anlık görüntü rapor](./media/cloudappdiscovery-set-up-snapshots/create-snapshot-form.png)
4. Seçin **veri kaynağı** günlük dosyalarını karşıya yüklemek istediğiniz.
5. İndirebilirsiniz örnek göre düzgün biçimlendirildiğinden emin olmak için günlük biçimini doğrulayın. Tıklatın **Görünüm ve doğrulama** ve ardından **indirme örnek günlük**. Ardından, günlük uyumlu olduğundan emin olmak için sağlanan örnek ile karşılaştırın.
  
  ![Günlük biçiminiz doğrulanıyor](./media/cloudappdiscovery-set-up-snapshots/create-snapshot-verify.png)
  >  [!NOTE]
  > FTP örnek biçimi anlık görüntülerini desteklenir ve yalnızca otomatik karşıya syslog destekleniyorsa karşıya yükleme otomatik. Örnek günlük indirme FTP günlüğü örneği yükler.
6. **Trafik günlükleri seçin** karşıya yüklemek istediğiniz. Aynı anda en çok 20 dosya yükleyebilirsiniz. Sıkıştırılmış ve sıkıştırılmış dosyaları da desteklenir.
  
7. **Oluştur**’a tıklayın. Karşıya yükleme işlemi tamamlandıktan sonra bunları ayrıştırılması ve analiz için biraz zaman alabilir. Destekliyorsa, Cloud App Discovery hazır olduğunuzda bir e-posta bildirimi gönderir.

8. Seçin **yönetmek anlık görüntü raporları** ve seçin, anlık görüntü rapor.
  
  ![Anlık görüntü rapor yönetimi](./media/cloudappdiscovery-set-up-snapshots/create-snapshot-manage.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Cloud App Discovery Azure AD'de kullanmaya başlama](cloudappdiscovery-get-started.md)
* [Sürekli raporlama için otomatik günlük karşıya yükleme yapılandırma](https://docs.microsoft.com/cloud-app-security/discovery-docker)
* [Özel günlük ayrıştırıcı kullanma](https://docs.microsoft.com/cloud-app-security/custom-log-parser)
