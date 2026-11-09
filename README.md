# Modelo de TCC do IFB com `ifbtcc`

Este repositório é um projeto de exemplo para escrever trabalhos de conclusão de curso do Instituto Federal de Brasília usando LaTeX. A classe `ifbtcc` carrega a classe `abntex2`, repassa a ela as opções recebidas e acrescenta configurações do modelo: dados de capa e folha de rosto, citações autor-data, ficha catalográfica, folha de aprovação e ambiente de quadros. O modelo é um ponto de partida; confirme com seu curso e orientador os requisitos institucionais vigentes.

## 1. Comece pelo arquivo principal

O arquivo principal é [`tcc.tex`](tcc.tex). Abra-o no seu editor LaTeX ou envie o projeto a um serviço compatível, como o Overleaf. Mantenha a estrutura de diretórios para que os caminhos dos `\include` e das imagens continuem válidos. Edite os arquivos de conteúdo e remova do `tcc.tex` as inclusões dos elementos que não serão usados.

O preâmbulo atual seleciona fonte de 12 pontos, papel A4, impressão frente e verso (`twoside`), capítulos iniciando em página ímpar (`openright`) e português como idioma principal. As opções são encaminhadas ao `abntex2`; opções adicionais podem ser passadas em `\documentclass`. Para impressão somente frente, por exemplo, troque `twoside` por `oneside`. O pacote `abntex2cite` está configurado com estilo alfabético (`alf`).

Compile `tcc.tex` com uma distribuição LaTeX que tenha os pacotes do abnTeX2 instalados. Para atualizar citações e referências BibTeX, a sequência usual é `pdflatex tcc`, `bibtex tcc`, `pdflatex tcc` e `pdflatex tcc` novamente (ou use `latexmk` configurado para BibTeX). Podem ser necessárias recompilações adicionais para atualizar sumário, listas, referências cruzadas e índice remissivo. Overleaf normalmente executa essa sequência automaticamente.

## 2. Organização dos arquivos

- `editaveis/informacoes.tex`: dados do autor, curso, título, instituição, orientação e banca.
- `editaveis/comandos.tex`: comandos próprios do modelo; acrescente aqui macros reutilizáveis.
- `capa/capa.tex`: personalização da capa e folha de rosto.
- `elementos-pre-textuais/`: resumo, aprovação, dedicatória, listas e outros elementos iniciais.
- `elementos-textuais/`: capítulos do trabalho.
- `elementos-pos-textuais/`: apêndices, anexos e índice remissivo.
- `referencias/referencias.bib`: base bibliográfica em formato BibTeX.
- `ifbtcc.cls`: implementação da classe; em geral, não é necessário alterá-la para preencher o trabalho.

O comando `\include{pasta/arquivo.tex}` insere cada parte do trabalho. O nome passado a `\include` não leva a extensão `.tex`. Use `\input{...}` para inserir trechos menores sem quebra de página automática.

## 3. Preencha os dados e a banca

Atualize `editaveis/informacoes.tex`. Os comandos disponíveis incluem:

```latex
\autor{Nome completo}
\curso{Nome do curso}
\grau{Grau pretendido}
\titulo{Título do trabalho}
\local{Cidade}
\data{Ano}
\instituicao{Instituto Federal de Educação, Ciência e Tecnologia de Brasília}
\tipotrabalho{Trabalho de Conclusão de Curso}
\orientador{Nome e titulação do orientador}
\coorientador{Nome e titulação do coorientador} % opcional
\preambulo{Texto que informa objetivo, instituição e requisito para obtenção do grau}
\cdu{Número CDU fornecido pela biblioteca}
\dataDaAprovacao{Data da aprovação}
\membroConvidadoUm{Nome do primeiro membro}
\membroConvidadoDois{Nome do segundo membro}
```

Os comandos da banca `\dataDaAprovacao`, `\membroConvidadoUm` e `\membroConvidadoDois` são definidos por este modelo em `editaveis/comandos.tex`. A folha de aprovação (`elementos-pre-textuais/folha-de-aprovacao.tex`) já imprime orientador e dois membros convidados; ajuste os nomes, instituições e, se necessário, inclua ou remova linhas `\assinatura`. Após a defesa, o exemplo permite substituir a página pela versão assinada em PDF usando `\includepdf` dentro do ambiente `folhadeaprovacao` (o pacote correspondente pode precisar ser carregado no preâmbulo).

