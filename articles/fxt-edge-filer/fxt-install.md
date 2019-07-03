---
title: Bir Azure FXT Edge dosyalayıcı fiziksel cihaz yükleme Öğreticisi | Microsoft Docs
description: Nasıl Cihazınızı kutusundan çıkarma, rafa ve Microsoft Azure FXT Edge dosyalayıcı karma depolama önbelleği fiziksel cihaz bileşeninin bağlama
services: ''
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 07/01/2019
ms.author: v-erkell
ms.openlocfilehash: ed9eca88e5ccc386b25acb95fa729a3cfb95cbd0
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543401"
---
# <a name="tutorial-install-azure-fxt-edge-filer"></a>Öğretici: Azure FXT Edge dosyalayıcı yükleyin 

Bu öğreticide, Azure FXT Edge dosyalayıcı karma depolama önbelleği için bir donanım düğüm yüklemeyi açıklar. Bir Azure FXT Edge dosyalayıcı kümesi oluşturmak için en az üç donanımı düğümlerine yüklemeniz gerekir.

Yükleme yordamını açılmasını ve cihazı bağlama ve kablo yönetim arm (CMA) ve ön Pano ekleme raf içerir. Ayrı bir öğretici düğmelere ağ kablosu ve bağlanan power açıklar. 

Bir Azure FXT Edge dosyalayıcı düğümüne yüklemek için yaklaşık bir saat sürer. 

Bu öğretici, bu kurulum adımları içerir: 

> [!div class="checklist"]
> * Cihazı kutusundan çıkarma
> * Bir raf üstü cihazı bağlayın
> * (İsteğe bağlı) ön Pano yükleme

## <a name="installation-prerequisites"></a>Yükleme önkoşulları 

Başlamadan önce veri merkezi ve kullanacağınız raf bu özelliklere sahip olduğundan emin olun:

