# FSP
P2 Ativ1 e Ativ2
Reprodução da atualização Dados P1 FSP 2026
O pacote inclui a planilha original, scripts de coleta das fontes públicas e o script de transformação que gera o Excel atualizado e o relatório Word. Os valores brutos usados nesta entrega já estão preservados na aba Dados brutos atualizados do arquivo Excel.

Arquivos
•	DadosP1FSP2026.xlsx: arquivo original fornecido; o script não altera suas células.
•	collect_siconfi.py: baixa as respostas anuais DCA-I-C, DCA-I-D e RGF-Anexo 01 para Fortaleza e Juazeiro do Norte, 2016–2025.
•	collect_public_sources.py: baixa população, IPCA e PIB municipal do IBGE, séries do FMI, ARCA, RON e arquivos de Hacienda.
•	collect_ideb.py: baixa os arquivos municipais IDEB 2025 e verifica os MD5 incluídos nos ZIPs do INEP.
•	build_deliverables.py: normaliza unidades, calcula deflatores e indicadores per capita, preserva o arquivo original e gera DadosP1FSP2026_Atualizado.xlsx e Relatorio_Atualizacao_DadosP1FSP2026.docx.

Dependências
Python 3.10 ou superior; pandas, openpyxl, requests, beautifulsoup4, xlrd, urllib3 e python-docx. LibreOffice é opcional para recalcular fórmulas e exportar o DOCX a PDF.

Instalação sugerida:

python3 -m pip install pandas openpyxl requests beautifulsoup4 xlrd urllib3 python-docx

Ordem de execução
Execute os scripts no diretório do pacote, com acesso à Internet:

python3 collect_siconfi.py
python3 collect_public_sources.py
python3 collect_ideb.py
python3 build_deliverables.py

O script de montagem procura DadosP1FSP2026.xlsx no mesmo diretório. É possível indicar outro caminho com a variável ORIGINAL_XLSX.

Para recalcular fórmulas e exportar o relatório para PDF usando LibreOffice:

mkdir -p pdf_out
soffice --headless --convert-to xlsx --outdir ./recalc ./DadosP1FSP2026_Atualizado.xlsx
soffice --headless --convert-to pdf --outdir ./pdf_out ./Relatorio_Atualizacao_DadosP1FSP2026.docx

Fórmulas centrais
•	IPCA Brasil: Fator_t = média(IPCA mensal de 2025) / média(IPCA mensal de t); Valor real R$2025 = valor nominal × fator.
•	Argentina: índice relativo com base 2016=100 encadeado pela inflação média anual do CPI WEO; Valor real ARS2025 = nominal × (CPI2025/CPI_t).
•	Per capita: valor real / população do mesmo ano, somente quando ambos existem.
•	IDEB: P × N, usando o indicador de rendimento e a nota média padronizada publicados pelo INEP.
•	PIB total municipal em R$ mil: multiplicado por 1.000 antes da deflação.
•	Transferências RON: soma de monto por jurisdição/regime/imposto; a escala não é declarada pela fonte e, por isso, não é convertida ou deflacionada.

Cuidados
As séries incorporam vintages e revisões das fontes. Em especial, o Siconfi pode mudar classificações; a população municipal tem quebra com o Censo 2022; PIB municipal está disponível até 2023; IDEB é bienal; RON não explicita unidade; e a execução fiscal 2026 ainda não constitui exercício anual encerrado. As lacunas permanecem vazias, sem imputação por zero. A coleta futura pode retornar valores revistos em comparação com a cópia bruta deste trabalho.
