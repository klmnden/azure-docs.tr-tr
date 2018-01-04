---
title: "Azure Active Directory koşullu erişim için VPN bağlantısı (Önizleme) | Microsoft Docs"
description: "VPN bağlantısı için Azure Active Directory koşullu erişim nasıl çalıştığını öğrenin. "
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/01/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 8941e631976eb11966c1f9ddd207af816df5dadf
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-conditional-access-for-vpn-connectivity-preview"></a>VPN bağlantısı (Önizleme) için Azure Active Directory koşullu erişim

İle [Azure Active Directory (Azure AD) koşullu erişim](active-directory-conditional-access-azure-portal.md), ince ayar yapabilirsiniz yetkili kullanıcıların, kaynaklara erişimini nasıl. Azure AD ile koşullu erişim sanal özel ağ (VPN) bağlantısı için VPN bağlantılarını korumaya yardımcı olabilir.


VPN bağlantısı için koşullu erişim yapılandırmak için aşağıdaki adımları tamamlamanız gerekir: 

1.  VPN sunucunuz yapılandırın.
2.  VPN istemcinizi yapılandırmak.
3.  Koşullu erişim ilkesini yapılandırın.


## <a name="before-you-begin"></a>Başlamadan önce

Bu konuda aşağıdaki konularda bilgi sahibi olduğunuz varsayılır:

