---
title: Bir öznitelik değil Azure AD Connect eşitleme sorunlarını giderme | Microsoft Docs
description: Bu konu ile sorun giderme görevini kullanarak özniteliği eşitleme sorunlarını gidermek için adımları sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a639b14c9313179816f6376aa0c5642a645ea344
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60455998"
---
# <a name="troubleshoot-an-attribute-not-synchronizing-in-azure-ad-connect"></a>Bir öznitelik değil Azure AD Connect eşitleme sorunlarını giderme

## <a name="recommended-steps"></a>**Önerilen Adımlar**

Öznitelik eşitleme sorunları araştırma önce anlayalım **Azure AD Connect** işlem eşitleme:

  ![Azure AD Connect eşitleme işlemi](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/syncingprocess.png)

### <a name="terminology"></a>**Terminoloji**

* **CS:** Bağlayıcı alanı, veritabanındaki bir tablo.
* **MV:** Meta veri deposu, veritabanındaki bir tablo.
* **AD:** Active Directory
* **AAD:** Azure Active Directory

### <a name="synchronization-steps"></a>**Eşitleme adımları**

* AD Al: Active Directory nesneleri AD CS kapsama alınır.

* AAD alma: Azure Active Directory nesnelerini AAD CS kapsama alınır.

* Eşitleme: **Gelen eşitleme kuralları** ve **giden eşitleme kuralı** gelen için daha düşük öncelik numarası sırasına göre daha yüksek çalıştırılır. Eşitleme kurallarını görüntülemek için giderek **eşitleme kuralları Düzenleyicisi** Masaüstü uygulamalarından. **Gelen olan eşitleme kuralları** verilerinde MV için CS getirir. **Giden eşitleme kuralı** CS'e MV verileri taşır.

* AD için dışarı aktarın: Eşitleme çalıştırıldıktan sonra nesneleri öğesinden AD CS için dışarı aktarılan **Active Directory**.

* AAD'ye dışarı aktarın: Eşitleme çalıştırıldıktan sonra nesneleri öğesinden AAD CS için dışarı aktarılan **Azure Active Directory**.

### <a name="step-by-step-investigation"></a>**Adım adım araştırma**

* Bizim aramadan başlayacağız **meta veri deposu** hedef kaynak öznitelik eşleme bakın.

* Başlatma **Eşitleme Hizmeti Yöneticisi** Masaüstü uygulamaları, aşağıda gösterilen:

  ![Eşitleme Hizmeti Yöneticisi'ni başlatma](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/startmenu.png)

* Üzerinde **Eşitleme Hizmeti Yöneticisi**seçin **meta veri deposu arama**seçin **nesne türüne göre kapsam**bir özniteliğini kullanarak nesneyi seçin ve tıklayın**Arama** düğmesi.

  ![Meta veri deposu arama](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvsearch.png)

* Bulunan nesne çift tıklayın **meta veri deposu** tüm öznitelikleri görüntülemek için arama. Tıklayabilirsiniz **Bağlayıcılar** tüm karşılık gelen nesne bakmak için sekmesinde **bağlayıcı alanları**.

  ![Meta veri deposu nesne bağlayıcılar](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvattributes.png)

* Çift tıklayın **Active Directory Bağlayıcısı** görüntülemek için **bağlayıcı alanına** öznitelikleri. Tıklayarak **Önizleme** düğmesi, aşağıdaki iletişim tıklayarak **oluşturma Önizleme** düğmesi.

  ![Bağlayıcı alanı öznitelikleri](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/csattributes.png)

* Artık tıklayarak **içeri aktarma öznitelik akışı**, bu öznitelikleri akışı gösterilmektedir **Active Directory Bağlayıcısı alanına** için **meta veri deposu**. **Eşitleme kuralı** sütunda görüntülenir, **eşitleme kuralı** bu öznitelik için katkıda bulunan. **Veri kaynağı** sütun öznitelikleri gösterir **bağlayıcı alanına**. **Meta veri deposu özniteliği** sütun öznitelikleri gösterir **meta veri deposu**. Burada eşitlemiyor özniteliğini bakabilirsiniz. Öznitelik burada bulamadığınız sonra bu eşlenmedi ve yeni özel oluşturmak varsa **eşitleme kuralı** öznitelik eşlemek için.

  ![Bağlayıcı alanı öznitelikleri](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/cstomvattributeflow.png)

* Tıklayarak **dışarı aktarma öznitelik akışı** gelen öznitelik akışı görüntülemek için sol bölmedeki **meta veri deposu** geri **Active Directory Bağlayıcısı alanına** kullanarak  **Giden eşitleme kuralı**.

  ![Bağlayıcı alanı öznitelikleri](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvtocsattributeflow.png)

* Benzer şekilde, görüntüleyebileceğiniz **Azure Active Directory Bağlayıcısı alanına** oluşturabilir ve nesne **Önizleme** gelen öznitelik akışı görüntülemek için **meta veri deposu** için**Bağlayıcı alanına** ve tersine, bu şekilde, araştırmanız neden bir öznitelik değil eşitleniyor.

## <a name="recommended-documents"></a>**Önerilen Belgeler**
* [Azure AD Connect eşitlemesi: Teknik Kavramlar](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-technical-concepts)
* [Azure AD Connect eşitlemesi: Mimariyi anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture)
* [Azure AD Connect eşitlemesi: Bildirim Temelli Sağlamayı Anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning)
* [Azure AD Connect eşitlemesi: Bildirim Temelli Sağlama İfadelerini Anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning-expressions)
* [Azure AD Connect eşitlemesi: Varsayılan yapılandırmayı anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-default-configuration)
* [Azure AD Connect eşitlemesi: Kullanıcıları, Grupları ve Kişileri Anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-user-and-contacts)
* [Azure AD Connect eşitlemesi: Gölge öznitelikler](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-syncservice-shadow-attributes)

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md).
- [Karma kimlik nedir? ](whatis-hybrid-identity.md).
