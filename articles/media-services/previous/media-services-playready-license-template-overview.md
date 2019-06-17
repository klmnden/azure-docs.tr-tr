---
title: Media Services PlayReady lisans şablonuna genel bakış
description: Bu konu, PlayReady lisansları yapılandırmak için kullanılan bir PlayReady lisans şablonu genel bir bakış sağlar.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: fddce5d0-1278-478f-ae05-9b985c748731
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: d2831472bcddf3480a87e69661b39e4ec715c786
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61463217"
---
# <a name="media-services-playready-license-template-overview"></a>Media Services PlayReady lisans şablonuna genel bakış
Azure Media Services PlayReady lisans sunma için bir hizmet artık sağlar. Oyuncu (örneğin, Silverlight) PlayReady korumalı içeriğinizi oynatma çalıştığında, bir lisans almak için bir istek için lisans teslimat hizmetinin gönderilir. Lisans hizmeti isteği onaylarsa, istemciye gönderilen ve şifresini çözmek ve belirtilen içeriğin yürütmek için kullanılan lisans verir.

Media Services PlayReady lisanslarınızı yapılandırmak için kullanabileceğiniz API'ler de sağlar. Lisans hakları içerir ve korumalı içeriği kayıttan yürütme bir kullanıcı çalıştığında zorlamak için PlayReady dijital hak yönetimi (DRM) runtime istediğiniz kısıtlamaları.
Belirtebileceğiniz PlayReady lisans kısıtlamaları bazı örnekleri aşağıda verilmiştir:

