---
title: Veri kutusu kenar güvenlik | Microsoft Docs
description: Veri kutusu Edge cihaz, hizmet ve şirket içindeki ve buluttaki verileri korumaya güvenlik ve gizlilik özellikleri açıklar.
services: Data Box Edge
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/22/2019
ms.author: alkohli
ms.openlocfilehash: 43de22f7e56178559df4fc45980d064962580d2b
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403400"
---
# <a name="data-box-edge-security-and-data-protection"></a>Veri kutusu kenar güvenlik ve veri koruması

Teknoloji gizli veya özel verilerle özellikle kullanılıyorsa, yeni bir Teknoloji benimseme zaman güvenlik önemli bir konudur. Microsoft Azure veri kutusu Edge çözümü, yalnızca yetkili varlıklar görüntüleme, değiştirme veya verilerinizi silme emin olun yardımcı olur.

Bu makalede, tüm çözüm bileşenlerini ve bunlar üzerinde depolanan verileri korumaya yardımcı olmak veri kutusu kenar güvenlik özellikleri açıklanır.

Azure veri kutusu Edge çözüm birbiriyle etkileşim dört ana bileşenden oluşur:

- **Veri kutusu Edge / veri kutusu ağ geçidi hizmeti, Azure'da barındırılan** – cihaz sırasını oluşturmak, cihazı yapılandırma ve ardından sırayla tamamlanması izlemek için kullandığınız yönetim kaynak.
- **Veri kutusu Edge cihazı** – şirket içi verilerinizi Azure'a aktarmanız kadar size sevk aktarım cihazı.
- **İstemciler/ana bilgisayarları bağlı cihaza** – veri kutusu Edge cihazına bağlanmak ve korunması gereken verileri içeren istemcilerin altyapınızdaki.
- **Bulut depolama** – Azure bulutunda verilerin depolandığı konum. Bu konum normalde, oluşturduğunuz veri kutusu Edge kaynağa bağlı depolama hesabıdır.


## <a name="data-box-edgedata-box-gateway-service-protection"></a>Veri kutusu Edge/veri kutusu ağ geçidi hizmeti koruma

Veri kutusu Edge/veri kutusu ağ geçidi hizmeti, Microsoft Azure'da barındırılan bir yönetim hizmetidir. Hizmet, yapılandırmak ve cihazı yönetmek için kullanılır.

