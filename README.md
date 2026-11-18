# Detector de Folhas: saudável x doente

Classificador de imagens de folhas (PyTorch + MobileNetV3) com API em FastAPI e front-end web.

## Rodar localmente

```bash
python run_pipeline.py
```

Cria a venv, instala as dependências, treina (se os dados ou o código mudaram), avalia e sobe a API.

- Front-end: http://localhost:8000/app
- Documentação da API: http://localhost:8000/docs
- Status: `GET /`
- Classificar: `POST /analisar` (form-data, campo `imagem`)

## Estrutura

| Arquivo | Função |
|---|---|
| `config.py` | Caminhos e hiperparâmetros |
| `dataset.py` | Carregamento das imagens e transformações |
| `modelo.py` | Arquiteturas (`mobilenet`, `resnet18`, `cnn`) |
| `train.py` | Treino e salvamento do melhor modelo |
| `evaluate.py` | Avaliação no conjunto de teste |
| `predict.py` | Classificação pela linha de comando |
| `api.py` | API web + rota do front-end |
| `frontend/index.html` | Página web |
| `run_pipeline.py` | Executa tudo de uma vez |

## Deploy no Render

O arquivo `render.yaml` já configura o serviço. A API usa o modelo já treinado em `modelos/melhor_modelo.pth` (não treina no servidor).

1. No Render: **New → Blueprint** e escolha este repositório.
2. Confirme a criação do serviço `plantas` (plano free).
3. Depois do deploy, acesse `https://<seu-servico>.onrender.com/app`.

O build instala só o necessário para a API (`requirements-render.txt`), com PyTorch para CPU.
No plano free o serviço "dorme" após 15 minutos sem acesso; o primeiro acesso depois disso demora cerca de 1 minuto.
