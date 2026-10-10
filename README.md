# Gerador de Música por Gramática

Gerador de composições simbólicas em MIDI usando uma gramática probabilística. O projeto explora duas famílias de arranjo, `balada` e `tropical`, inspiradas em características de *Don't* e *Shape of You*. As músicas são novas combinações de padrões rítmicos, harmonia e instrumentação; o programa não gera áudio nem grava vocais.

## Requisitos

- Python 3.10 ou superior
- Dependências listadas em `requirements.txt`
- Um sintetizador ou programa compatível com MIDI para ouvir os arquivos gerados

## Instalação

Na raiz do repositório, crie e ative um ambiente virtual e instale as dependências:

```bash
python3 -m venv venv
source venv/bin/activate
python -m pip install -r requirements.txt
```

No Windows PowerShell, ative o ambiente com:

```powershell
venv\Scripts\Activate.ps1
```

## Uso

Gere uma música com as escolhas padrão:

```bash
python SongGenerator.py
```

Sem opções, o programa sorteia a família, tonalidade, andamento, progressão, estrutura e ordem de entrada das camadas. O MIDI é salvo em `Samples/`, que é criado automaticamente.

Para fixar os principais parâmetros:

```bash
python SongGenerator.py --familia balada --tom Fm --bpm 96 --passes 2 --seed 5101
```

Com a mesma semente e os mesmos argumentos, o programa reproduz o mesmo arquivo MIDI. Quando `--seed` é usado, a semente faz parte do nome do arquivo de saída.

### Opções

| Opção | Descrição | Padrão |
|---|---|---|
| `--bpm N` | Fixa o andamento em BPM. | Sorteado dentro dos limites configurados |
| `--bpm-min N` | Limite inferior do sorteio do BPM. | `90` |
| `--bpm-max N` | Limite superior do sorteio do BPM. | `100` |
| `--tom TOM` | Fixa a tonalidade: `Fm`, `C#m`, `Gm`, `Em`, `Dm` ou `Bm`. | Sorteada com pesos definidos na gramática |
| `--familia F` | Fixa a família: `balada` ou `tropical`. | Sorteada com probabilidade igual |
| `--passes N` | Número de repetições do loop para cada camada que entra no build; valores são limitados a 1--4. | `2` |
| `--seed N` | Semente inteira para tornar a geração reproduzível. | Aleatória |

Para ver a ajuda completa:

```bash
python SongGenerator.py --help
```

## Reprodução do experimento

O relatório compara cinco músicas por família, mantendo tonalidade, andamento e passes constantes e variando a semente. A partir da raiz do repositório, execute:

```bash
for family in balada tropical; do
  for seed in 5101 5102 5103 5104 5105; do
    python SongGenerator.py \
      --familia "$family" \
      --tom Fm \
      --bpm 96 \
      --passes 2 \
      --seed "$seed"
  done
done
```

Os MIDI ficam em `Samples/`. Seus nomes registram família, tonalidade, BPM e semente. As métricas do relatório são extraídas desses arquivos e descrevem a estrutura MIDI; elas não medem qualidade musical nem similaridade estilística percebida.

## Estrutura do projeto

- `SongGenerator.py`: implementação do gerador e interface de linha de comando.
- `requirements.txt`: dependências Python.
- `Samples/`: MIDI gerados pelo programa.