* Burada cihaz bağlamak istediğiniz bir kullanılabilir 1U yuvada rafa.
* AC güç ve soğutma Azure FXT Edge dosyalayıcı ihtiyaçlarını karşılayan sistemler. (Okuma [gücü ve sıcaklık belirtimleri](fxt-specs.md#power-and-thermal-specifications) için planlama ve yükleme boyutlandırma yardımcı olur.)  

  > [!NOTE] 
  > Güç Dağıtım birimleri iki yedek güç kaynağı (PSUs) birim tam olarak yararlanmak için iki farklı dal devreler üzerinde AC gücü eklerken kullanın. Okuma [power kabloları bağlayın](fxt-network-power.md#connect-power-cables) Ayrıntılar için.  

## <a name="unpack-the-hardware-node"></a>Donanım düğüm paketinden çıkarma 

Her Azure FXT Edge dosyalayıcı düğümü tek bir kutuda gönderilir. Bir cihazın ambalajını açmak için aşağıdaki adımları tamamlayın.

1. Kutuyu düz ve sabit bir yüzeye yerleştirin.

2. Kutuda ve ambalajda ezik, kesik, su hasarı veya gözle görülür herhangi bir hasar olup olmadığını kontrol edin. Paketleme ve kutusu ciddi zarar, açmayın. Cihazın iyi durumda olup olmadığının değerlendirilmesi için Microsoft Desteği ile iletişim kurun.

3. Kutuyu açın. Aşağıdaki öğeler içerdiğinden emin olun:
   * Tek tek kutu FXT cihaz
   * İki güç kablosu
   * Bir ön Pano ve anahtarı
   * Bir parmaklık Seti derleme
   * Bir kablo yönetim arm (CMA)
   * CMA yükleme yönergeleri Kitapçığı
   * Raf yükleme yönergeleri Kitapçığı
   * Kitapçığı güvenliği, çevre ve yasal bilgiler

Listelenen öğelerin tümünü almadıysanız, aygıt satıcısı için desteğe başvurun. 

Cihaz yükleme veya üzerinde açmadan önce odası olarak aynı sıcaklık ulaşmak için yeterli bir süre beklendiğinden olduğundan emin olun. Cihaz herhangi bir kısmını buharlaşmayı fark ederseniz, yüklemeden önce en az 24 saat bekleyin.

Sonraki adım etmektir Cihazınızı bağlayın.

## <a name="rack-the-device"></a>Cihazı rafa yerleştirme

Azure FXT Edge dosyalayıcı cihaz standart 19 inç rafa yüklü olması gerekir. 

Azure FXT Edge dosyalayıcı karma depolama önbelleği üç veya daha fazla Azure FXT Edge dosyalayıcı aygıtların oluşur. Sisteminizin bir parçası olan her bir cihaz için raf yükleme adımlarını yineleyin. 

### <a name="rack-install-prerequisites"></a>Raf Önkoşulları Yükleme

* Başlamadan önce Cihazınızda setleriyle güvenliği, çevre ve yasal bilgiler Kitapçığı güvenlik yönergeleri okuyun.

  > [!NOTE]
  > Her zaman iki kişinin bir rafa yüklediğinizde veya rafa kaldırmak da dahil olmak üzere bir düğümü kaldırma olduğunda kullanın. 

* İle donanım rafı kullanılan parmaklık yükleme türünü belirleyin. 
  * Kare veya yuvarlak delik raflar'ın eklentisini alet parmaklık yönergeleri izleyin.
  * İş parçacıklı delik raflar için tooled parmaklık yönergeleri izleyin. 
  
    Yapılandırma bağlama tooled parmaklık için sekiz Vida, 10-32, 12-24 türü, M5 veya M6 sağlamanız gerekir. Baş Vida çapı 10 mm'lik küçüktür (0,4") olmalıdır.

### <a name="identify-the-rail-kit-contents"></a>Parmaklık Seti içeriği tanımlayın

Parmaklık Seti bütünleştirilmiş kod yükleme için bileşenleri bulun:

1. İki A7 Dell ReadyRails parmaklık derlemeleri (1) kayan II
1. İki bağlama ve (2) straps döngü

![Parmaklık Seti İçindekiler numaralı çizim](media/fxt-install/identify-rail-kit-contents-400.png)

### <a name="rail-assembly---tool-less-rails-square-hole-or-round-hole-racks"></a>Parmaklık derleme - alet rails (kare delik veya yuvarlak delik raflar)

Ek bileşenini kare veya yuvarlak delik Raflarla için rails yüklemek ve derlemek için bu yordamı izleyin. 

1. Etiketli sol ve sağ parmaklık son parçaları konumlandırma **ön** içe karşılıklı. Bu dikey raf çıkıntıları ön tarafındaki boşluklar lisans şekilde son birer konumlandırın. (1)

2. Her alt ve üst boşluklar son parçası, bağlamak istediğiniz alanı rafa, hizalayın. (2)

3. Parmaklık arka ucunu kadar tam lisans dikey raf Flanş ve Mandal tıklama yerine etkileşim kurun. Getirin ve ön uç parça dikey raf Flanş üzerinde bilgisayar lisansı için bu adımları yineleyin. (3)

> [!TIP]
> Rails kaldırmak için Mandal yayın düğmesini uç parça Orta (4) çekme ve her parmaklık unseat.

![Numaralı adımlara alet rails, kaldırma ve yükleme diyagramı](media/fxt-install/installing-removing-tool-less-rails-400.png)

### <a name="rail-assembly---tooled-rails-threaded-hole-racks"></a>Parmaklık derleme - tooled rails (zincir delik raflar)

İş parçacıklı boşluklarını Raflarla için rails yüklemek ve derlemek için bu yordamı izleyin.

1. PIN ile düz Eğimli tornavida montaj arka ve ön kaldırın. (1)
1. Çekme ve bağlama noktalarından kaldırılacak parmaklık Mandal yarı mamullerden döndürün. (2)
1. Sol ve sağda iki Vida çiftlerini kullanarak ön dikey raf çıkıntıları için rails bağlama ekleyin. (3)
1. Köşeli ayraçlar karşı arka dikey raf çıkıntıları iletmek ve iki Vida çiftlerini kullanarak bunları eklemek sol ve sağ arka kaydırın. (4)

![Numaralı adımlara tooled rails, kaldırma ve yükleme diyagramı](media/fxt-install/installing-removing-tooled-rails-400.png)

### <a name="install-the-system-in-the-rack"></a>Rafa sistemini yükleyin

Azure FXT Edge dosyalayıcı cihazı rafa bağlamak için aşağıdaki adımları izleyin.

1. Bunlar kilitlemek kadar iç slayt rails rafa dışında çekin. (1)
1. Her cihaz tarafında arka parmaklık standoff bulun ve bunları arka J yuvaları slayt derlemeler halinde düşürün. (2) 
1. Parmaklık standoffs J yuvalarda yerleştirildiğinden kadar cihaz Aşağı Döndür. (3)
1. Kilit levers yere tıklayana kadar cihaza içe gönderecektir.
1. Slayt yayın kilit düğmeleri hem rails (4) tuşuna basın ve cihazı rafa kaydırın.

![Sistem bir raf üstü diyagramda numaralı adımlara yükleyin.](media/fxt-install/installing-system-rack-400.png)

### <a name="remove-the-system-from-the-rack"></a>Sistem rafa kaldırın

Cihaz rafa kaldırmak için bu yordamı izleyin. 

1. Kilit levers iç rails (1) tarafında bulun.
2. Her seviyesini yayın konumuna kadar (2) döndürerek kilidini açın.
3. Sistem tarafının metz'in kavrayın ve parmaklık standoffs J yuvaları önündeki kadar ileriye doğru çekin. Sistem yedekleme ve raf uzağa lift- and -düzeyi surface (3) yerleştirin.

![Resmi bir sistemi numaralı adımlara raf kaldırılıyor](media/fxt-install/removing-system-rack-400.png)

### <a name="engage-the-slam-latch"></a>Slam Mandal etkileşim kurun

1. Öne bakan, sistem her iki tarafında slam Mandal (1) bulun.
2. Mandal otomatik olarak sistemi rafa gönderildiğinde gibi etkileşim kurun. 

Sistem kaldırırken Mandal serbest bırakmak için (2) çekin.

İsteğe bağlı sabit bağlama Vida Sevk irsaliyesi için veya diğer kararsız ortamlarda raf sisteme güvenli hale getirmek için sağlanır. Her Mandal altında Sarmal bulun ve bunları #2 ile sıkılaştıran Phillips tornavida (3).

![Numaralandırılmış çizimi ilgi çekici ve slam mandalı bırakılıyor](media/fxt-install/engaging-releasing-slam-latch-400.png)

### <a name="install-the-cable-management-arm"></a>Kablo yönetim kolu yükleyin 

İsteğe bağlı kablo yönetim arm (CMA) ile FXT Edge dosyalayıcı sağlanır. Paketi yüklemek belgelerdeki yönergeleri sağlanır. 

1. Paketten ve kablo yönetim arm Seti bileşenlerinin tanımlayın:
   * CMA Tepsisi (1)
   * CMA (2)
   * (3) naylon kablo KRAVAT sarmalar
   * CMA eki köşeli ayraçlar (4)
   * Durum göstergesi kablosu (5) 

   > [!TIP] 
   > Sevkiyat raftaki CMA güvenliğini sağlamak için sepetleri hem Tepsisi etrafında KRAVAT sarmalayan döngü ve sıkıca cinch. Bu şekilde CMA güvenli hale getirme sisteminiz kararsız ortamlarda güvenli.

   ![CMA bölümleri gösterimi](media/fxt-install/cma-kit-400.png)

2. CMA Tepsisi yükleyin.

   CMA Tepsisi desteği sağlar ve CMA için bir tutucu olarak görev yapar. 

   1. Hizalama ve her iki tarafındaki Tepsisi rails iç kenarlarında alıcı ayraç ile etkileşim kurun. 
   1. Tepsisi ileriye doğru yere tıklayana kadar gönderin. (1)
   1. Tepsisi kaldırmak için mandalı düğmeleri merkezine sığdırması ve alıcı köşeli ayraçlar (2) dışında Tepsisi çekme.

   ![CMA Tepsisi yükleme resmi](media/fxt-install/cma-tray-install-400.png)

3. CMA eki köşeli ayraçlar yükleyin. 

   > [!NOTE]
   >
   > * Sağa veya sola rota kabloları sistemden nasıl istediğinize bağlı olarak parmaklık, bağlama CMA ekleyebilirsiniz. 
   > * Kolaylık olması için güç kaynağı (yan A) ters tarafında CMA bağlayın. B tarafında takılmışsa, dış güç kaynağı kaldırmak için CMA kesilmelidir. 
   > * Her zaman güç kaynakları önce Tepsisi kaldırın. 

   ![CMA köşeli ayraç yükleme resmi](media/fxt-install/cma-bracket-l-r-install-400.png)

   1. Uygun CMA ek köşeli ayraç (yan B veya yan A) CMA bağlamak istediğiniz tarafı için seçin.
   1. Karşılık gelen yan A veya B yan işaretleme slayt parmaklık arkasına CMA ek köşeli ayraç yükleyin.
   1. Köşeli ayraç üzerinde boşluklarını slayt parmaklık iğneleri ile hizalar. Köşeli ayraç Yerleştir kilitler kadar itin. 

4. CMA yükleyin.

   1. Mandal (1) devreye girene kadar sistem arkasına CMA ön ucundaki Mandal slayt derlemenin en içteki ayraç üzerinde uygun. 
   1. Bir Mandal en dıştaki ayraç ucundaki Mandal (2) devreye girene kadar sığdırır. 
   1. CMA kaldırmak için her iki tutma (3) iç ve dış Mandal housings üst kısmındaki CMA yayın düğmeleri tuşlarına basarak disengage.

   ![Ana CMA yükleme resmi](media/fxt-install/cma-install-400.png)

   CMA uzağa erişimi ve hizmet Sistem döndürülebilir. Hinged sonunda, (1) unseat için CMA Tepsisi uzağa taşıyın. Tepsisinden unseated olduktan sonra (2) sistem uzağa CMA swing.

   ![Hizmet için açık CMA gösterimi Döndürülmüş](media/fxt-install/cma-swing-over-tray-400.png)

## <a name="install-the-front-bezel-optional"></a>(İsteğe bağlı) ön Pano yükleme

Bu bölümde, yükleme ve kaldırma (ekran) Azure FXT Edge dosyalayıcı donanım için ön Pano açıklanmaktadır. 

Ön pano yüklemek için: 

1. Bulun ve çerçeve paketinde sağlanan kasa anahtarını kaldırın. 
1. Çerçeve önüne kasa ile uyumlu hale getirmek ve düğümün sağ tarafındaki rafa takma Flanş üzerinde boşluklarını çerçeve sağ tarafında PIN'ler yerleştirin. 
1. Kasa üzerine çerçeve sol ucundaki sığdırır. Sol taraftaki düğmeyi yere tıklayana kadar çerçeve tuşuna basın.
1. Kasa anahtarıyla kilitleyin.

Ön pano kaldırmak için: 
1. Çerçeve kasa anahtarını kullanarak kilidini açın.
1. Sol tarafta yayın düğmesine basın ve kasa uzağa çerçeve sol ucundaki çekme.
1. Sağ ucuna tutulabilir ve çerçeve kaldırın.
   
   ![Çerçeve ve sol tarafındaki dışa doğru çekerek kaldırma solundaki yayın düğmesini gösteren resim](media/fxt-install/remove-bezel-edited-600.png)

## <a name="next-steps"></a>Sonraki adımlar

Açılmış ve cihaz yerleştirdiğinizi sonra ağ kablosu ekleme ve bir Azure FXT Edge dosyalayıcı AC gücü bağlanarak kurulum devam edin.

> [!div class="nextstepaction"]
> [Ağ bağlantı noktalarının kablo ve güç kaynağı](fxt-network-power.md)