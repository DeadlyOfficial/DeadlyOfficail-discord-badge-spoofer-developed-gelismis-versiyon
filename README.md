# Discord Badge Spoofer

Discord hesabınıza oyun süresi ve oyun sayısı rozetleri eklemek için Türkçe arayüzlü bir komut satırı aracı.
Not:Kod Geliştirilmiş bir versiyondur
> UYARI: Bu araç Discord Hizmet Şartları'nı ihlal edebilir ve hesabınızın kısıtlanma riski vardır. Lütfen kendi sorumluluğunuzda kullanın.

## İçindekiler

- [Özellikler](#özellikler)
- [Gereksinimler](#gereksinimler)
- [Kurulum](#kurulum)
- [Token Nasıl Alınır](#token-nasıl-alınır)
- [Cookie Nasıl Alınır](#cookie-nasıl-alınır)
- [Fingerprint Nasıl Alınır](#fingerprint-nasıl-alınır-isteğe-bağlı)
- [Kullanım](#kullanım)
- [Menü Seçenekleri](#menü-seçenekleri)
- [Örnek Kullanım](#örnek-kullanım)
- [Hata İşleme](#hata-işleme)
- [Dosya Yapısı](#dosya-yapısı)
- [Yapılandırma](#yapılandırma)
- [SSS](#sss)
- [Güvenlik Notları](#güvenlik-notları)
- [Yasal Uyarı](#yasal-uyarı)
- [Lisans](#lisans)

## Özellikler

- Oyun Süresi Ekle: Tüm oyunlara belirtilen saat süresini toplu olarak ekler
- Oyun Sayısını Artır: Oyunları oynanmış olarak işaretler
- AFK Modu: Sonsuz döngü, Ctrl+C ile durdurulana kadar sürekli süre ekler
- Tekrar Sayısı: Her modda birden fazla tur tekrarlamayı destekler
- Otomatik Yetkilendirme Yenileme: Analytics Token süresi dolduğunda otomatik olarak yeniden alınır
- Renkli Türkçe Arayüz
- Toplu Gönderim: 50'lik paketler halinde gönderim, hızlı ve verimli
- Oturum Kaydetme: Token ve Cookie science_state.json dosyasında saklanır, her seferinde girmeniz gerekmez

## Gereksinimler

- Python 3.10+
- İnternet bağlantısı
- Discord hesap Token ve Cookie (aşağıdaki nasıl alınır bölümüne bakın)

Harici kütüphane gerekmez, yalnızca Python standart kütüphanesi kullanılır.

## Kurulum

### 1. Repoyu klonlayın

git clone https://github.com/kullanici/discord-badge-spoofer.git
cd discord-badge-spoofer

### 2. Python sürümünüzü kontrol edin

python --version

Python 3.10 veya üzeri olmalıdır.

### 3. Programı çalıştırın

python discord_badge_spoofer.py

İlk çalıştırmada Token ve Cookie girmeniz istenecek, bunlar science_state.json dosyasına kaydedilecek.

## Token Nasıl Alınır

Token, Discord hesabınızın oturum kimlik bilgisidir. Onu ele geçiren herkes hesabınıza giriş yapabilir, bu yüzden kimseyle paylaşmayın.

### Yöntem A: Tarayıcı Geliştirici Araçları (Local Storage)

1. Tarayıcıda discord.com/app adresini açın ve giriş yapın
2. F12 tuşuna basarak Geliştirici Araçları'nı açın
3. Application (Uygulama) sekmesine geçin
4. Sol tarafta Local Storage seçeneğini genişletin, https://discord.com öğesine tıklayın
5. Sağ taraftaki anahtar listesinde token bulun, değeri kopyalayın

### Yöntem B: Ağ İsteği Başlığı (Network)

1. Geliştirici Araçları'nda Network (Ağ) sekmesine geçin
2. Filtre kutusuna science yazın
3. Sayfayı yenileyin (F5), science isteğini bulun ve tıklayın
4. Sağ taraftaki Headers bölümünde, Request Headers içinde authorization alanını bulun, değeri Token'ınızdır

### Yöntem C: Konsol (Console)

1. Geliştirici Araçları'nda Console sekmesine geçin
2. Şu komutu yapıştırın ve Enter'a basın:

(webpackChunkdiscord_app.push([[''],{},e=>{m=[];for(let c in e.c)m.push(e.c[c])}]),m).find(m=>m?.exports?.default?.getToken!==void 0).exports.default.getToken()

3. Çıkan değer Token'ınızdır

> Token'ınızı asla kimseyle paylaşmayın. Token ele geçirilirse hesabınız çalınabilir.

## Cookie Nasıl Alınır

Bu araç Cloudflare korumasını atlamak için cf_clearance cookie'sine ihtiyaç duyar. Bu cookie IP adresine ve User-Agent'a bağlıdır, farklı bir ortamda kullanıldığında geçersiz olur.

1. Tarayıcıda Discord'a giriş yapmış durumda, F12 ile Geliştirici Araçları'nı açın
2. Network (Ağ) sekmesine geçin
3. Herhangi bir isteğe tıklayın (örneğin science isteği)
4. Sağ taraftaki Headers bölümünde, Request Headers içinde cookie: satırını bulun
5. cookie: sonrasındaki tam içeriği kopyalayın (cf_clearance=... ve diğer cookie'ler dahil)

Örnek cookie formatı:

cf_clearance=abc123...; __dcfduid=xyz...; __sdcfduid=...; __cfruid=...

> İpucu: Cloudflare cookie'sinin geçerlilik süresi kısadır (genellikle 30-60 dakika), süresi dolduğunda yeniden almanız gerekir. Programda 3 - Yetkilendirme Yenile seçeneği ile yeni cookie yapıştırabilirsiniz.

## Fingerprint Nasıl Alınır (İsteğe Bağlı)

Fingerprint, Discord istemcisinin oyun başlatırken gönderdiği executable_fingerprint alanıdır, yalnızca oyun sayısı sayacını etkiler, oyun süresini etkilemez. Ayarlanmazsa, süre rozeti normal şekilde çalışır, yalnızca oyun sayısı sayacı çalışmayabilir.

### Alma Adımları

1. Discord masaüstü istemcisini açın (tarayıcı sürümü değil)
2. Discord'un tanıyabileceği bir oyunu başlatın
3. Discord'da Ctrl + Shift + I tuşlarına basarak Geliştirici Araçları'nı açın
4. Network (Ağ) sekmesine geçin, filtre kutusuna science yazın
5. Oyun algılama bilgisi gönderen science isteğini bulun, tıklayın
6. Sağ taraftaki Payload sekmesinde, launch_game olayını bulun
7. launch_game olayının properties bölümünde executable_fingerprint alanını bulun, değeri kopyalayın (genellikle uzun bir dize)

### Ayarlama Yöntemi

science_state.json dosyasını açın (programı bir kez çalıştırdıktan sonra oluşturulur), fingerprint alanını bulun, kopyaladığınız değeri tırnak içine yapıştırın:

{
  "token": "...",
  "cookie": "...",
  "fingerprint": "buraya kopyaladığınız executable_fingerprint değerini yapıştırın",
  "analytics_token": "...",
  "fetched_at": 0
}

Dosyayı kaydedin, programı yeniden başlatın, uyarı kaybolur.

> Fingerprint değeri hesabınıza ve çalıştırılabilir dosyanıza bağlıdır, başkalarının değeri kullanılamaz, kendi istemcinizden almanız gerekir.

## Kullanım

Programı çalıştırdıktan sonra menü görüntülenir:

1  Oyun Süresi Ekle (Saat)
2  Oyun Sayısını Artır
3  Yetkilendirme Yenile
4  Çıkış
5  AFK Modu - Sonsuz süre ekleme

## Menü Seçenekleri

| Seçenek | Açıklama |
|---------|----------|
| 1 | Oyun süresi ekleme. Saat girin, ardından tekrar sayısı sorulur. |
| 2 | Oyun sayısını artırma. Tüm oyunlar oynanmış olarak işaretlenir. |
| 3 | Yetkilendirme yenileme. Yeni cookie girin ve analytics token yeniden alınır. |
| 4 | Çıkış. |
| 5 | AFK modu. Sonsuz tekrar ile sürekli süre ekler. Ctrl+C ile durdurulur. |

## Örnek Kullanım

### Örnek 1: 2 saat süre ekleme (3 tur)

> 1
Oyun başına saat: 2
AFK / tekrar sayısı (boş = sonsuz, 0 = sonsuz): 3

Her oyuna 2 saat süre eklenir, toplam 3 tur tekrarlanır.

### Örnek 2: Oyun sayısını artırma (1 tur)

> 2
Oyun sayısı / tekrar sayısı (boş = sonsuz, 0 = sonsuz): 1

Tüm oyunlar 1 dakika süreyle oynanmış olarak işaretlenir.

### Örnek 3: AFK Modu

> 5
Oyun başına saat (AFK): 1

Her turda 1 saat süre eklenir, Ctrl+C ile durdurulana kadar devam eder.

### Örnek 4: Yetkilendirme yenileme

> 3
Yeni çerez header'ını yapıştırın (boş bırakırsanız mevcut korunur): cf_clearance=...

Yeni cookie kaydedilir ve analytics token yeniden alınır.

## Hata İşleme

| Hata | Anlamı | Çözüm |
|------|--------|-------|
| 401 / 403 | Token veya Cookie süresi dolmuş | Menüden 3 seçeneği ile yetkilendirmeyi yenileyin |
| Fingerprint uyarısı | Oyun sayısı sayacı çalışmayabilir | Süre rozeti etkilenmez, isteğe bağlı olarak fingerprint ayarlayın |
| Token reddedildi | Token geçersiz | Yeni token girin |
| analytics_token bulunamadı | Discord API yanıt vermiyor | İnternet bağlantınızı kontrol edin, tekrar deneyin |
| Oyun listesi boş | Oyun veritabanı indirilemedi | İnternet bağlantınızı kontrol edin |

## Dosya Yapısı

.
- discord_badge_spoofer.py   (Ana program)
- science_state.json         (Token/Cookie deposu, git'e YÜKLEMEYİN)
- data/games.json            (Oyun veritabanı, otomatik indirilir)
- README.md

## Yapılandırma

Kodun başındaki sabitler:

| Sabit | Varsayılan | Açıklama |
|-------|-----------|----------|
| BATCH_SIZE | 50 | Her istekte gönderilen oyun sayısı |
| BATCH_DELAY | 0.3 | Paketler arası bekleme (saniye) |
| AUTH_MAX_AGE | 43200 | Token ömrü (12 saat) |
| CLIENT_VERSION | 1.0.9253 | Discord istemci sürümü |
| CLIENT_BUILD_NUMBER | 594031 | Discord istemci derleme numarası |

## SSS

S: Fingerprint ayarlamazsam ne olur?
C: Oyun süresi rozeti normal şekilde çalışır. Yalnızca oyun sayısı sayacı çalışmayabilir. Önce ayarlamadan deneyin, etkisi olmazsa sonra alın.

S: Token ve Cookie ne kadar süre geçerli?
C: Token genellikle uzun süre geçerlidir (şifre değiştirilmedikçe veya çıkış yapılmadıkça). Cloudflare Cookie'sinin geçerlilik süresi kısadır, genellikle 30-60 dakika, süresi dolduğunda yeniden alınması gerekir.

S: Hesap yasaklanır mı?
C: Risk vardır. Bu araç Discord Hizmet Şartları'nı ihlal eder, kendi sorumluluğunuzda kullanın.

S: Rozetler ne zaman görünür?
C: Hemen görünmeyebilir, Discord'un güncellemesi birkaç saat sürebilir.

S: Programı çalıştırırken hata alıyorum, ne yapmalıyım?
C: Önce Python sürümünüzün 3.10+ olduğundan emin olun. İnternet bağlantınızı kontrol edin. Sorun devam ederse token ve cookie'nizi yenileyin.

S: Aynı anda birden fazla hesap kullanabilir miyim?
C: Evet, ancak her hesap için ayrı bir science_state.json dosyası kullanmanız gerekir. Her seferinde token ve cookie'yi değiştirmeniz yeterlidir.

S: Program çalışırken bilgisayarı kapatabilir miyim?
C: Hayır, program çalışırken bilgisayarı kapatmayın. İşlem tamamlanana kadar bekleyin.

S: Ne kadar süre ekleyebilirim?
C: Discord'un bir sınırı yoktur, ancak çok yüksek değerler (örneğin 100.000 saat) şüpheli görünebilir. Makul değerler kullanın.

S: Oyun sayısı rozeti neden gelmiyor?
C: Fingerprint ayarlanmamış olabilir. Yukarıdaki fingerprint bölümüne bakın. Ayrıca Discord'un rozet güncellemesi birkaç saat sürebilir.

S: Cookie süresi dolduğunda ne yapmalıyım?
C: Menüden 3 seçeneğini seçin, yeni cookie yapıştırın ve yetkilendirmeyi yenileyin.

S: Programı nasıl durdurabilirim?
C: Ctrl+C tuşlarına basarak programı durdurabilirsiniz.

## Güvenlik Notları

- Token'ınızı asla kimseyle paylaşmayın. Token ele geçirilirse hesabınız çalınabilir.
- science_state.json dosyasını asla GitHub'a yüklemeyin. .gitignore dosyasına ekleyin.
- Programı güvendiğiniz bir bilgisayarda çalıştırın.
- Ortak kullanılan bilgisayarlarda token ve cookie'lerinizi temizleyin.
- Program kapandıktan sonra science_state.json dosyasını silmek isterseniz silebilirsiniz.


## Yasal Uyarı

Bu araç yalnızca eğitim ve araştırma amacıyla sağlanmıştır. Discord Hizmet Şartları'nı ihlal ettiği için, kullanımdan doğacak tüm sorumluluk kullanıcıya aittir. Yazarlar herhangi bir kayıptan sorumlu değildir.

Bu aracı kullanarak:

- Discord Hizmet Şartları'nı ihlal etmiş olursunuz
- Hesabınızın kısıtlanma veya yasaklanma riskini kabul etmiş olursunuz
- Tüm sorumluluğu size ait olmak üzere kullanırsınız


## Lisans

MIT License

Copyright (c) 2026 kullanici

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Deadly Official Sunar discord.gg/gg/EDbZuPKpT4
