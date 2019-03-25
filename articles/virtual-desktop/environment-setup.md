---
title: Windows sanal masaüstü Önizleme ortam - Azure
description: Bir Windows sanal masaüstü Önizleme ortamı temel öğeleri.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: ceed6a8bb74206b7c6689ce542482148800e4ba9
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403519"
---
# <a name="windows-virtual-desktop-preview-environment"></a>Windows sanal masaüstü Önizleme ortamı

Windows sanal masaüstü Önizleme kullanıcıların sanallaştırılmış Masaüstü ve RemoteApps için kolay ve güvenli erişim sağlayan bir hizmettir. Bu konu Windows Sanal Masaüstü ortamının genel yapısı hakkında daha fazla bit bildirir.

## <a name="tenants"></a>Kiracılar

Windows sanal masaüstü Kiracı, Windows sanal masaüstü ortamınızı yönetmek için birincil arabirimidir. Azure Active Directory'ye ortama oturum açacak kullanıcıları içeren her bir Windows sanal masaüstü Kiracı ilişkilendirilmiş olması gerekir. Windows sanal masaüstü kiracıdan kullanıcılarınızın iş yüklerini çalıştırmak için ana bilgisayar havuzları oluşturarak başlayabilirsiniz.

## <a name="host-pools"></a>Ana bilgisayar havuzları

Bir konak havuzu, Windows sanal masaüstü Aracısı'nı çalıştırdığınızda, Windows sanal masaüstü oturum ana bilgisayarları olarak kaydeden Azure sanal makineler koleksiyonudur. Bir konak havuzundaki tüm oturumu konak sanal makinelerin tutarlı bir kullanıcı deneyimi için aynı görüntü kaynağı.

Bir konak havuzu iki tür biri olabilir:

- Kişisel, burada her oturumu ana bilgisayarı bireysel kullanıcılara atanır.
- Havuza, burada oturum ana konak havuz içindeki bir uygulama grubu için yetkili herhangi bir kullanıcı bağlantılarını kabul edebilir.

Kaç oturum her oturumu ana bilgisayarı alabilir, uygulamanın Yük Dengeleme davranışını değiştirmek için konak havuzu ve kullanıcı oturumu Konaklara Windows sanal masaüstü oturumları için oturum açmış durumdayken konak havuzdaki yapabileceklerinizi ek özellikleri ayarlayabilirsiniz. Kullanıcılara uygulama grupları aracılığıyla yayımlanan kaynak denetim.

## <a name="app-groups"></a>Uygulama grupları

Bir uygulama grubu, konak havuzu oturum ana bilgisayarda yüklü uygulamaların bir mantıksal grubudur. Bir uygulama grubu, iki tür biri olabilir:

- RemoteApp kullanıcıları RemoteApps eriştiği, ayrı ayrı seçin ve uygulama grubuna yayımlayın
- Kullanıcıların tam masaüstü eriştiği Masaüstü

Varsayılan olarak, her bir ana makine havuzu oluşturduğunuzda ("Masaüstü uygulama grubu" adlı) bir masaüstü uygulaması grubu otomatik olarak oluşturulur. Bu uygulama grubu dilediğiniz zaman kaldırabilirsiniz. Ancak, bir masaüstü uygulaması grubu bulunduğu sürece başka bir masaüstü uygulaması grubu konak havuzunda oluşturulamıyor. RemoteApps yayımlamak için bir RemoteApp uygulama grubu oluşturmanız gerekir. Farklı alt senaryolara uyum sağlamak için birden fazla RemoteApp uygulama grupları oluşturabilirsiniz. Farklı RemoteApp uygulama grupları, çakışan RemoteApps de içerebilir.

Kaynakları kullanıcılara yayımlamak için uygulama gruplara atamanız gerekir. Kullanıcılar uygulama gruplarına atarken, şunları göz önünde bulundurun:

- Bir kullanıcı Masaüstü uygulama grubu hem de RemoteApp uygulama grubu aynı konak havuzda atanamaz.
- Bir kullanıcı birden çok uygulama grupları aynı konak havuzdaki atanabilir ve kendi akış her iki uygulama grupları birikmesi olacaktır.

## <a name="tenant-groups"></a>Kiracı grupları

Windows sanal masaüstü, Windows sanal masaüstü kiracının nerede kurulumu ve yapılandırması çoğu olur. Windows sanal masaüstü Kiracı konak havuzları, uygulama grupları ve uygulama grubu kullanıcı atamalarını içerir. Ancak, özellikle bir bulut hizmeti sağlayıcısı (CSP) veya bir barındırma iş ortağı yöneticisiyseniz, tek bir seferde birden çok Windows sanal masaüstü kiracılar yönetme gereken belirli durumlar olabilir. Bu durumda, her biri müşterilerin Windows sanal masaüstü kiracılar koyun ve erişimini merkezi olarak yönetmek için özel bir Windows sanal masaüstü Kiracı grubu kullanabilirsiniz. Ancak, yalnızca tek bir Windows sanal masaüstü kiracısı yönetiyorsanız, Kiracı grubu kavramı geçerli değildir ve çalışır ve varsayılan Kiracı grubu mevcut kiracınız yönetmeye devam.

## <a name="end-users"></a>Son kullanıcılar

Kullanıcılar kendi uygulama gruplarına atadıktan sonra Windows sanal masaüstü istemcilerden herhangi biri ile bir Windows sanal masaüstü dağıtımı bağlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Temsilci erişimi ve rolleri kullanıcılara atama hakkında daha fazla bilgi [Temsilcili erişim Windows sanal masaüstü önizlemede](delegated-access-virtual-desktop.md).

Windows sanal masaüstü kiracınızı ayarladığınızda öğrenmek için bkz. [Windows sanal masaüstü Önizleme'de bir kiracı oluşturmanız](tenant-setup-azure-active-directory.md).

Windows sanal masaüstüne bağlanma hakkında bilgi almak için aşağıdaki makalelerden birine bakın:

- [Uzak Masaüstü İstemcisi Windows 7 ve Windows 10 bağlanma](connect-windows-7-and-10.md)
- [Windows sanal masaüstü Önizleme web istemcisi için Bağlan](connect-web.md)
