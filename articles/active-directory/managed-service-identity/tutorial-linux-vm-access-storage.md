---
title: Azure Depolama’ya erişmek için Linux VM Yönetilen Kimliği kullanma
description: Linux VM üzerinde bir Sistem Tarafından Atanmış Yönetilen Kimlik kullanarak Azure Depolama'ya erişme işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/09/2018
ms.author: daveba
ms.openlocfilehash: d4daccfdcb2bc11831e960aa20533e32801db946
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049346"
---
# <a name="tutorial-use-a-linux-vms-managed-identity-to-access-azure-storage"></a>Öğretici: Azure Depolama’ya erişmek için Linux VM’nin Yönetilen Kimliğini kullanma 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


Bu öğreticide, Azure Depolama’ya erişmek için Linux VM Yönetilen Kimliğini oluşturma ve kullanma işlemleri gösterilir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Yeni bir kaynak grubunda Linux sanal makinesi oluşturma
> * Linux Sanal Makinesinde (VM) Yönetilen Kimliği etkinleştirme
> * Depolama hesabında bir blob kapsayıcı oluşturma
> * Azure Depolama kapsayıcısına Linux VM’sinin Yönetilen Kimlik erişimini verme
> * Erişim belirteci alma ve bu belirteci Azure Depolama’yı çağırmak için kullanma

> [!NOTE]
> Azure Depolama için Azure Active Directory kimlik doğrulaması genel önizlemeye sunuldu.

## <a name="prerequisites"></a>Ön koşullar

Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com).

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Bu öğreticideki CLI betiği örneklerini çalıştırmak için iki seçeneğiniz vardır:

- Azure portaldan veya her kod bloğunun sağ üst köşesinde yer alan **Deneyin** düğmesi aracılığıyla [Azure Cloud Shell](~/articles/cloud-shell/overview.md)'i kullanın.
- Yerel bir CLI konsolu kullanmayı tercih ediyorsanız [en son CLI 2.0 sürümünü yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 veya üstü).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda Linux sanal makinesi oluşturma

Bu bölümde, daha sonra Yönetilen Kimlik verilen bir Linux VM oluşturursunuz.

