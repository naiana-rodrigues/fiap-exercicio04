# Protótipo Multimodal para Monitoramento e Análise de Dados em Saúde

## 1. Sobre o projeto


A solução reúne três módulos independentes de análise:
- **Vídeo e movimento:** detecção de pose corporal, rastreamento de pontos-chave e análise de movimentos em exercícios de reabilitação.
- **Áudio:** reconhecimento automático de fala e preparação de transcrições para análise textual.
- **Sinais vitais:** processamento de dados fisiológicos e identificação de padrões potencialmente anômalos.

O projeto foi desenvolvido em Python, utilizando o Google Colab como ambiente de execução.

> **Importante:** trata-se de um protótipo acadêmico e experimental. Os resultados não constituem diagnóstico médico, prescrição ou recomendação terapêutica.

## 2. Objetivos
- Identificar pontos corporais e características geométricas em vídeos.
- Analisar padrões de movimento e repetições de exercícios.
- Converter áudio em texto utilizando reconhecimento automático de fala.
- Extrair informações textuais relevantes das transcrições.
- Processar e analisar sinais fisiológicos.
- Identificar valores atípicos e possíveis anomalias em sinais vitais.
- Gerar e organizar alertas para consulta demonstrativa.


## 3. Estrutura dos módulos


### Análise de vídeo e movimento

**Base de dados:** REHAB24-6 — Zenodo.

São vídeos de reabilitação motora, semelhante a uma fisoterapia.

**Etapas do processamento:**

1. Carregamento dos vídeos.
2. Processamento dos quadros com YOLOv8 e com YOLO-Pose.
3. Identificação dos pontos-chave corporais.
4. Rastreamento das detecções utilizando ByteTrack.
5. Cálculo de ângulos articulares.
6. Identificação de picos e vales do movimento.
7. Extração de características para análise das repetições.
8. Treinamento experimental de um classificador de regressão logística.

Foi realizado um treinamento inicial com dez repetições do exercício PM_001.


### Processamento de áudio

**Base de dados:** PriMock57.

O módulo utiliza arquivos de áudio para demonstrar o reconhecimento automático de fala e a conversão do conteúdo falado em texto.

**Etapas do processamento:**

1. Seleção e carregamento dos arquivos de áudio.
2. Preparação dos arquivos para processamento.
3. Reconhecimento contínuo de fala utilizando Azure Speech. 
4. Configuração do idioma de reconhecimento (en-US).
5. Geração da transcrição textual.
6. Preparação da transcrição para análise com Azure AI Language.
A partir do item 3 foi feita a configuração, mas não houve teste efetivo.

**Funcionalidades previstas ou em validação na etapa textual:**

- Análise de sentimento;
- Reconhecimento de entidades;
- Extração de frases-chave.


### Monitoramento de sinais vitais

**Base de dados:** BIDMC PPG and Respiration Dataset — PhysioNet.

O módulo utiliza registros fisiológicos para analisar as seguintes variáveis:

- Frequência cardíaca (HR);
- Frequência respiratória (RESP/RR);
- Saturação periférica de oxigênio (SpO₂).

**Etapas do processamento:**

1. Carregamento e organização dos registros.
2. Identificação e tratamento de zeros e valores ausentes.
3. Análise exploratória dos dados.
4. Identificação de valores atípicos pelo método do intervalo interquartil (IQR).
5. Aplicação do algoritmo Isolation Forest para detecção de anomalias.
6. Organização dos alertas e consulta por identificador do registro.


### Configuração dos serviços Azure

O módulo de áudio utiliza serviços da Microsoft Azure.

As credenciais devem ser configuradas por variáveis de ambiente ou pelo sistema de gerenciamento de segredos do Google Colab.

Exemplo de variáveis necessárias:

```text
AZURE_SPEECH_KEY=
AZURE_SPEECH_REGION=
AZURE_LANGUAGE_KEY=
AZURE_LANGUAGE_ENDPOINT=
```

Os nomes devem corresponder aos utilizados no código do projeto.

## 7. Execução

A execução deve ser realizada por módulo, respeitando a sequência de preparação e processamento de cada modalidade.
Não esquecer que fazer o processamento do pip install e dos imports, pois estão condensados nas mesmas células.
Cada módulo está configurado para download somente do arquivo específico a ser utilizado, pois as bases possuem tamanho significativo (Gb).

## 8. Resultados e estado do desenvolvimento

O histórico de desenvolvimento registra os seguintes resultados e funcionalidades:

| Módulo | Resultado registrado |
|---|---|
| Sinais vitais | Análise de HR, RESP/RR e SpO₂, aplicação de IQR e Isolation Forest e consolidação de 202 alertas |
| Áudio | Reconhecimento contínuo de fala com Azure Speech e geração de transcrições |
| Análise textual | Integração com Azure AI Language em configuração/validação |
| Vídeo | Estimativa de pose, rastreamento ByteTrack, cálculo de ângulos e identificação de picos e vales |
| Classificação de movimento | Treinamento inicial de regressão logística com dez repetições do exercício PM_001 |



## 9. Limitações

- As bases utilizadas são independentes e não representam necessariamente os mesmos indivíduos.
- A integração entre os módulos é demonstrativa, sem sincronização clínica entre sinais vitais, áudio e vídeo.
- O Isolation Forest identifica padrões estatísticos incomuns, mas não estabelece diagnósticos.
- A qualidade das transcrições de áudio ainda requer avaliação quantitativa.
- O classificador de movimento necessita de amostras adicionais e validação independente.
- Os dados de prescrição utilizados na demonstração são sintéticos.
- O protótipo não foi validado para utilização em ambiente clínico real.

## 10. Segurança e uso responsável

O projeto possui finalidade exclusivamente acadêmica e experimental.

Seus resultados não devem ser utilizados isoladamente para:

- Diagnosticar condições de saúde;
- Prescrever medicamentos ou tratamentos;
- Definir condutas clínicas;
- Substituir profissionais de saúde;
- Realizar triagem ou monitoramento clínico autônomo.

> **Importante:** sempre buscar os profissionais de saúde responsáveis para tratamento, prevenção e auxílio em caso de doenças e dúvidas médicas.