---
title: Hızlı Başlangıç için Microsoft Azure veri kutusu ağır | Microsoft Docs
description: Hızlı bir şekilde, Azure veri kutusu ağır Azure portalında dağıtmayı öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: alkohli
ms.openlocfilehash: 3467b25c085fb86d4aed3918d5446d118f76ffb8
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446729"
---
# <a name="quickstart-deploy-azure-data-box-heavy-using-the-azure-portal"></a>Hızlı Başlangıç: Azure veri kutusu Azure portalını kullanarak ağır dağıtma

Bu hızlı başlangıçta, Azure veri kutusu Azure portalını kullanarak ağır dağıtmayı açıklar. Kablo, yapılandırma ve Azure'a yükler, verileri veri kutusu ağır kopyalama adımları içerir. Hızlı başlangıç Azure portalında ve cihazın yerel web kullanıcı arabiriminde gerçekleştirilir.

Ayrıntılı adım adım dağıtım ve izleme yönergeleri için Git [Öğreticisi: Azure veri kutusu ağır sırası](data-box-heavy-deploy-ordered.md)

## <a name="prerequisites"></a>Önkoşullar

Cihaz dağıtmadan önce yükleme site, Data Box hizmeti ve cihaz için aşağıdaki yapılandırma önkoşulları tamamlayın.

### <a name="for-installation-site"></a>Yüklemeyi site için

Başlamadan önce aşağıdakilerden emin olun:

- Cihaz, entryways uygun olamaz. Cihaz boyutlar: genişliği: 26" uzunluğu: 48" height: 28”.
- Bir zemin katından giriş yaptığınızı dışında üzerinde yüklemeyi planlıyorsanız bir Asansör veya bir yöntemi aracılığıyla cihaza erişimi vardır.
- Cihaz işlemek için iki kişinin var. Cihaz yaklaşık 500 ~ lbs hafiftir. tekerlekleri üzerinde içerir.
- Veri merkezinde yakınlık bu ayak izine sahip bir cihaz barındırabilecek bir kullanılabilir ağ bağlantısı ile düz bir siteniz.

### <a name="for-service"></a>Hizmet için

Başlamadan önce aşağıdakilerden emin olun:

- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.
- Data Box hizmeti için kullandığınız abonelik [Microsoft Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), [bulut çözümü sağlayıcısı (CSP)](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview), veya [Microsoft Azure Sponsorluğu](https://azure.microsoft.com/offers/ms-azr-0036p/).
- Veri kutusu ağır bir sipariş oluşturmak için abonelik sahibi veya katkıda bulunan erişimi var.

### <a name="for-device"></a>Cihaz için

Başlamadan önce aşağıdakilerden emin olun:

- Gözden geçirdim [güvenlik yönergeleri için veri kutusu ağır](data-box-safety.md).
- Veri Merkezi ağına bir konak bilgisayar var. Veri kutusu ağır verileri bu bilgisayardan kopyalayın. Ana bilgisayarınızda çalıştırmalısınız bir [işletim sistemi desteklenen](data-box-heavy-system-requirements.md).
- Sahip olduğunuz bir cihaz için yerel kullanıcı Arabirimi bağlanın ve RJ-45 kablosu ile dizüstü bilgisayar. Dizüstü bilgisayar, her cihazın düğümü kez yapılandırmak için kullanın.
- Veri merkezinizi yüksek hızlı ağ var ve en az bir sahip 10 GbE bağlantı.
- Bir 40 GB/sn kablo veya cihaz düğüm başına 10 GB/sn kablo gerekir. Mellanox MCX314A BCCT ile uyumlu olan kabloları ağ arabirimi seçin:
    - 40 GB/sn kablosunun, kablo cihazı sonuna QSFP + olması gerekir.
    - 10 Gbps kablosunun, içine bir QSFP + ile SFP + bağdaştırıcısı (veya QSA bağdaştırıcısı) aygıta takılan ucu için bir son 10 G anahtarda takılan SFP + kablo gerekir.
- Güç kabloları cihazın arkasına bir tepsisinde dahil edilir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="order"></a>Sipariş verme

Bu adım yaklaşık 5 dakika sürer.

1. Azure portalında yeni bir Azure Data Box kaynağı oluşturun.
2. Bu hizmetin etkinleştirildiği mevcut aboneliği seçin ve aktarım türünü **İçeri aktarma** olarak belirleyin. Verilerin bulunduğu **Kaynak ülkeyi** ve veri aktarımı için **Azure hedef bölgesini** seçin.
3. Seçin **veri kutusu ağır**. 770 TB kullanılabilir kapasite üst sınırı olan ve daha büyük boyutlu veriler için birden çok sipariş oluşturabilirsiniz.
4. Sipariş ayrıntılarını ve sevkiyat bilgilerini girin. Hizmet bölgenizde kullanılabilir durumdaysa bildirim e-posta adreslerini girin, özeti gözden geçirin ve siparişi oluşturun.

Sipariş oluşturulduktan sonra cihaz gönderilmek üzere hazırlanır.

## <a name="cable-for-power"></a>Güç kablosu

Bu adım, yaklaşık 5 dakika sürer.

Veri kutusu ağır aldığınızda, cihazınızın güç bağlantısını yapın ve cihazda etkinleştirmek için aşağıdaki adımları uygulayın.

1. Cihazla oynandığına veya cihazın hasarlı olduğuna ilişkin bir kanıt varsa, devam etmeyin. Size yeni bir cihaz gönderilmesi için Microsoft Desteği'ne başvurun.
2. Cihaz yükleme sitesine nasıl taşıyacağınızı ve arka tekerlekleri kilitleyin.
3. Cihazın arkasına güç kaynakları tüm dört güç kabloları bağlayın.
4. Güç düğmelerini ön düzlemi cihaz düğümler üzerinde etkinleştirmek için kullanın.

## <a name="cable-first-node-for-network"></a>Ağ kablosu ilk düğümü

Bu adımı tamamlamak için yaklaşık 10-15 dakika sürer.

1. Konak bilgisayarınızı cihazdaki yönetim bağlantı noktasına (MGMT) bağlamak için bir RJ 45 CAT 6 ağ kablosu kullanın.
2. En az bir 40 GB/sn bağlanmak için (tercih edilen 1 GB/sn) Twinax QSFP + siyah Bakır kablo kullanın veri 1 veya veriler için veri 2 ağ arabirimi. 10 GB/sn anahtarını kullanarak, bir Twinax SFP + siyah Bakır kablo ile bir QSFP + SFP + bağdaştırıcısına (QSA bağdaştırıcısı) veriler için 40 GB/sn ağ arabirimini bağlamak için kullanın.
3. Cihazın kablolarını aşağıda gösterildiği gibi takın.  

    ![Veri kutusu kablolu ağır](media/data-box-heavy-quickstart-portal/data-box-heavy-ports-cabled.png)  

## <a name="configure-first-node"></a>İlk düğüm yapılandırma

Bu adımın tamamlanması yaklaşık 5-7 dakika sürer.

1. Cihaz parolasını almak için [Azure portalında](https://portal.azure.com) **Genel > Cihaz ayrıntıları**'na gidin. Her iki cihaz düğümleri için aynı parolanın kullanılıp.
2. Veri kutusu ağır bağlanmak için kullanmakta olduğunuz bilgisayardaki Ethernet bağdaştırıcısını 192.168.100.5 ve 255.255.255.0 alt ağı bir statik IP adresi atayın. Cihazın yerel web kullanıcı arabirimine `https://192.168.100.10` adresinden erişin. Siz cihazı açtıktan sonra bağlantı kurulması 5 dakika kadar sürebilir.
3. Azure portalından alınan parolayı kullanarak oturum açın. Web sitesinin güvenlik sertifikasında sorun olduğunu belirten bir hata görürsünüz. Web sayfasına ilerlemek için tarayıcıya özel yönergeleri izleyin.
4. Varsayılan olarak, (MGMT hariç) arabirimleri için ağ ayarları DHCP yapılandırılır. Gerekirse, bu arabirimler statik olarak yapılandırıp bir IP adresi sağlayın.

## <a name="cable-and-configure-the-second-node"></a>İkinci düğümü yapılandırma ve kablolarını bağlama

Bu adımın tamamlanması yaklaşık 15-20 dakika sürer.

Kablo ve ikinci düğümü cihazda yapılandırmak için ilk düğüm için kullanılan adımları izleyin.  

## <a name="copy-data"></a>Veri kopyalama

Bu işlemi tamamlamak için geçen süre, veri boyutu ve ağ verilerin kopyalandığı hızına bağlıdır.
 
1. Her iki 40 GB/sn veri arabirimler paralel kullanarak her iki cihaz düğümlerine veri kopyalayın.

    - Bir Windows konak kullanarak kullanıyorsanız bir SMB uyumlu bir dosya kopyalama aracı gibi [Robocopy](https://technet.microsoft.com/library/ee851678.aspx).
    - NFS konağı için, `cp` veya `rsync` komutunu kullanarak verileri kopyalayın.
2. Yolu kullanarak cihaz paylaşımlarına bağlanmak:`\\<IP address of your device>\ShareName`. Paylaşım erişimi kimlik bilgilerini almak için şu adrese gidin **Bağlan & Kopyala** yerel Web kullanıcı Arabiriminde, veri kutusu ağır sayfası.
3. Paylaşım ve klasör adlarını ve veri açıklanan yönergeleri izlediğinizden emin olun [Azure depolama ve veri kutusu ağır hizmet sınırları](data-box-heavy-limits.md).

## <a name="prepare-to-ship"></a>Göndermeye hazırlama

Bu işlemi tamamlamak için gereken süre verilerinizin boyutuna göre değişir.

1. Herhangi bir hata olmadan veri kopyalama tamamlandığında, Git **göndermeye hazırlama** sayfasında yerel web kullanıcı Arabirimi ve gönderme hazırlığına başlayın.
2. Sonra **göndermeye hazırlama** olan düğümler üzerinde başarıyla tamamlandı, cihazın yerel Web kullanıcı arabirimini kapatın.

## <a name="ship-to-azure"></a>Azure'a gönderme

Bu işlemin tamamlanması yaklaşık 15-20 dakika sürer.

1. Kabloları kaldırın ve cihazın arkasına Tepsisi dönün.
2. Bir toplama bölgesel operatörünüz ile zamanlayın.
3. Ulaşın [veri kutusu işlemleri](mailto:DataBoxOps@microsoft.com) alma ile ilgili bilgilendirmek için ve iade Sevkiyat Etiketi alın.
4. İade gönderme etiketinin cihazın ön Temizle panelinde görünür olmalıdır.

## <a name="verify-data"></a>Verileri doğrulama

Bu işlemi tamamlamak için gereken süre verilerinizin boyutuna göre değişir.

1. Veri kutusu ağır cihaz Azure veri merkezi ağa bağlandığında, verileri Azure'a otomatik olarak yükler.
2. Veri kutusu hizmeti veri kopyalama Azure portalı üzerinden tamamlandığını bildirir.

    1. Hata günlüklerini kontrol ederek hata olup olmadığını kontrol edin ve gerekli eylemleri gerçekleştirin.
    2. Kaynaktan silmeden önce verilerinizin depolama hesaplarında olduğundan emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu adımın tamamlanması 2-3 dakika sürer.

- Sipariş işlenmeden önce Azure portalında veri kutusu ağır siparişi iptal edebilirsiniz. Siparişler işleme alındıktan sonra iptal edilemez. Sipariş, tamamlanma aşamasına gelene kadar ilerler. Siparişi iptal etmek için **Genel bakış**'a gidin ve komut çubuğundan **İptal**'e tıklayın.

- Siparişin durumu **Tamamlandı** veya **İptal edildi** olduğunda Azure portaldan silebilirsiniz. Siparişi silmek için **Genel bakış**'a gidin ve komut çubuğundan **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir veri kutusu verilerinizi Azure'a aktarmanız yardımcı olmak için ağır dağıttınız. Azure veri kutusu ağır yönetimi hakkında daha fazla bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Veri kutusu ağır yönetmek için Azure portalını kullanma](data-box-portal-admin.md)
