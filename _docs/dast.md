---
title: DAST - Dynamic Application Security Testing
tags: 
 - dast
 - acunetix
---

# DAST - Dynamic Application Security Testing

## DAST SCAN

### Definição

Executamos um scan DAST quando queremos buscar por vulnerabilidades em nossa aplicação funcional.

### Acunetix360 DAST

Acunetix 360 é a melhor solução de vulnerabilidade da web corporativa projetada para fazer parte de ambientes complexos. Ele fornece várias integrações, bem como opções de integração em contextos personalizados.

[Mais](https://www.acunetix.com/product/acunetix360/)

## Jenkins

No exemplo abaixo, um script para pipeline do jenkins simplificado é utilziado para configurar o pipeline do Jenkins para executar um scan.
```
---
pipeline {
   agent any
   stages {
       stage('DAST Scan'){
           steps{
                step([$class: 'NCScanBuilder', ncApiToken: '$APITOKEN', ncScanType: 'FullWithPrimaryProfile', ncWebsiteId: 'ed9f04a4-d530-43b5-f837-a88f03f0b886', ncDoNotFail: true, ncConfirmed: false, ncIgnoreFalsePositive: false, ncIgnoreRiskAccepted: false])
           }
       }
   }
}
---
```

{% include alert.html type="warning" title="Substitua $APITOKEN pelo seu token de API" %}
{% include alert.html type="warning" title="Substitua ncWebsiteId pelo seu website ID" %}

### Referências

<p class="editor-link"><a href="https://www.acunetix.com/support/docs/a360/integrations/installing-and-configuring-the-acunetix-360-scan-jenkins-plugin/" target="blank" class="btn"><strong>&#9998;</strong>Documentação oficial</a></p>