1. Azure portalın sol üst köşesinde bulunan **Yeni** düğmesini seçin.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. **Kimlik doğrulama türü** olarak **SSH ortak anahtarı**'nı veya **Parola**'yı seçin. Oluşturulan kimlik bilgileri VM'de oturum açmanıza olanak tanır.

   ![Sanal makine oluşturmak için "Temel" bölmesi](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. **Abonelik** listesinde, sanal makine için bir abonelik seçin.
5. İçinde sanal makinenin oluşturulmasını istediğiniz yeni bir kaynak grubu seçmek için, **Kaynak grubu** > **Yeni oluştur**'u seçin. İşiniz bittiğinde **Tamam**'a tıklayın.
6. VM'nin boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar bölmesinde varsayılan değerleri koruyun ve **Tamam**'ı seçin.

## <a name="enable-managed-identity-on-your-vm"></a>VM’nizde Yönetilen Kimliği etkinleştirme

Sanal Makine Yönetilen Kimliği kodunuza kimlik bilgileri yerleştirmeniz gerekmeden Azure AD'den erişim belirteçlerini almanıza olanak tanır. Azure portaldan bir Sanal Makinede Yönetilen Kimliği etkinleştirmek aslında şu iki şeyi gerçekleştirir: Bir yönetilen kimlik oluşturmak için VM’nizi Azure AD’ye kaydeder ve kimliği VM’de yapılandırır.

1. Yeni sanal makinenizin kaynak grubuna gidin ve önceki adımda oluşturduğunuz sanal makineyi seçin.
2. **Ayarlar** kategorisinin altında, **Yapılandırma**’ya tıklayın.
3. Yönetilen Kimliği etkinleştirmek için **Evet**’i seçin.
4. Yapılandırmayı uygulamak için **Kaydet**’e tıklayın. 

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Bu bölümde bir depolama hesabı oluşturursunuz. 

1. Azure portalın sol üst köşesinde bulunan **+ Kaynak oluştur** düğmesine tıklayın.
2. **Depolama**’ya ve sonra **Depolama hesabı - blob, dosya, tablo, kuyruk** öğesine tıklayın.
3. **Ad**’ın altında, depolama hesabı için bir ad girin.  
4. **Dağıtım modeli** ve **Hesap türü**, **Kaynak yöneticisi** ve **Depolama (genel amaçlı v1)** olarak ayarlanmalıdır. 
5. **Abonelik** ve **Kaynak Grubu** değerlerinin, önceki adımda VM'nizi oluştururken belirttiklerinizle eşleştiğinden emin olun.
6. **Oluştur**’a tıklayın.

    ![Yeni depolama hesabı oluşturma](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-and-upload-a-file-to-the-storage-account"></a>Bir blob kapsayıcı oluşturma ve depolama hesabına dosya yükleme

Dosyalar blob depolama alanı gerektirdiğinden dosyasının depolanacağı bir blob kapsayıcısı oluşturmanız gerekir. Ardından yeni depolama hesabındaki blob kapsayıcısına bir dosya yükleyin.

1. Yeni oluşturulan depolama hesabınıza geri gidin.
2. **Blob Hizmeti**’nin altında, **Kapsayıcılar**’a tıklayın.
3. Sayfanın üstündeki **+ Kapsayıcı** seçeneğine tıklayın.
4. **Yeni kapsayıcı**’nın altında, kapsayıcı için bir ad girin ve **Genel erişim düzeyi**’nin altında varsayılan değeri değiştirmeyin.

    ![Depolama kapsayıcısı oluşturma](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Tercih ettiğiniz bir düzenleyiciyi kullanarak yerel makinenizde *hello world.txt* başlıklı bir dosya oluşturun.  Dosyayı açıp (tırnak işaretleri olmadan) "Hello world! :)" metnini ekleyin ve sonra kaydedin. 

6. Kapsayıcı adına ve ardından **Karşıya yükle**’ye tıklayarak dosyayı yeni oluşturulan kapsayıcıya yükleyin
7. **Blobu karşıya yükle** bölmesinde **Dosyalar**’ın altında, klasör simgesine tıklayıp yerel makinenizde **hello_world.txt** dosyasına göz atın, dosyayı seçin ve ardından **Karşıya yükle**’ye tıklayın.

    ![Metin dosyasını karşıya yükleme](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-vm-access-to-an-azure-storage-container"></a>VM'nize Azure Depolama kapsayıcısı için erişim izni verme 

Azure depolama blobundaki verileri almak için VM’nin yönetilen kimliğini kullanabilirsiniz.   

1. Yeni oluşturulan depolama hesabınıza geri gidin.  
2. Sol bölmedeki **Erişim denetimi (IAM)** bağlantısına tıklayın.  
3. VM’nize yönelik yeni bir rol ataması eklemek için sayfanın üst kısmındaki **+ Ekle**’ye tıklayın.
4. **Rol**’ün altında, açılan listeden **Depolama Blob Verileri Okuyucusu (Önizleme)** seçeneğini belirleyin. 
5. Sonraki açılan listede **Erişimin atanacağı hedef** öğesinin altında **Sanal Makine**’yi seçin.  
6. Ardından, uygun aboneliğin **Abonelik**’te listelendiğinden emin olun ve sonra **Kaynak Grubu**’nu **Tüm kaynak grupları** olarak ayarlayın.  
7. **Seçin**’in altında, VM'nizi belirleyin ve ardından **Kaydet**’e tıklayın.

    ![İzin atama](~/articles/active-directory/managed-service-identity/media/tutorial-linux-vm-access-storage/access-storage-perms.png)

## <a name="get-an-access-token-and-use-it-to-call-azure-storage"></a>Erişim belirteci alma ve bu belirteci Azure Depolama’yı çağırmak için kullanma

Azure Depolama, Azure AD kimlik doğrulamasını yerel olarak desteklediğinden Yönetilen Kimlik kullanılarak alınan erişim belirteçlerini doğrudan kabul eder. Bu, Azure Depolama’nın Azure AD tümleştirmesi kapsamındadır ve bağlantı dizesinde kimlik bilgileri sağlama işleminden farklıdır.

Aşağıdaki adımları tamamlamak için, daha önce oluşturulmuş olan VM’den çalışmanız gerekir ve buna bağlanmak için de SSH istemciniz olmalıdır. Windows kullanıyorsanız, [Linux için Windows Alt Sistemi](https://msdn.microsoft.com/commandline/wsl/about)'ndeki SSH istemcisini kullanabilirsiniz. SSH istemcinizin anahtarlarını yapılandırmak için yardıma ihtiyacınız olursa, bkz. [Azure'da Windows ile SSH anahtarlarını kullanma](~/articles/virtual-machines/linux/ssh-from-windows.md) veya [Azure’da Linux VM’ler için SSH ortak ve özel anahtar çifti oluşturma](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. Azure portalında **Sanal Makineler**'e gidin, Linux sanal makinenize gidin ve ardından **Genel Bakış** sayfasında **Bağlan**'a tıklayın. VM'nize bağlanma dizesini kopyalayın.
2. Tercih ettiğiniz SSH istemciyle VM'ye **bağlanın**. 
3. Terminal penceresinde, Azure Depolama erişim belirtecini almak için CURL'yi kullanarak yerel Yönetilen Kimlik uç noktasına bir istek gönderin.
    
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fstorage.azure.com%2F' -H Metadata:true
    ```
4. Azure Depolama’ya erişmek için şimdi erişim belirtecini kullanın; örneğin, kapsayıcıya daha önce yüklediğiniz örnek dosyanın içeriğini okuyabilirsiniz. `<STORAGE ACCOUNT>`, `<CONTAINER NAME>` ve `<FILE NAME>` değerlerini daha önce belirttiğiniz değerlerle ve `<ACCESS TOKEN>` öğesini önceki adımda döndürülen belirteçle değiştirin.

   ```bash
   curl https://<STORAGE ACCOUNT>.blob.core.windows.net/<CONTAINER NAME>/<FILE NAME> -H "x-ms-version: 2017-11-09" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   Yanıt, dosyanın içeriklerini kapsar:

   ```bash
   Hello world! :)
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Depolama’ya erişmek için Linux sanal makinesi Yönetilen Kimliğini etkinleştirmeyi öğrendiniz.  Azure Depolama hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Azure Depolama](/azure/storage/common/storage-introduction)
