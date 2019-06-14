---
title: Azure AD Connect - Azure AD ile Azure AD Connect kullanarak AD FS güveni yönettiğinizde | Microsoft Docs
description: Azure AD güven Azure AD tarafından işleme işlem ayrıntılarını bağlanın.
keywords: AD FS, ADFS, AD FS yönetimi, AAD Connect, Connect, Azure AD güveni AAD, talep, talep, talep kuralları, verme, dönüşüm, kurallar, yedekleme, geri yükleme
services: active-directory
documentationcenter: ''
ms.reviewer: anandyadavmsft
manager: daveba
ms.subservice: hybrid
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/28/2018
ms.author: billmath
author: billmath
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8bd46bb820c7127c4fa6105fcc0be73bb66024c6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60245694"
---
# <a name="manage-ad-fs-trust-with-azure-ad-using-azure-ad-connect"></a>Azure AD ile Azure AD Connect kullanarak AD FS güvenini yönetme

## <a name="overview"></a>Genel Bakış

Azure AD Connect, şirket içi Active Directory Federasyon Hizmeti (AD FS) ile Azure arasında federasyon yönetebilir AD. Bu makalede, genel bir bakış sağlar:

* Güven Azure AD Connect tarafından yapılandırılan çeşitli ayarları
* Verme dönüştürme kuralları'nı (talep kuralları) Azure AD Connect tarafından ayarlayın
* Yedekleme ve geri yükleme, talep güncelleştirmeleri arasında yükseltme ve yapılandırma kuralları. 

## <a name="settings-controlled-by-azure-ad-connect"></a>Azure AD Connect tarafından denetlenen ayarlar

Azure AD Connect yönetir **yalnızca** Azure AD güveni ile ilişkili ayarlar. Azure AD Connect, tüm ayarlarını diğer bağlı olan taraf güvenlerini AD FS'de değiştirmez. Aşağıdaki tablo, Azure AD Connect tarafından denetlenen ayarlarını belirtir.

| Ayar | Açıklama |
| :--- | :--- |
| Belirteç imzalama sertifikası | Azure AD Connect, sıfırlama ve Azure AD ile güveni yeniden kullanılabilir. Azure AD Connect, AD FS belirteç imzalama sertifikaları, tek seferlik bir anlık geçiş yapar ve Azure AD etki alanı Federasyon ayarlarını güncelleştirir.|
| Belirteç imzalama algoritması | Microsoft, SHA-256'yı imzalama algoritması belirteci olarak kullanılmasını önerir. Azure AD Connect, belirteç imzalama algoritması değerine SHA-256'dan daha az güvenli ayarlanmışsa algılayabilir. Bu işlem, sonraki olası yapılandırma işleminde SHA-256 ayarı güncelleştirir. Yeni belirteç imzalama sertifikası kullanılacak diğer bağlı olan taraf güveni güncelleştirilmesi gerekir. |
| Azure AD güven tanımlayıcısı | Azure AD Connect, Azure AD güveni için doğru tanımlayıcı değerini ayarlar. AD FS tanımlayıcı değerini kullanarak bir Azure AD güvenini benzersiz olarak tanımlar. |
| Azure AD uç noktaları | Azure AD Connect, Azure AD güveni için yapılandırılmış uç noktalar her zaman dayanıklılık ve performans için en son önerilen değerleri uygun olduğunu emin olur. |
| Verme dönüştürme kuralları | Sayılar, Azure AD'de Federasyon ayarı özelliklerinden en iyi performans için gerekli bir talep kuralları vardır. Azure AD Connect, Azure AD güveni her zaman doğru ortaklık önerilen talep kuralları kümesi ile yapılandırılmış emin olur. |
| Alternatif kimlik | Azure AD Connect, eşitleme, alternatif kimlik kullanmak için yapılandırılmışsa, alternatif kimliği kullanarak kimlik doğrulaması gerçekleştirmek için AD FS yapılandırır. |
| Otomatik meta veri güncelleştirme | Azure AD ile güveni otomatik meta veri güncelleştirme için yapılandırılmış. AD FS, düzenli aralıklarla Azure AD güvenini meta verilerini denetler ve Azure AD tarafında değişir durumda güncel tutar. |
| Tümleşik Windows kimlik doğrulaması (IWA) | Hibrit Azure AD'ye katılma işlemi sırasında IWA alt düzey cihazları için hibrit Azure AD'ye katılma kolaylaştırmak cihaz kaydı için etkinleştirilir. |

