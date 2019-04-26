---
title: Azure Multi-Factor Authentication pilotu etkinleştirme
description: Bu öğreticide bir pilot kullanıcı grubu için Azure AD Multi-Factor Authentication özelliğini etkinleştireceksiniz
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5fdf88ed6cedaa38676a56536ff1eda7ee6bca66
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60413973"
---
# <a name="tutorial-complete-an-azure-multi-factor-authentication-pilot-roll-out"></a>Öğretici: Bir Azure multi-Factor Authentication pilot dağıtım tamamlayın

Bu öğreticide Azure portalda oturum açma sırasında Azure Multi-Factor Authentication (Azure MFA) özelliğini etkinleştiren bir koşullu erişim ilkesini yapılandırma adımları anlatılmaktadır. İlke sınırlı sayıda kullanıcıdan oluşan pilot grupta dağıtılmakta ve test edilmektedir. Azure MFA'nın koşullu erişim kullanılarak dağıtılması, kuruluşlar ve yöneticiler için geleneksel zorlamalı yönteme kısayla önemli ölçüde esneklik sunar.

> [!div class="checklist"]
> * Azure Multi-Factor Authentication’ı etkinleştirme
> * Azure Multi-Factor Authentication'ı test etme

## <a name="prerequisites"></a>Önkoşullar

* En az deneme sürümü lisansı etkinleştirilmiş çalışan bir Azure AD kiracısına erişim.
* Genel Yönetici ayrıcalıklarına sahip olan bir hesap.
* Bildiğiniz test etmek için bir kullanıcı oluşturmanız gerekiyorsa bir parola ile bir yönetici olmayan test kullanıcısını görmek makaleyi [hızlı başlangıç: Azure Active Directory'ye yeni kullanıcı ekleme](../add-users-azure-active-directory.md).
* Bu yönetici olmayan kullanıcının üyesi olduğu pilot grup. Grup oluşturmanız gerekiyorsa [Azure Active Directory'de grup oluşturma ve üye ekleme](../active-directory-groups-create-azure-portal.md) makalesine bakın.

## <a name="enable-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication’ı etkinleştirme

1. Genel Yönetici hesabını kullanarak [Azure portal](https://portal.azure.com) oturumu açın.
1. **Azure Active Directory**, **Koşullu erişim** sayfasına gidin
1. **Yeni ilke**'yi seçin
1. İlkenize **MFA Pilot** adını verin
1. **Kullanıcılar ve gruplar** bölümünde **Kullanıcı ve grupları seçin** radyo düğmesini seçin
    * Bu makalenin ön koşullar bölümünde oluşturduğunuz pilot grubunuzu seçin
    * **Bitti**’ye tıklayın
1. **Bulut uygulamaları** bölümünde **Uygulama seç** radyo düğmesini seçin
    * Azure portal için bulut uygulaması **Microsoft Azure Management** olacaktır
    * **Seç**'e tıklayın
    * **Bitti**’ye tıklayın
1. **Koşullar** bölümünü atlayın
1. **Erişim İzni Verme** bölümünde **Erişim izni ver** radyo düğmesinin seçili olduğundan emin olun
    * **Çok faktörlü kimlik doğrulaması gerektir** kutusunu işaretleyin
    * **Seç**'e tıklayın
1. **Oturum** bölümünü atlayın
1. **İlkeyi etkinleştir** ayarını **Açık** olarak değiştirin
1. **Oluştur**'a tıklayın

## <a name="test-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication'ı test etme

Koşullu erişim ilkesinin çalıştığından emin olmak için MFA gerektirmeyen bir kaynakta oturum açmayı ve ardından MFA gerektiren Azure portalda oturum açmayı test etmeniz gerekir.

1. InPrivate modunda veya gizli modda yeni bir tarayıcı penceresi açın ve [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com) adresine gidin.
   * Bu makalenin ön koşullar bölümünde oluşturduğunuz test kullanıcısıyla oturum açın ve MFA istemediğine dikkat edin.
   * Tarayıcı penceresini kapatın.
2. InPrivate modunda veya gizli modda yeni bir tarayıcı penceresi açın ve [https://portal.azure.com](https://portal.azure.com) adresine gidin.
   * Bu makalenin ön koşullar bölümünde oluşturduğunuz test kullanıcısıyla oturum açın ve şimdi Azure Multi-Factor Authentication kaydı ve kullanımı gerektiğine not edin.
   * Tarayıcı penceresini kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide yapılandırdığınız işlevi kullanmak istemediğinize karar verirseniz aşağıdaki değişikliği yapın.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Azure Active Directory**, **Koşullu erişim** sayfasına gidin.
1. Oluşturduğunuz koşullu erişim ilkesini seçin.
1. **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Multi-Factor Authentication özelliğini etkinleştirdiniz. Azure Kimlik Koruması özelliklerinin self servis parola sıfırlama ve Multi-Factor Authentication deneyimleriyle nasıl tümleştirileceğini görmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Oturum açma sırasında risk değerlendirmesi yapma](tutorial-risk-based-sspr-mfa.md)
