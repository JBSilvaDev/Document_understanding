Bem-vindo ao sumário do seu webbook sobre **Document Understanding (DU)** na UiPath, um tutorial focado em novos desenvolvedores de RPA. Este guia foi criado para apresentar os conceitos essenciais e as melhores práticas para começar a automatizar o processamento de documentos com o UiPath.

---

### **Sumário do Webbook: Desvendando o Document Understanding para Desenvolvedores RPA**

#### **[1. Introdução ao Document Understanding](./paginas/1.md)**
O **UiPath Document Understanding** é uma estrutura completa que facilita o processamento de arquivos de entrada, desde a digitalização de documentos até a validação de dados extraídos. O principal objetivo é simplificar a extração de dados, permitindo a criação de um único fluxo de trabalho para extrair informações de diversos tipos de documentos em um ambiente aberto e extensível.

#### **[2. Componentes Fundamentais da Estrutura DU](./paginas/2.md)**
A estrutura do Document Understanding é composta por vários componentes interligados que trabalham juntos para processar documentos:

*   **Taxonomia:** Define os tipos de documentos a serem processados e quais dados (campos) são necessários deles. É gerenciada através do **Gerenciador de Taxonomia**.
*   **Digitalização:** Converte arquivos em conteúdo legível por máquina, extraindo texto e o **Modelo de Objeto do Documento (DOM)**. A atividade principal é **Digitize Document**. **Mecanismos de OCR** são utilizados nesta etapa para arquivos de imagem ou PDFs sem conteúdo legível por máquina. Alguns exemplos de mecanismos de OCR incluem **UiPath Document OCR**, **UiPath Extended Languages OCR**, **OmniPage OCR**, Google Cloud Vision OCR, Microsoft Azure Computer Vision OCR, Microsoft OCR e Tesseract OCR.
*   **Classificação de Documentos:** Determina automaticamente os tipos de documentos encontrados em um arquivo digitalizado, mesmo que um único arquivo contenha vários tipos lógicos de documentos. A atividade central é **Classify Document Scope**, que permite configurar e executar diversos classificadores. Classificadores disponíveis incluem **Classificador baseado em palavra-chave**, **Intelligent Keyword Classifier**, **Machine Learning Classifier** e **Classificador Generativo**.
*   **Validação da Classificação de Documentos:** Permite a revisão manual e a correção dos resultados de classificação automática. A **Classification Station** é usada para essa finalidade.
*   **Treinamento em Classificação de Documentos:** Permite que o robô aprenda com as informações validadas por humanos para melhorar futuras previsões dos classificadores.
*   **Extração de Dados:** Captura as informações necessárias para o tipo de documento identificado, dentro do documento de entrada e intervalo de páginas. A atividade principal é **Data Extraction Scope**, que pode combinar e priorizar múltiplos extratores. Extratores incluem **Regex Based Extractor**, **Form Extractor**, **Machine Learning Extractor** e **Extrator Generativo**.
*   **Validação de Extração de Dados:** Etapa de revisão manual e correção dos dados extraídos automaticamente para garantir 100% de precisão. A **Validation Station** é usada para isso, seja como uma atividade assistida (**Present Validation Station**) ou como tarefas do Action Center (**Create Document Validation Action** e **Wait for Document Validation Action and Resume**).
*   **Treinamento em Extração de Dados:** Passa os dados extraídos validados por humanos de volta para os extratores, a fim de melhorar suas previsões futuras.
*   **Consumo de Dados:** Exporta os dados validados para uso posterior no fluxo de trabalho. O objeto **Document Data** é uma variável crucial que contém todas as informações sobre um documento, incluindo o texto, DOM e campos extraídos.

#### **[3. Pacotes de Atividades Essenciais](./paginas/3.md)**
O framework DU é principalmente encontrado nos pacotes **UiPath.DocumentUnderstanding.Activities** e **UiPath.IntelligentOCR.Activities**. É importante notar que esses dois pacotes não devem ser usados juntos no mesmo projeto; `UiPath.IntelligentOCR.Activities` é para fluxos de trabalho `Windows-Legacy`, enquanto `UiPath.DocumentUnderstanding.Activities` é para fluxos de trabalho `Windows` e `Multiplataforma`. O pacote **UiPath.DocumentUnderstanding.ML.Activities** também é fundamental para o uso de modelos de Machine Learning.

