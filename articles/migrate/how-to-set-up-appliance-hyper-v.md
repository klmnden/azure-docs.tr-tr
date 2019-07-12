---
title: Bir gereç geçişi Server değerlendirme/geçiş için Hyper-V Vm'leri için ayarlayın. | Microsoft Docs
description: Bulma, değerlendirme ve aracısız geçiş, Hyper-V Vm'lerini Azure geçişi sunucusu değerlendirme/geçişi kullanan bir gereç ayarlama işlemini açıklamaktadır.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/08/2019
ms.author: raynew
ms.openlocfilehash: c531fe49ebff6c021547c2d1c2f382bcd6c9caef
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811771"
---
# <a name="set-up-an-appliance-for-hyper-v-vms"></a>Hyper-V Vm'leri için bir gereç ayarlama

Bu makalede, Hyper-V Vm'lerini Azure geçişi Server değerlendirmesi aracı ile değerlendirme veya VMware sanal makinelerini Azure geçişi sunucu geçiş aracını kullanarak Azure'a geçiş, Azure geçişi gerecini ayarlamak açıklar.

Hyper-V VM gereç, aşağıdakileri yapmak için Azure geçişi Server değerlendirme/geçiş tarafından kullanılan basit bir gereçtir:

- Şirket içi Hyper-V Vm'lerini keşfedin.
- Meta veri ve performans verilerini bulunan VM'ler için Azure geçişi Server değerlendirme/geçirme gönderin.

[Daha fazla bilgi edinin](migrate-appliance.md) Azure geçişi Gereci hakkında.


## <a name="appliance-deployment-steps"></a>Gereç dağıtım adımları

Ayarladığınız gerecini ayarlamak için:
- Sıkıştırılmış bir Hyper-V VHD Azure portalından indirin.
- Aleti oluşturmak ve Azure geçişi Server değerlendirmesi için bağlanıp bağlanamadığını denetleyin. 
- İlk kez Gereci yapılandırın ve Azure geçişi projesi ile kaydedin.

## <a name="download-the-vhd"></a>VHD'yi indirin

Gereç için daraltılmış VHD şablonunu indirin.

1. İçinde **geçiş hedefleri** > **sunucuları** > **Azure geçişi: Server değerlendirmesi**, tıklayın **bulma**.
2. İçinde **makineleri keşfet** > **makineleriniz sanallaştırıldı mı?** , tıklayın **Evet, Hyper-V ile**.
3. Tıklayın **indirme** VHD dosyasını indirmek için.

    ![VM indirme](./media/how-to-set-up-appliance-hyper-v/download-appliance-hyperv.png)


### <a name="verify-security"></a>Güvenlik doğrulayın

Daraltılmış dosyayı dağıtmadan önce güvenli olup olmadığını denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. VHD için karma oluşturmak için aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3.  Gereç sürüm 1.19.05.10 oluşturulan karma bu ayarlara uygun olmalıdır.

  **Algoritma** | **Karma değeri**
  --- | ---
  SHA256 | 598d2e286f9c972bb7f7382885e79e768eddedfe8a3d3460d6b8a775af7d7f79


  
## <a name="create-the-appliance-vm"></a>' % S'Gereci VM oluşturma

İndirilen dosyayı içeri aktarın ve VM oluşturun.

1. Hyper-V konağında VM Gereci barındıracak sıkıştırılmış VHD dosyasını bir klasöre ayıklayın. Üç klasör ayıklanır.
2. Hyper-V Yöneticisi'ni açın. İçinde **eylemleri**, tıklayın **sanal makineyi içeri aktar**.

    ![VHD dağıtma](./media/how-to-set-up-appliance-hyper-v/deploy-vhd.png)

2. Sanal makine İçeri Aktarma Sihirbazı'nda > **başlamadan önce**, tıklayın **sonraki**.
3. İçinde **klasörü Bul**, ayıklanan VHD içeren klasörü belirtin. Ardından **İleri**'ye tıklayın.
1. İçinde **sanal makineyi Seç**, tıklayın **sonraki**.
2. İçinde **içe aktarma tipini seç**, tıklayın **sanal makineyi kopyala (yeni bir benzersiz kimlik Oluştur)** . Ardından **İleri**'ye tıklayın.
3. İçinde **seçin hedef**, varsayılan ayarı değiştirmeyin.           **İleri**'ye tıklayın.
4. İçinde **depolama klasörleri**, varsayılan ayarı değiştirmeyin.           **İleri**'ye tıklayın.
5. İçinde **ağ seçin**, VM'nin kullanacağı sanal anahtar belirtin. Anahtar verileri Azure'a göndermek için internet bağlantısı gerekir.
6. İçinde **özeti**, ayarları gözden geçirin. Ardından **son**.
7. Hyper-V Yöneticisi > **sanal makineler**, VM'yi başlatın.


### <a name="verify-appliance-access-to-azure"></a>Azure'a Gereci erişimi doğrulayın

