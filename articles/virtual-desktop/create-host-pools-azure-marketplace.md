---
title: Azure Marketi - Azure ile Windows sanal masaüstü Önizleme konak havuz oluşturma
description: Azure Marketi ile Windows sanal masaüstü Önizleme konak havuzu oluşturma
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 04/05/2019
ms.author: helohr
ms.openlocfilehash: 21979f1dee50fa846fb7888cfc95908b9d833392
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236805"
---
# <a name="tutorial-create-a-host-pool-with-azure-marketplace"></a>Öğretici: Azure Market ile ana bilgisayar havuzu oluşturma

Ana bilgisayar havuzları, Windows sanal masaüstü Önizleme Kiracı ortamlar içinde bir veya daha fazla aynı sanal makinelerden oluşan bir koleksiyondur. Her konak havuzu, fiziksel masaüstünde yaptıkları gibi kullanıcı etkileşim kurabilir bir uygulama grubu içerebilir.

Bu makalede, Microsoft Azure Marketi teklifi kullanarak bir Windows sanal masaüstü Kiracı içinde bir ana bilgisayar havuzu oluşturmayı açıklar. Bu şekilde, Desktop'ta Windows sanal bir kaynak grubu ile Vm'leri bir Azure aboneliğinde oluşturuluyor. Bu Vm'lere Active Directory etki alanına katılma ve Windows sanal masaüstü ile Vm'leri kaydetme, konak havuzu oluşturmayı içerir.

