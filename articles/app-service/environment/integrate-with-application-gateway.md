---
title: Azure Application Gateway ile - ILB App Service ortamı tümleştirin
description: Bir uygulamada bir ILB App Service ortamınızı bir Application Gateway ile tümleştirme yönergeleri
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a6a74f17-bb57-40dd-8113-a20b50ba3050
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/03/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: ea46b5e57e4e508a3311de8633ae61d346b574eb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60764923"
---
# <a name="integrate-your-ilb-app-service-environment-with-the-azure-application-gateway"></a>ILB App Service ortamınızı Azure Application Gateway ile tümleştirme #

[App Service ortamı](./intro.md) bir Azure App Service'in bir müşterinin Azure sanal ağ alt dağıtımıdır. Uygulama erişimi için bir genel veya özel uç noktası ile dağıtılabilir. Özel uç noktası (diğer bir deyişle, bir iç yük dengeleyici) ile App Service ortamı dağıtımı ILB App Service ortamı denir.  

Web uygulamasının güvenliğini sağlamaya yardımcı olur, SQL eklemelerini, siteler arası betik, kötü amaçlı yazılım yüklemelerini engellemek için gelen web trafiğini inceleyerek, web uygulamaları ve uygulama DDoS ve diğer saldırıları güvenlik duvarları. Ayrıca veri kaybı önleme (DLP) için arka uç web sunucularından gelen yanıtları denetler. Azure Market'ten bir WAF cihazı yönetime alabilirsiniz veya kullanabileceğiniz [Azure Application Gateway][appgw].

Azure Application Gateway, 7. Katman Yük Dengeleme, SSL yük boşaltma ve web uygulaması Güvenlik Duvarı (WAF) koruması sağlayan bir sanal gereçtir. Genel IP adresi ve rota trafik uygulama uç noktanıza üzerinde dinleyebilirsiniz. Aşağıdaki bilgiler, WAF yapılandırılmış bir uygulama ağ geçidini bir ILB App Service Ortamı'nda bir uygulama ile tümleştirmek açıklar.  

Uygulama ağ geçidini bir ILB App Service ortamı ile bir uygulama düzeyinde tümleştirmedir. ILB App Service ortamı ile uygulama ağ geçidi yapılandırdığınızda, ILB App Service Ortamı'nda belirli uygulamalar için yaptığınız. Bu teknik, tek bir ILB App Service ortamı güvenli çok kiracılı uygulamalarda barındırma sağlar.  

![Uygulama ağ geçidini bir ILB App Service ortamında uygulama işaret][1]

Bu bölümde şunları yapacaksınız:

* Azure uygulama ağ geçidi oluşturun.
* Uygulama ağ geçidini bir ILB App Service ortamınızı uygulamada işaret edecek şekilde yapılandırın.
* Özel etki alanı adını kabul etmenin uygulamanızı yapılandırın.
* Application gateway'iniz işaret eden genel DNS ana bilgisayar adını düzenleyin.

## <a name="prerequisites"></a>Önkoşullar

Application Gateway'iniz ILB App Service ortamı ile tümleştirmek için ihtiyacınız vardır:

* Bir ILB App Service ortamı.
* ILB App Service ortamında çalışan bir uygulama.
* ILB App Service ortamında uygulama ile kullanılmak üzere bir Internet yönlendirilebilir bir etki alanı adı.
* ILB App Service ortamınızı kullanan ILB adresini. Bu App Service ortamı portalında altında bilgilerdir **ayarları** > **IP adresleri**:

    ![ILB App Service ortamı tarafından kullanılan IP adresleri listesi örneği][9]
    
* Daha sonra Application Gateway'iniz işaret etmek için kullanılan genel bir DNS adı. 

Bir ILB App Service ortamı oluşturma hakkında daha fazla ayrıntı için bkz. [oluşturma ve bir ILB App Service ortamı kullanarak][ilbase].

Bu makalede, istediğiniz bir Application Gateway, aynı Azure sanal ağında App Service ortamı dağıtıldığı varsayılır. Uygulama ağ geçidi oluşturmaya başlamadan önce seçin veya ağ geçidi barındırmak için kullanacağınız bir alt ağ oluşturun. 

Olmayan bir adlandırılmış GatewaySubnet bir alt ağda kullanmanız gerekir. Uygulama ağ geçidi GatewaySubnet koyarsanız, daha sonra bir sanal ağ geçidi oluşturmak oluşturamazsınız. 

Ayrıca, ILB App Service ortamınızı kullanır alt ağdaki ağ geçidini konulamaz. App Service ortamı, bu alt ağda yalnızca bir şeydir.

## <a name="configuration-steps"></a>Yapılandırma adımları ##

1. Azure portalında Git **yeni** > **ağ** > **Application Gateway**.

