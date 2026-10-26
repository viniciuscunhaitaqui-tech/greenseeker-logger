# Leitor GreenSeeker (v0.1 · explorador)

Web app (PWA) que conecta ao GreenSeeker por Bluetooth Low Energy no Chrome do Android.
Ele registra cada puxada de gatilho com média, desvio, nº de amostras e GPS, e exporta CSV.
Funciona offline depois do primeiro acesso.

## 1. Publicar (uma vez, ~5 min)

O Web Bluetooth só funciona em página **https**. O GitHub Pages resolve isso de graça.

1. Crie um repositório público no GitHub, por exemplo `greenseeker-logger`.
2. Envie os arquivos desta pasta para a raiz do repositório (botão "Add file → Upload files"):
   `index.html`, `sw.js`, `manifest.webmanifest`, `icon-192.png`, `icon-512.png`.
3. Vá em **Settings → Pages**, escolha **Deploy from a branch**, depois `main` / `root`, e salve.
4. Em 1–2 min o app fica disponível em `https://SEU-USUARIO.github.io/greenseeker-logger/`.
5. Abra esse endereço no **Chrome do Android**. Pelo menu ⋮, toque em **Adicionar à tela inicial**.

## 2. Primeiro teste com o sensor

1. Ligue o Bluetooth e a localização do celular.
2. Toque em **Conectar sensor** e escolha o GreenSeeker na lista.
3. Abra **Explorador Bluetooth** e puxe o gatilho.
   - Se aparecerem pacotes, veja a tabela "Último pacote: interpretações".
     **Toque no valor verde que bate com o NDVI da tela do sensor.**
     Essa passa a ser a decodificação usada, e fica salva.
   - Se o explorador não listar nenhum serviço, abra o **nRF Connect**, conecte ao sensor
     e copie os UUIDs dos serviços. Cole em **Configurações → UUIDs extras** e conecte de novo.
4. Em último caso, baixe o **CSV bruto** e mande para análise.

## 3. Uso no campo

- **Parcela / ponto**: o botão **+1** avança a parcela (T1R09 → T1R10). Há também uma opção para avançar sozinho.
- Uma leitura termina depois de 1,5 s sem dados. Esse tempo é ajustável em Configurações.
- **Baixar CSV**: o separador é vírgula e o decimal é ponto, pronto para R, Python e QGIS.
  As colunas incluem `lat`, `lon`, `precisao_m` e `gps_idade_s`.
- Os dados ficam salvos no próprio celular (localStorage). Exporte ao fim de cada campanha.
- Leituras feitas no modo demo saem com `fonte = demo` no CSV.
