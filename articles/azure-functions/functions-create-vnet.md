---
title: Azure işlevleri, bir Azure sanal ağı ile tümleştirin
description: Bir işlev bir Azure sanal ağına bağlama gösteren adım adım öğretici
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: article
ms.date: 5/03/2019
ms.author: alkarche, glenga
ms.openlocfilehash: 55cce60ab3d1cda3cb870afd2f6214f917a04189
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063269"
---
# <a name="tutorial-integrate-functions-with-an-azure-virtual-network"></a>Öğretici: İşlevleri bir Azure sanal ağı ile tümleştirin.

Bu öğreticide bir Azure sanal ağındaki kaynaklara bağlanmak için Azure işlevleri kullanmayı gösterir. WordPress sanal ağda çalışan bir VM ve hem internet erişimi olan bir işlev oluşturmayı öğreneceksiniz.

> [!div class="checklist"]
> * Premium planında bir işlev uygulaması oluşturma
> * Bir WordPress sitesi, bir sanal ağda VM dağıtma
> * İşlev uygulaması sanal ağa bağlanma
> * WordPress kaynaklara erişmek için bir işlev proxy oluşturma
> * Sanal ağ içindeki bir WordPress dosyasından iste

> [!NOTE]  
> Bu öğreticide Premium planında bir işlev uygulaması oluşturur. Bu bir barındırma planı şu anda Önizleme aşamasındadır. Daha fazla bilgi için [Premium planı].

## <a name="topology"></a>Topoloji

Aşağıdaki diyagramda oluşturduğunuz çözüm mimarisi gösterilmektedir:

 ![Sanal ağ tümleştirmesi için kullanıcı Arabirimi](./media/functions-create-vnet/topology.png)