- Veri kutusu Edge/veri kutusu ağ geçidi hizmeti erişim, kuruluşunuzun bir Kurumsal Anlaşma (EA) veya Bulut çözümü sağlayıcısı (CSP) aboneliği olmasını gerektirir. Daha fazla bilgi için Git [bir Azure aboneliği için kaydolun](https://azure.microsoft.com/resources/videos/sign-up-for-microsoft-azure/)!
- Management hizmetiniz Azure'da barındırıldığı için Azure güvenlik özellikleri tarafından korunur. Microsoft Azure tarafından sağlanan güvenlik özellikleri hakkında daha fazla bilgi için [Microsoft Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/security/)’ne gidin.

## <a name="data-box-edge-device-protection"></a>Veri kutusu Edge cihaz koruma

Veri kutusu sınır cihazı, yerel olarak işleme ve sonra bunu Azure'a göndererek verileri dönüştürme yardımcı olan bir şirket içi cihazdır. Cihazınız:

- Veri kutusu Edge/veri kutusu ağ geçidi hizmetine erişmek için bir etkinleştirme anahtarı gerekir.
- Her zaman bir cihaz parola korumalı.
- Kilitli aygıttır. Cihaz BMC ve BIOS BIOS sınırlı kullanıcı erişimi ile parola korumalı.
- Güvenli Önyükleme etkin.
- Windows Defender'ı cihaz koruyucusu çalıştırır. Device Guard, yalnızca kod bütünlüğü ilkelerinizde tanımladığınız güvenilen uygulamaları çalıştıracak olanak tanır. 

### <a name="protect-the-device-via-activation-key"></a>Cihaz etkinleştirme anahtarı aracılığıyla koruma

Yetkili bir veri kutusu Edge cihazı yalnızca Azure aboneliğinizde oluşturduğunuz veri kutusu Edge/veri kutusu ağ geçidi hizmetine katılmaya izin verilmez. Bir cihaz yetkilendirmek için veri kutusu Edge/veri kutusu ağ geçidi hizmeti cihazla etkinleştirmek için etkinleştirme anahtarı kullanmanız gerekir. Daha fazla bilgi için Git [etkinleştirme anahtarı alma](data-box-edge-deploy-prep.md#get-the-activation-key).

Kullandığınız etkinleştirme anahtarı:

- Bir Azure Active Directory (AAD) tabanlı kimlik doğrulama anahtarı ' dir.
- Üç gün sonra süresi dolar.
- Cihaz etkinleştirildikten sonra kullanılmaz.
 
Bir cihaz etkinleştirildikten sonra Microsoft Azure ile iletişim kurmak için belirteçleri kullanır.

### <a name="protect-the-device-via-password"></a>Cihaz parola aracılığıyla koruma

Parolalar, verilerinizi yalnızca yetkili kullanıcılar için erişilebilir olduğundan emin olun. Veri kutusu Edge ve veri kutusu ağ cihazları önyükleme kilitli bir durumda.

Şunları yapabilirsiniz:

- Yerel web kullanıcı Arabirimi cihazın bir tarayıcı aracılığıyla bağlanın ve ardından cihazda oturum için bir parola sağlayın.
- Uzaktan HTTP üzerinden cihaz PowerShell arabirimine bağlanın. Uzaktan Yönetim varsayılan olarak etkinleştirilir. Sonra cihazda oturum açmasına cihaz parolasının sunabilir. Daha fazla bilgi için Git [veri kutusu Edge cihazınıza uzaktan bağlanma](data-box-edge-connect-powershell-interface.md#connect-to-the-powershell-interface).

Aşağıdaki en iyi uygulamaları göz önünde bulundurun:

- Veri kutusu Edge hizmetinin var olan parolaların alınamıyor: Bu yalnızca bunları Azure portalı üzerinden sıfırlayabilirsiniz. Unutulursa, parola sıfırlama gerekmez, tüm parolaları güvenli bir yerde depolamanız önerilir. Parola sıfırlama, bunu sıfırlamadan önce tüm kullanıcılara bildirin emin olun.
- Yerel web kullanıcı Arabirimine kullanım [parolayı değiştirmek](data-box-gateway-manage-access-power-connectivity-mode.md#manage-device-access). Parolayı değiştirirseniz, böylece bir oturum açma hatası yaşamamasını tüm uzaktan erişim kullanıcıları bilgilendir emin olun.
- Cihazınızın Windows PowerShell arabirimi HTTP uzaktan erişebilirsiniz. Güvenlik açısından en iyisi, yalnızca güvenilen ağlarda HTTP kullanmanız gerekir.
- Cihaz parolası güçlü ve iyi korumalı olduğundan emin olun. İzleyin [en iyi parola uygulamaları](https://docs.microsoft.com/azure/security/azure-security-identity-management-best-practices#enable-password-management).

## <a name="protect-the-data"></a>Verileri koruma

Bu bölümde, Taşınmakta olan veriler ve depolanan verileri korumaya veri kutusu kenar güvenlik özellikleri açıklanmaktadır.

### <a name="protect-data-at-rest"></a>Bekleyen verileri koruma

-Bekleyen veri için:

- -Bekleyen veri için veri kutusu Edge yerel verileri korumak için BitLocker XTS-AES-256 şifreleme kullanır.
- Paylaşımları bulunan verileri için paylaşımlara erişim sınırlıdır.

    - Paylaşım verilere erişen SMB istemcileriniz için bunların paylaşımıyla ilişkili kullanıcı kimlik bilgileri gerekir. Bu kimlik bilgileri paylaşımı oluşturma zamanında tanımlanır.
    - Paylaşımlara erişmek için NFS istemcilerinin, istemcileri IP adreslerini paylaşımı oluşturma sırasında eklenmesi gerekir.


### <a name="protect-data-in-flight"></a>Uçuş verileri koruma

Veri-de-uçuş için:

- Cihaz ve Azure arasında veri standart TLS 1.2 kullanılır. TLS 1.1 ve önceki sürümleri geri dönüş yoktur. Aracı iletişimi, TLS 1.2 desteklenmiyorsa engellenir. TLS 1.2 portal ve SDK'sı yönetim işlemleri için de gereklidir.
- İstemciler bir tarayıcıda yerel web kullanıcı Arabirimi üzerinden Cihazınızı eriştiğinizde, standart TLS 1.2 varsayılan güvenli protokol olarak kullanılır.

    - TLS 1.2 kullanmak için tarayıcınızı yapılandırmak için en iyi yöntem olacaktır.
    - Tarayıcı TLS 1.2 desteklemiyorsa, TLS 1.1 veya TLS 1.0 kullanabilirsiniz.
- Veri sunucularınızdan kopyaladığınızda verilerini korumak için SMB 3.0 şifreleme ile kullanmanızı öneririz.

### <a name="protect-data-via-storage-accounts"></a>Depolama hesapları ile verileri koruma

Cihazınızı verilerinizi azure'da için hedef olarak kullanılan bir depolama hesabı ile ilişkilidir. Depolama hesabına erişim için abonelik ve iki adet 512 bit Depolama tarafından denetlenen erişim anahtarları, depolama hesabıyla ilişkilendirilmiş.

Veri kutusu sınır cihazı, depolama hesabına eriştiğinde anahtarlarından birini kimlik doğrulaması için kullanılır. Başka bir tuşa anahtarlarını düzenli aralıklarla döndürmenizi sağlayan yedekte tutulur.

Güvenlik nedenleriyle, çok sayıda veri merkezleri anahtar döndürme gerektirir. Anahtar döndürme için bu en iyi uygulamaları izlemenizi öneririz:

- Depolama hesabı anahtarınız depolama hesabınızın kök parolasına benzer. Hesap anahtarınızın dikkatli bir şekilde koruyun. Yok parola diğer kullanıcılara dağıtmak, sabit kod veya başkalarının erişebileceği düz metin, herhangi bir yere kaydedin.
- [Hesap anahtarınızın yeniden](../storage/common/storage-account-manage.md#regenerate-access-keys) düşünüyorsanız, Azure portalını kullanarak tehlikeye girmiş olabilecek.
- Döndürme ve ardından [depolama hesap anahtarlarınızı eşitleme](data-box-gateway-manage-shares.md#sync-storage-keys) düzenli olarak, depolama hesabınıza yetkisiz kullanıcılar tarafından erişmediğinden emin olun yardımcı olmak için.
- Düzenli aralıklarla Azure yöneticiniz değiştirmek veya bu depolama hesabına doğrudan erişmek için Azure portalında depolama bölümünü kullanarak birincil veya ikincil anahtarını yeniden gerekir.


## <a name="manage-personal-information"></a>Kişisel bilgilerini yönetme

Data Box Edge / veri kutusu ağ geçidi hizmeti, aşağıdaki anahtar örneklerinde kişisel bilgileri toplar:

- **Sipariş ayrıntıları** – Sipariş oluşturulduktan sonra, kullanıcının teslimat adresi, e-postası, kişi bilgileri Azure portalında depolanır. Kaydedilen bilgiler:
  - Kişi adı
  - Telefon numarası
  - Email
  - Açık adres
  - Şehir
  - Posta kodu
  - Durum
  - Ülke/İl/Bölge
  - Kargo takip numarası

    Sipariş ayrıntılarını şifrelenir ve hizmetinde depolanır. Hizmet, kaynak veya sipariş açıkça silene kadar bilgileri saklar. Ayrıca, kaynak ve karşılık gelen sırası silinmesini cihaz Microsoft'a dönene kadar cihazı sevk edilen zaman engellenir.

- **Teslimat adresi** – siparişi veren sonra Data Box hizmeti UPS gibi üçüncü taraf operatörler için teslimat adresini sağlar.

- **Paylaşan kullanıcılar** -kullanıcıların Cihazınızda paylaşımlarını bulunan veriler da erişebilirsiniz. Paylaşımı verileri erişebilen kullanıcıları içeren bir liste görüntülenir ve görüntülenebilir. Bu liste, paylaşımları silindiğinde de silinir. Kimin erişebileceğini veya bir paylaşımı silmek kullanıcıların listesini görüntülemek için adımları izleyin. [yönetme veri kutusu uçta paylaşımları](data-box-gateway-manage-shares.md).

Daha fazla bilgi için, [Güven Merkezi](https://www.microsoft.com/trustcenter)’nde Microsoft Gizlilik ilkesini gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

[Veri kutusu Edge cihazınıza dağıtma](data-box-edge-deploy-prep.md).

