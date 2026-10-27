# Leitor GreenSeeker v1.0

Web app (PWA) que lê o NDVI do GreenSeeker de mão (modelo com Bluetooth, "GreenSeekerH BLE")
no Chrome do Android. Cada puxada de gatilho vira uma leitura, com GPS, e o resultado sai em CSV.

## Uso no campo
1. Segure o gatilho do sensor e toque em **Conectar sensor**.
2. Ajuste a **Parcela atual** (botões − e +).
3. Segure o gatilho no início da parcela e solte no fim, sem parar de andar.
   Ao soltar, o app salva a média do sensor, **fala "parcela, NDVI"** e avança para a próxima parcela.
4. Se errar uma parcela, toque em **Desfazer última**: a leitura é apagada e a parcela volta.
5. Ao final, toque em **Baixar CSV** ou **Compartilhar**.

Em Configurações:
- **Ordem das parcelas**: cole a lista na ordem do caminhamento (uma por linha), útil para zigue-zague.
- **Ignorar puxadas mais curtas que**: o padrão é 1 s, para descartar toques acidentais no gatilho.

## Colunas do CSV
| coluna | conteúdo |
|---|---|
| ndvi | média calculada pelo sensor (o valor do visor) |
| origem_ndvi | `sensor`, ou `app` se o pacote de média se perdeu e o app calculou a média das amostras |
| ndvi_media_amostras, ndvi_dp, ndvi_min, ndvi_max, n_amostras | estatísticas das amostras ao vivo (~2 por segundo) |
| duracao_s | tempo com o gatilho puxado |
| lat, lon | centro do caminhamento (média entre o início e o fim) |
| lat_ini, lon_ini, lat_fim, lon_fim | coordenadas ao puxar e ao soltar o gatilho |
| precisao_m, gps_idade_s | precisão e idade da última posição GPS |

## Protocolo Bluetooth (descoberto por engenharia reversa, out/2026)
- Serviço `f000aa90-0451-4000-b000-000000000000` (base Texas Instruments)
- `f000aa92` "Ndvi. Conf.": escrever `01` liga o envio de dados
- `f000aa91` "Ndvi. Data" (notify): 9 bytes, `[tipo][NDVI×100][00 00 01 00 00 00 E0]`
  - `d` 0x64 = gatilho puxado / pulso · `s` 0x73 = amostra ao vivo
  - `u` 0x75 = gatilho solto · `a` 0x61 = média final (o valor do visor)
  - Parado, o sensor repete o último pacote a cada ~1,8 s.
- Firmware testado: GreenSeeker Rev 2, 0.06 (Sep 28 2020)

## Problemas de conexão
O Bluetooth do sensor é instável. Siga esta ordem:
1. Segure o gatilho ao conectar.
2. Feche outros apps que possam estar conectados ao sensor (por exemplo, o nRF Connect).
3. Desligue e ligue o Bluetooth do celular.
4. Tire e recoloque a bateria do sensor (recomendação da Trimble).

## Atualizar
No GitHub, use **Add file → Upload files** com o arquivo novo e faça o commit.
No celular, feche e abra o app.
