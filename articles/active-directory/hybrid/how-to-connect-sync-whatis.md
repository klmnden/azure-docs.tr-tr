---
title: 'Azure AD Connect eşitleme: Anlama ve eşitleme özelleştirme | Microsoft Docs'
description: Açıklayan nasıl Azure AD Connect eşitleme çalışır ve nasıl özelleştirileceği.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ee4bf802-045b-4da0-986e-90aba2de58d6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e3b87f40d75d4045155e7dd953dc76ffd9de2b34
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60348748"
---
# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect eşitleme: Anlama ve eşitleme özelleştirme
Azure Active Directory Connect Eşitleme hizmetlerini (Azure AD Connect eşitleme) Azure AD Connect ana bileşenidir. Bu, şirket içi ortamınız ile Azure AD arasındaki kimlik verilerini eşitlemek üzere ilgili tüm işlemlerin üstlenir. Azure AD Connect eşitleme DirSync, Azure AD eşitleme ve Azure Active Directory yapılandırılmış Bağlayıcısı ile Forefront Identity Manager'ın yerine geçmiştir.

Bu konu için platformdur **Azure AD Connect eşitleme** (olarak da adlandırılan **eşitleme altyapısı**) ve onunla ilgili diğer konulara bağlantılar listeler. Azure AD Connect bağlantıları için bkz [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md).

Eşitleme hizmeti iki bileşenden, şirket içi oluşur **Azure AD Connect eşitleme** bileşeni ve Azure AD'de Hizmet tarafı adlı **Azure AD Connect eşitleme hizmeti**.

