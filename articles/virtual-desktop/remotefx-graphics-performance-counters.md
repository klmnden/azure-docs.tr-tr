---
title: Uzak Masaüstü - Azure grafik performans sorunlarını tanılama
description: Bu makalede, Windows sanal masaüstü grafikleri ile performans sorunlarını tanılamak için Uzak Masaüstü Protokolü oturumlarda RemoteFX grafik sayaçları kullanmayı açıklar.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshoot
ms.date: 05/23/2019
ms.author: v-chjenk
ms.openlocfilehash: 0b4113f1e0024415135aa99d1fb4e881efe448a3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66499272"
---
# <a name="diagnose-graphics-performance-issues-in-remote-desktop"></a>Uzak Masaüstü grafik performans sorunlarını tanılayın

Sistem beklendiği gibi değil gerçekleştirdiğinizde, sorunun kaynağını belirlemek önemlidir. Bu makalede tanımlamak ve Uzak Masaüstü Protokolü (RDP) oturumları sırasında grafikler ile ilgili performans sorunlarını gidermenize yardımcı olur.

## <a name="find-your-remote-session-name"></a>Uzak oturumu adınızı bulun

Grafik performans sayaçları tanımlamak için uzak oturum adı gerekir. Windows sanal masaüstü Önizleme uzak oturumu adınızı tanımlamak için bu bölümdeki yönergeleri izleyin.

1. Uzak oturumunuzdan Windows komut istemi açın.
2. Çalıştırma **qwinsta** komutu.
    - Oturumunuzu çok oturumu sanal makineler'de (VM) barındırılıyorsa: Oturum adınızla "rdp-tcp 37." gibi aynı soneki sonekidir her sayaç adı
    - Sanal grafik işlem birimi (vGPU) destekleyen bir VM'de oturumunuz barındırılıyorsa: Sayaçları, sanal makinenizin yerine sunucusunda depolanır. Oturum adı, VM adı numarası yerine örnekleri gibi yer "Win8 Kurumsal VM."

>[!NOTE]
> Sayaçları RemoteFX adlarında olsa da, vGPU senaryoları içinde hem de Uzak Masaüstü grafik içerir.

## <a name="access-performance-counters"></a>Erişim performans sayaçları

Performans sayaçları RemoteFX grafik çerçeve kodlama süresi gibi şeyleri izlemenize yardımcı olarak performans sorunlarını algılamanıza yardımcı ve çerçeveleri atlandı.

Uzak oturumu adınızı saptadıktan sonra uzak oturumunuz için RemoteFX grafik performans sayaçları toplamak için bu yönergeleri izleyin.

1. Seçin **Başlat** > **Yönetimsel Araçlar** > **Performans İzleyicisi**.
2. İçinde **Performans İzleyicisi** iletişim kutusunda **izleme araçları**seçin **Performans İzleyicisi**ve ardından **Ekle**.
3. İçinde **Sayaç Ekle** iletişim kutusu, gelen **kullanılabilir sayaçları** listesinde, RemoteFX grafik için performans sayacı nesnesini genişletin.
4. İzlenecek sayaçları seçin.
5. İçinde **seçili örnekleri nesne** listesinde, seçilen sayaçları izlenmesi ve ardından belirli örnekler seçin **Ekle**. Kullanılabilir tüm örnekleri seçmek için **tüm örnekleri**.
6. Sayaçlar ekledikten sonra seçin **Tamam**.

Seçili performans sayaçlarını Performans İzleyicisi ekranda görünür.

>[!NOTE]
>Her konakta etkin oturum kendi her performans sayacı örneği vardır.

## <a name="diagnosis"></a>Tanılama

Grafikler ile ilgili performans sorunlarını, genellikle dört kategoriye ayrılır:

- Düşük kare oranı
- Rastgele takılması
- Giriş yüksek gecikme süresi
- Düşük kare kalitesi

Giriş yüksek gecikme süresi düşük kare oranı ve rastgele takılması ele alarak başlayın. Sonraki bölümde, her kategoriye hangi performans sayaçlarını ölçmek bildirir.

### <a name="performance-counters"></a>Performans sayaçları

Bu bölüm, performans sorunlarını belirlemenize yardımcı olur.

İlk çıkış Frames/Second sayaç denetleyin. İstemciye sunulan kare sayısını ölçer. Bu değer giriş Frames/Second sayaç altındaysa, çerçeve atlanır. Performans sorunu tanımlamak için atlandı/saniye sayaçları çerçeveleri kullanın.

Çerçeve atlandı/saniye sayaçları üç tür vardır:

