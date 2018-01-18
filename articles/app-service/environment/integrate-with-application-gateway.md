---
title: "ILB uygulama hizmeti ortamınızı bir uygulama ağ geçidi ile tümleştirme"
description: "İzlenecek yol ILB uygulama hizmeti ortamınızı bir uygulamada bir uygulama ağ geçidi ile tümleştirme hakkında"
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
ms.date: 10/17/2017
ms.author: ccompy
ms.openlocfilehash: d56eab79c3b3f6b37dc39d8e4bea0d5b7759631a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="integrate-your-ilb-app-service-environment-with-an-application-gateway"></a>ILB uygulama hizmeti ortamınızı bir uygulama ağ geçidi ile tümleştirme #

[PowerApps için uygulama hizmeti ortamı](./intro.md) bir Azure uygulama hizmeti bir müşterinin Azure sanal ağ alt dağıtımıdır. Uygulama erişimi için bir genel veya özel uç noktası ile dağıtılabilir. Özel uç noktası (diğer bir deyişle, bir iç yük dengeleyici) ile uygulama hizmeti ortamı dağıtımı ILB uygulama hizmeti ortamı adı verilir.  

Azure uygulama ağ geçidi katman 7 Yük Dengeleme ve SSL boşaltma web uygulaması Güvenlik Duvarı (WAF) koruması sağlar ve bir sanal gereç olur. Bir ortak IP adresi ve rota trafiğin uygulama uç noktası üzerinde dinleme. 

Aşağıdaki bilgiler, ILB uygulama hizmeti ortamı'nda bir uygulama WAF yapılandırılmış bir uygulama ağ geçidi tümleştirileceğini açıklar.  

Uygulama ağ geçidi ILB uygulama hizmeti ortamı ile tümleştirilmesi, bir uygulama düzeyinde bulunur. ILB uygulama hizmeti ortamınızı ile uygulama ağ geçidi yapılandırdığınızda, ILB uygulama hizmeti ortamı'nda belirli uygulamalar için yaptığınız. Bu yöntem, tek bir ILB uygulama hizmeti ortamı güvenli çok kiracılı uygulamalarda barındırma sağlar.  

![Uygulama ağ geçidini bir ILB uygulama hizmeti ortamı uygulamasını işaret eden][1]

Bu bölümde şunları yapacaksınız:

* Bir uygulama ağ geçidi oluşturun.
* Uygulama ağ geçidini bir ILB uygulama hizmeti ortamınızı uygulamada işaret edecek şekilde yapılandırın.
* Özel etki alanı adı vermenizin uygulamanızı yapılandırın.
* Uygulama ağ geçidiniz için işaret Genel DNS ana bilgisayar adını düzenleyin.

## <a name="prerequisites"></a>Önkoşullar

Uygulama ağ geçidiniz ILB uygulama hizmeti ortamınızı ile tümleştirmek için gerekir:

* ILB uygulama hizmeti ortamı.
* ILB uygulama hizmeti ortamı'nda çalışan bir uygulama.
* ILB uygulama hizmeti ortamı uygulamanızda ile kullanılmak üzere bir Internet yönlendirilebilir etki alanı adı.
* ILB uygulama hizmeti ortamınızı kullanan ILB adresi. Bu uygulama hizmeti ortamı portalında altında bilgidir **ayarları** > **IP adreslerini**:

    ![ILB uygulama hizmeti ortamı tarafından kullanılan IP adresleri örnek listesi][9]
    
* Daha sonra uygulama ağ geçidiniz için işaret etmek için kullanılan ortak bir DNS adı. 

Bir ILB uygulama hizmeti ortamı oluşturma hakkında daha fazla bilgi için bkz [oluşturma ve bir ILB uygulama hizmeti ortamı kullanarak][ilbase].

Bu makalede, bir uygulama ağ geçidi aynı Azure sanal ağında uygulama hizmeti ortamı dağıtıldığı istediğinizi varsayar. Uygulama ağ geçidi oluşturmaya başlamadan önce seçin veya ağ geçidi ana bilgisayar için kullanacağı bir alt ağ oluşturun. 

Olmayan bir adlandırılmış GatewaySubnet bir alt ağda kullanmanız gerekir. GatewaySubnet uygulama ağ geçidi yerleştirirseniz, daha sonra bir sanal ağ geçidi oluşturamıyor olması. 

Ayrıca, ILB uygulama hizmeti ortamınızı kullanır alt ağdaki ağ geçidi konumuna geçiremezsiniz. Uygulama hizmeti ortamı, bu alt ağda yalnızca bir şeydir.

## <a name="configuration-steps"></a>Yapılandırma adımları ##

