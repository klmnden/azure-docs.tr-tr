---
title: "Azure yığını için planlama azure yığın güvenlik duvarı sistemlerini tümleşik | Microsoft Docs"
description: "Çok düğümlü dağıtımlar Azure yığın Azure bağlı Azure yığın güvenlik duvarı konuları açıklanmaktadır."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 737df48a24d8e077f898d00526c7138db69dbb93
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="azure-stack-firewall-integration"></a>Azure yığın güvenlik duvarı tümleştirmesi
Güvenli Azure yığın yardımcı olmak için bir güvenlik duvarı cihazı kullanmanız önerilir. Güvenlik duvarları ile dağıtılmış hizmet engelleme (DDOS) saldırıları, izinsiz giriş algılama ve içerik denetimi gibi yardımcı olmakla birlikte, aynı zamanda BLOB'lar, tablolar ve Kuyruklar gibi Azure storage Hizmetleri için bir işleme ayak haline gelebilir.

Azure Active Directory (Azure AD) veya Windows Server Active Directory Federasyon Hizmetleri (AD FS) kimlik modeline bağlı olarak, siz AD FS uç noktasına yayımlamak için gerekli olabilir. Bağlantısı kesilmiş dağıtım modu kullanılırsa, AD FS uç noktasına yayımlamanız gerekir. Daha fazla bilgi için bkz: [datacenter tümleştirme kimlik makale](azure-stack-integrate-identity.md).

Azure Resource Manager (Yönetici), Yönetici portalı ve anahtar kasası (Yönetici) uç noktaları dış yayımlama mutlaka gerektirmez. Örneğin, bir hizmet sağlayıcısı olarak, saldırı yüzeyini sınırlayabilir ve yalnızca Azure yığınından ağınızdaki ve internet'ten yönetmek isteyebilirsiniz.

Büyük kuruluşlar için dış ağ mevcut bir kurumsal ağ olabilir. Böyle bir senaryoda, şirket ağından Azure yığın çalışması için bu uç yayımlamanız gerekir.

### <a name="network-address-translation"></a>Ağ Adresi Çevirisi
Ağ adresi çevirisi (NAT) dağıtım sanal makine Acil Durum Kurtarma Konsolu (ERCS) VM'ler yanı sıra, dış kaynaklara ve dağıtım sırasında Internet'e erişmek için (DVM) ya da ayrıcalıklı uç noktası (CESARETLENDİRİCİ) sırasında izin vermek için önerilen yöntemdir kayıt ve sorun giderme.

NAT, dış ağ veya ortak VIP'ler genel IP adresleri için bir alternatif olarak da olabilir. Ancak, Kiracı kullanıcı deneyimi sınırlar ve karmaşıklığını artırır çünkü bunu yapmanız önerilmez. İki seçenek hala kullanıcı IP havuzu veya çok: 1 NAT kuralı bir kullanıcı kullanıyor olabilecek tüm bağlantı noktalarına ilişkilendirmeleri içeren kullanıcı VIP başına gerektiren NAT üzerindeki her bir genel IP gerektiren 1:1 NAT olacaktır.

Bazı ortak VIP için NAT kullanarak olumsuzlukları şunlardır:
- NAT, kullanıcıların kendi uç noktaları ve kendi yayımlama kuralları yazılım tanımlı ağ (SDN) yığınında denetlemek için güvenlik duvarı kuralları yönetirken yükü ekler. Kullanıcıların yayımlanan kendi VIP'ler almak ve bağlantı noktası listesini güncelleştirmek için Azure yığın işleci başvurmanız gerekir.
- NAT kullanımı kullanıcı deneyimini sınırlar sırasında tam denetim işlecine yayımlama istekleri üzerinden verir.
- Azure ile karma bulut senaryosu için Azure VPN tüneli için NAT'ı kullanarak bir uç nokta ayarlamayı desteklemiyor göz önünde bulundurun.

### <a name="ssl-decryption"></a>SSL şifre çözme
Şu anda SSL şifre çözme üzerinde Bizim önerimiz tüm Azure yığın trafiği devre dışı bırakmak için gelecekte biz SSL şifre çözme için Azure yığınına etkinleştirme hakkında kılavuzluk sağlar.

## <a name="edge-firewall-scenario"></a>Sınır güvenlik duvarı senaryosu
Bir sınır dağıtımında Azure yığın doğrudan sınır yönlendiricisi veya güvenlik duvarının arkasındaysa dağıtılır. Bu senaryolarda, kenarlığın ya da eşit maliyet çoklu yol (ECMP) BGP veya statik yönlendirme ile destekliyorsa, sınır cihazı olarak işlev gören üstünde olacak şekilde Güvenlik Duvarı'nda desteklenmiyor.

Genellikle, genel olarak yönlendirilebilir IP adreslerinin dış ağdan ortak VIP havuzu için dağıtım sırasında belirtilir. Bir sınır senaryosunda, bu ortak yönlendirilebilir IP'leri herhangi bir ağ üzerindeki güvenlik amacıyla kullanmak için önerilmez. Bu senaryo bir kullanıcı Azure gibi genel bulut gibi tam Self denetimli bulut deneyimi deneyimi sağlar.  

![Azure yığın sınır güvenlik duvarı örneği](.\media\azure-stack-firewall\edge-firewall-scenario.png)

## <a name="enterprise-intranet-or-perimeter-network-firewall-scenario"></a>Kurumsal intranet veya çevre ağ güvenlik duvarı senaryoları
Bir Kurumsal intranet veya çevre dağıtımında, Azure yığın çoklu bölgesinin bir güvenlik duvarı veya sınır güvenlik duvarı ve şirket, iç ağ güvenlik duvarı arasında dağıtılır. Trafik, ardından güvenli, çevre ağındaki (DMZ arasındaki veya) dağıtılır ve güvensiz bölgeler olarak aşağıda açıklanan:

- **Güvenli Bölge**: iç ya da şirket yönlendirilebilir IP adresleri kullanan iç ağ budur. Güvenli ağ ayrılabilir, Internet giden NAT üzerinden Güvenlik Duvarı'nı erişiminiz ve genellikle iç ağ üzerinden veri merkeziniz içine herhangi bir yerden erişilebilir. Tüm Azure yığın ağları dış ağın genel VIP havuzu dışında güvenli bölgede bulunmalıdır.
- **Çevre bölge**. Çevre ağ dış veya internet'e yönelik Web sunucuları genellikle dağıtılan gibi uygulamalar olabilir. Genellikle, DDoS ve hala internet'ten belirtilen gelen trafiğe izin verme sırasında izinsiz (korsan) gibi saldırılarını önlemek için bir güvenlik duvarı tarafından izlenir. Yalnızca dış ağ ortak VIP havuzu Azure yığınının DMZ bölgede bulunmalıdır.
- **Güvenli olmayan bölge**. Bu dış, Internet'e ağdır. Bu **değil** güvenli olmayan bölge içindeki Azure yığın dağıtmak için önerilir.

![Azure yığın çevre ağı örneği](.\media\azure-stack-firewall\perimeter-network-scenario.png)


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [bağlantı noktaları ve Azure yığın uç noktaları tarafından kullanılan protokoller](azure-stack-integrate-endpoints.md)

