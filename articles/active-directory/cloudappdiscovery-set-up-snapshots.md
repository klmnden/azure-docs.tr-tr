---
title: Azure Active Directory'de cloud App Discovery anlık görüntü raporlar oluşturun | Microsoft Docs
description: Bulma ve Cloud App Discovery, avantajları nelerdir ve nasıl çalıştığı ile uygulamaları yönetme hakkında bilgi sağlar.
services: active-directory
keywords: cloud app discovery'yi, uygulamaları yönetme
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: article
ms.date: 09/22/2017
ms.author: barbkess
ms.reviewer: nigu
ms.custom: it-pro
ms.openlocfilehash: 33ac83e04e43088f74bb46670f0571762b57e555
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
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