* Lisans geçerli saat ve tarihi.
* Lisans sona erdiğinde tarih saat değeri. 
* Lisans istemcide kalıcı depolama alanında kaydedilecek. Kalıcı lisans genellikle çevrimdışı kayıttan yürütme içeriği izin vermek için kullanılır.
* Bir oynatıcı içeriğinizi oynatmak için gereken en düşük güvenlik düzeyi. 
* Çıkış denetimleri audio\video içerik için çıkış koruma düzeyi. 
* Daha fazla bilgi için bkz: "Çıkış denetimleri" bölümüne (3.5) [PlayReady uyumluluk kuralları](https://www.microsoft.com/playready/licensing/compliance/) belge.

> [!NOTE]
> Şu anda yalnızca PlayReady lisans PlayRight yapılandırabilirsiniz. Bu hakkı gereklidir. PlayRight istemci içeriği kayıttan yürütme olanağı sağlar. PlayRight kayıttan yürütmek üzere belirli kısıtlamalarını yapılandırmak için de kullanabilirsiniz. Daha fazla bilgi için [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).
> 
> 

PlayReady lisansları Media Services'ı kullanarak yapılandırmak için Media Services PlayReady lisans şablonu yapılandırmanız gerekir. Şablon XML dosyasında tanımlanır.

Aşağıdaki örnek, temel bir akış lisans yapılandırır en basit (ve yaygın) şablonu gösterir. Bu lisans ile istemcilerinize PlayReady korumalı içerik geri oynatabilirsiniz.

    <?xml version="1.0" encoding="utf-8"?>
    <PlayReadyLicenseResponseTemplate xmlns:i="https://www.w3.org/2001/XMLSchema-instance" 
                                      xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <PlayRight />
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

XML "PlayReady lisans şablonu XML Şeması" bölümünde tanımlanan PlayReady lisans şablonu XML Şeması uyar.

Media Services, ayrıca XML gelen ve seri hale getrime ve için kullanılabilir .NET sınıf kümesi tanımlar. Ana Sınıf açıklaması için bkz: [Media Services .NET sınıfları](media-services-playready-license-template-overview.md#classes) lisans şablonları yapılandırmak için kullanılır.

PlayReady lisans şablonu yapılandırmak için .NET sınıfları kullanan bir uçtan uca örnek için bkz: [kullanım PlayReady dinamik şifreleme ve lisans teslimat hizmetinin](media-services-protect-with-playready-widevine.md).

## <a id="classes"></a>Lisans şablonları yapılandırmak için kullanılan bir Media Services .NET sınıfları
Media Services PlayReady lisans şablonları yapılandırmak için kullanılan ana .NET sınıfları sınıflardır. Bu sınıfların harita içinde tanımlanmış türlere [PlayReady lisans şablonu XML Şeması](media-services-playready-license-template-overview.md#schema).

[MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) sınıfı için ve Media Services lisans şablonu XML seri hale getrime ve için kullanılır.

### <a name="playreadylicenseresponsetemplate"></a>PlayReadyLicenseResponseTemplate
[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx): Bu sınıf, kullanıcıya geri gönderilen yanıtta şablonu temsil eder. Lisans sunucusu ve (özel uygulama mantığını için yararlı olabilir) uygulama arasında bir özel verileri dize için bir alan içeriyor. Ayrıca, bir veya daha fazla lisans şablonlarının bir listesini içerir.

Şablon hiyerarşideki "üst düzey" sınıf olarak yanıt şablon lisans şablonlarının bir listesini içerir. Lisans şablonları (doğrudan veya dolaylı olarak) içerir. şablon verileri seri hale yapmak diğer tüm sınıflar.

### <a name="playreadylicensetemplate"></a>PlayReadyLicenseTemplate
[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx): Bu sınıf, PlayReady lisansları kullanıcılara döndürülecek oluşturmak için kullanılan lisans şablonu temsil eder. Bu, içerik anahtarı lisans verilerini içerir. Ayrıca, tüm hakları veya kısıtlamaları içerik anahtarı kullanıldığında, PlayReady DRM çalışma zamanının zorlamalıdır de içerir.

### <a id="PlayReadyPlayRight"></a>PlayReadyPlayRight
[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx): Bu sınıf, PlayReady lisans PlayRight temsil eder. Kullanıcı Lisans ve (için kayıttan yürütme özel ilke) PlayRight kendisi üzerinde yapılandırılmış herhangi bir kısıtlama tabi içeriği kayıttan yürütme olanağı verir. Büyük bir PlayRight ilkesindeki içerik üzerinde yürütülen çıkışları türlerini kontrol çıkış kısıtlamaları ilgilidir. Ayrıca, belirli bir çıkış kullanıldığında, bir yere koymak herhangi bir kısıtlama da içerir. Örneğin, DigitalVideoOnlyContentRestriction etkinleştirilirse, DRM çalışma zamanının yalnızca videonun dijital çıktıların görüntülenmesini sağlar. (Analog video çıkışına içeriği geçmesine izin verilmez.)

> [!IMPORTANT]
> Bu tür kısıtlamaları güçlü olabilir, ancak yönelik tüketici deneyimi de etkileyebilir. Çıkış korumaları çok fazla kısıtlayıcıysa içeriği bazı istemcilerde unplayable olabilir. Daha fazla bilgi için [PlayReady uyumluluk kuralları](https://www.microsoft.com/playready/licensing/compliance/).
> 
> 

Silverlight destekleyen bir örnek koruma düzeyleri için bkz: [çıkış korumaları Silverlight desteğini](https://go.microsoft.com/fwlink/?LinkId=617318).

## <a id="schema"></a>PlayReady lisans şablonu XML Şeması
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:ser="http://schemas.microsoft.com/2003/10/Serialization/" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:xs="https://www.w3.org/2001/XMLSchema">
      <xs:import namespace="http://schemas.microsoft.com/2003/10/Serialization/" />
      <xs:complexType name="AgcAndColorStripeRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction" />
      <xs:simpleType name="ContentType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Unspecified" />
          <xs:enumeration value="UltravioletDownload" />
          <xs:enumeration value="UltravioletStreaming" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="ContentType" nillable="true" type="tns:ContentType" />
      <xs:complexType name="ExplicitAnalogTelevisionRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="BestEffort" type="xs:boolean" />
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ExplicitAnalogTelevisionRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction" />
      <xs:complexType name="PlayReadyContentKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="PlayReadyContentKey" nillable="true" type="tns:PlayReadyContentKey" />
      <xs:complexType name="ContentEncryptionKeyFromHeader">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence />
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromHeader" nillable="true" type="tns:ContentEncryptionKeyFromHeader" />
      <xs:complexType name="ContentEncryptionKeyFromKeyIdentifier">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence>
              <xs:element minOccurs="0" name="KeyIdentifier" type="ser:guid" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromKeyIdentifier" nillable="true" type="tns:ContentEncryptionKeyFromKeyIdentifier" />
      <xs:complexType name="PlayReadyLicenseResponseTemplate">
        <xs:sequence>
          <xs:element name="LicenseTemplates" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
          <xs:element minOccurs="0" name="ResponseCustomData" nillable="true" type="xs:string">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseResponseTemplate" nillable="true" type="tns:PlayReadyLicenseResponseTemplate" />
      <xs:complexType name="ArrayOfPlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfPlayReadyLicenseTemplate" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
      <xs:complexType name="PlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AllowTestDevices" type="xs:boolean" />
          <xs:element minOccurs="0" name="BeginDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element name="ContentKey" nillable="true" type="tns:PlayReadyContentKey" />
          <xs:element minOccurs="0" name="ContentType" type="tns:ContentType">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExpirationDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="GracePeriod" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="LicenseType" type="tns:PlayReadyLicenseType" />
          <xs:element minOccurs="0" name="PlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
      <xs:simpleType name="PlayReadyLicenseType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Nonpersistent" />
          <xs:enumeration value="Persistent" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="PlayReadyLicenseType" nillable="true" type="tns:PlayReadyLicenseType" />
      <xs:complexType name="PlayReadyPlayRight">
        <xs:sequence>
          <xs:element minOccurs="0" name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AllowPassingVideoContentToUnknownOutput" type="tns:UnknownOutputPassingOption">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AnalogVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="DigitalVideoOnlyContentRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExplicitAnalogTelevisionOutputRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="FirstPlayExpiration" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComponentVideoRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComputerMonitorRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyPlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
      <xs:simpleType name="UnknownOutputPassingOption">
        <xs:restriction base="xs:string">
          <xs:enumeration value="NotAllowed" />
          <xs:enumeration value="Allowed" />
          <xs:enumeration value="AllowedWithVideoConstriction" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="UnknownOutputPassingOption" nillable="true" type="tns:UnknownOutputPassingOption" />
      <xs:complexType name="ScmsRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction" />
    </xs:schema>



## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

