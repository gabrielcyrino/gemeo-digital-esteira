# Dashboard Web Local com FastAPI

## Resumo
Substituir o tablet/HUD por um dashboard web em `localhost`, com FastAPI recebendo eventos do Unity por `HTTP POST` e atualizando a pagina em tempo real via `SSE`. A primeira versao sera somente leitura, com historico apenas em memoria.

## Mudancas Principais
- Criar uma pasta `DashboardWeb` com servidor FastAPI, arquivos web estaticos, `requirements.txt` e `start_dashboard.bat`.
- Endpoints do servidor:
  - `POST /api/eventos`: recebe eventos do Unity.
  - `GET /eventos`: envia eventos em tempo real para o navegador via SSE.
  - `GET /api/estado`: retorna os eventos atuais em memoria quando a pagina abre.
  - `GET /`: abre o dashboard.
- Criar `ClienteDashboardWeb.cs` no Unity, com uma URL publica `http://localhost:8000/api/eventos` e um metodo simples para enviar eventos JSON usando `UnityWebRequest`.
- Alterar `GerenciadorDeSensores.cs` e `CabecoteDePintura.cs` para enviarem eventos ao dashboard usando uma referencia publica ao `ClienteDashboardWeb` no Inspector.
- Remover os arquivos antigos do tablet, incluindo `Tablet.cs` e as imagens de fundo do tablet, para evitar confusao no tutorial.

## Eventos
Usar JSON simples e didatico:

```json
{
  "tipo": "sensor",
  "origem": "Sensor_2",
  "caixa": "Caixa 1",
  "estagio": "Ponto A - pintura de base",
  "dataHora": "2026-06-25 14:30:12",
  "tempoSimulacao": 18.42,
  "mensagem": "Caixa 1 detectada no Ponto A"
}
```

Tipos iniciais:
- `sensor`: caixa detectada por um sensor.
- `cabecote`: pintura aplicada por um cabecote.

## Dashboard
A interface web tera:
- Blocos para os 4 sensores: entrada, ponto A, ponto B e ponto C.
- Blocos para os 3 cabecotes: base, acabamento e verniz.
- Indicadores de ultima caixa, horario do ultimo evento e totais por ponto.
- Historico com scroll mostrando os eventos recebidos em tempo real.

## Testes
- Rodar `start_dashboard.bat` e abrir `http://localhost:8000`.
- Enviar um evento manual para `POST /api/eventos` e confirmar que aparece no dashboard.
- Rodar a cena no Unity e validar eventos de `Sensor_1` a `Sensor_4`.
- Confirmar que `Cabecote_1`, `Cabecote_2` e `Cabecote_3` aparecem no historico ao aplicar seus materiais.
- Validar que, se o servidor estiver desligado, a simulacao Unity continua funcionando e apenas mostra aviso no Console.

## Assumptions
- FastAPI sera instalado via `requirements.txt`; o Python local existe, mas FastAPI ainda nao esta instalado.
- O servidor sera iniciado em terminal separado, nao pelo Unity.
- O dashboard nao enviara comandos de volta ao Unity nesta primeira versao.
- As referencias ao `ClienteDashboardWeb` serao feitas manualmente no Inspector, por ser mais didatico para iniciantes.