#### **[4. Melhores Práticas e Dicas para Novos Usuários](./paginas/4.md)**

*   **Comece com o UiPath Studio Web:** Projete e crie suas automações sem necessidade de conhecimento prévio para começar a automatizar rapidamente.
*   **Use Modelos de Projeto:** O **Processo do Document Understanding™: Modelo do Studio** é um modelo de projeto totalmente funcional que oferece uma base fácil de usar, pré-configurada com tipos de documentos, classificadores e extratores. Ele inclui log, tratamento de exceções e mecanismos de repetição. Você pode acessá-lo na aba **Templates** do UiPath Studio.
*   **Entenda o Objeto Document Data:** Este objeto é central para o fluxo de trabalho, atuando como variável de entrada e saída. Ele coleta todas as informações de um documento e pode ser reutilizado entre as atividades para evitar digitalizações redundantes.
*   **Otimize seus Prompts (Classificador e Extrator Generativo):**
    *   **Contexto Amplo:** Forneça descrições detalhadas para cada tipo de documento ou campo, em vez de descrições vagas, para evitar erros óbvios e melhorar a eficácia do modelo generativo.
    *   **Linguagem Precisa e Formato de Saída:** Use linguagem precisa e especifique o formato desejado para as respostas (ex: `yyyy-mm-dd` para datas, `##,###.##` para números) para reduzir a ambiguidade e simplificar o pós-processamento.
    *   **Opções Esperadas:** Ofereça um conjunto conhecido de respostas possíveis para perguntas de múltipla escolha.
    *   **Passo a Passo:** Divida perguntas complexas em etapas simples, ou até mesmo use um estilo de "programa de computador" para forçar o modelo a seguir instruções com maior precisão.
    *   **Evite Operações Aritméticas/Lógicas:** Não peça ao extrator generativo para realizar somas, multiplicações, comparações ou lógica complexa "se-então-senão", pois robôs são mais precisos e eficientes para essas tarefas.
    *   **Tabelas:** O Extrator Generativo não suporta campos de coluna diretamente. Para extrair tabelas, peça para retornar colunas ou linhas separadamente e monte-as no fluxo de trabalho. A tecnologia de IA generativa opera em texto linear e não compreende informações visuais bidimensionais em imagens.
    *   **Nível de Confiança:** Modelos de IA Generativa não fornecem níveis de confiança. Uma alternativa é repetir a mesma pergunta de várias maneiras diferentes; a consistência das respostas pode indicar baixa probabilidade de erro.
*   **Conexão Externa:** Para usar Robôs On-Premises com recursos de nuvem, você pode configurar uma conexão externa adicionando um aplicativo externo em sua organização Automation Cloud e fornecendo suas credenciais (ID do Aplicativo e Segredo do Aplicativo) na atividade ou como um Ativo de Credenciais no Orchestrator.
*   **Limitação de Variáveis DataTable:** Ao usar atividades "Wait for" que suspendem fluxos de trabalho com variáveis `DataTable`, certifique-se de que a `DataTable` seja serializável. Inicialize-a com um nome, por exemplo, `new System.Data.DataTable("MyTable")`, ou deixe o valor padrão vazio, para evitar falhas de execução.
*   **Implantação de Modelos ML:** O **AI Center** é a infraestrutura onde os modelos de Machine Learning do Document Understanding são executados. Você pode implantar pacotes de ML pré-configurados como habilidades de ML (e.g., **Faturas**, **UiPathDocumentOCR**).

#### **[5. Recursos Adicionais](./paginas/5.md)**
Para aprofundar seus conhecimentos, explore os cursos dedicados ao Document Understanding na **[UiPath RPA Academy](https://academy.uipath.com/)** e obtenha suporte da comunidade no **Fórum da UiPath**.