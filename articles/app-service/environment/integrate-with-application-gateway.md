---
title: "ILB ana Azure uygulama ağ geçidi ile tümleştirme"
description: "İzlenecek yol, ILB ana uygulamada Azure uygulama ağ geçidi ile tümleştirme hakkında"
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
ms.openlocfilehash: eedad8824add7fe425d34975dab640fbee82c2bc
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2017
---
# <a name="integrating-your-ilb-ase-with-an-application-gateway"></a>ILB ana bir uygulama ağ geçidi ile tümleştirme #

[Azure uygulama hizmeti ortamı (ana)](./intro.md) bir müşterinin Azure sanal ağ alt Azure App Service'te dağıtımıdır. Uygulama erişimi için bir genel veya özel uç noktası ile dağıtılabilir. Özel bir uç noktası ile ana dağıtımı ILB ana adı verilir.  
Azure uygulama ağ geçidi katman 7 Yük Dengeleme ve SSL boşaltma WAF koruma sağlar ve bir sanal gereç ' dir. Bir ortak IP adresi ve rota trafiğin uygulama uç noktası üzerinde dinleme. Aşağıdaki bilgileri nasıl tümleştireceğinizi açıklayan bir WAF uygulama ağ geçidini bir ILB ana üzerinde bir uygulama yapılandırılır.  

Uygulama ağ geçidi ILB ana ile tümleştirilmesi, bir uygulama düzeyinde bulunur.  Uygulama ağ geçidi, ILB ana ile yapılandırdığınızda, ILB ana belirli uygulamalar için yaptıklarını. Bu, tek bir ILB ana güvenli çok kiracılı uygulamalara barındırma sağlar.  

![Uygulama ağ geçidini bir ILB ana uygulamasını işaret eden][1]

Bu bölümde şunları yapacaksınız:

* Bir uygulama ağ geçidi oluşturma
* Uygulama ağ geçidi, ILB ana üzerinde bir uygulama için işaret edecek şekilde yapılandırma
* özel etki alanı adı vermenizin uygulamanızı yapılandırma
* Uygulama ağ geçidiniz için işaret eden, ortak DNS ana bilgisayar adını düzenleyin

Uygulama ağ geçidi, ILB ana ile tümleştirmek için gerekir:

* bir ILB ana
* ILB ana çalışan bir uygulama
* ILB ana uygulamanızda ile kullanılmak üzere bir Internet yönlendirilebilir etki alanı adı
* ILB ana tarafından kullanılan ILB adresi (Bu ana portalında altında olan **ayarlar -> IP adreslerini**)

    ![ILB ana tarafından kullanılan IP adresleri][9]
    
* Daha sonra uygulama ağ geçidiniz için işaret etmek için kullanılan ortak bir DNS adı 

Bir ILB ana oluşturma hakkında ayrıntılar belgeyi okumak için [oluşturma ve bir ILB ana kullanma][ilbase]

Bu kılavuz, aynı Azure sanal ağındaki ana içine dağıtılan bir uygulama ağ geçidi istediğinizi varsayar. Uygulama ağ geçidi oluşturma başlamadan seçin veya uygulama ağ geçidi ana bilgisayar için kullanacağı bir alt ağ oluşturun. Olmayan bir adlandırılmış gatewaysubnet bir alt ağ veya ILB ana tarafından kullanılan alt ağ kullanmanız gerekir.
Daha sonra uygulama ağ geçidi GatewaySubnet yerleştirirseniz daha sonra bir sanal ağ geçidi oluşturamıyor olacaktır. Ayrıca, kendi alt ağda olabilir tek şey ana olduğu gibi ILB ana tarafından kullanılan alt ağ içinde yerleştirin.

## <a name="steps-to-configure"></a>Yapılandırma adımları ##