## <a name="execution-flows-and-federation-settings-configured-by-azure-ad-connect"></a>Yürütme akışları ve Azure AD Connect tarafından yapılandırılan Federasyon ayarları

Azure AD connect yapılandırma akışlar sırasında Azure AD güveni için tüm ayarların güncelleştirmez. Değiştirilen ayarlar hangi görevin veya yürütme akışı yapılıyor bağlıdır. Aşağıdaki tabloda, farklı yürütme akışları etkilenen ayarlar listelenmiştir.

| Yürütme akışı | Etkilenen ayarlar |
| :--- | :--- |
| İlk yükleme (hızlı) geçirin | None |
| İlk yükleme (Yeni AD FS grubu) geçirin. | Yeni bir AD FS grubu oluşturulur ve Azure AD ile bir güven sıfırdan oluşturulur. |
| İlk yükleme (Azure AD güveni mevcut mevcut AD FS grubunu,) geçirin. | Azure AD güven tanımlayıcısı, verme dönüştürme kuralları, Azure AD uç noktaları, diğer kimliği (varsa), otomatik meta veri güncelleştirme |
| Azure AD güvenini sıfırlama | Belirteç imzalama sertifikası, belirteç imzalama algoritması, Azure AD güven tanımlayıcısı, verme dönüştürme, Azure AD uç noktaları, diğer kimliği (varsa), kurallar otomatik meta veri güncelleştirme |
| Federasyon sunucusu ekleme | None |
| WAP sunucusu ekleme | None |
| Cihaz seçenekleri | Verme dönüştürme kuralları, cihaz kaydı için IWA |
| Federasyon etki alanı ekleme | Etki alanı için ilk kez eklendiğinden, diğer bir deyişle, Kurulum birden çok etki alanını Federasyon için çoklu etki alanı Federasyon seçeneğinden değişiyor – Azure AD Connect güven sıfırdan yeniden oluşturur. Azure AD ile güveni için birden çok etki alanı zaten yapılandırılmışsa, verme dönüştürme kuralları yalnızca değiştirilen |
| SSL güncelleştirmesi | None |

Burada, tüm işlemleri sırasında herhangi bir ayarı değiştirilmiş, Azure AD Connect yapar geçerli güven ayarlarının yedeğidir **%ProgramData%\AADConnect\ADFS**

![Mevcut Azure AD güven yedekleme hakkında daha fazla Azure AD Connect sayfası gösterme iletisi](./media/how-to-connect-azure-ad-trust/backup2.png)

> [!NOTE]
> Sürümünden 1.1.873.0, yedekleme yalnızca verme dönüştürme kuralları toplamda ve bunlar Sihirbazı izleme günlük dosyasına yedeklendi.

## <a name="issuance-transform-rules-set-by-azure-ad-connect"></a>Azure AD Connect tarafından verme dönüştürme kuralları ayarlayın

Azure AD Connect, Azure AD güveni her zaman doğru ortaklık önerilen talep kuralları kümesi ile yapılandırılmış emin olur. Azure AD güvenini yönetmek için Azure AD kullanarak Microsoft önerir bağlanın. Bu bölümde, verme dönüştürme kuralları kümesi ve bunların açıklaması listelenir.