- [Azure Active Directory'de koşullu erişim](active-directory-conditional-access-azure-portal.md)
- [VPN ve koşullu erişim](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Microsoft bu özelliği nasıl uyguladığını üzerinde Öngörüler elde etmek için bkz: [uzaktan erişim'deki Windows 10 otomatik bir VPN profiliyle geliştirme](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile).   


## <a name="prerequisites"></a>Ön koşullar

VPN bağlantısı için Azure Active Directory koşullu erişim yapılandırmak için yapılandırılmış bir VPN sunucusu olması gerekir. 



## <a name="step-1-configure-your-vpn-server"></a>1. adım: VPN sunucunuzu yapılandırın 

Bu adım, Azure AD ile VPN kimlik doğrulaması için kök sertifikalarını yapılandırır. VPN bağlantısı için koşullu erişim yapılandırmak için aktarmanız gerekir:

1. Azure portalında bir VPN sertifika oluşturun.
2. VPN sertifikayı indirin.
2. Sertifika, VPN sunucusuna dağıtır.

Azure AD, Windows 10 istemcileri için VPN bağlantısı için Azure ad Kimlik doğrulanırken verilen sertifikaları imzalamak için VPN sertifikası kullanır. Windows 10 istemci isteklerini belirteci sonra VPN sunucusu bu durumda olan uygulamaya sunan bir sertifikadır.

![Koşullu erişim için sertifikayı indirin](./media/active-directory-conditional-access-vpn-connectivity-windows10/06.png)

Azure portalında bir sertifikanın dolmak üzere olduğunda geçiş yönetmek için iki sertifika oluşturabilirsiniz. Bir sertifika oluşturduğunuzda, kimlik doğrulaması sırasında bağlantı için sertifika imzalamak için kullanılan birincil sertifikası olup olmadığını seçebilirsiniz.

Bir VPN sertifikası oluşturmak için:

1. Oturum açın, [Azure portal](https://portal.azure.com) genel yönetici olarak.

2. Sol menüsünde **Azure Active Directory**. 

    ![Azure Active Directory'yi seçin](./media/active-directory-conditional-access-vpn-connectivity-windows10/01.png)

3. Üzerinde **Azure Active Directory** sayfasında **Yönet** 'yi tıklatın **koşullu erişim**.

    ![Koşullu erişim seçin](./media/active-directory-conditional-access-azure-portal-get-started/02.png)

4. Üzerinde **koşullu erişim** sayfasında **Yönet** 'yi tıklatın **VPN bağlantısı (Önizleme)**.

    ![VPN bağlantısı seçin](./media/active-directory-conditional-access-vpn-connectivity-windows10/03.png)

5. Üzerinde **VPN bağlantısı** sayfasında, **yeni sertifika**.

    ![Yeni bir sertifika seçin](./media/active-directory-conditional-access-vpn-connectivity-windows10/04.png)

6. Üzerinde **yeni** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Süre ve birincil seçin](./media/active-directory-conditional-access-vpn-connectivity-windows10/05.png)

    a. İçin **seçin süresi**seçin **1 yıl**.

    b. İçin **birincil**seçin **Evet**.

    c. **Oluştur**'a tıklayın.

7. VPN bağlantısı sayfasında, tıklatın **indirme sertifika**.


Artık, yeni oluşturulan sertifika, VPN sunucusuna dağıtmak hazırsınız. VPN sunucunuz üzerinde olarak indirilen sertifika Ekle bir *VPN kimlik doğrulaması için güvenilen kök CA*.

Windows RRAS tabanlı dağıtımlar, NPS sunucunuzda içine kök sertifika eklemek için *Kurumsal NTauth* depolamak aşağıdaki komutları çalıştırarak:

1. `certutil -dspublish <CACERT> RootCA`
2. `certutil -dspublish <CACERT> NtAuthCA`



## <a name="step-2-configure-your-vpn-client"></a>2. adım: VPN istemcinizi yapılandırma 

Bu adımda, VPN istemci bağlantısı profilinizi kısmında özetlendiği gibi yapılandırmanız [VPN ve koşullu erişim](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).


## <a name="step-3-configure-your-conditional-access-policy"></a>3. adım: koşullu erişim ilkesini yapılandırma

Bu bölümde VPN bağlantısı için koşullu erişim ilkenizi yapılandırmaya yönelik yönergeler sağlar.


1. Üzerinde **koşullu erişim** üstteki araç çubuğunda sayfasını tıklatın **Ekle**.

    ![Koşullu erişim sayfasında seçin ekleyin](./media/active-directory-conditional-access-vpn-connectivity-windows10/07.png)

2. Üzerinde **yeni** sayfasında **adı** ilkeniz için bir ad yazın. Örneğin, **VPN ilkesi**.

    ![Koşullu erişim sayfasında ilke için ad ekleyin](./media/active-directory-conditional-access-vpn-connectivity-windows10/08.png)

5. İçinde **atama** 'yi tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı ve grupları seçin](./media/active-directory-conditional-access-vpn-connectivity-windows10/09.png)

6. Üzerinde **kullanıcılar ve gruplar** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Select test kullanıcısı](./media/active-directory-conditional-access-vpn-connectivity-windows10/10.png)

    a. Tıklatın **kullanıcıları ve grupları seçin**.

    b. **Seç**'e tıklayın.

    c. Üzerinde **seçin** sayfasında, test kullanıcınız seçin ve ardından **seçin**.

    d. Üzerinde **kullanıcılar ve gruplar** sayfasında, **Bitti**.

7. Üzerinde **yeni** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bulut uygulamaları seçin](./media/active-directory-conditional-access-vpn-connectivity-windows10/11.png)

    a. İçinde **atamaları** 'yi tıklatın **bulut uygulamaları**.

    b. Üzerinde **bulut uygulamaları** sayfasında, **uygulamaları**ve ardından **seçin**.

    c. Üzerinde **seçin** sayfasında **uygulamaları** kutusuna **vpn**.

    d. Seçin **VPN sunucusu**.

    e. **Seç**'e tıklayın.


13. Üzerinde **yeni** sayfasını açmak için **Grant** sayfasında **denetimleri** 'yi tıklatın **Grant**.

    ![Select izni](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. Üzerinde **Grant** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çok faktörlü kimlik doğrulaması seçin gerektirir](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    a. Seçin **çok faktörlü kimlik doğrulaması gerektiren**.

    b. **Seç**'e tıklayın.

15. Üzerinde **yeni** sayfasında **ilkesini etkinleştir**, tıklatın **üzerinde**.

    ![İlkeyi etkinleştir](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. Üzerinde **yeni** sayfasında, **oluşturma**.



## <a name="next-steps"></a>Sonraki adımlar

Microsoft bu özelliği nasıl uyguladığını içine Öngörüler elde etmek için bkz: [uzaktan erişim'deki Windows 10 otomatik bir VPN profiliyle geliştirme](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile).    

