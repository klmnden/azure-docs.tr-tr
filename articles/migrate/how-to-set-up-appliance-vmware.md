---
title: VMware Vm'leri için Azure geçişi Server değerlendirme/geçiş için bir gereç ayarlama | Microsoft Docs
description: Bulma, değerlendirme ve VMware vm'lerinin Azure geçişi Server değerlendirme/geçiş kullanarak aracısız geçiş için bir gereç ayarlama açıklanır.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/08/2019
ms.author: raynew
ms.openlocfilehash: fe190381df346278e75a3e6fd9876b80c33bd86b
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811732"
---
# <a name="set-up-an-appliance-for-vmware-vms"></a>VMware Vm'leri için bir gereç ayarlama

Bu makalede, VMware Vm'lerini Azure geçişi Server değerlendirmesi aracı ile değerlendirme veya VMware sanal makinelerini Azure geçişi sunucu geçiş aracını kullanarak aracısız geçiş ile Azure'a geçiş, Azure geçişi gerecini ayarlamak açıklar.

VMware sanal gereç, aşağıdakileri yapmak için Azure geçişi Server değerlendirme/geçiş tarafından kullanılan basit bir gereçtir:

- Şirket içi VMware Vm'lerini keşfedin.
- Meta veri ve performans verilerini bulunan VM'ler için Azure geçişi Server değerlendirme/geçirme gönderin.

[Daha fazla bilgi edinin](migrate-appliance.md) Azure geçişi Gereci hakkında.


## <a name="appliance-deployment-steps"></a>Gereç dağıtım adımları

Ayarladığınız gerecini ayarlamak için:
- OVA bir şablon dosyasını indirin ve vCenter Server'a aktarın.
- Aleti oluşturmak ve Azure geçişi Server değerlendirmesi için bağlanıp bağlanamadığını denetleyin. 
- İlk kez Gereci yapılandırın ve Azure geçişi projesi ile kaydedin.

## <a name="download-the-ova-template"></a>OVA şablonunu indirme

1. İçinde **geçiş hedefleri** > **sunucuları** > **Azure geçişi: Server değerlendirmesi**, tıklayın **bulma**.
2. İçinde **makineleri keşfet** > **makineleriniz sanallaştırıldı mı?** , tıklayın **Evet, VMWare vSphere hiper Yöneticisi ile**.
3. Tıklayın **indirme** indirilemedi. OVA şablon dosyası.



### <a name="verify-security"></a>Güvenlik doğrulayın

OVA dosyasını dağıtmadan önce güvenli olup olmadığını denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. OVA'ın karmasını oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Gereç sürüm 1.0.0.5 oluşturulan karma bu ayarlara uygun olmalıdır. 

  **Algoritma** | **Karma değeri**
  --- | ---
  MD5 | ddfdf21c64af02a222ed517ce300c977


## <a name="create-the-appliance-vm"></a>' % S'Gereci VM oluşturma

İndirilen dosyayı içeri aktarın ve bir VM oluşturun.

1. vSphere Client konsolunda **Dosya** > **OVF Şablonu Dağıt**’a tıklayın.
2. OVF şablonu Dağıtma Sihirbazı > **kaynak**, OVA dosyasının konumunu belirtin.
3. İçinde **adı** ve **konumu**, VM için bir kolay ad belirtin. Sanal Makinenin barındırılacağı Envanter nesnesini seçin.
5. İçinde **konak/küme**, konağı veya kümeyi belirtin, sanal Makinenin çalıştığı.
6. İçinde **depolama**, VM için depolama hedefini belirtin.
7. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
8. İçinde **ağ eşlemesi**, sanal Makinenin bağlanacağı ağı belirtin. Ağ Azure geçişi Server değerlendirmesi için meta verileri göndermek için internet bağlantısına sahip olmalıdır.
9. Ayarları gözden geçirip onayladıktan sonra **Son**’a tıklayın.


### <a name="verify-appliance-access-to-azure"></a>Azure'a Gereci erişimi doğrulayın

