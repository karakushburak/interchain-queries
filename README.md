TASK-9 REHBER (RELAYER TASKS)

Form için :

https://docs.google.com/forms/d/e/1FAIpQLSeoZEC5kd89KCQSJjn5Zpf-NQPX-Gc8ERjTIChK1BEbiVfMVQ/viewform

1. Adım: Binary Dosyası İndirme: (kodları tek tek girin)

cd $HOME

git clone https://github.com/Stride-Labs/interchain-queries.git

cd interchain-queries

go build

sudo mv interchain-queries /usr/local/bin/icq

2. Adım: Yapılandırma Dosyası Ayarlama: (bir text dosyası yapılandırıp toplu girin)

cd $HOME && mkdir .icq

sudo tee $HOME/.icq/config.yaml > /dev/null <<EOF
																									
default_chain: stride-testnet
chains:
																									
  gaia-testnet:
																									
    key: CÜZDAN ADI
																									
    chain-id: GAIA
																									
    rpc-addr: http://BURAYA      # Gaia RPC yazacağız
																									
    grpc-addr: http://BURAYA     # Gaia GRPC yazacağız
																									
    account-prefix: cosmos
																									
    keyring-backend: test
																									
    gas-adjustment: 1.2
																									
    gas-prices: 0.001uatom
																									
    key-directory: /root/.icq/keys
																									
    debug: false
																									
    timeout: 20s
																									
    block-timeout: ""
																									
    output-format: json
																									
    sign-mode: direct
																									
  stride-testnet:
																									
    key: CÜZDAN ADI
																									
    chain-id: STRIDE-TESTNET-4
																									
    rpc-addr: http://BURAYA      # Stride RPC yazacağız
																									
    grpc-addr: http://BURAYA     # Stride GRPC yazacağız
																									
    account-prefix: stride
																									
    keyring-backend: test
																									
    gas-adjustment: 1.2
																									
    gas-prices: 0.001ustrd
																									
    key-directory: /root/.icq/keys
																									
    debug: false
																									
    timeout: 20s
																									
    block-timeout: ""
																									
    output-format: json
																									
    sign-mode: direct
																									
cl: {}
																									
EOF

3. Adım: Cüzdan Import
																									
icq keys restore --chain STRIDE CÜZDANINIZ
icq keys restore --chain GAIA CÜZDANINIZ
																									
*MNEMONICLERİ İSTEYECEKTİR
																									
4. Adım: ICQ Servis Dosyası Oluşturma
																									
sudo tee /etc/systemd/system/icqd.service > /dev/null <<EOF
	
[Unit]
	
Description=Interchain Query Service
	
After=network-online.target

[Service]
	
User=$USER
	
ExecStart=$(which icq) run --debug
	
Restart=on-failure
RestartSec=3
	
LimitNOFILE=65535

[Install]
	
WantedBy=multi-user.target
	
EOF
	
5. Adım: Servis Başlatma
	
sudo systemctl daemon-reload
	
sudo systemctl enable icqd
	
sudo systemctl restart icqd

6. Adım: Log Kontrol

journalctl -u icqd -f -o cat
	
*20 DK. CİVARINDA SÜREBİLİR
	
ÖRNEK ÇIKTI: 

store/bank/key
	
height parsed from GetHeightFromMetadata= 0
	
Fetching client update for height height 176886
	
store/bank/key
	
height parsed from GetHeightFromMetadata= 0
	
Fetching client update for height height 176886
	
Requerying lightblock
	
Requerying lightblock
	
Requerying lightblock
	
ICQ RELAYER | query.Height= 0
	
ICQ RELAYER | res.Height= 176885
	
Requerying lightblock
	
ICQ RELAYER | query.Height= 0
	
ICQ RELAYER | res.Height= 176885
	
Send batch of 4 messages
	
1 ClientUpdate message
	
1 SubmitResponse message
	
1 ClientUpdate message
	
1 SubmitResponse message
	
Sent batch of 2 (deduplicated) messages
	
7. ADIM: TX'leri Explorerdan Kontrol Edin

Explorer: 
	https://poolparty.stride.zone/
	
	https://stride.explorers.guru/
	
İŞLEMLER BU KADAR BOL ŞANS
	
BONUS:

rly transact transfer gaia stride 1000uatom STRIDE ADRESİ channel-0 --path stride-gaia
	
rly transact transfer stride gaia 1000ustrd GAIA ADRESİ channel-0 --path stride-gaia
	
(Eşleşme sonrası yapılmalıdır-Explorerdan kontrol edip tx alabilirsiniz)
	