## <a name="azure-ad-connect-sync-topics"></a>Azure AD Connect eşitleme konuları
| Konu | Neleri kapsar ve ne zaman okuma |
| --- | --- |
| **Azure AD Connect eşitleme temelleri** | |
| [Mimariyi anlama](concept-azure-ad-connect-sync-architecture.md) |Bu eşitleme altyapısında yenidir ve mimariyi ve kullanılan terimler hakkında bilgi edinmek istiyorsanız için. |
| [Teknik kavramlar](how-to-connect-sync-technical-concepts.md) |Kısa bir sürüm mimarisi konusunda ve kısaca kullanılan terimler açıklanmaktadır. |
| [Azure AD Connect için topolojiler](plan-connect-topologies.md) |Senaryoları eşitleme altyapısını destekler ve farklı topolojileri açıklanır. |
| **Özel yapılandırma** | |
| [Yükleme Sihirbazı'nı yeniden çalıştırmayı](how-to-connect-installation-wizard.md) |Ne seçeneklerini açıklar, Azure AD Connect Yükleme Sihirbazı'nı yeniden çalıştırdığınızda kullanılabilir olan. |
| [Bildirim temelli sağlama anlama](concept-azure-ad-connect-sync-declarative-provisioning.md) |Bildirim temelli sağlama adlı yapılandırma modeli açıklar. |
| [Bildirim temelli sağlama ifadelerini anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md) |Bildirim temelli sağlama kullanılan ifade dili sözdizimi açıklar. |
| [Varsayılan yapılandırmayı anlama](concept-azure-ad-connect-sync-default-configuration.md) |Kullanıma hazır kuralları ve varsayılan yapılandırmayı açıklar. Ayrıca kuralları çalışmak kullanıma hazır senaryoları için birlikte nasıl çalıştığını açıklar. |
| [Kullanıcıları ve kişileri anlama](concept-azure-ad-connect-sync-user-and-contacts.md) |Önceki konu devam eder ve nasıl kullanıcıları ve kişileri yapılandırmasını birlikte, özellikle çok ormanlı bir ortamda çalıştığını açıklar. |
| [Bir varsayılan yapılandırmaya bir değişiklik yapma](how-to-connect-sync-change-the-configuration.md) |Bir ortak yapılandırma öznitelik akışları değişikliği yapma konusunda yol göstermektedir. |
| [Varsayılan yapılandırmanın değiştirilmesine ilişkin önerilen yöntemler](how-to-connect-sync-best-practices-changing-default-configuration.md) |Destek sınırlamaları ve kullanıma hazır yapılandırmaya değişiklik yapmak için. |
| [Filtrelemeyi Yapılandırma](how-to-connect-sync-configure-filtering.md) |Hangi nesnelerin Azure AD'ye eşitlenen ve adım adım bu seçeneklerini yapılandırmak nasıl gönderildiğini nasıl sınırlamak farklı seçenekleri açıklar. |
| **Özellikler ve senaryolar** | |
| [Yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md) |Açıklar *yanlışlıkla silmeleri engelleme* özellik ve nasıl yapılandırılacağı. |
| [Scheduler](how-to-connect-sync-feature-scheduler.md) |İçeri aktarma, eşitleme ve verileri dışarı aktarma yerleşik bir zamanlayıcı açıklar. |
| [Parola Karması eşitlemeyi uygulama](how-to-connect-password-hash-synchronization.md) |Parola Eşitleme nasıl çalışır, nasıl uygulanacağı ve çalışır ve sorunlarını gidermeyi açıklar. |
| [Cihaz geri yazma](how-to-connect-device-writeback.md) |Cihaz geri yazma Azure AD Connect'i nasıl çalıştığı açıklanır. |
| [Dizin genişletmeleri](how-to-connect-sync-feature-directory-extensions.md) |Kendi özel öznitelikler ile Azure AD şemasını açıklar. |
| [Office 365 PreferredDataLocation](how-to-connect-sync-feature-preferreddatalocation.md) |Kullanıcının Office 365 kaynaklarına kullanıcı ile aynı bölgede put işlemini açıklamaktadır. |
| **Eşitleme hizmeti** | |
| [Azure AD Connect eşitleme hizmeti özellikleri](how-to-connect-syncservice-features.md) |Azure AD'de eşitleme ayarlarını değiştirmek eşitleme hizmeti yan ve açıklar. |
| [Yinelenen öznitelik dayanıklılığı](how-to-connect-syncservice-duplicate-attribute-resiliency.md) |Etkinleştirme ve kullanma işlemini açıklamaktadır **userPrincipalName** ve **proxyAddresses** yinelenen öznitelik değerleri dayanıklılığı. |
| **İşlemler ve kullanıcı Arabirimi** | |
| [Eşitleme Hizmeti Yöneticisi](how-to-connect-sync-service-manager-ui.md) |Eşitleme Hizmeti Yöneticisi dahil olmak üzere kullanıcı Arabirimi açıklar [işlemleri](how-to-connect-sync-service-manager-ui-operations.md), [Bağlayıcılar](how-to-connect-sync-service-manager-ui-connectors.md), [meta veri deposu Tasarımcısı](how-to-connect-sync-service-manager-ui-mvdesigner.md), ve [metaverideposuarama](how-to-connect-sync-service-manager-ui-mvsearch.md) sekmeler. |
| [İşletimsel görevler ve önemli noktalar](how-to-connect-sync-operations.md) |Olağanüstü durum kurtarma gibi işletimsel sorunlar açıklanmaktadır. |
| **Nasıl Yapılır...** | |
| [Azure AD hesabını Sıfırla](how-to-connect-azureadaccount.md) |Azure AD Connect Eşitleme'den Azure AD'ye bağlanmak için kullanılan hizmet hesabının kimlik bilgilerini sıfırlamak nasıl. |
| **Daha fazla bilgi ve başvurular** | |
| [Bağlantı Noktaları](reference-connect-ports.md) |Eşitleme altyapısı ve şirket içi dizinlere ve Azure AD arasında açmanız hangi bağlantı noktalarını listeler. |
| [Azure Active Directory ile eşitlenen öznitelikler](reference-connect-sync-attributes-synchronized.md) |Şirket içi arasında AD ve Azure AD'ye eşitlenen tüm öznitelikleri listeler. |
| [İşlevler Başvurusu](reference-connect-sync-functions-reference.md) |Bildirim temelli sağlama kullanılabilir tüm işlevleri listeler. |

## <a name="additional-resources"></a>Ek Kaynaklar
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