Başlamadan önce [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.

## <a name="sign-in-to-azure"></a>Oturum açın: Azure

<https://portal.azure.com> adresinden Azure portalında oturum açın.

## <a name="run-the-azure-marketplace-offering-to-provision-a-new-host-pool"></a>Yeni bir konak havuz sağlama teklifi Azure Marketi çalıştırın

Yeni bir konak havuz sağlama teklifi Azure Marketi çalıştırmak için:

1. Seçin **+** veya **+ kaynak Oluştur**.
2. Girin **Windows Sanal Masaüstü** Market arama penceresinde.
3. Seçin **Windows sanal masaüstü - bir ana makine havuzu sağlama**, ardından **Oluştur**.

Uygun dikey pencereleri bilgilerini girmek için yönergeleri izleyin.

### <a name="basics"></a>Temel

Temel bilgiler dikey penceresi için neler aşağıda verilmiştir:

1. Windows sanal masaüstü Kiracı içinde benzersiz olan konak havuzu için bir ad girin.
2. Kişisel Masaüstü için uygun seçeneği belirleyin. Seçerseniz **Evet**, bu ana havuzuna bağlanan her kullanıcı kalıcı olarak bir sanal makineye atanacak.
3. Windows sanal masaüstü istemcileri için oturum açın ve Azure Marketi'nde teklif tamamlandıktan sonra bir masaüstü erişebilen kullanıcıların virgülle ayrılmış listesini girin. Örneğin, atamak istediğiniz user1@contoso.com ve user2@contoso.com erişmek, girin "user1@contoso.com,user2@contoso.com."
4. Seçin **Yeni Oluştur** ve yeni kaynak grubu için bir ad sağlayın.
5. İçin **konumu**, Active Directory sunucusu bağlantısı olan bir sanal ağ olarak aynı konumu seçin.
6. **Tamam**’ı seçin.

### <a name="configure-virtual-machines"></a>Sanal makineleri yapılandırma

Yapılandırma sanal makineler dikey penceresi için:

1. Varsayılanları kabul edin ya da sayısını ve boyutunu sanal makinelerin özelleştirin.
2. Bir ön eki sanal makinelerin adlarını girin. Örneğin, "ön eki" adını girin, sanal makineler "ön eki-0," "ön eki-1" vb. çağrılır.
3. **Tamam**’ı seçin.

### <a name="virtual-machine-settings"></a>Sanal makine ayarları

Sanal makine ayarı dikey için:

1. Seçin **görüntü kaynağı** nasıl bulacağınızı ve depolamak için uygun bilgileri girin. Yönetilen diskleri kullanmayı tercih ederseniz, .vhd dosyasını içeren depolama hesabını seçin.
2. Vm'leri Active Directory etki alanına etki alanı hesabının kullanıcı asıl adını ve parolasını girin. Bu aynı kullanıcı adı ve parola sanal makinelerde yerel bir hesap oluşturulur. Daha sonra bu yerel hesaplar sıfırlayabilirsiniz.
3. Active Directory sunucuya bağlanabildiğini ve ardından sanal makineleri barındırmak için bir alt ağ, sanal ağı seçin.
4. **Tamam**’ı seçin.

### <a name="windows-virtual-desktop-preview-tenant-information"></a>Windows sanal masaüstü Önizleme Kiracı bilgileri

Windows sanal masaüstü Kiracı bilgileri dikey:

1. Girin **Windows sanal masaüstü Kiracı grubu adı** kiracınızın içerdiğinden Kiracı grubu. Planlı bir belirli Kiracı grubu adı yoksa, varsayılan olarak bırakın.
2. Girin **Windows sanal masaüstü Kiracı adı** Kiracı için bu ana havuzuna oluşturursunuz.
3. Windows sanal masaüstü Kiracı RDS sahibi olarak kimlik doğrulaması için kullanmak istediğiniz kimlik bilgileri türünü belirtin. Tamamlanmışsa [PowerShell öğretici ile hizmet sorumluları ve rol atamalarını oluşturma](./create-service-principal-role-powershell.md)seçin **hizmet sorumlusu**. Şimdi de girmeniz gerekecek **Azure AD Kiracı Kimliğinizi** hizmet sorumlusu içeren Azure Active Directory.
4. Ya da Kiracı yönetici hesabının kimlik bilgilerini girin. Yalnızca bir parola kimlik bilgisi ile hizmet sorumluları desteklenir.
5. **Tamam**’ı seçin.

## <a name="complete-setup-and-create-the-virtual-machine"></a>Kurulumu tamamlayın ve sanal makine oluşturun

Son iki dikey pencere için:

1. İçinde **özeti** dikey penceresinde, kurulum bilgilerini gözden geçirin. Bir şeyle değiştirmek istiyorsanız uygun dikey penceresine geri dönün ve devam etmeden önce değişikliği yapın. Bilgileri doğru görünüyorsa, seçin **Tamam**.
2. İçinde **satın** dikey penceresinde, Azure Market'ten satın alma hakkında ek bilgileri gözden geçirin.
3. Seçin **Oluştur** konak havuzunuz dağıtılacak.

Oluşturmakta olduğunuz kaç VM sayısına bağlı olarak bu işlem 30 dakika veya daha sürebilir.

## <a name="optional-assign-additional-users-to-the-desktop-application-group"></a>(İsteğe bağlı) Masaüstü uygulama grubuna ek kullanıcılar atayın

Azure Market teklifi tamamlandıktan sonra sanal makinelerinizde tam oturum Masaüstü test başlamadan önce ek kullanıcılar Masaüstü uygulama grubuna atayabilirsiniz. Azure Marketi'nde teklifin varsayılan kullanıcılar zaten ekledikten ve daha fazlasını eklemek istemiyorsanız, bu bölümü atlayabilirsiniz.

Masaüstü uygulama grubuna kullanıcıları atamak için bir PowerShell penceresi açmanız gerekir. Bundan sonra aşağıdaki iki cmdlet girmeniz gerekir.

Windows sanal masaüstü ortama oturum açmak için aşağıdaki cmdlet'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Aşağıdaki cmdlet'le sunan Azure Marketi'nde belirttiğiniz Windows sanal masaüstü Kiracı gruba bağlamını ayarlayın. Windows sanal masaüstü Kiracı grubu değer Azure Marketi'nde varsayılan değer olarak sunan bırakılırsa, bu adımı atlayabilirsiniz.

```powershell
Set-RdsContext -TenantGroupName <tenantgroupname>
```

Bu iki şey yaptıktan sonra Bu cmdlet ile masaüstü uygulama grubu kullanıcıları ekleyebilirsiniz:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

Kullanıcının UPN kullanıcının kimliğini Azure Active Directory'de eşleşmesi gerekir (örneğin, user1@contoso.com). Birden çok kullanıcı eklemek istiyorsanız, her kullanıcı için bu cmdlet'i çalıştırmanız gerekir.

Bu adımları tamamladıktan sonra Masaüstü uygulama grubuna eklenen kullanıcılar Windows sanal masaüstüne desteklenen Uzak Masaüstü istemcileri ile oturum açın ve bir kaynağı için bir oturum Masaüstü bakın.

Geçerli desteklenen istemciler şunlardır:

- [Windows 7 ve Windows 10 için Uzak Masaüstü İstemcisi](connect-windows-7-and-10.md)
- [Windows sanal masaüstü web istemcisi](connect-web.md)

>[!IMPORTANT]
>Güvenliğini sağlamaya yardımcı olmak için azure'da Windows sanal masaüstü ortamınızı Vm'lerinizde gelen bağlantı noktası 3389 açmayın öneririz. Windows sanal masaüstü açık bir konak havuzun Vm'leri erişmek kullanıcılar için 3389 numaralı gelen bağlantı noktası gerektirmez. Sorun giderme amacıyla 3389 numaralı bağlantı noktası açmanız gerekiyorsa, kullanmanızı öneririz [tam zamanında VM erişimi](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).

## <a name="next-steps"></a>Sonraki adımlar

Havuz ve atanan kullanıcılar, Masaüstü erişmek için bir konak yaptığınız, konak havuzunuz RemoteApps ile doldurabilirsiniz. Windows sanal masaüstü uygulamalarında yönetme hakkında daha fazla bilgi için Yönet uygulama grupları öğretici bakın.

> [!div class="nextstepaction"]
> [Uygulama grupları öğretici yönetme](./manage-app-groups.md)
