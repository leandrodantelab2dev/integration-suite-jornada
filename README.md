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

## Service Key
```
{
    "oauth": {
        "clientid": "sb-bdca692b-0f58-4be9-8e51-c5202344c127!b455374|it-rt-c05249b3trial!b26655",
        "clientsecret": "e104d341-0593-4398-bed9-0f82ca5fc730$1AxJFLVmLotCPUlLj_Kymfam01ztlbzihqbzBH8w70Q=",
        "url": "https://c05249b3trial.it-cpitrial05-rt.cfapps.us10-001.hana.ondemand.com",
        "createdate": "2025-07-02T00:17:31.919Z",
        "tokenurl": "https://c05249b3trial.authentication.us10.hana.ondemand.com/oauth/token"
    }
}
```

## Payload
```
{
  "userId": "104065",
  "nomeCompleto": "João da Silva",
  "email": "joao.silva@empresa.com",
  "departamento": "Financeiro",
  "tiket": {
    "titulo": "Erro no sistema de ponto",
    "descricao": "Não consigo registrar minhas horas no SuccessFactors.",
    "tipoSolicitacao": "Problema Técnico",
    "prioridade": "Alta",
    "sistemaAfetado": "SuccessFactors",
    "statusInicial": "Novo",
    "dataHoraSolicitacao": "2025-07-02T10:35:00Z"
  }
}
```

## Erro Body
```
{
  "Status": "${property.p_status}",
  "Message": "${property. p_message}"
}
```



## Solace
Link Solace: https://console.solace.cloud/login?reason=auth


## Policy 01
```
<KeyValueMapOperations mapIdentifier="Credential_SAP_CI" async="true" continueOnError="false" enabled="true" xmlns="http://www.sap.com/apimgmt">

  <Get assignTo="private.basicAuth.username">
      <Key>
          <Parameter>user</Parameter>
      </Key>
  </Get>
  <Get assignTo="private.basicAuth.password">
      <Key>
          <Parameter>password</Parameter>
      </Key>
  </Get>
  <!-- the scope of the key value map. Valid values are environment, organization, apiproxy and policy -->
  <Scope>environment</Scope>
</KeyValueMapOperations>
```

## Policy 02
```
<BasicAuthentication async='true' continueOnError='false' enabled='true' xmlns='http://www.sap.com/apimgmt'>
  <!-- Operation can be Encode or Decode -->
  <Operation>Encode</Operation>
  <IgnoreUnresolvedVariables>true</IgnoreUnresolvedVariables>
  <!-- for Encode, User element can be used to dynamically populate the user value -->
  <User ref='private.basicAuth.username'></User>

  <Password ref='private.basicAuth.password'></Password>
  <!-- Source is used to retrieve the encoded value of username and password. This should not be used if the operation is Encode-->

  <AssignTo createNew="false">request.header.Authorization</AssignTo>
</BasicAuthentication>
```
