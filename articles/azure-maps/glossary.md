---
title: Azure haritalar sözlüğü | Microsoft Docs
description: Azure haritalar, konum tabanlı hizmetler ve GIS ilişkili yaygın olarak kullanılan terimler sözlüğü.
author: rbrundritt
ms.author: richbrun
ms.date: 09/18/2018
ms.topic: resource
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: d220fea64f860ebe5b660f7ebe5ad7db7aec4534
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46368857"
---
# <a name="glossary"></a>Sözlük

Azure Haritalar ile kullanılan ortak kelimeler listesi verilmiştir.

## <a name="a"></a>A

<a name="address-validation"></a> **Adres doğrulaması**: adres varlığı doğrulama işlemi.

<a name="advanced-routing"></a> **Gelişmiş yönlendirme**: öncelikli işlemleri kullanarak erişilebilir aralıkları (izokron) hesaplama gibi yönlendirme verileri yol, hizmetler koleksiyonu matrislerde uzaklığı ve toplu istekleri.

<a name="aerial-imagery"></a> **Havadan tanımayı**: bkz [uydu tanımayı](#satellite-imagery). 

<a name="along-a-route-search"></a> **Bir rota arama boyunca**: uzamsal sorgu saati veya uzaklığı bir rota yolu belirtilen bir sapma içinde verileri arar.

<a name="altitude"></a> **Yükseklik**: Yükseklik veya bir başvuru yüzeyi üzerinde bir noktasının dikey yükseltme. Yükseklik ölçümler gibi ortalama Deniz düzeyinde bir verilen başvuru verisi temel alır. Ayrıca bkz: yükseltme.

<a name="ambiguous"></a> **Belirsiz**: bir nesne iki veya daha fazla değer belirli bir öznitelik için uygun şekilde atanması, var olan veri sınıflandırması belirsizliğin durumu. Örneğin, coğrafi kodlama "CA" iki belirsiz sonuçlar döndürülür; "Kanada" ve "California" "CA" olarak, her bir ülke ve eyalet kodu sırasıyla. 

<a name="annotation"></a> **Ek açıklama**: kullanıcıya bilgi sağlamak için metin ya da grafik harita üzerinde görüntülenir. Ek açıklama tanımlamak veya belirli bir eşleme varlığı açıklayan, harita üzerinde bir alan hakkındaki genel bilgileri girin veya eşleme hakkında bilgi sağlayın.

<a name="antimeridian"></a> **Antimeridian**: olarak da bilinen 180<sup>th</sup> Meridyen nerede-180 derece ve boylam 180 derece karşılamak noktasıdır. Dünya üzerindeki asal meridyen ters olduğu.

<a name="application-programming-interface-api"></a> **Uygulama programlama arabirimi (API)**: geliştiricilerin uygulamalar oluşturmasına olanak tanıyan bir belirtimi.

<a name="api-key"></a> **API anahtarı**: Azure haritalar anahtarını bakın.

<a name="area-of-interest-aoi"></a> **Alan ilgi (AOI)**: odak alanı eşlemesi veya veritabanı üretim için tanımlamak için kullanılan kapsam.

<a name="asset-tracking"></a> **Varlık İzleme**: bir kişi, araç veya başka bir nesne gibi bir varlığın konum izleme işlemi.

<a name="asynchronous-request"></a> **Zaman uyumsuz istek**: bir bağlantı açar ve zaman uyumsuz istek için bir tanımlayıcı döndürür sunucuya istekte bir HTTP isteği daha sonra bağlantıyı kapatır. Sunucu isteği işlemeye devam eder ve kullanıcı tanımlayıcısını kullanarak durumu denetleyebilirsiniz. İsteğin işlenmesi Bitti, kullanıcı ardından yükleme yanıt olduğunda. Bu istek türü genellikle uzun süre için kullanılan çalışan işlemleri.

<a name="autocomplete"></a> **Otomatik Tamamlama**: bir uygulamada bir özelliği bir kullanıcı yazarak bir sözcük geri kalanını tahmin eder. 

<a name="autosuggest"></a> **Otomatik öneri**: bir özellik, bir uygulamanın hangi kullanıcı yazmak için mantıksal olasılıklar tahmin eder.

<a name="azure-location-based-services-lbs"></a> **Azure konum tabanlı Hizmetler'in (LBS)**: Azure Önizleme aşamasında olduğu zaman haritalar eski adı.

<a name="azure-maps-key"></a> **Azure haritalar anahtarını**: bir Azure haritalar anahtarını olan bir kullanıcının Azure haritalar uygulaması veya hizmeti istek kimliğini doğrulamak için kullanılan benzersiz bir dize. 

## <a name="b"></a>B

<a name="base-map"></a> **Temel harita**: yollar, yer işareti ve siyasi sınırları gibi arka plan başvuru bilgileri görüntüleyen bir harita uygulamanın bir parçası.

<a name="batch-request"></a> **Toplu istek**: birden çok isteği tek bir istek birleştirme işlemine.

<a name="bearing"></a> **Pul**: yatay yöndeki bir diğer nokta ile ilgili bir noktanın. Bu, Açı 0-360 derece yönünde derece kuzeyden göreli olarak ifade edilir. 

<a name="boundary"></a> **Sınır**: satır ya da ülke, bölgeleri ve özellikler gibi bitişik siyasi varlıkları ayırarak Çokgen. Bir sınırı veya rivers, dağlarında yürüyüş ya da duvar gibi fiziksel özelliklerini uymayabilecek bir satırdır.

<a name="bounds"></a> **Sınırları**: bkz [Bounding kutusu](#bounding-box).

<a name="bounding-box"></a> **Sınırlayıcı kutu**: koordinatları harita üzerinde bir dikdörtgen alan temsil etmek için kullanılan bir dizi. 

## <a name="c"></a>C

<a name="cadastre"></a> **Cadastre**: kayıtlı land ve özelliklerinin bir kaydı. Ayrıca bkz: [paket](#parcel).

<a name="camera"></a> **Kamera**: etkileşimli harita denetimi bağlamında, bir kamera görünüm haritalar alanı tanımlar. Görünüm penceresinin kameranın birkaç harita parametrelerine göre belirlenir; Center, yakınlaştırma düzeyi, aralık, seçtiğiniz. 

<a name="centroid"></a> **Kütle Merkezi**: geometrik center özellik. Kütle merkezi bir alan merkezini bulunurken bir satırın kütle merkezi Orta olacaktır.

<a name="choropleth-map"></a> **Choropleth'te harita**: alanları olduğu bir ölçüm haritada görüntülenmesini istatistiksel bir değişkenin derlemekten gölgeli tematik bir harita. Örneğin, diğer tüm durumlara göreli yıllarındaki nüfusu göre her ABD devlet sınırları renklendirme.

<a name="concave-hull"></a> **İçbükey Kabuk**: Belirtilen veri kümesindeki tüm şekiller barındırır olası bir İçbükey geometrisini temsil eden bir şekli. Oluşturulan şekil verileri Plastik Sarma ile kaydırma ve ardından ısıtma benzer, bu nedenle büyük neden diğer veri noktası cave noktalarına arasında yayılır.

<a name="consumption-model"></a> **Kullanım modeli**: başlangıçtan bir araç yakıt veya elektrik kullanan oranı tanımlayan bilgileri. Ayrıca bkz: [tüketim modelini belgeleri](consumption-model.md).

<a name="control"></a> **Denetim**: arabirimin davranışları kümesini tanımlayan bir grafik kullanıcı arabirimi oluşan müstakil ya da yeniden kullanılabilir bir bileşen. Örneğin, bir harita denetimi olan genellikle etkileşimli bir harita yükler kullanıcı arabiriminin bir bölümü.

<a name="convex-hull"></a> **Dışbükey Kabuk**: dışbükey Kabuk belirtilen veri kümesindeki tüm şekiller barındırır minimum dışbükey geometrisini temsil eden şekildir. Oluşturulan veri kümesi etrafında bir esnek bant kaydırma benzer şekildir.

<a name="coordinate"></a> **Koordinat**: bir harita üzerindeki bir konumu temsil etmek için kullanılan boylam ve enlem değerlerini içerir.

<a name="coordinate-system"></a> **Koordinat sistemi**: alanında iki veya üç boyutta noktalarının konumlarını tanımlamak için kullanılan bir başvuru çerçevesi.

<a name="country-code"></a> **Ülke kodu**: bir ülke için benzersiz bir tanımlayıcı, ISO standardına dayalı. ISO2 ISO3 temsil eden üç karakterli (örneğin, ABD) kod ülke (örneğin, ABD), iki karakterli kodudur.

<a name="country-subdivision"></a> **Ülke alt**: genellikle bir eyalet veya il bilinen bir ülkenin birinci düzey alt bölümü.

<a name="country-secondary-subdivision"></a> **Ülke ikincil alt**: genellikle bir ilçe bilinen bir ülke, ikinci düzey bölümlemesini.

<a name="country-tertiary-subdivision"></a> **Ülke üçüncül alt**: ülke üçüncü düzey bölümlemesini, genellikle bir anlatan gibi adlandırılmış bir alan.

<a name="cross-street"></a> **Çapraz Sokak**: iki veya daha fazla sokaklar kesiştiği bir nokta.

<a name="cylindrical-projection"></a> **Silindirik izdüşümü**: spheroid veya sphere tanjantını veya secant silindir üzerine noktalarından dönüştüren bir projeksiyon. Silindir ardından üstten alta dilimlenebilir ve bir düzleme düzleştirilmiş.

## <a name="d"></a>D

<a name="datum"></a> **Datum**: başvuru belirtimleri bir ölçüm sisteminin bir koordinat sistemini bir yüzeyi (yatay datum) ya da yukarıdaki yüksekliklerini veya bir yüzeyi (dikey datum) altına yerleştirir.

<a name="dbf-file"></a> **DBF dosyası**: Şekil dosyaları (SHP) ile birlikte kullanılan bir veritabanı dosyası biçimi.

<a name="degree-minutes-seconds-dms"></a> **Derece dakika saniye (DMS)**: Enlem ve boylam tanımlamak için ölçü birimi. 1/360 derecelik bir olduğu<sup>th</sup> bir daire. Bir ölçüde daha fazla 60 dakika içinde ayrılmış ve bir dakika içinde 60 saniye ayrılmıştır.

<a name="delaunay-triangulation"></a> **Delaunay Üçlü**: bitişik, örtüşmeyen üçgenler ağıyla noktalarının kümesinden oluşturmak için bir yöntem. Her üçgenin circumscribing daire kendi iç kümesinde hiç noktalarından içerir.

<a name="demographics"></a> **Demografik bilgiler**: İnsan popülasyon istatistiksel özelliklerini (örneğin, yaş, Doğum oranı ve gelir).

<a name="destination"></a> **Hedef**: bir uç noktası veya, birisi seyahat için konum.

<a name="digital-elevation-model-dem"></a> **Dijital yükseltme modeli (DEM)**: ortak veri kullanarak düzenli aralıklarla bir alanda üzerinden yakalanan yükseltme değerlerini bir veri kümesi bir yüzeyi için ilgili. DEMs genellikle Arazi Tahliye temsil etmek için kullanılır.

<a name="dijkstra's-algorithm"></a> **Dijkstra'nın algoritması**: iki nokta arasındaki en kısa yolu bulmak için bir ağ bağlantısı inceleyen bir algoritma.

<a name="distance-matrix"></a> **Mesafe matrisi**: hedefleri ve kaynaklar kümesi arasında seyahat süresi ve uzaklık bilgilerini içeren bir matris. 

## <a name="e"></a>E

<a name="elevation"></a> **Yükseltme**: bir nokta ya da yukarıdaki nesne veya bir başvuru yüzey veya datum (genellikle ortalama Deniz düzeyinde) aşağıda dikey uzaklık. Yükseltme, genelde land dikey yüksekliğine anlamına gelir.

<a name="envelope"></a> **Zarf**: bkz [Bounding kutusu](#bounding-box).

<a name="extended-postal-code"></a> **Genişletilmiş posta kodu**: ek bilgi içerebilecek bir posta kodu. Örneğin, ABD beş basamak posta kodları vardır ancak zip + 4 olarak bilinen bir genişletilmiş posta kodu, dört ek basamaklar içerir. Bu ek basamaklar, beş basamaklı teslim alan bir şehir bloğu, binalarının bir grup veya verimli posta sıralama ve teslim yardımcı bir posta office kutusunu, gibi coğrafi bir Segmentte tanımlamak için kullanılır.

<a name="extent"></a> **Uzantı**: bkz [Bounding kutusu](#bounding-box).

## <a name="f"></a>F

<a name="federated-authentication"></a> **Federe kimlik doğrulaması**: birden çok web ve mobil uygulamalarınızda kullanılmak üzere çoklu oturum açma/kimlik doğrulama mekanizması sağlayan bir kimlik doğrulama yöntemi. 

<a name="feature"></a> **Özellik**: geometri ek meta veri bilgileri ile bir araya getiren bir nesne. 

<a name="feature-collection"></a> **Koleksiyon özellik**: özellik nesneleri koleksiyonu.

<a name="find-along-route"></a> **Yol**: uzamsal sorgu saati veya uzaklığı bir rota yolu belirtilen bir sapma içinde verileri arar.

<a name="find-nearby"></a> **Yakındaki Bul**: (crow ile uçuyor gibi) sabit, doğrusal bir uzaklık noktası olan bir uzamsal sorgu.

<a name="fleet-management"></a> **Filo Yönetimi**: araba, kamyon, verilir ve düzlemleri gibi ticari araçları yönetimi. Filo yönetimi, bir dizi araç Finansmanı, bakım, telematik (izleme ve tanılama) yanı sıra sürücü, hız, yakıt ve sistem durumu ve güvenlik yönetimi gibi işlevleri içerebilir. Filo yönetimi, riskleri en aza indirmek ve kamu mevzuatı uyum sağlarken kendi genel nakliye ve personel maliyetlerini azaltmak için kendi iş üzerinde nakliye kullanan şirketler tarafından kullanılan bir işlemdir.

<a name="free-flow-speed"></a> **Boş Akış hızı**: ideal koşullarda beklenen boş Akış hızı. Genellikle hız sınırı.

<a name="free-form-address"></a> **Serbest biçimli adresi**: tek bir metin satırı temsil edilen bir tam adresi.

<a name="fuzzy-search"></a> **Belirsiz arama**: metnin bir adresi serbest biçimli dize veya ilgi noktası alan arama. 

## <a name="g"></a>G

<a name="geocode"></a> **Geocode**: bir adres veya bu konumunu bir haritada görüntülemek için kullanılan bir koordinat dönüştürüldükten konumu. 

<a name="geocoding"></a> **Coğrafi kodlama**: ileriye doğru coğrafi kodlama da bilinen, konum verileri adresini koordinatları haline dönüştürme işlemidir. 

<a name="geodesic-path"></a> **Geodesic yolu**: eğri yüzeyinde iki nokta arasındaki en kısa yolu. Azure haritalar üzerinde işlendiğinde bu yolu Mercator izdüşümünü nedeniyle bir eğri çizgi görünür.

<a name="geofence"></a> **Bölge sınırının**: A tanımlanan bir cihaz girdiğinde veya bölgede mevcut olaylarını tetiklemek için kullanılabilecek bir coğrafi bölge.

<a name="geojson"></a> **GeoJSON**: noktaları, satırlar ve çokgenler gibi coğrafi vektör verilerini depolamak için kullanılan ortak bir JSON tabanlı bir dosya biçimidir. **Not**: Azure haritalar kullanan GeoJSON genişletilmiş bir sürümünü [burada belgelenmektedir](extend-geojson.md).

<a name="geometry"></a> **Geometri**: point, çizgi veya Çokgen gibi bir uzamsal nesnesini temsil eder.

<a name="geometrycollection"></a> **GeometryCollection**: geometri nesneleri koleksiyonu.

<a name="geopol"></a> **GeoPol**: tartışmalı kenarlık ve yer adlarına gibi ekleyemeyebilir hassas verileri ifade eder.

<a name="georeference"></a> **Jeo referans**: coğrafi veri veya bilinen bir koordinat sistemi için tanımayı hizalama işlemi. Bu işlem, kaydırma, döndürme, ölçeklendirme veya eğriltme verilerini oluşabilir.

<a name="georss"></a> **GeoRSS**: RSS özet akışlarına uzamsal veri ekleme, XML uzantı.

<a name="gis"></a> **GIS**: "Coğrafi bilgi sistemi" kısaltması. Eşleme sektör açıklamak için kullanılan genel bir terimdir.

<a name="gml"></a> **GML**: Coğrafya biçimlendirme dili da bilinir. Uzamsal veri depolamak için bir XML dosya uzantısı.

<a name="gps"></a> **GPS**: Genel konumlandırma sistemi olarak da bilinen bir dünya cihazları konumuna belirlemek için kullanılan Uyduları sistemidir. Gönderen yörüngedeki Uyduları trilateration aracılığıyla kendi konumunu hesaplamak GPS alıcısı herhangi bir yerinden izin sinyalleri iletir.

<a name="gpx"></a> **GPX**: GPS Takas Biçimi da bilinen yaygın olarak GPS cihazlardan oluşturulan XML dosya biçimi olan.  

<a name="great-circle-distance"></a> **Büyük daire uzaklık**: kürenin yüzeyinde iki nokta arasındaki en kısa uzaklığı.

<a name="greenwich-mean-time-gmt"></a> **(GMT) Greenwich saati**: Greenwich, İngiltere'deki Kraliyet Rasathanesi üzerinden çalışan asal meridyen zaman.

<a name="guid"></a> **GUID**: genel benzersiz tanımlayıcı. Bir arabirim, sınıf, tür kitaplığı, bileşen kategorisine veya kaydı benzersiz olarak tanımlamak için kullanılan bir dize.

## <a name="h"></a>H

<a name="haversine-formula"></a> **Haversine formül**: bir küre üzerinde iki nokta arasındaki mükemmel daire uzaklığı hesaplamak için kullanılan ortak bir eşitlik.

<a name="hd-maps"></a> **HD haritalar**: Ayrıca yüksek eşler tanımına olarak bilinir oluşur yüksek uygunluğa sahip yol ağ bilgilerini Kulvar işaretler gibi malzemelerinizin ve yönü ışık otonom sürüş için gerekli.

<a name="heading"></a> **Başlık**: bir şey işaret eden ya da karşılıklı yönü. Ayrıca bkz: [pul](#heading).

<a name="heatmap"></a> **Isı Haritası**: bir renk aralığı noktalarının belirli bir alandaki yoğunluğu temsil bir veri görselleştirmesi. Ayrıca bkz: [Tematik harita](#thermatic-map).

<a name="hybrid-imagery"></a> **Karma resimler**: uydu veya yol verileri ve çıktıklarını yayılan etiketleri olan havadan tanımayı.

## <a name="i"></a>I

<a name="iana"></a> **IANA**: Internet Atanmış Numaralar Yetkilisi kısaltması. Genel IP adresi ayırma'yı gözetir kar amacı gütmeyen bir grup.

<a name="isochrone"></a> **Isochrone**: alan, birisi alabileceği herhangi bir yönde nakliye türünü belirli bir süre içinde belirtilen bir konumdan bir isochrone tanımlar. Ayrıca bkz: [erişilebilir aralığı](#reachable-range).

<a name="isodistance"></a> **Isodistance**: bir konuma göz önünde bulundurulduğunda, bir isochrone, birisi alabileceği herhangi bir yönde nakliye türünü için belirtilen bir uzaklık içindeki alan tanımlar. Ayrıca bkz: [erişilebilir aralığı](#reachable-range).

## <a name="k"></a>K

<a name="kml"></a> **KML**: anahtar deliği biçimlendirme dili da bilinen olduğu noktaları, satırlar ve çokgenler gibi coğrafi vektör verilerini depolamak için ortak bir XML dosya biçimi. 

## <a name="l"></a>L

<a name="landsat"></a> **Landsat**: Multispectral, dünya gönderen yörüngedeki Uyduları geliştirilmiş olan Tarım, Ormancılık ve cartography gibi çok sayıda sektörde kullanılır tanımayı toplamak NASA tarafından.

<a name="latitude"></a> **Enlem**: olan Açısal uzaklık ekvatorun Kuzey veya Güney yönü dereceden cinsinden ölçülen genişliği.

<a name="level-of-detail"></a> **Ayrıntı düzeyi**: bkz [yakınlaştırma düzeyi](#zoom-level).

<a name="lidar"></a> **Lidar**: açık olan algılama ve değişen kısaltması. Yansıtıcı yüzeyleri için uzaklıkta ölçmek için Sever kullanan uzaktan algılama yöntemi.

<a name="linear-interpolation"></a> **Doğrusal enterpolasyon**: Bilinmeyen bir değere bilinen değerler arasındaki doğrusal uzaklık kullanarak tahmin.

<a name="linestring"></a> **LineString**: geometri bir satırı temsil etmek için kullanılan. Çoklu çizgi olarak da bilinir. 

<a name="localization"></a> **Yerelleştirme**: farklı dillere ve kültürlere desteği.

<a name="logistics"></a> **Lojistik**: kişiler, araçları, kaynakları veya varlıklar koordineli bir şekilde taşıma işlemi.

<a name="longitude"></a> **Boylam**: olan Açısal uzaklık Doğu veya Batı yönde asal meridyen kaynağından derece cinsinden ölçülen genişliği.

## <a name="m"></a>M

<a name="map-tile"></a> **Kutucuk harita**: dikdörtgen bir resim eşlem tuvali bölümünü temsil eder. Daha fazla bilgi için [yakınlaştırma düzeyleri ve döşeme Kılavuzu belgeleri](zoom-levels-and-tile-grid.md).

<a name="marker"></a> **İşaret**: olarak da bilinen bir PIN veya Raptiye, olan bir noktası konumunu bir haritada temsil eden bir simge.

<a name="mercator-projection"></a> **Mercator izdüşümünü**: sabit Elbette rhumb satırlar halinde bilinen çizgilerini temsil yeteneğini nedeniyle Denizcilik amaçlar için standart harita izdüşümü dönüştü bir Silindirik harita izdüşümü olarak düz, bunları ile korumak kesim meridyenleri. Tüm düz harita projeksiyonlar yeryüzünün doğru düzene karşılaştırıldığında harita boyutları ve şekiller çarpıtan. Daha küçük alanları kutupları yaklaştıkça harita üzerinde büyük görünür olacağı şekilde Mercator izdüşümünü ekvatorun gölgeden uzak alanları arttırdığınızda. 

<a name="multilinestring"></a> **MultiLineString**: geometri LineString nesnelerinin bir koleksiyonunu temsil eder. 

<a name="multipoint"></a> **MultiPoint**: geometri noktası nesnelerinin bir koleksiyonunu temsil eder.

<a name="multipolygon"></a> **MultiPolygon**: geometri Çokgen nesnelerinin bir koleksiyonunu temsil eder. Örneğin, Hawaii sınırları göstermek için sahip bir çokgenin her Adası özetlenen ve Hawaii sınırları, bu nedenle bir MultiPolygon olacaktır.

<a name="municipality"></a> **Belediye**: bir şehir veya şehir. 

<a name="municipality-subdivision"></a> **Belediye alt**: Komşuları veya "Şehir"merkezi gibi yerel adı gibi bir belediye bölümlemesini.

## <a name="n"></a>N

<a name="navigation-bar"></a> **Gezinti çubuğu**: yakınlaştırma düzeyi, aralık döndürme ayarlama ve temel bir harita katmanını değiştirme için kullanılan bir haritada denetim kümesi.

<a name="nearby-search"></a> **Arama yakındaki**: (crow ile uçuyor gibi) sabit, doğrusal bir uzaklık noktası olan bir uzamsal sorgu.

<a name="neutral-ground-truth"></a> **Nötr çığır Truth**: Etiket temsil ettiği bölge resmi dilinde ve yerel betikler varsa işleyen bir harita.

## <a name="o"></a>O

<a name="origin"></a> **Kaynak**: bir başlangıç noktası veya bir kullanıcı olduğu konum.

## <a name="p"></a>P

<a name="panning"></a> **Kaydırma**: bir sabit yakınlaştırma düzeyi sürdürürken herhangi bir yönde harita taşıma işlemi.

<a name="parcel"></a> **Paket**: land veya özellik sınır bir parçası.

<a name="pitch"></a> **Aralık**: Burada 0 baktığını doğrudan aşağı eşlemeyi dikey göre harita Eğim miktarı vardır.

<a name="point"></a> **Noktası**: geometri harita üzerinde tek bir konumu temsil eder. 

<a name="points-of-interest-poi"></a> **(POI) ilgi noktası**: iş, yer işareti veya ilgi ortak bir yerde.

<a name="polygon"></a> **Çokgen**: harita üzerinde bir alanı temsil eden bir düz geometri. 

<a name="polyline"></a> **Çoklu çizgi**: geometri bir satırı temsil etmek için kullanılan. LineString olarak da bilinir. 

<a name="position"></a> **Konum**: boylam, enlem ve bir noktanın yüksekliği (x, y, z koordinatları).

<a name="post-code"></a> **Posta kodu**: bkz [postalcode](#postal-code).

<a name="postal-code"></a> **Posta kodu**: harf veya sayı veya her ikisi de posta teslimini basitleştirmek için coğrafi bölgeler bölgelere ayırmak için bir ülke posta hizmeti tarafından kullanılan belirli bir biçimdeki bir dizi.

<a name="prime-meridian"></a> **Asal meridyen**: bir satırı temsil eden 0 derece boylam boylam. Genellikle, boylam değerleri 180 derece kadar westerly yönde dolaşırken azaltmak ve easterly yönde için -180 dolaşırken artırmak-derece. 

<a name="prj"></a> **PRJ**: veri kümesi bulunduğu öngörülen koordinat sistemi hakkında bilgi içeren bir şekil dosyası dosyası genellikle eşlik eden bir metin dosyası.

<a name="projection"></a> **Projeksiyon**: Öngörülen bir koordinat sistemini temel bir harita izdüşümü üzerinde gibi çapraz Mercator, Albers alan ve Robinson eşit. Bunlar, bir iki boyutlu Kartezyen koordinat düzlemi küresel yüzeyine dünya proje haritalarını olanağı sağlar. Öngörülen koordinat sistemi harita izdüşümler adlandırılır.

## <a name="q"></a>Q

<a name="quadkey"></a> **Quadkey**: quadtree döşeme sistemi içindeki bir kutucuk için bir temel 4 adresi dizini. Daha fazla bilgi için [yakınlaştırma düzeyleri ve döşeme Kılavuzu](zoom-levels-and-tile-grid.md) daha fazla bilgi için belgelere bakın.

<a name="quadtree"></a> **Önce Quadtree**: her düğümü tam olarak dört alt öğeleri olan bir veri yapısı. Bir kullanıcı bir düzey yakınlaştırır gibi her bir harita kutucuğunu alt dört kutucuğa sonlarını, Azure haritalar içinde kullanılan döşeme sistemi quadtree yapısı kullanır.  Daha fazla bilgi için [yakınlaştırma düzeyleri ve döşeme Kılavuzu](zoom-levels-and-tile-grid.md) daha fazla bilgi için belgelere bakın.

<a name="queries-per-second-qps"></a> **Sorgu başına ikinci (QPS)**: sorguları veya bir hizmeti veya platformu için bir saniye içinde yapılan isteklerin sayısı. 

## <a name="r"></a>R

<a name="radial-search"></a> **Radyal arama**: (crow ile uçuyor gibi) sabit, doğrusal bir uzaklık noktası olan bir uzamsal sorgu. 

<a name="raster-data"></a> **Hücresel veri**: hücreleri (veya piksel) matrisin düzenlenmiş satırları ve sütunları (veya bir kılavuz) burada her hücre sıcaklık gibi bilgileri temsil eden bir değer içeriyor. Izgara'nın dijital havadan fotoğraflar, tanımayı Uyduları, resimleri ve taranan eşlemeleri içerir.

<a name="raster-layer"></a> **Izgara katman**: Tarama görüntülerini oluşan bir döşeme katmanı.

<a name="reachable-range"></a> **Erişilebilir aralığı**: erişilebilir bir aralık içinde birisi alabileceği bir süre veya bir konumdan herhangi bir yönde hareket etmek nakliye türünü için uzaklık içinde alanı tanımlar. Ayrıca bkz: [Isochrone](#isochrone) ve [Isodistance](#isodistance).

<a name="remote-sensing"></a> **Uzak Algılama**: toplama ve bir uzaklıktan algılayıcı verileri yorumlama işlemi.

<a name="rest-service"></a> **REST hizmeti**: temsili durum transferi REST kısaltması anlamına gelir. Bir REST hizmeti, HTTP GET ve POST istekleri olan en yaygın yöntemleri iletişim kurmak için temel web teknolojisinden yararlanır bir URL tabanlı bir web hizmetidir. Bu tür hizmet, bana çok daha hızlı ve daha küçük geleneksel SOAP tabanlı Hizmetleri eğilimindedir.

<a name="reverse-geocode"></a> **Geocode ters**: bir koordinat alma ve olduğu bir haritada temsil adresi belirleme işlemi.

<a name="reproject"></a> **Reproject**: bkz [dönüştürme](#transformation).

<a name="rest-service"></a> **REST hizmeti**: temsili durum transferi kısaltması. Merkezi olmayan, dağıtılmış bir ortamda eşler arasında bilgi alışverişi için bir mimari. REST programlar bir işletim sistemi veya platformdan bağımsız olarak bir Tekdüzen Kaynak Konum Belirleyicisi (URL) için bir Köprü Metni Aktarım Protokolü (HTTP) istek göndererek iletişim kurmak ve verileri geri almak için farklı bilgisayarlarda sağlar.

<a name="route"></a> **Rota**: bir yolu da yönergeler için yol boyunca waypoints gibi ek bilgiler içerebilir, iki veya daha fazla konum arasında.

<a name="requests-per-second-rps"></a> **(RP'ler) saniye başına istek**: bkz [sorgular / saniye (QPS)](#queries-per-second-qps). 

<a name="rss"></a> **RSS**: kısaltması gerçekten basit dağıtım, kaynak açıklaması Framework (RDF) Site özeti veya kaynağa bağlı olarak zengin Site özeti. Farklı Web siteler arasında içerik paylaşımını bir basit, yapılandırılmış XML biçimi. RSS belgelerin yazar, tarih, başlık, kısa bir açıklama ve köprü metnine gibi önemli meta verilerin öğeleri içerir. Bir kullanıcı (veya bir RSS yayımcı hizmeti) karar verin malzemeleri değer, bu bilgiler yardımcı olur, daha fazla araştırma.

## <a name="s"></a>S

<a name="satellite-imagery"></a> **Uydu tanımayı**: düzlemleri ve düz aşağıyı Uyduları tarafından yakalanan görüntüler.

<a name="software-development-kit-sdk"></a> **Yazılım Geliştirme Seti (SDK)**: belge, örnek kod ve geliştiricilere uygulamalar oluşturmak için bir API kullanmak için örnek uygulamaları koleksiyonu.

<a name="shapefile-shp"></a> **Şekil dosyası (SHP)**: bir ESRI şekil dosyası da bilinir, konum, Şekil ve coğrafi özelliklerinin öznitelikleri depolamak için bir vektör veri depolama biçimidir. Bir şekil dosyası ilişkili dosyaları kümesi içinde depolanır.

<a name="spherical-mercator-projection"></a> **Küresel Mercator izdüşümünü**: bkz [Web Mercator](#web-mercator). 

<a name="spatial-query"></a> **Uzamsal sorgu**: uzamsal işlemi gerçekleştiren bir hizmete yapılan bir istek. Böyle bir Radyal arama olarak veya bir rota arama boyunca.

<a name="spatial-reference"></a> **Uzamsal başvuru**: tam olarak coğrafi varlıkları bulmak için kullanılan bir koordinat tabanlı yerel, Bölgesel ve genel sistem. Harita koordinatlarına göre gerçek dünyada konumlara ilişkilendirmek için kullanılan koordinat sistemini tanımlar. Uzamsal başvuruları doğru görüntüleme veya çözümleme için uzamsal veriler farklı katmanlar veya kaynaklarından tümleştirilebilir emin olun. Azure haritalar kullanan [EPSG:3857](https://epsg.io/3857) giriş geometri veri için başvuru sistemi ve WGS 84 koordinatı. 

<a name="sql-spatial"></a> **SQL uzamsal**: SQL Azure ve SQL Server 2008 ve üzeri yerleşik olan bir uzamsal işlevlerini gösterir. Bu uzamsal İşlevler, bağımsız olarak SQL Server kullanılan bir .NET kitaplığı olarak da kullanılabilir. Daha fazla bilgi için [uzamsal veriler (SQL Server) belgeleri](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) daha fazla bilgi için.

<a name="synchronous-request"></a> **Eşzamanlı istek**: bir HTTP isteği bir bağlantı açar ve bir yanıt bekler. Tarayıcılar sayfadan yapılan eşzamanlı HTTP istek sayısını sınırlayın. Aynı anda birden çok uzun süren zaman uyumlu istekleri yaptıysanız bu sınıra ulaşıldı ve istekleri diğer istekleri birine tamamlanıncaya kadar ertelendi.

## <a name="t"></a>T

<a name="telematics"></a> **Telematik**: gönderme, alma ve telekomünikasyon cihazları aracılığıyla bilgileri uzak nesnelerde denetim etkileyen ile birlikte depolama. 

<a name="temporal-data"></a> **Zamana bağlı veriler**: süreleri özellikle başvuran verileri veya tarih. Zamana bağlı veriler ışık durumda gibi ayrık olaylarına bakabilirsiniz; Tren gibi nesneleri taşıma; veya trafiği sensörlerden alınan sayımların gibi yinelenen gözlemler.

<a name="terrain"></a> **Arazi**: land sandy Arazi veya mountainous Arazi gibi belirli bir özelliği olan bir alanı.

<a name="thematic-maps"></a> **Tematik haritalar**: bir coğrafi bölge hakkında bir tema yansıtacak şekilde yapılan basit bir harita tematik haritasıdır. Bu tür bir eşleme için yaygın bir senaryo, yönetim bölgeleri gibi bazı veriler ölçüme göre ülkeler renk sağlamaktır.

<a name="tile-layer"></a> **Döşeme katmanı**: sürekli bir katmana harita kutucukları (dikdörtgen bölümlere) birleştirerek görüntülenen bir katman. Her iki tarama döşemeleri resim kutucukları veya kutucukları vektör. Izgara döşeme katmanları genellikle önceden işlenmiş ve sunucudaki bir görüntü olarak depolanır. Bu, depolama alanı çok fazla sürebilir. Vektör döşeme katmanları, istemci uygulamasının içinden hareket halindeyken işlenir, bu nedenle sunucu tarafı depolama gereksinimleri daha küçüktür.

<a name="time-zone"></a> **Saat dilimi**: yasal, ticari ve sosyal amaçları için Tekdüzen bir Standart Saati uygulamasını gözlediğini dünya bölgesi. Saat dilimlerini ülke ve onların alt bölümleri sınırları izleyin eğilimindedir.

<a name="transaction"></a> **İşlem**: Azure haritalar kullanan bir işlem tabanlı lisanslama modeli burada;

- Bir işlem istenen her 15 eşlemesi ya da trafiği döşeme için oluşturulur.
- Bir işlem oluşturulur için her API çağrısı bir Azure haritalar Hizmetleri gibi arama veya yönlendirme.

<a name="transformation"></a> **Dönüştürme**: farklı coğrafi koordinat sistemi arasında veri dönüştürme işlemidir. Örneğin, Birleşik Krallık'ta yakalanan ve OSGB 1936 coğrafi koordinat sistemini temel alan bazı veriler olabilir. Azure haritalar kullanan [EPSG:3857](https://epsg.io/3857) WGS84 koordinat başvuru sistemi değişken. Bu nedenle doğru verileri görüntülemek için başka bir sistemden dönüştürülmüş koordinatları sahip gerekecektir.

<a name="traveling-salesmen-problem-tsp"></a> **Gezici satıcı sorun (TSP)**: içinde bir satış temsilcisi gerekir Bul durdurur, bir dizi ziyaret etmek için en verimli şekilde Hamiltonian devre sorun sonra başlangıç konumu döndürür.  

<a name="trilateration"></a> **Trilateration**: tüm üç nokta arasındaki uzaklıkları ölçerek yeryüzünün diğer iki nokta ile ilgili bir noktasında konumunu belirleme işlemi.

<a name="turn-by-turn-navigation"></a> **Bırakma tarafından Aç Gezinti**: sonraki öngören rota yönergeleri her adım için bir rota kullanıcıları olarak sağlayan bir uygulama yaklaşıyor.

## <a name="v"></a>V

<a name="vector-data"></a> **Vektör verilerini**: koordinatına dayalı noktaları, çizgi veya çokgenler temsil edilen veri.

<a name="vector-tile"></a> **Vektör kutucuk**: harita denetimi aynı döşeme sistemini kullanarak Jeo-uzamsal vektör verilerini depolamak için bir açık veri belirtimi. Ayrıca bkz: [döşeme katmanı](#tile-layer).

<a name="vehicle-routing-problem-vrp"></a> **Araç yönlendirme sorunu (VRP)**: bir sınıf içinde sıralı yollar taşıtlardan filosundan için bir dizi hesaplanır kısıtlamaları kümesini olarak dikkate alma sırasında sorunları. Bu kısıtlamalar, teslim zaman pencereleri, birden çok yol kapasiteler gibi şeyler dahil ve seyahat süresi kısıtlamaları.

<a name="voronoi-diagram"></a> **Voronoi diyagram**: alanları veya geometrik bir nesne (genellikle noktası özellikleri) çevreleyen hücreler alanına ilişkin bir bölüm. Bu hücre veya çokgenler Delaunay üçgenler ölçütleri karşılaması gerekir. Bir alanı içindeki tüm konumları, kümedeki herhangi bir nesneye daha çevreleyen nesnesine yakındır. Voronoi diyagramları, genellikle coğrafi özellikler etrafında etki alanlarını ayırmak için kullanılır. 

## <a name="w"></a>W

<a name="waypoint"></a> **Güzergah noktası**: bir güzergah noktası enlemini ve boylamını gezinme amacıyla kullanılan tarafından tanımlanan bir belirtilen coğrafi konumdur. Genellikle birisi bir rota üzerinden gider bir noktayı temsil etmek için kullanılır.

<a name="waypoint-optimization"></a> **Güzergah noktası iyileştirme**: waypoints sağlanan tüm waypoints geçirmek için gerekli olan uzaklığı ve seyahat süresini en aza indirmek için bir dizi yeniden sıralama işlemi. Genellikle olarak adlandırılan [gezici satıcı sorun](#traveling-salesmen-problem-tsp) veya [Vehicle yönlendirme sorununu](#vehicle-routing-problem-vrp) iyileştirme karmaşıklığına bağlı olarak.

<a name="web-map-service-wms"></a> **Web (WMS) Map hizmet**: WMS görüntü tabanlı eşleme hizmeti tanımlayan bir açık coğrafi Consortium (OGC) standardı olan. WMS Hizmetleri, isteğe bağlı bir eşlem içindeki belirli alanları için harita görüntüleri sağlar. Görüntüleri, önceden işlenmiş simgesi içerir ve birkaç adlandırılmış stilleri birinde varsa hizmet tarafından tanımlanan çizilebilir.

<a name="web-mercator"></a> **Mercator web**: Küresel Mercator izdüşümünü Mercator izdüşümünü hafif bir değişken olarak da bilinen bir kullanılan öncelikli olarak Web tabanlı eşleme programlarında. Formüller olarak kullanılan standart Mercator izdüşümünü küçük ölçekli haritalar için kullanır. Ancak, küresel formülleri hiç ölçeklendirir büyük ölçekli Mercator normalde eşleştirir ancak Web Mercator kullandığı projeksiyonun ellipsoidal formu kullanın. Tutarsızlık, global bir ölçekte imperceptible ancak aynı ölçekte ellipsoidal Mercator eşler true öğesinden biraz cluster_count_prıor yerel alanları haritaları neden olur.

<a name="wgs84"></a> **WGS84**: eşleme yüzeyinde konumlara uzamsal koordinatları ilişkilendirmek için kullanılan sabit bir dizi. WGS84 datum en çevrimiçi eşleme sağlayıcılarının ve GPS aygıt tarafından kullanılan standart bir bileşendir. Azure haritalar kullanan [EPSG:3857](https://epsg.io/3857) WGS84 koordinat başvuru sistemi değişken.

## <a name="z"></a>Z

<a name="z-coordinate"></a> **Z koordinatı**: bkz [yüksekliği](#altitude). 

<a name="zip-code"></a> **Posta kodu**: bkz [postalcode](#postal-code).

<a name="Zoom level"></a> **Yakınlaştırma düzeyi**: harita ne kadarının görünür olduğunu ve ayrıntı düzeyini belirtir. Tüm bir düzey 0 uzaklaştırılacağını, tam dünya haritası genellikle görünüm olacaktır, ancak sınırlı ayrıntıları ülke adları ve kenarlıklar gibi yanı ve Okyanus adları gösterilir. İçinde daha yakın düzeyi 17 uzaklaştırılacağını, harita birkaç Şehir blokları ayrıntılı yol bilgilerini ile bir alanı görüntüler. Daha fazla bilgi için [yakınlaştırma düzeyleri ve döşeme Kılavuzu](zoom-levels-and-tile-grid.md) belgeleri.

