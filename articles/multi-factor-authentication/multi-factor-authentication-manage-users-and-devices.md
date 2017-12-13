---
title: "Yöneticileri, kullanıcılar ve aygıtlar - Azure MFA yönetme | Microsoft Docs"
description: "Bu güçlü işlem yeniden yapmak için kullanıcıların zorlama gibi kullanıcı ayarlarının nasıl değiştirileceğini açıklar."
documentationcenter: 
services: multi-factor-authentication
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: aac3b922-7cc1-428c-9044-273579aa7b5a
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: 718ac36c8f4ca2367f90ad51edb175715194a17c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Bulutta Azure multi-Factor Authentication ile kullanıcı ayarlarını yönetme
Yönetici olarak, aşağıdaki kullanıcı ve aygıt ayarlarını yönetebilirsiniz:

* Kullanıcıların iletişim yöntemlerini yeniden belirtmelerini iste
* Uygulama parolalarını Sil
* Tüm güvenilen aygıtlarda MFA gerektirir 

## <a name="require-users-to-provide-contact-methods-again"></a>Kullanıcıların iletişim yöntemlerini yeniden belirtmelerini iste
Bu ayar, kayıt işlemini tamamlamak için yeniden kullanıcı zorlar. Tarayıcı olmayan uygulamalar, kullanıcı bunlar için uygulama parolaları varsa çalışmaya devam eder.  Kullanıcıların uygulama parolaları ayrıca seçerek silebilirsiniz **seçilen kullanıcılar tarafından oluşturulan mevcut tüm uygulama parolalarını Sil**.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Nasıl kullanıcıların iletişim yöntemlerini yeniden belirtmelerini iste
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**. Çok faktörlü kimlik doğrulaması sayfasını açar. 
4. Yönetmek istediğiniz kullanıcıları ve kullanıcı yanındaki kutuyu işaretleyin. Hızlı adım seçeneklerin bir listesini sağ tarafta görünür. 
5. Seçin **kullanıcı ayarlarını yönetin**.
6. Onay kutusunu için **seçilen kullanıcıların iletişim yöntemlerini yeniden belirtmelerini iste**.
   ![İletişim yöntemleri sağlar](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
7. **Kaydet**’e tıklayın.
8. Tıklatın **kapatmak**.

## <a name="delete-users-existing-app-passwords"></a>Uygulama parolaları varolan kullanıcıları Sil
Bu ayar, bir kullanıcının oluşturduğu uygulama parolaları siler. Bu uygulama parolalarıyla ilişkilendirilmiş tarayıcı olmayan uygulamaları yeni bir parola oluşturuluncaya kadar çalışmasını durdurabilir.

### <a name="how-to-delete-users-existing-app-passwords"></a>Uygulama parolaları varolan kullanıcıları silme
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**. Çok faktörlü kimlik doğrulaması sayfasını açar. 
6. Yönetmek istediğiniz kullanıcıları ve kullanıcı yanındaki kutuyu işaretleyin. Hızlı adım seçeneklerin bir listesini sağ tarafta görünür. 
7. Seçin **kullanıcı ayarlarını yönetin**.
8. Onay kutusunu için **seçilen kullanıcılar tarafından oluşturulan mevcut tüm uygulama parolalarını Sil**.
   ![Uygulama parolalarını Sil](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
9. **Kaydet**’e tıklayın.
10. Tıklatın **kapatmak**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>MFA bir kullanıcı için tüm hatırlanan cihazlarda geri yükle
Azure çok faktörlü kimlik doğrulamasının yapılandırılabilir özelliklerden biri, kullanıcılarınızın cihaz güvenilir olarak işaretlemek için seçeneği veren. Daha fazla bilgi için bkz: [Azure çok faktörlü kimlik doğrulaması yapılandırma ayarlarını](multi-factor-authentication-whats-next.md#remember-multi-factor-authentication-for-devices-that-users-trust).

Kullanıcılar, yapılandırılabilir bir normal cihazlarına gün sayısı için iki aşamalı doğrulamayı dışında seçebilirsiniz. Güvenilir bir cihaz kaybolur ya da bir hesap güvenliği aşılmış, güvenilen durumunu kaldırın ve yeniden iki aşamalı doğrulama gerektiren gerekir.

**Tüm hatırlanan cihazlarda geri yükleme çok faktörlü kimlik doğrulaması** kullanıcı olacağı anlamına gelir ayarı yüküyle bunlar içinde olup olmadığını cihazlarını olarak işaretlemek seçtikleri bağımsız olarak bir sonraki oturum açışınızda iki aşamalı doğrulamayı gerçekleştirmek için güvenilir. 

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>Bir kullanıcı için askıya alınmış tüm cihazlarda MFA geri yükleme
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**. Çok faktörlü kimlik doğrulaması sayfasını açar. 
6. Yönetmek istediğiniz kullanıcıları ve kullanıcı yanındaki kutuyu işaretleyin. Hızlı adım seçeneklerin bir listesini sağ tarafta görünür. 
7. Seçin **kullanıcı ayarlarını yönetin**.
8. Onay kutusunu için **tüm hatırlanan cihazlarda geri yükleme çok faktörlü kimlik doğrulaması**
   ![uygulama parolalarını Sil](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. **Kaydet**’e tıklayın.
10. Tıklatın **kapatmak**.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında daha fazla bilgi almak [Azure çok faktörlü kimlik doğrulaması yapılandırma ayarları](multi-factor-authentication-whats-next.md)

- Kullanıcılarınızın yardıma gereksinim duyarsanız, bunları doğru noktası [iki aşamalı doğrulama için Kullanıcı Kılavuzu](./end-user/multi-factor-authentication-end-user.md)
