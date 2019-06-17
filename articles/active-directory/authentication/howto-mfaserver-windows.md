---
title: Windows kimlik doğrulaması ve Azure MFA sunucusu - Azure Active Directory
description: Windows Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu dağıtılıyor.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: fa52dcf08a5e4b152d9fe0db36710e41a5a79fe7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057317"
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Windows Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu

Uygulamalara yönelik Windows kimlik doğrulamasını etkinleştirmek ve yapılandırmak için Azure Multi-Factor Authentication Sunucusunun Windows Kimlik Doğrulaması bölümünü kullanın. Windows Kimlik Doğrulamasını ayarlamadan önce aşağıdaki listeyi aklınızda bulundurun:

* Ayarladıktan sonra Terminal Hizmetlerinin etkili olması için Azure Multi-Factor Authentication’ı yeniden başlatın.
* “Azure Multi-Factor Authentication kullanıcı eşleşmesi gerektir” seçeneği işaretliyse ve kullanıcı listesinden değilseniz, yeniden başlatma sonrası makinede oturum açamazsınız.
* Güvenilen IP'ler uygulamanın ile kimlik doğrulaması ile istemci IP’si sağlayıp sağlayamayacağına bağlıdır. Şu anda yalnızca Terminal Hizmetleri desteklenmektedir.  

> [!IMPORTANT]
> 1 Temmuz 2019'dan itibaren Microsoft artık yeni dağıtımlar için MFA sunucusu sunacaktır. Bulut tabanlı Azure multi-Factor Authentication, kullanıcıların multi-Factor authentication gerektirmesine istediğiniz yeni müşteriler kullanmanız gerekir. MFA sunucusu 1 Temmuz'dan önce etkinleştirmiş olan mevcut müşteriler, Gelecekteki güncelleştirmelerin en son sürümü indirip zamanki etkinleştirme kimlik bilgileri oluştur mümkün olacaktır.

> [!NOTE]
> Bu özellik, Windows Server 2012 R2’de Terminal Hizmetleri’ni güvenli hale getirmek için desteklenmemektedir.

## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Bir uygulamayı Windows kimlik doğrulaması ile güvenli hale getirmek için aşağıdaki yordamı kullanın.

1. Azure Multi-Factor Authentication Sunucusu’nda Windows Kimlik Doğrulaması simgesine tıklayın.
   ![MFA Sunucusu'nda Windows kimlik doğrulaması](./media/howto-mfaserver-windows/windowsauth.png)
2. **Windows Kimlik Doğrulamasını Etkinleştir** onay kutusunu işaretleyin. Varsayılan olarak, bu kutu işaretlenmemiştir.
3. Uygulamalar sekmesi yöneticinin Windows Kimlik Doğrulaması için bir veya daha fazla uygulamayı yapılandırmasını sağlar.
4. Bir sunucu veya uygulama seçin – sunucusu/uygulama etkin olup olmadığını belirtin. **Tamam**'ı tıklatın.
5. **Ekle...** düğmesine tıklayın.
6. Güvenilen IP'ler sekmesi belirli IP'lerden gelen Windows oturumları için Azure Multi-Factor Authentication’ı atlamanızı sağlar. Örneğin, çalışanlar ofiste ve evden uygulama kullanıyorsa, ofisteyken Azure Multi-Factor Authentication için telefonlarının çalmasını istemediğinize karar verebilirsiniz. Bunun için ofis alt ağını Güvenilen IP’ler girişi olarak belirtebilirsiniz.
7. **Ekle...** düğmesine tıklayın.
8. Tek bir IP adresini atlamak istiyorsanız, **Tek IP**’yi seçin.
9. Tüm IP aralığını atlamak istiyorsanız, **IP Aralığı**’nı seçin. Örnek: 10.63.193.1-10.63.193.100.
10. Bir alt ağ gösterimini kullanarak IP aralığı belirtmek istiyorsanız, **Alt Ağ**’ı seçin. Alt ağın başlangıç IP’sini girin ve açılır listede uygun ağ maskesini seçin.
11. **Tamam**'ı tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure MFA Sunucusu için üçüncü taraf VPN cihazları yapılandırma](howto-mfaserver-nps-vpn.md)

- [Azure MFA’ya yönelik NPS uzantısı ile mevcut kimlik doğrulama altyapınızı büyütün](howto-mfa-nps-extension.md)