| Kural adı | Açıklama |
| --- | --- |
| Sorun UPN | Bu kural, userPrincipalName eşitleme ayarlarında yapılandırılan öznitelik uğradıysa userprincipalname değeri sorgular.|
| ObjectGUID ve msdsconsistencyguid özel Immutableıd talebi için sorgu | Varsa, bu kural bir işlem hattının objectGUID için geçici değer ve msdsconsistencyguid değer ekler. |
| Msdsconsistencyguid varlığını denetle | Msdsconsistencyguid değeri veya mevcut bağlı olarak, biz ne Immutableıd kullanmak doğrudan bir geçici bayrağı ayarlayın |
| Varsa msdsconsistencyguid sabit kimlik verme. | Değer varsa msdsconsistencyguid Immutableıd verme |
| ObjectGuidRule msdsConsistencyGuid kural yoksa, sorunu | ObjectGUID değerini msdsconsistencyguid değeri mevcut değilse Immutableıd verilecek |
| Sorunu NameIdentifier | Bu kural NameIdentifier talebini değerini verir.|
| Etki alanına katılmış bilgisayarlar için sorun accounttype | Bir etki alanına katılmış cihaz kimlik doğrulaması gerçekleştirilen varlık ise bu kural hesap türü DJ gösteren bir etki alanına katılmış cihaz sorunları |
| ' % S'değeri bir bilgisayar hesabı olmadığında kullanıcı sorunu AccountType | Bu kural bir kullanıcı kimlik doğrulaması gerçekleştirilen varlık ise hesap türü kullanıcı olarak verir. |
| Bir bilgisayar hesabı olmadığında issuerıd sorunu | Bu kural, kimlik doğrulama varlığı bir cihaz olmadığında İssuerıd değerini verir. Değer, Azure AD Connect tarafından yapılandırılmış bir normal ifade aracılığıyla oluşturulur. Normal ifade, tüm etki alanlarını göz önünde bulundurarak, Azure AD Connect kullanarak Federasyon sonra oluşturulur. |
| Sorunu issuerıd DJ bilgisayar kimlik doğrulama için | Kimlik doğrulama varlığı, bir cihaz olduğunda bu kural İssuerıd değerini verir. |
| Etki alanına katılmış bilgisayarlar için sorun onpremobjectguid | Bir etki alanına katılmış cihaz kimlik doğrulaması gerçekleştirilen varlık ise bu kural, cihazın şirket içi objectGUID verir. |
| Birincil SID geçirin | Bu kural kimlik doğrulama varlığın birincil SID sorunları |
| Talebi - insideCorporateNetwork geçirin | Bu kural sorunları, Azure AD yardımcı olan bir talep biliyorsanız kurumsal ağ içinde veya harici olarak kimlik doğrulaması geldiğini |
| Talebi – Psso geçirin |   |
| Parola süre sonu talepleri verme | Bu kural sorunlarını üç, parolasını değiştirmek için yönlendirmek nereye parola sona erme saati, gün için parola kimlik doğrulaması gerçekleştirilen varlığı süresinin dolmasını ve URL için talep.|
| Talebi – authnmethodsreferences geçirin | Bu kural altında verilen talep değeri hangi kimlik doğrulaması türü için varlık gerçekleştirildiğini gösterir |
| Talebi - multifactorauthenticationinstant geçirin | Birden çok faktörlü kimlik doğrulaması gerçekleştirilen kullanıcının son bu talep değerini, UTC zamanı belirtir. |
| Talebi - AlternateLoginID geçirin | Alternatif bir oturum açma kimliğini kullanarak kimlik doğrulaması gerçekleştirildi, bu kural, AlternateLoginID talebi verir. |

> [!NOTE]
> Varsayılan olmayan seçim sırasında Azure AD Connect yapılandırması kullanıyorsanız, sorun UPN ve Immutableıd için talep kurallarını farklılık gösterir

## <a name="restore-issuance-transform-rules"></a>Verme dönüştürme kuralları'geri yükleme

Azure AD Connect sürümü 1.1.873.0 veya üzeri yapar yedek bir kopyası Azure AD güven ayarları Azure AD güven ayarlarında bir güncelleştirme yapıldığında. Azure AD güven ayarlarını adresindeki yedeklenir **%ProgramData%\AADConnect\ADFS**. Dosya adı şu biçimde AadTrust - olan&lt;tarih&gt;-&lt;zaman&gt;.txt, örneğin - 150216.txt 20180710 AadTrust

![Örneğin bir ekran görüntüsü Azure AD güvenini yedekleme](./media/how-to-connect-azure-ad-trust/backup.png)

Verme dönüştürme kuralları aşağıda önerilen adımları kullanarak geri yükleme

1. Sunucu Yöneticisi'nde kullanıcı Arabirimi AD FS Yönetimi'ni açın
2. Azure AD güven giderek özellikleri **AD FS &gt; bağlı olan taraf güvenlerini &gt; Microsoft Office 365 kimlik platformu &gt; talep verme ilkesini Düzenle**
3. Tıklayarak **Kuralı Ekle**
4. Talep kuralı şablon içinde bir özel kural kullanarak talepleri gönder'i seçin ve **İleri**
5. Talep kuralı adı yedekleme dosyasından kopyalayın ve alana yapıştırın **talep kuralı adı**
6. Talep kuralı metin alanı için yedekleme dosyasından kopyalayın **özel kural** tıklatıp **son**

> [!NOTE]
> Ek kuralları, Azure AD Connect tarafından yapılandırılan kuralları ile çakışmadığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect kullanarak Active Directory Federasyon Hizmetleri özelleştirme ve yönetme](how-to-connect-fed-management.md)
