---
title: Azure IoT Edge nedir | Microsoft Docs
description: Azure IoT Edge hizmetine genel bakış
author: kgremban
manager: philmea
ms.reviewer: chipalost
ms.service: iot-edge
services: iot-edge
ms.topic: overview
ms.date: 02/25/2019
ms.author: kgremban
ms.custom: mvc
ms.openlocfilehash: 2d1e4ce719311a28fb4b2075864f8f411cd2713d
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56877463"
---
# <a name="what-is-azure-iot-edge"></a>Azure IoT Edge nedir?

Azure IoT Edge bulut analizini ve özel iş mantığını cihazlara taşıyarak kuruluşunuzun veri yönetimi yerine iş öngörülerine odaklanmasını sağlar. IOT yazılımınızı yapılandırarak, bunu standart kapsayıcılar ve tümünü buluttan izleyerek üzerinden cihazlara dağıtın.

>[!NOTE]
>Azure IoT Edge, IoT Hub’ın ücretsiz ve standart katmanında kullanılabilir. Ücretsiz katman yalnızca test etme ve değerlendirme içindir. Temel ve standart katmanlar hakkında daha fazla bilgi için bkz. [Doğru IoT Hub katmanını seçme](../iot-hub/iot-hub-scaling.md).

Analizler IoT çözümlerinin iş değerini artırır, aman tüm analizlerin bulutta olması gerekmez. Bir cihazın acil durumlara mümkün olduğunca hızlı tepki vermesini istiyorsanız, cihazın kendisinde anomali algılama gerçekleştirebilirsiniz. Benzer şekilde, bant genişliği maliyetlerini azaltmak ve terabaytlarca ham veri aktarımını önlemek istiyorsanız, veri temizleme ve toplama işlemlerini yerel olarak yapabilirsiniz. Daha sonra ınsights analiz işlenmek üzere buluta gönderin. 

Azure IoT Edge üç bileşenden oluşur:
* IOT Edge modülleri, Azure hizmetlerini, üçüncü taraf hizmetleri veya kendi kodunuzu çalıştıran kapsayıcılardır. Modüller, IOT Edge cihazlarına dağıtılır ve bu cihazlarda yerel olarak yürütün. 
* IoT Edge çalışma zamanı her IoT Edge cihazında çalıştırılır ve her cihaza dağıtılan modülleri yönetir. 
* Bulut tabanlı bir arabirim, IoT Edge cihazlarını uzaktan izlemenize ve yönetmenize olanak tanır.

## <a name="iot-edge-modules"></a>IoT Edge modülleri

IOT Edge modülleri olarak iş mantığınızı uçta çalıştıran Docker uyumlu kapsayıcılar olarak uygulanan yürütme birimleridir. Birbiriyle iletişim kuracak birden çok modül yapılandırabilir ve böylelikle veri işleme için bir işlem hattı oluşturabilirsiniz. Çevrimdışı ve uçta öngörüler sağlayan kendi modüllerinizi geliştirebilir veya bazı Azure hizmetlerini modüller halinde paketleyebilirsiniz. 

### <a name="artificial-intelligence-on-the-edge"></a>Uç cihazlarında yapay zeka

Azure IOT Edge, şirket içinde yazmadan karmaşık olay işleme, makine öğrenme, resim tanıma ve diğer değerli yapay ZEKA dağıtmanıza olanak tanır. Azure işlevleri, Azure Stream Analytics gibi Azure Hizmetleri ve Azure Machine Learning tüm şirket içi Azure IOT Edge üzerinden çalıştırılabilir, ancak Azure işlevleriyle sınırlı değildir. Herkes AI modülleri oluşturabilir ve bunları topluluğun kullanımına Azure Marketi aracılığıyla sunma. 

### <a name="bring-your-own-code"></a>Kendi kodunuzu getirin