Gereç için VM bağlanabildiğinden emin olun [Azure URL'lerini](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access).

## <a name="configure-the-appliance"></a>Gereci yapılandırın

Gereç ilk kez ayarlayın.

1. Hyper-V Yöneticisi > **sanal makineler**, VM'ye sağ tıklayın > **Connect**.
2. Gereç için dil, saat dilimi ve parola sağlayın.
3. VM'ye bağlanın ve gereç web uygulamasının URL'sini açın herhangi bir makinede bir tarayıcı açın: **https://*gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulamayı uygulama kısayoluna tıklayarak Gereci Desktop'tan açabilirsiniz.
1. Web uygulamasında > **önkoşulları ayarlama**, aşağıdakileri yapın:
    - **Lisans**: Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - **Bağlantı**: Uygulama, VM internet erişimi olup olmadığını denetler. VM bir ara sunucu kullanıyorsa:
        - Tıklayın **Proxy ayarlarını**ve dinleme bağlantı noktasını ve proxy adresi biçiminde belirtin http://ProxyIPAddress veya http://ProxyFQDN.
        - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
        - Yalnızca HTTP proxy’si desteklenir.
    - **Zaman eşitleme**: Zaman doğrulanır. Gereç zamanında düzgün çalışması, VM bulma için internet saati ile eşitlenmiş olması gerekir.
    - **Güncelleştirmeleri yüklemek**: Azure geçişi Server değerlendirmesi, gereç en son güncelleştirmelerin yüklü olduğunu denetler.

### <a name="register-the-appliance-with-azure-migrate"></a>Gerecin Azure geçişi ile kaydetme

1. Tıklayın **oturum**. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bıraktık emin olun.
2. Yeni sekmede Azure kimlik bilgilerinizi kullanarak oturum açın. 
    - Kullanıcı adı ve parola ile oturum açın.
    - Bir PIN ile oturum açma desteklenmiyor.
3. Başarıyla oturum açtıktan sonra web uygulamasına geri dönün.
4. Azure geçişi projesi oluşturulduğu abonelik seçin. Ardından, projeyi seçin.
5. Gereç için bir ad belirtin. Ad ile 14 karakterler alfasayısal veya daha az olmalıdır.
6. Tıklayın **kaydetme**.


### <a name="delegate-credentials-for-smb-vhds"></a>SMB VHD'ler için kimlik bilgileri temsilcisi

VHD'ler SMB'ler üzerinde çalıştırıyorsanız, Hyper-V konaklarına gereç kimlik temsili etkinleştirmeniz gerekir. Bu gereçten yapmak için:

1. VM gerecinde şu komutu çalıştırın. Örnek konak adları HyperVHost1/HyperVHost2 var.

    ```
    Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force
    ```

2. Alternatif olarak, yerel Grup İlkesi Düzenleyicisi'nde gereçte yapın:
    - İçinde **yerel bilgisayar ilkesi** > **Bilgisayar Yapılandırması**, tıklayın **Yönetim Şablonları** > **sistem**  >  **Kimlik bilgileri temsilcisi**.
    - Çift **yeni kimlik bilgileri yetki aktarımına izin ver**seçip **etkin**.
    - İçinde **seçenekleri**, tıklayın **Göster**, ve ile listesi için keşfetmek istediğiniz her Hyper-V konağına eklemek **wsman /** öneki olarak.
    - İçinde **kimlik bilgileri devretme**, çift **yalnızca NTLM kimlik doğrulaması ile yeni kimlik bilgileri yetki aktarımına izin ver**. İle bir listesi için keşfetmek istediğiniz her Hyper-V konağı yeniden ekleyin **wsman /** öneki olarak.

## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Hyper-V konakları veya kümeleri Gereci bağlanmak ve VM bulmayı Başlat.

1. İçinde **kullanıcı adı** ve **parola**, gereç Vm'leri bulmak için kullanacağı hesap kimlik bilgilerini belirtin. Kimlik bilgileri için kolay bir ad belirtin ve tıklayın **ayrıntıları Kaydet**.
2. Tıklayın **konak Ekle**, Hyper-V konak/küme ayrıntıları belirtin.
3. Tıklayın **doğrulama**. Doğrulama sonrasında her konak/küme üzerinde bulunan VM sayısı gösterilir.
    - Bir konak için doğrulama başarısız olursa, simgenin üzerine gelerek hatayı gözden geçirme **durumu** sütun. Sorunları giderin ve yeniden doğrulayın.
    - Konaklara veya kümelere kaldırmak için seçin > **Sil**.
    - Belirli bir ana bilgisayara bir kümeden kaldırılamıyor. Tüm küme yalnızca kaldırabilirsiniz.
    - Küme içindeki belirli ana bilgisayarlar ile ilgili sorunlar olsa bile, bir küme ekleyebilirsiniz.
4. Doğrulama **kaydedin ve bulmayı Başlat** bulma işlemini başlatmak için.

Bu bulma başlatır. Azure Portalı'nda görünmesi için meta verileri bulunan VM'ler için yaklaşık 15 dakika sürer. 

## <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Bulma tamamlandıktan sonra sanal makinelerin portalda görüntülenip görüntülenmediğini doğrulayabilirsiniz.

1. Azure geçişi panoyu açın.
2. İçinde **Azure geçişi - sunucuları** > **Azure geçişi: Server değerlendirmesi** sayfasında, sayısını görüntüler simgesini **bulunan sunucuları**. 


## <a name="next-steps"></a>Sonraki adımlar

Denemenin [Hyper-V değerlendirme](tutorial-assess-hyper-v.md) Azure geçişi Server değerlendirmesi ile.