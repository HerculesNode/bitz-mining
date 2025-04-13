# 🚀 Eclipse Üzerinde Bitz Miner CLI Rehberi

## Bitz Nedir?
- Herkesin Eclipse üzerinde kazabileceği ilk **ePOW (Ethereum Proof-of-Work)** emtia tokenidir.
- Toplam arz: **5 Milyon**.
- Ön madencilik YOK, Takım/insider payı YOK.
- Token adresi: [Eclipsescan Linki](https://eclipsescan.xyz/token/64mggk2nXg6vHC1qCdsZdEFzd5QGN4id54Vbho4PswCF)
-  [DEX]( https://www.geckoterminal.com/tr/eclipse/pools/CUzwQT8ZThYuKCerH3SpNu12eyxB4tAU3d19snjhELWU)

---

## 🛠️ Kurulum Rehberi

### Gereksinimler:
- **ETH bakiyesi olan Eclipse cüzdanı** Backpack
- Temel donanıma sahip bir bilgisayar ya da VPS
- Linux (Ubuntu) terminali

---

## 🔧 Bağımlılıkları Kur

### 1. Gerekli Paketleri Yükle
```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt install screen curl nano -y
```

### 2. Rust Kurulumu
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
İstendiğinde `1` tuşlayın ve kurulumun tamamlanmasını bekleyin:
```bash
source $HOME/.cargo/env
```

### 3. Solana CLI Kurulumu
```bash
curl --proto '=https' --tlsv1.2 -sSfL https://solana-install.solana.workers.dev | bash
```

Eğer `solana: command not found` hatası alırsanız:
```bash
echo 'export PATH="/root/.local/share/solana/install/active_release/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### 4. RPC Adresini Eclipse’e Yönlendir
```bash
solana config set --url https://eclipse.helius-rpc.com/
```



## 🔐 Cüzdan CLI Kullanımı

### 1. CLI Cüzdan Oluştur
```bash
solana-keygen new
```
- Şifre belirleyebilir ya da Enter’a basarak atlayabilirsin.
- **Seed Phrase** ve **Public Key**’ini kaydet.
- Public Key, node adresin olacak.

### 2. Private Key’i Dışa Aktar

Keypair yolunu bul:
```bash
solana config get
```
> Çıktı: `~/.config/solana/id.json`

Private key’i al:
```bash
cat ~/.config/solana/id.json
```
Bu çıktıyı güvenli bir yere kaydet!

### 3. Node Cüzdanını İçe Aktar ve ETH Gönder
- `id.json` dosyasını Backpack gibi cüzdanlara içe aktar.
- Cüzdana Eclipse ağı üzerinden ETH gönder.

---

## ⛏️ Bitz Kurulumu
```bash
cargo install bitz
```

---

## ⚙️ Bitz Miner'ı Çalıştırma

### 1. Arka Planda Çalıştırmak İçin Ekran Aç
```bash
screen -S bitz
```

### 2. Miner’ı Başlat
```bash
bitz collect
```

> Varsayılan olarak 1 CPU çekirdeği kullanır. Çekirdek sayısını arttırmak için:
```bash
bitz collect --cores 8
```

#### Ekran komutları:
- **Ekranı küçült:** `Ctrl+A+D`  
- **Ekrana geri dön:** `screen -r bitz`  
- **Node’u durdur:** `Ctrl+C`  
- **Ekranları listele:** `screen -ls`  
- **Ekranı sonlandır:**  

```bash
screen -XS bitz quit
 ```

---

## 🧰 Faydalı Komutlar

### Hesap Bilgisi Görüntüleme
```bash
bitz account
```

### Kazılmış Bitz Token Talep Et
```bash
bitz claim
```

### Tüm Komutları Görüntüle
```bash
bitz -h
```

---

> 📌 Bu rehber, Bitz madenciliğini kolay ve hızlı bir şekilde başlatman için hazırlanmıştır.

Seevice ile çalıştırmak için

  nano /etc/systemd/system/bitz.service  

    [Unit]
    Description=Bitz Collection Service
    After=network.target
    
    [Service]
    Type=simple
    User=root
    WorkingDirectory=/root
    ExecStart=/root/.cargo/bin/bitz collect --cores 12
    Restart=on-failure
    RestartSec=10s
    
    [Install]
    WantedBy=multi-user.target
