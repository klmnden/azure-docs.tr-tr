---
title: Azure Depolama’ya erişmek için Windows VM Yönetilen Kimliği kullanma
description: Windows VM Yönetilen Kimliği kullanarak Azure Depolama’ya erişme işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2018
ms.author: daveba
ms.openlocfilehash: e001907b9df77eff1455043a3fd7ce5533838fcc
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056183"
---
# <a name="tutorial-use-a-windows-vm-managed-identity-to-access-azure-storage"></a>Öğretici: Azure Depolama’ya erişmek için Windows VM Yönetilen Kimliği kullanma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğreticide, Windows Sanal Makinesi için Yönetilen Kimliği etkinleştirme ve ardından bu kimliği kullanarak Azure Depolama'ya erişme işlemleri gösterilir.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Yeni bir kaynak grubunda Windows sanal makinesi oluşturma 
> * Windows Sanal Makinesinde (VM) Yönetilen Kimliği etkinleştirme
> * Depolama hesabında bir blob kapsayıcı oluşturma
> * Windows VM’nizin Yönetilen Kimliğine depolama hesabı için erişim izni verin 
> * Erişim elde edin ve bu erişimi Azure Depolama’yı çağırmak için kullanın 

> [!NOTE]
> Azure Depolama için Azure Active Directory kimlik doğrulaması genel önizlemeye sunuldu.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda Windows sanal makinesi oluşturma

Bu bölümde, daha sonra Yönetilen Kimlik verilen bir Windows VM oluşturursunuz.

1.  Azure portalın sol üst köşesinde bulunan **+/Yeni hizmet oluştur** düğmesine tıklayın.
2.  **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3.  Sanal makine bilgilerini girin. Burada oluşturulan **Kullanıcı adı** ve **Parola**, sanal makinede oturum açmak için kullandığınız kimlik bilgileridir.
4.  Açılan listede sanal makine için uygun **Aboneliği** seçin.
5.  İçinde sanal makinenin oluşturulmasını istediğiniz yeni bir **Kaynak Grubu** seçmek için, **Yeni Oluştur**'u seçin. İşlem tamamlandığında **Tamam**’a tıklayın.
6.  VM'nin boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

    ![Alternatif resim metni](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

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

Bir erişim belirteci kullanarak Azure Depolama’ya bağlantı açma ve ardından daha önce oluşturduğunuz dosyanın içeriklerini okumaya yönelik .Net kod örneği aşağıda verilmiştir. Bu kodun, VM’nin Yönetilen Kimlik uç noktasına erişebilmesi için VM üzerinde çalıştırılması gerekir. Erişim belirteci yöntemini kullanmak için .Net Framework 4.6 veya üzeri bir sürüm gereklidir. `<URI to blob file>` değerini uygun şekilde değiştirin. Blob depolama alanına oluşturup yüklediğiniz dosyaya giderek ve **Özellikler**’in altındaki **URL**’yi **Genel bakış** sayfasına kopyalayarak bu değeri alabilirsiniz.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IO;
using System.Net;
using System.Web.Script.Serialization; 
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;

namespace StorageOAuthToken
{
    class Program
    {
        static void Main(string[] args)
        {
            //get token
            string accessToken = GetMSIToken("https://storage.azure.com/");
           
            //create token credential
            TokenCredential tokenCredential = new TokenCredential(accessToken);

            //create storage credentials
            StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

            Uri blobAddress = new Uri("<URI to blob file>");

            //create block blob using storage credentials
            CloudBlockBlob blob = new CloudBlockBlob(blobAddress, storageCredentials);
        
            //retrieve blob contents
            Console.WriteLine(blob.DownloadText());
            Console.ReadLine();
        }

        static string GetMSIToken(string resourceID)
        {
            string accessToken = string.Empty;
            // Build request to acquire MSI token
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=" + resourceID);
            request.Headers["Metadata"] = "true";
            request.Method = "GET";

            try
            {
                // Call /token endpoint
                HttpWebResponse response = (HttpWebResponse)request.GetResponse();

                // Pipe response Stream to a StreamReader, and extract access token
                StreamReader streamResponse = new StreamReader(response.GetResponseStream());
                string stringResponse = streamResponse.ReadToEnd();
                JavaScriptSerializer j = new JavaScriptSerializer();
                Dictionary<string, string> list = (Dictionary<string, string>)j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
                accessToken = list["access_token"];
                return accessToken;
            }
            catch (Exception e)
            {
                string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
                return accessToken;
            }
        }            
    }
}
```

Yanıt, dosyanın içeriklerini kapsar:

`Hello world! :)`

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Depolama’ya erişmek için Windows sanal makine Yönetilen Kimliğini etkinleştirmeyi öğrendiniz.  Azure Depolama hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Azure Depolama](/azure/storage/common/storage-introduction)



