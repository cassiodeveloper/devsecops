---
title: SCA - Software Composition Analysis
tags: 
 - sca
 - checkmarx
 - cli
 - plugin
---

# SCA - Software Composition Analysis

## SCA SCAN

### Definição

Executamos um scan SCA quando queremos buscar por vulnerabilidades nas biblioetacas e códigos open-source que utilizamos.

### Checkmarx SCA

O software de hoje é construído usando componentes de código aberto e bibliotecas de terceiros, vinculados a um código personalizado. Os hackers visam componentes vulneráveis de código aberto para acessar dados confidenciais e valiosos, enquanto as regulamentações de proteção de dados se tornam mais rígidas em um esforço para encorajar melhores práticas de segurança de software. Enquanto tudo isso está acontecendo, o DevOps está dominando o mundo e o fardo de proteger o software está se expandindo rapidamente sob o controle dos desenvolvedores que o criam.

[Mais](https://www.checkmarx.com/products/software-composition-analysis/)

## Checkmarx CxCLI

CLI é um programa de linha de comando que aceita entrada de texto para executar funções do sistema operacional. Nas décadas de 1970 e 1980, a entrada de linha de comando era comumente usada por sistemas Unix e sistemas de PC como MS-DOS e Apple DOS. Hoje, com interfaces gráficas de usuário (GUI), a maioria dos usuários nunca usa interfaces de linha de comando (CLI). No entanto, a CLI ainda é usada por desenvolvedores de software e administradores de sistema para configurar computadores, instalar software e acessar recursos que não estão disponíveis na interface gráfica.

No Checkmarx existem dois modos de executar um scan via CxCLI, o modo síncrono e o assíncrono.

### Modo síncrono

Neste modo, o CxCLI ficará esperando o scan finalizar no servidor Checkmarx e não continuará sua execução até então. Este modo é normalmente utilizado para a "quebra do build" no pipeline.

```
---
Modo síncrono: - runCxConsole.cmd Scan -v -Projectname "CxServer\MeuProjeto" -cxServer https://10.32.3.128 -cxuser admin -cxpassword Cx123456! -locationtype folder -locationpath “D:\SourceCode\JavaVulnerableLableCode” -enableSca -scaUsername ********* -scaPassword *********** -scaAccount plugins -checkpolicy

Exemplo Windows: runCxConsole.cmd Scan -v -Projectname "CxServer\MeuProjeto" -cxServer https://10.32.3.128 -cxuser admin -cxpassword Cx123456! -locationtype folder -locationpath “D:\SourceCode\JavaVulnerableLableCode” -enableSca -scaUsername ********* -scaPassword *********** -scaAccount plugins -checkpolicy

Exemplo Linux: runCxConsole.sh Scan -v -Projectname "CxServer\MeuProjeto" -cxServer https://10.32.3.128 -cxuser admin -cxpassword Cx123456! -locationtype folder -locationpath “D:\SourceCode\JavaVulnerableLableCode” -enableSca -scaUsername ********* -scaPassword *********** -scaAccount plugins -checkpolicy
---
```
### Referências

<p class="editor-link"><a href="https://checkmarx.atlassian.net/wiki/spaces/SD/pages/1339424904/CLI+Plugin" target="blank" class="btn"><strong>&#9998;</strong>Documentação oficial</a></p>

## CicleCI

No exemplo abaixo, um YAML é utilziado para configurar o job do CircleCI para executar o CxCLI para realizar um scan síncrono.
```
---
version: 2.1

jobs:
  build-and-test:  
    docker:
      - image: cimg/openjdk:11.0
    steps:
      - checkout
      - run: |
          wget https://download.checkmarx.com/9.3.0GA/Plugins/CxConsolePlugin-1.1.5.zip
          mkdir CxConsolePlugin
          unzip CxConsolePlugin-1.1.5.zip -d CxConsolePlugin
          cd CxConsolePlugin
          chmod +x runCxConsole.sh
          pwd
          ./runCxConsole.sh Scan -v -Projectname "CxServer\MeuProjeto" -cxServer https://10.32.3.128 -cxuser admin -cxpassword Cx123456! -locationtype folder -locationpath “D:\SourceCode\JavaVulnerableLableCode” -enableSca -scaUsername ********* -scaPassword *********** -scaAccount plugins -checkpolicy
workflows:
  build_scan: 
    jobs:
      - build-and-test
---
```