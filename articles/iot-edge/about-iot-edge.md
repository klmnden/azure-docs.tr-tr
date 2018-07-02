---
title: Azure IoT Edge nedir | Microsoft Docs
description: Azure IoT Edge hizmetine genel bakış
author: kgremban
manager: timlt
ms.reviewer: chipalost
ms.service: iot-edge
services: iot-edge
ms.topic: overview
ms.date: 06/12/2018
ms.author: kgremban
ms.custom: mvc
ms.openlocfilehash: 6e3571fb54f12ef3bb5519f572b8af5bf9247e7d
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033093"
---
# <a name="what-is-azure-iot-edge"></a>Azure IoT Edge nedir?

Azure IoT Edge bulut analizini ve özel iş mantığını cihazlara taşıyarak kuruluşunuzun veri yönetimi yerine iş öngörülerine odaklanmasını sağlar. IoT yazılımınızı yapılandırarak, bunu standart kapsayıcılar üzerinden cihazlara dağıtarak ve tümünü buluttan izleyerek çözümünüzün gerçek anlamda ölçeklendirilmesine olanak sağlayın.

>[!NOTE]
>Azure IoT Edge, IoT Hub’ın ücretsiz ve standart katmanında kullanılabilir. Ücretsiz katman yalnızca test etme ve değerlendirme içindir. Temel ve standart katmanlar hakkında daha fazla bilgi için bkz. [Doğru IoT Hub katmanını seçme](../iot-hub/iot-hub-scaling.md).

Analizler IoT çözümlerinin iş değerini artırır, aman tüm analizlerin bulutta olması gerekmez. Bir cihazın acil durumlara mümkün olduğunca hızlı tepki vermesini istiyorsanız, cihazın kendisinde anomali algılama gerçekleştirebilirsiniz. Benzer şekilde, bant genişliği maliyetlerini azaltmak ve terabaytlarca ham veri aktarımını önlemek istiyorsanız, veri temizleme ve toplama işlemlerini yerel olarak yapabilirsiniz. Sonra da öngörüleri buluta gönderirsiniz. 

Azure IoT Edge üç bileşenden oluşur:
* IoT Edge modülleri, Azure hizmetlerini, üçüncü taraf hizmetleri ve sizin kendi kodunuzu çalıştıran kapsayıcılardır. Bunlar IoT Edge cihazlarına dağıtılır ve bu cihazlarda yerel olarak yürütülür. 
* IoT Edge çalışma zamanı her IoT Edge cihazında çalıştırılır ve her cihaza dağıtılan modülleri yönetir. 
* Bulut tabanlı bir arabirim, IoT Edge cihazlarını uzaktan izlemenize ve yönetmenize olanak tanır.

## <a name="iot-edge-modules"></a>IoT Edge modülleri

IoT Edge modülleri, şu anda Docker uyumlu kapsayıcılar olarak uygulanan ve iş mantığınızı uçta çalıştıran yürütme birimleridir. Birbiriyle iletişim kuracak birden çok modül yapılandırabilir ve böylelikle veri işleme için bir işlem hattı oluşturabilirsiniz. Çevrimdışı ve uçta öngörüler sağlayan kendi modüllerinizi geliştirebilir veya bazı Azure hizmetlerini modüller halinde paketleyebilirsiniz. 

### <a name="artificial-intelligence-on-the-edge"></a>Uç cihazlarında yapay zeka

Azure IoT Edge kendiniz yazmadan karmaşık olay işleme, makine öğrenme, resim tanıma ve diğer değerli yapay zeka (AI) özelliklerini dağıtmanızı sağlar. Azure İşlevleri, Azure Stream Analytics ve Azure Machine Learning gibi Azure işlevleri Azure IoT Edge üzerinden şirket içinde çalıştırılabilir; bununla birlikte Azure işlevleriyle sınırlı değilsiniz. Herkes AI modülleri oluşturabilir ve bunları topluluğun kullanımına sunabilir. 

### <a name="bring-your-own-code"></a>Kendi kodunuzu getirin

