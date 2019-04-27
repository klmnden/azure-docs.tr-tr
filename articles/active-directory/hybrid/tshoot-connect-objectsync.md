---
title: 'Azure AD Connect: Nesne eşitleme sorunlarını giderme | Microsoft Docs'
description: Bu konu ile sorun giderme görevini kullanarak nesne eşitleme sorunlarını gidermek için adımları sağlar.
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
ms.date: 05/15/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82139178d4c1db4774d539180e41e49699d8ee12
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60382510"
---
# <a name="troubleshoot-object-synchronization-with-azure-ad-connect-sync"></a>Azure AD Connect eşitlemesi ile nesne eşitleme sorunlarını giderme
Bu makalede, sorun giderme görevini kullanarak nesne eşitleme sorunlarını giderme için adımları sağlar. Azure Active Directory (Azure AD) Connect sorun giderme nasıl çalıştığını görmek için izleme [bu kısa video](https://aka.ms/AADCTSVideo).

## <a name="troubleshooting-task"></a>Görev sorunlarını giderme
Azure AD için dağıtım 1.1.749.0 sürümü ile bağlantı kurulamadı veya sonraki nesne eşitleme sorunları gidermek için Sihirbazı'nda sorun giderme görevini kullanın. Önceki sürümler için lütfen açıklandığı gibi manuel olarak sorun giderme [burada](tshoot-connect-object-not-syncing.md).

### <a name="run-the-troubleshooting-task-in-the-wizard"></a>Sihirbazı'nda sorun giderme görevini Çalıştır
Sihirbazı'nda sorun giderme görevini çalıştırmak için aşağıdaki adımları gerçekleştirin:

1.  Yeni bir Windows PowerShell oturumunda, Azure AD Connect sunucunuzda olan çalıştırma yönetici seçeneğini açın.
2.  Çalıştırma `Set-ExecutionPolicy RemoteSigned` veya `Set-ExecutionPolicy Unrestricted`.
3.  Azure AD Connect Sihirbazı'nı başlatın.
4.  Ek Görevler sayfasına gidin, sorun giderme seçin ve İleri'ye tıklayın.
5.  Sorun giderme sayfasında, sorun giderme menü PowerShell'de başlamak için Başlat'ı tıklatın.
6.  Ana menüde, nesne eşitleme sorunlarını giderme seçin.
![](media/tshoot-connect-objectsync/objsynch11.png)

### <a name="troubleshooting-input-parameters"></a>Sorun giderme giriş parametreleri
Aşağıdaki giriş parametreleri tarafından sorun giderme görevini gerekir:
1.  **Nesnesinin ayırt edici ad** – Bu, sorun giderme gerektiren nesnesinin ayırt edici ad
2.  **AD Bağlayıcısı adı** – yukarıdaki nesnesi bulunduğu AD ormanı adıdır.
3.  Azure AD kiracısı genel yönetici kimlik bilgileri ![](media/tshoot-connect-objectsync/objsynch1.png)

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Sorun giderme görevini sonuçlarını anlama
Sorun giderme görevini aşağıdaki denetimleri gerçekleştirir:

1.  Nesne Azure Active Directory'ye eşitlenen tümüyse UPN uyuşmazlığı
2.  Nesne etki alanı filtreleme nedeniyle filtre uygulanmış olup olmadığını denetleyin
3.  Nesnenin son OU filtreleme için filtre uygulanmış olup olmadığını denetleyin
4.  Bağlı bir posta kutusu nedeniyle nesne eşitleme engellenip engellenmediğini kontrol edin
5. Nesne eşitlenmesi çalıştırmaması dinamik dağıtım grubu olup olmadığını denetleyin

Bu bölümün geri kalanında, görev tarafından döndürülen belirli sonuçları açıklar. Her durumda, görev, sorunu çözmek için önerilen eylemleri tarafından izlenen analizini sağlar.

## <a name="detect-upn-mismatch-if-object-is-synced-to-azure-active-directory"></a>Nesne Azure Active Directory'ye eşitlenen tümüyse UPN uyuşmazlığı
### <a name="upn-suffix-is-not-verified-with-azure-ad-tenant"></a>UPN soneki, Azure AD Kiracınız ile doğrulanmadı
Zaman UserPrincipalName (UPN) / alternatif oturum açma Kimliğini soneki ile Azure AD Kiracısı doğrulanmamış ve ardından Azure Active Directory UPN soneki varsayılan etki alanı adı "onmicrosoft.com" ile değiştirir.

![](media/tshoot-connect-objectsync/objsynch2.png)

### <a name="changing-upn-suffix-from-one-federated-domain-to-another-federated-domain"></a>UPN soneki bir Federasyon etki alanından başka bir Federasyon etki alanı değiştirme
Azure Active Directory eşitleme UserPrincipalName (UPN) izin verme / alternatif oturum açma Kimliğini soneki değişiklik bir Federasyon etki alanından başka bir Federasyon etki alanı. Bu Azure AD Kiracısı ile doğrulanır ve kimlik doğrulaması türü olarak federe sahip etki alanları için geçerlidir.

![](media/tshoot-connect-objectsync/objsynch3.png) 

### <a name="azure-ad-tenant-dirsync-feature-synchronizeupnformanagedusers-is-disabled"></a>Azure AD Kiracı DirSync özelliğini 'SynchronizeUpnForManagedUsers' devre dışı bırakıldı
Azure AD Kiracısı DirSync özelliğini 'SynchronizeUpnForManagedUsers' devre dışı bırakıldığında, Azure Active Directory eşitleme güncelleştirmeleri UserPrincipalName/alternatif oturum açma kimliği için lisanslı kullanıcı hesapları ile yönetilen kimlik doğrulaması için izin vermez.

![](media/tshoot-connect-objectsync/objsynch4.png)

## <a name="object-is-filtered-due-to-domain-filtering"></a>Etki alanı filtreleme nedeniyle nesne filtrelenmiştir.
### <a name="domain-is-not-configured-to-sync"></a>Etki alanı yapılandırılmadı eşitlemek için
Nesne, yapılandırılmamış bir etki alanı nedeniyle kapsamı dışındadır. Ait olduğu etki alanı filtrelenmiş aşağıdaki örnekte, nesne eşitlenmemiş kapsamdır eşitleme.

![](media/tshoot-connect-objectsync/objsynch5.png)

### <a name="domain-is-configured-to-sync-but-is-missing-run-profilesrun-steps"></a>Etki alanı için yapılandırılmış eşitleme ancak çalıştırma profillerini çalıştırma adımları eksik
Etki alanı eksik olduğundan nesne kapsam dışına profilleri/çalıştırma adımları çalıştırılır. Ait olduğu etki alanı çalıştırma adımları tam çalıştırma profili içeri aktarma için eksik olarak aşağıdaki örnekte, nesne eşitlenmemiş kapsamdır.
![](media/tshoot-connect-objectsync/objsynch6.png)

## <a name="object-is-filtered-due-to-ou-filtering"></a>Nesnenin son OU filtreleme için filtrelenmiştir.
OU filtreleme yapılandırması nedeniyle eşitlenmedi kapsam nesnedir. Aşağıdaki örnekte, nesne OU'ya ait NoSync, DC = bvtadwbackdc, DC = com.  Bu OU'ya eşitleme kapsamında yer almaz.</br>

![KURULUŞ BİRİMİ](./media/tshoot-connect-objectsync/objsynch7.png)

## <a name="linked-mailbox-issue"></a>Bağlı posta kutusu sorunu
Bağlı bir posta kutusu, başka bir hesap güvenilen ormanda bulunan harici bir yönetici hesabı ile ilişkili olduğu varsayılır. Bu tür dış ana hesap var. sonra Azure AD Connect kullanıcı eşitlenmeyecek hesabı Azure AD kiracısı için Exchange ormanında bağlı posta kutusu karşılık gelir.</br>
![Bağlı posta kutusu](./media/tshoot-connect-objectsync/objsynch12.png)

## <a name="dynamic-distribution-group-issue"></a>Dinamik dağıtım grubu sorun
Şirket içi çeşitli farklılıkları nedeniyle Active Directory ve Azure Active Directory, Azure AD Connect eşitleme dinamik dağıtım grupları Azure AD kiracısı için.

![Dinamik dağıtım grubu](./media/tshoot-connect-objectsync/objsynch13.png)

## <a name="html-report"></a>HTML raporu
Nesne çözümlemenin yanı sıra, sorun giderme görevini de bilinen nesnesi hakkında her şeyi içeren bir HTML raporu oluşturur. Bu HTML raporu yapmak için destek ekibi ile paylaşılabilen daha fazla sorun giderme adımı gerekirse.

![](media/tshoot-connect-objectsync/objsynch8.png)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