1. Azure portalında Git **yeni** > **ağ** > **uygulama ağ geçidi**.

2. İçinde **Temelleri** alanı:

   a. İçin **adı**, uygulama ağ geçidi adını girin.

   b. İçin **katmanı**seçin **WAF**.

   c. İçin **abonelik**, uygulama hizmeti ortamı sanal ağ kullanan aynı aboneliği seçin.

   d. İçin **kaynak grubu**kaynak grubu seçin veya oluşturun.

   e. İçin **konumu**, uygulama hizmeti ortamı sanal ağ konumunu seçin.

   ![Yeni uygulama ağ geçidi oluşturma temelleri][2]

3. İçinde **ayarları** alanı:

   a. İçin **sanal ağ**, uygulama hizmeti ortamı sanal ağı seçin.

   b. İçin **alt**, uygulama ağ geçidi burada gereken dağıtılması için alt ağ seçin. GatewaySubnet, kullanmayın, çünkü VPN ağ geçitleri oluşturulmasını engeller.

   c. İçin **IP adresi türü**seçin **ortak**.

   d. İçin **genel IP adresi**, bir ortak IP adresi seçin. Yoksa, şimdi oluşturun.

   e. İçin **Protokolü**seçin **HTTP** veya **HTTPS**. HTTPS için yapılandırıyorsanız, bir PFX sertifikası sağlamanız gerekir.

   f. İçin **Web uygulaması güvenlik duvarı**, Güvenlik Duvarı'nı etkinleştirin ve ayrıca için ayarlamak **algılama** veya **önleme** uygun gördüğünüz şekilde.

   ![Yeni uygulama ağ geçidi oluşturma ayarları][3]
    
4. İçinde **Özet** bölümünde, ayarları gözden geçirin ve seçin **Tamam**. Uygulama ağ geçidi Kurulumu tamamlamak için 30 dakikadan biraz uzun sürebilir.  

5. Uygulama ağ geçidi Kurulum tamamlandıktan sonra uygulama ağ geçidi portalınıza gidin. Seçin **arka uç havuzu**. ILB adres ILB uygulama hizmeti ortamınız için ekleyin.

   ![Arka uç havuzunu yapılandırma][4]

6. Arka uç havuzunu yapılandırma işlemi tamamlandıktan sonra Seç **sistem durumu araştırmalarının**. Uygulamanız için kullanmak istediğiniz etki alanı adı için bir sistem durumu araştırma oluşturun. 

   ![Sistem durumu araştırmalarını yapılandırma][5]
    
7. Sistem durumu araştırmalarının yapılandırma işlemi tamamlandıktan sonra Seç **HTTP ayarları**. Var olan ayarları düzenleme, seçin **kullanım özel araştırma**ve yapılandırdığınız araştırma seçin.

   ![HTTP ayarlarını yapılandırma][6]
    
8. Uygulama ağ geçidi gidin **genel bakış** bölümünde ve uygulama ağ geçidini kullanan ortak IP adresi kopyalayın. Uygulama etki alanı adınız için bir A kaydı olarak, bu IP adresi ayarlayın veya bir CNAME kaydı bu adres için DNS adını kullanın. Genel IP adresini seçin ve genel IP adresi ait kullanıcı Arabiriminden kopyalamak yerine uygulama ağ geçidi bağlantıyı kopyalamak daha kolay **genel bakış** bölümü. 

   ![Uygulama ağ geçidi portalı][7]

9. Uygulamanız için özel etki alanı adı ILB uygulama hizmeti ortamınızı ayarlayın. Uygulamanızın portal ve altında gidin **ayarları**seçin **özel etki alanları**.

   ![Özel etki alanı adı uygulamasını ayarlama][8]

Makalede, web uygulamaları için özel etki alanı adlarını ayarlama hakkında bilgi [web uygulamanız için özel etki alanı adlarını ayarlama][custom-domain]. ILB uygulama hizmeti ortamı'nda bir uygulama için hiç ancak tüm etki alanı adını doğrulama. Uygulama uç noktaları yönetir DNS sahibi için var istediğiniz koyabilirsiniz. Bu durumda eklediğiniz özel etki alanı adı DNS sunucunuzun olması gerekmez, ancak hala uygulamanızın üzerinde yapılandırılması gerekmez. 

Kurulum tamamlandıktan sonra bir kısa sürede DNS değişikliklerinizin yayılması izin verilen, oluşturduğunuz özel etki alanı adını kullanarak uygulamanızı erişebilir. 


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
[appgw]: http://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[custom-domain]: ../app-service-web-tutorial-custom-domain.md
[ilbase]: ./create-ilb-ase.md
