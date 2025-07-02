# Jornada Integration Suite

## Links Úteis
Link para o BTP: https://account.us2.hana.ondemand.com/#/home/welcome

Link Integration Suite: https://c05249b3trial.integrationsuite-trial.cfapps.us10-001.hana.ondemand.com/shell/home

## Usuário Integration Suite
Usuário
```
usuario.live@lab2learn.com.br
```
Senha
```
Lab2dev#2025
```

## Código Groovy
```
import com.sap.gateway.ip.core.customdev.util.Message
import groovy.json.JsonSlurper

Message processData(Message message) {
    def body = message.getBody(String)
    def json = new JsonSlurper().parseText(body)

    def error_message
    def status


    def userId = json.userId
    def email  = json.email

    // Validação de campos obrigatórios
    if (!json.userId || json.userId.trim().isEmpty()) {  

        error_message = "Campo obrigatório 'userId' está ausente ou vazio."
        status  = "Error"

    }else{

        status = "Success"
    }

    message.setProperty("p_status", status)
    message.setProperty("p_message", error_message)

    return message
}
```

## Solace
Link Solace: https://console.solace.cloud/login?reason=auth