Cihazlarınıza kendi kodunuzu dağıtmak istediğinizde, Azure IoT Edge bunu da destekler. Azure IoT Edge, diğer Azure IoT hizmetleriyle alnı programlama modeline sahiptir. Aynı kod hem cihazda hem de bulutta çalıştırılabilir. Azure IoT Edge hem Linux'ı hem de Windows'u desteklediğinden, kendi seçtiğiniz platform için kod yazabilirsiniz. Bu Java, .NET Core 2.0, Node.js, C ve Python destekler, dolayısıyla Geliştiricileriniz zaten bildikleri ve var olan iş mantığındaki kullanan bir dilde kod yazabilirsiniz.

## <a name="iot-edge-runtime"></a>IoT Edge çalışma zamanı

Azure IoT Edge çalışma zamanı, IoT Edge cihazlarında özel mantığa ve bulut mantığına olanak tanır. IoT Edge cihazında durur; yönetim ve iletişim işlemlerini gerçekleştirir. Çalışma zamanı çeşitli işlevler gerçekleştirir:

* Yükleme ve cihazda iş yüklerini güncelleştirin.
* Cihazda Azure IOT Edge güvenlik standartlarını korur.
* IOT Edge modüllerinin her zaman çalıştığından emin olun.
* Bulutta Uzaktan izleme için modül durumunu rapor.
* Aşağı Akış yaprak cihazlarıyla IOT Edge cihazı arasındaki, bir IOT Edge cihazında modüller arasında ve bulut ile IOT Edge cihazı arasındaki iletişimi yönetin.

![IoT Edge çalışma zamanı öngörüleri ve raporları IoT Hub'ına gönderir](./media/about-iot-edge/runtime.png)

Azure IOT Edge cihazı nasıl kullandığınıza size bağlıdır. Çalışma zamanı, genellikle bu dağıtım modeli seçeneklerden yalnızca biridir ancak diğer hangi verileri toplar ve işler cihazlar, şirket içinde ağ geçitlerine AI dağıtmak için kullanılır. Yaprak cihazlar da, bir ağ geçidine mi yoksa doğrudan buluta mı bağlı olduklarına bakılmaksızın Azure IoT Edge cihazları olabilir.

Azure IoT Edge çalışma zamanı çok geniş bir IoT cihazları kümesinde çalıştırılarak çalışma zamanının çok çeşitli yollarla kullanılabilmesi sağlanır. Bu, hem Linux hem de Windows işletim sistemlerini destekler ve donanım ayrıntılarını çıkarır. Bir cihaz, kadar veri işlenmiyor veya kaynak kullanımı yoğun iş yüklerini çalıştırmak için bir industrialized sunucusu kullanmak daha küçük bir Raspberry Pi 3'ten kullanın.

## <a name="iot-edge-cloud-interface"></a>IoT Edge bulut arabirimi

Kurumsal cihazlarda yazılım yaşam döngüsünü yönetmek karmaşık bir işlemdir. Milyonlarca heterojen IoT cihazında yazılım yaşam döngüsünün yönetimi daha da zordur. Belirli bir cihaz türü için iş yüklerinin oluşturulması ve yapılandırılması, çözümünüzdeki milyonlara varan cihaza dağıtılması ve hatalı davranan cihazları yakalamak için izlenmesi gerekir. Bu cihazlar tek tek cihazlar temelinde yapılamaz, belirli bir ölçekte yapılmalıdır.

Azure IoT Edge, Azure IoT çözüm hızlandırıcıları ile rahatça tümleştirildiğinden, çözümünüzün gereksinimlerini karşılayacak tek bir denetim düzlemi sağlar. Bulut hizmetleri sağlar:

* Belirli bir tür cihaz üzerinde çalıştırılacak bir iş yükü oluşturabilir ve yapılandırılabilir.
* İş yükünü bir dizi cihaza gönderebilir.
* Sahadaki cihazlarda çalıştırılan iş yüklerini izleyebilir.

![Cihaz telemetrisi ve eylemleri bulutla eşgüdümlüdür](./media/about-iot-edge/cloud-interface.png)

## <a name="next-steps"></a>Sonraki adımlar

[Bir sanal cihaza IoT Edge dağıtımı yaparak](quickstart.md) bu kavramları deneyin.