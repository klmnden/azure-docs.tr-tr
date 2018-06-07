---
title: 'Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme | Microsoft Docs'
description: Açıklar nasıl Azure AD Connect eşitleme çalışır ve nasıl özelleştirileceğini.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: ee4bf802-045b-4da0-986e-90aba2de58d6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.component: hybrid
ms.author: markvi
ms.openlocfilehash: 6b2724f4c9511d606ab8eeac2dedea8759283883
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34595266"
---
# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme
Azure Active Directory Connect Eşitleme Hizmetleri (Azure AD Connect eşitleme) Azure AD Connect ana bileşenidir. Bu şirket içi ortamınız ile Azure AD arasındaki kimlik verilerini eşitlemek için ilgili tüm işlemleri ilgilenir. Azure AD Connect eşitleme DirSync, Azure AD eşitleme ve Azure Active Directory yapılandırılmış Bağlayıcısı ile Forefront Identity Manager ardılı ' dir.

Bu konu için giriş olan **Azure AD Connect eşitleme** (olarak da bilinir **eşitleme altyapısı**) ve işle ilgili diğer konulara yönelik bağlantılar listeler. Azure AD Connect bağlantıları için bkz: [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).

Eşitleme hizmeti şirket içi iki bileşenden oluşur **Azure AD Connect eşitleme** bileşeni ve Azure AD'de Hizmet tarafı adlı **Azure AD Connect eşitleme hizmeti**.