- Atlanan/saniye çerçeveler (yetersiz ağ kaynakları)
- Atlanan/saniye çerçeveler (yetersiz istemci kaynakları)
- Saniye başına Atlanan çerçeveler (yeterli sunucu kaynakları)

Atlanan/saniye sayaçları anlamına gelir sorun kaynağa ilişkilidir çerçevelerin herhangi bir yüksek değerli sayaç izler. Örneğin, istemci kod çözme değil ve sunucunun aynı hızda mevcut çerçeve çerçeveleri sağlar, çerçeve atlandı/saniye (istemci kaynaklar yetersiz) sayacı yüksek olacaktır.

Çıkış Frames/Second sayaç giriş Frames/Second sayaç eşleşirse, olağan dışı lag hala henüz elinizde veya durduruyor, sorunu kodlama ortalama süresi olabilir. Kodlama tek oturum (vGPU) senaryosunda sunucuda ve çoklu oturum senaryosunda VM gerçekleşen zaman uyumlu bir işlemdir. Ortalama süre kodlama 33 MS'nin altında olmalıdır. Kodlama ortalama süre 33 MS'nin altında olan ancak hala performans sorunları varsa, kullanmakta olduğunuz işletim sistemi ve uygulama ile ilgili bir sorun olabilir.

Uygulama ile ilgili sorunları tanılama hakkında daha fazla bilgi için bkz. [kullanıcı girişi gecikme performans sayaçları](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-rdsh-performance-counters).

RDP ortalama kodlama süresi 33 MS desteklediğinden, bir giriş kare hızı kadar 30 kare/saniye destekler. 33 ms desteklenen en yüksek kare hızı olduğuna dikkat edin. Çoğu durumda, kullanıcı tarafından deneyimli kare hızı ne sıklıkta bir çerçeve için RDP kaynağı tarafından sağlanan bağlı olarak, düşük olacaktır. Örneğin, bir video bir tam giriş kare hızı 30 kare/saniye gerektirir, ancak daha az kaynak yoğun görevleri seyrek bir word belgesini düzenleme gibi iyi bir kullanıcı deneyimi için Saniyedeki giriş, yüksek oranda gerektirmeyen izleme görevleri gibi.

Çerçeve Kalite sorunlarını tanılamak için kare kalitesi sayacı kullanın. Bu sayaç, çıkış çerçeve kalitesini kaynak çerçeveyi kalitesini yüzdesi olarak ifade eder. Kalite kaybı nedeniyle RemoteFX olabilir veya grafik kaynağı için devralınmış olabilir. RemoteFX kalite kaybına neden olursa, sorunu daha yüksek kaliteli içerik göndermek için ağ veya sunucu kaynak yetersizliği olabilir.

## <a name="mitigation"></a>Risk azaltma

Sunucu kaynaklarını performans sorununa neden oluyorsa, performansı artırmak için aşağıdaki şeylerden biri deneyin:

- Konak başına oturumlarının sayısını azaltın.
- Belleği arttırın ve işlem sunucusu üzerindeki kaynakları.
- Bağlantı çözümleme bırakın.

Ağ kaynaklarını performans sorununa neden olan oturumu başına ağ kullanılabilirliğini artırmak için aşağıdaki şeylerden biri deneyin:

- Konak başına oturumlarının sayısını azaltın.
- Bağlantı çözümleme bırakın.
- Daha yüksek bant genişliğine sahip ağ kullanın.

İstemci kaynakları performans sorununa neden oluyorsa birini veya her ikisini performansını artırmak için şunları yapın:

- En son uzak masaüstü istemcisini yükleyin.
- Belleği arttırın ve işlem kaynaklarını istemci makine üzerinde.

> [!NOTE]
> Kaynak Frames/Second sayaç şu anda desteklemiyoruz. Şimdilik, kaynak Frames/Second sayaç her zaman 0 olarak ayarlanır.

## <a name="next-steps"></a>Sonraki adımlar

- GPU için iyileştirilmiş bir Azure sanal makine oluşturmak için bkz: [grafik işlem birimi (GPU) hızlandırma Windows sanal masaüstü Önizleme ortamı için yapılandırma](https://docs.microsoft.com/azure/virtual-desktop/configure-vm-gpu).
- Sorun giderme ve yükseltme parçaları genel bakış için bkz. [genel bakış, geri bildirim ve destek sorunlarını giderme](https://docs.microsoft.com/azure/virtual-desktop/troubleshoot-set-up-overview).
- Önizleme hizmeti hakkında daha fazla bilgi için bkz: [Windows Desktop Önizleme ortamı](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
