---
title: Azure AD SSPR pilotunu etkinleştirme
description: Bu öğreticide bir pilot kullanıcı grubu için Azure AD self servis parola sıfırlama özelliğini etkinleştireceksiniz
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.openlocfilehash: 21f2081f5aae0bb93cb9066407140f5fd35dc06d
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54424061"
---
# <a name="tutorial-complete-an-azure-ad-self-service-password-reset-pilot-roll-out"></a>Öğretici: Pilot dağıtım eksiksiz bir Azure AD Self Servis parola sıfırlama

Bu öğreticide kuruluşunuzda Azure AD self servis parola sıfırlama (SSPR) pilotu dağıtımını tamamlayacak ve yönetici olmayan bir hesabı kullanarak test edeceksiniz.

Self servis parola sıfırlama özelliğinin yönetici olmayan hesaplarla test edilmesi önemlidir. Yönetici hesaplarının parola sıfırlama ilkesi Microsoft tarafından yönetilmektedir ve bunun için daha güçlü kimlik doğrulama yöntemleri kullanılmaktadır. Bu ilke, güvenlik sorularının ve yanıtlarının kullanılmasına izin vermez ve sıfırlama için iki yöntemin kullanılmasını gerektirir.

> [!div class="checklist"]
> * Kendi kendine parola sıfırlamayı etkinleştirme
> * Self servis parola sıfırlamayı kullanıcı olarak test etme

## <a name="prerequisites"></a>Önkoşullar

* Genel Yönetici hesabı

## <a name="enable-self-service-password-reset"></a>Kendi kendine parola sıfırlamayı etkinleştirme

1. Genel Yönetici hesabını kullanarak [Azure portal](https://portal.azure.com) oturumu açın.
1. **Azure Active Directory**'ye göz atın ve **Parola sıfırlama**'yı seçin.
1. Bir pilot grupla başlayarak self servis parola sıfırlama özelliği kuruluşunuzdaki kullanıcıların yalnızca bir bölümü için etkinleştirin.
   * **Özellikler** sayfasında, **Self servis parola sıfırlama etkinleştirildi** seçeneğinin altında **Seçili**'yi belirleyin ve bir pilot grup seçin.
      * SSPR işlevini yalnızca sizin seçtiğiniz belirli bir Azure AD grubunun üyeleri kullanabilir. Bu işlevin dağıtımını yaparken kavram kanıtı için bir kullanıcı grubu tanımlamanız ve bu ayarı kullanmanız önerilir. Güvenlik gruplarının iç içe geçmesi burada desteklenmiyor.
      * Seçtiğiniz gruptaki kullanıcıların gerekli lisanslara sahip olduğundan emin olun.
   * **Kaydet**’e tıklayın
1. **Kimlik doğrulama yöntemleri** sayfasında
   * **Sıfırlama için gereken yöntemlerin sayısı** için **2** değerini ayarlayın
   * **Kullanıcıların yararlanabileceği yöntemler** bölümünde kuruluşunuzun izin vermek istediği yöntemleri seçin. Bu öğretici için **E-posta**, **Cep telefonu** ve **Ofis telefonu** seçeneklerini etkinleştirin.
   * **Kaydet**’e tıklayın
1. **Kayıt** sayfasında
   * **Kullanıcılardan oturum açarken kaydolmalarını iste** için **Evet**'i seçin.
   * **Kullanıcıların kimlik doğrulaması bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısı** ayarını **180** olarak belirleyin.
   * **Kaydet**’e tıklayın
1. **Bildirimler** sayfasında
   * **Parola sıfırlamayı kullanıcılara bildirme** seçeneğini **Evet** olarak ayarlayın.
   * **Diğer yöneticiler parolalarını sıfırladığında tüm yöneticilere bildirme** seçeneğini **Evet** olarak ayarlayın.
1. **Özelleştirme** sayfasında
   * Microsoft, **Yardım masası bağlantısını özelleştir** ayarını **Evet** olarak değiştirmenizi ve **Özel yardım masası e-postası veya URL'si** alanına kullanıcılarınızın kuruluş içinde ek yardım almak için kullanabileceği bir e-posta adresi veya web sayfası URL'si yazmanızı önerir.
   * Bu öğreticide **Yardım masası bağlantısını özelleştir** ayarını **Hayır** olarak bırakacağız.

Self servis parola sıfırlamayı pilot grubunuzdaki bulut kullanıcıları için yapılandırdık.

## <a name="test-sspr-as-a-user"></a>Self servis parola sıfırlamayı kullanıcı olarak test etme

Self servis parola sıfırlamayı pilot grubunuza üye olan ve yönetici olmayan bir kullanıcıyla deneyin. **Yönetici rolüne sahip bir kullanıcıyla test ederseniz yönetici ilkesi Microsoft tarafından yönetildiğinden kimlik doğrulama yöntemleri ve sayısı seçtiğinizden farklı olabilir.**

1. Yeni bir InPrivate veya gizli tarayıcı penceresi açın.
1. Test kullanıcısıyla [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup) adresindeki kayıt portalından self servis parola sıfırlama hizmetine kaydolun.
1. Aynı test kullanıcısıyla [https://aka.ms/sspr](https://aka.ms/sspr) adresindeki self servis parola sıfırlama portalına gidin ve önceki adımda girdiğiniz bilgileri kullanarak parolanızı sıfırlamayı deneyin.
1. Parolanızın başarıyla sıfırlanması gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide yapılandırdığınız işlevi kullanmak istemediğinize karar verirseniz aşağıdaki değişikliği yapın.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Azure Active Directory**'ye göz atın ve **Parola sıfırlama**'yı seçin.
1. **Özellikler** sayfasında, **Self servis parola sıfırlama etkinleştirildi** seçeneğinin altında **Yok**'u seçin.
1. **Kaydet**’e tıklayın

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure AD'de self servis parola sıfırlama özelliğini etkinleştirdiniz. Self servis parola sıfırlama deneyimini şirket içi Active Directory Domain Services altyapısıyla nasıl tümleştirebileceğinizi görmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [SSPR şirket içi geri yazma tümleştirmesini etkinleştirme](tutorial-enable-writeback.md)