## <a name="azure-ad-connect-sync-topics"></a>Azure AD Connect eşitleme konuları
| Konu | Ne kapsar ve okumak ne zaman |
| --- | --- |
| **Azure AD Connect eşitleme temelleri** | |
| [Mimarisini anlama](active-directory-aadconnectsync-understanding-architecture.md) |Bu eşitleme altyapısında yenidir ve mimari ve kullanılan terimler hakkında bilgi edinmek isteyen, için. |
| [Teknik kavramlar](active-directory-aadconnectsync-technical-concepts.md) |Mimari konu ve kısaca kısa bir sürümünü kullanılan terimler açıklanmaktadır. |
| [Azure AD Connect için topolojiler](active-directory-aadconnect-topologies.md) |Eşitleme altyapısı desteklediği senaryolar ve farklı topolojileri açıklanır. |
| **Özel yapılandırma** | |
| [Yükleme Sihirbazı'nı yeniden çalıştırmayı](active-directory-aadconnectsync-installation-wizard.md) |Ne seçeneklerini açıklar sahip Azure AD Connect Yükleme Sihirbazı'nı yeniden çalıştırdığınızda kullanılabilir. |
| [Bildirim temelli hazırlama anlama](active-directory-aadconnectsync-understanding-declarative-provisioning.md) |Bildirim temelli hazırlama adlı yapılandırma modeli açıklar. |
| [Bildirim temelli hazırlama ifadelerini anlama](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |Bildirim temelli hazırlama kullanılan ifade dili sözdizimi açıklar. |
| [Varsayılan yapılandırmayı anlama](active-directory-aadconnectsync-understanding-default-configuration.md) |Out-of-box kurallarının ve varsayılan yapılandırma açıklanmaktadır. Ayrıca kuralları çalışmaya out-of-box senaryoları için birlikte nasıl çalıştığını açıklar. |
| [Kullanıcıları ve kişileri anlama](active-directory-aadconnectsync-understanding-users-and-contacts.md) |Önceki konuyla ilgili devam eder ve kullanıcıları ve kişileri yapılandırması birlikte, özellikle bir Çoklu orman ortamında nasıl çalıştığını açıklar. |
| [Varsayılan yapılandırma bir değişiklik yapma](active-directory-aadconnectsync-change-the-configuration.md) |Bir ortak yapılandırma öznitelik akışları değişikliği yapma size yol göstermektedir. |
| [Varsayılan yapılandırmanın değiştirilmesine ilişkin önerilen yöntemler](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) |Destek sınırlamaları ve out-of-box yapılandırmasında değişiklik yapılmadan için. |
| [Filtrelemeyi Yapılandırma](active-directory-aadconnectsync-configure-filtering.md) |Hangi nesnelerin bu seçenekleri yapılandırmak nasıl eşitlenmiş Azure ad ve adım adım nasıl sınırlandırılacağını farklı seçenekleri açıklar. |
| **Özellikler ve senaryolar** | |
| [Yanlışlıkla silmeleri engelleme](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) |Açıklar *yanlışlıkla silmeleri engelleme* özelliği ve nasıl yapılandırılacağı. |
| [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) |İçeri aktarma, eşitleme ve verileri dışarı aktarma yerleşik bir zamanlayıcı açıklar. |
| [Parola karma eşitleme uygulama](active-directory-aadconnectsync-implement-password-hash-synchronization.md) |Parola Eşitleme nasıl çalışır, nasıl uygulanacağını ve çalışır ve sorun giderme yöntemleri açıklar. |
| [Cihaz geri yazma](active-directory-aadconnect-feature-device-writeback.md) |Cihaz geri yazma Azure AD Connect nasıl çalıştığı açıklanmaktadır. |
| [Dizin genişletmeleri](active-directory-aadconnectsync-feature-directory-extensions.md) |Kendi özel özniteliklere sahip Azure AD şemasını açıklar. |
| [Office 365 PreferredDataLocation](active-directory-aadconnectsync-feature-preferreddatalocation.md) |Kullanıcı ile aynı bölgede kullanıcının Office 365 kaynakları put açıklar. |
| **Eşitleme hizmeti** | |
| [Azure AD Connect eşitleme hizmeti özellikleri](active-directory-aadconnectsyncservice-features.md) |Eşitleme hizmeti yan ve Azure AD eşitleme ayarlarını değiştirme açıklar. |
| [Yinelenen öznitelik dayanıklılık](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) |Etkinleştirmek ve kullanmak açıklar **userPrincipalName** ve **proxyAddresses** yinelenen öznitelik değerleri dayanıklılık. |
| **İşlemler ve kullanıcı Arabirimi** | |
| [Eşitleme Hizmeti Yöneticisi](active-directory-aadconnectsync-service-manager-ui.md) |Eşitleme Hizmeti Yöneticisi'ni de dahil olmak üzere kullanıcı Arabirimi açıklar [Operations](active-directory-aadconnectsync-service-manager-ui-operations.md), [Bağlayıcılar](active-directory-aadconnectsync-service-manager-ui-connectors.md), [meta veri deposu Tasarımcısı](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md), ve [meta veri deposu arama](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) sekmeleri. |
| [İşletimsel görevleri ve ilgili önemli noktalar](active-directory-aadconnectsync-operations.md) |Olağanüstü durum kurtarma gibi işletimsel sorunlar açıklanmaktadır. |
| **Nasıl Yapılır...** | |
| [Azure AD hesabının Sıfırla](active-directory-aadconnectsync-howto-azureadaccount.md) |Azure AD Connect Eşitleme'den Azure AD'ye bağlanmak için kullanılan hizmet hesabının kimlik bilgilerini sıfırlamak nasıl. |
| **Daha fazla bilgi ve başvurular** | |
| [Bağlantı Noktaları](active-directory-aadconnect-ports.md) |Eşitleme altyapısı ve şirket içi dizinlere ve Azure AD arasında açmanız hangi bağlantı noktalarını listeler. |
| [Azure Active Directory ile eşitlenen öznitelikler](active-directory-aadconnectsync-attributes-synchronized.md) |AD ve Azure AD şirket içi arasında eşitlenen tüm öznitelikleri listeler. |
| [İşlevler Başvurusu](active-directory-aadconnectsync-functions-reference.md) |Bildirim temelli hazırlama kullanılabilen tüm işlevleri listeler. |

## <a name="additional-resources"></a>Ek Kaynaklar
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
