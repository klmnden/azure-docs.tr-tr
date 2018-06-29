---
title: Azure AD uygulama proxy'si ile Uzak Masaüstü yayımlama | Microsoft Docs
description: Azure AD uygulama proxy'si bağlayıcılar hakkında temel bilgiler yer almaktadır.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: barbkess
ms.custom: it-pro
ms.reviewer: harshja
ms.openlocfilehash: 0a004ee6e5dbdd2ceb8546a4b7ce20b2b551fac9
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37084074"
---
# <a name="publish-remote-desktop-with-azure-ad-application-proxy"></a>Azure AD uygulama proxy'si ile Uzak Masaüstü yayımlama

Uzak Masaüstü hizmetine ve Azure AD uygulama proxy'si olarak şirket ağının yöntemi bir kenara bırakarak çalışanların verimliliğini artırmak için birlikte çalışır. 

Bu makalede hedef kitlesi aşağıdaki gibidir:
- Uzak Masaüstü Hizmetleri üzerinden şirket içi uygulamaları yayımlama tarafından kendi son kullanıcıları için daha fazla uygulama sunmak istediğinize geçerli uygulama proxy'si müşteriler.
- Azure AD uygulama proxy'si kullanarak bunların dağıtım saldırı yüzeyini küçültmek istediğiniz geçerli Uzak Masaüstü Hizmetleri müşteriler. Bu senaryo iki aşamalı doğrulama ve koşullu erişim denetimleri sınırlı sayıda için RDS'yi sağlar.

## <a name="how-application-proxy-fits-in-the-standard-rds-deployment"></a>Uygulama proxy'si standart RDS dağıtımda nasıl uyduğunu

Standart bir RDS dağıtma Windows Server'da çalışan çeşitli Uzak Masaüstü rol hizmetlerini içerir. Bakarak [Uzak Masaüstü Hizmetleri mimarisi](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture), birden çok dağıtım seçenekleri vardır. Diğer RDS dağıtım seçenekleri aksine [RDS dağıtımı Azure AD uygulama proxy'si ile](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) (Aşağıdaki diyagramda gösterildiği) bağlayıcı hizmetini çalıştıran sunucudan kalıcı giden bir bağlantı vardır. Bir yük dengeleyici üzerinden açık gelen bağlantılar diğer dağıtımlar bırakın.