2. İçinde **Temelleri** alan:

   a. İçin **adı**, uygulama ağ geçidinin adını girin.

   b. İçin **katmanı**seçin **WAF**.

   c. İçin **abonelik**, App Service ortamı sanal ağın kullandığı aynı aboneliği seçin.

   d. İçin **kaynak grubu**oluşturun veya kaynak grubunu seçin.

   e. İçin **konumu**, App Service ortamı sanal ağ konumunu seçin.

   ![Yeni uygulama ağ geçidi oluşturma temelleri][2]

3. İçinde **ayarları** alan:

   a. İçin **sanal ağ**, App Service ortamı sanal ağı seçin.

   b. İçin **alt**, uygulama ağ geçidi gereken yere dağıtılması için alt ağı seçin. VPN ağ geçitleri oluşturma olarak engelleyebileceğinden GatewaySubnet, kullanmayın.

   c. İçin **IP adresi türü**seçin **genel**.

   d. İçin **genel IP adresi**, genel bir IP adresi seçin. Yoksa, şimdi oluşturun.

   e. İçin **Protokolü**seçin **HTTP** veya **HTTPS**. HTTPS için yapılandırıyorsanız, bir PFX sertifikası vermeniz gerekir.

   f. İçin **Web uygulaması güvenlik duvarı**, güvenlik duvarını etkinleştirmek ve ayrıca için ayarlanmış **algılama** veya **önleme** gördüğünüz şekilde.

   ![Yeni uygulama ağ geçidi oluşturma ayarları][3]
    
4. İçinde **özeti** bölümünde ayarları gözden geçirin ve seçin **Tamam**. Application Gateway'iniz Kurulumu tamamlamak için biraz fazla 30 dakika sürebilir.  

5. Application Gateway'iniz Kurulum tamamlandıktan sonra uygulama ağ geçidi portalınıza gidin. Seçin **arka uç havuzu**. ILB App Service ortamı için ILB adresini ekleyin.

   ![Arka uç havuzunu yapılandırma][4]

6. Arka uç havuzu yapılandırma işlemi tamamlandıktan sonra seçin **sistem durumu araştırmalarının**. Uygulamanız için kullanmak istediğiniz etki alanı adı için bir durum araştırması oluşturun. 

   ![Sistem durumu araştırmalarını yapılandırma][5]
    
7. Sistem durumu araştırmalarını yapılandırma işlemi tamamlandıktan sonra seçin **HTTP ayarları**. Var olan ayarları düzenleme, seçin **kullanım özel araştırma**ve yapılandırdığınız araştırma seçin.

   ![HTTP ayarları yapılandırma][6]
    
8. Uygulama ağ geçidinin Git **genel bakış** bölümünde ve kullanan uygulama ağ geçidinizin genel IP adresini kopyalayın. Uygulama etki alanı adınız için bir A kaydı bu IP adresi yap veya bu adres için DNS adı bir CNAME kaydı kullanın. Genel IP adresini seçin ve genel IP adresinin kullanıcı Arabiriminden kopyalamak yerine, uygulama ağ geçidinin bağlantıyı kopyalamak daha kolay **genel bakış** bölümü. 

   ![Uygulama ağ geçidi portalı][7]

9. Uygulamanız için özel etki alanı adı, ILB App Service ortamınızı ayarlayın. Uygulamanızı portalı ve altında Git **ayarları**seçin **özel etki alanları**.

   ![Uygulama özel etki alanı adı ayarlayın][8]

Web uygulamalarınız için özel etki alanı adlarını makalesinde ayarlama hakkında bilgi [web uygulamanız için özel etki alanı adlarını belirleme][custom-domain]. Bir ILB App Service Ortamı'nda bir uygulama için mevcut değildir ancak tüm etki alanı adını doğrulama. Uygulama uç noktalarını yöneten DNS sahip olduğundan, burada istediğiniz koyabilirsiniz. Bu durumda eklediğiniz özel etki alanı adı DNS sunucunuzun olması gerekmez, ancak yine de uygulamanızı ile yapılandırılmış olması gerekmez. 

Kurulum tamamlandıktan sonra bir kısa sürede, DNS değişikliklerinin yayılması için izin verilen, uygulamanızı oluşturduğunuz özel etki alanı adını kullanarak erişebilirsiniz. 


<!--IMAGES-->
[1]: ./media/integrate-with-application-gateway/appgw-highlevel.png
[2]: ./media/integrate-with-application-gateway/appgw-createbasics.png
[3]: ./media/integrate-with-application-gateway/appgw-createsettings.png
[4]: ./media/integrate-with-application-gateway/appgw-backendpool.png
[5]: ./media/integrate-with-application-gateway/appgw-healthprobe.png
[6]: ./media/integrate-with-application-gateway/appgw-httpsettings.png
[7]: ./media/integrate-with-application-gateway/appgw-publicip.png
[8]: ./media/integrate-with-application-gateway/appgw-customdomainname.png
[9]: ./media/integrate-with-application-gateway/appgw-iplist.png

<!--LINKS-->
[appgw]: https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[custom-domain]: ../app-service-web-tutorial-custom-domain.md
[ilbase]: ./create-ilb-ase.md