## 4. Elementos pré-textuais

No `tcc.tex`, a capa e a folha de rosto são chamadas por `\imprimircapa` e `\imprimirfolhaderosto*`. O asterisco da folha de rosto solicita a ficha catalográfica. O arquivo `ficha-catalografica.tex` contém apenas um exemplo: solicite a ficha definitiva à biblioteca e substitua o conteúdo pelo PDF fornecido. A numeração CDU deve ser informada em `\cdu{...}`.

Edite os modelos em `elementos-pre-textuais/` conforme necessário:

- `resumo.tex`: use o ambiente `resumo` e termine com palavras-chave;
- `abstract.tex`: resumo em outro idioma, no exemplo em inglês;
- `folha-de-aprovacao.tex`: nomes da banca e dados da aprovação;
- `dedicatoria.tex`, `agradecimentos.tex`, `epigrafe.tex` e `errata.tex`: elementos opcionais, conforme exigência do curso;
- `lista-de-ilustracoes.tex`, `lista-de-tabelas.tex`, `lista-de-quadros.tex`, `lista-de-abreviaturas.tex`, `lista-de-simbolos.tex` e `sumario.tex`: listas automáticas ou manuais.

As listas de figuras e tabelas são produzidas a partir das respectivas legendas. A lista de quadros é gerada pelo ambiente `quadro` configurado na classe. A lista de siglas e a de símbolos são preenchidas manualmente nos ambientes `siglas` e `simbolos`.

## 5. Escreva os capítulos

Cada capítulo pode ficar em um arquivo separado, por exemplo `elementos-textuais/introducao.tex`:

```latex
\chapter{Introdução}\label{cap:introducao}
Texto do capítulo.

\section{Objetivos}\label{sec:objetivos}
Texto da seção.
```

O comando `\textual` no arquivo principal inicia a parte textual. Use `\chapter`, `\section` e `\subsection` para estruturar o conteúdo. Para se referir a uma seção, figura ou tabela, coloque `\label{...}` após o título ou legenda e use `\autoref{...}` ou `\ref{...}` no texto. Compile novamente até os números serem atualizados.

## 6. Citações, bibliografia e referências

Cadastre cada obra em `referencias/referencias.bib`, atribuindo uma chave única. Exemplo:

```bibtex
@book{silva2022,
  author    = {Silva, Maria},
  title     = {Título do livro},
  address   = {Brasília},
  publisher = {Editora},
  year      = {2022}
}
```

Use a chave nas citações do texto. Com `abntex2cite` no estilo alfabético, exemplos usuais são:

```latex
\cite{silva2022}                 % citação entre parênteses
\citeonline{silva2022}           % autor integrado à frase
\cite[p.~25]{silva2022}         % citação com página
```

Por exemplo: `\citeonline{silva2022} discute o tema` ou `... conforme estudo anterior \cite{silva2022}`. Ajuste o argumento opcional conforme a localização citada, como `\cite[p.~25]{silva2022}`. A chave precisa existir no arquivo `.bib`; somente itens citados entram na lista, salvo uso de `\nocite{*}` para listar toda a base.

No fim do documento, `\bibliography{referencias/referencias}` seleciona `referencias/referencias.bib` (sem escrever a extensão) e imprime as referências. Não crie manualmente uma segunda lista de referências. Confira e complete os campos de cada registro BibTeX, especialmente autoria, título, edição, local, editora, ano, URL e data de acesso quando aplicáveis.

## 7. Figuras, tabelas e quadros

Guarde as imagens no projeto e inclua-as com `graphicx`, já carregado pela classe. Use legenda e fonte, e cite cada elemento no texto.

**Figura:**

```latex
\begin{figure}[htb]
  \centering
  \includegraphics[width=0.75\textwidth]{figuras/arquitetura.png}
  \caption{Arquitetura proposta}\label{fig:arquitetura}
  \fonte{Elaboração própria (2026).}
\end{figure}
```