Premium planda çalışan işlevler Azure App VNet tümleştirme özelliğini içeren Service içinde web apps ile aynı barındırma özellikleri var. Sorun giderme ve Gelişmiş Yapılandırma gibi sanal ağ tümleştirmesi hakkında daha fazla bilgi için bkz: [uygulamanızı bir Azure sanal ağı ile tümleştirme](../app-service/web-sites-integrate-with-vnet.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, IP adresi ve alt ağ oluşturma anlamak önemlidir. İle başlayabilirsiniz [adresleme ve alt ağları temellerini kapsayan Bu makale](https://support.microsoft.com/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). Birçok çevrimiçi daha fazla makaleler ve videolar kullanılabilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-function-app-in-a-premium-plan"></a>Bir Premium planında bir işlev uygulaması oluşturma

İlk olarak, bir işlev uygulaması ile oluşturduğunuz [Premium planı]. Bu plan, sanal ağ tümleştirmesi desteklerken sunucusuz ölçek sağlar.

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]  

Sağ üst köşedeki Raptiye simgesini seçerek, işlev uygulaması panoya sabitleyebilirsiniz. Sabitleme, VM oluşturduktan sonra bu işlev uygulamasına döndürülecek kolaylaştırır.

## <a name="create-a-vm-inside-a-virtual-network"></a>Bir sanal ağ içinde bir VM oluşturma

Ardından, sanal ağ içinde çalıştırılan WordPress önceden yapılandırılmış bir VM oluşturun ([WordPress LEMP7 Max Performance](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure) Jetware tarafından). WordPress VM kolaylığı ve düşük maliyet nedeniyle kullanılır. REST API'leri, App Service ortamları ve diğer Azure Hizmetleri gibi bir sanal ağdaki herhangi bir kaynakla aynı senaryo çalışır. 

1. Portalında, **+ kaynak Oluştur** sol gezinti bölmesinde arama alan türü `WordPress LEMP7 Max Performance`, ve Enter tuşuna basın.

1. Seçin **Wordpress LEMP Max Performance** arama sonuçlarında. Bir yazılım planı seçin **CentOS için Wordpress LEMP Max Performance** olarak **yazılım planı** seçip **Oluştur**.

1. İçinde **Temelleri** sekmesinde, görüntünün altındaki tabloda belirtilen VM ayarlarını kullanın:

    ![Bir VM oluşturmak için sekmesinde temelleri](./media/functions-create-vnet/create-vm-1.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Abonelik** | Aboneliğiniz | Kaynaklarınızı oluşturulduğu abonelik. | 
    | **[Kaynak grubu](../azure-resource-manager/resource-group-overview.md)**  | myResourceGroup | Seçin `myResourceGroup`, veya işlev uygulamanızı oluşturduğunuz kaynak grubu. Barındırma planı ve WordPress VM işlev uygulaması için aynı kaynak grubunda'ı kullanarak, Bu öğretici ile işiniz bittiğinde kaynakları temizlemek kolaylaştırır. |
    | **Sanal makine adı** | VNET Wordpress | VM adı kaynak grubu içinde benzersiz olması gerekir |
    | **[Bölge](https://azure.microsoft.com/regions/)** | (Avrupa) Batı Avrupa | Size yakın veya sanal Makineye erişmek işlevleri yakın bir bölge seçin. |
    | **Boyut** | B1s | Seçin **değiştirme boyutu** ve ardından 1 vCPU ve 1 GB bellek B1s standart görüntüsünü seçin. |
    | **Kimlik doğrulaması türü** | Parola | Parola kimlik doğrulaması kullanmak için de belirtmeniz gerekir bir **kullanıcıadı**, güvenli **parola**, ardından **parolayı onayla**. Bu öğreticide, gidermeye ihtiyaç duyan sürece VM oturum gerekmez. |

1. Seçin **ağ** altında sanal ağları yapılandırma sekmenize **Yeni Oluştur**.

1. İçinde **sanal ağ oluştur**, görüntünün altındaki tabloda ayarları kullanın:

    ![Ağ sekmesinde VM'yi oluşturma](./media/functions-create-vnet/create-vm-2.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Ad** | vnet myResourceGroup | Sanal ağınız için oluşturulan varsayılan adı kullanabilirsiniz. |
    | **Adres aralığı** | 10.10.0.0/16 | Sanal ağ için bir tek adres aralığı kullanın. |
    | **Alt ağ adı** | Öğretici-Net | Alt ağın adı. |
    | **Adres aralığı** (alt ağ) | 10.10.1.0/24   | Alt ağ boyutu, kaç arabirimleri alt ağa eklenebilir tanımlar. Bu alt ağ WordPress sitesi tarafından kullanılır.  A `/24` alt ağ, 254 ana bilgisayar adresi sağlar. |

1. Seçin **Tamam** sanal ağ oluşturmak için.

1. Geri **ağ** sekmesini, **hiçbiri** için **genel IP**.

1. Seçin **Yönetim** sekmesini, ardından **tanılama depolama hesabı**, işlev uygulamanızı oluşturduğunuz depolama hesabını seçin.

1. **İncele ve oluştur**’u seçin. Doğrulama tamamlandıktan sonra seçin **Oluştur**. VM oluşturma işlemi işlem birkaç dakika sürer. Oluşturulan VM, yalnızca sanal ağa erişebilir.

1. VM oluşturulduktan sonra seçin **kaynağa Git** yeni VM'niz için sayfayı görüntülemek için ardından **ağ** altında **ayarları**.

1. Olduğunu doğrulayın hiçbir **genel IP**. Not **özel IP**, işlev uygulamanızı VM'ye bağlanmak için kullanılan.

    ![VM ağ ayarları](./media/functions-create-vnet/vm-networking.png)

Artık tamamen sanal ağınızda dağıtılan bir WordPress sitesi var. Bu site, genel internet'ten erişilebilir değil.

## <a name="connect-your-function-app-to-the-virtual-network"></a>İşlev uygulamanızı sanal ağınıza bağlama

Bir sanal ağdaki bir sanal makinede çalışan bir WordPress sitesi ile artık işlev uygulamanızı bu sanal ağa bağlanabilir.

1. Yeni işlev uygulamanızda seçin **Platform özellikleri** > **ağ**.

    ![İşlev uygulamasında ağı seçin](./media/functions-create-vnet/networking-0.png)

1. Altında **VNet tümleştirmesi**seçin **yapılandırmak için buraya tıklayın**.

    ![Bir ağ özelliğini yapılandırma durumu](./media/functions-create-vnet/Networking-1.png)

1. Sanal ağ tümleştirmesi sayfasında **VNet Ekle (Önizleme)** .

    ![Sanal ağ tümleştirmesi Önizleme Ekle](./media/functions-create-vnet/networking-2.png)

1. İçinde **ağ özelliği durumu**, görüntünün altındaki tabloda ayarları kullanın:

    ![İşlev uygulaması sanal ağ tanımlayın](./media/functions-create-vnet/networking-3.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Sanal Ağ** | MyResourceGroup-vnet | Bu sanal ağ, daha önce oluşturduğunuz bir bileşendir. |
    | **Alt ağ** | Yeni alt ağ oluşturma | Sanal ağ kullanmak işlev uygulamanız için bir alt ağ oluşturun. VNet tümleştirmesi, boş bir alt ağ kullanmak için yapılandırılmalıdır. İşlevlerinizi VM'niz farklı bir alt kullandığınız önemli değildir. Sanal ağı iki alt ağlar arasındaki trafiği otomatik olarak yönlendirir. |
    | **Alt ağ adı** | Net işlevi | Yeni alt ağın adı. |
    | **Sanal ağ adres bloğu** | 10.10.0.0/16 | WordPress sitesi tarafından kullanılan aynı adres bloğu seçin. Yalnızca tanımlanmış tek adres bloğu olmalıdır. |
    | **Adres aralığı** | 10.10.2.0/24   | Alt ağ boyutu Premium planı işlev uygulamanız için ölçeğini genişletebilirsiniz örneklerinin toplam sayısını sınırlar. Bu örnekte bir `/24` 254 ana kullanılabilir adresleri olan alt ağ. Bu alt ağ, fazla sağlama ve hesaplamak kolaydır. |

1. Seçin **Tamam** alt ağı eklemek için. İşlev uygulaması sayfanızın döndürmek için sanal ağ tümleştirme ve ağ özelliği durumu pencerelerini kapatın.

İşlev uygulaması, WordPress sitesi çalıştığı sanal ağ artık erişebilirsiniz. Ardından, kullandığınız [Azure işlev proxy'lerini](functions-proxies.md) WordPress siteden dosya dönün.

## <a name="create-a-proxy-to-access-vm-resources"></a>Sanal makine kaynaklarına erişmek için bir ara sunucu oluşturma

Sanal ağ ile tümleştirme, etkin işlev uygulamanız sanal ağ içinde çalıştırılan VM'ye isteklerini iletmek için bir proxy oluşturabilirsiniz.

1. İşlev uygulamanızı seçin **proxy'leri** >  **+** , ardından görüntünün altındaki tabloda Ara sunucu ayarlarını kullanın:

    ![Proxy ayarlarını tanımlayın](./media/functions-create-vnet/create-proxy.png)

    | Ayar  | Önerilen değer  | Açıklama      |
    | -------- | ---------------- | ---------------- |
    | **Ad** | Tesis | Ad, herhangi bir değer olabilir. Proxy tanımlamak için kullanılır. |
    | **Rota şablonu** | /plant | Bir VM kaynağına haritalar rota. |
    | **Arka uç URL'si** | http://<YOUR_VM_IP>/WP-Content/Themes/twentyseventeen/Assets/images/header.jpg | Değiştirin `<YOUR_VM_IP>` WordPress, daha önce oluşturduğunuz sanal makinenizin IP adresiyle. Bu eşleme, site veritabanından tek bir dosyayı döndürür. |

1. Seçin **Oluştur** proxy işlev uygulamanıza eklemek için.

## <a name="try-it-out"></a>Deneyin

1. Tarayıcınızda olarak kullanılan URL'nin erişmeye **arka uç URL'si**. Beklendiği gibi istek zaman aşımına uğradı. WordPress sitenizin yalnızca, sanal ağınızın ve değil internet'e bağlı olduğundan, bir zaman aşımı oluşur.

1. Kopyalama **Proxy URL'si** değer yeni Ara sunucunuz ve tarayıcınızın adres çubuğuna yapıştırın. Döndürülen resim, sanal ağ içinde çalıştırılan WordPress sitesi arasındadır.

    ![WordPress site veritabanından döndürülen tesis resim dosyası](./media/functions-create-vnet/plant.png)

İşlev uygulamanızı hem internet hem de sanal ağınıza bağlı. Proxy, genel internet üzerinden bir istek alma ve bağlı sanal ağ, isteği iletmek için basit bir HTTP proxy olarak görev yapan. Proxy ardından yanıt, genel internet üzerinden geçirir.

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, WordPress sitesi çağrılan işlev uygulamasına bir ara sunucu kullanarak API işlevi görür. Bu senaryo, ayarlamak ve görselleştirmek kolay olduğundan iyi bir öğretici sağlar. Dağıtılan bir sanal ağ içindeki herhangi bir API'yi kullanabilirsiniz. Ayrıca bir işlev ile sanal ağ içinde dağıtılan API'leri çağıran kod oluşturmuş. Daha gerçekçi bir senaryo sanal ağ içinde dağıtılan bir SQL Server örneği çağırmak için veri istemci API'leri kullanan bir işlevdir.

Premium planda çalışan işlevleri aynı App Service altyapının PremiumV2 planları üzerinde web apps'te olarak paylaşın. Tüm belgelerini [web uygulamaları Azure App Service'te](../app-service/overview.md) Premium planı işlevlere uygulanır.

> [!div class="nextstepaction"]
> [İşlevler içindeki ağ seçenekleri hakkında daha fazla bilgi edinin](./functions-networking-options.md)

[Premium planı]: functions-scale.md#premium-plan