1. Öğesinden Azure portalını Git **yeni > Ağ > Uygulama ağ geçidi** 
    1. Sağlar:
        1. Uygulama ağ geçidi adı
        1. WAF seçin
        1. Ana sanal ağ için kullanılan aynı aboneliği seçin
        1. Kaynak grubu seçin veya oluşturun
        1. Ana sanal ağ konumu seçin

    ![Yeni uygulama ağ geçidi oluşturma temelleri][2]   
    1. Ayarları alanı ayarlayın:
        1. Ana sanal ağ
        1. Uygulama ağ geçidi alt ağı uygulamasına dağıtılması gerekir. VPN ağ geçitleri oluşturulmasını engeller gibi hiçbir kullanım GatewaySubnet yapın.
        1. Ortak seçin
        1. Bir ortak IP adresi seçin. Sahip değilse daha sonra oluşturmak biri şu anda
        1. HTTP veya HTTPS için yapılandırın. HTTPS için yapılandırıyorsanız, bir PFX sertifikası sağlamanız gerekir
        1. Web uygulaması fireway ayarları seçin. Burada Güvenlik Duvarı'nı etkinleştirin ve ayrıca ya da algılamayı ayarlayın veya önleme gördüğünüz gibi uygun.

    ![Yeni uygulama ağ geçidi oluşturma ayarları][3]
    
    1. Özet bölümü gözden geçirme seçin **Tamam**. 30 dakikadan Uygulama Kurulumu tamamlamak ağ geçidiniz için biraz uzun sürebilir.  

2. Uygulama ağ geçidi Kurulum tamamlandıktan sonra uygulama ağ geçidi Portalı'na gidin. Seçin **arka uç havuzu**.  ILB adresi için ILB ana ekleyin.

    ![Arka uç havuzunu yapılandırma][4]

3. Arka uç havuzu yapılandırmak için işlem tamamlandıktan sonra seçin **sistem durumu araştırmalarının**. Uygulamanız için kullanmak istediğiniz etki alanı adı için bir sistem durumu araştırma oluşturun. 

    ![Sistem durumu araştırmalarını yapılandırma][5]
    
4. Sistem durumu araştırmalarının yapılandırmak için işlem tamamlandıktan sonra seçin **HTTP ayarları**.  Mevcut ayarı var. düzenleme, seçin **kullanım özel araştırma**ve yapılandırdığınız araştırma seçin.

    ![HTTP ayarlarını yapılandırma][6]
    
5. Uygulama ağ geçidi gidin **genel bakış**ve uygulama ağ geçidi için kullanılan genel IP adresini kopyalayın.  Uygulama etki alanı adınız için bir A kaydı olarak, bu IP adresi ayarlayın veya bir CNAME kaydı bu adres için DNS adını kullanın.  Genel IP adresini seçin ve genel IP adresini UI kopyalamak yerine uygulama ağ geçidi genel bakış bölümünde bağlantısında kopyalama çok kolaydır. 

    ![Uygulama ağ geçidi portalı][7]

6. Uygulamanız için özel etki alanı adı, ILB ana ayarlayın.  Uygulamanızın portal ve ayarları seçin altında gidin **özel etki alanları**

![Özel etki alanı adı uygulamasını ayarlama][8]

Web uygulamaları için özel etki alanı adlarını burada ayarlama hakkında bilgi [web uygulamanız için özel etki alanı adlarını ayarlama][custom-domain]. Bir uygulama ile bir ILB ana ve bu belge farktır tüm doğrulama etki alanı adı değil.  Uygulama uç noktaları yönetir DNS sahip olduğundan, var istediğiniz koyabilirsiniz. Bu durumda eklediğiniz özel etki alanı adı DNS sunucunuzun olması gerekmez, ancak hala uygulamanızın üzerinde yapılandırılması gerekmez. 

Kurulum tamamlandıktan sonra oluşturduğunuz özel etki alanı adına göre uygulamaya erişmek için daha sonra bir kısa sürede yaymak DNS değişikliklerinizin izin verdiğiniz. 


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