**Tabela:**

```latex
\begin{table}[htb]
  \centering
  \caption{Resultados por grupo}\label{tab:resultados}
  \begin{tabular}{lr}
    \hline
    Grupo & Quantidade \\ \hline
    A & 12 \\
    B & 18 \\ \hline
  \end{tabular}
  \fonte{Dados da pesquisa (2026).}
\end{table}
```

**Quadro:** a classe fornece o ambiente `quadro` e uma lista de quadros. A estrutura interna pode usar `tabular`, como no exemplo:

```latex
\begin{quadro}[htb]
  \caption{Comparação das abordagens}\label{qua:abordagens}
  \begin{tabular}{|l|l|}
    \hline Abordagem & Característica \\ \hline
    A & Exemplo \\ \hline
  \end{tabular}
  \fonte{Elaboração própria (2026).}
\end{quadro}
```

Referencie-os, por exemplo, com `\autoref{fig:arquitetura}`, `\autoref{tab:resultados}` e `\autoref{qua:abordagens}`. Os especificadores `[htb]` são sugestões de posicionamento para o LaTeX, não posições garantidas. Os arquivos de lista já imprimem figuras, tabelas e quadros; mantenha-os no pré-texto se essas listas forem necessárias.

## 8. Listagens de código

O modelo não carrega um pacote de listagem de código por padrão. Uma opção comum é carregar `listings` no preâmbulo de `tcc.tex` e configurar a linguagem. Exemplo:

```latex
\usepackage{listings}
\usepackage{xcolor}
\lstset{
  basicstyle=\ttfamily\small,
  numbers=left,
  numberstyle=\tiny,
  breaklines=true,
  frame=single
}
```

No corpo do texto:

```latex
\begin{lstlisting}[language=Python,caption={Exemplo de função},label={lst:funcao}]
def soma(a, b):
    return a + b
\end{lstlisting}
```

Consulte a documentação de `listings` para linguagens e personalização. Outra opção é `minted`, que requer configuração adicional de compilação e execução externa do Pygments; verifique se seu ambiente permite isso antes de adotá-lo.

## 9. Glossário, siglas e símbolos

Este modelo não configura um glossário automático. Para um glossário simples, crie uma seção pós-textual não numerada e liste os termos manualmente:

```latex
\chapter*{Glossário}
\addcontentsline{toc}{chapter}{Glossário}
\begin{description}
  \item[Termo] Definição do termo.
\end{description}
```

Para glossários indexados e listas automáticas, use o pacote `glossaries` (carregue-o no preâmbulo e siga sua documentação; a compilação requer uma etapa adicional de ferramenta). Para siglas, preencha `elementos-pre-textuais/lista-de-abreviaturas.tex`, dentro de `siglas`; para símbolos, use `elementos-pre-textuais/lista-de-simbolos.tex`, dentro de `simbolos`.

## 10. Apêndices, anexos e índice remissivo

O `tcc.tex` já inicia referências com `\postextual`, inclui os apêndices no ambiente `apendicesenv` e os anexos em `anexosenv`. Edite `elementos-pos-textuais/apendice.tex` e `anexo.tex`; para omitir algum conjunto, remova ou comente seu bloco no arquivo principal. Apêndices são produzidos pelo autor; anexos são documentos de terceiros incorporados ao trabalho.

A classe chama `\makeindex`, e o modelo inclui `\printindex` em `indice-remissivo.tex`. Marque entradas no texto com `\index{termo}`. Para gerar o índice, execute também `makeindex tcc` entre as compilações LaTeX, ou use uma ferramenta como `latexmk` configurada para isso. Se não usar índice remissivo, retire a inclusão correspondente.

## 11. Mais recursos do abnTeX2

O abnTeX2 oferece ambientes e comandos para dedicatória, epígrafe, agradecimentos, errata, folha de aprovação, resumo, listas, citações longas, notas, apêndices e anexos, entre outros. Consulte o manual da classe e o exemplo canônico do abnTeX2 para detalhes e opções avançadas. A classe deste repositório acrescenta personalizações para o IFB; ela não substitui a conferência das normas e orientações atuais do seu curso.