Cihazlarınıza kendi kodunuzu dağıtmak istediğinizde, Azure IoT Edge bunu da destekler. Azure IoT Edge, diğer Azure IoT hizmetleriyle alnı programlama modeline sahiptir. Aynı kod hem cihazda hem de bulutta çalıştırılabilir. Azure IoT Edge hem Linux'ı hem de Windows'u desteklediğinden, kendi seçtiğiniz platform için kod yazabilirsiniz. Java, .NET Core 2.0, Node.js, C ve Python'ı destekler; dolayısıyla geliştiricileriniz zaten bildikleri bir dilde kod yazabilir ve sıfırdan yazmak zorunda kalmadan mevcut iş mantığını kullanabilir.

## <a name="iot-edge-runtime"></a>IoT Edge çalışma zamanı

Azure IoT Edge çalışma zamanı, IoT Edge cihazlarında özel mantığa ve bulut mantığına olanak tanır. IoT Edge cihazında durur; yönetim ve iletişim işlemlerini gerçekleştirir. Çalışma zamanı çeşitli işlevler gerçekleştirir:

* Cihazda iş yüklerini yükler ve güncelleştirir.
* Cihazda Azure IoT Edge güvenlik standartlarını korur.
* IoT Edge modüllerinin her zaman çalıştırılmasını güvence altına alır.
* Uzaktan izleme için modül durumunu buluta bildirir.
* Aşağı akış yaprak cihazlarıyla IoT Edge cihazı arasındaki iletişimi kolaylaştırır.
* IoT Edge cihazında bulunan modüller arasındaki iletişimi kolaylaştırır.
* IoT Edge cihazıyla bulut arasındaki iletişimi kolaylaştırır.

![IoT Edge çalışma zamanı öngörüleri ve raporları IoT Hub'ına gönderir][1]

Azure IoT Edge cihazını nasıl kullanacağınız tamamen size bağlıdır. Çalışma zamanı çoğunlukla ağ geçitlerine AI dağıtımı için kullanılır ve bu ağ geçitleri diğer birden çok şirket içi cihazından verileri toplar ve işler; ama bu, seçeneklerden yalnızca biridir. Yaprak cihazlar da, bir ağ geçidine mi yoksa doğrudan buluta mı bağlı olduklarına bakılmaksızın Azure IoT Edge cihazları olabilir.

Azure IoT Edge çalışma zamanı çok geniş bir IoT cihazları kümesinde çalıştırılarak çalışma zamanının çok çeşitli yollarla kullanılabilmesi sağlanır. Hem Linux hem de Windows işletim sistemlerini destekler ve donanım ayrıntılarını çıkarır. Çok fazla veri işlemiyorsanız Raspberry Pi 3'ten daha küçük bir cihaz kullanın veya yoğun kaynak içeren iş yüklerini çalıştırmak için ölçeğini endüstri sunucusuna artırın.

## <a name="iot-edge-cloud-interface"></a>IoT Edge bulut arabirimi

Kurumsal cihazlarda yazılım yaşam döngüsünü yönetmek karmaşık bir işlemdir. Milyonlarca heterojen IoT cihazında yazılım yaşam döngüsünün yönetimi daha da zordur. Belirli bir cihaz türü için iş yüklerinin oluşturulması ve yapılandırılması, çözümünüzdeki milyonlara varan cihaza dağıtılması ve hatalı davranan cihazları yakalamak için izlenmesi gerekir. Bu cihazlar tek tek cihazlar temelinde yapılamaz, belirli bir ölçekte yapılmalıdır.

Azure IoT Edge, Azure IoT çözüm hızlandırıcıları ile rahatça tümleştirildiğinden, çözümünüzün gereksinimlerini karşılayacak tek bir denetim düzlemi sağlar. Bulut hizmetleriyle kullanıcılar:

* Belirli bir tür cihaz üzerinde çalıştırılacak bir iş yükü oluşturabilir ve yapılandırılabilir.
* İş yükünü bir dizi cihaza gönderebilir.
* Sahadaki cihazlarda çalıştırılan iş yüklerini izleyebilir.

![Telemetri, öngörüler ve cihaz eylemleri bulutla eşgüdümlüdür][2]

## <a name="next-steps"></a>Sonraki adımlar

[Bir sanal cihaza IoT Edge dağıtımı yaparak][lnk-quickstart] bu kavramları deneyin.

<!-- Images -->
[1]: ./media/about-iot-edge/runtime.png
[2]: ./media/about-iot-edge/cloud-interface.png

<!-- Links -->
[lnk-quickstart]: quickstart.md
