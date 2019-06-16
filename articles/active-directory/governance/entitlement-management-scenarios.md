---
title: Azure AD hak yönetimi (Önizleme) - Azure Active Directory yaygın senaryoları
description: Azure Active Directory hak yönetimi (Önizleme) içinde ortak senaryolar için izlemeniz gereken üst düzey adımları öğrenin.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/23/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6a50f4a8a63022668dac68c974f8c828c72777c9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66473167"
---
# <a name="common-scenarios-in-azure-ad-entitlement-management-preview"></a>Yaygın senaryoları Azure AD hak yönetimi (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) Yetkilendirme Yönetimi, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Kuruluşunuz için hak yönetimini yapılandırmadan birkaç yolu vardır. Ancak, yeni başlıyorsanız, Yöneticiler, onaylayanlar ve istek sahipleri için yaygın senaryoları anlamanıza yardımcı olur.

## <a name="administrators"></a>Yöneticiler

### <a name="im-new-to-entitlement-management-and-i-want-help-with-getting-started"></a>Hak Yönetimi için yeni ben ve çalışmaya başlama konusunda Yardım istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | [İlk erişim paketinizi oluşturmak için öğreticiyi izleyin](entitlement-management-access-package-first.md) | [![Azure portal simgesi](./media/entitlement-management-scenarios/azure-portal.png)](./media/entitlement-management-scenarios/azure-portal-expanded.png#lightbox) |

### <a name="i-want-to-allow-users-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>Gruplar, uygulamalara veya SharePoint siteleri erişim istemek için dizinimdeki kullanıcıların istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** [Bir katalogda yeni erişim paketi oluştur](entitlement-management-access-package-create.md#start-new-access-package) | ![Bir erişim paketi oluşturun](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Paket erişmek için kaynak rolleri Ekle](entitlement-management-access-package-edit.md#add-resource-roles)<ul><li>Gruplar</li><li>Uygulamalar</li><li>SharePoint siteleri</li></ul> | ![Kaynak rolleri Ekle](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [İlke Ekle](entitlement-management-access-package-edit.md#policy-for-users-in-your-directory)<ul><li>Dizininizdeki kullanıcılar için</li><li>Onay iste</li><li>Süre sonu ayarları</li></ul> | ![İlke ekleme](./media/entitlement-management-scenarios/policy.png) |

### <a name="i-want-to-allow-users-from-my-business-partners-directory-including-users-not-yet-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>(Henüz kullanıcıları dizinimdeki'dahil) iş ortakları Dizinimi kullanıcı grupları, uygulamaları veya SharePoint siteleri için erişim isteği izin istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** [Bir katalogda yeni erişim paketi oluştur](entitlement-management-access-package-create.md#start-new-access-package) | ![Bir erişim paketi oluşturun](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Paket erişmek için kaynak rolleri Ekle](entitlement-management-access-package-edit.md#add-resource-roles) | ![Kaynak rolleri Ekle](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [Dış kullanıcılar için ilke Ekle](entitlement-management-access-package-edit.md#policy-for-users-not-in-your-directory)<ul><li>Kullanıcılar için dizin içinde değil</li><li>Onay iste</li><li>Süre sonu ayarları</li></ul> | ![Dış kullanıcılar için ilke Ekle](./media/entitlement-management-scenarios/policy-external.png) |
> | **4.** [İş ortağınız için erişim paket isteği için My erişim portalı bağlantısı Gönder](entitlement-management-access-package-edit.md#copy-my-access-portal-link)<ul><li>İş ortağı bağlantı, kullanıcılarla paylaşabilirsiniz.</li></ul> |  |

### <a name="i-want-to-change-the-groups-applications-or-sharepoint-sites-in-an-access-package"></a>Grupları, uygulamaları veya SharePoint sitelerine erişim paket değiştirmek istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** Açık erişim paketi | ![Kaynak rolleri Ekle](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Kaynak rolleri Ekle Kaldır](entitlement-management-access-package-edit.md#add-resource-roles) | ![Kaynak rolleri Ekle](./media/entitlement-management-scenarios/resource-roles-add.png) |

### <a name="i-want-to-view-who-has-an-assignment-to-groups-applications-or-sharepoint-sites"></a>Atama grupları, uygulamaları veya SharePoint siteleri için kimlerin görüntülemek istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** Bir erişim paketi açılamadı | ![Kaynak rolleri Ekle](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Atamalarını görüntüle](entitlement-management-access-package-edit.md#view-who-has-an-assignment)<ul><li>Hangi kullanıcıların erişim paket erişimi görüntüle</li><li>Hangi kullanıcının erişim süresi görünümü</li></ul> |  |

### <a name="i-want-to-view-groups-applications-or-sharepoint-sites-a-user-has-access-to"></a>Grupları, uygulamaları veya bir kullanıcının erişimi olan SharePoint sitelerini görüntülemek istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | [Kullanıcı atamalarını raporunu görüntüle](entitlement-management-reports.md)<ul><li>Bunlar istenen Görünüm ve kimin Onaylandı</li></ul> |  |

## <a name="approvers"></a>Onaylayanlar

### <a name="i-want-to-approve-requests-to-access-groups-applications-or-sharepoint-sites"></a>Grupları, uygulamaları veya SharePoint sitelerine erişmek için istekleri onaylamak istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** [My erişim Portalı'nda açık isteği](entitlement-management-request-approve.md#open-request) | [![My erişim portalı simgesi](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** [Erişim isteği Onayla](entitlement-management-request-approve.md#approve-or-deny-request) | ![Erişimi onayla](./media/entitlement-management-scenarios/approve-access.png) |

## <a name="requestors"></a>İstek sahipleri

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-available-to-me-and-request-access"></a>Grupları, uygulamaları veya SharePoint siteleri Kullanabileceğim görüntülemek ve istek erişimi istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** [My erişim portalında oturum açın](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![My erişim portalı simgesi](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Paket erişim bulunamıyor |  |
> | **3.** [Erişim isteği](entitlement-management-request-access.md#request-an-access-package) | ![Erişim izni iste](./media/entitlement-management-scenarios/request-access.png) |

### <a name="im-an-external-user-and-i-want-to-request-access-to-groups-applications-or-sharepoint-sites-with-a-direct-link"></a>Ben bir dış kullanıcı ve gruplar, uygulamalara veya doğrudan bağlantısını içeren bir SharePoint sitelerine erişim isteyeceğiniz

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** [Aldığınız My erişim portalı bağlantısı Bul](entitlement-management-access-package-edit.md#copy-my-access-portal-link) |  |
> | **2.** [My erişim portalında oturum açın](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![My erişim portalı simgesi](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **3.** [Erişim isteği](entitlement-management-request-access.md#request-an-access-package) | ![Dış kullanıcı erişim isteği](./media/entitlement-management-scenarios/request-access-external.png) |

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-i-already-have-access-to"></a>Grupları, uygulamaları veya SharePoint siteleri zaten erişim sahibi görüntülemek istiyorum

> [!div class="mx-tableFixed"]
> | Adımlar | Örnek |
> | --- | --- |
> | **1.** [My erişim portalında oturum açın](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![My erişim portalı simgesi](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Etkin erişim paketleri görüntüle |  |

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: İlk erişim paketinizi oluşturmak](entitlement-management-access-package-first.md)
- [Var olan bir erişim paketini düzenleme ve yönetme](entitlement-management-access-package-edit.md)