Gereç için VM bağlanabildiğinden emin olun [Azure URL'lerini](migrate-support-matrix-vmware.md#assessment-url-access-requirements).


## <a name="configure-the-appliance"></a>Gereci yapılandırın

Gereç ilk kez ayarlayın.

1. vSphere Client konsolunda VM’ye sağ tıklayın ve **Konsolu Aç** seçeneğini belirleyin.
2. Gereç için dil, saat dilimi ve parola sağlayın.
3. VM'ye bağlanın ve gereç web uygulamasının URL'sini açın herhangi bir makinede bir tarayıcı açın: **https://*gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulamayı uygulama kısayoluna tıklayarak Gereci Desktop'tan açabilirsiniz.
4. Web uygulamasında > **önkoşulları ayarlama**, aşağıdakileri yapın:
    - **Lisans**: Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - **Bağlantı**: Uygulama, VM internet erişimi olup olmadığını denetler. VM bir ara sunucu kullanıyorsa:
        - Tıklayın **Proxy ayarlarını**ve dinleme bağlantı noktasını ve proxy adresi biçiminde belirtin http://ProxyIPAddress veya http://ProxyFQDN.
        - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
        - Yalnızca HTTP proxy’si desteklenir.
    - **Zaman eşitleme**: Zaman doğrulanır. Gereç zamanında düzgün çalışması bulma için internet saati ile eşitlenmiş olması gerekir.
    - **Güncelleştirmeleri yüklemek**: Azure geçişi, en son Gereci güncelleştirmelerin yüklü olduğunu denetler.
    - **VDDK yükleme**: Azure geçişi, VMWare vSphere Sanal Disk Geliştirme Seti (VDDK) yüklü olduğunu denetler.
        - Azure Migrates VDDK makineleri, azure'a geçiş sırasında çoğaltmak için kullanır.
        - Vmware'den VDDK 6.7 yükleyin ve indirilen ZIP içeriği gereçte belirtilen konuma ayıklayın.

## <a name="register-the-appliance-with-azure-migrate"></a>Gerecin Azure geçişi ile kaydetme

1. Tıklayın **oturum**. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bıraktık emin olun.
2. Yeni sekmede Azure kimlik bilgilerinizi kullanarak oturum açın. 
    - Kullanıcı adı ve parola ile oturum açın.
    - Bir PIN ile oturum desteklenmiyor.
3. Başarıyla oturum açtıktan sonra web uygulamasına geri dönün.
2. Azure geçişi projesi oluşturulduğu abonelik seçin. Ardından, projeyi seçin.
3. Gereç için bir ad belirtin. Ad ile 14 karakterler alfasayısal veya daha az olmalıdır.
4. Tıklayın **kaydetme**.


## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Şimdi, Gereci vCenter Server'a bağlanın ve VM bulmayı Başlat. 

1. İçinde **vCenter Server ayrıntılarını belirtin**, adını (FQDN) veya vCenter Server'ın IP adresi belirtin. Varsayılan bağlantı noktasını değiştirmeyin veya vCenter Server'ınıza dinlediği bir özel bağlantı noktası belirtin.
2. İçinde **kullanıcı adı** ve **parola**, Gereci vCenter sunucusundaki Vm'leri bulmak için kullanacağı salt okunur hesabın kimlik bilgilerini belirtin. Hesabı olduğundan emin olun [bulma için gerekli izinler](migrate-support-matrix-vmware.md#assessment-vcenter-server-permissions).
3. Tıklayın **bağlantısını doğrulama** için Gereci vCenter Server'a bağlanabildiğinden emin olun.
4. Bağlantı kurulduktan sonra tıklayın **kaydedin ve bulmayı Başlat**.


Bu bulma başlatır. Portalda görünmesi bulunan VM'ler meta verilerini yaklaşık 15 dakika sürer. 


## <a name="next-steps"></a>Sonraki adımlar

Öğreticileri için gözden [VMware değerlendirme](tutorial-assess-vmware.md) ve [aracısız geçiş](tutorial-migrate-vmware.md).
