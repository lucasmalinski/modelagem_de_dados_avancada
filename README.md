# Modelagem de Dados Avançada

## Modelo Conceitual
![mod_conceitual.png](/mod_conceitual.png)

## Modelo Lógico 
![modelo_logico.png](/mod_logico.png)

## Transformações no Power BI (Transform Data)

Com base nas consultas M do modelo semântico, foi realizado o seguinte processo de transformação:

### 1. Fonte de dados
- Leitura do arquivo CSV em bronze/sample_evg_enriquecido.csv.
- Promoção da primeira linha para cabeçalhos.

### 2. Padronização de tipos e datas
- Conversão de tipos das colunas (texto, inteiro, datetime).
- Extração da parte da data em `dt_matricula` (remoção da hora) e conversão para date.
- Conversão de `dt_fim` para date na tabela fato.

### 3. Construção das dimensões
- **DimInstituicao**: Seleção de instituicao, remoção de duplicados/nulos e criação de chave `cod_instituicao`.
- **DimConteudista:** seleção de conteudista e email_conteudista, remoção de duplicados, ordenação e criação de chave `cod_conteudista`.
- **DimPoder e DimEsfera:** deduplicação e geração de chaves (`cod_poder`, `cod_esfera`).
- **DimEndereco:** combinação de `municipio_pessoa` e `uf_pessoa` com deduplicação e chave `cod_endereco`.
- **DimTematica:** deduplicação de tematica e criação de `cod_tematica`.
- **DimCurso e DimTurma:** manutenção de `cod_curso`/`cod_turma` como chave natural com deduplicação.
- **DimPessoa:** deduplicação por `codigo_pessoa` e renomeação para `cod_pessoa`.

**4. Construção da fato**
- Criação de **FMatriculas** a partir da base tratada.
- Merge (left join) com dimensões para buscar chaves respectivas de:
    
    >  instituição, esfera, endereço, temática, poder e conteudista.
- Seleção final dos campos de negócio e chaves, incluindo `cod_pessoa`, `cod_curso` e `cod_turma`.

**5. Calendário**
- **DimCalendario_mat** gerada dinamicamente entre a menor e maior `dt_matricula`.
- Inclusão de atributos de tempo: Ano, Nome do Mês e Trimestre.
- **DimCalendario_fim** reutiliza a estrutura da **DimCalendario_mat**.

**6. Relacionamentos principais**
- **FMatriculas** relacionada às dimensões por chaves substitutas.
- Relacionamentos de data entre **FMatriculas** e calendários (`FMatriculas[dt_matricula]` -> `DimCalendario_mat[Data]` e `FMatriculas[dt_fim]` -> `DimCalendario_fim[Data]`).

## Preview do Painel
### [Painel Publicado](https://app.powerbi.com/links/R7_uTIU7RH?ctid=dfb66dc4-3f3c-492c-991d-727dbd1c89d4&pbi_source=linkShare)

> Observação ⚠️: Disponível apenas para e-mails dentre a organização UniCEUB. 

![painel_preview.png](/painel_preview.png)


