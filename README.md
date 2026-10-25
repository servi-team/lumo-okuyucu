# Proton Lumo Sohbet Okuyucu

Proton Lumo'dan dışa aktarılan sohbet JSON dosyalarını tarayıcıda okumak, düzenlemek ve arşivlemek için tek dosyalık bir uygulama.

Kurulum, derleme adımı veya harici bir kütüphane gerektirmez. Tüm HTML, CSS ve JavaScript `lumo-okuyucu.html` içindedir.

> Veriler tarayıcıda kalır. Sohbetler, notlar, klasörler ve medyalar herhangi bir sunucuya veya üçüncü taraf hizmete gönderilmez. JSON içinde uzak medya bağlantıları varsa tarayıcı bu dosyaları ilgili adresten yükleyebilir.

## İçindekiler

- [Öne çıkan özellikler](#öne-çıkan-özellikler)
- [Hızlı başlangıç](#hızlı-başlangıç)
- [Kullanım](#kullanım)
  - [Sohbet ekleme](#sohbet-ekleme)
  - [Sohbet listesi, favoriler ve klasörler](#sohbet-listesi-favoriler-ve-klasörler)
  - [Arama](#arama)
  - [Mesajlar](#mesajlar)
  - [Mesaj sürümleri](#mesaj-sürümleri)
  - [Medya](#medya)
  - [Notlar](#notlar)
  - [Dışa aktarma](#dışa-aktarma)
- [Klavye kısayolları](#klavye-kısayolları)
- [Veri saklama ve gizlilik](#veri-saklama-ve-gizlilik)
- [Desteklenen JSON yapıları](#desteklenen-json-yapıları)
- [Dosya yapısı](#dosya-yapısı)
- [Geliştirme](#geliştirme)
- [Sınırlamalar](#sınırlamalar)
- [Tarayıcı desteği](#tarayıcı-desteği)

## Öne çıkan özellikler

- Proton Lumo JSON dışa aktarımlarını içe aktarma
- `mapping` ağacı, `messages` listesi, sarmalayıcılar, dallanmış mesajlar ve ekleri otomatik algılama
- Sohbet listesi, favoriler, klasörler ve klasör notları
- Son güncelleme, oluşturulma, başlık, mesaj sayısı ve özel sıralama
- Tüm sohbetlerde arama ve açık sohbet içinde arama
- Mesajları düzenleme, yeni kullanıcı/asistan mesajı ekleme ve mesaj silme
- Mesaj sürüm seçici: dallanmış sürümler ve düzenleme geçmişi arasında geçiş
- Sürümleri tek tek silme; diğer sürümler korunur
- JSON'daki medyaları otomatik algılama, diskten medya ekleme, eksik dosyayı değiştirme
- Medya önizleme, indirme ve kaldırma
- Sohbeti yeniden adlandırma
- Sohbet ve klasör notları
- Açık/koyu tema
- Daraltılabilir ve genişliği ayarlanabilir sohbet paneli
- Daraltılabilir medya bölümü
- Markdown dışa aktarma ve JSON yedekleme
- IndexedDB kalıcılığı; localStorage yedeği ve eski sürüm verilerini taşıma
- Mobil uyumlu arayüz

## Hızlı başlangıç

### Doğrudan açma

`lumo-okuyucu.html` dosyasını modern bir tarayıcıya sürükleyerek de açabilirsiniz. Ancak kalıcı veri ve medya davranışı tarayıcıya göre değişebilir. Sorun yaşarsanız yerel sunucu yöntemini kullanın.

### Yerel sunucu ile

Proje klasöründe bir terminal açın:

```bash
python3 -m http.server 8765 --bind 127.0.0.1
```

Ardından tarayıcıda şu adresi açın:

```text
http://127.0.0.1:8765/lumo-okuyucu.html
```

Yerel sunucuyu deneyebilirsiniz çünkü bazı tarayıcılar `file://` adreslerinde IndexedDB veya yerel dosya erişimini kısıtlayabilir.

## Kullanım

### Sohbet ekleme

1. Sol paneldeki **JSON dosyası ekle** düğmesine tıklayın.
2. Bir veya birden fazla Lumo JSON dosyası seçin.
3. JSON dosyalarını uygulama penceresine sürükleyip bırakarak da içe aktarabilirsiniz.

Sürükle-bırak sırasında:

- `.json` dosyaları sohbet olarak içe aktarılır.
- Diğer dosyalar açık sohbete medya olarak eklenir.
- Aynı sohbet kimliğine sahip bir JSON tekrar içe aktarılırsa sohbet güncellenir. Favori durumu, klasörler, notlar ve elle eklenen medyalar korunur.

Kaynak JSON dosyaları değiştirilmez. Yeniden adlandırma, notlar, favoriler ve klasörler yalnızca tarayıcıdaki uygulama kaydında saklanır.

### Sohbet listesi, favoriler ve klasörler

- Favori sohbetler listenin üstündeki ayrı bölümde sabitlenir.
- **Yeni klasör** ile ad, renk ve not içeren klasörler oluşturabilirsiniz.
- Bir sohbeti klasöre sürükleyerek ekleyebilir veya sohbet menüsünden klasör seçebilirsiniz.
- Klasörlerin notları ve sohbetlerin notları otomatik kaydedilir.
- **Klasörsüz** görünümüyle hiçbir klasöre eklenmemiş sohbetleri görebilirsiniz.
- Sıralama menüsünden son güncelleme, oluşturulma, başlık veya mesaj sayısına göre sıralama yapabilirsiniz.
- **Özel sıra** seçiliyken sohbetleri sürükleyerek kendi sıranızı oluşturabilirsiniz.
- Bir sohbeti listeden kaldırmak, tarayıcıdaki sohbet kaydını ve o sohbete ait medyaları siler; kaynak JSON dosyasına dokunmaz.

Sohbet başlığını yeniden adlandırmak için:

- Sohbet menüsündeki **Yeniden adlandır** seçeneğini kullanın veya
- Başlığa çift tıklayın.

### Arama

- **Ctrl + K** veya sol paneldeki arama alanı: tüm sohbetlerde arama.
- Arama; sohbet başlığı, sohbet notu, mesaj metinleri ve medya adlarında eşleşme arar.
- **Ctrl + F** veya üst çubuktaki büyüteç: açık sohbet içinde arama.
- Sohbet içi aramada **Enter** sonraki, **Shift + Enter** önceki eşleşmeye gider.
- **Esc** aramayı veya açık pencereyi kapatır.

### Mesajlar

Her mesajın altındaki düğmelerle:

- Mesaj metnini kopyalayabilirsiniz.
- Mesajı düzenleyebilirsiniz.
- Mesajı silebilirsiniz.

Mesaj düzenleme penceresinde:

- Metni değiştirebilirsiniz.
- Mevcut ekleri kaldırabilirsiniz.
- Bilgisayardan yeni dosya ekleyebilirsiniz.
- Kullanıcı ve asistan mesajları düzenlenebilir.

Sohbetin sonundaki **Bu sohbete mesaj ekle** alanından:

- **Sen olarak ekle** ile kullanıcı mesajı,
- **Lumo olarak ekle** ile asistan mesajı ekleyebilirsiniz.

Bir mesaj silindiğinde, o mesajın altındaki mesajlar kopmaz; bir üst mesaja bağlanır. Mesaja bağlı medyalar sohbetin medya galerisinde kalmaya devam eder.

### Mesaj sürümleri

Düzenlenen veya JSON içinde dallanmış olan mesajlarda mesaj altında kompakt bir sürüm seçici görünür:

- **Güncel** veya **Sürüm N** düğmesine tıklayın.
- Açılan menüde sürümler kısa etiketler ve kısa önizlemelerle listelenir.
- Bir sürüm satırına tıklayarak o sürüme geçin.
- Her sürüm satırındaki çöp kutusu simgesiyle o sürümü silebilirsiniz.
- Güncel metin silinirse en son eski sürüm güncel metin olarak kullanılır.
- Dallanmış bir sürüm silinirse o dala bağlı sonraki mesajlar da silinir; diğer dallar korunur.
- Sürüm değiştirildiğinde metin ve varsa ekli dosyalar birlikte geri yüklenir.
- Düzenleme sırasında eklenen veya kaldırılan dosyalar da sürüm geçmişine kaydedilir.

Eski mesajların tamamı ayrı kartlar halinde gösterilmez; yalnızca sürüm seçici ve kısa etiketler gösterilir.

### Medya

**Bu sohbetteki medyalar** bölümü:

- JSON içinde bulunan medyaları otomatik olarak listeler.
- JSON'da dosya bağlantısı olmayan veya erişilemeyen medyaları eksik olarak işaretler.
- **Medya ekle** ile bilgisayardan dosya eklemenizi sağlar.
- Eksik bir medyayı yerel dosyayla değiştirmenize izin verir.
- Görsel önizleme, indirme ve kaldırma sunar.
- Başlıktaki daraltma düğmesiyle daraltılabilir. Daraltma durumu sohbet bazında saklanır.
- Yeni medya eklendiğinde bölüm otomatik olarak açılır.

Medyalar IndexedDB'de saklanır. JSON yedeği alındığında yerel/Blob medya içerikleri data URL olarak yedeğe eklenir; uzak medya bağlantıları URL olarak kalır.

### Notlar

- Üst çubuktaki not düğmesiyle sohbet notu ekleyebilirsiniz.
- Klasör satırındaki not düğmesiyle klasör notu ekleyebilirsiniz.
- Notlar yazarken otomatik kaydedilir.
- Notlar yalnızca bu tarayıcıda saklanır ve kaynak JSON dosyasını değiştirmez.

### Dışa aktarma

Üst çubuktaki **Dışa aktar** düğmesi iki seçenek sunar:

- **Markdown olarak indir**: Açık sohbeti başlık, mesajlar, not, sürüm geçmişi ve medya listesiyle birlikte `.md` dosyası olarak indirir.
- **Tüm verileri yedekle (JSON)**: Tüm sohbetleri, klasörleri, notları, favorileri, sıralamayı ve medyaları tek bir JSON dosyasında saklar.

JSON yedeği arşivleme ve saklama içindir. Uygulamada otomatik bir "yedekten geri yükle" düğmesi bulunmaz.

## Klavye kısayolları

| Kısayol | İşlev |
| --- | --- |
| `Ctrl + K` | Tüm sohbetlerde ara |
| `Ctrl + F` | Açık sohbette ara |
| `Enter` | Sohbet içi aramada sonraki eşleşme |
| `Shift + Enter` | Sohbet içi aramada önceki eşleşme |
| `Esc` | Aramayı veya açık pencereyi kapat |
| `Ctrl + Enter` | Not penceresinde notu hemen kaydet |

Ek olarak:

- Bir sohbeti klasöre sürükleyip bırakabilirsiniz.
- **Özel sıra** seçiliyken sohbetleri sürükleyerek sıralayabilirsiniz.
- Sohbet başlığına çift tıklayarak yeniden adlandırabilirsiniz.

## Veri saklama ve gizlilik

Uygulama verileri tarayıcıda saklar:

- **IndexedDB**: `lumo-reader-db` veritabanı, `state` deposu, `app` anahtarı.
- **localStorage yedeği**: IndexedDB kullanılamazsa `lumo-reader-state-v4` anahtarı.
- **Tema tercihi**: `lumo-reader-theme` anahtarı.
- Eski sürüm verileri varsa uygulama bunları otomatik olarak yeni yapıya taşımaya çalışır.

Önemli notlar:

- Veriler tarayıcıya ve profile özeldir. Farklı tarayıcıda veya gizli pencerede görünmez.
- Tarayıcı site verilerini temizlerseniz sohbetler, notlar, klasörler ve medyalar silinir.
- Yedek almak için **Dışa aktar → Tüm verileri yedekle (JSON)** seçeneğini kullanın.
- Uygulama sohbet verilerini bir sunucuya göndermez. JSON içinde uzak medya URL'leri varsa tarayıcı bu adreslere istek yapabilir.
- Kaynak JSON dosyaları yalnızca okunur; uygulama bu dosyaları değiştirmez.

## Desteklenen JSON yapıları

Uygulama aşağıdaki yapıları algılamaya çalışır:

- `mapping` ağacı ve `current_node` / `currentNode` bilgisi
- `messages`, `message_list` veya `chat` dizileri
- `conversations`, `conversation`, `chat`, `data`, `result`, `items` gibi sarmalayıcılar
- `parent` / `parent_id` ilişkileri
- Aynı üst mesaja bağlı dallanmış mesajlar
- `versions`, `revisions`, `edit_history`, `editHistory`, `history`, `previous_versions` gibi satır içi sürüm alanları
- JSON içindeki ekler ve medya bağlantıları
- `data:` URL'leri, uzak URL'ler ve eksik medya kayıtları
- Sohbet kimliği, başlık, oluşturulma ve güncellenme zamanları

Desteklenmeyen bir Lumo şemasıyla karşılaşırsanız JSON dosyasının yapısını kontrol edin. Uygulama geçerli bir sohbet bulamazsa dosyayı içe aktarmaz.

## Dosya yapısı

```text
lumo-okuyucu/
├── lumo-okuyucu.html                 # HTML aracı (tek dosya)
└── README.md                         # Bu dosya
```

## Geliştirme

- Uygulama saf HTML, CSS ve JavaScript kullanır.
- Harici kütüphane, paket yöneticisi veya derleme adımı yoktur.
- Ana uygulama sürümü `APP_VERSION` sabitiyle tutulur.
- Tüm uygulama kodu `lumo-okuyucu.html` içindeki `<style>` ve `<script>` bloklarındadır.

JavaScript sözdizimini kontrol etmek için script bloğunu ayırıp Node.js ile doğrulayabilirsiniz:

```bash
python3 - <<'PY'
import re
s = open("lumo-okuyucu.html", encoding="utf-8").read()
js = re.search(r"<script>(.*)</script>", s, re.S).group(1)
open("/tmp/lumo.js", "w", encoding="utf-8").write(js)
PY
node --check /tmp/lumo.js
```

## Sınırlamalar

- Lumo dışa aktarımında medya dosyaları yer almıyorsa bunlar otomatik olarak kurtarılamaz; **Medya ekle** ile elle eklenmelidir.
- Çok büyük medyalar tarayıcı depolama kotasını aşabilir. Bu durumda yedek alın ve gereksiz medyaları kaldırın.
- JSON yedeği indirilebilir; otomatik geri yükleme akışı yoktur.
- Çok büyük sohbetlerde tüm mesajlar aynı anda işlendiği için ilk açılış yavaşlayabilir.
- `file://` ile açıldığında bazı tarayıcılar IndexedDB veya yerel dosya erişimini kısıtlayabilir; yerel sunucu kullanın.
- Farklı tarayıcılar aynı IndexedDB verisini paylaşmaz.

## Tarayıcı desteği

Güncel Chrome, Edge, Firefox ve Safari sürümleri önerilir. Uygulama şu tarayıcı özelliklerini kullanır:

- IndexedDB
- Blob ve File API
- `color-mix()` desteği
- Modern CSS ve JavaScript özellikleri

Eski tarayıcılarda görsel veya işlevsel sorunlar görülebilir.
