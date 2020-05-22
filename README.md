# Woliver REST API

<!-- TOC depthFrom:1 depthTo:2 withLinks:1 updateOnSave:1 orderedList:0 -->

  - [Introdução](#introdução)
  - [Autenticação](#autenticação)
  - [Recomendações](#recomendações)
  - [Leads](#leads)

<!-- /TOC -->

## Introdução

A Woliver é uma empresa dedicada a democratização ao acesso a tecnologias com foco em Real Estate.

Essa documentação possui o manual da API Woliver que pode ser utilizada por desenvolvedores para integrações com nossas tecnologias.

Todas as integrações são realizadas com uma imobiliária e a **BASE URL** da API segue o padrão:

```http
example.api.woliver.net
```

Referente a imobiliária de URL: [example.woliver.net](https://example.woliver.net). A propósito, para questões de documentação usaremos a imobiliária **example** nesse manual.


Esperamos que você goste da API Woliver, mas não hesite em [criar uma issue](https://github.com/woliverhq/api/issues/new) se houver algo faltando.

## Autenticação

Todos os chamados de API exigem uma API token. A API token deve ser requisitada para equipe Woliver e é única para cada imobiliária.

```http
GET /api/v1/example/?token=12345678901234567890123456789012
```

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `token` | `string` | **Obrigatório**. Sua Woliver API token |

## Recomendações

### Criar URL de recomendações

Esse endpoint é utilizado para a geração de uma URL com recomendações de imóveis. Ao abrir a URL a lead irá visualizar uma lista de imóveis organizados de acordo com os parâmetros de criação listados a seguir.

```http
POST /api/v1/recommendations/?token=12345678901234567890123456789012
```

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `token` | `string` | **Obrigatório**. Sua Woliver API token |
| `coordinates` | `string` | Coordenadas para buscar imóveis próximos |
| `price` | `string` | Preço para aluguel do imóvel |
| `taxes_included` | `boolean` | Considerar valores de IPTU e condomínio no valor de aluguel |
| `city` | `string` | Cidades para busca de imóveis |
| `neighborhoods` | `string` | Bairros para busca de imóveis |
| `bathrooms` | `number` | Número de banheiros do imóvel |
| `bedrooms` | `number` | Número de quartos do imóvel |
| `parking_spots` | `number` | Número de garagens do imóvel |
| `furnished` | `string` | Tipo de mobilia: "furnished", "semi_furnished", "unfurnished" |
| `pets` | `boolean` | Imóvel permite pets ou não |
| `listing_type` | `string` | Tipo de imóvel: Casa e/ou Apartamento |
| `name` | `string` | Nome da lead |
| `email` | `string` | Email da lead utilizado para enviar recomendações |
| `hide_filters` | `boolean` | Esconder os filtros no header da url de recomendações |
| `limit` | `number` | Limite de imóveis a serem retornados |
| `page_size` | `number` | Número de imóveis a serem retornados por página |

#### Request

```javascript
{
  "coordinates": "[[-27.570903, -48.508389]]",
  "price": 1500.0,
  "city": "florianopolis,sao-jose",
  "neighborhoods": "trindade,itacorubi",
  "bathrooms": 1,
  "bedrooms": 2,
  "parking_spots": 1,
  "furnished": "unfurnished",
  "pets": "true",
  "listing_type": "res_home,res_apartment",
  "nome": "",
  "email": "dev@woliver.com.br",
  "hide_filters": false, 
  "limit": 10,
  "page_size": 3
}
```

#### Response - 201 (application/json)

```javascript
{
  "url": "https://example.woliver.net/recomendacoes/k28Jv5/"
}
```

## Leads

### Criar lead

Esse endpoint é utilizado para a criação de novas leads. O usuário deverá prover os parâmetros listados a seguir.

```http
POST /api/v1/lead/?token=12345678901234567890123456789012
```

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `token` | `string` |**Obrigatório.** Sua Woliver API token |
| `full_name` | `string` |**Obrigatório.** Nome completo do lead |
| `cpf` | `string` | CPF do lead |
| `email` | `string` |**Obrigatório.** E-mail do lead |
| `phone_number` | `string` | Número de telefone do lead com DDD |
| `listing` | `number` |**Obrigatório.** Código do imóvel de acordo com o XML da imobiliária |
| `comments` | `string` | Comentários do lead a respeito do imóvel |

#### Request

```javascript
{
  "full_name": "Lead Woliver",
  "cpf": "11111111111",
  "email": "dev@woliver.com",
  "phone_number": "(99)99999999",
  "listing": 1532,
  "comments": "Quero agendar uma visita!"
}
```

#### Response - 201 (application/json)

```javascript
{
  "id": 9,
  "user": {
      "id": 7,
      "phone_number": "(99)99999999",
      "full_name": "Lead Woliver",
      "email": "dev@woliver.com"
      },
  "listing": "Listing 1532 (1532) Neighborhood",
  "staff1": null,
  "leads_source": "leads-source-slug"
}
```