![Uygulama proxy'si RDS VM ve genel internet arasında bulunur](./media/application-proxy-integrate-with-remote-desktop-services/rds-with-app-proxy.png)

RDS dağıtımında, RD Web rolü ve RD Ağ Geçidi rol Internet'e yönelik makinelerde çalıştırın. Bu uç noktalar aşağıdaki nedenlerle ortaya çıkar:
- RD Web kullanıcı oturum açma ve çeşitli şirket içi uygulamalara ve masaüstlerine erişebilecekleri görüntülemek için genel bir uç nokta sağlar. Bir kaynak seçtikten sonra bir RDP bağlantısının işletim sisteminde yerel uygulama kullanılarak oluşturulur.
- Bir kullanıcı RDP bağlantı başlatır sonra RD Ağ Geçidi devreye. RD Ağ Geçidi internet üzerinden gelen şifrelenmiş RDP trafik işler ve kullanıcının bağlanmakta olduğu şirket içi sunucusuna çevirir. Bu senaryoda, RD Ağ Geçidi alma trafiği Azure AD uygulama proxy'si gelir.

>[!TIP]
>RDS önce dağıtılan henüz veya başlamadan önce daha fazla bilgi istiyorsanız öğrenin nasıl [sorunsuz bir şekilde Azure Resource Manager ve Azure Marketi RDS dağıtma](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure).

## <a name="requirements"></a>Gereksinimler

- Web istemcisi uygulama proxy'si desteklemediğinden Uzak Masaüstü web istemcisi dışındaki bir istemci kullanın.

- RD Web ve RD Ağ Geçidi uç noktalar aynı makine üzerindeki ve bir ortak kök ile yer alması gerekir. Böylece çoklu oturum açma deneyimini iki uygulama arasındaki sahip RD Web ve RD Ağ Geçidi uygulaması proxy'si ile tek bir uygulama olarak yayımlanır.

- Zaten yüklü olmalıdır [RDS dağıtılan](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure), ve [uygulama Proxy etkin](application-proxy-enable.md).

- Bu senaryo, son kullanıcılarınız RD Web sayfası aracılığıyla bağlanan Windows 7 veya Windows 10 Masaüstü üzerinde Internet Explorer aracılığıyla Git varsayar. Diğer işletim sistemlerinin desteklemeniz gerekiyorsa, bkz: [desteklemek için diğer istemci yapılandırmaları](#support-for-other-client-configurations).

- RD Web yayımlarken, aynı iç ve dış FQDN kullanmanız önerilir. Ardından dahili ve harici FQDN'ler farklıysa çeviri geçersiz bağlantıları alma istemcinin önlemek için istek üstbilgisi devre dışı bırakmalısınız. 

- Internet Explorer, RDS ActiveX eklenti etkinleştirin.

## <a name="deploy-the-joint-rds-and-application-proxy-scenario"></a>Birleşik RDS ve uygulama proxy'si senaryo dağıtma

RDS ve ortamınız için Azure AD uygulama proxy'si kurduktan sonra iki çözümleri birleştirmek için adımları izleyin. Bu adımları iki web dönük RDS uç noktaları (RD Web ve RD Ağ Geçidi) uygulamaları yayımlama ve uygulama proxy'si aracılığıyla gitmek için RDS üzerinde trafiği yönlendirerek yol.

### <a name="publish-the-rd-host-endpoint"></a>RD konak endpoint yayımlama

1. [Yeni bir uygulama proxy'si uygulama yayımlama](application-proxy-publish-azure-portal.md) aşağıdaki değerlere sahip:
   - İç URL: https://\<rdhost\>.com / nerede \<rdhost\> RD Web ve RD Ağ geçidi paylaşımı ortak kökü.
   - Dış URL: Bu alan otomatik olarak uygulama adına göre doldurulur, ancak bunu değiştirebilirsiniz. RDS'yi eriştiklerinde, kullanıcılarınızın bu URL'ye Git
   - Ön kimlik doğrulama yöntemi: Azure Active Directory
   - URL üstbilgileri çevir: Hayır
2. Kullanıcıların yayımlanan RD uygulamaya atayın. Tüm RDS, çok erişimi olduğundan emin olun.
3. Çoklu oturum açma yöntemi uygulama için olarak bırakın **Azure AD çoklu oturum açma devre dışı özelliğini**. Kullanıcılarınızın Azure ad, bir kez de RD Web kimlik doğrulaması, ancak çoklu oturum açma RD ağ geçidine sahip istenir.
4. Git **Azure Active Directory** > **uygulama kayıtlar** > *uygulamanız* > **ayarları**.
5. Seçin **özellikleri** ve güncelleştirme **giriş sayfası URL'si** , RD Web uç noktası için alan (https:// gibi\<rdhost\>.com/RDWeb).

### <a name="direct-rds-traffic-to-application-proxy"></a>Uygulama proxy'si doğrudan RDS trafiği

RDS dağıtımı yönetici olarak bağlanın ve dağıtım için RD Ağ Geçidi sunucusu adını değiştirin. Bu yapılandırma, bağlantılar Azure AD uygulama proxy'si hizmeti aracılığıyla Git sağlar.

1. RD Bağlantı Aracısı rol çalıştıran RDS sunucuya bağlanın.
2. Başlatma **Sunucu Yöneticisi'ni**.
3. Seçin **Uzak Masaüstü Hizmetleri** sol bölmesinden.
4. **Genel Bakış**’ı seçin.
5. Dağıtımına genel bakış bölümünde, aşağı açılan menüden seçip **dağıtım özelliklerini düzenleme**.
6. RD Ağ Geçidi sekmesinde, değiştirmek **sunucu adı** uygulama proxy'si RD konak uç için ayarladığınız dış URL alanı.
7. Değişiklik **oturum açma yöntemi** alanı **parola kimlik doğrulaması**.

  ![Dağıtım özellikleri RDS ekranda](./media/application-proxy-integrate-with-remote-desktop-services/rds-deployment-properties.png)

8. Her koleksiyon için bu komutu çalıştırın. Değiştir *\<yourcollectionname\>* ve *\<proxyfrontendurl\>* kendi bilgilerle. Bu komut, çoklu oturum açma RD Web ve RD Ağ Geçidi arasında sağlar ve performansı en iyi duruma getirir:

   ```
   Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s:<proxyfrontendurl>`nrequire pre-authentication:i:1"
   ```

   **Örneğin:**
   ```
   Set-RDSessionCollectionConfiguration -CollectionName "QuickSessionCollection" -CustomRdpProperty "pre-authentication server address:s:https://remotedesktoptest-aadapdemo.msappproxy.net/`nrequire pre-authentication:i:1"
   ```

9. Bu koleksiyon için RDWeb indirilir RDP dosya içeriğini görüntülemek yanı sıra özel RDP özellikler değiştirilmesini doğrulamak için aşağıdaki komutu çalıştırın:
    ```
    (get-wmiobject -Namespace root\cimv2\terminalservices -Class Win32_RDCentralPublishedRemoteDesktop).RDPFileContents
    ```

Uzak Masaüstü yapılandırdıysanız, Azure AD uygulama proxy'si RDS'yi Internet'e bileşeni olarak gerçekleştirdiği RD Web ve RD Ağ Geçidi makinelerde diğer ortak internet'e yönelik uç noktalar kaldırabilirsiniz.

## <a name="test-the-scenario"></a>Test senaryosu

Windows 7 veya 10 bilgisayarda Internet Explorer ile senaryoyu test edin.

1. Ayarladığınız dış URL gidin veya uygulamanızda Bul [MyApps Masası](https://myapps.microsoft.com).
2. Azure Active Directory kimlik doğrulaması yapmak istenir. Uygulamaya atanmış bir hesap kullanın.
3. RD Web kimlik doğrulaması yapmak istenir.
4. RDS kimlik doğrulaması başarılı olduktan sonra Masaüstü veya istediğiniz uygulamayı seçin ve çalışma başlatın.

## <a name="support-for-other-client-configurations"></a>Diğer istemci yapılandırmaları için destek

Bu makalede açıklanan Windows 7 veya 10, Internet Explorer ve RDS ActiveX eklenti sahip kullanıcılar için bir yapılandırmadır. Ancak, gerekirse, diğer işletim sistemleri veya tarayıcılar destekleyebilir. Kullandığınız kimlik doğrulama yöntemi farktır.

| Kimlik doğrulama Yöntemi | Desteklenen istemci yapılandırması |
| --------------------- | ------------------------------ |
| Kimlik doğrulaması öncesi    | Windows 7/10 kullanarak Internet Explorer + RDS ActiveX eklentisi |
| Geçiş | Tüm diğer Microsoft Uzak Masaüstü uygulaması destekleyen işletim sistemi |

Ön kimlik doğrulaması akışı geçiş akış'den daha fazla güvenlik avantajları sunar. Ön kimlik doğrulaması ile şirket içi kaynaklarınız için çoklu oturum açma, koşullu erişim ve iki aşamalı doğrulama gibi Azure AD kimlik doğrulama özellikleri kullanabilirsiniz. Ayrıca, yalnızca trafik ulaştığında, ağ kimliği doğrulanmış emin olun.

Geçiş kimlik doğrulaması kullanmak için bu makalede listelenen adımları yalnızca iki değişiklikler vardır:
1. İçinde [RD konak endpoint yayımlama](#publish-the-rd-host-endpoint) 1. adımda, ön kimlik doğrulama yöntemi kümesine **geçiş**.
2. İçinde [uygulama proxy'si doğrudan RDS trafiği](#direct-rds-traffic-to-application-proxy), tamamen 8. adıma geçin.

## <a name="next-steps"></a>Sonraki adımlar

[SharePoint Azure AD uygulama proxy'si ile uzaktan erişimi etkinleştir](application-proxy-integrate-with-sharepoint-server.md)  
[Azure AD uygulama proxy'si kullanarak uygulamalar uzaktan erişim için güvenlik konuları](application-proxy-security.md)
