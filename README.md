<h2 align=center> OVER VALIDATOR </h2>

- Join Channel Telegram : [Airdrop Node](https://t.me/airdrop_node)
---

## Prasyarat

Sebelum memulai pengaturan, pastikan alat-alat berikut terinstal di sistem Anda:

- **Docker**: Untuk menjalankan container.
- **Docker Compose**: Untuk mengelola aplikasi Docker yang menggunakan banyak container.
- **curl**: Untuk melakukan permintaan HTTP.
- **jq**: Untuk memproses data JSON.
- **Vps** Ram Minimal 8Gb Rekomended 16Gb Up

## Clone

```bash
git clone https://github.com/overprotocol/docker-scripts.git
mv docker-scripts overprotocol
cd overprotocol
```

## Ekspor IP Publik untuk Penemuan Node
Setel IP publik Anda untuk digunakan dalam penemuan node:

```bash
export PUBLIC_IP=$(curl -s ifconfig.me)
echo $PUBLIC_IP
```

## Inisialisasi Direktori Data dan Token JWT
Inisialisasi direktori data dan buat token JWT menggunakan perintah make

```bash
make init
```

## Jalankan Full Node
Jalankan container menggunakan skrip docker-compose yang disediakan:

```bash
docker-compose -f mainnet.yml up -d
```
-- Cek Logs
```bash
docker logs kairos -f
```
```bash
docker logs chronos -f
```

## Verifikasi status node Anda untuk memastikan node tersinkronisasi dengan baik:

```bash
curl 127.0.0.1:3500/eth/v1/node/syncing | jq
```
Ini akan mengambil status client konsensus. Jika sync_distance sama dengan 0, berarti node Anda sudah tersinkronisasi dengan baik.

## Menjalankan Node Validator
Untuk menjalankan validator, Anda harus melakukan staking OVER minimal 256 Over. Ikuti langkah-langkah berikut untuk menghasilkan kunci validator dan data deposit:

```bash
docker run -it --rm -v $(pwd)/validator_keys:/app/validator_keys overfoundation/staking-deposit-cli:v2.7.2 new-mnemonic
```

Ini akan menghasilkan file berikut di dalam folder ./validator_keys:
deposit_data-*.json
keystore-m_*.json

## Mengimpor Keystore
Setelah menghasilkan kunci validator, Anda perlu mengimpornya ke akun validator Anda. Anda dapat melakukannya dengan menjalankan perintah berikut, yang akan mengimpor keystore ke dalam container validator:

```bash
docker run -it -v $(pwd)/validator_keys:/keys \
  -v $(pwd)/wallet:/wallet \
  --name validator \
  overfoundation/chronos-validator:latest \
  accounts import \
  --keys-dir=/keys \
  --wallet-dir=/wallet \
  --accept-terms-of-use
```
Perintah ini akan mengimpor file keystore dari direktori ./validator_keys ke dalam direktori /wallet di dalam container, memungkinkan validator untuk menggunakannya dalam staking.

## Mengirim Transaksi Deposit
Setelah Anda menghasilkan kunci validator dan data deposit, Anda dapat mengirim transaksi deposit menggunakan Docker container. Ikuti langkah-langkah berikut:

```bash
docker build -t over-staking send-deposit/.
```

## Jalankan Container Staking
Jalankan container dengan kunci pribadi Anda, data deposit, dan penerima biaya yang disarankan dengan mengatur variabel lingkungan yang diperlukan:

```bash
docker run -v $(pwd)/validator_keys:/app/validator_keys \
  -e PUBLIC_RPC_URL=PUBLIC_RPC_URL \
  -e PRIVATE_KEY=YOUR_PRIVATE_KEY_WITH_0x_PREFIX \
  -e DEPOSIT_DATA_FILE_NAME=YOUR_DEPOSIT_DATA_FILE_NAME \
  over-staking --suggested-fee-recipient=YOUR_OVER_ADDRESS
```
Anda perlu menyediakan variabel lingkungan berikut:

PUBLIC_RPC_URL: URL untuk RPC publik (merujuk ke konfigurasi jaringan Anda).
YOUR_PRIVATE_KEY_WITH_0x_PREFIX: Kunci pribadi dengan dana yang cukup, termasuk awalan 0x.
YOUR_DEPOSIT_DATA_FILE_NAME: Nama file data deposit (misalnya, deposit_data-*.json).
YOUR_OVER_ADDRESS: Alamat tempat biaya transaksi akan dikirim.

## Jalankan Node

```bash
docker compose -f mainnet-validator.yml up -d
```

Cek Logs

```bash
docker logs validator -f
```
--DONE

## Support Airdrop Node Jika Script Ini Membantu

EVM
```bash
0xFd890C50abfF5d9F22b6E12f4717f7D4Ca13D615
```

https://trakteer.id/AirdropNode/tip

- Join Channel Telegram : [Airdrop Node](https://t.me/airdrop_node)
---
THANKS
